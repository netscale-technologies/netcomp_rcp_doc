# API


## CarePlan Types

This functions allow to retrieve information needed to build care plans

It is based on fhe file [careplan_types.yaml](../config/careplan_types.yaml)


### careplan_update_types

|Field|Type|Mandatory|Description
|---|---|---|---
|yaml|string|Y|YAML formatted base64 string

Replaces current types with the ones in this call.
At restart, stored yaml will be used again


### careplan_get_action_step

|Field|Type|Mandatory|Description
|---|---|---|---
|id|string|Y|Id

Returns
 
|Field|Type|Description
|---|---|---
|id|string|Id of record
|text|string|
|condition_id|string|
|type|string|`binary`, `freetext` or `choice`
|values| [string] |
        

### careplan_get_action_steps

|Field|Type|Mandatory|Description
|---|---|---|---
|condition_id|string|Y|Id

Returns array of actors


### careplan_get_area

|Field|Type|Mandatory|Description
|---|---|---|---
|id|string|Y|Id

Returns
 
|Field|Type|Description
|---|---|---
|id|string|Id of record
|text|string|


### careplan_get_areas

Returns all areas


### careplan_get_barrier

|Field|Type|Mandatory|Description
|---|---|---|---
|id|string|Y|Id

Returns
 
|Field|Type|Description
|---|---|---
|id|string|Id of record
|text|string|
|condition_id|string|
|type|string|`binary`, `freetext` or `choice`
|values| [string] |


### careplan_get_barriers

|Field|Type|Mandatory|Description
|---|---|---|---
|condition_id|string|Y|Id

Returns array of actors


### careplan_get_condition

|Field|Type|Mandatory|Description
|---|---|---|---
|id|string|Y|Id

Returns
 
|Field|Type|Description
|---|---|---
|id|string|Id of record
|text|string|
|area_ids| [string] |


### careplan_get_conditions

|Field|Type|Mandatory|Description
|---|---|---|---
|area_ids| [string] |Y|Id

Returns array of actors


### careplan_get_goal

|Field|Type|Mandatory|Description
|---|---|---|---
|id|string|Y|Id

Returns
 
|Field|Type|Description
|---|---|---
|id|string|Id of record
|text|string|
|condition_id|string|
|type|string|`binary`, `freetext` or `choice`
|values| [string] |

### careplan_get_goals

|Field|Type|Mandatory|Description
|---|---|---|---
|condition_id|string|Y|Id

Returns array of actors


### careplan_get_support_tool

|Field|Type|Mandatory|Description
|---|---|---|---
|id|string|Y|Id

Returns
 
|Field|Type|Description
|---|---|---
|id|string|Id of record
|text|string|
|condition_id|string|
|type|string|`binary`, `freetext` or `choice`
|values| [string] |


### careplan_get_support_tools

|Field|Type|Mandatory|Description
|---|---|---|---
|condition_id|string|Y|Id

Returns array of actors


### careplan_get_symptom

|Field|Type|Mandatory|Description
|---|---|---|---
|id|string|Y|Id

Returns
 
|Field|Type|Description
|---|---|---
|id|string|Id of record
|text|string|
|condition_id|string|
|type|string|`binary`, `freetext` or `choice`
|values| [string] |


### careplan_get_symptoms

|Field|Type|Mandatory|Description
|---|---|---|---
|condition_id|string|Y|Id

Returns array of actors


### careplan_get_script

|Field|Type|Mandatory|Description
|---|---|---|---
|id|string|Y|Id

Returns
 
|Field|Type|Description
|---|---|---
|id|string|Id of record
|text|string|
|condition_id|string|
|category|string|


### careplan_get_scripts

|Field|Type|Mandatory|Description
|---|---|---|---
|condition_id|string|Y|Id

Returns array of actors



