---
title: ASP.NET Core のミドルウェアへの HTTP ハンドラーとモジュールを移行します。
author: rick-anderson
description: ''
ms.author: tdykstra
ms.date: 12/07/2016
uid: migration/http-modules
ms.openlocfilehash: 516230a66ee3edba986c91d79684256aa8e4c994
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087017"
---
# <a name="migrate-http-handlers-and-modules-to-aspnet-core-middleware"></a>ASP.NET Core のミドルウェアへの HTTP ハンドラーとモジュールを移行します。

によって[Matt Perdeck](https://www.linkedin.com/in/mattperdeck)

この記事では、既存の ASP.NET に移行する方法を示しています。 [HTTP モジュールとハンドラー system.webserver](/iis/configuration/system.webserver/) ASP.NET core[ミドルウェア](xref:fundamentals/middleware/index)します。

## <a name="modules-and-handlers-revisited"></a>モジュールとハンドラーの再考

ASP.NET Core のミドルウェアを次に進む前にまず局所的 HTTP モジュールとハンドラーの動作方法。

![モジュールのハンドラー](http-modules/_static/moduleshandlers.png)

**ハンドラーは次のとおりです。**

* 実装するクラス[IHttpHandler](/dotnet/api/system.web.ihttphandler)

* 指定されたファイル名または拡張子で要求を処理するために使用*レポート*

* [構成されている](/iis/configuration/system.webserver/handlers/)で*Web.config*

**モジュールは次のとおりです。**

* 実装するクラス[IHttpModule](/dotnet/api/system.web.ihttpmodule)

* すべての要求に対して呼び出されます

* (さらに処理の停止の要求) をショート サーキットすること

* HTTP 応答に追加したり、独自に作成できません。

* [構成されている](/iis/configuration/system.webserver/modules/)で*Web.config*

**モジュールが受信要求を処理する順序は、によって決まります。**

1. [アプリケーションのライフ サイクル](https://msdn.microsoft.com/library/ms227673.aspx)、ASP.NET によって発生した一連のイベントであります。[BeginRequest](/dotnet/api/system.web.httpapplication.beginrequest)、 [AuthenticateRequest](/dotnet/api/system.web.httpapplication.authenticaterequest)など。各モジュールには、1 つまたは複数のイベントのハンドラーを作成できます。

2. 同一のイベントで構成されている順序*Web.config*します。

ライフ サイクル イベントのハンドラーを追加するだけでなく、モジュール、 *Global.asax.cs*ファイル。 これらのハンドラーは、構成済みのモジュール内のハンドラーの後に実行します。

## <a name="from-handlers-and-modules-to-middleware"></a>ハンドラーとモジュールからミドルウェアへ

**ミドルウェアは、HTTP モジュールとハンドラーよりも簡単です。**

* モジュール、ハンドラー、 *Global.asax.cs*、 *Web.config* (IIS の構成) を除くアプリケーションのライフ サイクルがなくなり、

* ミドルウェアによって作成されたモジュールとハンドラーの両方のロール

* ミドルウェアがコードを使用して構成されているのではなく*Web.config*

* [パイプラインを分岐](xref:fundamentals/middleware/index#use-run-and-map)要求ヘッダー、クエリ文字列などでも、URL だけでなくに基づいて、特定のミドルウェアに要求を送信することができます。

**ミドルウェアは、モジュールとよく似ています。**

* 基本的にすべての要求に対して呼び出されます

* によって、要求をショート サーキットできなければ[要求を次のミドルウェアに渡されていません。](#http-modules-shortcircuiting-middleware)

* 独自の HTTP 応答を作成できません。

**ミドルウェアとモジュールは、別の順序で処理されます。**

* ミドルウェアの順序が順番に挿入する要求パイプラインでは、モジュールの順序は主に基づいて順序に基づいて[アプリケーションのライフ サイクル](https://msdn.microsoft.com/library/ms227673.aspx)イベント

* 応答のミドルウェアの順序は、要求から逆モジュールの順序は要求と応答の同じ

* 参照してください[IApplicationBuilder を使用したミドルウェア パイプラインを作成します。](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder)

![ミドルウェア](http-modules/_static/middleware.png)

認証ミドルウェア サーキット要求上の図にする方法に注意してください。

## <a name="migrating-module-code-to-middleware"></a>ミドルウェアへのモジュール コードの移行

既存の HTTP モジュールは、次のようになります。

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyModule.cs?highlight=6,8,24,31)]

ように、[ミドルウェア](xref:fundamentals/middleware/index) ページで、ASP.NET Core のミドルウェアが公開するクラス、`Invoke`メソッドを`HttpContext`を返すと、`Task`します。 新しいミドルウェアは、次のようになります。

<a name="http-modules-usemiddleware"></a>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddleware.cs?highlight=9,13,20,24,28,30,32)]

上記のミドルウェアのテンプレート セクションから引用したもの[ミドルウェアを記述](xref:fundamentals/middleware/write)します。

*MyMiddlewareExtensions*ヘルパー クラスでミドルウェアを構成しやすい、`Startup`クラス。 `UseMyMiddleware`メソッドは、要求パイプラインにミドルウェア クラスを追加します。 ミドルウェアによって必要なサービスは、ミドルウェアのコンス トラクターで挿入を取得します。

<a name="http-modules-shortcircuiting-middleware"></a>

モジュール例では、ユーザーが許可されていない場合、要求が終了する可能性があります。

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyTerminatingModule.cs?highlight=9,10,11,12,13&name=snippet_Terminate)]

