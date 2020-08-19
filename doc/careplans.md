# API


## CarePlans


### create_careplan
Creates a new CarePlan object

|Field|Type|Mandatory|Description
|---|---|---|---
|id|string|N|Unique (for domain) id of careplan, no special characters allowed. Generated if not included.
|domain|string|N|Domain to use (like "zone1.type")
|spec|spec|Y|See bellow

Spec object:

|Field|Type|Mandatory|Description
|---|---|---|---
|patient_uid|string|Y|
|creator_uid|string|Y
|creation_time|string|Y|
|active|boolean|Y
|version|string|Y|
|reviewer_uid|string|N
|review_time|RFC3339|N
|editor_uid|string|N
|edition_time|RFC3339|N
|next_review_time|RFC3339|N
|validator_uid|string|N
|validation_time|RFC3339|N
|review_meeting_type|string|N|One in "phone", "in-person", "videocall"
|notes|string|N
|conditions| [condition] string|Y|

#### condition object

| Field | Type | Mandatory | Description |
| ---- | ---- | ---- | ---- |
| condition_id | string | Y | UID of CarePlanCondition |
| [goals] | [goal] | Y | See below |
| [last_month_goals] | [goal] | Y | See below |
| [symptoms] | [sympton] | Y | See below |
| [action_steps] | [action_step] | Y | See below |
| [barriers] | [barrier] | Y | See below |
| [support_tools] | [support_tool] | Y | See below |

#### goal object

| Field | Type | Mandatory | Description |
| ---- | ---- | ---- | ---- |
| goal_id | string | Y | UID of CarePlanGoal |
| value | string | N | Selected value if goal type is either "freetext" or "choice" |

#### symptom object

| Field | Type | Mandatory | Description |
| ---- | ---- | ---- | ---- |
| symptom_id | string | Y | UID of CarePlanSymptom |
| value | string | N | Selected value if symptom type is either "freetext" or "choice" |

#### action_step object

| Field | Type | Mandatory | Description |
| ---- | ---- | ---- | ---- |
| action_step_id | string | Y | UID of CarePlanActionStep |
| value | string | N | Selected value if action step type is either "freetext" or "choice" |

#### barrier object

| Field | Type | Mandatory | Description |
| ---- | ---- | ---- | ---- |
| barrier_id | string | Y | UID of CarePlanBarrier |
| value | string | N | Selected value if barrier type is either "freetext" or "choice" |

#### support_tool object

| Field | Type | Mandatory | Description |
| ---- | ---- | ---- | ---- |
| support_tool_id | string | Y | UID of CarePlanSupportTool |
| value | string | N | Selected value if goal type is either "freetext" or "choice" |



Returns:


|Field|Type|Description
|---|---|---
|uid|string|UID of the created careplan
|id|string|Unique (in domain) id


### get_careplan
Gets the data of an existing careplan. Two formats available:

|Field|Type|Mandatory|Description
|---|---|---|---
|uid|string|Y|UID of careplan


|Field|Type|Mandatory|Description
|---|---|---|---
|id|string|Y|ID of careplan
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



### update_careplan
Allows to update careplan's data

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


### delete_careplan

Allows to delete an existing careplan. Same parameters as get_careplan


### search_careplans
Allows for a powerful search operation on careplans


|Field|Type|Description
|---|---|---
|domain|string|Domain to filter on
|deep|boolean|If true, will search in sub-domains
|id|string|Id of careplan
|patient_uid|string|User friendly description
|creator_uid|string|
|creation_time|string|
|active|boolean|
|version|string|
|reviewer_uid| string \| null |
|review_time| RFC3339 \| null |
|editor_uid|string \| null |
|edition_time|RFC3339 \| null |
|next_review_time|RFC3339 \| null|
|validator_uid|string \| null |
|validation_time|RFC3339 \| null|
|review_meeting_type|string \| null|
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
