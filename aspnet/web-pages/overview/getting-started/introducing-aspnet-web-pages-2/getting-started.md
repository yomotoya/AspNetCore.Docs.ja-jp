---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
title: ASP.NET Web Pages の概要の紹介 |Microsoft Docs
author: tfitzmac
description: WebMatrix は推奨されなくの統合開発環境として ASP.NET Web Pages の。 Visual Studio または Visual Studio Code を使用します。 このガイダンスをしています.
ms.author: riande
ms.date: 05/28/2015
ms.assetid: a36d3bdf-ef1b-47a4-b932-3a0cf4cad716
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 835e359edc87335366c82e35c1ff04902b70334b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832163"
---
<a name="introducing-aspnet-web-pages---getting-started"></a>ASP.NET Web ページ - 作業の開始の概要
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> > [!NOTE] 
> > 
> > WebMatrix は推奨されなくの統合開発環境として ASP.NET Web Pages の。 使用[Visual Studio](../program-asp-net-web-pages-in-visual-studio.md)または[Visual Studio Code](https://code.visualstudio.com/)します。
> 
> 
> このガイダンスとアプリケーション、ASP.NET Web Pages (バージョン 2 またはそれ以降) の概要と Razor 構文を動的な web サイトを作成するための軽量なフレームワークを使用することができます。 また、WebMatrix、ページとサイトを作成するためのツールも導入されています。
> 
> **レベル**: 新しい ASP.NET Web ページ。  
> **スキルと見なされます**: HTML、カスケード スタイル シート (CSS) の基本的な。
> 
> 学習する内容、セットの最初のチュートリアルでは。
> 
> - どのような ASP.NET Web Pages テクノロジし、になります。
> - WebMatrix のです。
> - すべてをインストールする方法。
> - WebMatrix を使用して web サイトを作成する方法。
>   
> 
> 説明した機能/テクノロジ:
> 
> - Microsoft Web プラットフォーム インストーラー。
> - WebMatrix します。
> - *.cshtml*ページ
>   
> 
> Mike 教皇は、最初、このチュートリアルを作成しました。 Tom FitzMacken Microsoft WebMatrix 3 を更新します。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 3


## <a name="what-should-you-know"></a>必要な知識

慣れていると仮定します。

- **HTML**します。 深い専門知識は必要ありません。 HTML、については説明しませんが、私たちも使用しない複雑な。 いかに役立つ思います HTML チュートリアルへのリンクを提供いたします。
- **カスケード スタイル シート (CSS)** します。 同じ html です。
- **基本的なデータベース アイデア**します。 データのスプレッドシートを使用する並べ替えし、専門知識のレベルにあると、データをフィルター処理した、このチュートリアルのセットの仮定一般にします。

基本的なプログラミングに関心を仮定します。 ASP.NET Web ページは、c# と呼ばれるプログラミング言語を使用します。 プログラミングでは、ことに興味だけで、色の背景にする必要はありません。 前に、web ページで、JavaScript までに記述した場合、多数のバック グラウンドがあります。

プログラミングに慣れている場合、このチュートリアルを最初に設定動かさ緩やかに変化する新しいプログラマが備えられていますはありますに注意してください。 過去の最初のいくつかのチュートリアルさせてがありますを説明する基本的な小さいプログラミングと、モ ノは、高速なクリップに移動されます。

## <a name="what-do-you-need"></a>何が必要ですか。

必要なもの:

- Windows 8、Windows 7、Windows Server 2008、または Windows Server 2012 を実行しているコンピューター。
- インターネット接続。
- 管理者特権が (インストール必須)。

## <a name="what-is-aspnet-web-pages"></a>ASP.NET Web ページとは何ですか。

ASP.NET Web ページは、動的な web ページの作成に使用できるフレームワークです。 単純な HTML web ページは静的です。そのコンテンツは、ページ内にある固定の HTML マークアップによって決定されます。 ASP.NET Web Pages を作成するような動的なページを使用して、コードを使用して、その場でページのコンテンツを作成できます。

動的なページを使用して、さまざまな機能を実行できます。 フォームを使用してユーザー入力の入力を要求し、ページの表示または外観に変更できます。 ユーザーから情報を取得し、データベースに保存し、後で一覧表示できます。 サイトから電子メールを送信できます。 Web (マッピング サービスなど) では、その他のサービスと対話し、それらのソースから情報を統合するページを生成することができます。

## <a name="what-is-webmatrix"></a>WebMatrix は何ですか。

WebMatrix は、web ページ エディター、データベース ユーティリティ、ページ、および web サイトをインターネットに公開するための機能をテストするための web サーバーを統合するツールです。 WebMatrix は無料であり、簡単にインストールできる、使いやすくなります。 (もうまくだけプレーンな HTML ページ、および PHP のようなその他のテクノロジです。)

実際にそうしないと*が*WebMatrix を使用して ASP.NET Web ページを操作します。 エディター、テキストを使用してページを作成、できへのアクセスのある web サーバーを使用してページをテストできます。 ただし、WebMatrix で非常に簡単ではすべて、ので、これらのチュートリアルが全体にわたって WebMatrix を使用します。

## <a name="about-these-tutorials"></a>これらのチュートリアルについて

このチュートリアルのセットは、ASP.NET Web Pages を使用する方法について概説します。 この入門チュートリアルのセットの合計は 9 チュートリアルです。 実際、本格的な web サイトの作成に ASP.NET Web Pages の初心者から移動する一連のチュートリアルのセットの一部ですね。

この最初のチュートリアルでは、ASP.NET Web Pages を操作する方法の基本を学ぶことにまとめるのでを設定します。 完了すると、集荷場所この 1 つを終了し、Web ページを詳しく探索する追加のチュートリアルのセットを使用できます。

意図的に進む簡単に詳細な説明。 何かを紹介するたびにこのチュートリアルのセットを常に選択方法と考えることが理解する最も簡単です。 以降のチュートリアルのセットでは、さらにしより効率的なまたはより柔軟なアプローチ (もより楽しく) を表示します。 ただし、これらのチュートリアルでは、まず基本を理解する必要があります。

開始した、チュートリアル セットでは、ASP.NET Web Pages のこれらの機能について説明します。

- 概要とインストールされているすべてのものを取得します。 (内にあるチュートリアルをご覧になっている。)
- ASP.NET Web Pages のプログラミングの基礎です。
- データベースを作成します。
- 作成して、ユーザー入力フォームを処理します。
- 追加、更新、およびデータベース内のデータを削除しています。

## <a name="what-will-you-create"></a>どのような作成

このチュートリアルの設定し、たいビデオを一覧表示、web サイトに焦点を絞って以降。 映画を入力、編集すること、およびそれらを一覧表示することができます。 ここでいくつかのページで、このチュートリアルのセットを作成します。 1 つ目は、ムービーの一覧を作成するページを示しています。

![ムービーの一覧を表示はムービー アプリ](getting-started/_static/image1.png)

新しいムービーの情報をサイトに追加することができます ページを次に示します。

![ムービーの追加 ページを示す最終的なムービー アプリ](getting-started/_static/image2.png)

このチュートリアルの後続のセットのビルドでは、設定し、電子メールの送信や、ソーシャル メディアとの統合の画像のアップロード、ユーザーにログインできるようにすることなど、多くの機能を追加します。

## <a name="see-this-app-running-on-azure"></a>Azure で実行されているこのアプリを参照してください。

ライブ web アプリとして実行されている完成したサイトを参照してもよろしいですか。 Azure アカウントに、アプリの完全なバージョンを展開するには、次のボタンをクリックするだけです。

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebPagesMovies)

