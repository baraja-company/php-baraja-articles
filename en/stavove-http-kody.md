HTTP status codes
=================

> id: '97800bb6-004c-4bdb-b126-f87e77cc5120'
> slug:
> 	cs: stavove-http-kody
> 	en: http-status-codes
> 
> publicationDate: '2021-04-10 18:59:21'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '6c1da4a7233c1eba531e9336daae5acb'

In HTTP communication, so-called `state codes' are transmitted, which is information about how the transfer was made. I'm sure you know that a `200` code means success and a `404` code means a non-existent page.

Status codes are divided into several groups according to their prefix.

1xx Informational
--------------

| Code | Meaning |
|-------|--------|
| `100` | Continue |
| `101` | Switch Protocol |

2xx Success
----------

| Code | Meaning |
|-------|--------|
| `200` | OK (all right) |
| `201` | Created |
| `202` | Accepted |
| `203` | Unauthorized information |
| `204` | No content |
| `205` | Restore content |
| `206` | Partial content |

3xx Redirection
----------------

| Code | Meaning |
|-------|--------|
| `300` | Multiple Choice |
| `301` | Permanent Redirect |
| `302` | Found |
| `303` | See more |
| `304` | No change |
| `305` | Use proxy |
| `306` | **Old but reserved for future use** |
| `307` | Temporary redirection |

4xx Client (user) error
-----------------------------

| Code | Meaning |
|-------|--------|
| `400` | Bad request |
| `401` | Unauthorized connection |
| `402` | Payment Requested |
| `403` | Disabled |
| `404` | Not Found |
| `405` | Method not allowed |
| `406` | Unacceptable |
| `407` | Proxy authentication required |
| `408` | Request timed out |
| `409` | Network conflict |
| `410` | Data gone |
| `411` | Requested length does not match |
| `412` | Assumption failed |
| `413` | Entity request is too large |
| `414` | Request-URI is too long |
| `415` | Unsupported media type |
| `416` | Requested scope not satisfactory |
| `417` | Expectation failed |

5xx Server error
--------------

| Code | Meaning |
|-------|--------|
| `500` | Internal server error |
| `501` | Not implemented |
| `502` | Bad Gateway |
| `503` | Service Unavailable |
| `504` | Gateway timeout expired |
| `505` | HTTP version not supported |
| `509` | Bandwidth Limit Exceeded |
