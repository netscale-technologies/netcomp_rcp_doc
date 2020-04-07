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
|hub_uid|string|Assigned hub UID or `null`
|patient_uid|string|Assigned patient for hub UID or `null`
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


### device_assign_patient

Allows to assign a patient to an existing device. Use `null` to detach any patient from the device.


|Field|Type|Mandatory|Description
|---|---|---|---
|device_uid|string|Y*|Unique UUID
|patient_uid|string or `null`|Y|



### is_rule_compatible_device (TO BE REMOVED)

Checks if a rule is compatible with a device

|Field|Type|Mandatory|Description
|---|---|---|---
|device_uid|string|Y|UID of the device
|ruleclass_uid|string|Y|UID of the ruleclass

Returns `{"is_compatible": "yes"|"no"}`


### add_device_rule (TO BE REMOVED)

Allows to add a new rule to the device.
This call *will not* check id a rule of the same type exists previously, and will always add a new rule to the device.


|Field|Type|Mandatory|Description
|---|---|---|---
|device_uid|string|Y|UID of the device
|ruleclass_uid|string|Y|UID of the ruleclass
|priority|number|N|0..999 Overwrites default's rule priority
|parameters|object|Y|Parameters specific for this rule type

Returns created rule_uid



### del_device_rule (TO BE REMOVED)

Allows to remove a rule from a device

|Field|Type|Mandatory|Description
|---|---|---|---
|device_uid|string|Y|UID of the device
|rule_uid|string|Y|UID of the ruleclass


### search_devices
Allows for a powerful search operation on devices


|Field|Type|Description
|---|---|---
|domain|string|Domain to filter on
|deep|boolean|If true, will search in sub-domains
|device_id|string|
|label|string|
|type|string|
|class|string|
|status|string|
|in_office|string|Office this device is assigned to. Use `null` for devices with no office
|with_hub|string|Filter devices connected to this hub. Use `null` for devices not connected
|with_patient|Filter devices connected to hubs connected to this patient. Use `null` for devices without any patient
|with_rule|string|Filter devices with this rule uids
|with_ruleclass|string|Filter devices hacing a rule based on this ruleclass.
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


### update_device_check_time_daily (TO BE REMOVED)

Allows to update device's check up daily time.
This call will delete any previous rule existing on the device of this same type.

|Field|Type|Mandatory|Description
|---|---|---|---
|device_uid|string|Y|Device UUID
|active|boolean|Y|To activate or deactivate
|timezone|string|N|Timezone the hour refers to (default GMT)
|hour|integer|N|Hour (0-23) if activate=true. Default is 12
|minute|integer|N|Minute (0-59) if activate=true. Default is 0

Returns ok or error


### update_device_weight_range (TO BE REMOVED)

Allows to update device's weight range (only valid for scales).
This call will delete any previous rule existing on the device of this same type.


|Field|Type|Mandatory|Description
|---|---|---|---
|device_uid|string|Y|Device UUID
|active|boolean|Y|To activate or deactivate
|min_weight_kgs|Y|
|max_weight_kgs|Y|

Returns ok or error


### update_device_bpm_range (TO BE REMOVED)

Allows to update device's bpm range (only valid for bpm).
This call will delete any previous rule existing on the device of this same type.


|Field|Type|Mandatory|Description
|---|---|---|---
|device_uid|string|Y|Device UUID
|active|boolean|Y|To activate or deactivate
|min_bpm|number|N|Default: 60
|max_bpm|number|N|Default: 100
|min_diastolic_mmHg|number|N|Default: 60
|max_diastolic_mmHg|number|N|Default: 100
|min_systolic_mmHg|number|N|Default: 110
|max_systolic_mmHg|number|N|Default: 160



Returns ok or error


### update_device_pulseoximeter_range (TO BE REMOVED)

Allows to update device's pulseoximeter range (only valid for pulseoximeters).
This call will delete any previous rule existing on the device of this same type.


|Field|Type|Mandatory|Description
|---|---|---|---
|device_uid|string|Y|Device UUID
|active|boolean|Y|To activate or deactivate
|min_bpm|number|N|Default: 60
|max_bpm|number|N|Default: 100
|min_spo2_pct|number|N|Default: 94.0
|max_spo2_pct|number|N|Default: 100.0
|min_pi_pct|number|N|Default: 0.2
|max_pi_pct|number|N|Default: 100.0

Returns ok or error


### update_device_thermometer_range (TO BE REMOVED)

Allows to update device's temperature range (only valid for thermometers).
This call will delete any previous rule existing on the device of this same type.


|Field|Type|Mandatory|Description
|---|---|---|---
|device_uid|string|Y|Device UUID
|active|boolean|Y|To activate or deactivate
|min_temperature_celsius_body|number|N|Default: 36.11
|max_temperature_celsius_body|number|N|Default: 37.78
|min_temperature_celsius_tympanum|number|N|Default: 36.11
|max_temperature_celsius_tympanum|number|N|Default: 37.78

Returns ok or error


### update_device_glucose_range (TO BE REMOVED)

Allows to update device's glucose range (only valid for glucuose meters).
This call will delete any previous rule existing on the device of this same type.


|Field|Type|Mandatory|Description
|---|---|---|---
|device_uid|string|Y|Device UUID
|active|boolean|Y|To activate or deactivate
|min_glucose_mg_dl|number|N|Default: 90.0
|max_glucose_mg_dl|number|N|Default: 150.0

Returns ok or error


### device_test_notification (TO BE REMOVED)

Forces a trigger of some rules applied to this device

|Field|Type|Mandatory|Description
|---|---|---|---
|device_uid|string|Y|Device UUID
|type|Y|string|"time", "weight", "bpm", "pulseoximeter", "thermometer", "glucose"

Returns ok or error


### move_device_to_environment (TO BE REMOVED)

Moves a device to a new environment
* Device must not be linked to a hub
* Device must not exists on remote environment

|Field|Type|Mandatory|Description
|---|---|---|---
|device_uid|string|Y|Device UUID
|environment|string|Y|`dev`, `qa` or `prod`

Returns ok or error
