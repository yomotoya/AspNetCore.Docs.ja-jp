---
title: "単純な承認"
author: rick-anderson
description: "このドキュメントでは、Authorize 属性を使用して ASP.NET Core コント ローラーとアクションへのアクセスを制限する方法について説明します。"
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/simple
ms.openlocfilehash: 3299a8fcbd8d8e089d8d7f95e46551c102bcc054
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="simple-authorization"></a><span data-ttu-id="9913e-103">単純な承認</span><span class="sxs-lookup"><span data-stu-id="9913e-103">Simple Authorization</span></span>

<a name="security-authorization-simple"></a>

<span data-ttu-id="9913e-104">MVC での承認はによって制御されます、`AuthorizeAttribute`属性とそのさまざまなパラメーターです。</span><span class="sxs-lookup"><span data-stu-id="9913e-104">Authorization in MVC is controlled through the `AuthorizeAttribute` attribute and its various parameters.</span></span> <span data-ttu-id="9913e-105">簡単に言うと、適用、`AuthorizeAttribute`属性をコント ローラーまたはアクションへのアクセス制限コント ローラーまたは認証されたユーザーに操作します。</span><span class="sxs-lookup"><span data-stu-id="9913e-105">At its simplest, applying the `AuthorizeAttribute` attribute to a controller or action limits access to the controller or action to any authenticated user.</span></span>

<span data-ttu-id="9913e-106">たとえば、次のコードがへのアクセスを制限、`AccountController`認証されたユーザーにします。</span><span class="sxs-lookup"><span data-stu-id="9913e-106">For example, the following code limits access to the `AccountController` to any authenticated user.</span></span>

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

<span data-ttu-id="9913e-107">コント ローラーではなく、アクションに承認を適用する場合は、適用、`AuthorizeAttribute`操作自体に属性します。</span><span class="sxs-lookup"><span data-stu-id="9913e-107">If you want to apply authorization to an action rather than the controller, apply the `AuthorizeAttribute` attribute to the action itself:</span></span>

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

<span data-ttu-id="9913e-108">認証されたユーザーのみがアクセスできるようになりました、`Logout`関数。</span><span class="sxs-lookup"><span data-stu-id="9913e-108">Now only authenticated users can access the `Logout` function.</span></span>

<span data-ttu-id="9913e-109">使用することも、`AllowAnonymousAttribute`を個別のアクションに認証されていないユーザーによるアクセスを許可する属性。</span><span class="sxs-lookup"><span data-stu-id="9913e-109">You can also use the `AllowAnonymousAttribute` attribute to allow access by non-authenticated users to individual actions.</span></span> <span data-ttu-id="9913e-110">例:</span><span class="sxs-lookup"><span data-stu-id="9913e-110">For example:</span></span>

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

<span data-ttu-id="9913e-111">認証されたユーザーのみをこのようにすると、 `AccountController`、を除き、`Login`がその認証されたか、認証されていない/匿名の状態に関係なく、だれがアクセス可能なアクションです。</span><span class="sxs-lookup"><span data-stu-id="9913e-111">This would allow only authenticated users to the `AccountController`, except for the `Login` action, which is accessible by everyone, regardless of their authenticated or unauthenticated / anonymous status.</span></span>

>[!WARNING]
> <span data-ttu-id="9913e-112">`[AllowAnonymous]`承認のすべてのステートメントをバイパスします。</span><span class="sxs-lookup"><span data-stu-id="9913e-112">`[AllowAnonymous]` bypasses all authorization statements.</span></span> <span data-ttu-id="9913e-113">結合を適用する場合`[AllowAnonymous]`と任意`[Authorize]`し、属性、Authorize 属性は常に無視されます。</span><span class="sxs-lookup"><span data-stu-id="9913e-113">If you apply combine `[AllowAnonymous]` and any `[Authorize]` attribute then the Authorize attributes will always be ignored.</span></span> <span data-ttu-id="9913e-114">適用する場合などは`[AllowAnonymous]`レベル、コント ローラーでいずれかの`[Authorize]`同じコント ローラーで、または、内の任意のアクションの属性は無視されます。</span><span class="sxs-lookup"><span data-stu-id="9913e-114">For example if you apply `[AllowAnonymous]` at the controller level any `[Authorize]` attributes on the same controller, or on any action within it will be ignored.</span></span>
