---
uid: web-api/overview/advanced/configuring-aspnet-web-api
title: ASP.NET Web API 2 の構成 |Microsoft ドキュメント
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2014
ms.topic: article
ms.assetid: 9e10a700-8d91-4d2e-a31e-b8b569fe867c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/configuring-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: de2396710fb9434c84bf14a2faa37b98154f34d8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874981"
---
<a name="configuring-aspnet-web-api-2"></a><span data-ttu-id="a328d-102">ASP.NET Web API 2 の構成</span><span class="sxs-lookup"><span data-stu-id="a328d-102">Configuring ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="a328d-103">作成者 [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="a328d-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="a328d-104">このトピックでは、ASP.NET Web API を構成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="a328d-104">This topic describes how to configure ASP.NET Web API.</span></span>

- [<span data-ttu-id="a328d-105">構成設定</span><span class="sxs-lookup"><span data-stu-id="a328d-105">Configuration Settings</span></span>](#settings)
- [<span data-ttu-id="a328d-106">ホスティングで ASP.NET Web API の構成</span><span class="sxs-lookup"><span data-stu-id="a328d-106">Configuring Web API with ASP.NET Hosting</span></span>](#webhost)
- [<span data-ttu-id="a328d-107">OWIN の自己ホスト Web API の構成</span><span class="sxs-lookup"><span data-stu-id="a328d-107">Configuring Web API with OWIN Self-Hosting</span></span>](#selfhost)
- [<span data-ttu-id="a328d-108">グローバルの Web API サービス</span><span class="sxs-lookup"><span data-stu-id="a328d-108">Global Web API Services</span></span>](#services)
- [<span data-ttu-id="a328d-109">コント ローラーごとの構成</span><span class="sxs-lookup"><span data-stu-id="a328d-109">Per-Controller Configuration</span></span>](#percontrollerconfig)

<a id="settings"></a>
## <a name="configuration-settings"></a><span data-ttu-id="a328d-110">構成設定</span><span class="sxs-lookup"><span data-stu-id="a328d-110">Configuration Settings</span></span>

<span data-ttu-id="a328d-111">Web API の構成設定が定義されている、 [HttpConfiguration](https://msdn.microsoft.com/library/system.web.http.httpconfiguration.aspx)クラスです。</span><span class="sxs-lookup"><span data-stu-id="a328d-111">Web API configuration setttings are defined in the [HttpConfiguration](https://msdn.microsoft.com/library/system.web.http.httpconfiguration.aspx) class.</span></span>

| <span data-ttu-id="a328d-112">メンバー</span><span class="sxs-lookup"><span data-stu-id="a328d-112">Member</span></span> | <span data-ttu-id="a328d-113">説明</span><span class="sxs-lookup"><span data-stu-id="a328d-113">Description</span></span> |
| --- | --- |
| <span data-ttu-id="a328d-114">**DependencyResolver**</span><span class="sxs-lookup"><span data-stu-id="a328d-114">**DependencyResolver**</span></span> | <span data-ttu-id="a328d-115">コント ローラーの依存関係の挿入を有効にします。</span><span class="sxs-lookup"><span data-stu-id="a328d-115">Enables dependency injection for controllers.</span></span> <span data-ttu-id="a328d-116">参照してください[、Web API の依存関係競合回避モジュールを使用して](dependency-injection.md)です。</span><span class="sxs-lookup"><span data-stu-id="a328d-116">See [Using the Web API Dependency Resolver](dependency-injection.md).</span></span> |
| <span data-ttu-id="a328d-117">**Filters**</span><span class="sxs-lookup"><span data-stu-id="a328d-117">**Filters**</span></span> | <span data-ttu-id="a328d-118">アクション フィルター。</span><span class="sxs-lookup"><span data-stu-id="a328d-118">Action filters.</span></span> |
| <span data-ttu-id="a328d-119">**Formatters**</span><span class="sxs-lookup"><span data-stu-id="a328d-119">**Formatters**</span></span> | <span data-ttu-id="a328d-120">[メディア タイプ フォーマッタ](../formats-and-model-binding/media-formatters.md)です。</span><span class="sxs-lookup"><span data-stu-id="a328d-120">[Media-type formatters](../formats-and-model-binding/media-formatters.md).</span></span> |
| <span data-ttu-id="a328d-121">**IncludeErrorDetailPolicy**</span><span class="sxs-lookup"><span data-stu-id="a328d-121">**IncludeErrorDetailPolicy**</span></span> | <span data-ttu-id="a328d-122">サーバーが HTTP 応答メッセージで例外メッセージやスタック トレースなどのエラーの詳細を含めるかどうかを指定します。</span><span class="sxs-lookup"><span data-stu-id="a328d-122">Specifies whether the server should include error details, such as exception messages and stack traces, in HTTP response messages.</span></span> <span data-ttu-id="a328d-123">参照してください[IncludeErrorDetailPolicy](https://msdn.microsoft.com/library/system.web.http.includeerrordetailpolicy(v=vs.108))です。</span><span class="sxs-lookup"><span data-stu-id="a328d-123">See [IncludeErrorDetailPolicy](https://msdn.microsoft.com/library/system.web.http.includeerrordetailpolicy(v=vs.108)).</span></span> |
| <span data-ttu-id="a328d-124">**Initializer**</span><span class="sxs-lookup"><span data-stu-id="a328d-124">**Initializer**</span></span> | <span data-ttu-id="a328d-125">最終初期化を実行する関数、**HttpConfiguration**です。</span><span class="sxs-lookup"><span data-stu-id="a328d-125">A function that performs final initialization of the **HttpConfiguration**.</span></span> |
| <span data-ttu-id="a328d-126">**MessageHandlers**</span><span class="sxs-lookup"><span data-stu-id="a328d-126">**MessageHandlers**</span></span> | <span data-ttu-id="a328d-127">[HTTP メッセージ ハンドラー](http-message-handlers.md)です。</span><span class="sxs-lookup"><span data-stu-id="a328d-127">[HTTP message handlers](http-message-handlers.md).</span></span> |
| <span data-ttu-id="a328d-128">**ParameterBindingRules**</span><span class="sxs-lookup"><span data-stu-id="a328d-128">**ParameterBindingRules**</span></span> | <span data-ttu-id="a328d-129">コント ローラー アクションのパラメーターをバインドするための規則のコレクション。</span><span class="sxs-lookup"><span data-stu-id="a328d-129">A collection of rules for binding parameters on controller actions.</span></span> |
| <span data-ttu-id="a328d-130">**Properties**</span><span class="sxs-lookup"><span data-stu-id="a328d-130">**Properties**</span></span> | <span data-ttu-id="a328d-131">汎用プロパティ バッグ。</span><span class="sxs-lookup"><span data-stu-id="a328d-131">A generic property bag.</span></span> |
| <span data-ttu-id="a328d-132">**Routes**</span><span class="sxs-lookup"><span data-stu-id="a328d-132">**Routes**</span></span> | <span data-ttu-id="a328d-133">ルートのコレクション。</span><span class="sxs-lookup"><span data-stu-id="a328d-133">The collection of routes.</span></span> <span data-ttu-id="a328d-134">参照してください[ASP.NET Web API でルーティング](../web-api-routing-and-actions/routing-in-aspnet-web-api.md)です。</span><span class="sxs-lookup"><span data-stu-id="a328d-134">See [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span> |
| <span data-ttu-id="a328d-135">**Services**</span><span class="sxs-lookup"><span data-stu-id="a328d-135">**Services**</span></span> | <span data-ttu-id="a328d-136">サービスのコレクション。</span><span class="sxs-lookup"><span data-stu-id="a328d-136">The collection of services.</span></span> <span data-ttu-id="a328d-137">参照してください[Services](#services)です。</span><span class="sxs-lookup"><span data-stu-id="a328d-137">See [Services](#services).</span></span> |


## <a name="prerequisites"></a><span data-ttu-id="a328d-138">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="a328d-138">Prerequisites</span></span>

<span data-ttu-id="a328d-139">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community、Professional、または Enterprise Edition。</span><span class="sxs-lookup"><span data-stu-id="a328d-139">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional, or Enterprise Edition.</span></span>

<a id="webhost"></a>
## <a name="configuring-web-api-with-aspnet-hosting"></a><span data-ttu-id="a328d-140">ホスティングで ASP.NET Web API の構成</span><span class="sxs-lookup"><span data-stu-id="a328d-140">Configuring Web API with ASP.NET Hosting</span></span>

<span data-ttu-id="a328d-141">ASP.NET アプリケーションで、**Application\_Start** メソッドから [GlobalConfiguration.Configure](https://msdn.microsoft.com/library/system.web.http.globalconfiguration.configure.aspx) を呼び出して Web API を設定します。</span><span class="sxs-lookup"><span data-stu-id="a328d-141">In an ASP.NET application, configure Web API by calling [GlobalConfiguration.Configure](https://msdn.microsoft.com/library/system.web.http.globalconfiguration.configure.aspx) in the **Application\_Start** method.</span></span> <span data-ttu-id="a328d-142">**構成**メソッドが型の 1 つのパラメーターを持つデリゲートを受け取る**HttpConfiguration**です。</span><span class="sxs-lookup"><span data-stu-id="a328d-142">The **Configure** method takes a delegate with a single parameter of type **HttpConfiguration**.</span></span> <span data-ttu-id="a328d-143">デリゲート内の構成のすべてを実行します。</span><span class="sxs-lookup"><span data-stu-id="a328d-143">Perform all of your configuation inside the delegate.</span></span>

<span data-ttu-id="a328d-144">匿名デリゲートの使用例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="a328d-144">Here is an example using an anonymous delegate:</span></span>

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="a328d-145">Visual Studio 2017 で「ASP.NET Web アプリケーション」のプロジェクト テンプレートを自動的に設定、構成コードの"Web API"を選択した場合、**新しい ASP.NET プロジェクト**ダイアログ。</span><span class="sxs-lookup"><span data-stu-id="a328d-145">In Visual Studio 2017, the "ASP.NET Web Application" project template automatically sets up the configuration code, if you select "Web API" in the **New ASP.NET Project** dialog.</span></span>

[![](configuring-aspnet-web-api/_static/image2.png)](configuring-aspnet-web-api/_static/image1.png)

<span data-ttu-id="a328d-146">プロジェクト テンプレートは、アプリ内で WebApiConfig.cs をという名前のファイルを作成\_開始フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="a328d-146">The project template creates a file named WebApiConfig.cs inside the App\_Start folder.</span></span> <span data-ttu-id="a328d-147">このコード ファイルでは、Web API 構成コードを配置する必要があります、デリゲートを定義します。</span><span class="sxs-lookup"><span data-stu-id="a328d-147">This code file defines the delegate where you should put your Web API configuration code.</span></span>

![](configuring-aspnet-web-api/_static/image3.png)

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample2.cs?highlight=12)]

<span data-ttu-id="a328d-148">プロジェクト テンプレートもからデリゲートを呼び出すコードを追加**Application\_Start**です。
</span><span class="sxs-lookup"><span data-stu-id="a328d-148">The project template also adds the code that calls the delegate from **Application\_Start**.</span></span>

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample3.cs?highlight=5)]

<a id="selfhost"></a>
## <a name="configuring-web-api-with-owin-self-hosting"></a><span data-ttu-id="a328d-149">OWIN の自己ホスト Web API の構成</span><span class="sxs-lookup"><span data-stu-id="a328d-149">Configuring Web API with OWIN Self-Hosting</span></span>

<span data-ttu-id="a328d-150">OWIN の自己ホストの場合は、作成、新しい**HttpConfiguration**インスタンス。</span><span class="sxs-lookup"><span data-stu-id="a328d-150">If you are self-hosting with OWIN, create a new **HttpConfiguration** instance.</span></span> <span data-ttu-id="a328d-151">このインスタンスでの構成を行うし、インスタンスを渡す、 **Owin.UseWebApi**拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="a328d-151">Perform any configuration on this instance, and then pass the instance to the **Owin.UseWebApi** extension method.</span></span>

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample4.cs)]

<span data-ttu-id="a328d-152">チュートリアル[Self-Host ASP.NET Web API 2 を使用する OWIN](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)完了手順を示しています。</span><span class="sxs-lookup"><span data-stu-id="a328d-152">The tutorial [Use OWIN to Self-Host ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md) shows the complete steps.</span></span>

<a id="services"></a>
## <a name="global-web-api-services"></a><span data-ttu-id="a328d-153">グローバルの Web API サービス</span><span class="sxs-lookup"><span data-stu-id="a328d-153">Global Web API Services</span></span>

<span data-ttu-id="a328d-154">**HttpConfiguration.Services**コレクションには、コント ローラーの選択とコンテンツ ネゴシエーションなど、さまざまなタスクを実行する Web API を使用するグローバル サービスのセットが含まれています。</span><span class="sxs-lookup"><span data-stu-id="a328d-154">The **HttpConfiguration.Services** collection contains a set of global services that Web API uses to perform various tasks, such as controller selection and content negotiation.</span></span>

> [!NOTE]
> <span data-ttu-id="a328d-155">**Services**コレクションは、サービスの検出または依存関係の挿入の汎用の機構ではありません。</span><span class="sxs-lookup"><span data-stu-id="a328d-155">The **Services** collection is not a general-purpose mechanism for service discovery or dependency injection.</span></span> <span data-ttu-id="a328d-156">Web API フレームワークに認識されているサービスの種類のみ格納されます。</span><span class="sxs-lookup"><span data-stu-id="a328d-156">It only stores service types that are known to the Web API framework.</span></span>


<span data-ttu-id="a328d-157">**Services**コレクションは、サービスの既定のセットを初期化し、独自のカスタム実装を提供することができます。</span><span class="sxs-lookup"><span data-stu-id="a328d-157">The **Services** collection is initialized with a default set of services, and you can provide your own custom implementations.</span></span> <span data-ttu-id="a328d-158">いくつかのサービスは、インスタンス 1 つだけが他のユーザーに複数のインスタンスをサポートします。</span><span class="sxs-lookup"><span data-stu-id="a328d-158">Some services support multiple instances, while others can have only one instance.</span></span> <span data-ttu-id="a328d-159">(ただし、コント ローラー レベルでサービスを提供することもできます。 を参照してください[コント ローラーごとの構成](#percontrollerconfig)です。</span><span class="sxs-lookup"><span data-stu-id="a328d-159">(However, you can also provide services at the controller level; see [Per-Controller Configuration](#percontrollerconfig).</span></span>

<span data-ttu-id="a328d-160">単一インスタンス サービス</span><span class="sxs-lookup"><span data-stu-id="a328d-160">Single-Instance Services</span></span>


| <span data-ttu-id="a328d-161">サービス</span><span class="sxs-lookup"><span data-stu-id="a328d-161">Service</span></span> | <span data-ttu-id="a328d-162">説明</span><span class="sxs-lookup"><span data-stu-id="a328d-162">Description</span></span> |
| --- | --- |
| <span data-ttu-id="a328d-163">**IActionValueBinder**</span><span class="sxs-lookup"><span data-stu-id="a328d-163">**IActionValueBinder**</span></span> | <span data-ttu-id="a328d-164">パラメーターのバインドを取得します。</span><span class="sxs-lookup"><span data-stu-id="a328d-164">Gets a binding for a parameter.</span></span> |
| <span data-ttu-id="a328d-165">**IApiExplorer**</span><span class="sxs-lookup"><span data-stu-id="a328d-165">**IApiExplorer**</span></span> | <span data-ttu-id="a328d-166">アプリケーションによって公開される Api の説明を取得します。</span><span class="sxs-lookup"><span data-stu-id="a328d-166">Gets descriptions of the APIs exposed by the application.</span></span> <span data-ttu-id="a328d-167">参照してください[Web API のヘルプ ページを作成する](../getting-started-with-aspnet-web-api/creating-api-help-pages.md)です。</span><span class="sxs-lookup"><span data-stu-id="a328d-167">See [Creating a Help Page for a Web API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md).</span></span> |
| <span data-ttu-id="a328d-168">**IAssembliesResolver**</span><span class="sxs-lookup"><span data-stu-id="a328d-168">**IAssembliesResolver**</span></span> | <span data-ttu-id="a328d-169">アプリケーションのアセンブリの一覧を取得します。</span><span class="sxs-lookup"><span data-stu-id="a328d-169">Gets a list of the assemblies for the application.</span></span> <span data-ttu-id="a328d-170">参照してください[ルーティングとアクションの選択](../web-api-routing-and-actions/routing-and-action-selection.md)です。</span><span class="sxs-lookup"><span data-stu-id="a328d-170">See [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span> |
| <span data-ttu-id="a328d-171">**IBodyModelValidator**</span><span class="sxs-lookup"><span data-stu-id="a328d-171">**IBodyModelValidator**</span></span> | <span data-ttu-id="a328d-172">メディアの種類のフォーマッタによって要求の本体から読み取られるモデルを検証します。</span><span class="sxs-lookup"><span data-stu-id="a328d-172">Validates a model that is read from the request body by a media-type formatter.</span></span> |
| <span data-ttu-id="a328d-173">**IContentNegotiator**</span><span class="sxs-lookup"><span data-stu-id="a328d-173">**IContentNegotiator**</span></span> | <span data-ttu-id="a328d-174">コンテンツ ネゴシエーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="a328d-174">Performs content negotiation.</span></span> |
| <span data-ttu-id="a328d-175">**IDocumentationProvider**</span><span class="sxs-lookup"><span data-stu-id="a328d-175">**IDocumentationProvider**</span></span> | <span data-ttu-id="a328d-176">Api のドキュメントを提供します。</span><span class="sxs-lookup"><span data-stu-id="a328d-176">Provides documentation for APIs.</span></span> <span data-ttu-id="a328d-177">既定値は**null**です。</span><span class="sxs-lookup"><span data-stu-id="a328d-177">The default is **null**.</span></span> <span data-ttu-id="a328d-178">参照してください[Web API のヘルプ ページを作成する](../getting-started-with-aspnet-web-api/creating-api-help-pages.md)です。</span><span class="sxs-lookup"><span data-stu-id="a328d-178">See [Creating a Help Page for a Web API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md).</span></span> |
| <span data-ttu-id="a328d-179">**IHostBufferPolicySelector**</span><span class="sxs-lookup"><span data-stu-id="a328d-179">**IHostBufferPolicySelector**</span></span> | <span data-ttu-id="a328d-180">ホストが HTTP メッセージのエンティティ ボディをバッファーするかどうかを示します。</span><span class="sxs-lookup"><span data-stu-id="a328d-180">Indicates whether the host should buffer HTTP message entity bodies.</span></span> |
| <span data-ttu-id="a328d-181">**IHttpActionInvoker**</span><span class="sxs-lookup"><span data-stu-id="a328d-181">**IHttpActionInvoker**</span></span> | <span data-ttu-id="a328d-182">コント ローラーのアクションを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="a328d-182">Invokes a controller action.</span></span> <span data-ttu-id="a328d-183">参照してください[ルーティングとアクションの選択](../web-api-routing-and-actions/routing-and-action-selection.md)です。</span><span class="sxs-lookup"><span data-stu-id="a328d-183">See [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span> |
| <span data-ttu-id="a328d-184">**IHttpActionSelector**</span><span class="sxs-lookup"><span data-stu-id="a328d-184">**IHttpActionSelector**</span></span> | <span data-ttu-id="a328d-185">コント ローラーのアクションを選択します。</span><span class="sxs-lookup"><span data-stu-id="a328d-185">Selects a controller action.</span></span> <span data-ttu-id="a328d-186">参照してください[ルーティングとアクションの選択](../web-api-routing-and-actions/routing-and-action-selection.md)です。</span><span class="sxs-lookup"><span data-stu-id="a328d-186">See [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span> |
| <span data-ttu-id="a328d-187">**IHttpControllerActivator**</span><span class="sxs-lookup"><span data-stu-id="a328d-187">**IHttpControllerActivator**</span></span> | <span data-ttu-id="a328d-188">コント ローラーがアクティブにします。</span><span class="sxs-lookup"><span data-stu-id="a328d-188">Activates a controller.</span></span> <span data-ttu-id="a328d-189">参照してください[ルーティングとアクションの選択](../web-api-routing-and-actions/routing-and-action-selection.md)です。</span><span class="sxs-lookup"><span data-stu-id="a328d-189">See [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span> |
| <span data-ttu-id="a328d-190">**IHttpControllerSelector**</span><span class="sxs-lookup"><span data-stu-id="a328d-190">**IHttpControllerSelector**</span></span> | <span data-ttu-id="a328d-191">コント ローラーを選択します。</span><span class="sxs-lookup"><span data-stu-id="a328d-191">Selects a controller.</span></span> <span data-ttu-id="a328d-192">参照してください[ルーティングとアクションの選択](../web-api-routing-and-actions/routing-and-action-selection.md)です。</span><span class="sxs-lookup"><span data-stu-id="a328d-192">See [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span> |
| <span data-ttu-id="a328d-193">**IHttpControllerTypeResolver**</span><span class="sxs-lookup"><span data-stu-id="a328d-193">**IHttpControllerTypeResolver**</span></span> | <span data-ttu-id="a328d-194">アプリケーションで Web API コント ローラーの種類の一覧を示します。</span><span class="sxs-lookup"><span data-stu-id="a328d-194">Provides a list of the Web API controller types in the application.</span></span> <span data-ttu-id="a328d-195">参照してください[ルーティングとアクションの選択](../web-api-routing-and-actions/routing-and-action-selection.md)です。</span><span class="sxs-lookup"><span data-stu-id="a328d-195">See [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span> |
| <span data-ttu-id="a328d-196">**ITraceManager**</span><span class="sxs-lookup"><span data-stu-id="a328d-196">**ITraceManager**</span></span> | <span data-ttu-id="a328d-197">このトレース フレームワークを初期化します。</span><span class="sxs-lookup"><span data-stu-id="a328d-197">Initializes the tracing framework.</span></span> <span data-ttu-id="a328d-198">参照してください[ASP.NET web API Tracing](../testing-and-debugging/tracing-in-aspnet-web-api.md)です。</span><span class="sxs-lookup"><span data-stu-id="a328d-198">See [Tracing in ASP.NET Web API](../testing-and-debugging/tracing-in-aspnet-web-api.md).</span></span> |
| <span data-ttu-id="a328d-199">**ITraceWriter**</span><span class="sxs-lookup"><span data-stu-id="a328d-199">**ITraceWriter**</span></span> | <span data-ttu-id="a328d-200">トレース ライターを提供します。</span><span class="sxs-lookup"><span data-stu-id="a328d-200">Provides a trace writer.</span></span> <span data-ttu-id="a328d-201">既定では、"no-op"トレース ライターです。</span><span class="sxs-lookup"><span data-stu-id="a328d-201">The default is a "no-op" trace writer.</span></span> <span data-ttu-id="a328d-202">参照してください[ASP.NET web API Tracing](../testing-and-debugging/tracing-in-aspnet-web-api.md)です。</span><span class="sxs-lookup"><span data-stu-id="a328d-202">See [Tracing in ASP.NET Web API](../testing-and-debugging/tracing-in-aspnet-web-api.md).</span></span> |
| <span data-ttu-id="a328d-203">**IModelValidatorCache**</span><span class="sxs-lookup"><span data-stu-id="a328d-203">**IModelValidatorCache**</span></span> | <span data-ttu-id="a328d-204">モデル検証コントロールのキャッシュを提供します。</span><span class="sxs-lookup"><span data-stu-id="a328d-204">Provides a cache of model validators.</span></span> |

<span data-ttu-id="a328d-205">複数のインスタンスのサービス</span><span class="sxs-lookup"><span data-stu-id="a328d-205">Multiple-Instance Services</span></span>


|                 <span data-ttu-id="a328d-206">サービス</span><span class="sxs-lookup"><span data-stu-id="a328d-206">Service</span></span>                 |                                                                                                              <span data-ttu-id="a328d-207">説明</span><span class="sxs-lookup"><span data-stu-id="a328d-207">Description</span></span>                                                                                                               |
|-----------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <span data-ttu-id="a328d-208"><strong>IFilterProvider</strong></span><span class="sxs-lookup"><span data-stu-id="a328d-208"><strong>IFilterProvider</strong></span></span>     |                                                                                           <span data-ttu-id="a328d-209">コント ローラーのアクションのフィルターの一覧を返します。</span><span class="sxs-lookup"><span data-stu-id="a328d-209">Returns a list of filters for a controller action.</span></span>                                                                                           |
|  <span data-ttu-id="a328d-210"><strong>ModelBinderProvider</strong></span><span class="sxs-lookup"><span data-stu-id="a328d-210"><strong>ModelBinderProvider</strong></span></span>   |                                                                                                <span data-ttu-id="a328d-211">特定の種類のモデル バインダーを返します。</span><span class="sxs-lookup"><span data-stu-id="a328d-211">Returns a model binder for a given type.</span></span>                                                                                                |
| <span data-ttu-id="a328d-212"><strong>ModelMetadataProvider</strong></span><span class="sxs-lookup"><span data-stu-id="a328d-212"><strong>ModelMetadataProvider</strong></span></span>  |                                                                                                     <span data-ttu-id="a328d-213">モデルのメタデータを提供します。</span><span class="sxs-lookup"><span data-stu-id="a328d-213">Provides metadata for a model.</span></span>                                                                                                     |
| <span data-ttu-id="a328d-214"><strong>ModelValidatorProvider</strong></span><span class="sxs-lookup"><span data-stu-id="a328d-214"><strong>ModelValidatorProvider</strong></span></span> |                                                                                                   <span data-ttu-id="a328d-215">モデルの検証コントロールを提供します。</span><span class="sxs-lookup"><span data-stu-id="a328d-215">Provides a validator for a model.</span></span>                                                                                                    |
|  <span data-ttu-id="a328d-216"><strong>ValueProviderFactory</strong></span><span class="sxs-lookup"><span data-stu-id="a328d-216"><strong>ValueProviderFactory</strong></span></span>  | <span data-ttu-id="a328d-217">値プロバイダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="a328d-217">Creates a value provider.</span></span> <span data-ttu-id="a328d-218">詳細については、Mike Stall のブログの投稿を参照してください[WebAPI でカスタム値プロバイダーを作成する方法。](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)</span><span class="sxs-lookup"><span data-stu-id="a328d-218">For more information, see Mike Stall's blog post [How to create a custom value provider in WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)</span></span> |

<span data-ttu-id="a328d-219">マルチ インスタンスのサービスに、カスタム実装を追加するには、呼び出す**追加**または**挿入**上、 **Services**コレクション。</span><span class="sxs-lookup"><span data-stu-id="a328d-219">To add a custom implementation to a multi-instance service, call **Add** or **Insert** on the **Services** collection:</span></span>

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="a328d-220">呼び出しに単一インスタンスのサービスをカスタム実装に置き換えて、**置換**上、 **Services**コレクション。</span><span class="sxs-lookup"><span data-stu-id="a328d-220">To replace a single-instance service with a custom implementation, call **Replace** on the **Services** collection:</span></span>

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample6.cs)]

<a id="percontrollerconfig"></a>
## <a name="per-controller-configuration"></a><span data-ttu-id="a328d-221">コント ローラーごとの構成</span><span class="sxs-lookup"><span data-stu-id="a328d-221">Per-Controller Configuration</span></span>

<span data-ttu-id="a328d-222">コント ローラーあたりごとに、次の設定をオーバーライドすることができます。</span><span class="sxs-lookup"><span data-stu-id="a328d-222">You can override the following settings on a per-controller basis:</span></span>

- <span data-ttu-id="a328d-223">メディア タイプ フォーマッタ</span><span class="sxs-lookup"><span data-stu-id="a328d-223">Media-type formatters</span></span>
- <span data-ttu-id="a328d-224">パラメーター バインディング規則</span><span class="sxs-lookup"><span data-stu-id="a328d-224">Parameter binding rules</span></span>
- <span data-ttu-id="a328d-225">サービス</span><span class="sxs-lookup"><span data-stu-id="a328d-225">Services</span></span>

<span data-ttu-id="a328d-226">これを行うを実装するカスタム属性を定義する、 **IControllerConfiguration**インターフェイスです。</span><span class="sxs-lookup"><span data-stu-id="a328d-226">To do so, define a custom attribute that implements the **IControllerConfiguration** interface.</span></span> <span data-ttu-id="a328d-227">コント ローラーに属性が適用されます。</span><span class="sxs-lookup"><span data-stu-id="a328d-227">Then apply the attribute to the controller.</span></span>

<span data-ttu-id="a328d-228">次の例は、既定のメディア タイプ フォーマッタをカスタム フォーマッタに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="a328d-228">The following example replaces the default media-type formatters with a custom formatter.</span></span>

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="a328d-229">**IControllerConfiguration.Initialize**メソッドは、2 つのパラメーターを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="a328d-229">The **IControllerConfiguration.Initialize** method takes two parameters:</span></span>

- <span data-ttu-id="a328d-230">**HttpControllerSettings**オブジェクト</span><span class="sxs-lookup"><span data-stu-id="a328d-230">An **HttpControllerSettings** object</span></span>
- <span data-ttu-id="a328d-231">**HttpControllerDescriptor**オブジェクト</span><span class="sxs-lookup"><span data-stu-id="a328d-231">An **HttpControllerDescriptor** object</span></span>

<span data-ttu-id="a328d-232">**HttpControllerDescriptor**情報目的 (つまり、2 つのコント ローラー間で区別するために) を確認することができますが、コント ローラーの説明が含まれています。</span><span class="sxs-lookup"><span data-stu-id="a328d-232">The **HttpControllerDescriptor** contains a description of the controller, which you can examine for informational purposes (say, to distinguish between two controllers).</span></span>

<span data-ttu-id="a328d-233">使用して、 **HttpControllerSettings**コント ローラーを構成するオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="a328d-233">Use the **HttpControllerSettings** object to configure the controller.</span></span> <span data-ttu-id="a328d-234">このオブジェクトには、コント ローラーあたりごとにオーバーライド可能な構成パラメーターのサブセットが含まれています。</span><span class="sxs-lookup"><span data-stu-id="a328d-234">This object contains the subset of configuration parameters that you can override on a per-controller basis.</span></span> <span data-ttu-id="a328d-235">すべての設定は変更しないことを既定でグローバルに**HttpConfiguration**オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="a328d-235">Any settings that you don't change default to the global **HttpConfiguration** object.</span></span>
