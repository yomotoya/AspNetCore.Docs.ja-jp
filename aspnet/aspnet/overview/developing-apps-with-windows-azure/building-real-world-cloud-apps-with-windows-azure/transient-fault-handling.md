---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling
title: "Transient Fault Handling (Azure での実際のクラウド アプリの構築) |Microsoft ドキュメント"
author: MikeWasson
description: "Azure の電子書籍と構築実世界クラウド アプリは、Scott Guthrie が開発したプレゼンテーションに基づいています。 13 のパターンと彼をできるベスト プラクティスについて説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/03/2015
ms.topic: article
ms.assetid: 7ead83bc-c08c-4b26-8617-00e07292e35c
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling
msc.type: authoredcontent
ms.openlocfilehash: b743b04789c5e5ebf5ab922cf34a516a16a6d356
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="transient-fault-handling-building-real-world-cloud-apps-with-azure"></a>Transient Fault Handling (Azure での実際のクラウド アプリの構築)
====================
によって[Mike Wasson](https://github.com/MikeWasson)、 [Rick Anderson](https://github.com/Rick-Anderson)、 [Tom Dykstra](https://github.com/tdykstra)

[ダウンロード修正プロジェクト](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)または[電子書籍をダウンロード](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **現実世界クラウド アプリのビルドと Azure**電子書籍は Scott Guthrie が開発したプレゼンテーションに基づいています。 13 パターンについて説明します、プラクティスするのに役立つ、クラウドの web アプリの開発が成功します。 電子書籍の詳細については、次を参照してください。[第 1 章](introduction.md)です。


現実世界のクラウド アプリを設計するときは、一時的なサービスの中断を処理する方法について検討する必要があるものの 1 つです。 この問題は、ネットワーク接続と外部サービスに依存するためであるためクラウド アプリに一意に重要です。 頻繁に自動修復は、通常、ほとんどの問題を取得することができ、お客様の不適切なエクスペリエンスに結果がそれらを適切に処理する準備済みでない場合。

## <a name="causes-of-transient-failures"></a>一時的な障害の原因

失敗し、データベース接続を削除することがわかりますクラウド環境に定期的に発生します。 皆さんは、内部設置型環境と比較して複数のロード バランサーを通じて、web サーバーおよびデータベース サーバーが物理的に直接接続があるために一部です。 また、マルチ テナント サービスに依存しているときに、ことに頻度の高いヒット サービスを使用している他のユーザーがあるために、サービスの取得が低速またはタイムアウトへの呼び出しが表示されます。 サービス意図的に調整する – 接続を拒否: サービスの他のテナントに悪影響を防止するためものでは、それ以外の場合、サービスの頻度が高すぎる、ヒットは、ユーザーがあります。

## <a name="use-smart-retryback-off-logic-to-mitigate-the-effect-of-transient-failures"></a>一時的な障害の影響を軽減するためにスマート再試行/バックオフ ロジックを使用します。

例外をスローして、顧客に使用できないか、エラー ページを表示する、代わりには通常、一時的エラーを認識できる自動的に、エラーが発生する操作を再試行してよう努力して長時間すると、正常に実行する前にします。、 ほとんどの場合、操作が 2 つ目の試行で成功して、これまで、問題があったことに注意してください済み顧客せず、エラーから回復します。

スマート再試行ロジックを実装するいくつかの方法はあります。

- Microsoft Patterns&amp;プラクティス グループには、 [Transient Fault Handling Application Block](https://msdn.microsoft.com/library/dn440719(v=pandp.60).aspx)はすべて (Entity Framework) ではなく SQL データベースへのアクセスに ADO.NET を使用している場合。 だけのクエリを再試行する回数: 再試行ポリシーを設定することもコマンドとする待機時間の試行 – とラップの間、SQL 内のコード、*を使用して*ブロックします。

    [!code-csharp[Main](transient-fault-handling/samples/sample1.cs)]

    TFH もサポートしています。 [Azure In-role Cache](https://msdn.microsoft.com/library/windowsazure/dn386103.aspx)と[Service Bus](https://azure.microsoft.com/services/service-bus/)です。
- Entity Framework を使用すると通常作業行っていない SQL 接続を直接パターンとベスト プラクティス、このパッケージを使用することはできませんが、Entity Framework 6 フレームワークにこのような再試行ロジックを構築するためです。 同様の方法で再試行戦略を指定して、EF はその方法を使用し、データベースにアクセスするたびにします。

    派生するクラスを追加する必要がありますすべては、修正アプリでこの機能を使用する*DbConfiguration*再試行ロジックを有効にします。

    [!code-csharp[Main](transient-fault-handling/samples/sample2.cs)]

    フレームワークを通常の一時的なエラーとして識別する SQL データベースの例外、コードは EF、指数バックオフ遅延間隔、および 5 秒の最大遅延時間で 3 回まで、操作を再試行するように指示します。 指数バックオフを障害が発生した再試行の間隔の後に待機してから再試行する前に、長い期間のことを意味します。 行の 3 つの試行が失敗した場合は、例外がスローされます。 サーキット ブレーカーに関する次のセクションでは、指数バックオフと再試行回数を制限する理由について説明します。

    Blob は、修正、アプリと .NET ストレージ クライアント API は、同様のロジックを既に実装して、Azure ストレージ サービスを使用しているときに、同様の問題を持つことができます。 再試行ポリシーをだけを指定または場合は、既定の設定に満足したら必要もありません。

<a id="circuitbreakers"></a>
## <a name="circuit-breakers"></a>サーキット ブレーカー

再試行回数が多すぎます期間が長すぎますしたくない理由は、いくつか考えられます。

- 永続的に失敗した要求を再試行して多数のユーザーには、他のユーザーのエクスペリエンスが低下する可能性があります。 何百万という人々 が繰り返し行うすべての要求の再試行が場合するは、IIS ディスパッチ キューを結び付けることと、アプリがそれ以外の場合を処理が正常に要求を処理するを妨げて可能性があります。
- すべてのユーザーは再試行があるサービス エラーのためでしたする非常に多くの要求キューに登録する場合、サービス復旧が開始されるとあふれたを取得します。
- 調整のため、エラーは、調整を使用してサービス時間帯がある場合は、継続的な再試行でしたそのウィンドウを除外し、制限の続行します。
- Web ページを待機してユーザーをレンダリングするがあります。 作成するユーザーの待機が長すぎますは、その比較的迅速に通知を後でもう一度試みることよりいたずら可能性があります。

指数バックオフ サービスは、アプリケーションから取得できます再試行の頻度を制限することによってこれらの問題の一部に対処します。 する必要しますが、ありますが*ブレーカー*: つまり、アプリの再試行を停止し、次のいずれかなど、他のいくつかのアクションを実行するしきい値を再試行で特定します。

- カスタム フォールバックします。 ロイターから株価できない場合は、状況によって異なることができますから取得して Bloomberg です。または、場合は、データベースからデータを取得することはできません、状況によって異なることができますから取得してキャッシュします。
- サイレント失敗します。 サービスから必要なものがない場合、アプリの 0/1、だけ null を返す場合、データを取得することはできません。 修正タスクを表示するし、Blob サービスは応答していません、イメージなしのタスクの詳細を表示できます。
- 高速に失敗します。 エラーが発生して、サービスがいっぱいにユーザーには、他のユーザーのサービスの中断を引き起こす可能性がありますまたは 調整期間を拡張する要求が再試行してください。 わかりやすい「後でもう一度やり直してください」のメッセージを表示することができます。

対応に関して万能の再試行ポリシーがありません。 複数回再試行し、同期の web アプリケーションでは、ユーザーが応答を待つ場合よりも非同期バック グラウンド ワーカー プロセスで現在待機できます。 待機できるなったリレーショナル データベース サービスの再試行の間キャッシュ サービスの場合よりもします。 ここでは、推奨されるいくつかのサンプルを把握する方法、番号が異なる場合がありますにポリシーを再試行してください。 (「高速先頭」を意味する最初の再試行までに遅延します。

![再試行ポリシーのサンプル](transient-fault-handling/_static/image1.png)

SQL データベースの再試行ポリシー ガイダンスについては、次を参照してください。[一時的なエラーと SQL データベースへの接続エラーのトラブルシューティングを行う](https://azure.microsoft.com/documentation/articles/sql-database-connectivity-issues/)です。

## <a name="summary"></a>まとめ

再試行/バックオフ戦略を高めるため一時的なエラー非表示を顧客にほとんどの場合、および Microsoft は、ADO.NET、Entity Framework または Azure を使用しているかどうかの戦略を実装する作業を最小化に使用できるフレームワークを提供しています記憶域サービス。

[次のチャプター](distributed-caching.md)パフォーマンスを向上させる方法を紹介し、信頼性を使用して分散キャッシュします。

## <a name="resources"></a>リソース

詳細については、次のリソースを参照してください。

ドキュメント

- [Azure クラウド サービスで大規模なサービスのデザインに関するヒント集](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx)です。 Mark Simms、Michael Thomassy、ホワイト ペーパー。 フェール セーフ系列の詳細の操作方法に関する詳細情報になるに似ています。 遠隔測定と診断のセクションを参照してください。
- [フェール セーフ: 回復力のあるクラウド アーキテクチャについて](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx)です。 Marc Mercuri、Ulrich Homann、Andrew Townhill、ホワイト ペーパー。 フェール セーフ ビデオ シリーズの web ページ バージョンです。
- [Microsoft Patterns and Practices - Azure ガイダンス](https://msdn.microsoft.com/library/dn568099.aspx)です。 [再試行] を参照してくださいパターン、スケジューラ エージェント スーパーバイザー パターン。
- [Azure SQL データベース内のフォールト トレランス](https://blogs.msdn.com/b/windowsazure/archive/2012/07/30/fault-tolerance-in-windows-azure-sql-database.aspx)です。 ブログ投稿 Tony Petrossian でします。
- [Entity Framework の接続の回復の再試行ロジック/](https://msdn.microsoft.com/data/dn456835)です。 使用し、transient fault handling Entity Framework 6 の機能をカスタマイズする方法。
- [接続の回復と Entity Framework、ASP.NET MVC アプリケーションでのコマンド インターセプト](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)です。 4 番目、9 つの部分から成るチュートリアル シリーズでは、SQL データベースの EF 6 接続の復元機能を設定する方法を示します。

ビデオ

- [フェール セーフ: スケーラブル、かつ回復力のクラウド サービスの構築](https://channel9.msdn.com/Series/FailSafe)です。 Ulrich Homann、Marc Mercuri、Mark Simms、系列を 9 つの部分で構成します。 実際のお客様と Microsoft Customer ・ Advisory Team (CAT) エクスペリエンスから抽出されたストーリーで非常にアクセス可能な興味深い方法で高度な概念とアーキテクチャの原則を説明します。 サーキット ブレーカー 40:55 始まるエピソード 3 の説明を参照してください。
- [構築 Big: Azure のお客様のパート II から学んだ教訓](https://channel9.msdn.com/Events/Build/2012/3-030)です。 Mark Simms が語るデザイン エラーが発生する一時的なエラーの処理、およびすべてのインストルメント化します。

コード サンプル

- [クラウド サービスの基礎 azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649)です。 サンプル アプリケーション、Microsoft Azure カスタマー アドバイス チームを使用する方法を示すによって作成された、[エンタープライズ ライブラリ Transient Fault Handling Block](http://nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/) (TFH)。 詳細については、次を参照してください。[クラウド サービスの基本データ アクセス層: 一時的な障害処理](https://social.technet.microsoft.com/wiki/contents/articles/18665.cloud-service-fundamentals-data-access-layer-transient-fault-handling.aspx)です。 ADO.NET を使用して直接 (使用せずに Entity Framework) データベースへのアクセスには、TFH を使用することをお勧めします。

>[!div class="step-by-step"]
[前へ](monitoring-and-telemetry.md)
[次へ](distributed-caching.md)
