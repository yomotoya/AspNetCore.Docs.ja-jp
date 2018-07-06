---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling
title: Transient Fault Handling (Azure での実際のクラウド アプリの構築) |Microsoft Docs
author: MikeWasson
description: Azure 電子書籍で構築実世界クラウド アプリは、Scott Guthrie が開発したプレゼンテーションに基づいています。 13 のパターンとプラクティスを彼がについて説明しています.
ms.author: aspnetcontent
ms.date: 11/03/2015
ms.assetid: 7ead83bc-c08c-4b26-8617-00e07292e35c
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling
msc.type: authoredcontent
ms.openlocfilehash: e67f3106f060d52f90ba56d6684af64779009e39
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823379"
---
<a name="transient-fault-handling-building-real-world-cloud-apps-with-azure"></a>Transient Fault Handling (Azure での実際のクラウド アプリの構築)
====================
によって[Mike Wasson](https://github.com/MikeWasson)、 [Rick Anderson](https://github.com/Rick-Anderson)、 [Tom Dykstra](https://github.com/tdykstra)

[ダウンロードその修正プロジェクト](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)または[電子書籍をダウンロード](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **構築現実世界の Cloud Apps with Azure**電子書籍は Scott Guthrie が開発したプレゼンテーションに基づきます。 13 のパターンについて説明しするのに役立つプラクティスは、クラウドの web アプリの開発が成功します。 電子書籍の詳細については、次を参照してください。[第 1 章](introduction.md)します。


現実世界のクラウド アプリを設計するときは、一時的なサービスの中断を処理する方法について考慮する必要があることの 1 つです。 この問題は、そのため、ネットワーク接続や外部サービスに依存するため、クラウド アプリ内で一意に重要です。 自動修復は通常、ほとんどの故障を得られることし、お客様の不適切な経験では結果をインテリジェントに処理する準備済みでない場合。

## <a name="causes-of-transient-failures"></a>一時的な障害の原因

失敗したため、データベース接続を削除することがわかりますクラウド環境で定期的に発生します。 ユーザーにとっては、オンプレミス環境と比較して複数のロード バランサー、web サーバーおよびデータベース サーバーが物理的に直接接続があるために一部です。 また、マルチ テナント サービスに依存しているときは、サービスを使用する他の人がこのポートにヒット頻度の高いために、サービスの get が遅くなったり、タイムアウトへの呼び出しを確認します。 サービス意図的に調整する – 接続を拒否します – サービスの他のテナントに悪影響を防止するためや、それ以外の場合、サービスをあまり頻繁には、ヒットはユーザーがあります。

## <a name="use-smart-retryback-off-logic-to-mitigate-the-effect-of-transient-failures"></a>スマート再試行/バックオフ ロジックを使用して、一時的な障害の影響を緩和するには

例外がスローされると、お客様が使用できないか、エラー ページを表示する、代わりに、通常、一時的エラーを認識できる自動的にで、エラーが発生する操作を再試行よう努力して前に、成功した時間の長いなります。、 ほとんどの場合、操作が 2 つ目の試行で成功して、これまでに問題があったことに注意してくださいなった顧客せず、エラーから回復します。

スマートな再試行ロジックを実装するいくつかの方法はあります。

- Microsoft Patterns&amp;プラクティス グループには、 [Transient Fault Handling Application Block](https://msdn.microsoft.com/library/dn440719(v=pandp.60).aspx)はすべての (Entity Framework) ではなく SQL Database のアクセスに ADO.NET を使用している場合。 – クエリを再試行する回数の再試行ポリシーを設定するだけや、試行 – とラップのコマンドと待機時間を SQL がでコードを*を使用して*ブロックします。

    [!code-csharp[Main](transient-fault-handling/samples/sample1.cs)]

    TFH もサポートしています。 [Azure In-role Cache](https://msdn.microsoft.com/library/windowsazure/dn386103.aspx)と[Service Bus](https://azure.microsoft.com/services/service-bus/)します。
- Entity Framework を使用するときに通常使用していない直接 SQL 接続の場合は、このパターンとプラクティスのパッケージを使用することはできませんが、Entity Framework 6 フレームワークにこのような再試行ロジックを構築するため。 同様の方法で、再試行戦略を指定して、EF がデータベースにアクセスするときに戦略をどのように使用されます。

    派生したクラスを追加するすべてを Fix It アプリでこの機能を使用する*DbConfiguration*再試行ロジックを有効にします。

    [!code-csharp[Main](transient-fault-handling/samples/sample2.cs)]

    フレームワークは、通常は一時的なエラーとして識別する SQL データベースの例外のコードで、指数バックオフ遅延間隔、および 5 秒間の最大遅延時間を最大 3 回の操作を再試行する EF に指示します。 指数バックオフを各障害が発生した再試行後に待機してから再試行する前に、長い期間を意味します。 行の 3 つの試行が失敗した場合、例外がスローされます。 サーキット ブレーカーの詳細については、次のセクションでは、指数バックオフと再試行の数に制限する理由について説明します。

    Fix It アプリは、Blob と .NET ストレージ クライアント API が既に同じ種類のロジックを実装して、Azure Storage サービスを使用しているときに問題のようなことができます。 単に、再試行ポリシーを指定する、または場合は、既定の設定に満足したら必要もありません。

<a id="circuitbreakers"></a>
## <a name="circuit-breakers"></a>サーキット ブレーカー

何度も長い期間にわたって再試行する理由は、いくつか考えられます。

- 永続的に失敗した要求を再試行する多数のユーザーには、他のユーザーのエクスペリエンスが低下する可能性があります。 何百万人が繰り返しを行うすべての再試行要求の場合するは、IIS ディスパッチ キューを結び付けると、アプリがそれ以外の場合を処理できる正常に要求を処理するを防ぐでした。
- すべてのユーザーが再試行ありますサービス エラーのためが非常に多くの要求に入れられるを回復を開始時に大量に送られたサービスを取得します。
- エラーは調整のための制限を使用して、サービス時間帯がある場合は、再試行を続けないでしたそのウィンドウを移動し、続行する、調整が発生します。
- 表示するために web ページを待機してユーザーを使用するがあります。 作成するユーザーの待機が長すぎますよりを比較的簡単に忠告して後でもう一度お試しにいたずら可能性があります。

指数バックオフ、アプリケーションからサービスを取得できますの再試行の頻度を制限することによってこれらの問題の一部に対処します。 必要ですが、*サーキット ブレーカー*: つまり、アプリの再試行を停止し、次のいずれかなど、他の何らかのアクションは、しきい値を再試行で特定します。

- カスタム フォールバックします。 Reuters から株価を取得できない場合は、おそらくから取得できます、Bloomberg;または、データベースからデータを取得できない場合も取得できますキャッシュから。
- サイレントに失敗します。 サービスから必要なものがない場合、アプリのオール_オア_ナッシング、だけ null を返して、データを取得することはできません。 Fix It タスクを表示している Blob のサービスが応答しない場合は、イメージなしのタスクの詳細を表示できます。
- 高速に失敗します。 エラーを使用して、サービスの混雑を回避するためにユーザーを他のユーザーのサービスの中断、または 調整期間を拡張する要求を再試行してください。 わかりやすい「後でもう一度お試し」のメッセージを表示できます。

画一的な再試行ポリシーはありません。 複数回再試行し、同期 web アプリケーションでは、ユーザーが応答を待つよりも、非同期のバック グラウンド ワーカー プロセスに長い時間待つことができます。 待機できますなったリレーショナル データベース サービスの再試行の間はキャッシュ サービスの場合よりもします。 ここではいくつかのサンプルをお勧めしますが、番号が異なる場合がある方法の概要を把握するためのポリシーを再試行してください。 (「高速最初」は、最初の再試行までの待ち時間。

![再試行ポリシーのサンプル](transient-fault-handling/_static/image1.png)

SQL Database の再試行ポリシー ガイダンスについては、次を参照してください。[一過性の障害と SQL Database への接続エラーのトラブルシューティング](https://azure.microsoft.com/documentation/articles/sql-database-connectivity-issues/)します。

## <a name="summary"></a>まとめ

再試行/バックオフ戦略を高める一時的なエラー非表示を顧客にほとんどの場合と ADO.NET、Entity Framework、または、Azure を使用しているかどうかの戦略を実装する作業を最小限に抑えるに使用できるフレームワークを提供しますストレージ サービスです。

[[次へ] の章](distributed-caching.md)信頼性を使用して分散キャッシュやパフォーマンスを向上させる方法について説明します。

## <a name="resources"></a>リソース

詳細については、次のリソースを参照してください。

ドキュメント

- [Azure Cloud Services で大規模なサービスを設計するためのベスト プラクティス](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx)します。 Mark Simms、Michael Thomassy、ホワイト ペーパー。 フェール セーフ シリーズが操作方法の詳細になると似ています。 遠隔測定と診断のセクションを参照してください。
- [フェール セーフ: 回復力のあるクラウド アーキテクチャについて](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx)します。 Marc Mercuri、Ulrich Homann、Andrew Townhill してホワイト ペーパー。 フェール セーフ ビデオ シリーズの web ページ バージョンです。
- [Microsoft Patterns and Practices - Azure ガイダンス](https://msdn.microsoft.com/library/dn568099.aspx)します。 [再試行] を参照してください。 パターン、Scheduler Agent Supervisor パターン。
- [Azure SQL Database でのフォールト トレランス](https://blogs.msdn.com/b/windowsazure/archive/2012/07/30/fault-tolerance-in-windows-azure-sql-database.aspx)します。 Tony Petrossian によるブログ投稿です。
- [Entity Framework - 接続の回復/再試行ロジック](https://msdn.microsoft.com/data/dn456835)します。 使用して、transient fault handling Entity Framework 6 の機能をカスタマイズする方法。
- [接続復元性と、ASP.NET MVC アプリケーションで Entity Framework とコマンド傍受](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)します。 第 4 に 9 部のチュートリアル シリーズでは、SQL データベースの EF 6 接続の回復性機能を設定する方法を示します。

ビデオ

- [フェール セーフ: スケーラブルで回復力のあるクラウド サービスの構築](https://channel9.msdn.com/Series/FailSafe)します。 Ulrich Homann、Marc Mercuri、Mark Simms、9 部構成シリーズ。 Microsoft Customer ・ Advisory Team (CAT) の実際の顧客使用経験描画ストーリーと非常にアクセス可能な興味深い方法で高度な概念とアーキテクチャの原則を説明します。 エピソード 3 から始まる 40:55 でサーキット ブレーカーの説明を参照してください。
- [構築大きな: Azure のお客様 - パート II から学んだ教訓](https://channel9.msdn.com/Events/Build/2012/3-030)します。 一時的な障害の設計について、Mark Simms 説明は、処理、およびすべてのインストルメント化エラーします。

コード サンプル

- [クラウド サービス Azure の基礎](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649)します。 サンプル アプリケーションを作成して、Microsoft Azure Customer Advisory Team を使用する方法については、 [Enterprise Library Transient Fault Handling Block](http://nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/) (TFH)。 詳細については、次を参照してください。[クラウド サービスの基本データ アクセス層 – 一時的な障害処理](https://social.technet.microsoft.com/wiki/contents/articles/18665.cloud-service-fundamentals-data-access-layer-transient-fault-handling.aspx)します。 ADO.NET を使用して (せずに直接 Entity Framework を使用して) データベースへのアクセスには、TFH を使用することをお勧めします。

> [!div class="step-by-step"]
> [前へ](monitoring-and-telemetry.md)
> [次へ](distributed-caching.md)
