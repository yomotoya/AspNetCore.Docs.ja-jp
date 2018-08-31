---
title: IIS での ASP.NET Core のトラブルシューティング
author: guardrex
description: ASP.NET Core アプリのインターネット インフォメーション サービス (IIS) の展開に関する問題を診断する方法について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2018
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: f22914c9b0d6d1902dd37c9b21b80a18894c97e7
ms.sourcegitcommit: d1c4580f56656b503cf528ec9f5ba570d790b57d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/13/2018
ms.locfileid: "41751644"
---
# <a name="troubleshoot-aspnet-core-on-iis"></a><span data-ttu-id="34460-103">IIS での ASP.NET Core のトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="34460-103">Troubleshoot ASP.NET Core on IIS</span></span>

<span data-ttu-id="34460-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="34460-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="34460-105">この記事では、[インターネット インフォメーション サービス (IIS)](/iis) を使用してホストしている場合に、ASP.NET Core アプリの起動時に関する問題を診断する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="34460-105">This article provides instructions on how to diagnose an ASP.NET Core app startup issue when hosting with [Internet Information Services (IIS)](/iis).</span></span> <span data-ttu-id="34460-106">この記事の情報は、Windows Server および Windows デスクトップ上の IIS でのホスティングに適用されます。</span><span class="sxs-lookup"><span data-stu-id="34460-106">The information in this article applies to hosting in IIS on Windows Server and Windows Desktop.</span></span>

<span data-ttu-id="34460-107">Visual Studio では、ASP.NET Core プロジェクトのデバッグ時に [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) のホスティングが既定の設定です。</span><span class="sxs-lookup"><span data-stu-id="34460-107">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="34460-108">このトピックのアドバイスを参照すると、ローカルでのデバッグ時に発生する *502.5 処理エラー*を解決できます。</span><span class="sxs-lookup"><span data-stu-id="34460-108">A *502.5 Process Failure* that occurs when debugging locally can be troubleshooted using the advice in this topic.</span></span>

<span data-ttu-id="34460-109">その他のトラブルシューティング トピック:</span><span class="sxs-lookup"><span data-stu-id="34460-109">Additional troubleshooting topics:</span></span>

[<span data-ttu-id="34460-110">Azure App Service での ASP.NET Core のトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="34460-110">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)  
<span data-ttu-id="34460-111">App Service は[ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module)と IIS を使用してアプリをホストしますが、App Service 固有の手順については、この専用のトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="34460-111">Although App Service uses the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and IIS to host apps, see the dedicated topic for instructions specific to App Service.</span></span>

[<span data-ttu-id="34460-112">エラーの処理</span><span class="sxs-lookup"><span data-stu-id="34460-112">Handle errors</span></span>](xref:fundamentals/error-handling)  
<span data-ttu-id="34460-113">ローカル システム上で開発しているときに発生する ASP.NET Core アプリのエラーを処理する方法について説明しています。</span><span class="sxs-lookup"><span data-stu-id="34460-113">Discover how to handle errors in ASP.NET Core apps during development on a local system.</span></span>

[<span data-ttu-id="34460-114">Visual Studio を使用したデバッグについて理解する</span><span class="sxs-lookup"><span data-stu-id="34460-114">Learn to debug using Visual Studio</span></span>](/visualstudio/debugger/getting-started-with-the-debugger)  
<span data-ttu-id="34460-115">このトピックでは、Visual Studio デバッガーの機能を紹介しています。</span><span class="sxs-lookup"><span data-stu-id="34460-115">This topic introduces the features of the Visual Studio debugger.</span></span>

