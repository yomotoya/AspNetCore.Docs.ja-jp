---
title: ASP.NET Core で HttpContext にアクセスする
author: coderandhiker
description: ASP.NET Core で HttpContext にアクセスする方法について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 07/20/2018
uid: fundamentals/httpcontext
ms.openlocfilehash: b1ff80943db1788b465accd51c70a3c3a3462d5c
ms.sourcegitcommit: a3675f9704e4e73ecc7cbbbf016a13d2a5c4d725
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/23/2018
ms.locfileid: "39202708"
---
# <a name="access-httpcontext-in-aspnet-core"></a><span data-ttu-id="991d7-103">ASP.NET Core で HttpContext にアクセスする</span><span class="sxs-lookup"><span data-stu-id="991d7-103">Access HttpContext in ASP.NET Core</span></span>

<span data-ttu-id="991d7-104">ASP.NET Core アプリでは、[IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) インターフェイスとその既定の実装である [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor) を使用して、`HttpContext` にアクセスします。</span><span class="sxs-lookup"><span data-stu-id="991d7-104">ASP.NET Core apps access the `HttpContext` through the [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) interface and its default implementation [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor).</span></span>

::: moniker range=">= aspnetcore-2.0"

## <a name="use-httpcontext-from-razor-pages"></a><span data-ttu-id="991d7-105">Razor Pages から HttpContext を使用する</span><span class="sxs-lookup"><span data-stu-id="991d7-105">Use HttpContext from Razor Pages</span></span>

<span data-ttu-id="991d7-106">Razor Pages の [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) では、[HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext) プロパティが公開されます。</span><span class="sxs-lookup"><span data-stu-id="991d7-106">The Razor Pages [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) exposes the [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext) property:</span></span>

```csharp
public class AboutModel : PageModel
{
    public string Message { get; set; }

    public void OnGet()
    {
        Message = HttpContext.Request.PathBase;
    }
}
```

::: moniker-end

## <a name="use-httpcontext-from-a-controller"></a><span data-ttu-id="991d7-107">コントローラーから HttpContext を使用する</span><span class="sxs-lookup"><span data-stu-id="991d7-107">Use HttpContext from a controller</span></span>

<span data-ttu-id="991d7-108">コントローラーでは [ControllerBase.HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.httpcontext) プロパティが公開されます。</span><span class="sxs-lookup"><span data-stu-id="991d7-108">Controllers expose the [ControllerBase.HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.httpcontext) property:</span></span>

```csharp
public class HomeController : Controller
{
    public IActionResult About()
    {
        var pathBase = HttpContext.Request.PathBase;
        // Do something with the PathBase.

        return View();
    }
}
```

## <a name="use-httpcontext-from-middleware"></a><span data-ttu-id="991d7-109">ミドルウェアから HttpContext を使用する</span><span class="sxs-lookup"><span data-stu-id="991d7-109">Use HttpContext from middleware</span></span>

<span data-ttu-id="991d7-110">カスタム ミドルウェア コンポーネントを使用する場合、`HttpContext` は `Invoke` メソッドまたは `InvokeAsync` メソッドに渡され、ミドルウェアを構成する際にアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="991d7-110">When working with custom middleware components, `HttpContext` is passed into the `Invoke` or `InvokeAsync` method and can be accessed when the middleware is configured:</span></span>

```csharp
public class MyCustomMiddleware
{
    public Task InvokeAsync(HttpContext context)
    {
        // Middleware initialization optionally using HttpContext
    }
}
```

## <a name="use-httpcontext-from-custom-components"></a><span data-ttu-id="991d7-111">カスタム コンポーネントから HttpContext を使用する</span><span class="sxs-lookup"><span data-stu-id="991d7-111">Use HttpContext from custom components</span></span>

<span data-ttu-id="991d7-112">`HttpContext` へのアクセスを必要とするその他のフレームワークおよびカスタム コンポーネントに対して推奨される方法は、組み込みの[依存関係の挿入](xref:fundamentals/dependency-injection)コンテナーを使用して依存関係を登録することです。</span><span class="sxs-lookup"><span data-stu-id="991d7-112">For other framework and custom components that require access to `HttpContext`, the recommended approach is to register a dependency using the built-in [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="991d7-113">依存関係の挿入コンテナーは、それぞれのコンストラクター内で `IHttpContextAccessor` を依存関係として宣言するすべてのクラスに、これを提供します。</span><span class="sxs-lookup"><span data-stu-id="991d7-113">The dependency injection container supplies the `IHttpContextAccessor` to any classes that declare it as a dependency in their constructors.</span></span>

::: moniker range=">= aspnetcore-2.1"

```csharp
public void ConfigureServices(IServiceCollection services)
{
     services.AddMvc()
         .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
     services.AddHttpContextAccessor();
     services.AddTransient<IUserRepository, UserRepository>();
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
     services.AddMvc();
     services.AddSingleton<IHttpContextAccessor, HttpContextAccessor>();
     services.AddTransient<IUserRepository, UserRepository>();
}
```

::: moniker-end

<span data-ttu-id="991d7-114">前の例の場合:</span><span class="sxs-lookup"><span data-stu-id="991d7-114">In the preceding example:</span></span>

* <span data-ttu-id="991d7-115">`UserRepository` は `IHttpContextAccessor` に対する依存関係を宣言します。</span><span class="sxs-lookup"><span data-stu-id="991d7-115">`UserRepository` declares its dependency on `IHttpContextAccessor`.</span></span>
* <span data-ttu-id="991d7-116">依存関係の挿入で依存関係のチェーンが解決され、`UserRepository` のインスタンスが作成されると、依存関係が提供されます。</span><span class="sxs-lookup"><span data-stu-id="991d7-116">The dependency is supplied when dependency injection resolves the dependency chain and creates an instance of `UserRepository`.</span></span>

```csharp
public class UserRepository : IUserRepository
{
    private readonly IHttpContextAccessor _httpContextAccessor;

    public UserRepository(IHttpContextAccessor httpContextAccessor)
    {
        _httpContextAccessor = httpContextAccessor;
    }

    public void LogCurrentUser()
    {
        var username = _httpContextAccessor.HttpContext.User.Identity.Name;
        service.LogAccessRequest(username);
    }
}
```
