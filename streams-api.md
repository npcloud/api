# MQTT Streams for NPCloud Exchange
# General information
* The base endpoint is: **stream.npcloud.io:9443**
* Standard MQTT client can be used to connected to stream

# Detailed Stream information

## Task Streams

**Topic Name:** /task_update

**Payload:**
```javascript
{
  "eventType": "newTask",  // Event type
  "eventTime": 123456789,   // Event time
  "tid": "1233",      // task id
  "clientId": "1233",      // task id specified by use/r
  "contract": "SVPR",    // Contract symbol
  "createTime": 123332,    // task create time
  "createUser": 12345,       // Task's publish user Id
  "price": "0.001",     // Upper price for the solution
  "solutionCount": 0       //number of solutions
}
```

## Solution Streams

**Topic Name:** /solution_update

**Payload:**
```javascript
{
  "eventType": "newSolution",  // Event type
  "eventTime": 123456789,   // Event time
  "tid": "1233",         // task id
  "clientId": "1233",      // task id specified by user
  "sid": "1222",          //Solution id
  "contract": "SVPR",    // Contract symbol
  "createUser": "111"     //solver's id
  "price": "0.001",     //solution's price set by the worker
  "status": "SUBMITTED",     //status of the solution
  "objectives": {},
  "createTime": 1233444      //receive time for this solution
}
```