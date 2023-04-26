
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
|metadata|object|see bellow

Field metadata will contain different info depending on the context.
For nurse messages, it will include field 'alias' with the name to show


### patient_chat_nurse_post

Allows a medical to create a new message for a patient

|Field|Type|Mandatory|Description
|---|---|---|---
|patient_uid|string|Y|
|medical_uid|string|Y|
|text|string|Y|
|alias|string|N|If not provided, real name will be used

It returns message_uid
