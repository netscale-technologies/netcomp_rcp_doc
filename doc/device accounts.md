# API

## DeviceAccounts

A `deviceaccount` is an object that indicates that a patient has some sort of account with an external service that provides some type of devices.
Each device account has an unique "external_id" (like "4456-2375-6786")



### device_account_register

You can use this API to "register" a patient with an existing "account"

|Field|Type|Mandatory|Description
|---|---|---|---
|class|string|Y|The class of the account. Current classes are "dexcom", "fitbit" and "rescom"
|patient_uid|string|Y|Our existing patient uid
|user_id|string|Y|User ID on remote
|scopes|array of strings|N|Available scopes granted to this user
|devices|array of objects|N|If available, specific info on devices
|extra|object|N|Additional metadata to keep

When calling this function, first we try to locate an existing device account that matches:
* If a device account is found for this patient with same class and user_id, it is used
* If none is found, but one exists with same lass and user_id=null, it is used and user_id is updated (this is used in some pre-provision scenarios)
* If none is found again, a new one is created (along with a new external_id)

After calling this function, new or existing device account will be flagged as "authorized"


### device_account_unregister

You can use this API to "unregister" a patient from an existing "account".
Rules for finding the correct account are the same as described above. Device account, if it exists, will not be deleted, it will be only flagged as "not authorized".
If extra is used, it will overwrite existing metadata


|Field|Type|Mandatory|Description
|---|---|---|---
|class|string|Y|The class of the account. Current classes are "dexcom", "fitbit" and "rescom"
|patient_uid|string|Y|Our existing patient uid
|user_id|string|Y|User ID on remote
|extra|object|N|Additional metadata







