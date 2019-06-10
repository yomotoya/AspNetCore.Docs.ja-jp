---
title: Azure App Service での ASP.NET Core のトラブルシューティング
author: guardrex
description: ASP.NET Core Azure App Service の配置に関する問題を診断する方法を学習します。
ms.author: riande
ms.custom: mvc
ms.date: 03/06/2019
uid: host-and-deploy/azure-apps/troubleshoot
ms.openlocfilehash: 7a0bb7df27ebbea0eac79771452295846fad563a
ms.sourcegitcommit: a04eb20e81243930ec829a9db5dd5de49f669450
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/03/2019
ms.locfileid: "66470444"
---
# <a name="troubleshoot-aspnet-core-on-azure-app-service"></a><span data-ttu-id="cfc19-103">Azure App Service での ASP.NET Core のトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="cfc19-103">Troubleshoot ASP.NET Core on Azure App Service</span></span>

<span data-ttu-id="cfc19-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="cfc19-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE [Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

<span data-ttu-id="cfc19-105">この記事では、Azure App Service の診断ツールを使って ASP.NET Core アプリの起動時の問題を診断する方法の手順を説明します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-105">This article provides instructions on how to diagnose an ASP.NET Core app startup issue using Azure App Service's diagnostic tools.</span></span> <span data-ttu-id="cfc19-106">追加のトラブルシューティングのアドバイスについては、Azure ドキュメントの「[Azure App Service 診断の概要](/azure/app-service/app-service-diagnostics)」と「[方法: Azure App Service のアプリの監視](/azure/app-service/web-sites-monitor)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="cfc19-106">For additional troubleshooting advice, see [Azure App Service diagnostics overview](/azure/app-service/app-service-diagnostics) and [How to: Monitor Apps in Azure App Service](/azure/app-service/web-sites-monitor) in the Azure documentation.</span></span>

## <a name="app-startup-errors"></a><span data-ttu-id="cfc19-107">アプリ起動時のエラー</span><span class="sxs-lookup"><span data-stu-id="cfc19-107">App startup errors</span></span>

<span data-ttu-id="cfc19-108">**502.5 処理エラー** ワーカー プロセスが失敗します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-108">**502.5 Process Failure** The worker process fails.</span></span> <span data-ttu-id="cfc19-109">アプリは起動しません。</span><span class="sxs-lookup"><span data-stu-id="cfc19-109">The app doesn't start.</span></span>

<span data-ttu-id="cfc19-110">[ASP.NET Core モジュール](xref:host-and-deploy/aspnet-core-module)はワーカー プロセスの開始を試みますが、開始に失敗します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-110">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the worker process but it fails to start.</span></span> <span data-ttu-id="cfc19-111">この種の問題のトラブルシューティングには、アプリケーション イベント ログを調べると役に立つことがよくあります。</span><span class="sxs-lookup"><span data-stu-id="cfc19-111">Examining the Application Event Log often helps troubleshoot this type of problem.</span></span> <span data-ttu-id="cfc19-112">ログへのアクセスについては、「[アプリケーション イベント ログ](#application-event-log)」セクションで説明します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-112">Accessing the log is explained in the [Application Event Log](#application-event-log) section.</span></span>

<span data-ttu-id="cfc19-113">正しく構成されていないアプリによりワーカー プロセスが失敗する場合、"*502.5 処理エラー*" のエラー ページが返されます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-113">The *502.5 Process Failure* error page is returned when a misconfigured app causes the worker process to fail:</span></span>

![502.5 処理エラー ページが表示されているブラウザー ウィンドウ](troubleshoot/_static/process-failure-page.png)

<span data-ttu-id="cfc19-115">**500 内部サーバー エラー**</span><span class="sxs-lookup"><span data-stu-id="cfc19-115">**500 Internal Server Error**</span></span>

<span data-ttu-id="cfc19-116">アプリは起動しますが、エラーのためにサーバーは要求を実行できません。</span><span class="sxs-lookup"><span data-stu-id="cfc19-116">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="cfc19-117">このエラーは、起動時または応答の作成中に、アプリのコード内で発生します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-117">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="cfc19-118">応答にコンテンツが含まれていないか、またはブラウザーに "*500 内部サーバー エラー*" という応答が表示される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="cfc19-118">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="cfc19-119">通常、アプリケーション イベント ログではアプリが正常に起動したことが示されます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-119">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="cfc19-120">サーバーから見るとそれは正しいことです。</span><span class="sxs-lookup"><span data-stu-id="cfc19-120">From the server's perspective, that's correct.</span></span> <span data-ttu-id="cfc19-121">アプリは起動しましたが、有効な応答を生成できません。</span><span class="sxs-lookup"><span data-stu-id="cfc19-121">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="cfc19-122">問題のトラブルシューティングを行うには、[Kudu コンソールでアプリを実行する](#run-the-app-in-the-kudu-console)か、または [ASP.NET Core モジュールの stdout ログを有効にします](#aspnet-core-module-stdout-log)。</span><span class="sxs-lookup"><span data-stu-id="cfc19-122">[Run the app in the Kudu console](#run-the-app-in-the-kudu-console) or [enable the ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log) to troubleshoot the problem.</span></span>

::: moniker range="= aspnetcore-2.2"

### <a name="50030-in-process-startup-failure"></a><span data-ttu-id="cfc19-123">500.30 インプロセス起動エラー</span><span class="sxs-lookup"><span data-stu-id="cfc19-123">500.30 In-Process Startup Failure</span></span>

<span data-ttu-id="cfc19-124">ワーカー プロセスが失敗します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-124">The worker process fails.</span></span> <span data-ttu-id="cfc19-125">アプリは起動しません。</span><span class="sxs-lookup"><span data-stu-id="cfc19-125">The app doesn't start.</span></span>

<span data-ttu-id="cfc19-126">ASP.NET Core モジュールは .NET Core CLR の開始をインプロセスで試みますが、開始に失敗します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-126">The ASP.NET Core Module attempts to start the .NET Core CLR in-process, but it fails to start.</span></span> <span data-ttu-id="cfc19-127">プロセス起動時のエラーの原因は、通常、[アプリケーション イベント ログ](#application-event-log)と [ASP.NET Core モジュールの stdout ログ](#aspnet-core-module-stdout-log)のエントリから判断します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-127">The cause of a process startup failure is usually determined from entries in the [Application Event Log](#application-event-log) and the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="50031-ancm-failed-to-find-native-dependencies"></a><span data-ttu-id="cfc19-128">500.31 ANCM Failed to Find Native Dependencies (500.31 ANCM ネイティブの依存関係を見つけられませんでした)</span><span class="sxs-lookup"><span data-stu-id="cfc19-128">500.31 ANCM Failed to Find Native Dependencies</span></span>

<span data-ttu-id="cfc19-129">ワーカー プロセスが失敗します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-129">The worker process fails.</span></span> <span data-ttu-id="cfc19-130">アプリは起動しません。</span><span class="sxs-lookup"><span data-stu-id="cfc19-130">The app doesn't start.</span></span>

<span data-ttu-id="cfc19-131">ASP.NET Core モジュールで .NET Core ランタイムの開始がインプロセスで試行されますが、開始に失敗します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-131">The ASP.NET Core Module attempts to start the .NET Core runtime in-process, but it fails to start.</span></span> <span data-ttu-id="cfc19-132">このスタートアップ エラーの最も一般的な原因は、`Microsoft.NETCore.App` または `Microsoft.AspNetCore.App` ランタイムがインストールされていない場合です。</span><span class="sxs-lookup"><span data-stu-id="cfc19-132">The most common cause of this startup failure is when the `Microsoft.NETCore.App` or `Microsoft.AspNetCore.App` runtime isn't installed.</span></span> <span data-ttu-id="cfc19-133">アプリが ASP.NET Core 3.0 をターゲットとして展開されていて、そのバージョンがコンピューターに存在しない場合、このエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-133">If the app is deployed to target ASP.NET Core 3.0 and that version doesn't exist on the machine, this error occurs.</span></span> <span data-ttu-id="cfc19-134">エラー メッセージの例は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="cfc19-134">An example error message follows:</span></span>

```
The specified framework 'Microsoft.NETCore.App', version '3.0.0' was not found.
  - The following frameworks were found:
      2.2.1 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview5-27626-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27713-13 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27714-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27723-08 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
```

<span data-ttu-id="cfc19-135">エラー メッセージには、インストールされているすべての .NET Core のバージョンとアプリに必要なバージョンが一覧表示されます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-135">The error message lists all the installed .NET Core versions and the version requested by the app.</span></span> <span data-ttu-id="cfc19-136">このエラーを修正するには、次のいずれかを実行します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-136">To fix this error, either:</span></span>

* <span data-ttu-id="cfc19-137">適切なバージョンの .NET Core をマシンにインストールします。</span><span class="sxs-lookup"><span data-stu-id="cfc19-137">Install the appropriate version of .NET Core on the machine.</span></span>
* <span data-ttu-id="cfc19-138">マシンに存在する .NET Core のバージョンをターゲットにするようにアプリを変更します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-138">Change the app to target a version of .NET Core that's present on the machine.</span></span>
* <span data-ttu-id="cfc19-139">アプリを[自己完結型の展開](/dotnet/core/deploying/#self-contained-deployments-scd)として発行します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-139">Publish the app as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

<span data-ttu-id="cfc19-140">開発環境で実行している場合 (`ASPNETCORE_ENVIRONMENT` 環境変数が `Development` に設定されている場合)、特定のエラーが HTTP 応答に書き込まれます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-140">When running in development (the `ASPNETCORE_ENVIRONMENT` environment variable is set to `Development`), the specific error is written to the HTTP response.</span></span> <span data-ttu-id="cfc19-141">プロセスのスタートアップ エラーの原因は、[アプリケーション イベント ログ](#application-event-log)でも見つかります。</span><span class="sxs-lookup"><span data-stu-id="cfc19-141">The cause of a process startup failure is also found in the [Application Event Log](#application-event-log).</span></span>

### <a name="50032-ancm-failed-to-load-dll"></a><span data-ttu-id="cfc19-142">500.32 ANCM Failed to Load dll (500.32 ANCM DLL を読み込めませんでした)</span><span class="sxs-lookup"><span data-stu-id="cfc19-142">500.32 ANCM Failed to Load dll</span></span>

<span data-ttu-id="cfc19-143">ワーカー プロセスが失敗します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-143">The worker process fails.</span></span> <span data-ttu-id="cfc19-144">アプリは起動しません。</span><span class="sxs-lookup"><span data-stu-id="cfc19-144">The app doesn't start.</span></span>

<span data-ttu-id="cfc19-145">このエラーの最も一般的な原因は、アプリが互換性のないプロセッサ アーキテクチャ用に発行されていることです。</span><span class="sxs-lookup"><span data-stu-id="cfc19-145">The most common cause for this error is that the app is published for an incompatible processor architecture.</span></span> <span data-ttu-id="cfc19-146">ワーカー プロセスが 32 ビットアプリとして実行されていて、そのアプリが 64 ビットをターゲットとして発行されている場合、このエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-146">If the worker process is running as a 32-bit app and the app was published to target 64-bit, this error occurs.</span></span>

<span data-ttu-id="cfc19-147">このエラーを修正するには、次のいずれかを実行します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-147">To fix this error, either:</span></span>

* <span data-ttu-id="cfc19-148">ワーカー プロセスと同じプロセッサ アーキテクチャ用にアプリを再発行します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-148">Republish the app for the same processor architecture as the worker process.</span></span>
* <span data-ttu-id="cfc19-149">アプリを[フレームワークに依存する展開](/dotnet/core/deploying/#framework-dependent-executables-fde)として発行します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-149">Publish the app as a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-executables-fde).</span></span>

### <a name="50033-ancm-request-handler-load-failure"></a><span data-ttu-id="cfc19-150">500.33 ANCM Request Handler Load Failure (500.33 ANCM 要求ハンドラーの読み込みエラー)</span><span class="sxs-lookup"><span data-stu-id="cfc19-150">500.33 ANCM Request Handler Load Failure</span></span>

<span data-ttu-id="cfc19-151">ワーカー プロセスが失敗します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-151">The worker process fails.</span></span> <span data-ttu-id="cfc19-152">アプリは起動しません。</span><span class="sxs-lookup"><span data-stu-id="cfc19-152">The app doesn't start.</span></span>

<span data-ttu-id="cfc19-153">アプリは `Microsoft.AspNetCore.App` フレームワークを参照していませんでした。</span><span class="sxs-lookup"><span data-stu-id="cfc19-153">The app didn't reference the `Microsoft.AspNetCore.App` framework.</span></span> <span data-ttu-id="cfc19-154">ASP.NET Core モジュールでホストできるのは、`Microsoft.AspNetCore.App` フレームワークをターゲットとしているアプリのみです。</span><span class="sxs-lookup"><span data-stu-id="cfc19-154">Only apps targeting the `Microsoft.AspNetCore.App` framework can be hosted by the ASP.NET Core Module.</span></span>

<span data-ttu-id="cfc19-155">このエラーを修正するには、アプリが `Microsoft.AspNetCore.App` フレームワークをターゲットにしていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-155">To fix this error, confirm that the app is targeting the `Microsoft.AspNetCore.App` framework.</span></span> <span data-ttu-id="cfc19-156">アプリがターゲットとしているフレームワークを確認するには、`.runtimeconfig.json` を確認します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-156">Check the `.runtimeconfig.json` to verify the framework targeted by the app.</span></span>

### <a name="50034-ancm-mixed-hosting-models-not-supported"></a><span data-ttu-id="cfc19-157">500.34 ANCM Mixed Hosting Models Not Supported (500.34 ANCM 混在ホスティング モデルはサポートされません)</span><span class="sxs-lookup"><span data-stu-id="cfc19-157">500.34 ANCM Mixed Hosting Models Not Supported</span></span>

<span data-ttu-id="cfc19-158">ワーカー プロセスでは、インプロセス アプリとアウト プロセス アプリの両方を同じプロセスで実行できません。</span><span class="sxs-lookup"><span data-stu-id="cfc19-158">The worker process can't run both an in-process app and an out-of-process app in the same process.</span></span>

<span data-ttu-id="cfc19-159">このエラーを修正するには、別の IIS アプリケーション プールでアプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-159">To fix this error, run apps in separate IIS application pools.</span></span>

### <a name="50035-ancm-multiple-in-process-applications-in-same-process"></a><span data-ttu-id="cfc19-160">500.35 ANCM Multiple In-Process Applications in same Process (500.35 ANCM 同一プロセス内の複数のインプロセス アプリケーション)</span><span class="sxs-lookup"><span data-stu-id="cfc19-160">500.35 ANCM Multiple In-Process Applications in same Process</span></span>

<span data-ttu-id="cfc19-161">ワーカー プロセスでは、インプロセス アプリとアウト プロセス アプリの両方を同じプロセスで実行できません。</span><span class="sxs-lookup"><span data-stu-id="cfc19-161">The worker process can't run both an in-process app and an out-of-process app in the same process.</span></span>

<span data-ttu-id="cfc19-162">このエラーを修正するには、別の IIS アプリケーション プールでアプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-162">To fix this error, run apps in separate IIS application pools.</span></span>

### <a name="50036-ancm-out-of-process-handler-load-failure"></a><span data-ttu-id="cfc19-163">500.36 ANCM Out-Of-Process Handler Load Failure (500.36 ANCM アウト プロセス ハンドラーの読み込みエラー)</span><span class="sxs-lookup"><span data-stu-id="cfc19-163">500.36 ANCM Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="cfc19-164">アウト プロセス要求ハンドラーの *aspnetcorev2_outofprocess.dll* が *aspnetcorev2.dll* ファイルの次にありません。</span><span class="sxs-lookup"><span data-stu-id="cfc19-164">The out-of-process request handler, *aspnetcorev2_outofprocess.dll*, isn't next to the *aspnetcorev2.dll* file.</span></span> <span data-ttu-id="cfc19-165">これは、ASP.NET Core モジュールのインストールが破損していることを示しています。</span><span class="sxs-lookup"><span data-stu-id="cfc19-165">This indicates a corrupted installation of the ASP.NET Core Module.</span></span>

<span data-ttu-id="cfc19-166">このエラーを修正するには、[.NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (IIS 用) または Visual Studio (IIS Express 用) のインストールを修復します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-166">To fix this error, repair the installation of the [.NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (for IIS) or Visual Studio (for IIS Express).</span></span>

### <a name="50037-ancm-failed-to-start-within-startup-time-limit"></a><span data-ttu-id="cfc19-167">500.37 ANCM Failed to Start Within Startup Time Limit (500.37 ANCM スタートアップ時間の制限内に起動できませんでした)</span><span class="sxs-lookup"><span data-stu-id="cfc19-167">500.37 ANCM Failed to Start Within Startup Time Limit</span></span>

<span data-ttu-id="cfc19-168">指定されたスタートアップ時間の制限内に ANCM が起動に失敗しました。</span><span class="sxs-lookup"><span data-stu-id="cfc19-168">ANCM failed to start within the provied startup time limit.</span></span> <span data-ttu-id="cfc19-169">既定では、タイムアウトは 120 秒です。</span><span class="sxs-lookup"><span data-stu-id="cfc19-169">By default, the timeout is 120 seconds.</span></span>

<span data-ttu-id="cfc19-170">このエラーは、同じマシン上で多数のアプリを起動したときに発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="cfc19-170">This error can occur when starting a large number of apps on the same machine.</span></span> <span data-ttu-id="cfc19-171">スタートアップ中のサーバー上の CPU/メモリ使用量の急上昇を確認します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-171">Check for CPU/Memory usage spikes on the server during startup.</span></span> <span data-ttu-id="cfc19-172">必要に応じて、複数のアプリのスタートアップ プロセスをずらします。</span><span class="sxs-lookup"><span data-stu-id="cfc19-172">You may need to stagger the startup process of multiple apps.</span></span>

### <a name="50030-in-process-startup-failure"></a><span data-ttu-id="cfc19-173">500.30 インプロセス起動エラー</span><span class="sxs-lookup"><span data-stu-id="cfc19-173">500.30 In-Process Startup Failure</span></span>

<span data-ttu-id="cfc19-174">ワーカー プロセスが失敗します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-174">The worker process fails.</span></span> <span data-ttu-id="cfc19-175">アプリは起動しません。</span><span class="sxs-lookup"><span data-stu-id="cfc19-175">The app doesn't start.</span></span>

<span data-ttu-id="cfc19-176">ASP.NET Core モジュールで .NET Core ランタイムの開始がインプロセスで試行されますが、開始に失敗します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-176">The ASP.NET Core Module attempts to start the .NET Core runtime in-process, but it fails to start.</span></span> <span data-ttu-id="cfc19-177">プロセス起動時のエラーの原因は、通常、[アプリケーション イベント ログ](#application-event-log)と [ASP.NET Core モジュールの stdout ログ](#aspnet-core-module-stdout-log)のエントリから判断します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-177">The cause of a process startup failure is usually determined from entries in the [Application Event Log](#application-event-log) and the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log).</span></span>

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="cfc19-178">500.0 インプロセス ハンドラーの読み込みエラー</span><span class="sxs-lookup"><span data-stu-id="cfc19-178">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="cfc19-179">ワーカー プロセスが失敗します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-179">The worker process fails.</span></span> <span data-ttu-id="cfc19-180">アプリは起動しません。</span><span class="sxs-lookup"><span data-stu-id="cfc19-180">The app doesn't start.</span></span>

<span data-ttu-id="cfc19-181">プロセスのスタートアップ エラーの原因は、[アプリケーション イベント ログ](#application-event-log)でも見つかります。</span><span class="sxs-lookup"><span data-stu-id="cfc19-181">The cause of a process startup failure is also found in the [Application Event Log](#application-event-log).</span></span>

::: moniker-end

<span data-ttu-id="cfc19-182">**接続のリセット**</span><span class="sxs-lookup"><span data-stu-id="cfc19-182">**Connection reset**</span></span>

<span data-ttu-id="cfc19-183">ヘッダー送信後にエラーが発生した場合、サーバーが **500 内部サーバー エラー**を送信するには遅すぎます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-183">If an error occurs after the headers are sent, it's too late for the server to send a **500 Internal Server Error** when an error occurs.</span></span> <span data-ttu-id="cfc19-184">このような状況は、応答に対する複雑なオブジェクトのシリアル化中にエラーが起きたときによく発生します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-184">This often happens when an error occurs during the serialization of complex objects for a response.</span></span> <span data-ttu-id="cfc19-185">この種のエラーは、クライアントでは "*接続リセット*" エラーとして表示されます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-185">This type of error appears as a *connection reset* error on the client.</span></span> <span data-ttu-id="cfc19-186">この種のエラーのトラブルシューティングには、[アプリケーション ログ](xref:fundamentals/logging/index)が役に立つことがあります。</span><span class="sxs-lookup"><span data-stu-id="cfc19-186">[Application logging](xref:fundamentals/logging/index) can help troubleshoot these types of errors.</span></span>

## <a name="default-startup-limits"></a><span data-ttu-id="cfc19-187">既定の起動制限</span><span class="sxs-lookup"><span data-stu-id="cfc19-187">Default startup limits</span></span>

<span data-ttu-id="cfc19-188">ASP.NET Core モジュールの *startupTimeLimit* は、既定では 120 秒に構成されます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-188">The ASP.NET Core Module is configured with a default *startupTimeLimit* of 120 seconds.</span></span> <span data-ttu-id="cfc19-189">既定値のままにした場合、モジュールで処理エラーが記録されるまでに、アプリは最大で 2 分を起動にかけることができます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-189">When left at the default value, an app may take up to two minutes to start before the module logs a process failure.</span></span> <span data-ttu-id="cfc19-190">モジュールの構成の詳細については、「[AspNetCore 要素の属性](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="cfc19-190">For information on configuring the module, see [Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

## <a name="troubleshoot-app-startup-errors"></a><span data-ttu-id="cfc19-191">アプリの起動エラーのトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="cfc19-191">Troubleshoot app startup errors</span></span>

### <a name="application-event-log"></a><span data-ttu-id="cfc19-192">アプリケーション イベント ログ</span><span class="sxs-lookup"><span data-stu-id="cfc19-192">Application Event Log</span></span>

<span data-ttu-id="cfc19-193">アプリケーション イベント ログにアクセスするには、Azure portal の **[問題の診断と解決]** ブレードを使います。</span><span class="sxs-lookup"><span data-stu-id="cfc19-193">To access the Application Event Log, use the **Diagnose and solve problems** blade in the Azure portal:</span></span>

1. <span data-ttu-id="cfc19-194">Azure portal の **[App Services]** でアプリを開きます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-194">In the Azure portal, open the app in **App Services**.</span></span>
1. <span data-ttu-id="cfc19-195">**[問題の診断と解決]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-195">Select **Diagnose and solve problems**.</span></span>
1. <span data-ttu-id="cfc19-196">**[診断ツール]** という見出しを選択します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-196">Select the **Diagnostic Tools** heading.</span></span>
1. <span data-ttu-id="cfc19-197">**[サポート ツール]** で **[アプリケーション イベント]** ボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-197">Under **Support Tools**, select the **Application Events** button.</span></span>
1. <span data-ttu-id="cfc19-198">**[Source]\(ソース\)** 列で、*IIS AspNetCoreModule* または *IIS AspNetCoreModule V2* によって提供された最新のエラーを調べます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-198">Examine the latest error provided by the *IIS AspNetCoreModule* or *IIS AspNetCoreModule V2* entry in the **Source** column.</span></span>

<span data-ttu-id="cfc19-199">**[問題の診断と解決]** ブレードを使う代わりに、[Kudu](https://github.com/projectkudu/kudu/wiki) を使ってアプリケーション イベント ログ ファイルを直接調べることもできます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-199">An alternative to using the **Diagnose and solve problems** blade is to examine the Application Event Log file directly using [Kudu](https://github.com/projectkudu/kudu/wiki):</span></span>

1. <span data-ttu-id="cfc19-200">**[開発ツール]** 領域で **[高度なツール]** を開きます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-200">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="cfc19-201">**[Go&rarr;]** ボタンを選びます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-201">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="cfc19-202">新しいブラウザー タブまたはウィンドウで Kudu コンソールが開きます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-202">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="cfc19-203">ページの上部にあるナビゲーション バーを使って **[デバッグ コンソール]** を開き、 **[CMD]** を選びます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-203">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="cfc19-204">**LogFiles** フォルダーを開きます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-204">Open the **LogFiles** folder.</span></span>
1. <span data-ttu-id="cfc19-205">*eventlog.xml* ファイルの横にある鉛筆アイコンを選びます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-205">Select the pencil icon next to the *eventlog.xml* file.</span></span>
1. <span data-ttu-id="cfc19-206">ログを調べます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-206">Examine the log.</span></span> <span data-ttu-id="cfc19-207">ログの末尾までスクロールし、最新のイベントを確認します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-207">Scroll to the bottom of the log to see the most recent events.</span></span>

### <a name="run-the-app-in-the-kudu-console"></a><span data-ttu-id="cfc19-208">Kudu コンソールでアプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-208">Run the app in the Kudu console</span></span>

<span data-ttu-id="cfc19-209">多くの起動時エラーでは、アプリケーション イベント ログに役に立つ情報が生成されません。</span><span class="sxs-lookup"><span data-stu-id="cfc19-209">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="cfc19-210">[Kudu](https://github.com/projectkudu/kudu/wiki) のリモート実行コンソールでアプリを実行すると、エラーを検出することができます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-210">You can run the app in the [Kudu](https://github.com/projectkudu/kudu/wiki) Remote Execution Console to discover the error:</span></span>

1. <span data-ttu-id="cfc19-211">**[開発ツール]** 領域で **[高度なツール]** を開きます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-211">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="cfc19-212">**[Go&rarr;]** ボタンを選びます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-212">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="cfc19-213">新しいブラウザー タブまたはウィンドウで Kudu コンソールが開きます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-213">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="cfc19-214">ページの上部にあるナビゲーション バーを使って **[デバッグ コンソール]** を開き、 **[CMD]** を選びます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-214">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>

#### <a name="test-a-32-bit-x86-app"></a><span data-ttu-id="cfc19-215">32 ビット (x86) アプリをテストする</span><span class="sxs-lookup"><span data-stu-id="cfc19-215">Test a 32-bit (x86) app</span></span>

##### <a name="current-release"></a><span data-ttu-id="cfc19-216">現在のリリース</span><span class="sxs-lookup"><span data-stu-id="cfc19-216">Current release</span></span>

1. `cd d:\home\site\wwwroot`
1. <span data-ttu-id="cfc19-217">次のようにアプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-217">Run the app:</span></span>
   * <span data-ttu-id="cfc19-218">アプリが[フレームワークに依存する展開](/dotnet/core/deploying/#framework-dependent-deployments-fdd)の場合:</span><span class="sxs-lookup"><span data-stu-id="cfc19-218">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

     ```console
     dotnet .\{ASSEMBLY NAME}.dll
     ```

   * <span data-ttu-id="cfc19-219">アプリが[自己完結型の展開](/dotnet/core/deploying/#self-contained-deployments-scd)の場合:</span><span class="sxs-lookup"><span data-stu-id="cfc19-219">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

     ```console
     {ASSEMBLY NAME}.exe
     ```

<span data-ttu-id="cfc19-220">エラーを示すアプリからのコンソール出力はすべて、Kudu コンソールにパイプされます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-220">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

##### <a name="framework-dependent-deployment-running-on-a-preview-release"></a><span data-ttu-id="cfc19-221">プレビュー リリース上で実行されているフレームワークに依存する展開</span><span class="sxs-lookup"><span data-stu-id="cfc19-221">Framework-dependent deployment running on a preview release</span></span>

<span data-ttu-id="cfc19-222">"*ASP.NET Core {バージョン} (x86) ランタイムのサイト拡張機能をインストールする必要があります。* "</span><span class="sxs-lookup"><span data-stu-id="cfc19-222">*Requires installing the ASP.NET Core {VERSION} (x86) Runtime site extension.*</span></span>

1. <span data-ttu-id="cfc19-223">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` はランタイム バージョンです)</span><span class="sxs-lookup"><span data-stu-id="cfc19-223">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="cfc19-224">アプリを実行します: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="cfc19-224">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="cfc19-225">エラーを示すアプリからのコンソール出力はすべて、Kudu コンソールにパイプされます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-225">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

#### <a name="test-a-64-bit-x64-app"></a><span data-ttu-id="cfc19-226">64 ビット (x64) アプリをテストする</span><span class="sxs-lookup"><span data-stu-id="cfc19-226">Test a 64-bit (x64) app</span></span>

##### <a name="current-release"></a><span data-ttu-id="cfc19-227">現在のリリース</span><span class="sxs-lookup"><span data-stu-id="cfc19-227">Current release</span></span>

* <span data-ttu-id="cfc19-228">アプリが 64 ビット (x64) の[フレームワークに依存する展開](/dotnet/core/deploying/#framework-dependent-deployments-fdd)の場合:</span><span class="sxs-lookup"><span data-stu-id="cfc19-228">If the app is a 64-bit (x64) [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>
  1. `cd D:\Program Files\dotnet`
  1. <span data-ttu-id="cfc19-229">アプリを実行します: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="cfc19-229">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>
* <span data-ttu-id="cfc19-230">アプリが[自己完結型の展開](/dotnet/core/deploying/#self-contained-deployments-scd)の場合:</span><span class="sxs-lookup"><span data-stu-id="cfc19-230">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>
  1. `cd D:\home\site\wwwroot`
  1. <span data-ttu-id="cfc19-231">アプリを実行します: `{ASSEMBLY NAME}.exe`</span><span class="sxs-lookup"><span data-stu-id="cfc19-231">Run the app: `{ASSEMBLY NAME}.exe`</span></span>

<span data-ttu-id="cfc19-232">エラーを示すアプリからのコンソール出力はすべて、Kudu コンソールにパイプされます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-232">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

##### <a name="framework-dependent-deployment-running-on-a-preview-release"></a><span data-ttu-id="cfc19-233">プレビュー リリース上で実行されているフレームワークに依存する展開</span><span class="sxs-lookup"><span data-stu-id="cfc19-233">Framework-dependent deployment running on a preview release</span></span>

<span data-ttu-id="cfc19-234">"*ASP.NET Core {バージョン} (x64) ランタイムのサイト拡張機能をインストールする必要があります。* "</span><span class="sxs-lookup"><span data-stu-id="cfc19-234">*Requires installing the ASP.NET Core {VERSION} (x64) Runtime site extension.*</span></span>

1. <span data-ttu-id="cfc19-235">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` はランタイム バージョンです)</span><span class="sxs-lookup"><span data-stu-id="cfc19-235">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="cfc19-236">アプリを実行します: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="cfc19-236">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="cfc19-237">エラーを示すアプリからのコンソール出力はすべて、Kudu コンソールにパイプされます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-237">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

### <a name="aspnet-core-module-stdout-log"></a><span data-ttu-id="cfc19-238">ASP.NET Core モジュールの stdout ログ</span><span class="sxs-lookup"><span data-stu-id="cfc19-238">ASP.NET Core Module stdout log</span></span>

<span data-ttu-id="cfc19-239">ASP.NET Core モジュールの stdout には、アプリケーション イベント ログでは見つからない有用なエラー メッセージが記録されることがよくあります。</span><span class="sxs-lookup"><span data-stu-id="cfc19-239">The ASP.NET Core Module stdout log often records useful error messages not found in the Application Event Log.</span></span> <span data-ttu-id="cfc19-240">stdout ログを有効にして表示するには:</span><span class="sxs-lookup"><span data-stu-id="cfc19-240">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="cfc19-241">Azure portal で **[問題の診断と解決]** ブレードに移動します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-241">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="cfc19-242">**[SELECT PROBLEM CATEGORY]\(問題カテゴリの選択\)** で、 **[Web App Down]\(Web アプリのダウン\)** ボタンを選びます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-242">Under **SELECT PROBLEM CATEGORY**, select the **Web App Down** button.</span></span>
1. <span data-ttu-id="cfc19-243">**[Suggested Solutions]\(推奨される解決方法\)**  >  **[Enable Stdout Log Redirection]\(Stdout ログのリダイレクトを有効にする\)** で、ボタンをクリックして **[Open Kudu Console to edit Web.Config]\(Kudu コンソールを開いて Web.Config を編集する\)** ボタンを選びます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-243">Under **Suggested Solutions** > **Enable Stdout Log Redirection**, select the button to **Open Kudu Console to edit Web.Config**.</span></span>
1. <span data-ttu-id="cfc19-244">Kudu の**診断コンソール**で、パス **site** > **wwwroot** へのフォルダーを開きます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-244">In the Kudu **Diagnostic Console**, open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="cfc19-245">下にスクロールして、一覧の最後にある *web.config* ファイルを表示します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-245">Scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="cfc19-246">*web.config* ファイルの隣の鉛筆アイコンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="cfc19-246">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="cfc19-247">**stdoutLogEnabled** を `true` に設定し、**stdoutLogFile** パスを `\\?\%home%\LogFiles\stdout` に変更します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-247">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="cfc19-248">**[保存]** を選んで、更新した *web.config* ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-248">Select **Save** to save the updated *web.config* file.</span></span>
1. <span data-ttu-id="cfc19-249">アプリに対して要求します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-249">Make a request to the app.</span></span>
1. <span data-ttu-id="cfc19-250">Azure portal に戻ります。</span><span class="sxs-lookup"><span data-stu-id="cfc19-250">Return to the Azure portal.</span></span> <span data-ttu-id="cfc19-251">**[開発ツール]** 領域で **[高度なツール]** ブレードを選びます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-251">Select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="cfc19-252">**[Go&rarr;]** ボタンを選びます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-252">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="cfc19-253">新しいブラウザー タブまたはウィンドウで Kudu コンソールが開きます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-253">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="cfc19-254">ページの上部にあるナビゲーション バーを使って **[デバッグ コンソール]** を開き、 **[CMD]** を選びます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-254">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="cfc19-255">**LogFiles** フォルダーを選びます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-255">Select the **LogFiles** folder.</span></span>
1. <span data-ttu-id="cfc19-256">**[Modified]\(変更日\)** 列を調べて、変更日が最新の stdout ログの鉛筆アイコンを選んで編集します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-256">Inspect the **Modified** column and select the pencil icon to edit the stdout log with the latest modification date.</span></span>
1. <span data-ttu-id="cfc19-257">ログ ファイルが開くと、エラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-257">When the log file opens, the error is displayed.</span></span>

<span data-ttu-id="cfc19-258">トラブルシューティングが完了したら、stdout ログを無効にします。</span><span class="sxs-lookup"><span data-stu-id="cfc19-258">Disable stdout logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="cfc19-259">Kudu の**診断コンソール** で、パス **site** > **wwwroot** に戻り、*web.config* ファイルを表示します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-259">In the Kudu **Diagnostic Console**, return to the path **site** > **wwwroot** to reveal the *web.config* file.</span></span> <span data-ttu-id="cfc19-260">鉛筆アイコンを選んで **web.config** ファイルを再び開きます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-260">Open the **web.config** file again by selecting the pencil icon.</span></span>
1. <span data-ttu-id="cfc19-261">**stdoutLogEnabled** を `false` に設定します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-261">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="cfc19-262">**[保存]** を選んでファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-262">Select **Save** to save the file.</span></span>

> [!WARNING]
> <span data-ttu-id="cfc19-263">stdout ログを無効にしないと、アプリまたはサーバーで障害が発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="cfc19-263">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="cfc19-264">ログ ファイルのサイズまたは作成されるログ ファイルの数に制限はありません。</span><span class="sxs-lookup"><span data-stu-id="cfc19-264">There's no limit on log file size or the number of log files created.</span></span> <span data-ttu-id="cfc19-265">stdout ログは、アプリ起動時の問題のトラブルシューティングにのみ使ってください。</span><span class="sxs-lookup"><span data-stu-id="cfc19-265">Only use stdout logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="cfc19-266">起動後の ASP.NET Core アプリでの一般的なログの場合は、ログ ファイルのサイズを制限し、ログをローテーションするログ ライブラリを使います。</span><span class="sxs-lookup"><span data-stu-id="cfc19-266">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="cfc19-267">詳細については、「[サードパーティ製のログ プロバイダー](xref:fundamentals/logging/index#third-party-logging-providers)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="cfc19-267">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="aspnet-core-module-debug-log"></a><span data-ttu-id="cfc19-268">ASP.NET Core モジュール デバッグ ログ</span><span class="sxs-lookup"><span data-stu-id="cfc19-268">ASP.NET Core Module debug log</span></span>

<span data-ttu-id="cfc19-269">ASP.NET Core モジュール デバッグ ログでは、ASP.NET Core モジュールのさらに詳しいログが提供されます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-269">The ASP.NET Core Module debug log provides additional, deeper logging from the ASP.NET Core Module.</span></span> <span data-ttu-id="cfc19-270">stdout ログを有効にして表示するには:</span><span class="sxs-lookup"><span data-stu-id="cfc19-270">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="cfc19-271">強化された診断ログを有効にするには、次のいずれかを実行します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-271">To enable the enhanced diagnostic log, perform either of the following:</span></span>
   * <span data-ttu-id="cfc19-272">「[強化された診断ログ](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs)」の指示に従い、強化された診断ログをアプリに対して設定します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-272">Follow the instructions in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) to configure the app for an enhanced diagnostic logging.</span></span> <span data-ttu-id="cfc19-273">アプリを再デプロイします。</span><span class="sxs-lookup"><span data-stu-id="cfc19-273">Redeploy the app.</span></span>
   * <span data-ttu-id="cfc19-274">Kudu コンソールを利用し、「`<handlerSettings>`強化された診断ログ[」にある ](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) をライブ アプリの *web.config* ファイルに追加します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-274">Add the `<handlerSettings>` shown in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) to the live app's *web.config* file using the Kudu console:</span></span>
     1. <span data-ttu-id="cfc19-275">**[開発ツール]** 領域で **[高度なツール]** を開きます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-275">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="cfc19-276">**[Go&rarr;]** ボタンを選びます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-276">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="cfc19-277">新しいブラウザー タブまたはウィンドウで Kudu コンソールが開きます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-277">The Kudu console opens in a new browser tab or window.</span></span>
     1. <span data-ttu-id="cfc19-278">ページの上部にあるナビゲーション バーを使って **[デバッグ コンソール]** を開き、 **[CMD]** を選びます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-278">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
     1. <span data-ttu-id="cfc19-279">パス **site** > **wwwroot** へのフォルダーを開きます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-279">Open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="cfc19-280">鉛筆アイコンを選択し、*web.config* ファイルを編集します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-280">Edit the *web.config* file by selecting the pencil button.</span></span> <span data-ttu-id="cfc19-281">「[強化された診断ログ](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs)」にある `<handlerSettings>` セクションを追加します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-281">Add the `<handlerSettings>` section as shown in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs).</span></span> <span data-ttu-id="cfc19-282">**[保存]** ボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-282">Select the **Save** button.</span></span>
1. <span data-ttu-id="cfc19-283">**[開発ツール]** 領域で **[高度なツール]** を開きます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-283">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="cfc19-284">**[Go&rarr;]** ボタンを選びます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-284">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="cfc19-285">新しいブラウザー タブまたはウィンドウで Kudu コンソールが開きます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-285">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="cfc19-286">ページの上部にあるナビゲーション バーを使って **[デバッグ コンソール]** を開き、 **[CMD]** を選びます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-286">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="cfc19-287">パス **site** > **wwwroot** へのフォルダーを開きます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-287">Open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="cfc19-288">*aspnetcore-debug.log* ファイルにパスを指定しなかった場合、ファイルが一覧に表示されます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-288">If you didn't supply a path for the *aspnetcore-debug.log* file, the file appears in the list.</span></span> <span data-ttu-id="cfc19-289">パスを指定した場合、ログ ファイルの場所に移動します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-289">If you supplied a path, navigate to the location of the log file.</span></span>
1. <span data-ttu-id="cfc19-290">ファイル名の隣にある鉛筆アイコンでログ ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-290">Open the log file with the pencil button next to the file name.</span></span>

<span data-ttu-id="cfc19-291">トラブルシューティングが完了したら、debug ログを無効にします。</span><span class="sxs-lookup"><span data-stu-id="cfc19-291">Disable debug logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="cfc19-292">強化されたデバッグ ログを無効にするには、次のいずれかを実行します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-292">To disable the enhanced debug log, perform either of the following:</span></span>
   * <span data-ttu-id="cfc19-293">ローカルの *web.config* ファイルから `<handlerSettings>` を削除し、アプリを再デプロイします。</span><span class="sxs-lookup"><span data-stu-id="cfc19-293">Remove the `<handlerSettings>` from the *web.config* file locally and redeploy the app.</span></span>
   * <span data-ttu-id="cfc19-294">Kudu コンソールを使用して *web.config* ファイルを編集し、`<handlerSettings>` セクションを削除します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-294">Use the Kudu console to edit the *web.config* file and remove the `<handlerSettings>` section.</span></span> <span data-ttu-id="cfc19-295">ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-295">Save the file.</span></span>

> [!WARNING]
> <span data-ttu-id="cfc19-296">debug ログを無効にしないと、アプリまたはサーバーで障害が発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="cfc19-296">Failure to disable the debug log can lead to app or server failure.</span></span> <span data-ttu-id="cfc19-297">ログ ファイルのサイズに制限はありません。</span><span class="sxs-lookup"><span data-stu-id="cfc19-297">There's no limit on log file size.</span></span> <span data-ttu-id="cfc19-298">debug ログは、アプリ起動時の問題のトラブルシューティングにのみ使ってください。</span><span class="sxs-lookup"><span data-stu-id="cfc19-298">Only use debug logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="cfc19-299">起動後の ASP.NET Core アプリでの一般的なログの場合は、ログ ファイルのサイズを制限し、ログをローテーションするログ ライブラリを使います。</span><span class="sxs-lookup"><span data-stu-id="cfc19-299">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="cfc19-300">詳しくは、「[サードパーティ製のログ プロバイダー](xref:fundamentals/logging/index#third-party-logging-providers)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="cfc19-300">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

::: moniker-end

## <a name="common-startup-errors"></a><span data-ttu-id="cfc19-301">起動時の一般的なエラー</span><span class="sxs-lookup"><span data-stu-id="cfc19-301">Common startup errors</span></span>

<span data-ttu-id="cfc19-302">以下を参照してください。<xref:host-and-deploy/azure-iis-errors-reference></span><span class="sxs-lookup"><span data-stu-id="cfc19-302">See <xref:host-and-deploy/azure-iis-errors-reference>.</span></span> <span data-ttu-id="cfc19-303">このリファレンス トピックでは、アプリの起動を妨げる一般的な問題のほとんどが説明されています。</span><span class="sxs-lookup"><span data-stu-id="cfc19-303">Most of the common problems that prevent app startup are covered in the reference topic.</span></span>

## <a name="slow-or-hanging-app"></a><span data-ttu-id="cfc19-304">アプリの速度低下またはハング</span><span class="sxs-lookup"><span data-stu-id="cfc19-304">Slow or hanging app</span></span>

<span data-ttu-id="cfc19-305">要求に対してアプリの反応が遅い場合、またはハングする場合は、次の記事を参照してください。</span><span class="sxs-lookup"><span data-stu-id="cfc19-305">When an app responds slowly or hangs on a request, see the following articles:</span></span>

* [<span data-ttu-id="cfc19-306">Azure App Service での Web アプリのパフォーマンス低下に関する問題のトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="cfc19-306">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [<span data-ttu-id="cfc19-307">Use Crash Diagnoser Site Extension to Capture Dump for Intermittent Exception issues or performance issues on Azure Web App (クラッシュ診断サイト拡張機能を使用して Azure Web App での断続的な例外の問題またはパフォーマンスの問題用のダンプをキャプチャする)</span><span class="sxs-lookup"><span data-stu-id="cfc19-307">Use Crash Diagnoser Site Extension to Capture Dump for Intermittent Exception issues or performance issues on Azure Web App</span></span>](https://blogs.msdn.microsoft.com/asiatech/2015/12/28/use-crash-diagnoser-site-extension-to-capture-dump-for-intermittent-exception-issues-or-performance-issues-on-azure-web-app/)

## <a name="remote-debugging"></a><span data-ttu-id="cfc19-308">リモート デバッグ</span><span class="sxs-lookup"><span data-stu-id="cfc19-308">Remote debugging</span></span>

<span data-ttu-id="cfc19-309">次のトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="cfc19-309">See the following topics:</span></span>

* <span data-ttu-id="cfc19-310">[「Visual Studio を使用した Azure App Service のトラブルシューティング」の「Web アプリのリモート デバッグ」セクション](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) (Azure ドキュメント)</span><span class="sxs-lookup"><span data-stu-id="cfc19-310">[Remote debugging web apps section of Troubleshoot a web app in Azure App Service using Visual Studio](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) (Azure documentation)</span></span>
* <span data-ttu-id="cfc19-311">[Visual Studio 2017 を使用して Azure 内の IIS 上の ASP.NET Core をリモート デバッグする](/visualstudio/debugger/remote-debugging-azure) (Visual Studio ドキュメント)</span><span class="sxs-lookup"><span data-stu-id="cfc19-311">[Remote Debug ASP.NET Core on IIS in Azure in Visual Studio 2017](/visualstudio/debugger/remote-debugging-azure) (Visual Studio documentation)</span></span>

## <a name="application-insights"></a><span data-ttu-id="cfc19-312">Application Insights</span><span class="sxs-lookup"><span data-stu-id="cfc19-312">Application Insights</span></span>

<span data-ttu-id="cfc19-313">[Application Insights](https://azure.microsoft.com/services/application-insights/) は、Azure App Service でホストされているアプリからのテレメトリを提供し、エラー ログやレポート機能を備えています。</span><span class="sxs-lookup"><span data-stu-id="cfc19-313">[Application Insights](https://azure.microsoft.com/services/application-insights/) provides telemetry from apps hosted in the Azure App Service, including error logging and reporting features.</span></span> <span data-ttu-id="cfc19-314">Application Insights は、アプリのログ機能が使用可能になってアプリが起動した後で発生したエラーのみをレポートできます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-314">Application Insights can only report on errors that occur after the app starts when the app's logging features become available.</span></span> <span data-ttu-id="cfc19-315">詳しくは、「[Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="cfc19-315">For more information, see [Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="monitoring-blades"></a><span data-ttu-id="cfc19-316">監視ブレード</span><span class="sxs-lookup"><span data-stu-id="cfc19-316">Monitoring blades</span></span>

<span data-ttu-id="cfc19-317">監視ブレードでは、このトピックでこれまでに説明した方法に代わるトラブルシューティング エクスペリエンスが提供されます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-317">Monitoring blades provide an alternative troubleshooting experience to the methods described earlier in the topic.</span></span> <span data-ttu-id="cfc19-318">これらのブレードは、500 番台のエラーの診断に使用できます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-318">These blades can be used to diagnose 500-series errors.</span></span>

<span data-ttu-id="cfc19-319">ASP.NET Core 拡張機能がインストールされていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="cfc19-319">Confirm that the ASP.NET Core Extensions are installed.</span></span> <span data-ttu-id="cfc19-320">拡張機能がインストールされていない場合は、手動でインストールします。</span><span class="sxs-lookup"><span data-stu-id="cfc19-320">If the extensions aren't installed, install them manually:</span></span>

1. <span data-ttu-id="cfc19-321">**[開発ツール]** ブレード セクションで、 **[拡張機能]** ブレードを選びます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-321">In the **DEVELOPMENT TOOLS** blade section, select the **Extensions** blade.</span></span>
1. <span data-ttu-id="cfc19-322">**[ASP.NET Core Extensions]\(ASP.NET Core 拡張機能\)** が一覧に表示されます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-322">The **ASP.NET Core Extensions** should appear in the list.</span></span>
1. <span data-ttu-id="cfc19-323">拡張機能がインストールされていない場合は、 **[追加]** ボタンを選びます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-323">If the extensions aren't installed, select the **Add** button.</span></span>
1. <span data-ttu-id="cfc19-324">一覧から **[ASP.NET Core Extensions]\(ASP.NET Core 拡張機能\)** を選びます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-324">Choose the **ASP.NET Core Extensions** from the list.</span></span>
1. <span data-ttu-id="cfc19-325">**[OK]** を選んで法的条項に同意します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-325">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="cfc19-326">**[拡張機能の追加]** ブレードで **[OK]** を選びます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-326">Select **OK** on the **Add extension** blade.</span></span>
1. <span data-ttu-id="cfc19-327">拡張機能が正常にインストールされると、情報ポップアップ メッセージで通知されます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-327">An informational pop-up message indicates when the extensions are successfully installed.</span></span>

<span data-ttu-id="cfc19-328">stdout ログが有効になっていない場合は、次の手順のようにします。</span><span class="sxs-lookup"><span data-stu-id="cfc19-328">If stdout logging isn't enabled, follow these steps:</span></span>

1. <span data-ttu-id="cfc19-329">Azure portal の **[開発ツール]** 領域で **[高度なツール]** ブレードを選びます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-329">In the Azure portal, select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="cfc19-330">**[Go&rarr;]** ボタンを選びます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-330">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="cfc19-331">新しいブラウザー タブまたはウィンドウで Kudu コンソールが開きます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-331">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="cfc19-332">ページの上部にあるナビゲーション バーを使って **[デバッグ コンソール]** を開き、 **[CMD]** を選びます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-332">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="cfc19-333">パス **site** > **wwwroot** へのフォルダーを開き、下にスクロールして、一覧の最後にある *web.config* ファイルを表示します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-333">Open the folders to the path **site** > **wwwroot** and scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="cfc19-334">*web.config* ファイルの隣の鉛筆アイコンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="cfc19-334">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="cfc19-335">**stdoutLogEnabled** を `true` に設定し、**stdoutLogFile** パスを `\\?\%home%\LogFiles\stdout` に変更します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-335">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="cfc19-336">**[保存]** を選んで、更新した *web.config* ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-336">Select **Save** to save the updated *web.config* file.</span></span>

<span data-ttu-id="cfc19-337">続けて診断ログをアクティブにします。</span><span class="sxs-lookup"><span data-stu-id="cfc19-337">Proceed to activate diagnostic logging:</span></span>

1. <span data-ttu-id="cfc19-338">Azure portal で **[診断ログ]** ブレードを選びます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-338">In the Azure portal, select the **Diagnostics logs** blade.</span></span>
1. <span data-ttu-id="cfc19-339">**[アプリケーション ログ (ファイル システム)]** および **[詳細なエラー メッセージ]** の **[オン]** スイッチを選びます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-339">Select the **On** switch for **Application Logging (Filesystem)** and **Detailed error messages**.</span></span> <span data-ttu-id="cfc19-340">ブレードの上部にある **[保存]** ボタンを選びます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-340">Select the **Save** button at the top of the blade.</span></span>
1. <span data-ttu-id="cfc19-341">失敗した要求イベントのバッファ処理 (FREB) とも呼ばれる失敗した要求のトレースを含めるには、 **[失敗した要求のトレース]** で **[オン]** スイッチを選びます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-341">To include failed request tracing, also known as Failed Request Event Buffering (FREB) logging, select the **On** switch for **Failed request tracing**.</span></span>
1. <span data-ttu-id="cfc19-342">ポータルで **[診断ログ]** ブレードのすぐ下にある **[ログ ストリーム]** ブレードを選びます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-342">Select the **Log stream** blade, which is listed immediately under the **Diagnostics logs** blade in the portal.</span></span>
1. <span data-ttu-id="cfc19-343">アプリに対して要求します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-343">Make a request to the app.</span></span>
1. <span data-ttu-id="cfc19-344">ログ ストリーム データ内で、エラーの原因が示されます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-344">Within the log stream data, the cause of the error is indicated.</span></span>

<span data-ttu-id="cfc19-345">トラブルシューティングが完了したら、stdout ログを無効にしてください。</span><span class="sxs-lookup"><span data-stu-id="cfc19-345">Be sure to disable stdout logging when troubleshooting is complete.</span></span> <span data-ttu-id="cfc19-346">「[ASP.NET Core モジュールの stdout ログ](#aspnet-core-module-stdout-log)」セクションの説明をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="cfc19-346">See the instructions in the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log) section.</span></span>

<span data-ttu-id="cfc19-347">失敗した要求のトレース ログ (FREB ログ) を見るには:</span><span class="sxs-lookup"><span data-stu-id="cfc19-347">To view the failed request tracing logs (FREB logs):</span></span>

1. <span data-ttu-id="cfc19-348">Azure portal で **[問題の診断と解決]** ブレードに移動します。</span><span class="sxs-lookup"><span data-stu-id="cfc19-348">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="cfc19-349">サイド バーの **[SUPPORT TOOLS]\(サポート ツール\)** 領域で、 **[Failed Request Tracing Logs]\(失敗した要求のトレース ログ\)** を選びます。</span><span class="sxs-lookup"><span data-stu-id="cfc19-349">Select **Failed Request Tracing Logs** from the **SUPPORT TOOLS** area of the sidebar.</span></span>

<span data-ttu-id="cfc19-350">詳しくは、[「Azure App Service の Web アプリの診断ログの有効化」トピックの「失敗した要求トレース」セクション](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces)および[「Azure での Web アプリのアプリケーション パフォーマンスに関するよくあるご質問」の「失敗した要求トレースをオンにするにはどうすればよいですか?」](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing)をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="cfc19-350">See [Failed request traces section of the Enable diagnostics logging for web apps in Azure App Service topic](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) and the [Application performance FAQs for Web Apps in Azure: How do I turn on failed request tracing?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) for more information.</span></span>

<span data-ttu-id="cfc19-351">詳しくは、「[Azure App Service の Web アプリの診断ログの有効化](/azure/app-service/web-sites-enable-diagnostic-log)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="cfc19-351">For more information, see [Enable diagnostics logging for web apps in Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log).</span></span>

> [!WARNING]
> <span data-ttu-id="cfc19-352">stdout ログを無効にしないと、アプリまたはサーバーで障害が発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="cfc19-352">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="cfc19-353">ログ ファイルのサイズまたは作成されるログ ファイルの数に制限はありません。</span><span class="sxs-lookup"><span data-stu-id="cfc19-353">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="cfc19-354">ASP.NET Core アプリでのルーチン ログの場合は、ログ ファイルのサイズを制限し、ログをローテーションするログ ライブラリを使います。</span><span class="sxs-lookup"><span data-stu-id="cfc19-354">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="cfc19-355">詳細については、「[サードパーティ製のログ プロバイダー](xref:fundamentals/logging/index#third-party-logging-providers)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="cfc19-355">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cfc19-356">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="cfc19-356">Additional resources</span></span>

* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [<span data-ttu-id="cfc19-357">Visual Studio を使用した Azure App Service での Web アプリのトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="cfc19-357">Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [<span data-ttu-id="cfc19-358">Azure Web Apps での "502 bad gateway" および "503 service unavailable" の HTTP エラーのトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="cfc19-358">Troubleshoot HTTP errors of "502 bad gateway" and "503 service unavailable" in your Azure web apps</span></span>](/azure/app-service/app-service-web-troubleshoot-http-502-http-503)
* [<span data-ttu-id="cfc19-359">Azure App Service での Web アプリのパフォーマンス低下に関する問題のトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="cfc19-359">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [<span data-ttu-id="cfc19-360">Azure での Web アプリのアプリケーションパフォーマンスに関するよくあるご質問</span><span class="sxs-lookup"><span data-stu-id="cfc19-360">Application performance FAQs for Web Apps in Azure</span></span>](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* [<span data-ttu-id="cfc19-361">Azure Web アプリのサンドボックス (App Service ランタイムの実行の制限)</span><span class="sxs-lookup"><span data-stu-id="cfc19-361">Azure Web App sandbox (App Service runtime execution limitations)</span></span>](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)
* [<span data-ttu-id="cfc19-362">Azure Friday: Azure App Service の診断とトラブルシューティング (12 分間のビデオ)</span><span class="sxs-lookup"><span data-stu-id="cfc19-362">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
