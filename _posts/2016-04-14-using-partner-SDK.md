---
layout: post
title:  "Using Partner SDK to record extended Telemetry"
date:   2016-04-14 15:21:12 +0530
categories: genie-service sdk android telemetry partner extended-telemetry
---
# Pre-requisite
Have a look at [Consuming genie services SDKs in native Android]({% post_url 2016-04-14-using-genie-service-SDK %})
for a better understanding on how to integrate with the Partner SDK.

# Usage
Partner SDK can be consumed by clients who want to send extended data which is is not already covered in [Content Telemetry specifications]({% post_url 2016-04-14-content-telemetry-spec %}).
This extended data is encrypted on Genie and available for download from the Ekstep Cloud Platform as described at [Retrieving extended Telemetry](2016-04-04-retrieving-telemetry).

Note that even though the extended data has no specifications, developers are advised to follow best practices and policies as described
at [EkStep Data Policy and Limits]().

# Dependencies
Download the latest SDK and AIDL files from [here](https://platform.ekstep.org/downloads/content/repositories/production/org/ekstep/genieservices).

# API
Find the public methods for the Telemetry SDK below:

*register*

```java
/**
 * @param partnerID Unique Identifier of the Partner
 * @param publicKey Provide an appropriate RSA Public Key as a String in PEM format
 *                  with the delimiter.
 *                  <p>Example:</p>
 *
 *                  <p>-----BEGIN PUBLIC KEY-----
 *                  MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQDGdo5VYOK9cTrQQ+ajOxfHMgg/
 *                  TDX77o/eVTUjcErLLYKBQ6qb8t/jCCuRNexIexRBldk4gC9STyuVWN8x2xkSildf
 *                  Nch3KUTvwgJx1n2y/03tIHkimOxEONCg3rWPdiWx7nLdW4TuHbwZTZmMdhLjM4lI
 *                  OSyoyYpX/JmDnxjq4QIDAQAB
 *                  -----END PUBLIC KEY-----</p>
 *
 *                  <p>To generate an RSA Private key pair use,</p>
 *                      <p>openssl genrsa -out rsa_2048_priv.pem 2048</p>
 *                  <p>To generate the Public Key in PEM format use,</p>
 *                      <p>openssl rsa -pubout -in rsa_2048_priv.pem -out rsa_2048_pub.pem</p>
 *
 *                  <p>Use this to register a Partner.
 *
 *                  <p>In case of a failed response, response.getError() could return one
 *                  of the following:
 *
 *                  <p>MISSING_PARTNER_ID - partnerID is not provided</p>
 *                  <p>MISSING_PUBLIC_KEY - publicKey is not provided</p>
 *                  <p>INVALID_RSA_PUBLIC_KEY - publicKey is not as per specs</p>
 *                  <p>INVALID_DATA - some data is still invalid</p>
 *
 * @param responseHandler
 */
public void register(String partnerID, String publicKey,IResponseHandler responseHandler)
```

**Errors**

* MISSING_PARTNER_ID - partnerID is not provided
* MISSING_PUBLIC_KEY - publicKey is not provided
* INVALID_RSA_PUBLIC_KEY - publicKey is not as per specs (as mentioned above in the javadoc)
* INVALID_DATA - some data is still invalid

*startPartnerSession*

```java
/**
* @param partnerID Unique Identifier of the Partner
* @param responseHandler Callback handler to manage SDK async response
*
*                        Use this to start a Partner session.
*
*                        In case of a failed response, response.getError() could return one
*                        of the following:
*
*                        UNREGISTERED_PARTNER - partnerID is not registered
*/
public void startPartnerSession(String partnerID,IResponseHandler responseHandler)
```

**Errors**

* UNREGISTERED_PARTNER - partnerID is not registered

*terminatePartnerSession*

```java
/**
* @param partnerID Unique Identifier of the Partner
* @param responseHandler Callback handler to manage SDK async response
*
*                        Use this to terminate a Partner session.
*
*                        In case of a failed response, response.getError() could return one
*                        of the following:
*
*                        UNREGISTERED_PARTNER - partnerID is not registered
*
*/
    public void terminatePartnerSession(String partnerID,IResponseHandler responseHandler)
```

**Errors**

* UNREGISTERED_PARTNER - partnerID is not registered

*sendData*

```java
/**
* @param partnerID Unique Identifier of the Partner
* @param partnerData A Map representation of the data which the Partner wants to send
* @param responseHandler Callback handler to manage SDK async response
*
*                        Use this to send private data to Genie Services. This call will
*                        encrypt partnerData with the publicKey given earlier during
*                        registration.
*
*                        response.getResult() will return a Map with the encrypted data
*                        {
*                          "encrypted_data":"..."
*                        }
*                        In case of a failed response, response.getError() could return one
*                        of the following:
*
*                        UNREGISTERED_PARTNER - partnerID is not registered
*                        ENCRYPTION_FAILURE   - Genie Services could not encrypt data
*
*/
public void sendData(String partnerID,Map<String,Object> partnerData,IResponseHandler responseHandler)
```

**Errors**

* UNREGISTERED_PARTNER - partnerID is not registered
* ENCRYPTION_FAILURE - Genie Services could not encrypt data
