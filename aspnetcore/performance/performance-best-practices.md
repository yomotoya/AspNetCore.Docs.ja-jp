---
title: ASP.NET Core のパフォーマンスに関するベスト プラクティス
author: mjrousos
description: ASP.NET Core アプリでのパフォーマンスが向上し、一般的なパフォーマンスの問題を回避するためのヒント。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 04/13/2019
uid: performance/performance-best-practices
ms.openlocfilehash: 28dc7fb40c1b60f643108dcb44593a08942a1650
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087501"
---
# <a name="aspnet-core-performance-best-practices"></a>ASP.NET Core のパフォーマンスに関するベスト プラクティス

によって[Mike Rousos](https://github.com/mjrousos)

このトピックでは、ASP.NET Core でのベスト プラクティス、パフォーマンスのガイドラインを示します。

<a name="hot"></a>

このドキュメントで、*ホット コード パス*は頻繁に呼び出され、実行時間の大部分が発生したコード パスとして定義されます。 通常、ホット コード パスは、アプリのスケール アウトとパフォーマンスを制限します。

## <a name="cache-aggressively"></a>積極的にキャッシュします。

キャッシュは、このドキュメントの複数の部分で説明します。 詳細については、「 <xref:performance/caching/response> 」を参照してください。

## <a name="avoid-blocking-calls"></a>呼び出しがブロックされないように

ASP.NET Core アプリを設計すると、同時に多数の要求を処理する必要があります。 非同期 Api を使用すると、呼び出しをブロックしていない待機して何千もの同時要求を処理するスレッドの小さなプール。 実行時間の長い同期タスクが完了するを待機しているのではなく、スレッドは、別の要求を処理できます。

ASP.NET Core アプリで一般的なパフォーマンスの問題は、非同期可能性がある呼び出しをブロックしています。 多くの同期ブロック呼び出しが発生する[スレッド プールの枯渇](https://blogs.msdn.microsoft.com/vancem/2018/10/16/diagnosing-net-core-threadpool-starvation-with-perfview-why-my-service-is-not-saturating-all-cores-or-seems-to-stall/)し、応答時間が低下します。

**しない**:

* 呼び出すことによって、非同期実行をブロック[Task.Wait](/dotnet/api/system.threading.tasks.task.wait)または[Task.Result](/dotnet/api/system.threading.tasks.task-1.result)します。
* 一般的なコード パスでロックを取得します。 ASP.NET Core アプリでは、最も効率的なコードを並列で実行するように構築する場合です。

**行う**

* ように[ホット コード パス](#hot)非同期です。
* データ アクセスと実行時間の長い操作の Api を非同期的に呼び出します。
* コント ローラー/Razor ページのアクションを非同期にします。 呼び出し履歴全体がメリットを享受するために非同期[非同期/待機](/dotnet/csharp/programming-guide/concepts/async/)パターン。

プロファイラーをなど[PerfView](https://github.com/Microsoft/perfview)、スレッドに頻繁に追加の検索に使用できる、[スレッド プール](/windows/desktop/procthread/thread-pools)します。 `Microsoft-Windows-DotNETRuntime/ThreadPoolWorkerThread/Start`イベントがスレッド プールに追加のスレッドを示します。 <!--  For more information, see [async guidance docs](TBD-Link_To_Davifowl_Doc  -->

## <a name="minimize-large-object-allocations"></a>ラージ オブジェクトの割り当てを最小限に抑える

<!-- TODO review Bill - replaced original .NET language below with .NET Core since this targets .NET Core -->
[.NET Core のガベージ コレクター](/dotnet/standard/garbage-collection/) ASP.NET Core アプリで自動的にメモリの割り当てと解放を管理します。 一般に、自動ガベージ コレクションは、開発者がメモリを解放する方法やタイミングについて心配する必要があることを意味します。 ただし、未参照のオブジェクトのクリーンアップ時間がかかる CPU のため開発者は内のオブジェクトの割り当てを最小限に抑えて[ホット コード パス](#hot)します。 ガベージ コレクションは、ラージ オブジェクト (> 85 K バイト) では特に高価です。 大きなオブジェクトが格納されている、[大きなオブジェクト ヒープ](/dotnet/standard/garbage-collection/large-object-heap)完全 (第 2 世代) を必要とガベージ コレクションをクリーンアップします。 ジェネレーション 0 およびジェネレーション 1 のガベージ コレクションとは異なり、ジェネレーション 2 のコレクションには、アプリの実行の一時中断が必要です。 頻繁に割り当てと大きなオブジェクトの割り当てを解除することによってパフォーマンスの一貫性のない可能性があります。

推奨事項:

* **行う** よく使われる大規模なオブジェクトのキャッシュを検討してください。 ラージ オブジェクトをキャッシュには、高価な割り当てができないようにします。
* **行う** を使用してバッファーをプールする[ `ArrayPool<T>` ](/dotnet/api/system.buffers.arraypool-1)大きな配列を格納します。
* **しない**に割り当てる大きなオブジェクトの有効期間が短い、[ホット コード パス](#hot)します。

ガベージ コレクション (GC) の統計情報を確認して、前など、メモリの問題を診断できます[PerfView](https://github.com/Microsoft/perfview)を調べること。

* ガベージ コレクションの一時停止時間。
* プロセッサ時間の割合は、ガベージ コレクションに費やされました。
* ガベージ コレクションの数とは、世代 0、1、および 2 です。

詳細については、[ガベージ コレクションとパフォーマンス](/dotnet/standard/garbage-collection/performance)を参照してください。

## <a name="optimize-data-access"></a>データ アクセスを最適化します。

データ ストアと他のリモート サービスとの対話は、ASP.NET Core アプリの最も遅い部分ではよくあります。 データの読み書きを効率的には、良好なパフォーマンスにとって重要です。

推奨事項:

* **行う** すべてのデータ アクセス Api を非同期的に呼び出します。
* **しない**は必要以上のデータを取得します。 現在の HTTP 要求のために必要なデータだけを返すクエリを記述します。
* **行う**若干古いデータが許容される場合は、データベースやリモート サービスから取得されたデータをアクセス頻繁にキャッシュを検討してください。 シナリオによっては、使用、 [MemoryCache](xref:performance/caching/memory)または[DistributedCache](xref:performance/caching/distributed)します。 詳細については、「 <xref:performance/caching/response> 」を参照してください。
* **行う**を最小限に抑えるネットワーク ラウンド トリップします。 目標は、いくつかの呼び出しではなく、1 回の呼び出しで必要なデータを取得します。
* **行う** 使用[追跡なしのクエリ](/ef/core/querying/tracking#no-tracking-queries)読み取り専用の目的でデータにアクセスするときに、Entity Framework Core でします。 EF Core より効率的に追跡なしのクエリの結果を返すことができます。
* **行う**フィルターと集計の LINQ クエリ (で`.Where`、 `.Select`、または`.Sum`ステートメントなどの)、フィルター処理がデータベースで実行できるようにします。
* **行う** EF Core に、クライアントは、非効率的なクエリの実行につながる可能性のいくつかのクエリ演算子が解決されることを検討してください。 詳細については、次を参照してください。[クライアント評価のパフォーマンスの問題](/ef/core/querying/client-eval#client-evaluation-performance-issues)します。
* **しない**プロジェクション クエリを使用して、"n+1"を実行するがこのコレクションは、上の SQL クエリ。 詳細については、[相関サブクエリの最適化](/ef/core/what-is-new/ef-core-2.1#optimization-of-correlated-subqueries)を参照してください。

参照してください[EF 高性能](/ef/core/what-is-new/ef-core-2.0#explicitly-compiled-queries)のための手法を高スケールのアプリでのパフォーマンスを向上させる可能性があります。

* [DbContext プール](/ef/core/what-is-new/ef-core-2.0#dbcontext-pooling)
* [明示的にコンパイルされたクエリ](/ef/core/what-is-new/ef-core-2.0#explicitly-compiled-queries)

コード ベースをコミットする前に、前の高性能なアプローチの影響を測定することをお勧めします。 コンパイル済みクエリの複雑さを増加は、パフォーマンスの向上を見合わない場合があります。

クエリの時間を確認して問題を検出できますがのデータへのアクセスに費やされた[Application Insights](/azure/application-insights/app-insights-overview)またはプロファイル ツール。 ほとんどのデータベースも利用できる統計をに関する頻繁に実行されるクエリ。

## <a name="pool-http-connections-with-httpclientfactory"></a>HttpClientFactory との接続をプール HTTP

[HttpClient](/dotnet/api/system.net.http.httpclient)実装、`IDisposable`インターフェイスを再利用するために設計されています。 閉じられた`HttpClient`ソケットのままで開いているインスタンス、`TIME_WAIT`短時間の状態。 コード パスを作成して破棄する場合、`HttpClient`オブジェクトが頻繁に使用される、アプリが使用可能なソケットを使い果たす可能性があります。 [HttpClientFactory](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)この問題をソリューションとしての ASP.NET Core 2.1 で導入されました。 パフォーマンスと信頼性を最適化するためにプールの HTTP 接続を処理します。

推奨事項:

* **行う**の作成し、破棄の`HttpClient`直接インスタンス化します。
* **行う** 使用[HttpClientFactory](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)を取得する`HttpClient`インスタンス。 詳細については、[回復力のある HTTP 要求を実装するために使用 HttpClientFactory](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)を参照してください。

## <a name="keep-common-code-paths-fast"></a>高速の一般的なコード パスを維持します。

高速、頻繁に呼び出されるコード パスをコードのすべてが最も重要な最適化するためにします。

* アプリの要求処理パイプラインでミドルウェア コンポーネント、特にミドルウェアはパイプラインの早い段階で実行します。 これらのコンポーネントは、パフォーマンスに大きな影響を与えます。
* すべての要求または複数回要求ごとに実行されるコード。 たとえば、カスタム ログ、承認ハンドラー、または一時的なサービスの初期化にします。

推奨事項:

* **しない**実行時間の長いタスクでカスタムのミドルウェア コンポーネントを使用します。
* **行う**パフォーマンス プロファイリング ツールなどを使用して、 [Visual Studio 診断ツール](/visualstudio/profiling/profiling-feature-tour)または[PerfView](https://github.com/Microsoft/perfview)) を識別[ホット コード パス](#hot)します。

## <a name="complete-long-running-tasks-outside-of-http-requests"></a>HTTP 要求の外部で長時間タスクを完了します。

コント ローラーまたはページ モデルに必要なサービスを呼び出すと、HTTP 応答を返すことによって、ASP.NET Core アプリにほとんどの要求を処理できます。 要求実行時間の長いタスクに関連するいくつかの場合は、全体の要求-応答プロセスを非同期にすることをお勧めします。

推奨事項:

* **しない**実行時間の長いタスクが通常の HTTP 要求の処理の一部として完了するまで待ちます。
* **行う** での実行時間の長い要求の処理を検討してください[バック グラウンド サービス](xref:fundamentals/host/hosted-services)またはアウト プロセスで、 [Azure 関数](/azure/azure-functions/)します。 作業のアウト プロセスの完了は、CPU を消費するタスクに特に有益です。
* **行う**などのリアルタイム通信のオプションを使用して[SignalR](xref:signalr/introduction)クライアントと非同期的に通信します。

## <a name="minify-client-assets"></a>クライアントの資産を縮小します。

複雑なフロント エンドを使用した ASP.NET Core アプリは、多くの JavaScript、CSS、またはイメージ ファイルを頻繁に機能します。 により、初期読み込み要求のパフォーマンスを向上できます。

* 1 つに複数のファイルを結合するバンドル化します。
* 空白とコメントを削除することでファイルのサイズを縮小する、縮小します。

推奨事項:

* **行う** で ASP.NET Core の使用[組み込みサポート](xref:client-side/bundling-and-minification)バンドルと縮小クライアント資産。
* **行う**などその他のサード パーティ製ツールを検討してください[Gulp](xref:client-side/using-gulp)または[Webpack](https://webpack.js.org/)の複雑なクライアント資産管理します。

## <a name="compress-responses"></a>応答を圧縮します。

 通常、応答のサイズを小さく、アプリの応答性を多くの場合、大幅に増加します。 ペイロードのサイズを小さく 1 つの方法は、アプリの応答を圧縮します。 詳細については、次を参照してください。[応答圧縮](xref:performance/response-compression)します。

## <a name="use-the-latest-aspnet-core-release"></a>ASP.NET Core の最新のリリースを使用します。

ASP.NET Core の新しい各リリースには、パフォーマンスの向上が含まれています。 .NET Core と ASP.NET Core での最適化では、新しいバージョンが一般に以前のバージョンを上回ることを意味します。 .NET Core 2.1 にサポートが追加されてコンパイルされる正規表現との恩恵をたとえば、 [ `Span<T>`](https://msdn.microsoft.com/magazine/mt814808.aspx)します。 Http/2 のサポートを ASP.NET Core 2.2 を追加します。 パフォーマンスが優先度の場合は、現在のバージョンの ASP.NET Core へのアップグレードを検討してください。

<!-- TODO review link and taking advantage of new [performance features](#TBD)
Maybe skip this TBD link as each version will have perf improvements -->

## <a name="minimize-exceptions"></a>例外を最小限に抑える

例外は、まれである必要があります。 スローして、例外のキャッチは、他のコード フロー パターンを基準と遅いです。 このため、通常のプログラム フローを制御する例外を使用しないでください。

推奨事項:

* **しない**スローまたは通常のプログラム フローの手段として特にで例外のキャッチ[ホット コード パス](#hot)します。
* **行う** ロジックを検出して例外の原因となる条件を処理するアプリケーションに含めることができます。
* **行う** スローまたは異常なまたは予期しない条件の例外をキャッチします。

Application Insights など、アプリの診断ツールは、パフォーマンスに影響するアプリの一般的な例外を識別するのに役立ちます。
