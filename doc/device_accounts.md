
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
|user_id|string|N|User ID on remote
|scopes|array of strings|N|Available scopes granted to this user
|devices|array of objects|N|If available, specific info on devices
|extra|object|N|Additional metadata to keep

When calling this function, first we try to locate an existing device account that matches:
* If a device account is found for this patient with same class and user_id, it is used
* If none is found, but one exists with same class and user_id=null, it is used and user_id is updated (this is used in some pre-provision scenarios)
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


### device_account_get

You can use this API to retrive a specific device account using its uid

|Field|Type|Mandatory|Description
|---|---|---|---
|uid|string|Y|

Returned fields will include "spec" with the original specification, "metadata" with uid and creation and update fields and "status" returning `is_authorized` and `external_id`


### device_account_find_by_external_id

Finds an existing devide account with this external_id. Returned fields will be the same as above.

|Field|Type|Mandatory|Description
|---|---|---|---
|class|string|Y|class to use
|external_id|string|Y|

### device_account_find_by_patient

Finds all existing devce accounts for this patient. Returned fields will include "items" with a list of objects as above
|Field|Type|Mandatory|Description
|---|---|---|---
|patient_uid|string|Y|


### device_account_find_by_patient_group

Specialized call to finds all existing devce accounts for one or more partients. 

|Field|Type|Mandatory|Description
|---|---|---|---
|patient_uids|list of string|Y|

It will return field "patients" with a map, one key for each patient, and for each one a list of objects including fields uid, class, is_authorized and devices.


### device_account_iterate

Allows to iterate over all device accounts, sorted by uid.
Results can be paginated using "last_uid" of last returned uid to restart on next one.

|Field|Type|Mandatory|Description
|---|---|---|---
|last_uid|string|N|Default is ""
|size|intege|N|Default is 2.000

### device_account_delete

Deletes an existing device account.
Be careful since this means it cannot be used any more to know the patient had this account in the past.

|Field|Type|Mandatory|Description
|---|---|---|---
|uid|string|Y|



