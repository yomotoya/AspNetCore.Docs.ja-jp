---
title: Azure App Service での ASP.NET Core のホスト
author: guardrex
description: 役に立つリソースへのリンクを使用して Azure App Service で ASP.NET Core アプリをホストする方法を説明します。
ms.author: riande
ms.custom: mvc
ms.date: 07/24/2018
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: ece61a3e362ec5e2ff8f415351a0f9257fc72098
ms.sourcegitcommit: b4c7b1a4c48dec0865f27874275c73da1f75e918
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/24/2018
ms.locfileid: "39228612"
---
# <a name="host-aspnet-core-on-azure-app-service"></a><span data-ttu-id="7b93b-103">Azure App Service での ASP.NET Core のホスト</span><span class="sxs-lookup"><span data-stu-id="7b93b-103">Host ASP.NET Core on Azure App Service</span></span>

<span data-ttu-id="7b93b-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) は ASP.NET Core を含む Web アプリをホストするための [Microsoft クラウド コンピューティング プラットフォーム サービス](https://azure.microsoft.com/)です。</span><span class="sxs-lookup"><span data-stu-id="7b93b-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) is a [Microsoft cloud computing platform service](https://azure.microsoft.com/) for hosting web apps, including ASP.NET Core.</span></span>

## <a name="useful-resources"></a><span data-ttu-id="7b93b-105">役に立つリソース</span><span class="sxs-lookup"><span data-stu-id="7b93b-105">Useful resources</span></span>

<span data-ttu-id="7b93b-106">Azure の「[Web Apps のドキュメント](/azure/app-service/)」は、Azure アプリのドキュメント、チュートリアル、サンプル、ハウツー ガイド、その他のリソースのホームです。</span><span class="sxs-lookup"><span data-stu-id="7b93b-106">The Azure [Web Apps Documentation](/azure/app-service/) is the home for Azure Apps documentation, tutorials, samples, how-to guides, and other resources.</span></span> <span data-ttu-id="7b93b-107">ASP.NET Core アプリのホスティングに関連する次の 2 つのチュートリアルは特に重要です。</span><span class="sxs-lookup"><span data-stu-id="7b93b-107">Two notable tutorials that pertain to hosting ASP.NET Core apps are:</span></span>

[<span data-ttu-id="7b93b-108">クイック スタート: Azure に ASP.NET Core Web アプリを作成する</span><span class="sxs-lookup"><span data-stu-id="7b93b-108">Quickstart: Create an ASP.NET Core web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)  
<span data-ttu-id="7b93b-109">Visual Studio を使用して ASP.NET Core Web アプリを作成し、Windows の Azure App Service に配置します。</span><span class="sxs-lookup"><span data-stu-id="7b93b-109">Use Visual Studio to create and deploy an ASP.NET Core web app to Azure App Service on Windows.</span></span>

[<span data-ttu-id="7b93b-110">クイック スタート: App Service on Linux での .NET Core Web アプリの作成</span><span class="sxs-lookup"><span data-stu-id="7b93b-110">Quickstart: Create a .NET Core web app in App Service on Linux</span></span>](/azure/app-service/containers/quickstart-dotnetcore)  
<span data-ttu-id="7b93b-111">コマンド ラインを使用して ASP.NET Core Web アプリを作成し、Linux の Azure App Service に配置します。</span><span class="sxs-lookup"><span data-stu-id="7b93b-111">Use the command line to create and deploy an ASP.NET Core web app to Azure App Service on Linux.</span></span>

<span data-ttu-id="7b93b-112">ASP.NET Core のドキュメントでは、次の記事を参照できます。</span><span class="sxs-lookup"><span data-stu-id="7b93b-112">The following articles are available in ASP.NET Core documentation:</span></span>

[<span data-ttu-id="7b93b-113">Visual Studio による Azure への公開</span><span class="sxs-lookup"><span data-stu-id="7b93b-113">Publish to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)  
<span data-ttu-id="7b93b-114">Visual Studio を使用して Azure App Service に ASP.NET Core アプリを発行する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="7b93b-114">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>

[<span data-ttu-id="7b93b-115">CLI ツールによる Azure への公開</span><span class="sxs-lookup"><span data-stu-id="7b93b-115">Publish to Azure with CLI tools</span></span>](xref:tutorials/publish-to-azure-webapp-using-cli)  
<span data-ttu-id="7b93b-116">Git コマンド ライン クライアントを使用して Azure App Service に ASP.NET Core アプリを発行する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="7b93b-116">Learn how to publish an ASP.NET Core app to Azure App Service using the Git command-line client.</span></span>

[<span data-ttu-id="7b93b-117">Visual Studio と Git による Azure への継続的配置</span><span class="sxs-lookup"><span data-stu-id="7b93b-117">Continuous deployment to Azure with Visual Studio and Git</span></span>](xref:host-and-deploy/azure-apps/azure-continuous-deployment)  
<span data-ttu-id="7b93b-118">Visual Studio で ASP.NET Core Web アプリを作成し、それを Azure App Service に配置する方法について説明します。Git を利用し、継続的に配置します。</span><span class="sxs-lookup"><span data-stu-id="7b93b-118">Learn how to create an ASP.NET Core web app using Visual Studio and deploy it to Azure App Service using Git for continuous deployment.</span></span>

[<span data-ttu-id="7b93b-119">VSTS による Azure への継続的配置</span><span class="sxs-lookup"><span data-stu-id="7b93b-119">Continuous deployment to Azure with VSTS</span></span>](https://www.visualstudio.com/docs/build/aspnet/core/quick-to-azure)  
<span data-ttu-id="7b93b-120">ASP.NET Core アプリ用に CI ビルドを設定し、Azure App Service に継続的配置リリースを作成します。</span><span class="sxs-lookup"><span data-stu-id="7b93b-120">Set up a CI build for an ASP.NET Core app, then create a continuous deployment release to Azure App Service.</span></span>

[<span data-ttu-id="7b93b-121">Azure Web アプリのサンドボックス</span><span class="sxs-lookup"><span data-stu-id="7b93b-121">Azure Web App sandbox</span></span>](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
<span data-ttu-id="7b93b-122">Azure アプリのプラットフォームで適用される Azure App Service ランタイム実行の制限事項について説明します。</span><span class="sxs-lookup"><span data-stu-id="7b93b-122">Discover Azure App Service runtime execution limitations enforced by the Azure Apps platform.</span></span>

::: moniker range=">= aspnetcore-2.0"

## <a name="application-configuration"></a><span data-ttu-id="7b93b-123">アプリケーション構成</span><span class="sxs-lookup"><span data-stu-id="7b93b-123">Application configuration</span></span>

<span data-ttu-id="7b93b-124">ASP.NET Core 2.0 以降では、次の NuGet パッケージで Azure App Service にデプロイされたアプリの自動ログ記録機能が提供されます。</span><span class="sxs-lookup"><span data-stu-id="7b93b-124">In ASP.NET Core 2.0 or later, the following NuGet packages provide automatic logging features for apps deployed to Azure App Service:</span></span>

* <span data-ttu-id="7b93b-125">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) は [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) を使用して Azure App Service と ASP.NET Core の Light-Up 統合を提供します。</span><span class="sxs-lookup"><span data-stu-id="7b93b-125">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) uses [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) to provide ASP.NET Core light-up integration with Azure App Service.</span></span> <span data-ttu-id="7b93b-126">追加されるログ記録機能は `Microsoft.AspNetCore.AzureAppServicesIntegration` パッケージによって提供されます。</span><span class="sxs-lookup"><span data-stu-id="7b93b-126">The added logging features are provided by the `Microsoft.AspNetCore.AzureAppServicesIntegration` package.</span></span>
* <span data-ttu-id="7b93b-127">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) は [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) を実行して、`Microsoft.Extensions.Logging.AzureAppServices` パッケージに Azure App Service 診断ログ記録プロバイダーを追加します。</span><span class="sxs-lookup"><span data-stu-id="7b93b-127">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.</span></span>
* <span data-ttu-id="7b93b-128">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) はロガー実装を提供することで、Azure App Service 診断ログとログ ストリーミング機能をサポートします。</span><span class="sxs-lookup"><span data-stu-id="7b93b-128">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) provides logger implementations to support Azure App Service diagnostics logs and log streaming features.</span></span>

