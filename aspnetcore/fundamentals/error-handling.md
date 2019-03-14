---
title: ASP.NET Core のエラーを処理する
author: tdykstra
description: ASP.NET Core アプリでエラーを処理する方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/01/2019
uid: fundamentals/error-handling
ms.openlocfilehash: a2ae2cb25c8cc5048b189b4035abbfc32a29aaff
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/05/2019
ms.locfileid: "57345496"
---
# <a name="handle-errors-in-aspnet-core"></a>ASP.NET Core のエラーを処理する

作成者: [Tom Dykstra](https://github.com/tdykstra/)、[Luke Latham](https://github.com/guardrex)、[Steve Smith](https://ardalis.com/)

この記事では、ASP.NET Core アプリでエラーを処理するための一般的な手法について取り上げます。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。

## <a name="developer-exception-page"></a>開発者例外ページ

例外に関する詳細情報を表示するページを表示するアプリを構成するには、*開発者例外ページ*を使用します。 このページは [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app) 内で利用できる、[Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) パッケージによって使用可能になります。 `Startup.Configure` メソッドに次の行を追加します。

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevExceptionPage&highlight=5)]

例外をキャッチしたい任意のミドルウェアの前に <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*> への呼び出しを配置します。

> [!WARNING]
> **アプリを開発環境で実行するときにのみ**、開発者例外ページを有効にします。 アプリを実稼働環境で実行するときは、詳細な例外情報を公開しません。 環境の構成について詳しくは、「<xref:fundamentals/environments>」をご覧ください。

開発者例外ページを表示するには、環境を `Development` に設定してサンプル アプリを実行し、アプリのベース URL に `?throw=true` を追加します。 このページには、例外と要求に関する次の情報が含まれています。

* スタック トレース
* クエリ文字列のパラメーター (ある場合)
* Cookie (ある場合)
* ヘッダー

## <a name="configure-a-custom-exception-handling-page"></a>カスタム例外処理ページを構成する

アプリを開発環境で実行していない場合は、<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> 拡張メソッドを呼び出して例外処理ミドルウェアを追加します。 ミドルウェアでは次を行います。

* 例外をキャッチします。
* 例外をログに記録します。
* ページ用の、またはコントローラーが指定した別のパイプラインで、要求を再実行します。 応答が始まっていた場合、要求は再実行されません。

サンプル アプリの次の例では、<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> により非開発環境に例外処理ミドルウェアを追加しています。 この拡張メソッドでは、例外がキャッチされてログに記録された後、再実行された要求用に `/Error` エンドポイントにあるエラー ページまたはコントローラーを指定します。

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevExceptionPage&highlight=9)]

Razor Pages アプリのテンプレートには、エラー ページ (*.cshtml*) と <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> クラス (`ErrorModel`) が Pages フォルダー内に用意されています。

MVC アプリでは、次のエラー処理メソッドが MVC アプリ テンプレートに含まれ、ホーム コントローラーに表示されます。

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel 
        { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

`HttpGet` などの HTTP メソッド属性を使ってエラー ハンドラー アクション メソッドを修飾しないでください。 明示的な動詞を使用すると、要求がメソッドに届かないことがあります。 認証されていないユーザーがエラー ビューを受信できるように、メソッドへの匿名アクセスを許可します。

## <a name="configure-status-code-pages"></a>状態コード ページを構成する

ASP.NET Core アプリでは、既定で、"*404 - 見つかりません*" などの HTTP 状態コードの状態コード ページが表示されません。 アプリでは、状態コードと空の応答本文が返されます。 状態コード ページを提供するには、状態コード ページ ミドルウェアを使用します。

このミドルウェアは、[Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)内で使用できる、[Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) パッケージによって使用可能になります。

`Startup.Configure` メソッドに次の行を追加します。

```csharp
app.UseStatusCodePages();
```

要求処理ミドルウェア (たとえば、静的ファイル ミドルウェアや MVC ミドルウェア) の前に <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> メソッドを呼び出します。

既定では、状態コード ページ ミドルウェアにより、一般的な状態コード ("*404 - 見つかりません*" など) に対してテキストのみのハンドラーが追加されます。

```
Status Code: 404; Not Found
```

ミドルウェアではいくつかの拡張メソッドがサポートされていて、それを使ってその動作をカスタマイズすることができます。

<xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> のオーバーロードはラムダ式を受け取ります。これを使って、カスタム エラー処理ロジックを処理し、手動で応答を書き込むことができます。

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePages)]

<xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> のオーバーロードはコンテンツの種類と書式文字列を受け取ります。これを使って、コンテンツの種類と応答テキストをカスタマイズすることができます。

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

