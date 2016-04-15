---
layout: post
title:  "Using Telemetry SDK to record content Telemetry"
date:   2016-04-14 15:21:12 +0530
categories: genie-service sdk android telemetry
---
# Pre-requisite
Have a look at [Consuming genie services SDKs in native Android]({% post_url 2016-04-14-using-genie-service-SDK %})
for a better understanding on how to integrate with the Telemetry SDK. The Telemetry SDK can be used to log telemetry
events for a content developer as per the [Content Telemetry specifications]({% post_url 2016-04-14-content-telemetry-spec %}).

# Usage
Telemetry SDK can be consumed by clients who want to log Telemetry events with the EkStep Platform. [Retrieving Telemetry]({% post_url 2016-04-14-retrieving-telemetry %}) describes
how the content developer can extract these Telemetry events. Read [Using Partner SDK to record extended Telemetry]({% post_url 2016-04-14-using-partner-SDK %})
to understand how to log data which falls outside the scope of [Content Telemetry specifications]({% post_url 2016-04-14-content-telemetry-spec %}).

# Dependencies
Download the latest SDK and AIDL files from [here](https://platform.ekstep.org/downloads/content/repositories/production/org/ekstep/genieservices).

# API
Find the public methods for the Telemetry SDK below:

## 1. send
```send``` method which takes in the event json and callback handler. Since, ```send``` is an
asynchronous call, you are advised to wait on the ```responseHandler``` to know if the telemetry event sent earlier got
successfully saved.

### Signature

```java
/**
Send a single event to genie-services via genie-sdk
@param event - (String) - The event in the form of a json string
@param responseHandler - (IResponseHandler) - This is the callback. The class needs to be defined in client handling both success and failure scenario
@return - (void) - there is no return from the function. However when the call is done, the response will be sent to the responsehandler injected.
*/
public void send(String event, IResponseHandler responseHandler)

```

### Possible error codes returned

* DB_ERROR : If genie service can't save the event in database.
* INVALID_EVENT : If the event is not a valid telemetry event. Check [Content Telemetry specifications]({% post_url 2016-04-14-content-telemetry-spec %}).
* VALIDATION_ERROR - when event is not a valid json or is not a valid request when validation is enabled.
* GENIE_SERVICE_NOT_INSTALLED - when the genie service is not installed.
