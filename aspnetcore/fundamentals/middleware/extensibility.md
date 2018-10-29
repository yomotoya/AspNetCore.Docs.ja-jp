---
title: ASP.NET Core でのファクトリ ベースのミドルウェアのアクティブ化
author: guardrex
description: ASP.NET Core で、ファクトリ ベースの厳密に型指定されたミドルウェアをアクティブ化する方法を説明します。
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/14/2018
uid: fundamentals/middleware/extensibility
ms.openlocfilehash: 566a5c5f642a3f55e72a8e070c69d2bfddaee3a1
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207200"
---
# <a name="factory-based-middleware-activation-in-aspnet-core"></a>ASP.NET Core でのファクトリ ベースのミドルウェアのアクティブ化

作成者: [Luke Latham](https://github.com/guardrex)

[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory)/[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) は、[ミドルウェア](xref:fundamentals/middleware/index)のアクティブ化の拡張ポイントです。

`UseMiddleware` 拡張メソッドでは、ミドルウェアの登録済みの型で `IMiddleware` が実装されているかが確認されます。 実装されている場合、規則に基づくミドルウェアのライセンス認証ロジックを使用する代わりに、コンテナーに登録されている `IMiddlewareFactory` インスタンスが `IMiddleware` の実装の解決に使用されます。 ミドルウェアは、アプリのサービス コンテナーで、スコープ化されたまたは一時的なサービスとして登録されています。

利点:

* 要求ごとにライセンス認証 (スコープ化されたサービスの挿入)
* ミドルウェアの厳密な型指定

`IMiddleware` は要求ごとにアクティブ化されているので、スコープ化されたサービスを、ミドルウェアのコンストラクターに挿入することができます。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/sample)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。

このサンプル アプリでは、次でアクティブ化されるミドルウェアを示します。

* 規則。 従来のミドルウェアのアクティブ化については、「[ミドルウェア](xref:fundamentals/middleware/index)」のトピックを参照してください。
* [IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) の実装。 既定の [MiddlewareFactory クラス](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory)によってミドルウェアはアクティブ化されます。

ミドルウェアの実装は同一に機能し、クエリ文字列パラメーターで提供された値を記録します (`key`)。 ミドルウェアは、メモリ内データベースにクエリ文字列値を記録するのに、挿入されたデータベース コンテキスト (スコープ化されたサービス) を使用します。

## <a name="imiddleware"></a>IMiddleware

[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) では、アプリの要求パイプライン用にミドルウェアが定義されます。 要求は、[InvokeAsync (HttpContext, RequestDelegate)](/dotnet/api/microsoft.aspnetcore.http.imiddleware.invokeasync#Microsoft_AspNetCore_Http_IMiddleware_InvokeAsync_Microsoft_AspNetCore_Http_HttpContext_Microsoft_AspNetCore_Http_RequestDelegate_) メソッドによって処理され、ミドルウェアの実行を表す `Task` が返されます。

規則でアクティブ化されるミドルウェア:

[!code-csharp[](extensibility/sample/Middleware/ConventionalMiddleware.cs?name=snippet1)]

`MiddlewareFactory` でアクティブ化されるミドルウェア:

[!code-csharp[](extensibility/sample/Middleware/FactoryActivatedMiddleware.cs?name=snippet1)]

ミドルウェア用に拡張機能が作成されます。

[!code-csharp[](extensibility/sample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

`UseMiddleware` では、ファクトリでアクティブ化されたミドルウェアにオブジェクトを渡すことはできません。

```csharp
public static IApplicationBuilder UseFactoryActivatedMiddleware(
    this IApplicationBuilder builder, bool option)
{
    // Passing 'option' as an argument throws a NotSupportedException at runtime.
    return builder.UseMiddleware<FactoryActivatedMiddleware>(option);
}
```

ファクトリでアクティブ化されたミドルウェアは、*Startup.cs* の組み込みのコンテナーに追加されます。

[!code-csharp[](extensibility/sample/Startup.cs?name=snippet1&highlight=12)]

この 2 つのミドルウェアは、要求を処理する `Configure` のパイプラインに登録されます。

[!code-csharp[](extensibility/sample/Startup.cs?name=snippet2&highlight=14-15)]

## <a name="imiddlewarefactory"></a>IMiddlewareFactory

[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) では、ミドルウェアを作成するメソッドを提供します。 ミドルウェア ファクトリの実装は、スコープ化されたサービスとして、コンテナーに登録されます。

既定の `IMiddlewareFactory` の実装である [MiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory) は、[Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) パッケージにあります。

## <a name="additional-resources"></a>その他の技術情報

* <xref:fundamentals/middleware/index>
* <xref:fundamentals/middleware/extensibility-third-party-container>
