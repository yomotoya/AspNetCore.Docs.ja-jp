---
uid: mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-vb
title: ASP.NET MVC を使用して、さまざまなバージョンの IIS (VB) |Microsoft Docs
author: microsoft
description: このチュートリアルでは、異なるバージョンのインターネット インフォメーション サービスで ASP.NET MVC、および URL ルーティングを使用する方法について説明します。 さまざまな方法をについて説明します.
ms.author: riande
ms.date: 08/19/2008
ms.assetid: 1c1283b2-6956-4937-b568-d30de432ce23
msc.legacyurl: /mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-vb
msc.type: authoredcontent
ms.openlocfilehash: e83a5f8e5d1726dc2f39a9aee6515995ce0ed157
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826726"
---
<a name="using-aspnet-mvc-with-different-versions-of-iis-vb"></a>ASP.NET MVC を使用して、さまざまなバージョンの IIS (VB)
====================
によって[Microsoft](https://github.com/microsoft)

> このチュートリアルでは、異なるバージョンのインターネット インフォメーション サービスで ASP.NET MVC、および URL ルーティングを使用する方法について説明します。 IIS 7.0 (クラシック モード)、IIS 6.0 では、以前のバージョンの IIS と ASP.NET MVC を使用するためのさまざまな方法を学習します。


ASP.NET MVC フレームワークは、コント ローラー アクションへのブラウザー要求をルーティングする ASP.NET ルーティングに依存します。 ASP.NET ルーティングを利用するには、web サーバーで追加の構成手順を実行する必要があります。 すべては、インターネット インフォメーション サービス (IIS) と処理モードは、アプリケーションの要求のバージョンによって異なります。

異なるバージョンの IIS の概要を次に示します。

- IIS 7.0 (統合モード) - ASP.NET のルーティングを使用するために必要な特別な構成がありません。
- IIS 7.0 (クラシック モード) - ASP.NET のルーティングを使用する特別な構成を実行する必要があります。
- IIS 6.0 または以下の ASP.NET のルーティングを使用する特別な構成を実行する必要があります。

最新バージョンの IIS バージョン 7.5 (Win7) には IIS の IIS 7 は Windows Server 2008 と VISTA SP1 に含まれる以上です。 インストールすることもできる IIS 7.0 の Home Basic 以外の Vista オペレーティング システムの任意のバージョン (を参照してください[ https://technet.microsoft.com/library/cc731179%28WS.10%29.aspx ](https://technet.microsoft.com/library/cc731179%28WS.10%29.aspx))。

IIS 7.0 では、要求を処理する 2 つのモードをサポートします。 統合モードまたはクラシック モードを使用することができます。 統合モードで IIS 7.0 を使用する場合は、特別な構成手順を実行する必要はありません。 ただしは IIS 7.0 を使用してクラシック モードのときに、追加の構成を実行する必要があります。

Microsoft Windows Server 2003 には、IIS 6.0 が含まれています。 Windows Server 2003 オペレーティング システムを使用する場合は、IIS 6.0 を IIS 7.0 にアップグレードできません。 IIS 6.0 を使用する場合は、追加の構成手順を実行する必要があります。

Microsoft Windows XP Professional には、IIS 5.1 が含まれています。 IIS 5.1 を使用する場合は、追加の構成手順を実行する必要があります。

最後に、Microsoft Windows 2000 および Microsoft Windows 2000 Professional には、IIS 5.0 が含まれます。 IIS 5.0 を使用する場合は、追加の構成手順を実行する必要があります。

## <a name="integrated-versus-classic-mode"></a>クラシック モードとの統合

2 つの異なる要求の処理モードを使用して要求を処理できる IIS 7.0: 統合とクラシックします。 統合モードでは、パフォーマンスが向上しより多くの機能を提供します。 クラシック モードがの下位に含まれる IIS の以前のバージョンとの互換性。

要求の処理モードは、アプリケーション プールによって決定されます。 処理モードは、アプリケーションに関連付けられているアプリケーション プールを決定することにより、特定の web アプリケーションで使用されているを指定できます。 この場合は、以下の手順に従ってください。

1. インターネット インフォメーション サービス マネージャを起動します。
2. [接続] ウィンドウで、アプリケーションを選択します。
3. アクション ウィンドウ、**基本設定**アプリケーションの編集 ダイアログを開くためのリンク ボックス (図 1 参照)
4. 選択したアプリケーション プールをメモしてをおきます。

2 つのアプリケーション プールをサポートするために既定では、IIS が構成されている: **DefaultAppPool**と**クラシック .NET AppPool**します。 DefaultAppPool が選択されている場合は、統合された要求の処理モードでアプリケーションが実行されます。 従来の .NET AppPool が選択されている場合は、従来の要求の処理モードでは、アプリケーションが実行されています。


[![[新しいプロジェクト] ダイアログ ボックス](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image1.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image1.png)

**図 1**: 要求の処理モードの検出 ([フルサイズの画像を表示する をクリックします](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image2.png))。


アプリケーションの編集 ダイアログ ボックス内で要求の処理モードを変更できることに注意してください。 [選択] ボタンをクリックし、アプリケーションに関連付けられているアプリケーション プールを変更します。 クラシックから ASP.NET アプリケーションを統合モードに変更するときに互換性の問題が認識はことに注意してください。 詳細については、次の記事を参照してください。

- Windows Vista および Windows Server 2008 - 上の iis 7.0 の ASP.NET 1.1 のアップグレード [https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008)

- Iis 7.0 の ASP.NET の統合 [https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis)


ASP.NET アプリケーションは、DefaultAppPool を使用している場合は、ASP.NET のルーティング (とそのため ASP.NET MVC) を機能を取得する追加の手順を実行する必要はありません。 ただし、ASP.NET アプリケーションは、従来の .NET AppPool を使用し、読み取りを保持するように構成、さらに作業があります。

## <a name="using-aspnet-mvc-with-older-versions-of-iis"></a>古いバージョンの IIS と共に ASP.NET MVC を使用します。

IIS 7.0 よりも古いバージョンの IIS で ASP.NET MVC を使用する必要がある、または IIS 7.0 を使用してクラシック モードにする必要がある、2 つのオプションがあります。 最初に、ファイルの拡張機能を使用するルート テーブルを変更できます。 たとえば、/Store/Details ような URL を要求するには、代わりに/Store.aspx/Details ような URL を要求します。

2 番目のオプションと呼ばれるものを作成する、*ワイルドカード スクリプト マップ*します。 ワイルドカード スクリプト マップでは、ASP.NET フレームワークにすべての要求をマップすることができます。

Web サーバー (たとえば、アプリケーションは、インターネット サービス プロバイダーによってホストされている ASP.NET MVC) へのアクセスができない場合が、最初のオプションを使用する必要があります。 、Url の外観を変更する、web サーバーにアクセス権がある場合は、2 番目のオプションを使用することができます。

次のセクションで詳しくは、各オプションをについて説明します。

## <a name="adding-extensions-to-the-route-table"></a>ルート テーブルに拡張機能の追加

古いバージョンの IIS を使用する ASP.NET のルーティングを取得する最も簡単な方法では、Global.asax ファイルで、ルート テーブルを変更します。 既定値と、リスト 1 で Global.asax ファイルが変更されていない既定のルートをという名前の 1 つのルートを構成します。

**1 - Global.asax (未変更の状態) を一覧表示します。**

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample1.vb)]

リスト 1 で構成されている既定のルートを使用すると、次のようにルート Url:

/ホーム/インデックス

製品/詳細/3

/製品

残念ながら、古いバージョンの IIS、ASP.NET フレームワークにこれらの要求が転送されません。 そのため、これらの要求をコント ローラーにルーティングされません。 たとえば、URL/Home/インデックスのブラウザー要求を行う場合は、図 2 に、エラー ページが表示されます。


[![[新しいプロジェクト] ダイアログ ボックス](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image2.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image3.png)

**図 2**: 404 Not Found エラーが発生する ([フルサイズの画像を表示する をクリックします](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image4.png))。


古いバージョンの IIS は、ASP.NET フレームワークにのみ特定の要求をマップします。 適切なファイル拡張子を持つ URL に対して要求があります。 たとえば、/SomePage.aspx の要求は、ASP.NET フレームワークにマップを取得します。 ただし、/SomePage.htm 要求されていません。

そのため、ASP.NET のルーティングを機能させるには、ASP.NET フレームワークにマップされているファイル拡張子が含まれるように、既定のルートを変更する必要があります。

これは、という名前のスクリプトを使用して`registermvc.wsf`します。 ASP.NET MVC 1 リリースには含まれて`C:\Program Files\Microsoft ASP.NET\ASP.NET MVC\Scripts`、ASP.NET 2 の時点でこのスクリプト」で使用可能な ASP.NET Futures に移動しましたが、 [ http://aspnet.codeplex.com/releases/view/39978](http://aspnet.codeplex.com/releases/view/39978)します。

このスクリプトを実行すると、新しい .mvc 拡張機能を IIS に登録します。 .Mvc 拡張機能を登録すると後、は、ルートは、.mvc 拡張機能を使用するために、Global.asax ファイルで、ルートを変更できます。

リスト 2 で修正された Global.asax ファイルは、IIS の以前のバージョンで動作します。

**2 - Global.asax (拡張機能と変更) を一覧表示します。**

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample2.vb)]


重要:、Global.asax ファイルを変更した後にもう一度 ASP.NET MVC アプリケーションのビルドに注意してください。


リスト 2 で、Global.asax ファイルに 2 つの重要な変更があります。 Global.asax で定義されている 2 つのルートはようになりました。 既定のルートの最初のルート URL パターンは、ようになります。

{controller}.mvc/{action}/{id}

.Mvc 拡張機能の追加は、ASP.NET ルーティング モジュールをインターセプトするファイルの種類を変更します。 この変更により、ASP.NET MVC アプリケーションを今すぐは、次のような要求をルーティングします。

/Home.mvc/Index/

/Product.mvc/Details/3

/Product.mvc/

2 つ目のルートでは、ルートのルートは追加されました。 ルートのルートの場合は、この URL パターンは、空の文字列です。 このルートは、アプリケーションのルートに対して行われた要求に一致する必要があります。 たとえば、ルートのルートは、次のような要求と一致は。

[http://www.YourApplication.com/](http://www.YourApplication.com/)

ルート テーブルに次の変更を行った後は、すべてのアプリケーションのリンクが、これらの新しい URL パターンと互換性があるかどうかを確認する必要があります。 つまり、.mvc 拡張機能にはすべてのリンクが含まれているを確認します。 Html.ActionLink() のヘルパー メソッドを使用して、リンクを生成する場合何も変更する必要がありません。


Registermvc.wcf スクリプトを使用する代わりに iis を手動で ASP.NET フレームワークにマップされている新しい拡張機能を追加できます。 新しい拡張機能を自分で追加するときに、チェック ボックスをオンにというラベルが付いたこと確認**ファイルが存在することを確認**はチェックされません。


## <a name="hosted-server"></a>ホストされているサーバー

Web サーバーへのアクセスを常に必要はありません。 たとえば、インターネット ホストのプロバイダーを使用して、ASP.NET MVC アプリケーションをホストしている場合、必要がある必ずしも IIS へのアクセス。

その場合は、ASP.NET フレームワークにマップされている既存のファイル拡張子のいずれかを使用する必要があります。 ASP.NET にマップされているファイルの拡張機能の例には、.aspx、.axd、および .ashx 拡張機能が含まれます。

たとえば、リスト 3 の変更の Global.asax ファイルでは、.mvc 拡張機能ではなく .aspx 拡張機能を使用します。

**3 - Global.asax (拡張子が .aspx 変更) を一覧表示します。**

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample3.vb)]

リスト 3 の Global.asax ファイルは、正確に .mvc 拡張機能ではなく .aspx 拡張機能を使用してそれを除けば、以前の Global.asax ファイルと同じです。 .Aspx 拡張機能を使用するリモートの web サーバーでセットアップを実行する必要はありません。

## <a name="creating-a-wildcard-script-map"></a>ワイルドカード スクリプト マップを作成します。

ASP.NET MVC アプリケーションの Url を変更する、web サーバーにアクセス権がある場合は、1 つのオプションがあります。 ASP.NET framework web サーバーにすべての要求をマップするワイルドカード スクリプト マップを作成することができます。 これにより、(クラシック モード) では IIS 7.0 または IIS 6.0 は既定の ASP.NET MVC ルート テーブルを使用できます。

このオプションは、IIS web サーバーに対して行われたすべての要求をインターセプトすることに注意します。 これには、イメージ、従来の ASP ページ、および HTML ページの要求が含まれます。 そのため、ワイルドカードの有効化スクリプト マップの asp.net がパフォーマンスに影響します。

IIS 7.0 の場合、ワイルドカード スクリプト マップを有効にする方法を次に示します。

1. [接続] ウィンドウで、アプリケーションを選択します。
2. 必ず、**機能**ビューが選択されています。
3. ダブルクリックして、**ハンドラー マッピング**ボタン
4. をクリックして、**ワイルドカード スクリプト マップの追加**(図 3 参照) リンク
5. Aspnet へのパスを入力\_isapi.dll のファイル (PageHandlerFactory スクリプト マップからこのパスをコピーできます)
6. MVC の名前を入力します。
7. をクリックして、 **OK**ボタン


[![[新しいプロジェクト] ダイアログ ボックス](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image3.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image5.png)

**図 3**: IIS 7.0 でワイルドカード スクリプト マップの作成 ([フルサイズの画像を表示する をクリックします](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image6.png))。


IIS 6.0 ではワイルドカード スクリプト マップを作成する次の手順に従います。

1. Web サイトを右クリックし、プロパティを選択します。
2. 選択、**ホーム ディレクトリ** タブ
3. をクリックして、**構成**ボタン
4. 選択、**マッピング** タブ
5. をクリックして、**挿入**ボタン (図 4 参照)
6. Aspnet へのパスを貼り付けます\_isapi.dll (.aspx ファイルのスクリプト マップからこのパスをコピーできます)、実行可能ファイルのフィールドに
7. チェック ボックスをオフに**ファイルの存在を確認します。**
8. をクリックして、 **OK**ボタン


[![[新しいプロジェクト] ダイアログ ボックス](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image4.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image7.png)

**図 4**: IIS 6.0 ではワイルドカード スクリプト マップの作成 ([フルサイズの画像を表示する をクリックします](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image8.png))。


ワイルドカード スクリプト マップを有効にした後は、ルートのルートを含むように、Global.asax ファイルのルート テーブルを変更する必要があります。 それ以外の場合、アプリケーションのルート ページの要求を行うときに、図 5 で、エラー ページが表示されます。 リスト 4 変更後の Global.asax ファイルを使用することができます。


[![[新しいプロジェクト] ダイアログ ボックス](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image5.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image9.png)

**図 5**: 不足しているルートのルートのエラー ([フルサイズの画像を表示する をクリックします](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image10.png))。


**4 - Global.asax (ルートのルートで変更) を一覧表示します。**

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample4.vb)]

IIS 7.0 または IIS 6.0 のいずれかのワイルドカード スクリプト マップを有効にした後は、次のように既定のルーティング テーブルを使用する要求を行うことができます。

/

/ホーム/インデックス

製品/詳細/3

/製品

## <a name="summary"></a>まとめ

このチュートリアルの目的は、以前のバージョンの IIS (またはクラシック モードでの IIS 7.0) を使用する場合、ASP.NET MVC を使用する方法について説明することでした。 古いバージョンの IIS を使用する ASP.NET のルーティングを取得する 2 つの方法について説明しました。 既定のルーティング テーブルを変更またはワイルドカード スクリプト マップを作成します。

最初のオプションでは、ASP.NET MVC アプリケーションで使用される Url を変更する必要があります。 この最初のオプションの非常に重要な利点の 1 つは、ルート テーブルを変更するには、web サーバーへのアクセスが不要することです。 つまり、インターネットと ASP.NET MVC アプリケーションをホストしている場合でも、この最初のオプションを使用できますホスティング会社。

2 番目のオプションでは、ワイルドカード スクリプト マップを作成します。 この 2 つ目のオプションの利点は、Url を変更する必要はありません。 この 2 つ目のオプションの欠点は、ASP.NET MVC アプリケーションのパフォーマンス影響を与えることができます。

> [!div class="step-by-step"]
> [前へ](using-asp-net-mvc-with-different-versions-of-iis-cs.md)
