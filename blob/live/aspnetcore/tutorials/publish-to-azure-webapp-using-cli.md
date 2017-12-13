---
title: "コマンド ライン ツールを使用して Azure に ASP.NET Core アプリを公開する | Microsoft Docs"
description: "ASP.NET Core と Git コマンド ライン クライアントを使用して Microsoft Azure App をビルドし展開する方法について説明します。"
services: multiple
keywords: "ASP.NET Core, Azure, App Service, Git, コマンド ライン"
author: camsoper
ms.author: casoper
manager: wpickett
ms.date: 11/03/2017
ms.topic: get-started-article
ms.prod: asp.net-core
ms.technology: aspnet
ms.custom: mvc
ms.devlang: dotnet
uid: tutorials/publish-to-azure-webapp-using-cli
ms.openlocfilehash: 0bcff4f79356b960f663dcebb1d79a108417dbd2
ms.sourcegitcommit: f017f940a164dbaf84307410c78eb14e0f3ac811
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/15/2017
---
# <a name="deploy-an-aspnet-core-application-to-azure-app-service-from-the-command-line"></a><span data-ttu-id="25e01-104">コマンド ラインで Azure App Service へ ASP.NET Core アプリケーションを展開する</span><span class="sxs-lookup"><span data-stu-id="25e01-104">Deploy an ASP.NET Core application to Azure App Service from the command line</span></span>

