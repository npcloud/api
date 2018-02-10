# Web Socket Streams for NPCloud Exchange (2017-12-01)
# General WSS information
* The base endpoint is: **wss://stream.npcloud.io:9443**
* Streams can be access either in a single raw stream or a combined stream
* Raw streams are accessed at **/ws/\<streamName\>**
* Combined streams are accessed at **/stream?streams=\<streamName1\>/\<streamName2\>/\<streamName3\>**
* Combined stream events are wrapped as follows: **{"stream":"\<streamName\>","data":\<rawPayload\>}**
* All symbols for streams are **lowercase**
* A single connection to **stream.npcloud.io** is only valid for 24 hours; expect to be disconnected at the 24 hour mark

# Detailed Stream information

## New Task Streams

**Stream Name:** newTask

**Payload:**
```javascript
{
  "eventType": "newTask",  // Event type
  "eventTime": 123456789,   // Event time
  "taskID": "1233",      // task id
  "taskType": "SVPR",    // Task type
  "pubId": 12345,       // Task's publish user Id
  "pubPrice": "0.001",     // Upper price for the solution
  "taskInfo": {}           // detail information on the task
}
```

## Task Solution Streams


**Stream Name:** taskSolution

**Payload:**
```javascript
{
  "eventType": "newTask",  // Event type
  "eventTime": 123456789,   // Event time
  "taskID": "1233",         // task id
  "taskType": "SVPR",        // Task type
  "pubId": 12345,           // Task's publish user Id
  "pubPrice": "0.001",      // Upper price for the solution
  "workerId": 1222,          //Solution's worker user id
  "solutionId": 123,            //Id for this solution
  "solutionPrice": "0.002",     //Price for the solution,
  "solutionSummary": {},         //summary information of this solution
}
```

## Task Status Stream


**Stream Name:** taskStatus

**Payload:**
```javascript
{
  "eventType": "newTask",  // Event type
  "eventTime": 123456789,   // Event time
  "taskID": "1233",         // task id
  "taskType": "SVPR",        // Task type
  "taskStatus: "DOING",      //tasks latest status
}
```
```