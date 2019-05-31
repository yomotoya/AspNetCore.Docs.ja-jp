---
title: IIS での ASP.NET Core のトラブルシューティング
author: guardrex
description: ASP.NET Core アプリのインターネット インフォメーション サービス (IIS) の展開に関する問題を診断する方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/12/2019
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: 80994cb84e9e0658ee90198b6bf992e5b374bf3c
ms.sourcegitcommit: b4ef2b00f3e1eb287138f8b43c811cb35a100d3e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/21/2019
ms.locfileid: "65970027"
---
# <a name="troubleshoot-aspnet-core-on-iis"></a><span data-ttu-id="e92a0-103">IIS での ASP.NET Core のトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="e92a0-103">Troubleshoot ASP.NET Core on IIS</span></span>

<span data-ttu-id="e92a0-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e92a0-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="e92a0-105">この記事では、[インターネット インフォメーション サービス (IIS)](/iis) を使用してホストしている場合に、ASP.NET Core アプリの起動時に関する問題を診断する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="e92a0-105">This article provides instructions on how to diagnose an ASP.NET Core app startup issue when hosting with [Internet Information Services (IIS)](/iis).</span></span> <span data-ttu-id="e92a0-106">この記事の情報は、Windows Server および Windows デスクトップ上の IIS でのホスティングに適用されます。</span><span class="sxs-lookup"><span data-stu-id="e92a0-106">The information in this article applies to hosting in IIS on Windows Server and Windows Desktop.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="e92a0-107">Visual Studio では、ASP.NET Core プロジェクトのデバッグ時に [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) のホスティングが既定の設定です。</span><span class="sxs-lookup"><span data-stu-id="e92a0-107">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="e92a0-108">このトピックのアドバイスを参照すると、ローカルでのデバッグ時に発生する *502.5 処理エラー*または *500.30 - 開始エラー*を解決できます。</span><span class="sxs-lookup"><span data-stu-id="e92a0-108">A *502.5 - Process Failure* or a *500.30 - Start Failure* that occurs when debugging locally can be troubleshooted using the advice in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="e92a0-109">Visual Studio では、ASP.NET Core プロジェクトのデバッグ時に [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) のホスティングが既定の設定です。</span><span class="sxs-lookup"><span data-stu-id="e92a0-109">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="e92a0-110">このトピックのアドバイスを参照すると、ローカルでのデバッグ時に発生する *502.5 処理エラー*を解決できます。</span><span class="sxs-lookup"><span data-stu-id="e92a0-110">A *502.5 Process Failure* that occurs when debugging locally can be troubleshooted using the advice in this topic.</span></span>

::: moniker-end

<span data-ttu-id="e92a0-111">その他のトラブルシューティング トピック:</span><span class="sxs-lookup"><span data-stu-id="e92a0-111">Additional troubleshooting topics:</span></span>

<span data-ttu-id="e92a0-112"><xref:host-and-deploy/azure-apps/troubleshoot>App Service は [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) と IIS を使用してアプリをホストしますが、App Service 固有の手順については、この専用のトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="e92a0-112"><xref:host-and-deploy/azure-apps/troubleshoot> Although App Service uses the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) and IIS to host apps, see the dedicated topic for instructions specific to App Service.</span></span>

<span data-ttu-id="e92a0-113"><xref:fundamentals/error-handling> ローカル システム上で開発しているときに発生する ASP.NET Core アプリのエラーを処理する方法について説明しています。</span><span class="sxs-lookup"><span data-stu-id="e92a0-113"><xref:fundamentals/error-handling> Discover how to handle errors in ASP.NET Core apps during development on a local system.</span></span>

<span data-ttu-id="e92a0-114">[Visual Studio を使用したデバッグについて理解する](/visualstudio/debugger/getting-started-with-the-debugger) このトピックでは、Visual Studio デバッガーの機能を紹介しています。</span><span class="sxs-lookup"><span data-stu-id="e92a0-114">[Learn to debug using Visual Studio](/visualstudio/debugger/getting-started-with-the-debugger) This topic introduces the features of the Visual Studio debugger.</span></span>

