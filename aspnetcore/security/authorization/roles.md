---
title: ASP.NET Core でのロールベースの承認
author: rick-anderson
description: Authorize 属性にロールを渡すことによって、ASP.NET Core のコント ローラーとアクションのアクセスを制限する方法について説明します。
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/roles
ms.openlocfilehash: 0e01e1976e2721ca64720a67c6341661f646395c
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/19/2019
ms.locfileid: "58209097"
---
# <a name="role-based-authorization-in-aspnet-core"></a><span data-ttu-id="c8973-103">ASP.NET Core でのロールベースの承認</span><span class="sxs-lookup"><span data-stu-id="c8973-103">Role-based authorization in ASP.NET Core</span></span>

<a name="security-authorization-role-based"></a>

<span data-ttu-id="c8973-104">Id が作成されたときに、1 つまたは複数のロールに属する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="c8973-104">When an identity is created it may belong to one or more roles.</span></span> <span data-ttu-id="c8973-105">たとえば、Tracy は、Scott は、ユーザー ロールにのみ属することがあります、管理者およびユーザー ロールに属して可能性があります。</span><span class="sxs-lookup"><span data-stu-id="c8973-105">For example, Tracy may belong to the Administrator and User roles whilst Scott may only belong to the User role.</span></span> <span data-ttu-id="c8973-106">これらのロールを作成および管理する方法は、承認プロセスのバッキング ストアに依存します。</span><span class="sxs-lookup"><span data-stu-id="c8973-106">How these roles are created and managed depends on the backing store of the authorization process.</span></span> <span data-ttu-id="c8973-107">ロールは、開発者に公開、 [IsInRole](/dotnet/api/system.security.principal.genericprincipal.isinrole)メソッドを[ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal)クラス。</span><span class="sxs-lookup"><span data-stu-id="c8973-107">Roles are exposed to the developer through the [IsInRole](/dotnet/api/system.security.principal.genericprincipal.isinrole) method on the [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal) class.</span></span>

## <a name="adding-role-checks"></a><span data-ttu-id="c8973-108">追加の役割の確認</span><span class="sxs-lookup"><span data-stu-id="c8973-108">Adding role checks</span></span>

<span data-ttu-id="c8973-109">ロール ベースの承認チェックは、宣言型&mdash;開発者それらを埋め込みます、コント ローラーまたはコント ローラー内のアクションに対して、コード内で要求されたリソースにアクセスするのメンバーである必要があります、現在のユーザー ロールを指定します。</span><span class="sxs-lookup"><span data-stu-id="c8973-109">Role-based authorization checks are declarative&mdash;the developer embeds them within their code, against a controller or an action within a controller, specifying roles which the current user must be a member of to access the requested resource.</span></span>

<span data-ttu-id="c8973-110">次のコードがすべてのアクションへのアクセスを制限するなど、`AdministrationController`のメンバーであるユーザーに、`Administrator`ロール。</span><span class="sxs-lookup"><span data-stu-id="c8973-110">For example, the following code limits access to any actions on the `AdministrationController` to users who are a member of the `Administrator` role:</span></span>

```csharp
[Authorize(Roles = "Administrator")]
public class AdministrationController : Controller
{
}
```

<span data-ttu-id="c8973-111">複数のロールは、コンマ区切りリストとして指定できます。</span><span class="sxs-lookup"><span data-stu-id="c8973-111">You can specify multiple roles as a comma separated list:</span></span>

```csharp
[Authorize(Roles = "HRManager,Finance")]
public class SalaryController : Controller
{
}
```

<span data-ttu-id="c8973-112">このコント ローラーがメンバーであるユーザーがアクセスのみができるは`HRManager`ロールまたは`Finance`ロール。</span><span class="sxs-lookup"><span data-stu-id="c8973-112">This controller would be only accessible by users who are members of the `HRManager` role or the `Finance` role.</span></span>

<span data-ttu-id="c8973-113">アクセスのユーザーは、指定したすべてのロールのメンバーである必要がありますし、複数の属性を適用する場合次の例では、ユーザーが両方のメンバーでなければならないことが必要です、`PowerUser`と`ControlPanelUser`ロール。</span><span class="sxs-lookup"><span data-stu-id="c8973-113">If you apply multiple attributes then an accessing user must be a member of all the roles specified; the following sample requires that a user must be a member of both the `PowerUser` and `ControlPanelUser` role.</span></span>

```csharp
[Authorize(Roles = "PowerUser")]
[Authorize(Roles = "ControlPanelUser")]
public class ControlPanelController : Controller
{
}
```

<span data-ttu-id="c8973-114">さらに、アクション レベルで追加のロールの承認属性を適用することでアクセスを制限できます。</span><span class="sxs-lookup"><span data-stu-id="c8973-114">You can further limit access by applying additional role authorization attributes at the action level:</span></span>

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