### <a name="redirect-and-re-execute-extension-methods"></a>拡張メソッドをリダイレクトして再実行する

<xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*>:

* クライアントに *302 - Found* 状態コードを送信します。
* URL テンプレートで指定された場所にクライアントをリダイレクトします。

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

アプリが次の条件を満たす場合、<xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*> が一般的に使用されます。

* クライアントを別のエンドポイントにリダイレクトする必要がある場合 (通常は、別のアプリがエラーを処理する場合)。 Web アプリの場合は、クライアントのブラウザーのアドレス バーにリダイレクトされたエンドポイントが反映されます。
* 元の状態コードを保持し、初回のリダイレクト応答で返してはいけない場合。

<xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*>:

* 元の状態コードをクライアントに返します。
* 代替パスを使用して要求パイプラインを再実行することで、応答本文を生成します。

```csharp
app.UseStatusCodePagesWithReExecute("/Error/{0}");
```

アプリで次を行う必要がある場合、<xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*> が一般的に使用されます。

* 別のエンドポイントにリダイレクトすることなく要求を処理する。 Web アプリの場合は、クライアントのブラウザーのアドレス バーに、初めに要求されていたエンドポイントが反映されます。
* 元の状態コードを保持し、応答で返す。

テンプレートには、状態コード用のプレースホルダー (`{0}`) が含まれる場合があります。 テンプレートには、最初にスラッシュ (`/`) を付ける必要があります。 プレースホルダーを使用する場合は、エンドポイント (ページまたはコントローラー) でパスのセグメントを処理できることを確認します。 たとえば、エラー用の Razor ページでは、`@page` ディレクティブの付いた省略可能なパスのセグメント値を受け入れる必要があります。

```cshtml
@page "{code?}"
```

状態コード ページは、Razor ページ ハンドラー メソッドまたは MVC コントローラー内の特定の要求に対して無効にすることができます。 状態コード ページを無効にするには、要求の [HttpContext.Features](xref:Microsoft.AspNetCore.Http.HttpContext.Features) コレクションから <xref:Microsoft.AspNetCore.Diagnostics.IStatusCodePagesFeature> を取得してみて、それが使用可能だった場合は機能を無効にします。

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

アプリ内のエンドポイントを指す <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> オーバーロードを使用するには、そのエンドポイントの MVC ビューまたは Razor ページを作成します。 たとえば、Razor Pages アプリのテンプレートでは、次のページとページ モデル クラスが生成されます。

*Error.cshtml*:

::: moniker range=">= aspnetcore-2.2"

```cshtml
@page
@model ErrorModel
@{
    ViewData["Title"] = "Error";
}

<h1 class="text-danger">Error.</h1>
<h2 class="text-danger">An error occurred while processing your request.</h2>

@if (Model.ShowRequestId)
{
    <p>
        <strong>Request ID:</strong> <code>@Model.RequestId</code>
    </p>
}

<h3>Development Mode</h3>
<p>
    Swapping to the <strong>Development</strong> environment displays 
    detailed information about the error that occurred.
</p>
<p>
    <strong>The Development environment shouldn't be enabled for deployed 
    applications.</strong> It can result in displaying sensitive information 
    from exceptions to end users. For local debugging, enable the 
    <strong>Development</strong> environment by setting the 
    <strong>ASPNETCORE_ENVIRONMENT</strong> environment variable to 
    <strong>Development</strong> and restarting the app.
</p>
```

*Error.cshtml.cs*:

```csharp
[ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, NoStore = true)]
public class ErrorModel : PageModel
{
    public string RequestId { get; set; }

    public bool ShowRequestId => !string.IsNullOrEmpty(RequestId);

    public void OnGet()
    {
        RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier;
    }
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```cshtml
@page
@model ErrorModel
@{
    ViewData["Title"] = "Error";
}

<h1 class="text-danger">Error.</h1>
<h2 class="text-danger">An error occurred while processing your request.</h2>

@if (Model.ShowRequestId)
{
    <p>
        <strong>Request ID:</strong> <code>@Model.RequestId</code>
    </p>
}

<h3>Development Mode</h3>
<p>
    Swapping to <strong>Development</strong> environment will display more detailed 
    information about the error that occurred.
</p>
<p>
    <strong>Development environment should not be enabled in deployed applications
    </strong>, as it can result in sensitive information from exceptions being 
    displayed to end users. For local debugging, development environment can be 
    enabled by setting the <strong>ASPNETCORE_ENVIRONMENT</strong> environment 
    variable to <strong>Development</strong>, and restarting the application.
