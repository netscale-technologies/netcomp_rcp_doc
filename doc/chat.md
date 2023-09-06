
## Chat

### patient_chat_get_messages

Returns a list of messages for a patient.
Messages are returned in reverse order (last message first). If we use `start_date` they will be start on that date, always in reverse order.

|Field|Type|Mandatory|Description
|---|---|---|---
|patient_uid|string|Y|
|start_time|string|N
|size|integer|N

Returns array of messages. Each one containts the following fields:

|Field|Type|Description
|---|---|---
|uid|string|UID of the message
|group_uid|string|UID of the message room
|create_time|string|Timestamp of creation
|creator_uid|string|UID of creator (patient or medical)
|text|string|
|type|string|either `patient` or `nurse`
|alias|text|For nurse messages, it will include field 'alias' with the name to show
|patient_read|object or nil|it will include field 'time' if read by patient
|nurse_check|object or nil|it will include fields 'nurse_uid' and 'time' if checked by nurse


### patient_chat_nurse_post

Allows a medical to create a new message for a patient

|Field|Type|Mandatory|Description
|---|---|---|---
|patient_uid|string|Y|
|medical_uid|string|Y|
|text|string|Y|
|alias|string|N|If not provided, real name will be used

It returns message_uid

### patient_chat_nurse_check

Allows a medical to "check" a message

|Field|Type|Mandatory|Description
|---|---|---|---
|patient_uid|string|Y|
|medical_uid|string|Y|
|message_uid|string|Y|
|notification_uid|string|N|If provided, will be added to metadata

### patient_chat_patient_post

Allows a patient to create a new message

|Field|Type|Mandatory|Description
|---|---|---|---
|patient_uid|string|Y|
|text|string|Y|
|hash|string|N

It returns message_uid

### patient_chat_patient_read

Allows a patient to mark a message as "read"

|Field|Type|Mandatory|Description
|---|---|---|---
|patient_uid|string|Y|
|message_uid|string|Y|

