---
title: "ASP.NET Core でのエラー処理"
author: ardalis
description: "ASP.NET Core アプリケーションのエラーを処理する方法を検出します。"
keywords: "ASP.NET Core、エラー処理、例外処理"
ms.author: tdykstra
manager: wpickett
ms.date: 11/30/2016
ms.topic: article
ms.assetid: 4db51023-c8a6-4119-bbbe-3917e272c260
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/error-handling
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: de2ba0ff9ad17c198c06b510ecfb49f808721bdf
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="introduction-to-error-handling-in-aspnet-core"></a>ASP.NET Core でのエラー処理の概要

によって[Steve Smith](https://ardalis.com/)と[Tom Dykstra](https://github.com/tdykstra/)

この記事では、ASP.NET Core アプリケーションでエラーを処理する一般的な appoaches について説明します。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="the-developer-exception-page"></a>開発者の例外ページ

例外に関する詳細情報を表示するページを表示するアプリを構成するのには、インストール、 `Microsoft.AspNetCore.Diagnostics` NuGet パッケージ化し、行を追加して、[スタートアップ クラスでメソッドを構成する](startup.md):

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

Put`UseDeveloperExceptionPage`など、例外をキャッチするすべてのミドルウェアの前に`app.UseMvc`です。

>[!WARNING]
> 開発者の例外のページを有効にする**、アプリが開発環境で実行されている場合にのみ**です。 実稼働環境でアプリを実行するときに、パブリックに詳細な例外情報を共有します。 [環境の構成について](environments.md)です。

開発者の例外のページを表示する設定した環境でサンプル アプリケーションを実行`Development`、し、追加`?throw=true`アプリの基本 URL にします。 このページには、例外と、要求に関する情報をいくつかのタブが含まれます。 最初のタブには、スタック トレースが含まれています。 

![スタック トレース](error-handling/_static/developer-exception-page.png)

次のタブは、存在する場合に、クエリ文字列パラメーターを示します。

![クエリ文字列パラメーター](error-handling/_static/developer-exception-page-query.png)

この要求は、クッキーがありませんでしたが、サポートしていた場合に表示される、 **Cookie**タブです。最後のタブが渡されましたが、ヘッダーを表示できます。

![ヘッダー](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a>ページの処理、カスタム例外を構成します。

例外ハンドラーで、アプリが実行されていないときに使用するページを構成することをお勧め、`Development`環境。

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

MVC アプリケーションでない明示的に装飾 HTTP メソッドの属性を持つエラー ハンドラーのアクション メソッドなど`HttpGet`です。 明示的な動詞を使用して防ぐことがいくつかの要求メソッド。

```csharp
[Route("/Error")]
public IActionResult Index()
{
    // Handle error here
}
```

## <a name="configuring-status-code-pages"></a>ステータス コード ページを構成します。

既定では、アプリが提供されません豊富なステータス コード ページの HTTP ステータス コード 500 (内部サーバー エラー) または 404 (Not Found) など。 構成することができます、`StatusCodePagesMiddleware`に行を追加して、`Configure`メソッド。

```csharp
app.UseStatusCodePages();
```

既定では、このミドルウェアには、404 などの一般的な状態コードに対して、単純なテキストのみのハンドラーが追加されます。

![404 ページ](error-handling/_static/default-404-status-code.png)

ミドルウェアは、いくつかの種類の拡張メソッドをサポートします。 ラムダ式を 1 つは、他方はコンテンツの種類と形式の文字列を使用します。

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

リダイレクトの拡張メソッドもあります。 302 ステータス コードをクライアントに送信いずれかと、元の状態コードをクライアントに返しますがもリダイレクト URL のハンドラーを実行いずれか。

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

特定の要求のステータス コード ページを無効にする必要がある場合は、これを行うことができます。

```csharp
var statusCodePagesFeature = context.Features.Get<IStatusCodePagesFeature>();
if (statusCodePagesFeature != null)
{
  statusCodePagesFeature.Enabled = false;
}
```

## <a name="exception-handling-code"></a>例外処理コード

例外処理のページ内のコードは、例外をスローできます。 多くの場合、純粋に静的なコンテンツで構成する実稼働のエラー ページのことをお勧めします。

また、なることの応答ヘッダーが送信されると、応答のステータス コードを変更することはできません実行することもできる任意の例外のページやハンドラー。 応答を完了する必要があります。 または接続が中止されました。

## <a name="server-exception-handling"></a>サーバーの例外処理

アプリでは、ロジックを処理する例外に加え、[サーバー](servers/index.md)アプリをホストしているいくつかの例外処理を実行します。 サーバーは、ヘッダーが送信される前に、例外をキャッチ、サーバーは本文なしで 500 内部サーバー エラー応答を送信します。 サーバーは、ヘッダーが送信された後に例外をキャッチ、サーバーは接続を閉じます。 アプリによって処理されない要求は、サーバーによって処理されます。 サーバーの例外によって発生する例外が処理される処理します。 いずれかの構成のカスタム エラー ページまたは例外処理のミドルウェアまたはフィルターはこの動作に影響しません。

## <a name="startup-exception-handling"></a>スタートアップ例外処理

ホスト レイヤーだけでは、アプリの起動中に発生する例外を処理できます。 実行できます[エラーへの応答での起動中に、ホストの動作を構成する](hosting.md#detailed-errors)を使用して`captureStartupErrors`と`detailedErrors`キー。

ホスト アドレスとポートをバインドした後にエラーが発生する場合、ホストしていることができます、キャプチャしたスタートアップ エラーのエラー ページだけ表示されます。 任意のバインディングは、何らかの理由で失敗した場合、ホスト層 dotnet プロセスのクラッシュ、重大な例外をログに記録し、エラー ページは表示されません。

## <a name="aspnet-mvc-error-handling"></a>ASP.NET MVC のエラー処理

[MVC](../mvc/index.md)アプリにある例外フィルターを構成して、モデルの検証を実行するなどのエラーを処理するための追加オプション。

### <a name="exception-filters"></a>例外フィルター

グローバルまたは MVC アプリケーション コント ローラーごとまたは - アクションごとに、例外フィルターを構成することができます。 これらのフィルターは、コント ローラーのアクションまたは別のフィルターの実行中に発生する未処理の例外を処理し、それ以外の場合は呼び出されません。 例外フィルターの詳細について[フィルター](../mvc/controllers/filters.md)です。

>[!TIP]
> 例外フィルターは、MVC アクション内で発生する例外をトラップするのに便利ですが取り込まエラー ミドルウェアを処理する柔軟性がありません。 [全般] の場合、ミドルウェアを優先し、エラー処理のためにのみ必要があるフィルターを使用して*異なる*する MVC アクションの選択に基づいて。

### <a name="handling-model-state-errors"></a>処理のモデル状態エラー

[モデルの検証](../mvc/models/validation.md)前に呼び出される、各コント ローラー アクションを発生し、検査するアクション メソッドの責任である`ModelState.IsValid`して適切に対処します。

一部のアプリは後者モデル検証エラーを処理するため、標準の規則に従うを選択、[フィルター](../mvc/controllers/filters.md)適切な場所をこのようなポリシーを実装する場合があります。 無効なモデルの状態を持つユーザーの操作の動作をテストする必要があります。 詳細について[テスト コント ローラー ロジック](../mvc/controllers/testing.md)です。



