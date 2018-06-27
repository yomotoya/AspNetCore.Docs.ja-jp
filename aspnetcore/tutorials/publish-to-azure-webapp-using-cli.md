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
# <a name="publish-an-aspnet-core-app-to-azure-with-command-line-tools"></a>コマンド ライン ツールを使用して Azure に ASP.NET Core アプリを公開する

作成者: [Cam Soper](https://twitter.com/camsoper)

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

このチュートリアルでは、コマンド ライン ツールを使用して ASP.NET Core アプリをビルドし、Microsoft Azure App Service に展開する方法について説明します。 このチュートリアルを終えると、Azure App Service の Web アプリとしてホストされる ASP.NET Core でビルドした Razor Pages Web アプリが完成します。 本チュートリアルは Windows コマンド ライン ツールを使用して作成されていますが、macOS や Linux の環境にも適用可能です。

このチュートリアルでは、次の作業を行う方法について説明します。

> [!div class="checklist"]
> * Azure CLI を使用して Azure App Service を作成する
> * Git コマンド ライン ツールを使用して Azure App Service へ ASP.NET Core アプリを展開する

## <a name="prerequisites"></a>必須コンポーネント

このチュートリアルを完了するには、次のものが必要です。

* [Microsoft Azure サブスクリプション](https://azure.microsoft.com/free/)
* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* [Git](https://www.git-scm.com/) コマンド ライン クライアント

## <a name="create-a-web-app"></a>Web アプリの作成

Web アプリ用の新しいディレクトリを作成し、新しい ASP.NET Core Razor Pages アプリを作成してから Web サイトをローカルで実行します。

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

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

# <a name="othertabother"></a>[その他](#tab/other)

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

`http://localhost:5000` にアクセスしてアプリをテストします。

![ローカルで実行されている Web サイト](publish-to-azure-webapp-using-cli/_static/app_test.png)

## <a name="create-the-azure-app-service-instance"></a>Azure App Service インスタンスの作成

[Azure Cloud Shell](/azure/cloud-shell/quickstart) を使用して、リソース グループ、App Service プラン、App Service Web アプリを作成します。

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

展開する前に、次のコマンドを使用してアカウントレベルの展開資格情報を設定します。

```azurecli-interactive
az webapp deployment user set --user-name <desired user name> --password <desired password>
```

Git を使用してアプリを展開するには、展開 URL が必要です。 この URL は以下のようにして取得します。

```azurecli-interactive
az webapp deployment source config-local-git -n $webappname -g DotNetAzureTutorial --query [url] -o tsv
```

末尾が `.git` の表示された URL をメモします。 次の手順で使用します。

## <a name="deploy-the-app-using-git"></a>Git を使用してアプリを展開する

Git を使用してローカル コンピューターから展開する準備ができました。

> [!NOTE]
> 行の終わりに関する Git の警告は無視してかまいません。

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

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

# <a name="othertabother"></a>[その他](#tab/other)

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

Git から、先ほど設定した展開資格情報を求められます。 認証後、アプリがリモートの展開先へプッシュされ、ビルドされて展開されます。

![Git 展開の出力](publish-to-azure-webapp-using-cli/_static/post_deploy.png)

## <a name="test-the-app"></a>アプリのテスト

`https://<web app name>.azurewebsites.net` にアクセスしてアプリをテストします。 Cloud Shell (または Azure CLI) でこのアドレスを表示するには、次を使用します。

```azurecli-interactive
az webapp show -n $webappname -g DotNetAzureTutorial --query defaultHostName -o tsv
```

![Azure で実行されているアプリ](publish-to-azure-webapp-using-cli/_static/app_deployed.png)

## <a name="clean-up"></a>クリーンアップ

アプリのテストおよびコードとリソースの検査が終了したら、リソース グループを削除して Web アプリおよびプランを削除します。

```azurecli-interactive
az group delete -n DotNetAzureTutorial
```

## <a name="next-steps"></a>次の手順

このチュートリアルでは、次の作業を行う方法を学びました。

> [!div class="checklist"]
> * Azure CLI を使用して Azure App Service を作成する
> * Git コマンド ライン ツールを使用して Azure App Service へ ASP.NET Core アプリを展開する

次は、CosmosDB を使用する既存の Web アプリを、コマンド ラインを使用して展開する方法について学びます。

> [!div class="nextstepaction"]
> [.NET Core を使用してコマンド ラインで Azure へ展開する](/dotnet/azure/dotnet-quickstart-xplat)
