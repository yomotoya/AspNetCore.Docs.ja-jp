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
# <a name="simple-authorization"></a>単純な承認

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

使用することも、`AllowAnonymousAttribute`を個別のアクションに認証されていないユーザーによるアクセスを許可する属性。 例:

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
> `[AllowAnonymous]`承認のすべてのステートメントをバイパスします。 結合を適用する場合`[AllowAnonymous]`と任意`[Authorize]`し、属性、Authorize 属性は常に無視されます。 適用する場合などは`[AllowAnonymous]`レベル、コント ローラーでいずれかの`[Authorize]`同じコント ローラーで、または、内の任意のアクションの属性は無視されます。
