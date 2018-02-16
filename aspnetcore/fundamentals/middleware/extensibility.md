---
title: "ASP.NET Core でのファクトリ ベースのミドルウェアのアクティブ化"
author: guardrex
description: "ASP.NET Core で、ファクトリ ベースの厳密に型指定されたミドルウェアをアクティブ化する方法を説明します。"
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 01/29/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/middleware/extensibility
ms.openlocfilehash: 57ff9db2edbf307f2442443dc14e69b0498f7475
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/01/2018
---
# <a name="factory-based-middleware-activation-in-aspnet-core"></a><span data-ttu-id="061cd-103">ASP.NET Core でのファクトリ ベースのミドルウェアのアクティブ化</span><span class="sxs-lookup"><span data-stu-id="061cd-103">Factory-based middleware activation in ASP.NET Core</span></span>

<span data-ttu-id="061cd-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="061cd-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="061cd-105">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory)/[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) は、[ミドルウェア](xref:fundamentals/middleware/index)のアクティブ化の拡張ポイントです。</span><span class="sxs-lookup"><span data-stu-id="061cd-105">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory)/[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) is an extensibility point for [middleware](xref:fundamentals/middleware/index) activation.</span></span>

<span data-ttu-id="061cd-106">`UseMiddleware` 拡張メソッドでは、ミドルウェアの登録済みの型で `IMiddleware` が実装されているかが確認されます。</span><span class="sxs-lookup"><span data-stu-id="061cd-106">`UseMiddleware` extension methods check if a middleware's registered type implements `IMiddleware`.</span></span> <span data-ttu-id="061cd-107">実装されている場合、規則に基づくミドルウェアのライセンス認証ロジックを使用する代わりに、コンテナーに登録されている `IMiddlewareFactory` インスタンスが `IMiddleware` の実装の解決に使用されます。</span><span class="sxs-lookup"><span data-stu-id="061cd-107">If it does, the `IMiddlewareFactory` instance registered in the container is used to resolve the `IMiddleware` implementation instead of using the convention-based middleware activation logic.</span></span> <span data-ttu-id="061cd-108">ミドルウェアは、アプリのサービス コンテナーで、スコープ化されたまたは一時的なサービスとして登録されています。</span><span class="sxs-lookup"><span data-stu-id="061cd-108">The middleware is registered as a scoped or transient service in the app's service container.</span></span>

<span data-ttu-id="061cd-109">利点:</span><span class="sxs-lookup"><span data-stu-id="061cd-109">Benefits:</span></span>

* <span data-ttu-id="061cd-110">要求ごとにライセンス認証 (スコープ化されたサービスの挿入)</span><span class="sxs-lookup"><span data-stu-id="061cd-110">Activation per request (injection of scoped services)</span></span>
* <span data-ttu-id="061cd-111">ミドルウェアの厳密な型指定</span><span class="sxs-lookup"><span data-stu-id="061cd-111">Strong typing of middleware</span></span>

<span data-ttu-id="061cd-112">`IMiddleware` は要求ごとにアクティブ化されているので、スコープ化されたサービスを、ミドルウェアのコンストラクターに挿入することができます。</span><span class="sxs-lookup"><span data-stu-id="061cd-112">`IMiddleware` is activated per request, so scoped services can be injected into the middleware's constructor.</span></span>

<span data-ttu-id="061cd-113">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="061cd-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="061cd-114">このサンプル アプリでは、次でアクティブ化されるミドルウェアを示します。</span><span class="sxs-lookup"><span data-stu-id="061cd-114">The sample app demonstrates middleware activated by:</span></span>

* <span data-ttu-id="061cd-115">規則 (`ConventionalMiddleware`).</span><span class="sxs-lookup"><span data-stu-id="061cd-115">Convention (`ConventionalMiddleware`).</span></span> <span data-ttu-id="061cd-116">従来のミドルウェアのアクティブ化については、「[ミドルウェア](xref:fundamentals/middleware/index)」のトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="061cd-116">For more information on conventional middleware activation, see the [Middleware](xref:fundamentals/middleware/index) topic.</span></span>
* <span data-ttu-id="061cd-117">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) の実装 (`IMiddlewareMiddleware`)。</span><span class="sxs-lookup"><span data-stu-id="061cd-117">An [IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) implementation (`IMiddlewareMiddleware`).</span></span> <span data-ttu-id="061cd-118">既定の [MiddlewareFactory クラス](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory)によってミドルウェアはアクティブ化されます。</span><span class="sxs-lookup"><span data-stu-id="061cd-118">The default [MiddlewareFactory class](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory) activates the middleware.</span></span>

<span data-ttu-id="061cd-119">ミドルウェアの実装は同一に機能し、クエリ文字列パラメーターで提供された値を記録します (`key`)。</span><span class="sxs-lookup"><span data-stu-id="061cd-119">The middleware implementations function identically and record the value provided by a query string parameter (`key`).</span></span> <span data-ttu-id="061cd-120">ミドルウェアは、メモリ内データベースにクエリ文字列値を記録するのに、挿入されたデータベース コンテキスト (スコープ化されたサービス) を使用します。</span><span class="sxs-lookup"><span data-stu-id="061cd-120">The middlewares use an injected database context (a scoped service) to record the query string value in an in-memory database.</span></span>

