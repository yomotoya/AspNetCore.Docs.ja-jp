---
title: "ロール ベースの承認"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 5e014da1-8bc0-409b-951a-88b92c661fdf
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/roles
ms.openlocfilehash: 3dfba8a492c5d592b1fa9c8893a0ec4b1a2e70b9
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/13/2017
---
# <a name="role-based-authorization"></a><span data-ttu-id="1fbf3-103">ロール ベースの承認</span><span class="sxs-lookup"><span data-stu-id="1fbf3-103">Role based Authorization</span></span>

<a name="security-authorization-role-based"></a>

<span data-ttu-id="1fbf3-104">Id の作成時に 1 つまたは複数のロールに属している必要があります、中、Scott は、ユーザー ロールにのみ属することがありますを管理者およびユーザー ロールに属することがさんの例を示します。</span><span class="sxs-lookup"><span data-stu-id="1fbf3-104">When an identity is created it may belong to one or more roles, for example Tracy may belong to the Administrator and User roles whilst Scott may only belong to the user role.</span></span> <span data-ttu-id="1fbf3-105">これらのロールを作成および管理する方法は、承認プロセスのバッキング ストアによって異なります。</span><span class="sxs-lookup"><span data-stu-id="1fbf3-105">How these roles are created and managed depends on the backing store of the authorization process.</span></span> <span data-ttu-id="1fbf3-106">開発者が使用するロールが公開されている、 [IsInRole](https://docs.microsoft.com/dotnet/api/system.security.principal.genericprincipal.isinrole)プロパティを[ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal)クラスです。</span><span class="sxs-lookup"><span data-stu-id="1fbf3-106">Roles are exposed to the developer through the [IsInRole](https://docs.microsoft.com/dotnet/api/system.security.principal.genericprincipal.isinrole) property on the [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal) class.</span></span>

## <a name="adding-role-checks"></a><span data-ttu-id="1fbf3-107">追加のロールのチェック</span><span class="sxs-lookup"><span data-stu-id="1fbf3-107">Adding role checks</span></span>

<span data-ttu-id="1fbf3-108">ロール ベースの承認チェックは、宣言型 - 開発者は、そのファイルを埋め込みますコント ローラーや、コント ローラー内のアクションに対して、コード内で要求されたリソースにアクセスするのメンバーである必要があります、現在のユーザー ロールを指定します。</span><span class="sxs-lookup"><span data-stu-id="1fbf3-108">Role based authorization checks are declarative - the developer embeds them within their code, against a controller or an action within a controller, specifying roles which the current user must be a member of to access the requested resource.</span></span>

<span data-ttu-id="1fbf3-109">たとえば、次のコード制限操作へのアクセスに、`AdministrationController`のメンバーであるユーザーのユーザーに、`Administrator`グループ。</span><span class="sxs-lookup"><span data-stu-id="1fbf3-109">For example the following code would limit access to any actions on the `AdministrationController` to users who are a member of the `Administrator` group.</span></span>

```csharp
[Authorize(Roles = "Administrator")]
public class AdministrationController : Controller
{
}
```

<span data-ttu-id="1fbf3-110">コンマ区切りのリストです。 として複数のロールを指定することができます。</span><span class="sxs-lookup"><span data-stu-id="1fbf3-110">You can specify multiple roles as a comma separated list;</span></span>

```csharp
[Authorize(Roles = "HRManager,Finance")]
public class SalaryController : Controller
{
}
```

<span data-ttu-id="1fbf3-111">このコント ローラーにはメンバーであるユーザーがアクセスするだけの`HRManager`ロールまたは`Finance`ロール。</span><span class="sxs-lookup"><span data-stu-id="1fbf3-111">This controller would be only accessible by users who are members of the `HRManager` role or the `Finance` role.</span></span>

<span data-ttu-id="1fbf3-112">アクセスするユーザーは、指定したすべてのロールのメンバーである必要があります、複数の属性を適用する場合次の例では、ユーザーは両方のメンバーである必要がありますが必要です、`PowerUser`と`ControlPanelUser`ロール。</span><span class="sxs-lookup"><span data-stu-id="1fbf3-112">If you apply multiple attributes then an accessing user must be a member of all the roles specified; the following sample requires that a user must be a member of both the `PowerUser` and `ControlPanelUser` role.</span></span>

```csharp
[Authorize(Roles = "PowerUser")]
[Authorize(Roles = "ControlPanelUser")]
public class ControlPanelController : Controller
{
}
```

<span data-ttu-id="1fbf3-113">さらに操作レベルで追加の役割の承認属性を適用することによってアクセスを制限することができます。</span><span class="sxs-lookup"><span data-stu-id="1fbf3-113">You can further limit access by applying additional role authorization attributes at the action level;</span></span>

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

<span data-ttu-id="1fbf3-114">前のコード スニペットのメンバーで、`Administrator`ロールまたは`PowerUser`ロールがアクセスできる、コント ローラーと`SetTime`アクションがのメンバーだけ、`Administrator`ロールがアクセスできる、`ShutDown`アクション。</span><span class="sxs-lookup"><span data-stu-id="1fbf3-114">In the previous code snippet members of the `Administrator` role or the `PowerUser` role can access the controller and the `SetTime` action, but only members of the `Administrator` role can access the `ShutDown` action.</span></span>

<span data-ttu-id="1fbf3-115">コント ローラーをロックは個別のアクションに匿名で認証されていないアクセスを許可することもできます。</span><span class="sxs-lookup"><span data-stu-id="1fbf3-115">You can also lock down a controller but allow anonymous, unauthenticated access to individual actions.</span></span>

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

## <a name="policy-based-role-checks"></a><span data-ttu-id="1fbf3-116">ポリシー ベースの役割の確認</span><span class="sxs-lookup"><span data-stu-id="1fbf3-116">Policy based role checks</span></span>

<span data-ttu-id="1fbf3-117">構文を使用して、新しいポリシー、認証サービスの構成の一部として、開発者が起動時にポリシーを登録の役割の要件を表現することもできます。</span><span class="sxs-lookup"><span data-stu-id="1fbf3-117">Role requirements can also be expressed using the new Policy syntax, where a developer registers a policy at startup as part of the Authorization service configuration.</span></span> <span data-ttu-id="1fbf3-118">通常は、この`ConfigureServices()`で、 *Startup.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="1fbf3-118">This normally occurs in `ConfigureServices()` in your *Startup.cs* file.</span></span>

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

<span data-ttu-id="1fbf3-119">使用してポリシーが適用される、`Policy`プロパティを`AuthorizeAttribute`属性です。</span><span class="sxs-lookup"><span data-stu-id="1fbf3-119">Policies are applied using the `Policy` property on the `AuthorizeAttribute` attribute;</span></span>

```csharp
[Authorize(Policy = "RequireAdministratorRole")]
public IActionResult Shutdown()
{
    return View();
}
```

<span data-ttu-id="1fbf3-120">必要条件で許可されている複数のロールを指定するかどうかは、パラメーターとして指定することができます、`RequireRole`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="1fbf3-120">If you want to specify multiple allowed roles in a requirement then you can specify them as parameters to the `RequireRole` method;</span></span>

```csharp
options.AddPolicy("ElevatedRights", policy =>
                  policy.RequireRole("Administrator", "PowerUser", "BackupAdministrator"));
```

<span data-ttu-id="1fbf3-121">この例に属しているユーザーの承認、 `Administrator`、`PowerUser`または`BackupAdministrator`ロール。</span><span class="sxs-lookup"><span data-stu-id="1fbf3-121">This example authorizes users who belong to the `Administrator`, `PowerUser` or `BackupAdministrator` roles.</span></span>
