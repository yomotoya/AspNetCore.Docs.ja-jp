---
title: ASP.NET Core および Azure を使用した DevOps |ツールとダウンロード
author: CamSoper
description: Azure でホストされる ASP.NET Core アプリの DevOps パイプラインの構築に関するエンドツーエンドのガイダンスを提供するガイド。
ms.author: casoper
ms.date: 08/07/2018
uid: azure/devops/tools-and-downloads
ms.openlocfilehash: 5529068b83db475315784571fbf4151d7ecd0d5d
ms.sourcegitcommit: 57eccdea7d89a62989272f71aad655465f1c600a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/10/2018
ms.locfileid: "44340161"
---
# <a name="tools-and-downloads"></a>ツールとダウンロード

Azure では、いくつかのインターフェイスのプロビジョニングおよびなどのリソースを管理、 [Azure portal](https://portal.azure.com)、 [Azure CLI](https://docs.microsoft.com/cli/azure/)、 [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)、 [Azure クラウドシェル](https://shell.azure.com/bash)、および Visual Studio。 このガイドでは、必要最低限のアプローチを採用し、Azure Cloud Shell を減らすために必要な手順を実行可能な限りを使用します。 ただし、一部の Azure portal を使用する必要があります。

## <a name="prerequisites"></a>必須コンポーネント

次のサブスクリプションが必要です。

* Azure&mdash;アカウントを持っていない場合[無料試用版を入手](https://azure.microsoft.com/free/)します。
* Azure DevOps サービス&mdash;第 4 章で、DevOps の Azure サブスクリプションと組織を作成します。
* GitHub&mdash;アカウントを持っていない場合[無料でサインアップ](https://github.com/join)します。

次のツールが必要です。

* [Git](https://git-scm.com/downloads) &mdash;このガイドの Git の基本について理解をお勧めします。 レビュー、 [Git に関するドキュメント](https://git-scm.com/doc)、特に[git リモート](https://git-scm.com/docs/git-remote)と[git プッシュ](https://git-scm.com/docs/git-push)します。
* [.NET core SDK](https://www.microsoft.com/net/download/) &mdash; 2.1.300 のバージョンをビルドして、サンプル アプリの実行以降が必要です。 Visual Studio がインストールされている場合、 **.NET Core クロス プラットフォーム開発**ワークロードでは、.NET Core SDK が既にインストールされています。

    .NET Core SDK のインストールを確認します。 コマンド シェルを開き、次のコマンドを実行します。

    ```console
    dotnet --version
    ```

## <a name="recommended-tools-windows-only"></a>推奨されるツール (Windows のみ)

* [Visual Studio](https://www.visualstudio.com/)堅牢な Azure ツール説明 GUI このガイドで説明されている機能のほとんどの。 無料の Visual Studio Community Edition を含む、Visual Studio の任意のエディションは機能します。 チュートリアルは、Visual Studio の有無にかかわらず、開発、デプロイ、DevOps のデモンストレーションに書き込まれます。

  Visual Studio が、次のことを確認します。[ワークロード](https://docs.microsoft.com/visualstudio/install/modify-visual-studio)インストール。

  * ASP.NET と Web 開発
  * Azure の開発
  * .NET Core クロスプラットフォームの開発