ミドルウェアがこれを呼び出さなかったことで処理`Invoke`パイプラインの次のミドルウェアにします。 前のミドルウェアも応答するパイプラインを遡って、その方法とが呼び出されるために、要求を終了これは完全に留意してください。

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyTerminatingMiddleware.cs?highlight=7,8&name=snippet_Terminate)]

新しいミドルウェアにモジュールの機能を移行する場合ために、コードをコンパイルできないことを見つけることがあります、`HttpContext`クラスは、ASP.NET Core では大幅に変更します。 [後で](#migrating-to-the-new-httpcontext)、新しい ASP.NET Core HttpContext に移行する方法について説明します。

## <a name="migrating-module-insertion-into-the-request-pipeline"></a>要求パイプラインに移行するモジュールの挿入

HTTP モジュールは通常、要求パイプラインを使用する追加*Web.config*:

[!code-xml[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32-33,36,43,50,101)]

これによって変換[新しいミドルウェアを追加する](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder)で要求パイプラインに、`Startup`クラス。

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=16)]

新しいミドルウェアを挿入するパイプラインで正確なスポットをモジュールとして処理するイベントによって異なります (`BeginRequest`、`EndRequest`など) とその順序でモジュールの一覧に*Web.config*します。

前と同じ説明するがありますはなくアプリケーションのライフ サイクル ASP.NET Core で、応答がミドルウェアによって処理される順序は、モジュールによって使用される順序によって異なります。 これでしたかを決定順序付けより困難になります。

順序付けには、問題になると、モジュールを個別に注文する複数のミドルウェア コンポーネントに分割できます。

## <a name="migrating-handler-code-to-middleware"></a>ミドルウェアへハンドラー コードの移行

HTTP ハンドラーでは、このようなものはなります。

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/HttpHandlers/ReportHandler.cs?highlight=5,7,13,14,15,16)]

ASP.NET Core プロジェクトで、次のようなミドルウェアをこの変換は。

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/ReportHandlerMiddleware.cs?highlight=7,9,13,20,21,22,23,40,42,44)]

このミドルウェアは、モジュールに対応するミドルウェアによく似ています。 唯一の違いはここで呼び出しがない`_next.Invoke(context)`します。 ハンドラーは、要求パイプラインの最後があるため次のミドルウェアを呼び出すため、理にかなっています。

## <a name="migrating-handler-insertion-into-the-request-pipeline"></a>要求パイプラインに移行するハンドラーの挿入

HTTP ハンドラーの構成は*Web.config*次のようになります。

[!code-xml[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32,46-48,50,101)]

新しいハンドラー ミドルウェアをパイプラインでは、要求に追加することで、これを変換することも、`Startup`クラス、モジュールから変換されたミドルウェアに似ています。 このアプローチの問題は、新しいハンドラー ミドルウェアにすべての要求が送信します。 ただし、特定の拡張機能とミドルウェアに要求を送信するのみ。 与えられます、同じ機能が、HTTP ハンドラーを使用する必要があります。

1 つのソリューションは、特定の拡張機能の要求のパイプラインを分岐するのを使用して、`MapWhen`拡張メソッド。 同じで、これを行う`Configure`メソッドは、その他のミドルウェアを追加します。

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=27-34)]

`MapWhen` これらのパラメーターを受け取ります。

1. 受け取るラムダ、`HttpContext`返します`true`要求する必要があります、分岐がダウンした場合。 これは、要求だけでなく、その拡張機能が、要求ヘッダー、クエリ文字列パラメーターなどを基に分岐することを意味します。

