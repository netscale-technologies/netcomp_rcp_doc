# API


## Timeslots


### create_timeslots
Creates a new Timeslot object

|Field|Type|Mandatory|Description
|---|---|---|---
|spec|spec|Y|See bellow

Spec object:

|Field|Type|Mandatory|Description
|---|---|---|---
|type|string|Y|Type of timeslot. See bellow
|description|string|N|
|user_uid|string|Y|UID of medical or `"system"`
|patient_uid|string|Y|UID of patient
|notification_uid|string|N|UID of notification, observation or call
|start_time|string|Y|Datetime of starting slot
|duration_secs|integer|N|Duration in secs (needed if no `stop_time`)
|tags|list of string|N|
|program|string|
|invisible|boolean|N|Default: false
|meta|object|N|Additional data

Returns:


|Field|Type|Description
|---|---|---
|uid|string|UID of the created timeslot


### get_timeslot
Gets the data of an existing timeslot. 

|Field|Type|Mandatory|Description
|---|---|---|---
|uid|string|Y|UID of timeslot



If successful, full information will be retrieved:

|Field|Type|Description
|---|---|---
|spec|spec|See above
|metadata|metadata|See bellow
|status|status|See bellow

### user_update_timeslot
This API can be used to update partially a timeslot

|Field|Type|Mandatory|Description
|---|---|---|---
|uid|string|Y|UID of timeslot
|reason|string|Y|Reason for the update
|spec|object|Y|A subset of fields can be updated (see bellow)

Fields in spec that can be updated are:
* description
* start_time
* duration_secs
* tags
* program
* meta


Meta and tags will **replace** the previous meta, so read it first and add fields if needed
If the operation is successful, update info will be added to status, and field 'updates' will be incremented in view


### delete_timeslot

Allows to delete an existing timeslot. Same parameters as get_timeslot


