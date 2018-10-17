---
uid: web-api/overview/security/enabling-cross-origin-requests-in-web-api
title: ASP.NET Web API 2 でクロスオリジン要求の有効化 |Microsoft Docs
author: MikeWasson
description: ASP.NET Web API でクロス オリジン リソース共有 (CORS) をサポートする方法を示しています。
ms.author: riande
ms.date: 10/10/2018
ms.assetid: 9b265a5a-6a70-4a82-adce-2d7c56ae8bdd
msc.legacyurl: /web-api/overview/security/enabling-cross-origin-requests-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 118b779c89edb874f7f928315d1094738be5f097
ms.sourcegitcommit: 6e6002de467cd135a69e5518d4ba9422d693132a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/16/2018
ms.locfileid: "49348521"
---
<a name="enable-cross-origin-requests-in-aspnet-web-api-2"></a><span data-ttu-id="952fb-103">ASP.NET Web API 2 でのクロス オリジン要求を有効にします。</span><span class="sxs-lookup"><span data-stu-id="952fb-103">Enable cross-origin requests in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="952fb-104">作成者[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="952fb-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="952fb-105">ブラウザーのセキュリティは、Web ページが別のドメインに AJAX 要求を行うことを防止します。</span><span class="sxs-lookup"><span data-stu-id="952fb-105">Browser security prevents a web page from making AJAX requests to another domain.</span></span> <span data-ttu-id="952fb-106">この制限は*同一生成元ポリシー*と呼ばれ、悪意のあるサイトが別のサイトから機密データを読み取れないようにします。</span><span class="sxs-lookup"><span data-stu-id="952fb-106">This restriction is called the *same-origin policy*, and prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="952fb-107">ただし、場合がありますたい他のサイト、web API の呼び出しを使用します。</span><span class="sxs-lookup"><span data-stu-id="952fb-107">However, sometimes you might want to let other sites call your web API.</span></span>
>
> <span data-ttu-id="952fb-108">[クロス オリジン リソース共有](http://www.w3.org/TR/cors/)(CORS) は、サーバーに同一生成元ポリシーの制限を緩和させる W3C 標準の１つです。</span><span class="sxs-lookup"><span data-stu-id="952fb-108">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="952fb-109">CORS を使用することによって、不明なリクエストは拒否しながら、一部のクロス オリジン要求のみを明示的に許可できるようになります。</span><span class="sxs-lookup"><span data-stu-id="952fb-109">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="952fb-110">CORS は [JSONP](http://en.wikipedia.org/wiki/JSONP) のようなかつての技術より安全でフレキシブルなものです。</span><span class="sxs-lookup"><span data-stu-id="952fb-110">CORS is safer and more flexible than earlier techniques such as [JSONP](http://en.wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="952fb-111">このチュートリアルでは、Web API アプリケーションで CORS を有効にする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="952fb-111">This tutorial shows how to enable CORS in your Web API application.</span></span>
>
> ## <a name="software-used-in-the-tutorial"></a><span data-ttu-id="952fb-112">このチュートリアルで使用されるソフトウェア</span><span class="sxs-lookup"><span data-stu-id="952fb-112">Software used in the tutorial</span></span>
>
> - [<span data-ttu-id="952fb-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="952fb-113">Visual Studio</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - <span data-ttu-id="952fb-114">Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="952fb-114">Web API 2.2</span></span>

## <a name="introduction"></a><span data-ttu-id="952fb-115">はじめに</span><span class="sxs-lookup"><span data-stu-id="952fb-115">Introduction</span></span>

<span data-ttu-id="952fb-116">このチュートリアルでは、ASP.NET Web API における CORS のサポートについて説明します。</span><span class="sxs-lookup"><span data-stu-id="952fb-116">This tutorial demonstrates CORS support in ASP.NET Web API.</span></span> <span data-ttu-id="952fb-117">1 つと呼ばれる"WebService"、Web API コント ローラーをホストして、その他の呼び出された"WebClient"、web サービスを呼び出す – 2 つの ASP.NET プロジェクトの作成から始めます。</span><span class="sxs-lookup"><span data-stu-id="952fb-117">We'll start by creating two ASP.NET projects – one called "WebService", which hosts a Web API controller, and the other called "WebClient", which calls WebService.</span></span> <span data-ttu-id="952fb-118">2 つのアプリケーションが別のドメインでホストされているため、WebClient から web サービスへの AJAX 要求は、クロス オリジン要求です。</span><span class="sxs-lookup"><span data-stu-id="952fb-118">Because the two applications are hosted at different domains, an AJAX request from WebClient to WebService is a cross-origin request.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image1.png)

### <a name="what-is-same-origin"></a><span data-ttu-id="952fb-119">「同一生成元」とは</span><span class="sxs-lookup"><span data-stu-id="952fb-119">What is "same origin"?</span></span>

<span data-ttu-id="952fb-120">2 つの URL のスキーム、ホスト、ポートが同じである場合、その URL は同一生成元となります。</span><span class="sxs-lookup"><span data-stu-id="952fb-120">Two URLs have the same origin if they have identical schemes, hosts, and ports.</span></span> <span data-ttu-id="952fb-121">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span><span class="sxs-lookup"><span data-stu-id="952fb-121">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span></span>

<span data-ttu-id="952fb-122">次の 2 つの URL は生成元が同じです。</span><span class="sxs-lookup"><span data-stu-id="952fb-122">These two URLs have the same origin:</span></span>

- `http://example.com/foo.html`
- `http://example.com/bar.html`

<span data-ttu-id="952fb-123">次の URL は、上の URL とは生成元が異なります。</span><span class="sxs-lookup"><span data-stu-id="952fb-123">These URLs have different origins than the previous two:</span></span>

- <span data-ttu-id="952fb-124">`http://example.net` - 異なるドメイン </span><span class="sxs-lookup"><span data-stu-id="952fb-124">`http://example.net` - Different domain</span></span>
- <span data-ttu-id="952fb-125">`http://example.com:9000/foo.html` - 異なるポート</span><span class="sxs-lookup"><span data-stu-id="952fb-125">`http://example.com:9000/foo.html` - Different port</span></span>
- <span data-ttu-id="952fb-126">`https://example.com/foo.html` - 異なるスキーム</span><span class="sxs-lookup"><span data-stu-id="952fb-126">`https://example.com/foo.html` - Different scheme</span></span>
- <span data-ttu-id="952fb-127">`http://www.example.com/foo.html` - 異なるサブドメイン</span><span class="sxs-lookup"><span data-stu-id="952fb-127">`http://www.example.com/foo.html` - Different subdomain</span></span>

> [!NOTE]
> <span data-ttu-id="952fb-128">Internet Explorer では、配信元を比較するときに、ポートは考慮されません。</span><span class="sxs-lookup"><span data-stu-id="952fb-128">Internet Explorer does not consider the port when comparing origins.</span></span>

## <a name="create-the-webservice-project"></a><span data-ttu-id="952fb-129">Web サービス プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="952fb-129">Create the WebService project</span></span>

> [!NOTE]
> <span data-ttu-id="952fb-130">このセクションでは、Web API プロジェクトを作成する方法を知ってを前提としています。</span><span class="sxs-lookup"><span data-stu-id="952fb-130">This section assumes you already know how to create Web API projects.</span></span> <span data-ttu-id="952fb-131">そうでない場合は、次を参照してください。 [ASP.NET Web API の概要](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)します。</span><span class="sxs-lookup"><span data-stu-id="952fb-131">If not, see [Getting Started with ASP.NET Web API](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>

1. <span data-ttu-id="952fb-132">Visual Studio を起動し、新しい作成**ASP.NET Web アプリケーション (.NET Framework)** プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="952fb-132">Start Visual Studio and create a new **ASP.NET Web Application (.NET Framework)** project.</span></span>
2. <span data-ttu-id="952fb-133">**新しい ASP.NET Web アプリケーション**ダイアログ ボックスで、**空**プロジェクト テンプレート。</span><span class="sxs-lookup"><span data-stu-id="952fb-133">In the **New ASP.NET Web Application** dialog box, select the **Empty** project template.</span></span> <span data-ttu-id="952fb-134">**フォルダーを追加し、参照用のコア**を選択、 **Web API**チェック ボックスをオンします。</span><span class="sxs-lookup"><span data-stu-id="952fb-134">Under **Add folders and core references for**, select the **Web API** checkbox.</span></span>

   ![Visual Studio で新しい ASP.NET プロジェクト ダイアログ](enabling-cross-origin-requests-in-web-api/_static/new-web-app-dialog.png)

3. <span data-ttu-id="952fb-136">という名前の Web API コント ローラーを追加`TestController`を次のコード。</span><span class="sxs-lookup"><span data-stu-id="952fb-136">Add a Web API controller named `TestController` with the following code:</span></span>

   [!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample1.cs)]

4. <span data-ttu-id="952fb-137">ローカル アプリケーションを実行したり、Azure にデプロイできます。</span><span class="sxs-lookup"><span data-stu-id="952fb-137">You can run the application locally or deploy to Azure.</span></span> <span data-ttu-id="952fb-138">(このチュートリアルではスクリーン ショットは、アプリにデプロイされます Azure App Service Web Apps。)Web API が動作していることを確認するに移動します。`http://hostname/api/test/`ここで、*ホスト名*はアプリケーションをデプロイしたドメインです。</span><span class="sxs-lookup"><span data-stu-id="952fb-138">(For the screenshots in this tutorial, the app deploys to Azure App Service Web Apps.) To verify that the web API is working, navigate to `http://hostname/api/test/`, where *hostname* is the domain where you deployed the application.</span></span> <span data-ttu-id="952fb-139">応答テキスト、&quot;取得: テスト メッセージ&quot;します。</span><span class="sxs-lookup"><span data-stu-id="952fb-139">You should see the response text, &quot;GET: Test Message&quot;.</span></span>

   ![Web ブラウザーが表示されたテスト メッセージ](enabling-cross-origin-requests-in-web-api/_static/image4.png)

## <a name="create-the-webclient-project"></a><span data-ttu-id="952fb-141">WebClient のプロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="952fb-141">Create the WebClient project</span></span>

1. <span data-ttu-id="952fb-142">別の作成**ASP.NET Web アプリケーション (.NET Framework)** 順に選択して、 **MVC**プロジェクト テンプレート。</span><span class="sxs-lookup"><span data-stu-id="952fb-142">Create another **ASP.NET Web Application (.NET Framework)** project and select the **MVC** project template.</span></span> <span data-ttu-id="952fb-143">必要に応じて、**認証の変更** > **認証なし**します。</span><span class="sxs-lookup"><span data-stu-id="952fb-143">Optionally, select **Change Authentication** > **No Authentication**.</span></span> <span data-ttu-id="952fb-144">このチュートリアルでは、認証が不要です。</span><span class="sxs-lookup"><span data-stu-id="952fb-144">You don't need authentication for this tutorial.</span></span>

   ![Visual Studio で新しい ASP.NET プロジェクト ダイアログ ボックスで MVC テンプレート](enabling-cross-origin-requests-in-web-api/_static/new-web-app-dialog-mvc.png)

2. <span data-ttu-id="952fb-146">**ソリューション エクスプ ローラー**、ファイルを開く*Views/Home/Index.cshtml*します。</span><span class="sxs-lookup"><span data-stu-id="952fb-146">In **Solution Explorer**, open the file *Views/Home/Index.cshtml*.</span></span> <span data-ttu-id="952fb-147">次のようにこのファイル内のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="952fb-147">Replace the code in this file with the following:</span></span>

   [!code-cshtml[Main](enabling-cross-origin-requests-in-web-api/samples/sample2.cshtml?highlight=13)]

   <span data-ttu-id="952fb-148">*ServiceUrl*変数、web サービス アプリケーションの URI を使用します。</span><span class="sxs-lookup"><span data-stu-id="952fb-148">For the *serviceUrl* variable, use the URI of the WebService app.</span></span>

3. <span data-ttu-id="952fb-149">WebClient のアプリをローカルで実行または別の web サイトに発行します。</span><span class="sxs-lookup"><span data-stu-id="952fb-149">Run the WebClient app locally or publish it to another website.</span></span>

<span data-ttu-id="952fb-150">表示されている HTTP メソッドを使用して web サービス アプリケーションに AJAX 要求が送信される、試してみる ボタンをクリックすると (GET、POST、または PUT) ボックスの一覧。</span><span class="sxs-lookup"><span data-stu-id="952fb-150">When you click the "Try It" button, an AJAX request is submitted to the WebService app using the HTTP method listed in the dropdown box (GET, POST, or PUT).</span></span> <span data-ttu-id="952fb-151">これにより、別のクロス オリジン要求を調べることができます。</span><span class="sxs-lookup"><span data-stu-id="952fb-151">This lets you examine different cross-origin requests.</span></span> <span data-ttu-id="952fb-152">現時点では、ボタンをクリックした場合、エラーが表示されますので、web サービス アプリには、CORS はできません。</span><span class="sxs-lookup"><span data-stu-id="952fb-152">Currently, the WebService app does not support CORS, so if you click the button you'll get an error.</span></span>

![ブラウザーで '試して' エラー](enabling-cross-origin-requests-in-web-api/_static/image7.png)

> [!NOTE]
> <span data-ttu-id="952fb-154">などのツールで HTTP トラフィックを見る場合[Fiddler](http://www.telerik.com/fiddler)ブラウザーは、GET 要求を送信し、要求が成功するしますが、AJAX 呼び出しには、エラーが返されますことを確認します。</span><span class="sxs-lookup"><span data-stu-id="952fb-154">If you watch the HTTP traffic in a tool like [Fiddler](http://www.telerik.com/fiddler), you'll see that the browser does send the GET request, and the request succeeds, but the AJAX call returns an error.</span></span> <span data-ttu-id="952fb-155">同一オリジン ポリシーが、ブラウザーからできないことを理解することが重要*送信*要求。</span><span class="sxs-lookup"><span data-stu-id="952fb-155">It's important to understand that same-origin policy does not prevent the browser from *sending* the request.</span></span> <span data-ttu-id="952fb-156">代わりに、アプリケーションが表示されるを防止、*応答*します。</span><span class="sxs-lookup"><span data-stu-id="952fb-156">Instead, it prevents the application from seeing the *response*.</span></span>

![Fiddler web デバッガーが web 要求を表示](enabling-cross-origin-requests-in-web-api/_static/image8.png)

## <a name="enable-cors"></a><span data-ttu-id="952fb-158">CORS を有効にします。</span><span class="sxs-lookup"><span data-stu-id="952fb-158">Enable CORS</span></span>

<span data-ttu-id="952fb-159">これで web サービス アプリで CORS を有効にしてみましょう。</span><span class="sxs-lookup"><span data-stu-id="952fb-159">Now let's enable CORS in the WebService app.</span></span> <span data-ttu-id="952fb-160">最初に、CORS の NuGet パッケージを追加します。</span><span class="sxs-lookup"><span data-stu-id="952fb-160">First, add the CORS NuGet package.</span></span> <span data-ttu-id="952fb-161">Visual Studio から、**ツール**メニューの  **NuGet パッケージ マネージャー**を選択し、**パッケージ マネージャー コンソール**します。</span><span class="sxs-lookup"><span data-stu-id="952fb-161">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="952fb-162">パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="952fb-162">In the Package Manager Console window, type the following command:</span></span>

[!code-powershell[Main](enabling-cross-origin-requests-in-web-api/samples/sample3.ps1)]

<span data-ttu-id="952fb-163">このコマンドは、最新のパッケージをインストールし、core Web API のライブラリを含む、すべての依存関係を更新します。</span><span class="sxs-lookup"><span data-stu-id="952fb-163">This command installs the latest package and updates all dependencies, including the core Web API libraries.</span></span> <span data-ttu-id="952fb-164">使用して、`-Version`特定のバージョンを対象とするフラグ。</span><span class="sxs-lookup"><span data-stu-id="952fb-164">Use the `-Version` flag to target a specific version.</span></span> <span data-ttu-id="952fb-165">CORS のパッケージには、Web API 2.0 以降が必要です。</span><span class="sxs-lookup"><span data-stu-id="952fb-165">The CORS package requires Web API 2.0 or later.</span></span>

<span data-ttu-id="952fb-166">ファイルを開く*アプリ\_Start/WebApiConfig.cs*します。</span><span class="sxs-lookup"><span data-stu-id="952fb-166">Open the file *App\_Start/WebApiConfig.cs*.</span></span> <span data-ttu-id="952fb-167">次のコードを追加、 **WebApiConfig.Register**メソッド。</span><span class="sxs-lookup"><span data-stu-id="952fb-167">Add the following code to the **WebApiConfig.Register** method:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample4.cs?highlight=9)]

<span data-ttu-id="952fb-168">次に、追加、 **EnableCors**属性を`TestController`クラス。</span><span class="sxs-lookup"><span data-stu-id="952fb-168">Next, add the **[EnableCors]** attribute to the `TestController` class:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample5.cs?highlight=3,7)]

