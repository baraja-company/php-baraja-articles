How to set up an HTTPS / SSL certificate - complete guide
=========================================================

> id: '0346a091-2b2c-494f-b2fb-573796d4fb46'
> slug:
> 	cs: jak-nastavit-https-ssl-certifikat
> 	en: how-to-set-up-an-https-ssl-certificate---complete-guide
> 
> publicationDate: '2019-09-16 09:01:35'
> mainCategoryId: '02d93aa8-ed83-460a-ab97-4de21118f019'
> sourceContentHash: '67b2b3ccce4815204a4bcadce781fa59'

When deploying the `https` protocol on client sites, I often encountered various difficulties that stemmed from a lack of understanding of the issues and too much complexity of the concepts.

In this tutorial I describe in detail the steps to obtain and deploy a valid certificate on a web server.

**In each subheading, I always briefly summarize the step for advanced users, and at the bottom I discuss the details for beginners.**

> **Warning:** The entire process of deploying a certificate can take over an hour and is often intermittent (the site may be unavailable).

Input requirements
-----------------

The instructions assume that we have access to a Terminal web server running on Linux that uses Apache.

For Nginx the whole theory applies equally, just the certificate file linking is different.

## Connecting to the web server

We connect to the server via SSH.

- On Windows I recommend the program **Putty**,
- On Mac or Linux, just use the built-in Terminal.

On Mac or Linux, call the command:

```htaccess
ssh user@server
```

For example, I want to connect to the user `root` on the `baraja.cz` website:

```htaccess
ssh root@baraja.cz
```

Or to the user `jan` at a specific IP address:

```htaccess
ssh jan@127.0.0.1
```

After submitting the query, either the connection will be made directly or you will be asked for a password. Nothing is displayed when typing the password, so confirm the password with enter and wait for the connection to be authorized.

> **Warning:** If we do not have rights to an action, we need to assign them. Either switch directly to the `root` user with the `sudo su` command, or precede the command we want to execute under root with the word `root` at the beginning, for example `root rm <name>` under root will delete the file `<name>`. When using the `sudo` command, we may periodically be prompted for a password.

**Details:**

SSH access is set up by the particular hosting where you have leased a server.

- In case of VPS you will always get SSH access.
- In the case of hosting, you may not get SSH at all, and configuration is done differently (usually via the web interface, or contact support).

## Switching from HTTP to HTTPS

If you are converting an existing site from `http` to `https`, you need to guarantee that all traffic is redirected to the new `https` protocol.

In the case of Apache, this can easily be achieved by using a redirect in the `.htaccess` file:

```httaccess
<IfModule mod_rewrite.c>
	RewriteEngine On

	RewriteCond %{HTTPS} !on
	RewriteRule .? https://%{HTTP_HOST}%{REQUEST_URI} [R=301,L]
</IfModule>
```

This configuration will ensure that all requests to `http` are redirected with `HTTP code 301` to `https`. This configuration is the default for the Nette framework, but applies to all other cases as well.

**Details:**

The `.htaccess` file contains the specific web server configuration that affects each request. It is usually placed in the same directory as `index.php`, or other files that are accessible from the Internet.

Its setting is only valid for the Apache server and may be disabled or restricted for some hosts. For more detailed information, always contact the hosting where you host your site.

-----

Sometimes it happens that `.htaccess` doesn't want to cooperate well and the configuration is extremely difficult (for example for many domains, each behaving differently). In such a case, redirection to HTTPS can be handled directly in PHP in a pinch:

```php
if (empty($_SERVER['HTTPS']) || $_SERVER['HTTPS'] === 'off') {
	$location = 'https://' . $_SERVER['HTTP_HOST'] . $_SERVER['REQUEST_URI'];
	header('HTTP/1.1 301 Moved Permanently');
	header('Location: ' . preg_replace('/^(https:\/\/)(?:www\.)(.*)$/', '$1$2', $location));
	die;
}
```

Paste the script into `index.php`. However, I do not recommend this solution.

## Find files with virtual hosts

In the case of Apache, we need to find the Virtual hosts file.

They are usually located on the path `/etc/apache2/sites-available`.

