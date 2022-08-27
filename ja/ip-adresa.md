PHP でユーザーの IP アドレスを取得する
=======================

> id: '1d6d761e-c139-4624-8c32-b0f2c131a831'
> slug:
> 	cs: ip-adresa
> 	ja: php-deyuzano-ip-adoresuwo-qu-desuru
> 
> perex:
> 	- 'Zjištění IP adresy uživatele v PHP, ukládání IP adresy a ban uživatele. Ošetření VPN a uživatele za Proxy nebo NATem.'
> 	- PHPでユーザーのIPアドレスを取得し、そのIPアドレスを保存して、ユーザーをBANする。VPNとProxyやNATの背後にいるユーザーの調査。
> 
> publicationDate: '2020-02-28 10:30:21'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '54a77b5e4cc431b354903e42a27682a6'

PHPでは、基本的なレベルでIPアドレスを検出することは非常に簡単です。

```php
echo 'あのね、あなたのIPアドレスは' . $_SERVER['REMOTE_ADDR'] . '?';
```

> **Warning:** `$_SERVER['REMOTE_ADDR']` フィールドのキーとしてIPアドレスを取得することは、PHPがブラウザから呼び出された場合のみ可能です。CLIモード（例えば、ターミナルからcronで実行）では、IPアドレスは利用できません（ネットワーク要求が行われないので、これは理にかなっています）。

信頼性の高いIPアドレス発見
-----------------------------

何年もかけて開発した結果、最終的にこの実装にこだわりました。

```php
function getIp(): string
{
    if (isset($_SERVER['http_cf_connecting_ip'])) { // Cloudflare対応
        $ip = $_SERVER['http_cf_connecting_ip'];
    } elseif (isset($_SERVER['REMOTE_ADDR']) === true) {
        $ip = $_SERVER['REMOTE_ADDR'];
        if (preg_match('/^(?:127|10)\.0\.0\.[12]?\d{1,2}$/', $ip)) {
            if (isset($_SERVER['HTTP_X_REAL_IP'])) {
                $ip = $_SERVER['HTTP_X_REAL_IP'];
            } elseif (isset($_SERVER['http_x_forwarded_for'])) {
                $ip = $_SERVER['http_x_forwarded_for'];
            }
        }
    } else {
        $ip = '127.0.0.1';
    }
    if (in_array($ip, ['::1', '0.0.0.0', 'ローカルホスト'], true)) {
        $ip = '127.0.0.1';
    }
    $filter = filter_var($ip, FILTER_VALIDATE_IP, FILTER_FLAG_IPV4);
    if ($filter === false) {
        $ip = '127.0.0.1';
    }

    return $ip;
}
```

それなら、ずっといい。

```php
echo 'あのね、あなたのIPアドレスは' . getIp() . '?';
```

IPが直接検出できる場合、IPv6のみである場合、CLIモード（cronなど）である場合は、`127.0.0.1`（localhost）を返します。

X-Forwarded-For` や `X-Real-IP` ヘッダを考慮した実装は、PHPで直接行うには非常に危険です。データが簡単に変更され、攻撃者が偽のIPアドレスを偽装して、例えば、サイトの管理画面を見たりデバッグモードを有効にしたりできるからです (Nette Tracy).一方、プロキシされたリクエストも受け入れなければならない（例えば、Cloudflareを経由してプロキシされたトラフィックや、同じマシン上でApacheとNgnixを動作させている場合、それらが互いに直後にローカルで呼び出された場合など）。

それは、Apache (`RemoteIP` 拡張) と Nginx (`remote_ip` 拡張) で `X-Forwarded-For` が訪問者の実際の IP アドレスから設定され、その IP アドレスが HTTP ヘッダーで設定できないことを確認することです。

$_SERVER['REMOTE_ADDR']` フィールドは自動的に正しい IP アドレス (つまり、リクエストが PHP に直接来た IP アドレス) を取得するので、それを処理する必要はありません。

プロキシ経由のユーザーアクセス
----------------------------

ユーザーがプロキシ経由でアクセスすることはよくあることです。そして、実際のIPアドレスが変数 `$_SERVER['HTTP_X_FORWARDED_FOR']` に格納されます。

このケースは、例えば、サーバー上のルーティングが `Ngnix -> Apache -> PHP` という方法で解決され、`Ngnix` が `Apache` の前にリバースプロキシとして機能する場合に発生する可能性があります。この場合、PHP は内部ネットワーク内の IP アドレス (通常は `127.0.0.*` という形式) のみを認識します。

例えば、**Cloudflare**サービスがこのような動作をすることがあり、実際のユーザーのIPアドレスとプロキシのどちらを操作しているのかに注意する必要があります。私にとっては、この記事の冒頭で紹介した `getIp()` 関数を使うのが一番良い方法です。プロキシされたリクエストごとに自動的に送信される `$_SERVER['HTTP_CF_CONNECTING_IP']` キーの存在を確認することで、Cloudflareの検出を確実にすることができます。

IPアドレスの保存
------------------

どのようなIPアドレスが利用可能かによって異なります。

- IPv4のIPアドレスは4バイトで格納することができ、そのために `ip2long` 関数が使用される。
- しかし、IPv6のIPアドレスは16バイトを使わなければならず、変換機能はない。

データベースサーバーがIPアドレスのデータ型を直接サポートしていない場合、IPアドレスを `varchar(39)` として格納することをお勧めします。これは、両方のバージョンが文字列として収まり、人間が読めるようになります。

> IPアドレスを格納する場合、`gethostbyaddr`関数によって検出されたドメイン名も格納することが理にかなっているかどうかを検討します。リストアップして検索する場合、非常に時間がかかる上に、時間が経つと変わってしまうこともあるので、名前を調べることはできません。

訪問者のIPアドレスをブロックする
-----------------------------

理想的な解決策は、ブロックされたIPアドレスのリストを作成し、各リクエストでこのリストと現在のIPアドレスを比較することです。アドレスが一致した場合、リクエストは直ちに停止されます。

```php
$blackList = [
    'ファーストチップ',
    'ドルハチップ',
];

if (\in_array(getIp(), $blackList, true) === true) {
    echo '残念ながらあなたのIPアドレスはブロックされています :-(';
    die; // リクエストを終了する
}
```

この例では、上記の例と同様に `getIp()` 関数を実装することを想定しています。

より強力な解決策は、インデックスが配列内に出現しているかどうかをチェックすることです。

```php
$blackList = [
    'ファーストチップ' => true,
    'ドルハチップ' => true,
];

if (isset($blackList[getIp()]) === true) {
    echo '残念ながらあなたのIPアドレスはブロックされています :-(';
    die; // リクエストを終了する
}
```

サーバーのIPアドレスとサーバー名
---------------------------------

サーバのIPアドレスは通常 `$_SERVER['SERVER_ADDR']` フィールドに格納され、その名前は `gethostbyaddr($_SERVER['SERVER_ADDR'])` 構造体で取得することができる。

ただし、`Ngnix -> Apache -> PHP` というコンセプトで、`Ngnix` がリバースプロキシの役割をしている場合、サーバの本当のIPアドレスは表示されません。

この場合、サーバー名は `$_SERVER['SERVER_NAME']` フィールドで確認するか、 `php_uname('n')` 関数を使用して確認することができます。[uname関数公式ドキュメント](https://www.php.net/manual/en/function.php-uname.php)。

そして、このトリックを使ってサーバーの公開IPアドレスを見つけることができます: `gethostbyname(php_uname('n'))`.
