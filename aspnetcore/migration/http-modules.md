---
title: "HTTP ハンドラーと ASP.NET Core ミドルウェアにモジュールを移行します。"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: tdykstra
manager: wpickett
ms.date: 12/07/2016
ms.topic: article
ms.assetid: 9c826a76-fbd2-46b5-978d-6ca6df53531a
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/http-modules
ms.openlocfilehash: e14664133abf010b80374036e4855fdff71d1d5f
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/12/2017
---
# <a name="migrating-http-handlers-and-modules-to-aspnet-core-middleware"></a>HTTP ハンドラーと ASP.NET Core ミドルウェアにモジュールを移行します。 

によって[Matt Perdeck](https://www.linkedin.com/in/mattperdeck)

この記事は、既存の ASP.NET を移行する方法を示しています。 [HTTP モジュールとハンドラー system.webserver](https://docs.microsoft.com/iis/configuration/system.webserver/)を ASP.NET Core[ミドルウェア](../fundamentals/middleware.md)です。

## <a name="modules-and-handlers-revisited"></a>モジュールとハンドラーが見直され

ASP.NET Core ミドルウェアを前に、最初に要約 HTTP モジュールとハンドラーの動作方法。

![モジュール ハンドラー](http-modules/_static/moduleshandlers.png)

**ハンドラーは次のとおりです。**

   * 実装するクラス[IHttpHandler](https://docs.microsoft.com/dotnet/api/system.web.ihttphandler)

   * など、指定されたファイル名または拡張機能で要求を処理するために使用*レポート*

   * [構成されている](https://docs.microsoft.com//iis/configuration/system.webserver/handlers/)で*Web.config*

**モジュールは次のとおりです。**

   * 実装するクラス[IHttpModule](https://docs.microsoft.com/dotnet/api/system.web.ihttpmodule)

   * 要求ごとに呼び出されます

   * ショート サーキット (さらに処理の停止の要求の) すること

   * HTTP 応答に追加したり、独自に作成できません。

   * [構成されている](https://docs.microsoft.com//iis/configuration/system.webserver/modules/)で*Web.config*

**モジュールが受信要求を処理する順序は、によって決定されます。**

   1. [アプリケーションのライフ サイクル](https://msdn.microsoft.com/library/ms227673.aspx)、ASP.NET によって発生した一連のイベントである: [BeginRequest](https://docs.microsoft.com/dotnet/api/system.web.httpapplication.beginrequest)、 [AuthenticateRequest](https://docs.microsoft.com/dotnet/api/system.web.httpapplication.authenticaterequest), などです。各モジュールには、1 つまたは複数のイベントのハンドラーを作成できます。

   2. 同一のイベントで構成されている順序*Web.config*です。

ライフ サイクル イベントのハンドラーを追加するだけでなく、モジュール、 *Global.asax.cs*ファイル。 これらのハンドラーは、構成済みのモジュール内のハンドラーの後に実行します。

## <a name="from-handlers-and-modules-to-middleware"></a>ハンドラーとミドルウェアをモジュールから

**ミドルウェアは、HTTP モジュールとハンドラーよりも簡単です。**

   * モジュール、ハンドラー、 *Global.asax.cs*、 *Web.config* (IIS の構成) を除くアプリケーションのライフ サイクルはなくなり、

   * ミドルウェアによって作成されたモジュールとハンドラーの両方のロール

   * ミドルウェアは、コードを使用して構成されてなく*Web.config*

   * [パイプラインを分岐](../fundamentals/middleware.md#middleware-run-map-use)要求ヘッダー、クエリ文字列なども上にある URL だけでなくに基づいて、特定のミドルウェアに要求を送信することができます。

**ミドルウェアは、モジュールとよく似ています。**

   * 原則として要求ごとに呼び出されます

   * 要求をショート サーキットできなければ[要求を次のミドルウェアに渡されていません。](#http-modules-shortcircuiting-middleware)

   * 独自の HTTP 応答を作成できません。

**ミドルウェアとモジュールは、別の順序で処理されます。**

   * ミドルウェアの順序は順番に挿入された、要求パイプライン モジュールの順序の基準は、主に、順序に基づきます[アプリケーションのライフ サイクル](https://msdn.microsoft.com/library/ms227673.aspx)イベント

   * モジュールの順序は、同じ要求と応答の中に応答のミドルウェアの順序が要求の場合の逆順になって

   * 参照してください[IApplicationBuilder のミドルウェア パイプラインを作成します。](../fundamentals/middleware.md#creating-a-middleware-pipeline-with-iapplicationbuilder)

![ミドルウェア](http-modules/_static/middleware.png)

認証ミドルウェア short-circuited 要求、上記の図で、どのように注意してください。

## <a name="migrating-module-code-to-middleware"></a>ミドルウェアにモジュールのコードを移行します。

既存の HTTP モジュールは、次のようになります。

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyModule.cs?highlight=6,8,24,31)]

ように、[ミドルウェア](../fundamentals/middleware.md) ページで、ASP.NET Core ミドルウェアが公開するクラス、`Invoke`メソッドを`HttpContext`を返すと、`Task`です。 新しいミドルウェアは、次のようになります。

<a name=http-modules-usemiddleware></a>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddleware.cs?highlight=9,13,20,24,28,30,32)]

上記のミドルウェア テンプレートがセクションから取得されました[ミドルウェアを記述](../fundamentals/middleware.md#middleware-writing-middleware)です。

*MyMiddlewareExtensions*ヘルパー クラスを簡単にミドルウェアの構成、`Startup`クラスです。 `UseMyMiddleware`メソッドは、要求パイプラインにミドルウェア クラスを追加します。 ミドルウェアによって必要なサービスは、ミドルウェアのコンス トラクターで挿入されたを取得します。

<a name=http-modules-shortcircuiting-middleware></a>

モジュールは、たとえば、ユーザーの権限がない場合、要求を終了します。

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyTerminatingModule.cs?highlight=9,10,11,12,13&name=snippet_Terminate)]

ミドルウェアの処理を呼び出していない`Invoke`パイプラインの次のミドルウェアにします。 以前 middlewares は応答により、パイプラインから戻るときにも呼び出されるために、いるこの完全に終了しない、要求に留意してください。

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyTerminatingMiddleware.cs?highlight=7,8&name=snippet_Terminate)]

新しいミドルウェアにモジュールの機能を移行する場合がありますので、コードをコンパイルできないこと、`HttpContext`クラスが ASP.NET Core で大幅に変更します。 [後で](#migrating-to-the-new-httpcontext)、新規の ASP.NET Core HttpContext に移行する方法について説明します。

## <a name="migrating-module-insertion-into-the-request-pipeline"></a>要求パイプラインに移行するモジュールの挿入

HTTP モジュールが、要求を使用してパイプラインに追加されます通常*Web.config*:

[!code-xml[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32-33,36,43,50,101)]

これによって変換[新しいミドルウェアを追加する](../fundamentals/middleware.md#creating-a-middleware-pipeline-with-iapplicationbuilder)要求パイプラインへ、`Startup`クラス。

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=16)]

新しいミドルウェアを挿入するパイプラインで正確なスポットは、モジュールとして処理するイベントによって異なります (`BeginRequest`、`EndRequest`など) とその順序内のモジュールの一覧で、 *Web.config*です。

既に説明したようにがありません。 アプリケーションのライフ サイクル ASP.NET Core とミドルウェアによって応答が処理される順序は、モジュールによって使用される順序によって異なります。 これ行うことができる順序の決定より困難です。

順序付けには、問題になると、個別に注文する複数のミドルウェア コンポーネントに、モジュールを分割する可能性があります。

## <a name="migrating-handler-code-to-middleware"></a>ミドルウェアをハンドラー コードの移行

HTTP ハンドラーでは、次のようになります。

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/HttpHandlers/ReportHandler.cs?highlight=5,7,13,14,15,16)]

ASP.NET Core のプロジェクトで次のようなミドルウェアをこの変換は。

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/ReportHandlerMiddleware.cs?highlight=7,9,13,20,21,22,23,40,42,44)]

