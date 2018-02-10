# Public Rest API for NPCloud
# General API Information
* The base endpoint is: **https://api.npcloud.io**
* All endpoints return either a JSON object or array.
* Data is returned in **ascending** order. Oldest first, newest last.
* All time and timestamp related fields are in milliseconds.
* HTTP `4XX` return codes are used for for malformed requests;
  the issue is on the sender's side.
* HTTP `5XX` return codes are used for internal errors; the issue is on
  NPCloud's side.
* HTTP `504` return code is used when the API successfully sent the message
but not get a response within the timeout period.
* Any endpoint can retun an ERROR; the error payload is as follows:
```javascript
{
  "code": -1121,
  "msg": "Invalid symbol."
  "detail" : {}
}
```

* Specific error codes and messages defined in later int the document (see **error** section).
* For `GET` endpoints, parameters must be sent as a `query string`.
* For `POST`, `PUT`, and `DELETE` endpoints, the parameters should be sent in the `request body` with content type
  `application/x-www-form-urlencoded`.
* Parameters may be sent in any order.

# Endpoint security type
* Each endpoint has a security type that determines the how you will
  interact with it.
* API-keys are passed into the Rest API via the `X-NPC-APIKEY`
  header.
* API-keys and secret-keys **are case sensitive**.
* API-keys are required for trading rest API


## ENUM definitions

**Task status:**

* NEW
* DOING
* CLOSE
* REJECTED


## General endpoints
### Test connectivity
```
GET /api/v1/ping
```
Test connectivity to the Rest API.

**Weight:**
1

**Parameters:**
NONE

**Response:**
```javascript
{}
```

### Check server time
```
GET /api/v1/time
```
Test connectivity to the Rest API and get the current server time.


**Parameters:**
NONE

**Response:**
```javascript
{
  "serverTime": 1499827319559
}
```

### Contracts information
```
GET /api/v1/exchangeContracts
```
Current trading contracts


**Parameters:**
NONE

**Response:**
```javascript
{
  "contracts: ["VPR", "SVRP"]
}
```

## Data endpoints
### Live task book

Get all living tasks by contract symbol

```
GET /api/v1/tasks
```

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
contract | STRING | YES | contract symbol
limit | INT | NO | Default 100; max 1000.
page | INT | NO | Page number

**Caution:** setting limit=0 will return all living tasks.

**Response:**
```javascript
[{
  "tid": "1233",      // task id
  "contract": "SVPR",    // Task type
  "status": "DOING",    // Task type
  "expireOn": 122333,    // expire time of this task
  "pId": 12345,       // Task's publish user Id
  "price": "0.001",     // Upper price for the solution
  "setting": {}           // detail information on the task
  "solutionNumber": 1       //number of solutions
}]
```

### Task query

Query task by task Id

```
GET /api/v1/taskWithId
```

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
tid | STRING | YES | contract symbol


**Caution:** setting limit=0 will return all living tasks.

**Response:**
```javascript
{
  "tid": "1233",      // task id
  "contract": "SVPR",    // Task type
  "status": "DOING",    // Task type
  "expireOn": 122333,    // expire time of this task
  "ownerId": 12345,       // Task's publish user Id
  "price": "0.001",     // Upper price for the solution
  "setting": {} ,          // detail information on the task
  "solutionCount": 1       //number of solutions
}
```

### Task user's own query

Query current user's own tasks

```
GET /api/v1/taskOfUser
```

**Parameters:**
Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
contract | STRING | YES | contract symbol
limit | INT | NO | Default 100; max 1000.
page | INT | NO | Page number


**Caution:** setting limit=0 will return all user's tasks.

**Response:**
```javascript
[{
  "tid": "1233",      // task id
  "contract": "SVPR",    // Task type
  "status": "DOING",    // Task type
  "expireOn": 122333,    // expire time of this task
  "ownerId": 12345,       // Task's publish user Id
  "price": "0.001",     // Upper price for the solution
  "setting": {}           // detail information on the task
  "solutionCount": 1       //number of solutions
}]
```

### Solution query by task

Query all solution by task Id.

```
GET /api/v1/solutions
```

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
tid | STRING | YES | contract symbol


**Response:**
```javascript
{
  "tid": "1233",      // task id
  "contract": "SVPR",    // Task type
  "status": "DOING",    // Task type
  "ownerId": 12345,       // Task's publish user Id
  "expireOn": 122333,    // expire time of this task
  "price": "0.001",     // Upper price for the solution
  "solutionCount": 1       //number of solutions
  "solutions": [{       // detail information on the task
        "sId": 1222,          //Solution id
        "solverId": "111"     //solver's id
        "objectives": {},     //objectives of this solution, defined by particular contracts(see contracts document)
        "solutionTime": 1233444      //receive time for this solution
  }]
}
```

