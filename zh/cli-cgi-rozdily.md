CLI和CGI之间的区别
============

> id: '79b00e74-b59a-4ae6-8a59-8eecfe2a01ca'
> slug:
> 	cs: cli-cgi-rozdily
> 	zh: cli-hecgi-zhi-jian-de-qu-bie
> 
> perex:
> 	- Nejdůležitější rozdíly mezi CLI a CGI. Informace o prostředí.
> 	- CLI和CGI之间最重要的区别。关于环境的信息。
> 
> publicationDate: '2021-10-15 10:00:00'
> mainCategoryId: '4f1d7d70-c5b0-45f1-b1d2-d03c22aa4154'
> sourceContentHash: '0964358c913ca647cc947773de87ceda'

PHP可以在不同的环境中运行。最常见的环境是 `CGI`，它在 PHP 处理 HTTP 请求时运行。然而，也可以从终端运行PHP脚本，在这种情况下，它是一个所谓的CLI（命令行界面）任务。

CLI和CGI之间最重要的区别是
-------------------------------------

- 与`CGI SAPI`不同，`CLI`默认不向输出写入任何头文件。
- 有一些`php.ini`指令在`CLI SAPI`中被覆盖，因为它们在shell环境中没有意义。
   - `html_errors`: CLI默认为`FALSE`。
   - `implicit_flush`: CLI默认值为`TRUE`。
   - max_execution_time": CLI默认值为 "0"（无限）。
   - `register_argc_argv`: CLI默认值为`TRUE`。
- 该脚本可以接受命令行参数`$argc`变量给你传递给应用程序的参数数量。`$argv`字段给你一个实际参数的数组
- 为shell环境定义了3个新的常量：`STDIN`、`STDOUT`、`STDERR`。都是相应的壳设备的文件处理程序。例如，`STDIN`是`fopen('php://stdin', 'r')`的一个文件处理程序。所以你可以像这样从`STDIN`中读取一行：`$strLine = trim(fgets(STDIN));`。`STDIN`已经用`PHP CLI`为你定义好了。
- PHP CLI不会将当前目录改为正在执行的脚本的目录。脚本的当前目录将是运行PHP CLI命令的目录。
- PHP CLI有许多有用的选项。它允许你获得一些关于你的php设置的有价值的信息，你的php脚本或在不同模式下运行它。
- 在 PHP 5 中，CLI 和 CGI 的文件名有一些变化。在 PHP 5 中，CGI 版本被重新命名为 `php-cgi.exe`（以前是 `php.exe`），CLI 版本现在位于主目录中（以前是 `cli/php.exe`）。
- 在 PHP 5 中还引入了一种新的模式：`php-win.exe`。这等同于CLI版本，只是在`php-win`中没有打印任何东西，因此没有提供控制台（屏幕上没有显示 "dos box"）。这种行为与`PHP GTK`相似。
