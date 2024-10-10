
# API

This is the API for the second engine for medications management in the core (after the first one based on snapshots)

In this engine, you simply create individual medications for a patient, each of them having an status and an an associated log of actions (populated from surescripts)

## Medications

### create_medication

You must include field `spec` with following fields:


|Field|Type|Mandatory|Description
|---|---|---|---
|patient_uid|string|Y|Patient we refer to
|code|string|Y|Code for medication, usually and NDC
|code_type|string|Y|`ndc`, `rxcui`
|name|string|Y|Medication's title
|name_type|string|N|
|status|string|Y|Current status. Accepted values: (TO DECIDE)
|description|string|N|Extended description of medication
|origin|string|Y|`surescripts` `openfda`, `nih`, `custom`
|fda_extra| fda_extra|N|See bellow

fda_extra format:

|Field|Type|Mandatory|Description
|---|---|---|---
|active_ingredients| [ingredient] |N|
|quantity|string|N|
|route|string|N|
|type| string |N|
|conditions| [string] |N|

ingredient format:

|Field|Type|Mandatory|Description
|---|---|---|---
|name|string|N|
|strentgh|string|N|

If everything is ok, you will get a new patient_meds' uid

### update_medication

Can be used to update medication records. Fields `uid` and `spec` must be provided. Current editable fields are:
* name
* status
* description
* fda_extra

  
### list_medications

Allows to list current medications for a patient.

|Field|Type|Mandatory|Description
|---|---|---|---
|patient_uid|string|Y|
|code|string|N|
|status|string|N

### list_medication_logs

Allows to list "log updates" for a patient's medication. 
You must provide medication_uid, and if will be sorted by `fill_date`, descending

|Field|Type|Mandatory|Description
|---|---|---|---
|medication_uid|string|Y|
|patient_uid|string|N|Needed unless you have global permissions
|cursor|string|N|For pagination, or going to a fixed date
|size|integer|N|Max: 1000, default: 100









