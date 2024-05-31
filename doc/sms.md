## SMS Bulk service

### send_sms

Sends a new sms for a patient

|Field|Type|Mandatory|Description
|---|---|---|---
|user_uid|string|N|UID of destination user. Field phone_sms will be used.
|number|string|N|Override destination number
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
