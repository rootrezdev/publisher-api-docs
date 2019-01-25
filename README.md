# RootRez Publisher API Documentation

This document is meant to guide a developer through the process of connecting to the RootRez API. 
The Publisher API operates as a gateway for registered publishers to view properties, book 
reservations, and retrieve other meta data you’d except from an Online Travel Agency (OTA). 
The API is a restful-like web service that can be accessed via JSON payloads over HTTPS GET or
 POST depending on the endpoint.

The typical workflow of your application will be:

- Retrieve Publisher Settings
- Availability Search
- Property Details
- Checkout Preview
- Book Reservation
- View Reservation

## Environments

Prior to being given a production key your application will need to be certified by the 
RootRez development team. Depending on how you’re integrating with RootRez will determine 
whether you will develop to sandbox or staging.

**Sandbox** 

https://api-sandbox.rootrez.com/publisher/v3.0/

**Production** 

https://svc.rootrez.com/publisher/v3.0/

## Authentication

Access can be obtained through the use of a Publisher Key. This key will be provided by 
your technical implementation coordinator.


## Hello World 

Get started by saying hello world to us.

|  Endpoint | /publisher/v3.0/.json?key=MyKey |
| ------------- | ------------- |
| HTTP  | GET  |


Response:

```json
{
    "uptime": "4 days 23:37:42.78",
    "message": "Thanks for saying hello. Everything looks good so far."
}
```

## HTTP POST Requirements

When doing HTTP POST requests to the API, a client browser session_id key must be included in the 
JSON payload. Additional data such as client IP address and user agent are recommended to assist 
RootRez in debugging issues.

```json
{
   "key": "MyAPIKey",
   "meta_data":{
      "session_id":"1aa1b1671982e8539aeed0fcd2aadc54",
      "user_agent":"Mozilla\/5.0 (X11; Linux x86_64) AppleWebKit\/537.36 (KHTML, like Gecko) Chrome\/63.0.3239.132 Safari\/537.36",
      "ip":"127.0.0.1"
   }
}
```

## HTTP Status Codes

HTTP Status Codes are used to provide errors, these will be accompanied by error messages. 
Successful requests will result in a 200 OK code. Below is a list of the most commonly used 
error status codes and corresponding examples.

**400 Bad Request**

Your request is likely missing required information or json is malformed.

**401 Unauthorized**

Authentication failed, you probably have an invalid key or a key has not been setup.

**404 Not Found**

Resource or hotel property does not exist, or cannot be returned due to business logic.

**5xx Server Exceptions**

See below.

## Exceptions

Most exceptions will be accompanied with additional data. These can be caught in your application and specific, 
user-friendly messages can be created for your end-users.

**RoomUnavailableException**

The requested room is not available for booking.

**BookingValidationException**

Invalid data was received in your booking request

**AddressValidationException**

Any address related errors on a booking request.

**CreditCardException**

Invalid credit card data. Either we couldn’t verify the cardholders account or it failed a 
Luhn algorithm check. You should pre-check credit cards against the Luhn algorithm in your 
implementation.

**RemoteInventoryException**

RootRez was unable to verify the inventory exists in the hotels inventory system or within a 
third-parties inventory system.

**RemoteInformationException**

RootRez was unable to retrieve hotel meta data from the hotels information system and/or 
third party system.

**DuplicateBookingException**

A similar booking has already been processed.

**InternalErrorException**

These are general exception messages. If you come across generic exceptions please 
submit a bug report: https://www.rootrez.com/support/