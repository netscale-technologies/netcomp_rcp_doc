# API


## Patients


### create_patient
Creates a new Patient object

|Field|Type|Mandatory|Description
|---|---|---|---
|id|string|N|Unique (for domain) id of patient, no special characters allowed. Generated if not included.
|domain|string|N|Domain to use (like "zone1.type")
|spec|spec|Y|See bellow

Spec object:

|Field|Type|Mandatory|Description
|---|---|---|---
|login|N|For future use
|name|string|Y|Name (or full name) of user
|surname|string|N|Surname of user (optional)
|name_extra|string|N|Extra parts of name
|gender|string|N|Must be 'M', 'F', 'O' or 'U'
|phone|string|N|Phone
|phone_sms|string|N|Alternative phone
|email|string|N|Contact email
|timezone|string|N|Must be a valid timezone
|locale|string|N|Must be a valid locale
|address|address|N|See bellow
|alternative_id|string|N|Alternative Id
|ehr_id|string|Y|Alternative Id (MRW)
|ehr|ehr|N|See bellow
|office_uid|string|Y|UID of patient's office
|medicals|\[medical\]|N|List of medicals
|insurance_meta|object|Free field to store insurance data
|avatar|string|N|
|programs|\[string\]|N|List of programs (["CCM", "RCP"])
|risk_factor|integer|N|Risk factor (0, 1, 2)
|contact_preference|string|N|"SMS", "VOICE"
|meta|object|N|

Address object:

|Field|Type|Mandatory|Description
|---|---|---|---
|street|string|Y|
|code|string|N|
|city|string|N|
|province|string|N|
|state|string|N|
|country|string|N|


EHR object:

|Field|Type|Mandatory|Description
|---|---|---|---
|birth_time|string|N|RFC3339 date
|height|integer|N|
|weight|integer|N|
|meta|object|N|


Medical object:

|Field|Type|Mandatory|Description
|---|---|---|---
|uid|string|Y|UID of medical
|role|string|Y|
|primary|boolean|N


Returns:

|Field|Type|Description
|---|---|---
|uid|string|UID of the created patient
|id|string|Unique (in domain) id


### get_patient
Gets the data of an existing patient. Two formats available:

|Field|Type|Mandatory|Description
|---|---|---|---
|uid|string|Y|UID of patient
|expand_devices|boolean|N|True to get an expanded list of devices
|expand_surveys|boolean|N|True to get an expanded list of surveys cronjobs
|expand_reminders|boolean|N|True to get an expanded list of reminder cronjobs
|expand_timeslots|boolean|N|True to get an expanded list of this month's timeslots
|expand_default_ranges|boolean|N|True to get an expandeded list of default ranges


|Field|Type|Mandatory|Description
|---|---|---|---
|id|string|Y|ID of patient
|domain|string|N|Domain of creation
|expand_devices|boolean|N|True to get an expanded list of devices
|expand_surveys|boolean|N|True to get an expanded list of surveys cronjobs
|expand_reminders|boolean|N|True to get an expanded list of reminder cronjobs
|expand_timeslots|boolean|N|True to get an expanded list of this month's timeslots


If successful, full information will be retrieved:

|Field|Type|Description
|---|---|---
|spec|spec|See bellow
|status|spec|See bellow
|metadata|metadata|See bellow
|extra|extra|Only present if any expansion is requested. See bellow.


Status description:

|Field|Type|Description
|---|---|---
|conditions| [ condition ] | See bellow
|parameters| object | See parameters discussion bellow
|send_reminders|boolean|
|status|string|"inative", "testing" or "active"
|status_reason|string|

Condition object

|Field|Type|Description
|---|---|---
|type|string|"primary" or "secondary"
|code|string|
|text|string|
|date|string|

Metadata description:

|Field|Type|Description
|---|---|---
|uid|string|Globally unique UID
|id|string|Id unique for domain
|creation_date|string|Creation date in RFC3339 format
|update_date|string|Last update date in RFC3339 format
|generation|integer|Increases at each update


Extra description:

|Field|Type|Description
|---|---|---
|devicehubs|[devicehub]|Present if expand_devices was true
|devices|[device]|Present if expand_devices was true
|surveys|[survey]|Present if expand_surveys was true
|reminders|[reminder]|Present if expand_reminders was true
|timeslots|timeslot|Present if expand_timeslots was true


Devicehub description:

|Field|Description
|---|---
|hub_uid|UID of devicehub
|hub_id|Serial
|label|


Device description:

|Field|Description
|---|---
|device_uid|UID of devicehub
|device_id|Serial
|label|
|class|
|type|


Survey description:

