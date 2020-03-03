# API


## DeviceBox

A `devicebox` is a device (hub or medical device) that has no yet being assigned and ready to be used

The expected process is:

* A new devicebox is created, primary key is `class+batch+serial`
* Parameters `status` and `location` can be used on creation, or later updated
* A brand new `label` will be created and associated to the devicebox, based on class and batch
* At some moment, you can _promote_ the devicebox, creating a _device_ from it. You must provide `device_id` (_serial_ for hubs and _mac_ for medical devices) and `meta`. At this moment de devicebox is created with a new uid, and the devicebox object cannot be assigned again

### create_devicebox
Creates a new DeviceHub object

|Field|Type|Mandatory|Description
|---|---|---|---
|spec|spec|Y|See bellow

Spec object:

|Field|Type|Mandatory|Description
|---|---|---|---
|class|string|Y|Device class
|batch|string|Y|Device batch
|serial|string|Y|Device serial
|status|string|N|Like "available", etc.
|location|string|N|
|meta|map|N|


Returns:

|Field|Type|Description
|---|---|---
|uid|string|UID of the created devicebox
|label|string|Generated label


### get_devicebox
Gets the data of an existing devicebox. 

|Field|Type|Mandatory|Description
|---|---|---|---
|class|string|Y|Device class
|batch|string|Y|Device batch
|serial|string|Y|Device serial

In any case, if successful, full information will be retrieved:

|Field|Type|Description
|---|---|---
|spec|spec|See above
|status|status|Only if the device is already assigned. See bellow.
|metadata|metadata|See bellow


Status description:

|Field|Type|Description
|---|---|---
|label|uppercase|Generated label for devicebox
|device_id|uppercase|Hub serial or device MAC
|device_uid|UID of generated device actor

Metadata description:

|Field|Type|Description
|---|---|---
|uid|string|Globally unique UID
|creation_date|string|Creation date in RFC3339 format
|update_date|string|Last update date in RFC3339 format
|generation|integer|Increases at each update


### update_devicebox
Allows to update devicebox's data

|Field|Type|Mandatory|Description
|---|---|---|---
|updated_linked|boolean|If true, and the real device as already being created, try to update `meta` field
|spec|spec|Y|Spec object (must include class, batch and serial)

All fields in spec should be included, or the missing ones will be deleted from object

Returns ok or error


### delete_devicebox

Allows to delete an existing devicebox. 

|Field|Type|Mandatory|Description
|---|---|---|---
|class|string|Y|Device class
|batch|string|Y|Device batch
|serial|string|Y*|Serial Id
|delete_linked|boolean|If true, and the real device as already being created, try to delete it


### search_deviceboxes
Allows for a powerful search operation on devices
If the real device has already being created, fields `with_label`, `with_device_id` and `with_device_uid` can be used


|Field|Type|Description
|---|---|---
|class|string|
|batch|string|
|serial|string|
|status|string|
|location|string|
|device_is_created|boolean|Find devicebox with an already created device or not
|label|string|
|device_id|string|
|device_uid|string|
|creation_date|string|Creation date in RFC3339 format
|update_date|string|Last update date in RFC3339 format
|from|integer|Starting point (for pagination)
|size|integer|Number or records to retrieve
|get_total|boolean|If true, calculates the grand total of records
|sort|\[string\]|List of fields to sort on

* Can paginate with from and size
* Can filter on any of the previous fields, or any number of them, working like and 'and'
  * If you set a raw string, it will find objects with that exact string: `"phone": "123"`
  * You can use a prefix: `"phone": "prefix:123"`
  * You can target several values: `"phone": "123|321"`
* Can sort on one or several fields, ascending or descending: `"sort": ["asc:update_time", "timezone"]` 
* Operations on field 'name' will be performed over a 'normalized' version of the field (without uppercase, utf8, etc.)


### promote_devicebox_to_device

Allows to _assign_ the box, creating a real device or hub

Two formats are available:

|Field|Type|Mandatory|Description
|---|---|---|---
|uid|string|Y|
|device_id|uppercase|Y|Hub serial or device MAC


|Field|Type|Mandatory|Description
|---|---|---|---
|class|string|Y|
|batch|string|Y|
|serial|string|Y|Box serial
|device_id|uppercase|Y|Hub serial or device MAC

A new device or devicehub will be created based on this data.

Returns ok or error


