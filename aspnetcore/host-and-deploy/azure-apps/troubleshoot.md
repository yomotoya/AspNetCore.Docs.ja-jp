---
title: "Azure App Service の ASP.NET Core をトラブルシューティングします。"
author: guardrex
description: "ASP.NET Core Azure App Service の配置に関する問題を診断する方法を学習します。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/31/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-apps/troubleshoot
ms.openlocfilehash: 144af8e93bb935d07fd064d5f45b40faea4a2664
ms.sourcegitcommit: 7a87d66cf1d01febe6635c7306f2f679434901d1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/03/2018
---
# <a name="troubleshoot-aspnet-core-on-azure-app-service"></a><span data-ttu-id="80ed2-103">Azure App Service の ASP.NET Core をトラブルシューティングします。</span><span class="sxs-lookup"><span data-stu-id="80ed2-103">Troubleshoot ASP.NET Core on Azure App Service</span></span>

<span data-ttu-id="80ed2-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="80ed2-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="80ed2-105">この記事は、Azure App Service の診断ツールを使用してアプリの起動の問題、ASP.NET Core を診断する方法の手順を説明します。</span><span class="sxs-lookup"><span data-stu-id="80ed2-105">This article provides instructions on how to diagnose an ASP.NET Core app startup issue using Azure App Service's diagnostic tools.</span></span> <span data-ttu-id="80ed2-106">他のトラブルシューティング アドバイスについては、次を参照してください。 [Azure App Service の診断の概要](/azure/app-service/app-service-diagnostics)と[する方法: Azure App Service でアプリの監視](/azure/app-service/web-sites-monitor)、Azure ドキュメントでします。</span><span class="sxs-lookup"><span data-stu-id="80ed2-106">For additional troubleshooting advice, see [Azure App Service diagnostics overview](/azure/app-service/app-service-diagnostics) and [How to: Monitor Apps in Azure App Service](/azure/app-service/web-sites-monitor) in the Azure documentation.</span></span>

## <a name="app-startup-errors"></a><span data-ttu-id="80ed2-107">アプリの起動エラー</span><span class="sxs-lookup"><span data-stu-id="80ed2-107">App startup errors</span></span>

<span data-ttu-id="80ed2-108">**502.5 プロセス障害**</span><span class="sxs-lookup"><span data-stu-id="80ed2-108">**502.5 Process Failure**</span></span>  
<span data-ttu-id="80ed2-109">ワーカー プロセスは失敗します。</span><span class="sxs-lookup"><span data-stu-id="80ed2-109">The worker process fails.</span></span> <span data-ttu-id="80ed2-110">アプリが起動しません。</span><span class="sxs-lookup"><span data-stu-id="80ed2-110">The app doesn't start.</span></span>

