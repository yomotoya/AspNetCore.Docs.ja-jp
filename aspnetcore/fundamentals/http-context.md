---
title: ASP.NET Core で HttpContext にアクセスする
author: coderandhiker
description: ASP.NET Core で HttpContext にアクセスする方法について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 07/27/2018
uid: fundamentals/httpcontext
ms.openlocfilehash: babc637cdec8590ac14f7924c17e862e5b2f6a81
ms.sourcegitcommit: d22b3c23c45a076c4f394a70b1c8df2fbcdf656d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/31/2019
ms.locfileid: "55428487"
---
# <a name="access-httpcontext-in-aspnet-core"></a><span data-ttu-id="cfabc-103">ASP.NET Core で HttpContext にアクセスする</span><span class="sxs-lookup"><span data-stu-id="cfabc-103">Access HttpContext in ASP.NET Core</span></span>

<span data-ttu-id="cfabc-104">ASP.NET Core アプリでは、[IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) インターフェイスとその既定の実装である [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor) を使用して、`HttpContext` にアクセスします。</span><span class="sxs-lookup"><span data-stu-id="cfabc-104">ASP.NET Core apps access the `HttpContext` through the [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) interface and its default implementation [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor).</span></span> <span data-ttu-id="cfabc-105">`IHttpContextAccessor` を使用する必要があるのは、サービス内の `HttpContext` にアクセスする必要がある場合のみです。</span><span class="sxs-lookup"><span data-stu-id="cfabc-105">It's only necessary to use `IHttpContextAccessor` when you need access to the `HttpContext` inside a service.</span></span>

::: moniker range=">= aspnetcore-2.0"

## <a name="use-httpcontext-from-razor-pages"></a><span data-ttu-id="cfabc-106">Razor Pages から HttpContext を使用する</span><span class="sxs-lookup"><span data-stu-id="cfabc-106">Use HttpContext from Razor Pages</span></span>

<span data-ttu-id="cfabc-107">Razor Pages の [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) では、[HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext) プロパティが公開されます。</span><span class="sxs-lookup"><span data-stu-id="cfabc-107">The Razor Pages [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) exposes the [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext) property:</span></span>

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

## <a name="use-httpcontext-from-a-razor-view"></a><span data-ttu-id="cfabc-108">Razor ビューから HttpContext を使用する</span><span class="sxs-lookup"><span data-stu-id="cfabc-108">Use HttpContext from a Razor view</span></span>

<span data-ttu-id="cfabc-109">Razor ビューでは、[RazorPage.Context](/dotnet/api/microsoft.aspnetcore.mvc.razor.razorpage.context#Microsoft_AspNetCore_Mvc_Razor_RazorPage_Context) プロパティを使用して、ビューに直接 `HttpContext` が公開されます。</span><span class="sxs-lookup"><span data-stu-id="cfabc-109">Razor views expose the `HttpContext` directly via a [RazorPage.Context](/dotnet/api/microsoft.aspnetcore.mvc.razor.razorpage.context#Microsoft_AspNetCore_Mvc_Razor_RazorPage_Context) property on the view.</span></span> <span data-ttu-id="cfabc-110">次の例では、Windows 認証を使用して、イントラネット アプリで現在のユーザー名を取得します。</span><span class="sxs-lookup"><span data-stu-id="cfabc-110">The following example retrieves the current username in an Intranet app using Windows Authentication:</span></span>

```cshtml
@{
    var username = Context.User.Identity.Name;
}
```

## <a name="use-httpcontext-from-a-controller"></a><span data-ttu-id="cfabc-111">コントローラーから HttpContext を使用する</span><span class="sxs-lookup"><span data-stu-id="cfabc-111">Use HttpContext from a controller</span></span>

<span data-ttu-id="cfabc-112">コントローラーでは [ControllerBase.HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.httpcontext) プロパティが公開されます。</span><span class="sxs-lookup"><span data-stu-id="cfabc-112">Controllers expose the [ControllerBase.HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.httpcontext) property:</span></span>

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

## <a name="use-httpcontext-from-middleware"></a><span data-ttu-id="cfabc-113">ミドルウェアから HttpContext を使用する</span><span class="sxs-lookup"><span data-stu-id="cfabc-113">Use HttpContext from middleware</span></span>

<span data-ttu-id="cfabc-114">カスタム ミドルウェア コンポーネントを使用する場合、`HttpContext` は `Invoke` メソッドまたは `InvokeAsync` メソッドに渡され、ミドルウェアを構成する際にアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="cfabc-114">When working with custom middleware components, `HttpContext` is passed into the `Invoke` or `InvokeAsync` method and can be accessed when the middleware is configured:</span></span>

```csharp
public class MyCustomMiddleware
{
    public Task InvokeAsync(HttpContext context)
    {
        // Middleware initialization optionally using HttpContext
    }
}
```

## <a name="use-httpcontext-from-custom-components"></a><span data-ttu-id="cfabc-115">カスタム コンポーネントから HttpContext を使用する</span><span class="sxs-lookup"><span data-stu-id="cfabc-115">Use HttpContext from custom components</span></span>

<span data-ttu-id="cfabc-116">`HttpContext` へのアクセスを必要とするその他のフレームワークおよびカスタム コンポーネントに対して推奨される方法は、組み込みの[依存関係の挿入](xref:fundamentals/dependency-injection)コンテナーを使用して依存関係を登録することです。</span><span class="sxs-lookup"><span data-stu-id="cfabc-116">For other framework and custom components that require access to `HttpContext`, the recommended approach is to register a dependency using the built-in [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="cfabc-117">依存関係の挿入コンテナーは、それぞれのコンストラクター内で `IHttpContextAccessor` を依存関係として宣言するすべてのクラスに、これを提供します。</span><span class="sxs-lookup"><span data-stu-id="cfabc-117">The dependency injection container supplies the `IHttpContextAccessor` to any classes that declare it as a dependency in their constructors.</span></span>

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

<span data-ttu-id="cfabc-118">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="cfabc-118">In the following example:</span></span>

* <span data-ttu-id="cfabc-119">`UserRepository` は `IHttpContextAccessor` に対する依存関係を宣言します。</span><span class="sxs-lookup"><span data-stu-id="cfabc-119">`UserRepository` declares its dependency on `IHttpContextAccessor`.</span></span>
* <span data-ttu-id="cfabc-120">依存関係の挿入で依存関係のチェーンが解決され、`UserRepository` のインスタンスが作成されると、依存関係が提供されます。</span><span class="sxs-lookup"><span data-stu-id="cfabc-120">The dependency is supplied when dependency injection resolves the dependency chain and creates an instance of `UserRepository`.</span></span>

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