|Field|Description
|---|---
|uid|UID of cronjob
|schedule|
|meta|


Reminder description:

|Field|Description
|---|---
|uid|UID of cronjob
|schedule|
|meta|

Timeslot description:

|Field|Description
|---|---
|secs|Total secs for this month
|cache_uid|UID of the cache actor
|loaded_time|Time when the actor was refreshed
|created|True if we just created the cache actor


### update_patient_status
Allows to update patient's data

|Field|Type|Mandatory|Description
|---|---|---|---
|uid|string|Y|Unique UUID
|status|string|Y|New status
|reason|string|N|

Returns ok or error


### update_patient_send_reminders
Allows to update patient's "send_reminders" flag

|Field|Type|Mandatory|Description
|---|---|---|---
|uid|string|Y|Unique UUID
|send_reminders|boolean|Y|New status


### delete_patient

Allows to delete an existing patient. Same parameters as get_patient


### search_patients
Allows for a powerful search operation on patients


|Field|Type|Description
|---|---|---
|domain|string|Domain to filter on
|deep|boolean|If true, will search in sub-domains
|id|string|Genrated Id
|login|string|
|name|string|
|fts_name|string|See bellow
|surname|string|
|fts_surname|string|See bellow
|fts_fullname|string|See bellow
|status|string|
|status_reason|string|
|activation_time| string or null
|alternative_id|string|
|ehr_id|string|
|phone|string|
|phone_sms|string|
|email|string|
|timezone|string|
|in_office|string|Filter for patients belonging to this office. Use `null` to find patients with no office.
|in_organization|string|Filter for patients belonging to this organization
|with_medical|string|Filter for patients linked to this medical
|with_program|string|Filter for patients having this program
|risk_factor|string|"gte:1"
|contact_preference|string|
|creation_date|string|Creation date in RFC3339 format
|update_date|string|Last update date in RFC3339 format
|from|integer|Starting point (for pagination)
|size|integer|Number or records to retrieve
|get_total|boolean|If true, calculates the grand total of records
|sort|\[string\]|List of fields to sort on

* Can filter on domain, or domain and subdomains using 'deep'
* Can paginate with from and size
* Can filter on any of the previous fields, or any number of them, working like and 'and'
  * If you set a raw string, it will find objects with that exact string: `"phone": "123"`
  * You can use a prefix to modify the behaviour: eq, ne, gt, gte, lt, lte. They work for string and numeric fields 
  * You can use a prefix: `"phone": "prefix:123"`
  * You can target several values: `"phone": "123|321"`
* Can sort on one or several fields, ascending or descending: `"sort": ["asc:update_time", "timezone"]` 
* Operations on field 'name' will be performed over a 'normalized' version of the field (without uppercase, utf8, etc.)

#### Full text search

FTS queries can be performed by using the field 'fts_name'. You can query for an specific word (`"fts_name": "patient"`) or a prefix of any word in the name using a tailing "*" (`"fts_name": "off*"`).



### update_patient_parameters
Patient allow a number of growing number of parameters to be attached to them. It is a small database that can be updated with this command.

Currently, parameters support this patterns:

```javascript
{
    "ranges": {
        "type": {       // For example weight_kgs
            normal_min: 60.0
            normal_max: 90.0
            ideal: 70.0
        }
    }
}

```

Currently supported types for ranges are:

* weight_kgs
* bpm
* diastolic_mmHg
* systolic_mmHg
* spo2_pct
* pi_pct
* temperature_celsius_body
* temperature_celsius_tympanum
* glucose_mg_dl

By default, this command will update the parameters by *merging* the old data with the data. If `merge: false` is used, all old parameters will be replaced with the update. 

Two forms are available:

|Field|Type|Mandatory|Description
|---|---|---|---
|uid|string|Y|Unique UUID
|merge|boolean|N|Default true
|parameters|object|Y|

|Field|Type|Mandatory|Description
|---|---|---|---
|id|string|Y|ID of patient
|domain|string|N|Domain of creation
|merge|boolean|N|Default true
|parameters|object|Y|S


### update_patient_condition
Allows to add, update or delete a condition.
Currently only two conditions are supported: "primary" and "secondary"

|Field|Type|Mandatory|Description
|---|---|---|---
|uid|string|Y|Unique UUID
|condition| condition |Y|
|delete|boolean|N|true to delete the condition

Condition objet

|Field|Type|Mandatory|Description
|---|---|---|---
|type|string|Y|Primary or secondary
|code|string|N|Code of the condition
|text|string|N|Text for it

### update_patient_facility
Allows to change patient's facility
New facility should be under the same organization