<span data-ttu-id="7b93b-129">.NET Core を対象とし、[Microsoft.AspNetCore.All メタパッケージ](xref:fundamentals/metapackage)を参照している場合、パッケージが既に含まれています。</span><span class="sxs-lookup"><span data-stu-id="7b93b-129">If targeting .NET Core and referencing the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), the packages are already included.</span></span> <span data-ttu-id="7b93b-130">このパッケージは、新しい [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)には含まれません。</span><span class="sxs-lookup"><span data-stu-id="7b93b-130">The packages are absent from the newer [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="7b93b-131">.NET Framework を対象とし、`Microsoft.AspNetCore.App` メタパッケージを参照している場合は、個々のログ記録パッケージを参照します。</span><span class="sxs-lookup"><span data-stu-id="7b93b-131">If targeting .NET Framework or referencing the `Microsoft.AspNetCore.App` metapackage, reference the individual logging packages.</span></span>

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="7b93b-132">プロキシ サーバーとロード バランサーのシナリオ</span><span class="sxs-lookup"><span data-stu-id="7b93b-132">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="7b93b-133">要求が発生したスキーム (HTTP/HTTPS) とリモート IP アドレスを転送するために、Forwarded Headers Middleware を構成する IIS 統合ミドルウェアと、ASP.NET Core モジュールが構成されます。</span><span class="sxs-lookup"><span data-stu-id="7b93b-133">The IIS Integration Middleware, which configures Forwarded Headers Middleware, and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="7b93b-134">追加のプロキシ サーバーとロード バランサーの背後でホストされているアプリでは、追加の構成が必要になる場合があります。</span><span class="sxs-lookup"><span data-stu-id="7b93b-134">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="7b93b-135">詳細については、「[プロキシ サーバーとロード バランサーを使用するために ASP.NET Core を構成する](xref:host-and-deploy/proxy-load-balancer)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="7b93b-135">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="monitoring-and-logging"></a><span data-ttu-id="7b93b-136">監視およびログ記録</span><span class="sxs-lookup"><span data-stu-id="7b93b-136">Monitoring and logging</span></span>

<span data-ttu-id="7b93b-137">監視、ログ記録、トラブルシューティングに関する情報は、次の記事を参照してください。</span><span class="sxs-lookup"><span data-stu-id="7b93b-137">For monitoring, logging, and troubleshooting information, see the following articles:</span></span>

[<span data-ttu-id="7b93b-138">Azure App Service でアプリを監視する方法</span><span class="sxs-lookup"><span data-stu-id="7b93b-138">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)  
<span data-ttu-id="7b93b-139">アプリと App Service プランに関するクォータとメトリックを確認する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="7b93b-139">Learn how to review quotas and metrics for apps and App Service plans.</span></span>

[<span data-ttu-id="7b93b-140">Azure App Service の Web アプリの診断ログの有効化</span><span class="sxs-lookup"><span data-stu-id="7b93b-140">Enable diagnostics logging for web apps in Azure App Service</span></span>](/azure/app-service/web-sites-enable-diagnostic-log)  
<span data-ttu-id="7b93b-141">HTTP 状態コード、失敗した要求、Web サーバー アクティビティの診断ログを有効にしてアクセスする方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="7b93b-141">Discover how to enable and access diagnostic logging for HTTP status codes, failed requests, and web server activity.</span></span>

[<span data-ttu-id="7b93b-142">ASP.NET Core でのエラー処理の概要</span><span class="sxs-lookup"><span data-stu-id="7b93b-142">Introduction to Error Handling in ASP.NET Core</span></span>](xref:fundamentals/error-handling)  
<span data-ttu-id="7b93b-143">ASP.NET Core アプリでエラーを処理するための一般的な手法について理解します。</span><span class="sxs-lookup"><span data-stu-id="7b93b-143">Understand common approaches to handling errors in ASP.NET Core apps.</span></span>

[<span data-ttu-id="7b93b-144">Azure App Service での ASP.NET Core のトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="7b93b-144">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)  
<span data-ttu-id="7b93b-145">ASP.NET Core アプリを使用した Azure App Service の配置に関する問題を診断する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="7b93b-145">Learn how to diagnose issues with Azure App Service deployments with ASP.NET Core apps.</span></span>

[<span data-ttu-id="7b93b-146">Azure App Service および IIS と ASP.NET Core の一般的なエラーのリファレンス</span><span class="sxs-lookup"><span data-stu-id="7b93b-146">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)  
<span data-ttu-id="7b93b-147">Azure App Service/IIS によってホストされるアプリの一般的な配置の構成エラーのトラブルシューティングに関するアドバイスを参照してください。</span><span class="sxs-lookup"><span data-stu-id="7b93b-147">See the common deployment configuration errors for apps hosted by Azure App Service/IIS with troubleshooting advice.</span></span>

## <a name="data-protection-key-ring-and-deployment-slots"></a><span data-ttu-id="7b93b-148">データ保護キー リングとデプロイ スロット</span><span class="sxs-lookup"><span data-stu-id="7b93b-148">Data Protection key ring and deployment slots</span></span>

<span data-ttu-id="7b93b-149">[データ保護キー](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)は *%HOME%\ASP.NET\DataProtection-Keys* フォルダーに保存されます。</span><span class="sxs-lookup"><span data-stu-id="7b93b-149">[Data Protection keys](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) are persisted to the *%HOME%\ASP.NET\DataProtection-Keys* folder.</span></span> <span data-ttu-id="7b93b-150">このフォルダーはネットワーク ストレージにバックアップされ、アプリをホストしているすべてのマシンで同期されています。</span><span class="sxs-lookup"><span data-stu-id="7b93b-150">This folder is backed by network storage and is synchronized across all machines hosting the app.</span></span> <span data-ttu-id="7b93b-151">保存中のキーは保護されていません。</span><span class="sxs-lookup"><span data-stu-id="7b93b-151">Keys aren't protected at rest.</span></span> <span data-ttu-id="7b93b-152">このフォルダーから、単一のデプロイ スロットのアプリのすべてのインスタンスにキー リングが提供されます。</span><span class="sxs-lookup"><span data-stu-id="7b93b-152">This folder supplies the key ring to all instances of an app in a single deployment slot.</span></span> <span data-ttu-id="7b93b-153">ステージングや運用などの別のデプロイ スロットでは、キー リングが共有されません。</span><span class="sxs-lookup"><span data-stu-id="7b93b-153">Separate deployment slots, such as Staging and Production, don't share a key ring.</span></span>

<span data-ttu-id="7b93b-154">デプロイ スロットを切り替えると、データ保護を使用しているすべてのシステムが前のスロット内のキー リングを使用して格納されたデータを復号化できなくなります。</span><span class="sxs-lookup"><span data-stu-id="7b93b-154">When swapping between deployment slots, any system using data protection won't be able to decrypt stored data using the key ring inside the previous slot.</span></span> <span data-ttu-id="7b93b-155">ASP.NET Cookie ミドルウェアは、その Cookie の保護にデータ保護を使用します。</span><span class="sxs-lookup"><span data-stu-id="7b93b-155">ASP.NET Cookie Middleware uses data protection to protect its cookies.</span></span> <span data-ttu-id="7b93b-156">これにより、ユーザーが標準の ASP.NET Cookie ミドルウェアを使用するアプリからサインアウトします。</span><span class="sxs-lookup"><span data-stu-id="7b93b-156">This leads to users being signed out of an app that uses the standard ASP.NET Cookie Middleware.</span></span> <span data-ttu-id="7b93b-157">スロットに依存しないキー リング ソリューションの場合、次のような外部キー リング プロバイダーを使用します。</span><span class="sxs-lookup"><span data-stu-id="7b93b-157">For a slot-independent key ring solution, use an external key ring provider, such as:</span></span>

* <span data-ttu-id="7b93b-158">Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="7b93b-158">Azure Blob Storage</span></span>
* <span data-ttu-id="7b93b-159">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="7b93b-159">Azure Key Vault</span></span>
* <span data-ttu-id="7b93b-160">SQL ストア</span><span class="sxs-lookup"><span data-stu-id="7b93b-160">SQL store</span></span>
* <span data-ttu-id="7b93b-161">Redis Cache</span><span class="sxs-lookup"><span data-stu-id="7b93b-161">Redis cache</span></span>

<span data-ttu-id="7b93b-162">詳細については、「[キー記憶域プロバイダー](xref:security/data-protection/implementation/key-storage-providers)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="7b93b-162">For more information, see [Key storage providers](xref:security/data-protection/implementation/key-storage-providers).</span></span>

## <a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a><span data-ttu-id="7b93b-163">Azure App Service に ASP.NET Core プレビュー リリースを展開する</span><span class="sxs-lookup"><span data-stu-id="7b93b-163">Deploy ASP.NET Core preview release to Azure App Service</span></span>

<span data-ttu-id="7b93b-164">ASP.NET Core プレビュー アプリは、次の方法で Azure App Service に展開できます。</span><span class="sxs-lookup"><span data-stu-id="7b93b-164">ASP.NET Core preview apps can be deployed to Azure App Service with the following approaches:</span></span>

* [<span data-ttu-id="7b93b-165">プレビュー サイト拡張機能をインストールする</span><span class="sxs-lookup"><span data-stu-id="7b93b-165">Install the preview site extension</span></span>](#install-the-preview-site-extension)
* [<span data-ttu-id="7b93b-166">自己完結型アプリを展開する</span><span class="sxs-lookup"><span data-stu-id="7b93b-166">Deploy the app self-contained</span></span>](#deploy-the-app-self-contained)
* [<span data-ttu-id="7b93b-167">コンテナー用の Web アプリで Docker を使用する</span><span class="sxs-lookup"><span data-stu-id="7b93b-167">Use Docker with Web Apps for containers</span></span>](#use-docker-with-web-apps-for-containers)

<span data-ttu-id="7b93b-168">プレビュー サイト拡張機能の使用に関する問題が発生した場合は、[GitHub](https://github.com/aspnet/azureintegration/issues/new) に問題を投稿してください。</span><span class="sxs-lookup"><span data-stu-id="7b93b-168">If a problem occurs using the preview site extension, open an issue on [GitHub](https://github.com/aspnet/azureintegration/issues/new).</span></span>

### <a name="install-the-preview-site-extension"></a><span data-ttu-id="7b93b-169">プレビュー サイト拡張機能をインストールする</span><span class="sxs-lookup"><span data-stu-id="7b93b-169">Install the preview site extension</span></span>

1. <span data-ttu-id="7b93b-170">Azure Portal から [App Service] ブレードに移動します。</span><span class="sxs-lookup"><span data-stu-id="7b93b-170">From the Azure portal, navigate to the App Service blade.</span></span>
1. <span data-ttu-id="7b93b-171">Web アプリを選択します。</span><span class="sxs-lookup"><span data-stu-id="7b93b-171">Select the web app.</span></span>
1. <span data-ttu-id="7b93b-172">検索ボックスに「ex」と入力するか、管理ウィンドウの一覧を **[開発ツール]** までスクロール ダウンします。</span><span class="sxs-lookup"><span data-stu-id="7b93b-172">Enter "ex" in the search box or scroll down the list of management panes to **DEVELOPMENT TOOLS**.</span></span>
1. <span data-ttu-id="7b93b-173">**[開発ツール]** > 、**[拡張機能]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="7b93b-173">Select **DEVELOPMENT TOOLS** > **Extensions**.</span></span>
1. <span data-ttu-id="7b93b-174">**[追加]** を選びます。</span><span class="sxs-lookup"><span data-stu-id="7b93b-174">Select **Add**.</span></span>

   ![前の手順の [Azure App] ブレード](index/_static/x1.png)

1. <span data-ttu-id="7b93b-176">**[ASP.NET Core Extensions]\(ASP.NET Core 拡張機能\)** を選びます。</span><span class="sxs-lookup"><span data-stu-id="7b93b-176">Select **ASP.NET Core Extensions**.</span></span>
1. <span data-ttu-id="7b93b-177">**[OK]** を選んで法的条項に同意します。</span><span class="sxs-lookup"><span data-stu-id="7b93b-177">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="7b93b-178">**[OK]** を選択し、拡張機能をインストールします。</span><span class="sxs-lookup"><span data-stu-id="7b93b-178">Select **OK** to install the extension.</span></span>

<span data-ttu-id="7b93b-179">追加操作が完了すると、最新の .NET Core プレビューがインストールされます。</span><span class="sxs-lookup"><span data-stu-id="7b93b-179">When the add operations complete, the latest .NET Core preview is installed.</span></span> <span data-ttu-id="7b93b-180">コンソールで `dotnet --info` を実行してインストールを確認します。</span><span class="sxs-lookup"><span data-stu-id="7b93b-180">Verify the installation by running `dotnet --info` in the console.</span></span> <span data-ttu-id="7b93b-181">**[App Service]** ブレードから: </span><span class="sxs-lookup"><span data-stu-id="7b93b-181">From the **App Service** blade:</span></span>

1. <span data-ttu-id="7b93b-182">検索ボックスに「con」と入力するか、管理ウィンドウの一覧を **[開発ツール]** までスクロール ダウンします。</span><span class="sxs-lookup"><span data-stu-id="7b93b-182">Enter "con" in the search box or scroll down the list of management panes to **DEVELOPMENT TOOLS**.</span></span>
1. <span data-ttu-id="7b93b-183">**[開発ツール]** > 、**[コンソール]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="7b93b-183">Select **DEVELOPMENT TOOLS** > **Console**.</span></span>
1. <span data-ttu-id="7b93b-184">コンソールに `dotnet --info` と入力します。</span><span class="sxs-lookup"><span data-stu-id="7b93b-184">Enter `dotnet --info` in the console.</span></span>

<span data-ttu-id="7b93b-185">バージョン `2.1.300-preview1-008174` が最新のプレビュー リリースである場合、コマンド プロンプトで `dotnet --info` を実行すると、次が出力されます。</span><span class="sxs-lookup"><span data-stu-id="7b93b-185">If version `2.1.300-preview1-008174` is the latest preview release, the following output is obtained by running `dotnet --info` at the command prompt:</span></span>

![前の手順の [Azure App] ブレード](index/_static/cons.png)

<span data-ttu-id="7b93b-187">上の図の ASP.NET Core のバージョン、`2.1.300-preview1-008174` は例です。</span><span class="sxs-lookup"><span data-stu-id="7b93b-187">The version of ASP.NET Core shown in the preceding image, `2.1.300-preview1-008174`, is an example.</span></span> <span data-ttu-id="7b93b-188">`dotnet --info` を実行すると、サイト拡張機能が構成されたときの最新の ASP.NET Core のプレビュー バージョンが表示されます。</span><span class="sxs-lookup"><span data-stu-id="7b93b-188">The latest preview version of ASP.NET Core at the time the site extension is configured appears when you execute `dotnet --info`.</span></span>

<span data-ttu-id="7b93b-189">`dotnet --info` では、プレビューがインストールされているサイトの拡張機能へのパスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="7b93b-189">The `dotnet --info` displays the the path to the site extension where the Preview has been installed.</span></span> <span data-ttu-id="7b93b-190">アプリが既定の *ProgramFiles* の場所ではなく、サイトの拡張機能から実行されていることが示されます。</span><span class="sxs-lookup"><span data-stu-id="7b93b-190">It shows the app is running from the site extension instead of from the default *ProgramFiles* location.</span></span> <span data-ttu-id="7b93b-191">*ProgramFiles* が表示される場合は、サイトを再起動して、`dotnet --info` を実行してください。</span><span class="sxs-lookup"><span data-stu-id="7b93b-191">If you see *ProgramFiles*, restart the site and run `dotnet --info`.</span></span>

<span data-ttu-id="7b93b-192">**ARM テンプレートでプレビュー サイト拡張機能を使用する**</span><span class="sxs-lookup"><span data-stu-id="7b93b-192">**Use the preview site extension with an ARM template**</span></span>

<span data-ttu-id="7b93b-193">ARM テンプレートを使用してアプリを作成し、展開する場合は、リソースの種類として `siteextensions` を使用してサイト拡張機能を Web アプリに追加することができます。</span><span class="sxs-lookup"><span data-stu-id="7b93b-193">If an ARM template is used to create and deploy apps, the `siteextensions` resource type can be used to add the site extension to a web app.</span></span> <span data-ttu-id="7b93b-194">例:</span><span class="sxs-lookup"><span data-stu-id="7b93b-194">For example:</span></span>

[!code-json[Main](index/sample/arm.json?highlight=2)]

### <a name="deploy-the-app-self-contained"></a><span data-ttu-id="7b93b-195">自己完結型アプリを展開する</span><span class="sxs-lookup"><span data-stu-id="7b93b-195">Deploy the app self-contained</span></span>

<span data-ttu-id="7b93b-196">プレビュー ランタイムが展開に保持される[自己完結型アプリ](/dotnet/core/deploying/#self-contained-deployments-scd) を展開できます。</span><span class="sxs-lookup"><span data-stu-id="7b93b-196">A [self-contained app](/dotnet/core/deploying/#self-contained-deployments-scd) can be deployed that carries the preview runtime in the deployment.</span></span> <span data-ttu-id="7b93b-197">自己完結型アプリを展開する場合: </span><span class="sxs-lookup"><span data-stu-id="7b93b-197">When deploying a self-contained app:</span></span>

* <span data-ttu-id="7b93b-198">サイトを準備する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="7b93b-198">The site doesn't need to be prepared.</span></span>
* <span data-ttu-id="7b93b-199">このアプリの発行は、共有ランタイムとホストがサーバー上に置かれるフレームワーク依存の展開に対する発行とは別に行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="7b93b-199">The app must be published differently than when publishing for a framework-dependent deployment with the shared runtime and host on the server.</span></span>

<span data-ttu-id="7b93b-200">自己完結型アプリは、すべての ASP.NET Core アプリに対するオプションです。</span><span class="sxs-lookup"><span data-stu-id="7b93b-200">Self-contained apps are an option for all ASP.NET Core apps.</span></span>

### <a name="use-docker-with-web-apps-for-containers"></a><span data-ttu-id="7b93b-201">コンテナー用の Web アプリで Docker を使用する</span><span class="sxs-lookup"><span data-stu-id="7b93b-201">Use Docker with Web Apps for containers</span></span>

<span data-ttu-id="7b93b-202">[Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) には最新のプレビュー Docker イメージが含まれています。</span><span class="sxs-lookup"><span data-stu-id="7b93b-202">The [Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) contains the latest preview Docker images.</span></span> <span data-ttu-id="7b93b-203">イメージを基本イメージとして使用できます。</span><span class="sxs-lookup"><span data-stu-id="7b93b-203">The images can be used as a base image.</span></span> <span data-ttu-id="7b93b-204">通常は、イメージを使用して、Web App for Containers に展開します。</span><span class="sxs-lookup"><span data-stu-id="7b93b-204">Use the image and deploy to Web Apps for Containers normally.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7b93b-205">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="7b93b-205">Additional resources</span></span>

* [<span data-ttu-id="7b93b-206">Web Apps の概要 (5 分間の概要ビデオ)</span><span class="sxs-lookup"><span data-stu-id="7b93b-206">Web Apps overview (5-minute overview video)</span></span>](/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="7b93b-207">Azure App Service: .NET アプリのホスティングに最適な場所 (55 分間の概要ビデオ)</span><span class="sxs-lookup"><span data-stu-id="7b93b-207">Azure App Service: The Best Place to Host your .NET Apps (55-minute overview video)</span></span>](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [<span data-ttu-id="7b93b-208">Azure Friday: Azure App Service の診断とトラブルシューティング (12 分間のビデオ)</span><span class="sxs-lookup"><span data-stu-id="7b93b-208">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [<span data-ttu-id="7b93b-209">Azure App Service の診断の概要</span><span class="sxs-lookup"><span data-stu-id="7b93b-209">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)
* <xref:host-and-deploy/web-farm>

<span data-ttu-id="7b93b-210">Windows Server の Azure App Service では[インターネット インフォメーション サービス (IIS)](https://www.iis.net/) が使用されます。</span><span class="sxs-lookup"><span data-stu-id="7b93b-210">Azure App Service on Windows Server uses [Internet Information Services (IIS)](https://www.iis.net/).</span></span> <span data-ttu-id="7b93b-211">次のトピックは、基になる IIS テクノロジと関連しています。</span><span class="sxs-lookup"><span data-stu-id="7b93b-211">The following topics pertain to the underlying IIS technology:</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:fundamentals/servers/aspnet-core-module>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>
* [<span data-ttu-id="7b93b-212">Microsoft TechNet ライブラリ: Windows Server</span><span class="sxs-lookup"><span data-stu-id="7b93b-212">Microsoft TechNet Library: Windows Server</span></span>](/windows-server/windows-server-versions)
