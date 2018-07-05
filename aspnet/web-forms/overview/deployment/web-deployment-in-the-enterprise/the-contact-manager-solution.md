---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
title: 連絡先マネージャー ソリューション |Microsoft Docs
author: jrjlee
description: この一連のチュートリアルはサンプル ソリューションを使用して&#x2014;連絡先マネージャー ソリューション&#x2014;現実的なレベルで、エンタープライズ規模のアプリケーションを表す.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 4d8c8d19-055b-4b70-9ee1-f748f0db3a01
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: 8187766190da43ded52359892601f8129b9940ce
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388109"
---
<a name="the-contact-manager-solution"></a>連絡先マネージャー ソリューション
====================
によって[Jason Lee](https://github.com/jrjlee)

[PDF のダウンロード](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> これは、[一連のチュートリアル](web-deployment-in-the-enterprise.md)サンプル ソリューションを使用して&#x2014;連絡先マネージャー ソリューション&#x2014;に現実的な複雑さのレベルを持つエンタープライズ規模のアプリケーションを表します。 このトピックでは、連絡先マネージャー ソリューションを紹介し、ソリューションの主要なコンポーネントについて説明します、このようなエンタープライズ環境では、さまざまな対象プラットフォームのアプリケーションを展開する際の課題を識別します。
> 
> これらのチュートリアルで、トピックを操作するときは、エンタープライズ展開シナリオに固有の課題を満たす方法について説明するリファレンス実装として連絡先マネージャー ソリューションを使用できます。 次のトピックでは、 [、連絡先マネージャー ソリューションの設定を](setting-up-the-contact-manager-solution.md)、ダウンロードして、開発者ワークステーションでソリューションを実行する方法について説明します。


## <a name="solution-overview"></a>ソリューションの概要

連絡先マネージャー ソリューションは、次の 4 つの個別のプロジェクトで構成されます。

![](the-contact-manager-solution/_static/image1.png)

- **ContactManager.Mvc**します。 これは、ASP.NET MVC 3 web アプリケーション プロジェクトをソリューションのエントリ ポイントを表すです。 作成し、連絡先の詳細を表示する機能をユーザーに提供するなど、いくつかの基本的な web アプリケーション機能を提供します。 アプリケーションは、連絡先との認証と承認を管理するには、ASP.NET アプリケーション サービス データベースを管理する Windows Communication Foundation (WCF) サービスに依存します。
- **ContactManager.Database**します。 これは、Visual Studio データベース プロジェクトです。 プロジェクトでは、連絡先の詳細をストア データベースのスキーマを定義します。
- **ContactManager.Service**します。 これは、WCF web サービス プロジェクトです。 実行する呼び出し元を許可するエンドポイントを作成する WCF サービス公開の取得、更新、および delete (CRUD) 操作を**ContactManager**データベース。 サービスが依存、 **ContactManager**データベースおよび**ContactManager.Common.dll**アセンブリ。
- **ContactManager.Common**します。 これは、クラス ライブラリ プロジェクトです。 WCF サービスは、このアセンブリで定義された型に依存します。

ソリューションには、発行をという名前のソリューション フォルダーも含まれています。 これには、各種のカスタム プロジェクト ファイルおよび制御して、ビルドおよび配置プロセスを操作する方法を説明するコマンド ファイルが含まれています。 これらは、このチュートリアルの後半で詳しく説明します。

概念レベルでは、ソリューションのコンポーネントが一緒に合わせてこのような。

![](the-contact-manager-solution/_static/image2.png)

> [!NOTE]
> ASP.NET MVC 3 web アプリケーションでは、ASP.NET メンバーシップ プロバイダーを使用するときに、web アプリケーション内のすべてのページは匿名アクセスを許可します。 これは明らかに現実的な構成です。 ただし、ソリューションをすると、配置、およびユーザー アカウントとロールを構成しなくても、ソリューションをテストしやすくには、この方法で設定します。


## <a name="deployment-challenges"></a>展開の課題

連絡先マネージャー ソリューションは、エンタープライズ展開シナリオの多くに共通するいくつかの展開の課題を示しています。

- ソリューションは、複数の依存プロジェクトで構成されます。 これらのプロジェクトを同時に展開する必要があります。
- 接続文字列とサービスのエンドポイントは、環境ごとに更新する必要があり、多くの場合にこの情報は使用できません、開発者にします。
- 展開するときに、 **ContactManager**データベース、ステージングと運用環境には、以降のデプロイに既存のデータを保持する必要があります。
- ASP.NET アプリケーション サービス データベースを展開するときに一部の構成データをデプロイし、ユーザー アカウントのデータを省略する必要があります。
- 一部のファイルとフォルダーを展開する必要がなく、プロジェクトが含まれます。 展開プロセスからこれらのファイルとフォルダーを除外する必要があります。
- ソリューションは、Team Foundation Server (TFS) ビルド サーバーからの自動展開をサポートする必要があります。

## <a name="conclusion"></a>まとめ

このトピックでは、連絡先マネージャー ソリューションの概要を提供し、エンタープライズ展開シナリオの多くに共通する固有の展開の課題の一部を特定します。 このチュートリアルの残りのトピックでは、これらの課題を満たすために使用できる手法について説明します。

次のトピックでは、 [、連絡先マネージャー ソリューションの設定を](setting-up-the-contact-manager-solution.md)、ダウンロードして、開発者ワークステーションでソリューションを実行する方法について説明します。

> [!div class="step-by-step"]
> [前へ](web-deployment-in-the-enterprise.md)
> [次へ](setting-up-the-contact-manager-solution.md)
