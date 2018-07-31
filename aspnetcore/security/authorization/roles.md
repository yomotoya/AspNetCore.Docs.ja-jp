---
title: ASP.NET Core でのロールベースの承認
author: rick-anderson
description: Authorize 属性にロールを渡すことによって、ASP.NET Core のコント ローラーとアクションのアクセスを制限する方法について説明します。
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/roles
ms.openlocfilehash: 59753b90d3196b0bc16d4963f45b995f5108bc8b
ms.sourcegitcommit: d99a8554c91f626cf5e466911cf504dcbff0e02e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/31/2018
ms.locfileid: "39356676"
---
# <a name="role-based-authorization-in-aspnet-core"></a><span data-ttu-id="8338e-103">ASP.NET Core でのロールベースの承認</span><span class="sxs-lookup"><span data-stu-id="8338e-103">Role-based authorization in ASP.NET Core</span></span>

<a name="security-authorization-role-based"></a>

<span data-ttu-id="8338e-104">Id が作成されたときに、1 つまたは複数のロールに属する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="8338e-104">When an identity is created it may belong to one or more roles.</span></span> <span data-ttu-id="8338e-105">たとえば、Tracy は、Scott は、ユーザー ロールにのみ属することがあります、管理者およびユーザー ロールに属して可能性があります。</span><span class="sxs-lookup"><span data-stu-id="8338e-105">For example, Tracy may belong to the Administrator and User roles whilst Scott may only belong to the User role.</span></span> <span data-ttu-id="8338e-106">これらのロールを作成および管理する方法は、承認プロセスのバッキング ストアに依存します。</span><span class="sxs-lookup"><span data-stu-id="8338e-106">How these roles are created and managed depends on the backing store of the authorization process.</span></span> <span data-ttu-id="8338e-107">ロールは、開発者に公開、 [IsInRole](/dotnet/api/system.security.principal.genericprincipal.isinrole)メソッドを[ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal)クラス。</span><span class="sxs-lookup"><span data-stu-id="8338e-107">Roles are exposed to the developer through the [IsInRole](/dotnet/api/system.security.principal.genericprincipal.isinrole) method on the [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal) class.</span></span>

::: moniker range=">= aspnetcore-2.0"

> [!IMPORTANT]
> <span data-ttu-id="8338e-108">このトピックは Razor ページには適用**されません**。</span><span class="sxs-lookup"><span data-stu-id="8338e-108">This topic does **not** apply to Razor Pages.</span></span> <span data-ttu-id="8338e-109">Razor ページをサポートしています[IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter)と[IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter)します。</span><span class="sxs-lookup"><span data-stu-id="8338e-109">Razor Pages supports [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter) and [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter).</span></span> <span data-ttu-id="8338e-110">詳細については、[Razor ページのフィルター メソッド](xref:razor-pages/filter)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="8338e-110">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

::: moniker-end

## <a name="adding-role-checks"></a><span data-ttu-id="8338e-111">追加の役割の確認</span><span class="sxs-lookup"><span data-stu-id="8338e-111">Adding role checks</span></span>

<span data-ttu-id="8338e-112">ロール ベースの承認チェックは、宣言型&mdash;開発者それらを埋め込みます、コント ローラーまたはコント ローラー内のアクションに対して、コード内で要求されたリソースにアクセスするのメンバーである必要があります、現在のユーザー ロールを指定します。</span><span class="sxs-lookup"><span data-stu-id="8338e-112">Role-based authorization checks are declarative&mdash;the developer embeds them within their code, against a controller or an action within a controller, specifying roles which the current user must be a member of to access the requested resource.</span></span>

<span data-ttu-id="8338e-113">次のコードがすべてのアクションへのアクセスを制限するなど、`AdministrationController`のメンバーであるユーザーに、`Administrator`ロール。</span><span class="sxs-lookup"><span data-stu-id="8338e-113">For example, the following code limits access to any actions on the `AdministrationController` to users who are a member of the `Administrator` role:</span></span>

```csharp
[Authorize(Roles = "Administrator")]
public class AdministrationController : Controller
{
}
```

<span data-ttu-id="8338e-114">複数のロールは、コンマ区切りリストとして指定できます。</span><span class="sxs-lookup"><span data-stu-id="8338e-114">You can specify multiple roles as a comma separated list:</span></span>

```csharp
[Authorize(Roles = "HRManager,Finance")]
public class SalaryController : Controller
{
}
```

<span data-ttu-id="8338e-115">このコント ローラーがメンバーであるユーザーがアクセスのみができるは`HRManager`ロールまたは`Finance`ロール。</span><span class="sxs-lookup"><span data-stu-id="8338e-115">This controller would be only accessible by users who are members of the `HRManager` role or the `Finance` role.</span></span>