<span data-ttu-id="952fb-169">*オリジン*パラメーター、WebClient のアプリケーションをデプロイした URI を使用します。</span><span class="sxs-lookup"><span data-stu-id="952fb-169">For the *origins* parameter, use the URI where you deployed the WebClient application.</span></span> <span data-ttu-id="952fb-170">これにより、WebClient から引き続き他のすべてのクロス ドメイン要求を許可中に、クロス オリジン要求ができます。</span><span class="sxs-lookup"><span data-stu-id="952fb-170">This allows cross-origin requests from WebClient, while still disallowing all other cross-domain requests.</span></span> <span data-ttu-id="952fb-171">パラメーターを後で説明します**EnableCors**で詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="952fb-171">Later, I'll describe the parameters for **[EnableCors]** in more detail.</span></span>

<span data-ttu-id="952fb-172">末尾にスラッシュを含めないでください、*オリジン*URL。</span><span class="sxs-lookup"><span data-stu-id="952fb-172">Do not include a forward slash at the end of the *origins* URL.</span></span>

<span data-ttu-id="952fb-173">更新された web サービス アプリケーションを再デプロイします。</span><span class="sxs-lookup"><span data-stu-id="952fb-173">Redeploy the updated WebService application.</span></span> <span data-ttu-id="952fb-174">WebClient を更新する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="952fb-174">You don't need to update WebClient.</span></span> <span data-ttu-id="952fb-175">今すぐ WebClient から AJAX 要求は成功する必要があります。</span><span class="sxs-lookup"><span data-stu-id="952fb-175">Now the AJAX request from WebClient should succeed.</span></span> <span data-ttu-id="952fb-176">GET、PUT、POST メソッドはすべて許可します。</span><span class="sxs-lookup"><span data-stu-id="952fb-176">The GET, PUT, and POST methods are all allowed.</span></span>

