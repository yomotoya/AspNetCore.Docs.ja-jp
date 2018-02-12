---
title: "IIS で ASP.NET Core をトラブルシューティングします。"
author: guardrex
description: "ASP.NET Core アプリケーションのインターネット インフォメーション サービス (IIS) の展開に関する問題を診断する方法を説明します。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: 65173e0101a17c64f4cde583e5bbb9fb0a9c7718
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/11/2018
---
# <a name="troubleshoot-aspnet-core-on-iis"></a><span data-ttu-id="bc8f8-103">IIS で ASP.NET Core をトラブルシューティングします。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-103">Troubleshoot ASP.NET Core on IIS</span></span>

<span data-ttu-id="bc8f8-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="bc8f8-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="bc8f8-105">この記事では手順 ASP.NET Core を診断する方法のアプリ起動時の問題でホストする場合[インターネット インフォメーション サービス (IIS)](/iis)です。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-105">This article provides instructions on how to diagnose an ASP.NET Core app startup issue when hosting with [Internet Information Services (IIS)](/iis).</span></span> <span data-ttu-id="bc8f8-106">この記事の情報は、Windows Server および Windows デスクトップ上の IIS でホストに適用されます。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-106">The information in this article applies to hosting in IIS on Windows Server and Windows Desktop.</span></span>

<span data-ttu-id="bc8f8-107">ASP.NET Core プロジェクトは、Visual Studio で、 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)デバッグ中にホストします。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-107">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="bc8f8-108">A *502.5 プロセス エラー* troubleshooted このトピックで紹介するヒントを使用してデバッグすることができるローカルで発生します。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-108">A *502.5 Process Failure* that occurs when debugging locally can be troubleshooted using the advice in this topic.</span></span>

<span data-ttu-id="bc8f8-109">関連するトラブルシューティング トピック:</span><span class="sxs-lookup"><span data-stu-id="bc8f8-109">Additional troubleshooting topics:</span></span>

[<span data-ttu-id="bc8f8-110">Azure App Service での ASP.NET Core のトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="bc8f8-110">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)  
<span data-ttu-id="bc8f8-111">App Service を使用しますが、 [ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)し、IIS ホストのアプリを App Service に固有の方法については、専用のトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-111">Although App Service uses the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and IIS to host apps, see the dedicated topic for instructions specific to App Service.</span></span>

[<span data-ttu-id="bc8f8-112">エラー処理</span><span class="sxs-lookup"><span data-stu-id="bc8f8-112">Error handling</span></span>](xref:fundamentals/error-handling)  
<span data-ttu-id="bc8f8-113">ローカル システムでの開発時に ASP.NET Core アプリケーションでエラーを処理する方法を検出します。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-113">Discover how to handle errors in ASP.NET Core apps during development on a local system.</span></span>

[<span data-ttu-id="bc8f8-114">Visual Studio を使用したデバッグについて理解する</span><span class="sxs-lookup"><span data-stu-id="bc8f8-114">Learn to debug using Visual Studio</span></span>](/visualstudio/debugger/getting-started-with-the-debugger)  
<span data-ttu-id="bc8f8-115">このトピックでは、Visual Studio デバッガーの機能を紹介します。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-115">This topic introduces the features of the Visual Studio debugger.</span></span>

## <a name="app-startup-errors"></a><span data-ttu-id="bc8f8-116">アプリの起動エラー</span><span class="sxs-lookup"><span data-stu-id="bc8f8-116">App startup errors</span></span>

<span data-ttu-id="bc8f8-117">**502.5 プロセス障害**</span><span class="sxs-lookup"><span data-stu-id="bc8f8-117">**502.5 Process Failure**</span></span>  
<span data-ttu-id="bc8f8-118">ワーカー プロセスは失敗します。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-118">The worker process fails.</span></span> <span data-ttu-id="bc8f8-119">アプリが起動しません。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-119">The app doesn't start.</span></span>