### Accepted Solution Details by task

Query solution details by task Id. Only task's owner has he privilege to
query this API.

```
GET /api/v1/solutionDetails
```

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
tid | STRING | YES | contract symbol


**Response:**
```javascript
{
  "tid": "1233",      // task id
  "contract": "SVPR",    // Task type
  "status": "DOING",    // Task type
  "ownerId": 12345,       // Task's publish user Id
  "price": "0.001",     // Upper price for the solution
  "solutions": [{       // detail information on the task
        "sId": 1222,          //Solution id
        "solverId": "111"     //solver's id
        "objectives": {},     //objectives of this solution, defined by particular contracts(see contracts document)
        "details": {},     //details of this solution, defined by particular contracts(see contracts document)
        "solutionTime": 1233444      //receive time for this solution
  }]
}
```


## Trading endpoints
### New task
```
POST /api/v1/task
```
Send in a new task.

**Parameters:**
```javascript
{
  "contract": "SVPR",    // Contract symbol
  "ownerId": 12345,       // Task's publish user Id
  "price": "0.001",     // Upper price for the solution
  "setting": {}           // detail information on the task, defined by particular contracts(see contracts document)
}
```



**Response FULL:**
```javascript
{
  "tid": "1233",      // task id
  "contract": "SVPR",    // Contract symbol
  "createTime": 123332,    // task create time
  "status" : "DOING",       // status for task
  "ownerId": 12345,       // Task's publish user Id
  "price": "0.001",     // Upper price for the solution
  "solutionCount": 0       //number of solutions
  "setting": {}           // detail information on the task
}
```

### Accept Solution
```
POST /api/v1/acceptSolution
```
Task's owner can accept a valid solution, exchange will return solution's detail
and deduct token from owner's balance.

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
tId | STRING | NO | Task Id
sId | STRING | NO | Solution Id

**Response:**
```javascript
{
  "tid": "1233",      // task id
  "contract": "SVPR",    // Task type
  "status": "DOING",    // Task type
  "ownerId": 12345,       // Task's publish user Id
  "price": "0.001",     // Upper price for the solution
  "solution": {       // detail information on the task
        "sId": 1222,          //Solution id
        "solverId": "111"     //solver's id
        "objectives": {},     //objectives of this solution, defined by particular contracts(see contracts document)
        "detail": {},          //detail of this solution
        "solutionTime": 1233444      //receive time for this solution
  }
}
```

### Close Task
```
DELETE /api/v1/task
```
Close an active task. All existing solution will be

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
tId | STRING | NO |

**Response:**
```javascript
{
  "tid": "1233",      // task id
  "contract": "SVPR",    // Contract symbol
  "createTime": 123332,    // task create time
  "status" : "CANCEL",       // status for task
  "ownerId": 12345,       // Task's publish user Id
  "price": "0.001",     // Upper price for the solution
  "setting": {}           // detail information on the task
}
```



**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
tId | STRING | NO |

**Response:**
```javascript
{
  "tid": "1233",      // task id
  "contract": "SVPR",    // Contract symbol
  "createTime": 123332,    // task create time
  "status" : "CANCEL",       // status for task
  "ownerId": 12345,       // Task's publish user Id
  "price": "0.001",     // Upper price for the solution
  "setting": {}           // detail information on the task
}
```

### Publish Solution
```
POST /api/v3/allOrders (HMAC SHA256)
```
User can publish solution for task.

**Parameters:**

```javascript
{
    "tid": "1233",      // task id
    "solverId": "111"     //solver's id
    "objectives": {},     //objectives of this solution, defined by particular contracts(see contracts document)
    "detail": {},          //detail of this solution, defined by particular contracts(see contracts document)
    "solutionTime": 1233444      //receive time for this solution
}
```


**Response:**
```javascript
{
    "tid": "1233",      // task id
    "sid": "1233",      // solution id
    "solverId": "111"     //solver's id
    "objectives": {},     //objectives of this solution, defined by particular contracts(see contracts document)
    "detail": {},          //detail of this solution, defined by particular contracts(see contracts document)
    "solutionTime": 1233444      //receive time for this solution
}
```


### Account information
```
GET /api/v1/account
```
Get current account information.

**Parameters:**

**Response:**
```javascript
{
  "balances": [
    {
      "asset": "NPC",
      "free": "4723846.89208129",
      "locked": "0.00000000"
    }
  ]
}
```


## Stream endpoints
Specifics on how user data streams work is in another document.