![Web ブラウザーが表示されたテストが成功したメッセージ](enabling-cross-origin-requests-in-web-api/_static/image9.png)

## <a name="how-cors-works"></a><span data-ttu-id="952fb-178">CORS のしくみ</span><span class="sxs-lookup"><span data-stu-id="952fb-178">How CORS Works</span></span>

<span data-ttu-id="952fb-179">このセクションでは、HTTP メッセージのレベルでの CORS 要求での動作について説明します。</span><span class="sxs-lookup"><span data-stu-id="952fb-179">This section describes what happens in a CORS request, at the level of the HTTP messages.</span></span> <span data-ttu-id="952fb-180">構成できるようにするために、CORS のしくみを理解することが重要、 **EnableCors**正しく属性し、期待どおりに動作しなかった場合のトラブルシューティングを行います。</span><span class="sxs-lookup"><span data-stu-id="952fb-180">It's important to understand how CORS works, so that you can configure the **[EnableCors]** attribute correctly and troubleshoot if things don't work as you expect.</span></span>

<span data-ttu-id="952fb-181">CORS の仕様には、クロス オリジン要求を有効にするいくつかの新しい HTTP ヘッダーが導入されています。</span><span class="sxs-lookup"><span data-stu-id="952fb-181">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="952fb-182">ブラウザーでは、CORS をサポートする場合はクロス オリジン要求を自動的にこれらのヘッダーに設定します。JavaScript コードで特別な処理は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="952fb-182">If a browser supports CORS, it sets these headers automatically for cross-origin requests; you don't need to do anything special in your JavaScript code.</span></span>

