---
title: ASP.NET Core での Web サーバーの実装
author: guardrex
description: ASP.NET Core の Web サーバー Kestrel と HTTP.sys を検出します。 サーバーを選択する方法と、リバース プロキシ サーバーを使用するタイミングについて説明します。
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/13/2019
uid: fundamentals/servers/index
ms.openlocfilehash: 672fe2ce6fd0adae09c380fe508344a254f1a9fe
ms.sourcegitcommit: 6ba5fb1fd0b7f9a6a79085b0ef56206e462094b7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2019
ms.locfileid: "56248135"
---
# <a name="web-server-implementations-in-aspnet-core"></a><span data-ttu-id="bad51-104">ASP.NET Core での Web サーバーの実装</span><span class="sxs-lookup"><span data-stu-id="bad51-104">Web server implementations in ASP.NET Core</span></span>

<span data-ttu-id="bad51-105">作成者: [Tom Dykstra](https://github.com/tdykstra)、[Steve Smith](https://ardalis.com/)、[Stephen Halter](https://twitter.com/halter73)、[Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="bad51-105">By [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="bad51-106">ASP.NET Core アプリは、インプロセス HTTP サーバー実装を使用して実行されます。</span><span class="sxs-lookup"><span data-stu-id="bad51-106">An ASP.NET Core app runs with an in-process HTTP server implementation.</span></span> <span data-ttu-id="bad51-107">サーバーの実装では、HTTP 要求がリッスンされ、<xref:Microsoft.AspNetCore.Http.HttpContext> に構成された[要求機能](xref:fundamentals/request-features)のセットとしてアプリに公開されます。</span><span class="sxs-lookup"><span data-stu-id="bad51-107">The server implementation listens for HTTP requests and surfaces them to the app as a set of [request features](xref:fundamentals/request-features) composed into an <xref:Microsoft.AspNetCore.Http.HttpContext>.</span></span>

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="bad51-108">Windows</span><span class="sxs-lookup"><span data-stu-id="bad51-108">Windows</span></span>](#tab/windows)

<span data-ttu-id="bad51-109">ASP.NET Core には次のものが付属しています。</span><span class="sxs-lookup"><span data-stu-id="bad51-109">ASP.NET Core ships with the following:</span></span>

* <span data-ttu-id="bad51-110">[Kestrel サーバー](xref:fundamentals/servers/kestrel)は、クロスプラットフォーム HTTP サーバーの既定の実装です。</span><span class="sxs-lookup"><span data-stu-id="bad51-110">[Kestrel server](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server implementation.</span></span>
* <span data-ttu-id="bad51-111">IIS HTTP サーバーは、IIS 用の[インプロセス サーバー](#in-process-hosting-model)です。</span><span class="sxs-lookup"><span data-stu-id="bad51-111">IIS HTTP Server is an [in-process server](#in-process-hosting-model) for IIS.</span></span>
* <span data-ttu-id="bad51-112">[HTTP.sys サーバー](xref:fundamentals/servers/httpsys)は、[HTTP.sys カーネル ドライバーおよび HTTP サーバー API](/windows/desktop/Http/http-api-start-page) に基づく Windows 専用の HTTP サーバーです。</span><span class="sxs-lookup"><span data-stu-id="bad51-112">[HTTP.sys server](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](/windows/desktop/Http/http-api-start-page).</span></span>

<span data-ttu-id="bad51-113">[IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) または [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) を使用すると、アプリは次のどちらかで実行されます。</span><span class="sxs-lookup"><span data-stu-id="bad51-113">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), the app either runs:</span></span>

* <span data-ttu-id="bad51-114">[IIS HTTP サーバー](#iis-http-server)を使用して IIS ワーカー プロセスと同じプロセス内 ([インプロセス ホスティング モデル](#in-process-hosting-model))。</span><span class="sxs-lookup"><span data-stu-id="bad51-114">In the same process as the IIS worker process (the [in-process hosting model](#in-process-hosting-model)) with the [IIS HTTP Server](#iis-http-server).</span></span> <span data-ttu-id="bad51-115">"*インプロセス*" が推奨される構成です。</span><span class="sxs-lookup"><span data-stu-id="bad51-115">*In-process* is the recommended configuration.</span></span>
* <span data-ttu-id="bad51-116">[Kestrel サーバー](#kestrel)を使用して IIS ワーカー プロセスとは異なるプロセス内 ([プロセス外ホスティング モデル](#out-of-process-hosting-model))。</span><span class="sxs-lookup"><span data-stu-id="bad51-116">In a process separate from the IIS worker process (the [out-of-process hosting model](#out-of-process-hosting-model)) with the [Kestrel server](#kestrel).</span></span>

<span data-ttu-id="bad51-117">[ASP.NET Core モジュール](xref:host-and-deploy/aspnet-core-module)はネイティブの IIS モジュールであり、IIS とインプロセス IIS HTTP サーバーまたは Kestrel の間のネイティブ IIS 要求が処理されます。</span><span class="sxs-lookup"><span data-stu-id="bad51-117">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) is a native IIS module that handles native IIS requests between IIS and the in-process IIS HTTP Server or Kestrel.</span></span> <span data-ttu-id="bad51-118">詳細については、「<xref:host-and-deploy/aspnet-core-module>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="bad51-118">For more information, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

## <a name="hosting-models"></a><span data-ttu-id="bad51-119">ホスティング モデル</span><span class="sxs-lookup"><span data-stu-id="bad51-119">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="bad51-120">インプロセス ホスティング モデル</span><span class="sxs-lookup"><span data-stu-id="bad51-120">In-process hosting model</span></span>

<span data-ttu-id="bad51-121">インプロセス ホスティング モデルを使用する場合、ASP.NET Core アプリはその IIS ワーカー プロセスと同じプロセスで実行されます。</span><span class="sxs-lookup"><span data-stu-id="bad51-121">Using in-process hosting, an ASP.NET Core app runs in the same process as its IIS worker process.</span></span> <span data-ttu-id="bad51-122">インプロセス ホスティングは、パフォーマンスの点でアウトプロセス ホスティングよりも優れています。要求がループバック アダプター (発信されたネットワーク トラフィックを同じコンピューターに送り返すネットワーク インターフェイス) を介してプロキシされることがないからです。</span><span class="sxs-lookup"><span data-stu-id="bad51-122">In-process hosting provides improved performance over out-of-process hosting because requests aren't proxied over the loopback adapter, a network interface that returns outgoing network traffic back to the same machine.</span></span> <span data-ttu-id="bad51-123">IIS では [Windows プロセス アクティブ化サービス (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) を使用してプロセス管理が処理されます。</span><span class="sxs-lookup"><span data-stu-id="bad51-123">IIS handles process management with the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="bad51-124">ASP.NET Core モジュール:</span><span class="sxs-lookup"><span data-stu-id="bad51-124">The ASP.NET Core Module:</span></span>

* <span data-ttu-id="bad51-125">アプリの初期化を実行します。</span><span class="sxs-lookup"><span data-stu-id="bad51-125">Performs app initialization.</span></span>
  * <span data-ttu-id="bad51-126">[CoreCLR](/dotnet/standard/glossary#coreclr) を読み込みます。</span><span class="sxs-lookup"><span data-stu-id="bad51-126">Loads the [CoreCLR](/dotnet/standard/glossary#coreclr).</span></span>
  * <span data-ttu-id="bad51-127">`Program.Main`.</span><span class="sxs-lookup"><span data-stu-id="bad51-127">Calls `Program.Main`.</span></span>
* <span data-ttu-id="bad51-128">IIS ネイティブ要求の有効期間を処理します。</span><span class="sxs-lookup"><span data-stu-id="bad51-128">Handles the lifetime of the IIS native request.</span></span>

<span data-ttu-id="bad51-129">.NET Framework をターゲットにする ASP.NET Core アプリではインプロセス ホスティング モデルはサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="bad51-129">The in-process hosting model isn't supported for ASP.NET Core apps that target the .NET Framework.</span></span>

<span data-ttu-id="bad51-130">次の図は、IIS (ASP.NET Core モジュール) とインプロセスでホストされるアプリとの間のリレーションシップを示しています。</span><span class="sxs-lookup"><span data-stu-id="bad51-130">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted in-process:</span></span>

![ASP.NET Core モジュール](_static/ancm-inprocess.png)

<span data-ttu-id="bad51-132">Web からカーネル モードの HTTP.sys ドライバーに要求が届きます。</span><span class="sxs-lookup"><span data-stu-id="bad51-132">A request arrives from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="bad51-133">このドライバーによって、Web サイトの構成ポート (通常は 80 (HTTP) または 443 (HTTPS)) 上で IIS へのネイティブ要求がルーティングされます。</span><span class="sxs-lookup"><span data-stu-id="bad51-133">The driver routes the native request to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="bad51-134">モーダルは、ネイティブ要求を受け取り、それを IIS HTTP サーバー (`IISHttpServer`) に渡します。</span><span class="sxs-lookup"><span data-stu-id="bad51-134">The module receives the native request and passes it to IIS HTTP Server (`IISHttpServer`).</span></span> <span data-ttu-id="bad51-135">IIS HTTP サーバーは IIS 用のインプロセス サーバーの実装であり、要求がネイティブからマネージドに変換されます。</span><span class="sxs-lookup"><span data-stu-id="bad51-135">IIS HTTP Server is an in-process server implementation for IIS that converts the request from native to managed.</span></span>

<span data-ttu-id="bad51-136">IIS HTTP サーバーによって要求が処理された後、その要求は ASP.NET Core ミドルウェア パイプラインにプッシュされます。</span><span class="sxs-lookup"><span data-stu-id="bad51-136">After the IIS HTTP Server processes the request, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="bad51-137">ミドルウェア パイプラインは要求を処理し、`HttpContext` インスタンスとしてアプリのロジックに渡します。</span><span class="sxs-lookup"><span data-stu-id="bad51-137">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="bad51-138">アプリの応答が IIS に渡され、その応答は IIS から要求を開始したクライアントにプッシュで戻されます。</span><span class="sxs-lookup"><span data-stu-id="bad51-138">The app's response is passed back to IIS, which pushes it back out to the client that initiated the request.</span></span>

<span data-ttu-id="bad51-139">インプロセス ホスティングは既存のアプリではオプトインになっていますが、[dotnet new](/dotnet/core/tools/dotnet-new) テンプレートは既定ではすべての IIS および IIS Express シナリオにおいてインプロセス ホスティング モデルに設定されています。</span><span class="sxs-lookup"><span data-stu-id="bad51-139">In-process hosting is opt-in for existing apps, but [dotnet new](/dotnet/core/tools/dotnet-new) templates default to the in-process hosting model for all IIS and IIS Express scenarios.</span></span>

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="bad51-140">アウト プロセス ホスティング モデル</span><span class="sxs-lookup"><span data-stu-id="bad51-140">Out-of-process hosting model</span></span>

<span data-ttu-id="bad51-141">ASP.NET Core アプリは IIS ワーカー プロセスとは独立したプロセスで実行されるため、プロセス管理はモジュールによって処理されます。</span><span class="sxs-lookup"><span data-stu-id="bad51-141">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module handles process management.</span></span> <span data-ttu-id="bad51-142">モジュールでは、最初の要求が届いたときに ASP.NET Core アプリのプロセスが開始され、プロセスがシャットダウンまたはクラッシュした場合はアプリが再起動されます。</span><span class="sxs-lookup"><span data-stu-id="bad51-142">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it shuts down or crashes.</span></span> <span data-ttu-id="bad51-143">この動作は、インプロセスで実行されるアプリであり、[WAS (Windows プロセス アクティブ化サービス)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) によって管理されるアプリと基本的に同じです。</span><span class="sxs-lookup"><span data-stu-id="bad51-143">This is essentially the same behavior as seen with apps that run in-process that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="bad51-144">次の図は、IIS (ASP.NET Core モジュール) とアウトプロセスでホストされるアプリとの間のリレーションシップを示しています。</span><span class="sxs-lookup"><span data-stu-id="bad51-144">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted out-of-process:</span></span>

![ASP.NET Core モジュール](_static/ancm-outofprocess.png)

<span data-ttu-id="bad51-146">要求は、Web からカーネル モードの HTTP.sys ドライバーに到着します。</span><span class="sxs-lookup"><span data-stu-id="bad51-146">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="bad51-147">ドライバーは、Web サイトの構成ポート (通常は 80 (HTTP) または 443 (HTTPS)) で IIS への要求をルーティングします。</span><span class="sxs-lookup"><span data-stu-id="bad51-147">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="bad51-148">モジュールでは、アプリのランダムなポート (ポート 80 または 443 ではありません) で Kestrel に要求が転送されます。</span><span class="sxs-lookup"><span data-stu-id="bad51-148">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="bad51-149">モジュールが起動時に環境変数を介してポートを指定すると、サーバーは `http://localhost:{PORT}` をリッスンするように、IIS 統合ミドルウェアによって構成されます。</span><span class="sxs-lookup"><span data-stu-id="bad51-149">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{PORT}`.</span></span> <span data-ttu-id="bad51-150">追加のチェックが実行され、モジュールから発生していない要求は拒否されます。</span><span class="sxs-lookup"><span data-stu-id="bad51-150">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="bad51-151">モジュールでは HTTPS 転送がサポートされていないため、要求は HTTPS を介して IIS によって受信された場合でも、HTTP を介して転送されます。</span><span class="sxs-lookup"><span data-stu-id="bad51-151">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="bad51-152">Kestrel によってモジュールから要求が取り込まれた後、その要求は ASP.NET Core ミドルウェア パイプラインにプッシュされます。</span><span class="sxs-lookup"><span data-stu-id="bad51-152">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="bad51-153">ミドルウェア パイプラインは要求を処理し、`HttpContext` インスタンスとしてアプリのロジックに渡します。</span><span class="sxs-lookup"><span data-stu-id="bad51-153">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="bad51-154">IIS 統合によって追加されたミドルウェアでは、カーネルへの要求の転送を考慮して、スキーム、リモート IP、およびパスベースが更新されます。</span><span class="sxs-lookup"><span data-stu-id="bad51-154">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="bad51-155">アプリの応答が IIS に戻され、IIS はその応答を、要求を開始した HTTP クライアントに返します。</span><span class="sxs-lookup"><span data-stu-id="bad51-155">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="bad51-156">IIS と ASP.NET Core モジュールの構成のガイダンスについては、次のトピックをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="bad51-156">For IIS and ASP.NET Core Module configuration guidance, see the following topics:</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>

# <a name="macostabmacos"></a>[<span data-ttu-id="bad51-157">macOS</span><span class="sxs-lookup"><span data-stu-id="bad51-157">macOS</span></span>](#tab/macos)

<span data-ttu-id="bad51-158">ASP.NET Core には、既定のクロスプラットフォーム HTTP サーバーである [Kestrel サーバー](xref:fundamentals/servers/kestrel)が付属しています。</span><span class="sxs-lookup"><span data-stu-id="bad51-158">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="bad51-159">Linux</span><span class="sxs-lookup"><span data-stu-id="bad51-159">Linux</span></span>](#tab/linux)

<span data-ttu-id="bad51-160">ASP.NET Core には、既定のクロスプラットフォーム HTTP サーバーである [Kestrel サーバー](xref:fundamentals/servers/kestrel)が付属しています。</span><span class="sxs-lookup"><span data-stu-id="bad51-160">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="bad51-161">Windows</span><span class="sxs-lookup"><span data-stu-id="bad51-161">Windows</span></span>](#tab/windows)

<span data-ttu-id="bad51-162">ASP.NET Core には次のものが付属しています。</span><span class="sxs-lookup"><span data-stu-id="bad51-162">ASP.NET Core ships with the following:</span></span>

* <span data-ttu-id="bad51-163">[Kestrel サーバー](xref:fundamentals/servers/kestrel)は、既定のクロスプラットフォーム HTTP サーバーです。</span><span class="sxs-lookup"><span data-stu-id="bad51-163">[Kestrel server](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server.</span></span>
* <span data-ttu-id="bad51-164">[HTTP.sys サーバー](xref:fundamentals/servers/httpsys)は、[HTTP.sys カーネル ドライバーおよび HTTP サーバー API](/windows/desktop/Http/http-api-start-page) に基づく Windows 専用の HTTP サーバーです。</span><span class="sxs-lookup"><span data-stu-id="bad51-164">[HTTP.sys server](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](/windows/desktop/Http/http-api-start-page).</span></span>

<span data-ttu-id="bad51-165">[IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) または [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) を使用すると、アプリは [Kestrel サーバー](#kestrel)を使用して IIS ワーカー プロセスとは異なるプロセス内で実行されます ("*プロセス外*")。</span><span class="sxs-lookup"><span data-stu-id="bad51-165">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), the app runs in a process separate from the IIS worker process (*out-of-process*) with the [Kestrel server](#kestrel).</span></span>

<span data-ttu-id="bad51-166">ASP.NET Core アプリは IIS ワーカー プロセスとは独立したプロセスで実行されるため、プロセス管理はモジュールによって処理されます。</span><span class="sxs-lookup"><span data-stu-id="bad51-166">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module handles process management.</span></span> <span data-ttu-id="bad51-167">モジュールでは、最初の要求が届いたときに ASP.NET Core アプリのプロセスが開始され、プロセスがシャットダウンまたはクラッシュした場合はアプリが再起動されます。</span><span class="sxs-lookup"><span data-stu-id="bad51-167">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it shuts down or crashes.</span></span> <span data-ttu-id="bad51-168">この動作は、インプロセスで実行されるアプリであり、[WAS (Windows プロセス アクティブ化サービス)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) によって管理されるアプリと基本的に同じです。</span><span class="sxs-lookup"><span data-stu-id="bad51-168">This is essentially the same behavior as seen with apps that run in-process that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="bad51-169">次の図は、IIS (ASP.NET Core モジュール) とアウトプロセスでホストされるアプリとの間のリレーションシップを示しています。</span><span class="sxs-lookup"><span data-stu-id="bad51-169">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted out-of-process:</span></span>

![ASP.NET Core モジュール](_static/ancm-outofprocess.png)

<span data-ttu-id="bad51-171">要求は、Web からカーネル モードの HTTP.sys ドライバーに到着します。</span><span class="sxs-lookup"><span data-stu-id="bad51-171">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="bad51-172">ドライバーは、Web サイトの構成ポート (通常は 80 (HTTP) または 443 (HTTPS)) で IIS への要求をルーティングします。</span><span class="sxs-lookup"><span data-stu-id="bad51-172">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="bad51-173">モジュールでは、アプリのランダムなポート (ポート 80 または 443 ではありません) で Kestrel に要求が転送されます。</span><span class="sxs-lookup"><span data-stu-id="bad51-173">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="bad51-174">モジュールが起動時に環境変数を介してポートを指定すると、サーバーは `http://localhost:{port}` をリッスンするように、IIS 統合ミドルウェアによって構成されます。</span><span class="sxs-lookup"><span data-stu-id="bad51-174">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="bad51-175">追加のチェックが実行され、モジュールから発生していない要求は拒否されます。</span><span class="sxs-lookup"><span data-stu-id="bad51-175">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="bad51-176">モジュールでは HTTPS 転送がサポートされていないため、要求は HTTPS を介して IIS によって受信された場合でも、HTTP を介して転送されます。</span><span class="sxs-lookup"><span data-stu-id="bad51-176">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="bad51-177">Kestrel によってモジュールから要求が取り込まれた後、その要求は ASP.NET Core ミドルウェア パイプラインにプッシュされます。</span><span class="sxs-lookup"><span data-stu-id="bad51-177">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="bad51-178">ミドルウェア パイプラインは要求を処理し、`HttpContext` インスタンスとしてアプリのロジックに渡します。</span><span class="sxs-lookup"><span data-stu-id="bad51-178">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="bad51-179">IIS 統合によって追加されたミドルウェアでは、カーネルへの要求の転送を考慮して、スキーム、リモート IP、およびパスベースが更新されます。</span><span class="sxs-lookup"><span data-stu-id="bad51-179">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="bad51-180">アプリの応答が IIS に戻され、IIS はその応答を、要求を開始した HTTP クライアントに返します。</span><span class="sxs-lookup"><span data-stu-id="bad51-180">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="bad51-181">IIS と ASP.NET Core モジュールの構成のガイダンスについては、次のトピックをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="bad51-181">For IIS and ASP.NET Core Module configuration guidance, see the following topics:</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>

# <a name="macostabmacos"></a>[<span data-ttu-id="bad51-182">macOS</span><span class="sxs-lookup"><span data-stu-id="bad51-182">macOS</span></span>](#tab/macos)

<span data-ttu-id="bad51-183">ASP.NET Core には、既定のクロスプラットフォーム HTTP サーバーである [Kestrel サーバー](xref:fundamentals/servers/kestrel)が付属しています。</span><span class="sxs-lookup"><span data-stu-id="bad51-183">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="bad51-184">Linux</span><span class="sxs-lookup"><span data-stu-id="bad51-184">Linux</span></span>](#tab/linux)

<span data-ttu-id="bad51-185">ASP.NET Core には、既定のクロスプラットフォーム HTTP サーバーである [Kestrel サーバー](xref:fundamentals/servers/kestrel)が付属しています。</span><span class="sxs-lookup"><span data-stu-id="bad51-185">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

---

::: moniker-end

## <a name="kestrel"></a><span data-ttu-id="bad51-186">Kestrel</span><span class="sxs-lookup"><span data-stu-id="bad51-186">Kestrel</span></span>

<span data-ttu-id="bad51-187">Kestrel は、ASP.NET Core のプロジェクト テンプレートに含まれる既定の Web サーバーです。</span><span class="sxs-lookup"><span data-stu-id="bad51-187">Kestrel is the default web server included in ASP.NET Core project templates.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="bad51-188">Kestrel は次のように使用できます。</span><span class="sxs-lookup"><span data-stu-id="bad51-188">Kestrel can be used:</span></span>

* <span data-ttu-id="bad51-189">これ自体で、インターネットを含むネットワークから直接要求を処理するエッジ サーバーとして。</span><span class="sxs-lookup"><span data-stu-id="bad51-189">By itself as an edge server processing requests directly from a network, including the Internet.</span></span>

  ![リバース プロキシ サーバーなしでインターネットと直接通信する Kestrel](kestrel/_static/kestrel-to-internet2.png)

* <span data-ttu-id="bad51-191">[インターネット インフォメーション サービス (IIS)](https://www.iis.net/)、[Nginx](http://nginx.org)、[Apache](https://httpd.apache.org/) などの*リバース プロキシ サーバー*と共に。</span><span class="sxs-lookup"><span data-stu-id="bad51-191">With a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](http://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="bad51-192">リバース プロキシ サーバーはインターネットから HTTP 要求を受け取り、これを Kestrel に転送します。</span><span class="sxs-lookup"><span data-stu-id="bad51-192">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel.</span></span>

  ![IIS、Nginx、または Apache などのリバース プロキシ サーバーを介してインターネットと間接的に通信する Kestrel](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="bad51-194">いずれのホスティング構成も、リバース プロキシ サーバーの有無に関わらず、ASP.NET Core 2.1 以降のアプリに対してサポートされます。</span><span class="sxs-lookup"><span data-stu-id="bad51-194">Either hosting configuration&mdash;with or without a reverse proxy server&mdash;is supported for ASP.NET Core 2.1 or later apps.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="bad51-195">アプリが内部ネットワークからの要求のみを受け入れる場合は、Kestrel を単独で使用することができます。</span><span class="sxs-lookup"><span data-stu-id="bad51-195">If the app only accepts requests from an internal network, Kestrel can be used by itself.</span></span>

![内部ネットワークと直接通信する Kestrel](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="bad51-197">アプリをインターネットに公開する場合は、Kestrel が[インターネット インフォメーション サービス (IIS)](https://www.iis.net/)、[Nginx](http://nginx.org)、[Apache](https://httpd.apache.org/) などの*リバース プロキシ サーバー*を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="bad51-197">If the app is exposed to the Internet, Kestrel must use a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](http://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="bad51-198">リバース プロキシ サーバーはインターネットから HTTP 要求を受け取り、これを Kestrel に転送します。</span><span class="sxs-lookup"><span data-stu-id="bad51-198">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel.</span></span>

![IIS、Nginx、または Apache などのリバース プロキシ サーバーを介してインターネットと間接的に通信する Kestrel](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="bad51-200">インターネットに直接公開される一般向けのエッジ サーバー展開でリバース プロキシを使用する最も重要な理由は、セキュリティです。</span><span class="sxs-lookup"><span data-stu-id="bad51-200">The most important reason for using a reverse proxy for public-facing edge server deployments that are exposed directly the Internet is security.</span></span> <span data-ttu-id="bad51-201">1.x バージョンの Kestrel はインターネットからの攻撃を防御する重要なセキュリティ機能を備えていません。</span><span class="sxs-lookup"><span data-stu-id="bad51-201">The 1.x versions of Kestrel don't include important security features to defend against attacks from the Internet.</span></span> <span data-ttu-id="bad51-202">これには、適切なタイムアウト、要求サイズの制限、およびコンカレント接続の制限などが含まれます (ただし、これらに限定されない)。</span><span class="sxs-lookup"><span data-stu-id="bad51-202">This includes, but isn't limited to, appropriate timeouts, request size limits, and concurrent connection limits.</span></span>

::: moniker-end

<span data-ttu-id="bad51-203">Kestrel の構成ガイダンスおよびリバース プロキシ構成で Kestrel を使用するときの情報については、「<xref:fundamentals/servers/kestrel>」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="bad51-203">For Kestrel configuration guidance and information on when to use Kestrel in a reverse proxy configuration, see <xref:fundamentals/servers/kestrel>.</span></span>

### <a name="nginx-with-kestrel"></a><span data-ttu-id="bad51-204">Nginx と Kestrel</span><span class="sxs-lookup"><span data-stu-id="bad51-204">Nginx with Kestrel</span></span>

<span data-ttu-id="bad51-205">Kestrel のリバース プロキシ サーバーとして Linux で Nginx を使用する方法については、<xref:host-and-deploy/linux-nginx> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="bad51-205">For information on how to use Nginx on Linux as a reverse proxy server for Kestrel, see <xref:host-and-deploy/linux-nginx>.</span></span>

### <a name="apache-with-kestrel"></a><span data-ttu-id="bad51-206">Apache と Kestrel</span><span class="sxs-lookup"><span data-stu-id="bad51-206">Apache with Kestrel</span></span>

<span data-ttu-id="bad51-207">Kestrel のリバース プロキシ サーバーとして Linux で Apache を使用する方法については、<xref:host-and-deploy/linux-apache> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="bad51-207">For information on how to use Apache on Linux as a reverse proxy server for Kestrel, see <xref:host-and-deploy/linux-apache>.</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="iis-http-server"></a><span data-ttu-id="bad51-208">IIS HTTP サーバー</span><span class="sxs-lookup"><span data-stu-id="bad51-208">IIS HTTP Server</span></span>

<span data-ttu-id="bad51-209">IIS HTTP サーバーは、IIS 用の[インプロセス サーバー](#in-process-hosting-model)であり、インプロセスの展開に必要です。</span><span class="sxs-lookup"><span data-stu-id="bad51-209">IIS HTTP Server is an [in-process server](#in-process-hosting-model) for IIS and required for in-process deployments.</span></span> <span data-ttu-id="bad51-210">[ASP.NET Core モジュール](xref:host-and-deploy/aspnet-core-module)では、IIS と IIS HTTP サーバーの間のネイティブ IIS 要求が処理されます。</span><span class="sxs-lookup"><span data-stu-id="bad51-210">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) handles native IIS requests between IIS and IIS HTTP Server.</span></span> <span data-ttu-id="bad51-211">詳細については、「<xref:host-and-deploy/aspnet-core-module>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="bad51-211">For more information, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

::: moniker-end

## <a name="httpsys"></a><span data-ttu-id="bad51-212">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="bad51-212">HTTP.sys</span></span>

<span data-ttu-id="bad51-213">Windows で ASP.NET Core アプリを実行する場合は、HTTP.sys を Kestrel の代わりに使用できます。</span><span class="sxs-lookup"><span data-stu-id="bad51-213">If ASP.NET Core apps are run on Windows, HTTP.sys is an alternative to Kestrel.</span></span> <span data-ttu-id="bad51-214">最適なパフォーマンスを得るには、通常は Kestrel をお勧めします。</span><span class="sxs-lookup"><span data-stu-id="bad51-214">Kestrel is generally recommended for best performance.</span></span> <span data-ttu-id="bad51-215">HTTP.sys は、アプリがインターネットに公開されていて、必要な機能が HTTP.sys でサポートされているものの、Kestrel ではサポートされていないシナリオで使用できます。</span><span class="sxs-lookup"><span data-stu-id="bad51-215">HTTP.sys can be used in scenarios where the app is exposed to the Internet and required capabilities are supported by HTTP.sys but not Kestrel.</span></span> <span data-ttu-id="bad51-216">詳細については、「<xref:fundamentals/servers/httpsys>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="bad51-216">For more information, see <xref:fundamentals/servers/httpsys>.</span></span>

![インターネットと直接通信する HTTP.sys](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="bad51-218">HTTP.sys は、内部ネットワークにのみ公開されるアプリにも使用できます。</span><span class="sxs-lookup"><span data-stu-id="bad51-218">HTTP.sys can also be used for apps that are only exposed to an internal network.</span></span>

![内部ネットワークと直接通信する HTTP.sys](httpsys/_static/httpsys-to-internal.png)

<span data-ttu-id="bad51-220">HTTP.sys の構成のガイダンスについては、「<xref:fundamentals/servers/httpsys>」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="bad51-220">For HTTP.sys configuration guidance, see <xref:fundamentals/servers/httpsys>.</span></span>

## <a name="aspnet-core-server-infrastructure"></a><span data-ttu-id="bad51-221">ASP.NET Core サーバー インフラストラクチャ</span><span class="sxs-lookup"><span data-stu-id="bad51-221">ASP.NET Core server infrastructure</span></span>

<span data-ttu-id="bad51-222">`Startup.Configure` メソッドで使用可能な <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> は、<xref:Microsoft.AspNetCore.Http.Features.IFeatureCollection> 型の <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ServerFeatures> プロパティを公開します。</span><span class="sxs-lookup"><span data-stu-id="bad51-222">The <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> available in the `Startup.Configure` method exposes the <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ServerFeatures> property of type <xref:Microsoft.AspNetCore.Http.Features.IFeatureCollection>.</span></span> <span data-ttu-id="bad51-223">Kestrel および HTTP.sys は、それぞれ単独の機能 <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature> のみを公開しますが、サーバーの実装が異なると追加機能が公開される場合があります。</span><span class="sxs-lookup"><span data-stu-id="bad51-223">Kestrel and HTTP.sys only expose a single feature each, <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>, but different server implementations may expose additional functionality.</span></span>

<span data-ttu-id="bad51-224">`IServerAddressesFeature` を使用すれば、実行時にサーバー実装がバインドされたポートを見つけることができます。</span><span class="sxs-lookup"><span data-stu-id="bad51-224">`IServerAddressesFeature` can be used to find out which port the server implementation has bound at runtime.</span></span>

## <a name="custom-servers"></a><span data-ttu-id="bad51-225">カスタム サーバー</span><span class="sxs-lookup"><span data-stu-id="bad51-225">Custom servers</span></span>

<span data-ttu-id="bad51-226">組み込みサーバーがアプリの要件に合わない場合は、カスタム サーバー実装を作成できます。</span><span class="sxs-lookup"><span data-stu-id="bad51-226">If the built-in servers don't meet the app's requirements, a custom server implementation can be created.</span></span> <span data-ttu-id="bad51-227">[Nowin](https://github.com/Bobris/Nowin) ベースの <xref:Microsoft.AspNetCore.Hosting.Server.IServer> 実装の作成方法については、[Open Web Interface for .NET (OWIN) のガイド](xref:fundamentals/owin)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="bad51-227">The [Open Web Interface for .NET (OWIN) guide](xref:fundamentals/owin) demonstrates how to write a [Nowin](https://github.com/Bobris/Nowin)-based <xref:Microsoft.AspNetCore.Hosting.Server.IServer> implementation.</span></span> <span data-ttu-id="bad51-228">実装を必要とするのは、アプリで使用される機能のインターフェイスのみです。ただし、少なくとも <xref:Microsoft.AspNetCore.Http.Features.IHttpRequestFeature> と <xref:Microsoft.AspNetCore.Http.Features.IHttpResponseFeature> はサポートされている必要があります。</span><span class="sxs-lookup"><span data-stu-id="bad51-228">Only the feature interfaces that the app uses require implementation, though at a minimum <xref:Microsoft.AspNetCore.Http.Features.IHttpRequestFeature> and <xref:Microsoft.AspNetCore.Http.Features.IHttpResponseFeature> must be supported.</span></span>

## <a name="server-startup"></a><span data-ttu-id="bad51-229">サーバーの起動</span><span class="sxs-lookup"><span data-stu-id="bad51-229">Server startup</span></span>

<span data-ttu-id="bad51-230">統合開発環境 (IDE) またはエディターでアプリが開始されると、サーバーが起動されます。</span><span class="sxs-lookup"><span data-stu-id="bad51-230">The server is launched when the Integrated Development Environment (IDE) or editor starts the app:</span></span>

* <span data-ttu-id="bad51-231">[Visual Studio](https://www.visualstudio.com/vs/) &ndash; 起動プロファイルを使用して、[IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[ASP.NET Core モジュール](xref:host-and-deploy/aspnet-core-module)またはコンソールで、アプリとサーバーを開始できます。</span><span class="sxs-lookup"><span data-stu-id="bad51-231">[Visual Studio](https://www.visualstudio.com/vs/) &ndash; Launch profiles can be used to start the app and server with either [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) or the console.</span></span>
* <span data-ttu-id="bad51-232">[Visual Studio Code](https://code.visualstudio.com/) &ndash; CoreCLR デバッガーをアクティブ化する [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode) によって、アプリとサーバーが開始されます。</span><span class="sxs-lookup"><span data-stu-id="bad51-232">[Visual Studio Code](https://code.visualstudio.com/) &ndash; The app and server are started by [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), which activates the CoreCLR debugger.</span></span>
* <span data-ttu-id="bad51-233">[Visual Studio for Mac](https://www.visualstudio.com/vs/mac/) &ndash; アプリとサーバーは、[Mono Soft-Mode Debugger](https://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/) によって開始されます。</span><span class="sxs-lookup"><span data-stu-id="bad51-233">[Visual Studio for Mac](https://www.visualstudio.com/vs/mac/) &ndash; The app and server are started by the [Mono Soft-Mode Debugger](https://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).</span></span>

<span data-ttu-id="bad51-234">コマンド プロンプトからプロジェクトのフォルダーでアプリを起動すると、[dotnet run](/dotnet/core/tools/dotnet-run) によってアプリとサーバーが起動されます (Kestrel および HTTP.sys のみ)。</span><span class="sxs-lookup"><span data-stu-id="bad51-234">When launching the app from a command prompt in the project's folder, [dotnet run](/dotnet/core/tools/dotnet-run) launches the app and server (Kestrel and HTTP.sys only).</span></span> <span data-ttu-id="bad51-235">この構成は、`Debug` (既定) または `Release` のどちらかに設定された `-c|--configuration` オプションによって指定されます。</span><span class="sxs-lookup"><span data-stu-id="bad51-235">The configuration is specified by the `-c|--configuration` option, which is set to either `Debug` (default) or `Release`.</span></span> <span data-ttu-id="bad51-236">起動プロファイルが *launchSettings.json* ファイルに存在する場合は、`--launch-profile <NAME>` オプションを使用して起動プロファイルを設定します (`Development`、`Production` など)。</span><span class="sxs-lookup"><span data-stu-id="bad51-236">If launch profiles are present in a *launchSettings.json* file, use the `--launch-profile <NAME>` option to set the launch profile (for example, `Development` or `Production`).</span></span> <span data-ttu-id="bad51-237">詳しくは、「[dotnet run](/dotnet/core/tools/dotnet-run)」および「[.NET Core の配布パッケージ](/dotnet/core/build/distribution-packaging)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="bad51-237">For more information, see [dotnet run](/dotnet/core/tools/dotnet-run) and [.NET Core distribution packaging](/dotnet/core/build/distribution-packaging).</span></span>

## <a name="http2-support"></a><span data-ttu-id="bad51-238">HTTP/2 のサポート</span><span class="sxs-lookup"><span data-stu-id="bad51-238">HTTP/2 support</span></span>

<span data-ttu-id="bad51-239">[HTTP/2](https://httpwg.org/specs/rfc7540.html) は、次の展開シナリオでの ASP.NET Core でサポートされます。</span><span class="sxs-lookup"><span data-stu-id="bad51-239">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported with ASP.NET Core in the following deployment scenarios:</span></span>

::: moniker range=">= aspnetcore-2.2"

* [<span data-ttu-id="bad51-240">Kestrel</span><span class="sxs-lookup"><span data-stu-id="bad51-240">Kestrel</span></span>](xref:fundamentals/servers/kestrel#http2-support)
  * <span data-ttu-id="bad51-241">オペレーティング システム</span><span class="sxs-lookup"><span data-stu-id="bad51-241">Operating system</span></span>
    * <span data-ttu-id="bad51-242">Windows Server 2016/Windows 10 以降&dagger;</span><span class="sxs-lookup"><span data-stu-id="bad51-242">Windows Server 2016/Windows 10 or later&dagger;</span></span>
    * <span data-ttu-id="bad51-243">OpenSSL 1.0.2 以降を使用した Linux (Ubuntu 16.04 以降など)</span><span class="sxs-lookup"><span data-stu-id="bad51-243">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
    * <span data-ttu-id="bad51-244">将来のリリースでは HTTP/2 が macOS 上でサポートされるようになります。</span><span class="sxs-lookup"><span data-stu-id="bad51-244">HTTP/2 will be supported on macOS in a future release.</span></span>
  * <span data-ttu-id="bad51-245">ターゲット フレームワーク: .NET Core 2.2 以降</span><span class="sxs-lookup"><span data-stu-id="bad51-245">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="bad51-246">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="bad51-246">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="bad51-247">Windows Server 2016/Windows 10 以降</span><span class="sxs-lookup"><span data-stu-id="bad51-247">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="bad51-248">対象のフレームワーク:HTTP.sys の展開には適用できません。</span><span class="sxs-lookup"><span data-stu-id="bad51-248">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="bad51-249">IIS (インプロセス)</span><span class="sxs-lookup"><span data-stu-id="bad51-249">IIS (in-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="bad51-250">Windows Server 2016/Windows 10 以降、IIS 10 以降</span><span class="sxs-lookup"><span data-stu-id="bad51-250">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="bad51-251">ターゲット フレームワーク: .NET Core 2.2 以降</span><span class="sxs-lookup"><span data-stu-id="bad51-251">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="bad51-252">IIS (アウトプロセス)</span><span class="sxs-lookup"><span data-stu-id="bad51-252">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="bad51-253">Windows Server 2016/Windows 10 以降、IIS 10 以降</span><span class="sxs-lookup"><span data-stu-id="bad51-253">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="bad51-254">一般向けのエッジ サーバーでは HTTP/2 を使用しますが、Kestrel へのリバース プロキシ接続では HTTP/1.1 を使用します。</span><span class="sxs-lookup"><span data-stu-id="bad51-254">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="bad51-255">対象のフレームワーク:IIS アウトプロセスの展開には適用できません。</span><span class="sxs-lookup"><span data-stu-id="bad51-255">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

<span data-ttu-id="bad51-256">&dagger;Kestrel では、Windows Server 2012 R2 および Windows 8.1 上での HTTP/2 のサポートは制限されています。</span><span class="sxs-lookup"><span data-stu-id="bad51-256">&dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="bad51-257">サポートが制限されている理由は、これらのオペレーティング システムで使用できる TLS 暗号のスイートのリストが制限されているためです。</span><span class="sxs-lookup"><span data-stu-id="bad51-257">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="bad51-258">TLS 接続をセキュリティで保護するためには、楕円曲線デジタル署名アルゴリズム (ECDSA) を使用して生成した証明書が必要になる場合があります。</span><span class="sxs-lookup"><span data-stu-id="bad51-258">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [<span data-ttu-id="bad51-259">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="bad51-259">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="bad51-260">Windows Server 2016/Windows 10 以降</span><span class="sxs-lookup"><span data-stu-id="bad51-260">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="bad51-261">対象のフレームワーク:HTTP.sys の展開には適用できません。</span><span class="sxs-lookup"><span data-stu-id="bad51-261">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="bad51-262">IIS (アウトプロセス)</span><span class="sxs-lookup"><span data-stu-id="bad51-262">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="bad51-263">Windows Server 2016/Windows 10 以降、IIS 10 以降</span><span class="sxs-lookup"><span data-stu-id="bad51-263">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="bad51-264">一般向けのエッジ サーバーでは HTTP/2 を使用しますが、Kestrel へのリバース プロキシ接続では HTTP/1.1 を使用します。</span><span class="sxs-lookup"><span data-stu-id="bad51-264">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="bad51-265">対象のフレームワーク:IIS アウトプロセスの展開には適用できません。</span><span class="sxs-lookup"><span data-stu-id="bad51-265">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

::: moniker-end

<span data-ttu-id="bad51-266">HTTP/2 接続では、[アプリケーション レイヤー プロトコルのネゴシエーション (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) および TLS 1.2 以降を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="bad51-266">An HTTP/2 connection must use [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) and TLS 1.2 or later.</span></span> <span data-ttu-id="bad51-267">詳細については、ご利用のサーバーの展開シナリオに関連するトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="bad51-267">For more information, see the topics that pertain to your server deployment scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bad51-268">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="bad51-268">Additional resources</span></span>

* <xref:fundamentals/servers/kestrel>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:fundamentals/servers/httpsys>