2. 受け取るラムダ、`IApplicationBuilder`分岐のすべてのミドルウェアを追加します。 つまり、ハンドラー ミドルウェアの前に、分岐に追加のミドルウェアを追加できます。

ミドルウェアは、すべての要求に、分岐が呼び出される前に、パイプラインに追加分岐影響がないにします。

## <a name="loading-middleware-options-using-the-options-pattern"></a>読み込みオプション パターンを使用してミドルウェア オプション

格納されている構成オプションがあるいくつかのモジュールとハンドラー *Web.config*します。ただし、ASP.NET Core で新しい構成モデルを使用の代わりに*Web.config*します。

新しい[構成システム](xref:fundamentals/configuration/index)これを解決するオプションが表示されます。

* ように、ミドルウェアにオプションを直接挿入、[次のセクション](#loading-middleware-options-through-direct-injection)します。

* 使用して、[オプション パターン](xref:fundamentals/configuration/options):

1. たとえば、ミドルウェアのオプションを保持するクラスを作成します。

   [!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Options)]

2. オプションの値を格納します。

   構成システム オプション任意の場所を求める値を格納することができます。 ただし、使用を最もサイト*appsettings.json*ので、その方法を詳しく説明します。

   [!code-json[](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,14-18)]

   *MyMiddlewareOptionsSection*セクション名を次に示します。 オプション クラスの名前と同じである必要はありません。

3. オプション クラスを使用して、オプション値を関連付ける

    オプション パターンでは、ASP.NET Core の依存関係挿入フレームワークを使用して、オプションの種類を関連付けます (など`MyMiddlewareOptions`) で、`MyMiddlewareOptions`を実際のオプションを持つオブジェクト。

    更新プログラム、`Startup`クラス。

   1. 使用している場合*appsettings.json*、ビルダーでは、構成に追加して、`Startup`コンス トラクター。

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Ctor&highlight=5-6)]

   2. オプションのサービスを構成します。

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

   3. オプション クラスを含む、オプションを関連付けます。

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=6-8)]

4. ミドルウェアのコンス トラクターにオプションを挿入します。 これは、オプションは、コント ローラーに挿入することに似ています。

   [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_MiddlewareWithParams&highlight=4,7,10,15-16)]

   [UseMiddleware](#http-modules-usemiddleware)にミドルウェアを追加する拡張メソッド、`IApplicationBuilder`依存関係の挿入を行います。

   これに限定されません`IOptions`オブジェクト。 ミドルウェアを必要とするその他のオブジェクトには、この方法を挿入できます。

## <a name="loading-middleware-options-through-direct-injection"></a>直接挿入を介してミドルウェアのオプションの読み込み

オプション パターンでは、疎結合オプションの値と、コンシューマーが作成するという利点があります。 オプション クラスは、実際のオプション値に関連した、その他のクラスは、依存関係挿入フレームワークを通じてオプションへのアクセスを取得できます。 オプションの値を渡す必要はありません。

これは、分割がさまざまなオプションを 2 回、同じミドルウェアを使用する場合。 たとえば、承認ミドルウェアさまざまなロールを許可する別の分岐で使用します。 1 つのオプション クラスを使用して、2 つの異なるオプション オブジェクトを関連付けることはできません。

解決する実際のオプションの値とオプション オブジェクトを取得するには、`Startup`クラスし、ミドルウェアの各インスタンスに直接渡します。

1. 2 番目のキーを追加*appsettings.json*

   オプションの 2 番目のセットを追加する、 *appsettings.json*ファイルを一意に識別できる、新しいキーを使用します。

   [!code-json[](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,10-18&highlight=2-5)]

2. オプションの値を取得し、ミドルウェアに渡します。 `Use...`拡張メソッド (これは、パイプラインにミドルウェアを追加します) は、オプション値を渡す論理的な場所。 

   [!code-csharp[](http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=20-23)]

3. オプションのパラメーターを受け取るミドルウェアを有効にします。 オーバー ロードを提供、`Use...`拡張メソッド (オプションのパラメーターを受け取るしに渡されますを`UseMiddleware`)。 ときに`UseMiddleware`呼びますパラメーターを持つパラメーターに渡す、ミドルウェア コンス トラクター ミドルウェア オブジェクトをインスタンス化時にします。

   [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Extensions&highlight=9-14)]

   これでオプションのオブジェクトの折り返し方法に注意してください、`OptionsWrapper`オブジェクト。 これを実装して`IOptions`ミドルウェア コンス トラクターの期待どおりに、します。