<span data-ttu-id="952fb-183">クロス オリジン要求の例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="952fb-183">Here is an example of a cross-origin request.</span></span> <span data-ttu-id="952fb-184">"Origin"ヘッダーは、要求を行っているサイトのドメインを示します。</span><span class="sxs-lookup"><span data-stu-id="952fb-184">The "Origin" header gives the domain of the site that is making the request.</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample6.cmd?highlight=5)]

<span data-ttu-id="952fb-185">サーバーは、要求を許可している場合は、アクセス制御の許可-オリジン ヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="952fb-185">If the server allows the request, it sets the Access-Control-Allow-Origin header.</span></span> <span data-ttu-id="952fb-186">このヘッダーの値は配信元のヘッダーと一致するか、ワイルドカード値は、"\*"、任意のオリジンを許可することを意味します。</span><span class="sxs-lookup"><span data-stu-id="952fb-186">The value of this header either matches the Origin header, or is the wildcard value "\*", meaning that any origin is allowed.</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample7.cmd?highlight=5)]

<span data-ttu-id="952fb-187">応答に、アクセス制御の許可-オリジン ヘッダーが含まれていない場合、AJAX 要求は失敗します。</span><span class="sxs-lookup"><span data-stu-id="952fb-187">If the response does not include the Access-Control-Allow-Origin header, the AJAX request fails.</span></span> <span data-ttu-id="952fb-188">具体的には、ブラウザーには、要求が許可されていません。</span><span class="sxs-lookup"><span data-stu-id="952fb-188">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="952fb-189">サーバーに正常な応答が返される場合でも、ブラウザーは行いません応答クライアント アプリケーションで使用できます。</span><span class="sxs-lookup"><span data-stu-id="952fb-189">Even if the server returns a successful response, the browser does not make the response available to the client application.</span></span>

<span data-ttu-id="952fb-190">**プレフライト要求**</span><span class="sxs-lookup"><span data-stu-id="952fb-190">**Preflight Requests**</span></span>

<span data-ttu-id="952fb-191">一部の CORS 要求では、ブラウザーは、リソースの実際の要求を送信する前に「プレフライト要求を」と呼ばれる追加の要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="952fb-191">For some CORS requests, the browser sends an additional request, called a "preflight request", before it sends the actual request for the resource.</span></span>

<span data-ttu-id="952fb-192">次の条件に該当する場合、ブラウザーでプレフライト要求をスキップできます。</span><span class="sxs-lookup"><span data-stu-id="952fb-192">The browser can skip the preflight request if the following conditions are true:</span></span>

