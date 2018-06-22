---
title: ASP.NET Core での単純な承認
author: rick-anderson
description: Authorize 属性を使用して ASP.NET Core コント ローラーとアクションへのアクセスを制限する方法を説明します。
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/simple
ms.openlocfilehash: 3c5e9d5dfd65ded40c9828a666143c1868f5562f
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272066"
---
# <a name="simple-authorization-in-aspnet-core"></a><span data-ttu-id="56dc2-103">ASP.NET Core での単純な承認</span><span class="sxs-lookup"><span data-stu-id="56dc2-103">Simple authorization in ASP.NET Core</span></span>

<a name="security-authorization-simple"></a>

<span data-ttu-id="56dc2-104">MVC での承認はによって制御されます、`AuthorizeAttribute`属性とそのさまざまなパラメーターです。</span><span class="sxs-lookup"><span data-stu-id="56dc2-104">Authorization in MVC is controlled through the `AuthorizeAttribute` attribute and its various parameters.</span></span> <span data-ttu-id="56dc2-105">簡単に言うと、適用、`AuthorizeAttribute`属性をコント ローラーまたはアクションへのアクセス制限コント ローラーまたは認証されたユーザーに操作します。</span><span class="sxs-lookup"><span data-stu-id="56dc2-105">At its simplest, applying the `AuthorizeAttribute` attribute to a controller or action limits access to the controller or action to any authenticated user.</span></span>

<span data-ttu-id="56dc2-106">たとえば、次のコードがへのアクセスを制限、`AccountController`認証されたユーザーにします。</span><span class="sxs-lookup"><span data-stu-id="56dc2-106">For example, the following code limits access to the `AccountController` to any authenticated user.</span></span>

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

<span data-ttu-id="56dc2-107">コント ローラーではなく、アクションに承認を適用する場合は、適用、`AuthorizeAttribute`操作自体に属性します。</span><span class="sxs-lookup"><span data-stu-id="56dc2-107">If you want to apply authorization to an action rather than the controller, apply the `AuthorizeAttribute` attribute to the action itself:</span></span>

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

<span data-ttu-id="56dc2-108">認証されたユーザーのみがアクセスできるようになりました、`Logout`関数。</span><span class="sxs-lookup"><span data-stu-id="56dc2-108">Now only authenticated users can access the `Logout` function.</span></span>

<span data-ttu-id="56dc2-109">使用することも、`AllowAnonymous`を個別のアクションに認証されていないユーザーによるアクセスを許可する属性。</span><span class="sxs-lookup"><span data-stu-id="56dc2-109">You can also use the `AllowAnonymous` attribute to allow access by non-authenticated users to individual actions.</span></span> <span data-ttu-id="56dc2-110">例えば:</span><span class="sxs-lookup"><span data-stu-id="56dc2-110">For example:</span></span>

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

<span data-ttu-id="56dc2-111">認証されたユーザーのみをこのようにすると、 `AccountController`、を除き、`Login`がその認証されたか、認証されていない/匿名の状態に関係なく、だれがアクセス可能なアクションです。</span><span class="sxs-lookup"><span data-stu-id="56dc2-111">This would allow only authenticated users to the `AccountController`, except for the `Login` action, which is accessible by everyone, regardless of their authenticated or unauthenticated / anonymous status.</span></span>

>[!WARNING]
> <span data-ttu-id="56dc2-112">`[AllowAnonymous]` 承認のすべてのステートメントをバイパスします。</span><span class="sxs-lookup"><span data-stu-id="56dc2-112">`[AllowAnonymous]` bypasses all authorization statements.</span></span> <span data-ttu-id="56dc2-113">結合を適用する場合`[AllowAnonymous]`と任意`[Authorize]`し、属性、Authorize 属性は常に無視されます。</span><span class="sxs-lookup"><span data-stu-id="56dc2-113">If you apply combine `[AllowAnonymous]` and any `[Authorize]` attribute then the Authorize attributes will always be ignored.</span></span> <span data-ttu-id="56dc2-114">適用する場合などは`[AllowAnonymous]`レベル、コント ローラーでいずれかの`[Authorize]`同じコント ローラーで、または、内の任意のアクションの属性は無視されます。</span><span class="sxs-lookup"><span data-stu-id="56dc2-114">For example if you apply `[AllowAnonymous]` at the controller level any `[Authorize]` attributes on the same controller, or on any action within it will be ignored.</span></span>
