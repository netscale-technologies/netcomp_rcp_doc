# API

There are two objects to model Core Medications Engine.

* `patient_meds` refers to a current "medication list" for a patient, including the scheduling for the reminder of each one.
* `med_taken` rerers to an indivudual "take" of a specific medication for an specific patient

## PatientMeds

### create_patient_med

You can _create_ or _update_ a medication list for a patient calling to this API 
* Included schedules for all medications will be processed and cron jobs will be started to be launched at each specific time
* Inside patient_med record you have information to set each of the individual "takes" as "taken" or "skipped"
* When the each reminder time arrive, and at least one of the medications scheduled for that exact time is still in "pending" state
(it has not been marked yet as "taken" or "skipped" a reminder is set. 
* In any case, a new cron job is scheduled for the next iteration of the reminder.
* Each patient_med instance has a serial for each patient, starting at 1
* When you create a new patient_med instance, the new one gets the next serial. Previous one is deactivated and cron jobs
are removed. New cron jobs are generated according to new schedule.
* There is no option to modify the medication schedule, you need to create a new one, maybe copying part of the previous 
medication list and schedules

You must include field `spec` with followin fields:


|Field|Type|Mandatory|Description
|---|---|---|---
|patient_uid|string|Y|Patient we refer to
|timezone|string|Y|Timezone the schedule will refer to
|meds| [medication] |Y|List of medications. 

Medications must use following format:

|Field|Type|Mandatory|Description
|---|---|---|---
|code|string|Y|Unique code for this medication
|name|string|Y|Medication name
|description|string|Y|Medication description
|schedules| [schedule] |Y|Scheduling for reminders

Schedules use the following format:

|Field|Type|Mandatory|Description
|---|---|---|---
|display|string|Y|User-based time like "breakfast"
|repeat|string|Y|"daily", "weekly", "monthly"
|hour|integer|Y|0-23
|minute|integer|Y|0-59
|second|integer|N|0-59
|daily_week_days| [integer] |N| 0: sunday
|daily_step_days| [integer] |N| 1-25
|weekly_day|integer|N| 0: sunday
|monthly_day|integer|N|1-31
|start_date|date|N|No reminder before this date
|stop_date|date|N|No reminder after this date

Example:

```
"spec": {
  "patient_uid": "Patient-XXX",
  "timezone": "US/East",
  "meds": [
    {
      "code": "code1",
      "name": "name",
      "description": "desc",
      schedules: [{"repeat": :"daily", "hour": 12, "minute": 0, display: "breakfast"}]
    }
  ]
}
```

If everything is ok, you will get a new patient_meds' uid

### get_last_patient_med

Gets the last (with higher serial) patient med.
Then only required field is `patient_uid`

You will get the included _spec_ along with serial, uid, and next expected events

### patient_med_set_active

Marks a patient_med as active or inactive.
Required fields are `uid` and `is_active`.
To mark a patient_med as _active_, it must be the last one or an error will be returned (you cannot make active again an old patient_meds)


### delete_patient_med

Removes a patient meds and stops all pending reminders for it.
Only mandatory field is `uid`

### view_list_patient_meds

Allows you to list all patient_meds for a patient.
Sorted will be always from higher serial to lower serial

Allowed fields are:

|Field|Type|Mandatory|Description
|---|---|---|---
|patient_uid|string|Y|Mandatory patient
|is_active|boolean|N|Filter on active status
|cursor|string|N|Use last cursor for pagination if needed
|size|integer|N|Number of records (default 1.000)

### view_count_patient_meds

Allows you to count all patient_meds for a patient.

Allowed fields are:

|Field|Type|Mandatory|Description
|---|---|---|---
|patient_uid|string|Y|Mandatory patient
|is_active|boolean|N|Filter on active status


## MedTaken

Specific instances for a medication take are created every time a new patient_med is created or updated, and when the reminder time arrives and the next instance of each medication is generated.

### med_taken_set_status_uid

Allows you to set a med_taken as "taken" or "skipped", if you know the UID of this instance.
Allowed fields are:

|Field|Type|Mandatory|Description
|---|---|---|---
|uid|string|Y|
|status|string|Y| "taken" or "skipped"
|updater_uid|string|N|UID of Patient or Medical calling the API (provisional)
|updater_kind|string|N| "Patient" or "Medical" (provisional)
|updater_timestamp|string|Datetime of "taken" if not the same as now

In the near future, updater_uid and updater_kind will probably be extracted from token

You can call this API multiple times, and the last one will be considered the "last" status.
All previous statuses are stored inside the object.

### med_taken_set_status_code

Allows you to set a med_taken as "taken" or "skipped", if you don't know the UID for this instance but you know `patient_meds_uid`, `fire_time` and `code` (for example from push notification)

|Field|Type|Mandatory|Description
|---|---|---|---
|patient_meds_uid|string|Y|
|fire_time|string|Y|
|code|string|Y|
|status|string|Y| "taken" or "skipped"
|updater_uid|string|N|UID of Patient or Medical calling the API (provisional)
|updater_kind|string|N| "Patient" or "Medical" (provisional)
|updater_timestamp|string|N|Datetime of "taken" if not the same as now


### med_taken_get

Allows to get a specific record. Mandatory field is `uid`

### view_list_med_taken

Allows you to list all med_taken records for a patient (if generated so far)
Sorted will be always from more recent to less recent

Allowed fields are:

|Field|Type|Mandatory|Description
|---|---|---|---
|patient_uid|string|Y|Mandatory patient
|last_status|string|N|"pending", "taken", "skipped", "error"
|code|string|N|Filter by this med code
|start_time|string|N|Start on this datetime (for last_status_time)
|stop_time|string|N|Start on this datetime (for last_status_time)
|cursor|string|N|Use last cursor for pagination if needed
|size|integer|N|Number of records (default 1.000)

### view_count_med_taken

Allows you to count all patient_meds for a patient.

Allowed fields are:

|Field|Type|Mandatory|Description
|---|---|---|---
|patient_uid|string|Y|Mandatory patient
|last_status|string|N|"pending", "taken", "skipped", "error"
|code|string|N|Filter by this med code
|start_time|string|N|Start on this datetime (for last_status_time)
|stop_time|string|N|Start on this datetime (for last_status_time)











