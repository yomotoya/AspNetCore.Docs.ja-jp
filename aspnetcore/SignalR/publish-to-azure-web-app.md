---
title: ASP.NET Core の発行 SignalR アプリを Azure Web アプリ
author: rachelappel
description: ASP.NET Core の発行 SignalR アプリを Azure Web アプリ
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 04/20/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: fd7d38ad47d9004db2ae7b5858dc22609943f601
ms.sourcegitcommit: 2ab550f8c46e1a8a5d45e58be44d151c676af256
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/28/2018
---
# <a name="publish-an-aspnet-core-signalr-app-to-an-azure-web-app"></a><span data-ttu-id="54b2f-103">ASP.NET Core の発行、Azure Web アプリに SignalR アプリ</span><span class="sxs-lookup"><span data-stu-id="54b2f-103">Publish an ASP.NET Core SignalR app to an Azure Web App</span></span>

<span data-ttu-id="54b2f-104">[Azure の Web アプリ](/azure/app-service/app-service-web-overview)は、 [Microsoft クラウド コンピューティング](https://azure.microsoft.com/)ASP.NET Core を含め、web アプリをホストするためのプラットフォーム サービスです。</span><span class="sxs-lookup"><span data-stu-id="54b2f-104">[Azure Web App](/azure/app-service/app-service-web-overview) is a [Microsoft cloud computing](https://azure.microsoft.com/) platform service for hosting web apps, including ASP.NET Core.</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="54b2f-105">アプリの発行</span><span class="sxs-lookup"><span data-stu-id="54b2f-105">Publish the app</span></span>

<span data-ttu-id="54b2f-106">Visual Studio は、Azure Web アプリに発行のための組み込みのツールを提供します。</span><span class="sxs-lookup"><span data-stu-id="54b2f-106">Visual Studio provides built-in tools for publishing to an Azure Web App.</span></span> <span data-ttu-id="54b2f-107">Visual Studio Code のユーザーが使用できる[Azure CLI](/cli/azure)アプリを Azure に発行するコマンド。</span><span class="sxs-lookup"><span data-stu-id="54b2f-107">Visual Studio Code user can use [Azure CLI](/cli/azure) commands to publish apps to Azure.</span></span> <span data-ttu-id="54b2f-108">この記事では、Visual Studio でツールを使用して発行について説明します。</span><span class="sxs-lookup"><span data-stu-id="54b2f-108">This article covers publishing using the tools in Visual Studio.</span></span> <span data-ttu-id="54b2f-109">Azure CLI を使用してアプリを発行するを参照してください。[コマンド ライン ツールを使用して Azure に ASP.NET Core アプリケーションを公開](xref:tutorials/publish-to-azure-webapp-using-cli)です。</span><span class="sxs-lookup"><span data-stu-id="54b2f-109">To publish an app using Azure CLI, see [Publish an ASP.NET Core app to Azure with command line tools](xref:tutorials/publish-to-azure-webapp-using-cli).</span></span>

<span data-ttu-id="54b2f-110">プロジェクトを右クリックして**ソリューション エクスプ ローラー**選択**発行**です。</span><span class="sxs-lookup"><span data-stu-id="54b2f-110">Right-click on the project in **Solution Explorer** and select **Publish**.</span></span> <span data-ttu-id="54b2f-111">いることを確認**新規作成**がオンになって、**発行先の選択**ダイアログ、および選択**発行**です。</span><span class="sxs-lookup"><span data-stu-id="54b2f-111">Confirm that **Create new** is checked in the **Pick a publish target** dialog, and select **Publish**.</span></span>

![選択対象をパブリッシュします。](publish-to-azure-web-app/_static/pick-publish-target-dialog.png)

<span data-ttu-id="54b2f-113">次の情報を入力、 **App Service の作成**ダイアログと選択**作成**です。</span><span class="sxs-lookup"><span data-stu-id="54b2f-113">Enter the following information in the **Create App Service** dialog and select **Create**.</span></span>

| <span data-ttu-id="54b2f-114">アイテム</span><span class="sxs-lookup"><span data-stu-id="54b2f-114">Item</span></span> | <span data-ttu-id="54b2f-115">説明</span><span class="sxs-lookup"><span data-stu-id="54b2f-115">Description</span></span> |
| ---- | ----------- |
| <span data-ttu-id="54b2f-116">**アプリ名**</span><span class="sxs-lookup"><span data-stu-id="54b2f-116">**App name**</span></span> | <span data-ttu-id="54b2f-117">アプリの一意の名前。</span><span class="sxs-lookup"><span data-stu-id="54b2f-117">A unique name of the app.</span></span> |
| <span data-ttu-id="54b2f-118">**サブスクリプション**</span><span class="sxs-lookup"><span data-stu-id="54b2f-118">**Subscription**</span></span> | <span data-ttu-id="54b2f-119">アプリで使用する Azure サブスクリプション。</span><span class="sxs-lookup"><span data-stu-id="54b2f-119">The Azure subscription that the app uses.</span></span> |
| <span data-ttu-id="54b2f-120">**リソース グループ**</span><span class="sxs-lookup"><span data-stu-id="54b2f-120">**Resource Group**</span></span> | <span data-ttu-id="54b2f-121">関連するリソースをアプリが所属するグループ。</span><span class="sxs-lookup"><span data-stu-id="54b2f-121">The group of related resources to which the app belongs.</span></span>  |
| <span data-ttu-id="54b2f-122">**ホスティング プラン**</span><span class="sxs-lookup"><span data-stu-id="54b2f-122">**Hosting Plan**</span></span> | <span data-ttu-id="54b2f-123">Web アプリの料金プランです。</span><span class="sxs-lookup"><span data-stu-id="54b2f-123">The pricing plan for the web app.</span></span> |

![アプリ サービスを作成します。](publish-to-azure-web-app/_static/create-app-service-dialog.png)

<span data-ttu-id="54b2f-125">Visual Studio では、次のタスクを実行します。</span><span class="sxs-lookup"><span data-stu-id="54b2f-125">Visual Studio completes the following tasks:</span></span>

* <span data-ttu-id="54b2f-126">発行プロファイルを作成する発行の設定を格納します。</span><span class="sxs-lookup"><span data-stu-id="54b2f-126">Creates a Publish Profile containing publish settings.</span></span>
* <span data-ttu-id="54b2f-127">作成するか、既存の*Azure Web アプリ*提供された詳細とします。</span><span class="sxs-lookup"><span data-stu-id="54b2f-127">Creates or uses an existing *Azure Web App* with the provided details.</span></span>
* <span data-ttu-id="54b2f-128">アプリを発行します。</span><span class="sxs-lookup"><span data-stu-id="54b2f-128">Publishes the app.</span></span>
* <span data-ttu-id="54b2f-129">読み込まれた公開された web アプリと、ブラウザーを起動します。</span><span class="sxs-lookup"><span data-stu-id="54b2f-129">Launches a browser, with the published web app loaded.</span></span>

<span data-ttu-id="54b2f-130">URL の形式のことを確認して、アプリは*アプリ {name} .azurewebsites .net*です。</span><span class="sxs-lookup"><span data-stu-id="54b2f-130">Notice the format of the URL for the app is *{app name}.azurewebsites.net*.</span></span> <span data-ttu-id="54b2f-131">たとえば、という名前のアプリ`SignalRChattR`次のような URL が指定されて`https://signalrchattr.azurewebsites.net`です。</span><span class="sxs-lookup"><span data-stu-id="54b2f-131">For example, an app named `SignalRChattR` has a URL that looks like `https://signalrchattr.azurewebsites.net`.</span></span>

<span data-ttu-id="54b2f-132">HTTP 502.2 エラーが発生する場合は、次を参照してください。 [Azure App Service に ASP.NET Core の展開のプレビュー リリース](xref:host-and-deploy/azure-apps/index)問題を解決します。</span><span class="sxs-lookup"><span data-stu-id="54b2f-132">If an HTTP 502.2 error occurs, see [Deploy ASP.NET Core preview release to Azure App Service](xref:host-and-deploy/azure-apps/index) to resolve it.</span></span>

## <a name="configure-signalr-web-app"></a><span data-ttu-id="54b2f-133">SignalR web app を構成します。</span><span class="sxs-lookup"><span data-stu-id="54b2f-133">Configure SignalR web app</span></span>

<span data-ttu-id="54b2f-134">ASP.NET Core SignalR 公開されたアプリに Azure Web アプリが必要に応じて[ARR アフィニティ](https://en.wikipedia.org/wiki/Application_Request_Routing)有効にします。</span><span class="sxs-lookup"><span data-stu-id="54b2f-134">ASP.NET Core SignalR apps that are published as an Azure Web App must have [ARR Affinity](https://en.wikipedia.org/wiki/Application_Request_Routing) enabled.</span></span> <span data-ttu-id="54b2f-135">[Websocket](xref:fundamentals/websockets)を有効にする、Websocket トランスポート関数を使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="54b2f-135">[WebSockets](xref:fundamentals/websockets) should be enabled, to allow the WebSockets transport to function.</span></span>

<span data-ttu-id="54b2f-136">Azure ポータルに移動**アプリ設定**を web アプリです。</span><span class="sxs-lookup"><span data-stu-id="54b2f-136">In the Azure portal, navigate to **App Settings** for your web app.</span></span> <span data-ttu-id="54b2f-137">設定**Websocket**に**で**、ことを確認し、 **ARR アフィニティ**は**で**です。</span><span class="sxs-lookup"><span data-stu-id="54b2f-137">Set **WebSockets** to **On**, and verify **ARR Affinity** is **On**.</span></span>

![Azure ポータルで azure の Web アプリ設定](publish-to-azure-web-app/_static/azure-web-app-settings.png)

 <span data-ttu-id="54b2f-139">Websocket トランスポートとその他のトランスポート[App Service プランに基づいて制限されます](/azure/azure-subscription-service-limits#app-service-limits)です。</span><span class="sxs-lookup"><span data-stu-id="54b2f-139">WebSockets and other transports [are limited based on the App Service Plan](/azure/azure-subscription-service-limits#app-service-limits).</span></span>

## <a name="related-resources"></a><span data-ttu-id="54b2f-140">関連資料</span><span class="sxs-lookup"><span data-stu-id="54b2f-140">Related resources</span></span>

* [<span data-ttu-id="54b2f-141">コマンド ライン ツールを使用して Azure に ASP.NET Core アプリケーションを公開します。</span><span class="sxs-lookup"><span data-stu-id="54b2f-141">Publish an ASP.NET Core app to Azure with command line tools</span></span>](xref:tutorials/publish-to-azure-webapp-using-cli?tabs=windows)
* [<span data-ttu-id="54b2f-142">Visual Studio での Azure への ASP.NET Core アプリケーションを公開します。</span><span class="sxs-lookup"><span data-stu-id="54b2f-142">Publish an ASP.NET Core app to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)
* [<span data-ttu-id="54b2f-143">ホストし、Azure での ASP.NET Core プレビュー アプリを展開します。</span><span class="sxs-lookup"><span data-stu-id="54b2f-143">Host and deploy ASP.NET Core Preview apps on Azure</span></span>](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
