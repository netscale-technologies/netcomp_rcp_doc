# API


## Timeslots


### create_timeslots
Creates a new Timeslot object

|Field|Type|Mandatory|Description
|---|---|---|---
|id|string|N|Unique (for domain) id of timeslot, no special characters allowed. Generated if not included.
|domain|string|N|Domain to use (like "zone1.type")
|spec|spec|Y|See bellow

Spec object:

|Field|Type|Mandatory|Description
|---|---|---|---
|type|string|Y|Type of timeslot. See bellow
|description|string|N|
|user_uid|string|Y|UID of medical or `"system"`
|patient_uid|string|Y|UID of patient
|notification_uid|string|N|UID of notification
|start_time|string|Y|Datetime of starting slot
|stop_time|string|N|Datetime of stopping slot (needed if no `duration_secs`)
|duration_secs|integer|N|Duration in secs (needed if no `stop_time`)
|tags|list of string|N|
|meta|object|N|Additional data

Returns:


|Field|Type|Description
|---|---|---
|uid|string|UID of the created timeslot
|id|string|Unique (in domain) id



### get_timeslot
Gets the data of an existing timeslot. Two formats available:

|Field|Type|Mandatory|Description
|---|---|---|---
|uid|string|Y|UID of timeslot


|Field|Type|Mandatory|Description
|---|---|---|---
|id|string|Y|ID of timeslot
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


### update_timeslot
Timeslots _cannot_ be updated


### delete_timeslot

Allows to delete an existing timeslot. Same parameters as get_timeslot


### search_timeslots
Allows for a powerful search operation on timeslots. This query *is not optimized* and can be slow. Use specific queries for production.


|Field|Type|Description
|---|---|---
|domain|string|Domain to filter on
|deep|boolean|If true, will search in sub-domains
|type|string|
|user_uid|string|
|patient_uid|string|
|notification_uid|string|
|start_time|string|
|stop_time|string|
|duration_secs|string|
|creation_date|string|Creation date in RFC3339 format
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


### search_timeslots_by_user
Finds, in an efficient manner, all timeslots inserted by an user


|Field|Type|Mandatory|Description
|---|---|---|---
|user_uid|string|Y|User generating slots
|expand_patients|boolean|N|If true, expand found patients
|start_time|string|Y|
|stop_time|string|Y|
|order|string|N|`asc` (default) or `desc`


Returns:

|Field|Type|Mandatory|Description
|---|---|---|---
|items|\[patient_slot\]|Y|Found slots
|patients|map|N|If `expand patients` is used, map with all found patients


patient_slot:

|Field|Type|Mandatory|Description
|---|---|---|---
|time|string|Y|
|duration_secs|integer|Y|
|type|integer|Y|
|patient_uid|integer|Y|
|timeslot_uid|integer|Y|


### search_timeslots_by_patient
Finds, in an efficient manner, all timeslots inserted for a patient


|Field|Type|Mandatory|Description
|---|---|---|---
|patient_uid|string|Y|Patient reported to
|expand_timeslots|boolean|N|If true, expand full timeslot details
|expand_users|boolean|N|If true, expand found users
|start_time|string|Y|
|stop_time|string|Y
|order|string|N|`asc` (default) or `desc`
|make_csv|boolean|N|If true, a CSV will be returned in field `csv`

Returns (no make_csv)

|Field|Type|Mandatory|Description
|---|---|---|---
|items|\[user_slot\]|Y|Found slots
|users|map|N|If `expand users` is used, map with all found users


user_slot:

|Field|Type|Mandatory|Description
|---|---|---|---
|time|string|Y|
|duration_secs|integer|Y|
|type|integer|Y|
|user_uid|integer|Y|
|timeslot_uid|integer|Y|



