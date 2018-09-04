---
title: Azure App Service での ASP.NET Core のホスト
author: guardrex
description: 役に立つリソースへのリンクを使用して Azure App Service で ASP.NET Core アプリをホストする方法を説明します。
ms.author: riande
ms.custom: mvc
ms.date: 08/29/2018
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: bc2a686c5ddc44fded135c9eed5caf676218773a
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/30/2018
ms.locfileid: "43312071"
---
# <a name="host-aspnet-core-on-azure-app-service"></a><span data-ttu-id="f2f7e-103">Azure App Service での ASP.NET Core のホスト</span><span class="sxs-lookup"><span data-stu-id="f2f7e-103">Host ASP.NET Core on Azure App Service</span></span>

<span data-ttu-id="f2f7e-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) は ASP.NET Core を含む Web アプリをホストするための [Microsoft クラウド コンピューティング プラットフォーム サービス](https://azure.microsoft.com/)です。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) is a [Microsoft cloud computing platform service](https://azure.microsoft.com/) for hosting web apps, including ASP.NET Core.</span></span>

## <a name="useful-resources"></a><span data-ttu-id="f2f7e-105">役に立つリソース</span><span class="sxs-lookup"><span data-stu-id="f2f7e-105">Useful resources</span></span>

<span data-ttu-id="f2f7e-106">Azure の「[Web Apps のドキュメント](/azure/app-service/)」は、Azure アプリのドキュメント、チュートリアル、サンプル、ハウツー ガイド、その他のリソースのホームです。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-106">The Azure [Web Apps Documentation](/azure/app-service/) is the home for Azure Apps documentation, tutorials, samples, how-to guides, and other resources.</span></span> <span data-ttu-id="f2f7e-107">ASP.NET Core アプリのホスティングに関連する次の 2 つのチュートリアルは特に重要です。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-107">Two notable tutorials that pertain to hosting ASP.NET Core apps are:</span></span>

[<span data-ttu-id="f2f7e-108">クイック スタート: Azure に ASP.NET Core Web アプリを作成する</span><span class="sxs-lookup"><span data-stu-id="f2f7e-108">Quickstart: Create an ASP.NET Core web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)  
<span data-ttu-id="f2f7e-109">Visual Studio を使用して ASP.NET Core Web アプリを作成し、Windows の Azure App Service に配置します。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-109">Use Visual Studio to create and deploy an ASP.NET Core web app to Azure App Service on Windows.</span></span>

[<span data-ttu-id="f2f7e-110">クイック スタート: App Service on Linux での .NET Core Web アプリの作成</span><span class="sxs-lookup"><span data-stu-id="f2f7e-110">Quickstart: Create a .NET Core web app in App Service on Linux</span></span>](/azure/app-service/containers/quickstart-dotnetcore)  
<span data-ttu-id="f2f7e-111">コマンド ラインを使用して ASP.NET Core Web アプリを作成し、Linux の Azure App Service に配置します。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-111">Use the command line to create and deploy an ASP.NET Core web app to Azure App Service on Linux.</span></span>

<span data-ttu-id="f2f7e-112">ASP.NET Core のドキュメントでは、次の記事を参照できます。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-112">The following articles are available in ASP.NET Core documentation:</span></span>

[<span data-ttu-id="f2f7e-113">Visual Studio による Azure への公開</span><span class="sxs-lookup"><span data-stu-id="f2f7e-113">Publish to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)  
<span data-ttu-id="f2f7e-114">Visual Studio を使用して Azure App Service に ASP.NET Core アプリを発行する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-114">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>

[<span data-ttu-id="f2f7e-115">CLI ツールによる Azure への公開</span><span class="sxs-lookup"><span data-stu-id="f2f7e-115">Publish to Azure with CLI tools</span></span>](xref:tutorials/publish-to-azure-webapp-using-cli)  
<span data-ttu-id="f2f7e-116">Git コマンド ライン クライアントを使用して Azure App Service に ASP.NET Core アプリを発行する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-116">Learn how to publish an ASP.NET Core app to Azure App Service using the Git command-line client.</span></span>

