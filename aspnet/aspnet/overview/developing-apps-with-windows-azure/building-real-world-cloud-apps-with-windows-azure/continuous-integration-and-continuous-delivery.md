---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery
title: 継続的インテグレーションと継続的デリバリー (Azure で現実世界のクラウド アプリの構築) |Microsoft Docs
author: MikeWasson
description: Azure 電子書籍で構築実世界クラウド アプリは、Scott Guthrie が開発したプレゼンテーションに基づいています。 13 のパターンとプラクティスを彼がについて説明しています.
ms.author: riande
ms.date: 06/12/2014
ms.assetid: eaece9f5-f80c-428b-b771-5db66d275b7d
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery
msc.type: authoredcontent
ms.openlocfilehash: 7a92a68ce8bbeec604a22e082975d33f9f3377c1
ms.sourcegitcommit: 57eccdea7d89a62989272f71aad655465f1c600a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/10/2018
ms.locfileid: "44340070"
---
<a name="continuous-integration-and-continuous-delivery-building-real-world-cloud-apps-with-azure"></a>継続的インテグレーションと継続的デリバリー (Azure で現実世界のクラウド アプリの構築)
====================
によって[Mike Wasson](https://github.com/MikeWasson)、 [Rick Anderson](https://github.com/Rick-Anderson)、 [Tom Dykstra](https://github.com/tdykstra)

[ダウンロードその修正プロジェクト](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)または[電子書籍をダウンロード](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **構築現実世界の Cloud Apps with Azure**電子書籍は Scott Guthrie が開発したプレゼンテーションに基づきます。 13 のパターンについて説明しするのに役立つプラクティスは、クラウドの web アプリの開発が成功します。 電子書籍の詳細については、次を参照してください。[第 1 章](introduction.md)します。


最初の 2 つは、開発プロセスのパターンが推奨される[自動化すべて](automate-everything.md)と[ソース管理](source-control.md)プロセスの 3 つ目のパターンは、これらを結合します。 継続的インテグレーション (CI) では、開発者が、ソース リポジトリにコードをチェックインするたびに、ビルドが自動的にトリガーを意味します。 継続的デリバリー (CD) は、この 1 つの手順をさらには: さらに詳細なテストを実行できる環境にアプリケーションを自動的に展開後、ビルド、自動化された単体テストが成功するとします。

クラウドを使用すると、のみ支払いが、環境リソースを使用している限りために、テスト環境を維持するためのコストを最小限に抑えることができます。 CD プロセスは、必要なし、完了すると、環境を行えるテスト環境を設定できますテストします。

## <a name="continuous-integration-and-continuous-delivery-workflow"></a>継続的インテグレーションと継続的デリバリーのワークフロー

通常、開発環境とステージング環境を継続的デリバリーを使用することをお勧めします。 Microsoft、でも、ほとんどのチームでは、運用環境のデプロイの手動のレビューおよび承認プロセスが必要です。 実稼働ことを確認する展開は、開発チームの主要な人物サポートについては、または低トラフィックの期間中に使用可能な場合に発生します。 しかしは受け入れテストの設定は何も変更と、環境をチェックインが開発者が行うように、開発およびテスト環境を完全に自動化することを防止するため。

次の図から[、Microsoft Patterns and Practices 電子書籍の継続的デリバリーについて](http://aka.ms/ReleasePipeline)一般的なワークフローを示しています。 元のコンテキストでのフル サイズを確認する画像をクリックします。

[![継続的デリバリーのワークフロー](continuous-integration-and-continuous-delivery/_static/image1.png)](https://msdn.microsoft.com/library/dn449955.aspx)

## <a name="how-the-cloud-enables-cost-effective-ci-and-cd"></a>クラウドでコスト効率に優れた CI と CD を使用する方法

Azure でこれらのプロセスを自動化することは簡単です。 クラウドですべてを実行している、ために、購入またはビルドまたはテスト環境のサーバーを管理する必要はありません。 サーバーで、テストの実行に使用するを待機する必要はありません。 実行するすべてのビルドは、automation スクリプトや実行の受け入れテストに対して、さらに詳細なテストを使用して Azure でのテスト環境を作成でき、しだけ完了すると破棄でした。 マシンが実際に実行されている時間に対してのみ支払うことしているため、料金が発生する必要がある money の量は最小限で、2 時間、8 時間、1 日にのみそのサーバーを実行する場合。 たとえば、環境に必要な修正プログラムは無料レベルから 1 つの階層を上がる場合基本的に 1 時間あたり約 1% のコストがアプリケーション。 1 か月の過程で、ずつのみ、1 時間の環境を実行した場合、テスト環境はおそらくよりも低コスト、ラテ スターバックスで購入できます。

## <a name="azure-devops-services"></a>Azure DevOps Services 

Azure DevOps サービスは、さまざまな展開を計画からのアプリケーション開発を支援する機能を提供します。

- (分散) Git と TFVC (集中型) のソース管理の両方をサポートします。
- 動的に必要になったときに、ビルド サーバーを作成し、ダウン完了したら、ユーザーのことを意味するエラスティック ビルド サービスを提供します。 ソース コードの変更をチェックインすると、割り当てが、ほとんどの時間がアイドル状態にある、独自のビルド サーバーの支払いをする必要はありません、ビルドを自動的に開始できます。 ビルド数を超えない限り、ビルド サービスは無料です。 大量のビルドを実行する場合は、予約済みのビルド サーバーのほとんどの余分なを支払うことができます。
- Azure への継続的デリバリーをサポートします。
- 自動化されたロード テストをサポートします。 ロード テスト、クラウド アプリに不可欠ですが手遅れになるまでは軽視されがちです。 数千のボトルネックを見つけるし、スループットを向上できるように、ユーザーによってアプリを頻繁に使用をシミュレートするロード テスト-運用環境にアプリをリリースする前にします。
- リアルタイム コミュニケーションと小規模なアジャイル チームのコラボレーションを容易にするチーム ルーム コラボレーションをサポートします。
- アジャイル プロジェクト管理をサポートします。


継続的インテグレーションと配信機能を Azure DevOps サービスの詳細については、次を参照してください。 [Azure DevOps ドキュメント](/azure/devops/index)します。

ターン キー プロジェクト管理、探している場合は、チーム コラボレーション、およびソース管理ソリューションでは、Azure DevOps サービスを確認します。 サインアップ[Azure DevOps サービス](https://dev.azure.com/)します。

## <a name="summary"></a>まとめ

最初の 3 つのクラウド開発パターン低サイクル タイムを反復可能な信頼性の高い、予測可能な開発プロセスを実装する方法に関するものでした。 [[次へ] の章](web-development-best-practices.md)アーキテクチャとコーディング パターンを見てまずします。

## <a name="resources"></a>リソース

詳細については、次を参照してください。 [Azure App Service で web アプリのデプロイ](https://azure.microsoft.com/documentation/articles/web-sites-deploy/)します。

次のリソースを参照してください。

- [Team Foundation Server 2012 によるリリース パイプラインを構築](http://aka.ms/ReleasePipeline)します。 電子書籍、実践的なラボ、およびサンプル コードでは、Microsoft Patterns and Practices、継続的デリバリーの詳細な概要について説明します。 Visual Studio Lab Management の Visual Studio Release Management の使用をについて説明します。
- [ALM Rangers の DevOps ツールとガイダンス](https://aka.ms/vsarsolutions/)します。 ALM Rangers は DevOps Workbench サンプル付属ソリューションと実用的なガイダンス、パターンとの共同作業で導入された&amp;プラクティスに関する書籍*TFS 2012 によるリリース パイプラインを構築*、開始する優れた方法としてDevOps の概念を理解&amp;Release Management for TFS 2012 とでぜひします。 ガイダンスは、複数の環境を 1 回ビルドしてデプロイする方法を示します。
- [Visual Studio 2012 での継続的デリバリーのテスト](https://msdn.microsoft.com/library/jj159345.aspx)です。 によって、Microsoft Patterns and Practices、電子書籍では、継続的デリバリーを使用した自動テストを統合する方法について説明します。
- [WindowsAzureDeploymentTracker](https://github.com/RyanTBerry/WindowsAzureDeploymentTracker)します。 (ラベルに基づく)、TFS からのビルドをキャプチャするためのツールのソース コードは、ビルド、パッケージ化しの特定の側面を構成する DevOps ロールで許可するユーザーが Azure にプッシュします。 ツールは、""バージョンにロールバックする前に配置する操作を有効にするには、展開プロセスを追跡します。 このツールは、外部の依存関係を持たないし、TFS の Api と Azure SDK を使用してスタンドアロン機能できます。
- [継続的デリバリー: ビルド、テスト、およびデプロイの自動化から信頼性の高いソフトウェア リリース](https://www.amazon.com/Continuous-Delivery-Deployment-Automation-Addison-Wesley/dp/0321601912/ref=sr_1_1?s=books&amp;ie=UTF8&amp;qid=1377126361)します。 書籍は Jez Humble、します。
- [それを解放します。実稼働可能なソフトウェアの展開の設計と](https://www.amazon.com/Release-It-Production-Ready-Pragmatic-Programmers/dp/0978739213)します。 Michael T. Nygard 著の「します。

> [!div class="step-by-step"]
> [前へ](source-control.md)
> [次へ](web-development-best-practices.md)
