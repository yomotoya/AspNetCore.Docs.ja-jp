---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios
title: Visual Studio 2010 を使用するエンタープライズ シナリオで Web アプリケーションの配置 |Microsoft ドキュメント
author: jrjlee
description: このチュートリアルのセットは、ツールとさまざまなエンタープライズ シナリオで web アプリケーションの展開に使用できる手法について説明します。 最大限利用する方法を説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/03/2012
ms.topic: article
ms.assetid: 48cfe378-d62a-48c6-a4db-6be3cead6898
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios
msc.type: authoredcontent
ms.openlocfilehash: 921b1ccd8a1f2109a51f3f75149588422fefb91d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="deploying-web-applications-in-enterprise-scenarios-using-visual-studio-2010"></a>Visual Studio 2010 を使用するエンタープライズ シナリオで Web アプリケーションの配置
====================
によって[Jason lee 著](https://github.com/jrjlee)

[PDF をダウンロードします。](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このチュートリアルのセットは、ツールとさまざまなエンタープライズ シナリオで web アプリケーションの展開に使用できる手法について説明します。 最適な Visual Studio 2010、Microsoft Build Engine (MSBuild)、インターネット インフォメーション サービス (IIS) 7.5、IIS Web 配置ツール (Web 配置)、Web Farm Framework (WFF) およびに VSDBCMD.exe などのユーティリティなどのテクノロジを使用する方法について説明します簡略化し、展開プロセスを管理します。 概念の概要とするのに役立つタスク指向のガイダンスが含まれています。
> 
> - 確認し、エンタープライズ規模の web アプリケーションの展開要件を確立します。
> - Web 配置をサポートするために、テスト、ステージング、および運用の web サーバー環境を構成します。
> - 自動化 web 配置をサポートする Team Foundation Server (TFS) 継続的インテグレーション (CI) プロセスを構成します。
> - さまざまな要件と制限に別のサーバー環境にエンタープライズ規模の web アプリケーションを展開します。
> - さまざまなサーバー環境で実行されている web アプリケーションへの変更を配置します。
> 
> > [!NOTE]
> > これらのチュートリアルでは、TFS の CI サーバーとしての使用について説明、中にガイダンスが CI の任意のサーバーに簡単に適応します。 詳細な情報を理解し、チュートリアルを利用して、TFS の必要はありません。
> 
> 
> これらのチュートリアルのイタリア語の翻訳を参照してください。 [ http://www.lucamorelli.it](http://www.lucamorelli.it)です。


## <a name="about-the-authors"></a>作成者について

Jason Lee はでプリンシパル テクノロジスト[コンテンツ マスター](http://www.contentmaster.com/)位置していた Microsoft 製品とテクノロジ、特に SharePoint と、ASP.NET は数年前です。 Jason コンピューティングどんなを保持、られて現在 MCPD MCT 認定されています。 Jason の技術記事を参照して[www.jrjlee.com](http://www.jrjlee.com/)です。

Benjamin Curry はでプリンシパル テクノロジスト[コンテンツ マスター](http://www.contentmaster.com/)のキャリア中に、ホワイト ペーパーは、SDK のドキュメント、PowerPoint プレゼンテーション、およびインストラクター指導し、オンライン トレーニング コースを指定したが書き込まです。 ASP.NET ドキュメント チームの元のメンバー、彼が携わっての Microsoft の web テクノロジ。

## <a name="target-audience"></a>対象読者

このチュートリアルのセットは ASP.NET web アプリケーションの開発者と Visual Studio 2010 を使用して、エンタープライズ規模の web アプリケーションを作成するソリューション設計者です。 コンテンツからほとんどの値を取得するには、操作に精通して Visual Studio 2010 であり、TFS と ASP.NET MVC 3、Windows Communication Foundation (WCF)、IIS、SQL などの Microsoft web プラットフォーム テクノロジの認知度と共にの基礎知識がある必要があります。サーバー、および Visual Studio データベース プロジェクト。 ただし、展開ツールとテクノロジに慣れているまたは CI システムを設定する方法を理解する必要がある必要はありません。

## <a name="requirements"></a>要件

次のチュートリアルをこれらのチュートリアルを記述するタスクの実行は、開発用コンピューターでこのソフトウェアをインストールする必要があります。

- Visual Studio 2010 Premium または Ultimate Edition Service Pack 1
- .NET Framework 4.0
- .NET framework 3.5 Service Pack 1
- ASP.NET MVC 3.0
- IIS 7.5 Express します。
- SQL Server Express 2008 R2

これらのチュートリアルで説明されている展開の手順を実行するには、サンプル Web アプリケーションの展開環境にアクセスする必要があります。 最良の結果をこれらの環境は、組織のエンタープライズ配置パターンを反映する必要があります。 デプロイ環境とお客様の組織の要件を反映するように、このドキュメントのチュートリアルを変更できます。

## <a name="series-contents"></a>系列の内容

この概要のセクションでは、さらに 2 つのトピックで構成されます。 これらのチュートリアルについては、次のいくつかの広範なコンテキストを提供するもの。

- [エンタープライズ Web 展開: シナリオの概要](enterprise-web-deployment-scenario-overview.md)です。 このトピックでは、基盤のこのシリーズのチュートリアルは、各シナリオについて説明します。 シナリオでは、エンタープライズ規模の web アプリケーションを開発して Fabrikam, Inc. という名前の架空の会社のアプリケーション ライフ サイクル管理 (ALM) の要件に焦点を当てています。
- [アプリケーション ライフ サイクル管理: 実稼働環境に開発から](application-lifecycle-management-from-development-to-production.md)です。 このトピックでは、展開プロセスの大まかな、エンド ツー エンドの概要を説明します。 Fabrikam, Inc. が継続的な開発プロセスの一部としてテスト、ステージング、実稼働環境でエンタープライズ規模 ASP.NET web アプリケーションを移動する方法を示しています。

系列には、チュートリアルの 4 つのセットが含まれています。 各 web 配置のさまざまな側面に焦点を当てています。

- [Web、企業内の配置](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md)です。 このチュートリアルでは、概念的な概要 MSBuild プロジェクト ファイルを Web 発行パイプライン、Web デプロイ、およびその他の関連するテクノロジを提供します。 これは、複雑な展開プロセスを管理するこれらのツールを一緒に使用する方法について説明します。
- [Web 配置のサーバー環境を構成する](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md)です。 このチュートリアルでは、Web Deployment Agent サービス (「リモート エージェント」) または Web デプロイのハンドラーおよびリモートのデータベースの配置を使用してリモートの web パッケージの配置を含むさまざまな展開シナリオをサポートする Windows サーバーを構成する方法について説明します。 ご使用の環境の適切な展開方法を選択するためのガイダンスを提供し、WFF を使用して、サーバー ファーム内のすべての web サーバー経由で展開された web アプリケーションをレプリケートする方法について説明します。
- [Web 配置を Team Foundation Server を構成する](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md)です。 このチュートリアルでは、CI のプロセスの一部としての自動展開の特定のビルドの展開を手動でトリガーなど、さまざまな配置シナリオをサポートするために TFS を構成する方法について説明します。
- [企業の Web 配置を高度な](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md)します。 このチュートリアルは複数の環境のデータベースの配置をカスタマイズする、ファイルとフォルダーの展開から除外および展開プロセス中に web アプリケーションをオフラインにするなどのさまざまな高度な展開タスクを実行する方法を説明します.

## <a name="where-to-start"></a>開始します。

このチュートリアルのセットは、参照の実装を提供し、タスクとチュートリアル一般的なコンテキストを提供すると共に、架空の企業の展開シナリオの複雑さのレベルが現実的なサンプル ソリューションを使用します。 次のトピックでは、 [Enterprise Web 展開: シナリオの概要](enterprise-web-deployment-scenario-overview.md)シナリオとサンプルのソリューションについて説明します。 そこからチュートリアルおよびトピックにニーズに最も近いを通じて使用できます。

> [!div class="step-by-step"]
> [次へ](enterprise-web-deployment-scenario-overview.md)
