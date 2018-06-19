---
uid: web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
title: ASP.NET Web API 2 のトレース |Microsoft ドキュメント
author: MikeWasson
description: ASP.NET Web API でのトレースを有効にする方法を示します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 66a837e9-600b-4b72-97a9-19804231c64a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 7392ae5d9bc4c3aab45a9373099a0ee18e873a4f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
ms.locfileid: "28044209"
---
<a name="tracing-in-aspnet-web-api-2"></a>ASP.NET Web API 2 のトレース
====================
によって[Mike Wasson](https://github.com/MikeWasson)

> Web ベース アプリケーションをデバッグするときに、トレース ログの適切なセットを代わりにはありません。 このチュートリアルでは、ASP.NET Web API でのトレースを有効にする方法を示します。 この機能を使用すると、どのような Web API フレームワークは、コント ローラーを呼び出す前後にトレースされます。 また、独自のコードをトレースに使用することができます。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されているソフトウェアのバージョン
> 
> 
> - [Visual Studio 2017](https://www.visualstudio.com/downloads/) (Visual Studio 2015 でも動作します)
> - Web API 2
> - [Microsoft.AspNet.WebApi.Tracing](http://www.nuget.org/packages/Microsoft.AspNet.WebApi.Tracing)


## <a name="enable-systemdiagnostics-tracing-in-web-api"></a>System.Diagnostics Web API のトレースを有効にします。

最初に、新しい ASP.NET Web アプリケーション プロジェクトを作成します。 Visual Studio から、**ファイル**メニューの **新規**、し**プロジェクト**です。 **テンプレート**、 **Web** **ASP.NET Web アプリケーション**です。

[![](tracing-in-aspnet-web-api/_static/image2.png)](tracing-in-aspnet-web-api/_static/image1.png)

Web API プロジェクト テンプレートを選択します。

[![](tracing-in-aspnet-web-api/_static/image4.png)](tracing-in-aspnet-web-api/_static/image3.png)

**ツール**メニューの **ライブラリ パッケージ マネージャー**、し**パッケージの管理コンソール**です。

パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。

[!code-console[Main](tracing-in-aspnet-web-api/samples/sample1.cmd)]

最初のコマンドは、最新の Web API トレース パッケージをインストールします。 コア Web API パッケージも更新します。 2 番目のコマンドは、最新バージョンに WebApi.WebHost パッケージを更新します。

> [!NOTE]
> Web API の特定のバージョンを対象とする場合は、バージョン フラグは、トレースのパッケージをインストールするときにします。


アプリで WebApiConfig.cs ファイルを開く\_開始フォルダーです。 次のコードを追加、**登録**メソッドです。

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample2.cs?highlight=6)]

このコードを追加、 [SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx)クラスが Web API パイプラインをします。 **SystemDiagnosticsTraceWriter**クラスをトレースに書き込みます[System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace)です。

トレースを表示するには、デバッガーでアプリケーションを実行します。 ブラウザーに移動`/api/values`です。

![](tracing-in-aspnet-web-api/_static/image5.png)

トレース ステートメントは、Visual Studio では、出力ウィンドウに書き込まれます。 (から、**ビュー**メニューの **出力**)。

[![](tracing-in-aspnet-web-api/_static/image7.png)](tracing-in-aspnet-web-api/_static/image6.png)

**SystemDiagnosticsTraceWriter**トレースを書き込みます**System.Diagnostics.Trace**、別のトレース リスナーを登録することができます。 たとえば、書き込むトレース ログ ファイルにします。 トレース ライターの詳細については、次を参照してください。、[トレース リスナー](https://msdn.microsoft.com/library/4y5y10s7.aspx) MSDN のトピックです。

### <a name="configuring-systemdiagnosticstracewriter"></a>SystemDiagnosticsTraceWriter を構成します。

次のコードでは、トレース ライターを構成する方法を示します。

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample3.cs)]

制御できる 2 つの設定があります。

- IsVerbose: false の場合、各トレースは最小限の情報を格納します。 True の場合、トレースは、追加情報を含めます。
- MinimumLevel: 最小トレース レベルを設定します。 順序でのトレース レベルは、デバッグ、Info、Warn、Error、および致命的なエラーです。

## <a name="adding-traces-to-your-web-api-application"></a>トレースを Web API アプリケーションに追加します。