このソリューションを Azure にデプロイする Azure アカウントが必要です。 アカウントがいない場合は、次のオプションがあります。

- [無料 Azure アカウントを開く](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604)-クレジットを取得する有料の Azure サービスを使用することができますをも使用されるアカウントは維持する最大無料の Azure サービスを使用します。
- [MSDN サブスクライバーの特典をアクティブ化](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604)-MSDN サブスクリプションでは、クレジットの有料の Azure サービスを使用することができる毎月。

## <a name="installing-everything"></a>すべてのインストール

すべては、microsoft Web Platform Installer を使用してインストールできます。 実際には、インストーラーをインストールし、他のすべてのインストールに使用することです。

少なくともを持っていることがある Web ページを使用して、Windows XP SP3 をインストールまたは Windows Server 2008 またはそれ以降。

[Web Pages ページ](../../../index.md)ASP.NET web サイトの次のようにクリックします。**インストール**します。

![ASP.NET Web サイトが表示された&quot;WebMatrix のインストール&quot;ボタン](getting-started/_static/image3.png)

ライセンス条項と WebMatrix をインストールする前にプライバシーに関する声明に同意を求められます。

![インストールを開始する用語をそのまま使用します。](getting-started/_static/image4.png)

クリックして**実行**インストールを開始します。 (このインストーラーを保存する場合は、クリックして**保存**し保存したフォルダーから、インストーラーを実行します)。

