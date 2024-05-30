# API


## Common


### login
Performs a login and creates a new token

|Field|Type|Mandatory|Description
|---|---|---|---
|login|string|Y|Login of user. Mandatory if no token
|password|string|Y|Mandatory if login is present
|device|N|Device identification
|push|N|Optional push object. See bellow

A new token for new logins from the same device, deleting old token if present.
Token will be generated only if device is provided. Otherwhise, it will appear for compatibility but it will be invalid.

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

#### Push

Login process allows to set up push information. To use it, `device` must idetify the unique app to receive the push, and field `push` must be an object containing followig fields:

|Field|Type|Mandatory|Description
|---|---|---|---
|class|string|Y|Currently only `patient_app` is supported

#### JWT

You can now also login using a Keycloak Oauth token.
Then token must be signed by the Keycloak sitting in same environment, and must include two claims:

* preferred username (included by default)
* rcp-core-ui: You must configure your client with a "Mapper" that adds attribute "rcp-core-uid" from patient into a claim with the same name

Then you can use the format:

|Field|Type|Mandatory|Description
|---|---|---|---
|jwt|string|Y|Keycloak token
|device|string|N|Device identification
|push|object|N|Optional push object. See above

For now, core will be generating a new token working the same than classical login. **This will probably be dropped in the near future.**



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

After updating the password in core, it will try to update it in Keycloak too.
Returning field 'keycloak' will inform about the result of the operation: `ok` | `user_not_found` | `error`


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

### provision_kc_user

|Field|Type|Mandatory|Description
|---|---|---|---
|member_uid|string|N|Member to provision
|password|string|Y|New password

User to provision in KC must exist and have a valid "login".
Password will be updated in core and user will be created or updated in KC