<span data-ttu-id="c8973-115">前のコード スニペットのメンバーで、`Administrator`ロールまたは`PowerUser`コント ローラーにアクセスできるロールと`SetTime`アクションがのメンバーのみ、`Administrator`ロールがアクセスできる、`ShutDown`アクション。</span><span class="sxs-lookup"><span data-stu-id="c8973-115">In the previous code snippet members of the `Administrator` role or the `PowerUser` role can access the controller and the `SetTime` action, but only members of the `Administrator` role can access the `ShutDown` action.</span></span>

<span data-ttu-id="c8973-116">コント ローラーのロックダウンも、個々 のアクションへの匿名、認証されていないアクセスを許可できます。</span><span class="sxs-lookup"><span data-stu-id="c8973-116">You can also lock down a controller but allow anonymous, unauthenticated access to individual actions.</span></span>

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

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="c8973-117">Razor ページの場合、`AuthorizeAttribute`いずれかで適用できます。</span><span class="sxs-lookup"><span data-stu-id="c8973-117">For Razor Pages, the `AuthorizeAttribute` can be applied by either:</span></span>

* <span data-ttu-id="c8973-118">使用して、[規則](xref:razor-pages/razor-pages-conventions#page-model-action-conventions)、または</span><span class="sxs-lookup"><span data-stu-id="c8973-118">Using a [convention](xref:razor-pages/razor-pages-conventions#page-model-action-conventions), or</span></span>
* <span data-ttu-id="c8973-119">適用、`AuthorizeAttribute`を`PageModel`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="c8973-119">Applying the `AuthorizeAttribute` to the `PageModel` instance:</span></span>

```csharp
[Authorize(Policy = "RequireAdministratorRole")]
public class UpdateModel : PageModel
{
    public ActionResult OnPost()
    {
    }
}
```

> [!IMPORTANT]
> <span data-ttu-id="c8973-120">などの属性をフィルター処理`AuthorizeAttribute`PageModel にのみ適用できる、特定のページ ハンドラー メソッドには適用できません。</span><span class="sxs-lookup"><span data-stu-id="c8973-120">Filter attributes, including `AuthorizeAttribute`, can only be applied to PageModel and cannot be applied to specific page handler methods.</span></span>
::: moniker-end

<a name="security-authorization-role-policy"></a>

## <a name="policy-based-role-checks"></a><span data-ttu-id="c8973-121">ポリシー ベースの役割の確認</span><span class="sxs-lookup"><span data-stu-id="c8973-121">Policy based role checks</span></span>

<span data-ttu-id="c8973-122">承認サービスの構成の一部として、開発者が起動時にポリシーを登録、新しいポリシーの構文を使用しても、役割の要件を表現できます。</span><span class="sxs-lookup"><span data-stu-id="c8973-122">Role requirements can also be expressed using the new Policy syntax, where a developer registers a policy at startup as part of the Authorization service configuration.</span></span> <span data-ttu-id="c8973-123">これに通常発生`ConfigureServices()`で、 *Startup.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="c8973-123">This normally occurs in `ConfigureServices()` in your *Startup.cs* file.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("RequireAdministratorRole",
             policy => policy.RequireRole("Administrator"));
    });
}
```

<span data-ttu-id="c8973-124">使用してポリシーが適用される、`Policy`プロパティを`AuthorizeAttribute`属性。</span><span class="sxs-lookup"><span data-stu-id="c8973-124">Policies are applied using the `Policy` property on the `AuthorizeAttribute` attribute:</span></span>

```csharp
[Authorize(Policy = "RequireAdministratorRole")]
public IActionResult Shutdown()
{
    return View();
}
```

<span data-ttu-id="c8973-125">要件で許可されている複数のロールを指定するかどうかは、それらを指定するにはパラメーターとして、`RequireRole`メソッド。</span><span class="sxs-lookup"><span data-stu-id="c8973-125">If you want to specify multiple allowed roles in a requirement then you can specify them as parameters to the `RequireRole` method:</span></span>

```csharp
options.AddPolicy("ElevatedRights", policy =>
                  policy.RequireRole("Administrator", "PowerUser", "BackupAdministrator"));
```

<span data-ttu-id="c8973-126">この例に属しているユーザーの承認、 `Administrator`、`PowerUser`または`BackupAdministrator`ロール。</span><span class="sxs-lookup"><span data-stu-id="c8973-126">This example authorizes users who belong to the `Administrator`, `PowerUser` or `BackupAdministrator` roles.</span></span>

### <a name="add-role-services-to-identity"></a><span data-ttu-id="c8973-127">役割サービスの Id を追加します。</span><span class="sxs-lookup"><span data-stu-id="c8973-127">Add Role services to Identity</span></span>

<span data-ttu-id="c8973-128">追加[AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1)役割サービスを追加します。</span><span class="sxs-lookup"><span data-stu-id="c8973-128">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](roles/samples/Startup.cs?name=snippet&highlight=7)]
