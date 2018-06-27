---
title: コマンド ライン ツールを使用して Azure に ASP.NET Core アプリを公開する
author: camsoper
description: Git コマンド ライン クライアントを使用して Azure App Service に ASP.NET Core アプリを発行する方法を説明します。
ms.author: casoper
ms.custom: mvc
ms.date: 11/03/2017
services: multiple
uid: tutorials/publish-to-azure-webapp-using-cli
ms.openlocfilehash: 526ceef469d473706f39cdc3ee645280e99315b1
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279247"
---
# <a name="publish-an-aspnet-core-app-to-azure-with-command-line-tools"></a><span data-ttu-id="1cca0-103">コマンド ライン ツールを使用して Azure に ASP.NET Core アプリを公開する</span><span class="sxs-lookup"><span data-stu-id="1cca0-103">Publish an ASP.NET Core app to Azure with command line tools</span></span>

<span data-ttu-id="1cca0-104">作成者: [Cam Soper](https://twitter.com/camsoper)</span><span class="sxs-lookup"><span data-stu-id="1cca0-104">By [Cam Soper](https://twitter.com/camsoper)</span></span>

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

<span data-ttu-id="1cca0-105">このチュートリアルでは、コマンド ライン ツールを使用して ASP.NET Core アプリをビルドし、Microsoft Azure App Service に展開する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="1cca0-105">This tutorial will show you how to build and deploy an ASP.NET Core app to Microsoft Azure App Service using command line tools.</span></span> <span data-ttu-id="1cca0-106">このチュートリアルを終えると、Azure App Service の Web アプリとしてホストされる ASP.NET Core でビルドした Razor Pages Web アプリが完成します。</span><span class="sxs-lookup"><span data-stu-id="1cca0-106">When finished, you'll have a Razor Pages web app built in ASP.NET Core hosted as an Azure App Service Web App.</span></span> <span data-ttu-id="1cca0-107">本チュートリアルは Windows コマンド ライン ツールを使用して作成されていますが、macOS や Linux の環境にも適用可能です。</span><span class="sxs-lookup"><span data-stu-id="1cca0-107">This tutorial is written using Windows command line tools, but can be applied to macOS and Linux environments, as well.</span></span>

<span data-ttu-id="1cca0-108">このチュートリアルでは、次の作業を行う方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="1cca0-108">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1cca0-109">Azure CLI を使用して Azure App Service を作成する</span><span class="sxs-lookup"><span data-stu-id="1cca0-109">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="1cca0-110">Git コマンド ライン ツールを使用して Azure App Service へ ASP.NET Core アプリを展開する</span><span class="sxs-lookup"><span data-stu-id="1cca0-110">Deploy an ASP.NET Core app to Azure App Service using the Git command line tool</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1cca0-111">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="1cca0-111">Prerequisites</span></span>

<span data-ttu-id="1cca0-112">このチュートリアルを完了するには、次のものが必要です。</span><span class="sxs-lookup"><span data-stu-id="1cca0-112">To complete this tutorial, you'll need:</span></span>

* <span data-ttu-id="1cca0-113">[Microsoft Azure サブスクリプション](https://azure.microsoft.com/free/)</span><span class="sxs-lookup"><span data-stu-id="1cca0-113">A [Microsoft Azure subscription](https://azure.microsoft.com/free/)</span></span>
* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* <span data-ttu-id="1cca0-114">[Git](https://www.git-scm.com/) コマンド ライン クライアント</span><span class="sxs-lookup"><span data-stu-id="1cca0-114">[Git](https://www.git-scm.com/) command line client</span></span>

## <a name="create-a-web-app"></a><span data-ttu-id="1cca0-115">Web アプリの作成</span><span class="sxs-lookup"><span data-stu-id="1cca0-115">Create a web app</span></span>

<span data-ttu-id="1cca0-116">Web アプリ用の新しいディレクトリを作成し、新しい ASP.NET Core Razor Pages アプリを作成してから Web サイトをローカルで実行します。</span><span class="sxs-lookup"><span data-stu-id="1cca0-116">Create a new directory for the web app, create a new ASP.NET Core Razor Pages app, and then run the website locally.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="1cca0-117">Windows</span><span class="sxs-lookup"><span data-stu-id="1cca0-117">Windows</span></span>](#tab/windows)

::: moniker range=">= aspnetcore-2.1"

```console
REM Create a new ASP.NET Core Razor Pages app
dotnet new webapp -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the app
dotnet run
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
REM Create a new ASP.NET Core Razor Pages app
dotnet new razor -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the app
dotnet run
```

::: moniker-end

# <a name="othertabother"></a>[<span data-ttu-id="1cca0-119">その他</span><span class="sxs-lookup"><span data-stu-id="1cca0-119">Other</span></span>](#tab/other)

::: moniker range=">= aspnetcore-2.1"

```bash
# Create a new ASP.NET Core Razor Pages app
dotnet new webapp -o MyApplication

# Change to the new directory that was just created
cd MyApplication

# Run the app
dotnet run
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```bash
# Create a new ASP.NET Core Razor Pages app
dotnet new razor -o MyApplication

# Change to the new directory that was just created
cd MyApplication

# Run the app
dotnet run
```

::: moniker-end

---

![コマンド ライン出力](publish-to-azure-webapp-using-cli/_static/new_prj.png)

<span data-ttu-id="1cca0-122">`http://localhost:5000` にアクセスしてアプリをテストします。</span><span class="sxs-lookup"><span data-stu-id="1cca0-122">Test the app by browsing to `http://localhost:5000`.</span></span>

![ローカルで実行されている Web サイト](publish-to-azure-webapp-using-cli/_static/app_test.png)

## <a name="create-the-azure-app-service-instance"></a><span data-ttu-id="1cca0-124">Azure App Service インスタンスの作成</span><span class="sxs-lookup"><span data-stu-id="1cca0-124">Create the Azure App Service instance</span></span>

<span data-ttu-id="1cca0-125">[Azure Cloud Shell](/azure/cloud-shell/quickstart) を使用して、リソース グループ、App Service プラン、App Service Web アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="1cca0-125">Using the [Azure Cloud Shell](/azure/cloud-shell/quickstart), create a resource group, App Service plan, and an App Service web app.</span></span>

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

<span data-ttu-id="1cca0-126">展開する前に、次のコマンドを使用してアカウントレベルの展開資格情報を設定します。</span><span class="sxs-lookup"><span data-stu-id="1cca0-126">Before deployment, set the account-level deployment credentials using the following command:</span></span>

```azurecli-interactive
az webapp deployment user set --user-name <desired user name> --password <desired password>
```

<span data-ttu-id="1cca0-127">Git を使用してアプリを展開するには、展開 URL が必要です。</span><span class="sxs-lookup"><span data-stu-id="1cca0-127">A deployment URL is needed to deploy the app using Git.</span></span> <span data-ttu-id="1cca0-128">この URL は以下のようにして取得します。</span><span class="sxs-lookup"><span data-stu-id="1cca0-128">Retrieve the URL like this.</span></span>

```azurecli-interactive
az webapp deployment source config-local-git -n $webappname -g DotNetAzureTutorial --query [url] -o tsv
```

<span data-ttu-id="1cca0-129">末尾が `.git` の表示された URL をメモします。</span><span class="sxs-lookup"><span data-stu-id="1cca0-129">Note the displayed URL ending in `.git`.</span></span> <span data-ttu-id="1cca0-130">次の手順で使用します。</span><span class="sxs-lookup"><span data-stu-id="1cca0-130">It's used in the next step.</span></span>

## <a name="deploy-the-app-using-git"></a><span data-ttu-id="1cca0-131">Git を使用してアプリを展開する</span><span class="sxs-lookup"><span data-stu-id="1cca0-131">Deploy the app using Git</span></span>

<span data-ttu-id="1cca0-132">Git を使用してローカル コンピューターから展開する準備ができました。</span><span class="sxs-lookup"><span data-stu-id="1cca0-132">You're ready to deploy from your local machine using Git.</span></span>

> [!NOTE]
> <span data-ttu-id="1cca0-133">行の終わりに関する Git の警告は無視してかまいません。</span><span class="sxs-lookup"><span data-stu-id="1cca0-133">It's safe to ignore any warnings from Git about line endings.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="1cca0-134">Windows</span><span class="sxs-lookup"><span data-stu-id="1cca0-134">Windows</span></span>](#tab/windows)

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

# <a name="othertabother"></a>[<span data-ttu-id="1cca0-135">その他</span><span class="sxs-lookup"><span data-stu-id="1cca0-135">Other</span></span>](#tab/other)

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

<span data-ttu-id="1cca0-136">Git から、先ほど設定した展開資格情報を求められます。</span><span class="sxs-lookup"><span data-stu-id="1cca0-136">Git prompts for the deployment credentials that were set earlier.</span></span> <span data-ttu-id="1cca0-137">認証後、アプリがリモートの展開先へプッシュされ、ビルドされて展開されます。</span><span class="sxs-lookup"><span data-stu-id="1cca0-137">After authenticating, the app will be pushed to the remote location, built, and deployed.</span></span>

![Git 展開の出力](publish-to-azure-webapp-using-cli/_static/post_deploy.png)

## <a name="test-the-app"></a><span data-ttu-id="1cca0-139">アプリのテスト</span><span class="sxs-lookup"><span data-stu-id="1cca0-139">Test the app</span></span>

<span data-ttu-id="1cca0-140">`https://<web app name>.azurewebsites.net` にアクセスしてアプリをテストします。</span><span class="sxs-lookup"><span data-stu-id="1cca0-140">Test the app by browsing to `https://<web app name>.azurewebsites.net`.</span></span> <span data-ttu-id="1cca0-141">Cloud Shell (または Azure CLI) でこのアドレスを表示するには、次を使用します。</span><span class="sxs-lookup"><span data-stu-id="1cca0-141">To display the address in the Cloud Shell (or Azure CLI), use the following:</span></span>

```azurecli-interactive
az webapp show -n $webappname -g DotNetAzureTutorial --query defaultHostName -o tsv
```

![Azure で実行されているアプリ](publish-to-azure-webapp-using-cli/_static/app_deployed.png)

## <a name="clean-up"></a><span data-ttu-id="1cca0-143">クリーンアップ</span><span class="sxs-lookup"><span data-stu-id="1cca0-143">Clean up</span></span>

<span data-ttu-id="1cca0-144">アプリのテストおよびコードとリソースの検査が終了したら、リソース グループを削除して Web アプリおよびプランを削除します。</span><span class="sxs-lookup"><span data-stu-id="1cca0-144">When finished testing the app and inspecting the code and resources, delete the web app and plan by deleting the resource group.</span></span>

```azurecli-interactive
az group delete -n DotNetAzureTutorial
```

## <a name="next-steps"></a><span data-ttu-id="1cca0-145">次の手順</span><span class="sxs-lookup"><span data-stu-id="1cca0-145">Next steps</span></span>

<span data-ttu-id="1cca0-146">このチュートリアルでは、次の作業を行う方法を学びました。</span><span class="sxs-lookup"><span data-stu-id="1cca0-146">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1cca0-147">Azure CLI を使用して Azure App Service を作成する</span><span class="sxs-lookup"><span data-stu-id="1cca0-147">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="1cca0-148">Git コマンド ライン ツールを使用して Azure App Service へ ASP.NET Core アプリを展開する</span><span class="sxs-lookup"><span data-stu-id="1cca0-148">Deploy an ASP.NET Core app to Azure App Service using the Git command line tool</span></span>

<span data-ttu-id="1cca0-149">次は、CosmosDB を使用する既存の Web アプリを、コマンド ラインを使用して展開する方法について学びます。</span><span class="sxs-lookup"><span data-stu-id="1cca0-149">Next, learn to use the command line to deploy an existing web app that uses CosmosDB.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="1cca0-150">.NET Core を使用してコマンド ラインで Azure へ展開する</span><span class="sxs-lookup"><span data-stu-id="1cca0-150">Deploy to Azure from the command line with .NET Core</span></span>](/dotnet/azure/dotnet-quickstart-xplat)
