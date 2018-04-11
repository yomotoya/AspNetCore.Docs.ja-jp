---
title: Azure App Service での ASP.NET Core のホスト
author: guardrex
description: 役に立つリソースへのリンクを使用して Azure App Service で ASP.NET Core アプリをホストする方法を説明します。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: c2675f73880a41ee75f6ec13155419945387e109
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
# <a name="host-aspnet-core-on-azure-app-service"></a><span data-ttu-id="4c9c9-103">Azure App Service での ASP.NET Core のホスト</span><span class="sxs-lookup"><span data-stu-id="4c9c9-103">Host ASP.NET Core on Azure App Service</span></span>

<span data-ttu-id="4c9c9-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) は ASP.NET Core を含む Web アプリをホストするための [Microsoft クラウド コンピューティング プラットフォーム サービス](https://azure.microsoft.com/)です。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) is a [Microsoft cloud computing platform service](https://azure.microsoft.com/) for hosting web apps, including ASP.NET Core.</span></span>

## <a name="useful-resources"></a><span data-ttu-id="4c9c9-105">役に立つリソース</span><span class="sxs-lookup"><span data-stu-id="4c9c9-105">Useful resources</span></span>

<span data-ttu-id="4c9c9-106">Azure の「[Web Apps のドキュメント](/azure/app-service/)」は、Azure アプリのドキュメント、チュートリアル、サンプル、ハウツー ガイド、その他のリソースのホームです。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-106">The Azure [Web Apps Documentation](/azure/app-service/) is the home for Azure Apps documentation, tutorials, samples, how-to guides, and other resources.</span></span> <span data-ttu-id="4c9c9-107">ASP.NET Core アプリのホスティングに関連する次の 2 つのチュートリアルは特に重要です。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-107">Two notable tutorials that pertain to hosting ASP.NET Core apps are:</span></span>

[<span data-ttu-id="4c9c9-108">クイック スタート: Azure に ASP.NET Core Web アプリを作成する</span><span class="sxs-lookup"><span data-stu-id="4c9c9-108">Quickstart: Create an ASP.NET Core web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)  
<span data-ttu-id="4c9c9-109">Visual Studio を使用して ASP.NET Core Web アプリを作成し、Windows の Azure App Service に配置します。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-109">Use Visual Studio to create and deploy an ASP.NET Core web app to Azure App Service on Windows.</span></span>

[<span data-ttu-id="4c9c9-110">クイック スタート: App Service on Linux での .NET Core Web アプリの作成</span><span class="sxs-lookup"><span data-stu-id="4c9c9-110">Quickstart: Create a .NET Core web app in App Service on Linux</span></span>](/azure/app-service/containers/quickstart-dotnetcore)  
<span data-ttu-id="4c9c9-111">コマンド ラインを使用して ASP.NET Core Web アプリを作成し、Linux の Azure App Service に配置します。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-111">Use the command line to create and deploy an ASP.NET Core web app to Azure App Service on Linux.</span></span>

<span data-ttu-id="4c9c9-112">ASP.NET Core のドキュメントでは、次の記事を参照できます。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-112">The following articles are available in ASP.NET Core documentation:</span></span>

[<span data-ttu-id="4c9c9-113">Visual Studio による Azure への公開</span><span class="sxs-lookup"><span data-stu-id="4c9c9-113">Publish to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)  
<span data-ttu-id="4c9c9-114">Visual Studio を使用して Azure App Service に ASP.NET Core アプリを発行する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-114">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>

[<span data-ttu-id="4c9c9-115">CLI ツールによる Azure への公開</span><span class="sxs-lookup"><span data-stu-id="4c9c9-115">Publish to Azure with CLI tools</span></span>](xref:tutorials/publish-to-azure-webapp-using-cli)  
<span data-ttu-id="4c9c9-116">Git コマンド ライン クライアントを使用して Azure App Service に ASP.NET Core アプリを発行する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-116">Learn how to publish an ASP.NET Core app to Azure App Service using the Git command-line client.</span></span>

