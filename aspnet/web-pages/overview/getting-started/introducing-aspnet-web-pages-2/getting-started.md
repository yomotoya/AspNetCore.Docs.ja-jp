---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
title: "ASP.NET Web ページ - 作業の開始の概要 |Microsoft ドキュメント"
author: tfitzmac
description: "WebMatrix は推奨されなくなりました統合開発環境として ASP.NET Web Pages のです。 Visual Studio または Visual Studio のコードを使用します。 このガイドにしています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: a36d3bdf-ef1b-47a4-b932-3a0cf4cad716
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
msc.type: authoredcontent
ms.openlocfilehash: a6789ee75b4ca6e9443681cc7ec0bd3ab94cedcd
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="introducing-aspnet-web-pages---getting-started"></a>ASP.NET Web ページ - 作業の開始の概要
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> > [!NOTE] 
> > 
> > WebMatrix は推奨されなくなりました統合開発環境として ASP.NET Web Pages のです。 使用して[Visual Studio](../program-asp-net-web-pages-in-visual-studio.md)または[Visual Studio Code](https://code.visualstudio.com/)です。
> 
> 
> このガイダンスとアプリケーションには、ASP.NET Web Pages (バージョン 2 またはそれ以降) の概要と Razor 構文を動的な web サイトを作成するための軽量なフレームワーク。 また、WebMatrix、ページとサイトを作成するためのツールを紹介します。
> 
> **レベル**: ASP.NET Web ページに新しいです。  
> **スキルと見なされます**: HTML、基本的なカスケード スタイル シート (CSS)。
> 
> 学習する内容のセットの最初のチュートリアルで。
> 
> - どのような ASP.NET Web Pages テクノロジし、何ができます。
> - WebMatrix です。
> - すべてをインストールする方法。
> - WebMatrix を使用して、web サイトを作成する方法。
>   
> 
> 説明されている機能/テクノロジ:
> 
> - Microsoft Web Platform Installer です。
> - WebMatrix です。
> - *.cshtml*ページ
>   
> 
> 最初、Mike 教皇は、このチュートリアルを書き込みました。 Tom FitzMacken Microsoft WebMatrix 3 を更新します。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されているソフトウェアのバージョン
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 3


## <a name="what-should-you-know"></a>必要な知識

慣れているものと仮定します。

- **HTML**. 深い専門知識は必要ありません。 HTML、については説明しませんが、私たちもないものを使用して複雑です。 便利思います HTML チュートリアルへのリンクを提供します。
- **カスケード スタイル シート (CSS)**です。 同じ html です。
- **基本的なデータベース アイデア**です。 データのスプレッドシートを使用し、並べ替えを専門知識のレベルにあるデータをフィルター処理した場合、このチュートリアルのセットの仮定一般にします。

基本的なプログラミングの学習に該当するものもと仮定します。 ASP.NET Web ページは、c# と呼ばれるプログラミング言語を使用します。 任意の背景に関係するだけのプログラミングではありません。 前に web ページで、JavaScript を作成したこと場合、は、バック グラウンドの十分なを取得したらです。

プログラミングに慣れている場合、このチュートリアルを最初に設定動かさ緩やかに変化速度は新しいプログラマがもたらさありますに注意してください。 過去の最初のいくつかのチュートリアルにつれて、ただしを説明する小さい基本的なプログラミングおよびする処理が高速のクリップにに沿って移動されます。

## <a name="what-do-you-need"></a>何が必要ですか。

必要なもの:

- Windows 8、Windows 7、Windows Server 2008、または Windows Server 2012 を実行しているコンピューター。
- インターネット接続です。
- 管理者特権が (インストール プロセスに必要)。

## <a name="what-is-aspnet-web-pages"></a>ASP.NET Web ページとは何ですか。

ASP.NET Web ページは、動的な web ページの作成に使用できるフレームワークです。 単純な HTML web ページは静的です。そのコンテンツは、ページ内にある固定の HTML マークアップで決定されます。 ASP.NET Web Pages を作成するものと同様の動的なページを使用して、コードを使用して、実行時にページのコンテンツを作成できます。

動的なページを使用して、さまざまな機能を実行できます。 フォームを使用してユーザー入力の入力を要求し、ページの表示または外観に変更できます。 ユーザーから情報を取得し、データベースでは、保存し、後で一覧表示できます。 サイトから電子メールを送信できます。 Web (マッピング サービスなど) の他のサービスと対話し、それらのソースからの情報を統合するページを生成できます。

## <a name="what-is-webmatrix"></a>WebMatrix は何ですか。

WebMatrix は、web ページ エディター、データベース ユーティリティ、ページ、および web サイトをインターネットに公開するための機能をテストするための web サーバーを統合するツールです。 WebMatrix は無料であり、簡単にインストールし、使いやすい。 (でも単の HTML ページのほか、PHP などの他のテクノロジです。)

実際にはないかどうか*が*WebMatrix を使用した ASP.NET Web Pages を操作します。 エディター、テキストを使用してページを作成するなどしてへのアクセスのある web サーバーを使用してページをテストします。 ただし、WebMatrix、非常に簡単ではすべて、これらのチュートリアルは、全体にわたって WebMatrix を使用するようにします。

## <a name="about-these-tutorials"></a>これらのチュートリアルについて

このチュートリアルのセットは、ASP.NET Web Pages を使用する方法の概要です。 この入門チュートリアル セットの合計は 9 のチュートリアルです。 ASP.NET Web Pages の初心者から real、プロフェッショナルなデザインの web サイトを作成するのには一連のチュートリアルのセットの一部にできます。

この最初のチュートリアルでは、ASP.NET Web Pages を操作する方法の基礎を示すことで主要を設定します。 完了したら、この 1 つを終了し、Web ページをさらに詳しく調査する場所を取得する追加のチュートリアルのセットを操作できます。

意図的に進む簡単に詳細な説明。 何かを説明するたびにこのチュートリアルのセットのお常に選択方法と考えることと簡単に理解します。 以降チュートリアルのセットより詳細に移動しより効率的なまたはより柔軟なアプローチ (もずっと楽しく) を表示します。 これらのチュートリアルでは、まず、基本を理解する必要があります。

開始したチュートリアルのセットでは、ASP.NET Web Pages のこれらの機能について説明します。

- 概要と、インストールされているすべてのものを取得します。 (内にある、チュートリアルをご覧になっている。)
- ASP.NET Web Pages のプログラミングの基礎です。
- データベースを作成します。
- 作成して、ユーザー入力フォームを処理します。
- 追加、更新、およびデータベース内のデータを削除します。

## <a name="what-will-you-create"></a>新機能を作成しますか。

このチュートリアルが設定され、以降の中心と web サイトと同様に、ムービーを一覧表示できます。 動画の入力、編集、およびそれらを一覧表示することができます。 このチュートリアルのセットで作成したページがいくつかのとおりです。 1 つ目は、作成する ページの一覧を表示するムービーを示しています。

![ムービーの一覧を示すはムービー アプリ](getting-started/_static/image1.png)

新しいムービーの情報をサイトに追加することができます、ページを次に示します。

![ムービーの追加 ページを示す完成したムービー アプリ](getting-started/_static/image2.png)

このチュートリアルの後続のセットのビルドでは、設定し、電子メールを送信して、ソーシャル メディアとの統合の画像のアップロード、ログインしてもらうように、多くの機能を追加します。

## <a name="see-this-app-running-on-azure"></a>Azure で実行されているこのアプリを参照してください。

ライブ web アプリとして実行している完成したサイトを参照してもよろしいですか。 Azure アカウントに、アプリの完全なバージョンを展開するには、次のボタンをクリックするだけです。

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebPagesMovies)

