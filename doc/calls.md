# API


## Calls


### create_call
Creates a new Call object

|Field|Type|Mandatory|Description
|---|---|---|---
|id|string|N|Unique (for domain) id of call, no special characters allowed. Generated if not included.
|domain|string|N|Domain to use (like "zone1.type")
|spec|spec|Y|See bellow

Spec object:

|Field|Type|Mandatory|Description
|---|---|---|---
|patient_uid|string|Y|
|caller_uid|string|Y|




Returns:


|Field|Type|Description
|---|---|---
|uid|string|UID of the created call
|id|string|Unique (in domain) id


### get_call
Gets the data of an existing call. Two formats available:

|Field|Type|Mandatory|Description
|---|---|---|---
|uid|string|Y|UID of call


|Field|Type|Mandatory|Description
|---|---|---|---
|id|string|Y|ID of call
|domain|string|N|Domain of creation


If successful, full information will be retrieved:

|Field|Type|Description
|---|---|---
|spec|spec|See above
|status|status|See bellow
|metadata|metadata|See bellow


Metadata description:

|Field|Type|Description
|---|---|---
|uid|string|Globally unique UID
|id|string|Id unique for domain
|creation_date|string|Creation date in RFC3339 format
|update_date|string|Last update date in RFC3339 format
|generation|integer|Increases at each update


Status description:


|Field|Type|Description
|---|---|---
|status|string|Current status: launched, finished
|last_status_time|string|Date of last status update
|error_code|string|
|error_text|string|
|expire_time|string|
|meta|object|Added metadata



### delete_call

Allows to delete an existing call. Same parameters as get_call


### search_calls
Allows for a powerful search operation on calls


|Field|Type|Description
|---|---|---
|domain|string|Domain to filter on
|deep|boolean|If true, will search in sub-domains
|id|string|Id of call
|patient_uid|string|
|caller_uid|string|
|status|string|
|last_status_time|string|
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


### launch_call

Launches a created script manually

| Field | Type | Mandatory | Description |
| ---- | ---- | ---- | ---- |
| uid | string | Y | UID of call
| meta | map| N |

### finish_call

Allows to simulate an answer received for a call

|Field|Type|Mandatory|Description
|---|---|---|---
|uid|string|Y|
|meta|object|N|Added metadata