<span data-ttu-id="25e01-105">作成者: [Cam Soper](https://twitter.com/camsoper)</span><span class="sxs-lookup"><span data-stu-id="25e01-105">By [Cam Soper](https://twitter.com/camsoper)</span></span>

<span data-ttu-id="25e01-106">このチュートリアルでは、コマンド ライン ツールを使用して ASP.NET Core アプリケーションをビルドし、Microsoft Azure App Service に展開する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="25e01-106">This tutorial will show you how to build and deploy an ASP.NET Core application to Microsoft Azure App Service using command line tools.</span></span>  <span data-ttu-id="25e01-107">このチュートリアルを終えると、Azure App Service の Web アプリとしてホストされる ASP.NET MVC Core でビルドした Web アプリケーションが完成します。</span><span class="sxs-lookup"><span data-stu-id="25e01-107">When finished, you'll have a web application built in ASP.NET MVC Core hosted as an Azure App Service Web App.</span></span>  <span data-ttu-id="25e01-108">本チュートリアルは Windows コマンド ライン ツールを使用して作成されていますが、macOS や Linux の環境にも適用可能です。</span><span class="sxs-lookup"><span data-stu-id="25e01-108">This tutorial is written using Windows command line tools, but can be applied to macOS and Linux environments, as well.</span></span>  

<span data-ttu-id="25e01-109">このチュートリアルでは、次の作業を行う方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="25e01-109">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="25e01-110">Azure CLI を使用して Azure App Service を作成する</span><span class="sxs-lookup"><span data-stu-id="25e01-110">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="25e01-111">Git コマンド ラインを使用して Azure App Service へ ASP.NET Core アプリケーションを展開する</span><span class="sxs-lookup"><span data-stu-id="25e01-111">Deploy an ASP.NET Core application to Azure App Service using the Git command line tool</span></span>

## <a name="prerequisites"></a><span data-ttu-id="25e01-112">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="25e01-112">Prerequisites</span></span>

<span data-ttu-id="25e01-113">このチュートリアルを完了するには、次のものが必要です。</span><span class="sxs-lookup"><span data-stu-id="25e01-113">To complete this tutorial, you'll need:</span></span>

* <span data-ttu-id="25e01-114">[Microsoft Azure サブスクリプション](https://azure.microsoft.com/free/)</span><span class="sxs-lookup"><span data-stu-id="25e01-114">A [Microsoft Azure subscription](https://azure.microsoft.com/free/)</span></span>
* [<span data-ttu-id="25e01-115">.NET Core</span><span class="sxs-lookup"><span data-stu-id="25e01-115">.NET Core</span></span>](https://www.microsoft.com/net/download/core)
* <span data-ttu-id="25e01-116">[Git](https://www.git-scm.com/) コマンド ライン クライアント</span><span class="sxs-lookup"><span data-stu-id="25e01-116">[Git](https://www.git-scm.com/) command line client</span></span>

## <a name="create-a-web-application"></a><span data-ttu-id="25e01-117">Web アプリケーションの作成</span><span class="sxs-lookup"><span data-stu-id="25e01-117">Create a web application</span></span>

<span data-ttu-id="25e01-118">Web アプリケーション用の新しいディレクトリを作成し、新しい ASP.NET Core MVC アプリケーションを作成してから Web サイトをローカルで実行します。</span><span class="sxs-lookup"><span data-stu-id="25e01-118">Create a new directory for the web application, create a new ASP.NET Core MVC application, and then run the website locally.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="25e01-119">Windows</span><span class="sxs-lookup"><span data-stu-id="25e01-119">Windows</span></span>](#tab/windows)
```cmd
REM Create a new ASP.NET Core MVC application
dotnet new razor -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the application
dotnet run
```

# <a name="othertabother"></a>[<span data-ttu-id="25e01-120">その他</span><span class="sxs-lookup"><span data-stu-id="25e01-120">Other</span></span>](#tab/other)
```bash
# Create a new ASP.NET Core MVC application
dotnet new razor -o MyApplication

# Change to the new directory that was just created
cd MyApplication

# Run the application
dotnet run
```
---

![コマンド ライン出力](publish-to-azure-webapp-using-cli/_static/new_prj.png)

<span data-ttu-id="25e01-122">http://localhost:5000 にアクセスしてアプリケーションをテストします。</span><span class="sxs-lookup"><span data-stu-id="25e01-122">Test the application by browsing to http://localhost:5000.</span></span>

![ローカルで実行されている Web サイト](publish-to-azure-webapp-using-cli/_static/app_test.png)


## <a name="create-the-azure-app-service-instance"></a><span data-ttu-id="25e01-124">Azure App Service インスタンスの作成</span><span class="sxs-lookup"><span data-stu-id="25e01-124">Create the Azure App Service instance</span></span>

<span data-ttu-id="25e01-125">[Azure Cloud Shell](/azure/cloud-shell/quickstart) を使用して、リソース グループ、App Service プラン、App Service Web アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="25e01-125">Using the [Azure Cloud Shell](/azure/cloud-shell/quickstart), create a resource group, App Service plan, and an App Service web app.</span></span>

```azurecli-interactive
# Generate a unique Web App name
let randomNum=$RANDOM*$RANDOM
webappname=tutorialApp$randomNum

# Create the DotNetAzureTutorial resource group
az group create --name DotNetAzureTutorial --location EastUS

# Create an App Service plan.
az appservice plan create --name $webappname --resource-group DotNetAzureTutorial --sku FREE

# Create the Web App
az webapp create --name $webappname --resource-group DotNetAzureTutorial --plan $webappname
```

<span data-ttu-id="25e01-126">展開する前に、次のコマンドを使用してアカウントレベルの展開資格情報を設定します。</span><span class="sxs-lookup"><span data-stu-id="25e01-126">Before deployment, set the account-level deployment credentials using the following command:</span></span>

```azurecli-interactive
az webapp deployment user set --user-name <desired user name> --password <desired password>
```

<span data-ttu-id="25e01-127">Git を使用してアプリケーションを展開するには、展開 URL が必要です。</span><span class="sxs-lookup"><span data-stu-id="25e01-127">A deployment URL is needed to deploy the application using Git.</span></span>  <span data-ttu-id="25e01-128">この URL は以下のようにして取得します。</span><span class="sxs-lookup"><span data-stu-id="25e01-128">Retrieve the URL like this.</span></span>

```azurecli-interactive
az webapp deployment source config-local-git -n $webappname -g DotNetAzureTutorial --query [url] -o tsv
```
<span data-ttu-id="25e01-129">末尾が `.git` の表示された URL をメモします。</span><span class="sxs-lookup"><span data-stu-id="25e01-129">Note the displayed URL ending in `.git`.</span></span> <span data-ttu-id="25e01-130">次の手順で使用します。</span><span class="sxs-lookup"><span data-stu-id="25e01-130">It's used in the next step.</span></span>

## <a name="deploy-the-application-using-git"></a><span data-ttu-id="25e01-131">Git を使用してアプリケーションを展開する</span><span class="sxs-lookup"><span data-stu-id="25e01-131">Deploy the application using Git</span></span>

<span data-ttu-id="25e01-132">Git を使用してローカル コンピューターから展開する準備ができました。</span><span class="sxs-lookup"><span data-stu-id="25e01-132">You're ready to deploy from your local machine using Git.</span></span>

> [!NOTE]
> <span data-ttu-id="25e01-133">行の終わりに関する Git の警告は無視してかまいません。</span><span class="sxs-lookup"><span data-stu-id="25e01-133">It's safe to ignore any warnings from Git about line endings.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="25e01-134">Windows</span><span class="sxs-lookup"><span data-stu-id="25e01-134">Windows</span></span>](#tab/windows)
```cmd
REM Initialize the local Git repository
git init

REM Add the contents of the working directory to the repo
git add --all

REM Commit the changes to the local repo
git commit -a -m "Initial commit"

REM Add the URL as a Git remote repository
git remote add azure <THE GIT URL YOU NOTED EARLIER>

REM Push the local repository to the remote
git push azure master
```

# <a name="othertabother"></a>[<span data-ttu-id="25e01-135">その他</span><span class="sxs-lookup"><span data-stu-id="25e01-135">Other</span></span>](#tab/other)
```bash
# Initialize the local Git repository
git init

# Add the contents of the working directory to the repo
git add --all

# Commit the changes to the local repo
git commit -a -m "Initial commit"

# Add the URL as a Git remote repository
git remote add azure <THE GIT URL YOU NOTED EARLIER>

# Push the local repository to the remote
git push azure master
```
---

<span data-ttu-id="25e01-136">Git から、先ほど設定した展開資格情報を求められます。</span><span class="sxs-lookup"><span data-stu-id="25e01-136">Git will prompt for the deployment credentials that were set earlier.</span></span>  <span data-ttu-id="25e01-137">認証後、アプリケーションがリモートの展開先へプッシュされ、ビルドされて展開されます。</span><span class="sxs-lookup"><span data-stu-id="25e01-137">After authenticating, the application will be pushed to the remote location, built, and deployed.</span></span>

![Git 展開の出力](publish-to-azure-webapp-using-cli/_static/post_deploy.png)

## <a name="test-the-application"></a><span data-ttu-id="25e01-139">アプリケーションをテストする</span><span class="sxs-lookup"><span data-stu-id="25e01-139">Test the application</span></span>

<span data-ttu-id="25e01-140">`https://<web app name>.azurewebsites.net` にアクセスしてアプリケーションをテストします。</span><span class="sxs-lookup"><span data-stu-id="25e01-140">Test the application by browsing to `https://<web app name>.azurewebsites.net`.</span></span>  <span data-ttu-id="25e01-141">Cloud Shell (または Azure CLI) でこのアドレスを表示するには、次を使用します。</span><span class="sxs-lookup"><span data-stu-id="25e01-141">To display the address in the Cloud Shell (or Azure CLI), use the following:</span></span>

```azurecli-interactive
az webapp show -n $webappname -g DotNetAzureTutorial --query defaultHostName -o tsv
```

![Azure で実行されているアプリ](publish-to-azure-webapp-using-cli/_static/app_deployed.png)

## <a name="clean-up"></a><span data-ttu-id="25e01-143">クリーンアップ</span><span class="sxs-lookup"><span data-stu-id="25e01-143">Clean up</span></span>

<span data-ttu-id="25e01-144">アプリのテストおよびコードとリソースの検査が終了したら、リソース グループを削除して Web アプリおよびプランを削除します。</span><span class="sxs-lookup"><span data-stu-id="25e01-144">When finished testing the app and inspecting the code and resources, delete the web app and plan by deleting the resource group.</span></span>

```azurecli-interactive
az group delete -n DotNetAzureTutorial
```

## <a name="next-steps"></a><span data-ttu-id="25e01-145">次のステップ</span><span class="sxs-lookup"><span data-stu-id="25e01-145">Next steps</span></span>

<span data-ttu-id="25e01-146">このチュートリアルでは、次の作業を行う方法を学びました。</span><span class="sxs-lookup"><span data-stu-id="25e01-146">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="25e01-147">Azure CLI を使用して Azure App Service を作成する</span><span class="sxs-lookup"><span data-stu-id="25e01-147">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="25e01-148">Git コマンド ラインを使用して Azure App Service へ ASP.NET Core アプリケーションを展開する</span><span class="sxs-lookup"><span data-stu-id="25e01-148">Deploy an ASP.NET Core application to Azure App Service using the Git command line tool</span></span>

<span data-ttu-id="25e01-149">次は、CosmosDB を使用する既存の Web アプリを、コマンド ラインを使用して展開する方法について学びます。</span><span class="sxs-lookup"><span data-stu-id="25e01-149">Next, learn to use the command line to deploy an existing web app that uses CosmosDB.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="25e01-150">.NET Core を使用してコマンド ラインで Azure へ展開する</span><span class="sxs-lookup"><span data-stu-id="25e01-150">Deploy to Azure from the command line with .NET Core</span></span>](/dotnet/azure/dotnet-quickstart-xplat)
