# API


## Device Classes


### create_deviceclass
Creates a new DeviceClass object

|Field|Type|Mandatory|Description
|---|---|---|---
|spec|spec|Y|See bellow

Spec object:

|Field|Type|Mandatory|Description
|---|---|---|---
|class|string|Y|Like "SCT01"
|type|string|Y|Like "scale"
|name|string|N|Like "Renpho"
|model|string|N|Like "R5a"
|description|string|N|
|vendor|string|N|
|protocol|string|Y|
|module|string|Y|Erlang module supporting the protocol

Returns:

|Field|Type|Description
|---|---|---
|uid|string|UID of the created deviceclass


### get_deviceclass
Gets the data of an existing deviceclass. 

|Field|Type|Mandatory|Description
|---|---|---|---
|uid|string|Y|UID of deviceclass


If successful, full information will be retrieved:

|Field|Type|Description
|---|---|---
|spec|spec|See bellow
|metadata|metadata|See bellow


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


### update_deviceclass
Allows to update deviceclass's data
For example, allows to assign it to a hub

|Field|Type|Mandatory|Description
|---|---|---|---
|uid|string|Y*|Unique UUID
|spec|spec|Y|Spec object

All fields in spec should be included, or the missing ones will be deleted from object


Returns ok or error


### delete_deviceclass

Allows to delete an existing deviceclass. Same parameters as get_deviceclass


### search_deviceclasss
Allows for a powerful search operation on deviceclasss


|Field|Type|Description
|---|---|---
|domain|string|Domain to filter on
|deep|boolean|If true, will search in sub-domains
|class|string|
|type|string|
|name|string|
|model|string|
|description|
|vendor|string|
|protocol|string|
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
