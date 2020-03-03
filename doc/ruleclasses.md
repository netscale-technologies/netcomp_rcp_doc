# API


## RuleClasses


### create_ruleclass
Creates a new RuleClass object

|Field|Type|Mandatory|Description
|---|---|---|---
|id|string|N|Unique (for domain) id of rule, no special characters allowed. Generated if not included.
|domain|string|N|Domain to use (like "zone1.type")
|spec|spec|Y|See bellow

Spec object:

|Field|Type|Mandatory|Description
|---|---|---|---
|class|string|Y|Class for the ruleclass
|type|string|Y|For a class, a specific type
|description|string|N|
|module|string|Y|Erlang module supporting the rule implementation
|tags|\[string\]|Y|List of tags to aid in searching rules
|parameters|\[parameter_obj\]|Y|List of necessary parameters


Parameters object:

|Field|Type|Mandatory|Description
|---|---|---|---
|name|string|Y|Parameter name (like 'weight_kgs')
|type|string|Y|Parameter type ('string', integer', 'float', 'date')
|min|number|N|Minimum possible accepted value (for the ruleclass to be accepted)
|max|number|N|Maximum possible accepted value (for the ruleclass to be accepted)


Returns:


|Field|Type|Description
|---|---|---
|uid|string|UID of the created ruleclass
|id|string|Unique (in domain) id


### get_ruleclass
Gets the data of an existing rule. Two formats available:

|Field|Type|Mandatory|Description
|---|---|---|---
|uid|string|Y|UID of ruleclass


|Field|Type|Mandatory|Description
|---|---|---|---
|id|string|Y|ID of ruleclass
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



### update_ruleclass
Allows to update ruleclass's data

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

Updating a ruleclass *is not recomended* as existing rules based on this ruleclass could be affected


Returns ok or error


### delete_ruleclass

Allows to delete an existing ruleclass. Same parameters as get_ruleclass.


### search_ruleclasses
Allows for a powerful search operation on ruleclasses


|Field|Type|Description
|---|---|---
|domain|string|Domain to filter on
|deep|boolean|If true, will search in sub-domains
|class|string|
|type|string|
|with_tag|string|Like 'scale'
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


## Available ruleclasses

### Device-related ruleclasses

They have class `"devices"`

When they generate notifications, they will have class `patient_device_alert`. Metadata will include:

|Field|Type|Description
|---|---|---
|is_test|boolean|`true` if it is a test
|type|string|Like "device_reading_out_of_range"
|time|string|
|device|object|Info about device
|hub|object|Info about hub
|patient|object|Info about patient
|office|object|Info about office
|observation|object|Info about the full observation
|rule|object|Info about the rule that fired the notification: type, class, rule_uid, ruleclass_uid
|alerts|object|Map of alerts (see bellow)

For each detected alert, an entry in `alerts` will be inserted. The key will be the field with abnormal value (like `weight_kgs`) and the value will be the folling object:

|Field|Description
|---|---
|value|Problematic value
|min|Configured minimum for value
|max|Configured maximum for value


#### Missing reading


Can be applied to any device. It will wake up daily, at the specified time, and it will check if there are some new reading available from midnight to the wake up time.

For now it only sends a notification using SMS

Class is `devices`, type is `missing_reading_daily`. Supported parameters are:


|Name|Description
|---|---
|timezone|Timezone the time refers to. Default is GMT
|daily_hour|Hour (0..23)
|daily_minute|Minute (0..59)


#### Weight range

Can be applied to all current scales. It will check, on each reading, that is is inside the configured values

Class is `devices`, type is `weight_range`. Supported parameters are:


|Name|Description
|---|---
|min_weight_kgs|
|max_weight_kgs|


#### BPM range

Can be applied to all current bpms. 
Class is `devices`, type is `bpm_range`. Supported parameters are:


|Name|Description
|---|---
|min_bpm|
|max_bpm|
|min_diastolic_mmHg|
|max_diastolic_mmHg|
|min_systolic_mmHg|
|max_systolic_mmHg|


#### Pulseoximeter range

Can be applied to all current pulseoximeters. 

Class is `devices`, type is `pulseoximeter_range`. Supported parameters are:


|Name|Description
|---|---
|min_bpm|
|max_bpm|
|min_spo2_pct|
|max_spo2_pct|
|min_pi_pct|
|max_pi_pct|

#### Thermometer range

Can be applied to all current thermometers.

Class is `devices`, type is `thermometer_range`. Supported parameters are:


|Name|Description
|---|---
|min_temperature_celsius_body|
|max_temperature_celsius_body|
|min_temperature_celsius_tympanum|
|max_temperature_celsius_tympanum|

#### Glucose range

Can be applied to all current glucose meters.

Class is `devices`, type is `glucose_range`. Supported parameters are:


|Name|Description
|---|---
|min_glucose_mg_dl|
|max_glucose_mg_dl|