トレース ライターを追加することにより、即時にアクセス Web API パイプラインによって作成されたトレースです。 独自のコードを追跡するのにトレース ライターを使用することもできます。

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample4.cs)]

トレース ライターを取得する**HttpConfiguration.Services.GetTraceWriter**です。 このメソッドは、経由でアクセスできる、コント ローラーから、 **ApiController.Configuration**プロパティです。

トレースを作成するに呼び出せる、 **ITraceWriter.Trace**メソッドを直接、ですが、 [ITraceWriterExtensions](https://msdn.microsoft.com/library/system.web.http.tracing.itracewriterextensions.aspx)クラスは、わかりやすいは一部の拡張メソッドを定義します。 たとえば、**情報**前に示したメソッドは、トレース レベルでトレースを作成**情報**です。

## <a name="web-api-tracing-infrastructure"></a>Web API トレース インフラストラクチャ

このセクションでは、Web API のカスタム トレース ライターを作成する方法について説明します。

Microsoft.AspNet.WebApi.Tracing パッケージは、Web API の一般的なトレース インフラストラクチャに構築されます。 Microsoft.AspNet.WebApi.Tracing を使用する代わりにも接続できる他のトレース/ログイン ライブラリでなど[NLog](http://nlog-project.org/)または[log4net](http://logging.apache.org/log4net/)です。

トレースを収集するには、実装、 **ITraceWriter**インターフェイスです。 単純な例を次に示します。

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample5.cs)]

**ITraceWriter.Trace**メソッドは、トレースを作成します。 呼び出し元は、カテゴリおよびトレース レベルを指定します。 カテゴリは、任意のユーザー定義の文字列にすることができます。 実装**トレース**次を行う必要があります。

1. 新しい**TraceRecord**です。 ように、要求、カテゴリ、およびトレース レベルで初期化します。 これらの値は、呼び出し元によって提供されます。
2. 呼び出す、 *traceAction*を委任します。 このデリゲート内の残りの部分を埋めるために、呼び出し元が必要、 **TraceRecord**です。
3. 書き込み、 **TraceRecord**、したい任意のログ記録手法を使用します。 次に例を呼び出すだけ**System.Diagnostics.Trace**です。

## <a name="setting-the-trace-writer"></a>トレース ライターの設定

トレースを有効にするには、Web API を使用するを構成する必要があります、 **ITraceWriter**実装します。 これを行う、 **HttpConfiguration**オブジェクトを次のコードに示すようにします。

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample6.cs)]

1 つだけのトレース ライターは、アクティブにできます。 既定では、Web API の設定、 &quot;、no-op&quot;は何もトレースします。 (、 &quot;、No-op&quot;トレース コードがトレース ライターがあるかどうかを確認する必要がないように、トレースが存在する**null**トレースを書き込む前にします)。

## <a name="how-web-api-tracing-works"></a>どのように Web API の動作をトレース

Web API の使用例の Web API を使用してトレースを*ファサード*パターン: トレースが有効になっている場合は、Web API は、trace の呼び出しを実行するクラスを持つ要求パイプラインのさまざまな部分で折り返されます。 します。

たとえば、コント ローラーを選択すると、パイプラインを使用して、 **IHttpControllerSelector**インターフェイスです。 実装するクラスを挿入する、pipleline トレースを有効になっているに**IHttpControllerSelector**が実際の実装を通じて呼び出し。

![Web API トレースは、ファサード パターンを使用します。](tracing-in-aspnet-web-api/_static/image8.png)

このデザインの利点は次のとおりです。

- トレース ライターを追加しないと、トレースは、コンポーネントはインスタンス化されていないと、パフォーマンスの影響を与えるありません。
- など、既定のサービスを置き換える場合は、 **IHttpControllerSelector**独自のカスタム実装とトレースは影響しません、トレースは、ラッパー オブジェクトによって行われるためです。

既定値に置き換えることで、独自のカスタム framework では、Web API トレース フレームワーク全体を置換することもできます**ITraceManager**サービス。

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample7.cs)]

実装**ITraceManager.Initialize**トレースは、システムを初期化します。 これが置き換えられるに注意してください、*全体*のすべての Web API に組み込まれているトレース コードを含むトレース フレームワークです。
