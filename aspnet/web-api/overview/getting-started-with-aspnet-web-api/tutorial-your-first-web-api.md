---
uid: web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
title: "ASP.NET Web API 2 (c#) の概要"
author: MikeWasson
description: "HTTP は、web ページを提供しているだけではなくです。 サービスとデータを公開する Api を構築するための強力なプラットフォームです。 HTTP は、シンプルで柔軟なので、および ubiq には."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/28/2017
ms.topic: article
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
msc.type: authoredcontent
ms.openlocfilehash: fa1cd068a7466e0b6b6fe7716090c8a7afd2a4d5
ms.sourcegitcommit: ec9371e2fbfcb8d62e7e7cae69e7752f3f205385
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2017
---
<a name="getting-started-with-aspnet-web-api-2-c"></a>ASP.NET Web API 2 (c#) の概要
====================
によって[Mike Wasson](https://github.com/MikeWasson)

[完成したプロジェクトをダウンロードします。](https://code.msdn.microsoft.com/Sample-code-of-Getting-c56ccb28)

HTTP は、web ページを提供しているだけではなくです。 HTTP は、サービスとデータを公開する Api を構築するための強力なプラットフォームではもです。 HTTP、シンプルかつ柔軟なので、ユビキタスです。 ほとんどのプラットフォームの考えることができますには、HTTP サービスがクライアント、ブラウザー、モバイル デバイスは、従来のデスクトップ アプリケーションなどの広範な範囲にアクセスできるように HTTP ライブラリがします。
 
ASP.NET Web API は、web Api、.NET Framework 上に構築するためのフレームワークです。 このチュートリアルでは ASP.NET Web API を使用して web API 製品の一覧を表すオブジェクトを作成します。

 
 ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されているソフトウェアのバージョン
  
 - [Visual Studio 2017](https://www.visualstudio.com/downloads/)
 - Web API 2

参照してください[ASP.NET Core と Visual Studio for Windows で web API を作成する](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api)のこのチュートリアルの最新バージョンです。

## <a name="create-a-web-api-project"></a>Web API プロジェクトを作成します。

このチュートリアルでは ASP.NET Web API を使用して web API 製品の一覧を表すオブジェクトを作成します。 フロント エンドの web ページでは、jQuery を使用して、結果を表示します。

![](tutorial-your-first-web-api/_static/image1.png)

Visual Studio を起動し、選択**新しいプロジェクト**から、**開始**ページ。 またはから、**ファイル**メニューの **新規**し**プロジェクト**です。

**テンプレート**ペインで、**インストールされたテンプレート**を展開し、 **Visual c#**ノード。 **Visual c#** **Web**です。 プロジェクト テンプレートの一覧で選択**ASP.NET Web アプリケーション**です。 プロジェクト"ProductsApp"の名前し、をクリックして**OK**です。

![](tutorial-your-first-web-api/_static/image2.png)

**新しい ASP.NET プロジェクト**ダイアログで、選択、**空**テンプレート。 &quot;フォルダーを追加し、参照用のコア&quot;、確認**Web API**です。 **[OK]** をクリックします。

![](tutorial-your-first-web-api/_static/image3.png)

> [!NOTE]
> 使用して Web API プロジェクトを作成することも、 &quot;Web API&quot;テンプレート。 Web API テンプレートでは、ASP.NET MVC を使用して、API のヘルプ ページを提供します。 MVC せず Web API を表示するために、このチュートリアルでは、空のテンプレートを使っています。 一般に、Web API を使用して ASP.NET MVC を知る必要はありません。


## <a name="adding-a-model"></a>モデルを追加します。

*モデル*は、アプリケーションのデータを表すオブジェクトです。 ASP.NET Web API では、JSON、XML、または他のいくつかの形式をモデルを自動的にシリアル化でき、HTTP 応答メッセージの本文にシリアル化されたデータを記述することができます。 クライアントは、シリアル化形式を読み取ることができます、限り、オブジェクトを逆シリアル化できます。 ほとんどのクライアントには、XML または JSON を解析できます。 さらに、クライアントは、HTTP 要求メッセージに Accept ヘッダーを設定して、どの形式を指定できます。

製品を表す単純なモデルの作成を始めます。

ソリューション エクスプ ローラーが表示されない場合にクリックして、**ビュー**メニュー**ソリューション エクスプ ローラー**です。 ソリューション エクスプ ローラーで、Models フォルダーを右クリックします。 コンテキスト メニューから選択**追加**を選択し、**クラス**です。

![](tutorial-your-first-web-api/_static/image4.png)

クラスの名前を付けます&quot;製品&quot;です。 次のプロパティを追加、`Product`クラスです。

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample1.cs)]

## <a name="adding-a-controller"></a>コントローラーを追加する

Web API では、*コント ローラー* HTTP 要求を処理するオブジェクトです。 ID で指定された 1 つの製品または製品の一覧のいずれかを返すことができるコント ローラーを追加します

> [!NOTE]
> ASP.NET MVC を使用している場合、既に精通コント ローラー。 Web API コント ローラーは、MVC コント ローラーに似ていますが、継承、 **ApiController**クラスの代わりに、**コント ローラー**クラスです。

**ソリューション エクスプ ローラー**、コント ローラーのフォルダーを右クリックします。 選択**追加**し、**コント ローラー**です。

![](tutorial-your-first-web-api/_static/image5.png)

**追加 Scaffold**ダイアログで、 **Web API コント ローラー - 空**です。 **[追加]**をクリックします。

![](tutorial-your-first-web-api/_static/image6.png)

**コント ローラーの追加**ダイアログ ボックスで、名前、コント ローラー &quot;ProductsController&quot;です。 **[追加]**をクリックします。

![](tutorial-your-first-web-api/_static/image7.png)

スキャフォールディングでは、コント ローラーのフォルダーに ProductsController.cs をという名前のファイルを作成します。

![](tutorial-your-first-web-api/_static/image8.png)

> [!NOTE]
> コント ローラーをという名前のフォルダーに、コント ローラーを配置する必要はありません。 フォルダー名は、ソース ファイルを整理する便利な手段だけです。


このファイルがまだ開いていないを開くには、ファイルをダブルクリックします。 次のようにこのファイル内のコードに置き換えます。

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample2.cs)]

