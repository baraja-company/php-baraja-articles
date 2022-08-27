PHP およびサーバーの設定情報 (phpinfo()、php.ini)
====================================

> id: '6f4a78c1-ca06-4619-9a28-a716a65a12a8'
> slug:
> 	cs: info
> 	ja: php-oyobisabano-she-ding-qing-bao-phpinfo-php.ini
> 
> perex:
> 	- 'PHP info, informace o nastavení a konfiguraci webového serveru. Změna konfigurace přes php.ini'
> 	- PHP info、Webサーバーのセットアップや設定に関する情報。php.iniによる設定変更
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '4f1d7d70-c5b0-45f1-b1d2-d03c22aa4154'
> sourceContentHash: '9ad0961571b298fb1f30ea3e67b2973e'

多くの場合、サーバーに関するできるだけ多くの情報を得る必要があります。ネイティブの `phpinfo()` 関数は、このような場合に最適です。

```php
phpinfo();

die;	// 設定を書き込んだら、スクリプトを終了する
```

これにより、インストールされているバージョン、拡張機能、ライブラリなどを簡単に確認することができます。

> 設定や変更方法については、本記事の最後をご覧ください。

特定のコンフィギュレーションセクションを検索する
-------------------------------------

特定の情報のみをリストアップするのが便利な場合もあるので、最初のパラメータを設定して、興味のあるものを正確に指定することができます。

```php
phpinfo(INFO_MODULES);
```

設定にはあらかじめ定義された定数が使用される。

| 定数名｜値｜説明
|-------------------|-----------|------
| INFO_GENERAL｜1｜一般的な設定、php.iniの場所、最終更新日、Webサーバー、システム情報、その他。
| INFO_CREDITS | 2 | PHPクレジット、詳しくは `phpcredits()` を参照してください。
| INFO_CONFIGURATION｜4｜現在の場所と設定のディレクティブ。より詳細な情報は `ini_get()` 関数で提供されます。
| INFO_MODULES｜8｜インストールされたモジュールに関する情報です。詳しくは `get_loaded_extensions()` 関数を参照してください。
| INFO_ENVIRONMENT｜16｜ `$_ENV` として利用可能な `Environment` 変数に関する情報です。
| INFO_VARIABLES｜32｜ EGPCS` (Environment, GET, POST, Cookie, Server) と呼ばれるスーパーグローバル変数設定の概要です。
| INFO_LICENSE｜64｜PHPの使用ライセンスやその他の使用条件に関する情報です。
| INFO_ALL｜-1｜すべての情報を表示する（初期値）

スーパーグローバル変数 `$_SERVER` を使用する。
---------------------------------

スクリプトの実行中に、サーバー設定に関するかなり多くの情報（ウェブマスターへのメール、訪問者の現在のIPアドレスや現在呼び出されているURLなど）を直接知ることができます。

既存の値をすべてリストアップするのは簡単です。

```php
foreach ($_SERVER as $key => $value) {
    echo $key . ':' . $value . '<br>。';
}
```

> **Warning:** すべてのインデックスが存在する必要はありません (例えば、スクリプトがCLIモードでcronを実行する場合、ページのURLやリクエストのIPアドレスを持つインデックスは存在しません)。

特定の設定ディレクティブを読み込む
-----------------------------------------

設定の多くは `php.ini` に格納されており、通常の方法では PHP から直接アクセスできません。例えば、最大アップロードファイルサイズなど。

設定を直接読み込むには、 `ini_get()` 関数を使用します (注意: この関数はすべてのサーバーで有効とは限りません。特にホストの場合に顕著です)。

例えば、アップロードできる最大ファイルサイズを調べたい場合、独自に実装を書かなければならない。

```php
/**
 * 著者 ヤン・バラシェック
 */
public static function getMaxUploadFileSize(): int
{
    $maxUpload = min(
        ini_get('投稿最大サイズ'),
        ini_get('アップロードの最大ファイルサイズ')
    );

    if (strncmp($maxUpload, 'M', 1) === 0) {
        return (int) str_replace('M', '', $maxUpload);
    }

    return (int) $maxUpload;
}
```

upload_max_filesize` と `post_max_size` の最大値を MB 単位で返します。

サーバーの構成と設定の変更
-------------------------------------

設定自体は `php.ini` ファイルに保存されます。その場所は `phpinfo()` 関数を使うか、`php --ini` コマンドを呼び出すことで簡単に見つけることができます。

```shell
> php --ini

Configuration File (php.ini) Path: /etc/php/7.1/cli
Loaded Configuration File:         /etc/php/7.1/cli/php.ini
Scan for additional .ini files in: /etc/php/7.1/cli/conf.d
Additional .ini files parsed:      /etc/php/7.1/cli/conf.d/10-mysqlnd.ini,
/etc/php/7.1/cli/conf.d/10-opcache.ini,
/etc/php/7.1/cli/conf.d/10-pdo.ini,
/etc/php/7.1/cli/conf.d/20-calendar.ini,
/etc/php/7.1/cli/conf.d/20-ctype.ini,
/etc/php/7.1/cli/conf.d/20-exif.ini,
/etc/php/7.1/cli/conf.d/20-fileinfo.ini,
/etc/php/7.1/cli/conf.d/20-ftp.ini,
/etc/php/7.1/cli/conf.d/20-gd.ini,
/etc/php/7.1/cli/conf.d/20-gettext.ini
```

あるいは、パスを巧みに解析することもできます（Linuxシステムで動作します）。

```shell
php -r "phpinfo();" | grep php.ini
```

彼は戻ってきますよ。

```shell
Configuration File (php.ini) Path => /etc/php/7.1/cli
Loaded Configuration File => /etc/php/7.1/cli/php.ini
```

> 例えば、`CLI`の設定はCLIモード、つまりターミナルからcronやコマンドを呼び出すときのみ有効です。

アップロードファイルのサイズ制限を設定する
----------------------------------------------

よく`php.ini`で直接設定されるプロパティの例として、最大アップロードファイルサイズ（デフォルトは`2MB`、これは2018年現在すでに低い）があります。

設定ファイル内では、例えば次のように記述します。

```shell
; Maximum allowed size for uploaded files.
upload_max_filesize = 40M

; Must be greater than or equal to upload_max_filesize
post_max_size = 40M
```

*中点はコメント、その後に特定の設定ディレクティブが続くことを意味します。