Azure アカウントを Azure にこのソリューションを展開する必要があります。 アカウントがない場合は、次のオプションがあります。

- [無料の Azure アカウントを開設](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604)-クレジットを取得するでも使用されているアカウントを維持する最大と使用する無料の Azure サービスおよび有料の Azure サービスを試す使用できます。
- [MSDN サブスクライバー特典をアクティブ化](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604)-お客様の MSDN サブスクリプションでは、クレジット有料の Azure サービスを使用できるすべての月です。

## <a name="installing-everything"></a>すべてのインストール

Microsoft Web Platform Installer を使用して、すべてのものをインストールできます。 実際には、インストーラーをインストールし、他のすべてのインストールに使用することです。

少なくともを持っていることがある Web ページを使用して、Windows XP SP3 をインストールまたは Windows Server 2008 またはそれ以降。

[Web Pages ページ](../../../index.md)ASP.NET web サイトのをクリックして**インストール**です。

![ASP.NET Web サイトを示す&quot;WebMatrix のインストール&quot;ボタン](getting-started/_static/image3.png)

ライセンス条項および WebMatrix をインストールする前にプライバシーに関する声明に同意する必要があります。

![インストールを開始する用語を受け入れる](getting-started/_static/image4.png)

をクリックして**実行**インストールを開始します。 (インストーラーを保存する場合は、クリックして**保存**し、保存したフォルダーからインストーラーを実行します)。

