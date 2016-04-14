---
layout: post
title:  "Using Telemetry SDK to record content Telemetry"
date:   2016-04-14 15:21:12 +0530
categories: genie-service sdk android telemetry
---
# TELEMETRY

*send*

```java
/**
Send a single event to genie-services via genie-sdk
@param event - (String) - The event in the form of a json string
@param responseHandler - (IResponseHandler) - This is the callback. The class needs to be defined in client handling both success and failure scenario
@return - (void) - there is no return from the function. However when the call is done, the response will be sent to the responsehandler injected.
*/
public void send(String event, IResponseHandler responseHandler)

```
**Errors**:

* DB_ERROR : If genie service can't save the event in database.
* INVALID_EVENT : If the event is not a valid telemetry event. In case of development genie service build, you can get the details of errors in the errorMessages in the response.
* VALIDATION_ERROR - when event is not a valid json or is not a valid request when validation is enabled
* GENIE_SERVICE_NOT_INSTALLED - when the genie service is not installed
