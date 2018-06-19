---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery
title: 継続的インテグレーションと継続的な配信 (Azure と実際のクラウド アプリのビルド) |Microsoft ドキュメント
author: MikeWasson
description: Azure の電子書籍と構築実世界クラウド アプリは、Scott Guthrie が開発したプレゼンテーションに基づいています。 13 のパターンと彼をできるベスト プラクティスについて説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: eaece9f5-f80c-428b-b771-5db66d275b7d
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery
msc.type: authoredcontent
ms.openlocfilehash: 4d482aaa0d25d6e6baaf196df4b4bb9335408e46
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869414"
---
<a name="continuous-integration-and-continuous-delivery-building-real-world-cloud-apps-with-azure"></a>継続的インテグレーションと継続的な配信 (Azure と実際のクラウド アプリのビルド)
====================
によって[Mike Wasson](https://github.com/MikeWasson)、 [Rick Anderson](https://github.com/Rick-Anderson)、 [Tom Dykstra](https://github.com/tdykstra)

[ダウンロード修正プロジェクト](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)または[電子書籍をダウンロード](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **現実世界クラウド アプリのビルドと Azure**電子書籍は Scott Guthrie が開発したプレゼンテーションに基づいています。 13 パターンについて説明します、プラクティスするのに役立つ、クラウドの web アプリの開発が成功します。 電子書籍の詳細については、次を参照してください。[第 1 章](introduction.md)です。


最初の 2 つは、開発プロセスのパターンが推奨される[自動化すべて](automate-everything.md)と[ソース管理](source-control.md)プロセスの 3 つ目のパターンは、これらを結合します。 継続的インテグレーション (CI) は、開発者がソース リポジトリへのコードにチェックインされるたびに、ビルドは自動的にトリガーを意味します。 継続的な配信 (CD) はこの 1 つの手順をさらに、: 環境にアプリケーションをさらに詳細なテストを行うことができますを自動的に展開する、ビルド、自動化された単体テストが成功した後にします。

クラウドでは、支払えばよいためだけ環境のリソースを使用している限り、テスト環境を維持するためのコストを最小限にすることができます。 CD プロセスは、必要なし、を行う環境を完了すると、テスト環境を設定できますテストします。

## <a name="continuous-integration-and-continuous-delivery-workflow"></a>継続的インテグレーションと継続的デリバリーのワークフロー

通常、開発、ステージング環境への継続的な配信を行うことをお勧めします。 Microsoft では、ほとんどのチームでは、運用環境のデプロイが手動で確認および承認の処理が必要です。 実稼働のことを確認展開は、開発チームの主要な人物がトラフィックの少ない時間帯やサポートについては、使用可能な場合に発生します。 受け入れテストの設定を変更と、環境で確認を行うには、開発者はすべてができるように完全に開発およびテスト環境を自動化することを防ぐために何も行われません。

次の図から[、Microsoft Patterns and Practices 電子書籍した継続的配信の](http://aka.ms/ReleasePipeline)一般的なワークフローを示しています。 元のコンテキストでフル サイズを表示するイメージをクリックします。

[![継続的配信のワークフロー](continuous-integration-and-continuous-delivery/_static/image1.png)](https://msdn.microsoft.com/library/dn449955.aspx)

## <a name="how-the-cloud-enables-cost-effective-ci-and-cd"></a>費用効率の CI および CD、クラウドを使用する方法

Azure でこれらのプロセスを自動化することは簡単です。 クラウド内のすべてを実行している、ために購入またはビルドまたはテスト環境のサーバーを管理する必要はありません。 サーバーで、テストの実行に使用できるようにするに待機する必要はありません。 操作を行うすべてのビルドではでした、オートメーション スクリプトや実行受け入れテストに対して、さらに詳細なテストを使用して Azure でのテスト環境をスピンアップし、し終わったらだけに破棄します。 されのみに、2 時間または 8 時間または 1 日にそのサーバーを実行すると、それに対して料金を支払う必要がある金額が最小限に抑えるだけ料金を支払っている時間をマシンが実際に実行されているためです。 たとえば、環境に必要な修正プログラム アプリケーション基本的にコストは 1 時間あたり約 1 %free レベルから 1 つの階層を上がる場合。 月の過程で、のみ、時に 1 時間の環境を実行した場合、テスト環境が可能性がありますよりも低コストを購入することでした、ラテです。

## <a name="visual-studio-team-services-vsts"></a>Visual Studio Team Services (VSTS)

VSTS は、さまざまな展開の計画からのアプリケーションの開発に役立つ機能を提供します。

- (分散) Git と TFVC (中央) ソース管理の両方をサポートします。
- 動的になったときに、ビルド サーバーを作成し、終了とダウンは、エラスティック ビルド サービスを提供します。 ソース コードの変更がチェックインされたあり、ほとんどの時間アイドル状態にある独自のビルド サーバーの支払いを割り当てたりがない場合は、自動的にビルドを開始することができます。 ビルド数を超えていない限り、ビルド サービスは無料です。 大量のビルドを行う場合は、予約済みのビルド サーバーのほとんどの余分なを支払うことができます。
- これは、Azure に継続的な配信をサポートします。
- 自動化されたロード テストをサポートします。 ロード テスト クラウド アプリに不可欠ですには遅すぎますまで放置多くの場合は。 数千のボトルネックを検出およびスループットの向上を有効にすると、ユーザーによってアプリを頻繁に使用をシミュレートするロード テスト、実稼働環境にアプリをリリースする前にします。
- リアルタイム通信および小規模のアジャイル チームの共同作業を容易にする、チーム ルーム コラボレーションをサポートします。
- アジャイル プロジェクト管理がサポートしています。


継続的インテグレーションおよび VSTS の配信の機能の詳細については、次を参照してください。 [Visual Studio Team Services](https://www.visualstudio.com/team-services/)です。

ターンキー プロジェクト管理、探している場合は、チームのコラボレーション、およびソース管理ソリューションでは、チェック アウト VSTS します。 サービスは 5 つまでユーザーは、空き、時にサインアップすることができます[Visual Studio Team Services](https://www.visualstudio.com/team-services/)です。

## <a name="summary"></a>まとめ

最初の 3 つのクラウド開発パターンは、低のサイクル時間を反復可能な信頼性と予測可能な開発プロセスを実装する方法に関するされました。 [次のチャプター](web-development-best-practices.md)アーキテクチャとコーディング パターンを見てを開始します。

## <a name="resources"></a>リソース

詳細については、次を参照してください。 [Azure App service web アプリを配置](https://azure.microsoft.com/documentation/articles/web-sites-deploy/)です。

次のリソースを参照してください。

- [Team Foundation Server 2012 でリリース パイプラインの構築](http://aka.ms/ReleasePipeline)です。 電子書籍、実践的なラボ、およびサンプル コードでは、Microsoft Patterns and Practices、継続的デリバリーの詳細な概要について説明します。 Visual Studio Lab Management の Visual Studio リリース管理の使用をカバーします。
- [Tooling ALM Rangers DevOps とガイダンス](https://aka.ms/vsarsolutions/)です。 ALM Rangers 導入 DevOps Workbench サンプル コンパニオン ソリューションと、パターンと共同で実用的なガイダンス&amp;プラクティス book *TFS 2012 とするリリース パイプラインの構築*を起動する優れた手段としてDevOps の概念を理解&amp;試せるして TFS 2012 用のリリース管理します。 ガイダンスは、1 回ビルドおよび複数の環境に配置する方法を示します。
- [Visual Studio 2012 での継続的デリバリーのテスト](https://msdn.microsoft.com/library/jj159345.aspx)です。 Microsoft Patterns and Practices、によって電子書籍では、自動テストが継続的な配信を統合する方法について説明します。
- [WindowsAzureDeploymentTracker](https://github.com/RyanTBerry/WindowsAzureDeploymentTracker)です。 (ラベルに基づく)、TFS からのビルドをキャプチャするためのツールのソース コードは、ビルド、パッケージ化し化しの特定の側面を構成するのには、DevOps 役割で許可するユーザーを Azure にプッシュします。 ツールは、""バージョンにロールバック、以前に配置する操作を有効にするために、展開プロセスを追跡します。 ツールは、外部の依存関係がないと、TFS Api と Azure SDK を使用してスタンドアロン機能ことができます。
- [継続的に提供します。 信頼性の高いソフトウェアをビルド、テスト、および展開を自動化を通じて解放](https://www.amazon.com/Continuous-Delivery-Deployment-Automation-Addison-Wesley/dp/0321601912/ref=sr_1_1?s=books&amp;ie=UTF8&amp;qid=1377126361)です。 控えめな Jez によってブック。
- [それを解放します。設計および運用環境でソフトウェアの展開](https://www.amazon.com/Release-It-Production-Ready-Pragmatic-Programmers/dp/0978739213)です。 Michael T. Nygard によってブック。

> [!div class="step-by-step"]
> [前へ](source-control.md)
> [次へ](web-development-best-practices.md)