<span data-ttu-id="8338e-116">アクセスのユーザーは、指定したすべてのロールのメンバーである必要がありますし、複数の属性を適用する場合次の例では、ユーザーが両方のメンバーでなければならないことが必要です、`PowerUser`と`ControlPanelUser`ロール。</span><span class="sxs-lookup"><span data-stu-id="8338e-116">If you apply multiple attributes then an accessing user must be a member of all the roles specified; the following sample requires that a user must be a member of both the `PowerUser` and `ControlPanelUser` role.</span></span>

```csharp
[Authorize(Roles = "PowerUser")]
[Authorize(Roles = "ControlPanelUser")]
public class ControlPanelController : Controller
{
}
```

<span data-ttu-id="8338e-117">さらに、アクション レベルで追加のロールの承認属性を適用することでアクセスを制限できます。</span><span class="sxs-lookup"><span data-stu-id="8338e-117">You can further limit access by applying additional role authorization attributes at the action level:</span></span>

```csharp
[Authorize(Roles = "Administrator, PowerUser")]
public class ControlPanelController : Controller
{
    public ActionResult SetTime()
    {
    }

    [Authorize(Roles = "Administrator")]
    public ActionResult ShutDown()
    {
    }
}
```

<span data-ttu-id="8338e-118">前のコード スニペットのメンバーで、`Administrator`ロールまたは`PowerUser`コント ローラーにアクセスできるロールと`SetTime`アクションがのメンバーのみ、`Administrator`ロールがアクセスできる、`ShutDown`アクション。</span><span class="sxs-lookup"><span data-stu-id="8338e-118">In the previous code snippet members of the `Administrator` role or the `PowerUser` role can access the controller and the `SetTime` action, but only members of the `Administrator` role can access the `ShutDown` action.</span></span>

<span data-ttu-id="8338e-119">コント ローラーのロックダウンも、個々 のアクションへの匿名、認証されていないアクセスを許可できます。</span><span class="sxs-lookup"><span data-stu-id="8338e-119">You can also lock down a controller but allow anonymous, unauthenticated access to individual actions.</span></span>

```csharp
[Authorize]
public class ControlPanelController : Controller
{
    public ActionResult SetTime()
    {
    }

    [AllowAnonymous]
    public ActionResult Login()
    {
    }
}
```

<a name="security-authorization-role-policy"></a>

## <a name="policy-based-role-checks"></a><span data-ttu-id="8338e-120">ポリシー ベースの役割の確認</span><span class="sxs-lookup"><span data-stu-id="8338e-120">Policy based role checks</span></span>

<span data-ttu-id="8338e-121">承認サービスの構成の一部として、開発者が起動時にポリシーを登録、新しいポリシーの構文を使用しても、役割の要件を表現できます。</span><span class="sxs-lookup"><span data-stu-id="8338e-121">Role requirements can also be expressed using the new Policy syntax, where a developer registers a policy at startup as part of the Authorization service configuration.</span></span> <span data-ttu-id="8338e-122">これに通常発生`ConfigureServices()`で、 *Startup.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="8338e-122">This normally occurs in `ConfigureServices()` in your *Startup.cs* file.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("RequireAdministratorRole", policy => policy.RequireRole("Administrator"));
    });
}
```

<span data-ttu-id="8338e-123">使用してポリシーが適用される、`Policy`プロパティを`AuthorizeAttribute`属性。</span><span class="sxs-lookup"><span data-stu-id="8338e-123">Policies are applied using the `Policy` property on the `AuthorizeAttribute` attribute:</span></span>

```csharp
[Authorize(Policy = "RequireAdministratorRole")]
public IActionResult Shutdown()
{
    return View();
}
```

<span data-ttu-id="8338e-124">要件で許可されている複数のロールを指定するかどうかは、それらを指定するにはパラメーターとして、`RequireRole`メソッド。</span><span class="sxs-lookup"><span data-stu-id="8338e-124">If you want to specify multiple allowed roles in a requirement then you can specify them as parameters to the `RequireRole` method:</span></span>

```csharp
options.AddPolicy("ElevatedRights", policy =>
                  policy.RequireRole("Administrator", "PowerUser", "BackupAdministrator"));
```

<span data-ttu-id="8338e-125">この例に属しているユーザーの承認、 `Administrator`、`PowerUser`または`BackupAdministrator`ロール。</span><span class="sxs-lookup"><span data-stu-id="8338e-125">This example authorizes users who belong to the `Administrator`, `PowerUser` or `BackupAdministrator` roles.</span></span>
