# API

There are two objects to model the whole Medications Engine.

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



