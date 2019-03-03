---
title: カスタム ASP.NET Core ミドルウェアを記述する
author: rick-anderson
description: カスタム ASP.NET Core ミドルウェアを記述する方法について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 02/14/2019
uid: fundamentals/middleware/write
ms.openlocfilehash: 2c5577394a10370d92c8a83f9d806b63f3245c8b
ms.sourcegitcommit: b3894b65e313570e97a2ab78b8addd22f427cac8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/23/2019
ms.locfileid: "56744912"
---
# <a name="write-custom-aspnet-core-middleware"></a><span data-ttu-id="6b977-103">カスタム ASP.NET Core ミドルウェアを記述する</span><span class="sxs-lookup"><span data-stu-id="6b977-103">Write custom ASP.NET Core middleware</span></span>

<span data-ttu-id="6b977-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)、[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="6b977-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="6b977-105">ミドルウェアとは、要求と応答を処理するために、アプリのパイプラインに組み込まれたソフトウェアです。</span><span class="sxs-lookup"><span data-stu-id="6b977-105">Middleware is software that's assembled into an app pipeline to handle requests and responses.</span></span> <span data-ttu-id="6b977-106">ASP.NET Core からは、組み込みミドルウェア コンポーネントが豊富に提供されますが、カスタム ミドルウェアを記述したほうが良い場合もあります。</span><span class="sxs-lookup"><span data-stu-id="6b977-106">ASP.NET Core provides a rich set of built-in middleware components, but in some scenarios you might want to write a custom middleware.</span></span>

## <a name="middleware-class"></a><span data-ttu-id="6b977-107">ミドルウェア クラス</span><span class="sxs-lookup"><span data-stu-id="6b977-107">Middleware class</span></span>

<span data-ttu-id="6b977-108">ミドルウェアは一般に、クラスにカプセル化され、拡張メソッドを使用して公開されます。</span><span class="sxs-lookup"><span data-stu-id="6b977-108">Middleware is generally encapsulated in a class and exposed with an extension method.</span></span> <span data-ttu-id="6b977-109">クエリ文字列から現在の要求のカルチャを設定する次のようなミドルウェアを考慮します。</span><span class="sxs-lookup"><span data-stu-id="6b977-109">Consider the following middleware, which sets the culture for the current request from a query string:</span></span>

[!code-csharp[](index/snapshot/Culture/StartupCulture.cs?name=snippet1)]

<span data-ttu-id="6b977-110">上のサンプル コードを使って、ミドルウェア コンポーネントの作成方法を示します。</span><span class="sxs-lookup"><span data-stu-id="6b977-110">The preceding sample code is used to demonstrate creating a middleware component.</span></span> <span data-ttu-id="6b977-111">ASP.NET Core に組み込まれているローカライズのサポートについては、「<xref:fundamentals/localization>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="6b977-111">For ASP.NET Core's built-in localization support, see <xref:fundamentals/localization>.</span></span>

<span data-ttu-id="6b977-112">カルチャを渡すことによって、ミドルウェアをテストできます。</span><span class="sxs-lookup"><span data-stu-id="6b977-112">You can test the middleware by passing in the culture.</span></span> <span data-ttu-id="6b977-113">たとえば、`http://localhost:7997/?culture=no` のようにします。</span><span class="sxs-lookup"><span data-stu-id="6b977-113">For example, `http://localhost:7997/?culture=no`.</span></span>

<span data-ttu-id="6b977-114">次のコードは、ミドルウェアのデリゲートをクラスに移動します。</span><span class="sxs-lookup"><span data-stu-id="6b977-114">The following code moves the middleware delegate to a class:</span></span>

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddleware.cs)]

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="6b977-115">ミドルウェア `Task` メソッドの名前は `Invoke` である必要があります。</span><span class="sxs-lookup"><span data-stu-id="6b977-115">The middleware `Task` method's name must be `Invoke`.</span></span> <span data-ttu-id="6b977-116">ASP.NET Core 2.0 以降では、名前は `Invoke` でも `InvokeAsync` でも構いません。</span><span class="sxs-lookup"><span data-stu-id="6b977-116">In ASP.NET Core 2.0 or later, the name can be either `Invoke` or `InvokeAsync`.</span></span>

