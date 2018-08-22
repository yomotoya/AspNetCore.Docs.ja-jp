---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-a-tfs-build-server-for-web-deployment
title: Web 配置のビルド サーバーを TFS の構成 |Microsoft Docs
author: jrjlee
description: このトピックでは、チーム ビルドとインターネットの情報についてを使用して、ソリューション構築、デプロイする Team Foundation Server (TFS) ビルド サーバーを準備する方法について説明しています.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: f8400241-4f4b-4bbd-9994-54fb64909e6e
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-a-tfs-build-server-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 534a0edf5c26dd2daec3c44e41410f8c5de96c15
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834747"
---
<a name="configuring-a-tfs-build-server-for-web-deployment"></a>Web 配置の TFS ビルド サーバーを構成します。
====================
によって[Jason Lee](https://github.com/jrjlee)

[PDF のダウンロード](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このトピックでは、チーム ビルドとインターネット インフォメーション サービス (IIS) の Web 配置ツール (Web 配置) を使用して、ソリューション構築、デプロイする Team Foundation Server (TFS) ビルド サーバーを準備する方法について説明します。


このトピックでは、一連の Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に基づいているチュートリアルの一部を形成します。このチュートリアル シリーズは、サンプル ソリューションを使用して&#x2014;、[連絡先マネージャー ソリューション](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;現実的なレベルの ASP.NET MVC 3 アプリケーション、Windows の通信など、複雑な web アプリケーションを表すFoundation (WCF) サービスとデータベース プロジェクト。

これらのチュートリアルの中核の展開方法が分割のプロジェクト ファイルの方法で説明されているに基づいて[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)、によって制御されるビルド プロセスでは、2 つのプロジェクト ファイル&#x2014;1 つを格納しています。すべての変換先の環境と環境固有のビルドと配置の設定を含む 1 つに適用される手順をビルドします。 ビルド時に、環境固有のプロジェクト ファイルは、ビルド手順の完全なセットを形成する環境に依存しないプロジェクト ファイルにマージされます。

## <a name="task-overview"></a>タスクの概要

ソリューションをビルドおよびデプロイのビルド サーバーを準備するには、する必要があります。

- インストールし、TFS ビルド サービスを構成します。
- Visual Studio 2010 をインストールします。
- すべての製品または .NET Framework、ASP.NET MVC のバージョンと同様に、ソリューションを構築するために必要なコンポーネントをインストールします。
- Web Deploy 2.0 以降をインストールします。

このトピックでは、これらの手順を実施するか、存在する場合、その他のリソースをポイントする方法を説明します。 タスクとこのトピックの「チュートリアル仮定します。

- Windows Server 2008 R2 Service Pack 1 を実行しているサーバーのクリーン ビルドを開始します。
- サーバーは、静的 IP アドレスを持つドメインに参加しています。
- 」の説明に従って、別のサーバーを TFS アプリケーション層をインストールしたら[エンタープライズ Web 展開: シナリオの概要](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)します。

### <a name="who-performs-these-procedures"></a>これらの手順を実行しますか。

ほとんどの場合、TFS 管理者は、ビルド サーバーを構成する責任者になります。 場合によっては、開発者チームは特定のビルド サーバーの所有権をかかる場合があります。

## <a name="install-and-configure-the-tfs-build-service"></a>インストールして、TFS ビルド サービスの構成

ビルド サーバーを構成するときに、最初のタスクは、インストールして、TFS ビルド サービスの構成です。 このプロセスの一環として、する必要があります。

- TFS ビルド サービスをインストールし、サービス アカウントを構成します。 ビルド サービス アカウントの id を使用して、展開を含め、すべてのビルド タスクが実行されます。
- 作成、*ビルド コント ローラー*と 1 つ以上*ビルド エージェント*します。 各ビルド コント ローラーは、ビルド エージェントのセットを管理します。 ビルドをキューに、ビルド コント ローラーには、使用可能なビルド エージェントにビルド タスクが割り当てられます。 TFS の各チーム プロジェクト コレクションは、1 つのビルド コント ローラーにマップされます。
- ビルド出力のドロップ フォルダーを構成します。 これは、ネットワーク共有です。 いずれかにより、ビルドの出力は、web デプロイ パッケージと同様に、ドロップ フォルダーに送信されます。

[Team Foundation ビルドを管理する](https://msdn.microsoft.com/library/ms252495.aspx)MSDN に関する章には、これらのタスクを実行するために必要なすべてのリソースが含まれています。

- Team Foundation ビルドの概念的概要については、ビルド サービス、ビルド コント ローラーとビルド エージェント、参照を含む[Team Foundation ビルド システムについて](https://msdn.microsoft.com/library/dd793166.aspx)します。
- インストールして、ビルド サービスを構成するのには、次を参照してください。[ビルド コンピューターの構成](https://msdn.microsoft.com/library/ms181712.aspx)します。
- ビルド コント ローラーを作成する方法の詳細については、次を参照してください。[ビルド コント ローラーの作成と操作](https://msdn.microsoft.com/library/ee330987.aspx)します。
- ビルド エージェントを作成する方法の詳細については、次を参照してください。[ビルド エージェントの作成と操作](https://msdn.microsoft.com/library/bb399135.aspx)します。
- 作成して、ドロップ フォルダーを構成するのには、次を参照してください。[ドロップ フォルダーのセットアップ](https://msdn.microsoft.com/library/bb778394.aspx)します。

## <a name="install-required-products-and-components"></a>必要な製品とコンポーネントをインストールします。

ソリューションを構築するビルド サーバーを有効にするには、任意の製品、コンポーネント、またはソリューションに必要なアセンブリをインストールする必要があります。 Web プラットフォーム コンポーネントをインストールする前に、ビルド サーバーで Visual Studio 2010 (任意のバージョン) をインストールする必要があります。 これにより、Microsoft Build Engine (MSBuild) ターゲットのコア ファイルと Web 発行パイプライン (WPP) のターゲット ファイル、ビルド サービスを利用できるようにします。 Visual Studio インストーラーは、Web 展開、ビルド プロセスの一部として web パッケージを展開する予定の場合に必要とするなるをインストールする必要があります。

一般的な web プラットフォーム コンポーネントをインストールする最善の方法が使用するには、 [Web Platform Installer](https://go.microsoft.com/?linkid=9805118)します。 これにより、各製品の最新バージョンをインストールしていることとも自動的に検出し、各製品の前提条件がインストールされます。 場合、 [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)ソリューションでは、これらの製品とコンポーネントをインストール、Web Platform Installer を使用する必要があります。

- **.NET framework 4.0**します。 これは、このバージョンの .NET Framework で構築されたアプリケーションの実行に必要です。
- **Web 配置ツール 2.1 以降**します。 これにより、Web Deploy (とその基になる実行可能ファイル、MSDeploy.exe) がサーバーにインストールされます。 このプロセスの一環として、インストールされ、Web Deployment Agent サービスを開始します。 このサービスでは、リモート コンピューターから web パッケージを配置することができます。
- **ASP.NET MVC 3**します。 これにより、ASP.NET MVC 3 アプリケーションを実行する必要があるアセンブリがインストールされます。

**必要な製品とコンポーネントをインストールするには**

1. Visual Studio 2010 をインストールします。 インストールする機能の選択を求められたらを含める必要があります。

    1. コンパイルする必要があるプログラミング言語です。
    2. Visual Web Developer の場合。 これにより、WPP ターゲットがビルド サーバーに追加されます。

        ![](configuring-a-tfs-build-server-for-web-deployment/_static/image1.png)
2. Visual Studio 2010 のインストールが完了したら、ダウンロードしてインストール[Visual Studio 2010 Service Pack 1](https://go.microsoft.com/?linkid=9805133) (かどうかにはまだ含まれていない、インストール メディアで)。

    > [!NOTE]
    > Visual Studio 2010 Service Pack 1 では、実行可能ファイルの MSDeploy の検索から MSBuild を妨げる可能性のあるバグを解決します。
3. ダウンロードして起動、 [Web Platform Installer](https://go.microsoft.com/?linkid=9805118)します。
4. 上部にある、 **Web Platform Installer 3.0**ウィンドウで、をクリックして**製品**します。
5. ナビゲーション ウィンドウで、ウィンドウの左側にある クリックして**フレームワーク**します。
6. **Microsoft .NET Framework 4** 、.NET Framework が既にインストールされていない場合、行をクリックして**追加**します。

    > [!NOTE]
    > 既にインストールしている Windows Update を通じて、.NET Framework 4.0。 製品またはコンポーネントが既にインストールされている場合、Web Platform Installer 示されます置き換えることで、**追加**ボタン テキストを含む**インストール済み**します。

    ![](configuring-a-tfs-build-server-for-web-deployment/_static/image2.png)
7. **ASP.NET MVC 3 (Visual Studio 2010)** 行で、をクリックして**追加**します。
8. ナビゲーション ウィンドウで、 **Server**します。
9. **Web 配置ツール 2.1**行で、をクリックして**追加**します。
10. **[インストール]** をクリックします。 Web Platform Installer が製品の一覧を表示&#x2014;と関連する依存関係&#x2014;をインストールし、ライセンス条項に同意するように求められます。
11. ライセンス条項を確認し、条項に同意する場合にクリックします**同意**します。
12. インストールが完了したら、クリックして**完了**、閉じて、 **Web Platform Installer 3.0**ウィンドウ。

> [!NOTE]
> 展開プロセスには、VSDBCMD.exe または SQLCMD.exe などのツールの使用が含まれている場合は、ビルド サーバーでこれらがインストールされていることを確認する必要があります。 VSDBCMD.exe Visual Studio ツールは、Team Foundation ビルドをインストールするときに通常、サーバーに追加します。 SQLCMD.exe は、SQL Server ツールです。 SQLCMD.exe からのスタンドアロン バージョンをダウンロードすることができます、 [Microsoft SQL Server 2008 R2 Feature Pack](https://go.microsoft.com/?linkid=9805134)ページ。


## <a name="conclusion"></a>まとめ

この時点では、ビルド サーバーが構築し、web アプリケーション プロジェクトの配置を開始する準備が。 次のトピックでは、[配置を作成する、ビルド定義をサポートしています](creating-a-build-definition-that-supports-deployment.md)を作成し、タイミングを制御するビルド定義を構成する方法と、プロジェクトをビルドおよび配置する方法について説明します。

## <a name="further-reading"></a>関連項目

チーム ビルドを使用して作業の一般的なガイダンスについては、次を参照してください。 [Team Foundation ビルドを管理する](https://msdn.microsoft.com/library/ms252495.aspx)します。

> [!div class="step-by-step"]
> [前へ](adding-content-to-source-control.md)
> [次へ](creating-a-build-definition-that-supports-deployment.md)
