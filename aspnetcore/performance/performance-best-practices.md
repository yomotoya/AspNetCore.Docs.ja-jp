---
title: ASP.NET Core のパフォーマンスに関するベスト プラクティス
author: mjrousos
description: ASP.NET Core アプリでのパフォーマンスが向上し、一般的なパフォーマンスの問題を回避するためのヒント。
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.date: 11/29/2018
uid: performance/performance-best-practices
ms.openlocfilehash: 9f3ed97bf4d4eb371ff5ae3874234b44745cc4ca
ms.sourcegitcommit: 0fc89b80bb1952852ecbcf3c5c156459b02a6ceb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/29/2018
ms.locfileid: "52618117"
---
# <a name="aspnet-core-performance-best-practices"></a>ASP.NET Core のパフォーマンスに関するベスト プラクティス

によって[Mike Rousos](https://github.com/mjrousos)

このトピックでは、ASP.NET Core でのベスト プラクティス、パフォーマンスのガイドラインを示します。

<a name="hot"></a>
<!-- TODO review hot code paths is jargon that won't MT (machine translate) and is not well defined for native speakers. --> このドキュメントでは、ホット コード パスが頻繁に呼び出され、実行時間の大部分が発生したコード パスとして定義されます。 通常、ホット コード パスは、アプリのスケール アウトとパフォーマンスを制限します。

## <a name="cache-aggressively"></a>積極的にキャッシュします。

キャッシュは、このドキュメントの複数の部分で説明します。 詳細については、「 <xref:performance/caching/response> 」を参照してください。

## <a name="avoid-blocking-calls"></a>呼び出しがブロックされないように

ASP.NET Core アプリを設計すると、同時に多数の要求を処理する必要があります。 非同期 Api を使用すると、呼び出しをブロックしていない待機して何千もの同時要求を処理するスレッドの小さなプール。 実行時間の長い同期タスクが完了するを待機しているのではなく、スレッドは、別の要求を処理できます。

ASP.NET Core アプリで一般的なパフォーマンスの問題は、非同期可能性がある呼び出しをブロックしています。 多くの同期のブロッキング呼び出しにより[スレッド プールの枯渇](https://blogs.msdn.microsoft.com/vancem/2018/10/16/diagnosing-net-core-threadpool-starvation-with-perfview-why-my-service-is-not-saturating-all-cores-or-seems-to-stall/)応答時間の低下とします。

**しない**:

* 呼び出すことによって、非同期実行をブロック[Task.Wait](/dotnet/api/system.threading.tasks.task.wait)または[Task.Result](/dotnet/api/system.threading.tasks.task-1.result)します。
* 一般的なコード パスでロックを取得します。 ASP.NET Core アプリでは、最も効率的なコードを並列で実行するように構築する場合です。

**行う**

* ように[ホット コード パス](#hot)非同期です。
* データ アクセスと実行時間の長い操作の Api を非同期的に呼び出します。
* コント ローラー/Razor ページのアクションを非同期にします。 呼び出し履歴全体がメリットを享受するために非同期にする必要があります[非同期/待機](/dotnet/csharp/programming-guide/concepts/async/)パターン。

プロファイラーのような[PerfView](https://github.com/Microsoft/perfview)に頻繁に追加されているスレッドの検索に使用することができます、[スレッド プール](/windows/desktop/procthread/thread-pool)します。 `Microsoft-Windows-DotNETRuntime/ThreadPoolWorkerThread/Start`イベントがスレッド プールに追加されているスレッドを示します。 <!--  For more information, see [async guidance docs](TBD-Link_To_Davifowl_Doc  -->

## <a name="minimize-large-object-allocations"></a>ラージ オブジェクトの割り当てを最小限に抑える

<!-- TODO review Bill - replaced original .NET language below with .NET Core since this targets .NET Core --> [.NET Core のガベージ コレクター](/dotnet/standard/garbage-collection/) ASP.NET Core アプリで自動的にメモリの割り当てと解放を管理します。 一般に、自動ガベージ コレクションは、開発者がメモリを解放する方法やタイミングについて心配する必要があることを意味します。 ただし、未参照のオブジェクトのクリーンアップ時間がかかる CPU のため開発者は内のオブジェクトの割り当てを最小限に抑えて[ホット コード パス](#hot)します。 ガベージ コレクションは、ラージ オブジェクト (> 85 K バイト) では特に高価です。 大きなオブジェクトが格納されている、[大きなオブジェクト ヒープ](/dotnet/standard/garbage-collection/large-object-heap)完全 (第 2 世代) を必要とガベージ コレクションをクリーンアップします。 ジェネレーション 0 およびジェネレーション 1 のガベージ コレクションとは異なり、ジェネレーション 2 のコレクションにはアプリの実行を一時的に中断する必要があります。 頻繁に割り当てと大きなオブジェクトの割り当てを解除することによってパフォーマンスの一貫性のない可能性があります。

推奨事項:

* **行う** よく使われる大規模なオブジェクトのキャッシュを検討してください。 ラージ オブジェクトをキャッシュには、高価な割り当てができないようにします。
* **行う** を使用してバッファーをプールする[ `ArrayPool<T>` ](/dotnet/api/system.buffers.arraypool-1)大きな配列を格納します。
* **しない**に割り当てる大きなオブジェクトの有効期間が短い、[ホット コード パス](#hot)します。

メモリの問題でガベージ コレクション (GC) の統計情報を確認して、上記を診断できるように[PerfView](https://github.com/Microsoft/perfview)を調べること。

* ガベージ コレクションの一時停止時間。
* プロセッサ時間の割合は、ガベージ コレクションに費やされました。
* ガベージ コレクションの数とは、世代 0、1、および 2 です。

詳細については、次を参照してください。[ガベージ コレクションとパフォーマンス](/dotnet/standard/garbage-collection/performance)します。

## <a name="optimize-data-access"></a>データ アクセスを最適化します。

データ ストアまたは他のリモート サービスとのやり取りは、ASP.NET Core アプリの最も低速な部分ではよくあります。 データの読み書きを効率的には、良好なパフォーマンスにとって重要です。

推奨事項:

* **行う** すべてのデータ アクセス Api を非同期的に呼び出します。
* **しない**は必要以上のデータを取得します。 現在の HTTP 要求のために必要なデータだけを返すクエリを記述します。
* **** が若干古くなっているデータの許容される場合は、データベースやリモート サービスから取得されたデータをアクセス頻繁にキャッシュを検討してください。 シナリオによっては、使用する場合があります、 [MemoryCache](xref:performance/caching/memory)または[DistributedCache](xref:performance/caching/distributed)します。 詳細については、「 <xref:performance/caching/response> 」を参照してください。
* 最小限のネットワーク ラウンド トリップします。 目標は、いくつかの呼び出しではなく、1 回の呼び出しで必要なすべてのデータを取得します。
* **行う** 使用[追跡なしのクエリ](/ef/core/querying/tracking#no-tracking-queries)読み取り専用の目的でデータにアクセスするときに、Entity Framework Core でします。 EF Core より効率的に追跡なしのクエリの結果を返すことができます。
* **行う** フィルターと集計の LINQ クエリ (と`.Where`、 `.Select`、または`.Sum`ステートメントなどの) データベースをフィルター処理ができるようにします。
* **行う** EF Core に、クライアントは、非効率的なクエリの実行につながる可能性のいくつかのクエリ演算子が解決されることを検討してください。 詳細については、次を参照してください[クライアント評価のパフォーマンスの問題。](/ef/core/querying/client-eval#client-evaluation-performance-issues)
* **しない**プロジェクション クエリを使用して、"n+1"を実行するがこのコレクションは、上の SQL クエリ。 詳細については、次を参照してください。[相関サブクエリの最適化](/ef/core/what-is-new/ef-core-2.1#optimization-of-correlated-subqueries)します。

参照してください[EF 高性能](/ef/core/what-is-new/ef-core-2.0#explicitly-compiled-queries)のための手法を高スケールのアプリでのパフォーマンスを向上させる可能性があります。

* [DbContext プール](/ef/core/what-is-new/ef-core-2.0#dbcontext-pooling)
* [明示的にコンパイルされたクエリ](/ef/core/what-is-new/ef-core-2.0#explicitly-compiled-queries)

コード ベースをコミットする前に、前の高性能なアプローチの影響を測定することをお勧めします。 コンパイル済みクエリの複雑さを増加は、パフォーマンスの向上を見合わない場合があります。

クエリの時間を確認して問題を検出できますがのデータへのアクセスに費やされた[Application Insights](/azure/application-insights/app-insights-overview)またはプロファイル ツール。 ほとんどのデータベースも利用できる統計をに関する頻繁に実行されるクエリ。

## <a name="pool-http-connections-with-httpclientfactory"></a>HttpClientFactory との接続をプール HTTP

[HttpClient](/dotnet/api/system.net.http.httpclient?view=netstandard-2.0)実装、`IDisposable`を再利用目的インターフェイス。 閉じられた`HttpClient`ソケットのままで開いているインスタンス、`TIME_WAIT`短時間の状態。 その結果、コード パスを作成して破棄する場合`HttpClient`オブジェクトが頻繁に使用される、アプリが使用可能なソケットを使い果たす可能性があります。 [HttpClientFactory](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)この問題をソリューションとしての ASP.NET Core 2.1 で導入されました。 パフォーマンスと信頼性を最適化するためにプールの HTTP 接続を処理します。

推奨事項:

* **行う**の作成し、破棄の`HttpClient`直接インスタンス化します。
* **行う** 使用[HttpClientFactory](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)を取得する`HttpClient`インスタンス。 詳細については、次を参照してください。[回復力のある HTTP 要求を実装するために使用 HttpClientFactory](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)します。

## <a name="keep-common-code-paths-fast"></a>高速の一般的なコード パスを維持します。

すべてのコードを高速、したいが、頻繁に呼び出されるコード パスは、最も重要な最適化します。

* アプリの要求処理パイプラインでミドルウェア コンポーネント、特にミドルウェアはパイプラインの早い段階で実行します。 これらのコンポーネントは、パフォーマンスに大きな影響を与えます。
* すべての要求または複数回要求ごとに実行されるコード。 たとえば、カスタム ログ、承認ハンドラー、または一時的なサービスの初期化にします。

推奨事項:

* **しない**実行時間の長いタスクでカスタムのミドルウェア コンポーネントを使用します。
* **行う** パフォーマンス プロファイリング ツールを使用して (ような[Visual Studio 診断ツール](/visualstudio/profiling/profiling-feature-tour)または[PerfView](https://github.com/Microsoft/perfview)) を識別する[ホット コード パス](#hot)します。

## <a name="complete-long-running-tasks-outside-of-http-requests"></a>HTTP 要求の外部で長時間タスクを完了します。

コント ローラーまたはページ モデルに必要なサービスを呼び出すと、HTTP 応答を返すことによって、ASP.NET Core アプリにほとんどの要求を処理できます。 要求実行時間の長いタスクに関連するいくつかの場合は、全体の要求-応答プロセスを非同期にすることをお勧めします。

推奨事項:

* **しない**実行時間の長いタスクが通常の HTTP 要求の処理の一部として完了するまで待ちます。
* **行う** での実行時間の長い要求の処理を検討してください[バック グラウンド サービス](/aspnet/core/fundamentals/host/hosted-services)またはアウト プロセスで、 [Azure 関数](/azure/azure-functions/)します。 作業のアウト プロセスの完了は、CPU を消費するタスクに特に有益です。
* **行う** などのリアルタイム通信オプションを使用して、 [SignalR](xref:signalr/introduction)クライアントを非同期的に通信します。

## <a name="minify-client-assets"></a>クライアントの資産を縮小します。

複雑なフロント エンドを使用した ASP.NET Core アプリは、多くの JavaScript、CSS、またはイメージ ファイルを頻繁に機能します。 により、初期読み込み要求のパフォーマンスを向上できます。

* 1 つに複数のファイルを結合するバンドル化します。
* ファイルのサイズを縮小する、縮小します。

推奨事項:

* **行う** で ASP.NET Core の使用[組み込みサポート](xref:client-side/bundling-and-minification)バンドルと縮小クライアント資産。
* **行う** などの他のサード パーティ製ツールを検討してください[Gulp](uid:client-side/bundling-and-minification#consume-bundleconfigjson-from-gulp)または[Webpack](https://webpack.js.org/)クライアント資産管理をより複雑なのです。

## <a name="use-the-latest-aspnet-core-release"></a>ASP.NET Core の最新のリリースを使用します。

ASP.NET の新しい各リリースには、パフォーマンスの向上が含まれています。 .NET Core と ASP.NET Core での最適化では、新しいバージョンが古いバージョンを上回ることを意味します。 .NET Core 2.1 にサポートが追加されてコンパイルされる正規表現との恩恵をたとえば、 [ `Span<T>`](https://msdn.microsoft.com/magazine/mt814808.aspx)します。 Http/2 のサポートを ASP.NET Core 2.2 を追加します。 パフォーマンスが優先度の場合は、ASP.NET Core の最新バージョンへのアップグレードを検討してください。

<!-- TODO review link and taking advantage of new [performance features](#TBD)
Maybe skip this TBD link as each version will have perf improvements -->

## <a name="minimize-exceptions"></a>例外を最小限に抑える

例外は、まれである必要があります。 スローして、例外のキャッチは、他のコード フロー パターンを基準と遅いです。 このために、通常のプログラム フローを制御する例外を使用する必要がありますされません。

推奨事項:

* **しない**使用スローまたはホット コード パスで特にの通常のプログラム フローの手段として例外をキャッチします。
* **行う** ロジックを検出して例外の原因となる条件を処理するアプリケーションに含めることができます。
* **行う** スローまたは異常なまたは予期しない条件の例外をキャッチします。

(Application Insights) のようなアプリの診断ツールは、パフォーマンスに影響を与えるアプリケーションの一般的な例外を識別するのに役立ちます。