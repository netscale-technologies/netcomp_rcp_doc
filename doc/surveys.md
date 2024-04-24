# API


## Surveys



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


### surveys_set_reviewed

Marks an existing survey launch as 'reviewed'

|Field|Type|Mandatory|Description
|---|---|---|---
|surveys_uid|string|Y|UID of summary launch to update
|reviewer_uid|string|Y|UID of reviewer

If survey launch is not already reviewed, a new object "review" will be added to 'extra', containing "reviewer_uid" and "time",
and `{"result": true}` will be returned.

If it is already reviewed (field "extra.review" exists), nothing is changed and  `{"result": false}` is returned

