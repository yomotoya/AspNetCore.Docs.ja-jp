---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments
title: "エンタープライズ環境にメンバーシップ データベースを展開する |Microsoft ドキュメント"
author: jrjlee
description: "このトピックは、重要な考慮事項と課題を克服 ASP.NET アプリケーション サービス データベース (複数の一般的な... をプロビジョニングする際にする必要がありますについて説明します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 3cf765df-d311-4f68-a295-c9685ceea830
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments
msc.type: authoredcontent
ms.openlocfilehash: 27fade9fc5cae917579d4963da7bca12f6a5cda1
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2018
---
<a name="deploying-membership-databases-to-enterprise-environments"></a>エンタープライズ環境にメンバーシップ データベースの配置
====================
によって[Jason lee 著](https://github.com/jrjlee)

[PDF をダウンロードします。](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このトピックには、重要な考慮事項と課題 (メンバーシップ データベースとよく呼ばれる) データベースでのテスト、ステージング、または実稼働環境でのサービスの ASP.NET アプリケーションを準備するを解決する必要がありますがについて説明します。 これらの課題に対応する方法についても説明します。


このトピックの Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に関するチュートリアル シリーズの一部を形成します。サンプル ソリューション & #x 2014; このチュートリアルのシリーズを使用して、 [Contact Manager ソリューション](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; を ASP.NET MVC 3 アプリケーションを Windows のなどの複雑性のレベルが現実的な web アプリケーションを表すCommunication Foundation (WCF) サービスとデータベース プロジェクト。

説明されている分割プロジェクト ファイル アプローチに基づいて、これらのチュートリアルの中心に配置メソッド[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)、によって制御されるビルド プロセスで 2 つのプロジェクト ファイル & #x 2014; 1 つを含む各配置先の環境と環境固有のビルドと配置の設定を含む 1 つに適用される手順をビルドします。 ビルド時に環境固有のプロジェクト ファイルは、ビルドの手順の完全なセットを形成する環境に依存しないプロジェクト ファイルにマージされます。

## <a name="what-are-the-issues-when-you-deploy-a-membership-database"></a>メンバーシップ データベースを展開するときに、問題とは?

ほとんどの場合、データベースの配置戦略を策定するときに、まず検討する必要がどのようなデータを展開します。 開発またはテスト環境を迅速かつ簡単なテストを容易にするユーザー アカウントのデータを展開することができます。 ステージングまたは運用環境では、ユーザー アカウントのデータを配置することが非常に高くはありません。

残念ながら、ASP.NET メンバーシップ データベースには、もっと複雑なこの判断を行ういくつかの特定課題が導入されました。

- スキーマのみの展開によっては、非運用状態で、メンバーシップ データベースが残ります。 これは、メンバーシップ データベースには、一部の構成データが含まれているため (で、 **aspnet\_SchemaVersions**テーブル) をデータベースが機能するために必要とします。 そのため、ユーザー アカウントのデータを除外するために、メンバーシップ データベースのスキーマのみの展開を実行する場合は、重要な構成データを追加する配置後スクリプトを実行する必要があります。
- メンバーシップ データベースの構成方法に応じてメンバーシップ プロバイダーはマシン キーを使用してパスワードを暗号化し、データベースに保存する可能性があります。 ここでは、データベースを展開するユーザー アカウント データは移行先サーバーで使用できなくなります。 このため、ユーザー アカウントのデータを展開するシナリオはサポートではありません。

## <a name="choosing-a-membership-database-strategy"></a>メンバーシップ データベース ストラテジの選択

エンタープライズ サーバー環境でのメンバーシップ データベースをプロビジョニングする方法を選択すると、次のガイドラインを使用します。

- 可能な限りでは、メンバーシップ データベースを展開することはありません。 代わりに、ターゲット データベースのサーバーでメンバーシップ データベースを手動で作成します。 メンバーシップ データベース スキーマをカスタマイズしていない場合だけで、新しいもので situ で作成を使用して、移行先、 [ASP.NET SQL Server の登録ツール (aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx)です。
- メンバーシップ データベース & #x 2014; を展開するオプションがない場合など、データベース スキーマ & #x 2014; に対して大幅な変更を行った場合行う必要があります、スキーマのみ、データベースの配置のメンバーシップ、ユーザー アカウントのデータを除外して必要な構成データを追加する配置後スクリプトを実行します。 これらのアプローチの広範なガイダンスを検索できます[する方法: ASP.NET メンバーシップ データベースなしを含むユーザー アカウントを展開](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx)です。

留意することが重要*、メンバーシップ データベースのスキーマは比較的静的である可能性があります*です。 メンバーシップ データベース、カスタマイズした場合でも、定期的に & #x 2014 スキーマを更新する必要があります以外の場合は、web アプリケーションとデータベース プロジェクト内のコードと同じ頻度で変更することはしません可能性はありません。 そのため、自動または 1 ステップの展開プロセスにメンバーシップ データベースを含める必要はありません。

## <a name="using-vsdbcmd-to-update-a-membership-database-schema"></a>VSDBCMD を使用してメンバーシップ データベース スキーマを更新するには

場合は、最初の展開の後、メンバーシップ データベースの構造を変更することがありますしないするを (Web 配置) インターネット インフォメーション サービス (IIS) Web 配置ツールを使用して、データベースを再展開します。 Web Deploy でデータベースの配置機能には、転送先データベース & #x 2014 を差分更新を有効にする機能が含まれていません; 代わりに、Web デプロイを削除してください、データベースを再作成します。 これは、既存のユーザー アカウントのデータはステージング環境または実稼働環境で通常は望ましくないが失われることを意味します。

代わりに、VSDBCMD ユーティリティを使用して、転送先データベースのスキーマの更新を開始します。 VSDBCMD には、次の 2 つの重要な機能が含まれています。 最初に、既存のデータベースのスキーマ .dbschema ファイルにインポートできます。 次に既存のデータベースに差分更新プログラムとだけ変更があったとき、ターゲット データベースの最新の状態のために必要なすべてのデータが失われないとして .dbschema ファイルを展開することができます。

これらの大まかな手順を使用して、メンバーシップ データベース スキーマを更新することができます。

1. VSDBCMD を使用して**インポート**ソース メンバーシップ データベースの .dbschema ファイルを生成するアクション。 この手順で説明されて[する方法: コマンド プロンプトからスキーマをインポート](https://msdn.microsoft.com/library/dd172135.aspx)です。
2. VSDBCMD を使用して**展開**.dbschema ファイル先メンバーシップ データベースを配置するアクション。 この手順で説明されて[VSDBCMD のコマンド ライン リファレンスです。EXE (配置とスキーマのインポート)](https://msdn.microsoft.com/library/dd193283.aspx)です。

## <a name="conclusion"></a>まとめ

このトピックでは、さまざまなターゲット環境での ASP.NET メンバーシップ データベースをプロビジョニングする必要がある場合に直面する課題の一部について説明します。 具体的には、なぜスキーマのみのデプロイメント、メンバーシップ データベース非運用状態にして、ユーザー アカウントのデータの展開はサポートされていない理由について説明します。 トピックでは、準備、展開、およびさまざまなシナリオでのメンバーシップ データベースを更新する方法のガイダンスも示しました。

## <a name="further-reading"></a>関連項目

詳細なガイダンスと VSDBCMD を使用する方法の例については、次を参照してください。 [VSDBCMD のコマンド ライン リファレンスです。EXE (配置とスキーマのインポート)](https://msdn.microsoft.com/library/dd193283.aspx)と[する方法: コマンド プロンプトからスキーマをインポート](https://msdn.microsoft.com/library/dd172135.aspx)です。 Aspnet を使用する方法についての\_regsql.exe メンバーシップ データベースを作成するを参照してください[ASP.NET SQL Server の登録ツール (aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx)です。 メンバーシップ データベースの展開の一般的なガイダンスについては、次を参照してください。[する方法: ASP.NET メンバーシップ データベースなしを含むユーザー アカウントを展開](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx)です。

>[!div class="step-by-step"]
[前へ](deploying-database-role-memberships-to-test-environments.md)
[次へ](excluding-files-and-folders-from-deployment.md)
