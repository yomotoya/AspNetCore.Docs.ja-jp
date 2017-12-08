---
uid: mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-vb
title: "ASP.NET MVC を使用して、さまざまなバージョンの IIS (VB) |Microsoft ドキュメント"
author: microsoft
description: "このチュートリアルでは、インターネット インフォメーション サービスの異なるバージョンの ASP.NET MVC と URL がルーティングを使用する方法を学習します。 さまざまな方法について説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: 1c1283b2-6956-4937-b568-d30de432ce23
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-vb
msc.type: authoredcontent
ms.openlocfilehash: 57a729501d15ebf9a533716b2a1767766954bb4c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="using-aspnet-mvc-with-different-versions-of-iis-vb"></a>別のバージョンの IIS (VB) での ASP.NET MVC の使用
====================
によって[Microsoft](https://github.com/microsoft)

> このチュートリアルでは、インターネット インフォメーション サービスの異なるバージョンの ASP.NET MVC と URL がルーティングを使用する方法を学習します。 IIS 7.0 (クラシック モード)、IIS 6.0、および以前のバージョンの IIS で ASP.NET MVC を使用するためのさまざまな方法を学習します。


ASP.NET MVC フレームワークは、コント ローラーのアクションをブラウザー要求をルーティングする ASP.NET ルーティングに依存します。 ASP.NET ルーティングの利点を活かすため、web サーバーで追加の構成手順を実行する必要があります。 すべては、インターネット インフォメーション サービス (IIS) と、アプリケーションのモードの処理要求のバージョンによって異なります。

別のバージョンの IIS の概要を次に示します。

- IIS 7.0 (統合モード) - ASP.NET のルーティングを使用するために必要な特別な構成がありません。
- IIS 7.0 (クラシック モード) - ASP.NET のルーティングを使用する特別な構成を実行する必要があります。
- IIS 6.0 または以下の ASP.NET のルーティングを使用する特別な構成を実行する必要があります。

最新バージョンの IIS では、(Win7) バージョン 7.5 がします。 IIS の IIS 7 は、Windows Server 2008 および VISTA/SP1 に含まれる以降です。 インストールすることもできる IIS 7.0、Vista オペレーティング システムの Home Basic 以外のすべてのバージョンで (を参照してください[https://technet.microsoft.com/en-us/library/cc731179%28WS.10%29.aspx](https://technet.microsoft.com/en-us/library/cc731179%28WS.10%29.aspx))。

IIS 7.0 では、要求を処理する 2 つのモードをサポートします。 統合モードまたはクラシック モードを使用することができます。 統合モードで IIS 7.0 を使用する場合は、特別な構成手順を実行する必要はありません。 ただしはクラシック モードで IIS 7.0 を使用する場合は、追加の構成を実行する必要があります。

Microsoft Windows Server 2003 には、IIS 6.0 が含まれています。 Windows Server 2003 オペレーティング システムを使用する場合は、IIS 6.0 を IIS 7.0 にアップグレードできません。 IIS 6.0 を使用する場合は、追加の構成手順を実行する必要があります。

Microsoft Windows XP Professional には、IIS 5.1 が含まれています。 IIS 5.1 を使用する場合は、追加の構成手順を実行する必要があります。

最後に、Microsoft Windows 2000 と Microsoft Windows 2000 Professional には、IIS 5.0 が含まれます。 IIS 5.0 を使用する場合は、追加の構成手順を実行する必要があります。

## <a name="integrated-versus-classic-mode"></a>クラシック モードとの統合

IIS 7.0 は、2 つの異なる要求の処理モードを使用して要求を処理できます。 統合とクラシックです。 統合モードでは、パフォーマンスが向上しより多くの機能を提供します。 クラシック モードがの旧バージョンと含まれている IIS の以前のバージョンとの互換性。

要求の処理モードは、アプリケーション プールによって決定されます。 どちらの処理モードは、アプリケーションに関連付けられているアプリケーション プールを決定することにより、特定の web アプリケーションで使用されているを指定できます。 この場合は、以下の手順に従ってください。

1. インターネット インフォメーション サービス マネージャーを起動します。
2. [接続] ウィンドウで、アプリケーションを選択します
3. アクション ウィンドウ、**基本設定**アプリケーションの編集 ダイアログを開くためのリンク (図 1 を参照してください)
4. 選択したアプリケーション プールを書き留めます。

2 つのアプリケーション プールをサポートするために既定では、IIS が構成されている: **DefaultAppPool**と**Classic .NET AppPool**です。 DefaultAppPool が選択されている場合、アプリケーションは、統合された要求の処理モードで実行されています。 Classic .NET AppPool が選択されている場合は、従来の要求の処理モードで、アプリケーションが実行されています。


[![[新しいプロジェクト] ダイアログ ボックス](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image1.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image1.png)

**図 1**: 要求の処理モードの検出 ([フルサイズのイメージを表示するをクリックして](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image2.png))


アプリケーションの編集 ダイアログ ボックス内で要求の処理モードを変更できることに注意してください。 [選択] ボタンをクリックし、アプリケーションに関連付けられているアプリケーション プールを変更します。 ASP.NET アプリケーションをクラシックから統合モードに変更するときに互換性の問題が認識がある注意してください。 詳細については、次の記事を参照してください。

- Windows Vista および Windows Server 2008--上の iis 7.0 ASP.NET 1.1 をアップグレードする[https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008)

- IIS 7.0 の ASP.NET 統合[https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis)


ASP.NET アプリケーションが、DefaultAppPool を使用している場合しない、ASP.NET ルーティングと ASP.NET MVC) 作業を取得する追加の手順を実行する必要があります。 ただし、ASP.NET アプリケーションを構成して、Classic .NET AppPool を使用し、読み取りを保持する場合により多くの作業を行うにを用意します。

## <a name="using-aspnet-mvc-with-older-versions-of-iis"></a>古いバージョンの IIS で ASP.NET MVC の使用

IIS 7.0 より古いバージョンの IIS で ASP.NET MVC を使用する必要。 または、クラシック モードで IIS 7.0 を使用する必要があります、しする 2 つのオプションがあります。 最初に、ファイル拡張子を使用するルート テーブルを変更することができます。 たとえば、/Store/Details ような URL を要求するではなく/Store.aspx/Details ような URL を要求します。

2 番目のオプションと呼ばれるものを作成する、*ワイルドカード スクリプト マップ*です。 ワイルドカード スクリプト マップでは、ASP.NET フレームワークのすべての要求にマップすることができます。

場合は、web サーバー (たとえば、ASP.NET MVC アプリケーションは、インターネット サービス プロバイダーによってホストされている) にアクセスできませんが、最初のオプションを使用する必要があります。 Url の外観を変更したくない、web サーバーにアクセス権がある場合は、2 番目のオプションを使用することができます。

次のセクションで詳しくは、各オプションをについて説明します。

## <a name="adding-extensions-to-the-route-table"></a>ルート テーブルへの拡張機能の追加

古いバージョンの IIS を使用する ASP.NET のルーティングを取得する最も簡単な方法は、Global.asax ファイルで、ルート テーブルを変更するのにです。 既定値をリスト 1 の未変更の Global.asax ファイルが既定のルートをという名前の 1 つのルートを構成します。

**1 - Global.asax (未変更の状態) を一覧表示します。**

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample1.vb)]

リスト 1 で構成されている既定のルートでは、次のようにルート Url できます。

/Home/index

/製品/詳細/3

/製品

残念ながら、古いバージョンの IIS、ASP.NET フレームワークにこれらの要求が転送されません。 したがって、これらの要求、コント ローラーにルーティングされません。 たとえば、URL/Home/インデックスに対してブラウザー要求を行う場合は、図 2 にエラー ページを取得します。


[![[新しいプロジェクト] ダイアログ ボックス](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image2.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image3.png)

**図 2**: 404 Not Found エラーを受け取る ([フルサイズのイメージを表示するをクリックして](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image4.png))


古いバージョンの IIS は、ASP.NET フレームワークにのみ特定の要求をマップします。 要求は、適切なファイル拡張子を持つ URL になければなりません。 たとえば、/SomePage.aspx の要求は、ASP.NET フレームワークにマップを取得します。 ただし、/SomePage.htm の要求されていません。

そのため、ASP.NET が動作するルーティングを取得する、ASP.NET フレームワークにマップされているファイル拡張子が含まれるように既定のルートを変更する必要がありますおです。

これは、という名前のスクリプトを使用して`registermvc.wsf`です。 ASP.NET MVC 1 リリースには含まれて`C:\Program Files\Microsoft ASP.NET\ASP.NET MVC\Scripts`、ASP.NET 2 の時点でこのスクリプトに移動されました ASP.NET フューチャで使用できますが、 [http://aspnet.codeplex.com/releases/view/39978](http://aspnet.codeplex.com/releases/view/39978)です。

このスクリプトを実行すると、新しい .mvc 拡張機能を IIS に登録します。 .Mvc 拡張機能を登録した後は、ルートは、.mvc 拡張機能を使用できるように、Global.asax ファイルで、ルートを変更できます。

リスト 2 での変更の Global.asax ファイルは、古いバージョンの IIS で使用できます。

**2 - Global.asax (拡張子を持つ変更済み) を一覧表示します。**

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample2.vb)]


重要: Global.asax ファイルを変更した後に ASP.NET MVC アプリケーションをビルドしてください。


2 つの重要な変更を一覧表示する 2 の Global.asax ファイルがあります。 Global.asax で定義されている 2 つのルートはようになりました。 既定のルート、最初のルートの URL パターンは、ようになります。

{controller}.mvc/{action}/{id}

.Mvc 拡張機能の追加は、ASP.NET ルーティング モジュールをインターセプトできるファイルの種類を変更します。 この変更によりは、ASP.NET MVC アプリケーションは、次のように要求をルーティングします。

/Home.mvc/Index/

/Product.mvc/Details/3

/Product.mvc/

2 番目のルートでは、ルートのルートは、新しいです。 ルートのルートの次の URL パターンは、空の文字列です。 このルートは、アプリケーションのルートに対する要求に一致する必要があります。 たとえば、ルートのルートでは、次のような要求は一致します。

[http://www.YourApplication.com/](http://www.YourApplication.com/)

ルート テーブルに次の変更を行った後は、すべてのアプリケーションのリンクが、これらの新しい URL パターンと互換性があるかどうかを確認する必要があります。 つまり、すべてのリンクには、.mvc 拡張子を含めることを確認します。 Html.ActionLink() ヘルパー メソッドを使用して、リンクを生成する場合は、何も変更する必要する必要がありますありません。


Registermvc.wcf スクリプトを使用する代わりに、ASP.NET framework を手動でマップされている IIS に新しい拡張機能を追加できます。 新しい拡張機能を自分で追加するとき、チェック ボックスがラベル確認**ファイルが存在することを確認**はチェックされません。


## <a name="hosted-server"></a>ホストされているサーバー

Web サーバーへのアクセスを常がありません。 たとえば、インターネット ホスト プロバイダーを使用して、ASP.NET MVC アプリケーションをホストしている場合、必ずしもはありません IIS へのアクセス。

その場合は、ASP.NET フレームワークにマップされている既存のファイル拡張子のいずれかを使用する必要があります。 ASP.NET にマップされているファイル拡張機能の例には、.aspx には、.axd、.ashx 拡張機能が含まれます。

たとえば、リスト 3 の変更の Global.asax ファイルでは、.mvc 拡張機能ではなく、拡張子が .aspx を使用します。

**3 - Global.asax (.aspx 拡張子を持つ変更済み) を一覧表示します。**

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample3.vb)]

