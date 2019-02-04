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
# <a name="access-httpcontext-in-aspnet-core"></a>ASP.NET Core で HttpContext にアクセスする

ASP.NET Core アプリでは、[IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) インターフェイスとその既定の実装である [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor) を使用して、`HttpContext` にアクセスします。 `IHttpContextAccessor` を使用する必要があるのは、サービス内の `HttpContext` にアクセスする必要がある場合のみです。

::: moniker range=">= aspnetcore-2.0"

## <a name="use-httpcontext-from-razor-pages"></a>Razor Pages から HttpContext を使用する

Razor Pages の [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) では、[HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext) プロパティが公開されます。

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

## <a name="use-httpcontext-from-a-razor-view"></a>Razor ビューから HttpContext を使用する

Razor ビューでは、[RazorPage.Context](/dotnet/api/microsoft.aspnetcore.mvc.razor.razorpage.context#Microsoft_AspNetCore_Mvc_Razor_RazorPage_Context) プロパティを使用して、ビューに直接 `HttpContext` が公開されます。 次の例では、Windows 認証を使用して、イントラネット アプリで現在のユーザー名を取得します。

```cshtml
@{
    var username = Context.User.Identity.Name;
}
```

## <a name="use-httpcontext-from-a-controller"></a>コントローラーから HttpContext を使用する

コントローラーでは [ControllerBase.HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.httpcontext) プロパティが公開されます。

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

## <a name="use-httpcontext-from-middleware"></a>ミドルウェアから HttpContext を使用する

カスタム ミドルウェア コンポーネントを使用する場合、`HttpContext` は `Invoke` メソッドまたは `InvokeAsync` メソッドに渡され、ミドルウェアを構成する際にアクセスできます。

```csharp
public class MyCustomMiddleware
{
    public Task InvokeAsync(HttpContext context)
    {
        // Middleware initialization optionally using HttpContext
    }
}
```

## <a name="use-httpcontext-from-custom-components"></a>カスタム コンポーネントから HttpContext を使用する

`HttpContext` へのアクセスを必要とするその他のフレームワークおよびカスタム コンポーネントに対して推奨される方法は、組み込みの[依存関係の挿入](xref:fundamentals/dependency-injection)コンテナーを使用して依存関係を登録することです。 依存関係の挿入コンテナーは、それぞれのコンストラクター内で `IHttpContextAccessor` を依存関係として宣言するすべてのクラスに、これを提供します。

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

次に例を示します。

* `UserRepository` は `IHttpContextAccessor` に対する依存関係を宣言します。
* 依存関係の挿入で依存関係のチェーンが解決され、`UserRepository` のインスタンスが作成されると、依存関係が提供されます。

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