![](getting-started/_static/image5.png)

Web Platform Installer が表示されたら、WebMatrix をインストールする準備が整いました。 **[インストール]** をクリックします。

![](getting-started/_static/image6.png)

インストール プロセスはコンピューターにインストールする必要がある判別し、プロセスを開始します。 によってインストールされるように内容が正確に、プロセスできます任意の場所数分からに数分かかります。 選択**同意**ライセンス条項に同意します。

## <a name="hello-webmatrix"></a>こんにちは, WebMatrix

完了したら、インストール プロセスは WebMatrix を自動的に起動できます。 開かない場合は、Windows 内から、**開始**] メニューの [起動**Microsoft WebMatrix**します。

初めて WebMatrix を起動するときに、Microsoft アカウントで Microsoft Azure にサインインする機会が提供されます。 サインインすると、Azure での 10 個の無料の web アプリが表示されます。 これらの無料の web アプリでは、アプリをテストする便利な手段を提供します。 場合は、Azure アカウントでは、既に必要はありませんが、MSDN サブスクリプションが、 [MSDN サブスクリプションの特典をアクティブ化](https://www.windowsazure.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604)します。 それ以外の場合、ほんの数分で無料試用版アカウントを作成できます。 詳細については、次を参照してください。 [Azure 無料試用版](https://azure.microsoft.com/free/?WT.mc_id=A443DD604)します。

このチュートリアルを続行して今すぐサインインする必要はありません。 サインインしないようになりました場合、は、後でサインインするオプションが続行されます。 最後の[トピック](publishing.md)シリーズでは、このチュートリアルには、web サイトを Azure にデプロイする方法について説明します。 そのトピックを完了するサインインしなければ、そのためです。

この時点では、いずれかでサインイン、Microsoft アカウントまたは選択します**今**右下隅にします。

![サインイン](getting-started/_static/image7.png)

を開始するには、は空白の web サイトを作成し、ページを追加します。 このセットの後のチュートリアルでは、組み込みの web サイト テンプレートのいずれかで再生されます。

[開始] ウィンドウ**新規**します。

![WebMatrix の起動画面](getting-started/_static/image8.png)

テンプレートは、事前構築済みのファイルや web サイトのさまざまな種類のページです。 既定で使用できるテンプレートのすべてを表示するには、テンプレート ギャラリーのオプションを選択します。

![テンプレート ギャラリーの選択](getting-started/_static/image9.png)

**クイック スタート**ウィンドウで、**空のサイト**から、 **ASP.NET**をグループ化し、新しいサイトの名前を"WebPagesMovies"。

![選択したテンプレートを空のサイトでのウィンドウの WebMatrix クイック スタート](getting-started/_static/image10.png)

**[次へ]** をクリックします。

Microsoft アカウントにサインインした場合、サイトを Azure に作成する機会が表示されます。 既定の名前、サイトの名前に基づく**WebPagesMovies.azurewebsites.net**が提案されます。 ただし、感嘆符はこの名前が Windows Azure で使用できないことを示します。 わかりやすくするために、次のように選択します。**スキップ**を今すぐ Azure での web サイトの作成をバイパスします。 このシリーズの後半には、Azure にサイトを公開しますが。

![azure サイトを作成します。](getting-started/_static/image11.png)

WebMatrix は、作成し、サイトが開きます。

![新しい WebPagesMovies サイトを WebMatrix で開く](getting-started/_static/image12.png)

上部にあるクイック アクセス ツールバーとリボンがあります。 左下に表示ワークスペース セレクター タスク間で切り替えた (**サイト**、**ファイル**、**データベース**、**レポート**)。 右側には、コンテンツ ウィンドウはエディターとレポートします。 下部には、メッセージの通知バー場合によっては表示されます。

これらのチュートリアルを進めるときの詳細は WebMatrix とその機能について説明します。

## <a name="creating-a-web-page"></a>Web ページを作成します。

WebMatrix と ASP.NET Web ページを理解するには、単純なページを作成します。

ワークスペース セレクターで選択、**ファイル**ワークスペース。 このワークスペースを使用してファイルとフォルダーを操作できます。 左側のペインには、サイトのファイル構造が表示されます。 ファイルに関連するタスクを表示するリボン変更します。

![WebMatrix でファイル ワークスペース](getting-started/_static/image13.png)

リボンで、下の矢印をクリックします。**新規** をクリックし、**新しいファイル**します。

![使用して、&quot;新規&quot;で新しいファイルを作成するには、リボン コマンド](getting-started/_static/image14.png)

WebMatrix は、ファイルの種類の一覧を表示します。 選択**CSHTML**、し、**名前**ボックスに、"HelloWorld"を入力します。 CSHTML ページは、ASP.NET Web Pages ページです。

![HelloWorld.cshtml という名前の新しい CSHTML ページを作成します。](getting-started/_static/image15.png)

**[OK]** をクリックします。

WebMatrix は、ページを作成し、エディターで開かれます。

![WebMatrix エディターで新しい HelloWorld ページ](getting-started/_static/image16.png)

ご覧のように、ページには、次のような上部にあるブロックを除くのほとんどの場合の通常 HTML マークアップが含まれています。

[!code-cshtml[Main](getting-started/samples/sample1.cshtml)]

すぐにわかるコードを追加するためです。

注意して、ページのさまざまな部分&mdash;要素名、属性、テキスト、および、上部にあるブロック-は、異なる色ですべてです。 これは呼び出されます*構文の強調表示*、し、すべてをクリアしやすくなります。 WebMatrix で web ページを使用するが容易にする機能の 1 つになります。

コンテンツを追加、`<head>`と`<body>`次の例などの要素。 (する場合は、のみ次のブロックをコピーして全体の既存のページをこのコードに置き換えます。)

[!code-cshtml[Main](getting-started/samples/sample2.cshtml)]

クイック アクセス ツールバーまたは、**ファイル** メニューのをクリックして**保存**します。

![WebMatrix クイック アクセス ツールバーで [保存] ボタン](getting-started/_static/image17.png)

## <a name="testing-the-page"></a>ページのテスト

**ファイル** ワークスペースで、右クリックし、 *HelloWorld.cshtml*ページをクリックして**ブラウザーで起動**します。

![WebMatrix リボンの [実行] ボタンを使用してページを実行します。](getting-started/_static/image18.png)

WebMatrix は、コンピューター上のページのテストに使用できる組み込みの web サーバー (IIS Express) を開始します。 (なし、WebMatrix で IIS Express は発行する必要が、ページ、web サーバーにどこかに前に、テストすることでした。)ページは、既定のブラウザーに表示されます。

![&quot;Hello World&quot;ブラウザーで実行されているページ](getting-started/_static/image19.png)

WebMatrix のページをテストする場合、ブラウザーで URL がよう`http://localhost:33651/HelloWorld.cshtml.`名前*localhost*ページが、自分のコンピューター上にある web サーバーによって処理されることを意味する、ローカル サーバーを指します。 前述のように、WebMatrix には、IIS Express ページを起動するときに実行されるという名前の web サーバー プログラムが含まれています。

数値*localhost* (たとえば、 *localhost:33651*) を指す、*ポート番号*コンピューターにします。 これは、この特定の web サイトの IIS Express を使用する「チャネル」の数です。 ポート番号は、サイトを作成すると作成したすべてのサイトに対して異なる 1024 ~ 65536 の範囲からランダムに選択されます。 (独自のサイトをテストするときに、ポート番号ほぼ確実になります 33561 よりも数が異なる)。各 web サイトを別のポートを使用して IIS Express を保持できます直線がどのサイトが対話します。

表示されなくパブリック web サーバーにサイトを発行するときに後で*localhost* URL にします。 その時点でのような一般的な URL が表示されます`http://myhostingsite/mywebsite/HelloWorld.cshtml`か、どのようなページです。 このチュートリアル シリーズの後半で、サイトの発行について学びます。

## <a name="adding-some-server-side-code"></a>いくつかのサーバー側コードを追加します。

ブラウザーを閉じて、WebMatrix で、ページに戻ります。

次のように見えるように、コード ブロックに行を追加します。

[!code-cshtml[Main](getting-started/samples/sample3.cshtml)]

これは、少しの Razor コードです。 現在の日付と時刻を取得し、その値には、こと可能性があります明確では、*変数*という`currentDateTime`します。 詳細を確認します、次のチュートリアルの Razor 構文の詳細について。

ページの本文で後に、`<p>Hello World!</p>`要素、次の追加。

[!code-html[Main](getting-started/samples/sample4.html)]

このコードに含まれる値の取得、`currentDateTime`上部にある変数、ページのマークアップに挿入します。 `@`文字は、ページの ASP.NET Web ページ コードをマークします。

(WebMatrix 変更を保存しますが、ページを実行する前に) もう一度ページを実行します。 この時間、日付の表示とページの時間。

![&quot;Hello World&quot;を動的に生成された時刻の表示を使用してブラウザーで実行されているページ](getting-started/_static/image20.png)

少し待つし、ブラウザーでページを更新します。 日付と時刻の表示が更新されます。

ブラウザーでページ ソースを見てください。 次のマークアップのようになります。

[!code-html[Main](getting-started/samples/sample5.html)]

なお、`@{ }`上部にあるブロックがないです。 また、日付と時刻の表示が、実際の文字列の文字を示しています (`1/18/2012 2:49:50 PM`または) ではなく、`@currentDateTime`いたように、 *.cshtml*ページ。 ここでは、ページを実行したときに ASP.NET がすべてのコード (はここではほとんどありません) でマークされたを処理する変更点`@`します。 コードの出力は、およびその出力がページに挿入されます。

## <a name="this-is-what-aspnet-web-pages-are-about"></a>これは、ASP.NET Web Pages がについて

ASP.NET Web ページに動的な web コンテンツが生成されることを読み取るときに、という考えにはここで説明しました。 作成したページには、前に説明した同じの HTML マークアップが含まれています。 また、あらゆる種類のタスクを実行できるコードを含めることもできます。 この例で、現在の日付と時刻を取得する簡単な作業をでした。 学習したようにコードを分散させてを HTML ページの出力を生成することができます。 ユーザーが要求したときに、 *.cshtml* web サーバーの手の中に、ブラウザー、ASP.NET でページがページを処理します。 ASP.NET 挿入コードの出力 (ある場合)、ページに HTML としてします。 コードの処理を完了すると、ASP.NET は、結果のページをブラウザーに送信します。 HTML は、すべてこれまで、ブラウザーを取得します。 ダイアグラムを次に示します。

![ASP.NET が動的に HTML を生成の概念フロー](getting-started/_static/image21.png)

考え方は単純は、コードが実行できる多くの興味深いタスクとを動的に HTML コンテンツを追加できるページに多くの興味深い方法があります。 ASP.NET *.cshtml*任意の HTML ページのように、ページがブラウザー自体 (JavaScript と jQuery コード) で実行されるコードを含めることもできます。 このチュートリアルのセットと以降では、これらがすべてについて説明します。

## <a name="coming-up-next"></a>次回について

このシリーズの次のチュートリアルでは ASP.NET Web Pages は、もう少しプログラミングについて説明します。

## <a name="additional-resources"></a>その他のリソース

[ASP.NET web サイトを最初から作成](https://www.microsoft.com/web/post/create-an-aspnet-website-from-scratch)です。 これは具体的には、チュートリアルに関する WebMatrix (ASP.NET Web ページではない) を使用しています。 なる少しの詳細については、この一連のチュートリアルでは触れません WebMatrix の追加機能の一部です。

> [!div class="step-by-step"]
> [次へ](intro-to-web-pages-programming.md)
