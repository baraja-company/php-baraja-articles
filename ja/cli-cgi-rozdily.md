CLIとCGIの違い
==========

> id: '79b00e74-b59a-4ae6-8a59-8eecfe2a01ca'
> slug:
> 	cs: cli-cgi-rozdily
> 	ja: clitocgino-weii
> 
> perex:
> 	- Nejdůležitější rozdíly mezi CLI a CGI. Informace o prostředí.
> 	- CLIとCGIの最も重要な違い。環境に関する情報です。
> 
> publicationDate: '2021-10-15 10:00:00'
> mainCategoryId: '4f1d7d70-c5b0-45f1-b1d2-d03c22aa4154'
> sourceContentHash: '0964358c913ca647cc947773de87ceda'

PHPは様々な環境で動作させることができます。最も一般的な環境は `CGI` で、これは PHP が HTTP リクエストを処理する際に実行されます。しかし、ターミナルから PHP スクリプトを実行することも可能で、その場合はいわゆる CLI (Command-line interface) タスクとなります。

CLIとCGIの最も重要な違い
-------------------------------------

- CGI SAPI` とは異なり、`CLI` はデフォルトで出力にヘッダを書きません。
- シェル環境では無意味なため、`CLI SAPI`でオーバーライドされる `php.ini` ディレクティブがいくつか存在します。
   - `html_errors`: CLI のデフォルトは `FALSE` です。
   - implicit_flush`: CLI のデフォルト値は `TRUE` です。
   - max_execution_time`: CLI のデフォルト値は `0` (無制限)である。
   - `register_argc_argv`: CLI のデフォルト値は `TRUE` です。
- スクリプトはコマンドライン引数を取ることができますargc` 変数は、アプリケーションに渡された引数の数を表します。また、 `$argv` フィールドには、実際の引数の配列が入ります。
- シェル環境用の新しい定数として、 `STDIN`、`STDOUT`、`STDERR` の3つが定義されています。すべて対応するシェルデバイスのファイルハンドラです。例えば、`STDIN` は `fopen('php://stdin', 'r')` に対するファイルハンドラである。つまり、`$strLine = trim(fgets(STDIN));` のように `STDIN` から行を読み取ることができるのです。STDIN` は `PHP CLI` を使ってすでに定義されています。
- PHP CLI は、カレントディレクトリを実行中のスクリプトのディレクトリに変更することはありません。スクリプトのカレントディレクトリは、PHP CLIコマンドを実行したディレクトリになります。
- PHP CLI には、多くの有用なオプションが用意されています。phpの設定、phpスクリプト、異なるモードでの実行に関する貴重な情報を取得することができます。
- PHP 5 では、CLI および CGI のファイル名にいくつかの変更がありました。PHP 5 では、CGI 版は `php-cgi.exe` (旧 `php.exe`) に改名され、CLI 版はメインディレクトリに置かれるようになりました (旧 `cli/php.exe`).
- PHP 5 では、新しいモードとして `php-win.exe` が導入されました。これはCLIバージョンと同じですが、`php-win`では何も表示されないので、コンソールを提供しません (画面に "dos box" が表示されません)。この動作は `PHP GTK` に似ています。
