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


