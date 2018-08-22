---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
title: エンタープライズ Web 配置を高度な |Microsoft Docs
author: jrjlee
description: このチュートリアルでは、必要なまたはエンタープライズ展開シナリオの多くの望ましいさまざまなタスクを実行する方法を説明します。 イタリア語の translati をしています.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 7dcaba80-f2ec-4db3-ad98-daadc3afdb49
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 34f1bf3bc2c37afc66f458a60a29fe5ce8f6c018
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823799"
---
<a name="advanced-enterprise-web-deployment"></a>高度なエンタープライズ Web 展開
====================
によって[Jason Lee](https://github.com/jrjlee)

[PDF のダウンロード](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このチュートリアルでは、必要なまたはエンタープライズ展開シナリオの多くの望ましいさまざまなタスクを実行する方法を説明します。
> 
> これらのチュートリアルのイタリア語の翻訳を参照してください。 [ http://www.lucamorelli.it](http://www.lucamorelli.it)します。


これが Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に基づいているチュートリアルのシリーズの一部を形成します。このチュートリアル シリーズは、サンプル ソリューションを使用して&#x2014;、 [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)ソリューション&#x2014;現実的なレベルの ASP.NET MVC 3 アプリケーション、Windows の通信など、複雑な web アプリケーションを表すFoundation (WCF) サービスとデータベース プロジェクト。

これらのチュートリアルの中核の展開方法が分割のプロジェクト ファイルの方法で説明されているに基づいて[ビルド プロセスを理解する](../web-deployment-in-the-enterprise/understanding-the-build-process.md)、によって制御されるビルド プロセスでは、2 つのプロジェクト ファイル&#x2014;1 つを格納しています。すべての変換先の環境と環境固有のビルドと配置の設定を含む 1 つに適用される手順をビルドします。 ビルド時に、環境固有のプロジェクト ファイルは、ビルド手順の完全なセットを形成する環境に依存しないプロジェクト ファイルにマージされます。

## <a name="scenario-overview"></a>シナリオの概要

これらのチュートリアルの高度なシナリオについては、「[エンタープライズ Web 展開: シナリオの概要](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)します。 このトピックでは、このチュートリアルを開始する前に確認することをお勧めします。

## <a name="how-to-use-this-tutorial"></a>このチュートリアルを使用する方法

- このチュートリアルでは、トピックの各は自己完結型と、特定の課題やエンタープライズ展開シナリオで発生する問題に対処します。 特定の順序でこれらのトピックを使用する必要はありません。 ただし、このチュートリアルでは、高度なタスクについて説明します。 そのため、する必要がありますについて理解を深める概念および手法を[、企業の Web 展開](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md)このコンテンツからで最もメリットを得るためにチュートリアルに含まれます。
- このチュートリアルには、これらのトピックが含まれています。
- ["What If"展開を実行する](performing-a-what-if-deployment.md)します。 多くのシナリオでは、実際に変更を加える前に、ターゲット環境や、既存のコンテンツの提案された展開の影響を判断します。 このトピックでは、実際に変更を加えずをターゲット環境にコンテンツを展開する必要があるかのようにログ ファイルとデータベースの更新スクリプトを生成する"what-if"展開を実行する方法について説明します。 これらのリソースを分析することは、ライブの展開前に潜在的な問題を発見するのに役立ちます。
- [複数の環境のデータベースの展開をカスタマイズ](customizing-database-deployments-for-multiple-environments.md)します。 複数の変換先にデータベース プロジェクトを展開するときは、各ターゲット環境の配置プロパティをカスタマイズするとする多くの場合。 たとえば、テスト環境では通常再作成するデプロイごとに、データベース ステージングまたは運用環境では多く可能性が高く、データを保持するために増分更新を行うには。 このトピックでは、各ターゲット環境の環境に固有の展開構成 (.sqldeployment) ファイルを作成してのデプロイ ロジックにこれらのプロパティの変更を組み込む方法について説明します。
- データベース ロールのメンバーシップをテスト環境に展開します。 デプロイごとにデータベースを再作成するときに&#x2014;など、継続的インテグレーション (CI) の一部としてビルドおよびテスト環境へのデプロイ&#x2014;通常たびにデータベース ロールのメンバーシップを構成する必要があります。 たとえば、web アプリケーションに関連付けられているアプリケーション プール id へのアクセス許可を付与する、通常必要があります。 このトピックでは、デプロイ ロジックを配置後の SQL スクリプトを追加することで、このプロセスを自動化する方法について説明します。
- [エンタープライズ環境にメンバーシップ データベースを展開する](deploying-membership-databases-to-enterprise-environments.md)します。 ASP.NET メンバーシップ データベースには、展開プロセスが複雑になるさまざまな特性があります。 たとえば、スキーマのみのデプロイメントは操作不可状態のデータベースのままにします。 ほとんどのシナリオでは、各送信先の環境で直接メンバーシップ データベースを作成することをお勧めします。 ただし場合、メンバーシップ データベースを配置するがある、このトピックについて固有の課題に対応する方法のいくつか説明します。
- [ファイルとフォルダーを展開から除外](excluding-files-and-folders-from-deployment.md)します。 一部のシナリオでは、特定の送信先の環境に web パッケージの内容を調整します。 たとえば、クライアント側のデバッグ サポートしますが、縮小されたバージョンのライブラリを使用して、ステージング環境または運用環境にデプロイするときに、テスト環境に展開するときに JavaScript ライブラリの完全なバージョンを追加する可能性があります。 このトピックでは、パッケージの作成プロセスからな特定のファイルとフォルダーを除外する方法について説明します。
- [Web アプリケーションをオフラインに Web デプロイ](taking-web-applications-offline-with-web-deploy.md)します。 ステージング環境または運用環境にソリューションをデプロイするときは、展開プロセスの実行中の web アプリケーションをオフラインを実行するとする多くの場合。 このトピックでは、追加する方法について説明します、*アプリ\_offline.htm* web アプリケーション、展開プロセスの開始時にファイルを開き、最後に削除します。 中に、*アプリ\_offline.htm*ファイルを配置、web アプリケーションを参照するすべてのユーザーに自動的にリダイレクト、*アプリ\_offline.htm*ファイル。
- [MSBuild から Windows PowerShell スクリプトを実行している](running-windows-powershell-scripts-from-msbuild-project-files.md)します。 多くの展開シナリオでは、レジストリへのカスタム イベント ソースの追加や、SQL Server インスタンス間のレプリケーションを構成するより複雑な配置後アクションが必要です。 これらのアクションは多くの場合、Windows PowerShell スクリプトによって実行されます。 このトピックでは、Microsoft Build Engine (MSBuild) プロジェクト ファイルからビルドおよび配置プロセスの一部として Windows PowerShell スクリプトを実行する方法について説明します。
- [パッケージ化プロセスのトラブルシューティング](troubleshooting-the-packaging-process.md)します。 Web 発行パイプライン (WPP) という名前の MSBuild プロパティを定義する**EnablePackageProcessLoggingAndAssert** web アプリケーション プロジェクトのパッケージ化プロセスに関する詳細な情報の生成を行えます。 このトピックでは、プロパティの内容とその使用方法について説明します。

## <a name="key-technologies"></a>主要なテクノロジ

このチュートリアルでは、これらの製品やテクノロジを使用して、自動化されたビルドと web 配置をサポートする方法について説明します。

- Visual Studio 2010 および Team Foundation Server (TFS) 2010
- MSBuild と TFS チーム ビルド
- インターネット インフォメーション サービス (IIS) 7.5
- IIS Web 配置ツール (Web 配置) 2.1
- VSDBCMD.exe データベースの配置ユーティリティ

## <a name="other-tutorials-in-this-series"></a>このシリーズの他のチュートリアル

これは、エンタープライズ規模の web 展開の 5 つのチュートリアルのシリーズの一部を形成します。 その他のチュートリアル シリーズの次に示します。

- [エンタープライズのシナリオで Web アプリケーションを展開する](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md)します。 この概要のコンテンツは、チュートリアル シリーズのコンテキストの背景を提供します。 このチュートリアルのシナリオについて説明し、タスクとチュートリアル シリーズで説明されているが、広範なアプリケーション ライフ サイクル管理 (ALM) のプロセスに適合させる方法を示しています。
- [Web、企業内の配置](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md)します。 このチュートリアルでは、MSBuild プロジェクト ファイルの概念、WPP、Web デプロイ、およびその他の関連テクノロジを提供します。 これは、複雑な展開プロセスを管理するこれらのツールをまとめて使用する方法について説明します。
- [Web 配置のサーバー環境を構成する](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md)します。 このチュートリアルでは、Web Deployment Agent サービス (リモート エージェント) または Web 配置ハンドラーとリモートのデータベースの配置を使用して、リモート web パッケージ展開を含め、さまざまなデプロイメント シナリオをサポートするために Windows サーバーを構成する方法について説明します。 独自の環境の適切な展開方法の選択について説明して、Web Farm Framework (WFF) を使用して、サーバー ファーム内のすべての web サーバー間でデプロイされた web アプリケーションをレプリケートする方法について説明します。
- [Web 配置の Team Foundation Server を構成する](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md)します。 このチュートリアルでは、CI プロセスの一部として自動化された展開を含め、特定のビルドの展開を手動でトリガーされる、さまざまなデプロイ シナリオをサポートするために TFS を構成する方法について説明します。

> [!div class="step-by-step"]
> [次へ](performing-a-what-if-deployment.md)