List the contents of the directory with the `ls -al` command and find the file where the virtual is set up for our site.

VirtualHosts are usually located in files with the `.conf` extension. The default is often in `000-default.conf`.

**Details:**

- The directory is opened with the `cd` command, for example `cd /etc/apache2/sites-available`.
- The contents of the file are written to the Terminal using the `cat <name>` command, or edited using the `nano <name>` or `vim <name>` commands.
- The editor is usually accessed by using the `CTRL + X` keyboard shortcut or by pressing `ESC` twice.
- Files can be partially browsed in the GUI with Midnight commander (`mc` command), which in the case of Ubuntu is installed simply with `apt-get install mc` or `sudo apt-get install mc`.

## Set up a virtual host for port 443

Within the Virtual host, we need to prepare a new one for port `443` (a common problem can be the blocking of port 443 on the firewall).

The contents of the file might look like this, for example:

```htaccess
<IfModule mod_ssl.c>
     <VirtualHost *:443>
         ServerName baraja.cz

         ServerAdmin jan@barasek.com
         DocumentRoot /var/www/baraja.cz/www
         Header always set Strict-Transport-Security "max-age=63072000; includeSubdomains; preload"
         ErrorLog ${APACHE_LOG_DIR}/baraja.cz.ssl.error.log
         CustomLog ${APACHE_LOG_DIR}/baraja.cz.ssl.access.log combined
         SSLEngine on
         SSLCertificateFile /etc/ssl/baraja.cz/baraja.cz.crt
         SSLCertificateKeyFile /etc/ssl/baraja.cz/baraja.cz.key
         SSLCertificateChainFile /etc/ssl/baraja.cz/rapidssl.crt
         SSLProtocol all -SSLv3
         SSLCipherSuite ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA:ECDHE-RSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA:DHE-RSA-AES256-SHA256:DHE-RSA-AES256-SHA:ECDHE-ECDSA-DES-CBC3-SHA:ECDHE-RSA-DES-CBC3-SHA:EDH-RSA-DES-CBC3-SHA:AES128-GCM-SHA256:AES256-GCM-SHA384:AES128-SHA256:AES256-SHA256:AES128-SHA:AES256-SHA:DES-CBC3-SHA:!DSS
         SSLHonorCipherOrder on
         SSLCompression off
         <FilesMatch "\.(cgi|shtml|phtml|php)$">
                 SSLOptions +StdEnvVars
         </FilesMatch>
         <Directory /usr/lib/cgi-bin>
                 SSLOptions +StdEnvVars
         </Directory>
		 BrowserMatch "MSIE [2-6]" nokeepalive ssl-unclean-shutdown downgrade-1.0 force-response-1.0
         # MSIE 7 and newer should be able to use keepalive
         BrowserMatch "MSIE [17-9]"
         ssl-unclean-shutdown
     </VirtualHost>
</IfModule>
```

The most important area on the file is:

```htaccess
SSLEngine on
SSLCertificateFile /etc/ssl/baraja.cz/baraja.cz.crt
SSLCertificateKeyFile /etc/ssl/baraja.cz/baraja.cz.key
SSLCertificateChainFile /etc/ssl/baraja.cz/rapidssl.crt
```

Understanding this configuration is critical. If you need to google for more detailed information, use the words `SSLCertificateFile`, `SSLCertificateKeyFile` and `SSLCertificateChainFile` which are common to all Apache servers.

> **Warning:** The SSL issue is relatively old and different names are often used for the same thing to maintain backwards compatibility! It is therefore important to understand the principle.

**Details:**

It is important that VirualHost contains:

- `<IfModule mod_ssl.c>` says we want to use SSL
- `<VirtualHost *:443>` that all communication will run on port 443
- `SSLEngine on` that SSL is enabled for this VirtualHost
- The `SSLCertificateFile`, `SSLCertificateKeyFile` and `SSLCertificateChainFile` are files with specific keys.

Nothing else is needed.

## Understanding the principle of what we are going to do and how the certificate works

