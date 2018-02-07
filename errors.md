# Error codes for NPC rest API
Errors consist of two parts: an error code and a message. Codes are universal,
 but messages can vary. Here is the error JSON payload:
```javascript
{
  "code":-1121,
  "msg":"Invalid contact."
}
```


## 10xx - General Server or Network issues
#### -1000 UNKNOWN
 * An unknown error occured while processing the request.

#### -1001 DISCONNECTED
 * Internal error; unable to process your request. Please try again.

#### -1002 UNAUTHORIZED
 * You are not authorized to execute this request.

#### -1003 TOO_MANY_REQUESTS
 * Too many requests.
 * Too many requests queued.
 * Too many requests; current limit is %s requests per minute. Please use the websocket for live updates to avoid polling the API.
 * Way too many requests; IP banned until %s. Please use the websocket for live updates to avoid bans.

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

## 11xx - Request issues
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

## 20xx - Processing Issues
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

## 21xx - Demand Side Issues

#### -2101 PRICE_INVALID
 * Bid price is too high or too low

#### -2102 INVALID_CONTRACT_SETTINGS
 * Format or logical issues for contract settings

#### -2103 CONTRACT_SCALE_OVERFLOW
 * Contract settings scale is bigger than max threshold


## 22xx - Supply Side Issues

#### -2201 PRICE_INVALID
  * ask price is too high or too low

#### -2202 INVALID_ANSWER
  *
