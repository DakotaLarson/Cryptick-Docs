# Errors

Cryptick uses the following error codes:

All responses except those with status code `503` should have `content-type`: `application/json`.

Error Code | Meaning
---------- | -------
400 | Bad Request -- Your request is invalid.
401 | Unauthorized -- Review the [authentication docs](#authentication).
429 | Too Many Requests -- Review the [rate limit docs](#rate-limits).
500 | Internal Server Error -- If the behavior is consistent, please [reach out](https://cryptick.net/contact).
503 | Service Unavailable -- Servers may be restarting. If the behavior continues, please [reach out](https://cryptick.net/contact).