Global.asax ファイルを一覧表示する 3 では、.mvc 拡張機能ではなく、.aspx 拡張機能を使用している点を除いて前の Global.asax ファイルと同じではまったくです。 .Aspx 拡張子を使用して、リモート web サーバーで、セットアップを実行する必要はありません。

## <a name="creating-a-wildcard-script-map"></a>ワイルドカード スクリプト マップを作成します。

ASP.NET MVC アプリケーションの Url を変更しないし、web サーバーにアクセス権があるがある場合、追加のオプションです。 ASP.NET フレームワークに web サーバーにすべての要求をマップするワイルドカード スクリプト マップを作成することができます。 こうすれば、クラシック モードでの IIS 7.0 または IIS 6.0 で既定の ASP.NET MVC ルート テーブルを使用できます。

このオプションは、IIS web サーバーに対して行われたすべての要求をインターセプトすることに注意してください。 これには、イメージ、従来の ASP ページ、および HTML ページの要求が含まれます。 そのため、ワイルドカードを有効にする asp.net スクリプト マップがパフォーマンスに影響します。

IIS 7.0、ワイルドカード スクリプト マップを有効にする方法を次に示します。

1. [接続] ウィンドウで、アプリケーションを選択します。
2. 確認して、**機能**ビューを選択します。
3. ダブルクリックして、**ハンドラー マッピング**ボタン
4. クリックして、**ワイルドカード スクリプト マップの追加**リンク (図 3 を参照してください)
5. Aspnet へのパスを入力\_isapi.dll ファイル (PageHandlerFactory スクリプト マップからこのパスをコピーできます)
6. MVC の名前を入力します。
7. クリックして、 **OK**ボタン


