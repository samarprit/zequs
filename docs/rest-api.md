# Zequs API

Date: June 8, 2016

Document Version: v1.0

# Overview
This document describes the Zequs API v1.0, which supports queuing of images
from a client for handling by Zequs. The Zequs API provides a RESTful JSON 
interface for interacting with and managing the Zequs print queue.

# Common Responses
The Monasca API utilizes HTTP response codes to inform clients of the success
or failure of each request. Clients should use the HTTP response code to
trigger error handling if necessary. This section discusses various API error
responses.

* 200 - A request succeeded.
* 400 - Bad request
* 404 - Not found
* 500 - Internal Error

# JSON Results
All Zequs API results are in the form of JSON. In some cases, the JSON is an
empty object, when no meaningful data is to be returned.

# Get information about a specific print job

## GET /api/v1/zequs/{id}/

### Headers
* Accept (string) - application/json

### Path Parameters
Numeric ID of print job. The id was returned when the print job was created, 
and will be valid until the print job is deleted.

### Query Parameters
None.

### Request Body
None.

### Request Examples
```
GET /api/v1/zequs/17/ HTTP/1.1
Host: 192.168.10.4:8080
Accept: application/json
Cache-Control: no-cache
```

## Response
#### Status code
* 200 - OK
* 404 - Print job not found

### Response Body
Returns a JSON object that describes the status of the job. This status
consists of two fields. 

* 'id' is the print job ID. 
* 'state' is the state of the job, and can be one of the following values:

* 0 - queued 
* 1 - printing
* 2 - failed
* 3 - complete

### Response Examples
```
{
    "id": 17
    "state": 3
}
```
___

## Get Printer Status
Gets overall status of the printer.

### Get /api/v1/zequs/

#### Headers
* Accept (string) - application/json

#### Path Parameters
None.

#### Query Parameters
None.

#### Request Body
None.

#### Request Examples
```
GET /api/v1/zequs/ HTTP/1.1
Host: 192.168.10.4:8080
Content-Type: application/json
Cache-Control: no-cache
```

### Response
#### Status code
* 200 - OK
* 500 - Internal error

#### Response Body
Returns a JSON object with general status of the printer.

Fields returned include:

* enabled - true if printer enabled, else false
* jobs - number of jobs zequs is maintaining state for
* queued -  number of queued jobs
* printing - number of printing jobs
* failed - number of failed jobs
* complete - number of completed jobs

Only jobs that have not been deleted are reported. 

#### Response Examples
```
{
   "enabled": true,
   "jobs": 11,
   "queued": 4,
   "printing": 1,
   "failed": 0,
   "complete": 6
}
```
___

## Delete a job
Removes a job. If queued, it is removed from the queue. Completed or failed
jobs are retained until explicitly deleted (this allows a client to poll for
job status to determine the print job state)

### Delete /api/v1/zequs/{id}/

#### Headers
* Accept (string) - application/json

#### Path Parameters
id - numeric ID of the print job, which was returned when the print job was
created

#### Query Parameters
None.

#### Request Body
None.

#### Request Examples
```
DELETE /api/v1/zequs/18/ HTTP/1.1
Host: 192.168.10.4:8080
Content-Type: application/json
Cache-Control: no-cache
```

### Response
#### Status code
* 200 - OK
* 404 - Not found, no such print job

#### Response Body
An empty JSON object.
___

## Delete all jobs
Removes all jobs. 

### Delete /api/v1/zequs/

#### Headers
* Accept (string) - application/json

#### Path Parameters
None.

#### Query Parameters
None.

#### Request Body
None.

#### Request Examples
```
DELETE /api/v1/zequs/ HTTP/1.1
Host: 192.168.10.4:8080
Content-Type: application/json
Cache-Control: no-cache
```

### Response
#### Status code
* 200 - OK

#### Response Body
An empty JSON object.

___

## Add a print job 
Places a job in the print queue and makes its state "queued"

### POST /api/v1/zequs/

#### Headers
* content-type - application/json

#### Path Parameters
None.

#### Query Parameters
None.

#### Request Body
A JSON object with a data member. The data member consists of a base-64
encoded string that represents the image to be printed. The image encoding
is arbitrary.

#### Request Examples
```
POST /api/v1/zequs/ HTTP/1.1
Host: 192.168.10.4:8080
Content-Type: application/json
Cache-Control: no-cache

{
   "data":"base-64 encoded image data here..."
}
```

### Response
#### Status code
* 200 - OK
* 400 - Bad Request

#### Response Body
An empty JSON object is returned

___

## Enable the printer

Enables printing (default state is "enabled")

### PUT /api/v1/zequs/enable/

#### Headers
* content-type - application/json

#### Path Parameters
None.

#### Query Parameters
None.

#### Request Body
None

#### Request Examples
```
PUT /api/v1/zequs/enable HTTP/1.1
Host: 192.168.10.4:8080
Content-Type: application/json
Cache-Control: no-cache
```

### Response
#### Status code
* 200 - OK

#### Response Body
An empty JSON object is returned

___

## Disable the printer

Disables printing (default state is "enabled")

### PUT /api/v1/zequs/disable/

#### Headers
* content-type - application/json

#### Path Parameters
None.

#### Query Parameters
None.

#### Request Body
None

#### Request Examples
```
PUT /api/v1/zequs/disable HTTP/1.1
Host: 192.168.10.4:8080
Content-Type: application/json
Cache-Control: no-cache
```

### Response
#### Status code
* 200 - OK

#### Response Body
An empty JSON object is returned

## Disable the printer

Disables printing (default state is "enabled")

### PUT /api/v1/zequs/disable/

#### Headers
* content-type - application/json

#### Path Parameters
None.

#### Query Parameters
None.

#### Request Body
None

#### Request Examples
```
PUT /api/v1/zequs/disable HTTP/1.1
Host: 192.168.10.4:8080
Content-Type: application/json
Cache-Control: no-cache
```

### Response
#### Status code
* 200 - OK

#### Response Body
An empty JSON object is returned

___

## Enable test mode

Enables testmode (used by unit tests)

### PUT /api/v1/zequs/testmode/enable/

#### Headers
* content-type - application/json

#### Path Parameters
None.

#### Query Parameters
None.

#### Request Body
None

#### Request Examples
```
PUT /api/v1/zequs/testmode/enable HTTP/1.1
Host: 192.168.10.4:8080
Content-Type: application/json
Cache-Control: no-cache
```

### Response
#### Status code
* 200 - OK

#### Response Body
An empty JSON object is returned

___

## Disable test mode

Disables testmode (used by unit tests)

### PUT /api/v1/zequs/testmode/disable/

#### Headers
* content-type - application/json

#### Path Parameters
None.

#### Query Parameters
None.

#### Request Body
None

#### Request Examples
```
PUT /api/v1/zequs/testmode/disable HTTP/1.1
Host: 192.168.10.4:8080
Content-Type: application/json
Cache-Control: no-cache
```

### Response
#### Status code
* 200 - OK

#### Response Body
An empty JSON object is returned