このミドルウェアは、モジュールに対応するミドルウェアに非常に似ています。 唯一の違いはここでは呼び出されずに`_next.Invoke(context)`です。 ハンドラーは、要求パイプラインの最後があるためありません次のミドルウェアを呼び出すため理にかなっています。

## <a name="migrating-handler-insertion-into-the-request-pipeline"></a>要求パイプラインへの移行のハンドラー挿入

行われる HTTP ハンドラーを構成する*Web.config*し、次のようになります。

[!code-xml[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32,46-48,50,101)]

要求パイプラインへの新しいハンドラー ミドルウェアを追加することで、これを変換することも、`Startup`クラス、モジュールから変換されたミドルウェアに似ています。 その方法を使った問題は、新しいハンドラー ミドルウェアにすべての要求が送信します。 ただし、ミドルウェアに指定された拡張子を持つ要求を送信するだけです。 同じの機能を提供する、HTTP ハンドラーを持つ必要があります。

特定の拡張機能と要求パイプラインを分岐するのには、1 つのソリューションを使用して、`MapWhen`拡張メソッド。 こうと、同じ`Configure`他のミドルウェアを追加する方法。

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=27-34)]

`MapWhen`これらのパラメーターを受け取ります。

1. 受け取るラムダ、`HttpContext`し、返します`true`場合は、要求が、分岐を移動する必要があります。 これは、要求だけでなく、その拡張機能が、要求ヘッダー、クエリ文字列パラメーターなどを基に分岐することを意味します。

