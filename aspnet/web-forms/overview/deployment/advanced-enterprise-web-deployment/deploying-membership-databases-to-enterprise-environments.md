---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments
title: エンタープライズ環境にメンバーシップ データベースを展開する |Microsoft Docs
author: jrjlee
description: このトピックでは、重要な考慮事項と課題の克服 ASP.NET アプリケーション サービス データベース (の一般的な... をプロビジョニングするときにする必要がありますについて説明します。
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 3cf765df-d311-4f68-a295-c9685ceea830
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments
msc.type: authoredcontent
ms.openlocfilehash: 9df152866b54f55c2b00611331e868f98bd2f3e4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827189"
---
<a name="deploying-membership-databases-to-enterprise-environments"></a>エンタープライズ環境にメンバーシップ データベースを展開します。
====================
によって[Jason Lee](https://github.com/jrjlee)

[PDF のダウンロード](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このトピックでは、重要な考慮事項と課題の克服とテスト、ステージング、または実稼働環境では、(より一般的にはメンバーシップ データベースと呼ばれる) データベースのサービスの ASP.NET アプリケーションを準備する必要がありますについて説明します。 これらの課題を満たすために使用できる方法も説明します。


このトピックでは、一連の Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に基づいているチュートリアルの一部を形成します。このチュートリアル シリーズは、サンプル ソリューションを使用して&#x2014;、[連絡先マネージャー ソリューション](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;現実的なレベルの ASP.NET MVC 3 アプリケーション、Windows の通信など、複雑な web アプリケーションを表すFoundation (WCF) サービスとデータベース プロジェクト。

これらのチュートリアルの中核の展開方法が分割のプロジェクト ファイルの方法で説明されているに基づいて[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)、によって制御されるビルド プロセスでは、2 つのプロジェクト ファイル&#x2014;1 つを格納しています。すべての変換先の環境と環境固有のビルドと配置の設定を含む 1 つに適用される手順をビルドします。 ビルド時に、環境固有のプロジェクト ファイルは、ビルド手順の完全なセットを形成する環境に依存しないプロジェクト ファイルにマージされます。

## <a name="what-are-the-issues-when-you-deploy-a-membership-database"></a>メンバーシップ データベースを展開するときに問題は何ですか。

ほとんどの場合、データベースでは、展開戦略を立てるときにまず検討する必要があるがどのようなデータを展開します。 開発またはテスト環境を迅速かつ簡単にテストを容易にするユーザー アカウントのデータを展開する場合があります。 ステージング環境または運用環境では、多くのユーザー アカウントのデータを展開することはありません。

残念ながら、ASP.NET メンバーシップ データベースは、もっと複雑なを決定するいくつかの特定の課題を導入します。

- スキーマのみのデプロイメントは、非動作状態にメンバーシップ データベースに残ります。 これは、メンバーシップ データベースには、一部の構成データが含まれているため (で、 **aspnet\_SchemaVersions**テーブル)、データベースが機能するために必要とします。 そのため、ユーザー アカウントのデータを除外するために、メンバーシップ データベースのスキーマのみのデプロイを実行すると、重要な構成データを追加する配置後スクリプトを実行する必要があります。
- メンバーシップ データベースの構成方法に応じてメンバーシップ プロバイダーはマシン キーを使用してパスワードを暗号化し、データベース内に格納する可能性があります。 この場合、データベースを展開するすべてのユーザー アカウント データは移行先サーバーで使用できなくなります。 このため、ユーザー アカウントのデータを展開するシナリオはサポートではありません。

## <a name="choosing-a-membership-database-strategy"></a>メンバーシップ データベース ストラテジの選択

エンタープライズ サーバー環境でメンバーシップ データベースをプロビジョニングする方法を選択すると、次のガイドラインを使用します。

- 可能な場合は、メンバーシップ データベースを展開しないでください。 代わりに、ターゲット データベースのサーバーでメンバーシップ データベースを手動で作成します。 メンバーシップ データベース スキーマをカスタマイズしていない場合だけを作成、新しく situ でを使用して、変換先で、 [ASP.NET SQL Server の登録ツール (aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx)します。
- メンバーシップ データベースを展開するオプションがない場合&#x2014;例では、データベース スキーマに大幅な変更を行った場合の&#x2014;ユーザー アカウントのデータを除外する、メンバーシップ データベースのスキーマのみのデプロイを実行する必要があり、必要な構成データを追加する配置後スクリプトを実行します。 これらのアプローチで、広範なガイダンスを確認できます[方法: ASP.NET メンバーシップ データベースなしを含むユーザー アカウントをデプロイ](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx)します。

注意することが重要 *、メンバーシップ データベースのスキーマが非常に静的なある可能性があります*します。 可能性を定期的にスキーマを更新する必要がありますがない場合でも、メンバーシップ データベースをカスタマイズして&#x2014;web アプリケーションまたはデータベース プロジェクト内のコードと同じ頻度で変更することがないです。 そのため、自動またはシングル ステップの展開プロセスにメンバーシップ データベースを含める必要はありません。

## <a name="using-vsdbcmd-to-update-a-membership-database-schema"></a>VSDBCMD を使用して、メンバーシップ データベース スキーマを更新するには

最初のデプロイ後に、メンバーシップ データベースの構造を変更する場合、データベースを再デプロイする (Web 配置) インターネット インフォメーション サービス (IIS) Web 配置ツールを使用する可能性がありますされません。 Web 配置でデータベースのデプロイ機能には、移行先データベース差分更新を実行する機能は含まれません&#x2014;代わりに、Web 配置する必要があります削除し、データベースを再作成します。 これは、既存のユーザー アカウントのデータはステージング環境または運用環境で通常は望ましくないが失われることを意味します。

代わりに、VSDBCMD ユーティリティを使用して、転送先データベースのスキーマを更新することです。 VSDBCMD には、2 つの重要な機能が含まれています。 まず、.dbschema ファイルに既存のデータベースのスキーマをインポートできます。 次に、つまりのみ変更があったとき、ターゲット データベースの最新の状態のために必要なすべてのデータが失われないと差分の更新プログラムとして .dbschema ファイルを既存のデータベースにデプロイできます。

これらの大まかな手順を使用するにはメンバーシップ データベース スキーマを更新します。

1. VSDBCMD を使用して、**インポート**ソース メンバーシップ データベースの .dbschema ファイルを生成するアクション。 この手順で説明されて[方法: コマンド プロンプトからスキーマをインポート](https://msdn.microsoft.com/library/dd172135.aspx)します。
2. VSDBCMD を使用して、**デプロイ**先メンバーシップ データベースに .dbschema ファイルを展開するアクション。 この手順で説明されて[VSDBCMD のコマンド ライン リファレンス。EXE (展開およびスキーマのインポート)](https://msdn.microsoft.com/library/dd193283.aspx)します。

## <a name="conclusion"></a>まとめ

このトピックでは、さまざまなターゲット環境での ASP.NET メンバーシップ データベースをプロビジョニングする必要がある場合に発生する可能性の課題のいくつかについて説明します。 具体的には、理由スキーマのみのデプロイメント メンバーシップ データベースは操作不可状態にして、ユーザー アカウントのデータの展開はサポートされていない理由について説明します。 トピックには、プロビジョニング、デプロイ、およびさまざまなシナリオでメンバーシップ データベースを更新する方法のガイダンスも表示されます。

## <a name="further-reading"></a>関連項目

詳細なガイダンスと、VSDBCMD を使用する方法の例については、次を参照してください。 [VSDBCMD のコマンド ライン リファレンス。EXE (展開およびスキーマのインポート)](https://msdn.microsoft.com/library/dd193283.aspx)と[方法: コマンド プロンプトからスキーマをインポート](https://msdn.microsoft.com/library/dd172135.aspx)します。 Aspnet の使用の詳細については\_regsql.exe メンバーシップ データベースの作成を参照してください[ASP.NET SQL Server の登録ツール (aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx)します。 メンバーシップ データベースのデプロイに関する一般的なガイダンスについては、次を参照してください。[方法: ASP.NET メンバーシップ データベースなしを含むユーザー アカウントをデプロイ](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx)します。

> [!div class="step-by-step"]
> [前へ](deploying-database-role-memberships-to-test-environments.md)
> [次へ](excluding-files-and-folders-from-deployment.md)