[<span data-ttu-id="f2f7e-117">Visual Studio と Git による Azure への継続的配置</span><span class="sxs-lookup"><span data-stu-id="f2f7e-117">Continuous deployment to Azure with Visual Studio and Git</span></span>](xref:host-and-deploy/azure-apps/azure-continuous-deployment)  
<span data-ttu-id="f2f7e-118">Visual Studio で ASP.NET Core Web アプリを作成し、それを Azure App Service に配置する方法について説明します。Git を利用し、継続的に配置します。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-118">Learn how to create an ASP.NET Core web app using Visual Studio and deploy it to Azure App Service using Git for continuous deployment.</span></span>

[<span data-ttu-id="f2f7e-119">VSTS による Azure への継続的配置</span><span class="sxs-lookup"><span data-stu-id="f2f7e-119">Continuous deployment to Azure with VSTS</span></span>](https://www.visualstudio.com/docs/build/aspnet/core/quick-to-azure)  
<span data-ttu-id="f2f7e-120">ASP.NET Core アプリ用に CI ビルドを設定し、Azure App Service に継続的配置リリースを作成します。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-120">Set up a CI build for an ASP.NET Core app, then create a continuous deployment release to Azure App Service.</span></span>

[<span data-ttu-id="f2f7e-121">Azure Web アプリのサンドボックス</span><span class="sxs-lookup"><span data-stu-id="f2f7e-121">Azure Web App sandbox</span></span>](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
<span data-ttu-id="f2f7e-122">Azure アプリのプラットフォームで適用される Azure App Service ランタイム実行の制限事項について説明します。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-122">Discover Azure App Service runtime execution limitations enforced by the Azure Apps platform.</span></span>

::: moniker range=">= aspnetcore-2.0"

## <a name="application-configuration"></a><span data-ttu-id="f2f7e-123">アプリケーション構成</span><span class="sxs-lookup"><span data-stu-id="f2f7e-123">Application configuration</span></span>

<span data-ttu-id="f2f7e-124">ASP.NET Core 2.0 以降では、次の NuGet パッケージで Azure App Service にデプロイされたアプリの自動ログ記録機能が提供されます。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-124">In ASP.NET Core 2.0 or later, the following NuGet packages provide automatic logging features for apps deployed to Azure App Service:</span></span>

* <span data-ttu-id="f2f7e-125">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) は [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) を使用して Azure App Service と ASP.NET Core の Light-Up 統合を提供します。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-125">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) uses [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) to provide ASP.NET Core light-up integration with Azure App Service.</span></span> <span data-ttu-id="f2f7e-126">追加されるログ記録機能は `Microsoft.AspNetCore.AzureAppServicesIntegration` パッケージによって提供されます。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-126">The added logging features are provided by the `Microsoft.AspNetCore.AzureAppServicesIntegration` package.</span></span>
* <span data-ttu-id="f2f7e-127">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) は [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) を実行して、`Microsoft.Extensions.Logging.AzureAppServices` パッケージに Azure App Service 診断ログ記録プロバイダーを追加します。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-127">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.</span></span>
* <span data-ttu-id="f2f7e-128">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) はロガー実装を提供することで、Azure App Service 診断ログとログ ストリーミング機能をサポートします。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-128">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) provides logger implementations to support Azure App Service diagnostics logs and log streaming features.</span></span>