2. 受け取るラムダ、`IApplicationBuilder`し、分岐のすべてのミドルウェアを追加します。 つまり、その他のミドルウェアを分岐に追加するには、ハンドラーのミドルウェアの前にします。

すべての要求で、分岐が呼び出される前に、パイプラインにミドルウェアが追加されました。分岐による影響はありませんします。

## <a name="loading-middleware-options-using-the-options-pattern"></a>読み込みオプションのパターンを使用してミドルウェアのオプション

一部のモジュールとハンドラーに格納されている構成オプションがあります*Web.config*です。ただし、ASP.NET Core で新しい構成モデルが使用の代わりに*Web.config*です。

新しい[構成システム](../fundamentals/configuration.md)これを解決するオプションが表示されます。

* ように、ミドルウェアにオプションを直接挿入、[次のセクション](#loading-middleware-options-through-direct-injection)です。

* 使用して、[オプション パターン](../fundamentals/configuration.md#options-config-objects):

1.  たとえば、ミドルウェアのオプションを保持するクラスを作成します。

    [!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Options)]

2.  オプションの値を格納します。

    構成システムでは、オプション値任意の場所を格納することができます。 ただし、使用を最もサイト*される appsettings.json*ので、その方法を詳しく説明します。

    [!code-json[Main](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,14-18)]

    *MyMiddlewareOptionsSection*セクション名を次に示します。 オプション クラスの名前と同じである必要はありません。

3. オプションの値を関連付けるオプション クラス

    オプション パターンでは、ASP.NET Core の依存関係の挿入のフレームワークを使用して、オプションの種類を関連付けます (など`MyMiddlewareOptions`) で、`MyMiddlewareOptions`実際のオプションを持つオブジェクトです。

    更新プログラム、`Startup`クラス。

    1.  使用する場合*される appsettings.json*、ビルダーでは、構成を追加、`Startup`コンス トラクター。

      [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Ctor&highlight=5-6)]

    2.  オプションのサービスを構成します。

      [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

    3.  オプション クラスと、オプションを関連付けます。

      [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=6-8)]

4.  ミドルウェア コンス トラクターにオプションを挿入します。 これは、コント ローラーにオプションを挿入することに似ています。

  [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_MiddlewareWithParams&highlight=4,7,10,15-16)]

  [UseMiddleware](#http-modules-usemiddleware) 、ミドルウェアに追加する拡張メソッド、`IApplicationBuilder`依存関係の挿入を行います。

  これに限定されません`IOptions`オブジェクト。 ミドルウェアを必要とするその他のオブジェクトには、この方法を挿入できます。

## <a name="loading-middleware-options-through-direct-injection"></a>直接インジェクションを通じてミドルウェアのオプションの読み込み

オプションのパターンでは、疎結合のオプションの値とコンシューマーとの間で作成する利点があります。 オプション クラスは、実際のオプション値に関連した、他のクラスは、依存関係の挿入フレームワークを通じてオプションへのアクセスを取得できます。 オプションの値を渡す必要はありません。

これは、分割もオプションが異なる同じミドルウェアを 2 回、使用する場合。 たとえば、承認ミドルウェアの異なるロールを許可するさまざまな分岐で使用します。 1 つのオプション クラスを使用して 2 つの異なるオプション オブジェクトを関連付けることはできません。

解決する実際のオプションの値とオプション オブジェクトを取得するには、`Startup`クラスし、直接ミドルウェアの各インスタンスに渡します。

1.  2 番目のキーを追加*される appsettings.json*

    2 番目のオプションのセットを追加する、*される appsettings.json*ファイルを新しいキーを使用して一意に識別できるを。

    [!code-json[Main](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,10-18&highlight=2-5)]

2.  オプションの値を取得し、ミドルウェアに渡したりします。 `Use...` (パイプラインにミドルウェアを追加) する拡張メソッドは、オプションの値で渡す論理的な場所。 

    [!code-csharp[Main](http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=20-23)]

4.  ミドルウェアのオプション パラメーターを受け取ることを有効にします。 オーバー ロードを提供、`Use...`拡張メソッド (オプション パラメーターを使用してに渡します`UseMiddleware`)。 ときに`UseMiddleware`が呼び出されたパラメーターを持つパラメーターに渡して、ミドルウェア コンス トラクター ミドルウェア オブジェクトをインスタンス化時にします。

    [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Extensions&highlight=9-14)]

    オプション オブジェクトをラップするこの方法に注意してください、`OptionsWrapper`オブジェクト。 これは、手順で実装`IOptions`ミドルウェア コンス トラクターが予期するとおり、します。

