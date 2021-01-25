# API

## Views

These APIs are for fast-listing or to get detailed information in one shot from from pre-calculated tables.
It should be very fast no matter the number of records, unless filtering means returned objects are all at high positions in the sort order.

For pagination, caller must repeat the query but including last "seen" cursor at `cursor` for ascending order. For descending, must provide 
the first one.

### view_list_patients

|Field|Type|Description
|---|---|---
patient_uid|string|If provided, returns a single patient info
cursor|string|Last seen cursor, if order=asc, or first if order=desc to provide pagination
ehr_id_prefix|string|Get patients with ehr_id starting with this. Very fast, since it has specific index for it.
status|string|Get patients having this status
office_uid|string|
organization_uid|string|
primary_medical_uid|string|Get patients having this doctor as primary
main_program|string|Get patients having this "main program"
name_or_surname_prefix|string|Get patients with name or surname starting with this
size|string|Number of records to return (default 50)
order|string|`asc` (default) or `desc`

Following fields are returned:

|Field|Type|Description
|---|---|---
patient_uid|string|
cursor|string|To use in pagination
full_name|string|
ehr_id|string|
status|string|
office_uid|string|
organization_uid|string|
primary_medical_uid|string|Get patients having this doctor as primary
main_program|string|Get patients having this "main program"
risk_factor|integer|
devices| [object] |List of devices associated to this patient


### view_count_patients

Similar to view_list_patients, but it only counts the number of records matching the filter

|Field|Type|Description
|---|---|---
ehr_id_prefix|string|Get patients with ehr_id starting with this. Very fast, since it has specific index for it.
status|string|Get patients having this status
office_uid|string|
organization_uid|string|
primary_medical_uid|string|Get patients having this doctor as primary
main_program|string|Get patients having this "main program"
name_or_surname_prefix|string|Get patients with name or surname starting with this


### view_list_devicehubs

|Field|Type|Description
|---|---|---
device_uid|string|If provided, returns a single hub info
cursor|string|Last seen cursor, if order=asc, or first if order=desc to provide pagination
device_id|string|If provided, returns a single hub info
device_id_last4|string|Last for digits of device_id
label|string
patient_uid|string
office_uid|string|
organization_uid|string|
size|string|Number of records to return (default 50)
order|string|`asc` (default) or `desc`

Following fields are returned:

|Field|Type|Description
|---|---|---
device_uid|string|
label|string|
cursor|string|
device_id|string|
status|string|
owner_type|string|
owner_uid|string|
owner_time|string|
patient_uid|string|
patient_time|string|
office_uid|string|
organization_uid|string|
extra|string|

### view_count_devicehubs

Similar to view_list_devicehubs, but it only counts the number of records matching the filter

|Field|Type|Description
|---|---|---
patient_uid|string
office_uid|string|
organization_uid|string|