In the final configuration of VirtualHost, we will really only need 3 files `SSLCertificateFile`, `SSLCertificateKeyFile` and `SSLCertificateChainFile`, which we can place anywhere on the server (name and location don't matter). It is important to provide a working path and that the contents of the files are valid.

The specific method of obtaining the certificates may vary from CA to CA. The important thing is to understand the principle and then apply it to your case.

| File | Meaning |
|---------------------------|-------------------------------------|
| `SSLCertificateFile` | This certificate **is sent by the authority** |
| `SSLCertificateKeyFile` | **My generated** private key |
| `SSLCertificateChainFile` | I downloaded from the web type **intermediate + root** |

## Getting the private key and requesting the certificate

On the server, we first generate the private key. The word **private** is important, it means that nobody else knows it except the web server. Ideally, it should never leave the server and should be placed in a secure location. Losing this key means a loss of security, as an attacker will be able to impersonate a specific server.

To generate the key, use the command:

``shell
openssl req -new -newkey rsa:2048 -nodes -keyout yourdomain.key -out yourdomain.csr
```

To generate, we must have `openssl` installed on the server, which we can get by running `sudo apt install openssl`, for example.

There can be several kinds of keys, in this case we generate an RSA key 2048 bytes long (`rsa:2048`).

The output of the command is 2 files (which you name according to your domain):

- `yourdomain.key` - this is the private key. Save the path to this key in Apache VirtualHost to `SSLCertificateKeyFile`.
- `yourdomain.csr` - this is a `certificate request`, or a request to issue a certificate.

The contents of the request must always be submitted to the CA for approval. This is usually done via the web interface in the administration on the site where the certificates are sold. The approval of the request varies depending on the type of certificate. It is most often done automatically by a robot and takes from 5 minutes to 8 hours. In the case of expensive certificates, where the physical ownership of the site and the operating company is also verified, the verification is done manually and can take several days.

If you are in a hurry, there is a certification agency `Let's encrypt` that authorizes requests automatically with a validity of 3 months.

> **Warning:** In some cases, the CA offers automatic request generation. This is not a recommended practice because it knows the private key. If you do this, you must always obtain the private key from the agency and place it in a file on the server in the same way as if you were generating the request.

## Obtaining a key for a specific CA/security authority

In the meantime, while waiting for the request to be approved and the certificate to be issued, we will secure the **public key** of the CA. In Apache VirtualHost this is represented by the `SSLCertificateChainFile` file (in English this is called `Intermediate and Root CA`). This key is public in principle and tells the web browser who the CA is. This file is usually downloaded from the CA's website or delivered to us in an email.

You should always download the correct key according to the algorithm you choose. In the case of RapidSSL, for example, it can be downloaded here: https://knowledge.digicert.com/generalinformation/INFO1548.html

## Getting the certificate

Based on the request, the certificate authority has issued us a certificate. In the case of Apache VirtualHost, it is represented by the `SSLCertificateFile` file.

Importantly, the certificate has a limited validity and we need to repeat this process (ideally before expiration). You can easily find the expiration date in Chrome by clicking on the green padlock in the browser and viewing the certificate details, in case it is communicated by the CA.

After the expiration date, the certificate is invalid and the site will refuse connections and users will receive an error message about the security breach.

## Check and make changes

The correctness of the Apache settings can be partially verified with the `apache2ctl -S` command.

In order for the changes to take effect, Apache must be restarted, for example with the command:

``shell
sudo service apache2 restart
```

If no error message is thrown, we immediately go to verify functionality via a web browser (I recommend Google Chrome, which shows the most details).

In case of an error, we call the `apache2ctl -S` command, which can show the path to the logs, where we can see approximately what is wrong. It is important to do all the steps in this manual in the correct order and not to swap any of the keys.

## Future support

Don't forget to mark the expiration date on your calendar so you can renew your certificate in time. Some CAs send a notification email, but this is not always reliable.

You can do the renewal process and get a new certificate in parallel while the current one is running, and then just replace it at the same time.

For automated renewal, I recommend [Certbot](https://certbot.eff.org), which can automatically monitor validity and renew certificates. Renewal is done, for example, by a cron that calls a command once a month at night to issue a new certificate and deploys it immediately.

If you don't understand certificate management, it's a good idea to host with an established company that will supply you with functional hosting, including a certificate. This will save you a lot of hassle.