[<span data-ttu-id="4c9c9-117">Visual Studio と Git による Azure への継続的配置</span><span class="sxs-lookup"><span data-stu-id="4c9c9-117">Continuous deployment to Azure with Visual Studio and Git</span></span>](xref:host-and-deploy/azure-apps/azure-continuous-deployment)  
<span data-ttu-id="4c9c9-118">Visual Studio で ASP.NET Core Web アプリを作成し、それを Azure App Service に配置する方法について説明します。Git を利用し、継続的に配置します。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-118">Learn how to create an ASP.NET Core web app using Visual Studio and deploy it to Azure App Service using Git for continuous deployment.</span></span>

[<span data-ttu-id="4c9c9-119">VSTS による Azure への継続的配置</span><span class="sxs-lookup"><span data-stu-id="4c9c9-119">Continuous deployment to Azure with VSTS</span></span>](https://www.visualstudio.com/docs/build/aspnet/core/quick-to-azure)  
<span data-ttu-id="4c9c9-120">ASP.NET Core アプリ用に CI ビルドを設定し、Azure App Service に継続的配置リリースを作成します。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-120">Set up a CI build for an ASP.NET Core app, then create a continuous deployment release to Azure App Service.</span></span>

[<span data-ttu-id="4c9c9-121">Azure Web アプリのサンドボックス</span><span class="sxs-lookup"><span data-stu-id="4c9c9-121">Azure Web App sandbox</span></span>](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
<span data-ttu-id="4c9c9-122">Azure アプリのプラットフォームで適用される Azure App Service ランタイム実行の制限事項について説明します。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-122">Discover Azure App Service runtime execution limitations enforced by the Azure Apps platform.</span></span>

## <a name="application-configuration"></a><span data-ttu-id="4c9c9-123">アプリケーション構成</span><span class="sxs-lookup"><span data-stu-id="4c9c9-123">Application configuration</span></span>

<span data-ttu-id="4c9c9-124">ASP.NET Core 2.0 以降、[Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) の次の 3 つのパッケージから Azure App Service に配置されたアプリの自動ログ記録機能が提供されます。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-124">With ASP.NET Core 2.0 and later, three packages in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) provide automatic logging features for apps deployed to Azure App Service:</span></span>

* <span data-ttu-id="4c9c9-125">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) は [IHostingStartup](xref:host-and-deploy/platform-specific-configuration) を使用して Azure App Service と ASP.NET Core の Light-Up 統合を提供します。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-125">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) uses [IHostingStartup](xref:host-and-deploy/platform-specific-configuration) to provide ASP.NET Core lightup integration with Azure App Service.</span></span> <span data-ttu-id="4c9c9-126">追加されるログ記録機能は `Microsoft.AspNetCore.AzureAppServicesIntegration` パッケージによって提供されます。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-126">The added logging features are provided by the `Microsoft.AspNetCore.AzureAppServicesIntegration` package.</span></span>
* <span data-ttu-id="4c9c9-127">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) は [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) を実行して、`Microsoft.Extensions.Logging.AzureAppServices` パッケージに Azure App Service 診断ログ記録プロバイダーを追加します。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-127">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.</span></span>
* <span data-ttu-id="4c9c9-128">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) はロガー実装を提供することで、Azure App Service 診断ログとログ ストリーミング機能をサポートします。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-128">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) provides logger implementations to support Azure App Service diagnostics logs and log streaming features.</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="4c9c9-129">プロキシ サーバーとロード バランサーのシナリオ</span><span class="sxs-lookup"><span data-stu-id="4c9c9-129">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="4c9c9-130">要求が発生したスキーム (HTTP/HTTPS) とリモート IP アドレスを転送するために、Forwarded Headers Middleware を構成する IIS 統合ミドルウェアと、ASP.NET Core モジュールが構成されます。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-130">The IIS Integration Middleware, which configures Forwarded Headers Middleware, and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="4c9c9-131">追加のプロキシ サーバーとロード バランサーの背後でホストされているアプリでは、追加の構成が必要になる場合があります。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-131">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="4c9c9-132">詳細については、「[プロキシ サーバーとロード バランサーを使用するために ASP.NET Core を構成する](xref:host-and-deploy/proxy-load-balancer)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-132">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="monitoring-and-logging"></a><span data-ttu-id="4c9c9-133">監視およびログ記録</span><span class="sxs-lookup"><span data-stu-id="4c9c9-133">Monitoring and logging</span></span>

