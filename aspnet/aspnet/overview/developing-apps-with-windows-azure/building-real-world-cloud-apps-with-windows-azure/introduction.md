---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
title: "Azure での実際のクラウド アプリの構築 |Microsoft ドキュメント"
author: MikeWasson
description: "この電子書籍を紹介パターン ベースのアプローチを実際のクラウド ソリューションを構築します。 パターンを適用するために、開発プロセスもとして、."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: accfa16a-ab15-4c26-9ad4-babdc2a77d2e
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
msc.type: authoredcontent
ms.openlocfilehash: 4de0b52e0b4ae7ce00e7b07bce2decfc5068964a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="building-real-world-cloud-apps-with-azure"></a>Azure での実際のクラウド アプリの構築
====================
によって[Mike Wasson](https://github.com/MikeWasson)、 [Rick Anderson](https://github.com/Rick-Anderson)、 [Tom Dykstra](https://github.com/tdykstra)

[ダウンロード修正プロジェクト](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)または[電子書籍をダウンロード](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> この電子書籍を紹介パターン ベースのアプローチを実際のクラウド ソリューションを構築します。 パターンは、アーキテクチャとコーディング方法だけでなく、開発プロセスに適用されます。
> 
> Scott guthrie で開発および提供したノルウェー語開発者会議 (NDC) に 2013 年 6 月のプレゼンテーションに基づいて、コンテンツ ([パート 1](http://vimeo.com/68215538)、[パート 2](http://vimeo.com/68215602))、および Microsoft テクニカル Ed オーストラリアで2013 年 9 月、([パート 1](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR324)、[パート 2](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR325))。 [その他の多く](more-patterns-and-guidance.md#acknowledgments)が更新され、書き込まれたフォームへのビデオから移行中に、コンテンツを補完します。


## <a name="intended-audience"></a>想定読者

クラウドへの移行を検討して、クラウド向けの開発に関する調べたいまたはクラウド開発に慣れていない開発者は、ここで、最も重要な概念とプラクティスを理解する必要があるの簡潔な概要で検出されます。 具体的な例については、各章にリンクの詳細については、その他のリソースと概念を示します。 例では、およびその他のリソースへのリンクは、Microsoft のフレームワークとサービスが示す原則は他の web 開発フレームワークに適用され、クラウド環境にもします。

アイデアは、ここに役立つように成功には、既にクラウド向けに開発する開発者があります。 系列の各章読み取れるされません個別に選択およびで関心のあるトピックを選択できるようです。

Scott Guthrie のマークされている人*現実世界クラウド アプリのビルドと Azure*プレゼンテーションの詳細および最新の情報が見つかりますをここで希望しています。

<a id="patterns"></a>
## <a name="cloud-development-patterns"></a>クラウド開発パターン

この電子書籍では、13 個の推奨クラウド開発するためのパターンについて説明します。 「パターン」を広くするという意味での作業を行うことをお勧めはここで使用します。 には、開発、設計、およびクラウド アプリをコーディングする最適な方法です。 これらは、それらの操作を行う場合に役立てること「の成功度の pit に分類」が主要なパターンです。

- [すべてを自動化](automate-everything.md)です。

    - スクリプトを使用して、効率を最大化し、反復的なプロセスでエラーを最小限に抑えます。
    - チュートリアル: Azure 管理スクリプトを作成します。
- [ソース管理](source-control.md)です。 

    - DevOps ワークフローを容易にするために、ソース管理での分岐構造を設定します。
    - デモ: は、スクリプトをソース管理に追加します。
    - デモ: は、ソース管理からの機密データを保持します。
    - デモ: は、Visual Studio での Git を使用します。
- [継続的インテグレーションと配信](continuous-integration-and-continuous-delivery.md)です。 

    - ビルドと展開を各ソース制御チェックで自動化できます。
- [Web 開発のベスト プラクティス](web-development-best-practices.md)です。 

    - ステートレス web 層を保持します。
    - デモ: はスケーリングと Azure App service Web アプリでの自動スケーリングします。
    - セッション状態を回避します。
    - CDN を使用します。
    - 非同期プログラミング モデルを使用します。
    - ASP.NET MVC および Entity Framework でのデモ: 非同期です。
- [シングル サインオン](single-sign-on.md)です。 

    - Azure Active Directory の概要です。
    - デモ: は、Azure Active Directory を使用する ASP.NET アプリケーションを作成します。
- [データ記憶域オプション](data-storage-options.md)です。 

    - データ ストアの種類です。
    - 適切なデータ ストアを選択する方法。
    - チュートリアル: Azure SQL データベースです。
- [データのパーティション分割戦略](data-partitioning-strategies.md)です。 

    - 垂直方向に、行方向にデータをパーティション分割またはその両方がリレーショナル データベースのスケーリングを容易になります。
- [非構造化 blob ストレージ](unstructured-blob-storage.md)です。 

    - Blob サービスを使用してクラウドにファイルを格納します。
    - デモ: blob ストレージ アプリを使用してください。
- [障害にも対処デザイン](design-to-survive-failures.md)です。 

    - 失敗の種類。
    - 障害のスコープです。
    - Understanding Sla です。
- [監視と遠隔測定](monitoring-and-telemetry.md)です。 

    - 理由テレメトリ アプリを購入両方と、アプリをインストルメント化するコードを記述する必要があります。
    - Azure の New Relic のデモ:
    - デモ: は、修正、アプリにコードを記録します。
    - 修正アプリでのデモ: 依存関係の挿入します。
    - Azure でのデモ: 組み込みのログ記録のサポート。
- [一時的なエラー処理](transient-fault-handling.md)です。 

    - 一時的な障害の影響を軽減するためにスマート再試行/バックオフ ロジックを使用します。
    - デモ: 再試行/バックオフ Entity Framework 6 でします。
- [分散キャッシュ](distributed-caching.md)です。 

    - スケーラビリティが向上し、分散キャッシュを使用してデータベースのトランザクション コストを削減します。
- [キューを中心とした作業パターン](queue-centric-work-pattern.md)です。 

    - 高可用性を有効にして、web およびワーカー層を疎結合、スケーラビリティが向上します。
    - 修正、アプリのデモ: Azure ストレージ キュー。
- [他のクラウド アプリのパターンとガイダンス](more-patterns-and-guidance.md)です。
- [付録: Fix It サンプル アプリケーション](the-fix-it-sample-application.md)

    - 既知の問題
    - ベスト プラクティス
    - ダウンロード、ビルド、実行、および配置する方法。

すべてのクラウド環境にこれらのパターンを適用しますが、について Microsoft のテクノロジおよび Visual Studio、Team Foundation Service、ASP.NET、および Azure などのサービスに基づく例を使用して説明します。

この章の残りの部分には、修正、サンプル アプリケーションとで、修正、アプリが実行されているクラウド環境を Azure App Service の Web アプリが導入されています。

<a id="fixit"></a>
## <a name="the-fix-it-sample-application"></a>これはサンプル アプリケーションの修正プログラム

スクリーン ショットとこの電子書籍に示すようにコード例のほとんどは、修正、アプリによって開発されたに基づきます[Scott Guthrie](https://weblogs.asp.net/scottgu/)に推奨されるクラウド アプリの開発パターンとプラクティスを紹介します。

![アプリのホーム ページを修正します。](introduction/_static/image1.png)

サンプル アプリは、チケット システム簡単な作業項目です。 問題を解決する場合、作成、チケットと割り当て、他のユーザーと他のユーザーにログインして、割り当てられているチケットを参照してくださいそれらにしてチケットを作業が行われるときに完了としてマークします。

これは、標準の Visual Studio web プロジェクトです。 ASP.NET MVC 上でビルドされ、SQL Server データベースを使用します。 IIS Express でローカルに実行できるし、クラウドで実行する Azure Web サイトに展開できます。 フォーム認証と、ローカル データベースを使用して、または Google などのソーシャル プロバイダーを使用してログインすることができます。 (後でも紹介 Active Directory の組織アカウントでログインする方法です。)

![ログイン ページ](introduction/_static/image2.png)

ログインしている後にチケットを作成、だれかに割り当てるして固定を取得する対象の画像をアップロードします。

![修正タスクを作成します。](introduction/_static/image3.png)

![作成されたタスクを修正します。](introduction/_static/image4.png)

作成した作業項目の進行状況を追跡できます、完了済みとして、チケットの詳細の表示、および記号の項目に割り当てられているチケットを参照してください。

これは、機能の観点から非常にシンプルなアプリが数百万のユーザーに拡張できるし、回復力のあるデータベースの障害と接続の終了などになりますできるようにそれをビルドする方法について説明します。 単純なを起動し、アプリをより迅速かつ効率的に開発サイクルの反復処理するようにすることにより、自動とアジャイル開発のワークフローを作成する方法も表示されます。

<a id="waws"></a>
## <a name="web-apps-in-azure-app-service"></a>Azure App Service の web アプリ

修正アプリケーション用に使用されるクラウド環境は、Web サイトと呼ばれる Azure のサービスです。 このサービスは、ことができ、更新しておきます。 Vm を作成しなくても Azure で web アプリをホストする、インストールをなど、IIS を構成する方法です。お、Vm では、サイトをホストして、バックアップと回復、およびその他のサービスを自動的に提供します。 Web サイト サービスは、ASP.NET、Node.js、PHP、および Python で動作します。 Visual Studio、Web Deploy、FTP、Git または TFS を使用した非常に迅速に展開することができます。 一般的には、インターネット経由で、更新プログラムが利用可能な時間と、デプロイを開始する時間の間はわずか数秒です。 開始するにはすべて無料ですし、トラフィックの増大に応じてスケール アップできます。

バック グラウンドでは、Azure App service Web Apps は、多くの場合は、独自の Vm 上で IIS を使用して web サイトをホストする予定がご自身でビルドする必要がありますのアーキテクチャのコンポーネントと機能を提供します。 1 つのコンポーネントは、自動的に IIS を構成し、多くの Vm では、サイトを実行するように、アプリケーションをインストールする展開のエンド ポイントです。

![展開サービス](introduction/_static/image5.png)

ユーザーが web サイトに達すると IIS Vm に直接ヒットがない、を通じて[Application Request Routing (ARR)](https://www.iis.net/downloads/microsoft/application-request-routing)ロード バランサーです。 独自のサーバーでこれらを使用できますが、利点は、こと、セットアップが完了するために自動的にします。 スマート ヒューリスティックはセッション アフィニティ、IIS では、キューの深さなどの要因を考慮を使用し、ごとの CPU 使用率マシンから Vm へのトラフィックを直接ホストする web サイトです。

![ARR のロード バランサー](introduction/_static/image6.png)

場合は、マシンがダウンしても、Azure に自動的にローテーションから取り出す、スピンアップする新しい VM インスタンスでは、か、--すべてアプリケーションのダウンタイムなしの新しいインスタンスにトラフィックの送信を開始します。

![コンピューターの障害からの自動復旧](introduction/_static/image7.png)

これはすべて自動的に行わします。 行うには必要なは、web サイトを作成し、Windows PowerShell、Visual Studio または Azure 管理ポータルを使用して、アプリケーションを展開します。

すばやくかつ簡単なチュートリアルは Visual Studio で web アプリケーションを作成し、Azure Web サイトを展開する方法を示す、次を参照してください。 [Azure と ASP.NET の概要](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)です。

<a id="summary"></a>
## <a name="summary"></a>まとめ

この概要は、書籍は説明トピックでは、サンプル アプリケーションのスクリーン ショットと Azure App Service のクラウド環境で Web アプリの概要の一覧を提供しています。 クラウドのアプリの開発の利点としてのいずれかとは簡単にテスト環境を作成し、コードを展開するなどの反復的な開発タスクを自動化します。 サブジェクトで実行されている方法、[次のチャプター](automate-everything.md)です。

## <a name="resources"></a>リソース

この章で説明したトピックの詳細については、次のリソースを参照してください。

ドキュメント:

- [Web アプリを Azure App Service で](https://azure.microsoft.com/services/app-service/web/)です。 Web アプリに関するドキュメントについては Azure ポータルのページです。
- [Web アプリ、クラウド サービス、および Vm: どれを使用するときにしますか?](https://azure.microsoft.com/documentation/articles/choose-web-site-cloud-service-vm/) WAWS この章で示すようには、Azure で web アプリを実行する 3 つの方法の 1 つです。 この記事では、3 つの方法の違いについて説明し、シナリオに合った適切なものを選択する方法のガイダンスを示します。 Web サイトのようにクラウド サービスは、Azure の PaaS 機能です。 Vm は、IaaS 機能です。 IaaS と PaaS の詳細については、次を参照してください。、[データ オプション](data-storage-options.md#paasiaas)章します。

ビデオ:

- [Scott Guthrie は、ステップ 0 では、Azure のクラウド OS から開始しますか。](https://azure.microsoft.com/documentation/videos/what-is-the-cloud-os-scottgu/)
- [Web サイト アーキテクチャ - Stefan Schackow](https://azure.microsoft.com/documentation/videos/why-azure-web-sites-plus-architecture/)です。
- [Nir Mashkowski と azure の Web サイトの内部構造](https://channel9.msdn.com/Shows/Web+Camps+TV/Windows-Azure-Web-Sites-Internals-with-Nir-Mashkowski)です。

>[!div class="step-by-step"]
[次へ](automate-everything.md)
