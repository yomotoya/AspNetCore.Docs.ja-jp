---
uid: web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
title: ASP.NET Web API 2 でのトレース |Microsoft Docs
author: MikeWasson
description: ASP.NET Web API でのトレースを有効にする方法を示しています。
ms.author: riande
ms.date: 02/25/2014
ms.assetid: 66a837e9-600b-4b72-97a9-19804231c64a
msc.legacyurl: /web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 02805eda4f8dceb467547fa4e00aef8ea956f228
ms.sourcegitcommit: c684eb6c0999d11d19e15e65939e5c7f99ba47df
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/19/2018
ms.locfileid: "46292285"
---
<a name="tracing-in-aspnet-web-api-2"></a>ASP.NET Web API 2 でのトレース
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

> Web ベースのアプリケーションをデバッグするときは、適切な一連のトレース ログの代替はありません。 このチュートリアルでは、ASP.NET Web API でのトレースを有効にする方法を示します。 Web API フレームワークが、コント ローラーを呼び出す前後にはトレースには、この機能を使用することができます。 独自のコードをトレースに使用できます。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン
> 
> 
> - [Visual Studio 2017](https://www.visualstudio.com/downloads/) (Visual Studio 2015 でも機能します)
> - Web API 2
> - [Microsoft.AspNet.WebApi.Tracing](http://www.nuget.org/packages/Microsoft.AspNet.WebApi.Tracing)


## <a name="enable-systemdiagnostics-tracing-in-web-api"></a>System.Diagnostics Web API でのトレースを有効にします。

最初に新しい ASP.NET Web アプリケーション プロジェクトを作成します。 Visual Studio から、**ファイル**メニューの **新規**、し**プロジェクト**します。 **テンプレート**、 **Web**、 **ASP.NET Web アプリケーション**します。

[![](tracing-in-aspnet-web-api/_static/image2.png)](tracing-in-aspnet-web-api/_static/image1.png)

Web API プロジェクト テンプレートを選択します。

[![](tracing-in-aspnet-web-api/_static/image4.png)](tracing-in-aspnet-web-api/_static/image3.png)

**ツール**メニューの **ライブラリ パッケージ マネージャー**、し**Package Manage Console**します。

パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。

[!code-console[Main](tracing-in-aspnet-web-api/samples/sample1.cmd)]

最初のコマンドは、最新の Web API トレース パッケージをインストールします。 Core Web API のパッケージも更新されます。 2 番目のコマンドは、最新バージョンに WebApi.WebHost パッケージを更新します。

> [!NOTE]
> Web API の特定のバージョンを対象とする場合を使用して、バージョン フラグは、トレースのパッケージをインストールするときにします。


アプリで WebApiConfig.cs ファイルを開く\_開始フォルダー。 次のコードを追加、**登録**メソッド。

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample2.cs?highlight=6)]

このコードを追加、 [SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx)クラスを Web API パイプライン。 **SystemDiagnosticsTraceWriter**クラスへのトレースを書き込みます[System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace)します。

トレースを表示するには、デバッガーでアプリケーションを実行します。 ブラウザーに移動します。`/api/values`します。

![](tracing-in-aspnet-web-api/_static/image5.png)

トレース ステートメントは、Visual Studio の [出力] ウィンドウに書き込まれます。 (から、**ビュー**メニューの **出力**)。

[![](tracing-in-aspnet-web-api/_static/image7.png)](tracing-in-aspnet-web-api/_static/image6.png)

**SystemDiagnosticsTraceWriter**トレースを書き込みます**System.Diagnostics.Trace**、別のトレース リスナーを登録することができます。 たとえば、書き込むトレース ログ ファイルにします。 トレース ライターの詳細については、次を参照してください。、[トレース リスナー](https://msdn.microsoft.com/library/4y5y10s7.aspx) msdn トピック。

### <a name="configuring-systemdiagnosticstracewriter"></a>SystemDiagnosticsTraceWriter を構成します。

次のコードでは、トレース ライターを構成する方法を示します。

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample3.cs)]

制御可能な 2 つの設定があります。

- IsVerbose: false の場合、各トレースは最小限の情報を格納します。 True の場合、トレースには、詳細が含まれます。
- MinimumLevel: 最小トレース レベルを設定します。 順序でのトレース レベルは、デバッグ、Info、Warn、エラー、および致命的なエラー。

