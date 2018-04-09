---
title: ASP.NET Core での単純な承認
author: rick-anderson
description: Authorize 属性を使用して ASP.NET Core コント ローラーとアクションへのアクセスを制限する方法を説明します。
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/simple
ms.openlocfilehash: cef5cb146c6c1ff052430748a9a64c6a822d6fa3
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/22/2018
---
# <a name="simple-authorization-in-aspnet-core"></a>ASP.NET Core での単純な承認

<a name="security-authorization-simple"></a>

MVC での承認はによって制御されます、`AuthorizeAttribute`属性とそのさまざまなパラメーターです。 簡単に言うと、適用、`AuthorizeAttribute`属性をコント ローラーまたはアクションへのアクセス制限コント ローラーまたは認証されたユーザーに操作します。

たとえば、次のコードがへのアクセスを制限、`AccountController`認証されたユーザーにします。

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

コント ローラーではなく、アクションに承認を適用する場合は、適用、`AuthorizeAttribute`操作自体に属性します。

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

認証されたユーザーのみがアクセスできるようになりました、`Logout`関数。

使用することも、`AllowAnonymous`を個別のアクションに認証されていないユーザーによるアクセスを許可する属性。 例えば:

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

認証されたユーザーのみをこのようにすると、 `AccountController`、を除き、`Login`がその認証されたか、認証されていない/匿名の状態に関係なく、だれがアクセス可能なアクションです。

>[!WARNING]
> `[AllowAnonymous]` 承認のすべてのステートメントをバイパスします。 結合を適用する場合`[AllowAnonymous]`と任意`[Authorize]`し、属性、Authorize 属性は常に無視されます。 適用する場合などは`[AllowAnonymous]`レベル、コント ローラーでいずれかの`[Authorize]`同じコント ローラーで、または、内の任意のアクションの属性は無視されます。