- <span data-ttu-id="952fb-193">要求メソッドが GET、HEAD、または POST、*と*</span><span class="sxs-lookup"><span data-stu-id="952fb-193">The request method is GET, HEAD, or POST, *and*</span></span>
- <span data-ttu-id="952fb-194">アプリケーションが承諾、Accept-language、Content-language 以外のすべての要求ヘッダーを設定していないコンテンツの種類、または最後のイベント ID、*と*</span><span class="sxs-lookup"><span data-stu-id="952fb-194">The application does not set any request headers other than Accept, Accept-Language, Content-Language, Content-Type, or Last-Event-ID, *and*</span></span>
- <span data-ttu-id="952fb-195">Content-type ヘッダー (場合設定) は、次の 1 つです。</span><span class="sxs-lookup"><span data-stu-id="952fb-195">The Content-Type header (if set) is one of the following:</span></span>

    - <span data-ttu-id="952fb-196">application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="952fb-196">application/x-www-form-urlencoded</span></span>
    - <span data-ttu-id="952fb-197">マルチパート/フォーム データ</span><span class="sxs-lookup"><span data-stu-id="952fb-197">multipart/form-data</span></span>
    - <span data-ttu-id="952fb-198">テキスト/プレーン</span><span class="sxs-lookup"><span data-stu-id="952fb-198">text/plain</span></span>

<span data-ttu-id="952fb-199">アプリケーションで呼び出すことによって設定されたヘッダーを要求ヘッダーについて規則が適用される**setRequestHeader**上、 **XMLHttpRequest**オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="952fb-199">The rule about request headers applies to headers that the application sets by calling **setRequestHeader** on the **XMLHttpRequest** object.</span></span> <span data-ttu-id="952fb-200">(CORS の仕様は、これら「作成者要求ヘッダー」を呼び出します)。このルールは、ヘッダーには適用されません、*ブラウザー*ユーザー エージェント、ホスト、またはコンテンツの長さなど、設定できます。</span><span class="sxs-lookup"><span data-stu-id="952fb-200">(The CORS specification calls these "author request headers".) The rule does not apply to headers the *browser* can set, such as User-Agent, Host, or Content-Length.</span></span>

<span data-ttu-id="952fb-201">プレフライト要求の例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="952fb-201">Here is an example of a preflight request:</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample8.cmd?highlight=4-5)]

<span data-ttu-id="952fb-202">事前要求は HTTP OPTIONS メソッドを使用します。</span><span class="sxs-lookup"><span data-stu-id="952fb-202">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="952fb-203">2 つの特殊なヘッダーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="952fb-203">It includes two special headers:</span></span>

- <span data-ttu-id="952fb-204">アクセス制御の要求メソッド: 実際の要求に使用される HTTP メソッド。</span><span class="sxs-lookup"><span data-stu-id="952fb-204">Access-Control-Request-Method: The HTTP method that will be used for the actual request.</span></span>
- <span data-ttu-id="952fb-205">アクセス制御の要求ヘッダー: 要求ヘッダーの一覧を*アプリケーション*実際の要求に設定します。</span><span class="sxs-lookup"><span data-stu-id="952fb-205">Access-Control-Request-Headers: A list of request headers that the *application* set on the actual request.</span></span> <span data-ttu-id="952fb-206">(ここでも、これは含まれません、ブラウザーを設定するヘッダー。)</span><span class="sxs-lookup"><span data-stu-id="952fb-206">(Again, this does not include headers that the browser sets.)</span></span>

<span data-ttu-id="952fb-207">サーバーが要求を許可すると仮定すると、例応答を次に示します。</span><span class="sxs-lookup"><span data-stu-id="952fb-207">Here is an example response, assuming that the server allows the request:</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample9.cmd?highlight=6-7)]

<span data-ttu-id="952fb-208">応答には、許可されているメソッドを一覧表示するアクセスの制御-許可する-メソッド ヘッダーと、必要に応じて、アクセスの制御-許可する-ヘッダー ヘッダー、許可されたヘッダーの一覧を表示するが含まれます。</span><span class="sxs-lookup"><span data-stu-id="952fb-208">The response includes an Access-Control-Allow-Methods header that lists the allowed methods, and optionally an Access-Control-Allow-Headers header, which lists the allowed headers.</span></span> <span data-ttu-id="952fb-209">プレフライト要求が成功すると、ブラウザーは、前述のように、実際の要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="952fb-209">If the preflight request succeeds, the browser sends the actual request, as described earlier.</span></span>

## <a name="scope-rules-for-enablecors"></a><span data-ttu-id="952fb-210">EnableCors のスコープ規則</span><span class="sxs-lookup"><span data-stu-id="952fb-210">Scope Rules for [EnableCors]</span></span>

<span data-ttu-id="952fb-211">アクション、コント ローラーごと、またはすべての Web API コント ローラーをグローバルにあたり、アプリケーションで CORS を有効にできます。</span><span class="sxs-lookup"><span data-stu-id="952fb-211">You can enable CORS per action, per controller, or globally for all Web API controllers in your application.</span></span>

<span data-ttu-id="952fb-212">**アクションごと**</span><span class="sxs-lookup"><span data-stu-id="952fb-212">**Per Action**</span></span>

<span data-ttu-id="952fb-213">1 つのアクションで CORS を有効にするには設定、 **EnableCors**アクション メソッドの属性。</span><span class="sxs-lookup"><span data-stu-id="952fb-213">To enable CORS for a single action, set the **[EnableCors]** attribute on the action method.</span></span> <span data-ttu-id="952fb-214">次の例での CORS、`GetItem`メソッドのみです。</span><span class="sxs-lookup"><span data-stu-id="952fb-214">The following example enables CORS for the `GetItem` method only.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample10.cs)]

<span data-ttu-id="952fb-215">**コント ローラーごと**</span><span class="sxs-lookup"><span data-stu-id="952fb-215">**Per Controller**</span></span>

