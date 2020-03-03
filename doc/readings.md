# API


## Readings
Readings represent raw data received from a device.
Observations will be created based on readings


### create_reading
Creates a new Reading object

|Field|Type|Mandatory|Description
|---|---|---|---
|id|string|N|Unique (for domain) id of reading, no special characters allowed. Generated if not included.
|domain|string|N|Domain to use (like "zone1.type")
|spec|spec|Y|See bellow

Spec object:

|Field|Type|Mandatory|Description
|---|---|---|---
|type|string|Y|Type of reading. See bellow
|device_id|string|Y|UID of device that generated this
|device_uid|string|Y|UID of device that generated this
|device_type|string|Y|Device Type
|device_protocol|string|Y|Device Protocol
|hub_id|string|Y|UID of device that generated this
|hub_uid_uid|string|Y|UID of device that generated this
|hub_serial|integer|Y|Internal serial of hub
|patient_uid|string|Y|UID of patient
|data|object|Y|Actual reading data. See bellow.
|time|string|Y|Datetime of reading
|meta|object|H|Additional data

Returns:


|Field|Type|Description
|---|---|---
|uid|string|UID of the created reading
|id|string|Unique (in domain) id

The field 'meta' will have the following format:

|Field|Type|Description
|---|---|---
|values|array|List of values used for computation
|times|array|List of times of values
|session|string|Device session
|extra|object|See bellow


### get_reading
Gets the data of an existing reading. Two formats available:

|Field|Type|Mandatory|Description
|---|---|---|---
|uid|string|Y|UID of reading


|Field|Type|Mandatory|Description
|---|---|---|---
|id|string|Y|ID of reading
|domain|string|N|Domain of creation


If successful, full information will be retrieved:

|Field|Type|Description
|---|---|---
|spec|spec|See above
|metadata|metadata|See bellow


Metadata description:

|Field|Type|Description
|---|---|---
|uid|string|Globally unique UID
|id|string|Id unique for domain
|creation_date|string|Creation date in RFC3339 format
|update_date|string|Last update date in RFC3339 format
|generation|integer|Increases at each update


### update_reading
Readings _cannot_ be updated


### delete_reading

Allows to delete an existing reading. Same parameters as get_reading


### search_readings
Allows for a powerful search operation on readings


|Field|Type|Description
|---|---|---
|domain|string|Domain to filter on
|deep|boolean|If true, will search in sub-domains
|type|string|
|device_id|string|
|device_uid|string|
|device_type|string|
|device_protocol|string|
|hub_id|string|
|hub_uid|string|
|hub_serial|integer|
|patient_uid|string|
|start_time|string|
|stop_time|string|
|creation_date|string|Creation date in RFC3339 format
|update_date|string|Last update date in RFC3339 format
|from|integer|Starting point (for pagination)
|size|integer|Number or records to retrieve
|get_total|boolean|If true, calculates the grand total of records
|sort|\[string\]|List of fields to sort on

* Field `time` can also be used for sort
* Can filter on domain, or domain and subdomains using 'deep'
* Can paginate with from and size
* Can filter on any of the previous fields, or any number of them, working like and 'and'
  * If you set a raw string, it will find objects with that exact string: `"phone": "123"`
  * You can use a prefix to modify the behaviour: eq, ne, gt, gte, lt, lte. They work for string and numeric fields (like timezone): `"timezone": "gte:2"` 
  * You can use a prefix: `"phone": "prefix:123"`
  * You can target several values: `"phone": "123|321"`
* Can sort on one or several fields, ascending or descending: `"sort": ["asc:update_time", "timezone"]` 
* Operations on field 'name' will be performed over a 'normalized' version of the field (without uppercase, utf8, etc.)



# Readings type and data

The format of the fields `data` and `meta` depends on the field `device_protocol`.
See observations documentation for description of fields
