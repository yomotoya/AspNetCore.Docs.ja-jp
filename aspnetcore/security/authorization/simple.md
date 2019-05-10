---
title: ASP.NET Core での単純な承認
author: rick-anderson
description: Authorize 属性を使用して ASP.NET Core コント ローラーおよびアクションへのアクセスを制限する方法について説明します。
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/simple
ms.openlocfilehash: 6409def0508b855d3d2a4a1f4d3a3d15bfe5dd32
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897679"
---
# <a name="simple-authorization-in-aspnet-core"></a>ASP.NET Core での単純な承認

<a name="security-authorization-simple"></a>

MVC での承認はによって制御されます、`AuthorizeAttribute`属性とそのさまざまなパラメーター。 簡単に言うと、適用、`AuthorizeAttribute`属性をコント ローラーまたはアクションの制限へのアクセスをコント ローラーまたはアクションを認証されたユーザー。

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

コント ローラーではなく、アクションに承認を適用する場合は、適用、`AuthorizeAttribute`アクション自体に属性します。

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

認証されたユーザーのみがアクセスできるので、`Logout`関数。

使用することも、`AllowAnonymous`個々 のアクションに認証されていないユーザーによるアクセスを許可する属性。 例:

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

認証されたユーザーのみになります、 `AccountController`、を除き、`Login`が、認証されたか、認証されていない/匿名の状態に関係なく、全員がアクセス可能なアクション。

> [!WARNING]
> `[AllowAnonymous]` すべての承認ステートメントをバイパスします。 組み合わせた場合`[AllowAnonymous]`任意と`[Authorize]`属性、`[Authorize]`属性は無視されます。 適用する場合の例の`[AllowAnonymous]`コント ローラー レベルでは、すべて`[Authorize]`同じコント ローラー (または、すべての操作) の属性は無視されます。