## <a name="migrating-to-the-new-httpcontext"></a>新しい HttpContext への移行

先ほど見たを`Invoke`、ミドルウェア内でメソッドが型のパラメーターを受け取る`HttpContext`:

```csharp
public async Task Invoke(HttpContext context)
```

`HttpContext`ASP.NET Core で大幅に変更されました。 このセクションでは、の最も一般的に使用されるプロパティに変換する方法を示しています。 [System.Web.HttpContext](https://docs.microsoft.com/dotnet/api/system.web.httpcontext)を新しい`Microsoft.AspNetCore.Http.HttpContext`です。

### <a name="httpcontext"></a>HttpContext

**HttpContext.Items**に変換されます。

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Items)]

**一意の要求 ID (System.Web.HttpContext 対応はありません)**

使用する一意の id 要求ごとにできます。 ログに記録に非常に便利です。

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Trace)]

### <a name="httpcontextrequest"></a>HttpContext.Request

**HttpContext.Request.HttpMethod**に変換されます。

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Method)]

**HttpContext.Request.QueryString**に変換されます。

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Query)]

**HttpContext.Request.Url**と**HttpContext.Request.RawUrl**に変換します。

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Url)]

**HttpContext.Request.IsSecureConnection**に変換されます。

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Secure)]

**HttpContext.Request.UserHostAddress**に変換されます。

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Host)]

**HttpContext.Request.Cookies**に変換されます。

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Cookies)]

**HttpContext.Request.RequestContext.RouteData**に変換されます。

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Route)]

**HttpContext.Request.Headers**に変換されます。

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Headers)]

**HttpContext.Request.UserAgent**に変換されます。

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Agent)]

**HttpContext.Request.UrlReferrer**に変換されます。

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Referrer)]

**HttpContext.Request.ContentType**に変換されます。

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Type)]

**HttpContext.Request.Form**に変換されます。

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Form)]

> [!WARNING]
> コンテンツのサブタイプが場合にのみ、フォームの値を読み取る*x-www-form-urlencoded*または*フォーム データ*です。

**HttpContext.Request.InputStream**に変換されます。

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Input)]

> [!WARNING]
> パイプラインの最後のハンドラー型ミドルウェアでのみ、このコードを使用します。
>
>要求あたり 1 回だけ上記のように、生の本体を読み取ることができます。 ミドルウェアの最初の読み込み後に、本文を読み取るしようとしています。 空の本文が読み取られます。
>
>これは、バッファーからが完了するため、前に示したようにフォームを読み取り中には適用されません。

### <a name="httpcontextresponse"></a>HttpContext.Response

**HttpContext.Response.Status**と**HttpContext.Response.StatusDescription**に変換します。

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Status)]

**HttpContext.Response.ContentEncoding**と**HttpContext.Response.ContentType**に変換します。

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespType)]

**HttpContext.Response.ContentType**でそれ自体にも変換します。

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespTypeOnly)]

**HttpContext.Response.Output**に変換されます。

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Output)]

**HttpContext.Response.TransmitFile**

説明、ファイルを提供している[ここ](../fundamentals/request-features.md#middleware-and-request-features)です。

**HttpContext.Response.Headers**

応答ヘッダーの送信が複雑にする設定した場合、応答本文に何も書き込まれた後、それらは送信されません。

ソリューションでは、右、応答が開始に書き込む前に呼び出されるコールバック メソッドを設定します。 開始時にこれは、最適な`Invoke`ミドルウェア内でのメソッドです。 このコールバック メソッド、応答ヘッダーを設定することをお勧めします。

次のコードが呼び出されるコールバック メソッドを設定`SetHeaders`:

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

`SetHeaders`コールバック メソッドは次のようになります。

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetHeaders)]

**HttpContext.Response.Cookies**

Cookie をブラウザーに移動、 *Set-cookie*応答ヘッダー。 その結果、cookie を送信する必要があります使用したのと同じコールバック応答ヘッダーを送信するため。

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetCookies, state: httpContext);
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

`SetCookies`コールバック メソッドは、次のようになります。

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetCookies)]

## <a name="additional-resources"></a>その他のリソース

* [HTTP ハンドラーと HTTP モジュールの概要](https://docs.microsoft.com/iis/configuration/system.webserver/)

* [構成](../fundamentals/configuration.md)

* [アプリケーションの起動](../fundamentals/startup.md)

* [ミドルウェア](../fundamentals/middleware.md)
