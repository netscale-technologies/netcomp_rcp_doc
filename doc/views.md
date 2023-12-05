# API

## Views

These APIs are for fast-listing or to get detailed information in one shot from from pre-calculated tables.
It should be very fast no matter the number of records, unless filtering means returned objects are all at high positions in the sort order.

For pagination, caller must repeat the query but including last "seen" cursor at `cursor` for ascending order. For descending, must provide 
the first one.



### view_list_partners

|Field|Type|Description
|---|---|---
uid|string|If provided, returns a single object info
status|string|Filter on status ("active" or "inactive")
fts|string|Filter on records having this string or substrig in name, email or phone
expand|boolean|If true, expanded fields are returned (see bellow)

By default `expand` is false, and only name is returned, along with uid
If `expand is true`, following fields are returned:


|Field|Type|Description
|---|---|---
uid|stri
name|string
email|string
phone|string
status|string
url|string
children|integer|number of child organizations

### view_count_partners

Similar to view_list_partners, but it only counts the number of records matching the filter

|Field|Type|Description
|---|---|---
status|string|Filter on status ("active" or "inactive")
fts|string|Filter on records having this string or substring in name, email or phone


### view_list_organizations

|Field|Type|Description
|---|---|---
partner_uid|string|required
uid|string|If provided, returns a single object info
status|string|Filter on status ("active" or "inactive")
fts|string|Filter on records having this string or substrig in name, email or phone
expand|boolean|If true, expanded fields are returned (see bellow)

By default `expand` is false, and only name is returned, along with uid
If `expand is true`, following fields are returned:


|Field|Type|Description
|---|---|---
uid|stri
name|string
email|string
phone|string
status|string
url|string
children|integer|number of child facilities
partner_uid|string
parent_name|name of partner

### view_count_organizations

Similar to view_list_organizations, but it only counts the number of records matching the filter

|Field|Type|Description
|---|---|---
status|string|Filter on status ("active" or "inactive")
fts|string|Filter on records having this string or substring in name, email or phone


### view_list_offices

|Field|Type|Description
|---|---|---
organization_uid|string|required
uid|string|If provided, returns a single object info
status|string|Filter on status ("active" or "inactive")
fts|string|Filter on records having this string or substrig in name, email or phone
expand|boolean|If true, expanded fields are returned (see bellow)

By default `expand` is false, and only name is returned, along with uid
If `expand is true`, following fields are returned:


|Field|Type|Description
|---|---|---
uid|stri
name|string
email|string
phone|string
status|string
url|string
partner_uid|string
organization_uid|string
parent_name|name of organization

### view_count_offices

Similar to view_list_offices, but it only counts the number of records matching the filter

|Field|Type|Description
|---|---|---
status|string|Filter on status ("active" or "inactive")
fts|string|Filter on records having this string or substring in name, email or phone


### view_list_medicals

|Field|Type|Description
|---|---|---
uid|string|If provided, returns a single object info
status|string|Filter on status ("active" or "inactive")
office_uid|string|Filter on this
organization_uid|string|Filter on this
fts|string|Filter on records having this string or substrig in name, surname, npi, login, email, phone
cursor|string|Use las cursor to paginate
size|integer|default 10
Following fields are returned:

|Field|Type|Description
|---|---|---
uid|stri
name|string
surname|string
login|string
npi|string
status|string
facility_uid|string
type|string
email|string
phone|string
office_name|string
                
                
### view_count_medicals

Similar to view_list_medicals, but it only counts the number of records matching the filter

|Field|Type|Description
|---|---|---
status|string|Filter on status ("active" or "inactive")
office_uid|string|Filter on this
organization_uid|string|Filter on this
fts|string|Filter on records having this string or substrig in name, surname, npi, login, email, phone



### view_list_staff

|Field|Type|Description
|---|---|---
uid|string|If provided, returns a single object info
status|string|Filter on status ("active" or "inactive")
office_uid|string|Filter on this
organization_uid|string|Filter on this
fts|string|Filter on records having this string or substrig in name, surname, npi, login, email, phone
size|integer|default 10

Following fields are returned:

|Field|Type|Description
|---|---|---
uid|stri
name|string
surname|string
login|string
status|string
facility_uid|string
type|string
email|string
phone|string
office_name|string
      
                
### view_count_staff

Similar to view_list_staff, but it only counts the number of records matching the filter

|Field|Type|Description
|---|---|---
status|string|Filter on status ("active" or "inactive")
office_uid|string|Filter on this
organization_uid|string|Filter on this
fts|string|Filter on records having this string or substrig in name, surname, npi, login, email, phone


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
mode| alerts, todos or all| Default is "alerts"
group|string|"medical" or "staff"
class|string
type|string
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
mode| alerts, todos or all| Default is "alerts"
group|string|"medical" or "staff"
class|string
type|string
priority_min|integer|
priority_max|integer|

### view_list_notifications_readings (DEPRECATED)

Previous APIs exclude notifications with type 'patient_device_reading'. This isa specific API to return them

|Field|Type|Description
|---|---|---
topic|string|To use in pagination
start_time|string|Minimum time if order is asc, maximum if order is desc
size|string|Number of records to return (default 50)

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

### view_observations_last_device_by_label

Similar to view_list_observations, but it focus on getting last observation generated by this device (using label)

|Field|Type|Description
|---|---|---
device_label|string|

### view_observations_last_device_by_uid

Similar to view_list_observations, but it focus on getting last observation generated by this device (using device uid)

|Field|Type|Description
|---|---|---
device_uid|string|



### view_list_timeslots

|Field|Type|Description
|---|---|---
timeslot_uid|string|If provided, returns a single notification
cursor|string|Pagination
type|string|
user_uid|string
patient_uid|string
month|string|year+month string
start_time|string|starting time to consider
stop_time|string|stoppping time to coniser
visible|boolean|See bellow
size|string|Number of records to return (default 50)

By default, both visible and invisible entries will be returned. You can focus on any of them using 'visible' parameter


Following fields are returned:

|Field|Type|Description
|---|---|---
timeslot_uid|string
cursor|string|Pagination
type|string|
user_uid|string
patient_uid|string
month|string|year+month string
start_time|string|
duration|integer|In secs
visible|boolean|See bellow
extra|map

Order will be timeslots's time, descending. 


### view_count_timeslots

Similar to view_list_timeslots, but it only counts the number of records matching the filter

|Field|Type|Description
|---|---|---
timeslot_uid|string|If provided, returns a single notification
type|string|
user_uid|string
patient_uid|string
month|string|year+month string
start_time|string|starting time to consider
stop_time|string|stoppping time to consider
visible|boolean|See bellow

By default, both visible and invisible entries will be added. You can focus on any of them using 'visible' parameter


### view_report_timeslots

Similar to view_list_timeslots, but it focuses on a single patient and a period maximum of a month

|Field|Type|Description
|---|---|---
patient_uid|string (mandatory)
start_time|string|starting time to consider (madnatory)
stop_time|string|stoppping time to consider (mandatory)
type|string|
user_uid|string







