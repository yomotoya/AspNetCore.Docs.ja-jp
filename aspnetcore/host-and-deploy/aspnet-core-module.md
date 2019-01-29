---
title: ASP.NET Core モジュール
author: guardrex
description: ASP.NET Core アプリをホストするための ASP.NET Core モジュールを構成する方法について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 01/22/2019
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 4eea360d08c79b889db00132109cf49492f84de6
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837781"
---
# <a name="aspnet-core-module"></a><span data-ttu-id="b4cb7-103">ASP.NET Core モジュール</span><span class="sxs-lookup"><span data-stu-id="b4cb7-103">ASP.NET Core Module</span></span>

<span data-ttu-id="b4cb7-104">作成者: [Tom Dykstra](https://github.com/tdykstra)、[Rick Strahl](https://github.com/RickStrahl)、[Chris Ross](https://github.com/Tratcher)、[Rick Anderson](https://twitter.com/RickAndMSFT)、[Sourabh Shirhatti](https://twitter.com/sshirhatti)、[Justin Kotalik](https://github.com/jkotalik)、[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="b4cb7-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [Justin Kotalik](https://github.com/jkotalik), and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="b4cb7-105">ASP.NET Core モジュールはネイティブな IIS モジュールであり、次のいずれかを目的として、IIS パイプラインにプラグインされます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-105">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="b4cb7-106">IIS ワーカー プロセス (`w3wp.exe`) の内部で ASP.NET Core アプリをホストします。これは、[インプロセス ホスティング モデル](#in-process-hosting-model)と呼ばれています。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-106">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>
* <span data-ttu-id="b4cb7-107">[Kestrel サーバー](xref:fundamentals/servers/kestrel)を実行しているバックエンドの ASP.NET Core アプリに Web 要求を転送します。これは、[アウト プロセス ホスティング モデル](#out-of-process-hosting-model)と呼ばれています。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-107">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

<span data-ttu-id="b4cb7-108">サポートされている Windows バージョン:</span><span class="sxs-lookup"><span data-stu-id="b4cb7-108">Supported Windows versions:</span></span>

* <span data-ttu-id="b4cb7-109">Windows 7 以降</span><span class="sxs-lookup"><span data-stu-id="b4cb7-109">Windows 7 or later</span></span>
* <span data-ttu-id="b4cb7-110">Windows Server 2008 R2 以降</span><span class="sxs-lookup"><span data-stu-id="b4cb7-110">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="b4cb7-111">インプロセス ホスティングの場合、モジュールでは IIS HTTP サーバー (`IISHttpServer`) と呼ばれる IIS 用のインプロセス サーバー実装が使用されます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-111">When hosting in-process, the module uses an in-process server implementation for IIS, called IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="b4cb7-112">アウト プロセスでホストする場合、モジュールは Kestrel に対してのみ機能します。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-112">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="b4cb7-113">モジュールは [HTTP.sys](xref:fundamentals/servers/httpsys) と互換性はありません。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-113">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

## <a name="hosting-models"></a><span data-ttu-id="b4cb7-114">ホスティング モデル</span><span class="sxs-lookup"><span data-stu-id="b4cb7-114">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="b4cb7-115">インプロセス ホスティング モデル</span><span class="sxs-lookup"><span data-stu-id="b4cb7-115">In-process hosting model</span></span>

<span data-ttu-id="b4cb7-116">アプリに対してインプロセス ホスティングを構成するには、そのアプリのプロジェクト ファイルに `<AspNetCoreHostingModel>` プロパティを追加して、値を `InProcess` に設定します。(アウト プロセス ホスティングは `OutOfProcess` を使用して設定します)。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-116">To configure an app for in-process hosting, add the `<AspNetCoreHostingModel>` property to the app's project file with a value of `InProcess` (out-of-process hosting is set with `OutOfProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>InProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="b4cb7-117">.NET Framework をターゲットにする ASP.NET Core アプリではインプロセス ホスティング モデルはサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-117">The in-process hosting model isn't supported for ASP.NET Core apps that target the .NET Framework.</span></span>

<span data-ttu-id="b4cb7-118">`<AspNetCoreHostingModel>` プロパティがファイルに存在しない場合、既定値は `OutOfProcess` です。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-118">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>

<span data-ttu-id="b4cb7-119">インプロセスでホストする場合は、次の特性が適用されます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-119">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="b4cb7-120">[Kestrel](xref:fundamentals/servers/kestrel) サーバーの代わりに、IIS HTTP サーバー (`IISHttpServer`) が使用されます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-120">IIS HTTP Server (`IISHttpServer`) is used instead of [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

* <span data-ttu-id="b4cb7-121">[requestTimeout 属性](#attributes-of-the-aspnetcore-element) は、インプロセス ホスティングには適用されません。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-121">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="b4cb7-122">アプリ プールをアプリ間で共有することはサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-122">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="b4cb7-123">アプリごとに 1 つのアプリ プールを使用します。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-123">Use one app pool per app.</span></span>

* <span data-ttu-id="b4cb7-124">[Web 配置](/iis/publish/using-web-deploy/introduction-to-web-deploy)を使用したとき、または [app_offline.htm ファイルを手動で配置内に置いた](xref:host-and-deploy/iis/index#locked-deployment-files)ときには、開いている接続があると、アプリがすぐにシャットダウンされない場合があります。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-124">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="b4cb7-125">たとえば、WebSocket 接続では、アプリのシャットダウンが遅れる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-125">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="b4cb7-126">アプリとインストールされているランタイム (x64 または x86) のアーキテクチャ (ビット) は、アプリ プールのアーキテクチャと一致する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-126">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="b4cb7-127">([CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) ではなく) `WebHostBuilder` を使用してアプリのホストを手動で設定するときに、アプリがこれまで Kestrel サーバー上で直接実行されていた場合 (セルフホステッド)、`UseKestrel` を呼び出してから `UseIISIntegration` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-127">If setting up the app's host manually with `WebHostBuilder` (not using [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) and the app is ever run directly on the Kestrel server (self-hosted), call `UseKestrel` before calling `UseIISIntegration`.</span></span> <span data-ttu-id="b4cb7-128">順序を逆にすると、ホストは開始に失敗します。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-128">If the order is reversed, the host fails to start.</span></span>

* <span data-ttu-id="b4cb7-129">クライアントの切断が検出されます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-129">Client disconnects are detected.</span></span> <span data-ttu-id="b4cb7-130">クライアントが切断されると、[HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) キャンセル トークンが取り消されます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-130">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="b4cb7-131"><xref:System.IO.Directory.GetCurrentDirectory*> は、アプリのディレクトリではなく、IIS によって開始するプロセスのワーカー ディレクトリを返します (たとえば、*w3wp.exe* に対して *C:\Windows\System32\inetsrv*)。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-131"><xref:System.IO.Directory.GetCurrentDirectory*> returns the worker directory of the process started by IIS rather than the app's directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

  <span data-ttu-id="b4cb7-132">アプリの現在のディレクトリを設定するサンプル コードについては、「[CurrentDirectoryHelpers クラス](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-132">For sample code that sets the app's current directory, see the [CurrentDirectoryHelpers class](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span></span> <span data-ttu-id="b4cb7-133">`SetCurrentDirectory` メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-133">Call the `SetCurrentDirectory` method.</span></span> <span data-ttu-id="b4cb7-134"><xref:System.IO.Directory.GetCurrentDirectory*> の後続の呼び出しによって、アプリのディレクトリが指定されます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-134">Subsequent calls to <xref:System.IO.Directory.GetCurrentDirectory*> provide the app's directory.</span></span>

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="b4cb7-135">アウト プロセス ホスティング モデル</span><span class="sxs-lookup"><span data-stu-id="b4cb7-135">Out-of-process hosting model</span></span>

<span data-ttu-id="b4cb7-136">アウト プロセス ホスティング用のアプリを構成するには、プロジェクト ファイルで次の方法のいずれかを使用します。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-136">To configure an app for out-of-process hosting, use either of the following approaches in the project file:</span></span>

* <span data-ttu-id="b4cb7-137">`<AspNetCoreHostingModel>` プロパティは指定しないでください。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-137">Don't specify the `<AspNetCoreHostingModel>` property.</span></span> <span data-ttu-id="b4cb7-138">`<AspNetCoreHostingModel>` プロパティがファイルに存在しない場合、既定値は `OutOfProcess` です。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-138">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>
* <span data-ttu-id="b4cb7-139">`<AspNetCoreHostingModel>` プロパティの値を `OutOfProcess` に設定します (インプロセス ホスティングは `InProcess` で設定されます)。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-139">Set the value of the `<AspNetCoreHostingModel>` property to `OutOfProcess` (in-process hosting is set with `InProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="b4cb7-140">IIS HTTP サーバー (`IISHttpServer`) の代わりに、[Kestrel](xref:fundamentals/servers/kestrel) サーバーが使用されます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-140">[Kestrel](xref:fundamentals/servers/kestrel) server is used instead of IIS HTTP Server (`IISHttpServer`).</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="b4cb7-141">ホスティング モデルの変更</span><span class="sxs-lookup"><span data-stu-id="b4cb7-141">Hosting model changes</span></span>

<span data-ttu-id="b4cb7-142">`hostingModel` 設定が *web.config* ファイル内で変更された場合 (「[web.config での構成](#configuration-with-webconfig)」セクションを参照)、モジュールによって IIS 用のワーカー プロセスがリサイクルされます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-142">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="b4cb7-143">IIS Express の場合、モジュールによってワーカー プロセスのリサイクルは行われませんが、代わりに、現在の IIS Express プロセスの正常なシャットダウンがトリガーされます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-143">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="b4cb7-144">アプリに対して次の要求が出されると、IIS Express の新しいプロセスが生成されます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-144">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="b4cb7-145">プロセス名</span><span class="sxs-lookup"><span data-stu-id="b4cb7-145">Process name</span></span>

<span data-ttu-id="b4cb7-146">`Process.GetCurrentProcess().ProcessName` から、`w3wp`/`iisexpress` (インプロセス) または `dotnet` (アウト プロセス) がレポートされます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-146">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="b4cb7-147">ASP.NET Core モジュールは、ネイティブな IIS モジュールであり、バックエンドの ASP.NET Core アプリに Web 要求を転送するために IIS パイプラインにプラグインされます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-147">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to forward web requests to backend ASP.NET Core apps.</span></span>

<span data-ttu-id="b4cb7-148">サポートされている Windows バージョン:</span><span class="sxs-lookup"><span data-stu-id="b4cb7-148">Supported Windows versions:</span></span>

* <span data-ttu-id="b4cb7-149">Windows 7 以降</span><span class="sxs-lookup"><span data-stu-id="b4cb7-149">Windows 7 or later</span></span>
* <span data-ttu-id="b4cb7-150">Windows Server 2008 R2 以降</span><span class="sxs-lookup"><span data-stu-id="b4cb7-150">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="b4cb7-151">モジュールは、Kestrel に対してのみ機能します。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-151">The module only works with Kestrel.</span></span> <span data-ttu-id="b4cb7-152">モジュールは [HTTP.sys](xref:fundamentals/servers/httpsys) と互換性はありません。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-152">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="b4cb7-153">ASP.NET Core アプリは IIS ワーカー プロセスとは独立したプロセスで実行されるため、モジュールもプロセス管理を処理します。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-153">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="b4cb7-154">モジュールは、最初の要求を受信したときに ASP.NET Core アプリのプロセスを開始し、プロセスがクラッシュした場合はアプリを再起動します。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-154">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="b4cb7-155">この動作は、IIS のインプロセスで実行され、[WAS (Windows プロセス アクティブ化サービス)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) によって管理される ASP.NET 4.x アプリと基本的に同じです。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-155">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="b4cb7-156">次の図は、IIS (ASP.NET Core モジュール) とアプリの間のリレーションシップを示しています。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-156">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app:</span></span>

![ASP.NET Core モジュール](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="b4cb7-158">要求は、Web からカーネル モードの HTTP.sys ドライバーに到着します。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-158">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="b4cb7-159">ドライバーは、Web サイトの構成ポート (通常は 80 (HTTP) または 443 (HTTPS)) で IIS への要求をルーティングします。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-159">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="b4cb7-160">モジュールでは、アプリのランダムなポート (ポート 80 または 443 ではありません) で Kestrel に要求が転送されます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-160">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="b4cb7-161">モジュールが起動時に環境変数を介してポートを指定すると、サーバーは `http://localhost:{port}` をリッスンするように、IIS 統合ミドルウェアによって構成されます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-161">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="b4cb7-162">追加のチェックが実行され、モジュールから発生していない要求は拒否されます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-162">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="b4cb7-163">モジュールでは HTTPS 転送がサポートされていないため、要求は HTTPS を介して IIS によって受信された場合でも、HTTP を介して転送されます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-163">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="b4cb7-164">Kestrel によってモジュールから要求が取り込まれた後、その要求は ASP.NET Core ミドルウェア パイプラインにプッシュされます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-164">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="b4cb7-165">ミドルウェア パイプラインは要求を処理し、`HttpContext` インスタンスとしてアプリのロジックに渡します。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-165">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="b4cb7-166">IIS 統合によって追加されたミドルウェアでは、カーネルへの要求の転送を考慮して、スキーム、リモート IP、およびパスベースが更新されます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-166">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="b4cb7-167">アプリの応答が IIS に戻され、IIS はその応答を、要求を開始した HTTP クライアントに返します。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-167">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

::: moniker-end

<span data-ttu-id="b4cb7-168">Windows 認証などの多くのネイティブなモジュールは、アクティブなままです。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-168">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="b4cb7-169">ASP.NET Core モジュールと共にアクティブな IIS モジュールの詳細については、「<xref:host-and-deploy/iis/modules>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-169">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="b4cb7-170">ASP.NET Core モジュールでは次のことも行えます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-170">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="b4cb7-171">ワーカー プロセスの環境変数を設定する</span><span class="sxs-lookup"><span data-stu-id="b4cb7-171">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="b4cb7-172">起動に関する問題をトラブルシューティングするために、Stdout 出力をファイル ストレージに記録する</span><span class="sxs-lookup"><span data-stu-id="b4cb7-172">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="b4cb7-173">Windows 認証トークンを転送する</span><span class="sxs-lookup"><span data-stu-id="b4cb7-173">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="b4cb7-174">ASP.NET Core モジュールをインストールして使用する方法</span><span class="sxs-lookup"><span data-stu-id="b4cb7-174">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="b4cb7-175">ASP.NET Core モジュールをインストールして使用する方法の詳細については、「<xref:host-and-deploy/iis/index>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-175">For instructions on how to install and use the ASP.NET Core Module, see <xref:host-and-deploy/iis/index>.</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="b4cb7-176">web.config での構成</span><span class="sxs-lookup"><span data-stu-id="b4cb7-176">Configuration with web.config</span></span>

<span data-ttu-id="b4cb7-177">ASP.NET Core モジュールは、サイトの *web.config* ファイルの `system.webServer` ノードの `aspNetCore` セクションを使って構成します。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-177">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="b4cb7-178">次に示す *web.config* ファイルは、[フレームワークに依存する展開](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd)用に発行されたもので、サイトの要求を処理するように ASP.NET Core モジュールを構成します。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-178">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <handlers>
        <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModuleV2" resourceType="Unspecified" />
      </handlers>
      <aspNetCore processPath="dotnet" 
                  arguments=".\MyApp.dll" 
                  stdoutLogEnabled="false" 
                  stdoutLogFile=".\logs\stdout" 
                  hostingModel="InProcess" />
    </system.webServer>
  </location>
</configuration>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="dotnet" 
                arguments=".\MyApp.dll" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

::: moniker-end

<span data-ttu-id="b4cb7-179">次の *web.config* は、[自己完結型の展開](/dotnet/articles/core/deploying/#self-contained-deployments-scd)用に発行されたものです。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-179">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <handlers>
        <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModuleV2" resourceType="Unspecified" />
      </handlers>
      <aspNetCore processPath=".\MyApp.exe" 
                  stdoutLogEnabled="false" 
                  stdoutLogFile=".\logs\stdout" 
                  hostingModel="InProcess" />
    </system.webServer>
  </location>
</configuration>
```

<span data-ttu-id="b4cb7-180"><xref:System.Configuration.SectionInformation.InheritInChildApplications*> プロパティは、`false` に設定されます。これは、[\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) 要素内で指定された設定が、アプリのサブディレクトリにあるアプリによって継承されないことを示します。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-180">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath=".\MyApp.exe" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

::: moniker-end

<span data-ttu-id="b4cb7-181">アプリが [Azure App Service](https://azure.microsoft.com/services/app-service/) に対して展開されると、`stdoutLogFile` パスは `\\?\%home%\LogFiles\stdout` に設定されます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-181">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="b4cb7-182">パスは stdout ログを *LogFiles* フォルダーに保存します。これは、サービスによって自動的に作成される場所です。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-182">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="b4cb7-183">IIS サブアプリケーション構成について詳しくは、「<xref:host-and-deploy/iis/index#sub-applications>」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-183">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="b4cb7-184">aspNetCore 要素の属性</span><span class="sxs-lookup"><span data-stu-id="b4cb7-184">Attributes of the aspNetCore element</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="b4cb7-185">属性</span><span class="sxs-lookup"><span data-stu-id="b4cb7-185">Attribute</span></span> | <span data-ttu-id="b4cb7-186">説明</span><span class="sxs-lookup"><span data-stu-id="b4cb7-186">Description</span></span> | <span data-ttu-id="b4cb7-187">既定値</span><span class="sxs-lookup"><span data-stu-id="b4cb7-187">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="b4cb7-188">省略可能な文字列属性。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-188">Optional string attribute.</span></span></p><p><span data-ttu-id="b4cb7-189">**processPath** において指定されている実行可能ファイルへの引数です。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-189">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="b4cb7-190">省略可能な Boolean 属性です。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-190">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="b4cb7-191">true の場合、**502.5 - 処理エラー** ページは抑制され、*web.config* で構成されている 502 状態コード ページが優先されます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-191">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="b4cb7-192">省略可能な Boolean 属性です。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-192">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="b4cb7-193">true の場合、トークンは、%ASPNETCORE_PORT% でリッスンしている子プロセスに、要求ごとの 'MS-ASPNETCORE-WINAUTHTOKEN' ヘッダーとして転送されます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-193">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="b4cb7-194">要求ごとのこのトークンで CloseHandle を呼び出すのは、そのプロセスの役割です。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-194">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="b4cb7-195">省略可能な文字列属性。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-195">Optional string attribute.</span></span></p><p><span data-ttu-id="b4cb7-196">ホスティング モデルをインプロセス (`InProcess`) またはアウト プロセス (`OutOfProcess`) として指定します。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-196">Specifies the hosting model as in-process (`InProcess`) or out-of-process (`OutOfProcess`).</span></span></p> | `OutOfProcess` |
| `processesPerApplication` | <p><span data-ttu-id="b4cb7-197">省略可能な整数属性</span><span class="sxs-lookup"><span data-stu-id="b4cb7-197">Optional integer attribute.</span></span></p><p><span data-ttu-id="b4cb7-198">アプリごとにスピンアップすることができる **processPath** 設定内で指定したプロセスのインスタンス数が指定されます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-198">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="b4cb7-199">&dagger;インプロセス ホスティングの場合、値は `1` に制限されます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-199">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p> | <span data-ttu-id="b4cb7-200">既定値: `1`</span><span class="sxs-lookup"><span data-stu-id="b4cb7-200">Default: `1`</span></span><br><span data-ttu-id="b4cb7-201">最小値: `1`</span><span class="sxs-lookup"><span data-stu-id="b4cb7-201">Min: `1`</span></span><br><span data-ttu-id="b4cb7-202">最大値: `100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="b4cb7-202">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="b4cb7-203">必須の文字列属性です。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-203">Required string attribute.</span></span></p><p><span data-ttu-id="b4cb7-204">HTTP 要求をリッスンするプロセスを起動する実行可能ファイルへのパスです。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-204">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="b4cb7-205">相対パスがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-205">Relative paths are supported.</span></span> <span data-ttu-id="b4cb7-206">パスが `.` で始まる場合、パスはサイトのルートを基準とする相対パスであると見なされます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-206">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="b4cb7-207">省略可能な整数属性</span><span class="sxs-lookup"><span data-stu-id="b4cb7-207">Optional integer attribute.</span></span></p><p><span data-ttu-id="b4cb7-208">**processPath** で指定されているプロセスが 1 分間にクラッシュできる回数を指定します。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-208">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="b4cb7-209">この制限を超えた場合、モジュールは、1 分間の残りの間、プロセスの起動を停止します。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-209">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="b4cb7-210">インプロセス ホスティングではサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-210">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="b4cb7-211">既定値: `10`</span><span class="sxs-lookup"><span data-stu-id="b4cb7-211">Default: `10`</span></span><br><span data-ttu-id="b4cb7-212">最小値: `0`</span><span class="sxs-lookup"><span data-stu-id="b4cb7-212">Min: `0`</span></span><br><span data-ttu-id="b4cb7-213">最大値: `100`</span><span class="sxs-lookup"><span data-stu-id="b4cb7-213">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="b4cb7-214">省略可能な期間属性。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-214">Optional timespan attribute.</span></span></p><p><span data-ttu-id="b4cb7-215">%ASPNETCORE_PORT% でリッスンしているプロセスからの応答を ASP.NET Core モジュールが待機する期間を指定します。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-215">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="b4cb7-216">ASP.NET Core 2.1 以降のリリースに付属する ASP.NET Core モジュールのバージョンでは、`requestTimeout` は時間、分、および秒単位で指定します。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-216">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="b4cb7-217">インプロセス ホスティングには適用されません。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-217">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="b4cb7-218">インプロセス ホスティングの場合、アプリによって要求が処理されるまでモジュールは待機します。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-218">For in-process hosting, the module waits for the app to process the request.</span></span></p> | <span data-ttu-id="b4cb7-219">既定値: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="b4cb7-219">Default: `00:02:00`</span></span><br><span data-ttu-id="b4cb7-220">最小値: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="b4cb7-220">Min: `00:00:00`</span></span><br><span data-ttu-id="b4cb7-221">最大値: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="b4cb7-221">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="b4cb7-222">省略可能な整数属性</span><span class="sxs-lookup"><span data-stu-id="b4cb7-222">Optional integer attribute.</span></span></p><p><span data-ttu-id="b4cb7-223">*app_offline.htm* ファイルが検出されたときに、モジュールが実行可能ファイルの正常なシャットダウンを待機する秒単位の期間です。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-223">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="b4cb7-224">既定値: `10`</span><span class="sxs-lookup"><span data-stu-id="b4cb7-224">Default: `10`</span></span><br><span data-ttu-id="b4cb7-225">最小値: `0`</span><span class="sxs-lookup"><span data-stu-id="b4cb7-225">Min: `0`</span></span><br><span data-ttu-id="b4cb7-226">最大値: `600`</span><span class="sxs-lookup"><span data-stu-id="b4cb7-226">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="b4cb7-227">省略可能な整数属性</span><span class="sxs-lookup"><span data-stu-id="b4cb7-227">Optional integer attribute.</span></span></p><p><span data-ttu-id="b4cb7-228">ポートでリッスンするプロセスを実行可能ファイルが開始するのをモジュールが待機する秒単位の期間です。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-228">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="b4cb7-229">この制限時間を超えた場合、モジュールはプロセスを強制終了します。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-229">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="b4cb7-230">最後のローリング分においてアプリが **rapidFailsPerMinute** 回だけ開始を失敗しているのでないかぎり、モジュールは、新しい要求を受け取るとプロセスの再起動を試み、それ以降に受信した要求に対してプロセスの再起動を試み続けます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-230">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="b4cb7-231">値 0 (ゼロ) は無限のタイムアウトと見なされ**ません**。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-231">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="b4cb7-232">既定値: `120`</span><span class="sxs-lookup"><span data-stu-id="b4cb7-232">Default: `120`</span></span><br><span data-ttu-id="b4cb7-233">最小値: `0`</span><span class="sxs-lookup"><span data-stu-id="b4cb7-233">Min: `0`</span></span><br><span data-ttu-id="b4cb7-234">最大値: `3600`</span><span class="sxs-lookup"><span data-stu-id="b4cb7-234">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="b4cb7-235">省略可能な Boolean 属性です。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-235">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="b4cb7-236">true の場合、**processPath** で指定されているプロセスの **stdout** と **stderr** は、**stdoutLogFile** で指定されているファイルにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-236">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="b4cb7-237">省略可能な文字列属性。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-237">Optional string attribute.</span></span></p><p><span data-ttu-id="b4cb7-238">**processPath** で指定されているプロセスからの **stdout** と **stderr** がログに記録される相対ファイル パスまたは絶対ファイル パスを指定します。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-238">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="b4cb7-239">相対パスの基準はサイトのルートです。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-239">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="b4cb7-240">`.` で始まっているパスはすべてサイト ルートに対する相対パスであり、他のすべてのパスは絶対パスとして扱われます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-240">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="b4cb7-241">パスで指定されたフォルダーは、ログ ファイルの作成時、モジュールによって作成されます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-241">Any folders provided in the path are created by the module when the log file is created.</span></span> <span data-ttu-id="b4cb7-242">アンダースコアの区切り記号を使って、タイムスタンプ、プロセス ID、およびファイル拡張子 (*.log*) が、**stdoutLogFile** パスの最後のセグメントに追加されます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-242">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="b4cb7-243">たとえば、値として `.\logs\stdout` を指定し、2018 年 2 月 5 日の 19:41:32 にプロセス ID 1934 で保存すると、stdout ログは *logs* フォルダーに *stdout_20180205194132_1934.log* として保存されます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-243">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="b4cb7-244">属性</span><span class="sxs-lookup"><span data-stu-id="b4cb7-244">Attribute</span></span> | <span data-ttu-id="b4cb7-245">説明</span><span class="sxs-lookup"><span data-stu-id="b4cb7-245">Description</span></span> | <span data-ttu-id="b4cb7-246">既定値</span><span class="sxs-lookup"><span data-stu-id="b4cb7-246">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="b4cb7-247">省略可能な文字列属性。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-247">Optional string attribute.</span></span></p><p><span data-ttu-id="b4cb7-248">**processPath** において指定されている実行可能ファイルへの引数です。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-248">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="b4cb7-249">省略可能な Boolean 属性です。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-249">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="b4cb7-250">true の場合、**502.5 - 処理エラー** ページは抑制され、*web.config* で構成されている 502 状態コード ページが優先されます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-250">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="b4cb7-251">省略可能な Boolean 属性です。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-251">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="b4cb7-252">true の場合、トークンは、%ASPNETCORE_PORT% でリッスンしている子プロセスに、要求ごとの 'MS-ASPNETCORE-WINAUTHTOKEN' ヘッダーとして転送されます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-252">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="b4cb7-253">要求ごとのこのトークンで CloseHandle を呼び出すのは、そのプロセスの役割です。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-253">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="b4cb7-254">省略可能な整数属性</span><span class="sxs-lookup"><span data-stu-id="b4cb7-254">Optional integer attribute.</span></span></p><p><span data-ttu-id="b4cb7-255">アプリごとにスピンアップすることができる **processPath** 設定内で指定したプロセスのインスタンス数が指定されます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-255">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="b4cb7-256">既定値: `1`</span><span class="sxs-lookup"><span data-stu-id="b4cb7-256">Default: `1`</span></span><br><span data-ttu-id="b4cb7-257">最小値: `1`</span><span class="sxs-lookup"><span data-stu-id="b4cb7-257">Min: `1`</span></span><br><span data-ttu-id="b4cb7-258">最大値: `100`</span><span class="sxs-lookup"><span data-stu-id="b4cb7-258">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="b4cb7-259">必須の文字列属性です。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-259">Required string attribute.</span></span></p><p><span data-ttu-id="b4cb7-260">HTTP 要求をリッスンするプロセスを起動する実行可能ファイルへのパスです。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-260">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="b4cb7-261">相対パスがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-261">Relative paths are supported.</span></span> <span data-ttu-id="b4cb7-262">パスが `.` で始まる場合、パスはサイトのルートを基準とする相対パスであると見なされます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-262">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="b4cb7-263">省略可能な整数属性</span><span class="sxs-lookup"><span data-stu-id="b4cb7-263">Optional integer attribute.</span></span></p><p><span data-ttu-id="b4cb7-264">**processPath** で指定されているプロセスが 1 分間にクラッシュできる回数を指定します。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-264">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="b4cb7-265">この制限を超えた場合、モジュールは、1 分間の残りの間、プロセスの起動を停止します。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-265">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="b4cb7-266">既定値: `10`</span><span class="sxs-lookup"><span data-stu-id="b4cb7-266">Default: `10`</span></span><br><span data-ttu-id="b4cb7-267">最小値: `0`</span><span class="sxs-lookup"><span data-stu-id="b4cb7-267">Min: `0`</span></span><br><span data-ttu-id="b4cb7-268">最大値: `100`</span><span class="sxs-lookup"><span data-stu-id="b4cb7-268">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="b4cb7-269">省略可能な期間属性。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-269">Optional timespan attribute.</span></span></p><p><span data-ttu-id="b4cb7-270">%ASPNETCORE_PORT% でリッスンしているプロセスからの応答を ASP.NET Core モジュールが待機する期間を指定します。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-270">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="b4cb7-271">ASP.NET Core 2.1 以降のリリースに付属する ASP.NET Core モジュールのバージョンでは、`requestTimeout` は時間、分、および秒単位で指定します。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-271">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="b4cb7-272">既定値: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="b4cb7-272">Default: `00:02:00`</span></span><br><span data-ttu-id="b4cb7-273">最小値: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="b4cb7-273">Min: `00:00:00`</span></span><br><span data-ttu-id="b4cb7-274">最大値: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="b4cb7-274">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="b4cb7-275">省略可能な整数属性</span><span class="sxs-lookup"><span data-stu-id="b4cb7-275">Optional integer attribute.</span></span></p><p><span data-ttu-id="b4cb7-276">*app_offline.htm* ファイルが検出されたときに、モジュールが実行可能ファイルの正常なシャットダウンを待機する秒単位の期間です。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-276">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="b4cb7-277">既定値: `10`</span><span class="sxs-lookup"><span data-stu-id="b4cb7-277">Default: `10`</span></span><br><span data-ttu-id="b4cb7-278">最小値: `0`</span><span class="sxs-lookup"><span data-stu-id="b4cb7-278">Min: `0`</span></span><br><span data-ttu-id="b4cb7-279">最大値: `600`</span><span class="sxs-lookup"><span data-stu-id="b4cb7-279">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="b4cb7-280">省略可能な整数属性</span><span class="sxs-lookup"><span data-stu-id="b4cb7-280">Optional integer attribute.</span></span></p><p><span data-ttu-id="b4cb7-281">ポートでリッスンするプロセスを実行可能ファイルが開始するのをモジュールが待機する秒単位の期間です。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-281">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="b4cb7-282">この制限時間を超えた場合、モジュールはプロセスを強制終了します。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-282">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="b4cb7-283">最後のローリング分においてアプリが **rapidFailsPerMinute** 回だけ開始を失敗しているのでないかぎり、モジュールは、新しい要求を受け取るとプロセスの再起動を試み、それ以降に受信した要求に対してプロセスの再起動を試み続けます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-283">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="b4cb7-284">値 0 (ゼロ) は無限のタイムアウトと見なされ**ません**。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-284">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="b4cb7-285">既定値: `120`</span><span class="sxs-lookup"><span data-stu-id="b4cb7-285">Default: `120`</span></span><br><span data-ttu-id="b4cb7-286">最小値: `0`</span><span class="sxs-lookup"><span data-stu-id="b4cb7-286">Min: `0`</span></span><br><span data-ttu-id="b4cb7-287">最大値: `3600`</span><span class="sxs-lookup"><span data-stu-id="b4cb7-287">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="b4cb7-288">省略可能な Boolean 属性です。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-288">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="b4cb7-289">true の場合、**processPath** で指定されているプロセスの **stdout** と **stderr** は、**stdoutLogFile** で指定されているファイルにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-289">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="b4cb7-290">省略可能な文字列属性。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-290">Optional string attribute.</span></span></p><p><span data-ttu-id="b4cb7-291">**processPath** で指定されているプロセスからの **stdout** と **stderr** がログに記録される相対ファイル パスまたは絶対ファイル パスを指定します。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-291">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="b4cb7-292">相対パスの基準はサイトのルートです。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-292">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="b4cb7-293">`.` で始まっているパスはすべてサイト ルートに対する相対パスであり、他のすべてのパスは絶対パスとして扱われます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-293">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="b4cb7-294">モジュールがログ ファイルを作成するためには、パスで指定されているすべてのフォルダーが存在する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-294">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="b4cb7-295">アンダースコアの区切り記号を使って、タイムスタンプ、プロセス ID、およびファイル拡張子 (*.log*) が、**stdoutLogFile** パスの最後のセグメントに追加されます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-295">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="b4cb7-296">たとえば、値として `.\logs\stdout` を指定し、2018 年 2 月 5 日の 19:41:32 にプロセス ID 1934 で保存すると、stdout ログは *logs* フォルダーに *stdout_20180205194132_1934.log* として保存されます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-296">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

| <span data-ttu-id="b4cb7-297">属性</span><span class="sxs-lookup"><span data-stu-id="b4cb7-297">Attribute</span></span> | <span data-ttu-id="b4cb7-298">説明</span><span class="sxs-lookup"><span data-stu-id="b4cb7-298">Description</span></span> | <span data-ttu-id="b4cb7-299">既定値</span><span class="sxs-lookup"><span data-stu-id="b4cb7-299">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="b4cb7-300">省略可能な文字列属性。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-300">Optional string attribute.</span></span></p><p><span data-ttu-id="b4cb7-301">**processPath** において指定されている実行可能ファイルへの引数です。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-301">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="b4cb7-302">省略可能な Boolean 属性です。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-302">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="b4cb7-303">true の場合、**502.5 - 処理エラー** ページは抑制され、*web.config* で構成されている 502 状態コード ページが優先されます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-303">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="b4cb7-304">省略可能な Boolean 属性です。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-304">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="b4cb7-305">true の場合、トークンは、%ASPNETCORE_PORT% でリッスンしている子プロセスに、要求ごとの 'MS-ASPNETCORE-WINAUTHTOKEN' ヘッダーとして転送されます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-305">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="b4cb7-306">要求ごとのこのトークンで CloseHandle を呼び出すのは、そのプロセスの役割です。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-306">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="b4cb7-307">省略可能な整数属性</span><span class="sxs-lookup"><span data-stu-id="b4cb7-307">Optional integer attribute.</span></span></p><p><span data-ttu-id="b4cb7-308">アプリごとにスピンアップすることができる **processPath** 設定内で指定したプロセスのインスタンス数が指定されます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-308">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="b4cb7-309">既定値: `1`</span><span class="sxs-lookup"><span data-stu-id="b4cb7-309">Default: `1`</span></span><br><span data-ttu-id="b4cb7-310">最小値: `1`</span><span class="sxs-lookup"><span data-stu-id="b4cb7-310">Min: `1`</span></span><br><span data-ttu-id="b4cb7-311">最大値: `100`</span><span class="sxs-lookup"><span data-stu-id="b4cb7-311">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="b4cb7-312">必須の文字列属性です。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-312">Required string attribute.</span></span></p><p><span data-ttu-id="b4cb7-313">HTTP 要求をリッスンするプロセスを起動する実行可能ファイルへのパスです。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-313">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="b4cb7-314">相対パスがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-314">Relative paths are supported.</span></span> <span data-ttu-id="b4cb7-315">パスが `.` で始まる場合、パスはサイトのルートを基準とする相対パスであると見なされます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-315">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="b4cb7-316">省略可能な整数属性</span><span class="sxs-lookup"><span data-stu-id="b4cb7-316">Optional integer attribute.</span></span></p><p><span data-ttu-id="b4cb7-317">**processPath** で指定されているプロセスが 1 分間にクラッシュできる回数を指定します。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-317">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="b4cb7-318">この制限を超えた場合、モジュールは、1 分間の残りの間、プロセスの起動を停止します。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-318">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="b4cb7-319">既定値: `10`</span><span class="sxs-lookup"><span data-stu-id="b4cb7-319">Default: `10`</span></span><br><span data-ttu-id="b4cb7-320">最小値: `0`</span><span class="sxs-lookup"><span data-stu-id="b4cb7-320">Min: `0`</span></span><br><span data-ttu-id="b4cb7-321">最大値: `100`</span><span class="sxs-lookup"><span data-stu-id="b4cb7-321">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="b4cb7-322">省略可能な期間属性。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-322">Optional timespan attribute.</span></span></p><p><span data-ttu-id="b4cb7-323">%ASPNETCORE_PORT% でリッスンしているプロセスからの応答を ASP.NET Core モジュールが待機する期間を指定します。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-323">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="b4cb7-324">ASP.NET Core 2.0 以前のリリースに付属する ASP.NET Core モジュールのバージョンでは、`requestTimeout` は整数でのみ指定する必要があります。そうしないと、既定値の 2 分に設定されます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-324">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.0 or earlier, the `requestTimeout` must be specified in whole minutes only, otherwise it defaults to 2 minutes.</span></span></p> | <span data-ttu-id="b4cb7-325">既定値: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="b4cb7-325">Default: `00:02:00`</span></span><br><span data-ttu-id="b4cb7-326">最小値: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="b4cb7-326">Min: `00:00:00`</span></span><br><span data-ttu-id="b4cb7-327">最大値: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="b4cb7-327">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="b4cb7-328">省略可能な整数属性</span><span class="sxs-lookup"><span data-stu-id="b4cb7-328">Optional integer attribute.</span></span></p><p><span data-ttu-id="b4cb7-329">*app_offline.htm* ファイルが検出されたときに、モジュールが実行可能ファイルの正常なシャットダウンを待機する秒単位の期間です。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-329">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="b4cb7-330">既定値: `10`</span><span class="sxs-lookup"><span data-stu-id="b4cb7-330">Default: `10`</span></span><br><span data-ttu-id="b4cb7-331">最小値: `0`</span><span class="sxs-lookup"><span data-stu-id="b4cb7-331">Min: `0`</span></span><br><span data-ttu-id="b4cb7-332">最大値: `600`</span><span class="sxs-lookup"><span data-stu-id="b4cb7-332">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="b4cb7-333">省略可能な整数属性</span><span class="sxs-lookup"><span data-stu-id="b4cb7-333">Optional integer attribute.</span></span></p><p><span data-ttu-id="b4cb7-334">ポートでリッスンするプロセスを実行可能ファイルが開始するのをモジュールが待機する秒単位の期間です。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-334">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="b4cb7-335">この制限時間を超えた場合、モジュールはプロセスを強制終了します。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-335">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="b4cb7-336">最後のローリング分においてアプリが **rapidFailsPerMinute** 回だけ開始を失敗しているのでないかぎり、モジュールは、新しい要求を受け取るとプロセスの再起動を試み、それ以降に受信した要求に対してプロセスの再起動を試み続けます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-336">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p> | <span data-ttu-id="b4cb7-337">既定値: `120`</span><span class="sxs-lookup"><span data-stu-id="b4cb7-337">Default: `120`</span></span><br><span data-ttu-id="b4cb7-338">最小値: `0`</span><span class="sxs-lookup"><span data-stu-id="b4cb7-338">Min: `0`</span></span><br><span data-ttu-id="b4cb7-339">最大値: `3600`</span><span class="sxs-lookup"><span data-stu-id="b4cb7-339">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="b4cb7-340">省略可能な Boolean 属性です。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-340">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="b4cb7-341">true の場合、**processPath** で指定されているプロセスの **stdout** と **stderr** は、**stdoutLogFile** で指定されているファイルにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-341">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="b4cb7-342">省略可能な文字列属性。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-342">Optional string attribute.</span></span></p><p><span data-ttu-id="b4cb7-343">**processPath** で指定されているプロセスからの **stdout** と **stderr** がログに記録される相対ファイル パスまたは絶対ファイル パスを指定します。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-343">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="b4cb7-344">相対パスの基準はサイトのルートです。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-344">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="b4cb7-345">`.` で始まっているパスはすべてサイト ルートに対する相対パスであり、他のすべてのパスは絶対パスとして扱われます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-345">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="b4cb7-346">モジュールがログ ファイルを作成するためには、パスで指定されているすべてのフォルダーが存在する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-346">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="b4cb7-347">アンダースコアの区切り記号を使って、タイムスタンプ、プロセス ID、およびファイル拡張子 (*.log*) が、**stdoutLogFile** パスの最後のセグメントに追加されます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-347">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="b4cb7-348">たとえば、値として `.\logs\stdout` を指定し、2018 年 2 月 5 日の 19:41:32 にプロセス ID 1934 で保存すると、stdout ログは *logs* フォルダーに *stdout_20180205194132_1934.log* として保存されます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-348">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

### <a name="setting-environment-variables"></a><span data-ttu-id="b4cb7-349">環境変数の設定</span><span class="sxs-lookup"><span data-stu-id="b4cb7-349">Setting environment variables</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="b4cb7-350">`processPath` 属性で、プロセスに対する環境変数を指定できます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-350">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="b4cb7-351">`<environmentVariables>` コレクション要素の `<environmentVariable>` 子要素で、環境変数を指定します。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-351">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span> <span data-ttu-id="b4cb7-352">このセクションで設定された環境変数は、システム環境変数より優先されます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-352">Environment variables set in this section take precedence over system environment variables.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="b4cb7-353">`processPath` 属性で、プロセスに対する環境変数を指定できます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-353">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="b4cb7-354">`<environmentVariables>` コレクション要素の `<environmentVariable>` 子要素で、環境変数を指定します。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-354">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span>

> [!WARNING]
> <span data-ttu-id="b4cb7-355">このセクションで設定した環境変数は、同じ名前を使って設定したシステム環境変数と競合します。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-355">Environment variables set in this section conflict with system environment variables set with the same name.</span></span> <span data-ttu-id="b4cb7-356">*web.config* ファイルと Windows のシステム レベルの両方で 1 つの環境変数が設定されている場合、*web.config* ファイルからの値がシステム環境変数の値に追加されるため (例: `ASPNETCORE_ENVIRONMENT: Development;Development`)、アプリが起動できなくなります。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-356">If an environment variable is set in both the *web.config* file and at the system level in Windows, the value from the *web.config* file becomes appended to the system environment variable value (for example, `ASPNETCORE_ENVIRONMENT: Development;Development`), which prevents the app from starting.</span></span>

::: moniker-end

<span data-ttu-id="b4cb7-357">次の例では、2 つの環境変数を設定しています。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-357">The following example sets two environment variables.</span></span> <span data-ttu-id="b4cb7-358">`ASPNETCORE_ENVIRONMENT` は、`Development` に対するアプリの環境を構成します。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-358">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="b4cb7-359">開発者は、アプリの例外をデバッグするときに[開発者例外ページ](xref:fundamentals/error-handling)を強制的に読み込むため、*web.config* ファイルでこの値を一時的に設定できます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-359">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="b4cb7-360">`CONFIG_DIR` はユーザー定義の環境変数の例です。開発者はここに、アプリの構成ファイルを読み込むためのパスを形成するために起動時に値を読み取るコードを記述してあります。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-360">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout"
      hostingModel="InProcess">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="b4cb7-361">*web.config* 内で環境を直接設定する代わりに、発行プロファイル (*.pubxml*) またはプロジェクト ファイルに `<EnvironmentName>` プロパティを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-361">An alternative to setting the environment directly in *web.config* is to include the `<EnvironmentName>` property in the publish profile (*.pubxml*) or project file.</span></span> <span data-ttu-id="b4cb7-362">この方法では、プロジェクトが発行されるときに *web.config* に環境が設定されます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-362">This approach sets the environment in *web.config* when the project is published:</span></span>
>
> ```xml
> <PropertyGroup>
>   <EnvironmentName>Development</EnvironmentName>
> </PropertyGroup>
> ```

::: moniker-end

> [!WARNING]
> <span data-ttu-id="b4cb7-363">インターネットなどの信頼されていないネットワークにアクセスできないステージング サーバーやテスト サーバーでは、`ASPNETCORE_ENVIRONMENT` 環境変数を `Development` に設定するだけです。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-363">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="appofflinehtm"></a><span data-ttu-id="b4cb7-364">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="b4cb7-364">app_offline.htm</span></span>

<span data-ttu-id="b4cb7-365">*app_offline.htm* という名前のファイルがアプリのルート ディレクトリで検出された場合、ASP.NET Core モジュールはアプリを正常にシャットダウンし、受信要求の処理を停止することを試みます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-365">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="b4cb7-366">`shutdownTimeLimit` で定義されている秒数が経過してもまだアプリが実行している場合、ASP.NET Core モジュールは実行中のプロセスを強制終了します。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-366">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="b4cb7-367">*app_offline.htm* ファイルが存在している間、ASP.NET Core モジュールは、*app_offline.htm* ファイルの内容を返送することで、要求に応答します。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-367">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="b4cb7-368">*app_offline.htm* ファイルが削除されると、次の要求によってアプリが起動されます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-368">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="b4cb7-369">アウト プロセス ホスティング モデルを使用するときは、開いている接続があると、アプリがすぐにシャットダウンされない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-369">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="b4cb7-370">たとえば、WebSocket 接続では、アプリのシャットダウンが遅れる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-370">For example, a websocket connection may delay app shut down.</span></span>

::: moniker-end

## <a name="start-up-error-page"></a><span data-ttu-id="b4cb7-371">起動エラー ページ</span><span class="sxs-lookup"><span data-stu-id="b4cb7-371">Start-up error page</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="b4cb7-372">インプロセス ホスティングでもアウト プロセス ホスティングでも、アプリの起動に失敗すると、カスタム エラー ページが生成されます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-372">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="b4cb7-373">ASP.NET Core モジュールが、インプロセスまたはアウト プロセスのどちらかの要求ハンドラーの検索に失敗した場合、*500.0 - インプロセス/アウト プロセス ハンドラーの読み込みエラー*状態コード ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-373">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="b4cb7-374">インプロセス ホスティングで、ASP.NET Core モジュールによるアプリの起動が失敗すると、*500.30 - 開始エラー*状態コード ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-374">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="b4cb7-375">アウト プロセス ホスティングで、ASP.NET Core モジュールがバックエンド プロセスの起動に失敗した場合、またはバックエンド プロセスは開始しても構成されているポートでのリッスンに失敗した場合は、*502.5 処理エラー*状態コード ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-375">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="b4cb7-376">このページを抑制して、既定の IIS 5xx 状態コード ページに戻すには、`disableStartUpErrorPage` 属性を使います。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-376">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="b4cb7-377">カスタム エラー メッセージの構成方法について詳しくは、「[HTTP エラー \<httpErrors](/iis/configuration/system.webServer/httpErrors/)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-377">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="b4cb7-378">ASP.NET Core モジュールが、バックエンド プロセスの起動に失敗した場合、またはバックエンド プロセスは開始しても構成されているポートでのリッスンに失敗した場合は、*502.5 処理エラー*状態コード ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-378">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span> <span data-ttu-id="b4cb7-379">このページを抑制して、既定の IIS 502 状態コード ページに戻すには、`disableStartUpErrorPage` 属性を使います。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-379">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="b4cb7-380">カスタム エラー メッセージの構成方法について詳しくは、「[HTTP エラー \<httpErrors](/iis/configuration/system.webServer/httpErrors/)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-380">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

![502.5 処理エラーの状態コード ページ](aspnet-core-module/_static/ANCM-502_5.png)

::: moniker-end

## <a name="log-creation-and-redirection"></a><span data-ttu-id="b4cb7-382">ログの作成とリダイレクト</span><span class="sxs-lookup"><span data-stu-id="b4cb7-382">Log creation and redirection</span></span>

<span data-ttu-id="b4cb7-383">`aspNetCore` 要素の `stdoutLogEnabled` 属性および `stdoutLogFile` 属性が設定されている場合は、stdout および stderr コンソール出力が ASP.NET Core モジュールによってディスクにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-383">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="b4cb7-384">`stdoutLogFile` パスのフォルダーは、ログ ファイルの作成時、モジュールによって作成されます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-384">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="b4cb7-385">アプリ プールは、ログが書き込まれる場所への書き込みアクセス権を持っている必要があります (書き込みアクセス許可を提供するには、`IIS AppPool\<app_pool_name>` を使います)。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-385">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="b4cb7-386">プロセスのリサイクル/再起動が発生しない場合、ログは循環されません。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-386">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="b4cb7-387">ログが使用するディスク領域を制限するのは、ホストの役割です。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-387">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="b4cb7-388">stdout ログの使用は、アプリ起動時の問題をトラブルシューティングする場合にのみ推奨されます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-388">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="b4cb7-389">一般的なアプリ ログの目的には、stdout ログを使わないでください。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-389">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="b4cb7-390">ASP.NET Core アプリでのルーチン ログの場合は、ログ ファイルのサイズを制限し、ログをローテーションするログ ライブラリを使います。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-390">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="b4cb7-391">詳しくは、「[サードパーティ製のログ プロバイダー](xref:fundamentals/logging/index#third-party-logging-providers)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-391">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="b4cb7-392">ログ ファイルの作成時には、タイムスタンプとファイルの拡張子が自動的に追加されます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-392">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="b4cb7-393">ログ ファイル名は、タイムスタンプ、プロセス ID、およびファイル拡張子 (*.log*) を `stdoutLogFile` パスの最後のセグメント (通常は *stdout*) にアンダースコアで区切って追加することで構成されます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-393">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="b4cb7-394">`stdoutLogFile` パスが *stdout* で終わっている場合、PID が 1934 で 2018 年 2 月 5 日の 19:42:32 に作成されたアプリのログのファイル名は、*stdout_20180205194132_1934.log* になります。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-394">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="b4cb7-395">`stdoutLogEnabled` が false の場合は、アプリの起動時に発生するエラーがキャプチャされ、30 KB までイベント ログに出力されます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-395">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="b4cb7-396">起動後、すべての追加のログが破棄されます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-396">After startup, all additional logs are discarded.</span></span>

::: moniker-end

<span data-ttu-id="b4cb7-397">次の `aspNetCore` 要素の例では、Azure App Service でホストされているアプリの stdout ログを構成しています。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-397">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="b4cb7-398">ローカル ログには、ローカル パスまたはネットワーク共有パスを使用できます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-398">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="b4cb7-399">AppPool のユーザー ID に、指定されたパスへの書き込みアクセス許可があることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-399">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
</aspNetCore>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="b4cb7-400">強化された診断ログ</span><span class="sxs-lookup"><span data-stu-id="b4cb7-400">Enhanced diagnostic logs</span></span>

<span data-ttu-id="b4cb7-401">ASP.NET Core モジュールは、強化された診断ログを提供するよう構成できます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-401">The ASP.NET Core Module is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="b4cb7-402">*web.config* で、`<aspNetCore>` 要素に `<handlerSettings>` 要素を追加します。`debugLevel` を `TRACE` に設定すると、診断情報が再現性の高いものになります。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-402">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
  <handlerSettings>
    <handlerSetting name="debugFile" value="aspnetcore-debug.log" />
    <handlerSetting name="debugLevel" value="FILE,TRACE" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="b4cb7-403">デバッグ レベル (`debugLevel`) 値には、レベルと場所の両方を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-403">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="b4cb7-404">レベルは次のとおりです (情報量が少ないものから多いものへの順)。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-404">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="b4cb7-405">ERROR</span><span class="sxs-lookup"><span data-stu-id="b4cb7-405">ERROR</span></span>
* <span data-ttu-id="b4cb7-406">WARNING</span><span class="sxs-lookup"><span data-stu-id="b4cb7-406">WARNING</span></span>
* <span data-ttu-id="b4cb7-407">INFO</span><span class="sxs-lookup"><span data-stu-id="b4cb7-407">INFO</span></span>
* <span data-ttu-id="b4cb7-408">TRACE</span><span class="sxs-lookup"><span data-stu-id="b4cb7-408">TRACE</span></span>

<span data-ttu-id="b4cb7-409">場所は次のとおりです (複数の場所を指定できます)。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-409">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="b4cb7-410">CONSOLE</span><span class="sxs-lookup"><span data-stu-id="b4cb7-410">CONSOLE</span></span>
* <span data-ttu-id="b4cb7-411">EVENTLOG</span><span class="sxs-lookup"><span data-stu-id="b4cb7-411">EVENTLOG</span></span>
* <span data-ttu-id="b4cb7-412">ファイル</span><span class="sxs-lookup"><span data-stu-id="b4cb7-412">FILE</span></span>

<span data-ttu-id="b4cb7-413">ハンドラー設定は、次の環境変数を使用して指定することもできます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-413">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="b4cb7-414">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; デバッグ ログ ファイルへのパス </span><span class="sxs-lookup"><span data-stu-id="b4cb7-414">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="b4cb7-415">(既定: *aspnetcore debug.log*)。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-415">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="b4cb7-416">`ASPNETCORE_MODULE_DEBUG` &ndash; デバッグ レベルの設定。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-416">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="b4cb7-417">配置内でデバッグ ログを、問題のトラブルシューティングに必要な時間よりも長く有効のままに**しないでください**。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-417">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="b4cb7-418">ログのサイズは制限されていません。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-418">The size of the log isn't limited.</span></span> <span data-ttu-id="b4cb7-419">デバッグ ログを有効のままにすると、使用可能なディスク領域が使い果たされ、サーバーまたはアプリ サービスがクラッシュする可能性があります。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-419">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

::: moniker-end

<span data-ttu-id="b4cb7-420">*web.config* ファイルでの `aspNetCore` 要素の例については、「[web.config での構成](#configuration-with-webconfig)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-420">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="b4cb7-421">プロキシの構成で HTTP プロトコルとペアリング トークンを使用する</span><span class="sxs-lookup"><span data-stu-id="b4cb7-421">Proxy configuration uses HTTP protocol and a pairing token</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="b4cb7-422">*アウト プロセス ホスティングにのみ適用されます。*</span><span class="sxs-lookup"><span data-stu-id="b4cb7-422">*Only applies to out-of-process hosting.*</span></span>

::: moniker-end

<span data-ttu-id="b4cb7-423">ASP.NET Core モジュールと Kestrel の間に作成されるプロキシは、HTTP プロトコルを使います。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-423">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="b4cb7-424">HTTP を使うと、パフォーマンスが最適化されます。この場合、モジュールと Kestrel の間のトラフィックは、ネットワーク インターフェイスから切り離されたループバック アドレスで発生します。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-424">Using HTTP is a performance optimization, where the traffic between the module and Kestrel takes place on a loopback address off of the network interface.</span></span> <span data-ttu-id="b4cb7-425">モジュールと Kestrel の間のトラフィックが、サーバーから離れた場所で傍受される危険はありません。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-425">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="b4cb7-426">ペアリング トークンを使用すると、Kestrel によって受信される要求が IIS によってプロキシされたものであり、他のソースからのものでないことを保証できます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-426">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="b4cb7-427">ペアリング トークンは、モジュールによって作成されて、環境変数 (`ASPNETCORE_TOKEN`) に設定されます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-427">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="b4cb7-428">ペアリング トークンはまた、プロキシされたすべての要求のヘッダー (`MS-ASPNETCORE-TOKEN`) にも設定されます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-428">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="b4cb7-429">IIS ミドルウェアは、受信した各要求をチェックし、ペアリング トークン ヘッダーの値が環境変数の値と一致することを確認します。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-429">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="b4cb7-430">トークンの値が一致しない場合、要求はログに記録され、拒否されます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-430">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="b4cb7-431">ペアリング トークン環境変数およびモジュールと Kestrel の間のトラフィックには、サーバーから離れた場所からアクセスすることはできません。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-431">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="b4cb7-432">ペアリング トークンの値がわからなければ、攻撃者は IIS ミドルウェアのチェックをバイパスする要求を送信できません。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-432">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="b4cb7-433">IIS 共有構成での ASP.NET Core モジュール</span><span class="sxs-lookup"><span data-stu-id="b4cb7-433">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="b4cb7-434">ASP.NET Core モジュールのインストーラーは、**SYSTEM** アカウントのアクセス許可を使って実行します。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-434">The ASP.NET Core Module installer runs with the privileges of the **SYSTEM** account.</span></span> <span data-ttu-id="b4cb7-435">ローカル システム アカウントには、IIS 共有構成によって使われる共有パスに対する変更アクセス許可がないため、インストーラーが共有上の *applicationHost.config* 内のモジュール設定を構成しようとすると、アクセス拒否エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-435">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer hits an access denied error when attempting to configure the module settings in *applicationHost.config* on the share.</span></span> <span data-ttu-id="b4cb7-436">IIS 共有構成を使うときは、次の手順で行います。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-436">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="b4cb7-437">IIS 共有構成を無効にします。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-437">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="b4cb7-438">インストーラーを実行します。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-438">Run the installer.</span></span>
1. <span data-ttu-id="b4cb7-439">更新された *applicationHost.config* ファイルを共有にエクスポートします。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-439">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="b4cb7-440">IIS 共有構成を再び有効にします。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-440">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="b4cb7-441">モジュールのバージョンとホスティング バンドル インストーラーのログ</span><span class="sxs-lookup"><span data-stu-id="b4cb7-441">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="b4cb7-442">インストールされている ASP.NET Core モジュールのバージョンを確認するには:</span><span class="sxs-lookup"><span data-stu-id="b4cb7-442">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="b4cb7-443">ホスティング システム上で、*%windir%\System32\inetsrv* に移動します。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-443">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="b4cb7-444">*aspnetcore.dll* ファイルを探します。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-444">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="b4cb7-445">ファイルを右クリックし、コンテキスト メニューの **[プロパティ]** を選びます。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-445">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="b4cb7-446">**[詳細]** タブを選びます。**[ファイル バージョン]** と **[製品バージョン]** が、インストールされているモジュールのバージョンを表します。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-446">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="b4cb7-447">モジュールのホスティング バンドル インストーラーのログは、*C:\\Users\\%UserName%\\AppData\\Local\\Temp* にあります。ファイルの名前は、*dd_DotNetCoreWinSvrHosting__\<タイムスタンプ>_000_AspNetCoreModule_x64.log* です。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-447">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="b4cb7-448">モジュール、スキーマ、構成ファイルの場所</span><span class="sxs-lookup"><span data-stu-id="b4cb7-448">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="b4cb7-449">Module</span><span class="sxs-lookup"><span data-stu-id="b4cb7-449">Module</span></span>

<span data-ttu-id="b4cb7-450">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="b4cb7-450">**IIS (x86/amd64):**</span></span>

   * <span data-ttu-id="b4cb7-451">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="b4cb7-451">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

   * <span data-ttu-id="b4cb7-452">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="b4cb7-452">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="b4cb7-453">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="b4cb7-453">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="b4cb7-454">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="b4cb7-454">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

<span data-ttu-id="b4cb7-455">**IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="b4cb7-455">**IIS Express (x86/amd64):**</span></span>

   * <span data-ttu-id="b4cb7-456">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="b4cb7-456">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

   * <span data-ttu-id="b4cb7-457">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="b4cb7-457">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="b4cb7-458">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="b4cb7-458">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="b4cb7-459">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="b4cb7-459">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

### <a name="schema"></a><span data-ttu-id="b4cb7-460">Schema</span><span class="sxs-lookup"><span data-stu-id="b4cb7-460">Schema</span></span>

<span data-ttu-id="b4cb7-461">**IIS**</span><span class="sxs-lookup"><span data-stu-id="b4cb7-461">**IIS**</span></span>

   * <span data-ttu-id="b4cb7-462">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="b4cb7-462">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="b4cb7-463">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="b4cb7-463">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end
<span data-ttu-id="b4cb7-464">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="b4cb7-464">**IIS Express**</span></span>

   * <span data-ttu-id="b4cb7-465">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="b4cb7-465">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="b4cb7-466">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="b4cb7-466">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="b4cb7-467">構成</span><span class="sxs-lookup"><span data-stu-id="b4cb7-467">Configuration</span></span>

<span data-ttu-id="b4cb7-468">**IIS**</span><span class="sxs-lookup"><span data-stu-id="b4cb7-468">**IIS**</span></span>

   * <span data-ttu-id="b4cb7-469">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="b4cb7-469">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="b4cb7-470">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="b4cb7-470">**IIS Express**</span></span>

   * <span data-ttu-id="b4cb7-471">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="b4cb7-471">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>
   
   * <span data-ttu-id="b4cb7-472">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="b4cb7-472">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="b4cb7-473">ファイルは、*applicationHost.config* ファイルで *aspnetcore* を検索することにより見つかります。</span><span class="sxs-lookup"><span data-stu-id="b4cb7-473">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b4cb7-474">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="b4cb7-474">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* [<span data-ttu-id="b4cb7-475">ASP.NET Core モジュールの GitHub リポジトリ (参照元)</span><span class="sxs-lookup"><span data-stu-id="b4cb7-475">ASP.NET Core Module GitHub repository (reference source)</span></span>](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>
