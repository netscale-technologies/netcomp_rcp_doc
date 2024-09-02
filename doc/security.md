# Security

### security_check_permissions

This api allows you to check if an specific user has one or several permissions over a resource, targeted at some specific point.

|Field|Type|Mandatory|Description
|---|---|---|---
|token|string|Y|token identifying user requesting access (see bellow)
|resource|string|Y|protected resource (for example, "medications.surescripts")
|permissions| [string] |Y|requested permissions (for example ["view"])
|target_uid|string|Y|uid of the target to protect (usually a Patient's uid)

For token, differente formats are supported:
* Keycloak JWT token: token must be valid and not expired
* Core token
* User (medical, staff, patient or companion) UID. In this case, it must be prefixed like: "uid|Medical-XXX"

Example:

 ```json
{
 "token": "uid|medicals-0K98H1FA3aHWXwLEUvFbP5YuySt",
 "resource": "medications.surescripts",
 "permissions": ["view"],
 "target_uid": "patients-016d96379484Aaib3ZrYEjceD27"
}
```



### security_list_update_patient

Lists all records of calls to API "update_patient" filtering by patient_uid

|Field|Type|Mandatory|Description
|---|---|---|---
|uid|string|Y|patient's uid
|start_time|string|N|Limits the search starting after this datetime
|stop_time|string|N|Limits the search stopping before this datetime
|size|pos_integer|N|Default 20

Order will be `timestamp`, descending. In the returned struct, `target` would be the _uid_ of the user or token who launched the request, and `payload` containts the full request, along with `timestamp` and `uid` of the event.

**In order to make the query fast** do limit your start_time and stop_time in as few months as possible, since the table is partitioned by full months.