## <a name="migrating-to-the-new-httpcontext"></a>新しい HttpContext への移行

先ほど見たを`Invoke`ミドルウェアでメソッドが型のパラメーターを受け取る`HttpContext`:

```csharp
public async Task Invoke(HttpContext context)
```

`HttpContext` ASP.NET Core で大幅に変更されました。 このセクションの最もよく使用されるプロパティを変換する方法を示しています。 [System.Web.HttpContext](/dotnet/api/system.web.httpcontext)を新しい`Microsoft.AspNetCore.Http.HttpContext`します。

### <a name="httpcontext"></a>HttpContext

**HttpContext.Items**に変換されます。

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Items)]

**一意の要求 ID (System.Web.HttpContext が認識されていません)**

うえで、一意の id の各要求。 ログに含める非常に便利です。

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Trace)]

### <a name="httpcontextrequest"></a>HttpContext.Request

**HttpContext.Request.HttpMethod**に変換されます。

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Method)]

**HttpContext.Request.QueryString**に変換されます。

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Query)]

**HttpContext.Request.Url**と**HttpContext.Request.RawUrl**に変換します。

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Url)]

**HttpContext.Request.IsSecureConnection**に変換されます。

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Secure)]

**HttpContext.Request.UserHostAddress**に変換されます。

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Host)]

**HttpContext.Request.Cookies**に変換されます。

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Cookies)]

**HttpContext.Request.RequestContext.RouteData** translates to:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Route)]

**HttpContext.Request.Headers**に変換されます。

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Headers)]

**HttpContext.Request.UserAgent**に変換されます。

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Agent)]

**HttpContext.Request.UrlReferrer**に変換されます。

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Referrer)]

**HttpContext.Request.ContentType**に変換されます。

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Type)]

**HttpContext.Request.Form**に変換されます。

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Form)]

> [!WARNING]
> コンテンツのサブタイプの場合にのみ、フォームの値を読み取る*x-www-form-urlencoded*または*フォーム データ*します。

**HttpContext.Request.InputStream**に変換されます。

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Input)]

> [!WARNING]
> パイプラインの最後に、ハンドラーの型ミドルウェアでのみ、このコードを使用します。
>
>要求あたり 1 回だけ上に示す未加工の本文を読み取ることができます。 最初の読み込み後に、本文の読み取りを試みるミドルウェアでは、空の本文を読み取ります。
>
>これは、バッファーから行うことがあるため、前述のように、フォームの読み取りに適用されません。

### <a name="httpcontextresponse"></a>HttpContext.Response

**HttpContext.Response.Status**と**HttpContext.Response.StatusDescription**に変換します。

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Status)]

**HttpContext.Response.ContentEncoding**と**HttpContext.Response.ContentType**に変換します。

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespType)]

**HttpContext.Response.ContentType**で独自にも変換します。

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespTypeOnly)]

**HttpContext.Response.Output**に変換されます。

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Output)]

**HttpContext.Response.TransmitFile**

については、ファイルを提供している[ここ](../fundamentals/request-features.md#middleware-and-request-features)します。

**HttpContext.Response.Headers**

応答ヘッダーを送信するという事実の影響は複雑になります、設定した場合に何かが応答本文に書き込まれた後、それらは送信されません。

ソリューションでは、右、応答が開始に書き込む前に呼び出されるコールバック メソッドを設定します。 先頭にこれは、最適な`Invoke`ミドルウェア内のメソッド。 このコールバック メソッド、応答ヘッダーを設定することをお勧めします。

次のコードが呼び出されるコールバック メソッドを設定`SetHeaders`:

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

`SetHeaders`コールバック メソッドに次のようになります。

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetHeaders)]

**HttpContext.Response.Cookies**

ブラウザーに cookie の送信、 *Set-cookie*応答ヘッダー。 結果として、cookie を送信する必要があります使用したのと同じコールバック応答ヘッダーを送信するため。

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetCookies, state: httpContext);
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

`SetCookies`コールバック メソッドに次のようになります。

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetCookies)]

## <a name="additional-resources"></a>その他の技術情報

* [HTTP ハンドラーと HTTP モジュールの概要](/iis/configuration/system.webserver/)
* [構成](xref:fundamentals/configuration/index)
* [アプリケーションの起動](xref:fundamentals/startup)
* [ミドルウェア](xref:fundamentals/middleware/index)