<span data-ttu-id="952fb-216">設定した場合**EnableCors**コント ローラー クラスで、コント ローラーのすべてのアクションに適用されます。</span><span class="sxs-lookup"><span data-stu-id="952fb-216">If you set **[EnableCors]** on the controller class, it applies to all the actions on the controller.</span></span> <span data-ttu-id="952fb-217">アクションに対して CORS を無効にする追加の**DisableCors**属性をアクションにします。</span><span class="sxs-lookup"><span data-stu-id="952fb-217">To disable CORS for an action, add the **[DisableCors]** attribute to the action.</span></span> <span data-ttu-id="952fb-218">次の例では、CORS を有効を除くすべてのメソッドに対して`PutItem`します。</span><span class="sxs-lookup"><span data-stu-id="952fb-218">The following example enables CORS for every method except `PutItem`.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample11.cs)]

<span data-ttu-id="952fb-219">**グローバルに**</span><span class="sxs-lookup"><span data-stu-id="952fb-219">**Globally**</span></span>

<span data-ttu-id="952fb-220">アプリケーション内のすべての Web API コント ローラーで CORS を有効にするのには、渡す、 **EnableCorsAttribute**インスタンスを**EnableCors**メソッド。</span><span class="sxs-lookup"><span data-stu-id="952fb-220">To enable CORS for all Web API controllers in your application, pass an **EnableCorsAttribute** instance to the **EnableCors** method:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample12.cs)]

<span data-ttu-id="952fb-221">1 つ以上のスコープで、属性を設定した場合の優先順位には。</span><span class="sxs-lookup"><span data-stu-id="952fb-221">If you set the attribute at more than one scope, the order of precedence is:</span></span>

1. <span data-ttu-id="952fb-222">アクション</span><span class="sxs-lookup"><span data-stu-id="952fb-222">Action</span></span>
2. <span data-ttu-id="952fb-223">コントローラー</span><span class="sxs-lookup"><span data-stu-id="952fb-223">Controller</span></span>
3. <span data-ttu-id="952fb-224">Global</span><span class="sxs-lookup"><span data-stu-id="952fb-224">Global</span></span>

## <a name="set-the-allowed-origins"></a><span data-ttu-id="952fb-225">許可されるオリジンを設定します。</span><span class="sxs-lookup"><span data-stu-id="952fb-225">Set the allowed origins</span></span>

<span data-ttu-id="952fb-226">*オリジン*のパラメーター、 **EnableCors**属性は、リソースのアクセスを許可するオリジンを指定します。</span><span class="sxs-lookup"><span data-stu-id="952fb-226">The *origins* parameter of the **[EnableCors]** attribute specifies which origins are allowed to access the resource.</span></span> <span data-ttu-id="952fb-227">値が、許可されたオリジンのコンマ区切り一覧です。</span><span class="sxs-lookup"><span data-stu-id="952fb-227">The value is a comma-separated list of the allowed origins.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample13.cs)]

<span data-ttu-id="952fb-228">ワイルドカード値を使用することもできます。"\*"任意のオリジンからの要求を許可します。</span><span class="sxs-lookup"><span data-stu-id="952fb-228">You can also use the wildcard value "\*" to allow requests from any origins.</span></span>

<span data-ttu-id="952fb-229">任意のオリジンからの要求を許可する前に慎重に検討してください。</span><span class="sxs-lookup"><span data-stu-id="952fb-229">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="952fb-230">あらゆる web サイトが web API への AJAX 呼び出しを実行できることを意味します。</span><span class="sxs-lookup"><span data-stu-id="952fb-230">It means that literally any website can make AJAX calls to your web API.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample14.cs)]

## <a name="set-the-allowed-http-methods"></a><span data-ttu-id="952fb-231">許可される HTTP メソッドを設定します。</span><span class="sxs-lookup"><span data-stu-id="952fb-231">Set the allowed HTTP methods</span></span>

<span data-ttu-id="952fb-232">*メソッド*のパラメーター、 **EnableCors**属性は、どの HTTP メソッドは、リソースへのアクセス許可を指定します。</span><span class="sxs-lookup"><span data-stu-id="952fb-232">The *methods* parameter of the **[EnableCors]** attribute specifies which HTTP methods are allowed to access the resource.</span></span> <span data-ttu-id="952fb-233">すべてのメソッドを許可するワイルドカード値を使用して、"\*"。</span><span class="sxs-lookup"><span data-stu-id="952fb-233">To allow all methods, use the wildcard value "\*".</span></span> <span data-ttu-id="952fb-234">次の例では、GET と POST の要求のみを許可します。</span><span class="sxs-lookup"><span data-stu-id="952fb-234">The following example allows only GET and POST requests.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample15.cs)]

## <a name="set-the-allowed-request-headers"></a><span data-ttu-id="952fb-235">許可されている要求ヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="952fb-235">Set the allowed request headers</span></span>

<span data-ttu-id="952fb-236">この記事では、プレフライト要求がアプリケーションで設定される HTTP ヘッダーの一覧を表示するアクセス制御の要求ヘッダー ヘッダーを含めることがあります以前方法を説明 (いわゆる"author 要求ヘッダー")。</span><span class="sxs-lookup"><span data-stu-id="952fb-236">This article described earlier how a preflight request might include an Access-Control-Request-Headers header, listing the HTTP headers set by the application (the so-called "author request headers").</span></span> <span data-ttu-id="952fb-237">*ヘッダー*のパラメーター、 **EnableCors**属性を指定する作成者の要求ヘッダーが許可されます。</span><span class="sxs-lookup"><span data-stu-id="952fb-237">The *headers* parameter of the **[EnableCors]** attribute specifies which author request headers are allowed.</span></span> <span data-ttu-id="952fb-238">すべてのヘッダーは、次のように設定します。*ヘッダー*に"\*"。</span><span class="sxs-lookup"><span data-stu-id="952fb-238">To allow any headers, set *headers* to "\*".</span></span> <span data-ttu-id="952fb-239">ホワイト リストの特定のヘッダーを次のように設定します。*ヘッダー*が許可されるヘッダーのコンマ区切りのリストに。</span><span class="sxs-lookup"><span data-stu-id="952fb-239">To whitelist specific headers, set *headers* to a comma-separated list of the allowed headers:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample16.cs)]

