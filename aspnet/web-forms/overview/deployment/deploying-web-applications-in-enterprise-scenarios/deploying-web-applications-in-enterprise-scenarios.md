---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios
title: Visual Studio 2010 を使用してエンタープライズ シナリオで Web アプリケーションの配置 |Microsoft Docs
author: jrjlee
description: この一連のチュートリアルでは、ツールとさまざまなエンタープライズ シナリオで web アプリケーションの展開に使用できる手法について説明します。 最適な使用方法について説明しています.
ms.author: aspnetcontent
ms.date: 05/03/2012
ms.assetid: 48cfe378-d62a-48c6-a4db-6be3cead6898
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios
msc.type: authoredcontent
ms.openlocfilehash: b55aeb863694ca3ac71f2a1a46d11981e178dcb4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816588"
---
<a name="deploying-web-applications-in-enterprise-scenarios-using-visual-studio-2010"></a>Visual Studio 2010 を使用してエンタープライズ シナリオで Web アプリケーションを展開します。
====================
によって[Jason Lee](https://github.com/jrjlee)

[PDF のダウンロード](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> この一連のチュートリアルでは、ツールとさまざまなエンタープライズ シナリオで web アプリケーションの展開に使用できる手法について説明します。 最適な Visual Studio 2010、Microsoft Build Engine (MSBuild)、インターネット インフォメーション サービス (IIS) 7.5、IIS の Web 配置ツール (Web 配置)、Web Farm Framework (WFF) およびに VSDBCMD.exe などのユーティリティなどのテクノロジを使用する方法について説明します簡略化と展開プロセスを管理します。 概念の要約とするのに役立つタスク指向のガイダンスが含まれています。
> 
> - 確認し、エンタープライズ規模の web アプリケーションのデプロイ要件を確立します。
> - Web 配置をサポートするために、テスト、ステージング、および運用環境の web サーバー環境を構成します。
> - 自動 web 配置をサポートする Team Foundation Server (TFS) の継続的インテグレーション (CI) プロセスを構成します。
> - さまざまな要件と制限の別のサーバー環境にエンタープライズ規模の web アプリケーションをデプロイします。
> - 変更を別のサーバー環境で実行されている web アプリケーションに配置します。
> 
> > [!NOTE]
> > これらのチュートリアルでは、CI サーバーとして、TFS の使用について説明する、中にガイダンスが任意の CI サーバーに簡単に応用です。 理解して、チュートリアルでは、TFS の詳細な知識は不要です。
> 
> 
> これらのチュートリアルのイタリア語の翻訳を参照してください。 [ http://www.lucamorelli.it](http://www.lucamorelli.it)します。


## <a name="about-the-authors"></a>作成者について

Jason Lee はプリンシパルの技術者と[コンテンツ マスター](http://www.contentmaster.com/)で取り組んでいます Microsoft 製品とテクノロジ、SharePoint と ASP.NET を特に数年間です。 Jason コンピューティングで博士号を持ちは現在 MCPD、MCTS 認定します。 Jason の技術的なブログを読むことができます[www.jrjlee.com](http://www.jrjlee.com/)します。

Benjamin Curry はプリンシパルの技術者と[コンテンツ マスター](http://www.contentmaster.com/)キャリアの中に、ホワイト ペーパー、SDK のドキュメント、PowerPoint プレゼンテーション、およびインストラクター主導とオンライン トレーニング コースを記述するがします。 ASP.NET ドキュメント チームの元のメンバー、携わってきましたを Microsoft の web テクノロジ 10 年以上。

## <a name="target-audience"></a>対象ユーザー

このチュートリアルのセットは ASP.NET web アプリケーションの開発者と Visual Studio 2010 を使用して、エンタープライズ規模の web アプリケーションを作成するソリューション設計者です。 コンテンツからほとんどの値を取得するには、Visual Studio 2010 を使用して快適し、と共に ASP.NET MVC 3、Windows Communication Foundation (WCF)、IIS、SQL などの Microsoft web プラットフォーム テクノロジの認識、TFS での基本的な知識がある必要があります。サーバー、および Visual Studio データベース プロジェクト。 ただし、CI システムを設定する方法を理解する必要がありますまたは展開ツールとテクノロジについて理解する必要はありません。

## <a name="requirements"></a>必要条件

次のチュートリアルをこれらのチュートリアルを記述するタスクの実行は、開発用コンピューターにこのソフトウェアをインストールする必要があります。

- Visual Studio 2010 Premium または Ultimate Edition Service Pack 1
- .NET Framework 4.0
- .NET framework 3.5 Service pack 1
- ASP.NET MVC 3.0
- IIS 7.5 Express します。
- SQL Server Express 2008 R2

これらのチュートリアルで説明されている展開の手順を実行するには、サンプル Web アプリケーションの展開環境にアクセスする必要があります。 最良の結果をこれらの環境は、組織のエンタープライズ デプロイ パターンを反映する必要があります。 デプロイ環境と組織の要件を反映するように、このドキュメントのチュートリアルを変更できます。

## <a name="series-contents"></a>シリーズの内容

この概要のセクションでは、さらに 2 つのトピックで構成されます。 これらは、次のチュートリアルについては、いくつかのより広範なコンテキストを提供する設計されています。

- [エンタープライズ Web 展開: シナリオの概要](enterprise-web-deployment-scenario-overview.md)します。 このトピックを支えるは、このシリーズのチュートリアルでは、各シナリオについて説明します。 シナリオでは、エンタープライズ規模の web アプリケーションを開発して、Fabrikam, Inc. のという架空の会社のアプリケーション ライフ サイクル管理 (ALM) の要件に重点を置いています。
- [アプリケーション ライフ サイクル管理: 開発運用から](application-lifecycle-management-from-development-to-production.md)します。 このトピックでは、展開プロセスの概要、エンド ツー エンドの概要を説明します。 Fabrikam, Inc. が継続的な開発プロセスの一部としてテスト、ステージング、実稼働環境でエンタープライズ スケール ASP.NET web アプリケーションを移動する方法を示しています。

系列には、チュートリアルの 4 つのセットが含まれています。 各 web 配置のさまざまな側面に焦点を当てています。

- [Web、企業内の配置](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md)します。 このチュートリアルでは、概念的な概要 MSBuild プロジェクト ファイルを Web 発行パイプライン、Web デプロイ、およびその他の関連テクノロジを提供します。 これは、複雑な展開プロセスを管理するこれらのツールをまとめて使用する方法について説明します。
- [Web 配置のサーバー環境を構成する](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md)します。 このチュートリアルでは、Web Deployment Agent サービス (「リモート エージェント」) または Web 配置ハンドラーとリモートのデータベースの配置を使用して、リモート web パッケージ展開を含め、さまざまなデプロイメント シナリオをサポートするために Windows サーバーを構成する方法について説明します。 独自の環境の適切な展開方法の選択のガイダンスを提供し、WFF を使用して、サーバー ファーム内のすべての web サーバー間でデプロイされた web アプリケーションをレプリケートする方法について説明します。
- [Web 配置の Team Foundation Server を構成する](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md)します。 このチュートリアルでは、CI プロセスの一部として自動化された展開を含め、特定のビルドの展開を手動でトリガーされる、さまざまなデプロイ シナリオをサポートするために TFS を構成する方法について説明します。
- [エンタープライズ Web 配置を高度な](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md)します。 このチュートリアルは、複数の環境のデータベースの展開をカスタマイズ、展開からのファイルとフォルダーの除外、展開プロセス中に web アプリケーションをオフラインすることなどのさまざまな高度な展開タスクを実行する方法を説明します.

## <a name="where-to-start"></a>開始します。

この一連のチュートリアルは、参照実装を提供して、タスクとチュートリアルの一般的なコンテキストを提供すると共に、架空の企業の展開シナリオの複雑さの現実的なレベルを持つサンプル ソリューションを使用します。 次のトピックでは、[エンタープライズ Web 展開: シナリオの概要](enterprise-web-deployment-scenario-overview.md)シナリオとサンプルのソリューションが導入されています。 そこから、チュートリアルと、ニーズに最も近いトピックを操作できます。

> [!div class="step-by-step"]
> [次へ](enterprise-web-deployment-scenario-overview.md)
