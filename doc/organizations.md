# API


## Organizations


### create_organization
Creates a new Organization object

|Field|Type|Mandatory|Description
|---|---|---|---
|id|string|N|Unique (for domain) id of organization, no special characters allowed. Generated if not included.
|domain|string|N|Domain to use (like "zone1.type")
|spec|spec|Y|See bellow

Spec object:

|Field|Type|Mandatory|Description
|---|---|---|---
|name|string|Y|User friendly description. Mandatory.
|url|string|N|URL or organization or clinic website
|phone|string|N|Phone
|phone_sms|string|N|SMS phone
|phone_mpro|string|N|Mpro phone
|account_id|string|N|
|email|string|N|Contact email
|address|address|N|See bellow
|partner_uid|string|Y|Partner that this organization belongs to
|send_reminders|boolean|N|Assumed yes if not present

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
|uid|string|UID of the created organization
|id|string|Unique (in domain) id


### get_organization
Gets the data of an existing organization. Two formats available:

|Field|Type|Mandatory|Description
|---|---|---|---
|uid|string|Y|UID of organization


|Field|Type|Mandatory|Description
|---|---|---|---
|id|string|Y|ID of organization
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
|url|string|URL or organization or clinic website
|phone|string|Phone
|email|string|Contact email
|address|address|Address
|partner_uid|string|Partner UID

Metadata description:

|Field|Type|Description
|---|---|---
|uid|string|Globally unique UID
|id|string|Id unique for domain
|creation_date|string|Creation date in RFC3339 format
|update_date|string|Last update date in RFC3339 format
|generation|integer|Increases at each update



### update_organization
Allows to update organization's data

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


### delete_organization

Allows to delete an existing organization. Same parameters as get_organization

### update_organization_status
Allows to update organization's status and status reason

|Field|Type|Mandatory|Description
|---|---|---|---
|uid|string|Y|Unique UUID
|status|spec|Y|Current status
|reason|spec|Y|Reason for change



### search_organizations
Allows for a powerful search operation on organizations


|Field|Type|Description
|---|---|---
|domain|string|Domain to filter on
|deep|boolean|If true, will search in sub-domains
|id|string|Actor generated id
|name|string|User friendly description
|status|string|Filter having this status
|fts_name|string|See bellow
|url|string|URL or organization or clinic website
|phone|string|Phone
|phone_sms|string|Phone
|phone_mpro|string|Phone
|account_id|string|
|email|string|Contact email
|in_partner|string|Partner UID
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

FTS queries can be performed by using the field 'fts_name'. You can query for an specific word (`"fts_name": "organization"`) or a prefix of any word in the name using a tailing "*" (`"fts_name": "off*"`).


