# Netcomposer RCP

## Connecting to RCP - Development

The external RPC can be used in two ways:

### Using http/s

* You must send a `POST` to `https://cloud.hub.remotecarepartners.com/[dev|qa|prod]/netcomp_rcp/rpc/api` with `Content-Type` equal to `application/json`
* For nearly all operations you must login first [login](doc/common.md#login) to the server.
* Alternatively, you can include a header `Authorization: Bearer (Token)` with the value of the token you got when login, or use _basic authentication_ with a valid _user_ and _password_ 
* You must provide a JSON payload as the following sample:
```
    "cmd": "rcp/v1a1/create_office",
    "data": {
        ...
    }
```


### Using ws/s

* You must connect to `https://cloud.hub.remotecarepartners.com/[dev|qa|prod]/netcomp_rcp/rpc/ws`
* For nearly all operations you must login first [login](doc/common.md#login) to the server, and the connection will be authenticated.
* You must provide a JSON payload as the following sample:
```
    "tid": 1,
    "cmd": "rcp/v1a1/create_office",
    "data": {
        "token": ...
        ...
    }
```
* A response will be provided with the same `tid`



### Available RPC commands

Commands available (all with cmd prefix `rcp/v1a1/`)

* [Common](doc/common.md)
* [Partners](doc/partners.md)
* [Organizations](doc/organizations.md)
* [Offices](doc/offices.md)
* [Staff](doc/staff.md)
* [Medicals](doc/medicals.md)
* [Patients](doc/patients.md)
* [DeviceBoxes](doc/deviceboxes.md)
* [DeviceHubs](doc/devicehubs.md)
* [DeviceClasses](doc/deviceclasses.md)
* [Devices](doc/devices.md)
* [RuleClasses](doc/ruleclasses.md)
* [Rules](doc/rules.md)
* [Readings](doc/readings.md)
* [Observations](doc/observations.md)
* [Notifications](doc/notifications.md)
* [Timeslots](doc/timeslots.md)
* [CarePlanTypes](doc/careplan_types.md)
* [CarePlans](doc/careplans.md)
* [Surveys](doc/surveys.md)
* [Calls](doc/calls.md)
* [PreOrders](doc/preorders.md)
* [Orders](doc/orders.md)
* [Views](doc/views.md)
* [Chat](doc/chat.md)



## Connecting to API

A number of external APIs are available for several operations:

* Port is 9001 for http, 9003 for https
* You must send a request to `https://cloud.hub.remotecarepartners.com/[dev|qa|prod]/netcomp_rcp/api/...`

Available APIs:

* GET /test
* GET /report_sample_data