|Field|Type|Mandatory|Description
|---|---|---|---
|uid         |string|Y|
|facility_uid|string|Y|

### add_patient_survey_rule (TO BE REMOVED)

Adds a new schedule rule that, when fired, will create a survey

|Field|Type|Mandatory|Description
|---|---|---|---
|patient_uid|string|Y|ID of patient
|script_id|string|Y|Script ID
|careplan_uid|string|Y|
|schedule|schedule|Y|See bellow
|timeout_secs|integer|N|Defaut 7200 (2h)
|priority|integer|N|0..999


Schedule object:

|Field|Type|Mandatory|Description
|---|---|---|---
|repeat|string|Y|"daily", "weekly" or "monthly"
|hour|integer|N|Hour for launch (default 12)
|minute|integer|N|Minute for launch (default 0)
|timezone|string|N|Valid timezone (default GMT)
|daily_week_days| [0..6]|N|For daily, use only this week days (0:Sunday, 6:Saturday) (default all days)
|daily_step_days|0..25|N|For daily, jump this step days after each call (default: 1)
|weekly_day|0..6|N|For weekly, use this day
|monthly_day|1..25 &#124; last|N|For monthly, use this day. Use "last" for last day of each month
|start_date|string|N|Do not launch before this date
|stop_date|string|N|Do not launch after this date


To test the rule before the activation time, you can use command `launch_rule`. To delete the rule, use command `delete_rule`


### call_patient_phone

Orders a call to patient_phone, using patient's office as caller, but calling medical's phone instead.

|Field|Type|Mandatory|Description
|---|---|---|---
|patient_uid|string|Y|UID of patient
|caller_uid|string|Y|UID of caller (medical)
|patient_phone|string|N
|caller_phone|string|N
|intermediate_phone|string|N


### patient_list_values

Allows to list "values" (big data readings) for a patient

|Field|Type|Mandatory|Description
|---|---|---|---
|uid|string|Y|UID of patient
|class|string|Y|Class of the reading (currently only "dexcom")
|type|string|Y|Type of the reading (currently only "glucose_est")
|start_date|datetime|N
|stop_date|datetime|N
|start_datetime|datetime|N
|stop_datetime|datetime|N
|order|asc or desc|N
|size|integer|0-1000

### patient_count_values

Allows to count "values" (big data readings) for a patient

|Field|Type|Mandatory|Description
|---|---|---|---
|uid|string|Y|UID of patient
|class|string|Y|Class of the reading (currently only "dexcom")
|type|string|Y|Type of the reading (currently only "glucose_est")
|start_date|datetime|N
|stop_date|datetime|N


### patient_list_values_daily

Works like `patient_list_values` but it will group values by date, taking timezone into account.
It will generate a record for each date. In each one, for each detected values, stats "min", "max", "count" and "sum"
will be calculated.

First call for a set of params may take some time. Next calls with the same parameters will use pre-calculated data

|Field|Type|Mandatory|Description
|---|---|---|---
|uid|string|Y|UID of patient
|timezone|string|Y|Timezone for day calculations
|class|string|Y|Class of the reading (currently only "dexcom")
|type|string|Y|Type of the reading (currently only "glucose_est")
|start_date|date|Y
|stop_date|datetime|Y


### patient_list_values_hourly

Works like `patient_list_values` but it will group values by date and hour, taking timezone into account.
It will generate a record for each date & hour. In each one, for each detected values, stats "min", "max", "count" and "sum"
will be calculated.

First call for a set of params may take some time. Next calls with the same parameters will use pre-calculated data

|Field|Type|Mandatory|Description
|---|---|---|---
|uid|string|Y|UID of patient
|timezone|string|Y|Timezone for day calculations
|class|string|Y|Class of the reading (currently only "dexcom")
|type|string|Y|Type of the reading (currently only "glucose_est")
|start_date|date|Y
|stop_date|datetime|Y



### patient_list_summaries

Allows to list "summaries" (daily readings) for a patient

|Field|Type|Mandatory|Description
|---|---|---|---
|uid|string|Y|UID of patient
|class|string|Y|Class of the reading (currently only "fitbit")
|type|string|Y|Type of the reading (currently only "sleep_sum", "activity_sum", "heart_var_sum", "breath_sum", "spo2_sum"
|start_date|datetime|N
|stop_date|datetime|N
|size|integer|0-1000

### patient_count_summaries

Allows to count "summaries" (daily readings) for a patient

|Field|Type|Mandatory|Description
|---|---|---|---
|uid|string|Y|UID of patient
|class|string|Y|Class of the reading (see above)
|type|string|Y|Type of the reading (see above)
|start_date|datetime|N
|stop_date|datetime|N







