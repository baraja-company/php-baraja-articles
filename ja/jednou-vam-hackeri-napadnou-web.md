ある日突然、ハッカーがあなたのウェブサイトを攻撃する
==========================

> id: a11ca0a6-77e1-4ba4-8eca-83449c9cbf48
> slug:
> 	cs: jednou-vam-hackeri-napadnou-web
> 	ja: aru-ri-tu-ran-hakkagaanatanou-ebusaitowo-gong-jisuru
> 
> publicationDate: '2020-08-04 12:00:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: '0f0ac4c091115e9c1f0833d318f34f5d'

そんなことはないだろう？:DDDDD

300を超えるWebサイトを運営していると、ときどきさまざまな緊急事態に見舞われることがあります。かなり熱くなることもありますが、完全に平凡な内容であることが多いですね。私のように、過去にプログラミングの誘惑に負けたことがあり、また、【プログラミングは苦痛】（http://borisovo.cz/programming-sucks-cz.html）ということを知っている人なら、きっと私の意見に賛同してくれるはずです。

邪悪なハッカーに支配された
----------------------

ウェブアプリケーションが普及すると、ハッカーにとって魅力的なターゲットになります。彼らの動機は、通常、ウェブサイト全体をダウンさせることではなく、その反対です。実は、**ハッカーは、サーバー管理者であるあなたに全く気づかれないことを望んでいる**のです。優秀なハッカーは何ヶ月も待って、ウェブサーバーを監視し、最も価値のあるもの、つまりあなたのデータを手に入れます。

