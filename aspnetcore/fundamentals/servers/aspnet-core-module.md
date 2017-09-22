---
title: "ASP.NET Core モジュール"
author: tdykstra
description: "ASP.NET Core モジュール (ANCM)、IIS または IIS Express を使用して、リバース プロキシ サーバーとして Kestrel web サーバーができるように IIS モジュールが導入されています。"
keywords: "ASP.NET Core、IIS、IIS Express,ASP.NET コア モジュール、UseIISIntegration"
ms.author: tdykstra
manager: wpickett
ms.date: 08/03/2017
ms.topic: article
ms.assetid: 4661af33-34c5-4d71-93a0-8c7632f43580
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/aspnet-core-module
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 479fc3a389168bb08e278aa3d9da3bf7df1b49f4
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/22/2017
---
# <a name="introduction-to-aspnet-core-module"></a><span data-ttu-id="72c8c-104">ASP.NET Core モジュールの概要</span><span class="sxs-lookup"><span data-stu-id="72c8c-104">Introduction to ASP.NET Core Module</span></span>

<span data-ttu-id="72c8c-105">によって[Tom Dykstra](https://github.com/tdykstra)、 [Rick Strahl](https://github.com/RickStrahl)、および[Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="72c8c-105">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), and [Chris Ross](https://github.com/Tratcher)</span></span> 

<span data-ttu-id="72c8c-106">ASP.NET Core モジュール (ANCM) では、アプリケーション、IIS の背後にある ASP.NET Core を実行できますこれは、新機能 (セキュリティ、管理容易性、および多数詳細) でも適切に IIS を使用すると、を使用して[Kestrel](kestrel.md)には、新機能 (されている非常に高速)、でも適切に取得しています、。一度に 2 つのテクノロジからの利点があります。</span><span class="sxs-lookup"><span data-stu-id="72c8c-106">ASP.NET Core Module (ANCM) lets you run ASP.NET Core applications behind IIS, using IIS for what it's good at (security, manageability, and lots more) and using [Kestrel](kestrel.md) for what it's good at (being really fast), and getting the benefits from both technologies at once.</span></span> <span data-ttu-id="72c8c-107">**ANCM は Kestrel; でのみ機能します。WebListener と互換性がない (ASP.NET Core で 1.x)、または HTTP.sys (2.x) にします。**</span><span class="sxs-lookup"><span data-stu-id="72c8c-107">**ANCM works only with Kestrel; it isn't compatible with WebListener (in ASP.NET Core 1.x) or HTTP.sys (in 2.x).**</span></span> 

<span data-ttu-id="72c8c-108">サポートされている Windows のバージョン:</span><span class="sxs-lookup"><span data-stu-id="72c8c-108">Supported Windows versions:</span></span>

* <span data-ttu-id="72c8c-109">Windows 7 および Windows Server 2008 R2 以降</span><span class="sxs-lookup"><span data-stu-id="72c8c-109">Windows 7 and Windows Server 2008 R2 and later</span></span>

[<span data-ttu-id="72c8c-110">サンプル コードを表示またはダウンロードする</span><span class="sxs-lookup"><span data-stu-id="72c8c-110">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/aspnet-core-module/sample)

## <a name="what-aspnet-core-module-does"></a><span data-ttu-id="72c8c-111">ASP.NET Core モジュールの実行内容</span><span class="sxs-lookup"><span data-stu-id="72c8c-111">What ASP.NET Core Module does</span></span>

<span data-ttu-id="72c8c-112">ANCM は、IIS のパイプラインにフックし、トラフィックをバックエンド ASP.NET Core アプリケーションにリダイレクトするネイティブの IIS モジュールです。</span><span class="sxs-lookup"><span data-stu-id="72c8c-112">ANCM is a native IIS module that hooks into the IIS pipeline and redirects traffic to the backend ASP.NET Core application.</span></span> <span data-ttu-id="72c8c-113">Windows 認証など、他のほとんどのモジュールは、まだ実行する機会を取得します。</span><span class="sxs-lookup"><span data-stu-id="72c8c-113">Most other modules, such as windows authentication, still get a chance to run.</span></span> <span data-ttu-id="72c8c-114">ANCM はコントロールをだけは、ハンドラーが、要求を選択し、ハンドラー マッピングがアプリケーションで定義されている場合に*web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="72c8c-114">ANCM only takes control when a handler is selected for the request, and handler mapping is defined in the application *web.config* file.</span></span>

<span data-ttu-id="72c8c-115">IIS ワーカー プロセスからの個別のプロセスで実行する ASP.NET Core アプリケーション、ため ANCM もが、処理の管理。</span><span class="sxs-lookup"><span data-stu-id="72c8c-115">Because ASP.NET Core applications run in a process separate from the IIS worker process, ANCM also does process management.</span></span> <span data-ttu-id="72c8c-116">最初の要求はクラッシュして再起動し、ANCM は ASP.NET Core アプリケーションのプロセスを開始します。</span><span class="sxs-lookup"><span data-stu-id="72c8c-116">ANCM starts the process for the ASP.NET Core application when the first request comes in and restarts it when it crashes.</span></span> <span data-ttu-id="72c8c-117">これは、従来の ASP.NET アプリケーションと基本的に同じ動作を実行するインプロセス iis および WAS (Windows ライセンス認証サービスなど) によって管理されています。</span><span class="sxs-lookup"><span data-stu-id="72c8c-117">This is essentially the same behavior as classic ASP.NET applications that run in-process in IIS and are managed by WAS (Windows Activation Service).</span></span>

<span data-ttu-id="72c8c-118">ここでは、IIS、ANCM、および ASP.NET Core アプリケーションの間のリレーションシップを示す図。</span><span class="sxs-lookup"><span data-stu-id="72c8c-118">Here's a diagram that illustrates the relationship between IIS, ANCM, and ASP.NET Core applications.</span></span>

![ASP.NET Core モジュール](aspnet-core-module/_static/ancm.png)

<span data-ttu-id="72c8c-120">要求は、Web から受け取るし、プライマリ ポート (80) または SSL ポート (443) 上の IIS にルーティングする、カーネル モード Http.Sys ドライバーをヒットします。</span><span class="sxs-lookup"><span data-stu-id="72c8c-120">Requests come in from the Web and hit the kernel mode Http.Sys driver which routes them into IIS on the primary port (80) or SSL port (443).</span></span> <span data-ttu-id="72c8c-121">ANCM は、ポート 80 および 443 ではないアプリケーション用に構成された HTTP ポート上の ASP.NET Core アプリケーションに要求を転送します。</span><span class="sxs-lookup"><span data-stu-id="72c8c-121">ANCM forwards the requests to the ASP.NET Core application on the HTTP port configured for the application, which is not port 80/443.</span></span>

<span data-ttu-id="72c8c-122">Kestrel は、ANCM からのトラフィックをリッスンします。</span><span class="sxs-lookup"><span data-stu-id="72c8c-122">Kestrel listens for traffic coming from ANCM.</span></span>  <span data-ttu-id="72c8c-123">ANCM が起動時に、環境変数を使用してポートを指定し、 [UseIISIntegration](#call-useiisintegration)メソッドでリッスンするサーバーの構成`http://localhost:{port}`です。</span><span class="sxs-lookup"><span data-stu-id="72c8c-123">ANCM specifies the port via environment variable at startup, and the [UseIISIntegration](#call-useiisintegration) method configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="72c8c-124">これは、追加のチェック ANCM からではなく要求を拒否します。</span><span class="sxs-lookup"><span data-stu-id="72c8c-124">There are additional checks to reject requests not from ANCM.</span></span> <span data-ttu-id="72c8c-125">(ANCM はサポートされません HTTPS 転送、HTTPS 経由で IIS によって受信された場合でも要求が HTTP 経由で転送されるようにします。)</span><span class="sxs-lookup"><span data-stu-id="72c8c-125">(ANCM does not support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.)</span></span>

<span data-ttu-id="72c8c-126">Kestrel が ANCM から要求を取得し、それらにし、それらを処理し、それらとして ASP.NET Core のミドルウェア パイプラインにプッシュ`HttpContext`アプリケーション ロジックのインスタンス。</span><span class="sxs-lookup"><span data-stu-id="72c8c-126">Kestrel picks up requests from ANCM and pushes them into the ASP.NET Core middleware pipeline, which then handles them and passes them on as `HttpContext` instances to application logic.</span></span> <span data-ttu-id="72c8c-127">アプリケーションの応答は、IIS、それらクライアントに返信、HTTP 要求を開始したどのプッシュに渡されます。</span><span class="sxs-lookup"><span data-stu-id="72c8c-127">The application's responses are then passed back to IIS, which pushes them back out to the HTTP client that initiated the requests.</span></span>

<span data-ttu-id="72c8c-128">ANCM が他のいくつかの機能もあります。</span><span class="sxs-lookup"><span data-stu-id="72c8c-128">ANCM has a few other functions as well:</span></span>

* <span data-ttu-id="72c8c-129">環境変数を設定します。</span><span class="sxs-lookup"><span data-stu-id="72c8c-129">Sets environment variables.</span></span>
* <span data-ttu-id="72c8c-130">ログ`stdout`ファイル記憶域に出力します。</span><span class="sxs-lookup"><span data-stu-id="72c8c-130">Logs `stdout` output to file storage.</span></span>
* <span data-ttu-id="72c8c-131">Windows 認証トークンを転送します。</span><span class="sxs-lookup"><span data-stu-id="72c8c-131">Forwards Windows authentication tokens.</span></span>

## <a name="how-to-use-ancm-in-aspnet-core-apps"></a><span data-ttu-id="72c8c-132">ASP.NET Core アプリケーションで ANCM を使用する方法</span><span class="sxs-lookup"><span data-stu-id="72c8c-132">How to use ANCM in ASP.NET Core apps</span></span>

<span data-ttu-id="72c8c-133">このセクションでは、IIS サーバーおよび ASP.NET Core アプリケーションを設定するため、プロセスの概要を示します。</span><span class="sxs-lookup"><span data-stu-id="72c8c-133">This section provides an overview of the process for setting up an IIS server and ASP.NET Core application.</span></span> <span data-ttu-id="72c8c-134">詳細については、次を参照してください。[を IIS に発行](../../publishing/iis.md)です。</span><span class="sxs-lookup"><span data-stu-id="72c8c-134">For detailed instructions, see [Publishing to IIS](../../publishing/iis.md).</span></span>

### <a name="install-ancm"></a><span data-ttu-id="72c8c-135">ANCM をインストールします。</span><span class="sxs-lookup"><span data-stu-id="72c8c-135">Install ANCM</span></span>

<span data-ttu-id="72c8c-136">ASP.NET Core モジュールは、インストールする IIS で、サーバーと IIS Express で、開発用コンピューターで持っています。</span><span class="sxs-lookup"><span data-stu-id="72c8c-136">The ASP.NET Core Module has to be installed in IIS on your servers and in IIS Express on your development machines.</span></span> <span data-ttu-id="72c8c-137">サーバーでは、ANCM に含まれる、 [.NET コア Windows Server をホストしているバンドル](https://aka.ms/dotnetcore.2.0.0-windowshosting)です。</span><span class="sxs-lookup"><span data-stu-id="72c8c-137">For servers, ANCM is included in the [.NET Core Windows Server Hosting bundle](https://aka.ms/dotnetcore.2.0.0-windowshosting).</span></span> <span data-ttu-id="72c8c-138">開発用コンピューターの Visual Studio 自動的にインストール ANCM IIS および IIS Express で、コンピューターに既にインストールされている場合。</span><span class="sxs-lookup"><span data-stu-id="72c8c-138">For development machines, Visual Studio automatically installs ANCM in IIS Express, and in IIS if it is already installed on the machine.</span></span>

### <a name="install-the-iisintegration-nuget-package"></a><span data-ttu-id="72c8c-139">IISIntegration NuGet パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="72c8c-139">Install the IISIntegration NuGet package</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="72c8c-140">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="72c8c-140">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="72c8c-141">[Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) ASP.NET Core metapackages にパッケージが含まれます ([Microsoft.AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore/)と[Microsoft.AspNetCore.All](xref:fundamentals/metapackage)).</span><span class="sxs-lookup"><span data-stu-id="72c8c-141">The [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) package is included in the ASP.NET Core metapackages ([Microsoft.AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore/) and [Microsoft.AspNetCore.All](xref:fundamentals/metapackage)).</span></span> <span data-ttu-id="72c8c-142">を使用しない、metapackages のいずれかの場合は、インストール`Microsoft.AspNetCore.Server.IISIntegration`とは別にします。</span><span class="sxs-lookup"><span data-stu-id="72c8c-142">If you don't use one of the metapackages, install `Microsoft.AspNetCore.Server.IISIntegration` separately.</span></span> <span data-ttu-id="72c8c-143">`IISIntegration`パッケージは、アプリを設定する ANCM によってブロードキャストされた環境変数を読み取るの相互運用性パックします。</span><span class="sxs-lookup"><span data-stu-id="72c8c-143">The `IISIntegration` package is an interoperability pack that reads environment variables broadcast by ANCM to set up your app.</span></span> <span data-ttu-id="72c8c-144">環境変数では、リッスンするポートなどの構成情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="72c8c-144">The environment variables provide configuration information, such as the port to listen on.</span></span> 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="72c8c-145">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="72c8c-145">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="72c8c-146">アプリケーションでインストール[Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/)です。</span><span class="sxs-lookup"><span data-stu-id="72c8c-146">In your application, install [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/).</span></span> <span data-ttu-id="72c8c-147">`IISIntegration`パッケージは、アプリを設定する ANCM によってブロードキャストされた環境変数を読み取るの相互運用性パックします。</span><span class="sxs-lookup"><span data-stu-id="72c8c-147">The `IISIntegration` package  is an interoperability pack that reads environment variables broadcast by ANCM to set up your app.</span></span> <span data-ttu-id="72c8c-148">環境変数では、リッスンするポートなどの構成情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="72c8c-148">The environment variables provide configuration information, such as the port to listen on.</span></span> 

---

### <a name="call-useiisintegration"></a><span data-ttu-id="72c8c-149">呼び出し UseIISIntegration</span><span class="sxs-lookup"><span data-stu-id="72c8c-149">Call UseIISIntegration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="72c8c-150">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="72c8c-150">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="72c8c-151">`UseIISIntegration`拡張メソッドを[ `WebHostBuilder` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) IIS を実行したときに自動的に呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="72c8c-151">The `UseIISIntegration` extension method on [`WebHostBuilder`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) is called automatically when you run with IIS.</span></span>

<span data-ttu-id="72c8c-152">ASP.NET Core metapackages のいずれかを使用していないしがインストールされていないかどうか、`Microsoft.AspNetCore.Server.IISIntegration`パッケージ、実行時エラーを取得します。</span><span class="sxs-lookup"><span data-stu-id="72c8c-152">If you aren't using one of the ASP.NET Core metapackages and haven't installed the  `Microsoft.AspNetCore.Server.IISIntegration` package, you get a runtime error.</span></span> <span data-ttu-id="72c8c-153">呼び出す場合`UseIISIntegration`パッケージがインストールされていない場合、コンパイル時エラーに明示的に、取得します。</span><span class="sxs-lookup"><span data-stu-id="72c8c-153">If you call `UseIISIntegration` explicitly, you get a compile time error if the package isn't installed.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="72c8c-154">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="72c8c-154">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="72c8c-155">アプリケーションの`Main`メソッドを呼び出し、`UseIISIntegration`拡張メソッドを[ `WebHostBuilder`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder)です。</span><span class="sxs-lookup"><span data-stu-id="72c8c-155">In your application's `Main` method, call the `UseIISIntegration` extension method on [`WebHostBuilder`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> 

[!code-csharp[](aspnet-core-module/sample/Program.cs?name=snippet_Main&highlight=12)]

---

<span data-ttu-id="72c8c-156">`UseIISIntegration`メソッドの ANCM 設定すると、環境変数とそのキャッシュなしでこれらがない場合。</span><span class="sxs-lookup"><span data-stu-id="72c8c-156">The `UseIISIntegration` method looks for environment variables that ANCM sets, and it no-ops if they aren't found.</span></span> <span data-ttu-id="72c8c-157">この動作には、開発および macOS または Linux でのテストと IIS を実行しているサーバーへの展開のようなシナリオが容易になります。</span><span class="sxs-lookup"><span data-stu-id="72c8c-157">This behavior facilitates scenarios like developing and testing on macOS or Linux and deploying to a server that runs IIS.</span></span> <span data-ttu-id="72c8c-158">で macOS または Linux を実行しているときに、Kestrel が web サーバーとして機能します。ただし、アプリが IIS 環境に展開されると、自動的に ANCM と IIS 使用されます。</span><span class="sxs-lookup"><span data-stu-id="72c8c-158">While running on macOS or Linux, Kestrel acts as the web server; but when the app is deployed to the IIS environment, it automatically uses ANCM and IIS.</span></span>

### <a name="ancm-port-binding-overrides-other-port-bindings"></a><span data-ttu-id="72c8c-159">ANCM ポートのバインドが他のポートのバインドをオーバーライドします</span><span class="sxs-lookup"><span data-stu-id="72c8c-159">ANCM port binding overrides other port bindings</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="72c8c-160">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="72c8c-160">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="72c8c-161">ANCM には、バックエンド プロセスに代入する動的なポートが生成されます。</span><span class="sxs-lookup"><span data-stu-id="72c8c-161">ANCM generates a dynamic port to assign to the back-end process.</span></span> <span data-ttu-id="72c8c-162">`UseIISIntegration`メソッドは、この動的なポートを取得し、Kestrel でリッスンするように構成`http://locahost:{dynamicPort}/`です。</span><span class="sxs-lookup"><span data-stu-id="72c8c-162">The `UseIISIntegration` method picks up this dynamic port and configures Kestrel to listen on `http://locahost:{dynamicPort}/`.</span></span> <span data-ttu-id="72c8c-163">呼び出しなど、その他の URL の構成が上書きされます。`UseUrls`または[Kestrel のリッスン API](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration)です。</span><span class="sxs-lookup"><span data-stu-id="72c8c-163">This overrides other URL configurations, such as calls to `UseUrls` or [Kestrel's Listen API](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span> <span data-ttu-id="72c8c-164">呼び出す必要はありませんので、`UseUrls`または Kestrel の`Listen`API ANCM を使用するとします。</span><span class="sxs-lookup"><span data-stu-id="72c8c-164">Therefore, you don't need to call `UseUrls` or Kestrel's `Listen` API when you use ANCM.</span></span> <span data-ttu-id="72c8c-165">呼び出す場合`UseUrls`または`Listen`Kestrel は IIS ことがなくアプリを実行するときに指定したポートでリッスンします。</span><span class="sxs-lookup"><span data-stu-id="72c8c-165">If you do call `UseUrls` or `Listen`, Kestrel listens on the port you specify when you run the app without IIS.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="72c8c-166">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="72c8c-166">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="72c8c-167">ANCM には、バックエンド プロセスに代入する動的なポートが生成されます。</span><span class="sxs-lookup"><span data-stu-id="72c8c-167">ANCM generates a dynamic port to assign to the back-end process.</span></span> <span data-ttu-id="72c8c-168">`UseIISIntegration`メソッドは、この動的なポートを取得し、Kestrel でリッスンするように構成`http://locahost:{dynamicPort}/`です。</span><span class="sxs-lookup"><span data-stu-id="72c8c-168">The `UseIISIntegration` method picks up this dynamic port and configures Kestrel to listen on `http://locahost:{dynamicPort}/`.</span></span> <span data-ttu-id="72c8c-169">呼び出しなど、その他の URL の構成が上書きされます。`UseUrls`です。</span><span class="sxs-lookup"><span data-stu-id="72c8c-169">This overrides other URL configurations, such as calls to `UseUrls`.</span></span> <span data-ttu-id="72c8c-170">呼び出す必要はありませんので、 `UseUrls` ANCM を使用するとします。</span><span class="sxs-lookup"><span data-stu-id="72c8c-170">Therefore, you don't need to call `UseUrls` when you use ANCM.</span></span> <span data-ttu-id="72c8c-171">呼び出す場合`UseUrls`Kestrel は IIS ことがなくアプリを実行するときに指定したポートでリッスンします。</span><span class="sxs-lookup"><span data-stu-id="72c8c-171">If you do call `UseUrls`, Kestrel listens on the port you specify when you run the app without IIS.</span></span>

<span data-ttu-id="72c8c-172">呼び出す場合に、ASP.NET Core 1.0 で`UseUrls`、それを呼び出す**する前に**呼び出す`UseIISIntegration`ANCM に構成されたポートの上書きを取得しないようにします。</span><span class="sxs-lookup"><span data-stu-id="72c8c-172">In ASP.NET Core 1.0, if you call `UseUrls`, call it **before** you call `UseIISIntegration` so that the ANCM-configured port doesn't get overwritten.</span></span> <span data-ttu-id="72c8c-173">ANCM 設定より優先されるため、この呼び出しの順序が ASP.NET Core 1.1 のため必要はありません`UseUrls`です。</span><span class="sxs-lookup"><span data-stu-id="72c8c-173">This calling order isn't required in ASP.NET Core 1.1, because the ANCM setting overrides `UseUrls`.</span></span>

---

### <a name="configure-ancm-options-in-webconfig"></a><span data-ttu-id="72c8c-174">Web.config で ANCM オプションを構成します。</span><span class="sxs-lookup"><span data-stu-id="72c8c-174">Configure ANCM options in Web.config</span></span>

<span data-ttu-id="72c8c-175">ASP.NET Core モジュールの構成に保存、 *Web.config*アプリケーションのルート フォルダーに配置されているファイル。</span><span class="sxs-lookup"><span data-stu-id="72c8c-175">Configuration for the ASP.NET Core Module is stored in the *Web.config* file that is located in the application's root folder.</span></span> <span data-ttu-id="72c8c-176">このファイルの設定は、スタートアップ コマンドおよび ASP.NET Core アプリを起動する引数をポイントします。</span><span class="sxs-lookup"><span data-stu-id="72c8c-176">Settings in this file point to the startup command and arguments that start your ASP.NET Core app.</span></span> <span data-ttu-id="72c8c-177">Web.config のサンプル コードと構成オプションの詳細については、次を参照してください。 [ASP.NET コア モジュールの構成の参照](../../hosting/aspnet-core-module.md)です。</span><span class="sxs-lookup"><span data-stu-id="72c8c-177">For sample Web.config code and guidance on configuration options, see [ASP.NET Core Module Configuration Reference](../../hosting/aspnet-core-module.md).</span></span>

### <a name="run-with-iis-express-in-development"></a><span data-ttu-id="72c8c-178">IIS Express を使って開発での実行します。</span><span class="sxs-lookup"><span data-stu-id="72c8c-178">Run with IIS Express in development</span></span>

<span data-ttu-id="72c8c-179">IIS Express は、Visual Studio の ASP.NET Core テンプレートで定義されている既定のプロファイルを使用して起動できます。</span><span class="sxs-lookup"><span data-stu-id="72c8c-179">IIS Express can be launched by Visual Studio using the default profile defined by the ASP.NET Core templates.</span></span>

## <a name="next-steps"></a><span data-ttu-id="72c8c-180">次のステップ</span><span class="sxs-lookup"><span data-stu-id="72c8c-180">Next steps</span></span>

<span data-ttu-id="72c8c-181">詳細については、次のリソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="72c8c-181">For more information, see the following resources:</span></span>

* [<span data-ttu-id="72c8c-182">この記事のサンプル アプリ</span><span class="sxs-lookup"><span data-stu-id="72c8c-182">Sample app for this article</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/aspnet-core-module/sample)
* [<span data-ttu-id="72c8c-183">ASP.NET Core モジュールのソース コード</span><span class="sxs-lookup"><span data-stu-id="72c8c-183">ASP.NET Core Module source code</span></span>](https://github.com/aspnet/AspNetCoreModule)
* [<span data-ttu-id="72c8c-184">ASP.NET Core モジュール構成リファレンス</span><span class="sxs-lookup"><span data-stu-id="72c8c-184">ASP.NET Core Module Configuration Reference</span></span>](../../hosting/aspnet-core-module.md)
* [<span data-ttu-id="72c8c-185">IIS に発行します。</span><span class="sxs-lookup"><span data-stu-id="72c8c-185">Publishing to IIS</span></span>](../../publishing/iis.md)