<span data-ttu-id="4c9c9-134">監視、ログ記録、トラブルシューティングに関する情報は、次の記事を参照してください。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-134">For monitoring, logging, and troubleshooting information, see the following articles:</span></span>

[<span data-ttu-id="4c9c9-135">Azure App Service でアプリを監視する方法</span><span class="sxs-lookup"><span data-stu-id="4c9c9-135">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)  
<span data-ttu-id="4c9c9-136">アプリと App Service プランに関するクォータとメトリックを確認する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-136">Learn how to review quotas and metrics for apps and App Service plans.</span></span>

[<span data-ttu-id="4c9c9-137">Azure App Service の Web アプリの診断ログの有効化</span><span class="sxs-lookup"><span data-stu-id="4c9c9-137">Enable diagnostics logging for web apps in Azure App Service</span></span>](/azure/app-service/web-sites-enable-diagnostic-log)  
<span data-ttu-id="4c9c9-138">HTTP 状態コード、失敗した要求、Web サーバー アクティビティの診断ログを有効にしてアクセスする方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-138">Discover how to enable and access diagnostic logging for HTTP status codes, failed requests, and web server activity.</span></span>

[<span data-ttu-id="4c9c9-139">ASP.NET Core でのエラー処理の概要</span><span class="sxs-lookup"><span data-stu-id="4c9c9-139">Introduction to Error Handling in ASP.NET Core</span></span>](xref:fundamentals/error-handling)  
<span data-ttu-id="4c9c9-140">ASP.NET Core アプリでエラーを処理するための一般的な手法について理解します。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-140">Understand common appoaches to handling errors in ASP.NET Core apps.</span></span>

[<span data-ttu-id="4c9c9-141">Azure App Service での ASP.NET Core のトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="4c9c9-141">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)  
<span data-ttu-id="4c9c9-142">ASP.NET Core アプリを使用した Azure App Service の配置に関する問題を診断する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-142">Learn how to diagnose issues with Azure App Service deployments with ASP.NET Core apps.</span></span>

[<span data-ttu-id="4c9c9-143">Azure App Service および IIS と ASP.NET Core の一般的なエラーのリファレンス</span><span class="sxs-lookup"><span data-stu-id="4c9c9-143">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)  
<span data-ttu-id="4c9c9-144">Azure App Service/IIS によってホストされるアプリの一般的な配置の構成エラーのトラブルシューティングに関するアドバイスを参照してください。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-144">See the common deployment configuration errors for apps hosted by Azure App Service/IIS with troubleshooting advice.</span></span>

## <a name="data-protection-key-ring-and-deployment-slots"></a><span data-ttu-id="4c9c9-145">データ保護キー リングと展開スロット</span><span class="sxs-lookup"><span data-stu-id="4c9c9-145">Data Protection key ring and deployment slots</span></span>

<span data-ttu-id="4c9c9-146">[データ保護キー](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)は *%HOME%\ASP.NET\DataProtection-Keys* フォルダーに保存されます。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-146">[Data Protection keys](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) are persisted to the *%HOME%\ASP.NET\DataProtection-Keys* folder.</span></span> <span data-ttu-id="4c9c9-147">このフォルダーはネットワーク ストレージにバックアップされ、アプリをホストしているすべてのマシンで同期されています。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-147">This folder is backed by network storage and is synchronized across all machines hosting the app.</span></span> <span data-ttu-id="4c9c9-148">保存中のキーは保護されていません。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-148">Keys aren't protected at rest.</span></span> <span data-ttu-id="4c9c9-149">このフォルダーから、単一の展開スロットのアプリのすべてのインスタンスにキー リングが提供されます。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-149">This folder supplies the key ring to all instances of an app in a single deployment slot.</span></span> <span data-ttu-id="4c9c9-150">ステージングや運用などの別の展開スロットでは、キー リングが共有されません。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-150">Separate deployment slots, such as Staging and Production, don't share a key ring.</span></span>

