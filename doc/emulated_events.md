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


## Partners

* resouce will be `partners`
* target is partner's uid
* type will be `created` | `updated` | `deleted`

**create sample**

```javascript
{
    "data": {
        "spec": {
            "name": "partner1"
        },
        "status": {
            "status": "active"
        }
    },
    "date": "2024-01-22T18:53:14.271276Z",
    "group": "rcp-v2",
    "metadata": {
        "trace_id": "b0808cba6de4cc43"
    },
    "namespace": "",
    "resource": "partners",
    "target": "partners-0LRPACB1ZLIKDTDzVEImjEcZs2f",
    "type": "created",
    "uid": "1HKP8A0KV-9TCW2W3UMIEB5WVN67"
}
```

**update sample**

```javascript
{
    "data": {
        "update": {
            "spec": {
                "name": "carlos3"
            },
            "status": {
                "status": "active"
            }
        }
    },
    "date": "2024-01-22T19:06:05.799727Z",
    "group": "rcp-v2",
    "metadata": {
        "trace_id": "237c1e9ae1b8d876"
    },
    "namespace": "",
    "resource": "partners",
    "target": "partners-0LRPAS29PAJBOrTU5Tq0WkuhTNW",
    "type": "updated",
    "uid": "1HKP91I37-BACH34WHCF4ZZOLR9F"
}
```

**delete sample**

```javascript
{
    "data": {},
    "date": "2024-01-22T19:04:37.021053Z",
    "group": "rcp-v2",
    "metadata": {
        "trace_id": "6d8f0effd32df71a"
    },
    "namespace": "",
    "resource": "partners",
    "target": "partners-0LRPACB1ZLIKDTDzVEImjEcZs2f",
    "type": "deleted",
    "uid": "1HKP8URCT-JC8IFVICYTUVFBKYY9"
}
```


## Organizations

* resouce will be `organizations`
* target is organizatuion's uid
* type will be `created` | `updated` | `deleted`

**create sample**

```javascript
{
    "data": {},
    "date": "2024-01-22T19:14:35.671352Z",
    "group": "rcp-v2",
    "metadata": {
        "trace_id": "46b5618560ab7b21"
    },
    "namespace": "",
    "resource": "organizations",
    "target": "organizations-0LRPAXGQZE1IzAcmQlcZ87OYu5y",
    "type": "deleted",
    "uid": "1HKP9H40N-TQO4LDS0AJY36BLVJZ"
}
```

**update sample**

```javascript
{
    "data": {
        "update": {
            "spec": {
                "name": "org1",
                "partner_uid": "partners-0LRPAS29PAJBOrTU5Tq0WkuhTNW"
            },
            "status": {
                "status": "active"
            }
        }
    },
    "date": "2024-01-22T19:13:58.967101Z",
    "group": "rcp-v2",
    "metadata": {
        "trace_id": "af9a18e975fe9697"
    },
    "namespace": "",
    "resource": "organizations",
    "target": "organizations-0LRPAXGQZE1IzAcmQlcZ87OYu5y",
    "type": "updated",
    "uid": "1HKP9G05N-L2NOKGSY5CL3EQJQVK"
}
```

## Offices

* resouce will be `offices`
* target is office's uid
* type will be `created` | `updated` | `deleted`

**create sample**

```javascript
{
    "data": {
        "spec": {
            "name": "carlos1",
            "timezone": "GMT",
            "branding": "brand, 
            "organization_uid": "organizations-0LRPB4KPCBR6pIiShlQQpGlGroN"
        },
        "status": {
            "status": "active"
        }
    },
    "date": "2024-01-22T19:22:32.463285Z",
    "group": "rcp-v2",
    "metadata": {
        "trace_id": "aa6f55b9a992cca6"
    },
    "namespace": "",
    "resource": "offices",
    "target": "offices-0LRPBDZ7KSDGRcfij6ldOpIA6JI",
    "type": "created",
    "uid": "1HKP9VLKF-24DWG6L6111BBK50RQ"
}
```

## Medicals

An event will be generated when a medical is created, updated or deleted

* resource is `medicals`
* type is `created`|`updated`|`deleted`
* target device medicals's uid
* 

### create sample

