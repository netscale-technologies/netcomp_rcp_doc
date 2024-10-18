
# API

This is the API for the second engine for medications management in the core (after the first one based on snapshots)

In this engine, you simply create individual medications for a patient, each of them having an status and an an associated log of actions (populated from surescripts)

## Medications

### create_medication

You must include field `spec` with following fields:


|Field|Type|Mandatory|Description
|---|---|---|---
|patient_uid|string|Y|Patient we refer to
|code|string|Y|Code for medication, usually and NDC
|code_type|string|Y|`ndc`, `rxcui`
|name|string|Y|Medication's title
|name_type|string|N|
|status|string|Y|Current status. Accepted values: (TO DECIDE)
|description|string|N|Extended description of medication
|origin|string|Y|`sc` `openfda`, `nih`, `custom`
|directions|string|N|
|notes|string|N|
|fda_extra| fda_extra|N|See bellow

fda_extra format:

|Field|Type|Mandatory|Description
|---|---|---|---
|active_ingredients| [ingredient] |N|
|quantity|string|N|
|route|string|N|
|type| string |N|
|conditions| [string] |N|

ingredient format:

|Field|Type|Mandatory|Description
|---|---|---|---
|name|string|N|
|strentgh|string|N|

If everything is ok, you will get a new patient_meds' uid

### update_medication

Can be used to update medication records. Fields `uid` and `spec` must be provided. Current editable fields are:
* name
* status
* description
* fda_extra
* directions
* notes

  
### list_medications

Allows to list current medications for a patient.

|Field|Type|Mandatory|Description
|---|---|---|---
|patient_uid|string|Y|
|code|string|N|
|status|string|N

Creation fields (from user or Surescripts) will be returned into "spec" object. Example:

```json
{
  "patient_uid": "patient-0f78482118fb41da886978652f66cf62",
  "code": "00093310905",
  "code_type": "nd",
  "name": "AMOXICILLIN 500 MG CAPSULE",
  "name_type": "..",
  "directions": "TAKE 1 CAPSULE BY MOUTH TWICE DAILY WITH MEALS",
  "notes": "..."
  "ext_id": "fd9b2f06-935e-4f7b-8695-a2fdec7698f7",
  "origin": "sc",
  "status": "pending",
  "fda_extra": {
    "active_ingredients": [
      {"name": "...", "strengh": "..."}
    ],
    "quantity": "...",
    "route": "...",
    "type": "..."
    "conditions": ["..."]
  }
}
```


Also, "status" object will return updates made to the core fields. It also contains last update from surescripts, if available:

```json
{
  "updates": [
    {
      "update": {"directions": "... new directions ..."},
      "timestamp": "2024-10-10T10:07:17.099262Z",
      "made_by": "Medical-XXX"
    }
    {
      "update": {"status": "pending"},
      "timestamp": "2024-10-07T17:07:17.099262Z"
    },
    {
      "update": {"status": "created"},
      "timestamp": "2024-10-07T17:07:17.099262Z"
    },
  ],
  "sc_extra": {
    "medication": {
      "id": "fd9b2f06-935e-4f7b-8695-a2fdec7698f7",
      "added_at": "2024-10-04T17:11:23.070386+00:00",
      "pharmacy": {
        "npi": "1851306070",
        "zip": "72944",
        "city": "DERBY",
        "name": "ROBERT & BILL'S DISCOUNT DRUGS",
        "phone": "6825315059",
        "state": "AR",
        "ncpdpid": "7535703",
        "fax_number": null,
        "address_line1": "745 N CHICKADEE LN",
        "address_line2": "Suite 220"
      },
      "strength": null,
      "form_code": null,
      "directions": "TAKE 1 CAPSULE BY MOUTH TWICE DAILY WITH MEALS",
      "prescriber": {
        "dea": null,
        "npi": "8975432156",
        "zip": "36777",
        "city": "Mobile",
        "phone": "3348877666",
        "state": "AL",
        "last_name": "O'Neal",
        "fax_number": null,
        "first_name": "Janice",
        "middle_name": null,
        "name_prefix": null,
        "name_suffix": null,
        "address_line1": "1234 gray pl",
        "address_line2": null,
        "state_license_number": null,
        "place_location_qualifier": null
      },
      "updated_at": "2024-10-15T17:07:43.9107181+00:00",
      "days_supply": 7,
      "patient_uid": "patient-0f78482118fb41da886978652f66cf62",
      "dea_schedule": null,
      "product_code": "00093310905",
      "written_date": "2024-10-13",
      "drug_db_coded": null,
      "refills_value": 8,
      "strength_code": null,
      "substitutions": null,
      "history_source": {
        "bin": null,
        "pcn": null,
        "group_id": null,
        "plan_code": null,
        "qualifier": "P2",
        "fill_number": "00",
        "payment_code": null,
        "cardholder_number": null,
        "reference_id_value": "7535703",
        "source_description": "ROBERT & BILL'S DISCOUNT DRUGS",
        "prescription_number": "271927318",
        "reference_id_qualifier": "D3",
        "electronic_rx_ref_number": "00000000000000000000027206841398086",
        "electronic_prescription_order_number": "3333367661234740000"
      },
      "drug_description": "AMOXICILLIN 500 MG CAPSULE",
      "form_source_code": null,
      "latest_sold_date": "2024-10-13",
      "prior_auth_value": null,
      "unit_source_code": null,
      "potency_unit_code": null,
      "refills_qualifier": null,
      "latest_filled_date": "2024-10-13",
      "code_list_qualifier": null,
      "quantity_prescribed": 180,
      "prior_auth_qualifier": null,
      "strength_source_code": null,
      "drug_db_code_qualifier": null,
      "product_code_qualifier": "ND",
      "latest_update_request_source": "Notifications",
      "latest_update_medication_source": "Pharmacy"
    }
  }
}
```


### list_medication_logs

Allows to list "log updates" for a patient's medication. 
You must provide medication_uid, and if will be sorted by `fill_date`, descending

|Field|Type|Mandatory|Description
|---|---|---|---
|medication_uid|string|Y|
|patient_uid|string|N|Needed unless you have global permissions
|cursor|string|N|For pagination, or going to a fixed date
|size|integer|N|Max: 1000, default: 100









