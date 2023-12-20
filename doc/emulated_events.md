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
```

## Careplans

An event will be generated when a careplan is created, updated or deleted

* resource is `careplans`
* type is `create`, `update` or `delete`
* target is carenplan's uid
* data will include "spec" with updated careplan spec

## Orders

An event will be generated when an order is created, updated, deleted or its status is updated

* resource is `orders`
* target is ordcer's uid

### create sample

```javascript
{                                                                                                              
  "group": "rcp-v2",
  "date": "2023-11-13T15:20:43.090222Z",
  "metadata": {
    "trace_id": "372a734e1c783f5d"
  },
  "namespace": "",
  "resource": "orders",
  "target": "order-1hf4kah7lCjbgdut2SthClv4Ccq",
  "type": "updated",
  "uid": "1HF4KAIAI-2PEP6EY02KINA1FEBV",
  "data": {                                                                                                    
    "spec": {                                                                                                  
      "address": {                                                                                             
        "street": "calle1"                                                                                     
      },                                                                                                       
      "devices": [
        {
          "type": "t1",
          "units": 2
        }
      ],
      "email": "carlosj.gf@gmail.com",
      "locale": "es",
      "meta": {
        "a": 2
      },
      "name": "name1",
      "patient_uid": "patients-016d96379484Aaib3ZrYEjceD27",
      "phone": "1234567890",
      "phone_sms": "14354126330",
      "surname": "González Florido",
      "timezone": "Europe/Madrid"
    },
    "status": {
      "last_status": "created",
      "last_status_time": "2023-11-13T15:20:41.973638Z"
    }
  },
}
```

### update sample

```javascript
{                                                                                                              
  "group": "rcp-v2",
  "date": "2023-11-13T15:20:43.090222Z",
  "metadata": {
    "trace_id": "372a734e1c783f5d"
  },
  "namespace": "",
  "resource": "orders",
  "target": "order-1hf4kah7lCjbgdut2SthClv4Ccq",
  "type": "updated",
  "uid": "1HF4KAIAI-2PEP6EY02KINA1FEBV",
  "data": {                                                                                                    
    "spec": {                                                                                                  
      "address": {                                                                                             
        "street": "calle1"                                                                                     
      },                                                                                                       
      "devices": [
        {
          "type": "t1",
          "units": 2
        }
      ],
      "email": "carlosj.gf@gmail.com",
      "locale": "es",
      "meta": {
        "a": 2
      },
      "name": "name1",
      "patient_uid": "patients-016d96379484Aaib3ZrYEjceD27",
      "phone": "1234567890",
      "phone_sms": "14354126330",
      "surname": "González Florido",
      "timezone": "Europe/Madrid"
    },
    "status": {
      "last_status": "created",
      "last_status_time": "2023-11-13T15:20:41.973638Z"
    }
  }
}
```

### status update sample

```javascript
{                                                                                                              
  "group": "rcp-v2",
  "date": "2023-11-13T15:20:44.108768Z",
  "metadata": {
    "trace_id": "399d8f9bef927b3a"
  },
  "namespace": "",
  "resource": "orders",
  "target": "order-1hf4kah7lCjbgdut2SthClv4Ccq",
  "type": "status_updated",
  "uid": "1HF4KAJAC-958O1IB9INWGSRZFNV",
  "data": {                                                                                                    
    "status": {                                                                                 
      "last_status": "created",
      "last_status_time": "2023-11-13T15:20:41.973638Z"
    }
  }
}
```

### delete sample

```javascript
{                                                                                                              
  "group": "rcp-v2",
  "metadata": {
    "trace_id": "7deaf83c6548fb79"
  },
  "namespace": "",
  "resource": "orders",
  "target": "order-1hf4kah7lCjbgdut2SthClv4Ccq",
  "type": "deleted",
  "uid": "1HF4KAKUG-MK4LHB7IYAGK428I56",
  "data": {},                                                                                             
  "date": "2023-11-13T15:20:45.776276Z"
}
```

## Summaries

An event will be generated on each summary update

* resource is `summaries`
* target is summaries's uid

### sample

```javascript
{                                                                                                              
    "data": {
        "extra": {
            "last_update": "2023-12-18T11:29:40.088073",
            "updates": 22
        },
        "spec": {
            "class": "fitbit",
            "date": "2023-12-18",
            "patient_uid": "patients-0L84YBQR5UJRlFkItr7H98ZUXnj",
            "protocol": "activity_sum_0",
            "reading": {
                "distance_meters": 0,
                "fairly_active_seconds": 0,
                "lightly_active_seconds": 0,
                "sedentary_seconds": 19740,
                "steps": 0,
                "very_active_seconds": 0
            },
            "timestamp": "2023-12-18T11:29:38.117368",
            "type": "activity_sum",
            "user_id": "Fitbit-38J9HN"
        }
    },
    "date": "2023-12-18T11:29:40.093887Z",
    "group": "rcp-v2",
    "metadata": {
        "trace_id": "0a7b2015091a1c1b"
    },
    "namespace": "",
    "resource": "summaries",
    "target": "summary-1hhtogr20BkCqMjTFsDwjkJ5g7b",
    "type": "inserted",
    "uid": "1HHUB0L7T-8SPKYPYR5ECM0PU0M2"
}
```

## Observations

An event will be generated when an observation is created, updated or deleted

* resource is `observations`
* type is `created`, `updated`, `deleted`
* target is call's uid

sample:

```javascript
```






