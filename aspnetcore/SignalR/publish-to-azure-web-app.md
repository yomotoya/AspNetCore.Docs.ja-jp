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
# <a name="publish-an-aspnet-core-signalr-app-to-an-azure-web-app"></a>ASP.NET Core の発行、Azure Web アプリに SignalR アプリ

[Azure の Web アプリ](/azure/app-service/app-service-web-overview)は、 [Microsoft クラウド コンピューティング](https://azure.microsoft.com/)ASP.NET Core を含め、web アプリをホストするためのプラットフォーム サービスです。

## <a name="publish-the-app"></a>アプリの発行

Visual Studio は、Azure Web アプリに発行のための組み込みのツールを提供します。 Visual Studio Code のユーザーが使用できる[Azure CLI](/cli/azure)アプリを Azure に発行するコマンド。 この記事では、Visual Studio でツールを使用して発行について説明します。 Azure CLI を使用してアプリを発行するを参照してください。[コマンド ライン ツールを使用して Azure に ASP.NET Core アプリケーションを公開](xref:tutorials/publish-to-azure-webapp-using-cli)です。

プロジェクトを右クリックして**ソリューション エクスプ ローラー**選択**発行**です。 いることを確認**新規作成**がオンになって、**発行先の選択**ダイアログ、および選択**発行**です。

![選択対象をパブリッシュします。](publish-to-azure-web-app/_static/pick-publish-target-dialog.png)

次の情報を入力、 **App Service の作成**ダイアログと選択**作成**です。

| アイテム | 説明 |
| ---- | ----------- |
| **アプリ名** | アプリの一意の名前。 |
| **サブスクリプション** | アプリで使用する Azure サブスクリプション。 |
| **リソース グループ** | 関連するリソースをアプリが所属するグループ。  |
| **ホスティング プラン** | Web アプリの料金プランです。 |

![アプリ サービスを作成します。](publish-to-azure-web-app/_static/create-app-service-dialog.png)

Visual Studio では、次のタスクを実行します。

* 発行プロファイルを作成する発行の設定を格納します。
* 作成するか、既存の*Azure Web アプリ*提供された詳細とします。
* アプリを発行します。
* 読み込まれた公開された web アプリと、ブラウザーを起動します。

URL の形式のことを確認して、アプリは*アプリ {name} .azurewebsites .net*です。 たとえば、という名前のアプリ`SignalRChattR`次のような URL が指定されて`https://signalrchattr.azurewebsites.net`です。

HTTP 502.2 エラーが発生する場合は、次を参照してください。 [Azure App Service に ASP.NET Core の展開のプレビュー リリース](xref:host-and-deploy/azure-apps/index)問題を解決します。

## <a name="configure-signalr-web-app"></a>SignalR web app を構成します。

ASP.NET Core SignalR 公開されたアプリに Azure Web アプリが必要に応じて[ARR アフィニティ](https://en.wikipedia.org/wiki/Application_Request_Routing)有効にします。 [Websocket](xref:fundamentals/websockets)を有効にする、Websocket トランスポート関数を使用できるようにします。

Azure ポータルに移動**アプリ設定**を web アプリです。 設定**Websocket**に**で**、ことを確認し、 **ARR アフィニティ**は**で**です。

![Azure ポータルで azure の Web アプリ設定](publish-to-azure-web-app/_static/azure-web-app-settings.png)

 Websocket トランスポートとその他のトランスポート[App Service プランに基づいて制限されます](/azure/azure-subscription-service-limits#app-service-limits)です。

## <a name="related-resources"></a>関連資料

* [コマンド ライン ツールを使用して Azure に ASP.NET Core アプリケーションを公開します。](xref:tutorials/publish-to-azure-webapp-using-cli?tabs=windows)
* [Visual Studio での Azure への ASP.NET Core アプリケーションを公開します。](xref:tutorials/publish-to-azure-webapp-using-vs)
* [ホストし、Azure での ASP.NET Core プレビュー アプリを展開します。](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
