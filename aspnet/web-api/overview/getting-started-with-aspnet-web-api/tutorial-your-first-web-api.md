---
uid: web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
title: ASP.NET Web API 2 (c#) の概要します。
author: MikeWasson
description: HTTP は web ページを提供するためだけではありません。 サービスとデータを公開する Api を構築するための強力なプラットフォームです。 HTTP は、シンプルで柔軟なおよび ubiq には.
ms.author: riande
ms.date: 11/28/2017
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
msc.type: authoredcontent
ms.openlocfilehash: 62e99a41ba935470c39476c9aea8ee4193543425
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/04/2018
ms.locfileid: "48795294"
---
<a name="get-started-with-aspnet-web-api-2-c"></a>ASP.NET Web API 2 (c#) の概要します。
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

[完成したプロジェクトのダウンロード](https://code.msdn.microsoft.com/Sample-code-of-Getting-c56ccb28)

HTTP は web ページを提供するためだけではありません。 HTTP は、サービスとデータを公開する Api を構築するための強力なプラットフォームではまたです。 HTTP は、シンプルかつ柔軟なユビキタスです。 考えることができ、ほとんどすべてのプラットフォームでは、HTTP ライブラリを持つため、HTTP サービスがクライアント、ブラウザー、モバイル デバイスは、従来のデスクトップ アプリケーションなどの広範な範囲に到達できます。

ASP.NET Web API とは、web Api、.NET Framework 上に構築するためのフレームワークです。 このチュートリアルでは、web を製品の一覧を返す API を作成するのに ASP.NET Web API を使用します。

## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン

- [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
- Web API 2

参照してください[ASP.NET Core と Windows の Visual Studio で web API の作成](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api)のこのチュートリアルの最新バージョン。

## <a name="create-a-web-api-project"></a>Web API プロジェクトを作成します。

このチュートリアルでは、web を製品の一覧を返す API を作成するのに ASP.NET Web API を使用します。 フロント エンドの web ページは、結果を表示するのに jQuery を使用します。

![](tutorial-your-first-web-api/_static/image1.png)

Visual Studio を起動し、選択**新しいプロジェクト**から、**開始**ページ。 またはから、**ファイル**メニューの **新規**し**プロジェクト**します。

**テンプレート**ペインで、**インストールされたテンプレート**を展開し、 **Visual c#** ノード。 **Visual c#**、 **Web**します。 プロジェクト テンプレートの一覧で選択**ASP.NET Web アプリケーション**します。 プロジェクトに"ProductsApp"という名前にして**OK**します。

![](tutorial-your-first-web-api/_static/image2.png)

**新しい ASP.NET プロジェクト**ダイアログ ボックスで、**空**テンプレート。 &quot;フォルダーを追加し、参照用のコア&quot;、確認**Web API**します。 **[OK]** をクリックします。

![](tutorial-your-first-web-api/_static/image3.png)

> [!NOTE]
> 使用して Web API プロジェクトを作成することも、 &quot;Web API&quot;テンプレート。 Web API テンプレートでは、ASP.NET MVC を使用して、API のヘルプ ページを提供します。 MVC せず Web API を表示する必要があるために、このチュートリアルでは、空のテンプレートを使っています。 一般に、Web API を使用して ASP.NET MVC を把握する必要はありません。


## <a name="adding-a-model"></a>モデルの追加

*モデル*は、アプリケーションのデータを表すオブジェクトです。 ASP.NET Web API では、JSON、XML、またはその他のいくつかの形式にモデルを自動的にシリアル化でき、HTTP 応答メッセージの本文にシリアル化されたデータを記述することができます。 クライアントでは、シリアル化形式を読み取り、限り、オブジェクトを逆シリアル化できます。 ほとんどのクライアントには、XML または JSON を解析できます。 さらに、クライアントは、HTTP 要求メッセージに Accept ヘッダーを設定して、どの形式を指定できます。

製品を表す単純なモデルを作成してみましょう。

ソリューション エクスプ ローラーが表示されない場合は、クリックして、**ビュー**メニュー選択し、**ソリューション エクスプ ローラー**します。 ソリューション エクスプ ローラーで、Models フォルダーを右クリックします。 コンテキスト メニューでは、次のように選択します。**追加**選び**クラス**します。

![](tutorial-your-first-web-api/_static/image4.png)

クラスの名前&quot;製品&quot;します。 次のプロパティを追加、`Product`クラス。

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample1.cs)]

## <a name="adding-a-controller"></a>コントローラーを追加する

Web api で、*コント ローラー*は HTTP 要求を処理するオブジェクトです。 ID で指定された 1 つの製品または製品の一覧のいずれかを返すことができるコント ローラーを追加します

> [!NOTE]
> ASP.NET MVC を使用している場合を理解しているコント ローラー。 Web API コント ローラーは MVC コント ローラーに似ていますが、継承、 **ApiController**クラスの代わりに、**コント ローラー**クラス。

**ソリューション エクスプ ローラー**、Controllers フォルダーを右クリックします。 選択**追加**選び**コント ローラー**します。

![](tutorial-your-first-web-api/_static/image5.png)

**スキャフォールディングの追加**ダイアログ ボックスで、 **Web API コント ローラー - 空**します。 **[追加]** をクリックします。

![](tutorial-your-first-web-api/_static/image6.png)

**コント ローラーの追加**ダイアログ ボックスで、名前、コント ローラー &quot;ProductsController&quot;します。 **[追加]** をクリックします。

![](tutorial-your-first-web-api/_static/image7.png)

スキャフォールディングは、Controllers フォルダーで ProductsController.cs という名前のファイルを作成します。

![](tutorial-your-first-web-api/_static/image8.png)

> [!NOTE]
> コント ローラーをという名前のフォルダーに、コント ローラーを配置する必要はありません。 フォルダー名が、ソース ファイルを整理する便利な方法だけです。


このファイルがまだ開いていない場合、は、開くファイルをダブルクリックします。 次のようにこのファイル内のコードに置き換えます。

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample2.cs)]

