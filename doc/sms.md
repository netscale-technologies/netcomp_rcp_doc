## SMS Bulk service

### send_sms

Sends a new sms for a patient

|Field|Type|Mandatory|Description
|---|---|---|---
|user_uid|string|N|UID of destination user. Field phone_sms will be used.
|number|string|N|Override destination number (mandatory if no `user_uid`)
|class|string|Y|Class of the message, related to campaign. Current valid options: ["test", "app-register"]
|text|string|Y|Body
|priority|integer|N|0-9 (default 5). Higher priorities will be sent first

If the user has opted-out for this number, corresponding error will be returned.
Otherwise, uid of pending-to-send message will be returned

Example

```
curl -v -X POST \
 -H "Content-Type: application/json" \
 -H "Authorization: Bearer ..." \
 -d '{"cmd":"send_sms", "data": {"number": "18139023251", "class": "app-register", "text": "Test1"}}' \
 https://v1-proxy-dev.nk.rcp.care/api
```


### list_sms_messages

|Field|Type|Mandatory|Description
|---|---|---|---
|uid|string|N|Specific UID
|class|string|N|For example "app-register"
|direction|string|N|"inbound" or "outbound"
|number|string|N|User's number
|trunk|string|N|Used trunk number
|user_uid|string|N|User's uid, if available
|last_status|string|N|
|start_time|string|N|Minimum timestamp for last_status_time
|stop_time|string|N|Maximum timestamp for last_status_time
|size|integer|N|Default 1000

Order wil be last_status_time, descending

### list_sms_users

|Field|Type|Mandatory|Description
|---|---|---|---
|number|string|N|User's number
|class|string|N|For example "app-register"
|user_uid|string|N|User's uid, if available
|last_status|string|N|
|start_time|string|N|Minimum timestamp for last_status_time
|stop_time|string|N|Maximum timestamp for last_status_time
|size|integer|N|Default 1000

Order wil be last_status_time, descending