<span data-ttu-id="bc8f8-120">ASP.NET Core モジュールは、ワーカー プロセスを開始しようとしていますが、開始に失敗しました。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-120">The ASP.NET Core Module attempts to start the worker process but it fails to start.</span></span> <span data-ttu-id="bc8f8-121">プロセスの起動エラーの原因は、内のエントリから通常決定できます、[アプリケーション イベント ログ](#application-event-log)と[ASP.NET Core モジュール stdout ログ](#aspnet-core-module-stdout-log)です。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-121">The cause of a process startup failure can usually be determined from entries in the [Application Event Log](#application-event-log) and the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log).</span></span>

<span data-ttu-id="bc8f8-122">*502.5 プロセス エラー*をホストしているか、アプリ構成が正しくないと、ワーカー プロセスが失敗する場合、エラー ページが返されます。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-122">The *502.5 Process Failure* error page is returned when a hosting or app misconfiguration causes the worker process to fail:</span></span>

![ブラウザー ウィンドウ 502.5 プロセスのエラー ページの表示](troubleshoot/_static/process-failure-page.png)

<span data-ttu-id="bc8f8-124">**500 内部サーバー エラー**</span><span class="sxs-lookup"><span data-stu-id="bc8f8-124">**500 Internal Server Error**</span></span>  
<span data-ttu-id="bc8f8-125">アプリが起動、エラーが発生して、サーバー要求を満たすことができます。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-125">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="bc8f8-126">このエラーは、スタートアップ時または応答の作成中に、アプリのコード内で発生します。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-126">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="bc8f8-127">応答にコンテンツを含んでいない可能性がありますか、応答が表示されます、 *500 Internal Server Error*ブラウザーにします。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-127">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="bc8f8-128">アプリケーション イベント ログは、通常、アプリが正常に開始されたことを示しています。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-128">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="bc8f8-129">サーバーの観点からは正しい動作です。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-129">From the server's perspective, that's correct.</span></span> <span data-ttu-id="bc8f8-130">アプリが起動しますが、有効な応答を生成できません。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-130">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="bc8f8-131">[コマンド プロンプトで、アプリを実行](#run-the-app-at-a-command-prompt)サーバー上または[ASP.NET コア モジュールの標準出力ログを有効にする](#aspnet-core-module-stdout-log)問題をトラブルシューティングします。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-131">[Run the app at a command prompt](#run-the-app-at-a-command-prompt) on the server or [enable the ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log) to troubleshoot the problem.</span></span>

<span data-ttu-id="bc8f8-132">**接続のリセット**</span><span class="sxs-lookup"><span data-stu-id="bc8f8-132">**Connection reset**</span></span>

<span data-ttu-id="bc8f8-133">サーバーが送信するためには遅すぎるはヘッダーが送信された後にエラーが発生した場合、 **500 Internal Server Error**エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-133">If an error occurs after the headers are sent, it's too late for the server to send a **500 Internal Server Error** when an error occurs.</span></span> <span data-ttu-id="bc8f8-134">これは多くの場合、複合オブジェクトの応答のシリアル化中にエラーが発生したときに発生します。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-134">This often happens when an error occurs during the serialization of complex objects for a response.</span></span> <span data-ttu-id="bc8f8-135">としてこの種類のエラーが表示されます、*接続のリセット*クライアントでエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-135">This type of error appears as a *connection reset* error on the client.</span></span> <span data-ttu-id="bc8f8-136">[アプリケーションのログ記録](xref:fundamentals/logging/index)この種のエラーのトラブルシューティングに役立つことができます。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-136">[Application logging](xref:fundamentals/logging/index) can help troubleshoot these types of errors.</span></span>

## <a name="default-startup-limits"></a><span data-ttu-id="bc8f8-137">既定のスタートアップの制限</span><span class="sxs-lookup"><span data-stu-id="bc8f8-137">Default startup limits</span></span>

<span data-ttu-id="bc8f8-138">ASP.NET Core モジュールが、既定値で構成されている*startupTimeLimit* 120 秒です。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-138">The ASP.NET Core Module is configured with a default *startupTimeLimit* of 120 seconds.</span></span> <span data-ttu-id="bc8f8-139">既定値のままにするとアプリ、モジュール、プロセスのエラーをログに記録する前に開始に最大 2 分かかる場合があります。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-139">When left at the default value, an app may take up to two minutes to start before the module logs a process failure.</span></span> <span data-ttu-id="bc8f8-140">モジュールを構成する方法の詳細については、次を参照してください。 [aspNetCore 要素の属性](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element)です。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-140">For information on configuring the module, see [Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

## <a name="troubleshoot-app-startup-errors"></a><span data-ttu-id="bc8f8-141">アプリの起動エラーをトラブルシューティングします。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-141">Troubleshoot app startup errors</span></span>

### <a name="application-event-log"></a><span data-ttu-id="bc8f8-142">アプリケーション イベント ログ</span><span class="sxs-lookup"><span data-stu-id="bc8f8-142">Application Event Log</span></span>

<span data-ttu-id="bc8f8-143">アプリケーション イベント ログにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-143">Access the Application Event Log:</span></span>

1. <span data-ttu-id="bc8f8-144">[スタート] メニューを開き、検索**イベント ビューアー**、クリックして、**イベント ビューアー**アプリ。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-144">Open the Start menu, search for **Event Viewer**, and then select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="bc8f8-145">**イベント ビューアー**を開き、 **Windows ログ**ノード。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-145">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="bc8f8-146">選択**アプリケーション**をアプリケーション イベント ログを開きます。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-146">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="bc8f8-147">問題のあるアプリに関連付けられたエラーを検索します。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-147">Search for errors associated with the failing app.</span></span> <span data-ttu-id="bc8f8-148">エラーの値を持つ*IIS AspNetCore モジュール*または*IIS Express AspNetCore モジュール*で、*ソース*列です。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-148">Errors have a value of *IIS AspNetCore Module* or *IIS Express AspNetCore Module* in the *Source* column.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="bc8f8-149">コマンド プロンプトで、アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-149">Run the app at a command prompt</span></span>

<span data-ttu-id="bc8f8-150">多くのスタートアップ エラーは、アプリケーション イベント ログでの有用な情報を得られない。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-150">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="bc8f8-151">ホスト システムで、コマンド プロンプトで、アプリを実行していくつかのエラーの原因を見つけることができます。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-151">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span>

<span data-ttu-id="bc8f8-152">**フレームワークに依存する展開**</span><span class="sxs-lookup"><span data-stu-id="bc8f8-152">**Framework-dependent deployment**</span></span>

<span data-ttu-id="bc8f8-153">アプリの場合、[フレームワークに依存する展開](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span><span class="sxs-lookup"><span data-stu-id="bc8f8-153">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

1. <span data-ttu-id="bc8f8-154">コマンド プロンプトでの展開フォルダに移動しを持つアプリのアセンブリを実行することによって、アプリを実行*dotnet.exe*です。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-154">At a command prompt, navigate to the deployment folder and run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="bc8f8-155">次のコマンドでのアプリのアセンブリの名前に置き換えます\<アセンブリ名 >:`dotnet .\<assembly_name>.dll`です。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-155">In the following command, substitute the name of the app's assembly for \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span></span>
1. <span data-ttu-id="bc8f8-156">コンソール アプリから、すべてのエラーを示す出力がコンソール ウィンドウに書き込まれます。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-156">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="bc8f8-157">アプリへの要求を行うときにエラーが発生した場合は、ホストと Kestrel がリッスンするポートに要求します。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-157">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="bc8f8-158">既定のホストと post を使用して要求を行う`http://localhost:5000/`です。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-158">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="bc8f8-159">応答した場合、アプリ通常 Kestrel エンドポイント アドレスで、問題がより多く関連のリバース プロキシの構成、およびアプリ内可能性が低い。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-159">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the reverse proxy configuration and less likely within the app.</span></span>

<span data-ttu-id="bc8f8-160">**自己完結型の配置**</span><span class="sxs-lookup"><span data-stu-id="bc8f8-160">**Self-contained deployment**</span></span>

<span data-ttu-id="bc8f8-161">アプリの場合、[自己完結型の配置](/dotnet/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="bc8f8-161">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

1. <span data-ttu-id="bc8f8-162">コマンド プロンプトでの展開フォルダに移動し、アプリの実行可能ファイルを実行します。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-162">At a command prompt, navigate to the deployment folder and run the app's executable.</span></span> <span data-ttu-id="bc8f8-163">次のコマンドでのアプリのアセンブリの名前に置き換えます\<アセンブリ名 >:`<assembly_name>.exe`です。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-163">In the following command, substitute the name of the app's assembly for \<assembly_name>: `<assembly_name>.exe`.</span></span>
1. <span data-ttu-id="bc8f8-164">コンソール アプリから、すべてのエラーを示す出力がコンソール ウィンドウに書き込まれます。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-164">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="bc8f8-165">アプリへの要求を行うときにエラーが発生した場合は、ホストと Kestrel がリッスンするポートに要求します。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-165">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="bc8f8-166">既定のホストと post を使用して要求を行う`http://localhost:5000/`です。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-166">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="bc8f8-167">応答した場合、アプリ通常 Kestrel エンドポイント アドレスで、問題がより多く関連のリバース プロキシの構成、およびアプリ内可能性が低い。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-167">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the reverse proxy configuration and less likely within the app.</span></span>

### <a name="aspnet-core-module-stdout-log"></a><span data-ttu-id="bc8f8-168">ASP.NET Core モジュール stdout ログ</span><span class="sxs-lookup"><span data-stu-id="bc8f8-168">ASP.NET Core Module stdout log</span></span>

<span data-ttu-id="bc8f8-169">有効にし、標準出力ログを表示します。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-169">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="bc8f8-170">ホスト システムに、サイトの展開フォルダに移動します。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-170">Navigate to the site's deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="bc8f8-171">場合、*ログ*フォルダーが存在しない、フォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-171">If the *logs* folder isn't present, create the folder.</span></span> <span data-ttu-id="bc8f8-172">MSBuild を有効にする方法については、作成する、*ログ*展開内のフォルダーは自動的を参照してください、[ディレクトリ構造](xref:host-and-deploy/directory-structure)トピックです。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-172">For instructions on how to enable MSBuild to create the *logs* folder in the deployment automatically, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>
1. <span data-ttu-id="bc8f8-173">編集、 *web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-173">Edit the *web.config* file.</span></span> <span data-ttu-id="bc8f8-174">設定**stdoutLogEnabled**に`true`を変更して、 **stdoutLogFile**を指すパス、*ログ*フォルダー (たとえば、 `.\logs\stdout`)。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-174">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to point to the *logs* folder (for example, `.\logs\stdout`).</span></span> <span data-ttu-id="bc8f8-175">`stdout`パスは、ログ ファイル名のプレフィックスです。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-175">`stdout` in the path is the log file name prefix.</span></span> <span data-ttu-id="bc8f8-176">ログが作成されるときは、タイムスタンプ、プロセス id、およびファイルの拡張子が自動的に追加します。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-176">A timestamp, process id, and file extension are added automatically when the log is created.</span></span> <span data-ttu-id="bc8f8-177">使用して`stdout`一般的なログ ファイルの名前は、ファイル名のプレフィックスとして*stdout_20180205184032_5412.log*です。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-177">Using `stdout` as the file name prefix, a typical log file is named *stdout_20180205184032_5412.log*.</span></span> 
1. <span data-ttu-id="bc8f8-178">更新された保存*web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-178">Save the updated *web.config* file.</span></span>
1. <span data-ttu-id="bc8f8-179">アプリへの要求を作成します。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-179">Make a request to the app.</span></span>
1. <span data-ttu-id="bc8f8-180">移動し、*ログ*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-180">Navigate to the *logs* folder.</span></span> <span data-ttu-id="bc8f8-181">検索して、最新の標準出力ログを開きます。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-181">Find and open the most recent stdout log.</span></span>
1. <span data-ttu-id="bc8f8-182">エラー ログを調査します。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-182">Study the log for errors.</span></span>

<span data-ttu-id="bc8f8-183">**大事な！**</span><span class="sxs-lookup"><span data-stu-id="bc8f8-183">**Important!**</span></span> <span data-ttu-id="bc8f8-184">標準出力ログのトラブルシューティングが完了したら無効にします。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-184">Disable stdout logging when troubleshooting is complete.</span></span>

1. <span data-ttu-id="bc8f8-185">編集、 *web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-185">Edit the *web.config* file.</span></span>
1. <span data-ttu-id="bc8f8-186">設定**stdoutLogEnabled**に`false`です。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-186">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="bc8f8-187">ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-187">Save the file.</span></span>

> [!WARNING]
> <span data-ttu-id="bc8f8-188">エラーを標準出力ログを無効にするは、アプリケーションまたはサーバーの障害につながります。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-188">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="bc8f8-189">ログ ファイルのサイズに制限はありませんか、作成されるログ ファイルの数があります。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-189">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="bc8f8-190">ASP.NET Core アプリケーションの日常的なログ記録、ログ ファイルのサイズを制限し、ログを回転するログ記録ライブラリを使用します。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-190">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="bc8f8-191">詳細については、次を参照してください。[サード パーティ製のログ記録プロバイダー](xref:fundamentals/logging/index#third-party-logging-providers)です。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-191">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="enabling-the-developer-exception-page"></a><span data-ttu-id="bc8f8-192">開発者の例外のページを有効にします。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-192">Enabling the Developer Exception Page</span></span>

<span data-ttu-id="bc8f8-193">`ASPNETCORE_ENVIRONMENT` [環境変数は web.config に追加できる](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)開発環境でアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-193">The `ASPNETCORE_ENVIRONMENT` [environment variable can be added to web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) to run the app in the Development environment.</span></span> <span data-ttu-id="bc8f8-194">環境はありませんでアプリのスタートアップでオーバーライドされた場合に限り`UseEnvironment`ホスト ビルダーで、環境変数を設定することができます、[開発者例外ページ](xref:fundamentals/error-handling)に表示されるときに、アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-194">As long as the environment isn't overridden in app startup by `UseEnvironment` on the host builder, setting the environment variable allows the [Developer Exception Page](xref:fundamentals/error-handling) to appear when the app is run.</span></span>

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

<span data-ttu-id="bc8f8-195">環境変数を設定`ASPNETCORE_ENVIRONMENT`ステージングと、インターネットに公開されないサーバーのテストで使用するためのみお勧めします。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-195">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` is only recommended for use on staging and testing servers that aren't exposed to the Internet.</span></span> <span data-ttu-id="bc8f8-196">環境変数からの削除、 *web.config*トラブルシューティング後のファイルです。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-196">Remove the environment variable from the *web.config* file after troubleshooting.</span></span> <span data-ttu-id="bc8f8-197">環境変数の設定の詳細について*web.config*を参照してください[aspNetCore の子要素を environmentVariables](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)です。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-197">For information on setting environment variables in *web.config*, see [environmentVariables child element of aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span></span>

## <a name="common-startup-errors"></a><span data-ttu-id="bc8f8-198">起動の一般的なエラー</span><span class="sxs-lookup"><span data-stu-id="bc8f8-198">Common startup errors</span></span> 

<span data-ttu-id="bc8f8-199">参照してください、 [ASP.NET Core の一般的なエラーのリファレンス](xref:host-and-deploy/azure-iis-errors-reference)です。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-199">See the [ASP.NET Core common errors reference](xref:host-and-deploy/azure-iis-errors-reference).</span></span> <span data-ttu-id="bc8f8-200">ほとんどのアプリの起動を妨げる一般的な問題は、リファレンス トピックについて説明します。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-200">Most of the common problems that prevent app startup are covered in the reference topic.</span></span>

## <a name="slow-or-hanging-app"></a><span data-ttu-id="bc8f8-201">速度低下またはハングしているアプリ</span><span class="sxs-lookup"><span data-stu-id="bc8f8-201">Slow or hanging app</span></span>

<span data-ttu-id="bc8f8-202">アプリは、緩やかに変化が応答または要求でハング、ときに取得し、分析、[ダンプ ファイル](/visualstudio/debugger/using-dump-files)です。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-202">When an app responds slowly or hangs on a request, obtain and analyze a [dump file](/visualstudio/debugger/using-dump-files).</span></span> <span data-ttu-id="bc8f8-203">ダンプ ファイルは、次のツールを使用して取得できます。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-203">Dump files can be obtained using any of the following tools:</span></span>

* [<span data-ttu-id="bc8f8-204">ProcDump</span><span class="sxs-lookup"><span data-stu-id="bc8f8-204">ProcDump</span></span>](/sysinternals/downloads/procdump)
* [<span data-ttu-id="bc8f8-205">DebugDiag</span><span class="sxs-lookup"><span data-stu-id="bc8f8-205">DebugDiag</span></span>](https://www.microsoft.com/download/details.aspx?id=49924)
* <span data-ttu-id="bc8f8-206">WinDbg: [Windows 用デバッグ ツールをダウンロード](https://developer.microsoft.com/windows/hardware/download-windbg)、[を使用して WinDbg のデバッグ](/windows-hardware/drivers/debugger/debugging-using-windbg)</span><span class="sxs-lookup"><span data-stu-id="bc8f8-206">WinDbg: [Download Debugging tools for Windows](https://developer.microsoft.com/windows/hardware/download-windbg), [Debugging Using WinDbg](/windows-hardware/drivers/debugger/debugging-using-windbg)</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="bc8f8-207">リモート デバッグ</span><span class="sxs-lookup"><span data-stu-id="bc8f8-207">Remote debugging</span></span>

<span data-ttu-id="bc8f8-208">参照してください[リモート デバッグ ASP.NET Core の IIS のリモート コンピューターに Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) Visual Studio のマニュアルでします。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-208">See [Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) in the Visual Studio documentation.</span></span>

## <a name="application-insights"></a><span data-ttu-id="bc8f8-209">Application Insights</span><span class="sxs-lookup"><span data-stu-id="bc8f8-209">Application Insights</span></span>

<span data-ttu-id="bc8f8-210">[Application Insights](/azure/application-insights/)エラー ログとレポート機能を含む、IIS でホストされているアプリから製品利用統計情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-210">[Application Insights](/azure/application-insights/) provides telemetry from apps hosted by IIS, including error logging and reporting features.</span></span> <span data-ttu-id="bc8f8-211">Application Insights は、アプリケーションのログ記録機能が使用可能になるときに、アプリの起動後に発生したエラーに関するレポートのみできます。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-211">Application Insights can only report on errors that occur after the app starts when the app's logging features become available.</span></span> <span data-ttu-id="bc8f8-212">詳細については、次を参照してください。 [ASP.NET Core の Application Insights](/azure/application-insights/app-insights-asp-net-core)です。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-212">For more information, see [Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="additional-troubleshooting-advice"></a><span data-ttu-id="bc8f8-213">追加のトラブルシューティングのアドバイス</span><span class="sxs-lookup"><span data-stu-id="bc8f8-213">Additional troubleshooting advice</span></span>

<span data-ttu-id="bc8f8-214">機能しているアプリが失敗する、開発コンピューターまたはパッケージのバージョンで、アプリ内でいずれか、.NET Core SDK をアップグレードした後すぐにします。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-214">Sometimes a functioning app fails immediately after upgrading either the .NET Core SDK on the development machine or package versions within the app.</span></span> <span data-ttu-id="bc8f8-215">場合によっては、パッケージに統一性がないと、メジャー アップグレード実行時にアプリが破壊されることがあります。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-215">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="bc8f8-216">これらの問題のほとんどは、次の手順に従って修正できます。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-216">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="bc8f8-217">削除、 *bin*と*obj*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-217">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="bc8f8-218">パッケージをキャッシュするチェック ボックスをオフ*%userprofile%\\.nuget\\パッケージ*と*%localappdata%\\Nuget\\v3 キャッシュ*です。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-218">Clear the package caches at *%UserProfile%\\.nuget\\packages* and *%LocalAppData%\\Nuget\\v3-cache*.</span></span>
1. <span data-ttu-id="bc8f8-219">復元し、プロジェクトをリビルドします。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-219">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="bc8f8-220">アプリを再展開する前に、サーバー上の以前の展開が完全に削除されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-220">Confirm that the prior deployment on the server has been completely deleted prior to redeploying the app.</span></span>

> [!TIP]
> <span data-ttu-id="bc8f8-221">パッケージのキャッシュを消去する便利な方法は、実行する`dotnet nuget locals all --clear`コマンド プロンプトからです。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-221">A convenient way to clear package caches is to execute `dotnet nuget locals all --clear` from a command prompt.</span></span>
> 
> <span data-ttu-id="bc8f8-222">パッケージのキャッシュをクリアする行うこともできますを使用して、 [nuget.exe](https://www.nuget.org/downloads)ツールとコマンドを実行する`nuget locals all -clear`です。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-222">Clearing package caches can also be accomplished by using the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="bc8f8-223">*nuget.exe* Windows デスクトップ オペレーティング システムにバンドルされている、インストールされていないしから個別に取得する必要があります、 [NuGet の web サイト](https://www.nuget.org/downloads)です。</span><span class="sxs-lookup"><span data-stu-id="bc8f8-223">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bc8f8-224">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="bc8f8-224">Additional resources</span></span>

* [<span data-ttu-id="bc8f8-225">ASP.NET Core でのエラー処理の概要</span><span class="sxs-lookup"><span data-stu-id="bc8f8-225">Introduction to Error Handling in ASP.NET Core</span></span>](xref:fundamentals/error-handling)
* [<span data-ttu-id="bc8f8-226">Azure App Service および IIS と ASP.NET Core の一般的なエラーのリファレンス</span><span class="sxs-lookup"><span data-stu-id="bc8f8-226">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)
* [<span data-ttu-id="bc8f8-227">ASP.NET Core モジュール構成リファレンス</span><span class="sxs-lookup"><span data-stu-id="bc8f8-227">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="bc8f8-228">Azure App Service での ASP.NET Core のトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="bc8f8-228">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)
