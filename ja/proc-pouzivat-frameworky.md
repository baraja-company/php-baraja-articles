フレームワークやライブラリを使う理由と方法
=====================

> id: '5ea6cdde-f67c-4f3f-8871-37b27ee8e23a'
> slug:
> 	cs: proc-pouzivat-frameworky
> 	ja: furemuwakuyaraiburariwo-shiu-li-youto-fang-fa
> 
> publicationDate: '2020-02-16 22:13:46'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '0926b7ec4f59904c008388431ff22eb5'

プログラマがフレームワークを使い始めるのは、自分でフレームワークを書いてみて、それがどこにも通用しないことが分かってからだ、という有名なジョークがあります。面白いのは、それが事実だということです。私自身、経験済みです。2回目も。

しかし、<a href="https://nette.org">ネッテ</a>のメインページにはこう書かれている。

> 本当のプログラマはフレームワークを使わない**。彼らは、コマンドラインから直接サーバーにウェブアプリケーションを書き込むのです。これは、彼らへのオマージュです。それ以外の人は、ネッテを使うことで仕事がぐっと楽になり、楽しくなるはずです。

アプリケーション開発におけるフレームワークの役割
-----------------------------------

自分でもわかっているはずです。

素晴らしいプロジェクトのアイデアが浮かんだので、純粋なPHPで直接グリーンフィールドコードを書き始めると、数時間後には作業の多くが反復的で、基本的なシステムの問題を解決していることに気がつくでしょう。データベースへの接続方法、フォームのレンダリング方法、データの検証方法、電子メールの送信方法など、さまざまな方法があります。

これらの作業を行うために、自分で関数を書いて呼び出すこともあるでしょう。同時に複数のプロジェクトを書いている場合、これらの関数を1つの大きな雑多なファイルでプロジェクト間で共有するようになり、必要に応じて継続的に微調整を行うことになります。

そして、その機能の上に毎回新しいプロジェクトを立ち上げて、そのまま開発を始めるような一定の段階になると、自分でフレームワーク、少なくともライブラリを書いたと言えるようになります。

フレームワークとは何か、何をするのか
-------------------------

すでに生活の中の実例で示したように、フレームワークはプログラマの作業を軽減する多くの機能（ただし理想的にはクラスとオブジェクト）の集合体です。開発の際、データベースに接続する方法を考える必要はなく、すでにどこかにプログラムされていることを知り、「ただ動くだけ」だという。

完成したフレームワークは、このように、一つのソリューションと一つのエコシステムをデバッグして新しいプロジェクトに取り組むために、長い時間をかけて何千ものアプリケーションを開発してきた何百人もの人々の知恵を集めたものなのです。

フレームワークを使うと、考えなくても問題を解決できる方法である「ベストプラクティス**」も手に入れることができます。もし問題にぶつかったら、誰かが先に解決しているはずですし、自分で複雑に解決するよりも、ドキュメントで解決策を見つけた方がいいに決まっています。

抽象プログラミングとカプセル化原則
---------------------------------------------

Netteのようなよくできたフレームワークでは、**非常に高い抽象度**でプログラミングを行うことができます。

こうすれば、プログラマー（フレームワークのユーザー）は、内部で何が起こっているのか、コンポーネントが内部でどのように動いているのかを正確に理解する必要がなくなり、時間と労力を大幅に節約することができるのです。彼は、アプリケーションの真の問題解決に集中し、非常に迅速に結果を得ることができます。

David Grudl自身（Netteフレームワークの作者）が、「パブから遅く帰ってきて、朝にはWebサイトを立ち上げなければならないクライアントのために、一晩でプログラミングできるようにNetteを設計した」と語ったことがある。これは、開発者が技術的なものから完全に離れてしまっていることが主な原因です。

これが機能し、開発者がただ完成したフレームワークを全体として使うためには、<a href="/encapsulation">カプセル化の原則</a>を正しく使用することが非常に重要です。
