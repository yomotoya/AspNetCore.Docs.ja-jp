---
title: "コマンド ライン ツールを使用して Azure に ASP.NET Core アプリを公開する"
author: camsoper
description: "Git コマンド ライン クライアントを使用して Azure App Service に ASP.NET Core アプリを発行する方法を説明します。"
manager: wpickett
ms.author: casoper
ms.custom: mvc
ms.date: 11/03/2017
ms.devlang: dotnet
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
services: multiple
uid: tutorials/publish-to-azure-webapp-using-cli
ms.openlocfilehash: 0418a2695d3afb6dc2c55b8f694a97d62239835f
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="deploy-an-aspnet-core-application-to-azure-app-service-from-the-command-line"></a><span data-ttu-id="b8a8b-103">コマンド ラインで Azure App Service へ ASP.NET Core アプリケーションを展開する</span><span class="sxs-lookup"><span data-stu-id="b8a8b-103">Deploy an ASP.NET Core application to Azure App Service from the command line</span></span>

<span data-ttu-id="b8a8b-104">作成者: [Cam Soper](https://twitter.com/camsoper)</span><span class="sxs-lookup"><span data-stu-id="b8a8b-104">By [Cam Soper](https://twitter.com/camsoper)</span></span>

<span data-ttu-id="b8a8b-105">このチュートリアルでは、コマンド ライン ツールを使用して ASP.NET Core アプリケーションをビルドし、Microsoft Azure App Service に展開する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="b8a8b-105">This tutorial will show you how to build and deploy an ASP.NET Core application to Microsoft Azure App Service using command line tools.</span></span>  <span data-ttu-id="b8a8b-106">このチュートリアルを終えると、Azure App Service の Web アプリとしてホストされる ASP.NET MVC Core でビルドした Web アプリケーションが完成します。</span><span class="sxs-lookup"><span data-stu-id="b8a8b-106">When finished, you'll have a web application built in ASP.NET MVC Core hosted as an Azure App Service Web App.</span></span>  <span data-ttu-id="b8a8b-107">本チュートリアルは Windows コマンド ライン ツールを使用して作成されていますが、macOS や Linux の環境にも適用可能です。</span><span class="sxs-lookup"><span data-stu-id="b8a8b-107">This tutorial is written using Windows command line tools, but can be applied to macOS and Linux environments, as well.</span></span>  

<span data-ttu-id="b8a8b-108">このチュートリアルでは、次の作業を行う方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="b8a8b-108">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b8a8b-109">Azure CLI を使用して Azure App Service を作成する</span><span class="sxs-lookup"><span data-stu-id="b8a8b-109">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="b8a8b-110">Git コマンド ラインを使用して Azure App Service へ ASP.NET Core アプリケーションを展開する</span><span class="sxs-lookup"><span data-stu-id="b8a8b-110">Deploy an ASP.NET Core application to Azure App Service using the Git command line tool</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b8a8b-111">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="b8a8b-111">Prerequisites</span></span>

<span data-ttu-id="b8a8b-112">このチュートリアルを完了するには、次のものが必要です。</span><span class="sxs-lookup"><span data-stu-id="b8a8b-112">To complete this tutorial, you'll need:</span></span>

* <span data-ttu-id="b8a8b-113">[Microsoft Azure サブスクリプション](https://azure.microsoft.com/free/)</span><span class="sxs-lookup"><span data-stu-id="b8a8b-113">A [Microsoft Azure subscription](https://azure.microsoft.com/free/)</span></span>
* [<span data-ttu-id="b8a8b-114">.NET Core</span><span class="sxs-lookup"><span data-stu-id="b8a8b-114">.NET Core</span></span>](https://www.microsoft.com/net/download/core)
* <span data-ttu-id="b8a8b-115">[Git](https://www.git-scm.com/) コマンド ライン クライアント</span><span class="sxs-lookup"><span data-stu-id="b8a8b-115">[Git](https://www.git-scm.com/) command line client</span></span>

## <a name="create-a-web-application"></a><span data-ttu-id="b8a8b-116">Web アプリケーションの作成</span><span class="sxs-lookup"><span data-stu-id="b8a8b-116">Create a web application</span></span>

<span data-ttu-id="b8a8b-117">Web アプリケーション用の新しいディレクトリを作成し、新しい ASP.NET Core MVC アプリケーションを作成してから Web サイトをローカルで実行します。</span><span class="sxs-lookup"><span data-stu-id="b8a8b-117">Create a new directory for the web application, create a new ASP.NET Core MVC application, and then run the website locally.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="b8a8b-118">Windows</span><span class="sxs-lookup"><span data-stu-id="b8a8b-118">Windows</span></span>](#tab/windows)
```cmd
REM Create a new ASP.NET Core MVC application
dotnet new razor -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the application
dotnet run
```

# <a name="othertabother"></a>[<span data-ttu-id="b8a8b-119">その他</span><span class="sxs-lookup"><span data-stu-id="b8a8b-119">Other</span></span>](#tab/other)
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

<span data-ttu-id="b8a8b-121">http://localhost:5000 にアクセスしてアプリケーションをテストします。</span><span class="sxs-lookup"><span data-stu-id="b8a8b-121">Test the application by browsing to http://localhost:5000.</span></span>

![ローカルで実行されている Web サイト](publish-to-azure-webapp-using-cli/_static/app_test.png)


## <a name="create-the-azure-app-service-instance"></a><span data-ttu-id="b8a8b-123">Azure App Service インスタンスの作成</span><span class="sxs-lookup"><span data-stu-id="b8a8b-123">Create the Azure App Service instance</span></span>

<span data-ttu-id="b8a8b-124">[Azure Cloud Shell](/azure/cloud-shell/quickstart) を使用して、リソース グループ、App Service プラン、App Service Web アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="b8a8b-124">Using the [Azure Cloud Shell](/azure/cloud-shell/quickstart), create a resource group, App Service plan, and an App Service web app.</span></span>

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

<span data-ttu-id="b8a8b-125">展開する前に、次のコマンドを使用してアカウントレベルの展開資格情報を設定します。</span><span class="sxs-lookup"><span data-stu-id="b8a8b-125">Before deployment, set the account-level deployment credentials using the following command:</span></span>

```azurecli-interactive
az webapp deployment user set --user-name <desired user name> --password <desired password>
```

<span data-ttu-id="b8a8b-126">Git を使用してアプリケーションを展開するには、展開 URL が必要です。</span><span class="sxs-lookup"><span data-stu-id="b8a8b-126">A deployment URL is needed to deploy the application using Git.</span></span>  <span data-ttu-id="b8a8b-127">この URL は以下のようにして取得します。</span><span class="sxs-lookup"><span data-stu-id="b8a8b-127">Retrieve the URL like this.</span></span>

```azurecli-interactive
az webapp deployment source config-local-git -n $webappname -g DotNetAzureTutorial --query [url] -o tsv
```
<span data-ttu-id="b8a8b-128">末尾が `.git` の表示された URL をメモします。</span><span class="sxs-lookup"><span data-stu-id="b8a8b-128">Note the displayed URL ending in `.git`.</span></span> <span data-ttu-id="b8a8b-129">次の手順で使用します。</span><span class="sxs-lookup"><span data-stu-id="b8a8b-129">It's used in the next step.</span></span>

## <a name="deploy-the-application-using-git"></a><span data-ttu-id="b8a8b-130">Git を使用してアプリケーションを展開する</span><span class="sxs-lookup"><span data-stu-id="b8a8b-130">Deploy the application using Git</span></span>

<span data-ttu-id="b8a8b-131">Git を使用してローカル コンピューターから展開する準備ができました。</span><span class="sxs-lookup"><span data-stu-id="b8a8b-131">You're ready to deploy from your local machine using Git.</span></span>

> [!NOTE]
> <span data-ttu-id="b8a8b-132">行の終わりに関する Git の警告は無視してかまいません。</span><span class="sxs-lookup"><span data-stu-id="b8a8b-132">It's safe to ignore any warnings from Git about line endings.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="b8a8b-133">Windows</span><span class="sxs-lookup"><span data-stu-id="b8a8b-133">Windows</span></span>](#tab/windows)
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

# <a name="othertabother"></a>[<span data-ttu-id="b8a8b-134">その他</span><span class="sxs-lookup"><span data-stu-id="b8a8b-134">Other</span></span>](#tab/other)
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

<span data-ttu-id="b8a8b-135">Git から、先ほど設定した展開資格情報を求められます。</span><span class="sxs-lookup"><span data-stu-id="b8a8b-135">Git prompts for the deployment credentials that were set earlier.</span></span> <span data-ttu-id="b8a8b-136">認証後、アプリケーションがリモートの展開先へプッシュされ、ビルドされて展開されます。</span><span class="sxs-lookup"><span data-stu-id="b8a8b-136">After authenticating, the application will be pushed to the remote location, built, and deployed.</span></span>

![Git 展開の出力](publish-to-azure-webapp-using-cli/_static/post_deploy.png)

## <a name="test-the-application"></a><span data-ttu-id="b8a8b-138">アプリケーションをテストする</span><span class="sxs-lookup"><span data-stu-id="b8a8b-138">Test the application</span></span>

<span data-ttu-id="b8a8b-139">`https://<web app name>.azurewebsites.net` にアクセスしてアプリケーションをテストします。</span><span class="sxs-lookup"><span data-stu-id="b8a8b-139">Test the application by browsing to `https://<web app name>.azurewebsites.net`.</span></span>  <span data-ttu-id="b8a8b-140">Cloud Shell (または Azure CLI) でこのアドレスを表示するには、次を使用します。</span><span class="sxs-lookup"><span data-stu-id="b8a8b-140">To display the address in the Cloud Shell (or Azure CLI), use the following:</span></span>

```azurecli-interactive
az webapp show -n $webappname -g DotNetAzureTutorial --query defaultHostName -o tsv
```

![Azure で実行されているアプリ](publish-to-azure-webapp-using-cli/_static/app_deployed.png)

## <a name="clean-up"></a><span data-ttu-id="b8a8b-142">クリーンアップ</span><span class="sxs-lookup"><span data-stu-id="b8a8b-142">Clean up</span></span>

<span data-ttu-id="b8a8b-143">アプリのテストおよびコードとリソースの検査が終了したら、リソース グループを削除して Web アプリおよびプランを削除します。</span><span class="sxs-lookup"><span data-stu-id="b8a8b-143">When finished testing the app and inspecting the code and resources, delete the web app and plan by deleting the resource group.</span></span>

```azurecli-interactive
az group delete -n DotNetAzureTutorial
```

## <a name="next-steps"></a><span data-ttu-id="b8a8b-144">次の手順</span><span class="sxs-lookup"><span data-stu-id="b8a8b-144">Next steps</span></span>

<span data-ttu-id="b8a8b-145">このチュートリアルでは、次の作業を行う方法を学びました。</span><span class="sxs-lookup"><span data-stu-id="b8a8b-145">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b8a8b-146">Azure CLI を使用して Azure App Service を作成する</span><span class="sxs-lookup"><span data-stu-id="b8a8b-146">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="b8a8b-147">Git コマンド ラインを使用して Azure App Service へ ASP.NET Core アプリケーションを展開する</span><span class="sxs-lookup"><span data-stu-id="b8a8b-147">Deploy an ASP.NET Core application to Azure App Service using the Git command line tool</span></span>

<span data-ttu-id="b8a8b-148">次は、CosmosDB を使用する既存の Web アプリを、コマンド ラインを使用して展開する方法について学びます。</span><span class="sxs-lookup"><span data-stu-id="b8a8b-148">Next, learn to use the command line to deploy an existing web app that uses CosmosDB.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="b8a8b-149">.NET Core を使用してコマンド ラインで Azure へ展開する</span><span class="sxs-lookup"><span data-stu-id="b8a8b-149">Deploy to Azure from the command line with .NET Core</span></span>](/dotnet/azure/dotnet-quickstart-xplat)