```javascript
{
    "data": {
        "data": {
            "spec": {
                "email": "a@a",
                "is_primary": false,
                "locale": "es",
                "login": "test-proxy@medical",
                "name": "test-proxy",
                "npi": "123",
                "offices": [
                    "offices-0K5F4MI9G9GsVOt1L98gm9tBN7A"
                ],
                "phone": "001234567890",
                "roles": [
                    "role1"
                ],
                "surname": "surname",
                "timezone": "Europe/Madrid",
                "type": "medical"
            },
            "status": {}
        }
    },
    "date": "2024-01-23T10:15:59.263751Z",
    "group": "rcp-v2",
    "metadata": {
        "trace_id": "55bf81cbab14c16e"
    },
    "namespace": "",
    "resource": "medicals",
    "target": "medicals-0LRQ7AYCEPZbpjIyLriCMQb617C",
    "type": "created",
    "uid": "1HKQT3K0V-J67CNHD5STQOJNPHHO"
}
```

### update sample

```javascript
{
    "data": {
        "update": {
            "data": {
                "spec": {
                    "email": "a@a",
                    "is_primary": false,
                    "locale": "es",
                    "login": "test-proxy2@medical",
                    "name": "test-proxy2",
                    "npi": "proxynpi",
                    "offices": [
                        "offices-0LRQ96OR2JFuLaleztXShU8gE1G"
                    ],
                    "phone": "11234567890",
                    "roles": [
                        "role1"
                    ],
                    "surname": "surname",
                    "timezone": "Europe/Madrid",
                    "type": "medical"
                },
                "status": {}
            }
        }
    },
    "date": "2024-01-23T11:08:41.326446Z",
    "group": "rcp-v2",
    "metadata": {
        "trace_id": "cd247244fd7f4e9e"
    },
    "namespace": "",
    "resource": "medicals",
    "target": "medicals-0LRQ96PN487MJ2qUidvUjvOF4wn",
    "type": "updated",
    "uid": "1HKR043VE-FCDXTC58DITKZEPKNG"
}
```

## Staff

An event will be generated when a staff is created, updated or deleted

* resource is `staff`
* type is `created`|`updated`|`deleted`
* target device medicals's uid
* 

### create sample

```javascript
{
    "data": {
        "data": {
            "spec": {
                "email": "a@a",
                "locale": "es",
                "login": "test-proxy@staff",
                "name": "test-proxy",
                "offices": [
                    "offices-0LRQ9M6P740Dxvwwn1nBvNtz704"
                ],
                "phone": "001234567890",
                "surname": "surname",
                "timezone": "Europe/Madrid",
                "type": "staff"
            },
            "status": {}
        }
    },
    "date": "2024-01-23T11:39:14.646772Z",
    "group": "rcp-v2",
    "metadata": {
        "trace_id": "64c5a7bdfa19b8f9"
    },
    "namespace": "",
    "resource": "staff",
    "target": "staff-0LRQAA11VJBRtOHDJZXcaHOdbYD",
    "type": "created",
    "uid": "1HKR1S2AM-N7TNXFSIYJAUFVYAV8"
}
```

### update sample

```javascript
{
    "data": {
        "update": {
            "data": {
                "spec": {
                    "email": "a@a",
                    "locale": "es",
                    "login": "test-proxy@staff",
                    "name": "test-proxy2",
                    "offices": [
                        "offices-0LRQ9M6P740Dxvwwn1nBvNtz704"
                    ],
                    "phone": "11234567890",
                    "surname": "surname",
                    "timezone": "Europe/Madrid",
                    "type": "staff"
                },
                "status": {}
            }
        }
    },
    "date": "2024-01-23T11:39:16.009841Z",
    "group": "rcp-v2",
    "metadata": {
        "trace_id": "ecab8c90b93558ce"
    },
    "namespace": "",
    "resource": "staff",
    "target": "staff-0LRQAA11VJBRtOHDJZXcaHOdbYD",
    "type": "updated",
    "uid": "1HKR1S3L9-BEK6A9832EIWRYT82K"
}
```

## Patient

An event will be generated when a patient is created, updated or deleted

* resource is `patients`
* type is `created`|`updated`|`deleted`
* target device pateints's uid
* 

