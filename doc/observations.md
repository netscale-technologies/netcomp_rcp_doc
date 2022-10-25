# API


## Observations


### create_observation
Creates a new Observation object

|Field|Type|Mandatory|Description
|---|---|---|---
|id|string|N|Unique (for domain) id of observation, no special characters allowed. Generated if not included.
|domain|string|N|Domain to use (like "zone1.type")
|spec|spec|Y|See bellow

Spec object:

|Field|Type|Mandatory|Description
|---|---|---|---
|type|string|Y|Type of observation. See bellow
|patient_uid|string|Y|UID of patient
|device_uid|string|Y|UID of device that generated this
|device_type|string|Y|Device Type
|device_protocol|string|Y|Device Protocol
|data|object|Y|Actual observation data. See bellow.
|time|string|Y|Datetime of observation
|is_valid|boolean|N|Default true
|is_test|boolean|N|Default false
|reporter_uid|string|N|
|virtual_device_id|string|N|
|notes|string|N|**Should be used only for notes (JSON or text), not operation info**
|meta|object|H|Additional data

Returns:


|Field|Type|Description
|---|---|---
|uid|string|UID of the created observation
|id|string|Unique (in domain) id

The field 'meta' will have the following format:

|Field|Type|Description
|---|---|---
|values|array|List of values used for computation
|times|array|List of times of values
|session|string|Device session
|extra|object|See bellow


For a description of `data` and `extras`, see  



### get_observation
Gets the data of an existing observation. Two formats available:

|Field|Type|Mandatory|Description
|---|---|---|---
|uid|string|Y|UID of observation


|Field|Type|Mandatory|Description
|---|---|---|---
|id|string|Y|ID of observation
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


### update_observation
Observations _cannot_ be updated


### delete_observation

Allows to delete an existing observation. Same parameters as get_observation


### search_observations
Allows for a powerful search operation on observations


|Field|Type|Description
|---|---|---
|domain|string|Domain to filter on
|deep|boolean|If true, will search in sub-domains
|type|string|
|patient_uid|string|
|device_uid|string|
|start_time|string|
|stop_time|string|
|is_valid|string|
|is_test|string|
|creation_date|string|Creation date in RFC3339 format
|update_date|string|Last update date in RFC3339 format
|from|integer|Starting point (for pagination)
|size|integer|Number or records to retrieve
|get_total|boolean|If true, calculates the grand total of records
|sort|\[string\]|List of fields to sort on

* Field `time` can also be used for sort
* Can filter on domain, or domain and subdomains using 'deep'
* Can paginate with from and size
* Can filter on any of the previous fields, or any number of them, working like and 'and'
  * If you set a raw string, it will find objects with that exact string: `"phone": "123"`
  * You can use a prefix to modify the behaviour: eq, ne, gt, gte, lt, lte. They work for string and numeric fields (like timezone): `"timezone": "gte:2"` 
  * You can use a prefix: `"phone": "prefix:123"`
  * You can target several values: `"phone": "123|321"`
* Can sort on one or several fields, ascending or descending: `"sort": ["asc:update_time", "timezone"]` 
* Operations on field 'name' will be performed over a 'normalized' version of the field (without uppercase, utf8, etc.)


### update_observation_status

Changes the status of some fields. Fields not included will be left

|Field|Type|Mandatory|Description
|---|---|---|---
|observation_uid|string|Y|
|is_valid|boolean|-|
|is_false|boolean|-|

### check_observation_alerts

Force the check of alerts for self-reported observations

|Field|Type|Mandatory|Description
|---|---|---|---
|observation_uid|string|Y|


### update_observation_notes

Changes the 'notes' field of an observation

|Field|Type|Mandatory|Description
|---|---|---|---
|observation_uid|string|Y|
|notes|string|Y|Use a JSON




### make_observations_report

|Field|Type|Mandatory|Description
|---|---|---|---
|patient_uid|string|Y|
|title|string|Y|Tittle for report
|subtitle|string|N|Tittle for report
|start_time|string|Y|For example 2019-01-01
|stop_time|string|Y|For example 2020

Mock for now



# Observations type and data

The format of the fields `data` and `meta` depends on the field `device_protocol`.

he currently defined types are:
 * scale
 * pulseoximeter
 * bpm
 * thermometer
 * glucose
 * spirometer
 * heartrate

#### Protocol

The protocol for the device must be known to generate it. Currently defined are:

* escs20m
* adfb06
* adfb19
* adfb102w
* adfb885t
* adfb34a
* adfb38a
* adfb27
* adfmsa100
* polh7
* ihlpo3m
* ihl550bt
* ihlhs2
* ihlhs4
* ihlbg5
* ihlts28b
* tsbf8028
* tsbf8011

## escs20m

### data

|Field|Type
|---|---
|weight_kgs|number
|impedance_ohms|number

### extra (without impedance)

|Field|Type
|---|---
|height|integer
|weight|number
|age|number
|gender|string|
|basal_metabolic_rate|number
|visceral_fat|number
|bmi|number
|ideal_weigth|number

### extra (additional fields with impedance)
|Field|Type
|---|---
|impendance|number
|lean_body_mass|number
|fat_percentage|number
|water_percentage|number
|bone_mass|number
|muscle_mass|number
|protein_percentage|number


## adfb06

### data

|Field|Type
|---|---
|bpm|number
|spo2_pct|number
|pi_pct|number



## adfb19

### data

|Field|Type
|---|---
|bpm|number
|diastolic_mmHg|number
|systolic_mmHg|number


## adfb102w

### data

|Field|Type
|---|---
|bpm|number
|diastolic_mmHg|number
|systolic_mmHg|number


## adfb885t

Same as escs20m


## adfb34a

### data

|Field|Type
|---|---
|temperature_celsius_body|number
|temperature_celsius_tympanum|number


## adfb38a

### data

|Field|Type
|---|---
|temperature_celsius_body|number
|temperature_celsius_surface|number


## adfb27

### data

|Field|Type
|---|---
|glucose_mg_dl|number


## adfmsa100
|Field|Type
|---|---
|fev_liters|number
|pef_liters_per_minute|number
