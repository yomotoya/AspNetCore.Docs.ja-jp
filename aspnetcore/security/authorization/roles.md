---
title: "ロール ベースの承認"
author: rick-anderson
description: "このドキュメントでは、Authorize attribute にロールを渡すことで ASP.NET Core のコント ローラーとアクションのアクセスを制限する方法を示します。"
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/roles
ms.openlocfilehash: 764d1fcc384fc8370d1a536f9609333de6bd4357
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="role-based-authorization"></a><span data-ttu-id="f8c61-103">ロール ベースの承認</span><span class="sxs-lookup"><span data-stu-id="f8c61-103">Role based Authorization</span></span>

<a name="security-authorization-role-based"></a>

<span data-ttu-id="f8c61-104">Id が作成されるときは、1 つまたは複数のロールに属する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="f8c61-104">When an identity is created it may belong to one or more roles.</span></span> <span data-ttu-id="f8c61-105">たとえば、さんは、中、Scott は、ユーザー ロールにのみ属することがあります、管理者とユーザーのロールに属する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="f8c61-105">For example, Tracy may belong to the Administrator and User roles whilst Scott may only belong to the User role.</span></span> <span data-ttu-id="f8c61-106">これらのロールを作成および管理する方法は、承認プロセスのバッキング ストアによって異なります。</span><span class="sxs-lookup"><span data-stu-id="f8c61-106">How these roles are created and managed depends on the backing store of the authorization process.</span></span> <span data-ttu-id="f8c61-107">開発者が使用するロールが公開されている、 [IsInRole](https://docs.microsoft.com/dotnet/api/system.security.principal.genericprincipal.isinrole)メソッドを[ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal)クラスです。</span><span class="sxs-lookup"><span data-stu-id="f8c61-107">Roles are exposed to the developer through the [IsInRole](https://docs.microsoft.com/dotnet/api/system.security.principal.genericprincipal.isinrole) method on the [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal) class.</span></span>

## <a name="adding-role-checks"></a><span data-ttu-id="f8c61-108">追加のロールのチェック</span><span class="sxs-lookup"><span data-stu-id="f8c61-108">Adding role checks</span></span>

<span data-ttu-id="f8c61-109">ロール ベースの承認チェックが宣言&mdash;開発者は、そのファイルを埋め込みますコント ローラーや、コント ローラー内のアクションに対して、コード内で要求されたリソースにアクセスするのメンバーである必要があります、現在のユーザー ロールを指定します。</span><span class="sxs-lookup"><span data-stu-id="f8c61-109">Role-based authorization checks are declarative&mdash;the developer embeds them within their code, against a controller or an action within a controller, specifying roles which the current user must be a member of to access the requested resource.</span></span>

<span data-ttu-id="f8c61-110">次のコードがすべてのアクションへのアクセスを制限するなど、`AdministrationController`のメンバーであるユーザーのユーザーに、`Administrator`ロール。</span><span class="sxs-lookup"><span data-stu-id="f8c61-110">For example, the following code limits access to any actions on the `AdministrationController` to users who are a member of the `Administrator` role:</span></span>

```csharp
[Authorize(Roles = "Administrator")]
public class AdministrationController : Controller
{
}
```

<span data-ttu-id="f8c61-111">複数の役割は、コンマ区切りリストとして指定できます。</span><span class="sxs-lookup"><span data-stu-id="f8c61-111">You can specify multiple roles as a comma separated list:</span></span>

```csharp
[Authorize(Roles = "HRManager,Finance")]
public class SalaryController : Controller
{
}
```

<span data-ttu-id="f8c61-112">このコント ローラーにはメンバーであるユーザーがアクセスするだけの`HRManager`ロールまたは`Finance`ロール。</span><span class="sxs-lookup"><span data-stu-id="f8c61-112">This controller would be only accessible by users who are members of the `HRManager` role or the `Finance` role.</span></span>

<span data-ttu-id="f8c61-113">アクセスするユーザーは、指定したすべてのロールのメンバーである必要があります、複数の属性を適用する場合次の例では、ユーザーは両方のメンバーである必要がありますが必要です、`PowerUser`と`ControlPanelUser`ロール。</span><span class="sxs-lookup"><span data-stu-id="f8c61-113">If you apply multiple attributes then an accessing user must be a member of all the roles specified; the following sample requires that a user must be a member of both the `PowerUser` and `ControlPanelUser` role.</span></span>

```csharp
[Authorize(Roles = "PowerUser")]
[Authorize(Roles = "ControlPanelUser")]
public class ControlPanelController : Controller
{
}
```

<span data-ttu-id="f8c61-114">さらに操作レベルで追加の役割の承認属性を適用することによってアクセスを制限できます。</span><span class="sxs-lookup"><span data-stu-id="f8c61-114">You can further limit access by applying additional role authorization attributes at the action level:</span></span>

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

<span data-ttu-id="f8c61-115">前のコード スニペットのメンバーで、`Administrator`ロールまたは`PowerUser`ロールがアクセスできる、コント ローラーと`SetTime`アクションがのメンバーだけ、`Administrator`ロールがアクセスできる、`ShutDown`アクション。</span><span class="sxs-lookup"><span data-stu-id="f8c61-115">In the previous code snippet members of the `Administrator` role or the `PowerUser` role can access the controller and the `SetTime` action, but only members of the `Administrator` role can access the `ShutDown` action.</span></span>

<span data-ttu-id="f8c61-116">コント ローラーをロックは個別のアクションに匿名で認証されていないアクセスを許可することもできます。</span><span class="sxs-lookup"><span data-stu-id="f8c61-116">You can also lock down a controller but allow anonymous, unauthenticated access to individual actions.</span></span>

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

## <a name="policy-based-role-checks"></a><span data-ttu-id="f8c61-117">ポリシー ベースの役割の確認</span><span class="sxs-lookup"><span data-stu-id="f8c61-117">Policy based role checks</span></span>

<span data-ttu-id="f8c61-118">構文を使用して、新しいポリシー、認証サービスの構成の一部として、開発者が起動時にポリシーを登録の役割の要件を表現することもできます。</span><span class="sxs-lookup"><span data-stu-id="f8c61-118">Role requirements can also be expressed using the new Policy syntax, where a developer registers a policy at startup as part of the Authorization service configuration.</span></span> <span data-ttu-id="f8c61-119">通常は、この`ConfigureServices()`で、 *Startup.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="f8c61-119">This normally occurs in `ConfigureServices()` in your *Startup.cs* file.</span></span>

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

<span data-ttu-id="f8c61-120">使用してポリシーが適用される、`Policy`プロパティを`AuthorizeAttribute`属性。</span><span class="sxs-lookup"><span data-stu-id="f8c61-120">Policies are applied using the `Policy` property on the `AuthorizeAttribute` attribute:</span></span>

```csharp
[Authorize(Policy = "RequireAdministratorRole")]
public IActionResult Shutdown()
{
    return View();
}
```

<span data-ttu-id="f8c61-121">必要条件で許可されている複数のロールを指定するかどうかは、パラメーターとして指定することができます、`RequireRole`メソッド。</span><span class="sxs-lookup"><span data-stu-id="f8c61-121">If you want to specify multiple allowed roles in a requirement then you can specify them as parameters to the `RequireRole` method:</span></span>

```csharp
options.AddPolicy("ElevatedRights", policy =>
                  policy.RequireRole("Administrator", "PowerUser", "BackupAdministrator"));
```

<span data-ttu-id="f8c61-122">この例に属しているユーザーの承認、 `Administrator`、`PowerUser`または`BackupAdministrator`ロール。</span><span class="sxs-lookup"><span data-stu-id="f8c61-122">This example authorizes users who belong to the `Administrator`, `PowerUser` or `BackupAdministrator` roles.</span></span>
