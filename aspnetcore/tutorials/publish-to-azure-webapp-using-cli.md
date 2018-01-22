---
title: "コマンド ライン ツールを使用して Azure に ASP.NET Core アプリを公開する | Microsoft Docs"
description: "Git コマンド ライン クライアントを使用して Azure App Service に ASP.NET Core アプリを発行する方法を説明します。"
services: multiple
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
ms.openlocfilehash: 4797260f95443954e86aae1614140c0caa5ca8bd
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="deploy-an-aspnet-core-application-to-azure-app-service-from-the-command-line"></a>コマンド ラインで Azure App Service へ ASP.NET Core アプリケーションを展開する

作成者: [Cam Soper](https://twitter.com/camsoper)

このチュートリアルでは、コマンド ライン ツールを使用して ASP.NET Core アプリケーションをビルドし、Microsoft Azure App Service に展開する方法について説明します。  このチュートリアルを終えると、Azure App Service の Web アプリとしてホストされる ASP.NET MVC Core でビルドした Web アプリケーションが完成します。  本チュートリアルは Windows コマンド ライン ツールを使用して作成されていますが、macOS や Linux の環境にも適用可能です。  

このチュートリアルでは、次の作業を行う方法について説明します。

> [!div class="checklist"]
> * Azure CLI を使用して Azure App Service を作成する
> * Git コマンド ラインを使用して Azure App Service へ ASP.NET Core アプリケーションを展開する

## <a name="prerequisites"></a>必須コンポーネント

このチュートリアルを完了するには、次のものが必要です。

* [Microsoft Azure サブスクリプション](https://azure.microsoft.com/free/)
* [.NET Core](https://www.microsoft.com/net/download/core)
* [Git](https://www.git-scm.com/) コマンド ライン クライアント

## <a name="create-a-web-application"></a>Web アプリケーションの作成

Web アプリケーション用の新しいディレクトリを作成し、新しい ASP.NET Core MVC アプリケーションを作成してから Web サイトをローカルで実行します。

# <a name="windowstabwindows"></a>[Windows](#tab/windows)
```cmd
REM Create a new ASP.NET Core MVC application
dotnet new razor -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the application
dotnet run
```

# <a name="othertabother"></a>[その他](#tab/other)
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

http://localhost:5000 にアクセスしてアプリケーションをテストします。

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

Git を使用してアプリケーションを展開するには、展開 URL が必要です。  この URL は以下のようにして取得します。

```azurecli-interactive
az webapp deployment source config-local-git -n $webappname -g DotNetAzureTutorial --query [url] -o tsv
```
末尾が `.git` の表示された URL をメモします。 次の手順で使用します。

## <a name="deploy-the-application-using-git"></a>Git を使用してアプリケーションを展開する

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

Git から、先ほど設定した展開資格情報を求められます。  認証後、アプリケーションがリモートの展開先へプッシュされ、ビルドされて展開されます。

![Git 展開の出力](publish-to-azure-webapp-using-cli/_static/post_deploy.png)

## <a name="test-the-application"></a>アプリケーションをテストする

`https://<web app name>.azurewebsites.net` にアクセスしてアプリケーションをテストします。  Cloud Shell (または Azure CLI) でこのアドレスを表示するには、次を使用します。

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
> * Git コマンド ラインを使用して Azure App Service へ ASP.NET Core アプリケーションを展開する

次は、CosmosDB を使用する既存の Web アプリを、コマンド ラインを使用して展開する方法について学びます。

> [!div class="nextstepaction"]
> [.NET Core を使用してコマンド ラインで Azure へ展開する](/dotnet/azure/dotnet-quickstart-xplat)
