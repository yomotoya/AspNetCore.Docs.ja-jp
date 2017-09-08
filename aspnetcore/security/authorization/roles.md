---
title: "ロール ベースの承認"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 5e014da1-8bc0-409b-951a-88b92c661fdf
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/roles
ms.openlocfilehash: d8dfcbb16ee7977d197b019c4e5e1b30fff17755
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/11/2017
---
# <a name="role-based-authorization"></a><span data-ttu-id="bced8-103">ロール ベースの承認</span><span class="sxs-lookup"><span data-stu-id="bced8-103">Role based Authorization</span></span>

<a name=security-authorization-role-based></a>

<span data-ttu-id="bced8-104">Id の作成時に 1 つまたは複数のロールに属している必要があります、中、Scott は、ユーザー ロールにのみ属することがありますを管理者およびユーザー ロールに属することがさんの例を示します。</span><span class="sxs-lookup"><span data-stu-id="bced8-104">When an identity is created it may belong to one or more roles, for example Tracy may belong to the Administrator and User roles whilst Scott may only belong to the user role.</span></span> <span data-ttu-id="bced8-105">これらのロールを作成および管理する方法は、承認プロセスのバッキング ストアによって異なります。</span><span class="sxs-lookup"><span data-stu-id="bced8-105">How these roles are created and managed depends on the backing store of the authorization process.</span></span> <span data-ttu-id="bced8-106">開発者が使用するロールが公開されている、 [IsInRole](https://msdn.microsoft.com/library/system.security.claims.claimsprincipal.isinrole(v=vs.110).aspx)プロパティを[ClaimsPrincipal](https://msdn.microsoft.com/library/system.security.claims.claimsprincipal(v=vs.110).aspx)クラスです。</span><span class="sxs-lookup"><span data-stu-id="bced8-106">Roles are exposed to the developer through the [IsInRole](https://msdn.microsoft.com/library/system.security.claims.claimsprincipal.isinrole(v=vs.110).aspx) property on the [ClaimsPrincipal](https://msdn.microsoft.com/library/system.security.claims.claimsprincipal(v=vs.110).aspx) class.</span></span>

## <a name="adding-role-checks"></a><span data-ttu-id="bced8-107">追加のロールのチェック</span><span class="sxs-lookup"><span data-stu-id="bced8-107">Adding role checks</span></span>

<span data-ttu-id="bced8-108">ロール ベースの承認チェックは、宣言型 - 開発者は、そのファイルを埋め込みますコント ローラーや、コント ローラー内のアクションに対して、コード内で要求されたリソースにアクセスするのメンバーである必要があります、現在のユーザー ロールを指定します。</span><span class="sxs-lookup"><span data-stu-id="bced8-108">Role based authorization checks are declarative - the developer embeds them within their code, against a controller or an action within a controller, specifying roles which the current user must be a member of to access the requested resource.</span></span>

<span data-ttu-id="bced8-109">たとえば、次のコード制限操作へのアクセスに、`AdministrationController`のメンバーであるユーザーのユーザーに、`Administrator`グループ。</span><span class="sxs-lookup"><span data-stu-id="bced8-109">For example the following code would limit access to any actions on the `AdministrationController` to users who are a member of the `Administrator` group.</span></span>

```csharp
[Authorize(Roles = "Administrator")]
public class AdministrationController : Controller
{
}
```

<span data-ttu-id="bced8-110">コンマ区切りのリストです。 として複数のロールを指定することができます。</span><span class="sxs-lookup"><span data-stu-id="bced8-110">You can specify multiple roles as a comma separated list;</span></span>

```csharp
[Authorize(Roles = "HRManager,Finance")]
public class SalaryController : Controller
{
}
```

<span data-ttu-id="bced8-111">このコント ローラーにはメンバーであるユーザーがアクセスするだけの`HRManager`ロールまたは`Finance`ロール。</span><span class="sxs-lookup"><span data-stu-id="bced8-111">This controller would be only accessible by users who are members of the `HRManager` role or the `Finance` role.</span></span>

<span data-ttu-id="bced8-112">アクセスするユーザーは、指定したすべてのロールのメンバーである必要があります、複数の属性を適用する場合次の例では、ユーザーは両方のメンバーである必要がありますが必要です、`PowerUser`と`ControlPanelUser`ロール。</span><span class="sxs-lookup"><span data-stu-id="bced8-112">If you apply multiple attributes then an accessing user must be a member of all the roles specified; the following sample requires that a user must be a member of both the `PowerUser` and `ControlPanelUser` role.</span></span>

```csharp
[Authorize(Roles = "PowerUser")]
[Authorize(Roles = "ControlPanelUser")]
public class ControlPanelController : Controller
{
}
```

<span data-ttu-id="bced8-113">さらに操作レベルで追加の役割の承認属性を適用することによってアクセスを制限することができます。</span><span class="sxs-lookup"><span data-stu-id="bced8-113">You can further limit access by applying additional role authorization attributes at the action level;</span></span>

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

<span data-ttu-id="bced8-114">前のコード スニペットのメンバーで、`Administrator`ロールまたは`PowerUser`ロールがアクセスできる、コント ローラーと`SetTime`アクションがのメンバーだけ、`Administrator`ロールがアクセスできる、`ShutDown`アクション。</span><span class="sxs-lookup"><span data-stu-id="bced8-114">In the previous code snippet members of the `Administrator` role or the `PowerUser` role can access the controller and the `SetTime` action, but only members of the `Administrator` role can access the `ShutDown` action.</span></span>

<span data-ttu-id="bced8-115">コント ローラーをロックは個別のアクションに匿名で認証されていないアクセスを許可することもできます。</span><span class="sxs-lookup"><span data-stu-id="bced8-115">You can also lock down a controller but allow anonymous, unauthenticated access to individual actions.</span></span>

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

<a name=security-authorization-role-policy></a>

## <a name="policy-based-role-checks"></a><span data-ttu-id="bced8-116">ポリシー ベースの役割の確認</span><span class="sxs-lookup"><span data-stu-id="bced8-116">Policy based role checks</span></span>

<span data-ttu-id="bced8-117">構文を使用して、新しいポリシー、認証サービスの構成の一部として、開発者が起動時にポリシーを登録の役割の要件を表現することもできます。</span><span class="sxs-lookup"><span data-stu-id="bced8-117">Role requirements can also be expressed using the new Policy syntax, where a developer registers a policy at startup as part of the Authorization service configuration.</span></span> <span data-ttu-id="bced8-118">通常は、この`ConfigureServices()`で、 *Startup.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="bced8-118">This normally occurs in `ConfigureServices()` in your *Startup.cs* file.</span></span>

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

<span data-ttu-id="bced8-119">使用してポリシーが適用される、`Policy`プロパティを`AuthorizeAttribute`属性です。</span><span class="sxs-lookup"><span data-stu-id="bced8-119">Policies are applied using the `Policy` property on the `AuthorizeAttribute` attribute;</span></span>

```csharp
[Authorize(Policy = "RequireAdministratorRole")]
public IActionResult Shutdown()
{
    return View();
}
```

<span data-ttu-id="bced8-120">必要条件で許可されている複数のロールを指定するかどうかは、パラメーターとして指定することができます、`RequireRole`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="bced8-120">If you want to specify multiple allowed roles in a requirement then you can specify them as parameters to the `RequireRole` method;</span></span>

```csharp
options.AddPolicy("ElevatedRights", policy =>
                  policy.RequireRole("Administrator", "PowerUser", "BackupAdministrator"));
```

<span data-ttu-id="bced8-121">この例に属しているユーザーの承認、 `Administrator`、`PowerUser`または`BackupAdministrator`ロール。</span><span class="sxs-lookup"><span data-stu-id="bced8-121">This example authorizes users who belong to the `Administrator`, `PowerUser` or `BackupAdministrator` roles.</span></span>
