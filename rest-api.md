# Trading and Data API for NPCloud Exchange
# General API Information
* The base endpoint is: **https://api.npcloud.io**
* All endpoints return either a JSON object or array.
* Data is returned in **descending** order in most case. Newest first, oldest last.
* All time and timestamp related fields are in milliseconds.
* HTTP `4XX` return codes are used for for malformed requests;
  the issue is on the sender's side.
* HTTP `5XX` return codes are used for internal errors; the issue is on
  NPCloud's side.
* Any endpoint can return an ERROR message; the error payload is as follows:
```javascript
{
  "code": -1121,
  "msg": "Invalid symbol."
  "data" : {}
}
```

* When endpoint process your request successfully, the payload is as follows:
```javascript
{
  "code": 200,
  "msg": ""
  "data" : {}
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
* API-key are passed into the Rest API via the `X-NPC-APIKEY`;
* API-keys and secret-keys **are case sensitive**.
* API-keys are required for trading rest API


## ENUM definitions

**Task status:**

* NEW
* DOING
* CLOSED
* REJECTED


## General endpoints
### Test connectivity
```
GET /api/v1/ping
```
Test connectivity to the Rest API.


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
Test connectivity to the Rest API and get the current server time (GMT time).


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
GET /api/v1/contracts
```
Current trading contracts


**Parameters:**
NONE

**Response:**
```javascript
{
  "contracts": ["VPR", "SVRP"]
}
```

## Data endpoints
### Query all live tasks

```
GET /api/v1/live_tasks
```
Get all living tasks by contract symbol


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
  "clientTaskId": "1233",      // task id specified by user
  "contract": "SVPR",    // contract name
  "status": "DOING",    // Task status
  "createTime": 123332,    // task's create time
  "expireTime": 122333,    // task's expire time
  "createUser": 12345,       // Task's owner's user Id
  "price": "0.001",     // Upper price for the solution
  "solutionCount": 1       //number of solutions
}]
```


### Query user's published tasks

```
GET /api/v1/published_tasks
```
Query current user's publish tasks

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
  "status": "NEW",    // Task type
  "createTime": 123332,    // task create time
  "expireTime": 122333,    // expire time of this task
  "createUser": 12345,       // Task's publish user Id
  "price": "0.001",     // Upper price for the solution
  "solutionCount": 1,       //number of solutions
}]
```

### Query user's solved tasks