<span data-ttu-id="80ed2-111">[ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)ワーカー プロセスを開始する試行の開始に失敗します。</span><span class="sxs-lookup"><span data-stu-id="80ed2-111">The [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) attempts to start the worker process but it fails to start.</span></span> <span data-ttu-id="80ed2-112">多くの場合、アプリケーション イベント ログを調べることは、この種の問題のトラブルシューティングに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="80ed2-112">Examining the Application Event Log often helps troubleshoot this type of problem.</span></span> <span data-ttu-id="80ed2-113">」で説明されて、ログへのアクセス、[アプリケーション イベント ログ](#application-event-log)セクションです。</span><span class="sxs-lookup"><span data-stu-id="80ed2-113">Accessing the log is explained in the [Application Event Log](#application-event-log) section.</span></span>

<span data-ttu-id="80ed2-114">*502.5 プロセス エラー*正しく構成されていないアプリにより、ワーカー プロセスが失敗する場合、エラー ページが返されます。</span><span class="sxs-lookup"><span data-stu-id="80ed2-114">The *502.5 Process Failure* error page is returned when a misconfigured app causes the worker process to fail:</span></span>

![ブラウザー ウィンドウ 502.5 プロセスのエラー ページの表示](troubleshoot/_static/process-failure-page.png)

<span data-ttu-id="80ed2-116">**500 内部サーバー エラー**</span><span class="sxs-lookup"><span data-stu-id="80ed2-116">**500 Internal Server Error**</span></span>  
<span data-ttu-id="80ed2-117">アプリが起動、エラーが発生して、サーバー要求を満たすことができます。</span><span class="sxs-lookup"><span data-stu-id="80ed2-117">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="80ed2-118">このエラーは、スタートアップ時または応答の作成中に、アプリのコード内で発生します。</span><span class="sxs-lookup"><span data-stu-id="80ed2-118">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="80ed2-119">応答にコンテンツを含んでいない可能性がありますか、応答が表示されます、 *500 Internal Server Error*ブラウザーにします。</span><span class="sxs-lookup"><span data-stu-id="80ed2-119">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="80ed2-120">アプリケーション イベント ログは、通常、アプリが正常に開始されたことを示しています。</span><span class="sxs-lookup"><span data-stu-id="80ed2-120">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="80ed2-121">サーバーの観点からは正しい動作です。</span><span class="sxs-lookup"><span data-stu-id="80ed2-121">From the server's perspective, that's correct.</span></span> <span data-ttu-id="80ed2-122">アプリが起動しますが、有効な応答を生成できません。</span><span class="sxs-lookup"><span data-stu-id="80ed2-122">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="80ed2-123">[Kudu コンソールで、アプリを実行](#run-the-app-in-the-kudu-console)または[ASP.NET コア モジュールの標準出力ログを有効にする](#aspnet-core-module-stdout-log)して問題をトラブルシューティングします。</span><span class="sxs-lookup"><span data-stu-id="80ed2-123">[Run the app in the Kudu console](#run-the-app-in-the-kudu-console) or [enable the ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log) to troubleshoot the problem.</span></span>

## <a name="troubleshoot-app-startup-errors"></a><span data-ttu-id="80ed2-124">アプリの起動エラーをトラブルシューティングします。</span><span class="sxs-lookup"><span data-stu-id="80ed2-124">Troubleshoot app startup errors</span></span>

### <a name="application-event-log"></a><span data-ttu-id="80ed2-125">アプリケーション イベント ログ</span><span class="sxs-lookup"><span data-stu-id="80ed2-125">Application Event Log</span></span>

<span data-ttu-id="80ed2-126">アプリケーション イベント ログにアクセスするには、使用、**診断し、問題を解決して**ブレードで、Azure ポータルで。</span><span class="sxs-lookup"><span data-stu-id="80ed2-126">To access the Application Event Log, use the **Diagnose and solve problems** blade in the Azure portal :</span></span>

1. <span data-ttu-id="80ed2-127">Azure ポータルでのアプリのブレードを開く、 **App Services**ブレードです。</span><span class="sxs-lookup"><span data-stu-id="80ed2-127">In the Azure portal, open the app's blade in the **App Services** blade.</span></span>
1. <span data-ttu-id="80ed2-128">選択、**診断し、問題を解決して**ブレードです。</span><span class="sxs-lookup"><span data-stu-id="80ed2-128">Select the **Diagnose and solve problems** blade.</span></span>
1. <span data-ttu-id="80ed2-129">**問題カテゴリの選択**、select、 **Web アプリを**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="80ed2-129">Under **SELECT PROBLEM CATEGORY**, select the **Web App Down** button.</span></span>
1. <span data-ttu-id="80ed2-130">**推奨されるソリューション**のウィンドウを開く**アプリケーション イベント ログを開く**です。</span><span class="sxs-lookup"><span data-stu-id="80ed2-130">Under **Suggested Solutions**, open the pane for **Open Application Event Logs**.</span></span> <span data-ttu-id="80ed2-131">選択、**アプリケーション イベント ログを開く**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="80ed2-131">Select the **Open Application Event Logs** button.</span></span>
1. <span data-ttu-id="80ed2-132">によって提供される最新のエラーを調べて、 *IIS AspNetCoreModule*で、**ソース**列です。</span><span class="sxs-lookup"><span data-stu-id="80ed2-132">Examine the latest error provided by the *IIS AspNetCoreModule* in the **Source** column.</span></span>

<span data-ttu-id="80ed2-133">使用する代わりに、**診断し、問題を解決して**を使用して直接アプリケーション イベント ログ ファイルを調べるには、ブレード[Kudu](https://github.com/projectkudu/kudu/wiki):</span><span class="sxs-lookup"><span data-stu-id="80ed2-133">An alternative to using the **Diagnose and solve problems** blade is to examine the Application Event Log file directly using [Kudu](https://github.com/projectkudu/kudu/wiki):</span></span>

1. <span data-ttu-id="80ed2-134">選択、**高度なツール**ブレードに、**開発ツール**領域。</span><span class="sxs-lookup"><span data-stu-id="80ed2-134">Select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="80ed2-135">選択、**移動&rarr;**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="80ed2-135">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="80ed2-136">Kudu コンソールは、新しいブラウザー タブまたはウィンドウで開きます。</span><span class="sxs-lookup"><span data-stu-id="80ed2-136">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="80ed2-137">ページの上部にあるナビゲーション バーを使用して開きます**デバッグ コンソール**選択**CMD**です。</span><span class="sxs-lookup"><span data-stu-id="80ed2-137">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="80ed2-138">開く、 **LogFiles**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="80ed2-138">Open the **LogFiles** folder.</span></span>
1. <span data-ttu-id="80ed2-139">の横にある鉛筆アイコンを選択、 *eventlog.xml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="80ed2-139">Select the pencil icon next to the *eventlog.xml* file.</span></span>
1. <span data-ttu-id="80ed2-140">ログを調べます。</span><span class="sxs-lookup"><span data-stu-id="80ed2-140">Examine the log.</span></span> <span data-ttu-id="80ed2-141">最新のイベントのログの末尾までスクロールします。</span><span class="sxs-lookup"><span data-stu-id="80ed2-141">Scroll to the bottom of the log to see the most recent events.</span></span>

### <a name="run-the-app-in-the-kudu-console"></a><span data-ttu-id="80ed2-142">Kudu コンソールで、アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="80ed2-142">Run the app in the Kudu console</span></span>

<span data-ttu-id="80ed2-143">多くのスタートアップ エラーは、アプリケーション イベント ログでの有用な情報を得られない。</span><span class="sxs-lookup"><span data-stu-id="80ed2-143">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="80ed2-144">アプリを実行することができます、 [Kudu](https://github.com/projectkudu/kudu/wiki)エラーを検出するリモートの実行コンソール。</span><span class="sxs-lookup"><span data-stu-id="80ed2-144">You can run the app in the [Kudu](https://github.com/projectkudu/kudu/wiki) Remote Execution Console to discover the error:</span></span>

1. <span data-ttu-id="80ed2-145">選択、**高度なツール**ブレードに、**開発ツール**領域。</span><span class="sxs-lookup"><span data-stu-id="80ed2-145">Select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="80ed2-146">選択、**移動&rarr;**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="80ed2-146">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="80ed2-147">Kudu コンソールは、新しいブラウザー タブまたはウィンドウで開きます。</span><span class="sxs-lookup"><span data-stu-id="80ed2-147">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="80ed2-148">ページの上部にあるナビゲーション バーを使用して開きます**デバッグ コンソール**選択**CMD**です。</span><span class="sxs-lookup"><span data-stu-id="80ed2-148">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="80ed2-149">パスのフォルダーを開いて**サイト** > **wwwroot**です。</span><span class="sxs-lookup"><span data-stu-id="80ed2-149">Open the folders to the path **site** > **wwwroot**.</span></span>
1. <span data-ttu-id="80ed2-150">コンソールで、アプリのアセンブリを実行することによって、アプリを実行*dotnet.exe*です。</span><span class="sxs-lookup"><span data-stu-id="80ed2-150">In the console, run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="80ed2-151">次のコマンドでのアプリのアセンブリの名前に置き換えます`<assembly_name>`:</span><span class="sxs-lookup"><span data-stu-id="80ed2-151">In the following command, substitute the name of the app's assembly for `<assembly_name>`:</span></span>
   ```console
   dotnet .\<assembly_name>.dll
   ```
1. <span data-ttu-id="80ed2-152">コンソール アプリから、すべてのエラーを示す出力は、Kudu コンソールにパイプします。</span><span class="sxs-lookup"><span data-stu-id="80ed2-152">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

### <a name="aspnet-core-module-stdout-log"></a><span data-ttu-id="80ed2-153">ASP.NET Core モジュール stdout ログ</span><span class="sxs-lookup"><span data-stu-id="80ed2-153">ASP.NET Core Module stdout log</span></span>

<span data-ttu-id="80ed2-154">ASP.NET Core モジュールは、標準出力ログは、多くの場合、有用なエラー メッセージがアプリケーション イベント ログが見つかりません。 を記録します。</span><span class="sxs-lookup"><span data-stu-id="80ed2-154">The ASP.NET Core Module stdout log often records useful error messages not found in the Application Event Log.</span></span> <span data-ttu-id="80ed2-155">有効にし、標準出力ログを表示します。</span><span class="sxs-lookup"><span data-stu-id="80ed2-155">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="80ed2-156">移動し、**診断し、問題を解決して**ブレードで、Azure ポータルでします。</span><span class="sxs-lookup"><span data-stu-id="80ed2-156">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="80ed2-157">**問題カテゴリの選択**、select、 **Web アプリを**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="80ed2-157">Under **SELECT PROBLEM CATEGORY**, select the **Web App Down** button.</span></span>
1. <span data-ttu-id="80ed2-158">**推奨されるソリューション** > **標準出力ログのリダイレクトを有効にする**、ボタンをクリックして**Web.Config を編集する Kudu コンソールを開く**です。</span><span class="sxs-lookup"><span data-stu-id="80ed2-158">Under **Suggested Solutions** > **Enable Stdout Log Redirection**, select the button to **Open Kudu Console to edit Web.Config**.</span></span>
1. <span data-ttu-id="80ed2-159">Kudu**診断コンソール**、パスにフォルダーを開く**サイト** > **wwwroot**です。</span><span class="sxs-lookup"><span data-stu-id="80ed2-159">In the Kudu **Diagnostic Console**, open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="80ed2-160">公開まで下にスクロール、 *web.config*一覧の下部にあるファイルです。</span><span class="sxs-lookup"><span data-stu-id="80ed2-160">Scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="80ed2-161">次に、鉛筆アイコンをクリックして、 *web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="80ed2-161">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="80ed2-162">設定**stdoutLogEnabled**に`true`を変更して、 **stdoutLogFile**へのパス:`\\?\%home%\LogFiles\stdout`です。</span><span class="sxs-lookup"><span data-stu-id="80ed2-162">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="80ed2-163">選択**保存**を保存、更新された*web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="80ed2-163">Select **Save** to save the updated *web.config* file.</span></span>
1. <span data-ttu-id="80ed2-164">アプリへの要求を作成します。</span><span class="sxs-lookup"><span data-stu-id="80ed2-164">Make a request to the app.</span></span>
1. <span data-ttu-id="80ed2-165">Azure ポータルに戻ります。</span><span class="sxs-lookup"><span data-stu-id="80ed2-165">Return to the Azure portal.</span></span> <span data-ttu-id="80ed2-166">選択、**高度なツール**ブレードに、**開発ツール**領域。</span><span class="sxs-lookup"><span data-stu-id="80ed2-166">Select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="80ed2-167">選択、**移動&rarr;**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="80ed2-167">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="80ed2-168">Kudu コンソールは、新しいブラウザー タブまたはウィンドウで開きます。</span><span class="sxs-lookup"><span data-stu-id="80ed2-168">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="80ed2-169">ページの上部にあるナビゲーション バーを使用して開きます**デバッグ コンソール**選択**CMD**です。</span><span class="sxs-lookup"><span data-stu-id="80ed2-169">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="80ed2-170">選択、 **LogFiles**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="80ed2-170">Select the **LogFiles** folder.</span></span>
1. <span data-ttu-id="80ed2-171">検査、 **Modified**最新の変更日にログに記録列と、標準出力を編集する鉛筆アイコンを選択します。</span><span class="sxs-lookup"><span data-stu-id="80ed2-171">Inspect the **Modified** column and select the pencil icon to edit the stdout log with the latest modification date.</span></span>
1. <span data-ttu-id="80ed2-172">ログ ファイルが開き、エラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="80ed2-172">When the log file opens, the error is displayed.</span></span>

<span data-ttu-id="80ed2-173">**大事な！**</span><span class="sxs-lookup"><span data-stu-id="80ed2-173">**Important!**</span></span> <span data-ttu-id="80ed2-174">標準出力ログのトラブルシューティングが完了したら無効にします。</span><span class="sxs-lookup"><span data-stu-id="80ed2-174">Disable stdout logging when troubleshooting is complete.</span></span>

1. <span data-ttu-id="80ed2-175">Kudu**診断コンソール**パスに戻り、**サイト** > **wwwroot**を表示する、 *web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="80ed2-175">In the Kudu **Diagnostic Console**, return to the path **site** > **wwwroot** to reveal the *web.config* file.</span></span> <span data-ttu-id="80ed2-176">開く、 **web.config**鉛筆アイコンを選択してもう一度ファイル。</span><span class="sxs-lookup"><span data-stu-id="80ed2-176">Open the **web.config** file again by selecting the pencil icon.</span></span>
1. <span data-ttu-id="80ed2-177">設定**stdoutLogEnabled**に`false`です。</span><span class="sxs-lookup"><span data-stu-id="80ed2-177">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="80ed2-178">選択**保存**ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="80ed2-178">Select **Save** to save the file.</span></span>

> [!WARNING]
> <span data-ttu-id="80ed2-179">エラーを標準出力ログを無効にするは、アプリケーションまたはサーバーの障害につながります。</span><span class="sxs-lookup"><span data-stu-id="80ed2-179">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="80ed2-180">ログ ファイルのサイズに制限はありませんか、作成されるログ ファイルの数があります。</span><span class="sxs-lookup"><span data-stu-id="80ed2-180">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="80ed2-181">ASP.NET Core アプリケーションの日常的なログ記録、ログ ファイルのサイズを制限し、ログを回転するログ記録ライブラリを使用します。</span><span class="sxs-lookup"><span data-stu-id="80ed2-181">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="80ed2-182">詳細については、次を参照してください。[サード パーティ製のログ記録プロバイダー](xref:fundamentals/logging/index#third-party-logging-providers)です。</span><span class="sxs-lookup"><span data-stu-id="80ed2-182">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="common-startup-errors"></a><span data-ttu-id="80ed2-183">起動の一般的なエラー</span><span class="sxs-lookup"><span data-stu-id="80ed2-183">Common startup errors</span></span> 

<span data-ttu-id="80ed2-184">参照してください、 [ASP.NET Core の一般的なエラーのリファレンス](xref:host-and-deploy/azure-iis-errors-reference)です。</span><span class="sxs-lookup"><span data-stu-id="80ed2-184">See the [ASP.NET Core common errors reference](xref:host-and-deploy/azure-iis-errors-reference).</span></span> <span data-ttu-id="80ed2-185">ほとんどのアプリの起動を妨げる一般的な問題は、リファレンス トピックについて説明します。</span><span class="sxs-lookup"><span data-stu-id="80ed2-185">Most of the common problems that prevent app startup are covered in the reference topic.</span></span>

## <a name="process-dump-for-a-slow-or-hanging-app"></a><span data-ttu-id="80ed2-186">アプリの速度低下またはハングしているプロセスのダンプ</span><span class="sxs-lookup"><span data-stu-id="80ed2-186">Process dump for a slow or hanging app</span></span>

<span data-ttu-id="80ed2-187">参照するアプリでは、応答が遅くなると、要求でハング、 [Azure App Service のトラブルシューティング低速の web アプリケーション パフォーマンスの問題](/azure/app-service/app-service-web-troubleshoot-performance-degradation)のガイダンスをデバッグします。</span><span class="sxs-lookup"><span data-stu-id="80ed2-187">When an app responds slowly or hangs on a request, see [Troubleshoot slow web app performance issues in Azure App Service](/azure/app-service/app-service-web-troubleshoot-performance-degradation) for debugging guidance.</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="80ed2-188">リモート デバッグ</span><span class="sxs-lookup"><span data-stu-id="80ed2-188">Remote debugging</span></span>

<span data-ttu-id="80ed2-189">参照してください[リモート デバッグのトラブルシューティング: Visual Studio を使用して Azure App service web アプリの web アプリ セクション](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug)Azure ドキュメントでします。</span><span class="sxs-lookup"><span data-stu-id="80ed2-189">See [Remote debugging web apps section of Troubleshoot a web app in Azure App Service using Visual Studio](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) in the Azure documentation.</span></span>

## <a name="application-insights"></a><span data-ttu-id="80ed2-190">Application Insights</span><span class="sxs-lookup"><span data-stu-id="80ed2-190">Application Insights</span></span>

<span data-ttu-id="80ed2-191">[Application Insights](https://azure.microsoft.com/services/application-insights/)エラー ログとレポート機能を含む、Azure App Service でホストされているアプリから製品利用統計情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="80ed2-191">[Application Insights](https://azure.microsoft.com/services/application-insights/) provides telemetry from apps hosted in the Azure App Service, including error logging and reporting features.</span></span> <span data-ttu-id="80ed2-192">Application Insights は、アプリケーションのログ記録機能が使用可能になるときに、アプリの起動後に発生したエラーに関するレポートのみできます。</span><span class="sxs-lookup"><span data-stu-id="80ed2-192">Application Insights can only report on errors that occur after the app starts when the app's logging features become available.</span></span> <span data-ttu-id="80ed2-193">詳細については、次を参照してください。 [ASP.NET Core の Application Insights](/azure/application-insights/app-insights-asp-net-core)です。</span><span class="sxs-lookup"><span data-stu-id="80ed2-193">For more information, see [Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="monitoring-blades"></a><span data-ttu-id="80ed2-194">ブレードの監視</span><span class="sxs-lookup"><span data-stu-id="80ed2-194">Monitoring blades</span></span>

<span data-ttu-id="80ed2-195">監視ブレードでは、このトピックで説明した方法にエクスペリエンスをトラブルシューティング代替手段を提供します。</span><span class="sxs-lookup"><span data-stu-id="80ed2-195">Monitoring blades provide an alternative troubleshooting experience to the methods described earlier in the topic.</span></span> <span data-ttu-id="80ed2-196">このブレードは、500 番台のエラーの診断に使用できます。</span><span class="sxs-lookup"><span data-stu-id="80ed2-196">These blades can be used to diagnose 500-series errors.</span></span>

<span data-ttu-id="80ed2-197">ASP.NET Core Extensions がインストールされていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="80ed2-197">Confirm that the ASP.NET Core Extensions are installed.</span></span> <span data-ttu-id="80ed2-198">拡張機能がインストールされていない場合は、手動でインストールします。</span><span class="sxs-lookup"><span data-stu-id="80ed2-198">If the extensions aren't installed, install them manually:</span></span>

1. <span data-ttu-id="80ed2-199">**開発ツール**ブレード セクションで、select、**拡張**ブレードです。</span><span class="sxs-lookup"><span data-stu-id="80ed2-199">In the **DEVELOPMENT TOOLS** blade section, select the **Extensions** blade.</span></span>
1. <span data-ttu-id="80ed2-200">**ASP.NET Core Extensions**一覧に表示する必要があります。</span><span class="sxs-lookup"><span data-stu-id="80ed2-200">The **ASP.NET Core Extensions** should appear in the list.</span></span>
1. <span data-ttu-id="80ed2-201">拡張機能がインストールされていない場合は、選択、**追加**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="80ed2-201">If the extensions aren't installed, select the **Add** button.</span></span>
1. <span data-ttu-id="80ed2-202">選択、 **ASP.NET Core Extensions**一覧からです。</span><span class="sxs-lookup"><span data-stu-id="80ed2-202">Choose the **ASP.NET Core Extensions** from the list.</span></span>
1. <span data-ttu-id="80ed2-203">選択**OK**法的条項に同意します。</span><span class="sxs-lookup"><span data-stu-id="80ed2-203">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="80ed2-204">選択**OK**上、**拡張機能を追加**ブレードです。</span><span class="sxs-lookup"><span data-stu-id="80ed2-204">Select **OK** on the **Add extension** blade.</span></span>
1. <span data-ttu-id="80ed2-205">情報のポップアップ メッセージは、拡張機能が正常にインストールされている場合を示します。</span><span class="sxs-lookup"><span data-stu-id="80ed2-205">An informational pop-up message indicates when the extensions are successfully installed.</span></span>

<span data-ttu-id="80ed2-206">Stdout のログ記録が有効でない場合は、次の手順に従います。</span><span class="sxs-lookup"><span data-stu-id="80ed2-206">If stdout logging isn't enabled, follow these steps:</span></span>

1. <span data-ttu-id="80ed2-207">Azure ポータルで、選択、**高度なツール**ブレードに、**開発ツール**領域。</span><span class="sxs-lookup"><span data-stu-id="80ed2-207">In the Azure portal, select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="80ed2-208">選択、**移動&rarr;**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="80ed2-208">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="80ed2-209">Kudu コンソールは、新しいブラウザー タブまたはウィンドウで開きます。</span><span class="sxs-lookup"><span data-stu-id="80ed2-209">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="80ed2-210">ページの上部にあるナビゲーション バーを使用して開きます**デバッグ コンソール**選択**CMD**です。</span><span class="sxs-lookup"><span data-stu-id="80ed2-210">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="80ed2-211">パスのフォルダーを開いて**サイト** > **wwwroot**表示まで下にスクロールし、 *web.config*一覧の下部にあるファイルです。</span><span class="sxs-lookup"><span data-stu-id="80ed2-211">Open the folders to the path **site** > **wwwroot** and scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="80ed2-212">次に、鉛筆アイコンをクリックして、 *web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="80ed2-212">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="80ed2-213">設定**stdoutLogEnabled**に`true`を変更して、 **stdoutLogFile**へのパス:`\\?\%home%\LogFiles\stdout`です。</span><span class="sxs-lookup"><span data-stu-id="80ed2-213">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="80ed2-214">選択**保存**を保存、更新された*web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="80ed2-214">Select **Save** to save the updated *web.config* file.</span></span>

<span data-ttu-id="80ed2-215">診断ログの記録をアクティブ化する手順に従います。</span><span class="sxs-lookup"><span data-stu-id="80ed2-215">Proceed to activate diagnostic logging:</span></span>

1. <span data-ttu-id="80ed2-216">Azure ポータルで、選択、**診断ログ**ブレードです。</span><span class="sxs-lookup"><span data-stu-id="80ed2-216">In the Azure portal, select the **Diagnostics logs** blade.</span></span>
1. <span data-ttu-id="80ed2-217">選択、**で**スイッチを**アプリケーションのログ記録 (ファイル システム)**と**エラー メッセージの詳細な**です。</span><span class="sxs-lookup"><span data-stu-id="80ed2-217">Select the **On** switch for **Application Logging (Filesystem)** and **Detailed error messages**.</span></span> <span data-ttu-id="80ed2-218">選択、**保存**ブレードの上部にあるボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="80ed2-218">Select the **Save** button at the top of the blade.</span></span>
1. <span data-ttu-id="80ed2-219">失敗要求イベントのバッファリング (FREB) ログ記録とも呼ばれる、失敗した要求トレースを含めるには 、**で**の切り替える**失敗した要求トレース**です。</span><span class="sxs-lookup"><span data-stu-id="80ed2-219">To include failed request tracing, also known as Failed Request Event Buffering (FREB) logging, select the **On** switch for **Failed request tracing**.</span></span> 
1. <span data-ttu-id="80ed2-220">選択、**ログ ストリーム**ブレードで、すぐに 下にある、**診断ログ**ポータルのブレードです。</span><span class="sxs-lookup"><span data-stu-id="80ed2-220">Select the **Log stream** blade, which is listed immediately under the **Diagnostics logs** blade in the portal.</span></span>
1. <span data-ttu-id="80ed2-221">アプリへの要求を作成します。</span><span class="sxs-lookup"><span data-stu-id="80ed2-221">Make a request to the app.</span></span>
1. <span data-ttu-id="80ed2-222">ログ ストリーム データ内で、エラーの原因が示されます。</span><span class="sxs-lookup"><span data-stu-id="80ed2-222">Within the log stream data, the cause of the error is indicated.</span></span>

<span data-ttu-id="80ed2-223">**大事な！**</span><span class="sxs-lookup"><span data-stu-id="80ed2-223">**Important!**</span></span> <span data-ttu-id="80ed2-224">標準出力ログのトラブルシューティングが完了したら無効にすることを確認します。</span><span class="sxs-lookup"><span data-stu-id="80ed2-224">Be sure to disable stdout logging when troubleshooting is complete.</span></span> <span data-ttu-id="80ed2-225">手順を参照して、 [ASP.NET Core モジュール stdout ログ](#aspnet-core-module-stdout-log)セクションです。</span><span class="sxs-lookup"><span data-stu-id="80ed2-225">See the instructions in the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log) section.</span></span>

<span data-ttu-id="80ed2-226">失敗した要求トレース ログ (FREB ログ) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="80ed2-226">To view the failed request tracing logs (FREB logs):</span></span>

1. <span data-ttu-id="80ed2-227">移動し、**診断し、問題を解決して**ブレードで、Azure ポータルでします。</span><span class="sxs-lookup"><span data-stu-id="80ed2-227">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="80ed2-228">選択**失敗した要求トレース ログ**から、 **SUPPORT TOOLS**サイド バーの領域。</span><span class="sxs-lookup"><span data-stu-id="80ed2-228">Select **Failed Request Tracing Logs** from the **SUPPORT TOOLS** area of the sidebar.</span></span>

<span data-ttu-id="80ed2-229">参照してください[失敗した要求トレースのセクションのトピックの Azure App Service web アプリの診断ログを有効に](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces)と[Azure で Web アプリのアプリケーションのパフォーマンスに関する Faq: 失敗した要求トレースを有効にする方法ですか?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="80ed2-229">See [Failed request traces section of the Enable diagnostics logging for web apps in Azure App Service topic](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) and the [Application performance FAQs for Web Apps in Azure: How do I turn on failed request tracing?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) for more information.</span></span>

<span data-ttu-id="80ed2-230">詳細については、次を参照してください。 [Azure App service web アプリの診断ログ記録を有効にする](/azure/app-service/web-sites-enable-diagnostic-log)です。</span><span class="sxs-lookup"><span data-stu-id="80ed2-230">For more information, see [Enable diagnostics logging for web apps in Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log).</span></span>

> [!WARNING]
> <span data-ttu-id="80ed2-231">エラーを標準出力ログを無効にするは、アプリケーションまたはサーバーの障害につながります。</span><span class="sxs-lookup"><span data-stu-id="80ed2-231">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="80ed2-232">ログ ファイルのサイズに制限はありませんか、作成されるログ ファイルの数があります。</span><span class="sxs-lookup"><span data-stu-id="80ed2-232">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="80ed2-233">ASP.NET Core アプリケーションの日常的なログ記録、ログ ファイルのサイズを制限し、ログを回転するログ記録ライブラリを使用します。</span><span class="sxs-lookup"><span data-stu-id="80ed2-233">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="80ed2-234">詳細については、次を参照してください。[サード パーティ製のログ記録プロバイダー](xref:fundamentals/logging/index#third-party-logging-providers)です。</span><span class="sxs-lookup"><span data-stu-id="80ed2-234">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="80ed2-235">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="80ed2-235">Additional resources</span></span>

* [<span data-ttu-id="80ed2-236">ASP.NET Core でのエラー処理の概要</span><span class="sxs-lookup"><span data-stu-id="80ed2-236">Introduction to Error Handling in ASP.NET Core</span></span>](xref:fundamentals/error-handling)
* [<span data-ttu-id="80ed2-237">Azure App Service および IIS と ASP.NET Core の一般的なエラーのリファレンス</span><span class="sxs-lookup"><span data-stu-id="80ed2-237">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)
* [<span data-ttu-id="80ed2-238">Visual Studio を使用して Azure App service web アプリをトラブルシューティングします。</span><span class="sxs-lookup"><span data-stu-id="80ed2-238">Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [<span data-ttu-id="80ed2-239">「502 無効なゲートウェイ」と「503 サービス利用不可」では、Azure web アプリの HTTP エラーをトラブルシューティングします。</span><span class="sxs-lookup"><span data-stu-id="80ed2-239">Troubleshoot HTTP errors of "502 bad gateway" and "503 service unavailable" in your Azure web apps</span></span>](/app-service/app-service-web-troubleshoot-http-502-http-503)
* [<span data-ttu-id="80ed2-240">Azure App Service で低速の web アプリのパフォーマンスに関する問題をトラブルシューティングします。</span><span class="sxs-lookup"><span data-stu-id="80ed2-240">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [<span data-ttu-id="80ed2-241">Azure で Web アプリのアプリケーションのパフォーマンスに関する Faq</span><span class="sxs-lookup"><span data-stu-id="80ed2-241">Application performance FAQs for Web Apps in Azure</span></span>](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* [<span data-ttu-id="80ed2-242">Azure Friday: Azure アプリケーション サービスの診断とトラブルシューティング エクスペリエンス (12 分のビデオ)</span><span class="sxs-lookup"><span data-stu-id="80ed2-242">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
