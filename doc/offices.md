# API


## Offices


### create_office
Creates a new Office object

|Field|Type|Mandatory|Description
|---|---|---|---
|id|string|N|Unique (for domain) id of office, no special characters allowed. Generated if not included.
|domain|string|N|Domain to use (like "zone1.type")
|spec|spec|Y|See bellow

Spec object:

|Field|Type|Mandatory|Description
|---|---|---|---
|name|string|Y|User friendly description. Mandatory.
|url|string|N|URL or office or clinic website
|phone|string|N|Phone
|phone_sms|string|N|SMS phone
|phone_mpro|string|N|Mpro phone
|account_id|string|N|
|email|string|N|Contact email
|address|address|N|See bellow
|timezone|string|N|Must be a valid timezone
|organization_uid|string|N|UID of valid organization
|avatar|string|N|



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
|uid|string|UID of the created office
|id|string|Unique (in domain) id


### get_office
Gets the data of an existing office. Two formats available:

|Field|Type|Mandatory|Description
|---|---|---|---
|uid|string|Y|UID of office


|Field|Type|Mandatory|Description
|---|---|---|---
|id|string|Y|ID of office
|domain|string|N|Domain of creation


If successful, full information will be retrieved:

|Field|Type|Description
|---|---|---
|spec|spec|See bellow
|metadata|metadata|See bellow


Spec description:

|Field|Type|Description
|---|---|---
|name|string|User friendly description. Mandatory.
|url|string|URL or office or clinic website
|phone|string|Phone
|phone_sms|string|Phone
|phone_mpro|string|Phone
|account_id|string|Phone
|email|string|Contact email
|address|address|Address
|avatar|string|


Metadata description:

|Field|Type|Description
|---|---|---
|uid|string|Globally unique UID
|id|string|Id unique for domain
|creation_date|string|Creation date in RFC3339 format
|update_date|string|Last update date in RFC3339 format
|generation|integer|Increases at each update



### update_office
Allows to update office's data

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


### delete_office

Allows to delete an existing office. Same parameters as get_office


### search_offices
Allows for a powerful search operation on offices


|Field|Type|Description
|---|---|---
|domain|string|Domain to filter on
|deep|boolean|If true, will search in sub-domains
|id|string|Id of office
|name|string|User friendly description
|fts_name|string|See bellow
|url|string|URL or office or clinic website
|phone|string|Phone
|phone_sms|string|Phone
|phone_mpro|string|Phone
|account_id|string|
|in_organization|string|Use `null` to find offices with no organization
|timezone|string|Phone
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

FTS queries can be performed by using the field 'fts_name'. You can query for an specific word (`"fts_name": "office"`) or a prefix of any word in the name using a tailing "*" (`"fts_name": "off*"`).