</p>
```

*Error.cshtml.cs*:

```csharp
public class ErrorModel : PageModel
{
    public string RequestId { get; set; }

    public bool ShowRequestId => !string.IsNullOrEmpty(RequestId);

    [ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, 
        NoStore = true)]
    public void OnGet()
    {
        RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier;
    }
}
```

::: moniker-end

## <a name="exception-handling-code"></a>例外処理コード

例外処理ページのコードは例外をスローすることがあります。 実稼働のエラー ページは純粋に静的なコンテンツで構成することをお勧めします。

また、応答のヘッダーが送信されたら、次のことに注意してください。

* アプリで応答の状態コードを変更できません。
* すべての例外ページやハンドラーを実行できません。 応答は完了している必要があります。あるいは、接続が中止となっている必要があります。

## <a name="server-exception-handling"></a>サーバー例外処理

アプリ内の例外処理ロジックに加えて、[サーバー実装](xref:fundamentals/servers/index)でも一部の例外を処理できます。 応答ヘッダーの送信前にサーバーで例外がキャッチされると、サーバーによって "*500 - 内部サーバー エラーです*" 応答が応答本文なしで送信されます。 応答ヘッダーの送信後にサーバーで例外がキャッチされた場合、サーバーは接続を閉じます。 アプリで処理されない要求はサーバーで処理されます。 例外が発生すると、サーバーの例外処理で処理されます。 カスタムのエラー ページ、例外処理ミドルウェア、フィルターを構成しても、この動作は変わりません。

## <a name="startup-exception-handling"></a>起動時の例外処理

アプリの起動中に起こる例外はホスティング層だけが処理できます。 [Web ホスト](xref:fundamentals/host/web-host)を使うと、`captureStartupErrors` と `detailedErrors` キーを使って、[起動中のエラーに対するホストの動作方法を構成する](xref:fundamentals/host/web-host#detailed-errors)ことができます。

ホスティングでは、ホスト アドレス/ポート バインディング後にエラーが発生した場合のみ、キャプチャされた起動時エラーに対してエラー ページを表示できます。 なんらかの理由でいずれかのバインドが失敗した場合:

* ホスティング レイヤーにより重大な例外がログに記録されます。
* dotnet プロセスがクラッシュします。
* アプリを [Kestrel](xref:fundamentals/servers/kestrel) サーバー上で実行している場合、エラー ページが表示されません。

[IIS](/iis) または [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) 上で実行している場合、プロセスを開始できなければ、[ASP.NET Core モジュール](xref:host-and-deploy/aspnet-core-module)から "*502.5 - 処理エラー*" が返されます。 詳細については、「<xref:host-and-deploy/iis/troubleshoot>」を参照してください。 Azure App Service での起動の問題のトラブルシューティングについては、<xref:host-and-deploy/azure-apps/troubleshoot> を参照してください。

## <a name="aspnet-core-mvc-error-handling"></a>ASP.NET Core MVC エラー処理

[MVC](xref:mvc/overview) アプリには、エラー処理の追加オプションがいくつかあります。例外フィルターの構成やモデル検証の実行などです。

### <a name="exception-filters"></a>例外フィルター

例外フィルターはグローバルに構成するか、MVC アプリのコントローラーまたはアクション単位で構成できます。 このようなフィルターはコントローラー アクションや別のフィルターの実行中に発生する未処理の例外を処理します。 これらのフィルターは、それ以外の場合には呼び出されません。 詳細については、<xref:mvc/controllers/filters> をご覧ください。

> [!TIP]
> 例外フィルターは、MVC アクション内で発生した例外をトラップする場合は便利ですが、エラー処理ミドルウェアほど柔軟ではありません。 ミドルウェアの使用をお勧めします。 選択された MVC アクションに応じて "*異なる方法で*" エラー処理を実行する必要がある場合にのみ、フィルターを使用します。

### <a name="handle-model-state-errors"></a>モデルの状態エラーを処理する

[モデル検証](xref:mvc/models/validation)は各コントローラー アクションを呼び出す前に発生します。[ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) を検査して適切に対処するのは、アクション メソッドの仕事です。

一部のアプリでは、[モデル検証](xref:mvc/models/validation)エラーを処理するための標準的な規則に従うことを選択します。その場合、そのようなポリシーを実装する場所として[フィルター](xref:mvc/controllers/filters)が適していることがあります。 無効なモデル状態でアクションの動作をテストしてください。 詳細については、「<xref:mvc/controllers/testing>」を参照してください。

## <a name="additional-resources"></a>その他の技術情報

* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-apps/troubleshoot>
