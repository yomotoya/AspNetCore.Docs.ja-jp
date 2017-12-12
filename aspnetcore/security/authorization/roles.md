---
title: "ロール ベースの承認"
author: rick-anderson
description: "このドキュメントでは、Authorize attribute にロールを渡すことで ASP.NET Core のコント ローラーとアクションのアクセスを制限する方法を示します。"
keywords: "ASP.NET Core、承認、ロール"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 5e014da1-8bc0-409b-951a-88b92c661fdf
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/roles
ms.openlocfilehash: 649b21d99c742843534748b0ba9d7b7b22483a62
ms.sourcegitcommit: 703593d5fd14076e79be2ba75a5b8da12a60ab15
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/05/2017
---
# <a name="role-based-authorization"></a>ロール ベースの承認

<a name="security-authorization-role-based"></a>

Id が作成されるときは、1 つまたは複数のロールに属する可能性があります。 たとえば、さんは、中、Scott は、ユーザー ロールにのみ属することがあります、管理者とユーザーのロールに属する可能性があります。 これらのロールを作成および管理する方法は、承認プロセスのバッキング ストアによって異なります。 開発者が使用するロールが公開されている、 [IsInRole](https://docs.microsoft.com/dotnet/api/system.security.principal.genericprincipal.isinrole)メソッドを[ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal)クラスです。

## <a name="adding-role-checks"></a>追加のロールのチェック

ロール ベースの承認チェックは、宣言型&mdash;開発者は、そのファイルを埋め込みますコント ローラーや、コント ローラー内のアクションに対して、コード内で要求されたリソースにアクセスするのメンバーである必要があります、現在のユーザー ロールを指定します。

たとえば、次のコードと制限操作へのアクセスに、`AdministrationController`のメンバーであるユーザーのユーザーに、`Administrator`グループ。

```csharp
[Authorize(Roles = "Administrator")]
public class AdministrationController : Controller
{
}
```

複数の役割は、コンマ区切りリストとして指定できます。

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

さらに操作レベルで追加の役割の承認属性を適用することによってアクセスを制限できます。

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

使用してポリシーが適用される、`Policy`プロパティを`AuthorizeAttribute`属性。

```csharp
[Authorize(Policy = "RequireAdministratorRole")]
public IActionResult Shutdown()
{
    return View();
}
```

必要条件で許可されている複数のロールを指定するかどうかは、パラメーターとして指定することができます、`RequireRole`メソッド。

```csharp
options.AddPolicy("ElevatedRights", policy =>
                  policy.RequireRole("Administrator", "PowerUser", "BackupAdministrator"));
```

この例に属しているユーザーの承認、 `Administrator`、`PowerUser`または`BackupAdministrator`ロール。
