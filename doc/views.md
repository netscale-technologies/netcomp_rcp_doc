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
primary_nurse_uid|string or null|Get patients having this nurse as primary
medical_uid|string|Get patients having this doctor (any role)
main_program|string|Get patients having this "main program"
name_or_surname_prefix|string|Get patients with name or surname starting with this
size|string|Number of records to return (default 50)
order|string|`asc` (default) or `desc`

Following fields are returned:

|Field|Type|Description
|---|---|---
patient_uid|string|
cursor|string|To use in pagination
ehr_id|string|
status|string|
birth_date|string|Date of birth
phone|string|
gender|string| M,F,U,O
office_uid|string|
organization_uid|string|
primary_medical_uid|string|Get patients having this doctor as primary
primary_nurse_uid|string|Get patients having this doctor as primary
medical_uid|string|Get patients having this doctor (any role)
main_program|string|Get patients having this "main program"
risk_factor|integer|
devices| [object] |List of devices associated to this patient
extra| [object] |Additional data (name, surname, programs)

Order will be by (surname, name) in normalized form, ascending. Can be reversed with `order`


### view_count_patients

Similar to view_list_patients, but it only counts the number of records matching the filter

|Field|Type|Description
|---|---|---
ehr_id_prefix|string|Get patients with ehr_id starting with this. Very fast, since it has specific index for it.
status|string|Get patients having this status
office_uid|string|
organization_uid|string|
primary_medical_uid|string|Get patients having this doctor as primary
primary_nurse_uid|string or null|Get patients having this nurse as primary
main_program|string|Get patients having this "main program"
name_or_surname_prefix|string|Get patients with name or surname starting with this


### view_list_devicehubs

|Field|Type|Description
|---|---|---
device_uid|string|If provided, returns a single hub info
cursor|string|Last seen cursor, if order=asc, or first if order=desc to provide pagination
device_id|string|If provided, returns a single hub info
device_id4|string|Last four digits of device_id
label_prefix|string
has_patient|boolean|Filter devices having or not having a patient
patient_uid|string or null
office_uid|string or null|
organization_uid|string or null|
size|string|Number of records to return (default 50)
order|string|`asc` (default) or `desc`

Order will be label, ascending. Can be reversed with `order`

Following fields are returned:

|Field|Type|Description
|---|---|---
device_uid|string|
label|string|
cursor|string|
device_id|string|
status|string|
is_synced|boolean|
last_status|string
last_status_time|string
patient_uid|string|
office_uid|string|
organization_uid|string|
extra|object|Includes info about patient, owner, office and device

### view_count_devicehubs

Similar to view_list_devicehubs, but it only counts the number of records matching the filter

|Field|Type|Description
|---|---|---
device_id4|string|Last four digits of device_id
label_prefix|string
has_patient|boolean|Filter devices having or not having a patient
patient_uid|string
office_uid|string|
organization_uid|string|

### view_list_devices

|Field|Type|Description
|---|---|---
device_uid|string|If provided, returns a single hub info
cursor|string|Last seen cursor, if order=asc, or first if order=desc to provide pagination
device_id|string|If provided, returns a single hub info
label_prefix|string|
type|string|
patient_uid|string or null
office_uid|string or null|
organization_uid|string or null|
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
type|string
patient_uid|string|
office_uid|string|
organization_uid|string|
extra|object|Includes info about patient, owner, office and device

Order will be label, ascending. Can be reversed with `order`

### view_count_devices

Similar to view_list_devices, but it only counts the number of records matching the filter

|Field|Type|Description
|---|---|---
type|string|
label_prefix|string|
patient_uid|string
office_uid|string|
organization_uid|string|


### view_list_orders

|Field|Type|Description
|---|---|---
order_uid|string|If provided, returns a single patient info
patient_uid|string|
cursor|string|Last seen cursor, if order=asc, or first if order=desc to provide pagination
name_or_surname_prefix|string|Get patients with name or surname starting with this
last_status|string|
size|string|Number of records to return (default 50)
order|string|`asc` (default) or `desc`

Following fields are returned:

|Field|Type|Description
|---|---|---
order_uid|sting
cursor|string|To use in pagination
patient_uid|string|
creation_timer|string|
last_status|string|
last_status_time|string|
extra|object|Will include name, surname, address, phone and devices

devices will be a list of devices including 'type' and 'units'

Order will be creation time, descending. Can be reversed with `order`

### view_count_orders

Similar to view_list_orders, but it only counts the number of records matching the filter

|Field|Type|Description
|---|---|---
patient_uid|string|
name_or_surname_prefix|string|Get patients with name or surname starting with this
last_status|string|





### view_list_notifications

|Field|Type|Description
|---|---|---
notification_uid|string|If provided, returns a single notification
topic|string|To use in pagination
start_time|string|Minimum time if order is asc, maximum if order is desc
is_read|boolean|
group|string|"medical" or "staff"
priority_min|integer|
priority_max|integer|
size|string|Number of records to return (default 50)
order|string|`asc` (default) or `desc`

Following fields are returned:

|Field|Type|Description
|---|---|---
notification_uid|string
topic|string,
group|string
class|string
type|string
priority|integer
is_read|boolean
time|date
extra|map

Order will be update time, descending. Can be reversed with `order`


### view_count_notifications

Similar to view_list_notifications, but it only counts the number of records matching the filter

|Field|Type|Description
|---|---|---
topic|string|To use in pagination
start_time|string|Minimum time if order is asc, maximum if order is desc
is_read|boolean|
group|string|"medical" or "staff"
priority_min|integer|
priority_max|integer|




### view_list_observations

|Field|Type|Description
|---|---|---
observation_uid|string|If provided, returns a single notification
start_time|string|Minimum time if order is asc, maximum if order is desc
stop_time|string|Maximum time if order is asc, minimum if order is desc
type|string
device_id|string
device_uid|string
device_label|string
hub_id|string
hub_uid|string
hub_label|string
patient_uid|string
is_valid|boolean
is_alert|boolean
is_test|boolean
size|string|Number of records to return (default 50)
order|string|`asc` (default) or `desc`

Following fields are returned:

|Field|Type|Description
|---|---|---
observation_uid|string
type|string
device_uid|string
device_type|string
device_protocol|string
device_label|string
hub_uid|string
hub_label|string
patient_uid|string
time|string
is_valid|boolean
is_alert|boolean
is_test|boolean
extra|map

Order will be observation's time, descending. Can be reversed with `order`


### view_count_observations

Similar to view_list_observations, but it only counts the number of records matching the filter

|Field|Type|Description
|---|---|---
start_time|string|Minimum time if order is asc, maximum if order is desc
stop_time|string|Maximum time if order is asc, minimum if order is desc
type|string
device_id|string
device_uid|string
device_label|string
hub_id|string
hub_uid|string
hub_label|string
patient_uid|string
is_valid|boolean
is_alert|boolean
is_test|boolean