例を簡潔にするには、製品は、コント ローラー クラス内で固定長の配列に格納されます。 もちろん、実際のアプリケーションには、データベースの照会または他の外部データ ソースを使用するは。

コント ローラーには、製品を返す 2 つのメソッドを定義します。

- `GetAllProducts`メソッド全体として製品のリストを返します、 **IEnumerable&lt;製品&gt;** 型。
- `GetProduct`メソッドは、ID によって 1 つの製品を検索

これで完了です。 作業用の web API があります。 コント ローラーの各メソッドは、1 つまたは複数の Uri に対応します。

| コント ローラー メソッド | URI |
| --- | --- |
| GetAllProducts | /api/products |
| GetProduct | /api/products/*id* |

`GetProduct`メソッド、 *id* URI にプレース ホルダーです。 たとえば、id が 5 の製品を取得する URI は`api/products/5`します。

Web API がコント ローラーのメソッドに HTTP 要求をルーティングする方法の詳細については、次を参照してください。 [ASP.NET Web API におけるルーティング](../web-api-routing-and-actions/routing-in-aspnet-web-api.md)します。

## <a name="calling-the-web-api-with-javascript-and-jquery"></a>Javascript や jQuery による Web API を呼び出す

このセクションでは、web API を呼び出す AJAX を使用する HTML ページを追加します。 AJAX 呼び出しを実行して結果で、ページを更新することも、jQuery を使用します。

ソリューション エクスプ ローラーでプロジェクトを右クリックし、選択**追加**を選択し、**新しい項目の**します。

![](tutorial-your-first-web-api/_static/image9.png)

**新しい項目の追加**ダイアログ ボックスで、 **Web**ノードの下**Visual c#** を選び、 **HTML ページ**項目。 ページの名前を付けます&quot;index.html&quot;します。

![](tutorial-your-first-web-api/_static/image10.png)

次のように、このファイル内のすべてを置き換えます。

[!code-html[Main](tutorial-your-first-web-api/samples/sample3.html)]

jQuery を取得するには、いくつかの方法があります。 この例では使用して、 [Microsoft Ajax CDN](../../../ajax/cdn/overview.md)します。 ダウンロードすることも[ http://jquery.com/ ](http://jquery.com/)、および"Web API"の ASP.NET プロジェクト テンプレートが jQuery にも含まれています。

### <a name="getting-a-list-of-products"></a>製品の一覧を取得します。

製品の一覧を取得する HTTP GET 要求を送信&quot;api/製品&quot;します。

JQuery [getJSON](http://api.jquery.com/jQuery.getJSON/)関数は、AJAX 要求を送信します。 応答には、JSON オブジェクトの配列が含まれています。 `done`関数は、要求が成功した場合に呼び出されるコールバックを指定します。 コールバックでは、製品情報、DOM を更新します。

[!code-html[Main](tutorial-your-first-web-api/samples/sample4.html)]

### <a name="getting-a-product-by-id"></a>ID によって製品を取得します。

ID によって製品を取得する HTTP GET 要求を送信&quot;/api/製品/*id*&quot;ここで、 *id*は製品 ID です。

[!code-javascript[Main](tutorial-your-first-web-api/samples/sample5.js)]

まだ呼んで`getJSON`この時間が、AJAX 要求を送信する要求 URI に ID を格納します。 この要求からの応答は、1 つの製品の JSON 表現です。

## <a name="running-the-application"></a>アプリケーションの実行

F5 キーを押して、アプリケーションのデバッグを開始します。 Web ページは、次のようになります。

![](tutorial-your-first-web-api/_static/image11.png)

ID によって製品を取得するには、ID を入力し、検索 をクリックします。

![](tutorial-your-first-web-api/_static/image12.png)

無効な ID を入力すると、サーバーは HTTP エラーを返します。

![](tutorial-your-first-web-api/_static/image13.png)

## <a name="using-f12-to-view-the-http-request-and-response"></a>F12 キーを使用して、HTTP 要求と応答を表示するには

HTTP サービスを使用する場合は、HTTP 要求を表示、メッセージを要求する非常に便利ですができます。 Internet Explorer 9 で F12 開発者ツールを使用して、これを行うことができます。 Internet Explorer 9 からキーを押します。 **F12**ツールを開きます。 をクリックして、**ネットワーク**タブ キーを押します**キャプチャ開始**します。 キーを押して、web ページに移動**F5** web ページを更新します。 Internet Explorer では、ブラウザーと web サーバー間の HTTP トラフィックをキャプチャします。 概要ビューは、ページのすべてのネットワーク トラフィックを示しています。

![](tutorial-your-first-web-api/_static/image14.png)

相対 URI のエントリを見つけ"api/製品/"。 このエントリを選択し、クリックして**詳細ビューに移動**します。 詳細ビューでは、要求と応答ヘッダーと本文を表示するタブがあります。 たとえばをクリックする、**要求ヘッダー**  タブで、クライアントが要求された参照できます&quot;、application/json&quot; Accept ヘッダー。

![](tutorial-your-first-web-api/_static/image15.png)

応答の本文 タブをクリックすると、製品一覧が JSON にシリアル化する方法がわかります。 その他のブラウザーでは、同様の機能があります。 もう 1 つの便利なツールが[Fiddler](http://www.fiddler2.com/fiddler2/)の web デバッグ プロキシです。 使用できます Fiddler、HTTP トラフィックを表示して、HTTP 要求を作成することも、要求で HTTP ヘッダーを完全に制御を提供します。

## <a name="see-this-app-running-on-azure"></a>Azure で実行されているこのアプリを参照してください。

ライブ web アプリとして実行されている完成したサイトを参照してもよろしいですか。 Azure アカウントに、アプリの完全なバージョンを展開するには、次のボタンをクリックするだけです。

[![](http://azuredeploy.net/deploybutton.png)](https://deploy.azure.com/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebAPI-ProductsApp#/form/setup)

このソリューションを Azure にデプロイする Azure アカウントが必要です。 アカウントがいない場合は、次のオプションがあります。

- [無料 Azure アカウントを開く](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604)-クレジットを取得する有料の Azure サービスを使用することができますをも使用されるアカウントは維持する最大無料の Azure サービスを使用します。
- [MSDN サブスクライバーの特典をアクティブ化](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604)-MSDN サブスクリプションでは、クレジットの有料の Azure サービスを使用することができる毎月。

## <a name="next-steps"></a>次の手順

- POST、PUT、および DELETE 操作をサポートし、データベースに書き込みます HTTP サービスのより完全な例を参照してください。 [Entity Framework 6 で Web API 2 を使用して](../data/using-web-api-with-entity-framework/part-1.md)します。
- HTTP サービス上で流動性と応答性の高い web アプリケーションを作成する方法の詳細は、次を参照してください。 [ASP.NET Single Page Application](../../../single-page-application/index.md)します。
- Visual Studio web プロジェクトを Azure App Service にデプロイする方法については、次を参照してください。 [Azure App Service で ASP.NET web アプリを作成](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)です。