[<span data-ttu-id="34460-116">Visual Studio Code でのデバッグ</span><span class="sxs-lookup"><span data-stu-id="34460-116">Debugging with Visual Studio Code</span></span>](https://code.visualstudio.com/docs/editor/debugging)  
<span data-ttu-id="34460-117">Visual Studio Code に組み込まれているデバッグのサポートについて説明します。</span><span class="sxs-lookup"><span data-stu-id="34460-117">Learn about the debugging support built into Visual Studio Code.</span></span>

## <a name="app-startup-errors"></a><span data-ttu-id="34460-118">アプリ起動時のエラー</span><span class="sxs-lookup"><span data-stu-id="34460-118">App startup errors</span></span>

<span data-ttu-id="34460-119">**502.5 処理エラー**</span><span class="sxs-lookup"><span data-stu-id="34460-119">**502.5 Process Failure**</span></span>  
<span data-ttu-id="34460-120">ワーカー プロセスが失敗します。</span><span class="sxs-lookup"><span data-stu-id="34460-120">The worker process fails.</span></span> <span data-ttu-id="34460-121">アプリは起動しません。</span><span class="sxs-lookup"><span data-stu-id="34460-121">The app doesn't start.</span></span>

<span data-ttu-id="34460-122">ASP.NET Core モジュールはワーカー プロセスの開始を試みますが、開始に失敗します。</span><span class="sxs-lookup"><span data-stu-id="34460-122">The ASP.NET Core Module attempts to start the worker process but it fails to start.</span></span> <span data-ttu-id="34460-123">プロセス起動時のエラーの原因は、通常、[アプリケーション イベント ログ](#application-event-log)と [ASP.NET Core モジュールの stdout ログ](#aspnet-core-module-stdout-log)のエントリから判断できます。</span><span class="sxs-lookup"><span data-stu-id="34460-123">The cause of a process startup failure can usually be determined from entries in the [Application Event Log](#application-event-log) and the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log).</span></span>

<span data-ttu-id="34460-124">正しく構成されていないホスティングやアプリが原因でワーカー プロセスが失敗する場合、"*502.5 処理エラー*" のエラー ページが返されます。</span><span class="sxs-lookup"><span data-stu-id="34460-124">The *502.5 Process Failure* error page is returned when a hosting or app misconfiguration causes the worker process to fail:</span></span>

![502.5 処理エラー ページが表示されているブラウザー ウィンドウ](troubleshoot/_static/process-failure-page.png)

<span data-ttu-id="34460-126">**500 内部サーバー エラー**</span><span class="sxs-lookup"><span data-stu-id="34460-126">**500 Internal Server Error**</span></span>  
<span data-ttu-id="34460-127">アプリは起動しますが、エラーのためにサーバーは要求を実行できません。</span><span class="sxs-lookup"><span data-stu-id="34460-127">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="34460-128">このエラーは、起動時または応答の作成中に、アプリのコード内で発生します。</span><span class="sxs-lookup"><span data-stu-id="34460-128">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="34460-129">応答にコンテンツが含まれていないか、またはブラウザーに "*500 内部サーバー エラー*" という応答が表示される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="34460-129">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="34460-130">通常、アプリケーション イベント ログではアプリが正常に起動したことが示されます。</span><span class="sxs-lookup"><span data-stu-id="34460-130">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="34460-131">サーバーから見るとそれは正しいことです。</span><span class="sxs-lookup"><span data-stu-id="34460-131">From the server's perspective, that's correct.</span></span> <span data-ttu-id="34460-132">アプリは起動しましたが、有効な応答を生成できません。</span><span class="sxs-lookup"><span data-stu-id="34460-132">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="34460-133">問題を解決するには、[サーバー上のコマンド プロンプトでアプリを実行する](#run-the-app-at-a-command-prompt)か、[ASP.NET Core モジュールの stdout ログを有効にします](#aspnet-core-module-stdout-log)。</span><span class="sxs-lookup"><span data-stu-id="34460-133">[Run the app at a command prompt](#run-the-app-at-a-command-prompt) on the server or [enable the ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log) to troubleshoot the problem.</span></span>

<span data-ttu-id="34460-134">**接続のリセット**</span><span class="sxs-lookup"><span data-stu-id="34460-134">**Connection reset**</span></span>

<span data-ttu-id="34460-135">ヘッダー送信後にエラーが発生した場合、サーバーが **500 内部サーバー エラー**を送信するには遅すぎます。</span><span class="sxs-lookup"><span data-stu-id="34460-135">If an error occurs after the headers are sent, it's too late for the server to send a **500 Internal Server Error** when an error occurs.</span></span> <span data-ttu-id="34460-136">このような状況は、応答に対する複雑なオブジェクトのシリアル化中にエラーが起きたときによく発生します。</span><span class="sxs-lookup"><span data-stu-id="34460-136">This often happens when an error occurs during the serialization of complex objects for a response.</span></span> <span data-ttu-id="34460-137">この種のエラーは、クライアントでは "*接続リセット*" エラーとして表示されます。</span><span class="sxs-lookup"><span data-stu-id="34460-137">This type of error appears as a *connection reset* error on the client.</span></span> <span data-ttu-id="34460-138">この種のエラーのトラブルシューティングには、[アプリケーション ログ](xref:fundamentals/logging/index)が役に立つことがあります。</span><span class="sxs-lookup"><span data-stu-id="34460-138">[Application logging](xref:fundamentals/logging/index) can help troubleshoot these types of errors.</span></span>

## <a name="default-startup-limits"></a><span data-ttu-id="34460-139">既定の起動制限</span><span class="sxs-lookup"><span data-stu-id="34460-139">Default startup limits</span></span>

<span data-ttu-id="34460-140">ASP.NET Core モジュールの *startupTimeLimit* は、既定では 120 秒に構成されます。</span><span class="sxs-lookup"><span data-stu-id="34460-140">The ASP.NET Core Module is configured with a default *startupTimeLimit* of 120 seconds.</span></span> <span data-ttu-id="34460-141">既定値のままにした場合、モジュールで処理エラーが記録されるまでに、アプリは最大で 2 分を起動にかけることができます。</span><span class="sxs-lookup"><span data-stu-id="34460-141">When left at the default value, an app may take up to two minutes to start before the module logs a process failure.</span></span> <span data-ttu-id="34460-142">モジュールの構成の詳細については、「[AspNetCore 要素の属性](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="34460-142">For information on configuring the module, see [Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

## <a name="troubleshoot-app-startup-errors"></a><span data-ttu-id="34460-143">アプリの起動エラーのトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="34460-143">Troubleshoot app startup errors</span></span>

### <a name="application-event-log"></a><span data-ttu-id="34460-144">アプリケーション イベント ログ</span><span class="sxs-lookup"><span data-stu-id="34460-144">Application Event Log</span></span>

<span data-ttu-id="34460-145">アプリケーション イベント ログにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="34460-145">Access the Application Event Log:</span></span>

1. <span data-ttu-id="34460-146">[スタート] メニューを開き、**イベント ビューアー**を検索し、**イベント ビューアー** アプリを選択します。</span><span class="sxs-lookup"><span data-stu-id="34460-146">Open the Start menu, search for **Event Viewer**, and then select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="34460-147">**イベント ビューアー**で **[Windows ログ]** ノードを開きます。</span><span class="sxs-lookup"><span data-stu-id="34460-147">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="34460-148">**[Application]** を選択して、アプリケーション イベント ログを開きます。</span><span class="sxs-lookup"><span data-stu-id="34460-148">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="34460-149">失敗したアプリに関連するエラーを検索します。</span><span class="sxs-lookup"><span data-stu-id="34460-149">Search for errors associated with the failing app.</span></span> <span data-ttu-id="34460-150">エラーの *[ソース]* 列には *IIS AspNetCore Module* または *IIS Express AspNetCore Module* の値が表示されます。</span><span class="sxs-lookup"><span data-stu-id="34460-150">Errors have a value of *IIS AspNetCore Module* or *IIS Express AspNetCore Module* in the *Source* column.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="34460-151">コマンド プロンプトでアプリを実行する</span><span class="sxs-lookup"><span data-stu-id="34460-151">Run the app at a command prompt</span></span>

<span data-ttu-id="34460-152">多くの起動時エラーでは、アプリケーション イベント ログに役に立つ情報が生成されません。</span><span class="sxs-lookup"><span data-stu-id="34460-152">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="34460-153">ホスティング システムのコマンド プロンプトでアプリを実行すると、エラーの原因がわかることがあります。</span><span class="sxs-lookup"><span data-stu-id="34460-153">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span>

<span data-ttu-id="34460-154">**フレームワークに依存する展開**</span><span class="sxs-lookup"><span data-stu-id="34460-154">**Framework-dependent deployment**</span></span>

<span data-ttu-id="34460-155">アプリが[フレームワークに依存する展開](/dotnet/core/deploying/#framework-dependent-deployments-fdd)の場合:</span><span class="sxs-lookup"><span data-stu-id="34460-155">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

1. <span data-ttu-id="34460-156">コマンド プロンプトで展開フォルダーに移動し、*dotnet.exe* 使用してアプリのアセンブリを実行して、アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="34460-156">At a command prompt, navigate to the deployment folder and run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="34460-157">コマンド `dotnet .\<assembly_name>.dll` の \<assembly_name> にアプリのアセンブリの名前を指定して実行します。</span><span class="sxs-lookup"><span data-stu-id="34460-157">In the following command, substitute the name of the app's assembly for \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span></span>
1. <span data-ttu-id="34460-158">エラーを示すアプリからのコンソール出力は、すべてコンソール ウィンドウに書き込まれます。</span><span class="sxs-lookup"><span data-stu-id="34460-158">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="34460-159">アプリへの要求時にエラーが発生した場合は、Kestrel がリッスンしているホストとポートに要求が送信されます。</span><span class="sxs-lookup"><span data-stu-id="34460-159">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="34460-160">既定のホストと post を使用して `http://localhost:5000/` に要求を行います。</span><span class="sxs-lookup"><span data-stu-id="34460-160">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="34460-161">アプリが Kestrel のエンドポイント アドレスで正常に応答する場合、問題はリバース プロキシ設定に関連している可能性が高く、アプリ内が原因の可能性は低くなります。</span><span class="sxs-lookup"><span data-stu-id="34460-161">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the reverse proxy configuration and less likely within the app.</span></span>

<span data-ttu-id="34460-162">**自己完結型の展開**</span><span class="sxs-lookup"><span data-stu-id="34460-162">**Self-contained deployment**</span></span>

<span data-ttu-id="34460-163">アプリが[自己完結型の展開](/dotnet/core/deploying/#self-contained-deployments-scd)の場合:</span><span class="sxs-lookup"><span data-stu-id="34460-163">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

1. <span data-ttu-id="34460-164">コマンド プロンプトで、展開フォルダーに移動し、アプリの実行可能ファイルを実行します。</span><span class="sxs-lookup"><span data-stu-id="34460-164">At a command prompt, navigate to the deployment folder and run the app's executable.</span></span> <span data-ttu-id="34460-165">コマンド `<assembly_name>.exe` の \<assembly_name> にアプリのアセンブリの名前を指定して実行します。</span><span class="sxs-lookup"><span data-stu-id="34460-165">In the following command, substitute the name of the app's assembly for \<assembly_name>: `<assembly_name>.exe`.</span></span>
1. <span data-ttu-id="34460-166">エラーを示すアプリからのコンソール出力は、すべてコンソール ウィンドウに書き込まれます。</span><span class="sxs-lookup"><span data-stu-id="34460-166">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="34460-167">アプリへの要求時にエラーが発生した場合は、Kestrel がリッスンしているホストとポートに要求が送信されます。</span><span class="sxs-lookup"><span data-stu-id="34460-167">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="34460-168">既定のホストと post を使用して `http://localhost:5000/` に要求を行います。</span><span class="sxs-lookup"><span data-stu-id="34460-168">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="34460-169">アプリが Kestrel のエンドポイント アドレスで正常に応答する場合、問題はリバース プロキシ設定に関連している可能性が高く、アプリ内が原因の可能性は低くなります。</span><span class="sxs-lookup"><span data-stu-id="34460-169">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the reverse proxy configuration and less likely within the app.</span></span>

### <a name="aspnet-core-module-stdout-log"></a><span data-ttu-id="34460-170">ASP.NET Core モジュールの stdout ログ</span><span class="sxs-lookup"><span data-stu-id="34460-170">ASP.NET Core Module stdout log</span></span>

<span data-ttu-id="34460-171">stdout ログを有効にして表示するには:</span><span class="sxs-lookup"><span data-stu-id="34460-171">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="34460-172">ホスティング システム上のサイトの展開フォルダーに移動します。</span><span class="sxs-lookup"><span data-stu-id="34460-172">Navigate to the site's deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="34460-173">*logs* フォルダーが存在しない場合は、フォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="34460-173">If the *logs* folder isn't present, create the folder.</span></span> <span data-ttu-id="34460-174">MSBuild で展開フォルダーに *logs* フォルダーを自動的に作成されるようにする手順については、「[ディレクトリ構造](xref:host-and-deploy/directory-structure)」のトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="34460-174">For instructions on how to enable MSBuild to create the *logs* folder in the deployment automatically, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>
1. <span data-ttu-id="34460-175">*web.config* ファイルを編集します。</span><span class="sxs-lookup"><span data-stu-id="34460-175">Edit the *web.config* file.</span></span> <span data-ttu-id="34460-176">**stdoutLogEnabled** を `true` に設定し、**stdoutLogFile** のパスを *logs* フォルダー (たとえば `.\logs\stdout`) を指すように変更します。</span><span class="sxs-lookup"><span data-stu-id="34460-176">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to point to the *logs* folder (for example, `.\logs\stdout`).</span></span> <span data-ttu-id="34460-177">パスの `stdout` はログ ファイル名のプレフィックスです。</span><span class="sxs-lookup"><span data-stu-id="34460-177">`stdout` in the path is the log file name prefix.</span></span> <span data-ttu-id="34460-178">ログ ファイルの作成時には、タイムスタンプ、プロセス ID、およびファイルの拡張子が自動的に追加されます。</span><span class="sxs-lookup"><span data-stu-id="34460-178">A timestamp, process id, and file extension are added automatically when the log is created.</span></span> <span data-ttu-id="34460-179">ファイル名のプレフィックスとして `stdout` を使用すると、一般的なログ ファイルの名前は *stdout_20180205184032_5412.log* になります。</span><span class="sxs-lookup"><span data-stu-id="34460-179">Using `stdout` as the file name prefix, a typical log file is named *stdout_20180205184032_5412.log*.</span></span> 
1. <span data-ttu-id="34460-180">アプリケーション プールの ID に *logs* フォルダーへの書き込みアクセス許可があることを確認します。</span><span class="sxs-lookup"><span data-stu-id="34460-180">Ensure your application pool's identity has write permissions to the *logs* folder.</span></span>
1. <span data-ttu-id="34460-181">更新した *web.config* ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="34460-181">Save the updated *web.config* file.</span></span>
1. <span data-ttu-id="34460-182">アプリに対して要求します。</span><span class="sxs-lookup"><span data-stu-id="34460-182">Make a request to the app.</span></span>
1. <span data-ttu-id="34460-183">*logs* フォルダーに移動します。</span><span class="sxs-lookup"><span data-stu-id="34460-183">Navigate to the *logs* folder.</span></span> <span data-ttu-id="34460-184">最新の stdout ログを探して開きます。</span><span class="sxs-lookup"><span data-stu-id="34460-184">Find and open the most recent stdout log.</span></span>
1. <span data-ttu-id="34460-185">ログのエラーを調べます。</span><span class="sxs-lookup"><span data-stu-id="34460-185">Study the log for errors.</span></span>

<span data-ttu-id="34460-186">**重要**。</span><span class="sxs-lookup"><span data-stu-id="34460-186">**Important!**</span></span> <span data-ttu-id="34460-187">トラブルシューティングが完了したら、stdout ログを無効にします。</span><span class="sxs-lookup"><span data-stu-id="34460-187">Disable stdout logging when troubleshooting is complete.</span></span>

1. <span data-ttu-id="34460-188">*web.config* ファイルを編集します。</span><span class="sxs-lookup"><span data-stu-id="34460-188">Edit the *web.config* file.</span></span>
1. <span data-ttu-id="34460-189">**stdoutLogEnabled** を `false` に設定します。</span><span class="sxs-lookup"><span data-stu-id="34460-189">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="34460-190">ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="34460-190">Save the file.</span></span>

> [!WARNING]
> <span data-ttu-id="34460-191">stdout ログを無効にしないと、アプリまたはサーバーで障害が発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="34460-191">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="34460-192">ログ ファイルのサイズまたは作成されるログ ファイルの数に制限はありません。</span><span class="sxs-lookup"><span data-stu-id="34460-192">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="34460-193">ASP.NET Core アプリでのルーチン ログの場合は、ログ ファイルのサイズを制限し、ログをローテーションするログ ライブラリを使います。</span><span class="sxs-lookup"><span data-stu-id="34460-193">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="34460-194">詳細については、「[サードパーティ製のログ プロバイダー](xref:fundamentals/logging/index#third-party-logging-providers)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="34460-194">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="enabling-the-developer-exception-page"></a><span data-ttu-id="34460-195">開発者例外ページを有効にする</span><span class="sxs-lookup"><span data-stu-id="34460-195">Enabling the Developer Exception Page</span></span>

<span data-ttu-id="34460-196">開発環境でアプリを実行するには、`ASPNETCORE_ENVIRONMENT` [環境変数を web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) に追加することができます。</span><span class="sxs-lookup"><span data-stu-id="34460-196">The `ASPNETCORE_ENVIRONMENT` [environment variable can be added to web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) to run the app in the Development environment.</span></span> <span data-ttu-id="34460-197">アプリの起動時にホスト ビルダー上で `UseEnvironment` によって環境がオーバーライドされない限り、環境変数を設定すると、アプリの実行時に[開発者例外ページ](xref:fundamentals/error-handling)が表示されます。</span><span class="sxs-lookup"><span data-stu-id="34460-197">As long as the environment isn't overridden in app startup by `UseEnvironment` on the host builder, setting the environment variable allows the [Developer Exception Page](xref:fundamentals/error-handling) to appear when the app is run.</span></span>

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

<span data-ttu-id="34460-198">`ASPNETCORE_ENVIRONMENT` の環境変数を設定する方法は、インターネットに公開されないステージング サーバーやテスト用サーバーの場合にのみお勧めされます。</span><span class="sxs-lookup"><span data-stu-id="34460-198">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` is only recommended for use on staging and testing servers that aren't exposed to the Internet.</span></span> <span data-ttu-id="34460-199">トラブルシューティング後は、必ず *web.config* ファイルからこの環境変数を削除してください。</span><span class="sxs-lookup"><span data-stu-id="34460-199">Remove the environment variable from the *web.config* file after troubleshooting.</span></span> <span data-ttu-id="34460-200">*web.config* で環境変数を設定する方法の詳細については、[aspNetCore の environmentVariables 子要素](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="34460-200">For information on setting environment variables in *web.config*, see [environmentVariables child element of aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span></span>

## <a name="common-startup-errors"></a><span data-ttu-id="34460-201">起動時の一般的なエラー</span><span class="sxs-lookup"><span data-stu-id="34460-201">Common startup errors</span></span> 

<span data-ttu-id="34460-202">[ASP.NET Core の一般的なエラーのリファレンス](xref:host-and-deploy/azure-iis-errors-reference)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="34460-202">See the [ASP.NET Core common errors reference](xref:host-and-deploy/azure-iis-errors-reference).</span></span> <span data-ttu-id="34460-203">このリファレンス トピックでは、アプリの起動を妨げる一般的な問題のほとんどが説明されています。</span><span class="sxs-lookup"><span data-stu-id="34460-203">Most of the common problems that prevent app startup are covered in the reference topic.</span></span>

## <a name="slow-or-hanging-app"></a><span data-ttu-id="34460-204">アプリの速度低下またはハング</span><span class="sxs-lookup"><span data-stu-id="34460-204">Slow or hanging app</span></span>

<span data-ttu-id="34460-205">要求に対してアプリの反応が遅い場合、またはハングする場合は、[ダンプ ファイル](/visualstudio/debugger/using-dump-files)を取得して分析します。</span><span class="sxs-lookup"><span data-stu-id="34460-205">When an app responds slowly or hangs on a request, obtain and analyze a [dump file](/visualstudio/debugger/using-dump-files).</span></span> <span data-ttu-id="34460-206">ダンプ ファイルは、次のいずれかのツールを使用して取得できます。</span><span class="sxs-lookup"><span data-stu-id="34460-206">Dump files can be obtained using any of the following tools:</span></span>

* [<span data-ttu-id="34460-207">ProcDump</span><span class="sxs-lookup"><span data-stu-id="34460-207">ProcDump</span></span>](/sysinternals/downloads/procdump)
* [<span data-ttu-id="34460-208">DebugDiag</span><span class="sxs-lookup"><span data-stu-id="34460-208">DebugDiag</span></span>](https://www.microsoft.com/download/details.aspx?id=49924)
* <span data-ttu-id="34460-209">WinDbg: 「[Download Debugging tools for Windows](https://developer.microsoft.com/windows/hardware/download-windbg)」(Windows 用デバッグ ツールのダウンロード)、「[Debugging Using WinDbg](/windows-hardware/drivers/debugger/debugging-using-windbg)」(WinDbg を使用したデバッグ)</span><span class="sxs-lookup"><span data-stu-id="34460-209">WinDbg: [Download Debugging tools for Windows](https://developer.microsoft.com/windows/hardware/download-windbg), [Debugging Using WinDbg](/windows-hardware/drivers/debugger/debugging-using-windbg)</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="34460-210">リモート デバッグ</span><span class="sxs-lookup"><span data-stu-id="34460-210">Remote debugging</span></span>

<span data-ttu-id="34460-211">Visual Studio ドキュメントの「[Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer)」(Visual Studio 2017 でリモート IIS コンピューター上の ASP.NET Core をリモート デバッグする) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="34460-211">See [Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) in the Visual Studio documentation.</span></span>

## <a name="application-insights"></a><span data-ttu-id="34460-212">Application Insights</span><span class="sxs-lookup"><span data-stu-id="34460-212">Application Insights</span></span>

<span data-ttu-id="34460-213">[Application Insights](/azure/application-insights/) は、IIS でホストされているアプリからのテレメトリを提供し、エラー ログやレポート機能を備えています。</span><span class="sxs-lookup"><span data-stu-id="34460-213">[Application Insights](/azure/application-insights/) provides telemetry from apps hosted by IIS, including error logging and reporting features.</span></span> <span data-ttu-id="34460-214">Application Insights は、アプリのログ機能が使用可能になってアプリが起動した後で発生したエラーのみをレポートできます。</span><span class="sxs-lookup"><span data-stu-id="34460-214">Application Insights can only report on errors that occur after the app starts when the app's logging features become available.</span></span> <span data-ttu-id="34460-215">詳細については、「[Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="34460-215">For more information, see [Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="additional-troubleshooting-advice"></a><span data-ttu-id="34460-216">その他のトラブルシューティングのアドバイス</span><span class="sxs-lookup"><span data-stu-id="34460-216">Additional troubleshooting advice</span></span>

<span data-ttu-id="34460-217">開発コンピューター上の .NET Core SDK またはアプリ内のパッケージ バージョンのいずれかをアップグレードした直後に、機能しているアプリが失敗することがあります。</span><span class="sxs-lookup"><span data-stu-id="34460-217">Sometimes a functioning app fails immediately after upgrading either the .NET Core SDK on the development machine or package versions within the app.</span></span> <span data-ttu-id="34460-218">場合によっては、パッケージに統一性がないと、メジャー アップグレード実行時にアプリが破壊されることがあります。</span><span class="sxs-lookup"><span data-stu-id="34460-218">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="34460-219">これらの問題のほとんどは、次の手順で解決できます。</span><span class="sxs-lookup"><span data-stu-id="34460-219">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="34460-220">*bin* フォルダーと *obj* フォルダーを削除します。</span><span class="sxs-lookup"><span data-stu-id="34460-220">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="34460-221">*%UserProfile%\\.nuget\\packages* と *%LocalAppData%\\Nuget\\v3-cache* のパッケージ キャッシュをクリアします。</span><span class="sxs-lookup"><span data-stu-id="34460-221">Clear the package caches at *%UserProfile%\\.nuget\\packages* and *%LocalAppData%\\Nuget\\v3-cache*.</span></span>
1. <span data-ttu-id="34460-222">プロジェクトを復元してリビルドします。</span><span class="sxs-lookup"><span data-stu-id="34460-222">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="34460-223">アプリを再展開する前に、サーバー上の以前の展開が完全に削除されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="34460-223">Confirm that the prior deployment on the server has been completely deleted prior to redeploying the app.</span></span>

> [!TIP]
> <span data-ttu-id="34460-224">パッケージ キャッシュをクリアするには、コマンド プロンプトから `dotnet nuget locals all --clear` を実行する方法が便利です。</span><span class="sxs-lookup"><span data-stu-id="34460-224">A convenient way to clear package caches is to execute `dotnet nuget locals all --clear` from a command prompt.</span></span>
> 
> <span data-ttu-id="34460-225">パッケージ キャッシュをクリアするには、[nuget.exe](https://www.nuget.org/downloads) ツールを使用して `nuget locals all -clear` コマンドを実行する方法もあります。</span><span class="sxs-lookup"><span data-stu-id="34460-225">Clearing package caches can also be accomplished by using the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="34460-226">*nuget.exe* は、Windows デスクトップ オペレーティング システムにバンドルされているインストールではなく、[NuGet Web サイト](https://www.nuget.org/downloads)から別に入手する必要があります。</span><span class="sxs-lookup"><span data-stu-id="34460-226">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="34460-227">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="34460-227">Additional resources</span></span>

* [<span data-ttu-id="34460-228">ASP.NET Core でのエラー処理の概要</span><span class="sxs-lookup"><span data-stu-id="34460-228">Introduction to Error Handling in ASP.NET Core</span></span>](xref:fundamentals/error-handling)
* [<span data-ttu-id="34460-229">Azure App Service および IIS と ASP.NET Core の一般的なエラーのリファレンス</span><span class="sxs-lookup"><span data-stu-id="34460-229">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)
* [<span data-ttu-id="34460-230">ASP.NET Core モジュール構成リファレンス</span><span class="sxs-lookup"><span data-stu-id="34460-230">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="34460-231">Azure App Service での ASP.NET Core のトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="34460-231">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)