## <a name="imiddleware"></a><span data-ttu-id="061cd-121">IMiddleware</span><span class="sxs-lookup"><span data-stu-id="061cd-121">IMiddleware</span></span>

<span data-ttu-id="061cd-122">[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) では、アプリの要求パイプライン用にミドルウェアが定義されます。</span><span class="sxs-lookup"><span data-stu-id="061cd-122">[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) defines middleware for the app's request pipeline.</span></span> <span data-ttu-id="061cd-123">要求は、[InvokeAsync (HttpContext, RequestDelegate)](/dotnet/api/microsoft.aspnetcore.http.imiddleware.invokeasync#Microsoft_AspNetCore_Http_IMiddleware_InvokeAsync_Microsoft_AspNetCore_Http_HttpContext_Microsoft_AspNetCore_Http_RequestDelegate_) メソッドによって処理され、ミドルウェアの実行を表す `Task` が返されます。</span><span class="sxs-lookup"><span data-stu-id="061cd-123">The [InvokeAsync(HttpContext, RequestDelegate)](/dotnet/api/microsoft.aspnetcore.http.imiddleware.invokeasync#Microsoft_AspNetCore_Http_IMiddleware_InvokeAsync_Microsoft_AspNetCore_Http_HttpContext_Microsoft_AspNetCore_Http_RequestDelegate_) method handles requests and returns a `Task` that represents the execution of the middleware.</span></span>

<span data-ttu-id="061cd-124">規則でアクティブ化されるミドルウェア:</span><span class="sxs-lookup"><span data-stu-id="061cd-124">Middleware activated by convention:</span></span>

[!code-csharp[Main](extensibility/sample/Middleware/ConventionalMiddleware.cs?name=snippet1)]

<span data-ttu-id="061cd-125">`MiddlewareFactory` でアクティブ化されるミドルウェア:</span><span class="sxs-lookup"><span data-stu-id="061cd-125">Middleware activated by `MiddlewareFactory`:</span></span>

[!code-csharp[Main](extensibility/sample/Middleware/IMiddlewareMiddleware.cs?name=snippet1)]

<span data-ttu-id="061cd-126">ミドルウェア用に拡張機能が作成されます。</span><span class="sxs-lookup"><span data-stu-id="061cd-126">Extensions are created for the middlewares:</span></span>

[!code-csharp[Main](extensibility/sample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

<span data-ttu-id="061cd-127">`UseMiddleware` では、ファクトリでアクティブ化されたミドルウェアにオブジェクトを渡すことはできません。</span><span class="sxs-lookup"><span data-stu-id="061cd-127">It isn't possible to pass objects to the factory-activated middleware with `UseMiddleware`:</span></span>

```csharp
public static IApplicationBuilder UseIMiddlewareMiddleware(
    this IApplicationBuilder builder, bool option)
{
    // Passing 'option' as an argument throws a NotSupportedException at runtime.
    return builder.UseMiddleware<IMiddlewareMiddleware>(option);
}
```

<span data-ttu-id="061cd-128">ファクトリでアクティブ化されたミドルウェアは、*Startup.cs* の組み込みのコンテナーに追加されます。</span><span class="sxs-lookup"><span data-stu-id="061cd-128">The factory-activated middleware is added to the built-in container in *Startup.cs*:</span></span>

[!code-csharp[Main](extensibility/sample/Startup.cs?name=snippet1&highlight=6)]

<span data-ttu-id="061cd-129">この 2 つのミドルウェアは、要求を処理する `Configure` のパイプラインに登録されます。</span><span class="sxs-lookup"><span data-stu-id="061cd-129">Both middlewares are registered in the request processing pipeline in `Configure`:</span></span>

[!code-csharp[Main](extensibility/sample/Startup.cs?name=snippet2&highlight=12-13)]

## <a name="imiddlewarefactory"></a><span data-ttu-id="061cd-130">IMiddlewareFactory</span><span class="sxs-lookup"><span data-stu-id="061cd-130">IMiddlewareFactory</span></span>

<span data-ttu-id="061cd-131">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) では、ミドルウェアを作成するメソッドを提供します。</span><span class="sxs-lookup"><span data-stu-id="061cd-131">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) provides methods to create middleware.</span></span> <span data-ttu-id="061cd-132">ミドルウェア ファクトリの実装は、スコープ化されたサービスとして、コンテナーに登録されます。</span><span class="sxs-lookup"><span data-stu-id="061cd-132">The middleware factory implementation is registered in the container as a scoped service.</span></span>

<span data-ttu-id="061cd-133">既定の `IMiddlewareFactory` の実装である [MiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory) は、[Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) パッケージ ([参照用ソース](https://github.com/aspnet/HttpAbstractions/blob/release/2.0/src/Microsoft.AspNetCore.Http/MiddlewareFactory.cs)) にあります。</span><span class="sxs-lookup"><span data-stu-id="061cd-133">The default `IMiddlewareFactory` implementation, [MiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory), is found in the [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) package ([reference source](https://github.com/aspnet/HttpAbstractions/blob/release/2.0/src/Microsoft.AspNetCore.Http/MiddlewareFactory.cs)).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="061cd-134">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="061cd-134">Additional resources</span></span>

* [<span data-ttu-id="061cd-135">ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="061cd-135">Middleware</span></span>](xref:fundamentals/middleware/index)