### create sample

```javascript
{
    "data": {
        "data": {
            "spec": {
                "alternative_id": "aid",
                "contact_preference": "call",
                "ehr": {
                    "birth_time": "2020-01-01T00:00:00Z",
                    "height": 100,
                    "meta": {
                        "notes": "abc"
                    }
                },
                "ehr_id": "p-ehrid",
                "gender": "U",
                "insurance_meta": {
                    "a": 1
                },
                "is_billable": false,
                "locale": "es",
                "login": "test-proxy@patient",
                "medicals": [
                    {
                        "primary": true,
                        "role": "m",
                        "uid": "medicals-0K5F7CKVOA4mq4CAPB63Oqs39DT"
                    }
                ],
                "name": "test-proxy",
                "office_uid": "offices-0K5F4MI9G9GsVOt1L98gm9tBN7A",
                "primary_medical": "medicals-0K5F7CKVOA4mq4CAPB63Oqs39DT",
                "primary_nurse": "medicals-0K5F7CKVOA4mq4CAPB63Oqs39DT",
                "programs": [
                    "TLH"
                ],
                "risk_factor": 1,
                "surname": "surname",
                "timezone": "Europe/Madrid"
            },
            "status": {
                "activation_time": null,
                "conditions": [],
                "parameters": {
                    "ranges": {}
                },
                "send_reminders": true,
                "status": "inactive",
                "status_reason": ""
            }
        },
        "extra": {
            "office_uid": "offices-0K5F4MI9G9GsVOt1L98gm9tBN7A",
            "organization_uid": "organizations-0K9NW330VRVPHFJAMmQHO28zIa0",
            "partner_uid": "partners-0KN8XO2ZQIsBe1Iu2VvOl2gzf3e"
        }
    },
    "date": "2024-01-24T10:34:16.186772Z",
    "group": "rcp-v2",
    "metadata": {
        "trace_id": "6ac9f83fdc845c20"
    },
    "namespace": "",
    "resource": "ehrs",
    "target": "EHR-1hktghppi3igCczrECJEHqFsVT2",
    "type": "created",
    "uid": "1HKTGHQ7Q-NSED7GNOFVQA100776"
}
```

### update sample

```javascript
{
    "data": {
        "extra": {
            "office_uid": "offices-0K5F4MI9G9GsVOt1L98gm9tBN7A",
            "organization_uid": "organizations-0K9NW330VRVPHFJAMmQHO28zIa0",
            "partner_uid": "partners-0KN8XO2ZQIsBe1Iu2VvOl2gzf3e"
        },
        "update": {
            "data": {
                "spec": {
                    "alternative_id": "aid2",
                    "contact_preference": "text",
                    "ehr": {
                        "birth_time": "2020-01-01T00:00:00Z",
                        "height": 100,
                        "meta": {
                            "notes": "abc"
                        }
                    },
                    "ehr_id": "p-ehrid",
                    "gender": "U",
                    "insurance_meta": {
                        "a": 1
                    },
                    "is_billable": true,
                    "locale": "es",
                    "login": "test-proxy2@patient",
                    "medicals": [
                        {
                            "primary": false,
                            "role": "m",
                            "uid": "medicals-0K5F7CKVOA4mq4CAPB63Oqs39DT"
                        }
                    ],
                    "name": "test-proxy2",
                    "office_uid": "offices-0K5F4MI9G9GsVOt1L98gm9tBN7A",
                    "primary_medical": "medicals-0K5F7CKVOA4mq4CAPB63Oqs39DT",
                    "primary_nurse": "medicals-0K5F7CKVOA4mq4CAPB63Oqs39DT",
                    "programs": [
                        "TLH"
                    ],
                    "risk_factor": 1,
                    "surname": "surname",
                    "timezone": "Europe/Madrid"
                },
                "status": {
                    "activation_time": null,
                    "conditions": [],
                    "parameters": {
                        "ranges": {}
                    },
                    "send_reminders": true,
                    "status": "inactive",
                    "status_reason": "creation"
                }
            }
        }
    },
    "date": "2024-01-24T15:33:00.865952Z",
    "group": "rcp-v2",
    "metadata": {
        "trace_id": "77f39a89e47466ac"
    },
    "namespace": "",
    "resource": "patients",
    "target": "patients-0LRRY2G4VR3eksSbcmLVvjK0Fjq",
    "type": "updated",
    "uid": "1HKU1KQQ1-KL449M0B677LR9KBPT"
}
```

