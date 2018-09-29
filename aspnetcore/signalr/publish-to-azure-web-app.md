---
title: 発行、ASP.NET Core SignalR のアプリを Azure Web アプリに
author: tdykstra
description: 発行、ASP.NET Core SignalR のアプリを Azure Web アプリに
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 04/20/2018
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: a6a0e44f5c67fefdac6bd26b3772c23e75f8bfc1
ms.sourcegitcommit: 32f5ee0690604d451f61e9a5c28881c9fcf85738
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2018
ms.locfileid: "47454727"
---
# <a name="publish-an-aspnet-core-signalr-app-to-an-azure-web-app"></a>発行、ASP.NET Core SignalR アプリケーションを Azure Web アプリ

[Azure Web アプリの](/azure/app-service/app-service-web-overview)は、 [Microsoft クラウド コンピューティング](https://azure.microsoft.com/)ASP.NET Core を含む、web アプリをホストするためのプラットフォーム サービスです。

> [!NOTE]
> この記事では、Visual Studio から ASP.NET Core SignalR アプリの発行を指します。 参照してください[for Azure SignalR サービス](https://azure.microsoft.com/en-gb/services/signalr-service?)詳細については、Azure で SignalR を使用します。

## <a name="publish-the-app"></a>アプリの発行

Visual Studio は、Azure Web App に発行のための組み込みのツールを提供します。 Visual Studio Code のユーザーが使用できる[Azure CLI](/cli/azure)アプリを Azure に発行するコマンド。 この記事では、Visual Studio でツールを使用して公開について説明します。 Azure CLI を使用してアプリを発行するを参照してください。[コマンド ライン ツールを使用して Azure に ASP.NET Core アプリを発行](/azure/app-service/app-service-web-get-started-dotnet)します。

プロジェクトを右クリックして**ソリューション エクスプ ローラー**選択**発行**します。 確認します**新規作成**がチェックイン、**発行先を選択**ダイアログ、および選択**発行**します。

![発行対象を選択](publish-to-azure-web-app/_static/pick-publish-target-dialog.png)

次の情報を入力、 **App Service の作成**ダイアログと選択**作成**です。

| アイテム | 説明 |
| ---- | ----------- |
| **アプリ名** | アプリの一意の名前。 |
| **サブスクリプション** | アプリを使用する Azure サブスクリプション。 |
| **リソース グループ** | アプリが属している関連するリソースのグループ。  |
| **ホスティング プラン** | Web アプリの価格プランです。 |

![App service を作成します。](publish-to-azure-web-app/_static/create-app-service-dialog.png)

Visual Studio では、次のタスクを実行します。

* 発行プロファイルを作成します。 発行の設定を格納しています。
* 作成するか、既存の*Azure Web App*で提供された詳細。
* アプリを発行します。
* 公開された web アプリが読み込まれると、ブラウザーを起動します。

アプリは、URL の形式に注意してください *{アプリ名} .azurewebsites.net .net*します。 たとえば、という名前のアプリ`SignalRChattR`が次のような URL `https://signalrchattr.azurewebsites.net`。

HTTP 502.2 エラーが発生する場合は、次を参照してください。 [Azure App Service に ASP.NET Core の展開のプレビュー リリース](xref:host-and-deploy/azure-apps/index)問題を解決します。

## <a name="configure-signalr-web-app"></a>SignalR web アプリを構成します。

Azure Web アプリがありますに公開されている ASP.NET Core SignalR アプリ[ARR アフィニティ](https://en.wikipedia.org/wiki/Application_Request_Routing)を有効にします。 [Websocket](xref:fundamentals/websockets) Websocket トランスポートは関数を許可する、有効にする必要があります。

Azure portal に移動します**アプリ設定**web アプリ。 設定**Websocket**に**で**、ことを確認し、 **ARR アフィニティ**は**で**します。

![Azure portal で azure の Web アプリの設定](publish-to-azure-web-app/_static/azure-web-app-settings.png)

 Websocket と他のトランスポート[App Service プランの](/azure/azure-subscription-service-limits#app-service-limits)します。

## <a name="related-resources"></a>関連資料

* [コマンド ライン ツールを使用して Azure に ASP.NET Core アプリを発行します。](/azure/app-service/app-service-web-get-started-dotnet)
* [Visual Studio を使用した Azure への ASP.NET Core アプリを発行します。](xref:tutorials/publish-to-azure-webapp-using-vs)
* [ホストし、Azure で ASP.NET Core プレビュー アプリを展開します。](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
