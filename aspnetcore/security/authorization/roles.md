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
# <a name="role-based-authorization"></a>ロール ベースの承認

<a name=security-authorization-role-based></a>

Id の作成時に 1 つまたは複数のロールに属している必要があります、中、Scott は、ユーザー ロールにのみ属することがありますを管理者およびユーザー ロールに属することがさんの例を示します。 これらのロールを作成および管理する方法は、承認プロセスのバッキング ストアによって異なります。 開発者が使用するロールが公開されている、 [IsInRole](https://msdn.microsoft.com/library/system.security.claims.claimsprincipal.isinrole(v=vs.110).aspx)プロパティを[ClaimsPrincipal](https://msdn.microsoft.com/library/system.security.claims.claimsprincipal(v=vs.110).aspx)クラスです。

## <a name="adding-role-checks"></a>追加のロールのチェック

ロール ベースの承認チェックは、宣言型 - 開発者は、そのファイルを埋め込みますコント ローラーや、コント ローラー内のアクションに対して、コード内で要求されたリソースにアクセスするのメンバーである必要があります、現在のユーザー ロールを指定します。

たとえば、次のコード制限操作へのアクセスに、`AdministrationController`のメンバーであるユーザーのユーザーに、`Administrator`グループ。

```csharp
[Authorize(Roles = "Administrator")]
public class AdministrationController : Controller
{
}
```

コンマ区切りのリストです。 として複数のロールを指定することができます。

```csharp
[Authorize(Roles = "HRManager,Finance")]
public class SalaryController : Controller
{
}
```

このコント ローラーにはメンバーであるユーザーがアクセスするだけの`HRManager`ロールまたは`Finance`ロール。

アクセスするユーザーは、指定したすべてのロールのメンバーである必要があります、複数の属性を適用する場合次の例では、ユーザーは両方のメンバーである必要がありますが必要です、`PowerUser`と`ControlPanelUser`ロール。

```csharp
[Authorize(Roles = "PowerUser")]
[Authorize(Roles = "ControlPanelUser")]
public class ControlPanelController : Controller
{
}
```

さらに操作レベルで追加の役割の承認属性を適用することによってアクセスを制限することができます。

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

前のコード スニペットのメンバーで、`Administrator`ロールまたは`PowerUser`ロールがアクセスできる、コント ローラーと`SetTime`アクションがのメンバーだけ、`Administrator`ロールがアクセスできる、`ShutDown`アクション。

コント ローラーをロックは個別のアクションに匿名で認証されていないアクセスを許可することもできます。

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

## <a name="policy-based-role-checks"></a>ポリシー ベースの役割の確認

構文を使用して、新しいポリシー、認証サービスの構成の一部として、開発者が起動時にポリシーを登録の役割の要件を表現することもできます。 通常は、この`ConfigureServices()`で、 *Startup.cs*ファイル。

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

使用してポリシーが適用される、`Policy`プロパティを`AuthorizeAttribute`属性です。

```csharp
[Authorize(Policy = "RequireAdministratorRole")]
public IActionResult Shutdown()
{
    return View();
}
```

必要条件で許可されている複数のロールを指定するかどうかは、パラメーターとして指定することができます、`RequireRole`メソッドです。

```csharp
options.AddPolicy("ElevatedRights", policy =>
                  policy.RequireRole("Administrator", "PowerUser", "BackupAdministrator"));
```

この例に属しているユーザーの承認、 `Administrator`、`PowerUser`または`BackupAdministrator`ロール。
