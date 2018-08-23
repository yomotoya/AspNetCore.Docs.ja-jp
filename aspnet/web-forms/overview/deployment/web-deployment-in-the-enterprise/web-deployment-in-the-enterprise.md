---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/web-deployment-in-the-enterprise
title: Web、企業内の配置 |Microsoft Docs
author: jrjlee
description: このチュートリアルでは、devel するエンタープライズ規模の web アプリケーションの展開を管理するときに発生する課題の多くを達成する方法について説明しています.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: b8283698-7b82-42a8-8d83-3aeb18ca7fcc
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/web-deployment-in-the-enterprise
msc.type: authoredcontent
ms.openlocfilehash: 735cc5ac37e369e6149c526174c3f74a08db9758
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835790"
---
<a name="web-deployment-in-the-enterprise"></a>企業の web 展開
====================
によって[Jason Lee](https://github.com/jrjlee)

[PDF のダウンロード](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このチュートリアルでは、エンタープライズ規模の web アプリケーションを開発、テスト、ステージング、および運用環境のデプロイを管理するときに発生する課題の多くを達成する方法について説明します。 このチュートリアルには、さまざまな一般的なタスクと手順について説明する概念とタスク指向のコンテンツの組み合わせと共に参照ソリューションが含まれています。
> 
> これらのチュートリアルのイタリア語の翻訳を参照してください。 [ http://www.lucamorelli.it](http://www.lucamorelli.it)します。


## <a name="enterprise-deployment-challenges"></a>エンタープライズ展開の課題

組織には、管理、複雑なエンタープライズ規模のソリューションの配置期待しているときにこれらの課題が発生する多くの場合。

- できる必要がありますをプロジェクトの開発者など、複数の環境に配置またはテスト環境、プラットフォーム、および実稼働サーバーをステージングします。 ソリューションは、環境ごとに異なる構成設定を展開する必要があります。
- シングル ステップまたは自動化されたビルドと展開プロセスの一環として同時に複数の依存プロジェクトをデプロイする必要があります。
- できるようにドライブを展開する自動プロセスでする必要があります。 たとえば、継続的インテグレーション (CI) プロセスを使用して、新しいコードをチェックインするときに、テスト環境に web アプリケーションをデプロイします。
- 開発者は、適切な構成設定やターゲット環境ごとに必要な資格情報を持つ可能性がありますが、展開プロセスを制御し、Visual Studio の外部から展開の変数を設定するをできるようにする必要があります。
- データベースのスキーマに基づくプロジェクトをデプロイし、以降のデプロイに既存のデータを保持する必要があります。
- ユーザー アカウントのデータを展開することがなく、アド ホックにメンバーシップ データベースをデプロイする必要があります。 また、既存のユーザー アカウントのデータを失うことがなく展開されたメンバーシップ データベースのスキーマを更新する必要があります。
- コンテンツをさまざまなターゲット環境に展開するときに、特定のファイルまたはフォルダーを除外する必要があります。

## <a name="overview-of-approach"></a>アプローチの概要

と共にこのシリーズでは、その他のチュートリアルは、このチュートリアルでは、この高度なアプローチを使用して、上記の課題に対応します。

- **カスタムの Microsoft Build Engine (MSBuild) プロジェクト ファイルを使用すると、全体的なビルドおよび配置プロセスを制御できます。**
- これにより、ビルドして、1 つのスクリプト可能な操作の一部として、ソリューション内のすべてのプロジェクトをデプロイすることができます。
- 環境に固有の設定は、単純な環境に固有のプロジェクト ファイルを使用して構成されます。 ソリューション構成を使用して Visual Studio – 中心のアプローチとは対照的と展開のさまざまな環境を構成するプロファイルの発行、このアプローチでは、構成および Visual Studio の外部から、展開プロセスを管理することができます。 これは、サポート技術情報の接続文字列、サービス エンドポイント、サーバーの資格情報、およびその他の配置の変数を展開先環境をしない開発者を進める必要があることを意味します。
- カスタムのプロジェクト ファイルは、チームが Team Foundation Server (TFS) のワークフローの一部としてビルドによって呼び出すことができます。 CI のシナリオのデプロイを自動構成できます。

**インターネット インフォメーション サービス (IIS) の Web 配置ツール (Web 配置) を使用して、パッケージ化し、web アプリケーション プロジェクトをデプロイします。**

- Web Deploy のパッケージ化し、依存関係、構成設定、セキュリティの設定、およびその他すべての要件と、移行先の IIS web サーバーに web アプリケーションのコンテンツをデプロイすることができますフレームワークを提供します。
- カスタム MSBuild プロジェクト ファイル内から全体のパッケージ化と配置のプロセスを制御できます。 接続文字列、サービス エンドポイント、および IIS の出力先の詳細など、web 配置パッケージに付随する構成設定を操作することもできます。
- Web 発行パイプラインと共に、web Deploy では、多数の展開をカスタマイズできる機能拡張ポイントを提供します。 たとえば、web 展開パッケージから不要なファイルとフォルダーを除外する簡単なは。

**VSDBCMD.exe ユーティリティを使用して、デプロイし、データベース スキーマを更新します。**

- VSDBCMD を使用すると、Visual Studio データベース プロジェクトをビルドするときに生成されるデータベース スキーマ ファイル (.dbschema) からデータベースをデプロイできます。 一方、Web 配置に含まれるデータベースの展開機能は、ローカルの SQL Server インスタンスから既存のデータベースを展開する方が適しています。
- データベース プロジェクトをデプロイするため、Visual Studio の機能とは異なり VSDBCMD は既存のターゲット データベースに差分更新を展開することができます。 これにより、データベース スキーマをアップグレードする間、既存のデータを保持することができます。
- カスタム MSBuild プロジェクト ファイル内から VSDBCMD コマンドを実行できます。

## <a name="content-map"></a>コンテンツ マップ

このチュートリアルには、次の 4 つの主な領域に分類されるトピックが含まれています。

これらのトピックが参照ソリューションを導入&#x2014;連絡先マネージャー ソリューション&#x2014;ダウンロードし、ローカル コンピューターに構成する方法について説明します。

- [連絡先マネージャー ソリューション](the-contact-manager-solution.md)
- [連絡先マネージャー ソリューションを設定する](setting-up-the-contact-manager-solution.md)

これらのトピックでは、MSBuild プロジェクト ファイルを導入、記述する方法を作成し、カスタムのプロジェクト ファイルを使用して連絡先マネージャー ソリューションの展開プロセスについて説明します。

- [プロジェクト ファイルについて理解する](understanding-the-project-file.md)
- [ビルド処理について理解する](understanding-the-build-process.md)

これらのトピックには、ビルドとパッケージ化プロセスの動作、ビルド プロセスが Web 発行パイプラインと統合する方法、デプロイのパラメーターを変更する方法および変換先に web パッケージをデプロイする方法を含め、web アプリケーションのデプロイがについて説明します環境:

- [Web アプリケーション プロジェクトのビルドとパッケージ化](building-and-packaging-web-application-projects.md)
- [Web パッケージ展開のパラメーターを構成する](configuring-parameters-for-web-package-deployment.md)
- [Web パッケージを展開する](deploying-web-packages.md)

- [データベース プロジェクトの配置](deploying-database-projects.md)長所と短所のそれぞれの方法と、Visual Studio データベース プロジェクトを展開に使用できるさまざまな手法について説明します。 [作成と展開コマンド ファイルを実行している](creating-and-running-a-deployment-command-file.md)デプロイ ロジックをカプセル化し、1 つの手順として複雑なソリューションをデプロイすることができます簡単なコマンド ファイルを作成する方法について説明します。
- 最後に、 [Web パッケージを手動でインストール](manually-installing-web-packages.md)で IIS に web パッケージをインポートすることを示すことによって、チュートリアルは終了します。

## <a name="key-technologies"></a>主要なテクノロジ

このチュートリアルのトピックでは、主に、これらのテクノロジを使用してビルドと展開の管理します。

- Visual Studio 2010
- MSBuild
- IIS 7.5
- Web 配置 2.0
- VSDBCMD.exe データベースの配置ユーティリティ

## <a name="other-tutorials-in-this-series"></a>このシリーズの他のチュートリアル

これは、エンタープライズ規模の web 展開の 5 つのチュートリアルのシリーズの一部を形成します。 他のチュートリアル シリーズの次に示します。

- [エンタープライズのシナリオで Web アプリケーションを展開する](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md)します。 この概要のコンテンツは、チュートリアル シリーズのコンテキストの背景を提供します。 このチュートリアルのシナリオについて説明し、タスクとチュートリアル シリーズで説明されているが、広範なアプリケーション ライフ サイクル管理 (ALM) のプロセスに適合させる方法を示しています。
- [Web 配置のサーバー環境を構成する](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md)します。 このチュートリアルでは、Web Deployment Agent サービス (リモート エージェント) または Web 配置ハンドラーとリモートのデータベースの配置を使用して、リモート web パッケージ展開を含め、さまざまなデプロイメント シナリオをサポートするために Windows サーバーを構成する方法について説明します。 独自の環境の適切な展開方法の選択について説明して、Web Farm Framework (WFF) を使用して、サーバー ファーム内のすべての web サーバー間でデプロイされた web アプリケーションをレプリケートする方法について説明します。
- [Web 配置の Team Foundation Server を構成する](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md)します。 このチュートリアルでは、CI プロセスの一部として自動化された展開を含め、特定のビルドの展開を手動でトリガーされる、さまざまなデプロイ シナリオをサポートするために TFS を構成する方法について説明します。
- [エンタープライズ Web 配置を高度な](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md)します。 このチュートリアルは、複数の環境のデータベースの展開をカスタマイズ、展開からのファイルとフォルダーの除外、展開プロセス中に web アプリケーションをオフラインすることなどのさまざまな高度な展開タスクを実行する方法を説明します.

> [!div class="step-by-step"]
> [次へ](the-contact-manager-solution.md)
