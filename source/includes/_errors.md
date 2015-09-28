# Errors

The Mapn API uses the following error codes:


Error Code | Meaning
---------- | -------
400 | Bad Request -- Your request sucks
401 | Unauthorized -- Your authentication token is wrong
403 | Forbidden -- You don't have permission
404 | Not Found -- The specified thing could not be found
405 | Method Not Allowed -- You tried to access a thing with an invalid method
406 | Not Acceptable -- You requested a format that isn't json
429 | Too Many Requests -- You're requesting too many things! Slow down!
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarially offline for maintanance. Please try again later.
