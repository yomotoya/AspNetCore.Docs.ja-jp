---
title: 発行、ASP.NET Core SignalR のアプリを Azure Web アプリに
author: bradygaster
description: 発行、ASP.NET Core SignalR のアプリを Azure Web アプリに
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/20/2018
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: 66fa855590c49c4284e4b42cae57f3d4d81dd0fc
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837677"
---
# <a name="publish-an-aspnet-core-signalr-app-to-an-azure-web-app"></a><span data-ttu-id="aa2c8-103">発行、ASP.NET Core SignalR アプリケーションを Azure Web アプリ</span><span class="sxs-lookup"><span data-stu-id="aa2c8-103">Publish an ASP.NET Core SignalR app to an Azure Web App</span></span>

<span data-ttu-id="aa2c8-104">[Azure Web アプリの](/azure/app-service/app-service-web-overview)は、 [Microsoft クラウド コンピューティング](https://azure.microsoft.com/)ASP.NET Core を含む、web アプリをホストするためのプラットフォーム サービスです。</span><span class="sxs-lookup"><span data-stu-id="aa2c8-104">[Azure Web App](/azure/app-service/app-service-web-overview) is a [Microsoft cloud computing](https://azure.microsoft.com/) platform service for hosting web apps, including ASP.NET Core.</span></span>

> [!NOTE]
> <span data-ttu-id="aa2c8-105">この記事では、Visual Studio から ASP.NET Core SignalR アプリの発行を指します。</span><span class="sxs-lookup"><span data-stu-id="aa2c8-105">This article refers to publishing an ASP.NET Core SignalR app from Visual Studio.</span></span> <span data-ttu-id="aa2c8-106">参照してください[for Azure SignalR サービス](https://azure.microsoft.com/en-gb/services/signalr-service?)詳細については、Azure で SignalR を使用します。</span><span class="sxs-lookup"><span data-stu-id="aa2c8-106">Visit [SignalR service for Azure](https://azure.microsoft.com/en-gb/services/signalr-service?) for more information about using SignalR on Azure.</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="aa2c8-107">アプリの発行</span><span class="sxs-lookup"><span data-stu-id="aa2c8-107">Publish the app</span></span>

<span data-ttu-id="aa2c8-108">Visual Studio は、Azure Web App に発行のための組み込みのツールを提供します。</span><span class="sxs-lookup"><span data-stu-id="aa2c8-108">Visual Studio provides built-in tools for publishing to an Azure Web App.</span></span> <span data-ttu-id="aa2c8-109">Visual Studio Code のユーザーが使用できる[Azure CLI](/cli/azure)アプリを Azure に発行するコマンド。</span><span class="sxs-lookup"><span data-stu-id="aa2c8-109">Visual Studio Code user can use [Azure CLI](/cli/azure) commands to publish apps to Azure.</span></span> <span data-ttu-id="aa2c8-110">この記事では、Visual Studio でツールを使用して公開について説明します。</span><span class="sxs-lookup"><span data-stu-id="aa2c8-110">This article covers publishing using the tools in Visual Studio.</span></span> <span data-ttu-id="aa2c8-111">Azure CLI を使用してアプリを発行するを参照してください。[コマンド ライン ツールを使用して Azure に ASP.NET Core アプリを発行](/azure/app-service/app-service-web-get-started-dotnet)します。</span><span class="sxs-lookup"><span data-stu-id="aa2c8-111">To publish an app using Azure CLI, see [Publish an ASP.NET Core app to Azure with command line tools](/azure/app-service/app-service-web-get-started-dotnet).</span></span>

<span data-ttu-id="aa2c8-112">プロジェクトを右クリックして**ソリューション エクスプ ローラー**選択**発行**します。</span><span class="sxs-lookup"><span data-stu-id="aa2c8-112">Right-click on the project in **Solution Explorer** and select **Publish**.</span></span> <span data-ttu-id="aa2c8-113">確認します**新規作成**がチェックイン、**発行先を選択**ダイアログ、および選択**発行**します。</span><span class="sxs-lookup"><span data-stu-id="aa2c8-113">Confirm that **Create new** is checked in the **Pick a publish target** dialog, and select **Publish**.</span></span>

![発行対象を選択](publish-to-azure-web-app/_static/pick-publish-target-dialog.png)

<span data-ttu-id="aa2c8-115">次の情報を入力、 **App Service の作成**ダイアログと選択**作成**です。</span><span class="sxs-lookup"><span data-stu-id="aa2c8-115">Enter the following information in the **Create App Service** dialog and select **Create**.</span></span>

| <span data-ttu-id="aa2c8-116">アイテム</span><span class="sxs-lookup"><span data-stu-id="aa2c8-116">Item</span></span> | <span data-ttu-id="aa2c8-117">説明</span><span class="sxs-lookup"><span data-stu-id="aa2c8-117">Description</span></span> |
| ---- | ----------- |
| <span data-ttu-id="aa2c8-118">**アプリ名**</span><span class="sxs-lookup"><span data-stu-id="aa2c8-118">**App name**</span></span> | <span data-ttu-id="aa2c8-119">アプリの一意の名前。</span><span class="sxs-lookup"><span data-stu-id="aa2c8-119">A unique name of the app.</span></span> |
| <span data-ttu-id="aa2c8-120">**サブスクリプション**</span><span class="sxs-lookup"><span data-stu-id="aa2c8-120">**Subscription**</span></span> | <span data-ttu-id="aa2c8-121">アプリを使用する Azure サブスクリプション。</span><span class="sxs-lookup"><span data-stu-id="aa2c8-121">The Azure subscription that the app uses.</span></span> |
| <span data-ttu-id="aa2c8-122">**リソース グループ**</span><span class="sxs-lookup"><span data-stu-id="aa2c8-122">**Resource Group**</span></span> | <span data-ttu-id="aa2c8-123">アプリが属している関連するリソースのグループ。</span><span class="sxs-lookup"><span data-stu-id="aa2c8-123">The group of related resources to which the app belongs.</span></span>  |
| <span data-ttu-id="aa2c8-124">**ホスティング プラン**</span><span class="sxs-lookup"><span data-stu-id="aa2c8-124">**Hosting Plan**</span></span> | <span data-ttu-id="aa2c8-125">Web アプリの価格プランです。</span><span class="sxs-lookup"><span data-stu-id="aa2c8-125">The pricing plan for the web app.</span></span> |

![App service を作成します。](publish-to-azure-web-app/_static/create-app-service-dialog.png)

<span data-ttu-id="aa2c8-127">Visual Studio では、次のタスクを実行します。</span><span class="sxs-lookup"><span data-stu-id="aa2c8-127">Visual Studio completes the following tasks:</span></span>

* <span data-ttu-id="aa2c8-128">発行プロファイルを作成します。 発行の設定を格納しています。</span><span class="sxs-lookup"><span data-stu-id="aa2c8-128">Creates a Publish Profile containing publish settings.</span></span>
* <span data-ttu-id="aa2c8-129">作成するか、既存の*Azure Web App*で提供された詳細。</span><span class="sxs-lookup"><span data-stu-id="aa2c8-129">Creates or uses an existing *Azure Web App* with the provided details.</span></span>
* <span data-ttu-id="aa2c8-130">アプリを発行します。</span><span class="sxs-lookup"><span data-stu-id="aa2c8-130">Publishes the app.</span></span>
* <span data-ttu-id="aa2c8-131">公開された web アプリが読み込まれると、ブラウザーを起動します。</span><span class="sxs-lookup"><span data-stu-id="aa2c8-131">Launches a browser, with the published web app loaded.</span></span>

<span data-ttu-id="aa2c8-132">アプリは、URL の形式に注意してください *{アプリ名} .azurewebsites.net .net*します。</span><span class="sxs-lookup"><span data-stu-id="aa2c8-132">Notice the format of the URL for the app is *{app name}.azurewebsites.net*.</span></span> <span data-ttu-id="aa2c8-133">たとえば、という名前のアプリ`SignalRChattR`が次のような URL `https://signalrchattr.azurewebsites.net`。</span><span class="sxs-lookup"><span data-stu-id="aa2c8-133">For example, an app named `SignalRChattR` has a URL that looks like `https://signalrchattr.azurewebsites.net`.</span></span>

<span data-ttu-id="aa2c8-134">HTTP 502.2 エラーが発生する場合は、[Azure App Service に ASP.NET Core の展開のプレビュー リリース](xref:host-and-deploy/azure-apps/index)問題を解決を参照してください。</span><span class="sxs-lookup"><span data-stu-id="aa2c8-134">If an HTTP 502.2 error occurs, see [Deploy ASP.NET Core preview release to Azure App Service](xref:host-and-deploy/azure-apps/index) to resolve it.</span></span>

## <a name="configure-signalr-web-app"></a><span data-ttu-id="aa2c8-135">SignalR web アプリを構成します。</span><span class="sxs-lookup"><span data-stu-id="aa2c8-135">Configure SignalR web app</span></span>

<span data-ttu-id="aa2c8-136">Azure Web アプリがありますに公開されている ASP.NET Core SignalR アプリ[ARR アフィニティ](https://en.wikipedia.org/wiki/Application_Request_Routing)を有効にします。</span><span class="sxs-lookup"><span data-stu-id="aa2c8-136">ASP.NET Core SignalR apps that are published as an Azure Web App must have [ARR Affinity](https://en.wikipedia.org/wiki/Application_Request_Routing) enabled.</span></span> <span data-ttu-id="aa2c8-137">[Websocket](xref:fundamentals/websockets) Websocket トランスポートは関数を許可する、有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="aa2c8-137">[WebSockets](xref:fundamentals/websockets) should be enabled, to allow the WebSockets transport to function.</span></span>

<span data-ttu-id="aa2c8-138">Azure portal に移動します**アプリ設定**web アプリ。</span><span class="sxs-lookup"><span data-stu-id="aa2c8-138">In the Azure portal, navigate to **App Settings** for your web app.</span></span> <span data-ttu-id="aa2c8-139">設定**Websocket**に**で**、ことを確認し、 **ARR アフィニティ**は**で**します。</span><span class="sxs-lookup"><span data-stu-id="aa2c8-139">Set **WebSockets** to **On**, and verify **ARR Affinity** is **On**.</span></span>

![Azure portal で azure の Web アプリの設定](publish-to-azure-web-app/_static/azure-web-app-settings.png)

 <span data-ttu-id="aa2c8-141">Websocket と他のトランスポート[App Service プランの](/azure/azure-subscription-service-limits#app-service-limits)します。</span><span class="sxs-lookup"><span data-stu-id="aa2c8-141">WebSockets and other transports [are limited based on the App Service Plan](/azure/azure-subscription-service-limits#app-service-limits).</span></span>

## <a name="related-resources"></a><span data-ttu-id="aa2c8-142">関連資料</span><span class="sxs-lookup"><span data-stu-id="aa2c8-142">Related resources</span></span>

* [<span data-ttu-id="aa2c8-143">コマンド ライン ツールを使用して Azure に ASP.NET Core アプリを発行します。</span><span class="sxs-lookup"><span data-stu-id="aa2c8-143">Publish an ASP.NET Core app to Azure with command line tools</span></span>](/azure/app-service/app-service-web-get-started-dotnet)
* [<span data-ttu-id="aa2c8-144">Visual Studio を使用した Azure への ASP.NET Core アプリを発行します。</span><span class="sxs-lookup"><span data-stu-id="aa2c8-144">Publish an ASP.NET Core app to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)
* [<span data-ttu-id="aa2c8-145">ホストし、Azure で ASP.NET Core プレビュー アプリを展開します。</span><span class="sxs-lookup"><span data-stu-id="aa2c8-145">Host and deploy ASP.NET Core Preview apps on Azure</span></span>](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
