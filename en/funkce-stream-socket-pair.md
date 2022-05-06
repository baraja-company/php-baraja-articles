PHP function stream_socket_pair()
=================================

> id: '0d18c258-9192-462e-9b2e-46bb95e76867'
> slug:
> 	cs: funkce-stream-socket-pair
> 	en: php-function-stream-socket-pair
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: b807e051f8b34240968018b9ef55d36f

Availability in versions: `PHP 5.1.0`

Creates a pair of connected, indistinguishable socket streams


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$domain` | `int` | *not* | The protocol family to be used: STREAM_PF_INET, STREAM_PF_INET6 or STREAM_PF_UNIX |
| `$type` | `int` | *not* | The type of communication to be used: STREAM_SOCK_DGRAM, STREAM_SOCK_RAW, STREAM_SOCK_RDM, STREAM_SOCK_SEQPACKET or STREAM_SOCK_STREAM |
| `$protocol` | `int` | *not* | The protocol to be used: STREAM_IPPROTO_ICMP, STREAM_IPPROTO_IP, STREAM_IPPROTO_RAW, STREAM_IPPROTO_TCP or STREAM_IPPROTO_UDP |


Return values
----------------

`array`

|bool an array with the two socket resources on success, or
false on failure.

Other resources
------------

[Official stream-socket-pair documentation](https://www.php.net/manual/en/function.stream-socket-pair.php)