<span data-ttu-id="952fb-240">ただし、ブラウザーでは、このアクセス制御の要求ヘッダーを設定する方法で完全に一貫性がありません。</span><span class="sxs-lookup"><span data-stu-id="952fb-240">However, browsers are not entirely consistent in how they set Access-Control-Request-Headers.</span></span> <span data-ttu-id="952fb-241">たとえば、Chrome では、"origin"現在含まれています。</span><span class="sxs-lookup"><span data-stu-id="952fb-241">For example, Chrome currently includes "origin".</span></span> <span data-ttu-id="952fb-242">FireFox では、スクリプトでアプリケーションを設定している場合でも、「受け入れ」などの標準ヘッダーは含まれません。</span><span class="sxs-lookup"><span data-stu-id="952fb-242">FireFox does not include standard headers such as "Accept", even when the application sets them in script.</span></span>

<span data-ttu-id="952fb-243">設定した場合*ヘッダー*以外の値を"\*"を含める必要がある、少なくとも"accept"、「コンテンツの種類」と"origin"、およびサポートするカスタム ヘッダー。</span><span class="sxs-lookup"><span data-stu-id="952fb-243">If you set *headers* to anything other than "\*", you should include at least "accept", "content-type", and "origin", plus any custom headers that you want to support.</span></span>

## <a name="set-the-allowed-response-headers"></a><span data-ttu-id="952fb-244">許可される応答ヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="952fb-244">Set the allowed response headers</span></span>

<span data-ttu-id="952fb-245">既定では、ブラウザーは公開されませんすべてのアプリケーションに応答ヘッダー。</span><span class="sxs-lookup"><span data-stu-id="952fb-245">By default, the browser does not expose all of the response headers to the application.</span></span> <span data-ttu-id="952fb-246">既定で使用できる応答ヘッダーは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="952fb-246">The response headers that are available by default are:</span></span>

- <span data-ttu-id="952fb-247">キャッシュ制御</span><span class="sxs-lookup"><span data-stu-id="952fb-247">Cache-Control</span></span>
- <span data-ttu-id="952fb-248">コンテンツの言語</span><span class="sxs-lookup"><span data-stu-id="952fb-248">Content-Language</span></span>
- <span data-ttu-id="952fb-249">Content-Type</span><span class="sxs-lookup"><span data-stu-id="952fb-249">Content-Type</span></span>
- <span data-ttu-id="952fb-250">有効期限が切れます</span><span class="sxs-lookup"><span data-stu-id="952fb-250">Expires</span></span>
- <span data-ttu-id="952fb-251">Last-Modified</span><span class="sxs-lookup"><span data-stu-id="952fb-251">Last-Modified</span></span>
- <span data-ttu-id="952fb-252">プラグマ</span><span class="sxs-lookup"><span data-stu-id="952fb-252">Pragma</span></span>

<span data-ttu-id="952fb-253">CORS の仕様を呼び出す[単純な応答ヘッダー](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header)します。</span><span class="sxs-lookup"><span data-stu-id="952fb-253">The CORS spec calls these [simple response headers](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header).</span></span> <span data-ttu-id="952fb-254">で他のヘッダーをアプリケーションに使用できるようにするには設定、 *exposedHeaders*パラメーターの**EnableCors**します。</span><span class="sxs-lookup"><span data-stu-id="952fb-254">To make other headers available to the application, set the *exposedHeaders* parameter of **[EnableCors]**.</span></span>

<span data-ttu-id="952fb-255">次の例で、コント ローラーの`Get`メソッドが 'X カスタム ヘッダー' という名前のカスタム ヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="952fb-255">In the following example, the controller's `Get` method sets a custom header named ‘X-Custom-Header'.</span></span> <span data-ttu-id="952fb-256">既定では、ブラウザーでは、クロス オリジン要求では、このヘッダーは公開しません。</span><span class="sxs-lookup"><span data-stu-id="952fb-256">By default, the browser will not expose this header in a cross-origin request.</span></span> <span data-ttu-id="952fb-257">ヘッダーを使用できるようにするに 'X カスタム ヘッダー' を含める*exposedHeaders*します。</span><span class="sxs-lookup"><span data-stu-id="952fb-257">To make the header available, include ‘X-Custom-Header' in *exposedHeaders*.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample17.cs)]

## <a name="pass-credentials-in-cross-origin-requests"></a><span data-ttu-id="952fb-258">クロス オリジン要求で資格情報を渡す</span><span class="sxs-lookup"><span data-stu-id="952fb-258">Pass credentials in cross-origin requests</span></span>

<span data-ttu-id="952fb-259">資格情報では、CORS 要求で特別な処理が必要です。</span><span class="sxs-lookup"><span data-stu-id="952fb-259">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="952fb-260">既定では、ブラウザーは、クロス オリジン要求と共に資格情報を送信しません。</span><span class="sxs-lookup"><span data-stu-id="952fb-260">By default, the browser does not send any credentials with a cross-origin request.</span></span> <span data-ttu-id="952fb-261">資格情報には、cookie として HTTP 認証方式がなどがあります。</span><span class="sxs-lookup"><span data-stu-id="952fb-261">Credentials include cookies as well as HTTP authentication schemes.</span></span> <span data-ttu-id="952fb-262">クロス オリジン要求に資格情報を送信するクライアントを設定する必要があります**XMLHttpRequest.withCredentials**を true にします。</span><span class="sxs-lookup"><span data-stu-id="952fb-262">To send credentials with a cross-origin request, the client must set **XMLHttpRequest.withCredentials** to true.</span></span>

<span data-ttu-id="952fb-263">使用して**XMLHttpRequest**直接。</span><span class="sxs-lookup"><span data-stu-id="952fb-263">Using **XMLHttpRequest** directly:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample18.cs)]

