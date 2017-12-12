---
uid: web-api/overview/error-handling/web-api-global-error-handling
title: "グローバルのエラー処理 ASP.NET Web API 2 |Microsoft ドキュメント"
author: davidmatson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/03/2014
ms.topic: article
ms.assetid: bffd7863-f63b-4b23-a13c-372b5492e9fb
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/error-handling/web-api-global-error-handling
msc.type: authoredcontent
ms.openlocfilehash: d2bdf04b4da2a099f3a2af100b16682c68f946f2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="global-error-handling-in-aspnet-web-api-2"></a>グローバルなエラー ASP.NET Web API 2 の処理
====================
によって[David Matson](https://github.com/davidmatson)、 [Rick Anderson](https://github.com/Rick-Anderson)

今日はありません簡単な方法でログインするか、グローバルにエラーを処理する Web API です。 使用していくつかのハンドルされない例外を処理できる[例外フィルター](exception-handling.md)はさまざまな例外フィルターが処理できない場合がありますが、します。 例:

1. コント ローラーのコンス トラクターからスローされる例外。
2. メッセージ ハンドラーからスローされる例外。
3. ルーティング時にスローされる例外。
4. 応答のコンテンツのシリアル化中にスローされる例外。

ログインし、(可能な場合) を処理するシンプルで一貫した方法を提供するこれらの例外。 

エラー応答を送信することはできませんし、すべて行うには大文字と小文字が例外をログの場合、例外を処理するための 2 つの主要な場合があります。 後者の場合の例は、応答のコンテンツをストリーミングの途中で例外がスローされたときその場合に遅すぎますステータス コード、ヘッダー、およびコンテンツの一部が既に実行がネットワーク経由で単に接続を中止しましたのでために、新しい応答メッセージを送信します。 新しい応答メッセージを生成するためには、例外を処理することはできない場合でも、例外をログ記録がサポートされます。 エラーを検出できる場合、次に示すように、該当するエラー応答を返すことができますお。

[!code-csharp[Main](web-api-global-error-handling/samples/sample1.cs?highlight=6)]

### <a name="existing-options"></a>既存のオプション

加え[例外フィルター](exception-handling.md)、[メッセージ ハンドラー](../advanced/http-message-handlers.md) 、500 番台のすべての応答を観察する今日使用できますが、元のエラーに関するコンテキストを持っていないものとして、これらの応答で動作するが困難ですが、します。 メッセージ ハンドラーでは、いくつかのケースを処理できるに関する例外フィルターと同じ制限があります。Web API にはエラー状態をキャプチャするトレース インフラストラクチャはありますが、トレース インフラストラクチャは診断目的およびは not に設計されています。 または実稼働環境で実行するために適しています。 グローバル例外処理とログ記録は、実稼動中に実行でき、既存の監視ソリューションに接続するサービスをする必要があります (たとえば、 [ELMAH](https://code.google.com/p/elmah/) )。

### <a name="solution-overview"></a>ソリューションの概要

 2 つの新しいユーザー-置き換え可能なサービスを提供して[IExceptionLogger](../releases/whats-new-in-aspnet-web-api-21.md)と IExceptionHandler、ログに記録し、未処理の例外を処理します。 サービスは、次の 2 つの主な相違点と、非常に似ています。

1. サポートされていますが、複数の例外ロガー、1 つの例外ハンドラーのみを登録します。
2. 例外ロガー常に呼び出される、接続を中止しようとしている場合でもです。 例外ハンドラーの取得時のみ呼び出すまだを選択する応答メッセージを送信できませんでした。

両方のサービス例外が検出された時点からの関連情報を含む例外コンテキストへのアクセスを提供する、特に[HttpRequestMessage](https://msdn.microsoft.com/en-us/library/system.net.http.httprequestmessage(v=vs.110).aspx)、 [HttpRequestContext](https://msdn.microsoft.com/en-us/library/system.web.http.controllers.httprequestcontext(v=vs.118).aspx)では、例外と例外のソース (以下詳細) がスローされます。

### <a name="design-principles"></a>デザインの原則

1. **互換性に影響する変更なし**ものに影響するソリューションの重要な制約は、契約を入力するか重大な変更がなければ、マイナー リリースまたは動作をこの機能が追加されているためです。 この制約は、500 の応答に例外にすると、既存の catch ブロックの観点からを実行するよう一部のクリーンアップから除外します。 この追加のクリーンアップが後続のメジャー リリースのことをお勧めです。 これは重要な場合くださいにについて投票する[ASP.NET Web API ユーザー音声](http://aspnet.uservoice.com/forums/147201-asp-net-web-api/suggestions/5451321-add-flag-to-enable-iexceptionlogger-and-iexception)です。
2. **Web API との一貫性を維持する構築**Web API のフィルターのパイプラインは、特定のアクション、コント ローラー固有またはグローバル スコープでロジックを適用する際の柔軟性に横断的関心事に対応する優れた方法です。 フィルター、例外、フィルターなどのグローバル スコープで登録されている場合でも常にアクションとコント ローラーのコンテキストであります。 コントラクトに適したフィルター、例外フィルターもグローバルにスコープを持つものにいくつかの例外が、アクションまたはコント ローラーのコンテキストがありませんは、メッセージ ハンドラーからの例外など、状況を処理に適していないことを意味が存在します。 例外処理フィルターで利用可能になる柔軟なスコープを使用したい場合は、例外フィルターお必要があります。 コント ローラー コンテキストの外部での例外を処理する必要がある場合も必要として、別のコンストラクト (何か、コント ローラー コンテキストとアクション コンテキストの制限なし) の完全なグローバル エラー処理のためです。

### <a name="when-to-use"></a>使用する場合

- 例外ロガーは、Web API によってキャッチされたすべての未処理の例外を参照するソリューションです。
- 例外ハンドラーは、Web API によってキャッチされた未処理の例外へのすべての可能な応答をカスタマイズするためのソリューションです。
- 例外フィルターは、特定のアクションまたはコント ローラーに関連するサブセット未処理の例外を処理する最も簡単なソリューションです。

### <a name="service-details"></a>サービスの詳細情報

 例外ロガーとハンドラー サービス インターフェイスには、単純な非同期メソッドのそれぞれのコンテキストを取得します。 

[!code-csharp[Main](web-api-global-error-handling/samples/sample2.cs)]

 これらのインターフェイスの両方の基本クラスも提供します。 ログインするか、推奨される処理に必要なすべてのコア (同期または非同期) メソッドをオーバーライドする時間。 ログ記録、`ExceptionLogger`基底クラスはコア ログ記録のメソッドが呼び出されるようにのみ 1 回例外ごとに (後で反映される場合でもさらに、呼び出し履歴の最大および再度でキャッチされます)。 `ExceptionHandler`基底クラスが入れ子になったレガシを無視して、コール スタックの上部にある例外 catch ブロックにのみ、メソッドを処理コアを呼び出します。 (これらの基本クラスの簡素化されたバージョンは、後述の付録では) です。両方`IExceptionLogger`と`IExceptionHandler`経由で例外に関する情報が表示される、`ExceptionContext`です。

[!code-csharp[Main](web-api-global-error-handling/samples/sample3.cs)]

常に提供するために、フレームワークの例外ロガーまたは例外ハンドラーは、呼び出すとき、`Exception`と`Request`です。 単体テストを除いて常に提供する、`RequestContext`です。 提供することはほとんどありません、`ControllerContext`と`ActionContext`(例外フィルターの catch ブロックからの呼び出し) の場合のみです。 非常にまれを提供する、 `Response`(ときの途中で、応答を書き込もうとしている IIS 場合) でのみです。 これらのプロパティのいくつかありますので注意してください`null`をチェックするコンシューマーの責任です`null`例外クラスのメンバーにアクセスする前にします。`CatchBlock` catch ブロックが例外を説明を示す文字列です。 Catch ブロックの文字列は次のとおりです。

- HttpServer (SendAsync メソッド)
- HttpControllerDispatcher (SendAsync メソッド)
- HttpBatchHandler (SendAsync メソッド)
- IExceptionFilter (ExecuteAsync 例外フィルター パイプラインの ApiController の処理)
- OWIN ホスト:

    - (出力をバッファリング) 用 HttpMessageHandlerAdapter.BufferResponseContentAsync
    - (出力をストリーミング) 用 HttpMessageHandlerAdapter.CopyResponseContentAsync
- Web ホスト:

    - (出力をバッファリング) 用 HttpControllerHandler.WriteBufferedResponseContentAsync
    - (出力をストリーミング) 用 HttpControllerHandler.WriteStreamedResponseContentAsync
    - HttpControllerHandler.WriteErrorResponseContentAsync (のバッファーされた出力モードでのエラー回復エラー)

Catch ブロックの文字列のリストも静的読み取り専用プロパティを使用して使用できます。 (静的 ExceptionCatchBlocks には、コアの catch ブロックの文字列は以外の場合は 1 つ静的クラスの各 OWIN と web ホストの残りの部分が表示されます)。`IsTopLevelCatchBlock` 呼び出しスタックの一番上でのみ例外処理の推奨のパターンに従うにとって便利です。 入れ子になった catch ブロックで発生した任意の場所 500 の応答に例外を有効ではなく、例外ハンドラーにホストで認識されるに約されるまで反映されるまで例外ことができます。

加え、 `ExceptionContext`、ロガー取得完全経由で情報のもう 1 つのピース`ExceptionLoggerContext`:

[!code-csharp[Main](web-api-global-error-handling/samples/sample4.cs)]

2 番目のプロパティでは、 `CanBeHandled`、により、処理できない例外を識別するロガー。 ときに、接続は中止されますと新しい応答メッセージを送信できません、ロガーが呼び出されますが、ハンドラーが***いない***呼び出される、ロガーはこのプロパティからは、このシナリオを特定できます。

追加する、 `ExceptionContext`、ハンドラーが完全に設定できます 1 つの複数のプロパティを取得`ExceptionHandlerContext`例外を処理します。

[!code-csharp[Main](web-api-global-error-handling/samples/sample5.cs)]

例外ハンドラーを設定して例外を処理したことを示します、`Result`アクションの結果をプロパティ (たとえば、 [ExceptionResult](https://msdn.microsoft.com/en-us/library/system.web.http.results.exceptionresult(v=vs.118).aspx)、 [InternalServerErrorResult](https://msdn.microsoft.com/en-us/library/system.web.http.results.internalservererrorresult(v=vs.118).aspx)、 [StatusCodeResult](https://msdn.microsoft.com/en-us/library/system.web.http.results.statuscoderesult(v=vs.118).aspx)、またはカスタムの結果)。 場合、`Result`プロパティが null で、例外が処理されないと、元の例外が再度スローされます。

コール スタックの上部にある例外の特別な手順で API の呼び出し元に対して適切な応答をように作成しました。 例外は、ホストまで伝達する場合は、呼び出し元は死亡の黄色の画面を参照してくださいまたはその他のホストには、応答は通常、HTML と通常は適切な API エラー応答が提供されています。 このような場合は、null 以外の場合と、カスタムの例外ハンドラーが明示的に設定する場合にだけ、結果が開始されるバックアップを作成する`null`(未処理の) は、例外ホストに伝達します。 設定`Result`に`null`このような場合に利用できます 2 つのシナリオ。

1. OWIN には、カスタム例外処理前に、/外部 Web API を登録するミドルウェアを Web API がホストされています。
2. ローカル ブラウザー経由でのデバッグ、死亡の黄色の画面は未処理の例外の応答を実際には便利です。

例外ロガーと例外ハンドラーの両方で何もしませんロガーやハンドラー自体は、例外をスローした場合に回復します。 (例外を伝達、場合に、このページの下部にあるフィードバックを送信できるようにすること以外がある場合より適切な方法です。)例外ロガーやハンドラーのコントラクトは、それらにならないように伝達; 呼び出し元の例外それ以外の場合、例外はだけ、多くの場合、一番ホストに伝達する (ASP のようなエラーが HTML の発生しました。NET の黄色の画面) (JSON または XML を期待する API の呼び出し元に対して推奨されるオプションは通常ありません) をクライアントに送信されています。

## <a name="examples"></a>例

### <a name="tracing-exception-logger"></a>トレースの例外ロガー

例外データを送信する (Visual Studio でのデバッグ出力ウィンドウを含む) 構成済みのトレース ソースに以下の例外ロガー。

[!code-csharp[Main](web-api-global-error-handling/samples/sample6.cs)]

### <a name="custom-error-message-exception-handler"></a>カスタム エラー メッセージの例外ハンドラー

次は、サポートに連絡用電子メール アドレスを含む、クライアントへのカスタム エラー応答を生成します。

[!code-csharp[Main](web-api-global-error-handling/samples/sample7.cs)]

## <a name="registering-exception-filters"></a>例外フィルターを登録します。

「ASP.NET MVC 4 Web アプリケーション」のプロジェクト テンプレートを使用して、プロジェクトを作成する場合は、コードを記述して Web API 構成中、`WebApiConfig`クラスで、*アプリ/_Start*フォルダー。

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

## <a name="appendix-base-class-details"></a>付録: 基底クラスの詳細

[!code-csharp[Main](web-api-global-error-handling/samples/sample8.cs)]
