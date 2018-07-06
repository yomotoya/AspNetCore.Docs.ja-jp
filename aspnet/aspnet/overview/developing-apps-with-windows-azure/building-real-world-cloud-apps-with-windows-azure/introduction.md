---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
title: Azure での実際のクラウド アプリの構築 |Microsoft Docs
author: MikeWasson
description: この電子書籍手法について説明、パターンに基づいた現実世界のクラウド ソリューションを構築します。 パターンが的にも開発プロセスに適用する.
ms.author: aspnetcontent
ms.date: 06/12/2014
ms.assetid: accfa16a-ab15-4c26-9ad4-babdc2a77d2e
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
msc.type: authoredcontent
ms.openlocfilehash: 11388b9c58d2d21c7a337a343c10d33c7257ccf1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840499"
---
<a name="building-real-world-cloud-apps-with-azure"></a>Azure での実際のクラウド アプリの構築
====================
によって[Mike Wasson](https://github.com/MikeWasson)、 [Rick Anderson](https://github.com/Rick-Anderson)、 [Tom Dykstra](https://github.com/tdykstra)

[ダウンロードその修正プロジェクト](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)または[電子書籍をダウンロード](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> この電子書籍手法について説明、パターンに基づいた現実世界のクラウド ソリューションを構築します。 パターンは、ほかのアーキテクチャとコーディングのプラクティス、開発プロセスに適用されます。
> 
> Scott Guthrie によって開発され、2013 年 6 月の Norwegian 開発者 Conference (NDC) で自分が配信プレゼンテーションに基づいて、コンテンツ ([パート 1](http://vimeo.com/68215538)、[パート 2](http://vimeo.com/68215602)) とで Microsoft Tech Ed オーストラリア2013 年 9 月 ([パート 1](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR324)、[パート 2](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR325))。 [その他の多く](more-patterns-and-guidance.md#acknowledgments)更新およびビデオから記述形式への移行中に、コンテンツを増強します。


## <a name="intended-audience"></a>対象読者

クラウドの開発に関心をクラウドに移行を検討またはクラウド開発に慣れていない開発者は最も重要な概念と認識する必要があるプラクティスの概要を簡潔ここで紹介します。 具体的な例については、各章は、詳細については、その他のリソースにリンクしています。 とは、概念が示されています。 例では、およびその他のリソースへのリンクは Microsoft のフレームワークとサービス向けですが、示されている原則は、他の web 開発フレームワークに適用して、クラウド環境も。

ヘルプはアイデアをここで成功に導くためにより、クラウドの開発は既にしている開発者があります。 系列の各章読み取れるいない、独立してを選択し、興味のトピックを選択できるように。

Scott Guthrie を監視するすべてのユーザー*構築現実世界の Cloud Apps with Azure*プレゼンテーションの詳細および更新された情報の詳細はこちらを希望しています。

<a id="patterns"></a>
## <a name="cloud-development-patterns"></a>クラウド開発パターン

この電子書籍では、13 の推奨クラウド開発のパターンについて説明します。 「パターン」は広い意味での作業を行うことをお勧めの意味はここで使用: 開発、設計、およびクラウド アプリをコーディングに着手する最適な方法です。 これらは、それらの操作を行う場合に役立てること「成功の pit に分類されます」が主要なパターンです。

- [すべてを自動化](automate-everything.md)します。

    - スクリプトを使用して、効率を最大化し、反復的なプロセスでエラーを最小限に抑えます。
    - チュートリアル: Azure の管理スクリプトです。
- [ソース管理](source-control.md)します。 

    - DevOps ワークフローを容易にするために、ソース管理では分岐構造を設定します。
    - デモ: は、スクリプトをソース コントロールに追加します。
    - デモ: は、ソース管理からの機密データを保持します。
    - チュートリアル: Visual Studio で Git を使用します。
- [継続的インテグレーションと配信](continuous-integration-and-continuous-delivery.md)します。 

    - 各ソース コントロール チェックイン配置ビルドを自動化します。
- [Web 開発のベスト プラクティス](web-development-best-practices.md)します。 

    - ステートレス web 層を保持します。
    - デモ: スケーリングと、Azure App Service で Web アプリで自動スケーリングします。
    - セッション状態を回避します。
    - CDN が利用できない場合は、フォールバック CDN を使用します。
    - 非同期プログラミング モデルを使用します。
    - ASP.NET MVC と Entity Framework でのデモ: 非同期です。
- [シングル サインオン](single-sign-on.md)します。 

    - Azure Active Directory を紹介します。
    - チュートリアル: Azure Active Directory を使用する ASP.NET アプリを作成します。
- [データ ストレージ オプション](data-storage-options.md)します。 

    - データ ストアの種類。
    - 適切なデータ ストアを選択する方法。
    - チュートリアル: Azure SQL Database。
- [データのパーティション分割戦略](data-partitioning-strategies.md)します。 

    - データをパーティション分割して、垂直、水平、またはその両方に、リレーショナル データベースのスケーリングを容易にします。
- [非構造化 blob storage](unstructured-blob-storage.md)します。 

    - Blob サービスを使用して、クラウド内のファイルを保存します。
    - デモ: Fix It アプリで blob ストレージを使用します。
- [障害から復旧する設計](design-to-survive-failures.md)します。 

    - エラーの種類。
    - エラーのスコープです。
    - についての Sla です。
- [監視とテレメトリ](monitoring-and-telemetry.md)します。 

    - なぜテレメトリ アプリを購入両方と、アプリをインストルメント化するコードを記述する必要があります。
    - Azure 用の New Relic のデモ:
    - デモ: は、修正、アプリをコードを記録します。
    - Fix It アプリのデモ: 依存関係挿入します。
    - Azure でのデモ: 組み込みのログ記録のサポート。
- [一時的な障害処理](transient-fault-handling.md)します。 

    - 一時的な障害の影響を緩和するのにには、スマート再試行/バックオフ ロジックを使用します。
    - デモ: 再試行/バックオフで Entity Framework 6 です。
- [分散キャッシュ](distributed-caching.md)します。 

    - スケーラビリティが向上し、分散キャッシュを使用してデータベースのトランザクション コストを削減します。
- [キューを中心とした作業パターン](queue-centric-work-pattern.md)します。 

    - 高可用性を有効にして、web とワーカーの層を疎結合することで、スケーラビリティが向上します。
    - Fix It アプリのデモ: Azure storage キュー。
- [他のクラウド アプリのパターンとガイダンス](more-patterns-and-guidance.md)します。
- [付録: Fix It サンプル アプリケーション](the-fix-it-sample-application.md)

    - 既知の問題
    - ベスト プラクティス
    - ダウンロード、ビルド、実行、およびデプロイする方法。

これらのパターンは、すべてのクラウド環境に適用されますが、について Microsoft テクノロジと Visual Studio、Team Foundation Service、ASP.NET、および Azure などのサービスに基づく例を使用して説明します。

この章の残りの部分では、Fix It サンプル アプリケーションとで Fix It アプリが実行されているクラウド環境を Azure App Service で Web アプリを紹介します。

<a id="fixit"></a>
## <a name="the-fix-it-sample-application"></a>It サンプル アプリケーションの修正

スクリーン ショットとコードの例では、この電子書籍のほとんどはアプリに基づいて、Fix It によって開発された[Scott Guthrie](https://weblogs.asp.net/scottgu/)を推奨されるクラウド アプリの開発パターンとプラクティスを示します。

![アプリのホーム ページを修正します。](introduction/_static/image1.png)

サンプル アプリは、チケット システムの簡単な作業項目です。 問題を解決する場合は、チケットと割り当て、人と他のユーザーにログインして割り当てられているチケットを参照してくださいをそれらに作成し、チケットのマーク付け、作業が終わったときに完了しました。

これは標準の Visual Studio web プロジェクトです。 ASP.NET MVC 上に構築し、SQL Server データベースを使用します。 IIS Express でローカルで実行できるし、クラウドで実行する Azure の Web サイトを展開できます。 フォーム認証と、ローカル データベースを使用して、または Google などのソーシャル プロバイダーを使用してログインすることができます。 (後でも紹介します Active Directory の組織アカウントでログインする方法。)

![ログイン ページ](introduction/_static/image2.png)

ログイン後にチケットを作成のユーザーに割り当てして修正する内容の画像をアップロードします。

![Fix It タスクを作成します。](introduction/_static/image3.png)

![修正タスクの作成](introduction/_static/image4.png)

完了したため、チケットの詳細を表示、およびアイテムを開封済みに割り当てられているチケットを参照してください、作成した作業項目の進行状況を追跡できます。

これは、機能の観点から非常に単純なアプリですが、このように何百万ものユーザーにスケールでき、データベースの障害と接続の終了などの回復性があることを構築する方法を確認します。 使用すると、簡単に開始し、アプリをより迅速かつ効率的に開発サイクルを反復処理するように自動でアジャイルな開発ワークフローを作成する方法を確認します。

<a id="waws"></a>
## <a name="web-apps-in-azure-app-service"></a>Azure App Service で web アプリ

Fix It のアプリケーションで使用するクラウド環境とは、Web サイトと呼ばれる Azure のサービスです。 このサービスは、ことは Vm を作成し、それらを更新された保持することがなく Azure で web アプリをホストする、インストールして構成する IIS などの方法です。Vm で、サイトをホストし、自動的にバックアップと回復、およびその他のサービスを提供します。 Web サイト サービスでは、ASP.NET、Node.js、PHP、および Python で動作します。 Visual Studio、Web Deploy、FTP、Git または TFS を使用して、非常に迅速にデプロイすることができます。 一般的には、デプロイを開始する時間と、インターネット経由で、更新が利用可能時間はわずか数秒です。 開始するにはすべて無料ですし、トラフィックの増加に応じてスケール アップできます。

バック グラウンドでは、Azure App Service で Web アプリは、多くのアーキテクチャのコンポーネントと、独自の Vm 上で IIS を使用して web サイトをホストする場合、自分で構築することになる機能を提供します。 1 つのコンポーネントは、自動的に IIS を構成し、多くの Vm で、サイトを実行するように、アプリケーションをインストールする展開のエンド ポイントです。

![展開サービス](introduction/_static/image5.png)

Web サイトにユーザーが、IIS Vm を直接ヒットはありませんに至るまで[Application Request Routing (ARR)](https://www.iis.net/downloads/microsoft/application-request-routing)ロード バランサー。 、サーバーとこれらを使用できますが、ここでの利点は、いるセットアップが自動的にします。 セッション アフィニティ、IIS では、キューの深さなどの要因が考慮に入れてスマート ヒューリスティックを使用して各 CPU 使用率機械の Vm にトラフィックを送信するホストする web サイト。

![ARR のロード バランサー](introduction/_static/image6.png)

マシンがダウンした場合 Azure に自動的に、ローテーションからプル、新しい VM インスタンスをスピンアップし、開始--すべてで、アプリケーションのダウンタイムなくの新しいインスタンスにトラフィックを誘導します。

![マシンの障害からの自動復旧](introduction/_static/image7.png)

このすべては自動的に実行します。 行う必要があるものは、web サイトを作成し、Windows PowerShell、Visual Studio、または Azure 管理ポータルを使用して、アプリケーションをデプロイするだけです。

迅速かつ簡単なチュートリアルについては Visual Studio で web アプリケーションを作成して、Azure の Web サイトにデプロイする方法を示す、次を参照してください。 [Azure と ASP.NET の概要](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)します。

<a id="summary"></a>
## <a name="summary"></a>まとめ

この概要は、この書籍が取り扱うトピック、サンプル アプリケーションのスクリーン ショットとクラウド環境を Azure App Service で Web アプリの概要のリストを提供しています。 アプリの開発でと、クラウド向けの優れたメリットの 1 つは、テスト環境を作成し、コードを展開するなどの反復的な開発タスクを自動化する簡単であります。 サブジェクトで実行されている方法、 [[次へ] の章](automate-everything.md)します。

## <a name="resources"></a>リソース

この章で説明したトピックの詳細については、次のリソースを参照してください。

ドキュメント:

- [Web アプリを Azure App Service で](https://azure.microsoft.com/services/app-service/web/)します。 Web アプリに関するドキュメントについては Azure ポータルのページ。
- [Web Apps、Cloud Services、および Vm: いつどれを使用するでしょうか。](https://azure.microsoft.com/documentation/articles/choose-web-site-cloud-service-vm/) WAWS この章で示すようには、Azure で web アプリを実行する 3 つの方法の 1 つにすぎません。 この記事では、3 つの方法の違いについて説明し、シナリオでは、適切などの 1 つを選択する方法に関するガイダンスを提供します。 Web サイトでは、ようなクラウド サービスは、Azure の PaaS 機能です。 Vm は、IaaS 機能です。 IaaS と PaaS の詳細については、次を参照してください。、[データ オプション](data-storage-options.md#paasiaas)」の章。

ビデオ:

- [Scott Guthrie は、手順 0 - 新機能、Azure のクラウド OS から開始しますか。](https://azure.microsoft.com/documentation/videos/what-is-the-cloud-os-scottgu/)
- [Web サイトのアーキテクチャ - Stefan schackow 共演](https://azure.microsoft.com/documentation/videos/why-azure-web-sites-plus-architecture/)します。
- [Azure の Web サイトの内部構造」Nir Mashkowski](https://channel9.msdn.com/Shows/Web+Camps+TV/Windows-Azure-Web-Sites-Internals-with-Nir-Mashkowski)します。

> [!div class="step-by-step"]
> [次へ](automate-everything.md)