<span data-ttu-id="952fb-264">Jquery では。</span><span class="sxs-lookup"><span data-stu-id="952fb-264">In jQuery:</span></span>

[!code-javascript[Main](enabling-cross-origin-requests-in-web-api/samples/sample19.js)]

<span data-ttu-id="952fb-265">さらに、サーバーは、資格情報を許可する必要があります。</span><span class="sxs-lookup"><span data-stu-id="952fb-265">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="952fb-266">Web API でクロス オリジンの資格情報をできるように、設定、 **SupportsCredentials**プロパティを true に、 **EnableCors**属性。</span><span class="sxs-lookup"><span data-stu-id="952fb-266">To allow cross-origin credentials in Web API, set the **SupportsCredentials** property to true on the **[EnableCors]** attribute:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample20.cs)]

<span data-ttu-id="952fb-267">このプロパティが true の場合、HTTP 応答には、アクセス コントロール-許可する-資格情報のヘッダーが含まれます。</span><span class="sxs-lookup"><span data-stu-id="952fb-267">If this property is true, the HTTP response will include an Access-Control-Allow-Credentials header.</span></span> <span data-ttu-id="952fb-268">このヘッダーは、サーバーは、クロス オリジン要求の資格情報をブラウザーに指示します。</span><span class="sxs-lookup"><span data-stu-id="952fb-268">This header tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="952fb-269">ブラウザーが資格情報を送信、応答が有効なアクセス制御を許可する-資格情報のヘッダーに含まれない場合は、ブラウザーは、アプリケーションへの応答を公開しないと、AJAX 要求は失敗します。</span><span class="sxs-lookup"><span data-stu-id="952fb-269">If the browser sends credentials, but the response does not include a valid Access-Control-Allow-Credentials header, the browser will not expose the response to the application, and the AJAX request fails.</span></span>

<span data-ttu-id="952fb-270">設定に関する注意**SupportsCredentials**を true に別のドメインに web サイトは、ユーザーを認識することがなく、ユーザーの代わりに、Web API へのログイン ユーザーの資格情報を送信することができますが行われるためです。</span><span class="sxs-lookup"><span data-stu-id="952fb-270">Be careful about setting **SupportsCredentials** to true, because it means a website at another domain can send a logged-in user's credentials to your Web API on the user's behalf, without the user being aware.</span></span> <span data-ttu-id="952fb-271">CORS の仕様もその設定を示す*オリジン*に&quot; \* &quot;有効でない場合**SupportsCredentials**が true。</span><span class="sxs-lookup"><span data-stu-id="952fb-271">The CORS spec also states that setting *origins* to &quot;\*&quot; is invalid if **SupportsCredentials** is true.</span></span>

## <a name="custom-cors-policy-providers"></a><span data-ttu-id="952fb-272">カスタムの CORS ポリシー プロバイダー</span><span class="sxs-lookup"><span data-stu-id="952fb-272">Custom CORS policy providers</span></span>

<span data-ttu-id="952fb-273">**EnableCors**実装の属性、 **ICorsPolicyProvider**インターフェイス。</span><span class="sxs-lookup"><span data-stu-id="952fb-273">The **[EnableCors]** attribute implements the **ICorsPolicyProvider** interface.</span></span> <span data-ttu-id="952fb-274">派生したクラスを作成して、独自の実装を行うことができます**属性**実装と**ICorsProlicyProvider**します。</span><span class="sxs-lookup"><span data-stu-id="952fb-274">You can provide your own implementation by creating a class that derives from **Attribute** and implements **ICorsProlicyProvider**.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample21.cs)]

<span data-ttu-id="952fb-275">任意の場所にする場合は、属性を適用するようになりました**EnableCors**します。</span><span class="sxs-lookup"><span data-stu-id="952fb-275">Now you can apply the attribute any place that you would put **[EnableCors]**.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample22.cs)]

<span data-ttu-id="952fb-276">たとえば、カスタム CORS ポリシー プロバイダーは、構成ファイルから設定を読み取る可能性があります。</span><span class="sxs-lookup"><span data-stu-id="952fb-276">For example, a custom CORS policy provider could read the settings from a configuration file.</span></span>

<span data-ttu-id="952fb-277">属性を使用する代わりに、登録することができます、 **ICorsPolicyProviderFactory**オブジェクトを作成する**ICorsPolicyProvider**オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="952fb-277">As an alternative to using attributes, you can register an **ICorsPolicyProviderFactory** object that creates **ICorsPolicyProvider** objects.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample23.cs)]

<span data-ttu-id="952fb-278">設定する、 **ICorsPolicyProviderFactory**を呼び出し、 **SetCorsPolicyProviderFactory**起動時に次のように、拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="952fb-278">To set the **ICorsPolicyProviderFactory**, call the **SetCorsPolicyProviderFactory** extension method at startup, as follows:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample24.cs)]

## <a name="browser-support"></a><span data-ttu-id="952fb-279">ブラウザーのサポート</span><span class="sxs-lookup"><span data-stu-id="952fb-279">Browser support</span></span>

<span data-ttu-id="952fb-280">Web API CORS パッケージは、サーバー側テクノロジです。</span><span class="sxs-lookup"><span data-stu-id="952fb-280">The Web API CORS package is a server-side technology.</span></span> <span data-ttu-id="952fb-281">ユーザーのブラウザーが CORS をサポートするためにも必要です。</span><span class="sxs-lookup"><span data-stu-id="952fb-281">The user's browser also needs to support CORS.</span></span> <span data-ttu-id="952fb-282">幸いにも、すべての主要なブラウザーの現在のバージョンを含める[cors サポート](http://caniuse.com/cors)します。</span><span class="sxs-lookup"><span data-stu-id="952fb-282">Fortunately, the current versions of all major browsers include [support for CORS](http://caniuse.com/cors).</span></span>
