# API


## Surveys


### create_survey
Creates a new Survey object

|Field|Type|Mandatory|Description
|---|---|---|---
|id|string|N|Unique (for domain) id of survey, no special characters allowed. Generated if not included.
|domain|string|N|Domain to use (like "zone1.type")
|spec|spec|Y|See bellow

Spec object:

|Field|Type|Mandatory|Description
|---|---|---|---
|patient_uid|string|Y|
|script_id|string|Y|
|ruleclass_uid|string|Y|
|rule_uid|string|Y|
|type|string|Y|



Returns:


|Field|Type|Description
|---|---|---
|uid|string|UID of the created survey
|id|string|Unique (in domain) id


### get_survey
Gets the data of an existing survey. Two formats available:

|Field|Type|Mandatory|Description
|---|---|---|---
|uid|string|Y|UID of survey


|Field|Type|Mandatory|Description
|---|---|---|---
|id|string|Y|ID of survey
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
|status|string|Current status
|last_status_time|string|Date of last status update
|questions| [question] |List of questions
|error_code|string
|error_text|string
|expire_time|string
|meta|object|Added metadata





#### question object

| Field | Type | Mandatory | Description |
| ---- | ---- | ---- | ---- |
| id | string | Y | ID of question from Call Communication |
| question | map | Y | Question text ("en": "text") |
| answer | map | N | Answer text. If missing it means question was not launched because of the script execution flow |
| status | string | N | Either "ok", "warning" or "alert". If missing it means question was not launched because of the script execution flow |





### update_survey
Allows to update survey's data

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


### delete_survey

Allows to delete an existing survey. Same parameters as get_survey


### search_surveys
Allows for a powerful search operation on surveys


|Field|Type|Description
|---|---|---
|domain|string|Domain to filter on
|deep|boolean|If true, will search in sub-domains
|id|string|Id of survey
|patient_uid|string|
|script_id|string|
|ruleclass_uid|string|
|rule_uid|string|
|type|string|
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


### launch_survey

Launches a created script manually

| Field | Type | Mandatory | Description |
| ---- | ---- | ---- | ---- |
| uid | string | Y | UID of survey


### answer_survey

Allows to simulate an answer received for a survey

|Field|Type|Mandatory|Description
|---|---|---|---
|uid|string|Y|
|questions| [question] |Y|List of questions
|meta|object|N|Added metadata


