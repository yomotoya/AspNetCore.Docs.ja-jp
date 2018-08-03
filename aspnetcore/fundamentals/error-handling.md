---
title: ASP.NET Core のエラーを処理する
author: ardalis
description: ASP.NET Core アプリでエラーを処理する方法について説明します。
ms.author: tdykstra
ms.custom: mvc
ms.date: 07/05/2018
uid: fundamentals/error-handling
ms.openlocfilehash: d7e60c0f615841461a17b093bffe5fb3f82f8616
ms.sourcegitcommit: 506a199274e9fe5fb4070b273ba94f29f14cb619
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/28/2018
ms.locfileid: "39332276"
---
# <a name="handle-errors-in-aspnet-core"></a>ASP.NET Core のエラーを処理する

執筆: [Steve Smith](https://ardalis.com/)、[Tom Dykstra](https://github.com/tdykstra/)

この記事では、ASP.NET Core アプリでエラーを処理するための一般的な手法について取り上げます。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x/ErrorHandlingSample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="the-developer-exception-page"></a>開発者例外ページ

::: moniker range=">= aspnetcore-2.1"

例外に関する詳細情報を表示するページを表示するアプリを構成するには、*開発者例外ページ*を使用します。 このページは [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) で使用できる、[Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) パッケージによって使用可能になったページです。 `Startup.Configure` メソッドに次の行を追加します。

::: moniker-end

::: moniker range="= aspnetcore-2.0"

例外に関する詳細情報を表示するページを表示するアプリを構成するには、*開発者例外ページ*を使用します。 このページは [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) で使用できる、[Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) パッケージによって使用可能になります。 `Startup.Configure` メソッドに次の行を追加します。

::: moniker-end

::: moniker range="< aspnetcore-2.0"

例外に関する詳細情報を表示するページを表示するアプリを構成するには、*開発者例外ページ*を使用します。 このページは、プロジェクト ファイルに [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) パッケージのパッケージ参照を追加することで使用できるようになります。 `Startup.Configure` メソッドに次の行を追加します。

::: moniker-end

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

`app.UseMvc` などの例外をキャッチする任意のミドルウェアの前に [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) の呼び出しを配置します。

>[!WARNING]
> **アプリを開発環境で実行するときにのみ**、開発者例外ページを有効にします。 アプリを実稼働環境で実行するときは、詳細な例外情報を公開しません。 [環境の構成についてはこちらをご覧ください](xref:fundamentals/environments)。

開発者例外ページを表示するには、環境を `Development` に設定してサンプル アプリを実行し、アプリの基礎 URL に `?throw=true` を追加します。 このページには、例外と要求に関する情報を含むタブがいくつかあります。 最初のタブにはスタック トレースがあります。

![スタック トレース](error-handling/_static/developer-exception-page.png)

次のタブには、クエリ文字列パラメーターがあればそれが表示されます。

![クエリ文字列のパラメーター](error-handling/_static/developer-exception-page-query.png)

要求に Cookie がある場合は、**[Cookie]** タブ上に表示されます。ヘッダーは、最後のタブに表示されます。

![ヘッダー](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a>カスタム例外処理ページを構成する

アプリが `Development` 環境で実行されていないときに使用する例外ハンドラー ページを構成します。

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

Razor Pages アプリでは、[dotnet new](/dotnet/core/tools/dotnet-new) Razor Pages テンプレートがエラー ページと `ErrorModel` ページ モデル クラスを *Pages* フォルダーで提供します。

MVC アプリでは、`HttpGet` など、HTTP メソッド属性でエラー ハンドラー アクション メソッドを修飾しないでください。 明示的な動詞を使用すると、要求がメソッドに届かないことがあります。 認証されていないユーザーがエラー ビューを受信できるように、メソッドへの匿名アクセスを許可します。

たとえば、次のエラー処理メソッドは [dotnet new](/dotnet/core/tools/dotnet-new) MVC テンプレートによって提供され、ホーム コントローラーに表示されます。

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel 
        { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

## <a name="configuring-status-code-pages"></a>ステータス コード ページを構成する

アプリは既定で、*404 見つかりません*などの HTTP 状態コードのリッチ状態コード ページを表示しません。 状態コード ページを表示するには、`Startup.Configure` メソッドに行を追加して状態コード ページ ミドルウェアを構成します。

```csharp
app.UseStatusCodePages();
```

既定では、状態コード ページ ミドルウェアは 404 などの一般的な状態コードにテキストのみのハンドラーを追加します。

![404 ページ](error-handling/_static/default-404-status-code.png)

このミドルウェアは、いくつかの拡張メソッドに対応しています。 1 つのメソッドがラムダ式を受け取ります。

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePages)]

別のメソッドがコンテンツの種類と書式文字列を受け取ります。

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

また、リダイレクトと再実行の拡張メソッドもあります。 リダイレクト メソッドでは、*302 Found* 状態コードをクライアントに送信し、クライアントを指定された場所の URL テンプレートにリダイレクトします。 テンプレートには、状態コードの `{0}` プレースホルダーが含まれる場合があります。 `~` で始まる URL には、先頭に基本パスが付加されています。 `~` で始まっていない URL はそのまま使用されます。

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

再実行メソッドでは、元の状態コードをクライアントに返し、代替パスを使用して要求パイプラインを再実行することで、応答本文を生成することを指定します。 このパスには、状態コードの `{0}` プレースホルダーが含まれる場合があります。

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

状態コード ページは、Razor ページ ハンドラー メソッドまたは MVC コントローラー内の特定の要求に対して無効にすることができます。 状態コード ページを無効にするには、要求の [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) コレクションから [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature) を取得し、機能が有効な場合は無効にします。

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

アプリ内のエンドポイントをポイントする `UseStatusCodePages*` オーバーロードを使用している場合、そのエンドポイントの MVC ビューまたは Razor ページを作成します。 たとえば、Razor Pages アプリの [dotnet new](/dotnet/core/tools/dotnet-new) テンプレートでは、次のページとページ モデル クラスを生成します。

*Error.cshtml*:

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

## <a name="exception-handling-code"></a>例外処理コード

例外処理ページのコードは例外をスローすることがあります。 実稼働のエラー ページは純粋に静的なコンテンツで構成することをお勧めします。

また、応答のヘッダーの送信後、応答のステータス コードを変更できなくなり、例外ページやハンドラーを実行できなくなることに注意してください。 応答は完了している必要があります。あるいは、接続が中止となっている必要があります。

## <a name="server-exception-handling"></a>サーバー例外処理

アプリの例外処理ロジックに加え、アプリをホストしている[サーバー](xref:fundamentals/servers/index)がいくつかの例外処理を実行します。 サーバーがヘッダーの送信前に例外をキャッチすると、サーバーは *500 内部サーバー エラー*を本文なしで送信します。 サーバーがヘッダーの送信後に例外をキャッチすると、サーバーは接続を閉じます。 アプリで処理されない要求はサーバーで処理されます。 例外が発生すると、サーバーの例外処理で処理されます。 カスタムのエラー ページ、例外処理ミドルウェア、フィルターを構成しても、この動作は変わりません。

## <a name="startup-exception-handling"></a>起動時の例外処理

アプリの起動中に起こる例外はホスティング層だけが処理できます。 [Web ホスト](xref:fundamentals/host/web-host)を使うと、`captureStartupErrors` と `detailedErrors` キーを利用して、[起動中のエラーに対するホストの動作を構成](xref:fundamentals/host/web-host#detailed-errors)できます。

ホスティングでは、ホスト アドレス/ポート バインディング後にエラーが発生した場合のみ、キャプチャされた起動時エラーに対してエラー ページを表示できます。 何らかの理由でバインディングに失敗した場合、ホスティング層は重要な例外をログに記録し、dotnet プロセスがクラッシュします。[Kestrel](xref:fundamentals/servers/kestrel) サーバー上でアプリが実行されている場合、エラー ページは表示されません。

[IIS](/iis) または [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) 上で実行されている場合、プロセスを開始できなければ、[ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)から *502.5 プロセス エラー*が返されます。 IIS でホストするときの起動の問題のトラブルシューティングについては、<xref:host-and-deploy/iis/troubleshoot> を参照してください。 Azure App Service での起動の問題のトラブルシューティングについては、<xref:host-and-deploy/azure-apps/troubleshoot> を参照してください。

## <a name="aspnet-mvc-error-handling"></a>ASP.NET MVC エラー処理

[MVC](xref:mvc/overview) アプリには、エラー処理の追加オプションがいくつかあります。例外フィルターの構成やモデル検証の実行などです。

### <a name="exception-filters"></a>例外フィルター

例外フィルターはグローバルに構成するか、MVC アプリのコントローラーまたはアクション単位で構成できます。 このようなフィルターはコントローラー アクションや別のフィルターの実行中に発生する未処理の例外を処理します。 これらのフィルターは、それ以外の場合には呼び出されません。 詳細については、[フィルターに関するページ](xref:mvc/controllers/filters)を参照してください。

> [!TIP]
> 例外フィルターは、MVC アクション内で発生する例外をトラップするのには適していますが、エラー処理ミドルウェアほどの柔軟性はありません。 一般的にはミドルウェアの使用を選択し、MVC アクションで選択されたのとは*異なる*方法でエラー処理を行う必要がある場合にのみフィルターを使用します。

### <a name="handling-model-state-errors"></a>モデルの状態エラーの処理

[モデル検証](xref:mvc/models/validation)は各コントローラー アクションを呼び出す前に行われます。`ModelState.IsValid` を検査し、適切に対処するのはアクション メソッドの仕事です。

一部のアプリでは、モデル検証エラーを処理するとき、標準的な規則に従います。その場合、そのようなポリシーを実装する場所として[フィルター](xref:mvc/controllers/filters)が適していることがあります。 無効なモデル状態でアクションの動作をテストしてください。 詳細については、[コントローラー ロジックのテスト](xref:mvc/controllers/testing)に関するページを参照してください。

## <a name="additional-resources"></a>その他の技術情報

* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-apps/troubleshoot>