See down events for _device_attached_, _device_detached_, _devicehub_attached_ and _devicehub_detached_ in their corresponding _Device_ and _Hub_ sections


## Hubs

An event will be generated when a hub is created, updated or deleted

* resource is `devicehubs`
* type is `created`, `updated`, `deleted`
* target is hubs's uid

sample:

```
{
  "data": {
    "spec": {
      "devicebox_uid": null,
      "financially_liable": "",
      "hub_id": "hub123",
      "label": "H-KKK",
      "location": "loc",
      "meta": {},
      "status": "my-status"
    }
  },
  "metadata": {
    "trace_id": "91e7d0c396a003d5"
  },
  "resource": "devicehubs",
  "target": "devicehubs-0LR69MMPSGw8We1msyOUTKHuAc9",
  "timestamp": "2024-01-09T11:25:42.828498Z",
  "type": "update",
  "uid": "1HJMVH7HC-4GQF4F69TAC82L9QIY"
}
```

also, an event for resouce 'patients' will be generated when a hub is attached or detached from a patient. 

* resource is `patients`
* type is `devicehub_attached` or `devicehub_detached`
* target is patient's uid

sample:

```
{
    "data": {
        "extra": {
            "organization_uid": null
        },
        "hub_uid": "devicehubs-0LR6AQU9FWaJZ1eJDRsMIM3FYKH"
    },
    "date": "2024-01-09T11:57:05.019479Z",
    "group": "rcp-v2",
    "metadata": {
        "patient_uid": "patients-016d96379484Aaib3ZrYEjceD27",
        "trace_id": "ab70eb5c613ef8e4"
    },
    "namespace": "",
    "resource": "patients",
    "target": "patients-016d96379484Aaib3ZrYEjceD27",
    "type": "devicehub_detached",
    "uid": "1HJN1ALJR-3ADURXCABAGD141VK8"
}
```



## Devices

An event will be generated when a device is created, updated or deleted

* resource is `devices`
* type is `created`, `updated`, `deleted` and `office_updated`
* target is devices's uid

sample:

```
{
    "data": {
        "spec": {
            "device_id": "AA:BB:CC:DD:EE:FF",
            "financially_liable": "liable",
            "label": "BP01-KKK",
            "location": "loc",
            "meta": {},
            "status": "my-status"
        }
    },
    "date": "2024-01-09T16:58:21.976996Z",
    "group": "rcp-v2",
    "metadata": {
        "trace_id": "a53a68ad872d12ce"
    },
    "namespace": "",
    "resource": "devices",
    "target": "devices-0LR6LIHXX4BC17L3K7Ybs8VtDck",
    "type": "create",
    "uid": "1HJNIIASO-F8IRS2F2O763SMW2SF"
}
```

sample for office_updated:

```
{
    "data": {
        "prev_uid": null,
        "time": "2024-01-09T16:58:30.460691Z",
        "uid": "offices-016d4dc11e16SrGukg8CI0io2se"
    },
    "date": "2024-01-09T16:58:30.871068Z",
    "group": "rcp-v2",
    "metadata": {
        "device_id": "AA:BB:CC:DD:EE:FF",
        "device_uid": "devices-0LR6LIHXX4BC17L3K7Ybs8VtDck",
        "label": "BP01-KKK",
        "trace_id": "bf5316219c09b794"
    },
    "namespace": "",
    "resource": "patients",
    "target": "",
    "type": "office_updated",
    "uid": "1HJNIIJIN-5FLDWCO473JWAOND35"
}
```

also, an event for resouce 'patients' will be generated when a device is attached or detached from a patient. 

* resource is `patients`
* type is `device_attached` or `device_detached`
* target is patient's uid

sample:

