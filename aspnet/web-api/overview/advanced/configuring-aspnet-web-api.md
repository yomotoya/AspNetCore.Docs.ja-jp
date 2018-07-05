---
uid: web-api/overview/advanced/configuring-aspnet-web-api
title: ASP.NET Web API 2 の構成 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2014
ms.topic: article
ms.assetid: 9e10a700-8d91-4d2e-a31e-b8b569fe867c
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/advanced/configuring-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 8fd08098b5a425f2cbd7939f5f90550b98c34071
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37387084"
---
<a name="configuring-aspnet-web-api-2"></a><span data-ttu-id="3e36b-102">ASP.NET Web API 2 の構成</span><span class="sxs-lookup"><span data-stu-id="3e36b-102">Configuring ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="3e36b-103">作成者[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="3e36b-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="3e36b-104">このトピックでは、ASP.NET Web API を構成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="3e36b-104">This topic describes how to configure ASP.NET Web API.</span></span>

- [<span data-ttu-id="3e36b-105">構成設定</span><span class="sxs-lookup"><span data-stu-id="3e36b-105">Configuration Settings</span></span>](#settings)
- [<span data-ttu-id="3e36b-106">ASP.NET ホストによる Web API の構成</span><span class="sxs-lookup"><span data-stu-id="3e36b-106">Configuring Web API with ASP.NET Hosting</span></span>](#webhost)
- [<span data-ttu-id="3e36b-107">OWIN 自己ホストによる Web API の構成</span><span class="sxs-lookup"><span data-stu-id="3e36b-107">Configuring Web API with OWIN Self-Hosting</span></span>](#selfhost)
- [<span data-ttu-id="3e36b-108">グローバルな Web API サービス</span><span class="sxs-lookup"><span data-stu-id="3e36b-108">Global Web API Services</span></span>](#services)
- [<span data-ttu-id="3e36b-109">コント ローラーごとの構成</span><span class="sxs-lookup"><span data-stu-id="3e36b-109">Per-Controller Configuration</span></span>](#percontrollerconfig)

<a id="settings"></a>
## <a name="configuration-settings"></a><span data-ttu-id="3e36b-110">構成設定</span><span class="sxs-lookup"><span data-stu-id="3e36b-110">Configuration Settings</span></span>

<span data-ttu-id="3e36b-111">Web API の構成設定が定義されている、 [HttpConfiguration](https://msdn.microsoft.com/library/system.web.http.httpconfiguration.aspx)クラス。</span><span class="sxs-lookup"><span data-stu-id="3e36b-111">Web API configuration setttings are defined in the [HttpConfiguration](https://msdn.microsoft.com/library/system.web.http.httpconfiguration.aspx) class.</span></span>

| <span data-ttu-id="3e36b-112">メンバー</span><span class="sxs-lookup"><span data-stu-id="3e36b-112">Member</span></span> | <span data-ttu-id="3e36b-113">説明</span><span class="sxs-lookup"><span data-stu-id="3e36b-113">Description</span></span> |
| --- | --- |
| <span data-ttu-id="3e36b-114">**DependencyResolver**</span><span class="sxs-lookup"><span data-stu-id="3e36b-114">**DependencyResolver**</span></span> | <span data-ttu-id="3e36b-115">コント ローラーの依存関係の挿入を有効にします。</span><span class="sxs-lookup"><span data-stu-id="3e36b-115">Enables dependency injection for controllers.</span></span> <span data-ttu-id="3e36b-116">参照してください[、Web API の依存関係競合回避モジュールを使用して](dependency-injection.md)します。</span><span class="sxs-lookup"><span data-stu-id="3e36b-116">See [Using the Web API Dependency Resolver](dependency-injection.md).</span></span> |
| <span data-ttu-id="3e36b-117">**Filters**</span><span class="sxs-lookup"><span data-stu-id="3e36b-117">**Filters**</span></span> | <span data-ttu-id="3e36b-118">アクション フィルター。</span><span class="sxs-lookup"><span data-stu-id="3e36b-118">Action filters.</span></span> |
| <span data-ttu-id="3e36b-119">**Formatters**</span><span class="sxs-lookup"><span data-stu-id="3e36b-119">**Formatters**</span></span> | <span data-ttu-id="3e36b-120">[メディア タイプ フォーマッタ](../formats-and-model-binding/media-formatters.md)です。</span><span class="sxs-lookup"><span data-stu-id="3e36b-120">[Media-type formatters](../formats-and-model-binding/media-formatters.md).</span></span> |
| <span data-ttu-id="3e36b-121">**IncludeErrorDetailPolicy**</span><span class="sxs-lookup"><span data-stu-id="3e36b-121">**IncludeErrorDetailPolicy**</span></span> | <span data-ttu-id="3e36b-122">サーバーが HTTP 応答メッセージで例外メッセージやスタック トレースなどのエラーの詳細を含めるかどうかを指定します。</span><span class="sxs-lookup"><span data-stu-id="3e36b-122">Specifies whether the server should include error details, such as exception messages and stack traces, in HTTP response messages.</span></span> <span data-ttu-id="3e36b-123">参照してください[IncludeErrorDetailPolicy](https://msdn.microsoft.com/library/system.web.http.includeerrordetailpolicy(v=vs.108))します。</span><span class="sxs-lookup"><span data-stu-id="3e36b-123">See [IncludeErrorDetailPolicy](https://msdn.microsoft.com/library/system.web.http.includeerrordetailpolicy(v=vs.108)).</span></span> |
| <span data-ttu-id="3e36b-124">**Initializer**</span><span class="sxs-lookup"><span data-stu-id="3e36b-124">**Initializer**</span></span> | <span data-ttu-id="3e36b-125">最終初期化を実行する関数、**HttpConfiguration**です。</span><span class="sxs-lookup"><span data-stu-id="3e36b-125">A function that performs final initialization of the **HttpConfiguration**.</span></span> |
| <span data-ttu-id="3e36b-126">**MessageHandlers**</span><span class="sxs-lookup"><span data-stu-id="3e36b-126">**MessageHandlers**</span></span> | <span data-ttu-id="3e36b-127">[HTTP メッセージ ハンドラー](http-message-handlers.md)します。</span><span class="sxs-lookup"><span data-stu-id="3e36b-127">[HTTP message handlers](http-message-handlers.md).</span></span> |
| <span data-ttu-id="3e36b-128">**ParameterBindingRules**</span><span class="sxs-lookup"><span data-stu-id="3e36b-128">**ParameterBindingRules**</span></span> | <span data-ttu-id="3e36b-129">コント ローラー アクションのパラメーターをバインドするための規則のコレクション。</span><span class="sxs-lookup"><span data-stu-id="3e36b-129">A collection of rules for binding parameters on controller actions.</span></span> |
| <span data-ttu-id="3e36b-130">**Properties**</span><span class="sxs-lookup"><span data-stu-id="3e36b-130">**Properties**</span></span> | <span data-ttu-id="3e36b-131">汎用プロパティ バッグ。</span><span class="sxs-lookup"><span data-stu-id="3e36b-131">A generic property bag.</span></span> |
| <span data-ttu-id="3e36b-132">**Routes**</span><span class="sxs-lookup"><span data-stu-id="3e36b-132">**Routes**</span></span> | <span data-ttu-id="3e36b-133">ルートのコレクション。</span><span class="sxs-lookup"><span data-stu-id="3e36b-133">The collection of routes.</span></span> <span data-ttu-id="3e36b-134">参照してください[ASP.NET Web API でルーティング](../web-api-routing-and-actions/routing-in-aspnet-web-api.md)です。</span><span class="sxs-lookup"><span data-stu-id="3e36b-134">See [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span> |
| <span data-ttu-id="3e36b-135">**Services**</span><span class="sxs-lookup"><span data-stu-id="3e36b-135">**Services**</span></span> | <span data-ttu-id="3e36b-136">サービスのコレクション。</span><span class="sxs-lookup"><span data-stu-id="3e36b-136">The collection of services.</span></span> <span data-ttu-id="3e36b-137">参照してください[Services](#services)です。</span><span class="sxs-lookup"><span data-stu-id="3e36b-137">See [Services](#services).</span></span> |


## <a name="prerequisites"></a><span data-ttu-id="3e36b-138">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="3e36b-138">Prerequisites</span></span>

<span data-ttu-id="3e36b-139">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community、Professional、または Enterprise Edition。</span><span class="sxs-lookup"><span data-stu-id="3e36b-139">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional, or Enterprise Edition.</span></span>

<a id="webhost"></a>
## <a name="configuring-web-api-with-aspnet-hosting"></a><span data-ttu-id="3e36b-140">ASP.NET ホストによる Web API の構成</span><span class="sxs-lookup"><span data-stu-id="3e36b-140">Configuring Web API with ASP.NET Hosting</span></span>

<span data-ttu-id="3e36b-141">ASP.NET アプリケーションで、**Application\_Start** メソッドから [GlobalConfiguration.Configure](https://msdn.microsoft.com/library/system.web.http.globalconfiguration.configure.aspx) を呼び出して Web API を設定します。</span><span class="sxs-lookup"><span data-stu-id="3e36b-141">In an ASP.NET application, configure Web API by calling [GlobalConfiguration.Configure](https://msdn.microsoft.com/library/system.web.http.globalconfiguration.configure.aspx) in the **Application\_Start** method.</span></span> <span data-ttu-id="3e36b-142">**構成**メソッドは型の 1 つのパラメーターを持つデリゲートを受け取ります**HttpConfiguration**します。</span><span class="sxs-lookup"><span data-stu-id="3e36b-142">The **Configure** method takes a delegate with a single parameter of type **HttpConfiguration**.</span></span> <span data-ttu-id="3e36b-143">デリゲート内で、構成のすべてを実行します。</span><span class="sxs-lookup"><span data-stu-id="3e36b-143">Perform all of your configuation inside the delegate.</span></span>

<span data-ttu-id="3e36b-144">匿名デリゲートの使用例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="3e36b-144">Here is an example using an anonymous delegate:</span></span>

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="3e36b-145">Visual Studio 2017 では、「ASP.NET Web アプリケーション」プロジェクト テンプレートを自動的に設定、構成コードの"Web API"を選択した場合、**新しい ASP.NET プロジェクト**ダイアログ。</span><span class="sxs-lookup"><span data-stu-id="3e36b-145">In Visual Studio 2017, the "ASP.NET Web Application" project template automatically sets up the configuration code, if you select "Web API" in the **New ASP.NET Project** dialog.</span></span>

[![](configuring-aspnet-web-api/_static/image2.png)](configuring-aspnet-web-api/_static/image1.png)

<span data-ttu-id="3e36b-146">プロジェクト テンプレートは、アプリ内で WebApiConfig.cs という名前のファイルを作成します。\_開始フォルダー。</span><span class="sxs-lookup"><span data-stu-id="3e36b-146">The project template creates a file named WebApiConfig.cs inside the App\_Start folder.</span></span> <span data-ttu-id="3e36b-147">このコード ファイルは、Web API の構成コードを配置する必要があります、デリゲートを定義します。</span><span class="sxs-lookup"><span data-stu-id="3e36b-147">This code file defines the delegate where you should put your Web API configuration code.</span></span>

![](configuring-aspnet-web-api/_static/image3.png)

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample2.cs?highlight=12)]

<span data-ttu-id="3e36b-148">プロジェクト テンプレートもからデリゲートを呼び出すコードを追加**Application\_Start**です。
</span><span class="sxs-lookup"><span data-stu-id="3e36b-148">The project template also adds the code that calls the delegate from **Application\_Start**.</span></span>

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample3.cs?highlight=5)]

<a id="selfhost"></a>
## <a name="configuring-web-api-with-owin-self-hosting"></a><span data-ttu-id="3e36b-149">OWIN 自己ホストによる Web API の構成</span><span class="sxs-lookup"><span data-stu-id="3e36b-149">Configuring Web API with OWIN Self-Hosting</span></span>

<span data-ttu-id="3e36b-150">OWIN 自己ホストする場合は、作成、新しい**HttpConfiguration**インスタンス。</span><span class="sxs-lookup"><span data-stu-id="3e36b-150">If you are self-hosting with OWIN, create a new **HttpConfiguration** instance.</span></span> <span data-ttu-id="3e36b-151">このインスタンスで、任意の構成を実行し、インスタンスを渡す、 **Owin.UseWebApi**拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="3e36b-151">Perform any configuration on this instance, and then pass the instance to the **Owin.UseWebApi** extension method.</span></span>

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample4.cs)]

<span data-ttu-id="3e36b-152">このチュートリアル[Self-Host ASP.NET Web API 2 を使用して OWIN](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)は完全な手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="3e36b-152">The tutorial [Use OWIN to Self-Host ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md) shows the complete steps.</span></span>

<a id="services"></a>
## <a name="global-web-api-services"></a><span data-ttu-id="3e36b-153">グローバルな Web API サービス</span><span class="sxs-lookup"><span data-stu-id="3e36b-153">Global Web API Services</span></span>

<span data-ttu-id="3e36b-154">**HttpConfiguration.Services**コレクションには、Web API を使用してコント ローラーの選択とコンテンツ ネゴシエーションなどのさまざまなタスクを実行するグローバル サービスのセットが含まれています。</span><span class="sxs-lookup"><span data-stu-id="3e36b-154">The **HttpConfiguration.Services** collection contains a set of global services that Web API uses to perform various tasks, such as controller selection and content negotiation.</span></span>

> [!NOTE]
> <span data-ttu-id="3e36b-155">**サービス**コレクションは、サービスの検出または依存関係の挿入の汎用メカニズムではありません。</span><span class="sxs-lookup"><span data-stu-id="3e36b-155">The **Services** collection is not a general-purpose mechanism for service discovery or dependency injection.</span></span> <span data-ttu-id="3e36b-156">Web API フレームワークに認識されているサービスの種類のみ格納されます。</span><span class="sxs-lookup"><span data-stu-id="3e36b-156">It only stores service types that are known to the Web API framework.</span></span>


<span data-ttu-id="3e36b-157">**サービス**コレクションは、サービスの既定のセットで初期化されており、独自のカスタム実装を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="3e36b-157">The **Services** collection is initialized with a default set of services, and you can provide your own custom implementations.</span></span> <span data-ttu-id="3e36b-158">一部のサービスは、インスタンス 1 つだけを持つ他のユーザーに複数のインスタンスをサポートします。</span><span class="sxs-lookup"><span data-stu-id="3e36b-158">Some services support multiple instances, while others can have only one instance.</span></span> <span data-ttu-id="3e36b-159">(ただし、コント ローラー レベルでサービスを提供することもできます。 を参照してください[コント ローラーごとの構成](#percontrollerconfig)します。</span><span class="sxs-lookup"><span data-stu-id="3e36b-159">(However, you can also provide services at the controller level; see [Per-Controller Configuration](#percontrollerconfig).</span></span>

<span data-ttu-id="3e36b-160">単一インスタンス サービス</span><span class="sxs-lookup"><span data-stu-id="3e36b-160">Single-Instance Services</span></span>


| <span data-ttu-id="3e36b-161">サービス</span><span class="sxs-lookup"><span data-stu-id="3e36b-161">Service</span></span> | <span data-ttu-id="3e36b-162">説明</span><span class="sxs-lookup"><span data-stu-id="3e36b-162">Description</span></span> |
| --- | --- |
| <span data-ttu-id="3e36b-163">**IActionValueBinder**</span><span class="sxs-lookup"><span data-stu-id="3e36b-163">**IActionValueBinder**</span></span> | <span data-ttu-id="3e36b-164">パラメーターのバインドを取得します。</span><span class="sxs-lookup"><span data-stu-id="3e36b-164">Gets a binding for a parameter.</span></span> |
| <span data-ttu-id="3e36b-165">**IApiExplorer**</span><span class="sxs-lookup"><span data-stu-id="3e36b-165">**IApiExplorer**</span></span> | <span data-ttu-id="3e36b-166">アプリケーションによって公開される Api の説明を取得します。</span><span class="sxs-lookup"><span data-stu-id="3e36b-166">Gets descriptions of the APIs exposed by the application.</span></span> <span data-ttu-id="3e36b-167">参照してください[Web API のヘルプ ページを作成する](../getting-started-with-aspnet-web-api/creating-api-help-pages.md)します。</span><span class="sxs-lookup"><span data-stu-id="3e36b-167">See [Creating a Help Page for a Web API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md).</span></span> |
| <span data-ttu-id="3e36b-168">**IAssembliesResolver**</span><span class="sxs-lookup"><span data-stu-id="3e36b-168">**IAssembliesResolver**</span></span> | <span data-ttu-id="3e36b-169">アプリケーションのアセンブリの一覧を取得します。</span><span class="sxs-lookup"><span data-stu-id="3e36b-169">Gets a list of the assemblies for the application.</span></span> <span data-ttu-id="3e36b-170">参照してください[ルーティングとアクションの選択](../web-api-routing-and-actions/routing-and-action-selection.md)します。</span><span class="sxs-lookup"><span data-stu-id="3e36b-170">See [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span> |
| <span data-ttu-id="3e36b-171">**IBodyModelValidator**</span><span class="sxs-lookup"><span data-stu-id="3e36b-171">**IBodyModelValidator**</span></span> | <span data-ttu-id="3e36b-172">要求本文からメディア タイプ フォーマッタによって読み取られるモデルを検証します。</span><span class="sxs-lookup"><span data-stu-id="3e36b-172">Validates a model that is read from the request body by a media-type formatter.</span></span> |
| <span data-ttu-id="3e36b-173">**IContentNegotiator**</span><span class="sxs-lookup"><span data-stu-id="3e36b-173">**IContentNegotiator**</span></span> | <span data-ttu-id="3e36b-174">コンテンツ ネゴシエーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="3e36b-174">Performs content negotiation.</span></span> |
| <span data-ttu-id="3e36b-175">**IDocumentationProvider**</span><span class="sxs-lookup"><span data-stu-id="3e36b-175">**IDocumentationProvider**</span></span> | <span data-ttu-id="3e36b-176">Api のドキュメントを提供します。</span><span class="sxs-lookup"><span data-stu-id="3e36b-176">Provides documentation for APIs.</span></span> <span data-ttu-id="3e36b-177">既定値は**null**します。</span><span class="sxs-lookup"><span data-stu-id="3e36b-177">The default is **null**.</span></span> <span data-ttu-id="3e36b-178">参照してください[Web API のヘルプ ページを作成する](../getting-started-with-aspnet-web-api/creating-api-help-pages.md)します。</span><span class="sxs-lookup"><span data-stu-id="3e36b-178">See [Creating a Help Page for a Web API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md).</span></span> |
| <span data-ttu-id="3e36b-179">**IHostBufferPolicySelector**</span><span class="sxs-lookup"><span data-stu-id="3e36b-179">**IHostBufferPolicySelector**</span></span> | <span data-ttu-id="3e36b-180">ホストで HTTP メッセージのエンティティ ボディをバッファーする必要があるかどうかを示します。</span><span class="sxs-lookup"><span data-stu-id="3e36b-180">Indicates whether the host should buffer HTTP message entity bodies.</span></span> |
| <span data-ttu-id="3e36b-181">**IHttpActionInvoker**</span><span class="sxs-lookup"><span data-stu-id="3e36b-181">**IHttpActionInvoker**</span></span> | <span data-ttu-id="3e36b-182">コント ローラー アクションを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="3e36b-182">Invokes a controller action.</span></span> <span data-ttu-id="3e36b-183">参照してください[ルーティングとアクションの選択](../web-api-routing-and-actions/routing-and-action-selection.md)します。</span><span class="sxs-lookup"><span data-stu-id="3e36b-183">See [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span> |
| <span data-ttu-id="3e36b-184">**IHttpActionSelector**</span><span class="sxs-lookup"><span data-stu-id="3e36b-184">**IHttpActionSelector**</span></span> | <span data-ttu-id="3e36b-185">コント ローラーのアクションを選択します。</span><span class="sxs-lookup"><span data-stu-id="3e36b-185">Selects a controller action.</span></span> <span data-ttu-id="3e36b-186">参照してください[ルーティングとアクションの選択](../web-api-routing-and-actions/routing-and-action-selection.md)します。</span><span class="sxs-lookup"><span data-stu-id="3e36b-186">See [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span> |
| <span data-ttu-id="3e36b-187">**IHttpControllerActivator**</span><span class="sxs-lookup"><span data-stu-id="3e36b-187">**IHttpControllerActivator**</span></span> | <span data-ttu-id="3e36b-188">コント ローラーがアクティブにします。</span><span class="sxs-lookup"><span data-stu-id="3e36b-188">Activates a controller.</span></span> <span data-ttu-id="3e36b-189">参照してください[ルーティングとアクションの選択](../web-api-routing-and-actions/routing-and-action-selection.md)します。</span><span class="sxs-lookup"><span data-stu-id="3e36b-189">See [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span> |
| <span data-ttu-id="3e36b-190">**IHttpControllerSelector**</span><span class="sxs-lookup"><span data-stu-id="3e36b-190">**IHttpControllerSelector**</span></span> | <span data-ttu-id="3e36b-191">コント ローラーを選択します。</span><span class="sxs-lookup"><span data-stu-id="3e36b-191">Selects a controller.</span></span> <span data-ttu-id="3e36b-192">参照してください[ルーティングとアクションの選択](../web-api-routing-and-actions/routing-and-action-selection.md)します。</span><span class="sxs-lookup"><span data-stu-id="3e36b-192">See [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span> |
| <span data-ttu-id="3e36b-193">**IHttpControllerTypeResolver**</span><span class="sxs-lookup"><span data-stu-id="3e36b-193">**IHttpControllerTypeResolver**</span></span> | <span data-ttu-id="3e36b-194">アプリケーションで Web API コント ローラーの種類の一覧を示します。</span><span class="sxs-lookup"><span data-stu-id="3e36b-194">Provides a list of the Web API controller types in the application.</span></span> <span data-ttu-id="3e36b-195">参照してください[ルーティングとアクションの選択](../web-api-routing-and-actions/routing-and-action-selection.md)します。</span><span class="sxs-lookup"><span data-stu-id="3e36b-195">See [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span> |
| <span data-ttu-id="3e36b-196">**ITraceManager**</span><span class="sxs-lookup"><span data-stu-id="3e36b-196">**ITraceManager**</span></span> | <span data-ttu-id="3e36b-197">このトレース フレームワークを初期化します。</span><span class="sxs-lookup"><span data-stu-id="3e36b-197">Initializes the tracing framework.</span></span> <span data-ttu-id="3e36b-198">参照してください[ASP.NET Web API でのトレース](../testing-and-debugging/tracing-in-aspnet-web-api.md)します。</span><span class="sxs-lookup"><span data-stu-id="3e36b-198">See [Tracing in ASP.NET Web API](../testing-and-debugging/tracing-in-aspnet-web-api.md).</span></span> |
| <span data-ttu-id="3e36b-199">**ITraceWriter**</span><span class="sxs-lookup"><span data-stu-id="3e36b-199">**ITraceWriter**</span></span> | <span data-ttu-id="3e36b-200">トレース ライターを提供します。</span><span class="sxs-lookup"><span data-stu-id="3e36b-200">Provides a trace writer.</span></span> <span data-ttu-id="3e36b-201">既定では"no op"トレース ライターです。</span><span class="sxs-lookup"><span data-stu-id="3e36b-201">The default is a "no-op" trace writer.</span></span> <span data-ttu-id="3e36b-202">参照してください[ASP.NET Web API でのトレース](../testing-and-debugging/tracing-in-aspnet-web-api.md)します。</span><span class="sxs-lookup"><span data-stu-id="3e36b-202">See [Tracing in ASP.NET Web API](../testing-and-debugging/tracing-in-aspnet-web-api.md).</span></span> |
| <span data-ttu-id="3e36b-203">**IModelValidatorCache**</span><span class="sxs-lookup"><span data-stu-id="3e36b-203">**IModelValidatorCache**</span></span> | <span data-ttu-id="3e36b-204">モデル検証コントロールのキャッシュを提供します。</span><span class="sxs-lookup"><span data-stu-id="3e36b-204">Provides a cache of model validators.</span></span> |

<span data-ttu-id="3e36b-205">複数インスタンスのサービス</span><span class="sxs-lookup"><span data-stu-id="3e36b-205">Multiple-Instance Services</span></span>


|                 <span data-ttu-id="3e36b-206">サービス</span><span class="sxs-lookup"><span data-stu-id="3e36b-206">Service</span></span>                 |                                                                                                              <span data-ttu-id="3e36b-207">説明</span><span class="sxs-lookup"><span data-stu-id="3e36b-207">Description</span></span>                                                                                                               |
|-----------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <span data-ttu-id="3e36b-208"><strong>IFilterProvider</strong></span><span class="sxs-lookup"><span data-stu-id="3e36b-208"><strong>IFilterProvider</strong></span></span>     |                                                                                           <span data-ttu-id="3e36b-209">コント ローラー アクションのフィルターの一覧を返します。</span><span class="sxs-lookup"><span data-stu-id="3e36b-209">Returns a list of filters for a controller action.</span></span>                                                                                           |
|  <span data-ttu-id="3e36b-210"><strong>ModelBinderProvider</strong></span><span class="sxs-lookup"><span data-stu-id="3e36b-210"><strong>ModelBinderProvider</strong></span></span>   |                                                                                                <span data-ttu-id="3e36b-211">指定された型のモデル バインダーを返します。</span><span class="sxs-lookup"><span data-stu-id="3e36b-211">Returns a model binder for a given type.</span></span>                                                                                                |
| <span data-ttu-id="3e36b-212"><strong>ModelMetadataProvider</strong></span><span class="sxs-lookup"><span data-stu-id="3e36b-212"><strong>ModelMetadataProvider</strong></span></span>  |                                                                                                     <span data-ttu-id="3e36b-213">モデルのメタデータを提供します。</span><span class="sxs-lookup"><span data-stu-id="3e36b-213">Provides metadata for a model.</span></span>                                                                                                     |
| <span data-ttu-id="3e36b-214"><strong>ModelValidatorProvider</strong></span><span class="sxs-lookup"><span data-stu-id="3e36b-214"><strong>ModelValidatorProvider</strong></span></span> |                                                                                                   <span data-ttu-id="3e36b-215">モデルの検証コントロールを提供します。</span><span class="sxs-lookup"><span data-stu-id="3e36b-215">Provides a validator for a model.</span></span>                                                                                                    |
|  <span data-ttu-id="3e36b-216"><strong>ValueProviderFactory</strong></span><span class="sxs-lookup"><span data-stu-id="3e36b-216"><strong>ValueProviderFactory</strong></span></span>  | <span data-ttu-id="3e36b-217">値プロバイダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="3e36b-217">Creates a value provider.</span></span> <span data-ttu-id="3e36b-218">詳細については、Mike Stall のブログの投稿を参照してください[WebAPI でカスタム値プロバイダーを作成する方法。](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)</span><span class="sxs-lookup"><span data-stu-id="3e36b-218">For more information, see Mike Stall's blog post [How to create a custom value provider in WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)</span></span> |

<span data-ttu-id="3e36b-219">マルチ インスタンスのサービスには、カスタム実装を追加するには、呼び出す**追加**または**挿入**上、**サービス**コレクション。</span><span class="sxs-lookup"><span data-stu-id="3e36b-219">To add a custom implementation to a multi-instance service, call **Add** or **Insert** on the **Services** collection:</span></span>

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="3e36b-220">単一インスタンスのサービスをカスタム実装で置き換えるには、呼び出す**置換**上、**サービス**コレクション。</span><span class="sxs-lookup"><span data-stu-id="3e36b-220">To replace a single-instance service with a custom implementation, call **Replace** on the **Services** collection:</span></span>

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample6.cs)]

<a id="percontrollerconfig"></a>
## <a name="per-controller-configuration"></a><span data-ttu-id="3e36b-221">コント ローラーごとの構成</span><span class="sxs-lookup"><span data-stu-id="3e36b-221">Per-Controller Configuration</span></span>

<span data-ttu-id="3e36b-222">コント ローラーごとに、次の設定をオーバーライドすることができます。</span><span class="sxs-lookup"><span data-stu-id="3e36b-222">You can override the following settings on a per-controller basis:</span></span>

- <span data-ttu-id="3e36b-223">メディア タイプ フォーマッタ</span><span class="sxs-lookup"><span data-stu-id="3e36b-223">Media-type formatters</span></span>
- <span data-ttu-id="3e36b-224">パラメーター バインディング規則</span><span class="sxs-lookup"><span data-stu-id="3e36b-224">Parameter binding rules</span></span>
- <span data-ttu-id="3e36b-225">Services</span><span class="sxs-lookup"><span data-stu-id="3e36b-225">Services</span></span>

<span data-ttu-id="3e36b-226">これを行うには、実装するカスタム属性を定義、 **IControllerConfiguration**インターフェイス。</span><span class="sxs-lookup"><span data-stu-id="3e36b-226">To do so, define a custom attribute that implements the **IControllerConfiguration** interface.</span></span> <span data-ttu-id="3e36b-227">コント ローラーに、属性を適用します。</span><span class="sxs-lookup"><span data-stu-id="3e36b-227">Then apply the attribute to the controller.</span></span>

<span data-ttu-id="3e36b-228">次の例では、カスタム フォーマッタで既定のメディア タイプ フォーマッタを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="3e36b-228">The following example replaces the default media-type formatters with a custom formatter.</span></span>

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="3e36b-229">**IControllerConfiguration.Initialize**メソッドは 2 つのパラメーターを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="3e36b-229">The **IControllerConfiguration.Initialize** method takes two parameters:</span></span>

- <span data-ttu-id="3e36b-230">**HttpControllerSettings**オブジェクト</span><span class="sxs-lookup"><span data-stu-id="3e36b-230">An **HttpControllerSettings** object</span></span>
- <span data-ttu-id="3e36b-231">**HttpControllerDescriptor**オブジェクト</span><span class="sxs-lookup"><span data-stu-id="3e36b-231">An **HttpControllerDescriptor** object</span></span>

<span data-ttu-id="3e36b-232">**HttpControllerDescriptor**調べることができます (たとえば、2 つのコント ローラー間で区別するために) 情報提供を目的のコント ローラーの説明が含まれています。</span><span class="sxs-lookup"><span data-stu-id="3e36b-232">The **HttpControllerDescriptor** contains a description of the controller, which you can examine for informational purposes (say, to distinguish between two controllers).</span></span>

<span data-ttu-id="3e36b-233">使用して、 **HttpControllerSettings**コント ローラーを構成するオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="3e36b-233">Use the **HttpControllerSettings** object to configure the controller.</span></span> <span data-ttu-id="3e36b-234">このオブジェクトには、コント ローラーごとにオーバーライドできる構成パラメーターのサブセットが含まれています。</span><span class="sxs-lookup"><span data-stu-id="3e36b-234">This object contains the subset of configuration parameters that you can override on a per-controller basis.</span></span> <span data-ttu-id="3e36b-235">すべての設定は変更しないことを既定のグローバル**HttpConfiguration**オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="3e36b-235">Any settings that you don't change default to the global **HttpConfiguration** object.</span></span>
