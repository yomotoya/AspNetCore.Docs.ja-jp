---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
title: 連絡先のマネージャー ソリューション |Microsoft ドキュメント
author: jrjlee
description: この一連のチュートリアルのサンプル ソリューションを使用して&#x2014;Contact Manager ソリューション&#x2014;現実的なレベルを持つエンタープライズ規模アプリケーションを表すしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 4d8c8d19-055b-4b70-9ee1-f748f0db3a01
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: d7034f800df98747d10401d7e2c7297fea0e46d4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30883701"
---
<a name="the-contact-manager-solution"></a>連絡先のマネージャー ソリューション
====================
によって[Jason lee 著](https://github.com/jrjlee)

[PDF をダウンロードします。](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> これは、[一連のチュートリアル](web-deployment-in-the-enterprise.md)サンプル ソリューションを使用&#x2014;Contact Manager ソリューション&#x2014;現実的な複雑さのレベルを持つエンタープライズ規模アプリケーションを表すです。 このトピックは、連絡先のマネージャー ソリューションが導入されています、ソリューションの主要なコンポーネントについて説明し、この種のエンタープライズ環境でさまざまな対象プラットフォームにアプリケーションの展開の課題を特定します。
> 
> トピックを介してこれらのチュートリアルで作業するときは、エンタープライズ展開シナリオでの特定の課題に対応する方法を示しています。 参照の実装として、連絡先のマネージャー ソリューションを使用できます。 次のトピックでは、 [、連絡先のマネージャー ソリューションの設定を](setting-up-the-contact-manager-solution.md)、ダウンロードして、ソリューション開発者のワークステーションを実行する方法について説明します。


## <a name="solution-overview"></a>ソリューションの概要

連絡先のマネージャー ソリューションは、次の 4 つの個々 のプロジェクトで構成されます。

![](the-contact-manager-solution/_static/image1.png)

- **ContactManager.Mvc**です。 これは、ASP.NET MVC 3 web アプリケーション プロジェクトをソリューションのエントリ ポイントを表すです。 ユーザーの作成し、連絡先の詳細を表示する機能に提供など、いくつかの基本的な web アプリケーション機能を提供します。 アプリケーションは、取引先担当者および認証と承認を管理する ASP.NET アプリケーション サービス データベースを管理する Windows Communication Foundation (WCF) サービスに依存します。
- **ContactManager.Database**です。 これは、Visual Studio データベース プロジェクトです。 プロジェクトでは、連絡先の詳細をストア データベース用のスキーマを定義します。
- **ContactManager.Service**です。 これは、WCF web サービス プロジェクトです。 取得、更新、および delete (CRUD) 操作を実行する呼び出し元を許可するエンドポイントを作成する WCF サービス公開、 **ContactManager**データベース。 サービスが依存、 **ContactManager**データベースおよび**ContactManager.Common.dll**アセンブリ。
- **ContactManager.Common**です。 これは、クラス ライブラリ プロジェクトです。 WCF サービスは、このアセンブリで定義されている型に依存します。

ソリューションには、発行をという名前のソリューション フォルダーも含まれています。 これには、各種のカスタム プロジェクト ファイルおよびを制御し、ビルドおよび配置プロセスを操作する方法をデモンストレーションするコマンド ファイルが含まれています。 これらは、このチュートリアルで後でさらに詳しく説明します。

概念レベルで、ソリューションのコンポーネントは次のように継ぎ合わさ。

![](the-contact-manager-solution/_static/image2.png)

> [!NOTE]
> ASP.NET MVC 3 web アプリケーションでは、ASP.NET メンバーシップ プロバイダーを使用するときに、web アプリケーション内のすべてのページは匿名アクセスを許可します。 これはいない現実的な構成では明確です。 ただし、ソリューションは簡単に展開し、ソリューションをテストするユーザー アカウントとロールを構成せずにするには、この方法でセットアップられます。


## <a name="deployment-challenges"></a>展開に関する問題

連絡先のマネージャー ソリューションは、エンタープライズ展開シナリオの多くに共通するいくつかの展開の課題を示しています。

- ソリューションは、複数の依存プロジェクトで構成されます。 これらのプロジェクトを同時に展開する必要があります。
- 接続文字列とサービス エンドポイントと環境ごとに更新する必要があります。 でき、多くの場合にこの情報はいない開発者にします。
- 展開するときに、 **ContactManager**データベース、ステージングと運用環境には、後続のデプロイに既存のデータを保持する必要があります。
- ASP.NET アプリケーション サービス データベースを展開するときに、構成データの一部の展開が、すべてのユーザー アカウントのデータを省略する必要があります。
- 一部のファイルとフォルダーを展開する必要がなく、プロジェクトが含まれます。 展開プロセスからこれらのファイルとフォルダーを除外する必要があります。
- ソリューションは、Team Foundation Server (TFS) のビルド サーバーからの自動展開をサポートする必要があります。

## <a name="conclusion"></a>まとめ

このトピックでは、連絡先のマネージャー ソリューションの概要を提供し、エンタープライズ展開シナリオの多くに共通する固有の展開の課題の一部を識別します。 このチュートリアルの残りのトピックでは、これらの課題に使用できる手法について説明します。

次のトピックでは、 [、連絡先のマネージャー ソリューションの設定を](setting-up-the-contact-manager-solution.md)、ダウンロードして、ソリューション開発者のワークステーションを実行する方法について説明します。

> [!div class="step-by-step"]
> [前へ](web-deployment-in-the-enterprise.md)
> [次へ](setting-up-the-contact-manager-solution.md)