<span data-ttu-id="4c9c9-151">展開スロットを切り替えると、データ保護を使用しているすべてのシステムが前のスロット内のキー リングを使用して格納されたデータを復号化できなくなります。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-151">When swapping between deployment slots, any system using data protection won't be able to decrypt stored data using the key ring inside the previous slot.</span></span> <span data-ttu-id="4c9c9-152">ASP.NET Cookie ミドルウェアは、その Cookie の保護にデータ保護を使用します。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-152">ASP.NET Cookie Middleware uses data protection to protect its cookies.</span></span> <span data-ttu-id="4c9c9-153">これにより、ユーザーが標準の ASP.NET Cookie ミドルウェアを使用するアプリからサインアウトします。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-153">This leads to users being signed out of an app that uses the standard ASP.NET Cookie Middleware.</span></span> <span data-ttu-id="4c9c9-154">スロットに依存しないキー リング ソリューションの場合、次のような外部キー リング プロバイダーを使用します。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-154">For a slot-independent key ring solution, use an external key ring provider, such as:</span></span>

* <span data-ttu-id="4c9c9-155">Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="4c9c9-155">Azure Blob Storage</span></span>
* <span data-ttu-id="4c9c9-156">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="4c9c9-156">Azure Key Vault</span></span>
* <span data-ttu-id="4c9c9-157">SQL ストア</span><span class="sxs-lookup"><span data-stu-id="4c9c9-157">SQL store</span></span>
* <span data-ttu-id="4c9c9-158">Redis Cache</span><span class="sxs-lookup"><span data-stu-id="4c9c9-158">Redis cache</span></span>

<span data-ttu-id="4c9c9-159">詳細については、「[キー記憶域プロバイダー](xref:security/data-protection/implementation/key-storage-providers)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-159">For more information, see [Key storage providers](xref:security/data-protection/implementation/key-storage-providers).</span></span>

## <a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a><span data-ttu-id="4c9c9-160">Azure App Service に ASP.NET Core プレビュー リリースを展開する</span><span class="sxs-lookup"><span data-stu-id="4c9c9-160">Deploy ASP.NET Core preview release to Azure App Service</span></span>

<span data-ttu-id="4c9c9-161">ASP.NET Core プレビュー アプリは、次の方法で Azure App Service に展開できます。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-161">ASP.NET Core preview apps can be deployed to Azure App Service with the following approaches:</span></span>

