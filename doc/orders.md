# API


## Orders


### create_order
Creates a new Order object

|Field|Type|Mandatory|Description
|---|---|---|---
|id|string|N|Unique (for domain) id of order, no special characters allowed. Generated if not included.
|domain|string|N|Domain to use (like "zone1.type")
|spec|spec|Y|See bellow

Spec object:

|Field|Type|Mandatory|Description
|---|---|---|---
|patient_uid|N|If present, will copy patient's info
|name|string|N|Mandatory if not patient_uid
|surname|string|N|
|phone|string|N|
|phone_sms|string|N|
|email|string|N|Contact email
|timezone|string|N|
|locale|string|N|
|address|address|N|See bellow
|device_types| [string] |Y|Like 'scale'
|meta|map|N|


Address object:

|Field|Type|Mandatory|Description
|---|---|---|---
|street|string|Y|
|code|string|N|
|city|string|N|
|province|string|N|
|state|string|N|
|country|string|N|


Returns:


|Field|Type|Description
|---|---|---
|uid|string|UID of the created order
|id|string|Unique (in domain) id


### get_order
Gets the data of an existing order. Two formats available:

|Field|Type|Mandatory|Description
|---|---|---|---
|uid|string|Y|UID of order


|Field|Type|Mandatory|Description
|---|---|---|---
|id|string|Y|ID of order
|domain|string|N|Domain of creation


If successful, full information will be retrieved:

|Field|Type|Description
|---|---|---
|spec|spec|See bellow
|status|spec|See bellow
|metadata|metadata|See bellow

Status:

|Field|Type|Description
|---|---|---
|last_status|string|
|last_status_time|string|
|events| [event] |List of past events


Metadata description:

|Field|Type|Description
|---|---|---
|uid|string|Globally unique UID
|id|string|Id unique for domain
|creation_date|string|Creation date in RFC3339 format
|update_date|string|Last update date in RFC3339 format
|generation|integer|Increases at each update



### update_order
Allows to update order's data

|Field|Type|Mandatory|Description
|---|---|---|---
|uid|string|Y|Unique UUID
|spec|spec|Y|Spec object

|Field|Type|Mandatory|Description
|---|---|---|---
|id|string|Y|Unique ID for domain
|domain|string|N|Domain to use (like "zone1.type")
|spec|spec|Y|Spec object

Must supply either UID or Id (possibly with domain). Fields UID, id and domain cannot be modified.

All fields in spec should be included, or the missing ones will be deleted from object


Returns ok or error


### delete_order

Allows to delete an existing order. Same parameters as get_order


### search_orders
Allows for a powerful search operation on orders


|Field|Type|Description
|---|---|---
|domain|string|Domain to filter on
|deep|boolean|If true, will search in sub-domains
|id|string|Actor generated id
|name|string|User friendly description
|fts_name|string|See bellow
|type|string|
|status|string|N|
|assignee_uid|string|N|
|quantity|integer|
|phone|string|Phone
|email|string|Contact email
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

#### Full text search

FTS queries can be performed by using the field 'fts_name'. You can query for an specific word (`"fts_name": "order"`) or a prefix of any word in the name using a tailing "*" (`"fts_name": "off*"`).


### add_order_status
Allows to add a new status to an order data

|Field|Type|Mandatory|Description
|---|---|---|---
|uid|string|Y|Unique UUID
|status|string|Y|Like "sent", "received"
|meta|map|N|