[![[新しいプロジェクト] ダイアログ ボックス](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image3.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image5.png)

**図 3**: IIS 7.0 でワイルドカード スクリプト マップを作成する ([フルサイズのイメージを表示するをクリックして](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image6.png))


IIS 6.0 ではワイルドカード スクリプト マップを作成する手順に従います。

1. Web サイトを右クリックし、[プロパティ]
2. 選択、**ホーム ディレクトリ** タブ
3. クリックして、**構成**ボタン
4. 選択、**マッピング** タブ
5. クリックして、**挿入**ボタン (図 4 を参照)
6. Aspnet へのパスを貼り付け\_(.aspx ファイルのスクリプト マップからこのパスをコピーできます)、実行可能ファイルのフィールドに isapi.dll
7. チェック ボックスをオフに**ファイルの存在を確認してください。**
8. クリックして、 **OK**ボタン


[![[新しいプロジェクト] ダイアログ ボックス](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image4.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image7.png)

**図 4**: IIS 6.0 ではワイルドカード スクリプト マップを作成する ([フルサイズのイメージを表示するをクリックして](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image8.png))


ワイルドカード スクリプト マップを有効にした後は、ルートのルートが含まれるように、Global.asax ファイルでルート テーブルを変更する必要があります。 それ以外の場合、アプリケーションのルート ページに対する要求を行うと図 5 に、エラー ページが表示されます。 リスト 4 変更後の Global.asax ファイルを使用することができます。


[![[新しいプロジェクト] ダイアログ ボックス](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image5.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image9.png)

**図 5**: 見つからないルート ルート エラー ([フルサイズのイメージを表示するをクリックして](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image10.png))


**4 - Global.asax (ルートのルートに変更) を一覧表示します。**

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample4.vb)]

IIS 7.0 または IIS 6.0 のいずれかのワイルドカード スクリプト マップを有効にした後は、次のように既定のルート テーブルを使用する要求を行うことができます。

/

/Home/index

/製品/詳細/3

/製品

## <a name="summary"></a>概要

このチュートリアルの目的は、IIS (またはクラシック モードの IIS 7.0) の古いバージョンを使用する場合、ASP.NET MVC を使用する方法について説明することでした。 古いバージョンの IIS を使用する ASP.NET のルーティングを取得する 2 つの方法を説明した: 既定のルート テーブルを変更またはワイルドカード スクリプト マップを作成します。

最初のオプションでは、ASP.NET MVC アプリケーションで使用される Url を変更する必要があります。 この最初のオプションの非常に重要な利点の 1 つは、ルート テーブルを変更するのには、web サーバーへのアクセスが必要はないことです。 つまり、インターネットと ASP.NET MVC アプリケーションをホストしている場合でも、この最初のオプションを使用できるポータルをホストしています。

2 番目のオプションでは、ワイルドカード スクリプト マップを作成します。 この 2 つ目のオプションの利点は、Url を変更する必要はありません。 この 2 つ目のオプションの欠点は、ASP.NET MVC アプリケーションのパフォーマンスに悪影響です。

>[!div class="step-by-step"]
[前へ](using-asp-net-mvc-with-different-versions-of-iis-cs.md)
