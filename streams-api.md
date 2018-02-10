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
  "tid": "1233",      // task id
  "contract": "SVPR",    // Contract symbol
  "createTime": 123332,    // task create time
  "ownerId": 12345,       // Task's publish user Id
  "price": "0.001",     // Upper price for the solution
  "solutionCount": 0,       //number of solutions
  "setting": {}           // detail information on the task
}
```

## Task Solution Streams


**Stream Name:** taskSolution

**Payload:**
```javascript
{
  "eventType": "newTask",  // Event type
  "eventTime": 123456789,   // Event time
  "tid": "1233",         // task id
  "createTime": 123332,    // task create time
  "task": {},           // detail information on task
  "sId": 1222,          //Solution id
  "solverId": "111"     //solver's id
  "objectives": {},     //objectives of this solution, defined by particular contracts(see contracts document)
  "solutionTime": 1233444      //receive time for this solution
}
```

## Task Status Stream


**Stream Name:** taskStatus

**Payload:**
```javascript
{
  "eventType": "newTask",  // Event type
  "eventTime": 123456789,   // Event time
  "tid": "1233",         // task id
  "createTime": 123332,    // task create time
  "contract": "SVPR",    // Contract symbol
  "solutionCount": 0,       //number of solutions
  "status: "DOING",      //tasks latest status
}
```
```