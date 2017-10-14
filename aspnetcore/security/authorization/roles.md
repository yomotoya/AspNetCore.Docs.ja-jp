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
# <a name="role-based-authorization"></a>ロール ベースの承認

<a name="security-authorization-role-based"></a>

Id の作成時に 1 つまたは複数のロールに属している必要があります、中、Scott は、ユーザー ロールにのみ属することがありますを管理者およびユーザー ロールに属することがさんの例を示します。 これらのロールを作成および管理する方法は、承認プロセスのバッキング ストアによって異なります。 開発者が使用するロールが公開されている、 [IsInRole](https://docs.microsoft.com/dotnet/api/system.security.principal.genericprincipal.isinrole)プロパティを[ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal)クラスです。

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

<a name="security-authorization-role-policy"></a>

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
