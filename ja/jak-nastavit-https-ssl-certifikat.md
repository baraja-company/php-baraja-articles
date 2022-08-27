HTTPS/SSL証明書を設定する方法 - 完全ガイド
===========================

> id: '0346a091-2b2c-494f-b2fb-573796d4fb46'
> slug:
> 	cs: jak-nastavit-https-ssl-certifikat
> 	ja: https-ssl-zheng-ming-shuwo-she-dingsuru-fang-fa-wan-quangaido
> 
> publicationDate: '2019-09-16 09:01:35'
> mainCategoryId: '02d93aa8-ed83-460a-ab97-4de21118f019'
> sourceContentHash: '67b2b3ccce4815204a4bcadce781fa59'

https プロトコルをクライアントサイトに導入する際、問題点の理解不足や複雑すぎる概念に起因する様々な困難にしばしば遭遇しました。

このチュートリアルでは、有効な証明書を取得し、Webサーバーに配置する手順を詳しく説明します。

**各小見出しでは、必ず上級者向けのステップを簡単にまとめ、一番下に初心者向けの詳細を述べています**。

> **警告:** 証明書の展開の全プロセスは1時間以上かかることがあり、しばしば断続的になります（サイトが利用できなくなる可能性があります）。

入力条件
-----------------

この手順では、Apache を使用する Linux 上で動作するターミナル Web サーバーにアクセスできることを前提としています。

Nginxの場合は、証明書ファイルのリンクが異なるだけで、すべての理論が同じように適用されます。

## Webサーバーに接続する

SSHでサーバーに接続します。

- Windowsでは、**Putty**というプログラムをお勧めします。
- MacやLinuxでは、内蔵のターミナルを使うだけです。

MacやLinuxでは、コマンドを呼び出してください。

```htaccess
ssh uživatel@server
```

例えば、`baraja.cz`というサイトでユーザー`root`に接続したいとします。

```htaccess
ssh root@baraja.cz
```

または、ユーザー `jan` に特定の IP アドレスに。

```htaccess
ssh jan@127.0.0.1
```

問い合わせをすると、直接接続されるか、パスワードが要求されます。パスワードを入力しても何も表示されないので、Enterキーでパスワードを確認し、接続が認証されるのを待ちます。

> **警告:** アクションに権利がない場合、権利を割り当てる必要があります。sudo su` コマンドで直接 `root` ユーザに切り替えるか、root で実行したいコマンドの前に `root` という単語を付けてください。例えば root で `root rm <name>` を実行すると `<name>` というファイルが削除されます。sudo`コマンドを使用する際、定期的にパスワードの入力を求められることがあります。

**詳細:**

SSHアクセスは、あなたがサーバーを借りている特定のホスティングによって設定されます。

- VPSの場合、常にSSHアクセスが可能です。
- ホスティングの場合、SSHが全く使えない場合があり、設定は別の方法で行います（通常はWebインターフェースから、またはサポートに連絡します）。

## HTTP から HTTPS への移行について

既存のサイトを `http` から `https` に変換する場合、すべてのトラフィックが新しいプロトコルである `https` にリダイレクトされることを保証する必要があります。

Apacheの場合、`.htaccess`ファイルでリダイレクトを使用することで簡単に実現することができます。

```htaccess
<IfModule mod_rewrite.c>
	RewriteEngine On

	RewriteCond %{HTTPS} !on
	RewriteRule .? https://%{HTTP_HOST}%{REQUEST_URI} [R=301,L]
</IfModule>
```

この設定により、`http`へのすべてのリクエストは、`HTTP code 301`で`https`にリダイレクトされるようになります。この設定は、Netteフレームワークのデフォルトですが、他のすべてのケースにも適用されます。

**詳細:**

.htaccess`ファイルには、各リクエストに影響する特定のウェブサーバー設定が含まれています。通常は `index.php` など、インターネットからアクセスできるファイルと同じディレクトリに配置されます。

この設定はApacheサーバーでのみ有効であり、ホストによっては無効または制限される場合があります。より詳細な情報については、常にあなたのサイトをホストしているホスティングにお問い合わせください。

-----

時々、`.htaccess`がうまく協調しようとせず、設定が非常に困難になることがあります（例えば、多くのドメインで、それぞれが異なる動作をしている場合など）。そのような場合、HTTPSへのリダイレクトは、いざというときにPHPで直接処理することができます。

```php
if (empty($_SERVER['エイチティーティーピーエス']) || $_SERVER['エイチティーティーピーエス'] === 'オフ') {
	$location = 'https://' . $_SERVER['HTTP_HOST'] . $_SERVER['REQUEST_URI'];
	header('HTTP/1.1 301 Moved Permanently');
	header('場所' . preg_replace('/^(https:\//(?:www.)(.*)$/)', '$1$2', $location));
	die;
}
```

スクリプトを `index.php` に配置します。しかし、この解決策はお勧めしません。

## バーチャルホストのあるファイルを探す

Apacheの場合、Virtual hosts ファイルを見つける必要があります。

これらは通常、パス `/etc/apache2/sites-available` に配置されています。

ls -al` コマンドでディレクトリの中身をリストアップし、当サイトの仮想が設定されているファイルを探します。