![](getting-started/_static/image5.png)

WebMatrix をインストールする準備ができて、Web Platform Installer が表示されます。 **[インストール]**をクリックします。

![](getting-started/_static/image6.png)

インストール プロセスでは、コンピューターにインストールする必要がある出し、プロセスを開始します。 インストールする必要のあるものだけによって処理にかかる任意の場所、数分間から数分です。 選択**同意**ライセンス条項に同意します。

## <a name="hello-webmatrix"></a>こんにちは, WebMatrix

実行されると、インストール処理は WebMatrix を自動的に起動できます。 インストールされていない場合、Windows から、**開始**] メニューの [起動**Microsoft WebMatrix**です。

WebMatrix を初めて起動すると、Microsoft アカウントで Microsoft Azure にサインインする機会が与えられます。 サインインすると、Azure 経由で 10 個の無料の web アプリが表示されます。 これらの free web アプリは、アプリをテストする便利な手段を提供します。 場合は、Azure アカウントを既に必要はありませんが、MSDN サブスクリプションが、 [MSDN サブスクリプション特典をアクティブ化](https://www.windowsazure.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604)です。 それ以外の場合、ほんの数分で無料の試用アカウントを作成できます。 詳細については、「 [Azure 無料評価版](https://azure.microsoft.com/free/?WT.mc_id=A443DD604)です。

このチュートリアルを続行する今すぐにサインインする必要はありません。 署名しないようになりました場合、は、後でサインインするオプションが続行されます。 最後の[トピック](publishing.md)シリーズをこのチュートリアルでは、web サイトを Azure に配置する方法をについて説明以外のトピックを完了にサインインする必要がありますはそのため、します。

この時点では、いずれかはサインイン、Microsoft アカウントまたは選択して**Not Now**右下隅にします。

![サインイン](getting-started/_static/image7.png)

を開始するにを空白の web サイトを作成し、ページを追加します。 このセットの後のチュートリアルでは、組み込みの web サイト テンプレートのいずれかで再生されます。

[スタート] ウィンドウ**新規**です。

![WebMatrix の起動画面](getting-started/_static/image8.png)

テンプレートは、構築済みのファイルおよびさまざまな種類の web サイトのページです。 すべての既定で使用可能なテンプレートを表示するには、テンプレート ギャラリー オプションを選択します。

![テンプレート ギャラリーの選択](getting-started/_static/image9.png)

**クイック スタート**ウィンドウで、**空サイト**から、 **ASP.NET**をグループ化し、新しいサイト"WebPagesMovies"という名前です。

![空のサイト テンプレートが選択されていると WebMatrix のクイック スタート ウィンドウ](getting-started/_static/image10.png)

**[次へ]**をクリックします。

Microsoft アカウントにサインインしている場合は、Azure でサイトを作成することが表示されます。 既定の名前、サイトの名前に基づく**WebPagesMovies.azurewebsites.net**をお勧めします。 ただし、感嘆符はこの名前が Windows Azure で使用できないことを示します。 わかりやすくするため、次のように選択します。 **Skip**を Azure に web サイトを作成すると、今すぐをバイパスします。 この系列に後で Azure にサイトが公開します。

![azure サイトを作成します。](getting-started/_static/image11.png)

WebMatrix は、作成し、サイトが開きます。

![WebMatrix で新しい WebPagesMovies サイトを開く](getting-started/_static/image12.png)

上部は、クイック アクセス ツールバーとリボンがあります。 左下に表示ワークスペース セレクター タスク間切り替えた (**サイト**、**ファイル**、**データベース**、**レポート**)。 右側は、コンテンツ ウィンドウおよびレポートのエディターです。 下部にして場合によっては表示されますメッセージの通知バー。

WebMatrix と機能の詳細、これらのチュートリアルを進めるときに学習します。

## <a name="creating-a-web-page"></a>Web ページを作成します。

WebMatrix と ASP.NET Web Pages について理解しておくには、単純なページを作成します。

ワークスペース セレクターで選択、**ファイル**ワークスペース。 このワークスペースでは、ファイルとフォルダーを操作することができます。 左側のウィンドウは、サイトのファイル構造を示しています。 ファイルに関連するタスクを表示するリボン変更します。

![WebMatrix でファイル ワークスペース](getting-started/_static/image13.png)

リボンでは、下の矢印をクリックして**新規** をクリックし、**新しいファイル**です。

![使用して、&quot;新規&quot;新しいファイルを作成するには、リボンのコマンド](getting-started/_static/image14.png)

WebMatrix には、ファイルの種類の一覧が表示されます。 選択**CSHTML**、し、、**名前**ボックスに、"HelloWorld"を入力します。 ASP.NET Web Pages ページ、CSHTML ページです。

![名前付き HelloWorld.cshtml の新しい CSHTML ページの作成](getting-started/_static/image15.png)

**[OK]**をクリックします。

WebMatrix は、ページが作成され、エディターで開きます。

![WebMatrix エディターで新しい HelloWorld ページ](getting-started/_static/image16.png)

ご覧のように、ページには、上部の次のようなブロックを除くのほとんどの場合の通常 HTML マークアップが含まれています。

[!code-cshtml[Main](getting-started/samples/sample1.cshtml)]

コードを追加、すぐにわかります。

注意して、ページのさまざまな部分&mdash;要素名、属性、テキスト、および上部にあるブロック-は、すべて異なる色。 これと呼ばれる*構文の強調表示*とそれを簡単にクリアします。 これは、WebMatrix で web ページを使用しやすく機能の 1 つです。

コンテンツを追加、`<head>`と`<body>`要素は、次の例と同様にします。 (する場合は、だけ次のブロックをコピーして全体の既存のページをコードに置き換えます。)

[!code-cshtml[Main](getting-started/samples/sample2.cshtml)]

クイック アクセス ツールバーまたは、**ファイル** メニューのをクリックして**保存**です。

![WebMatrix クイック アクセス ツールバーの [保存] ボタン](getting-started/_static/image17.png)

## <a name="testing-the-page"></a>ページのテスト

**ファイル** ワークスペースを右クリックし、 *HelloWorld.cshtml*ページし、をクリックして**ブラウザーで起動**です。

![WebMatrix のリボンの [実行] ボタンを使用し、ページを実行しています。](getting-started/_static/image18.png)

WebMatrix は、コンピューター上のページのテストに使用できる組み込みの web サーバー (IIS Express) を開始します。 (せず、WebMatrix で IIS Express が発行する必要が、ページ、web サーバーにどこかにテストすることも前にします。)ページは、既定のブラウザーで表示されます。

![&quot;Hello World&quot;ブラウザーで実行されているページ](getting-started/_static/image19.png)

WebMatrix では、ページをテストするときに、ブラウザーで URL がよう`http://localhost:33651/HelloWorld.cshtml.`名前*localhost*ページは、自分のコンピューター上にある web サーバーによって処理されていることを意味、ローカル サーバーを指します。 前述のように、WebMatrix には、ページを起動するときに実行される IIS Express という名前の web サーバー プログラムが含まれています。

数値*localhost* (たとえば、 *localhost:33651*) を指す、*ポート番号*コンピューターにします。 これは、""IIS Express を使用するチャネルのこの特定の web サイトの数です。 ポート番号は、サイトを作成すると異なっているすべてのサイトを作成することは 1024 ~ 65536 範囲からランダムに選択されます。 (独自のサイトをテストするときに、ポート番号ほぼ確実になります 33561 よりも数が異なる)。別のポートを使用して、web サイトごとに、IIS Express を保存できます直線がどのサイトに通信することができます。

表示されない、パブリック web サーバーにサイトを発行するときに後で*localhost* URL にします。 その時点では、ような一般的な URL が表示されます`http://myhostingsite/mywebsite/HelloWorld.cshtml`か、どのようなページです。 このチュートリアル シリーズの後で、サイトの発行について学びます。

## <a name="adding-some-server-side-code"></a>一部のサーバー側コードを追加します。

ブラウザーを終了し、WebMatrix 内のページに戻ります。

次のコードのように見えるように、コード ブロックに行を追加します。

[!code-cshtml[Main](getting-started/samples/sample3.cshtml)]

これは、少しの Razor コードです。 現在の日付と時刻を取得および値を配置するには、おそらく明確では、*変数*という`currentDateTime`です。 詳細をお読みを次のチュートリアルでは、Razor 構文の詳細。

ページの本文で後に、`<p>Hello World!</p>`要素、次の追加。

[!code-html[Main](getting-started/samples/sample4.html)]

このコードに含まれる値を取得する、`currentDateTime`上部にある変数し、ページのマークアップに挿入します。 `@`文字は、ページ内の ASP.NET Web ページ コードをマークします。

(WebMatrix の変更を保存するページを実行する前に) もう一度ページを実行します。 この日付が表示する時間とページの時間。

![&quot;Hello World&quot;動的に生成された時刻の表示を使用してブラウザーで実行されているページ](getting-started/_static/image20.png)

しばらく待ってから、ブラウザーでページを更新します。 日付と時刻の表示が更新されます。

ブラウザーでページ ソースを見てください。 次のマークアップようになります。

[!code-html[Main](getting-started/samples/sample5.html)]

注意して、`@{ }`上部のブロックがないです。 日付と時刻が表示された文字の実際の文字列に注意してください (`1/18/2012 2:49:50 PM`または) ではなく、`@currentDateTime`に表示されていたように、 *.cshtml*ページ。 ここでは、ページを実行したときに、ASP.NET のすべてのコード (はここではほとんどありません) でマークされたと処理を何が起こった`@`です。 コードの出力は、し、その出力が、ページに挿入します。

## <a name="this-is-what-aspnet-web-pages-are-about"></a>これは、ASP.NET Web Pages がについて

ASP.NET Web Pages で動的 web コンテンツが生成されることを読み取るときにここで確認した新機能をお勧めします。 作成したページには、前に説明した同じ HTML マークアップが含まれています。 また、あらゆる種類の作業を実行するコードを含めることもできます。 この例では、現在の日付と時刻を取得する簡単な作業をでした。 学習したようにコードを挿入する html ページの出力を生成することができます。 他のユーザーが要求したとき、 *.cshtml* web サーバーの手にまだある間に、ブラウザー、ASP.NET のページがページを処理します。 ASP.NET は、コードの出力 (もしあれば) ページに挿入します HTML として。 コード処理が完了したら、ASP.NET は、結果のページをブラウザーに送信します。 ブラウザーが受け取ることは、HTML です。 ダイアグラムを次に示します。

![ASP.NET が動的に HTML を生成の概念フロー](getting-started/_static/image21.png)

考え方は単純な場合、ですは、コードを実行できる、多くの興味深いタスクとを動的にコンテンツを追加できます HTML ページに多くの興味深い方法があります。 ASP.NET *.cshtml*任意の HTML ページなどのページでは、ブラウザー自体 (JavaScript と jQuery コード) で実行されるコードを含めることもできます。 このチュートリアルのセットと以降では、これらがすべてを調べてみます。

## <a name="coming-up-next"></a>次へ直近の見通し

このシリーズの次のチュートリアルでは、ASP.NET Web Pages は、もう少しプログラミングを調査します。

## <a name="additional-resources"></a>その他のリソース

[ASP.NET web サイトを最初から作成](https://www.microsoft.com/web/post/create-an-aspnet-website-from-scratch)です。 これは、具体的には、チュートリアル WebMatrix (ASP.NET Web ページではない) の使用方法です。 入る、少し詳細については、このチュートリアルのセットには触れません WebMatrix の他の機能の一部です。

>[!div class="step-by-step"]
[次へ](intro-to-web-pages-programming.md)
