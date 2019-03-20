---
title: Azure App Service での ASP.NET Core のトラブルシューティング
author: guardrex
description: ASP.NET Core Azure App Service の配置に関する問題を診断する方法を学習します。
ms.author: riande
ms.custom: mvc
ms.date: 03/06/2019
uid: host-and-deploy/azure-apps/troubleshoot
ms.openlocfilehash: 326f66070d51c04298abbf6292d2d350414311de
ms.sourcegitcommit: 34bf9fc6ea814c039401fca174642f0acb14be3c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2019
ms.locfileid: "57841403"
---
# <a name="troubleshoot-aspnet-core-on-azure-app-service"></a><span data-ttu-id="edf04-103">Azure App Service での ASP.NET Core のトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="edf04-103">Troubleshoot ASP.NET Core on Azure App Service</span></span>

<span data-ttu-id="edf04-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="edf04-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE [Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

<span data-ttu-id="edf04-105">この記事では、Azure App Service の診断ツールを使って ASP.NET Core アプリの起動時の問題を診断する方法の手順を説明します。</span><span class="sxs-lookup"><span data-stu-id="edf04-105">This article provides instructions on how to diagnose an ASP.NET Core app startup issue using Azure App Service's diagnostic tools.</span></span> <span data-ttu-id="edf04-106">追加のトラブルシューティングのアドバイスについては、Azure ドキュメントの「[Azure App Service 診断の概要](/azure/app-service/app-service-diagnostics)」と「[方法: Azure App Service のアプリの監視](/azure/app-service/web-sites-monitor)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="edf04-106">For additional troubleshooting advice, see [Azure App Service diagnostics overview](/azure/app-service/app-service-diagnostics) and [How to: Monitor Apps in Azure App Service](/azure/app-service/web-sites-monitor) in the Azure documentation.</span></span>

## <a name="app-startup-errors"></a><span data-ttu-id="edf04-107">アプリ起動時のエラー</span><span class="sxs-lookup"><span data-stu-id="edf04-107">App startup errors</span></span>

<span data-ttu-id="edf04-108">**502.5 処理エラー**</span><span class="sxs-lookup"><span data-stu-id="edf04-108">**502.5 Process Failure**</span></span>  
<span data-ttu-id="edf04-109">ワーカー プロセスが失敗します。</span><span class="sxs-lookup"><span data-stu-id="edf04-109">The worker process fails.</span></span> <span data-ttu-id="edf04-110">アプリは起動しません。</span><span class="sxs-lookup"><span data-stu-id="edf04-110">The app doesn't start.</span></span>

<span data-ttu-id="edf04-111">[ASP.NET Core モジュール](xref:host-and-deploy/aspnet-core-module)はワーカー プロセスの開始を試みますが、開始に失敗します。</span><span class="sxs-lookup"><span data-stu-id="edf04-111">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the worker process but it fails to start.</span></span> <span data-ttu-id="edf04-112">この種の問題のトラブルシューティングには、アプリケーション イベント ログを調べると役に立つことがよくあります。</span><span class="sxs-lookup"><span data-stu-id="edf04-112">Examining the Application Event Log often helps troubleshoot this type of problem.</span></span> <span data-ttu-id="edf04-113">ログへのアクセスについては、「[アプリケーション イベント ログ](#application-event-log)」セクションで説明します。</span><span class="sxs-lookup"><span data-stu-id="edf04-113">Accessing the log is explained in the [Application Event Log](#application-event-log) section.</span></span>

<span data-ttu-id="edf04-114">正しく構成されていないアプリによりワーカー プロセスが失敗する場合、"*502.5 処理エラー*" のエラー ページが返されます。</span><span class="sxs-lookup"><span data-stu-id="edf04-114">The *502.5 Process Failure* error page is returned when a misconfigured app causes the worker process to fail:</span></span>

![502.5 処理エラー ページが表示されているブラウザー ウィンドウ](troubleshoot/_static/process-failure-page.png)

<span data-ttu-id="edf04-116">**500 内部サーバー エラー**</span><span class="sxs-lookup"><span data-stu-id="edf04-116">**500 Internal Server Error**</span></span>  
<span data-ttu-id="edf04-117">アプリは起動しますが、エラーのためにサーバーは要求を実行できません。</span><span class="sxs-lookup"><span data-stu-id="edf04-117">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="edf04-118">このエラーは、起動時または応答の作成中に、アプリのコード内で発生します。</span><span class="sxs-lookup"><span data-stu-id="edf04-118">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="edf04-119">応答にコンテンツが含まれていないか、またはブラウザーに "*500 内部サーバー エラー*" という応答が表示される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="edf04-119">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="edf04-120">通常、アプリケーション イベント ログではアプリが正常に起動したことが示されます。</span><span class="sxs-lookup"><span data-stu-id="edf04-120">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="edf04-121">サーバーから見るとそれは正しいことです。</span><span class="sxs-lookup"><span data-stu-id="edf04-121">From the server's perspective, that's correct.</span></span> <span data-ttu-id="edf04-122">アプリは起動しましたが、有効な応答を生成できません。</span><span class="sxs-lookup"><span data-stu-id="edf04-122">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="edf04-123">問題のトラブルシューティングを行うには、[Kudu コンソールでアプリを実行する](#run-the-app-in-the-kudu-console)か、または [ASP.NET Core モジュールの stdout ログを有効にします](#aspnet-core-module-stdout-log)。</span><span class="sxs-lookup"><span data-stu-id="edf04-123">[Run the app in the Kudu console](#run-the-app-in-the-kudu-console) or [enable the ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log) to troubleshoot the problem.</span></span>

<span data-ttu-id="edf04-124">**接続のリセット**</span><span class="sxs-lookup"><span data-stu-id="edf04-124">**Connection reset**</span></span>

<span data-ttu-id="edf04-125">ヘッダー送信後にエラーが発生した場合、サーバーが **500 内部サーバー エラー**を送信するには遅すぎます。</span><span class="sxs-lookup"><span data-stu-id="edf04-125">If an error occurs after the headers are sent, it's too late for the server to send a **500 Internal Server Error** when an error occurs.</span></span> <span data-ttu-id="edf04-126">このような状況は、応答に対する複雑なオブジェクトのシリアル化中にエラーが起きたときによく発生します。</span><span class="sxs-lookup"><span data-stu-id="edf04-126">This often happens when an error occurs during the serialization of complex objects for a response.</span></span> <span data-ttu-id="edf04-127">この種のエラーは、クライアントでは "*接続リセット*" エラーとして表示されます。</span><span class="sxs-lookup"><span data-stu-id="edf04-127">This type of error appears as a *connection reset* error on the client.</span></span> <span data-ttu-id="edf04-128">この種のエラーのトラブルシューティングには、[アプリケーション ログ](xref:fundamentals/logging/index)が役に立つことがあります。</span><span class="sxs-lookup"><span data-stu-id="edf04-128">[Application logging](xref:fundamentals/logging/index) can help troubleshoot these types of errors.</span></span>

## <a name="default-startup-limits"></a><span data-ttu-id="edf04-129">既定の起動制限</span><span class="sxs-lookup"><span data-stu-id="edf04-129">Default startup limits</span></span>

<span data-ttu-id="edf04-130">ASP.NET Core モジュールの *startupTimeLimit* は、既定では 120 秒に構成されます。</span><span class="sxs-lookup"><span data-stu-id="edf04-130">The ASP.NET Core Module is configured with a default *startupTimeLimit* of 120 seconds.</span></span> <span data-ttu-id="edf04-131">既定値のままにした場合、モジュールで処理エラーが記録されるまでに、アプリは最大で 2 分を起動にかけることができます。</span><span class="sxs-lookup"><span data-stu-id="edf04-131">When left at the default value, an app may take up to two minutes to start before the module logs a process failure.</span></span> <span data-ttu-id="edf04-132">モジュールの構成の詳細については、「[AspNetCore 要素の属性](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="edf04-132">For information on configuring the module, see [Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

## <a name="troubleshoot-app-startup-errors"></a><span data-ttu-id="edf04-133">アプリの起動エラーのトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="edf04-133">Troubleshoot app startup errors</span></span>

### <a name="application-event-log"></a><span data-ttu-id="edf04-134">アプリケーション イベント ログ</span><span class="sxs-lookup"><span data-stu-id="edf04-134">Application Event Log</span></span>

<span data-ttu-id="edf04-135">アプリケーション イベント ログにアクセスするには、Azure portal の **[問題の診断と解決]** ブレードを使います。</span><span class="sxs-lookup"><span data-stu-id="edf04-135">To access the Application Event Log, use the **Diagnose and solve problems** blade in the Azure portal:</span></span>

1. <span data-ttu-id="edf04-136">Azure portal の **[App Services]** でアプリを開きます。</span><span class="sxs-lookup"><span data-stu-id="edf04-136">In the Azure portal, open the app in **App Services**.</span></span>
1. <span data-ttu-id="edf04-137">**[問題の診断と解決]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="edf04-137">Select **Diagnose and solve problems**.</span></span>
1. <span data-ttu-id="edf04-138">**[診断ツール]** という見出しを選択します。</span><span class="sxs-lookup"><span data-stu-id="edf04-138">Select the **Diagnostic Tools** heading.</span></span>
1. <span data-ttu-id="edf04-139">**[サポート ツール]** で **[アプリケーション イベント]** ボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="edf04-139">Under **Support Tools**, select the **Application Events** button.</span></span>
1. <span data-ttu-id="edf04-140">**[Source]\(ソース\)** 列で、*IIS AspNetCoreModule* または *IIS AspNetCoreModule V2* によって提供された最新のエラーを調べます。</span><span class="sxs-lookup"><span data-stu-id="edf04-140">Examine the latest error provided by the *IIS AspNetCoreModule* or *IIS AspNetCoreModule V2* entry in the **Source** column.</span></span>

<span data-ttu-id="edf04-141">**[問題の診断と解決]** ブレードを使う代わりに、[Kudu](https://github.com/projectkudu/kudu/wiki) を使ってアプリケーション イベント ログ ファイルを直接調べることもできます。</span><span class="sxs-lookup"><span data-stu-id="edf04-141">An alternative to using the **Diagnose and solve problems** blade is to examine the Application Event Log file directly using [Kudu](https://github.com/projectkudu/kudu/wiki):</span></span>

1. <span data-ttu-id="edf04-142">**[開発ツール]** 領域で **[高度なツール]** を開きます。</span><span class="sxs-lookup"><span data-stu-id="edf04-142">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="edf04-143">**[Go&rarr;]** ボタンを選びます。</span><span class="sxs-lookup"><span data-stu-id="edf04-143">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="edf04-144">新しいブラウザー タブまたはウィンドウで Kudu コンソールが開きます。</span><span class="sxs-lookup"><span data-stu-id="edf04-144">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="edf04-145">ページの上部にあるナビゲーション バーを使って **[デバッグ コンソール]** を開き、**[CMD]** を選びます。</span><span class="sxs-lookup"><span data-stu-id="edf04-145">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="edf04-146">**LogFiles** フォルダーを開きます。</span><span class="sxs-lookup"><span data-stu-id="edf04-146">Open the **LogFiles** folder.</span></span>
1. <span data-ttu-id="edf04-147">*eventlog.xml* ファイルの横にある鉛筆アイコンを選びます。</span><span class="sxs-lookup"><span data-stu-id="edf04-147">Select the pencil icon next to the *eventlog.xml* file.</span></span>
1. <span data-ttu-id="edf04-148">ログを調べます。</span><span class="sxs-lookup"><span data-stu-id="edf04-148">Examine the log.</span></span> <span data-ttu-id="edf04-149">ログの末尾までスクロールし、最新のイベントを確認します。</span><span class="sxs-lookup"><span data-stu-id="edf04-149">Scroll to the bottom of the log to see the most recent events.</span></span>

### <a name="run-the-app-in-the-kudu-console"></a><span data-ttu-id="edf04-150">Kudu コンソールでアプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="edf04-150">Run the app in the Kudu console</span></span>

<span data-ttu-id="edf04-151">多くの起動時エラーでは、アプリケーション イベント ログに役に立つ情報が生成されません。</span><span class="sxs-lookup"><span data-stu-id="edf04-151">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="edf04-152">[Kudu](https://github.com/projectkudu/kudu/wiki) のリモート実行コンソールでアプリを実行すると、エラーを検出することができます。</span><span class="sxs-lookup"><span data-stu-id="edf04-152">You can run the app in the [Kudu](https://github.com/projectkudu/kudu/wiki) Remote Execution Console to discover the error:</span></span>

1. <span data-ttu-id="edf04-153">**[開発ツール]** 領域で **[高度なツール]** を開きます。</span><span class="sxs-lookup"><span data-stu-id="edf04-153">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="edf04-154">**[Go&rarr;]** ボタンを選びます。</span><span class="sxs-lookup"><span data-stu-id="edf04-154">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="edf04-155">新しいブラウザー タブまたはウィンドウで Kudu コンソールが開きます。</span><span class="sxs-lookup"><span data-stu-id="edf04-155">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="edf04-156">ページの上部にあるナビゲーション バーを使って **[デバッグ コンソール]** を開き、**[CMD]** を選びます。</span><span class="sxs-lookup"><span data-stu-id="edf04-156">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>

#### <a name="test-a-32-bit-x86-app"></a><span data-ttu-id="edf04-157">32 ビット (x86) アプリをテストする</span><span class="sxs-lookup"><span data-stu-id="edf04-157">Test a 32-bit (x86) app</span></span>

##### <a name="current-release"></a><span data-ttu-id="edf04-158">現在のリリース</span><span class="sxs-lookup"><span data-stu-id="edf04-158">Current release</span></span>

1. `cd d:\home\site\wwwroot`
1. <span data-ttu-id="edf04-159">次のようにアプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="edf04-159">Run the app:</span></span>
   * <span data-ttu-id="edf04-160">アプリが[フレームワークに依存する展開](/dotnet/core/deploying/#framework-dependent-deployments-fdd)の場合:</span><span class="sxs-lookup"><span data-stu-id="edf04-160">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

     ```console
     dotnet .\{ASSEMBLY NAME}.dll
     ```
   * <span data-ttu-id="edf04-161">アプリが[自己完結型の展開](/dotnet/core/deploying/#self-contained-deployments-scd)の場合:</span><span class="sxs-lookup"><span data-stu-id="edf04-161">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

     ```console
     {ASSEMBLY NAME}.exe
     ```
   
<span data-ttu-id="edf04-162">エラーを示すアプリからのコンソール出力はすべて、Kudu コンソールにパイプされます。</span><span class="sxs-lookup"><span data-stu-id="edf04-162">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>
   
##### <a name="framework-depdendent-deployment-running-on-a-preview-release"></a><span data-ttu-id="edf04-163">プレビュー リリース上で実行されているフレームワークに依存する展開</span><span class="sxs-lookup"><span data-stu-id="edf04-163">Framework-depdendent deployment running on a preview release</span></span>

<span data-ttu-id="edf04-164">"*ASP.NET Core {バージョン} (x86) ランタイムのサイト拡張機能をインストールする必要があります。*"</span><span class="sxs-lookup"><span data-stu-id="edf04-164">*Requires installing the ASP.NET Core {VERSION} (x86) Runtime site extension.*</span></span>

1. <span data-ttu-id="edf04-165">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` はランタイム バージョンです)</span><span class="sxs-lookup"><span data-stu-id="edf04-165">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="edf04-166">アプリを実行します: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="edf04-166">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="edf04-167">エラーを示すアプリからのコンソール出力はすべて、Kudu コンソールにパイプされます。</span><span class="sxs-lookup"><span data-stu-id="edf04-167">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

#### <a name="test-a-64-bit-x64-app"></a><span data-ttu-id="edf04-168">64 ビット (x64) アプリをテストする</span><span class="sxs-lookup"><span data-stu-id="edf04-168">Test a 64-bit (x64) app</span></span>

##### <a name="current-release"></a><span data-ttu-id="edf04-169">現在のリリース</span><span class="sxs-lookup"><span data-stu-id="edf04-169">Current release</span></span>

* <span data-ttu-id="edf04-170">アプリが 64 ビット (x64) の[フレームワークに依存する展開](/dotnet/core/deploying/#framework-dependent-deployments-fdd)の場合:</span><span class="sxs-lookup"><span data-stu-id="edf04-170">If the app is a 64-bit (x64) [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>
  1. `cd D:\Program Files\dotnet`
  1. <span data-ttu-id="edf04-171">アプリを実行します: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="edf04-171">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>
* <span data-ttu-id="edf04-172">アプリが[自己完結型の展開](/dotnet/core/deploying/#self-contained-deployments-scd)の場合:</span><span class="sxs-lookup"><span data-stu-id="edf04-172">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>
  1. `cd D:\home\site\wwwroot`
  1. <span data-ttu-id="edf04-173">アプリを実行します: `{ASSEMBLY NAME}.exe`</span><span class="sxs-lookup"><span data-stu-id="edf04-173">Run the app: `{ASSEMBLY NAME}.exe`</span></span>

<span data-ttu-id="edf04-174">エラーを示すアプリからのコンソール出力はすべて、Kudu コンソールにパイプされます。</span><span class="sxs-lookup"><span data-stu-id="edf04-174">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

##### <a name="framework-depdendent-deployment-running-on-a-preview-release"></a><span data-ttu-id="edf04-175">プレビュー リリース上で実行されているフレームワークに依存する展開</span><span class="sxs-lookup"><span data-stu-id="edf04-175">Framework-depdendent deployment running on a preview release</span></span>

<span data-ttu-id="edf04-176">"*ASP.NET Core {バージョン} (x64) ランタイムのサイト拡張機能をインストールする必要があります。*"</span><span class="sxs-lookup"><span data-stu-id="edf04-176">*Requires installing the ASP.NET Core {VERSION} (x64) Runtime site extension.*</span></span>

1. <span data-ttu-id="edf04-177">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` はランタイム バージョンです)</span><span class="sxs-lookup"><span data-stu-id="edf04-177">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="edf04-178">アプリを実行します: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="edf04-178">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="edf04-179">エラーを示すアプリからのコンソール出力はすべて、Kudu コンソールにパイプされます。</span><span class="sxs-lookup"><span data-stu-id="edf04-179">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

### <a name="aspnet-core-module-stdout-log"></a><span data-ttu-id="edf04-180">ASP.NET Core モジュールの stdout ログ</span><span class="sxs-lookup"><span data-stu-id="edf04-180">ASP.NET Core Module stdout log</span></span>

<span data-ttu-id="edf04-181">ASP.NET Core モジュールの stdout には、アプリケーション イベント ログでは見つからない有用なエラー メッセージが記録されることがよくあります。</span><span class="sxs-lookup"><span data-stu-id="edf04-181">The ASP.NET Core Module stdout log often records useful error messages not found in the Application Event Log.</span></span> <span data-ttu-id="edf04-182">stdout ログを有効にして表示するには:</span><span class="sxs-lookup"><span data-stu-id="edf04-182">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="edf04-183">Azure portal で **[問題の診断と解決]** ブレードに移動します。</span><span class="sxs-lookup"><span data-stu-id="edf04-183">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="edf04-184">**[SELECT PROBLEM CATEGORY]\(問題カテゴリの選択\)** で、**[Web App Down]\(Web アプリのダウン\)** ボタンを選びます。</span><span class="sxs-lookup"><span data-stu-id="edf04-184">Under **SELECT PROBLEM CATEGORY**, select the **Web App Down** button.</span></span>
1. <span data-ttu-id="edf04-185">**[Suggested Solutions]\(推奨される解決方法\)** > **[Enable Stdout Log Redirection]\(Stdout ログのリダイレクトを有効にする\)** で、ボタンをクリックして **[Open Kudu Console to edit Web.Config]\(Kudu コンソールを開いて Web.Config を編集する\)** ボタンを選びます。</span><span class="sxs-lookup"><span data-stu-id="edf04-185">Under **Suggested Solutions** > **Enable Stdout Log Redirection**, select the button to **Open Kudu Console to edit Web.Config**.</span></span>
1. <span data-ttu-id="edf04-186">Kudu の**診断コンソール**で、パス **site** > **wwwroot** へのフォルダーを開きます。</span><span class="sxs-lookup"><span data-stu-id="edf04-186">In the Kudu **Diagnostic Console**, open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="edf04-187">下にスクロールして、一覧の最後にある *web.config* ファイルを表示します。</span><span class="sxs-lookup"><span data-stu-id="edf04-187">Scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="edf04-188">*web.config* ファイルの隣の鉛筆アイコンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="edf04-188">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="edf04-189">**stdoutLogEnabled** を `true` に設定し、**stdoutLogFile** パスを `\\?\%home%\LogFiles\stdout` に変更します。</span><span class="sxs-lookup"><span data-stu-id="edf04-189">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="edf04-190">**[保存]** を選んで、更新した *web.config* ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="edf04-190">Select **Save** to save the updated *web.config* file.</span></span>
1. <span data-ttu-id="edf04-191">アプリに対して要求します。</span><span class="sxs-lookup"><span data-stu-id="edf04-191">Make a request to the app.</span></span>
1. <span data-ttu-id="edf04-192">Azure portal に戻ります。</span><span class="sxs-lookup"><span data-stu-id="edf04-192">Return to the Azure portal.</span></span> <span data-ttu-id="edf04-193">**[開発ツール]** 領域で **[高度なツール]** ブレードを選びます。</span><span class="sxs-lookup"><span data-stu-id="edf04-193">Select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="edf04-194">**[Go&rarr;]** ボタンを選びます。</span><span class="sxs-lookup"><span data-stu-id="edf04-194">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="edf04-195">新しいブラウザー タブまたはウィンドウで Kudu コンソールが開きます。</span><span class="sxs-lookup"><span data-stu-id="edf04-195">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="edf04-196">ページの上部にあるナビゲーション バーを使って **[デバッグ コンソール]** を開き、**[CMD]** を選びます。</span><span class="sxs-lookup"><span data-stu-id="edf04-196">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="edf04-197">**LogFiles** フォルダーを選びます。</span><span class="sxs-lookup"><span data-stu-id="edf04-197">Select the **LogFiles** folder.</span></span>
1. <span data-ttu-id="edf04-198">**[Modified]\(変更日\)** 列を調べて、変更日が最新の stdout ログの鉛筆アイコンを選んで編集します。</span><span class="sxs-lookup"><span data-stu-id="edf04-198">Inspect the **Modified** column and select the pencil icon to edit the stdout log with the latest modification date.</span></span>
1. <span data-ttu-id="edf04-199">ログ ファイルが開くと、エラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="edf04-199">When the log file opens, the error is displayed.</span></span>

<span data-ttu-id="edf04-200">トラブルシューティングが完了したら、stdout ログを無効にします。</span><span class="sxs-lookup"><span data-stu-id="edf04-200">Disable stdout logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="edf04-201">Kudu の**診断コンソール** で、パス **site** > **wwwroot** に戻り、*web.config* ファイルを表示します。</span><span class="sxs-lookup"><span data-stu-id="edf04-201">In the Kudu **Diagnostic Console**, return to the path **site** > **wwwroot** to reveal the *web.config* file.</span></span> <span data-ttu-id="edf04-202">鉛筆アイコンを選んで **web.config** ファイルを再び開きます。</span><span class="sxs-lookup"><span data-stu-id="edf04-202">Open the **web.config** file again by selecting the pencil icon.</span></span>
1. <span data-ttu-id="edf04-203">**stdoutLogEnabled** を `false` に設定します。</span><span class="sxs-lookup"><span data-stu-id="edf04-203">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="edf04-204">**[保存]** を選んでファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="edf04-204">Select **Save** to save the file.</span></span>

> [!WARNING]
> <span data-ttu-id="edf04-205">stdout ログを無効にしないと、アプリまたはサーバーで障害が発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="edf04-205">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="edf04-206">ログ ファイルのサイズまたは作成されるログ ファイルの数に制限はありません。</span><span class="sxs-lookup"><span data-stu-id="edf04-206">There's no limit on log file size or the number of log files created.</span></span> <span data-ttu-id="edf04-207">stdout ログは、アプリ起動時の問題のトラブルシューティングにのみ使ってください。</span><span class="sxs-lookup"><span data-stu-id="edf04-207">Only use stdout logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="edf04-208">起動後の ASP.NET Core アプリでの一般的なログの場合は、ログ ファイルのサイズを制限し、ログをローテーションするログ ライブラリを使います。</span><span class="sxs-lookup"><span data-stu-id="edf04-208">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="edf04-209">詳細については、「[サードパーティ製のログ プロバイダー](xref:fundamentals/logging/index#third-party-logging-providers)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="edf04-209">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="aspnet-core-module-debug-log"></a><span data-ttu-id="edf04-210">ASP.NET Core モジュール デバッグ ログ</span><span class="sxs-lookup"><span data-stu-id="edf04-210">ASP.NET Core Module debug log</span></span>

<span data-ttu-id="edf04-211">ASP.NET Core モジュール デバッグ ログでは、ASP.NET Core モジュールのさらに詳しいログが提供されます。</span><span class="sxs-lookup"><span data-stu-id="edf04-211">The ASP.NET Core Module debug log provides additional, deeper logging from the ASP.NET Core Module.</span></span> <span data-ttu-id="edf04-212">stdout ログを有効にして表示するには:</span><span class="sxs-lookup"><span data-stu-id="edf04-212">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="edf04-213">強化された診断ログを有効にするには、次のいずれかを実行します。</span><span class="sxs-lookup"><span data-stu-id="edf04-213">To enable the enhanced diagnostic log, perform either of the following:</span></span>
   * <span data-ttu-id="edf04-214">「[強化された診断ログ](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs)」の指示に従い、強化された診断ログをアプリに対して設定します。</span><span class="sxs-lookup"><span data-stu-id="edf04-214">Follow the instructions in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) to configure the app for an enhanced diagnostic logging.</span></span> <span data-ttu-id="edf04-215">アプリを再デプロイします。</span><span class="sxs-lookup"><span data-stu-id="edf04-215">Redeploy the app.</span></span>
   * <span data-ttu-id="edf04-216">Kudu コンソールを利用し、「`<handlerSettings>`強化された診断ログ[」にある ](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) をライブ アプリの *web.config* ファイルに追加します。</span><span class="sxs-lookup"><span data-stu-id="edf04-216">Add the `<handlerSettings>` shown in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) to the live app's *web.config* file using the Kudu console:</span></span>
     1. <span data-ttu-id="edf04-217">**[開発ツール]** 領域で **[高度なツール]** を開きます。</span><span class="sxs-lookup"><span data-stu-id="edf04-217">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="edf04-218">**[Go&rarr;]** ボタンを選びます。</span><span class="sxs-lookup"><span data-stu-id="edf04-218">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="edf04-219">新しいブラウザー タブまたはウィンドウで Kudu コンソールが開きます。</span><span class="sxs-lookup"><span data-stu-id="edf04-219">The Kudu console opens in a new browser tab or window.</span></span>
     1. <span data-ttu-id="edf04-220">ページの上部にあるナビゲーション バーを使って **[デバッグ コンソール]** を開き、**[CMD]** を選びます。</span><span class="sxs-lookup"><span data-stu-id="edf04-220">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
     1. <span data-ttu-id="edf04-221">パス **site** > **wwwroot** へのフォルダーを開きます。</span><span class="sxs-lookup"><span data-stu-id="edf04-221">Open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="edf04-222">鉛筆アイコンを選択し、*web.config* ファイルを編集します。</span><span class="sxs-lookup"><span data-stu-id="edf04-222">Edit the *web.config* file by selecting the pencil button.</span></span> <span data-ttu-id="edf04-223">「[強化された診断ログ](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs)」にある `<handlerSettings>` セクションを追加します。</span><span class="sxs-lookup"><span data-stu-id="edf04-223">Add the `<handlerSettings>` section as shown in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs).</span></span> <span data-ttu-id="edf04-224">**[保存]** ボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="edf04-224">Select the **Save** button.</span></span>
1. <span data-ttu-id="edf04-225">**[開発ツール]** 領域で **[高度なツール]** を開きます。</span><span class="sxs-lookup"><span data-stu-id="edf04-225">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="edf04-226">**[Go&rarr;]** ボタンを選びます。</span><span class="sxs-lookup"><span data-stu-id="edf04-226">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="edf04-227">新しいブラウザー タブまたはウィンドウで Kudu コンソールが開きます。</span><span class="sxs-lookup"><span data-stu-id="edf04-227">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="edf04-228">ページの上部にあるナビゲーション バーを使って **[デバッグ コンソール]** を開き、**[CMD]** を選びます。</span><span class="sxs-lookup"><span data-stu-id="edf04-228">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="edf04-229">パス **site** > **wwwroot** へのフォルダーを開きます。</span><span class="sxs-lookup"><span data-stu-id="edf04-229">Open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="edf04-230">*aspnetcore-debug.log* ファイルにパスを指定しなかった場合、ファイルが一覧に表示されます。</span><span class="sxs-lookup"><span data-stu-id="edf04-230">If you didn't supply a path for the *aspnetcore-debug.log* file, the file appears in the list.</span></span> <span data-ttu-id="edf04-231">パスを指定した場合、ログ ファイルの場所に移動します。</span><span class="sxs-lookup"><span data-stu-id="edf04-231">If you supplied a path, navigate to the location of the log file.</span></span>
1. <span data-ttu-id="edf04-232">ファイル名の隣にある鉛筆アイコンでログ ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="edf04-232">Open the log file with the pencil button next to the file name.</span></span>

<span data-ttu-id="edf04-233">トラブルシューティングが完了したら、debug ログを無効にします。</span><span class="sxs-lookup"><span data-stu-id="edf04-233">Disable debug logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="edf04-234">強化されたデバッグ ログを無効にするには、次のいずれかを実行します。</span><span class="sxs-lookup"><span data-stu-id="edf04-234">To disable the enhanced debug log, peform either of the following:</span></span>
   * <span data-ttu-id="edf04-235">ローカルの *web.config* ファイルから `<handlerSettings>` を削除し、アプリを再デプロイします。</span><span class="sxs-lookup"><span data-stu-id="edf04-235">Remove the `<handlerSettings>` from the *web.config* file locally and redeploy the app.</span></span>
   * <span data-ttu-id="edf04-236">Kudu コンソールを使用して *web.config* ファイルを編集し、`<handlerSettings>` セクションを削除します。</span><span class="sxs-lookup"><span data-stu-id="edf04-236">Use the Kudu console to edit the *web.config* file and remove the `<handlerSettings>` section.</span></span> <span data-ttu-id="edf04-237">ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="edf04-237">Save the file.</span></span>

> [!WARNING]
> <span data-ttu-id="edf04-238">debug ログを無効にしないと、アプリまたはサーバーで障害が発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="edf04-238">Failure to disable the debug log can lead to app or server failure.</span></span> <span data-ttu-id="edf04-239">ログ ファイルのサイズに制限はありません。</span><span class="sxs-lookup"><span data-stu-id="edf04-239">There's no limit on log file size.</span></span> <span data-ttu-id="edf04-240">debug ログは、アプリ起動時の問題のトラブルシューティングにのみ使ってください。</span><span class="sxs-lookup"><span data-stu-id="edf04-240">Only use debug logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="edf04-241">起動後の ASP.NET Core アプリでの一般的なログの場合は、ログ ファイルのサイズを制限し、ログをローテーションするログ ライブラリを使います。</span><span class="sxs-lookup"><span data-stu-id="edf04-241">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="edf04-242">詳しくは、「[サードパーティ製のログ プロバイダー](xref:fundamentals/logging/index#third-party-logging-providers)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="edf04-242">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

::: moniker-end

## <a name="common-startup-errors"></a><span data-ttu-id="edf04-243">起動時の一般的なエラー</span><span class="sxs-lookup"><span data-stu-id="edf04-243">Common startup errors</span></span>

<span data-ttu-id="edf04-244">以下を参照してください。<xref:host-and-deploy/azure-iis-errors-reference></span><span class="sxs-lookup"><span data-stu-id="edf04-244">See <xref:host-and-deploy/azure-iis-errors-reference>.</span></span> <span data-ttu-id="edf04-245">このリファレンス トピックでは、アプリの起動を妨げる一般的な問題のほとんどが説明されています。</span><span class="sxs-lookup"><span data-stu-id="edf04-245">Most of the common problems that prevent app startup are covered in the reference topic.</span></span>

## <a name="slow-or-hanging-app"></a><span data-ttu-id="edf04-246">アプリの速度低下またはハング</span><span class="sxs-lookup"><span data-stu-id="edf04-246">Slow or hanging app</span></span>

<span data-ttu-id="edf04-247">要求に対してアプリの反応が遅い場合、またはハングする場合は、次の記事を参照してください。</span><span class="sxs-lookup"><span data-stu-id="edf04-247">When an app responds slowly or hangs on a request, see the following articles:</span></span>

* [<span data-ttu-id="edf04-248">Azure App Service での Web アプリのパフォーマンス低下に関する問題のトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="edf04-248">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [<span data-ttu-id="edf04-249">Use Crash Diagnoser Site Extension to Capture Dump for Intermittent Exception issues or performance issues on Azure Web App (クラッシュ診断サイト拡張機能を使用して Azure Web App での断続的な例外の問題またはパフォーマンスの問題用のダンプをキャプチャする)</span><span class="sxs-lookup"><span data-stu-id="edf04-249">Use Crash Diagnoser Site Extension to Capture Dump for Intermittent Exception issues or performance issues on Azure Web App</span></span>](https://blogs.msdn.microsoft.com/asiatech/2015/12/28/use-crash-diagnoser-site-extension-to-capture-dump-for-intermittent-exception-issues-or-performance-issues-on-azure-web-app/)

## <a name="remote-debugging"></a><span data-ttu-id="edf04-250">リモート デバッグ</span><span class="sxs-lookup"><span data-stu-id="edf04-250">Remote debugging</span></span>

<span data-ttu-id="edf04-251">次のトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="edf04-251">See the following topics:</span></span>

* <span data-ttu-id="edf04-252">[「Visual Studio を使用した Azure App Service のトラブルシューティング」の「Web アプリのリモート デバッグ」セクション](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) (Azure ドキュメント)</span><span class="sxs-lookup"><span data-stu-id="edf04-252">[Remote debugging web apps section of Troubleshoot a web app in Azure App Service using Visual Studio](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) (Azure documentation)</span></span>
* <span data-ttu-id="edf04-253">[Visual Studio 2017 を使用して Azure 内の IIS 上の ASP.NET Core をリモート デバッグする](/visualstudio/debugger/remote-debugging-azure) (Visual Studio ドキュメント)</span><span class="sxs-lookup"><span data-stu-id="edf04-253">[Remote Debug ASP.NET Core on IIS in Azure in Visual Studio 2017](/visualstudio/debugger/remote-debugging-azure) (Visual Studio documentation)</span></span>

## <a name="application-insights"></a><span data-ttu-id="edf04-254">Application Insights</span><span class="sxs-lookup"><span data-stu-id="edf04-254">Application Insights</span></span>

<span data-ttu-id="edf04-255">[Application Insights](https://azure.microsoft.com/services/application-insights/) は、Azure App Service でホストされているアプリからのテレメトリを提供し、エラー ログやレポート機能を備えています。</span><span class="sxs-lookup"><span data-stu-id="edf04-255">[Application Insights](https://azure.microsoft.com/services/application-insights/) provides telemetry from apps hosted in the Azure App Service, including error logging and reporting features.</span></span> <span data-ttu-id="edf04-256">Application Insights は、アプリのログ機能が使用可能になってアプリが起動した後で発生したエラーのみをレポートできます。</span><span class="sxs-lookup"><span data-stu-id="edf04-256">Application Insights can only report on errors that occur after the app starts when the app's logging features become available.</span></span> <span data-ttu-id="edf04-257">詳しくは、「[Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="edf04-257">For more information, see [Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="monitoring-blades"></a><span data-ttu-id="edf04-258">監視ブレード</span><span class="sxs-lookup"><span data-stu-id="edf04-258">Monitoring blades</span></span>

<span data-ttu-id="edf04-259">監視ブレードでは、このトピックでこれまでに説明した方法に代わるトラブルシューティング エクスペリエンスが提供されます。</span><span class="sxs-lookup"><span data-stu-id="edf04-259">Monitoring blades provide an alternative troubleshooting experience to the methods described earlier in the topic.</span></span> <span data-ttu-id="edf04-260">これらのブレードは、500 番台のエラーの診断に使用できます。</span><span class="sxs-lookup"><span data-stu-id="edf04-260">These blades can be used to diagnose 500-series errors.</span></span>

<span data-ttu-id="edf04-261">ASP.NET Core 拡張機能がインストールされていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="edf04-261">Confirm that the ASP.NET Core Extensions are installed.</span></span> <span data-ttu-id="edf04-262">拡張機能がインストールされていない場合は、手動でインストールします。</span><span class="sxs-lookup"><span data-stu-id="edf04-262">If the extensions aren't installed, install them manually:</span></span>

1. <span data-ttu-id="edf04-263">**[開発ツール]** ブレード セクションで、**[拡張機能]** ブレードを選びます。</span><span class="sxs-lookup"><span data-stu-id="edf04-263">In the **DEVELOPMENT TOOLS** blade section, select the **Extensions** blade.</span></span>
1. <span data-ttu-id="edf04-264">**[ASP.NET Core Extensions]\(ASP.NET Core 拡張機能\)** が一覧に表示されます。</span><span class="sxs-lookup"><span data-stu-id="edf04-264">The **ASP.NET Core Extensions** should appear in the list.</span></span>
1. <span data-ttu-id="edf04-265">拡張機能がインストールされていない場合は、**[追加]** ボタンを選びます。</span><span class="sxs-lookup"><span data-stu-id="edf04-265">If the extensions aren't installed, select the **Add** button.</span></span>
1. <span data-ttu-id="edf04-266">一覧から **[ASP.NET Core Extensions]\(ASP.NET Core 拡張機能\)** を選びます。</span><span class="sxs-lookup"><span data-stu-id="edf04-266">Choose the **ASP.NET Core Extensions** from the list.</span></span>
1. <span data-ttu-id="edf04-267">**[OK]** を選んで法的条項に同意します。</span><span class="sxs-lookup"><span data-stu-id="edf04-267">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="edf04-268">**[拡張機能の追加]** ブレードで **[OK]** を選びます。</span><span class="sxs-lookup"><span data-stu-id="edf04-268">Select **OK** on the **Add extension** blade.</span></span>
1. <span data-ttu-id="edf04-269">拡張機能が正常にインストールされると、情報ポップアップ メッセージで通知されます。</span><span class="sxs-lookup"><span data-stu-id="edf04-269">An informational pop-up message indicates when the extensions are successfully installed.</span></span>

<span data-ttu-id="edf04-270">stdout ログが有効になっていない場合は、次の手順のようにします。</span><span class="sxs-lookup"><span data-stu-id="edf04-270">If stdout logging isn't enabled, follow these steps:</span></span>

1. <span data-ttu-id="edf04-271">Azure portal の **[開発ツール]** 領域で **[高度なツール]** ブレードを選びます。</span><span class="sxs-lookup"><span data-stu-id="edf04-271">In the Azure portal, select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="edf04-272">**[Go&rarr;]** ボタンを選びます。</span><span class="sxs-lookup"><span data-stu-id="edf04-272">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="edf04-273">新しいブラウザー タブまたはウィンドウで Kudu コンソールが開きます。</span><span class="sxs-lookup"><span data-stu-id="edf04-273">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="edf04-274">ページの上部にあるナビゲーション バーを使って **[デバッグ コンソール]** を開き、**[CMD]** を選びます。</span><span class="sxs-lookup"><span data-stu-id="edf04-274">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="edf04-275">パス **site** > **wwwroot** へのフォルダーを開き、下にスクロールして、一覧の最後にある *web.config* ファイルを表示します。</span><span class="sxs-lookup"><span data-stu-id="edf04-275">Open the folders to the path **site** > **wwwroot** and scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="edf04-276">*web.config* ファイルの隣の鉛筆アイコンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="edf04-276">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="edf04-277">**stdoutLogEnabled** を `true` に設定し、**stdoutLogFile** パスを `\\?\%home%\LogFiles\stdout` に変更します。</span><span class="sxs-lookup"><span data-stu-id="edf04-277">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="edf04-278">**[保存]** を選んで、更新した *web.config* ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="edf04-278">Select **Save** to save the updated *web.config* file.</span></span>

<span data-ttu-id="edf04-279">続けて診断ログをアクティブにします。</span><span class="sxs-lookup"><span data-stu-id="edf04-279">Proceed to activate diagnostic logging:</span></span>

1. <span data-ttu-id="edf04-280">Azure portal で **[診断ログ]** ブレードを選びます。</span><span class="sxs-lookup"><span data-stu-id="edf04-280">In the Azure portal, select the **Diagnostics logs** blade.</span></span>
1. <span data-ttu-id="edf04-281">**[アプリケーション ログ (ファイル システム)]** および **[詳細なエラー メッセージ]** の **[オン]** スイッチを選びます。</span><span class="sxs-lookup"><span data-stu-id="edf04-281">Select the **On** switch for **Application Logging (Filesystem)** and **Detailed error messages**.</span></span> <span data-ttu-id="edf04-282">ブレードの上部にある **[保存]** ボタンを選びます。</span><span class="sxs-lookup"><span data-stu-id="edf04-282">Select the **Save** button at the top of the blade.</span></span>
1. <span data-ttu-id="edf04-283">失敗した要求イベントのバッファ処理 (FREB) とも呼ばれる失敗した要求のトレースを含めるには、**[失敗した要求のトレース]** で **[オン]** スイッチを選びます。</span><span class="sxs-lookup"><span data-stu-id="edf04-283">To include failed request tracing, also known as Failed Request Event Buffering (FREB) logging, select the **On** switch for **Failed request tracing**.</span></span> 
1. <span data-ttu-id="edf04-284">ポータルで **[診断ログ]** ブレードのすぐ下にある **[ログ ストリーム]** ブレードを選びます。</span><span class="sxs-lookup"><span data-stu-id="edf04-284">Select the **Log stream** blade, which is listed immediately under the **Diagnostics logs** blade in the portal.</span></span>
1. <span data-ttu-id="edf04-285">アプリに対して要求します。</span><span class="sxs-lookup"><span data-stu-id="edf04-285">Make a request to the app.</span></span>
1. <span data-ttu-id="edf04-286">ログ ストリーム データ内で、エラーの原因が示されます。</span><span class="sxs-lookup"><span data-stu-id="edf04-286">Within the log stream data, the cause of the error is indicated.</span></span>

<span data-ttu-id="edf04-287">トラブルシューティングが完了したら、stdout ログを無効にしてください。</span><span class="sxs-lookup"><span data-stu-id="edf04-287">Be sure to disable stdout logging when troubleshooting is complete.</span></span> <span data-ttu-id="edf04-288">「[ASP.NET Core モジュールの stdout ログ](#aspnet-core-module-stdout-log)」セクションの説明をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="edf04-288">See the instructions in the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log) section.</span></span>

<span data-ttu-id="edf04-289">失敗した要求のトレース ログ (FREB ログ) を見るには:</span><span class="sxs-lookup"><span data-stu-id="edf04-289">To view the failed request tracing logs (FREB logs):</span></span>

1. <span data-ttu-id="edf04-290">Azure portal で **[問題の診断と解決]** ブレードに移動します。</span><span class="sxs-lookup"><span data-stu-id="edf04-290">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="edf04-291">サイド バーの **[SUPPORT TOOLS]\(サポート ツール\)** 領域で、**[Failed Request Tracing Logs]\(失敗した要求のトレース ログ\)** を選びます。</span><span class="sxs-lookup"><span data-stu-id="edf04-291">Select **Failed Request Tracing Logs** from the **SUPPORT TOOLS** area of the sidebar.</span></span>

<span data-ttu-id="edf04-292">詳しくは、[「Azure App Service の Web アプリの診断ログの有効化」トピックの「失敗した要求トレース」セクション](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces)および[「Azure での Web アプリのアプリケーション パフォーマンスに関するよくあるご質問」の「失敗した要求トレースをオンにするにはどうすればよいですか?」](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing)をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="edf04-292">See [Failed request traces section of the Enable diagnostics logging for web apps in Azure App Service topic](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) and the [Application performance FAQs for Web Apps in Azure: How do I turn on failed request tracing?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) for more information.</span></span>

<span data-ttu-id="edf04-293">詳しくは、「[Azure App Service の Web アプリの診断ログの有効化](/azure/app-service/web-sites-enable-diagnostic-log)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="edf04-293">For more information, see [Enable diagnostics logging for web apps in Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log).</span></span>

> [!WARNING]
> <span data-ttu-id="edf04-294">stdout ログを無効にしないと、アプリまたはサーバーで障害が発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="edf04-294">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="edf04-295">ログ ファイルのサイズまたは作成されるログ ファイルの数に制限はありません。</span><span class="sxs-lookup"><span data-stu-id="edf04-295">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="edf04-296">ASP.NET Core アプリでのルーチン ログの場合は、ログ ファイルのサイズを制限し、ログをローテーションするログ ライブラリを使います。</span><span class="sxs-lookup"><span data-stu-id="edf04-296">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="edf04-297">詳細については、「[サードパーティ製のログ プロバイダー](xref:fundamentals/logging/index#third-party-logging-providers)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="edf04-297">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="edf04-298">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="edf04-298">Additional resources</span></span>

* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [<span data-ttu-id="edf04-299">Visual Studio を使用した Azure App Service での Web アプリのトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="edf04-299">Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [<span data-ttu-id="edf04-300">Azure Web Apps での "502 bad gateway" および "503 service unavailable" の HTTP エラーのトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="edf04-300">Troubleshoot HTTP errors of "502 bad gateway" and "503 service unavailable" in your Azure web apps</span></span>](/azure/app-service/app-service-web-troubleshoot-http-502-http-503)
* [<span data-ttu-id="edf04-301">Azure App Service での Web アプリのパフォーマンス低下に関する問題のトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="edf04-301">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [<span data-ttu-id="edf04-302">Azure での Web アプリのアプリケーションパフォーマンスに関するよくあるご質問</span><span class="sxs-lookup"><span data-stu-id="edf04-302">Application performance FAQs for Web Apps in Azure</span></span>](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* [<span data-ttu-id="edf04-303">Azure Web アプリのサンドボックス (App Service ランタイムの実行の制限)</span><span class="sxs-lookup"><span data-stu-id="edf04-303">Azure Web App sandbox (App Service runtime execution limitations)</span></span>](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)
* [<span data-ttu-id="edf04-304">Azure Friday: Azure App Service の診断とトラブルシューティング (12 分間のビデオ)</span><span class="sxs-lookup"><span data-stu-id="edf04-304">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