VirtualHost は通常 `.conf` という拡張子を持つファイルに格納されています。デフォルトは `000-default.conf` にあることが多い。

**詳細:**

- ディレクトリは `cd` コマンドで開きます。例えば `cd /etc/apache2/sites-available` とします。
- ファイルの内容は、`cat <name>` コマンドでターミナルに書き込まれるか、`nano <name>` や `vim <name>` コマンドで編集されます。
- エディタにアクセスするには、通常 `CTRL + X` キーボードショートカットを使用するか、`ESC` を 2 回押してください。
- Midnight commander (`mc` コマンド) を使えば、GUI でファイルの一部を閲覧することができます。Ubuntu の場合は `apt-get install mc` または `sudo apt-get install mc` で簡単にインストールすることができます。

## ポート443のバーチャルホストを設定する

バーチャルホスト内で、ポート443`用に新しいものを用意する必要があります（よくある問題は、ファイアウォールでポート443がブロックされていることです）。

ファイルの中身は、例えばこんな感じ。

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
         SSLCertificateFile      /etc/ssl/baraja.cz/baraja.cz.crt
         SSLCertificateKeyFile   /etc/ssl/baraja.cz/baraja.cz.key
         SSLCertificateChainFile /etc/ssl/baraja.cz/rapidssl.crt
         SSLProtocol             all -SSLv3
         SSLCipherSuite          ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA:ECDHE-RSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA:DHE-RSA-AES256-SHA256:DHE-RSA-AES256-SHA:ECDHE-ECDSA-DES-CBC3-SHA:ECDHE-RSA-DES-CBC3-SHA:EDH-RSA-DES-CBC3-SHA:AES128-GCM-SHA256:AES256-GCM-SHA384:AES128-SHA256:AES256-SHA256:AES128-SHA:AES256-SHA:DES-CBC3-SHA:!DSS
         SSLHonorCipherOrder     on
         SSLCompression          off
         <FilesMatch "\.(cgi|shtml|phtml|php)$">
                 SSLOptions +StdEnvVars
         </FilesMatch>
         <Directory /usr/lib/cgi-bin>
                 SSLOptions +StdEnvVars
         </Directory>
		 BrowserMatch "MSIE [2-6]" nokeepalive ssl-unclean-shutdown downgrade-1.0 force-response-1.0
         # MSIE 7 and newer should be able to use keepalive
         BrowserMatch "MSIE [17-9]" \
         ssl-unclean-shutdown
     </VirtualHost>
</IfModule>
```

ファイル上は最も重要な部分です。

```htaccess
SSLEngine on
SSLCertificateFile      /etc/ssl/baraja.cz/baraja.cz.crt
SSLCertificateKeyFile   /etc/ssl/baraja.cz/baraja.cz.key
SSLCertificateChainFile /etc/ssl/baraja.cz/rapidssl.crt
```

この構成を理解することは、絶対に必要なことです。より詳細な情報を得るためにググる必要がある場合は、すべての Apache サーバーに共通する `SSLCertificateFile`、`SSLCertificateKeyFile`、`SSLCertificateChainFile` という単語を使ってみてください。

> **Warning:** SSL問題は比較的古く、後方互換性を維持するために同じものに対して異なる名前がしばしば使用されます!そのため、原理を理解することが重要です。

**詳細:**

VirualHostが含まれていることが重要です。

