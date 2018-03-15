---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
title: "企業の Web 配置の詳細 |Microsoft ドキュメント"
author: jrjlee
description: "このチュートリアルでは、必要なまたは多数のエンタープライズ展開シナリオに適しているさまざまなタスクを実行する方法を示します。 イタリア語 translati をしています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 7dcaba80-f2ec-4db3-ad98-daadc3afdb49
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: c3cb7f63cf7c0246a0c4da6038a65a6ac43a7b59
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2018
---
<a name="advanced-enterprise-web-deployment"></a>高度なエンタープライズ Web 配置
====================
によって[Jason lee 著](https://github.com/jrjlee)

[PDF をダウンロードします。](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このチュートリアルでは、必要なまたは多数のエンタープライズ展開シナリオに適しているさまざまなタスクを実行する方法を示します。
> 
> これらのチュートリアルのイタリア語の翻訳を参照してください。 [ http://www.lucamorelli.it](http://www.lucamorelli.it)です。


これが Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に関するチュートリアル シリーズの一部を形成します。このチュートリアルのシリーズを使用してサンプル ソリューション & #x 2014;、[連絡先のマネージャー](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)複雑さ、ASP.NET MVC 3 アプリケーション、Windows などの現実的なレベルで web アプリケーションを表すためには、ソリューション & #x 2014;Communication Foundation (WCF) サービスとデータベース プロジェクト。

説明されている分割プロジェクト ファイル アプローチに基づいて、これらのチュートリアルの中心に配置メソッド[ビルド プロセスの理解](../web-deployment-in-the-enterprise/understanding-the-build-process.md)、によって制御されるビルド プロセスで 2 つのプロジェクト ファイル & #x 2014; 1 つを含む各配置先の環境と環境固有のビルドと配置の設定を含む 1 つに適用される手順をビルドします。 ビルド時に環境固有のプロジェクト ファイルは、ビルドの手順の完全なセットを形成する環境に依存しないプロジェクト ファイルにマージされます。

## <a name="scenario-overview"></a>シナリオの概要

これらのチュートリアルの高度なシナリオについては、「 [Enterprise Web 展開: シナリオの概要](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)です。 このチュートリアルを始める前に、このトピックの内容を確認することをお勧めします。

## <a name="how-to-use-this-tutorial"></a>このチュートリアルを使用する方法

- このチュートリアルのトピックの各自己完結型は、エンタープライズ展開シナリオで発生する問題の特定の課題に対処します。 特定の順序でこれらのトピックを使用する必要はありません。 ただし、このチュートリアルでは、高度なタスクについて説明します。 ような場合は理解しておくの概念と手法を[、企業内の Web 配置](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md)最大限に活用このコンテンツを取得するためにチュートリアルの内容はします。
- このチュートリアルには、これらのトピックが含まれています。
- ["What If"展開を実行する](performing-a-what-if-deployment.md)です。 多数のシナリオでは、実際に変更を加える前に、ターゲット環境または、既存のコンテンツの提案された展開の影響を判断します。 このトピックでは、実際に変更を加えずをターゲット環境にコンテンツを展開する必要があるかのようにログ ファイルおよびデータベース更新スクリプトを生成する"what-if"展開を実行する方法について説明します。 これらのリソースを分析すると、ライブの展開前に潜在的な問題を発見することができます。
- [複数の環境のデータベースの配置をカスタマイズする](customizing-database-deployments-for-multiple-environments.md)です。 複数の変換先にデータベース プロジェクトを展開するときを多くの場合を各ターゲット環境の配置プロパティをカスタマイズします。 たとえば、テスト環境では通常を再作成するそれぞれの配置上のデータベース ステージングまたは運用環境で、データを保持するために増分更新を行うよりもずっと可能性の高いでしょうがします。 このトピックでは、それぞれのターゲット環境、環境固有の配置の構成 (.sqldeployment) ファイルを作成することでのデプロイ ロジックにこれらのプロパティの変更を組み込む方法について説明します。
- データベース ロール メンバーシップをテスト環境に展開します。 すべての配置 & #x 2014。 たとえば、継続的インテグレーション (CI) ビルドの一部としてデータベースを再作成およびテスト環境 & #x 2014; に展開するときは、通常にたびにデータベース ロール メンバーシップを構成する必要があります。 たとえば、web アプリケーションに関連付けられているアプリケーション プール id にアクセス許可を与える、通常必要があります。 このトピックでは、デプロイ ロジックを配置後 SQL スクリプトを追加することで、このプロセスを自動化する方法について説明します。
- [エンタープライズ環境にメンバーシップ データベースを配置する](deploying-membership-databases-to-enterprise-environments.md)です。 ASP.NET メンバーシップ データベースには、展開プロセスが複雑になるさまざまな特性があります。 たとえば、スキーマのみの展開は非運用状態で、データベースのままにします。 ほとんどのシナリオでは、各配置先の環境で直接メンバーシップ データベースを作成することをお勧めします。 ただし、メンバーシップ データベースを配置するが、このトピックの内容について固有の課題に対応する方法のいくつか説明します。
- [ファイルとフォルダーの展開から除外](excluding-files-and-folders-from-deployment.md)です。 一部のシナリオでは、特定の展開先環境は、web パッケージの内容を調整します。 たとえば、クライアント側のデバッグはサポートが縮小されたバージョンのライブラリを使用して、ステージング環境または運用環境に展開するときに、テスト環境に展開するときに、JavaScript ライブラリの完全バージョンを含めることができます。 このトピックでは、パッケージ作成処理からな特定のファイルとフォルダーを除外する方法について説明します。
- [Web アプリケーションをオフラインでの web 展開](taking-web-applications-offline-with-web-deploy.md)です。 ステージングまたは運用環境にソリューションを配置するときに多くの場合、展開プロセスの実行中、web アプリケーションをオフラインにします。 このトピックでは、追加する方法について説明します、*アプリ\_offline.htm*展開プロセスの開始時に、web アプリケーションへのファイルし、最後に削除します。 中に、*アプリ\_offline.htm*インプレースのファイルは、web アプリケーションを参照するすべてのユーザーに自動的にリダイレクト、*アプリ\_offline.htm*ファイル。
- [MSBuild から Windows PowerShell スクリプトを実行している](running-windows-powershell-scripts-from-msbuild-project-files.md)です。 多くの展開シナリオでは、カスタム イベントのソースをレジストリに追加するか、SQL Server インスタンス間でレプリケーションを構成するのと同様より複雑な配置後アクションが必要です。 これらのアクションは多くの場合、Windows PowerShell スクリプトによって実行されます。 このトピックでは、Microsoft Build Engine (MSBuild) プロジェクト ファイルからビルドおよび配置プロセスの一部として Windows PowerShell スクリプトを実行する方法について説明します。
- [パッケージ化プロセスのトラブルシューティング](troubleshooting-the-packaging-process.md)です。 Web 発行パイプライン (WPP) MSBuild プロパティという名前を定義する**EnablePackageProcessLoggingAndAssert** web アプリケーション プロジェクトのパッケージ化プロセスに関する詳細な情報の生成を行えます。 このトピックでは、プロパティの動作とその使用方法について説明します。

## <a name="key-technologies"></a>主要なテクノロジ

このチュートリアルは、自動ビルドと web 配置をサポートするためにこれらの製品およびテクノロジを使用する方法について説明します。

- Visual Studio 2010 および Team Foundation Server (TFS) 2010
- MSBuild と TFS チームのビルド
- インターネット インフォメーション サービス (IIS) 7.5
- IIS Web 配置ツール (Web 配置) 2.1
- VSDBCMD.exe データベース配置ユーティリティ

## <a name="other-tutorials-in-this-series"></a>このシリーズの他のチュートリアル

これは、エンタープライズ規模の web 配置で 5 つのチュートリアルの一連の一部を形成します。 これらは、系列内の他のチュートリアルです。

- [エンタープライズのシナリオで Web アプリケーションを展開する](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md)です。 この入門的な内容は、一連のチュートリアルのコンテキストの背景を提供します。 チュートリアルのシナリオについて説明し、どのタスクと、系列全体で説明したチュートリアルに適合する広範なアプリケーション ライフ サイクル管理 (ALM) プロセスを示しています。
- [Web、企業内の配置](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md)です。 このチュートリアルでは、概念的な概要 MSBuild プロジェクト ファイルを WPP、Web デプロイ、およびその他の関連するテクノロジを提供します。 これは、複雑な展開プロセスを管理するこれらのツールを一緒に使用する方法について説明します。
- [Web 配置のサーバー環境を構成する](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md)です。 このチュートリアルでは、Web Deployment Agent サービス (リモート エージェント) または Web デプロイのハンドラーおよびリモートのデータベースの配置を使用してリモートの web パッケージの配置を含むさまざまな展開シナリオをサポートする Windows サーバーを構成する方法について説明します。 ご使用の環境の適切な展開方法を選択するためのガイダンスを提供し、Web Farm Framework (WFF) を使用して、サーバー ファーム内のすべての web サーバー経由で展開された web アプリケーションをレプリケートする方法について説明します。
- [Web 配置を Team Foundation Server を構成する](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md)です。 このチュートリアルでは、CI のプロセスの一部としての自動展開の特定のビルドの展開を手動でトリガーなど、さまざまな配置シナリオをサポートするために TFS を構成する方法について説明します。

>[!div class="step-by-step"]
[次へ](performing-a-what-if-deployment.md)
