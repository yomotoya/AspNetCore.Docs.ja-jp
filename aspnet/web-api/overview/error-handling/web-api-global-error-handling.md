---
uid: web-api/overview/error-handling/web-api-global-error-handling
title: ASP.NET Web API 2 のグローバル エラー処理 |Microsoft Docs
author: davidmatson
description: ''
ms.author: riande
ms.date: 02/03/2014
ms.assetid: bffd7863-f63b-4b23-a13c-372b5492e9fb
msc.legacyurl: /web-api/overview/error-handling/web-api-global-error-handling
msc.type: authoredcontent
ms.openlocfilehash: a52c2a1589327421b7f498ff551145676c80e3e8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832209"
---
<a name="global-error-handling-in-aspnet-web-api-2"></a>ASP.NET Web API 2 のグローバル エラー処理
====================
によって[David Matson](https://github.com/davidmatson)、 [Rick Anderson](https://github.com/Rick-Anderson)

今日は簡単な方法でログインするか、世界中にエラーを処理する Web API です。 使用して、いくつかのハンドルされない例外を処理できる[例外フィルター](exception-handling.md)が、さまざまな例外フィルターが処理できない場合があります。 例えば:

1. コント ローラーのコンス トラクターからスローされる例外。
2. メッセージ ハンドラーからスローされる例外。
3. ルーティング中にスローされる例外。
4. 応答コンテンツのシリアル化中にスローされる例外。

ログインし、処理可能な限りシンプルで一貫した方法を提供するこれらの例外。 

エラー応答を送信することがあり、すべてためには、ログ、例外の場合、例外を処理するための 2 つの主要な場合があります。 後者の場合の例は応答コンテンツのストリーミングの途中で例外がスローされたときにその例では遅すぎるため、状態コード、ヘッダー、およびコンテンツの一部が既に終了、ネットワーク経由で接続を単純に中止しましたので、新しい応答メッセージを送信します。 場合でも、新しい応答メッセージを生成するために、例外を処理できない場合、例外をログ記録を引き続きサポートします。 エラーを検出できる場合では、次に示すように、適切なエラー応答を返すできます。

[!code-csharp[Main](web-api-global-error-handling/samples/sample1.cs?highlight=6)]

### <a name="existing-options"></a>既存のオプション

ほかに[例外フィルター](exception-handling.md)、[メッセージ ハンドラー](../advanced/http-message-handlers.md) 500 番台のすべての応答を観察する今日使用できますが、元のエラーに関するコンテキストがないと、それらの応答で動作するが困難であり、します。 メッセージ ハンドラーでは、いくつかの例外フィルターに関する処理できるようにする場合と同じ制限もあります。Web API はエラー状態をキャプチャするトレース インフラストラクチャは、トレース インフラストラクチャは診断目的とはしないに設計されています。 または運用環境で実行するために最適な。 グローバルの例外処理とログ記録は、実稼動中に実行でき、既存の監視ソリューションに接続するサービスをする必要があります (たとえば、 [ELMAH](https://code.google.com/p/elmah/) )。

### <a name="solution-overview"></a>ソリューションの概要

 2 つの新しいユーザーによるサービス提供[IExceptionLogger](../releases/whats-new-in-aspnet-web-api-21.md) IExceptionHandler、ログインして、未処理の例外を処理するとします。 サービスは、2 つの主な相違点とよく似ています。

1. サポートされていますが、複数の例外ロガー、1 つの例外ハンドラーのみを登録します。
2. 例外ロガー常に呼び出される、接続を中止しようとしている場合でもです。 例外ハンドラーの取得時のみ呼び出す私たちに送信する応答メッセージを選択することも可能です。

両方のサービス例外が検出されたポイントからの関連情報を格納している例外コンテキストへのアクセスを提供する、特に[HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage(v=vs.110).aspx)、 [HttpRequestContext](https://msdn.microsoft.com/library/system.web.http.controllers.httprequestcontext(v=vs.118).aspx)、例外と例外のソース (以下の詳細) をスローします。

### <a name="design-principles"></a>設計の原則

1. **互換性に影響する変更なし**または動作を 1 つのソリューションに影響を与える重要な制約が存在しないように重大な変更は、契約を入力するいずれかをマイナー リリースでこの機能が追加されているためです。 この制約は、今回は 500 の応答に例外を有効にする既存の catch ブロックの観点からの作業が完了するいくつかのクリーンアップを候補から外されました。 この追加のクリーンアップは後続のメジャー リリースのことをお勧めします。 これは重要な場合については、「ご投票ください[ASP.NET Web API のユーザーの声](http://aspnet.uservoice.com/forums/147201-asp-net-web-api/suggestions/5451321-add-flag-to-enable-iexceptionlogger-and-iexception)します。
2. **Web API を使用した整合性の維持を構築します**Web API のフィルターのパイプラインは、特定のアクション、コント ローラーに固有またはグローバル スコープでロジックを適用する柔軟性を横断的問題を処理する優れた方法です。 フィルターなど、例外フィルター、フィルターでは、グローバル スコープで登録されている場合でも、常にアクションとコント ローラーのコンテキストがあります。 コントラクトに適したフィルターがいくつかの例外を処理、コンテキストなしでアクションまたはコント ローラーの場合は、メッセージ ハンドラーからの例外など、使用に適していますが、例外フィルターもグローバルにスコープを持つものにないことになります。 次のように存在します。 例外処理のためのフィルターによって、柔軟なスコープを使用する場合、例外フィルター必要があります。 必要がありますコント ローラー コンテキストの外部で例外の処理に必要な場合も別のコンストラクト (何か、コント ローラー コンテキストとアクション コンテキストの制限なし) の完全なグローバル エラー処理します。

### <a name="when-to-use"></a>使用する場合

- 例外ロガーは、Web API によってキャッチされたすべてのハンドルされない例外を表示するソリューションです。
- 例外ハンドラーは、Web API でキャッチされた未処理の例外をすべての可能な応答をカスタマイズするためのソリューションです。
- 例外フィルターは、特定のアクションまたはコント ローラーに関連するサブセットが未処理の例外を処理するための最も簡単なソリューションです。

### <a name="service-details"></a>サービスの詳細

 例外ロガーやハンドラーのサービス インターフェイスは、それぞれのコンテキストを受け取る単純な非同期メソッドです。 

[!code-csharp[Main](web-api-global-error-handling/samples/sample2.cs)]

 これらのインターフェイスの両方の基本クラスも指定します。 ログインするか、推奨される処理に必要なすべてがコア (同期または非同期) メソッドをオーバーライドする時間。 ログに記録、`ExceptionLogger`基底クラスのようになります、core のログ記録メソッドは、例外ごとに 1 回というのみが (場合でも、後で伝達さらに、コール スタックをセットアップおよび再度キャッチされます)。 `ExceptionHandler`基底クラスは、コア処理メソッドを入れ子になっているレガシを無視して、コール スタックの上部にある例外の catch ブロックにのみを呼び出します。 (これらの基本クラスの簡略化されたバージョンは、後述の付録では) です。両方`IExceptionLogger`と`IExceptionHandler`経由での例外に関する情報が表示される、`ExceptionContext`します。

[!code-csharp[Main](web-api-global-error-handling/samples/sample3.cs)]

常に提供するフレームワークが例外ロガーをまたは例外ハンドラーを呼び出すと、`Exception`と`Request`します。 単体テストを除いても常に提供する、`RequestContext`します。 提供することはほとんどありません、`ControllerContext`と`ActionContext`(例外フィルターの catch ブロックからの呼び出し) の場合のみです。 非常にまれを提供する、 `Response`(特定の IIS 場合のみ、応答を記述しようとしています。 中央ときに)。 これらのプロパティの一部になるので注意して`null`をチェックするコンシューマーの責任です`null`例外クラスのメンバーにアクセスする前にします。`CatchBlock` catch ブロックが例外を見たを示す文字列です。 Catch ブロックの文字列は次のとおりです。

- HttpServer (SendAsync メソッド)
- HttpControllerDispatcher (SendAsync メソッド)
- HttpBatchHandler (SendAsync メソッド)
- IExceptionFilter (ExecuteAsync の例外フィルター パイプラインの ApiController の処理)
- OWIN ホスト:

    - HttpMessageHandlerAdapter.BufferResponseContentAsync (の出力をバッファリング)
    - (ストリーミング出力) 用 HttpMessageHandlerAdapter.CopyResponseContentAsync
- Web ホスト:

    - HttpControllerHandler.WriteBufferedResponseContentAsync (の出力をバッファリング)
    - (ストリーミング出力) 用 HttpControllerHandler.WriteStreamedResponseContentAsync
    - HttpControllerHandler.WriteErrorResponseContentAsync (のバッファリングされている出力モードでのエラー回復エラー)

Catch ブロックの文字列のリストも静的読み取り専用プロパティを使用して使用できます。 (静的 ExceptionCatchBlocks はコアの catch ブロックの文字列。 1 つ静的クラスの各 OWIN と web ホストの残りの部分が表示されます)。`IsTopLevelCatchBlock` 呼び出しスタックの一番上にのみ例外処理の推奨パターンに従うに役立ちます。 500 の応答を入れ子になった catch ブロックが発生した任意の場所に例外を有効にするのではなく、例外ハンドラーに例外がホストで認識されるに約までに伝達することができます。

加え、 `ExceptionContext`、ロガー経由での完全な情報のもう 1 つを取得する`ExceptionLoggerContext`:

[!code-csharp[Main](web-api-global-error-handling/samples/sample4.cs)]

2 番目のプロパティでは、 `CanBeHandled`、処理できない例外を識別するためにロガーを使用します。 ときに、接続が中止されるし新しい応答メッセージを送信できません、ロガーが呼び出されますが、ハンドラーは***いない***が呼び出されると、ロガーこのプロパティからは、このシナリオを特定できます。

追加する、 `ExceptionContext`、ハンドラーが完全に設定できます 1 つの複数のプロパティを取得`ExceptionHandlerContext`例外を処理します。

[!code-csharp[Main](web-api-global-error-handling/samples/sample5.cs)]

例外ハンドラーを設定して例外を処理したことを示します、`Result`プロパティをアクションの結果 (たとえば、 [ExceptionResult](https://msdn.microsoft.com/library/system.web.http.results.exceptionresult(v=vs.118).aspx)、 [InternalServerErrorResult](https://msdn.microsoft.com/library/system.web.http.results.internalservererrorresult(v=vs.118).aspx)、 [StatusCodeResult](https://msdn.microsoft.com/library/system.web.http.results.statuscoderesult(v=vs.118).aspx)、またはカスタムの結果)。 場合、`Result`プロパティが null で、例外が処理されないと、元の例外が再度スローされます。

コール スタックの上部にある例外の場合は、応答が API の呼び出し元に適切なことを確認する追加の手順をしました。 例外は、ホストまで伝達する場合、呼び出し元の死の黄色い画面が表示または他のホストには、これは通常の HTML 応答と、通常は適切な API エラー応答が提供されています。 このような場合は、null 以外の場合、およびカスタム例外ハンドラーが明示的に設定されている場合にのみ、結果が開始のバックアップを`null`(未処理の) は、例外ホストに伝達します。 設定`Result`に`null`このような場合は 2 つのシナリオに適していることができます。

1. OWIN には、カスタム例外処理ミドルウェアを登録する前に/外部 Web API と Web API がホストされています。
2. ローカルの死の黄色い画面が実際に未処理の例外の便利な応答は、ブラウザー経由でデバッグします。

例外ロガーと例外ハンドラーの両方で何もしませんロガーやハンドラー自体が例外をスローする場合に回復します。 (例外の伝達で場合に、このページの下部でフィードバックを送信をそのまま使用できるようにすること以外があるのより優れたアプローチです。)例外ロガーやハンドラーのコントラクトは、例外が伝達されます。 呼び出し元が許可する必要がありますいないされました。それ以外の場合、例外はだけを反映する多くの場合、(ASP のような HTML エラーが発生するホストに至る。NET の黄色い画面) (これは、通常は、JSON または XML に予想される API の呼び出し元の推奨されるオプション)、クライアントに返送されます。

## <a name="examples"></a>使用例

### <a name="tracing-exception-logger"></a>トレースの例外ロガー

次の例外ロガーは、構成済みのトレース ソース (Visual Studio でのデバッグ出力ウィンドウを含む) に例外データを送信します。

[!code-csharp[Main](web-api-global-error-handling/samples/sample6.cs)]

### <a name="custom-error-message-exception-handler"></a>カスタム エラー メッセージの例外ハンドラー

次は、サポートに連絡用電子メール アドレスを含む、クライアントへのカスタム エラー応答を生成します。

[!code-csharp[Main](web-api-global-error-handling/samples/sample7.cs)]

## <a name="registering-exception-filters"></a>例外フィルターを登録します。

配置内で Web API の構成コードの「ASP.NET MVC 4 Web アプリケーション」プロジェクト テンプレートをプロジェクトの作成に使用する場合、`WebApiConfig`クラスで、*アプリ/開始 (_s)* フォルダー。

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

## <a name="appendix-base-class-details"></a>付録: 基底クラスの詳細

[!code-csharp[Main](web-api-global-error-handling/samples/sample8.cs)]