ユーザーを保護するためには、すべてのデータを暗号化し、何重にも保護することが不可欠です。パスワードには、[Bcrypt + salt](https://php.baraja.cz/hashovani), Argon2, PBKDF2, Scrypt などの低速関数を使用することができます。セキュリティ評価](https://pulse.michalspacek.cz/passwords/storages/rating#slow-hashes)については、すでにMichal Spacekさんが書かれていますが、その記事に補足していただき、感謝します。サイトオーナーとして、プログラマーと議論する際には、適切なパスワードハッシュ化を主張する必要があります。そうでなければ、自分には関係ないと錯覚していた以前の多くの人たちと同じように、泣きを見ることになりますよ。

攻撃されているのがわかるか？
---------------------------------

あるウェブサーバーのトラフィックがある閾値に達すると（チェコ共和国では、1日あたり約50人以上の訪問者）、どこからともなく一連の攻撃が向けられるようになることに気づいたのです。なぜなら、攻撃者はインフラのどの部分を攻撃するか選ぶことができ、ウェブサーバーの所有者であるあなたは、それをいち早く察知し、身を守り、次回の攻撃に備えなければならないのですから、簡単なことではありません。

例えば、先月は何回くらい攻撃があったのでしょうか？正確にご存知ですか？何人くらい擁護したんですか？あなたのサーバーにアクセスできる人はいますか？

今、誰かがあなたを攻撃しているとしたら？そして今...と、今度は......？

残念ながら、攻撃を認識するための万能な方法はありません。しかし、大きなアドバンテージを得ることができるツールがあるのです。

最近、一番効果があったこと。

- 攻撃者がサーバーの物理的な位置を知らない場合**、彼らはより困難な立場に立たされることになります。物理アーキテクチャの情報は、すべてのネットワーク通信をプロキシ（双方向に暗号化）する[Cloudflare](https://www.cloudflare.com/)を使えば、非常にうまく難読化することができます。また、海外からの取得の高速化、DDOS攻撃からの自動保護、資産の圧縮、そして最後に不正訪問者の自動ブロックなど、多くの利点があります。私はすべてのウェブサイトでCloudflareを使用しています（そして、ほぼすべてのプロジェクトで異なる技術を使用しています）。
- **エラーログ** です。いつでも、なんでも。あなたのアプリケーションには、プログラマーによって引き起こされた多くのバグが含まれている可能性があります。キャッチできない例外](https://php.baraja.cz/vyjimky)、アプリケーションエラー、SQLインジェクション、などなど。PHPでプログラミングしていてロギングを理解していない場合、[Tracy](https://tracy.nette.org/) (そしておそらく [Sentry](https://sentry.io/) などの他のツール) やサーバ自体のアクセスログで十分に捕捉できる問題があります。エラーログはあればいいというものではなく、必ず必要なものです。私が積極的に管理しているすべてのサイトのバグを記録し、新しいバグの情報を私のメールに送ってもらい、すぐに知ることができるようにしています。
- **セキュアかつアップデートされたプラットフォーム**。あなたのサイトでは、WordPressを導入していますか？正しい更新の仕方を知っていますか？あなたは、自分が直面するすべてのリスクを知っていますか？何かが無料であるとき、それはほとんどの場合、他の何かを犠牲にしています。個人的には、アプリケーション開発には[Nette framework](https://nette.org/cs/) version 3を使用しており、毎週更新し、セキュリティ上の脆弱性を積極的に探してパッチを当てています。車を定期的に整備するように、ウェブサイトも定期的に、少なくとも月に一度は整備する必要があります。サイトのメンテナンスには、すべての外部ライブラリの更新と、一貫したログの監視が含まれます。古いバージョンのライブラリを使用している場合、攻撃者がその脆弱性を知っていて、それを利用しようとする可能性が非常に高いです。
問題は、いつ
私の地域では、信じられないほど多くの人がセキュリティを軽視し、非常に簡単に破られ、悪用されるコンテンツをインターネットに晒しています。

実際、大企業でも[XSS](https://www.zive.cz/clanky/vyvojar-objevil-zranitelnost-v-seznamu-dokazal-mezi-vysledky-propasovat-zakazany-kod/sc-3-a-200023/default.aspx)(クロスサイトスクリプティング)攻撃のような些細なことでミスを犯すことがあるのです。

問題は、誰かがあなたのウェブサイトを破ることがあるかどうかではありません。問題は、それがいつ起こるか、そしてそれに対処する十分な準備があるかどうかです。もしかしたら、ハッカーが何ヶ月も前からあなたのサーバーにアクセスしていて、あなたはそれを（まだ）知らないかもしれません。

トラブル発生時の対応
-------------------------------

ハッカーの仕業だとわかったときには、たいてい手遅れで、ハッカーは必要なことはすべてやり終えているのです。彼はサーバー上にマルウェアを配置し、マシンを少なくとも部分的に制御している可能性が非常に高いです。

これは**カオスなタイプの問題であり、迅速に行動する必要があります**。

これは攻撃の最終段階なので、個人的にはWebサーバーをインターネットから完全に切り離し、何が起こったのかを徐々に解明していくことをお勧めします。それに、サーバーが何か別のことを隠しているのかどうか、正確にはわからないでしょう。攻撃側には常に大きなアドバンテージがあります。

もし、最後に動作したバックアップをサーバーに戻すことになれば、攻撃者を大いに助けることになる。彼はすでにあなたのサーバーに侵入する方法を知っており、数時間後に再びそれを行うことを止めるものは何もありません。また、バックアップにはマルウェアなどのウイルスが含まれている可能性があります。

これは私のクライアントの間では非常に不評なオプションですが、私自身はいつも最初に**WordPressサイト**や、クライアントが更新しないセキュリティ脅威の多いシステムを完全に削除することをお勧めしています。例えば、1台のサーバーで2年以上前のサイトを30個ホストしている場合、そのうちの少なくとも1個は何らかの脆弱性を含んでいることが事実上保証されています。

もし、あなたが問題を理解していないのであれば、あなたの問題に対して深い知識を持ち、広い意味で物事を理解している適切な専門家に早い段階でコンタクトを取った方が良いでしょう。

**倫理的な注意事項：***

セキュリティインシデントを起こしたクライアントのウェブサイトを管理している場合、そのクライアントに知らせる責任があります。もし何らかのデータ（パスワードなど、しかし多くの場合それ以上）が流出した場合、エンドユーザーにもパスワードが公になっていることを知らせ、どこでもパスワードを変更するようにしなければなりません。低速のハッシュ関数（例えば、**Bcrypt + salt**）を使用すると、攻撃者がパスワードを解読するのが著しく困難になります。（残念ながら、[Bcryptパスワードは解読できる](https://arstechnica.com/information-technology/2015/08/cracking-all-hacked-ashley-madison-passwords-could-take-a-lifetime/)。自分のアカウントが流出したかどうかについての情報を定期的に受け取りたい場合は、[Have i been pwned?](https://haveibeenpwned.com/) に登録することをお薦めします。）セキュリティ侵害】(https://m.uoou.cz/vismo/zobraz_dok.asp?id_ktg=5020&n=poruseni-zabezpeceni)の詳細については、OOOOのウェブサイトをご覧ください。

アトミックハビッツ
--------------

この半年で【アトミック・ハビッツ】(https://www.melvil.cz/kniha-atomove-navyky/)という本を読み終え、目から鱗が落ちる思いでした。この本には、毎日1％ずつ改善していくことで、長い目で見たときに大きな効果が得られるプロセスが書かれています。

私は、技術負債の様々な段階の企業や個人から相談を受けるので、最悪なことをまとめておきたいと思います。

- **サーバー通信にFTPは意味がない**。パスワードで制御される安全でないプロトコルです。誰かにアクセスさせたい場合、その人はパスワードを知っている必要があり、あなたと同じことができる。彼のアクセス権を奪いたいなら、パスワードを変更するしかないでしょう。完全にSSHに切り替え、RSAキーを使用するのがベターです。最近は企業でもこの問題を解決していることに驚かされることが結構あります。すべてのパスワードのテーブルを1つ作らない。そしてもちろん、チーム全体で共有することはありません。
- ソースコードはGit**に、データはデータベースに、静的コンテンツ（画像、PDF、...）はディスク上のファイルに所属しています。間違ったデータストレージを選択すると、アプリケーションの設計やセキュリティが悪化します。PDF（請求書など）へのURLは、受信者だけが知っているランダムに生成されたコンテンツを含む必要があります。極めて機密性の高いコンテンツ（銀行取引明細書など）については、ディスク上で暗号化し、ログイン後にのみ提供することが重要です。
- サイトの非公開部分は、Googleにインデックスされないように、METAヘッダーの `noindex` と `nofollow` を含む必要があります。競合他社に知られてはならない機密性の高いコンテンツは、パスワードで保護する必要があります。
- サーバーの[directory content listing](https://www.simplified.guide/apache/disable-directory-listing)を無効にしてください。不適切に設定された場合、サイト全体を機械でクロールし、機密ファイルを見つけることができます。
- プロジェクト・ルート!設定ファイルへの直接のURLは絶対にあってはならない。 例えば、最初のチュートリアルから[Nette does it right](https://doc.nette.org/cs/3.0/quickstart/getting-started#toc-obsah-web-projectu)のような右往左往する。公開されているgitリポジトリ](https://smitka.me/open-git/)も同様です。この種の脆弱性は極めて過小評価されやすく、その結果は悲劇的なものになりがちです。
- また、不用意な開発ミスからバグが発生することもあります。変更のたびに生身の人間によるコードレビューを行うことは非常に重要です。多くのバグは、人間がコードを見る前に[PHPStan](https://github.com/phpstan/phpstan)などのツールで発見されます。
- 決して[パスワードを読める形でメールで送る](https://www.lupa.cz/clanky/reset-a-poslani-hesla-v-citelne-podobe-e-mailem-nebezpecna-praktika/)のではなく、必ず別の解決方法があるはずです。
- 古いバージョンのライブラリやソフトウェアのセキュリティ問題。ロボットは毎秒インターネットを徘徊し、既知のセキュリティホール（SQLインジェクション、XSS、CSRF、...）を攻撃しています。既知のホールの多くは、古いバージョンのWordPressを使用しています。

この他にも多くの脆弱性があり、プロジェクトによって問題点は様々です。もし、簡単なサーバー監査が必要なら、[Maldet](https://www.rfxn.com/projects/linux-malware-detect/)を使い、その後、適切な専門家を雇って[フルサイト監査](https://baraja.cz/audit-webu)を行うことをお勧めします。セキュリティ上の理由だけでなく、そのような理由もあります。

セキュリティエンジニアは海の中のプラスチックのようなもので、永遠に続くだけです。慣れることです。

自然界の作用・反作用の原理
-------------------------------

また、自然界ではほとんどの場合、反作用の原理が使われていることにお気づきでしょう。つまり、特定の環境で何かが起きると、周囲の生物はそれに対してさまざまな反応を示すのです。この方法には多くの利点がありますが、1つだけ大きな欠点があります。

考える人（デザイナー、開発者、コンサルタント）としての大きな強み、それは行動力、つまり大きなリスクの一定部分を事前に把握し、未然に防ぐことを積極的に行うことです。すべての失敗を防ぐことはできないかもしれませんが、少なくとも結果を軽減することはできますし、問題を早期に発見して対応するための制御機構を構築することはできます。

ほとんどの攻撃は長期間にわたって行われるため、ログを取っておけば、通常は問題を発見するのに十分な時間があります。
