---
title: ASP.NET Core モジュール
author: guardrex
description: Kestrel Web サーバーが IIS または IIS Express を使用できるようにするための ASP.NET Core モジュールについて説明します。
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/30/2018
uid: fundamentals/servers/aspnet-core-module
ms.openlocfilehash: d3f3a42dd7aebc425905b865376a584bcf0e5153
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861460"
---
# <a name="aspnet-core-module"></a><span data-ttu-id="8aa6d-103">ASP.NET Core モジュール</span><span class="sxs-lookup"><span data-stu-id="8aa6d-103">ASP.NET Core Module</span></span>

<span data-ttu-id="8aa6d-104">作成者: [Tom Dykstra](https://github.com/tdykstra)、[Rick Strahl](https://github.com/RickStrahl)、[Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="8aa6d-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), and [Chris Ross](https://github.com/Tratcher)</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="8aa6d-105">ASP.NET Core モジュールでは、ASP.NET Core アプリを IIS ワーカー プロセス (*インプロセス*) で実行することも、ASP.NET Core アプリを IIS の背後でリバース プロキシの構成 (*アウトプロセス*) で実行することもできます。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-105">The ASP.NET Core Module allows ASP.NET Core apps to run in an IIS worker process (*in-process*) or behind IIS in a reverse proxy configuration (*out-of-process*).</span></span> <span data-ttu-id="8aa6d-106">IIS では、高度な Web アプリ セキュリティ機能と管理機能が提供されます。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-106">IIS provides advanced web app security and manageability features.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="8aa6d-107">ASP.NET Core モジュールでは、リバース プロキシの構成で ASP.NET Core アプリを IIS の背後で実行することを許可します。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-107">The ASP.NET Core Module allows ASP.NET Core apps to run behind IIS in a reverse proxy configuration.</span></span> <span data-ttu-id="8aa6d-108">IIS では、高度な Web アプリ セキュリティ機能と管理機能が提供されます。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-108">IIS provides advanced web app security and manageability features.</span></span>

::: moniker-end

<span data-ttu-id="8aa6d-109">サポートされている Windows バージョン:</span><span class="sxs-lookup"><span data-stu-id="8aa6d-109">Supported Windows versions:</span></span>

* <span data-ttu-id="8aa6d-110">Windows 7 以降</span><span class="sxs-lookup"><span data-stu-id="8aa6d-110">Windows 7 or later</span></span>
* <span data-ttu-id="8aa6d-111">Windows Server 2008 R2 以降</span><span class="sxs-lookup"><span data-stu-id="8aa6d-111">Windows Server 2008 R2 or later</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="8aa6d-112">インプロセス ホスティングの場合、モジュールでは IIS インプロセス サーバー実装である IIS HTTP サーバー (`IISHttpServer`) が使用されます。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-112">When hosting in-process, the module uses an IIS in-process server implementation, IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="8aa6d-113">アウト プロセスでホストする場合、モジュールは Kestrel に対してのみ機能します。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-113">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="8aa6d-114">このモジュールは、[HTTP.sys](xref:fundamentals/servers/httpsys) (旧称 [WebListener](xref:fundamentals/servers/weblistener)) と互換性がありません。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-114">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="8aa6d-115">モジュールは、Kestrel に対してのみ機能します。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-115">The module only works with Kestrel.</span></span> <span data-ttu-id="8aa6d-116">このモジュールは、[HTTP.sys](xref:fundamentals/servers/httpsys) (旧称 [WebListener](xref:fundamentals/servers/weblistener)) と互換性がありません。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-116">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span>

::: moniker-end

## <a name="aspnet-core-module-description"></a><span data-ttu-id="8aa6d-117">ASP.NET Core モジュールの説明</span><span class="sxs-lookup"><span data-stu-id="8aa6d-117">ASP.NET Core Module description</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="8aa6d-118">ASP.NET Core モジュールはネイティブな IIS モジュールであり、次のいずれかを目的として、IIS パイプラインにプラグインされます。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-118">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="8aa6d-119">IIS ワーカー プロセス (`w3wp.exe`) の内部で ASP.NET Core アプリをホストします。これは、[インプロセス ホスティング モデル](#in-process-hosting-model)と呼ばれています。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-119">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>

* <span data-ttu-id="8aa6d-120">[Kestrel サーバー](xref:fundamentals/servers/kestrel)を実行しているバックエンドの ASP.NET Core アプリに Web 要求を転送します。これは、[アウト プロセス ホスティング モデル](#out-of-process-hosting-model)と呼ばれています。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-120">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="8aa6d-121">インプロセス ホスティング モデル</span><span class="sxs-lookup"><span data-stu-id="8aa6d-121">In-process hosting model</span></span>

<span data-ttu-id="8aa6d-122">インプロセス ホスティング モデルを使用する場合、ASP.NET Core アプリはその IIS ワーカー プロセスと同じプロセスで実行されます。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-122">Using in-process hosting, an ASP.NET Core app runs in the same process as its IIS worker process.</span></span> <span data-ttu-id="8aa6d-123">これにより、アウト プロセス ホスティング モデルを使用する場合に、ループバック アダプター経由で要求をプロキシすることによるパフォーマンスの低下が解消されます。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-123">This removes the performance penalty of proxying requests over the loopback adapter when using the out-of-process hosting model.</span></span> <span data-ttu-id="8aa6d-124">IIS では [Windows プロセス アクティブ化サービス (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) を使用してプロセス管理が処理されます。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-124">IIS handles process management with the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="8aa6d-125">ASP.NET Core モジュール:</span><span class="sxs-lookup"><span data-stu-id="8aa6d-125">The ASP.NET Core Module:</span></span>

* <span data-ttu-id="8aa6d-126">アプリの初期化を実行します。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-126">Performs app initialization.</span></span>
  * <span data-ttu-id="8aa6d-127">[CoreCLR](/dotnet/standard/glossary#coreclr) を読み込みます。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-127">Loads the [CoreCLR](/dotnet/standard/glossary#coreclr).</span></span>
  * <span data-ttu-id="8aa6d-128">`Program.Main`.</span><span class="sxs-lookup"><span data-stu-id="8aa6d-128">Calls `Program.Main`.</span></span>
* <span data-ttu-id="8aa6d-129">IIS ネイティブ要求の有効期間を処理します。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-129">Handles the lifetime of the IIS native request.</span></span>

<span data-ttu-id="8aa6d-130">次の図は、IIS (ASP.NET Core モジュール) とインプロセスでホストされるアプリとの間のリレーションシップを示しています。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-130">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted in-process:</span></span>

![ASP.NET Core モジュール](aspnet-core-module/_static/ancm-inprocess.png)

<span data-ttu-id="8aa6d-132">Web からカーネル モードの HTTP.sys ドライバーに要求が届きます。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-132">A request arrives from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="8aa6d-133">このドライバーによって、Web サイトの構成ポート (通常は 80 (HTTP) または 443 (HTTPS)) 上で IIS へのネイティブ要求がルーティングされます。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-133">The driver routes the native request to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="8aa6d-134">モーダルは、ネイティブ要求を受け取り、それを IIS HTTP サーバー (`IISHttpServer`) に渡します。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-134">The module receives the native request and passes it to IIS HTTP Server (`IISHttpServer`).</span></span> <span data-ttu-id="8aa6d-135">IIS HTTP サーバーは、要求をネイティブからマネージドに変換する IIS インプロセス サーバー実装です。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-135">IIS HTTP Server is an IIS in-process server implementation that converts the request from native to managed.</span></span>

<span data-ttu-id="8aa6d-136">IIS HTTP サーバーによって要求が処理された後、その要求は ASP.NET Core ミドルウェア パイプラインにプッシュされます。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-136">After the IIS HTTP Server processes the request, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="8aa6d-137">ミドルウェア パイプラインは要求を処理し、`HttpContext` インスタンスとしてアプリのロジックに渡します。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-137">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="8aa6d-138">アプリの応答が IIS に渡され、その応答は IIS から要求を開始したクライアントにプッシュで戻されます。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-138">The app's response is passed back to IIS, which pushes it back out to the client that initiated the request.</span></span>

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="8aa6d-139">アウト プロセス ホスティング モデル</span><span class="sxs-lookup"><span data-stu-id="8aa6d-139">Out-of-process hosting model</span></span>

<span data-ttu-id="8aa6d-140">ASP.NET Core アプリは IIS ワーカー プロセスとは独立したプロセスで実行されるため、プロセス管理はモジュールによって処理されます。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-140">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module handles process management.</span></span> <span data-ttu-id="8aa6d-141">モジュールでは、最初の要求が届いたときに ASP.NET Core アプリのプロセスが開始され、プロセスがシャットダウンまたはクラッシュした場合はアプリが再起動されます。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-141">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it shuts down or crashes.</span></span> <span data-ttu-id="8aa6d-142">この動作は、インプロセスで実行されるアプリであり、[WAS (Windows プロセス アクティブ化サービス)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) によって管理されるアプリと基本的に同じです。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-142">This is essentially the same behavior as seen with apps that run in-process that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="8aa6d-143">次の図は、IIS (ASP.NET Core モジュール) とアウトプロセスでホストされるアプリとの間のリレーションシップを示しています。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-143">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted out-of-process:</span></span>

![ASP.NET Core モジュール](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="8aa6d-145">要求は、Web からカーネル モードの HTTP.sys ドライバーに到着します。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-145">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="8aa6d-146">ドライバーは、Web サイトの構成ポート (通常は 80 (HTTP) または 443 (HTTPS)) で IIS への要求をルーティングします。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-146">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="8aa6d-147">モジュールでは、アプリのランダムなポートで Kestrel に要求を転送します。このポートは 80/443 ではありません。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-147">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80/443.</span></span>

<span data-ttu-id="8aa6d-148">モジュールが起動時に環境変数を介してポートを指定すると、サーバーは `http://localhost:{port}` をリッスンするように、IIS 統合ミドルウェアによって構成されます。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-148">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="8aa6d-149">追加のチェックが実行され、モジュールから発生していない要求は拒否されます。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-149">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="8aa6d-150">モジュールでは HTTPS 転送がサポートされていないため、要求は HTTPS を介して IIS によって受信された場合でも、HTTP を介して転送されます。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-150">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="8aa6d-151">Kestrel によってモジュールから要求が取り込まれた後、その要求は ASP.NET Core ミドルウェア パイプラインにプッシュされます。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-151">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="8aa6d-152">ミドルウェア パイプラインは要求を処理し、`HttpContext` インスタンスとしてアプリのロジックに渡します。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-152">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="8aa6d-153">IIS 統合によって追加されたミドルウェアでは、カーネルへの要求の転送を考慮して、スキーム、リモート IP、およびパスベースが更新されます。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-153">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="8aa6d-154">アプリの応答が IIS に戻され、IIS はその応答を、要求を開始した HTTP クライアントに返します。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-154">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="8aa6d-155">ASP.NET Core モジュールは、ネイティブな IIS モジュールであり、バックエンドの ASP.NET Core アプリに Web 要求を転送するために IIS パイプラインにプラグインされます。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-155">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to forward web requests to backend ASP.NET Core apps.</span></span>

<span data-ttu-id="8aa6d-156">ASP.NET Core アプリは IIS ワーカー プロセスとは独立したプロセスで実行されるため、モジュールもプロセス管理を処理します。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-156">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="8aa6d-157">モジュールは、最初の要求を受信したときに ASP.NET Core アプリのプロセスを開始し、プロセスがクラッシュした場合はアプリを再起動します。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-157">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="8aa6d-158">この動作は、IIS のインプロセスで実行され、[WAS (Windows プロセス アクティブ化サービス)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) によって管理される ASP.NET 4.x アプリと基本的に同じです。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-158">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="8aa6d-159">次の図は、IIS (ASP.NET Core モジュール) とアプリの間のリレーションシップを示しています。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-159">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app:</span></span>

![ASP.NET Core モジュール](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="8aa6d-161">要求は、Web からカーネル モードの HTTP.sys ドライバーに到着します。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-161">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="8aa6d-162">ドライバーは、Web サイトの構成ポート (通常は 80 (HTTP) または 443 (HTTPS)) で IIS への要求をルーティングします。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-162">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="8aa6d-163">モジュールでは、アプリのランダムなポートで Kestrel に要求を転送します。このポートは 80/443 ではありません。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-163">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80/443.</span></span>

<span data-ttu-id="8aa6d-164">モジュールが起動時に環境変数を介してポートを指定すると、サーバーは `http://localhost:{port}` をリッスンするように、IIS 統合ミドルウェアによって構成されます。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-164">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="8aa6d-165">追加のチェックが実行され、モジュールから発生していない要求は拒否されます。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-165">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="8aa6d-166">モジュールでは HTTPS 転送がサポートされていないため、要求は HTTPS を介して IIS によって受信された場合でも、HTTP を介して転送されます。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-166">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="8aa6d-167">Kestrel によってモジュールから要求が取り込まれた後、その要求は ASP.NET Core ミドルウェア パイプラインにプッシュされます。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-167">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="8aa6d-168">ミドルウェア パイプラインは要求を処理し、`HttpContext` インスタンスとしてアプリのロジックに渡します。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-168">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="8aa6d-169">IIS 統合によって追加されたミドルウェアでは、カーネルへの要求の転送を考慮して、スキーム、リモート IP、およびパスベースが更新されます。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-169">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="8aa6d-170">アプリの応答が IIS に戻され、IIS はその応答を、要求を開始した HTTP クライアントに返します。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-170">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

::: moniker-end

<span data-ttu-id="8aa6d-171">Windows 認証などの多くのネイティブなモジュールは、アクティブなままです。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-171">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="8aa6d-172">このモジュールと共にアクティブな IIS モジュールの詳細については、「<xref:host-and-deploy/iis/modules>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-172">To learn more about IIS modules active with the module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="8aa6d-173">ASP.NET Core モジュールには、他にも機能があります。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-173">The ASP.NET Core Module has a few other functions.</span></span> <span data-ttu-id="8aa6d-174">モジュールでは次の操作を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-174">The module can:</span></span>

* <span data-ttu-id="8aa6d-175">ワーカー プロセスの環境変数を設定する</span><span class="sxs-lookup"><span data-stu-id="8aa6d-175">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="8aa6d-176">起動に関する問題をトラブルシューティングするために、Stdout 出力をファイル ストレージに記録する</span><span class="sxs-lookup"><span data-stu-id="8aa6d-176">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="8aa6d-177">Windows 認証トークンを転送する</span><span class="sxs-lookup"><span data-stu-id="8aa6d-177">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="8aa6d-178">ASP.NET Core モジュールをインストールして使用する方法</span><span class="sxs-lookup"><span data-stu-id="8aa6d-178">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="8aa6d-179">ASP.NET Core モジュールをインストールして使用する方法の詳細な手順については、「<xref:host-and-deploy/iis/index>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-179">For detailed instructions on how to install and use the ASP.NET Core Module, see <xref:host-and-deploy/iis/index>.</span></span> <span data-ttu-id="8aa6d-180">モジュールの構成の詳細については、「<xref:host-and-deploy/aspnet-core-module>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8aa6d-180">For information on configuring the module, see the <xref:host-and-deploy/aspnet-core-module>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8aa6d-181">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="8aa6d-181">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* [<span data-ttu-id="8aa6d-182">ASP.NET Core モジュールの GitHub リポジトリ (ソース コード)</span><span class="sxs-lookup"><span data-stu-id="8aa6d-182">ASP.NET Core Module GitHub repository (source code)</span></span>](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>