<span data-ttu-id="f2f7e-129">.NET Core を対象とし、[Microsoft.AspNetCore.All メタパッケージ](xref:fundamentals/metapackage)を参照している場合、パッケージが既に含まれています。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-129">If targeting .NET Core and referencing the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), the packages are already included.</span></span> <span data-ttu-id="f2f7e-130">このパッケージは、新しい [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)には含まれません。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-130">The packages are absent from the newer [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="f2f7e-131">.NET Framework を対象とし、`Microsoft.AspNetCore.App` メタパッケージを参照している場合は、個々のログ記録パッケージを参照します。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-131">If targeting .NET Framework or referencing the `Microsoft.AspNetCore.App` metapackage, reference the individual logging packages.</span></span>

::: moniker-end

## <a name="override-app-configuration-using-the-azure-portal"></a><span data-ttu-id="f2f7e-132">Azure Portal を使用してアプリの構成をオーバーライドする</span><span class="sxs-lookup"><span data-stu-id="f2f7e-132">Override app configuration using the Azure Portal</span></span>

<span data-ttu-id="f2f7e-133">**[アプリケーションの設定]** ブレードの **[アプリの設定]** 領域を使用すると、アプリの環境変数を設定できます。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-133">The **App Settings** area of the **Application settings** blade permits you to set environment variables for the app.</span></span> <span data-ttu-id="f2f7e-134">環境変数は、[環境変数構成プロバイダー](xref:fundamentals/configuration/index#environment-variables-configuration-provider)で使用できます。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-134">Environment variables can be consumed by the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

<span data-ttu-id="f2f7e-135">アプリが [Web ホスト](xref:fundamentals/host/web-host)を使用し、[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) を使用してホストをビルドする場合、ホストを構成する環境変数では `ASPNETCORE_` プレフィックスが使用されます。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-135">When an app uses the [Web Host](xref:fundamentals/host/web-host) and builds the host using [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), environment variables that configure the host use the `ASPNETCORE_` prefix.</span></span> <span data-ttu-id="f2f7e-136">詳細については、<xref:fundamentals/host/web-host> および「[Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider)」(環境変数構成プロバイダー) をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-136">For more information, see <xref:fundamentals/host/web-host> and the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

<span data-ttu-id="f2f7e-137">アプリが[汎用ホスト](xref:fundamentals/host/generic-host)を使用する場合、環境変数は既定ではアプリの構成に読み込まれません。開発者が構成プロバイダーを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-137">When an app uses the [Generic Host](xref:fundamentals/host/generic-host), environment variables aren't loaded into an app's configuration by default and the configuration provider must be added by the developer.</span></span> <span data-ttu-id="f2f7e-138">開発者は、構成プロバーダーを追加する際に環境変数のプレフィックスを決定します。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-138">The developer determines the environment variable prefix when the configuration provider is added.</span></span> <span data-ttu-id="f2f7e-139">詳細については、<xref:fundamentals/host/generic-host> および「[Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider)」(環境変数構成プロバイダー) をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-139">For more information, see <xref:fundamentals/host/generic-host> and the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="f2f7e-140">プロキシ サーバーとロード バランサーのシナリオ</span><span class="sxs-lookup"><span data-stu-id="f2f7e-140">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="f2f7e-141">要求が発生したスキーム (HTTP/HTTPS) とリモート IP アドレスを転送するために、Forwarded Headers Middleware を構成する IIS 統合ミドルウェアと、ASP.NET Core モジュールが構成されます。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-141">The IIS Integration Middleware, which configures Forwarded Headers Middleware, and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="f2f7e-142">追加のプロキシ サーバーとロード バランサーの背後でホストされているアプリでは、追加の構成が必要になる場合があります。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-142">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="f2f7e-143">詳細については、「[プロキシ サーバーとロード バランサーを使用するために ASP.NET Core を構成する](xref:host-and-deploy/proxy-load-balancer)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-143">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="monitoring-and-logging"></a><span data-ttu-id="f2f7e-144">監視およびログ記録</span><span class="sxs-lookup"><span data-stu-id="f2f7e-144">Monitoring and logging</span></span>

<span data-ttu-id="f2f7e-145">監視、ログ記録、トラブルシューティングに関する情報は、次の記事を参照してください。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-145">For monitoring, logging, and troubleshooting information, see the following articles:</span></span>

[<span data-ttu-id="f2f7e-146">Azure App Service でアプリを監視する方法</span><span class="sxs-lookup"><span data-stu-id="f2f7e-146">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)  
<span data-ttu-id="f2f7e-147">アプリと App Service プランに関するクォータとメトリックを確認する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-147">Learn how to review quotas and metrics for apps and App Service plans.</span></span>

[<span data-ttu-id="f2f7e-148">Azure App Service の Web アプリの診断ログの有効化</span><span class="sxs-lookup"><span data-stu-id="f2f7e-148">Enable diagnostics logging for web apps in Azure App Service</span></span>](/azure/app-service/web-sites-enable-diagnostic-log)  
<span data-ttu-id="f2f7e-149">HTTP 状態コード、失敗した要求、Web サーバー アクティビティの診断ログを有効にしてアクセスする方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-149">Discover how to enable and access diagnostic logging for HTTP status codes, failed requests, and web server activity.</span></span>

[<span data-ttu-id="f2f7e-150">ASP.NET Core でのエラー処理の概要</span><span class="sxs-lookup"><span data-stu-id="f2f7e-150">Introduction to Error Handling in ASP.NET Core</span></span>](xref:fundamentals/error-handling)  
<span data-ttu-id="f2f7e-151">ASP.NET Core アプリでエラーを処理するための一般的な手法について理解します。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-151">Understand common approaches to handling errors in ASP.NET Core apps.</span></span>

[<span data-ttu-id="f2f7e-152">Azure App Service での ASP.NET Core のトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="f2f7e-152">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)  
<span data-ttu-id="f2f7e-153">ASP.NET Core アプリを使用した Azure App Service の配置に関する問題を診断する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-153">Learn how to diagnose issues with Azure App Service deployments with ASP.NET Core apps.</span></span>

[<span data-ttu-id="f2f7e-154">Azure App Service および IIS と ASP.NET Core の一般的なエラーのリファレンス</span><span class="sxs-lookup"><span data-stu-id="f2f7e-154">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)  
<span data-ttu-id="f2f7e-155">Azure App Service/IIS によってホストされるアプリの一般的な配置の構成エラーのトラブルシューティングに関するアドバイスを参照してください。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-155">See the common deployment configuration errors for apps hosted by Azure App Service/IIS with troubleshooting advice.</span></span>

## <a name="data-protection-key-ring-and-deployment-slots"></a><span data-ttu-id="f2f7e-156">データ保護キー リングとデプロイ スロット</span><span class="sxs-lookup"><span data-stu-id="f2f7e-156">Data Protection key ring and deployment slots</span></span>

<span data-ttu-id="f2f7e-157">[データ保護キー](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)は *%HOME%\ASP.NET\DataProtection-Keys* フォルダーに保存されます。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-157">[Data Protection keys](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) are persisted to the *%HOME%\ASP.NET\DataProtection-Keys* folder.</span></span> <span data-ttu-id="f2f7e-158">このフォルダーはネットワーク ストレージにバックアップされ、アプリをホストしているすべてのマシンで同期されています。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-158">This folder is backed by network storage and is synchronized across all machines hosting the app.</span></span> <span data-ttu-id="f2f7e-159">保存中のキーは保護されていません。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-159">Keys aren't protected at rest.</span></span> <span data-ttu-id="f2f7e-160">このフォルダーから、単一のデプロイ スロットのアプリのすべてのインスタンスにキー リングが提供されます。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-160">This folder supplies the key ring to all instances of an app in a single deployment slot.</span></span> <span data-ttu-id="f2f7e-161">ステージングや運用などの別のデプロイ スロットでは、キー リングが共有されません。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-161">Separate deployment slots, such as Staging and Production, don't share a key ring.</span></span>

<span data-ttu-id="f2f7e-162">デプロイ スロットを切り替えると、データ保護を使用しているすべてのシステムが前のスロット内のキー リングを使用して格納されたデータを復号化できなくなります。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-162">When swapping between deployment slots, any system using data protection won't be able to decrypt stored data using the key ring inside the previous slot.</span></span> <span data-ttu-id="f2f7e-163">ASP.NET Cookie ミドルウェアは、その Cookie の保護にデータ保護を使用します。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-163">ASP.NET Cookie Middleware uses data protection to protect its cookies.</span></span> <span data-ttu-id="f2f7e-164">これにより、ユーザーが標準の ASP.NET Cookie ミドルウェアを使用するアプリからサインアウトします。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-164">This leads to users being signed out of an app that uses the standard ASP.NET Cookie Middleware.</span></span> <span data-ttu-id="f2f7e-165">スロットに依存しないキー リング ソリューションの場合、次のような外部キー リング プロバイダーを使用します。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-165">For a slot-independent key ring solution, use an external key ring provider, such as:</span></span>

* <span data-ttu-id="f2f7e-166">Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="f2f7e-166">Azure Blob Storage</span></span>
* <span data-ttu-id="f2f7e-167">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="f2f7e-167">Azure Key Vault</span></span>
* <span data-ttu-id="f2f7e-168">SQL ストア</span><span class="sxs-lookup"><span data-stu-id="f2f7e-168">SQL store</span></span>
* <span data-ttu-id="f2f7e-169">Redis Cache</span><span class="sxs-lookup"><span data-stu-id="f2f7e-169">Redis cache</span></span>

<span data-ttu-id="f2f7e-170">詳細については、「[キー記憶域プロバイダー](xref:security/data-protection/implementation/key-storage-providers)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-170">For more information, see [Key storage providers](xref:security/data-protection/implementation/key-storage-providers).</span></span>

## <a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a><span data-ttu-id="f2f7e-171">Azure App Service に ASP.NET Core プレビュー リリースを展開する</span><span class="sxs-lookup"><span data-stu-id="f2f7e-171">Deploy ASP.NET Core preview release to Azure App Service</span></span>

<span data-ttu-id="f2f7e-172">ASP.NET Core プレビュー アプリは、次の方法で Azure App Service に展開できます。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-172">ASP.NET Core preview apps can be deployed to Azure App Service with the following approaches:</span></span>

* <span data-ttu-id="f2f7e-173">[プレビュー サイト拡張機能をインストールする](#install-the-preview-site-extension)
<!-- * [Deploy the app self-contained](#deploy-the-app-self-contained) --></span><span class="sxs-lookup"><span data-stu-id="f2f7e-173">[Install the preview site extension](#install-the-preview-site-extension)
<!-- * [Deploy the app self-contained](#deploy-the-app-self-contained) --></span></span>
* [<span data-ttu-id="f2f7e-174">コンテナー用の Web アプリで Docker を使用する</span><span class="sxs-lookup"><span data-stu-id="f2f7e-174">Use Docker with Web Apps for containers</span></span>](#use-docker-with-web-apps-for-containers)

### <a name="install-the-preview-site-extension"></a><span data-ttu-id="f2f7e-175">プレビュー サイト拡張機能をインストールする</span><span class="sxs-lookup"><span data-stu-id="f2f7e-175">Install the preview site extension</span></span>

<span data-ttu-id="f2f7e-176">プレビュー サイト拡張機能の使用に関する問題が発生した場合は、[GitHub](https://github.com/aspnet/azureintegration/issues/new) に問題を投稿してください。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-176">If a problem occurs using the preview site extension, open an issue on [GitHub](https://github.com/aspnet/azureintegration/issues/new).</span></span>

1. <span data-ttu-id="f2f7e-177">Azure Portal から [App Service] ブレードに移動します。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-177">From the Azure portal, navigate to the App Service blade.</span></span>
1. <span data-ttu-id="f2f7e-178">Web アプリを選択します。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-178">Select the web app.</span></span>
1. <span data-ttu-id="f2f7e-179">検索ボックスに「ex」と入力するか、管理セクションの一覧を **[開発ツール]** までスクロール ダウンします。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-179">Type "ex" in the search box or scroll down the list of management sections to **DEVELOPMENT TOOLS**.</span></span>
1. <span data-ttu-id="f2f7e-180">**[開発ツール]** > 、**[拡張機能]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-180">Select **DEVELOPMENT TOOLS** > **Extensions**.</span></span>
1. <span data-ttu-id="f2f7e-181">**[追加]** を選びます。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-181">Select **Add**.</span></span>
1. <span data-ttu-id="f2f7e-182">**[ASP.NET Core &lt;x.y&gt; (x86) ランタイム]** 拡張機能を一覧から選択します。`<x.y>` は ASP.NET Core のプレビュー バージョンです (**ASP.NET Core 2.2 (x86) ランタイム**など)。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-182">Select the **ASP.NET Core &lt;x.y&gt; (x86) Runtime** extension from the list, where `<x.y>` is the ASP.NET Core preview version (for example, **ASP.NET Core 2.2 (x86) Runtime**).</span></span> <span data-ttu-id="f2f7e-183">x86 ランタイムは、ASP.NET Core モジュールによるプロセス外ホスティングに依存する[フレームワーク依存のデプロイ](/dotnet/core/deploying/#framework-dependent-deployments-fdd)に適しています。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-183">The x86 runtime is appropriate for [framework-dependent deployments](/dotnet/core/deploying/#framework-dependent-deployments-fdd) that rely on out-of-process hosting by the ASP.NET Core Module.</span></span>
1. <span data-ttu-id="f2f7e-184">**[OK]** を選んで法的条項に同意します。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-184">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="f2f7e-185">**[OK]** を選択し、拡張機能をインストールします。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-185">Select **OK** to install the extension.</span></span>

<span data-ttu-id="f2f7e-186">操作が完了すると、最新の .NET Core プレビューがインストールされます。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-186">When the operation completes, the latest .NET Core preview is installed.</span></span> <span data-ttu-id="f2f7e-187">次のようにしてインストールを検証します。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-187">Verify the installation:</span></span>

1. <span data-ttu-id="f2f7e-188">**[開発ツール]** の下の **[高度なツール]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-188">Select **Advanced Tools** under **DEVELOPMENT TOOLS**.</span></span>
1. <span data-ttu-id="f2f7e-189">**[高度なツール]** ブレードの下の **[Go]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-189">Select **Go** on the **Advanced Tools** blade.</span></span>
1. <span data-ttu-id="f2f7e-190">**[デバッグ コンソール]** > **[PowerShell]** のメニュー項目を選択します。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-190">Select the **Debug console** > **PowerShell** menu item.</span></span>
1. <span data-ttu-id="f2f7e-191">PowerShell のプロンプトで次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-191">At the PowerShell prompt, execute the following command.</span></span> <span data-ttu-id="f2f7e-192">コマンドの `<x.y>` を ASP.NET Core ランタイム バージョンに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-192">Substitute the ASP.NET Core runtime version for `<x.y>` in the command:</span></span>

   ```powershell
   Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.<x.y>.x86\
   ```
   <span data-ttu-id="f2f7e-193">インストールされているプレビュー ランタイムが ASP.NET Core 2.2 向けである場合、コマンドは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-193">If the installed preview runtime is for ASP.NET Core 2.2, the command is:</span></span>
   ```powershell
   Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.2.2.x86\
   ```
   <span data-ttu-id="f2f7e-194">x64 プレビューのランタイムがインストールされていると、コマンドで `True` が返されます。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-194">The command returns `True` when the x64 preview runtime is installed.</span></span>

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="f2f7e-195">App Services アプリのプラットフォーム アーキテクチャ (x86/x64) が、A シリーズ計算、またはより優れたホスティング階層でホストされているアプリの **[全般設定]** の下の **[アプリケーション設定]** に設定されます。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-195">The platform architecture (x86/x64) of an App Services app is set in the **Application Settings** blade under **General Settings** for apps that are hosted on an A-series compute or better hosting tier.</span></span> <span data-ttu-id="f2f7e-196">アプリがインプロセス モードで実行されていて、プラットフォーム アーキテクチャが 64 ビット (x64) 向けに構成されている場合は、ASP.NET Core モジュールで 64 ビット プレビュー ランタイム (ある場合) が使用されます。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-196">If the app is run in in-process mode and the platform architecture is configured for 64-bit (x64), the ASP.NET Core Module uses the 64-bit preview runtime, if present.</span></span> <span data-ttu-id="f2f7e-197">**ASP.NET Core &lt;x.y&gt; (x64) ランタイム**拡張機能 (たとえば、**ASP.NET Core 2.2 (x64) ランタイム**) をインストールします。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-197">Install the **ASP.NET Core &lt;x.y&gt; (x64) Runtime** extension (for example, **ASP.NET Core 2.2 (x64) Runtime**).</span></span>
>
> <span data-ttu-id="f2f7e-198">x64 プレビュー ランタイムがインストールされたら、Kudu PowerShell コマンド ウィンドウで次のコマンドを実行して、インストールを確認します。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-198">After installing the x64 preview runtime, run the following command in the Kudu PowerShell command window to verify the installation.</span></span> <span data-ttu-id="f2f7e-199">コマンドの `<x.y>` を ASP.NET Core ランタイム バージョンに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-199">Substitute the ASP.NET Core runtime version for `<x.y>` in the command:</span></span>
>
> ```powershell
> Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.<x.y>.x64\
> ```
> <span data-ttu-id="f2f7e-200">インストールされているプレビュー ランタイムが ASP.NET Core 2.2 向けである場合、コマンドは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-200">If the installed preview runtime is for ASP.NET Core 2.2, the command is:</span></span>
> ```powershell
> Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.2.2.x64\
> ```
> <span data-ttu-id="f2f7e-201">x64 プレビューのランタイムがインストールされていると、コマンドで `True` が返されます。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-201">The command returns `True` when the x64 preview runtime is installed.</span></span>

::: moniker-end

> [!NOTE]
> <span data-ttu-id="f2f7e-202">**ASP.NET Core 拡張機能**によって、たとえば Azure のログ記録など、Azure App Services での ASP.NET Core の追加機能が有効になります。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-202">**ASP.NET Core Extensions** enables additional functionality for ASP.NET Core on Azure App Services, such as enabling Azure logging.</span></span> <span data-ttu-id="f2f7e-203">この拡張機能は、Visual Studio からデプロイするときに自動的にインストールされます。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-203">The extension is installed automatically when deploying from Visual Studio.</span></span> <span data-ttu-id="f2f7e-204">拡張機能がインストールされていない場合は、アプリにインストールします。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-204">If the extension isn't installed, install it for the app.</span></span>

<span data-ttu-id="f2f7e-205">**ARM テンプレートでプレビュー サイト拡張機能を使用する**</span><span class="sxs-lookup"><span data-stu-id="f2f7e-205">**Use the preview site extension with an ARM template**</span></span>

<span data-ttu-id="f2f7e-206">ARM テンプレートを使用してアプリを作成し、展開する場合は、リソースの種類として `siteextensions` を使用してサイト拡張機能を Web アプリに追加することができます。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-206">If an ARM template is used to create and deploy apps, the `siteextensions` resource type can be used to add the site extension to a web app.</span></span> <span data-ttu-id="f2f7e-207">例:</span><span class="sxs-lookup"><span data-stu-id="f2f7e-207">For example:</span></span>

[!code-json[Main](index/sample/arm.json?highlight=2)]

<!--
### Deploy the app self-contained

A [self-contained app](/dotnet/core/deploying/#self-contained-deployments-scd) can be deployed that carries the preview runtime in the deployment. When deploying a self-contained app:

* The site doesn't need to be prepared.
* The app must be published differently than when publishing for a framework-dependent deployment with the shared runtime and host on the server.

Self-contained apps are an option for all ASP.NET Core apps.
-->

### <a name="use-docker-with-web-apps-for-containers"></a><span data-ttu-id="f2f7e-208">コンテナー用の Web アプリで Docker を使用する</span><span class="sxs-lookup"><span data-stu-id="f2f7e-208">Use Docker with Web Apps for containers</span></span>

<span data-ttu-id="f2f7e-209">[Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) には最新のプレビュー Docker イメージが含まれています。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-209">The [Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) contains the latest preview Docker images.</span></span> <span data-ttu-id="f2f7e-210">イメージを基本イメージとして使用できます。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-210">The images can be used as a base image.</span></span> <span data-ttu-id="f2f7e-211">通常は、イメージを使用して、Web App for Containers に展開します。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-211">Use the image and deploy to Web Apps for Containers normally.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f2f7e-212">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="f2f7e-212">Additional resources</span></span>

* [<span data-ttu-id="f2f7e-213">Web Apps の概要 (5 分間の概要ビデオ)</span><span class="sxs-lookup"><span data-stu-id="f2f7e-213">Web Apps overview (5-minute overview video)</span></span>](/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="f2f7e-214">Azure App Service: .NET アプリのホスティングに最適な場所 (55 分間の概要ビデオ)</span><span class="sxs-lookup"><span data-stu-id="f2f7e-214">Azure App Service: The Best Place to Host your .NET Apps (55-minute overview video)</span></span>](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [<span data-ttu-id="f2f7e-215">Azure Friday: Azure App Service の診断とトラブルシューティング (12 分間のビデオ)</span><span class="sxs-lookup"><span data-stu-id="f2f7e-215">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [<span data-ttu-id="f2f7e-216">Azure App Service の診断の概要</span><span class="sxs-lookup"><span data-stu-id="f2f7e-216">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)
* <xref:host-and-deploy/web-farm>

<span data-ttu-id="f2f7e-217">Windows Server の Azure App Service では[インターネット インフォメーション サービス (IIS)](https://www.iis.net/) が使用されます。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-217">Azure App Service on Windows Server uses [Internet Information Services (IIS)](https://www.iis.net/).</span></span> <span data-ttu-id="f2f7e-218">次のトピックは、基になる IIS テクノロジと関連しています。</span><span class="sxs-lookup"><span data-stu-id="f2f7e-218">The following topics pertain to the underlying IIS technology:</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:fundamentals/servers/aspnet-core-module>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>
* [<span data-ttu-id="f2f7e-219">Microsoft TechNet ライブラリ: Windows Server</span><span class="sxs-lookup"><span data-stu-id="f2f7e-219">Microsoft TechNet Library: Windows Server</span></span>](/windows-server/windows-server-versions)
