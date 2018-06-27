---
title: ASP.NET Core での単純な承認
author: rick-anderson
description: Authorize 属性を使用して ASP.NET Core コント ローラーとアクションへのアクセスを制限する方法を説明します。
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/simple
ms.openlocfilehash: 6409def0508b855d3d2a4a1f4d3a3d15bfe5dd32
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961124"
---
# <a name="simple-authorization-in-aspnet-core"></a><span data-ttu-id="39811-103">ASP.NET Core での単純な承認</span><span class="sxs-lookup"><span data-stu-id="39811-103">Simple authorization in ASP.NET Core</span></span>

<a name="security-authorization-simple"></a>

<span data-ttu-id="39811-104">MVC での承認はによって制御されます、`AuthorizeAttribute`属性とそのさまざまなパラメーターです。</span><span class="sxs-lookup"><span data-stu-id="39811-104">Authorization in MVC is controlled through the `AuthorizeAttribute` attribute and its various parameters.</span></span> <span data-ttu-id="39811-105">簡単に言うと、適用、`AuthorizeAttribute`属性をコント ローラーまたはアクションへのアクセス制限コント ローラーまたは認証されたユーザーに操作します。</span><span class="sxs-lookup"><span data-stu-id="39811-105">At its simplest, applying the `AuthorizeAttribute` attribute to a controller or action limits access to the controller or action to any authenticated user.</span></span>

<span data-ttu-id="39811-106">たとえば、次のコードがへのアクセスを制限、`AccountController`認証されたユーザーにします。</span><span class="sxs-lookup"><span data-stu-id="39811-106">For example, the following code limits access to the `AccountController` to any authenticated user.</span></span>

```csharp
[Authorize]
public class AccountController : Controller
{
    public ActionResult Login()
    {
    }

    public ActionResult Logout()
    {
    }
}
```

<span data-ttu-id="39811-107">コント ローラーではなく、アクションに承認を適用する場合は、適用、`AuthorizeAttribute`操作自体に属性します。</span><span class="sxs-lookup"><span data-stu-id="39811-107">If you want to apply authorization to an action rather than the controller, apply the `AuthorizeAttribute` attribute to the action itself:</span></span>

```csharp
public class AccountController : Controller
{
   public ActionResult Login()
   {
   }

   [Authorize]
   public ActionResult Logout()
   {
   }
}
```

<span data-ttu-id="39811-108">認証されたユーザーのみがアクセスできるようになりました、`Logout`関数。</span><span class="sxs-lookup"><span data-stu-id="39811-108">Now only authenticated users can access the `Logout` function.</span></span>

<span data-ttu-id="39811-109">使用することも、`AllowAnonymous`を個別のアクションに認証されていないユーザーによるアクセスを許可する属性。</span><span class="sxs-lookup"><span data-stu-id="39811-109">You can also use the `AllowAnonymous` attribute to allow access by non-authenticated users to individual actions.</span></span> <span data-ttu-id="39811-110">例えば:</span><span class="sxs-lookup"><span data-stu-id="39811-110">For example:</span></span>

```csharp
[Authorize]
public class AccountController : Controller
{
    [AllowAnonymous]
    public ActionResult Login()
    {
    }

    public ActionResult Logout()
    {
    }
}
```

<span data-ttu-id="39811-111">認証されたユーザーのみをこのようにすると、 `AccountController`、を除き、`Login`がその認証されたか、認証されていない/匿名の状態に関係なく、だれがアクセス可能なアクションです。</span><span class="sxs-lookup"><span data-stu-id="39811-111">This would allow only authenticated users to the `AccountController`, except for the `Login` action, which is accessible by everyone, regardless of their authenticated or unauthenticated / anonymous status.</span></span>

> [!WARNING]
> <span data-ttu-id="39811-112">`[AllowAnonymous]` 承認のすべてのステートメントをバイパスします。</span><span class="sxs-lookup"><span data-stu-id="39811-112">`[AllowAnonymous]` bypasses all authorization statements.</span></span> <span data-ttu-id="39811-113">結合する場合`[AllowAnonymous]`と任意`[Authorize]`、属性、`[Authorize]`属性は無視されます。</span><span class="sxs-lookup"><span data-stu-id="39811-113">If you combine `[AllowAnonymous]` and any `[Authorize]` attribute, the `[Authorize]` attributes are ignored.</span></span> <span data-ttu-id="39811-114">適用する場合などは`[AllowAnonymous]`、コント ローラー レベル、`[Authorize]`同じコント ローラー (または、すべての操作) の属性は無視されます。</span><span class="sxs-lookup"><span data-stu-id="39811-114">For example if you apply `[AllowAnonymous]` at the controller level, any `[Authorize]` attributes on the same controller (or on any action within it) is ignored.</span></span>