```
{
    "data": {
        "device_id": "AA:BB:CC:DD:EE:FF",
        "device_uid": "devices-0LR6LIHXX4BC17L3K7Ybs8VtDck",
        "label": "BP01-KKK"
    },
    "date": "2024-01-09T16:58:23.033169Z",
    "group": "rcp-v2",
    "metadata": {
        "patient_uid": "patients-016d96379484Aaib3ZrYEjceD27",
        "trace_id": "5417fadc65dd135f"
    },
    "namespace": "",
    "resource": "patients",
    "target": "patients-016d96379484Aaib3ZrYEjceD27",
    "type": "device_attached",
    "uid": "1HJNIIBTP-CLZKEWK89ZU5A4MI7L"
}
```

## Device Accounts

An event will be generated when a device account is created, updated or deleted

* resource is `deviceaccounts`
* type is `created`|`updated`|`deleted`
* target device accounts's uid
* data contains spec (same for create and update)

### sample

```javascript
{
    "data": {
        "spec": {
            "class": "test",
            "devices": [],
            "is_authorized": true,
            "patient_uid": "patients-016d96379484Aaib3ZrYEjceD27",
            "scopes": [],
            "user_id": "user_id"
        }
    },
    "date": "2024-01-23T09:43:40.314665Z",
    "group": "rcp-v2",
    "metadata": {
        "trace_id": "96f29fb0ad30bd29"
    },
    "namespace": "",
    "resource": "deviceaccounts",
    "target": "DeviceAccount-1hkqr8e3dTxcLfkvJwJOfFKYSPg",
    "type": "created",
    "uid": "1HKQR8EGQ-8DQD9WQ28C9XW81DEA"
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
* target is observations's uid

sample:

```javascript
{
    "data": {
        "extra": {
            "office_uid": "offices-016d4dc11e16SrGukg8CI0io2se",
            "organization_uid": "organizations-016e46137059Fw7IyqENPAeud9c",
            "partner_uid": "partners-0KN8XO2ZQIsBe1Iu2VvOl2gzf3e",
            "reporter_uid": "r1",
            "virtual_device_id": "v1"
        },
        "spec": {
            "alerts": {
                "bpm": {
                    "min": 1.0,
                    "value": 0.0
                }
            },
            "notes": "abc",
            "patient_uid": "patients-016d96379484Aaib3ZrYEjceD27",
            "reading": {
                "bpm": 0
            },
            "time": "2023-12-21 11:11:01.175285Z",
            "type": "bpm",
            "device_protocol": "adfb102w",
            "device_type": "bpm",
            "device_uid": "devices-016efebc2588AsPqAzbHIjJGt69",
            "is_alert": true,
            "is_self_report": true,
            "is_test": true,
            "is_valid": false
        }
    },
    "date": "2023-12-21T11:11:02.490591Z",
    "group": "rcp-v2",
    "metadata": {
        "trace_id": "bc58c7a96e439440"
    },
    "namespace": "",
    "resource": "observations",
    "target": "observations-1hi614mk9P0d5u7779CGkyLPS2I",
    "type": "created",
    "uid": "1HI614MQQ-FG8TZBHD1Z6VNF0M7Z"
}
```


### Survey launches

An event will be generated every time a launched event updates its status

* resource is `surveys`
* type is `status`
* possible values for last_status are `launched`, ...

sample:

```javascript
{
    "data": {
        "spec": {
            "type": "text",
            "script_id": "ccm_08_diabetes_type2_survey",
            "expire_time": "2024-04-12T11:30:34.408050Z",
            "patient_uid": "patients-016d96379484Aaib3ZrYEjceD27",
            "careplan_uid": "",
            "survey_uid": "Survey-1hr900pgp6LdeoMDyMBSDEPJUcH"
        }
        "status": {
            "last_status_time": "2024-04-12T11:10:34.408438Z",
            "last_status": "launched"
        },
    },
    "type": "status",
    "date": "2024-04-12T11:10:34.674495Z",
    "group": "rcp-v2",
    "metadata": {
        "trace_id": "26f49ea46d0cacca"
    },
    "uid": "1HR9012LI-PAFA81Z1PNB1KS7YY4-514",
    "resource": "surveys",
    "target": "surveys-1hr9012d8LJSRnWSkVGJfsCVbho",
    "namespace": ""
}
```

If the survey is answered, questions and answers will appear:

```javascript
{
}
```



