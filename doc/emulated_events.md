# V2 Emulated Events

All events follow the same format, with followig fields:

```javascript
{
  "group": "rcp-v2", 
  "namespace": "",
  "uid": "<uid of event>",
  "date": "<timestamp of event generation, ISO8601>"
  "resource": "<see each section>"
  "type": "<see each section>"
  "target": "<see each section>"
  "data": "<see each section>"
  "metadata": {
    "trace_id": "<trace_id of operation>"
  }
}
```


## Timeslots

* resource will be `timeslots`
* target will be Timelot's uid

### create
* type will be `created`
* data will be (some fields could be missing):
```javascript
{
  "spec": {
    "type": "<worklog type>"
    "user_uid": "<user creating timeslot>"
    "patient_uid": "<patient>"
    "start_time": "<timestamp of start time>"
    "duration_secs": "<duration in sec>"
    "reporter_uid": "<reporter's uid, if available>"
    "program": "<reported program>"
    "tags": []
    "description": "<reported description>"
    "meta": "<meta field from timeslot>"
    "stop_time": "",
    "notification_uid": "<worklog's target uid>"
  }
}
```

### update

* type will be `updated`
* data will be (some fields could be missing)

```javascript
  {
    "updater_uid": "<uid of updater user>"
    "reason": "<reason>"
    "updates": {
      "start_time": "<updated start time>",
      "duration_secs": "<updated">,
      "type": "<updated">,
      "description": "<updated">,
      "user_uid": "<updated">,
      "user_data": "<core captured data>",
      "reporter_uid": "<updated">,
      "patient_uid": "<updated">,
      "target_uid": "<updated">,
      "tags": [],
      "program": "<updated">,
      "extra": "<meta field from timeslot>"
    }
  }
```

### delete

* type will be `deleted`

## Patient Calls  

An event will be generated when a call is finished:

* resource is `patient_calls`
* type is `updated`
* target is call's uid
* data is:

```javascript
{
  "status": "finished",
  "patient_uid": "<patient_uid>",
  "user_uid": "<caller_uid>",
  "program": "<program>",
  "start_time": "<call start time>",
  "duration_secs": "<call duration in secs>",
  "call_id": "<COMM engine call id>"
}
            



