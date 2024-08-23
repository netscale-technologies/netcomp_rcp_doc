## Companion

Companion objects model external persons who can login on the system, usually to gain access
to some other objects, usually a Patient.

When a companion is created, if a login is assigned it will be able to login but it will not be able
to use any API

You must use the APIs in [AuthRequests](../auth_requests.md) to generate authorization requests. Once they are approved, specific roles are assigned to Companions, allowing them to access some specific APIs for some specific objects


### create_companion
Creates a new Companion object

|Field|Type|Mandatory|Description
|---|---|---|---
|spec|spec|Y|See bellow

Spec object:

|Field|Type|Mandatory|Description
|---|---|---|---
|contact|contact|Y|See bellow
|login|string|N|If presented, it can do login
|password|string|N|
|status|string|N|Like "available". . OPTIONS NOT YET DEFINED
|timezone|string|N|Must be a valid timezone
|locale|string|N|Must be a valid locale

Contact object:

|Field|Type|Mandatory|Description
|---|---|---|---
|name|string|Y|Name (or full name) of user
|surname|string|N|Surname of user (optional)
|phone|string|N|Phone
|phone_sms|string|N|Phone
|email|string|N|Contact email
|address|address|N|See bellow

Address object:

|Field|Type|Mandatory|Description
|---|---|---|---
|street|string|Y|
|code|string|N|
|city|string|N|
|province|string|N|
|state|string|N|
|country|string|N|


### get_companion
Gets the data of an existing companion. Field `uid` is mandatory


### update_companion
Allows to update companion's data

|Field|Type|Mandatory|Description
|---|---|---|---
|uid|string|Y|Unique UUID
|spec|spec|Y|Spec object

### delete_companion

Allows to delete an existing companion. Same parameters as get_companion


### list_companions
Allows for a powerful search operation on companions

|Field|Type|Description
|---|---|---
|login|string|
|status|string|
|fts|string|Finds in login, name, surname, phone and phone_sms
|target_uid|string|Filter companions having roles over this target


### companion_set_patient
Adds to a companion the role "core.companion" targetting the patient calling the API.
Roles can be retrieved with `get_companion` in `metadata.roles`

|Field|Type|Mandatory|Description
|---|---|---|---
|companion_uid|string|Y|Companion's UID


### companion_unset_patient
Removes from a Companion the role "core.companion" targetting the patient calling the API.
Roles can be retrieved with `get_companion` in `metadata.roles`

|Field|Type|Mandatory|Description
|---|---|---|---
|companion_uid|string|Y|Companion's UID