<span data-ttu-id="e92a0-115">[Visual Studio Code でのデバッグ](https://code.visualstudio.com/docs/editor/debugging) Visual Studio Code に組み込まれているデバッグのサポートについて説明します。</span><span class="sxs-lookup"><span data-stu-id="e92a0-115">[Debugging with Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging) Learn about the debugging support built into Visual Studio Code.</span></span>

## <a name="app-startup-errors"></a><span data-ttu-id="e92a0-116">アプリ起動時のエラー</span><span class="sxs-lookup"><span data-stu-id="e92a0-116">App startup errors</span></span>

### <a name="5025-process-failure"></a><span data-ttu-id="e92a0-117">502.5 処理エラー</span><span class="sxs-lookup"><span data-stu-id="e92a0-117">502.5 Process Failure</span></span>

<span data-ttu-id="e92a0-118">ワーカー プロセスが失敗します。</span><span class="sxs-lookup"><span data-stu-id="e92a0-118">The worker process fails.</span></span> <span data-ttu-id="e92a0-119">アプリは起動しません。</span><span class="sxs-lookup"><span data-stu-id="e92a0-119">The app doesn't start.</span></span>

<span data-ttu-id="e92a0-120">ASP.NET Core モジュールはバックエンドのドットネット プロセスの開始を試みますが、開始に失敗します。</span><span class="sxs-lookup"><span data-stu-id="e92a0-120">The ASP.NET Core Module attempts to start the backend dotnet process but it fails to start.</span></span> <span data-ttu-id="e92a0-121">プロセス起動時のエラーの原因は、通常、[アプリケーション イベント ログ](#application-event-log)と [ASP.NET Core モジュールの stdout ログ](#aspnet-core-module-stdout-log)のエントリから判断できます。</span><span class="sxs-lookup"><span data-stu-id="e92a0-121">The cause of a process startup failure can usually be determined from entries in the [Application Event Log](#application-event-log) and the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log).</span></span>

<span data-ttu-id="e92a0-122">一般的なエラー条件は、存在しないバージョンの ASP.NET Core 共有フレームワークが対象にされていて、アプリが正しく構成されていないことです。</span><span class="sxs-lookup"><span data-stu-id="e92a0-122">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="e92a0-123">対象のコンピューターにどのバージョンの ASP.NET Core 共有フレームワークがインストールされているかを確認します。</span><span class="sxs-lookup"><span data-stu-id="e92a0-123">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span> <span data-ttu-id="e92a0-124">*共有フレームワーク*は、コンピューター上にインストールされたアセンブリ ( *.dll* ファイル) のセットであり、`Microsoft.AspNetCore.App` などのメタパッケージによって参照されます。</span><span class="sxs-lookup"><span data-stu-id="e92a0-124">The *shared framework* is the set of assemblies (*.dll* files) that are installed on the machine and referenced by a metapackage such as `Microsoft.AspNetCore.App`.</span></span> <span data-ttu-id="e92a0-125">メタパッケージの参照には、必要な最低限のバージョンを指定できます。</span><span class="sxs-lookup"><span data-stu-id="e92a0-125">The metapackage reference can specify a minimum required version.</span></span> <span data-ttu-id="e92a0-126">詳しくは、[共有フレームワーク](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/)に関するページをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="e92a0-126">For more information, see [The shared framework](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span></span>

<span data-ttu-id="e92a0-127">正しく構成されていないホスティングやアプリが原因でワーカー プロセスが失敗する場合、"*502.5 処理エラー*" のエラー ページが返されます。</span><span class="sxs-lookup"><span data-stu-id="e92a0-127">The *502.5 Process Failure* error page is returned when a hosting or app misconfiguration causes the worker process to fail:</span></span>

![502.5 処理エラー ページが表示されているブラウザー ウィンドウ](troubleshoot/_static/process-failure-page.png)

::: moniker range=">= aspnetcore-2.2"

### <a name="50030-in-process-startup-failure"></a><span data-ttu-id="e92a0-129">500.30 インプロセス起動エラー</span><span class="sxs-lookup"><span data-stu-id="e92a0-129">500.30 In-Process Startup Failure</span></span>

<span data-ttu-id="e92a0-130">ワーカー プロセスが失敗します。</span><span class="sxs-lookup"><span data-stu-id="e92a0-130">The worker process fails.</span></span> <span data-ttu-id="e92a0-131">アプリは起動しません。</span><span class="sxs-lookup"><span data-stu-id="e92a0-131">The app doesn't start.</span></span>

<span data-ttu-id="e92a0-132">ASP.NET Core モジュールは .NET Core CLR の開始をインプロセスで試みますが、開始に失敗します。</span><span class="sxs-lookup"><span data-stu-id="e92a0-132">The ASP.NET Core Module attempts to start the .NET Core CLR in-process, but it fails to start.</span></span> <span data-ttu-id="e92a0-133">プロセス起動時のエラーの原因は、通常、[アプリケーション イベント ログ](#application-event-log)と [ASP.NET Core モジュールの stdout ログ](#aspnet-core-module-stdout-log)のエントリから判断できます。</span><span class="sxs-lookup"><span data-stu-id="e92a0-133">The cause of a process startup failure can usually be determined from entries in the [Application Event Log](#application-event-log) and the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log).</span></span>

<span data-ttu-id="e92a0-134">一般的なエラー条件は、存在しないバージョンの ASP.NET Core 共有フレームワークが対象にされていて、アプリが正しく構成されていないことです。</span><span class="sxs-lookup"><span data-stu-id="e92a0-134">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="e92a0-135">対象のコンピューターにどのバージョンの ASP.NET Core 共有フレームワークがインストールされているかを確認します。</span><span class="sxs-lookup"><span data-stu-id="e92a0-135">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span>

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="e92a0-136">500.0 インプロセス ハンドラーの読み込みエラー</span><span class="sxs-lookup"><span data-stu-id="e92a0-136">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="e92a0-137">ワーカー プロセスが失敗します。</span><span class="sxs-lookup"><span data-stu-id="e92a0-137">The worker process fails.</span></span> <span data-ttu-id="e92a0-138">アプリは起動しません。</span><span class="sxs-lookup"><span data-stu-id="e92a0-138">The app doesn't start.</span></span>

<span data-ttu-id="e92a0-139">ASP.NET Core モジュールは .NET Core CLR の検出に失敗し、インプロセス要求ハンドラー (*aspnetcorev2_inprocess.dll*) を検出します。</span><span class="sxs-lookup"><span data-stu-id="e92a0-139">The ASP.NET Core Module fails to find the .NET Core CLR and find the in-process request handler (*aspnetcorev2_inprocess.dll*).</span></span> <span data-ttu-id="e92a0-140">次の点をご確認ください。</span><span class="sxs-lookup"><span data-stu-id="e92a0-140">Check that:</span></span>

* <span data-ttu-id="e92a0-141">アプリが [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) NuGet パッケージまたは [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)を対象としている。</span><span class="sxs-lookup"><span data-stu-id="e92a0-141">The app targets either the [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) NuGet package or the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
* <span data-ttu-id="e92a0-142">アプリが対象としているバージョンの ASP.NET Core 共有フレームワークが対象のコンピューターにインストールされている。</span><span class="sxs-lookup"><span data-stu-id="e92a0-142">The version of the ASP.NET Core shared framework that the app targets is installed on the target machine.</span></span>

### <a name="5000-out-of-process-handler-load-failure"></a><span data-ttu-id="e92a0-143">500.0 アウト プロセス ハンドラーの読み込みエラー</span><span class="sxs-lookup"><span data-stu-id="e92a0-143">500.0 Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="e92a0-144">ワーカー プロセスが失敗します。</span><span class="sxs-lookup"><span data-stu-id="e92a0-144">The worker process fails.</span></span> <span data-ttu-id="e92a0-145">アプリは起動しません。</span><span class="sxs-lookup"><span data-stu-id="e92a0-145">The app doesn't start.</span></span>

<span data-ttu-id="e92a0-146">ASP.NET Core モジュールは、アウト プロセスのホスティング要求ハンドラーの検索に失敗します。</span><span class="sxs-lookup"><span data-stu-id="e92a0-146">The ASP.NET Core Module fails to find the out-of-process hosting request handler.</span></span> <span data-ttu-id="e92a0-147">*aspnetcorev2_outofprocess.dll* が *aspnetcorev2.dll* の隣のサブフォルダーにあることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="e92a0-147">Make sure the *aspnetcorev2_outofprocess.dll* is present in a subfolder next to *aspnetcorev2.dll*.</span></span>

::: moniker-end

### <a name="500-internal-server-error"></a><span data-ttu-id="e92a0-148">500 内部サーバー エラー</span><span class="sxs-lookup"><span data-stu-id="e92a0-148">500 Internal Server Error</span></span>

<span data-ttu-id="e92a0-149">アプリは起動しますが、エラーのためにサーバーは要求を実行できません。</span><span class="sxs-lookup"><span data-stu-id="e92a0-149">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="e92a0-150">このエラーは、起動時または応答の作成中に、アプリのコード内で発生します。</span><span class="sxs-lookup"><span data-stu-id="e92a0-150">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="e92a0-151">応答にコンテンツが含まれていないか、またはブラウザーに "*500 内部サーバー エラー*" という応答が表示される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="e92a0-151">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="e92a0-152">通常、アプリケーション イベント ログではアプリが正常に起動したことが示されます。</span><span class="sxs-lookup"><span data-stu-id="e92a0-152">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="e92a0-153">サーバーから見るとそれは正しいことです。</span><span class="sxs-lookup"><span data-stu-id="e92a0-153">From the server's perspective, that's correct.</span></span> <span data-ttu-id="e92a0-154">アプリは起動しましたが、有効な応答を生成できません。</span><span class="sxs-lookup"><span data-stu-id="e92a0-154">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="e92a0-155">問題を解決するには、[サーバー上のコマンド プロンプトでアプリを実行する](#run-the-app-at-a-command-prompt)か、[ASP.NET Core モジュールの stdout ログを有効にします](#aspnet-core-module-stdout-log)。</span><span class="sxs-lookup"><span data-stu-id="e92a0-155">[Run the app at a command prompt](#run-the-app-at-a-command-prompt) on the server or [enable the ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log) to troubleshoot the problem.</span></span>

### <a name="failed-to-start-application-errorcode-0x800700c1"></a><span data-ttu-id="e92a0-156">アプリケーションの起動に失敗しました (ErrorCode '0x800700c1')</span><span class="sxs-lookup"><span data-stu-id="e92a0-156">Failed to start application (ErrorCode '0x800700c1')</span></span>

```
EventID: 1010
Source: IIS AspNetCore Module V2
Failed to start application '/LM/W3SVC/6/ROOT/', ErrorCode '0x800700c1'.
```

<span data-ttu-id="e92a0-157">アプリのアセンブリ ( *.dll*) を読み込めなかったため、アプリの起動に失敗しました。</span><span class="sxs-lookup"><span data-stu-id="e92a0-157">The app failed to start because the app's assembly (*.dll*) couldn't be loaded.</span></span>

<span data-ttu-id="e92a0-158">このエラーは、公開されたアプリと w3wp/iisexpress プロセスの間でビットの不一致がある場合に発生します。</span><span class="sxs-lookup"><span data-stu-id="e92a0-158">This error occurs when there's a bitness mismatch between the published app and the w3wp/iisexpress process.</span></span>

<span data-ttu-id="e92a0-159">アプリ プールの 32 ビット設定が正しいことを確認してください。</span><span class="sxs-lookup"><span data-stu-id="e92a0-159">Confirm that the app pool's 32-bit setting is correct:</span></span>

1. <span data-ttu-id="e92a0-160">IIS マネージャーの **[アプリケーション プール]** でそのアプリ プールを選択します。</span><span class="sxs-lookup"><span data-stu-id="e92a0-160">Select the app pool in IIS Manager's **Application Pools**.</span></span>
1. <span data-ttu-id="e92a0-161">**[アクション]** パネルの **[アプリケーション プールの編集]** で **[詳細設定]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="e92a0-161">Select **Advanced Settings** under **Edit Application Pool** in the **Actions** panel.</span></span>
1. <span data-ttu-id="e92a0-162">**[32 ビット アプリケーションの有効化]** を設定します。</span><span class="sxs-lookup"><span data-stu-id="e92a0-162">Set **Enable 32-Bit Applications**:</span></span>
   * <span data-ttu-id="e92a0-163">32 ビット (x86) アプリを展開する場合は、この値を `True` に設定します。</span><span class="sxs-lookup"><span data-stu-id="e92a0-163">If deploying a 32-bit (x86) app, set the value to `True`.</span></span>
   * <span data-ttu-id="e92a0-164">64 ビット (x64) アプリを展開する場合は、この値を `False` に設定します。</span><span class="sxs-lookup"><span data-stu-id="e92a0-164">If deploying a 64-bit (x64) app, set the value to `False`.</span></span>

### <a name="connection-reset"></a><span data-ttu-id="e92a0-165">接続のリセット</span><span class="sxs-lookup"><span data-stu-id="e92a0-165">Connection reset</span></span>

<span data-ttu-id="e92a0-166">ヘッダー送信後にエラーが発生した場合、サーバーが **500 内部サーバー エラー**を送信するには遅すぎます。</span><span class="sxs-lookup"><span data-stu-id="e92a0-166">If an error occurs after the headers are sent, it's too late for the server to send a **500 Internal Server Error** when an error occurs.</span></span> <span data-ttu-id="e92a0-167">このような状況は、応答に対する複雑なオブジェクトのシリアル化中にエラーが起きたときによく発生します。</span><span class="sxs-lookup"><span data-stu-id="e92a0-167">This often happens when an error occurs during the serialization of complex objects for a response.</span></span> <span data-ttu-id="e92a0-168">この種のエラーは、クライアントでは "*接続リセット*" エラーとして表示されます。</span><span class="sxs-lookup"><span data-stu-id="e92a0-168">This type of error appears as a *connection reset* error on the client.</span></span> <span data-ttu-id="e92a0-169">この種のエラーのトラブルシューティングには、[アプリケーション ログ](xref:fundamentals/logging/index)が役に立つことがあります。</span><span class="sxs-lookup"><span data-stu-id="e92a0-169">[Application logging](xref:fundamentals/logging/index) can help troubleshoot these types of errors.</span></span>

## <a name="default-startup-limits"></a><span data-ttu-id="e92a0-170">既定の起動制限</span><span class="sxs-lookup"><span data-stu-id="e92a0-170">Default startup limits</span></span>

<span data-ttu-id="e92a0-171">ASP.NET Core モジュールの *startupTimeLimit* は、既定では 120 秒に構成されます。</span><span class="sxs-lookup"><span data-stu-id="e92a0-171">The ASP.NET Core Module is configured with a default *startupTimeLimit* of 120 seconds.</span></span> <span data-ttu-id="e92a0-172">既定値のままにした場合、モジュールで処理エラーが記録されるまでに、アプリは最大で 2 分を起動にかけることができます。</span><span class="sxs-lookup"><span data-stu-id="e92a0-172">When left at the default value, an app may take up to two minutes to start before the module logs a process failure.</span></span> <span data-ttu-id="e92a0-173">モジュールの構成の詳細については、「[AspNetCore 要素の属性](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e92a0-173">For information on configuring the module, see [Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

## <a name="troubleshoot-app-startup-errors"></a><span data-ttu-id="e92a0-174">アプリの起動エラーのトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="e92a0-174">Troubleshoot app startup errors</span></span>

### <a name="enable-the-aspnet-core-module-debug-log"></a><span data-ttu-id="e92a0-175">ASP.NET Core モジュールのデバッグ ログを有効にする</span><span class="sxs-lookup"><span data-stu-id="e92a0-175">Enable the ASP.NET Core Module debug log</span></span>

<span data-ttu-id="e92a0-176">ASP.NET Core モジュールのデバッグ ログを有効にするには、次のハンドラー設定をアプリの *web.config* ファイルに追加します。</span><span class="sxs-lookup"><span data-stu-id="e92a0-176">Add the following handler settings to the app's *web.config* file to enable ASP.NET Core Module debug logs:</span></span>

```xml
<aspNetCore ...>
  <handlerSettings>
    <handlerSetting name="debugLevel" value="file" />
    <handlerSetting name="debugFile" value="c:\temp\ancm.log" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="e92a0-177">ログに指定されたパスが存在することと、アプリ プールの ID にその場所への書き込みアクセス許可があることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e92a0-177">Confirm that the path specified for the log exists and that the app pool's identity has write permissions to the location.</span></span>

### <a name="application-event-log"></a><span data-ttu-id="e92a0-178">アプリケーション イベント ログ</span><span class="sxs-lookup"><span data-stu-id="e92a0-178">Application Event Log</span></span>

<span data-ttu-id="e92a0-179">アプリケーション イベント ログにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="e92a0-179">Access the Application Event Log:</span></span>

1. <span data-ttu-id="e92a0-180">[スタート] メニューを開き、**イベント ビューアー**を検索し、**イベント ビューアー** アプリを選択します。</span><span class="sxs-lookup"><span data-stu-id="e92a0-180">Open the Start menu, search for **Event Viewer**, and then select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="e92a0-181">**イベント ビューアー**で **[Windows ログ]** ノードを開きます。</span><span class="sxs-lookup"><span data-stu-id="e92a0-181">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="e92a0-182">**[Application]** を選択して、アプリケーション イベント ログを開きます。</span><span class="sxs-lookup"><span data-stu-id="e92a0-182">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="e92a0-183">失敗したアプリに関連するエラーを検索します。</span><span class="sxs-lookup"><span data-stu-id="e92a0-183">Search for errors associated with the failing app.</span></span> <span data-ttu-id="e92a0-184">エラーの *[ソース]* 列には *IIS AspNetCore Module* または *IIS Express AspNetCore Module* の値が表示されます。</span><span class="sxs-lookup"><span data-stu-id="e92a0-184">Errors have a value of *IIS AspNetCore Module* or *IIS Express AspNetCore Module* in the *Source* column.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="e92a0-185">コマンド プロンプトでアプリを実行する</span><span class="sxs-lookup"><span data-stu-id="e92a0-185">Run the app at a command prompt</span></span>

<span data-ttu-id="e92a0-186">多くの起動時エラーでは、アプリケーション イベント ログに役に立つ情報が生成されません。</span><span class="sxs-lookup"><span data-stu-id="e92a0-186">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="e92a0-187">ホスティング システムのコマンド プロンプトでアプリを実行すると、エラーの原因がわかることがあります。</span><span class="sxs-lookup"><span data-stu-id="e92a0-187">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span>

#### <a name="framework-dependent-deployment"></a><span data-ttu-id="e92a0-188">フレームワークに依存する展開</span><span class="sxs-lookup"><span data-stu-id="e92a0-188">Framework-dependent deployment</span></span>

<span data-ttu-id="e92a0-189">アプリが[フレームワークに依存する展開](/dotnet/core/deploying/#framework-dependent-deployments-fdd)の場合:</span><span class="sxs-lookup"><span data-stu-id="e92a0-189">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

1. <span data-ttu-id="e92a0-190">コマンド プロンプトで展開フォルダーに移動し、*dotnet.exe* 使用してアプリのアセンブリを実行して、アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="e92a0-190">At a command prompt, navigate to the deployment folder and run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="e92a0-191">コマンド `dotnet .\<assembly_name>.dll` の \<assembly_name> にアプリのアセンブリの名前を指定して実行します。</span><span class="sxs-lookup"><span data-stu-id="e92a0-191">In the following command, substitute the name of the app's assembly for \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span></span>
1. <span data-ttu-id="e92a0-192">エラーを示すアプリからのコンソール出力は、すべてコンソール ウィンドウに書き込まれます。</span><span class="sxs-lookup"><span data-stu-id="e92a0-192">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="e92a0-193">アプリへの要求時にエラーが発生した場合は、Kestrel がリッスンしているホストとポートに要求が送信されます。</span><span class="sxs-lookup"><span data-stu-id="e92a0-193">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="e92a0-194">既定のホストと post を使用して `http://localhost:5000/` に要求を行います。</span><span class="sxs-lookup"><span data-stu-id="e92a0-194">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="e92a0-195">アプリが Kestrel のエンドポイント アドレスで正常に応答する場合、問題はホスティングの構成に関連している可能性が高く、アプリ内が原因の可能性は低くなります。</span><span class="sxs-lookup"><span data-stu-id="e92a0-195">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

#### <a name="self-contained-deployment"></a><span data-ttu-id="e92a0-196">自己完結型の展開</span><span class="sxs-lookup"><span data-stu-id="e92a0-196">Self-contained deployment</span></span>

<span data-ttu-id="e92a0-197">アプリが[自己完結型の展開](/dotnet/core/deploying/#self-contained-deployments-scd)の場合:</span><span class="sxs-lookup"><span data-stu-id="e92a0-197">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

1. <span data-ttu-id="e92a0-198">コマンド プロンプトで、展開フォルダーに移動し、アプリの実行可能ファイルを実行します。</span><span class="sxs-lookup"><span data-stu-id="e92a0-198">At a command prompt, navigate to the deployment folder and run the app's executable.</span></span> <span data-ttu-id="e92a0-199">コマンド `<assembly_name>.exe` の \<assembly_name> にアプリのアセンブリの名前を指定して実行します。</span><span class="sxs-lookup"><span data-stu-id="e92a0-199">In the following command, substitute the name of the app's assembly for \<assembly_name>: `<assembly_name>.exe`.</span></span>
1. <span data-ttu-id="e92a0-200">エラーを示すアプリからのコンソール出力は、すべてコンソール ウィンドウに書き込まれます。</span><span class="sxs-lookup"><span data-stu-id="e92a0-200">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="e92a0-201">アプリへの要求時にエラーが発生した場合は、Kestrel がリッスンしているホストとポートに要求が送信されます。</span><span class="sxs-lookup"><span data-stu-id="e92a0-201">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="e92a0-202">既定のホストと post を使用して `http://localhost:5000/` に要求を行います。</span><span class="sxs-lookup"><span data-stu-id="e92a0-202">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="e92a0-203">アプリが Kestrel のエンドポイント アドレスで正常に応答する場合、問題はホスティングの構成に関連している可能性が高く、アプリ内が原因の可能性は低くなります。</span><span class="sxs-lookup"><span data-stu-id="e92a0-203">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

### <a name="aspnet-core-module-stdout-log"></a><span data-ttu-id="e92a0-204">ASP.NET Core モジュールの stdout ログ</span><span class="sxs-lookup"><span data-stu-id="e92a0-204">ASP.NET Core Module stdout log</span></span>

<span data-ttu-id="e92a0-205">stdout ログを有効にして表示するには:</span><span class="sxs-lookup"><span data-stu-id="e92a0-205">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="e92a0-206">ホスティング システム上のサイトの展開フォルダーに移動します。</span><span class="sxs-lookup"><span data-stu-id="e92a0-206">Navigate to the site's deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="e92a0-207">*logs* フォルダーが存在しない場合は、フォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="e92a0-207">If the *logs* folder isn't present, create the folder.</span></span> <span data-ttu-id="e92a0-208">MSBuild で展開フォルダーに *logs* フォルダーを自動的に作成されるようにする手順については、「[ディレクトリ構造](xref:host-and-deploy/directory-structure)」のトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="e92a0-208">For instructions on how to enable MSBuild to create the *logs* folder in the deployment automatically, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>
1. <span data-ttu-id="e92a0-209">*web.config* ファイルを編集します。</span><span class="sxs-lookup"><span data-stu-id="e92a0-209">Edit the *web.config* file.</span></span> <span data-ttu-id="e92a0-210">**stdoutLogEnabled** を `true` に設定し、**stdoutLogFile** のパスを *logs* フォルダー (たとえば `.\logs\stdout`) を指すように変更します。</span><span class="sxs-lookup"><span data-stu-id="e92a0-210">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to point to the *logs* folder (for example, `.\logs\stdout`).</span></span> <span data-ttu-id="e92a0-211">パスの `stdout` はログ ファイル名のプレフィックスです。</span><span class="sxs-lookup"><span data-stu-id="e92a0-211">`stdout` in the path is the log file name prefix.</span></span> <span data-ttu-id="e92a0-212">ログ ファイルの作成時には、タイムスタンプ、プロセス ID、およびファイルの拡張子が自動的に追加されます。</span><span class="sxs-lookup"><span data-stu-id="e92a0-212">A timestamp, process id, and file extension are added automatically when the log is created.</span></span> <span data-ttu-id="e92a0-213">ファイル名のプレフィックスとして `stdout` を使用すると、一般的なログ ファイルの名前は *stdout_20180205184032_5412.log* になります。</span><span class="sxs-lookup"><span data-stu-id="e92a0-213">Using `stdout` as the file name prefix, a typical log file is named *stdout_20180205184032_5412.log*.</span></span>
1. <span data-ttu-id="e92a0-214">アプリケーション プールの ID に *logs* フォルダーへの書き込みアクセス許可があることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e92a0-214">Ensure your application pool's identity has write permissions to the *logs* folder.</span></span>
1. <span data-ttu-id="e92a0-215">更新した *web.config* ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="e92a0-215">Save the updated *web.config* file.</span></span>
1. <span data-ttu-id="e92a0-216">アプリに対して要求します。</span><span class="sxs-lookup"><span data-stu-id="e92a0-216">Make a request to the app.</span></span>
1. <span data-ttu-id="e92a0-217">*logs* フォルダーに移動します。</span><span class="sxs-lookup"><span data-stu-id="e92a0-217">Navigate to the *logs* folder.</span></span> <span data-ttu-id="e92a0-218">最新の stdout ログを探して開きます。</span><span class="sxs-lookup"><span data-stu-id="e92a0-218">Find and open the most recent stdout log.</span></span>
1. <span data-ttu-id="e92a0-219">ログのエラーを調べます。</span><span class="sxs-lookup"><span data-stu-id="e92a0-219">Study the log for errors.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e92a0-220">トラブルシューティングが完了したら、stdout ログを無効にします。</span><span class="sxs-lookup"><span data-stu-id="e92a0-220">Disable stdout logging when troubleshooting is complete.</span></span>

1. <span data-ttu-id="e92a0-221">*web.config* ファイルを編集します。</span><span class="sxs-lookup"><span data-stu-id="e92a0-221">Edit the *web.config* file.</span></span>
1. <span data-ttu-id="e92a0-222">**stdoutLogEnabled** を `false` に設定します。</span><span class="sxs-lookup"><span data-stu-id="e92a0-222">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="e92a0-223">ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="e92a0-223">Save the file.</span></span>

> [!WARNING]
> <span data-ttu-id="e92a0-224">stdout ログを無効にしないと、アプリまたはサーバーで障害が発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="e92a0-224">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="e92a0-225">ログ ファイルのサイズまたは作成されるログ ファイルの数に制限はありません。</span><span class="sxs-lookup"><span data-stu-id="e92a0-225">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="e92a0-226">ASP.NET Core アプリでのルーチン ログの場合は、ログ ファイルのサイズを制限し、ログをローテーションするログ ライブラリを使います。</span><span class="sxs-lookup"><span data-stu-id="e92a0-226">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="e92a0-227">詳細については、「[サードパーティ製のログ プロバイダー](xref:fundamentals/logging/index#third-party-logging-providers)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e92a0-227">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="enable-the-developer-exception-page"></a><span data-ttu-id="e92a0-228">開発者例外ページを有効にする</span><span class="sxs-lookup"><span data-stu-id="e92a0-228">Enable the Developer Exception Page</span></span>

<span data-ttu-id="e92a0-229">開発環境でアプリを実行するには、`ASPNETCORE_ENVIRONMENT` [環境変数を web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) に追加することができます。</span><span class="sxs-lookup"><span data-stu-id="e92a0-229">The `ASPNETCORE_ENVIRONMENT` [environment variable can be added to web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) to run the app in the Development environment.</span></span> <span data-ttu-id="e92a0-230">アプリの起動時にホスト ビルダー上で `UseEnvironment` によって環境がオーバーライドされない限り、環境変数を設定すると、アプリの実行時に[開発者例外ページ](xref:fundamentals/error-handling)が表示されます。</span><span class="sxs-lookup"><span data-stu-id="e92a0-230">As long as the environment isn't overridden in app startup by `UseEnvironment` on the host builder, setting the environment variable allows the [Developer Exception Page](xref:fundamentals/error-handling) to appear when the app is run.</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout"
      hostingModel="InProcess">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

<span data-ttu-id="e92a0-231">`ASPNETCORE_ENVIRONMENT` の環境変数を設定する方法は、インターネットに公開されないステージング サーバーやテスト用サーバーの場合にのみお勧めされます。</span><span class="sxs-lookup"><span data-stu-id="e92a0-231">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` is only recommended for use on staging and testing servers that aren't exposed to the Internet.</span></span> <span data-ttu-id="e92a0-232">トラブルシューティング後は、必ず *web.config* ファイルからこの環境変数を削除してください。</span><span class="sxs-lookup"><span data-stu-id="e92a0-232">Remove the environment variable from the *web.config* file after troubleshooting.</span></span> <span data-ttu-id="e92a0-233">*web.config* で環境変数を設定する方法の詳細については、[aspNetCore の environmentVariables 子要素](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e92a0-233">For information on setting environment variables in *web.config*, see [environmentVariables child element of aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span></span>

## <a name="common-startup-errors"></a><span data-ttu-id="e92a0-234">起動時の一般的なエラー</span><span class="sxs-lookup"><span data-stu-id="e92a0-234">Common startup errors</span></span>

<span data-ttu-id="e92a0-235">以下を参照してください。<xref:host-and-deploy/azure-iis-errors-reference></span><span class="sxs-lookup"><span data-stu-id="e92a0-235">See <xref:host-and-deploy/azure-iis-errors-reference>.</span></span> <span data-ttu-id="e92a0-236">このリファレンス トピックでは、アプリの起動を妨げる一般的な問題のほとんどが説明されています。</span><span class="sxs-lookup"><span data-stu-id="e92a0-236">Most of the common problems that prevent app startup are covered in the reference topic.</span></span>

## <a name="obtain-data-from-an-app"></a><span data-ttu-id="e92a0-237">アプリからデータを取得する</span><span class="sxs-lookup"><span data-stu-id="e92a0-237">Obtain data from an app</span></span>

<span data-ttu-id="e92a0-238">アプリが要求に応答できる場合は、ターミナル インライン ミドルウェアを使用して、要求、接続、その他のデータをアプリから取得します。</span><span class="sxs-lookup"><span data-stu-id="e92a0-238">If an app is capable of responding to requests, obtain request, connection, and additional data from the app using terminal inline middleware.</span></span> <span data-ttu-id="e92a0-239">詳細およびサンプル コードについては、「<xref:test/troubleshoot#obtain-data-from-an-app>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e92a0-239">For more information and sample code, see <xref:test/troubleshoot#obtain-data-from-an-app>.</span></span>

## <a name="create-a-dump"></a><span data-ttu-id="e92a0-240">ダンプを作成する</span><span class="sxs-lookup"><span data-stu-id="e92a0-240">Create a dump</span></span>

<span data-ttu-id="e92a0-241">"*ダンプ*" とはシステムのメモリのスナップショットであり、アプリのクラッシュ、起動の失敗、遅いアプリの原因を突き止めるのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="e92a0-241">A *dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="e92a0-242">アプリのクラッシュまたは例外の発生</span><span class="sxs-lookup"><span data-stu-id="e92a0-242">App crashes or encounters an exception</span></span>

<span data-ttu-id="e92a0-243">[Windows エラー報告 (WER)](/windows/desktop/wer/windows-error-reporting) からダンプを取得して分析します。</span><span class="sxs-lookup"><span data-stu-id="e92a0-243">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="e92a0-244">`c:\dumps` にクラッシュ ダンプ ファイルを保持するフォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="e92a0-244">Create a folder to hold crash dump files at `c:\dumps`.</span></span> <span data-ttu-id="e92a0-245">アプリ プールには、そのフォルダーへの書き込みアクセス権が必要です。</span><span class="sxs-lookup"><span data-stu-id="e92a0-245">The app pool must have write access to the folder.</span></span>
1. <span data-ttu-id="e92a0-246">[EnableDumps PowerShell スクリプト](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/EnableDumps.ps1) を実行します。</span><span class="sxs-lookup"><span data-stu-id="e92a0-246">Run the [EnableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/EnableDumps.ps1):</span></span>
   * <span data-ttu-id="e92a0-247">アプリで[インプロセス ホスティング モデル](xref:fundamentals/servers/index#in-process-hosting-model)が使われている場合は、*w3wp.exe* に対するスクリプトを実行します。</span><span class="sxs-lookup"><span data-stu-id="e92a0-247">If the app uses the [in-process hosting model](xref:fundamentals/servers/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\EnableDumps w3wp.exe c:\dumps
     ```

   * <span data-ttu-id="e92a0-248">アプリで[アウト プロセス ホスティング モデル](xref:fundamentals/servers/index#out-of-process-hosting-model)が使われている場合は、*dotnet.exe* に対するスクリプトを実行します。</span><span class="sxs-lookup"><span data-stu-id="e92a0-248">If the app uses the [out-of-process hosting model](xref:fundamentals/servers/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\EnableDumps dotnet.exe c:\dumps
     ```

1. <span data-ttu-id="e92a0-249">クラッシュが発生する条件の下でアプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="e92a0-249">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="e92a0-250">クラッシュが発生した後、[DisableDumps PowerShell スクリプト](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/DisableDumps.ps1)を実行します。</span><span class="sxs-lookup"><span data-stu-id="e92a0-250">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/DisableDumps.ps1):</span></span>
   * <span data-ttu-id="e92a0-251">アプリで[インプロセス ホスティング モデル](xref:fundamentals/servers/index#in-process-hosting-model)が使われている場合は、*w3wp.exe* に対するスクリプトを実行します。</span><span class="sxs-lookup"><span data-stu-id="e92a0-251">If the app uses the [in-process hosting model](xref:fundamentals/servers/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\DisableDumps w3wp.exe
     ```

   * <span data-ttu-id="e92a0-252">アプリで[アウト プロセス ホスティング モデル](xref:fundamentals/servers/index#out-of-process-hosting-model)が使われている場合は、*dotnet.exe* に対するスクリプトを実行します。</span><span class="sxs-lookup"><span data-stu-id="e92a0-252">If the app uses the [out-of-process hosting model](xref:fundamentals/servers/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\DisableDumps dotnet.exe
     ```

<span data-ttu-id="e92a0-253">アプリがクラッシュし、ダンプの収集が完了したら、アプリを普通に終了してかまいません。</span><span class="sxs-lookup"><span data-stu-id="e92a0-253">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="e92a0-254">PowerShell スクリプトにより、アプリごとに最大 5 つのダンプを収集する WER が構成されます。</span><span class="sxs-lookup"><span data-stu-id="e92a0-254">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="e92a0-255">クラッシュ ダンプでは、大量のディスク領域 (1 つにつき最大、数ギガバイト) が使用される場合があります。</span><span class="sxs-lookup"><span data-stu-id="e92a0-255">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="e92a0-256">アプリが起動時または正常な実行中にハングまたは失敗する</span><span class="sxs-lookup"><span data-stu-id="e92a0-256">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="e92a0-257">アプリが起動時または正常な実行中に "*ハング*" (応答を停止するがクラッシュしない) または失敗するときは、「[User-Mode Dump Files:Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool)」(ユーザー モード ダンプ ファイル: 最適なツールの選択) を参照し、適切なツールを選択してダンプを生成します。</span><span class="sxs-lookup"><span data-stu-id="e92a0-257">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

### <a name="analyze-the-dump"></a><span data-ttu-id="e92a0-258">ダンプを分析する</span><span class="sxs-lookup"><span data-stu-id="e92a0-258">Analyze the dump</span></span>

<span data-ttu-id="e92a0-259">ダンプはいくつかの方法で分析できます。</span><span class="sxs-lookup"><span data-stu-id="e92a0-259">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="e92a0-260">詳細については、「[Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file)」(ユーザー モード ダンプ ファイルの分析) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e92a0-260">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="e92a0-261">リモート デバッグ</span><span class="sxs-lookup"><span data-stu-id="e92a0-261">Remote debugging</span></span>

<span data-ttu-id="e92a0-262">Visual Studio ドキュメントの「[Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer)」(Visual Studio 2017 でリモート IIS コンピューター上の ASP.NET Core をリモート デバッグする) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e92a0-262">See [Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) in the Visual Studio documentation.</span></span>

## <a name="application-insights"></a><span data-ttu-id="e92a0-263">Application Insights</span><span class="sxs-lookup"><span data-stu-id="e92a0-263">Application Insights</span></span>

<span data-ttu-id="e92a0-264">[Application Insights](/azure/application-insights/) は、IIS でホストされているアプリからのテレメトリを提供し、エラー ログやレポート機能を備えています。</span><span class="sxs-lookup"><span data-stu-id="e92a0-264">[Application Insights](/azure/application-insights/) provides telemetry from apps hosted by IIS, including error logging and reporting features.</span></span> <span data-ttu-id="e92a0-265">Application Insights は、アプリのログ機能が使用可能になってアプリが起動した後で発生したエラーのみをレポートできます。</span><span class="sxs-lookup"><span data-stu-id="e92a0-265">Application Insights can only report on errors that occur after the app starts when the app's logging features become available.</span></span> <span data-ttu-id="e92a0-266">詳細については、「[Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e92a0-266">For more information, see [Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="additional-advice"></a><span data-ttu-id="e92a0-267">その他の注意事項</span><span class="sxs-lookup"><span data-stu-id="e92a0-267">Additional advice</span></span>

<span data-ttu-id="e92a0-268">開発コンピューター上の .NET Core SDK またはアプリ内のパッケージ バージョンのいずれかをアップグレードした直後に、機能しているアプリが失敗することがあります。</span><span class="sxs-lookup"><span data-stu-id="e92a0-268">Sometimes a functioning app fails immediately after upgrading either the .NET Core SDK on the development machine or package versions within the app.</span></span> <span data-ttu-id="e92a0-269">場合によっては、パッケージに統一性がないと、メジャー アップグレード実行時にアプリが破壊されることがあります。</span><span class="sxs-lookup"><span data-stu-id="e92a0-269">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="e92a0-270">これらの問題のほとんどは、次の手順で解決できます。</span><span class="sxs-lookup"><span data-stu-id="e92a0-270">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="e92a0-271">*bin* フォルダーと *obj* フォルダーを削除します。</span><span class="sxs-lookup"><span data-stu-id="e92a0-271">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="e92a0-272">*%UserProfile%\\.nuget\\packages* と *%LocalAppData%\\Nuget\\v3-cache* のパッケージ キャッシュをクリアします。</span><span class="sxs-lookup"><span data-stu-id="e92a0-272">Clear the package caches at *%UserProfile%\\.nuget\\packages* and *%LocalAppData%\\Nuget\\v3-cache*.</span></span>
1. <span data-ttu-id="e92a0-273">プロジェクトを復元してリビルドします。</span><span class="sxs-lookup"><span data-stu-id="e92a0-273">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="e92a0-274">アプリを再展開する前に、サーバー上の以前の展開が完全に削除されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e92a0-274">Confirm that the prior deployment on the server has been completely deleted prior to redeploying the app.</span></span>

> [!TIP]
> <span data-ttu-id="e92a0-275">パッケージ キャッシュをクリアするには、コマンド プロンプトから `dotnet nuget locals all --clear` を実行する方法が便利です。</span><span class="sxs-lookup"><span data-stu-id="e92a0-275">A convenient way to clear package caches is to execute `dotnet nuget locals all --clear` from a command prompt.</span></span>
>
> <span data-ttu-id="e92a0-276">パッケージ キャッシュをクリアするには、[nuget.exe](https://www.nuget.org/downloads) ツールを使用して `nuget locals all -clear` コマンドを実行する方法もあります。</span><span class="sxs-lookup"><span data-stu-id="e92a0-276">Clearing package caches can also be accomplished by using the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="e92a0-277">*nuget.exe* は、Windows デスクトップ オペレーティング システムにバンドルされているインストールではなく、[NuGet Web サイト](https://www.nuget.org/downloads)から別に入手する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e92a0-277">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e92a0-278">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="e92a0-278">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/azure-apps/troubleshoot>
