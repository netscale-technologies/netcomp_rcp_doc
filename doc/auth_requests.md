# AuthRequests

These APIs allow to "send" an authorization request from a _target_ to an _authorized_. Once both parties _authorize_ the request,
indicated roles will be applied to _authorized_ over _target_.

Typically, _target_ will be a _Patient_ and _authorized_ will be a _Companion_

### create_patient_auth_request

Allows to create a new auth request for a patient
Field 'spec' must be provided with following information:

|Field|Type|Mandatory|Description
|---|---|---|---
|target_uid|string|Y|patient's uid
|authorized_uid|string|Y|companion's uid
|roles| [string] |Y|List of roles. Allowed values: `["test"]`
|ttl_secs|integer|N|Expiration ttl (default 24h)

Returns UID of the config object

Request will be deleted once both parties approved it, or the timeout happens.

### auth_request_target_approve

Marks a request as _approved_ by target

|Field|Type|Mandatory|Description
|---|---|---|---
|request_uid|string|Y|UID of request
|target_uid|string|Y|target's (usually patient) uid


### auth_request_authorized_approve

Marks a request as _approved_ by authorized

|Field|Type|Mandatory|Description
|---|---|---|---
|request_uid|string|Y|UID of request
|authorized_uid|string|Y|target's (usually companion) uid


### delete_auth_request

Deletes an existing auth request before expiration time. 
Field `uid` must be provided


### list_auth_request

Lists pending auth requests 

|Field|Type|Mandatory|Description
|---|---|---|---
|target_uid|string|Y|UID of target (typically patient)


