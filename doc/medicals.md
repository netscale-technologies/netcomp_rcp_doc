# API


## Medicals


### create_medical
Creates a new Medical object

|Field|Type|Mandatory|Description
|---|---|---|---
|id|string|N|Unique (for domain) id of medical, no special characters allowed. Generated if not included.
|domain|string|N|Domain to use (like "zone1.type")
|spec|spec|Y|See bellow

Spec object:

|Field|Type|Mandatory|Description
|---|---|---|---
|login|string|N|If present, can do login
|name|string|Y|Name (or full name) of user
|surname|string|N|Surname of user (optional)
|type|string|Y|Like "doctor". OPTIONS NOT YET DEFINED
|roles|\[string\]|N|List of positions or capabilities
|status|string|N|Like "available". . OPTIONS NOT YET DEFINED
|phone|string|N|Phone
|phone_sms|string|N|Phone
|phone_mpro|string|N|Phone
|email|string|N|Contact email
|timezone|string|N|Must be a valid timezone
|locale|string|N|Must be a valid locale
|address|address|N|See bellow
|offices|\[office]|N|See bellow
|npi|string|N|Medical's NPI


Address object:

|Field|Type|Mandatory|Description
|---|---|---|---
|street|string|Y|
|code|string|N|
|city|string|N|
|province|string|N|
|state|string|N|
|country|string|N|

Medical object:

|Field|Type|Mandatory|Description
|---|---|---|---
|id|string|Y|Id of a existing office

(MORE FIELDS TO BE DEFINED)


Returns:


|Field|Type|Description
|---|---|---
|uid|string|UID of the created medical
|id|string|Unique (in domain) id


### get_medical
Gets the data of an existing medical. Two formats available:

|Field|Type|Mandatory|Description
|---|---|---|---
|uid|string|Y|UID of medical


|Field|Type|Mandatory|Description
|---|---|---|---
|id|string|Y|ID of medical
|domain|string|N|Domain of creation


If successful, full information will be retrieved:

|Field|Type|Description
|---|---|---
|spec|spec|See bellow
|metadata|metadata|See bellow


Metadata description:

|Field|Type|Description
|---|---|---
|uid|string|Globally unique UID
|id|string|Id unique for domain
|creation_date|string|Creation date in RFC3339 format
|update_date|string|Last update date in RFC3339 format
|generation|integer|Increases at each update



### update_medical
Allows to update medical's data

|Field|Type|Mandatory|Description
|---|---|---|---
|uid|string|Y*|Unique UUID
|spec|spec|Y|Spec object

|Field|Type|Mandatory|Description
|---|---|---|---
|id|string|Y*|Unique ID for domain
|domain|string|N|Domain to use (like "zone1.type")
|spec|spec|Y|Spec object


Must supply either UID or Id (possibly with domain). Fields UID, id and domain cannot be modified.

All fields in spec should be included, or the missing ones will be deleted from object


Returns ok or error


### delete_medical

Allows to delete an existing medical. Same parameters as get_medical


### search_medicals
Allows for a powerful search operation on medicals


|Field|Type|Description
|---|---|---
|domain|string|Domain to filter on
|deep|boolean|If true, will search in sub-domains
|id|string|Generated id
|login|string|
|type|string|
|status|string|
|name|string|
|fts_name|string|See bellow
|surname|string|
|fts_surname|string|See bellow
|fts_fullname|string|See bellow
|phone|string|
|phone_sms|string|
|phone_mpro|string|
|timezone|string|
|email|string|
|in_office|binary|Filter for medicals belonging to this office
|in_organization|binary|Filter for medicals belonging to this organization|
|is_primary|boolean|
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
  * You can use a prefix to modify the behaviour: eq, ne, gt, gte, lt, lte. They work for string and numeric fields 
  * You can use a prefix: `"phone": "prefix:123"`
  * You can target several values: `"phone": "123|321"`
* Can sort on one or several fields, ascending or descending: `"sort": ["asc:update_time", "timezone"]` 
* Operations on field 'name' will be performed over a 'normalized' version of the field (without uppercase, utf8, etc.)

#### Full text search

FTS queries can be performed by using the field 'fts_name'. You can query for an specific word (`"fts_name": "medical"`) or a prefix of any word in the name using a tailing "*" (`"fts_name": "off*"`).