```
GET /api/v1/solved_tasks
```
Query current user's  tasks

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
  "status": "NEW",    // Task type
  "clientTaskId": "1233",      // task id specified by user
  "createTime": 123332,    // task create time
  "expireTime": 122333,    // expire time of this task
  "createUser": 12345,       // Task's publish user Id
  "price": "0.001",     // Upper price for the solution
  "solutionCount": 1,       //number of solutions
}]
```

### Query task detail by task id

```
GET /api/v1/task_info
```

Query task by task Id, return task information and task's solution summary.

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
contract | STRING | YES | contract symbol
tid | STRING | YES | task id


**Caution:** setting limit=0 will return all living tasks.

**Response:**
```javascript
{
  "tid": "1233",      // task id
  "contract": "SVPR",    // Task type
  "status": "NEW",    // Task type
  "clientTaskId": "1233",      // task id specified by user
  "expireTime": 122333,    // task's expire time
  "createTime": 123332,    // task create time
  "createUser": 12345,       // Task's owner's user Id
  "price": "0.001",     // Upper price for the solution
  "setting": {} ,          // detail information on the task
  "solutions": [{       // solutions for this task
          "sid": 1222,          //Solution id
          "createUser": "111"     //solver's id
          "objectives": {},       // solution's objectives defined by particular contracts(see contracts document)
          "status": "SUBMITTED",       // is the solution is accepted by task publisher
          "createTime": 1233444      //receive time for this solution
  }]
}
```

### Query solution detail

```
GET /api/v1/solution_info
```

Query solution by solution id. Only solution's creator and solutions' acceptor can
see solution's detail

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
sid | STRING | YES | solution id


**Response:**
```javascript
{
        "tid": 12,             //task id
        "sid": 1222,          //Solution id
        "workerId": "111"     //solver's id
        "objectives": {},     //objectives of this solution, defined by particular contracts(see contracts document)
        "details": {},     //details of this solution, defined by particular contracts(see contracts document)
        "createTime": 1233444      //receive time for this solution
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
  "clientTaskId": "1233",      // task id specified by user
  "price": "0.001",     // Upper price for the solution
  "setting": {}           // detail information on the task, defined by particular contracts(see contracts document)
}
```



**Response:**
```javascript
{
  "tid": "1233",      // task id
  "contract": "SVPR",    // Contract symbol
  "clientTaskId": "1233",      // task id specified by user
  "createTime": 123332,    // task create time
  "status" : "NEW",       // status for task
  "createUser": 12345,       // Task's publish user Id
  "price": "0.001",     // Upper price for the solution
}
```


### Close Task
```
POST /api/v1/close_task
```
Try to close an active task. New solution will not be submitted to task.
Task is closed successfully when all pending solutions are processed.

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
tid | STRING | NO | task id

**Response:**
```javascript
{
  "tid": "1233",      // task id
  "clientTaskId": "1233",      // task id specified by user
  "contract": "SVPR",    // Contract symbol
  "createTime": 123332,    // task create time
  "createUser": 12345,       // Task's publish user Id
  "status" : "CLOSED",       // status for task
  "price": "0.001"     // Upper price for the solution
}
```


### Publish Solution
```
POST /api/v1/solution
```
User publish solution for task.

**Parameters:**

```javascript
{
    "tid": "1233",      // task id
    "price": "12.2"     //price set by the worker
    "solution": {},     // solution detail, defined by particular contracts(see contracts document)
    "%OTHER_SOLUTION_FIELDS%": {}, //detail of this solution, defined by particular contracts(see contracts document)
}
```


**Response:**
```javascript
{
    "tid": "1233",      // task id
    "sid": "1233",      // solution id
    "createUser": "111"     //solver's id
    "price": "12.2"     //price set by the worker
    "createTime": 1233444      //receive time for this solution
}
```

### Task owner accept Solution
```
POST /api/v1/accept_solution
```
Task's owner can accept a valid solution, exchange will return solution's detail
and deduct token from owner's balance.

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
sid | STRING | NO | Solution Id

**Response:**
```javascript
{
  "tid": "1233",      // task id
  "sid": 1222,          //Solution id
  "workerId": "111"     //solver's id
  "objectives": {},     //objectives of this solution, defined by particular contracts(see contracts document)
  "%OTHER_SOLUTION_FIELDS%": {},          //detail of this solution, defined by particular contracts(see contracts document)
  "createTime": 1233444      //receive time for this solution
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
GET /api/v1/stream
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
POST /api/v1/stream
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
  "code":-1121,
  "msg":"Invalid contact.",
  "detail": {}
}
```

### 200 - Success Request

### 10xx - General Server or Network issues
#### -1000 UNKNOWN
 * An unknown error occured while processing the request.

#### -1001 DISCONNECTED
 * Internal error; unable to process your request. Please try again.

#### -1002 UNAUTHORIZED
 * You are not authorized to execute this request.

#### -1003 TOO_MANY_REQUESTS
 * You have sent too many requests.

#### -1007 TIMEOUT
 * Timeout waiting for response from backend server. Send status unknown; execution status unknown.

#### -1013 INVALID_MESSAGE
 * INVALID_MESSAGE

#### -1016 SERVICE_SHUTTING_DOWN
 * This service is no longer available.

#### -1020 UNSUPPORTED_OPERATION
 * This operation is not supported.

#### -1022 INVALID_SIGNATURE
 * Signature for this request is not valid.

### 11xx - Request issues
#### -1100 ILLEGAL_CHARS
 * Illegal characters found in a parameter.

#### -1101 TOO_MANY_PARAMETERS
 * Too many parameters sent for this endpoint.
 * Duplicate values for a parameter detected.

#### -1102 MANDATORY_PARAM_EMPTY_OR_MALFORMED
 * A mandatory parameter was not sent, was empty/null, or malformed.
 * Mandatory parameter '%s' was not sent, was empty/null, or malformed.

#### -1103 UNKNOWN_PARAM
 * An unknown parameter was sent.

#### -1105 PARAM_EMPTY
 * A parameter was empty.

#### -1125 INVALID_LISTEN_KEY
 * This listenKey does not exist.


### 20xx - Processing Issues

#### -2001 NO_SUCH_CONTRACT
 * Contract does not exist.

#### -2002 CONTRACT_SUSPEND
* Contract is not enabled

#### -2101 BUY_PRICE_INVALID
 * result's buy price is too high or too low

#### -2102 INVALID_CONTRACT_SETTING
 * Format or logical issues for contract setting

#### -2103 CONTRACT_SCALE_OVERFLOW
 * Contract setting scale is bigger than max threshold

#### -2104 BALANCE_NOT_ENOUGH
 * User's balance is not enough to support creation of new task or accepting new solution

#### -2105 NO_SOLUTION_FOUND
 * Solution not found to support operation

#### -2201 SELL_PRICE_INVALID
  * result's sell price is too high or too low

#### -2202 INVALID_ANSWER
  * Optimization result given by the provider is invalid. Detail format of the
   errors varies by different contact definition.

#### -2203 NO_TASK_FOUND
 * Task not found to support operation

#### -2204 TASK_IS_CLOSED
 * Task is not live to support operation