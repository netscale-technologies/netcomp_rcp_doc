# API


## DeviceHubs


### create_devicehub
Creates a new DeviceHub object

|Field|Type|Mandatory|Description
|---|---|---|---
|spec|spec|Y|See bellow

Spec object:

|Field|Type|Mandatory|Description
|---|---|---|---
|hub_id|string|Y|RCP's id of hub (like 202481593979652)
|label|string|Y|Label of device
|status|string|N|Like "available". . OPTIONS NOT YET DEFINED
|meta|map|N|
|test_device|boolean|N|If true, no real connection is done
|device_is_master|boolean|N|If true, devices are copied from device and overwritten in db
|location|string|N|To be used by Odoo
|financially_liable|string|N|To be used by Odoo


Returns:


|Field|Type|Description
|---|---|---
|uid|string|UID of the created devicehub


### get_devicehub
Gets the data of an existing devicehub.

|Field|Type|Mandatory|Description
|---|---|---|---
|uid|string|Y|UID of devicehub


If successful, full information will be retrieved:

|Field|Type|Description
|---|---|---
|spec|spec|See bellow
|status|status|See bellow
|metadata|metadata|See bellow


Status description:

|Field|Type|Description
|---|---|---
|patient|patient|See bellow
|office|patient|See bellow
|owner|patient|See bellow
|os_vsn|string|
|app_vsn|string|
|last_status|string|
|last_status_time|date|
|state_hash|string|
|last_serial|date|
|gps_data|object|
|gsm_data|object|

Patient info:

|Field|Type|Description
|---|---|---
|uid|string|Current UID or `null`
|prev_uid|string|Previous UID
|time|string|Time of last change

Office info:

|Field|Type|Description
|---|---|---
|uid|string|Current UID or `null`
|prev_uid|string|Previous UID
|time|string|Time of last change

Owner info:

|Field|Type|Description
|---|---|---
|type|string|Current type: `null`, `rcp`, `office` or `patient` 
|uid|string|Current UID or `null`
|prev_type|string|Previous type
|prev_uid|string|Previous UID
|time|string|Time of last change


Metadata description:

|Field|Type|Description
|---|---|---
|uid|string|Globally unique UID
|creation_date|string|Creation date in RFC3339 format
|update_date|string|Last update date in RFC3339 format
|generation|integer|Increases at each update


### update_devicehub
Allows to update devicehub's data
For example, allows to assign it to a patient

|Field|Type|Mandatory|Description
|---|---|---|---
|uid|string|Y*|Unique UUID
|spec|spec|Y|Spec object

All fields in spec should be included, or the missing ones will be deleted from object


Returns ok or error


### delete_devicehub

Allows to delete an existing devicehub. Same parameters as get_devicehub


### search_devicehubs
Allows for a powerful search operation on devicehubs


|Field|Type|Description
|---|---|---
|domain|string|Domain to filter on
|deep|boolean|If true, will search in sub-domains
|hub_id|string|
|label|string|
|status|string|
|with_patient|binary|Filter for devicehubs linked to this patient. Use `null` to find hubs not associated to any patient.
|in_office|binary|Office this device is assigned to. Use `null` to find hubs without office.
|in_organization|binary|Organization this device is assigned to
|with_owner_type|binary|Owner type or `null`
|with_owner_uid|binary|Owner uid or `null`
|expand_patients|boolean|If true, field `extra` will appear with additional info (patient, office and organization)
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
  * You can use a prefix to modify the behaviour: eq, ne, gt, gte, lt, lte. They work for string and numeric fields (like timezone): `"timezone": "gte:2"` 
  * You can use a prefix: `"phone": "prefix:123"`
  * You can target several values: `"phone": "123|321"`
* Can sort on one or several fields, ascending or descending: `"sort": ["asc:update_time", "timezone"]` 
* Operations on field 'name' will be performed over a 'normalized' version of the field (without uppercase, utf8, etc.)


### assign_patient_to_devicehub
Assigns the hub to an existing patient

|Field|Type|Mandatory|Description
|---|---|---|---
|devicehub_uid|string|Y|UID of the Hub
|patient_uid|string|Y|UID of the patient. `null` to remove
|force_date|string|N|If present, updates the assignation date of patient. Device must be already assigned.


### assign_office_to_devicehub
Assigns the hub to an existing office

|Field|Type|Mandatory|Description
|---|---|---|---
|devicehub_uid|string|Y|UID of the Hub
|office_uid|string|Y|UID of office. `null` to remove


### assign_owner_to_devicehub
Assigns the hub to an owner

|Field|Type|Mandatory|Description
|---|---|---|---
|devicehub_uid|string|Y|UID of the Hub
|owner_type|string|Y|Can be `rcp`, `office` or `patient`
|owner_uid|string|N|If available, UID of office or patient


### move_devicehub_to_environment

Moves a hub to a new environment
* Hub must not be linked to a patient
* Hub must not have devices
* Hub must not exists on remote environment

|Field|Type|Mandatory|Description
|---|---|---|---
|devicehub_uid|string|Y|Device UUID
|environment|string|Y|`dev`, `qa` or `prod`

Returns ok or error


