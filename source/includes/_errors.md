# Errors

The Movem Partner API uses the following error codes:


Error Code | Meaning
---------- | -------
204 | No Content -- The requested application or referencce does not exist.
400 | Bad Request -- Your request is invalid.
401 | Unauthorized -- Your API key is wrong.
403 | Forbidden -- The application or reference requested is hidden.
404 | Not Found -- The specified application or reference could not be found.
405 | Method Not Allowed -- You tried to access an application or reference with an invalid method.
406 | Not Acceptable -- You requested a format that isn't json.
410 | Gone -- The application requested has been removed from our servers.
429 | Too Many Requests -- You're requesting too many application! Slow down!
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarily offline for maintenance. Please try again later.
