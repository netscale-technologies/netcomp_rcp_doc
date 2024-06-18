# Security

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
