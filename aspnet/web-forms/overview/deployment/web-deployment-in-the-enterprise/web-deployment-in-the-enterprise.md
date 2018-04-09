---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/web-deployment-in-the-enterprise
title: Web、企業内の配置 |Microsoft ドキュメント
author: jrjlee
description: このチュートリアルでは、devel にエンタープライズ規模の web アプリケーションの展開を管理するときに発生する課題の多くを満たす方法について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: b8283698-7b82-42a8-8d83-3aeb18ca7fcc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/web-deployment-in-the-enterprise
msc.type: authoredcontent
ms.openlocfilehash: fc463cb689f4f63a12788b80958c9fc8fe20119d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="web-deployment-in-the-enterprise"></a>企業内の web 配置
====================
によって[Jason lee 著](https://github.com/jrjlee)

[PDF をダウンロードします。](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このチュートリアルでは、多数のエンタープライズ規模の web アプリケーションを開発、テスト、ステージング、および運用環境の展開を管理するときに発生する課題に対処する方法について説明します。 このチュートリアルには、さまざまな一般的なタスクと手順について説明する概念とタスク指向のコンテンツの組み合わせと共に参照ソリューションが含まれています。
> 
> これらのチュートリアルのイタリア語の翻訳を参照してください。 [ http://www.lucamorelli.it](http://www.lucamorelli.it)です。


## <a name="enterprise-deployment-challenges"></a>企業の展開に関する問題

組織では、複雑な場合は、エンタープライズ規模のソリューションの展開の管理を検索するときに、これらの課題がしばしば見します。

- する必要がある開発者と同様に、複数の環境にプロジェクトを配置するテスト環境、ステージング プラットフォームでは、および実稼働サーバー。 ソリューションは、それぞれの環境のさまざまな構成設定を展開する必要があります。
- シングル ステップまたは自動ビルドおよび配置プロセスの一部として同時に複数の依存プロジェクトを展開する必要があります。
- 自動化されたプロセスからドライブの展開にでく必要があります。 たとえば、新しいコードをチェックインするときに、テスト環境への web アプリケーションを展開する継続的インテグレーション (CI) プロセスを使用します。
- 開発者は、適切な構成設定またはすべてのターゲット環境に必要な資格情報を持つ可能性の高い展開プロセスを制御し、Visual Studio の外部からの展開の変数を設定することである必要があります。
- データベースのスキーマに基づくプロジェクトを展開し、後続のデプロイに既存のデータを保持する必要があります。
- ユーザー アカウントのデータを配置することがなく、アド ホックごとにメンバーシップ データベースを展開する必要があります。 また、既存のユーザー アカウントのデータを失うことがなく展開済みのメンバーシップ データベースのスキーマを更新する必要があります。
- さまざまなターゲット環境にコンテンツを展開するときに、特定のファイルまたはフォルダーを除外する必要があります。

## <a name="overview-of-approach"></a>アプローチの概要

このチュートリアルでは、と共に、このシリーズの他のチュートリアルでは、この高度なアプローチを使用して、上記の課題に対応します。

- **カスタムの Microsoft Build Engine (MSBuild) プロジェクト ファイルを使用すると、全体的なビルドおよび配置プロセスを制御できます。**
- これにより、ビルドおよびスクリプト可能な 1 つの操作の一環として、ソリューション内のすべてのプロジェクトを配置することができます。
- 環境に固有の設定は、単純な環境に固有のプロジェクト ファイルを使用して構成されます。 ソリューション構成を使用する Visual Studio – 中心のアプローチと対照的に異なる環境での展開を構成するプロファイルの発行し、この方法で構成し、Visual Studio の外部からの展開プロセスを管理できます。 これは、開発者に対して、接続文字列、サービス エンドポイント、サーバーの資格情報、および展開先環境の他の展開変数の知識を進める必要がありますしないことを意味します。
- カスタム プロジェクト ファイルは、Team Foundation Server (TFS) のワークフローの一部としてチームがビルドによって呼び出すことができます。 CI のシナリオの展開の自動構成できます。

**インターネット インフォメーション サービス (IIS) Web 配置ツール (Web 配置) を使用して、パッケージ化し、web アプリケーション プロジェクトを展開します。**

- Web Deploy は、パッケージ化し、の依存関係、設定、セキュリティの設定、およびその他の要件と、移行先の IIS web サーバーに web アプリケーションのコンテンツを配置することができますフレームワークを提供します。
- カスタム MSBuild プロジェクト ファイル内でパッケージ化と配置からプロセス全体を制御できます。 接続文字列、サービス エンドポイント、および変換先の詳細情報を IIS のように、web 配置パッケージに付随する構成設定を操作することもできます。
- Web 発行パイプラインと共に、web Deploy は、多数の展開をカスタマイズすることのできる機能拡張ポイントを提供します。 たとえば、簡単に web 配置パッケージから不要なファイルとフォルダーを除外するのにです。

**VSDBCMD.exe ユーティリティを使用して、展開して、データベース スキーマを更新します。**

- VSDBCMD では、Visual Studio データベース プロジェクトをビルドするときに生成されるデータベース スキーマ ファイル (.dbschema) からデータベースを展開することができます。 これに対し、Web Deploy に含まれるデータベースの展開機能がローカル SQL Server インスタンスから既存のデータベースを展開する方が適しています。
- データベース プロジェクトを展開するため、Visual Studio の機能とは異なり VSDBCMD に差分更新を既存のターゲット データベースに配置することができます。 これにより、データベース スキーマをアップグレードするときに、既存のデータを保持することができます。
- VSDBCMD コマンドは、カスタム MSBuild プロジェクト ファイル内で実行できます。

## <a name="content-map"></a>コンテンツ マップ

このチュートリアルには、次の 4 つの主な領域に分類されるトピックが含まれています。

これらのトピックを紹介参照ソリューション&#x2014;Contact Manager ソリューション&#x2014;し、それをダウンロードしてローカル コンピューターに構成する方法について説明。

- [連絡先マネージャー ソリューション](the-contact-manager-solution.md)
- [連絡先マネージャー ソリューションを設定する](setting-up-the-contact-manager-solution.md)

これらのトピックでは、MSBuild プロジェクト ファイルを導入、できますの作成方法とカスタム プロジェクト ファイルを使用して、連絡先のマネージャー ソリューションの配置プロセスを順を追ってを記述します。

- [プロジェクト ファイルについて理解する](understanding-the-project-file.md)
- [ビルド処理について理解する](understanding-the-build-process.md)

これらのトピックの説明、ビルドとパッケージ化プロセスの動作、ビルド プロセスが、Web 発行パイプラインと統合する方法、デプロイ パラメーターを変更する方法および変換先に web パッケージを展開する方法を含む web アプリケーションの配置環境:

- [Web アプリケーション プロジェクトのビルドとパッケージ化](building-and-packaging-web-application-projects.md)
- [Web パッケージ展開のパラメーターを構成する](configuring-parameters-for-web-package-deployment.md)
- [Web パッケージを展開する](deploying-web-packages.md)

- [データベース プロジェクトの配置](deploying-database-projects.md)長所と短所をそれぞれの方法と、Visual Studio データベース プロジェクトを展開に使用できるさまざまな手法について説明します。 [作成と展開コマンド ファイルを実行している](creating-and-running-a-deployment-command-file.md)デプロイ ロジックをカプセル化し、1 つの手順として複雑なソリューションを展開することができますを単純なコマンド ファイルを作成する方法について説明します。
- 最後に、 [Web パッケージを手動でインストールする](manually-installing-web-packages.md)で IIS に web パッケージをインポートすることを示すことでチュートリアルは終わりです。

## <a name="key-technologies"></a>主要なテクノロジ

このチュートリアルのトピックでは、主に、これらのテクノロジを使用してビルドと展開の管理します。

- Visual Studio 2010
- MSBuild
- IIS 7.5
- Web 配置 2.0
- VSDBCMD.exe データベース配置ユーティリティ

## <a name="other-tutorials-in-this-series"></a>このシリーズの他のチュートリアル

これは、エンタープライズ規模の web 配置で 5 つのチュートリアルの一連の一部を形成します。 系列の他のチュートリアルは、次に示します。

- [エンタープライズのシナリオで Web アプリケーションを展開する](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md)です。 この入門的な内容は、一連のチュートリアルのコンテキストの背景を提供します。 チュートリアルのシナリオについて説明し、どのタスクと、系列全体で説明したチュートリアルに適合する広範なアプリケーション ライフ サイクル管理 (ALM) プロセスを示しています。
- [Web 配置のサーバー環境を構成する](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md)です。 このチュートリアルでは、Web Deployment Agent サービス (リモート エージェント) または Web デプロイのハンドラーおよびリモートのデータベースの配置を使用してリモートの web パッケージの配置を含むさまざまな展開シナリオをサポートする Windows サーバーを構成する方法について説明します。 ご使用の環境の適切な展開方法を選択するためのガイダンスを提供し、Web Farm Framework (WFF) を使用して、サーバー ファーム内のすべての web サーバー経由で展開された web アプリケーションをレプリケートする方法について説明します。
- [Web 配置を Team Foundation Server を構成する](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md)です。 このチュートリアルでは、CI のプロセスの一部としての自動展開の特定のビルドの展開を手動でトリガーなど、さまざまな配置シナリオをサポートするために TFS を構成する方法について説明します。
- [企業の Web 配置を高度な](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md)します。 このチュートリアルは複数の環境のデータベースの配置をカスタマイズする、ファイルとフォルダーの展開から除外および展開プロセス中に web アプリケーションをオフラインにするなどのさまざまな高度な展開タスクを実行する方法を説明します.

> [!div class="step-by-step"]
> [次へ](the-contact-manager-solution.md)
