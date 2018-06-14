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
ms.openlocfilehash: 8612824cc9d4a9ace1c214411c44754350100f06
ms.sourcegitcommit: 7e87671fea9a5f36ca516616fe3b40b537f428d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/12/2018
ms.locfileid: "35341809"
---
# <a name="publish-an-aspnet-core-signalr-app-to-an-azure-web-app"></a><span data-ttu-id="42ee6-103">ASP.NET Core の発行、Azure Web アプリに SignalR アプリ</span><span class="sxs-lookup"><span data-stu-id="42ee6-103">Publish an ASP.NET Core SignalR app to an Azure Web App</span></span>

<span data-ttu-id="42ee6-104">[Azure の Web アプリ](/azure/app-service/app-service-web-overview)は、 [Microsoft クラウド コンピューティング](https://azure.microsoft.com/)ASP.NET Core を含め、web アプリをホストするためのプラットフォーム サービスです。</span><span class="sxs-lookup"><span data-stu-id="42ee6-104">[Azure Web App](/azure/app-service/app-service-web-overview) is a [Microsoft cloud computing](https://azure.microsoft.com/) platform service for hosting web apps, including ASP.NET Core.</span></span>

> [!NOTE]
> <span data-ttu-id="42ee6-105">この資料では、Visual Studio から ASP.NET Core SignalR アプリケーションを発行します。</span><span class="sxs-lookup"><span data-stu-id="42ee6-105">This article refers to publishing an ASP.NET Core SignalR app from Visual Studio.</span></span> <span data-ttu-id="42ee6-106">参照してください[SignalR Azure サービスの](https://azure.microsoft.com/en-gb/services/signalr-service?)SignalR を使用して、Azure での詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="42ee6-106">Visit [SignalR service for Azure](https://azure.microsoft.com/en-gb/services/signalr-service?) for more information about using SignalR on Azure.</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="42ee6-107">アプリの発行</span><span class="sxs-lookup"><span data-stu-id="42ee6-107">Publish the app</span></span>

<span data-ttu-id="42ee6-108">Visual Studio は、Azure Web アプリに発行のための組み込みのツールを提供します。</span><span class="sxs-lookup"><span data-stu-id="42ee6-108">Visual Studio provides built-in tools for publishing to an Azure Web App.</span></span> <span data-ttu-id="42ee6-109">Visual Studio Code のユーザーが使用できる[Azure CLI](/cli/azure)アプリを Azure に発行するコマンド。</span><span class="sxs-lookup"><span data-stu-id="42ee6-109">Visual Studio Code user can use [Azure CLI](/cli/azure) commands to publish apps to Azure.</span></span> <span data-ttu-id="42ee6-110">この記事では、Visual Studio でツールを使用して発行について説明します。</span><span class="sxs-lookup"><span data-stu-id="42ee6-110">This article covers publishing using the tools in Visual Studio.</span></span> <span data-ttu-id="42ee6-111">Azure CLI を使用してアプリを発行するを参照してください。[コマンド ライン ツールを使用して Azure に ASP.NET Core アプリケーションを公開](xref:tutorials/publish-to-azure-webapp-using-cli)です。</span><span class="sxs-lookup"><span data-stu-id="42ee6-111">To publish an app using Azure CLI, see [Publish an ASP.NET Core app to Azure with command line tools](xref:tutorials/publish-to-azure-webapp-using-cli).</span></span>

<span data-ttu-id="42ee6-112">プロジェクトを右クリックして**ソリューション エクスプ ローラー**選択**発行**です。</span><span class="sxs-lookup"><span data-stu-id="42ee6-112">Right-click on the project in **Solution Explorer** and select **Publish**.</span></span> <span data-ttu-id="42ee6-113">いることを確認**新規作成**がオンになって、**発行先の選択**ダイアログ、および選択**発行**です。</span><span class="sxs-lookup"><span data-stu-id="42ee6-113">Confirm that **Create new** is checked in the **Pick a publish target** dialog, and select **Publish**.</span></span>

![選択対象をパブリッシュします。](publish-to-azure-web-app/_static/pick-publish-target-dialog.png)

<span data-ttu-id="42ee6-115">次の情報を入力、 **App Service の作成**ダイアログと選択**作成**です。</span><span class="sxs-lookup"><span data-stu-id="42ee6-115">Enter the following information in the **Create App Service** dialog and select **Create**.</span></span>

| <span data-ttu-id="42ee6-116">アイテム</span><span class="sxs-lookup"><span data-stu-id="42ee6-116">Item</span></span> | <span data-ttu-id="42ee6-117">説明</span><span class="sxs-lookup"><span data-stu-id="42ee6-117">Description</span></span> |
| ---- | ----------- |
| <span data-ttu-id="42ee6-118">**アプリ名**</span><span class="sxs-lookup"><span data-stu-id="42ee6-118">**App name**</span></span> | <span data-ttu-id="42ee6-119">アプリの一意の名前。</span><span class="sxs-lookup"><span data-stu-id="42ee6-119">A unique name of the app.</span></span> |
| <span data-ttu-id="42ee6-120">**サブスクリプション**</span><span class="sxs-lookup"><span data-stu-id="42ee6-120">**Subscription**</span></span> | <span data-ttu-id="42ee6-121">アプリで使用する Azure サブスクリプション。</span><span class="sxs-lookup"><span data-stu-id="42ee6-121">The Azure subscription that the app uses.</span></span> |
| <span data-ttu-id="42ee6-122">**リソース グループ**</span><span class="sxs-lookup"><span data-stu-id="42ee6-122">**Resource Group**</span></span> | <span data-ttu-id="42ee6-123">関連するリソースをアプリが所属するグループ。</span><span class="sxs-lookup"><span data-stu-id="42ee6-123">The group of related resources to which the app belongs.</span></span>  |
| <span data-ttu-id="42ee6-124">**ホスティング プラン**</span><span class="sxs-lookup"><span data-stu-id="42ee6-124">**Hosting Plan**</span></span> | <span data-ttu-id="42ee6-125">Web アプリの料金プランです。</span><span class="sxs-lookup"><span data-stu-id="42ee6-125">The pricing plan for the web app.</span></span> |

![アプリ サービスを作成します。](publish-to-azure-web-app/_static/create-app-service-dialog.png)

<span data-ttu-id="42ee6-127">Visual Studio では、次のタスクを実行します。</span><span class="sxs-lookup"><span data-stu-id="42ee6-127">Visual Studio completes the following tasks:</span></span>

* <span data-ttu-id="42ee6-128">発行プロファイルを作成する発行の設定を格納します。</span><span class="sxs-lookup"><span data-stu-id="42ee6-128">Creates a Publish Profile containing publish settings.</span></span>
* <span data-ttu-id="42ee6-129">作成するか、既存の*Azure Web アプリ*提供された詳細とします。</span><span class="sxs-lookup"><span data-stu-id="42ee6-129">Creates or uses an existing *Azure Web App* with the provided details.</span></span>
* <span data-ttu-id="42ee6-130">アプリを発行します。</span><span class="sxs-lookup"><span data-stu-id="42ee6-130">Publishes the app.</span></span>
* <span data-ttu-id="42ee6-131">読み込まれた公開された web アプリと、ブラウザーを起動します。</span><span class="sxs-lookup"><span data-stu-id="42ee6-131">Launches a browser, with the published web app loaded.</span></span>

<span data-ttu-id="42ee6-132">URL の形式のことを確認して、アプリは*アプリ {name} .azurewebsites .net*です。</span><span class="sxs-lookup"><span data-stu-id="42ee6-132">Notice the format of the URL for the app is *{app name}.azurewebsites.net*.</span></span> <span data-ttu-id="42ee6-133">たとえば、という名前のアプリ`SignalRChattR`次のような URL が指定されて`https://signalrchattr.azurewebsites.net`です。</span><span class="sxs-lookup"><span data-stu-id="42ee6-133">For example, an app named `SignalRChattR` has a URL that looks like `https://signalrchattr.azurewebsites.net`.</span></span>

<span data-ttu-id="42ee6-134">HTTP 502.2 エラーが発生する場合は、次を参照してください。 [Azure App Service に ASP.NET Core の展開のプレビュー リリース](xref:host-and-deploy/azure-apps/index)問題を解決します。</span><span class="sxs-lookup"><span data-stu-id="42ee6-134">If an HTTP 502.2 error occurs, see [Deploy ASP.NET Core preview release to Azure App Service](xref:host-and-deploy/azure-apps/index) to resolve it.</span></span>

## <a name="configure-signalr-web-app"></a><span data-ttu-id="42ee6-135">SignalR web app を構成します。</span><span class="sxs-lookup"><span data-stu-id="42ee6-135">Configure SignalR web app</span></span>

<span data-ttu-id="42ee6-136">ASP.NET Core SignalR 公開されたアプリに Azure Web アプリが必要に応じて[ARR アフィニティ](https://en.wikipedia.org/wiki/Application_Request_Routing)有効にします。</span><span class="sxs-lookup"><span data-stu-id="42ee6-136">ASP.NET Core SignalR apps that are published as an Azure Web App must have [ARR Affinity](https://en.wikipedia.org/wiki/Application_Request_Routing) enabled.</span></span> <span data-ttu-id="42ee6-137">[Websocket](xref:fundamentals/websockets)を有効にする、Websocket トランスポート関数を使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="42ee6-137">[WebSockets](xref:fundamentals/websockets) should be enabled, to allow the WebSockets transport to function.</span></span>

<span data-ttu-id="42ee6-138">Azure ポータルに移動**アプリ設定**を web アプリです。</span><span class="sxs-lookup"><span data-stu-id="42ee6-138">In the Azure portal, navigate to **App Settings** for your web app.</span></span> <span data-ttu-id="42ee6-139">設定**Websocket**に**で**、ことを確認し、 **ARR アフィニティ**は**で**です。</span><span class="sxs-lookup"><span data-stu-id="42ee6-139">Set **WebSockets** to **On**, and verify **ARR Affinity** is **On**.</span></span>

![Azure ポータルで azure の Web アプリ設定](publish-to-azure-web-app/_static/azure-web-app-settings.png)

 <span data-ttu-id="42ee6-141">Websocket トランスポートとその他のトランスポート[App Service プランに基づいて制限されます](/azure/azure-subscription-service-limits#app-service-limits)です。</span><span class="sxs-lookup"><span data-stu-id="42ee6-141">WebSockets and other transports [are limited based on the App Service Plan](/azure/azure-subscription-service-limits#app-service-limits).</span></span>

## <a name="related-resources"></a><span data-ttu-id="42ee6-142">関連資料</span><span class="sxs-lookup"><span data-stu-id="42ee6-142">Related resources</span></span>

* [<span data-ttu-id="42ee6-143">コマンド ライン ツールを使用して Azure に ASP.NET Core アプリケーションを公開します。</span><span class="sxs-lookup"><span data-stu-id="42ee6-143">Publish an ASP.NET Core app to Azure with command line tools</span></span>](xref:tutorials/publish-to-azure-webapp-using-cli?tabs=windows)
* [<span data-ttu-id="42ee6-144">Visual Studio での Azure への ASP.NET Core アプリケーションを公開します。</span><span class="sxs-lookup"><span data-stu-id="42ee6-144">Publish an ASP.NET Core app to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)
* [<span data-ttu-id="42ee6-145">ホストし、Azure での ASP.NET Core プレビュー アプリを展開します。</span><span class="sxs-lookup"><span data-stu-id="42ee6-145">Host and deploy ASP.NET Core Preview apps on Azure</span></span>](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