### Start user data stream
```
POST /api/v1/stream
```
Start a new  data stream. The stream will close after 60 minutes unless a keepalive is sent.


**Parameters:**
NONE

**Response:**
```javascript
{
  "listenKey": "pqia91ma19a5s61cv6a81va65sdf19v8a65a1a5s61cv6a81va65sdf19v8a65a1"
}
```

### Keepalive user data stream
```
PUT /api/v1/stream
```
Keepalive a user data stream to prevent a time out. User data streams will close after 60 minutes. It's recommended to send a ping about every 30 minutes.


**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
listenKey | STRING | YES

**Response:**
```javascript
{}
```

### Close user data stream (USER_STREAM)
```
DELETE /api/v1/stream
```
Close out a user data stream.

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
listenKey | STRING | YES

**Response:**
```javascript
{}
```


## Error codes for NPC rest API
Errors consist of three parts: an error code, a message and a detail
 message optionally. Codes are universal,
 but messages can vary. Here is the error JSON payload:
```javascript
{
  "code":1121,
  "msg":"Invalid contact.",
  "detail": {}
}
```

### 200 - Success Request
 * request is success, request id is given in details.requestId

### 10xx - General Server or Network issues
#### -1000 UNKNOWN
 * An unknown error occured while processing the request.

#### -1001 DISCONNECTED
 * Internal error; unable to process your request. Please try again.

#### -1002 UNAUTHORIZED
 * You are not authorized to execute this request.

#### -1003 TOO_MANY_REQUESTS
 * Too many requests.
 * Too many requests queued.

#### -1006 UNEXPECTED_RESP
 * An unexpected response was received from the message bus. Execution status unknown.

#### -1007 TIMEOUT
 * Timeout waiting for response from backend server. Send status unknown; execution status unknown.

#### -1013 INVALID_MESSAGE
 * INVALID_MESSAGE

#### -1016 SERVICE_SHUTTING_DOWN
 * This service is no longer available.

#### -1020 UNSUPPORTED_OPERATION
 * This operation is not supported.

#### -1021 INVALID_TIMESTAMP
 * Timestamp for this request is outside of the recvWindow.
 * Timestamp for this request was 1000ms ahead of the server's time.

#### -1022 INVALID_SIGNATURE
 * Signature for this request is not valid.


### 11xx - Request issues
#### -1100 ILLEGAL_CHARS
 * Illegal characters found in a parameter.
 * Illegal characters found in parameter '%s'; legal range is '%s'.

#### -1101 TOO_MANY_PARAMETERS
 * Too many parameters sent for this endpoint.
 * Too many parameters; expected '%s' and received '%s'.
 * Duplicate values for a parameter detected.

#### -1102 MANDATORY_PARAM_EMPTY_OR_MALFORMED
 * A mandatory parameter was not sent, was empty/null, or malformed.
 * Mandatory parameter '%s' was not sent, was empty/null, or malformed.
 * Param '%s' or '%s' must be sent, but both were empty/null!

#### -1103 UNKNOWN_PARAM
 * An unknown parameter was sent.

#### -1105 PARAM_EMPTY
 * A parameter was empty.
 * Parameter '%s' was was empty.

#### -1121 BAD_SYMBOL
 * Invalid contract symbol.

#### -1125 INVALID_LISTEN_KEY
 * This listenKey does not exist.

#### -1130 INVALID_PARAMETER
 * Invalid data sent for a parameter.
 * Data sent for paramter '%s' is not valid.


### 20xx - Processing Issues
#### -2008 BAD_API_ID
 * Invalid Api-Key ID

#### -2009 DUPLICATE_API_KEY_DESC
 * API-key desc already exists.

#### -2012 CANCEL_ALL_FAIL
 * Batch cancel failure.

#### -2013 NO_SUCH_ORDER
 * Order does not exist.

#### -2014 BAD_API_KEY_FMT
 * API-key format invalid.

#### -2015 REJECTED_MBX_KEY
 * Invalid API-key, IP, or permissions for action.

### 21xx - Algorithm User Side Issues

#### -2101 PRICE_INVALID
 * result's buy price is too high or too low

#### -2102 INVALID_CONTRACT_SETTINGS
 * Format or logical issues for contract settings

#### -2103 CONTRACT_SCALE_OVERFLOW
 * Contract settings scale is bigger than max threshold


### 22xx - Algorithm Provider Side Issues

#### -2201 PRICE_INVALID
  * result's sell price is too high or too low

#### -2202 INVALID_ANSWER
  * optimization result given by the provider is invalid. Detail format of the
   errors varies by different contact definition.
