# API


## Common


### login
Performs a login and creates a new token

|Field|Type|Mandatory|Description
|---|---|---|---
|login|string|Y|Login of user. Mandatory if no token
|password|string|Y|Mandatory if login is present
|device|N|Device identification

A new token for new logins from the same device, deleting old token if present

If successful, replies:

|Field|Type|Description
|---|---|---
token_uid|string|Generated token
ttl|integer|TTL in seconds for generated token
member_uid|string|Member (medical, staff, patient) id
class|string|Member class (medical, staff, patient)
type|string|Member type (doctor...)
roles|\[string\]|Member's roles
device|string|User login
login|string|Login
user_uid|string|Internal user


Alternatively, you can login using an existing token:

|Field|Type|Mandatory|Description
|---|---|---|---
|token|string|Y|Existing user

In this case, no new token will be generated, but the session will be authenticated as per the user this token belongs to. 

You can also use the static _admin token_ from the the server, which allows any operation to be performed on the server without restrictions. 


### logout
Performs a logout and removes token

|Field|Type|Mandatory|Description
|---|---|---|---
|close_socket|boolean|N|If false, socket will not stop


### get_timezones

Can be used to get all timezones the server recognizes

|Field|Type|Mandatory|Description
|---|---|---|---

Normal users can only get its own avatar.

Staff users can get any avatar

Returns:

|Field|Type|Mandatory|Description
|---|---|---|---
|timezones|\[string\]|Y|


### update_password

|Field|Type|Mandatory|Description
|---|---|---|---
|password|string|Y|New password
|old_password|string|N|Old Password
|member_uid|N|Member to change password to

Normal users must include password and old_password.

Staff users must include password and member_uid of te member to change the password to.


### upload_avatar

Can be used to update a patient, staff or medical avatar

|Field|Type|Mandatory|Description
|---|---|---|---
|content_type|string|Y|
|base64|string|Y|
|member_uid|N|Member to update avatar (only for staff)

Normal users can only update its own avatar.

Staff users can update any avatar

Returns:

|Field|Type|Mandatory|Description
|---|---|---|---
|file_uid|string|Y|


### download_avatar

Can be used to download a patient, staff or medical avatar

|Field|Type|Mandatory|Description
|---|---|---|---
|member_uid|N|Member to update avatar (only for staff)

Normal users can only get its own avatar.

Staff users can get any avatar

Returns:

|Field|Type|Mandatory|Description
|---|---|---|---
|content_type|string|Y|
|base64|string|Y|


### download_avatar

Can be used to download a patient, staff or medical avatar

|Field|Type|Mandatory|Description
|---|---|---|---
|member_uid|N|Member to update avatar (only for staff)

Normal users can only get its own avatar.

Staff users can get any avatar

Returns:

|Field|Type|Mandatory|Description
|---|---|---|---
|content_type|string|Y|
|base64|string|Y|


### get_timetracking_types

Can be used to get the current list of timetracking types
No parameter is needed


Returns:

|Field|Type|Mandatory|Description
|---|---|---|---
|types|\[type\]|Y|


Type description:

|Field|Type|Mandatory|Description
|---|---|---|---
|id|string|Y|Type code
|type|string|Y|Currently `provider`, `staff` or `system`
|auto|boolean|Y|If it is an automatic entry
|text|object|N|Map with descriptions with language codes (`en: Text`) 


### get_templates

Proxy service to template description

Fields:

|Field|Type|Mandatory|Description
|---|---|---|---
|full|boolean|N|If true, retrieves full details


### make_label

Service to generate unique labels

Fields:

|Field|Type|Mandatory|Description
|---|---|---|---
|class|string|Y|Like "SC02"
|batch|string|Y|Like "C01"

Returns generated label


### start_hub_app

Finds if the logged patient has a devicehub with class "HB10". If true, returns its data. If false, it will create one, associate with the patient and return data.

Returned data:

|Field|Type
|---|---
|patient_uid|string|
|devicehub_uid|string|
|devicehub_id|string|
|label|string|
|info|string|See bellow

Data in "info" will have the followig fields interpolated:


|Field|Type
|---|---
|{env}|Current environment
|{hub_id}|string|Created or selected hub id
|{label}|Created or selected hub label

If the hub is created, "owner" will be set to "patient" and field "uid" will point to patient's uid. Also office will be copied from patient's office









