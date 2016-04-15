---
layout: post
title:  "Consuming genie services SDKs in native Android"
date:   2016-04-14 15:21:12 +0530
categories: genie-service sdk android
---
The Genie Services SDK provides two kinds of reponses: GenieResponse and GenieListResponse.

**GenieResponse**

This response is used when the result contains a single object (eg: getCurrentProfile()). It has following getters:

```
* getStatus - (String) - status of the call.
* getErrorMessages - (List<String>) - List of error messages if the call failed.
* getError - (String) - Type of error (like NETWORK_ERROR, DB_ERROR etc).
* getMessage - (String) - If the call is successful, message related to the call if any.
* getResult - (Map< String, Object >) - The result of the call, if the call is successful.
```
**GenieListResponse**

This response is used when the result contains an array of object (eg: getAllProfiles()). It has following getters:

```java
* getStatus - (String) - status of the call.
* getErrorMessages - (List<String>) - List of error messages if the call failed.
* getError - (String) - Type of error (like NETWORK_ERROR, DB_ERROR etc).
* getMessage - (String) - If the call is successful, message related to the call if any.
* getResults - (List< Map< String, Object > >) - The results of the call, if the call is successful.
```

These responses will be used in following ResponseHandlers: `IResponseHandler` (uses GenieResponse) and `IListResponseHandler` (uses GenieListResponse). Depending on the functionality one of the interface needs to be implemented in the client and passed to the sdk. This is the class which will recieve the response when the call succeeds or fails. Here are the sample implementation of both the handlers

**Sample Implementation for IResponseHandler**

```java

public class ResponseHandler implements IResponseHandler {
    /**
    This is callback for success, it will be invoked by sdk when the call was successful.
    @param response - (GenieResponse) - contains all the relevant information for the call
    **/
    @Override
    public void onSuccess(GenieResponse response) {
        // Code to handle success scenario
    }

    /**
    This is callback for failure, it will be invoked by sdk when the call fails for some reason.
    @param response - (GenieResponse) - contains all the information for the call failure
    **/
    @Override
    public void onFailure(GenieResponse response) {
        // Code to handle error scenario
    }
}
```

**Sample Implementation for IListResponseHandler**

```java
/** Sample Implementation for IListResponseHandler **/
public class ResponseHandler implements IListResponseHandler {
    /**
    This is callback for success, it will be invoked by sdk when the call was successful.
    @param response - (GenieListResponse) - contains all the relevant information for the call
    **/
    @Override
    public void onSuccess(GenieListResponse response) {
        // Code to handle success scenario
    }
    /**
    This is callback for failure, it will be invoked by sdk when the call fails for some reason.
    @param response - (GenieListResponse) - contains all the information for the call failure
    **/
    @Override
    public void onFailure(GenieListResponse response) {
        // Code to handle error scenario
    }
}
```