単純な例から始めてに製品は、コント ローラー クラス内の固定長の配列に格納されます。 もちろん、実際のアプリケーションではデータベースを照会するかを使用して他の外部データ ソース。

コント ローラーには、製品を返す 2 つのメソッドが定義されています。

- `GetAllProducts`メソッドとして製品の全体の一覧を返します、 **IEnumerable&lt;製品&gt;**型です。
- `GetProduct`メソッドは、ID によって 1 つの製品を検索

これで完了です。 作業用の web API があります。 コント ローラーの各メソッドは、1 つまたは複数の Uri に対応しています。

| コント ローラー メソッド | URI |
| --- | --- |
| GetAllProducts | 製品/api |
| GetProduct | 製品が/api/*id* |

`GetProduct` 、メソッド、 *id* URI には、プレース ホルダーです。 たとえば、5 の ID を持つ製品を取得するには、URI は`api/products/5`します。

コント ローラーのメソッドに、Web API が HTTP 要求をルーティングする方法の詳細については、次を参照してください。 [ASP.NET Web API でルーティング](../web-api-routing-and-actions/routing-in-aspnet-web-api.md)です。

## <a name="calling-the-web-api-with-javascript-and-jquery"></a>Javascript と jQuery Web API を呼び出す

このセクションでは、web API を呼び出す AJAX を使用する HTML ページを追加します。 JQuery AJAX 呼び出しを行うと、結果を含むページを更新するも使用されます。

ソリューション エクスプ ローラーでプロジェクトを右クリックし、**追加**選択してから、**新しい項目の**します。

![](tutorial-your-first-web-api/_static/image9.png)

**新しい項目の追加**ダイアログで、選択、 **Web**ノードの下**Visual c#**、クリックして、 **HTML ページ**項目。 ページの名前を付けます&quot;index.html&quot;です。

![](tutorial-your-first-web-api/_static/image10.png)

次のように、このファイル内のすべてを置き換えます。

[!code-html[Main](tutorial-your-first-web-api/samples/sample3.html)]

JQuery を取得するいくつかの方法はあります。 この例では使用して、 [Microsoft Ajax CDN](../../../ajax/cdn/overview.md)です。 ダウンロードすることも[http://jquery.com/](http://jquery.com/)、および"Web API"ASP.NET プロジェクト テンプレートには、jQuery もが含まれています。

### <a name="getting-a-list-of-products"></a>製品の一覧を取得します。

製品の一覧を取得するには、HTTP GET 要求を送信&quot;/api 製品&quot;です。

JQuery [getJSON](http://api.jquery.com/jQuery.getJSON/)関数が AJAX 要求を送信します。 応答には、JSON オブジェクトの配列が含まれています。 `done`関数は、要求が成功した場合に呼び出されるコールバックを指定します。 コールバックでは、製品情報を使用して DOM を更新します。

[!code-html[Main](tutorial-your-first-web-api/samples/sample4.html)]

### <a name="getting-a-product-by-id"></a>ID によって製品を取得します。

ID によって製品を取得するには、送信に HTTP GET 要求を&quot;/api 製品/*id*&quot;ここで、 *id*製品 ID です。

[!code-javascript[Main](tutorial-your-first-web-api/samples/sample5.js)]

まだ呼び`getJSON`AJAX 要求がこの時間を送信する要求 URI で ID を挿入しました。 この要求からの応答は、1 つの製品の JSON 表現です。

## <a name="running-the-application"></a>アプリケーションの実行

F5 キーを押して、アプリケーションのデバッグを開始します。 Web ページは、次のようになります。

![](tutorial-your-first-web-api/_static/image11.png)

ID によって製品を取得し、ID を入力して検索 をクリックします。

![](tutorial-your-first-web-api/_static/image12.png)

無効な ID を入力すると、サーバーは HTTP エラーを返します。

![](tutorial-your-first-web-api/_static/image13.png)

## <a name="using-f12-to-view-the-http-request-and-response"></a>F12 キーを使用して、HTTP 要求と応答を表示するには

HTTP サービスを使用する場合は、HTTP 要求を表示、メッセージを要求する場合に役立ちますができます。 これは、Internet Explorer 9 で F12 開発者ツールを使用して行うことができます。 Internet Explorer 9 を押して、 **F12**を開くには、ツールです。 をクリックして、**ネットワーク**タブとキーを押して**キャプチャ開始**です。 これで、キーを押して、web ページに戻ってください**f5 キーを押して**web ページを更新します。 Internet Explorer では、ブラウザーと web サーバー間の HTTP トラフィックをキャプチャします。 概要ビューは、ページのすべてのネットワーク トラフィックを示しています。

![](tutorial-your-first-web-api/_static/image14.png)

相対 URI のエントリが見つかりません"api 製品//"です。 このエントリを選択し、をクリックして**詳細ビューに移動して**です。 詳細ビューでは、要求と応答ヘッダーと本文を表示するタブがあります。 たとえばをクリックする、**要求ヘッダー**  タブで、クライアントが要求した確認できます&quot;アプリケーション/json&quot; Accept ヘッダー。

![](tutorial-your-first-web-api/_static/image15.png)

応答本文 タブをクリックすると、製品の一覧が JSON にシリアル化される方法が確認できます。 その他のブラウザーでは、同様の機能があります。 他の便利なツールが[Fiddler](http://www.fiddler2.com/fiddler2/)、web デバッグ プロキシです。 使用できる Fiddler、HTTP トラフィックを表示して、HTTP 要求の作成にもため、要求で HTTP ヘッダーを完全に制御を提供します。

## <a name="see-this-app-running-on-azure"></a>Azure で実行されているこのアプリを参照してください。

ライブ web アプリとして実行している完成したサイトを参照してもよろしいですか。 Azure アカウントに、アプリの完全なバージョンを展開するには、次のボタンをクリックするだけです。

[![](http://azuredeploy.net/deploybutton.png)](https://deploy.azure.com/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebAPI-ProductsApp#/form/setup)

Azure アカウントを Azure にこのソリューションを展開する必要があります。 アカウントがない場合は、次のオプションがあります。

- [無料の Azure アカウントを開設](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A443DD604)-クレジットを取得するでも使用されているアカウントを維持する最大と使用する無料の Azure サービスおよび有料の Azure サービスを試す使用できます。
- [MSDN サブスクライバー特典をアクティブ化](https://azure.microsoft.com/en-us/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604)-お客様の MSDN サブスクリプションでは、クレジット有料の Azure サービスを使用できるすべての月です。

## <a name="next-steps"></a>次の手順

- POST、PUT、および DELETE の操作をサポートし、データベースに書き込みます HTTP サービスのより完全な例を参照してください。 [Entity Framework 6 の Web API 2 を使用して](../data/using-web-api-with-entity-framework/part-1.md)です。
- HTTP サービスの上位の流動性および応答 web アプリケーションの作成に関する詳細は、次を参照してください。 [ASP.NET Single Page Application](../../../single-page-application/index.md)です。
- Azure App Service に、Visual Studio web プロジェクトを展開する方法については、次を参照してください。 [Azure App Service で ASP.NET web アプリを作成](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-get-started/)です。
