---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-a-tfs-build-server-for-web-deployment
title: Web 配置のビルド サーバーに、TFS の構成 |Microsoft ドキュメント
author: jrjlee
description: このトピックでは、ビルドし、チーム ビルドとインターネット Informat を使用して、ソリューションの配置を Team Foundation Server (TFS) ビルド サーバーを準備する方法について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: f8400241-4f4b-4bbd-9994-54fb64909e6e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-a-tfs-build-server-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 7b3130ca7d36ffec457e1871fa62c1077b5e3174
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="configuring-a-tfs-build-server-for-web-deployment"></a>Web 配置のビルド サーバーに TFS を構成します。
====================
によって[Jason lee 著](https://github.com/jrjlee)

[PDF をダウンロードします。](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このトピックでは、ビルドし、チーム ビルドと、インターネット インフォメーション サービス (IIS) Web 配置ツール (Web 配置) を使用して、ソリューションの配置を Team Foundation Server (TFS) ビルド サーバーを準備する方法について説明します。


このトピックの Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に関するチュートリアル シリーズの一部を形成します。このチュートリアルの一連のサンプル ソリューションを使用する&#x2014;、 [Contact Manager ソリューション](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;現実的な ASP.NET MVC 3 アプリケーション、Windows Communication も含め、複雑さのレベルを持つ web アプリケーションを表すFoundation (WCF) サービスとデータベース プロジェクト。

説明されている分割プロジェクト ファイル アプローチに基づいて、これらのチュートリアルの中心に配置メソッド[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)、によって制御されるビルド プロセスでは、2 つのプロジェクト ファイル&#x2014;1 つを含む各配置先の環境と環境固有のビルドと配置の設定を含む 1 つに適用される手順をビルドします。 ビルド時に環境固有のプロジェクト ファイルは、ビルドの手順の完全なセットを形成する環境に依存しないプロジェクト ファイルにマージされます。

## <a name="task-overview"></a>タスクの概要

ビルド サーバーをビルドし、ソリューションの配置を準備するには、する必要があります。

- インストールし、TFS ビルド サービスを構成します。
- Visual Studio 2010 をインストールします。
- 任意の製品または .NET Framework または ASP.NET MVC のバージョンと同様に、ソリューションをビルドするために必要なコンポーネントをインストールします。
- Web Deploy 2.0 以降をインストールします。

このトピックでは、これらの手順を実施するか、存在する場合、その他のリソースをポイントする方法を示します。 タスクとこのトピックの「チュートリアル仮定します。

- Windows Server 2008 R2 Service Pack 1 を実行するクリーン サーバー ビルドを開始しています。
- サーバーは、静的 IP アドレスでドメインに参加しています。
- 説明に従って、別のサーバーに TFS アプリケーション層をインストールした[Enterprise Web 展開: シナリオの概要](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)です。

### <a name="who-performs-these-procedures"></a>ユーザーには、これらの手順を実行しますか。

ほとんどの場合、TFS 管理者を担当するビルド サーバーを構成します。 場合によっては、開発者チームは、特定のビルド サーバーの所有権可能性があります。

## <a name="install-and-configure-the-tfs-build-service"></a>インストールし、TFS ビルド サービスの構成

ビルド サーバーを構成するときに、最初のタスクをインストールして、TFS ビルド サービスの構成です。 このプロセスの一環として、する必要があります。

- TFS ビルド サービスをインストールし、サービス アカウントを構成します。 ビルド サービス アカウントの id を使用して、展開を含む、すべてのビルド タスクが実行されます。
- 作成、*ビルド コント ローラー*と 1 つまたは複数*ビルド エージェント*です。 各ビルド コント ローラーは、ビルド エージェントのセットを管理します。 ビルドをキューに配置しときに、ビルド コント ローラーは、利用可能なビルド エージェントをビルド タスクを割り当てます。 TFS の各チーム プロジェクト コレクションは、1 つのビルド コント ローラーにマップされます。
- ビルドの出力のドロップ フォルダーを構成します。 これは、ネットワーク共有です。 いずれかにより、ビルドの web 展開パッケージと同様に、出力、ドロップ フォルダに送信されます。

[Team Foundation ビルドの管理](https://msdn.microsoft.com/library/ms252495.aspx)msdn 章には、これらのタスクを実行するために必要なすべてのリソースが含まれています。

- Team Foundation ビルドの概念的概要については、ビルド サービス、ビルド コント ローラーとビルド エージェント、参照を含む[Team Foundation ビルド システムを理解する](https://msdn.microsoft.com/library/dd793166.aspx)です。
- インストールして、ビルド サービスの構成については、次を参照してください。[ビルド コンピューターの構成](https://msdn.microsoft.com/library/ms181712.aspx)です。
- ビルド コント ローラーを作成する方法については、次を参照してください。[ビルド コント ローラーの作成と操作](https://msdn.microsoft.com/library/ee330987.aspx)です。
- ビルド エージェントを作成する方法については、次を参照してください。[ビルド エージェントの作成と操作](https://msdn.microsoft.com/library/bb399135.aspx)です。
- 作成し、ドロップ フォルダーの構成については、次を参照してください。[ドロップ フォルダーのセットアップ](https://msdn.microsoft.com/library/bb778394.aspx)です。

## <a name="install-required-products-and-components"></a>必要な製品とコンポーネントをインストールします。

ソリューションのビルドをビルド サーバーを有効にするには、任意の製品、コンポーネント、またはソリューションに必要なアセンブリをインストールする必要があります。 Web プラットフォーム コンポーネントをインストールする前に、ビルド サーバーで、Visual Studio 2010 (任意のバージョン) をインストールしてください。 これにより、コア Microsoft Build Engine (MSBuild) のターゲット ファイルと Web 発行パイプライン (WPP) のターゲット ファイルがビルド サービスで使用できること。 Visual Studio インストーラーでは、Web を展開する場合は、ビルド プロセスの一部として web パッケージの展開を計画する必要がありますはインストールも必要があります。

一般的な web プラットフォーム コンポーネントをインストールする最善の方法が使用するが、 [Web Platform Installer](https://go.microsoft.com/?linkid=9805118)です。 これにより、各製品の最新バージョンをインストールしても自動的に検出し、各製品の前提条件をインストールします。 場合、[連絡先のマネージャー](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)ソリューションでは、これらの製品とコンポーネントをインストールする、Web Platform Installer を使用する必要があります。

- **.NET Framework 4.0**. これは、このバージョンの .NET Framework で構築されたアプリケーションの実行に必要です。
- **Web 配置ツール 2.1 以降**です。 これにより、Web Deploy (とその基になる実行可能ファイル、MSDeploy.exe) がサーバーにインストールされます。 このプロセスの一環としてがインストールされ、Web Deployment Agent サービスを開始します。 このサービスでは、リモート コンピューターから web パッケージを展開できます。
- **ASP.NET MVC 3**. これにより、ASP.NET MVC 3 アプリケーションを実行する必要があるアセンブリがインストールされます。

**必要な製品とコンポーネントをインストールするには**

1. Visual Studio 2010 をインストールします。 インストールする機能の選択を求められたらを含める必要があります。

    1. コンパイルする必要があります。 任意のプログラミング言語です。
    2. Visual Web Developer です。 これにより、WPP ターゲットがビルド サーバーに追加されます。

        ![](configuring-a-tfs-build-server-for-web-deployment/_static/image1.png)
2. Visual Studio 2010 のインストールが完了したらをダウンロードしてインストール[Visual Studio 2010 Service Pack 1](https://go.microsoft.com/?linkid=9805133) (かどうかには含まれていない、インストール メディアで)。

    > [!NOTE]
    > Visual Studio 2010 Service Pack 1 では、実行可能ファイルの MSDeploy の検索から MSBuild を妨げる可能性のあるバグを解決します。
3. ダウンロードして起動、 [Web Platform Installer](https://go.microsoft.com/?linkid=9805118)です。
4. 上部にある、 **Web Platform Installer 3.0**ウィンドウで、をクリックして**製品**です。
5. ナビゲーション ウィンドウで、ウィンドウの左側にあるをクリックして**フレームワーク**です。
6. **Microsoft .NET Framework 4** 、.NET Framework がインストールされていない場合、行をクリックして**追加**です。

    > [!NOTE]
    > 既にインストールしている Windows Update から .NET Framework 4.0。 製品またはコンポーネントがインストール済みの場合、Web Platform Installer は、これに置き換えることで、**追加**ボタン テキストを**インストール**です。

    ![](configuring-a-tfs-build-server-for-web-deployment/_static/image2.png)
7. **ASP.NET MVC 3 (Visual Studio 2010)**行で、をクリックして**追加**です。
8. ナビゲーション ウィンドウで **サーバー**です。
9. **Web 配置ツール 2.1**行で、をクリックして**追加**です。
10. **[インストール]**をクリックします。 Web Platform Installer が製品の一覧を表示&#x2014;関連する依存関係のいずれかと共に&#x2014;をインストールして、ライセンス条項に同意するように求められます。
11. ライセンス条項を確認し、条項に同意した場合にをクリックして**同意**です。
12. インストールが完了したらをクリックして**完了**、し、閉じます、 **Web Platform Installer 3.0**ウィンドウです。

> [!NOTE]
> 展開プロセスには、VSDBCMD.exe または SQLCMD.exe などのツールの使用が含まれている場合は、ビルド サーバーにインストールされることを確認する必要があります。 VSDBCMD.exe、Visual Studio ツールは、Team Foundation ビルドをインストールするときに通常、サーバーに追加します。 SQLCMD.exe は、SQL Server ツールです。 SQLCMD.exe のスタンドアロン バージョンをダウンロードすることができます、 [Microsoft SQL Server 2008 R2 Feature Pack](https://go.microsoft.com/?linkid=9805134)ページ。


## <a name="conclusion"></a>まとめ

この時点で、ビルド サーバーは、ビルドと、web アプリケーション プロジェクトの展開を開始する準備です。 次のトピックでは、[展開を作成するビルド定義をサポートしている](creating-a-build-definition-that-supports-deployment.md)、作成して、タイミングを制御するビルド定義を構成する方法と、プロジェクトをビルドおよび配置する方法について説明します。

## <a name="further-reading"></a>関連項目

チーム ビルドを使用して作業の一般的なガイダンスについては、次を参照してください。 [Team Foundation ビルドの管理](https://msdn.microsoft.com/library/ms252495.aspx)です。

> [!div class="step-by-step"]
> [前へ](adding-content-to-source-control.md)
> [次へ](creating-a-build-definition-that-supports-deployment.md)
