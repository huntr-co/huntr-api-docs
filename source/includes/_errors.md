# Errors

## Error response

> A sample error response:

```json
{
    "message": "Limit must be an integer under 500",
    "error": {
        "message": "Limit must be an integer under 500",
        "param": "limit",
        "value": "10000",
        "location": "query",
        "status": 400
    },
    "errors": [
        {
            "message": "Limit must be an integer under 500",
            "param": "limit",
            "value": "10000",
            "location": "query",
            "status": 400
        },
        {
            "message": "Invalid value",
            "param": "next",
            "value": "abcd",
            "location": "query",
            "status": 400
        }
    ]
}
```

Field | Type | Description
----- | ---- | -----------
`message` | String | Human-readable message providing more details about the first error found.
`error` | Object | First error, see [Error format](#error-format). We suggest you use this parameter to detect errors.
`errors` | Array | Array of all [Errors](#error-format) found, when there is more than one.

## Error format

Field | Type | Description
----- | ---- | -----------
`message` | String | Error message
`param` | String | If the error is parameter-specific, the parameter related to the error.
`value` | String | Value of the parameter that caused the error.
`location` | String | Location of the param that generated this error. It's either body, query, params, cookies or headers.
`status` | Number | See [Error Codes](#error-codes)

## Error codes

Error Code | Meaning
---------- | -------
`400` | Bad Request -- Your request is invalid.
`401` | Unauthorized -- Your API key is wrong.
`403` | Forbidden -- The resource requested is hidden for administrators only.
`404` | Not Found -- The specified resource could not be found.
`405` | Method Not Allowed -- You tried to access a resource with an invalid method.
`406` | Not Acceptable -- You requested a format that isn't json.
`410` | Gone -- The resource requested has been removed from our servers.
`429` | Too Many Requests -- You're requesting too many resources! Slow down!
`500` | Internal Server Error -- We had a problem with our server. Try again later.
`503` | Service Unavailable -- We're temporarily offline for maintenance. Please try again later.