* [<span data-ttu-id="4c9c9-162">プレビュー サイト拡張機能をインストールする</span><span class="sxs-lookup"><span data-stu-id="4c9c9-162">Install the preview site extention</span></span>](#site-x)
* [<span data-ttu-id="4c9c9-163">自己完結型アプリを展開する</span><span class="sxs-lookup"><span data-stu-id="4c9c9-163">Deploy the app self contained</span></span>](#self)
* [<span data-ttu-id="4c9c9-164">コンテナー用の Web アプリで Docker を使用する</span><span class="sxs-lookup"><span data-stu-id="4c9c9-164">Use Docker with Web Apps for containers</span></span>](#docker)

<span data-ttu-id="4c9c9-165">プレビュー サイト拡張機能の使用に問題がある場合は、[GitHub](https://github.com/aspnet/azureintegration/issues/new) に問題を投稿してください。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-165">If you have a problem using the preview site extension, open an issue on [GitHub](https://github.com/aspnet/azureintegration/issues/new).</span></span>

<a name="site-x"></a>
### <a name="install-the-preview-site-extention"></a><span data-ttu-id="4c9c9-166">プレビュー サイト拡張機能をインストールする</span><span class="sxs-lookup"><span data-stu-id="4c9c9-166">Install the preview site extention</span></span>

* <span data-ttu-id="4c9c9-167">Azure Portal から [App Service] ブレードに移動します。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-167">From the Azure portal, navigate to the App Service blade.</span></span>
* <span data-ttu-id="4c9c9-168">検索ボックスに「ex」と入力します。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-168">Enter "ex" in the search box.</span></span>
* <span data-ttu-id="4c9c9-169">**[拡張機能]** を選びます。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-169">Select **Extensions**.</span></span>
* <span data-ttu-id="4c9c9-170">[追加] を選びます。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-170">Select "Add".</span></span>

![前の手順の [Azure App] ブレード](index/_static/x1.png)

* <span data-ttu-id="4c9c9-172">**[ASP.NET Core Runtime Extensions]\(ASP.NET Core ランタイム拡張機能\)** を選びます。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-172">Select **ASP.NET Core Runtime Extensions**.</span></span>
* <span data-ttu-id="4c9c9-173">**[OK]** > **[OK]** の順に選びます。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-173">Select **OK** > **OK**.</span></span>

<span data-ttu-id="4c9c9-174">追加操作が完了すると、最新の .NET Core 2.1 プレビューがインストールされます。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-174">When the add operations completes, the latest .NET Core 2.1 preview is installed.</span></span> <span data-ttu-id="4c9c9-175">コンソールで `dotnet --info` を実行すると、インストールを確認することができます。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-175">You can verify the installation by running `dotnet --info` in the console.</span></span> <span data-ttu-id="4c9c9-176">[App Service] ブレードから: </span><span class="sxs-lookup"><span data-stu-id="4c9c9-176">From the App Service blade:</span></span>

* <span data-ttu-id="4c9c9-177">検索ボックスに「con」と入力します。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-177">Enter "con" in the search box.</span></span>
* <span data-ttu-id="4c9c9-178">**[コンソール]** を選びます。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-178">Select **Console**.</span></span>
* <span data-ttu-id="4c9c9-179">コンソールに `dotnet --info` と入力します。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-179">Enter `dotnet --info` in the console.</span></span>

![前の手順の [Azure App] ブレード](index/_static/cons.png)

<span data-ttu-id="4c9c9-181">前の画像は、これが書き込まれた時点での最新です。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-181">The preceding image was current at the time this was written.</span></span> <span data-ttu-id="4c9c9-182">別のバージョンが表示される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-182">You may see a different version.</span></span>

<span data-ttu-id="4c9c9-183">`dotnet --info` では、プレビューがインストールされているサイトの拡張機能へのパスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-183">The `dotnet --info` displays the the path to the site extension where the Preview has been installed.</span></span> <span data-ttu-id="4c9c9-184">アプリが既定の *ProgramFiles* の場所ではなく、サイトの拡張機能から実行されていることが示されます。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-184">It shows the app is running from the site extension instead of from the default *ProgramFiles* location.</span></span> <span data-ttu-id="4c9c9-185">*ProgramFiles* が表示される場合は、サイトを再起動して、`dotnet --info` を実行してください。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-185">If you see *ProgramFiles*, restart the site and run `dotnet --info`.</span></span>

#### <a name="use-the-preview-site-extention-with-an-arm-template"></a><span data-ttu-id="4c9c9-186">ARM テンプレートでプレビュー サイト拡張機能を使用する</span><span class="sxs-lookup"><span data-stu-id="4c9c9-186">Use the preview site extention with an ARM template</span></span>

<span data-ttu-id="4c9c9-187">ARM テンプレートを使用してアプリケーションを作成し、展開している場合は、リソースの種類に `siteextensions` を使用してサイト拡張機能を Web アプリに追加することができます。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-187">If you are using an ARM template to create and deploy applications you can use the `siteextensions` resource type to add the site extension to a Web App.</span></span> <span data-ttu-id="4c9c9-188">例:</span><span class="sxs-lookup"><span data-stu-id="4c9c9-188">For example:</span></span>

[!code-json[Main](index/sample/arm.json?highlight=2)]

<a name="self"></a>
### <a name="deploy-the-app-self-contained"></a><span data-ttu-id="4c9c9-189">自己完結型アプリを展開する</span><span class="sxs-lookup"><span data-stu-id="4c9c9-189">Deploy the app self contained</span></span>

<span data-ttu-id="4c9c9-190">展開時にプレビューのランタイムが保持される[自己完結型アプリ](/dotnet/core/deploying/#self-contained-deployments-scd)を展開することができます。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-190">You can deploy a [self-contained app](/dotnet/core/deploying/#self-contained-deployments-scd) that carries the preview runtime with it when being deployed.</span></span> <span data-ttu-id="4c9c9-191">自己完結型アプリを展開する場合: </span><span class="sxs-lookup"><span data-stu-id="4c9c9-191">When deploying a self contained app:</span></span>

* <span data-ttu-id="4c9c9-192">サイトを準備する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-192">You don’t need to prepare your site.</span></span>
* <span data-ttu-id="4c9c9-193">SDK がサーバーにインストールされたら、アプリを展開する場合と違う方法でアプリケーションを公開する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-193">Requires you to publish your application differently than you would when deploying an app once the SDK is installed on the server.</span></span>

<span data-ttu-id="4c9c9-194">自己完結型のアプリは、すべての .NET Core アプリケーションに対するオプションです。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-194">Self-contained apps are an option for all .NET Core applications.</span></span>

<a name="docker"></a>
### <a name="use-docker-with-web-apps-for-containers"></a><span data-ttu-id="4c9c9-195">コンテナー用の Web アプリで Docker を使用する</span><span class="sxs-lookup"><span data-stu-id="4c9c9-195">Use Docker with Web Apps for containers</span></span>

<span data-ttu-id="4c9c9-196">[Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) には最新の 2.1 プレビュー Docker イメージが含まれています。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-196">The [Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) contains the latest 2.1 preview Docker images.</span></span> <span data-ttu-id="4c9c9-197">このイメージを基本イメージとして使用して、通常どおりにコンテナー用の Web アプリに展開できます。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-197">You can use them as your base image and deploy to Web Apps for Containers as you normally would.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4c9c9-198">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="4c9c9-198">Additional resources</span></span>

* [<span data-ttu-id="4c9c9-199">Web Apps の概要 (5 分間の概要ビデオ)</span><span class="sxs-lookup"><span data-stu-id="4c9c9-199">Web Apps overview (5-minute overview video)</span></span>](/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="4c9c9-200">Azure App Service: .NET アプリのホスティングに最適な場所 (55 分間の概要ビデオ)</span><span class="sxs-lookup"><span data-stu-id="4c9c9-200">Azure App Service: The Best Place to Host your .NET Apps (55-minute overview video)</span></span>](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [<span data-ttu-id="4c9c9-201">Azure Friday: Azure App Service の診断とトラブルシューティング (12 分間のビデオ)</span><span class="sxs-lookup"><span data-stu-id="4c9c9-201">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [<span data-ttu-id="4c9c9-202">Azure App Service の診断の概要</span><span class="sxs-lookup"><span data-stu-id="4c9c9-202">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)

<span data-ttu-id="4c9c9-203">Windows Server の Azure App Service では[インターネット インフォメーション サービス (IIS)](https://www.iis.net/) が使用されます。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-203">Azure App Service on Windows Server uses [Internet Information Services (IIS)](https://www.iis.net/).</span></span> <span data-ttu-id="4c9c9-204">次のトピックは、基になる IIS テクノロジと関連しています。</span><span class="sxs-lookup"><span data-stu-id="4c9c9-204">The following topics pertain to the underlying IIS technology:</span></span>

* [<span data-ttu-id="4c9c9-205">IIS を使用した Windows での ASP.NET Core のホスト</span><span class="sxs-lookup"><span data-stu-id="4c9c9-205">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="4c9c9-206">ASP.NET Core モジュールの概要</span><span class="sxs-lookup"><span data-stu-id="4c9c9-206">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="4c9c9-207">ASP.NET Core モジュール構成リファレンス</span><span class="sxs-lookup"><span data-stu-id="4c9c9-207">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="4c9c9-208">IIS モジュールと ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4c9c9-208">IIS Modules with ASP.NET Core</span></span>](xref:host-and-deploy/iis/modules)
* [<span data-ttu-id="4c9c9-209">Microsoft TechNet ライブラリ: Windows Server</span><span class="sxs-lookup"><span data-stu-id="4c9c9-209">Microsoft TechNet Library: Windows Server</span></span>](/windows-server/windows-server-versions)
