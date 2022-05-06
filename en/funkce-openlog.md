PHP function openlog()
======================

> id: ba2edc19-0df8-4eef-b8df-b8a0bb8aa9f1
> slug:
> 	cs: funkce-openlog
> 	en: php-function-openlog
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: a64905c5176d01e664643504fcdaa75a

Available in `PHP 4`, `PHP 5`, `PHP 7`

Opens a connection to the system logger.

Parameters
---------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$ident` | `string` | *not* | Identifier (of type `string`) to be added to the message. |
| `$option` | `int` | *not* | The `$option` argument is used to indicate which logging options will be used when generating the log message. Details below. |
| `$facility` | `int` | *not* | The `$facility` argument is used to indicate what type of program logs the message. This allows you to specify (in your machine's syslog configuration) how messages coming from different devices will be handled. Details below.

`$option`
---------

The `$option` argument is used to indicate which logging options will be used when generating a log message.

Possible values:

| Constant | Description |
|-------------|-------|
| LOG_CONS | If an error occurs when writing to the logger (or sending data), it prints a message to the system console.
| LOG_NDELAY | Immediately opens a connection to the logger.
| LOG_ODELAY | (default) Delay between writing individual messages. It is mainly used when writing the first message to the logger.
| LOG_PERROR | Print the logged error also as a standard PHP error.
| LOG_PID | Include the `PID` number in the message.

You can select one or more options.

If you use multiple options, use the `OR` logical notation between constants.

For example: `LOG_CONS | LOG_NDELAY | LOG_PID`.

`$facility`
-----------

The `$facility` argument is used to specify what type of program is logging the message. This allows you to specify (in your machine's syslog configuration) how messages coming from different devices will be handled.

| Constant | Description |
|--------------|-------|
| LOG_AUTH | Security or authorization messages. If this constant is not defined on your system, use `LOG_AUTHPRIV`.
| LOG_AUTHPRIV | Security or authorization messages. **Private Message**.
| LOG_CRON | Hourly retries (cron).
| LOG_DAEMON | System deamon that waits for events.
| LOG_KERN | Messages from the operating system kernel.
| LOG_LOCAL0 ... LOG_LOCAL7 | Reserved for use on a specific local device or server. Not available on Windows.
| LOG_LPR | Subsystem for line dump.
| LOG_MAIL | Subsystem for handling e-mail and mail server.
| LOG_NEWS | Subsystem for handling news and messages (`USENET`).
| LOG_SYSLOG | Messages generated internally by syslogd (may contain system messages from the operating system).
| LOG_USER | Default messages defined by the user or a specific extension. They are not system messages.
| LOG_UUCP | Subsystem `UUCP`.

Return values
-----------------

`bool` - returns `true` on success, `false` on error.

Other resources
------------

[Official openlog function documentation]([Official PHP.net documentation](https://www.php.net/manual/en/function.openlog.php))