- `<IfModule mod_ssl.c>` は、SSL を使いたいと言っています。
- すべての通信が443番ポートで実行されるように `<VirtualHost *:443>` を設定します。
- この VirtualHost に対して SSL が有効であることを `SSLEngine on` で通知します。
- SSLCertificateFile`、`SSLCertificateKeyFile`、`SSLCertificateChainFile`は特定のキーを持つファイルである。

他に必要なものはありません。

## これからやろうとしていることの原理と証明書の仕組みの理解

VirtualHost の最終的な設定では、本当に 3 つのファイル `SSLCertificateFile`、`SSLCertificateKeyFile`、`SSLCertificateChainFile` が必要なだけで、これらはサーバーのどこにでも置くことができます（名前と場所は重要ではありません）。作業パスを指定することと、ファイルの内容が有効であることが重要です。

具体的な証明書の取得方法は、CA 毎に異なる場合があります。大切なのは、その原理を理解した上で、自分のケースに当てはめてみることです。

| ファイル｜意味
|---------------------------|-------------------------------------|
| `SSLCertificateFile`｜この証明書は**当局から送られたものです**｜。
| `SSLCertificateKeyFile` | **My generated** private key |.
| `SSLCertificateChainFile`｜ウェブタイプからダウンロードした **intermediate + root** |。

## 秘密鍵の取得と証明書の要求

サーバー上では、まず秘密鍵を生成する。プライベート**という言葉が重要で、これはウェブサーバー以外には誰も知らないという意味です。理想的には、サーバーを離れることなく、安全な場所に置かれるべきです。この鍵を失うと、攻撃者は特定のサーバーになりすますことができるようになるため、セキュリティが失われることになる。

鍵を生成するには、コマンドを使用します。

```shell
openssl req -new -newkey rsa:2048 -nodes -keyout yourdomain.key -out yourdomain.csr
```

これを生成するには、サーバーに `openssl` プログラムがインストールされている必要があります。例えば、`sudo apt install openssl` を実行して、これを取得できます。

鍵は何種類かありますが、今回は2048バイト長のRSA鍵（`rsa:2048`）を生成します。

コマンドの出力は、2つのファイル（ドメインに応じた名前を付ける）です。

- `yourdomain.key` - これは秘密鍵です。この鍵のパスをApacheのVirtualHostの `SSLCertificateKeyFile` に保存してください。
- `yourdomain.csr` - これは `certificate request` 、つまり証明書を発行するためのリクエストです。

要求の内容は、必ず CA に提出し、承認を受けなければならない。これは通常、証明書が販売されているサイトの管理部門のウェブインターフェースを介して行われます。要求の承認は、証明書の種類によって異なる。ロボットが自動で行うことがほとんどで、5分から8時間程度かかります。サイトや運営会社の物理的な所有権も確認する高価な証明書の場合、確認は手作業で行われ、数日かかることもある。

お急ぎの場合は、3ヶ月の有効期限で自動的にリクエストを承認する認証機関`Let's encrypt`があります。

> **注意:** 場合によっては、CAは自動リクエスト生成を提供します。これは秘密鍵を知っているため、推奨される方法ではない。この場合、常に代理店から秘密鍵を入手し、リクエストを生成するのと同じ方法で、サーバー上のファイルに配置する必要があります。

## 特定の CA/security authority の鍵を取得する。

その間、リクエストが承認され証明書が発行されるのを待つ間、CAの**公開鍵**を確保します。Apacheでは、これは `SSLCertificateChainFile` ファイルで表されます（英語では、これは `Intermediate and Root CA` と呼ばれます）。この鍵は原則公開で、WebブラウザにCAが誰であるかを知らせます。このファイルは、通常、CAのウェブサイトからダウンロードされるか、電子メールで配信されます。

選択したアルゴリズムに応じて、常に正しいキーをダウンロードする必要があります。例えばRapidSSLの場合、こちらからダウンロードできます：https://knowledge.digicert.com/generalinformation/INFO1548.html

## 証明書の取得

その依頼に基づき、認証局から証明書が発行されました。ApacheのVirtualHostの場合、`SSLCertificateFile`ファイルで表現されます。

重要なのは、この証明書には有効期限があるため、このプロセスを繰り返す必要があることです（理想的には有効期限が切れる前に）。有効期限は、CAから通知されている場合は、ブラウザの緑の南京錠をクリックし、証明書の詳細を表示すれば、Chromeで簡単に確認することができます。

有効期限を過ぎると、証明書は無効となり、サイトへの接続が拒否され、ユーザーにはセキュリティ侵害のエラーメッセージが表示されます。

## 確認と変更を行う

Apacheの設定が正しいかどうかは、`apache2ctl -S`コマンドで部分的に確認することができます。

変更を有効にするためには、例えばコマンドでApacheを再起動する必要があります。

```shell
sudo service apache2 restart
```

エラーメッセージが投げられない場合は、すぐにWebブラウザ（最も詳細が表示されるGoogle Chromeをお勧めします）で機能を確認しに行きます。

エラーが発生した場合は、`apache2ctl -S` コマンドを呼び出すと、ログへのパスが表示され、何が問題なのかをおおよそ確認することができます。このマニュアルのすべてのステップを正しい順序で行い、キーを入れ替えないことが重要です。

## 今後の対応

有効期限をカレンダーに記入し、期限内に証明書を更新することを忘れないでください。一部のCAは通知メールを送信しますが、これは必ずしも信頼できるものではありません。

現在の証明書が稼働している間に、更新作業と新しい証明書の取得を並行して行い、同時に入れ替えればいいのです。

自動更新には、証明書の有効性を自動的に監視して更新できる[Certbot](https://certbot.eff.org)がおすすめです。更新は、例えば月に一度、夜にコマンドを呼び出して新しい証明書を発行し、すぐにデプロイするcronで行う。

証明書管理を理解していない場合、証明書を含む機能的なホスティングを供給してくれる確立された会社でホスティングするのが良いでしょう。そうすれば、手間が省けます。