::: moniker-end

## <a name="middleware-extension-method"></a><span data-ttu-id="6b977-117">ミドルウェア拡張メソッド</span><span class="sxs-lookup"><span data-stu-id="6b977-117">Middleware extension method</span></span>

<span data-ttu-id="6b977-118">次の拡張メソッドは、<xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> を介してミドルウェアを公開します。</span><span class="sxs-lookup"><span data-stu-id="6b977-118">The following extension method exposes the middleware through <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>:</span></span>

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddlewareExtensions.cs)]

<span data-ttu-id="6b977-119">次のコードは、`Startup.Configure` からミドルウェアを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="6b977-119">The following code calls the middleware from `Startup.Configure`:</span></span>

[!code-csharp[](index/snapshot/Culture/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="6b977-120">ミドルウェアは、コンストラクターで依存関係を公開することによって、[明示的な依存関係の原則](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)に従う必要があります。</span><span class="sxs-lookup"><span data-stu-id="6b977-120">Middleware should follow the [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies) by exposing its dependencies in its constructor.</span></span> <span data-ttu-id="6b977-121">ミドルウェアは、"*アプリケーションの有効期間*" ごとに 1 回構築されます。</span><span class="sxs-lookup"><span data-stu-id="6b977-121">Middleware is constructed once per *application lifetime*.</span></span> <span data-ttu-id="6b977-122">要求内でミドルウェアとサービスを共有する必要がある場合は、「[要求ごとの依存関係](#per-request-dependencies)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="6b977-122">See the [Per-request dependencies](#per-request-dependencies) section if you need to share services with middleware within a request.</span></span>

<span data-ttu-id="6b977-123">ミドルウェア コンポーネントは、コンストラクター パラメーターにより、[依存関係の挿入 (DI)](xref:fundamentals/dependency-injection) から依存関係を解決できます。</span><span class="sxs-lookup"><span data-stu-id="6b977-123">Middleware components can resolve their dependencies from [dependency injection (DI)](xref:fundamentals/dependency-injection) through constructor parameters.</span></span> <span data-ttu-id="6b977-124">[UseMiddleware&lt;T&gt;](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) は、追加パラメーターを直接受け入れることもできます。</span><span class="sxs-lookup"><span data-stu-id="6b977-124">[UseMiddleware&lt;T&gt;](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) can also accept additional parameters directly.</span></span>

## <a name="per-request-dependencies"></a><span data-ttu-id="6b977-125">要求ごとの依存関係</span><span class="sxs-lookup"><span data-stu-id="6b977-125">Per-request dependencies</span></span>

<span data-ttu-id="6b977-126">ミドルウェアは要求ごとではなくアプリの起動時に構築されるため、ミドルウェアのコンストラクターによって使われる "*スコープ*" 有効期間のサービスは、各要求の間に、依存関係によって挿入される他の種類と共有されません。</span><span class="sxs-lookup"><span data-stu-id="6b977-126">Because middleware is constructed at app startup, not per-request, *scoped* lifetime services used by middleware constructors aren't shared with other dependency-injected types during each request.</span></span> <span data-ttu-id="6b977-127">ミドルウェアとその他の種類の間で "*スコープ*" サービスを共有する必要がある場合は、これらのサービスを `Invoke` メソッドのシグネチャに追加します。</span><span class="sxs-lookup"><span data-stu-id="6b977-127">If you must share a *scoped* service between your middleware and other types, add these services to the `Invoke` method's signature.</span></span> <span data-ttu-id="6b977-128">`Invoke` メソッドは、DI によって設定される追加のパラメーターを受け取ることができます。</span><span class="sxs-lookup"><span data-stu-id="6b977-128">The `Invoke` method can accept additional parameters that are populated by DI:</span></span>

```csharp
public class CustomMiddleware
{
    private readonly RequestDelegate _next;

    public CustomMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    // IMyScopedService is injected into Invoke
    public async Task Invoke(HttpContext httpContext, IMyScopedService svc)
    {
        svc.MyProperty = 1000;
        await _next(httpContext);
    }
}
```

## <a name="additional-resources"></a><span data-ttu-id="6b977-129">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="6b977-129">Additional resources</span></span>

* <xref:fundamentals/middleware/index>
* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>
