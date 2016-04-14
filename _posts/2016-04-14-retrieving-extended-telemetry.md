---
layout: post
title:  "Retrieving extended Telemetry"
date:   2016-04-14 15:21:12 +0530
categories: genie-service sdk android extended-telemetry
---
# Data Exhaust Dataset API

``POST /v1/datasets/{datasetId}/{resourceId}/{fromDate}/{toDate}``

This api will return the Data Exhaust Dataset based on datasetId, resourceId, fromDate and toDate.

* **datasetId**: ID of the dataset.
* **resourceId**: ID of the resource. For partner data, this will be the hash of the partner id (org.ekstep.partner.name). Hash must be generated using SHA-1 algorithm and must be 128 bits long
* **fromDate**: Must be in YYYY-MM-DD format. Will be defaulted to yesterday, if not specified
* **toDate**: Must be in YYYY-MM-DD format. Will be defaulted to yesterday, if not specified. toDate must be greather than or equal to fromDate, toDate must be less than today. This is as per

**Note**: Maximum one month's (31 days) data could be downloaded in one API call

## Request

```javascript
{
    "id": "ekstep.data_exhaust_dataset_service",
    "ver": "1.0",
    "ts": "2015-08-04T17:36:36+05:30",
    "params": {
        "requesterId": "",
        "did": "ff305d54-85b4-341b-da2f-eb6b9e5460fa",
        "key": "",
        "msgid": "ff305d54-85b4-341b-da2f-eb6b9e5460fa"
    },
    "request": {
         "licensekey": "61d81fac-cc60-4f2f-9fe7-daef674af21a"
    }
}
```

## Response

In case of success, a zip file is returned in response body. This zip file will internally contain multiple zip files, one file per day.

In case of error, an appropriate error response is returned, as JSON, and the format of the same is below:

```javascript
{
    "id": "ekstep.data_exhaust_dataset_service",
    "ver": "1,0",
    "ts": "",
    "params": {
        "resmsgid": "054f3b10-309f-4552-ae11-02c66640967b",
        "msgid": "ff305d54-85b4-341b-da2f-eb6b9e5460fa",
        "status": "successful", // successful or failed
        "err": "", //Will contain error code if status is 'failed'
        "errmsg": "" //Will contain an error message if status is 'failed'
    }
}
```

Possible error codes are:

* **INVALID_DATA_ERROR**: Request JSON is not parsable
* **INVALID_DATE**: fromDate is after toDate or toDate is not before today
* **DATE_RANGE_TOO_LARGE**: Data requested for more than 31 days
* **AUTHORIZATION_FAILED**: User does not have access to the given dataset or resource
* **LOGIN_FAILED**: License key is invalid
* **INTERNAL_ERROR**: Technical error in the API
