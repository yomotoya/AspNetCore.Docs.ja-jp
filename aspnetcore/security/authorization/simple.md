---
title: "単純な承認"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 391bcaad-205f-43e4-badc-fa592d6f79f3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/simple
ms.openlocfilehash: 91f402b1cbf73e212418d197a8a7230ce22e9e1d
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/13/2017
---
# <a name="simple-authorization"></a><span data-ttu-id="b4900-103">単純な承認</span><span class="sxs-lookup"><span data-stu-id="b4900-103">Simple Authorization</span></span>

<a name="security-authorization-simple"></a>

<span data-ttu-id="b4900-104">MVC での承認はによって制御されます、`AuthorizeAttribute`属性とそのさまざまなパラメーターです。</span><span class="sxs-lookup"><span data-stu-id="b4900-104">Authorization in MVC is controlled through the `AuthorizeAttribute` attribute and its various parameters.</span></span> <span data-ttu-id="b4900-105">その最も簡単な適用することで、`AuthorizeAttribute`属性をコント ローラーまたはアクションへのアクセス制限コント ローラーまたは認証されたユーザーに操作します。</span><span class="sxs-lookup"><span data-stu-id="b4900-105">At its simplest applying the `AuthorizeAttribute` attribute to a controller or action limits access to the controller or action to any authenticated user.</span></span>

<span data-ttu-id="b4900-106">たとえば、次のコードがへのアクセスを制限、`AccountController`認証されたユーザーにします。</span><span class="sxs-lookup"><span data-stu-id="b4900-106">For example, the following code limits access to the `AccountController` to any authenticated user.</span></span>

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

<span data-ttu-id="b4900-107">単に、コント ローラーではなく、アクションに承認を適用する場合は、適用、`AuthorizeAttribute`属性をアクション自体です。</span><span class="sxs-lookup"><span data-stu-id="b4900-107">If you want to apply authorization to an action rather than the controller simply apply the `AuthorizeAttribute` attribute to the action itself;</span></span>

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

<span data-ttu-id="b4900-108">これで認証されたユーザーだけは logout 関数にアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="b4900-108">Now only authenticated users can access the logout function.</span></span>

<span data-ttu-id="b4900-109">使用することも、`AllowAnonymousAttribute`を個々 のアクションに認証されていないユーザーによるアクセスを許可する属性の例</span><span class="sxs-lookup"><span data-stu-id="b4900-109">You can also use the `AllowAnonymousAttribute` attribute to allow access by non-authenticated users to individual actions; for example</span></span>

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

<span data-ttu-id="b4900-110">認証されたユーザーのみをこのようにすると、 `AccountController`、を除き、`Login`がその認証されたか、認証されていない/匿名の状態に関係なく、だれがアクセス可能なアクションです。</span><span class="sxs-lookup"><span data-stu-id="b4900-110">This would allow only authenticated users to the `AccountController`, except for the `Login` action, which is accessible by everyone, regardless of their authenticated or unauthenticated / anonymous status.</span></span>

>[!WARNING]
> <span data-ttu-id="b4900-111">`[AllowAnonymous]`承認のすべてのステートメントをバイパスします。</span><span class="sxs-lookup"><span data-stu-id="b4900-111">`[AllowAnonymous]` bypasses all authorization statements.</span></span> <span data-ttu-id="b4900-112">結合を適用する場合`[AllowAnonymous]`と任意`[Authorize]`し、属性、Authorize 属性は常に無視されます。</span><span class="sxs-lookup"><span data-stu-id="b4900-112">If you apply combine `[AllowAnonymous]` and any `[Authorize]` attribute then the Authorize attributes will always be ignored.</span></span> <span data-ttu-id="b4900-113">適用する場合などは`[AllowAnonymous]`レベル、コント ローラーでいずれかの`[Authorize]`同じコント ローラーで、または、内の任意のアクションの属性は無視されます。</span><span class="sxs-lookup"><span data-stu-id="b4900-113">For example if you apply `[AllowAnonymous]` at the controller level any `[Authorize]` attributes on the same controller, or on any action within it will be ignored.</span></span>