## <a name="adding-traces-to-your-web-api-application"></a>トレースを Web API アプリケーションに追加します。

トレース ライターを追加する即時にアクセス Web API パイプラインによって作成されたトレース。 独自のコードをトレースにトレース ライターを使用することもできます。

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample4.cs)]

トレース ライターを取得する**HttpConfiguration.Services.GetTraceWriter**します。 コント ローラーからこのメソッドはからアクセスできる、 **ApiController.Configuration**プロパティ。

トレースを書き込むには、呼び出すことができます、 **ITraceWriter.Trace**メソッドを直接が、 [ITraceWriterExtensions](https://msdn.microsoft.com/library/system.web.http.tracing.itracewriterextensions.aspx)クラスのわかりやすいは一部の拡張メソッドを定義します。 たとえば、**情報**前に示したメソッドは、トレース レベルでトレースを作成します**情報**します。

## <a name="web-api-tracing-infrastructure"></a>Web API トレース インフラストラクチャ

このセクションでは、Web API のカスタム トレース ライターを作成する方法について説明します。

Microsoft.AspNet.WebApi.Tracing パッケージは、Web API で一般的なトレース インフラストラクチャ上に構築されます。 Microsoft.AspNet.WebApi.Tracing を使用せずに組み込むことも可能で他のトレース/ログイン ライブラリなど[NLog](http://nlog-project.org/)または[log4net](http://logging.apache.org/log4net/)します。

トレースを収集するには、実装、 **ITraceWriter**インターフェイス。 簡単な例を次に示します。

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample5.cs)]

**ITraceWriter.Trace**メソッドは、トレースを作成します。 呼び出し元には、カテゴリおよびトレース レベルを指定します。 カテゴリには、任意のユーザー定義文字列を指定できます。 実装**トレース**次を実行する必要があります。

1. 新規作成**TraceRecord**します。 ように、要求、カテゴリ、およびトレースのレベルで初期化します。 これらの値は、呼び出し元によって提供されます。
2. 呼び出す、 *traceAction*を委任します。 残りの部分を埋めるために、このデリゲート内で、呼び出し元が期待どおり、 **TraceRecord**します。
3. 書き込み、 **TraceRecord**を任意のログ記録手法を使用します。 次に例を呼び出すだけです**System.Diagnostics.Trace**します。

## <a name="setting-the-trace-writer"></a>トレース ライターの設定

トレースを有効にするを使用する Web API を構成する必要があります、 **ITraceWriter**実装します。 これを行う、 **HttpConfiguration**オブジェクト、次のコードに示すようにします。

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample6.cs)]

1 つだけのトレース ライターはアクティブにすることはできます。 既定では、Web API の設定、&quot;は no-op&quot;は何もトレースします。 (、&quot;は no-op&quot;トレース コードがトレース ライターがあるかどうかを確認する必要がないように、トレースが存在する**null**トレースを書き込む前にします)。

## <a name="how-web-api-tracing-works"></a>Web API の動作をトレースの方法

Web API でのトレースを使用して、*ファサード*パターン: Web API トレースを有効にすると、トレースの呼び出しを実行するクラスを使用して、要求パイプラインのさまざまな部分が折り返されます。

たとえば、コント ローラーを選択するときに、パイプラインを使用して、 **IHttpControllerSelector**インターフェイス。 実装するクラスを挿入する、pipleline トレースが有効になっている、 **IHttpControllerSelector**が実際の実装を通じて呼び出し。

![Web API のトレースは、ファサード パターンを使用します。](tracing-in-aspnet-web-api/_static/image8.png)

この設計の利点は次のとおりです。

- トレース ライターを追加しない場合、トレーシング コンポーネントはインスタンス化されませんをパフォーマンスに与える影響はありません。
- など、既定のサービスを置き換える場合**IHttpControllerSelector**を独自のカスタム実装とトレースが影響を受けませんトレースはラッパー オブジェクトによって行われるためです。

既定値を置き換えることで、独自のカスタム フレームワークで Web API トレース フレームワーク全体を置換することもできます**ITraceManager**サービス。

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample7.cs)]

実装**ITraceManager.Initialize**トレース システムを初期化します。 これに置き換わることに注意してください、*全体*トレース フレームワーク、すべての Web API に組み込まれているトレース コードを含むです。
