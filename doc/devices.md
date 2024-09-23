# API


## Devices


### create_device
Creates a new Device object

|Field|Type|Mandatory|Description
|---|---|---|---
|spec|spec|Y|See bellow

Spec object:

|Field|Type|Mandatory|Description
|---|---|---|---
|device_id|string|Y|MAC address
|label|string|Y|Label of device
|status|string|N|Like "available". . OPTIONS NOT YET DEFINED
|office_uid|string|N|Office this device is assigned to
|location|string|N|To be used by Odoo
|financially_liable|string|N|To be used by Odoo


Returns:

|Field|Type|Description
|---|---|---
|uid|string|UID of the created device


### get_device
Gets the data of an existing device. 

|Field|Type|Mandatory|Description
|---|---|---|---
|uid|string|Y|UID of device


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
|battery|number|
|class|string|
|type|string|See bellow for available types
|name|string|
|description|string|
|model|string|
|vendor|string|
|protocol|string|
|last_readings|\[reading]\|
|rules|\[rule\]|Current rules
|check_time_daily|check_time_daily|

Reading description

|Field|Type|Description
|---|---|---
|date|string|
|reading|object|


Rule description

|Field|Type|Description
|---|---|---
|rule_uid|string|UID of rule
|ruleclass_uid|string|UID of rule
|class|string|Class of the rule
|type|string|Type of the rule
|priority|integer|
|parameters|map|Parameters specific for this rule
|meta|map|Internal information for the rule


Check time daily

|Field|Type|Description
|---|---|---
|timezone|string|
|hour|object|
|minute|object|

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



#### Type

The currently defined types are:
 * scale
 * pulseoximeter
 * bpm
 * thermometer
 * glucose
 * spirometer
 * heartrate




Metadata description:

|Field|Type|Description
|---|---|---
|uid|string|Globally unique UID
|creation_date|string|Creation date in RFC3339 format
|update_date|string|Last update date in RFC3339 format
|generation|integer|Increases at each update


### update_device
Allows to update device's data
For example, allows to assign it to a hub

|Field|Type|Mandatory|Description
|---|---|---|---
|uid|string|Y*|Unique UUID
|spec|spec|Y|Spec object

All fields in spec should be included, or the missing ones will be deleted from object


Returns ok or error


### delete_device

Allows to delete an existing device. Same parameters as get_device


### assign_patient_to_device
Assigns the device to an existing patient

|Field|Type|Mandatory|Description
|---|---|---|---
|device_uid|string|Y|UID of the Device
|patient_uid|string|Y|UID of the patient. `null` to remove
|force_date|string|N|If present, updates the assignation date of patient. Device must be already assigned.


### assign_office_to_device
Assigns the device to an existing office

|Field|Type|Mandatory|Description
|---|---|---|---
|device_uid|string|Y|UID of the Device
|office_uid|string|Y|UID of office. `null` to remove


### assign_owner_to_device
Assigns the device to an owner

|Field|Type|Mandatory|Description
|---|---|---|---
|device_uid|string|Y|UID of the Hub
|owner_type|string|Y|Can be `rcp`, `office` or `patient`
|owner_uid|string|N|If available, UID of office or patient

