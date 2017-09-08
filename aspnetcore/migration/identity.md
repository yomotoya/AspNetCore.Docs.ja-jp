---
title: "移行の認証と Id"
author: ardalis
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 0db145cb-41a5-448a-b889-72e2d789ad7f
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/identity
ms.openlocfilehash: c86e9b98bcb43b383ac1077fe2749d0dfcd7392a
ms.sourcegitcommit: fb518f856f31fe53c09196a13309eacb85b37a22
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/08/2017
---
# <a name="migrating-authentication-and-identity"></a>移行の認証と Id

<a name=migration-identity></a>

によって[Steve Smith](http://ardalis.com)

前の記事お[ASP.NET MVC プロジェクトから ASP.NET Core MVC に構成を移行した](configuration.md)です。 この記事では、登録、ログイン、およびユーザー管理機能を移行します。

## <a name="configure-identity-and-membership"></a>Id およびメンバーシップを構成します。

ASP.NET mvc で Startup.Auth.cs および IdentityConfig.cs、App_Start フォルダーにある ASP.NET Id を使用して認証と id の機能が構成されます。 ASP.NET Core mvc でこれらの機能が構成されている*Startup.cs*です。

インストール、`Microsoft.AspNetCore.Identity.EntityFrameworkCore`と`Microsoft.AspNetCore.Authentication.Cookies`NuGet パッケージの管理。

その後、Startup.cs と更新プログラムを開く、 `ConfigureServices()` Entity Framework と Id サービスを使用する方法。

```csharp
public void ConfigureServices(IServiceCollection services)
{
  // Add EF services to the services container.
  services.AddEntityFramework(Configuration)
    .AddSqlServer()
    .AddDbContext<ApplicationDbContext>();

  // Add Identity services to the services container.
  services.AddIdentity<ApplicationUser, IdentityRole>(Configuration)
    .AddEntityFrameworkStores<ApplicationDbContext>();

  services.AddMvc();
}
```

この時点では、ASP.NET MVC プロジェクトから移行していないまだ上記のコードで参照されている 2 つの種類があります:`ApplicationDbContext`と`ApplicationUser`です。 新しい*モデル*ASP.NET Core フォルダー プロジェクト、およびこれらの型に対応する 2 つのクラスを追加します。 見つかります ASP.NET MVC のバージョンでこれらのクラスの`/Models/IdentityModels.cs`がより明確であるため移行対象のプロジェクト内のクラスごとに 1 ファイルが使用されます。

ApplicationUser.cs:

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#"} -->

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvc6Project.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

ApplicationDbContext.cs:

```csharp
using Microsoft.AspNetCore.Identity.EntityFramework;
using Microsoft.Data.Entity;

namespace NewMvc6Project.Models
{
  public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
  {
    public ApplicationDbContext()
    {
      Database.EnsureCreated();
    }

    protected override void OnConfiguring(DbContextOptionsBuilder options)
    {
      options.UseSqlServer();
    }
  }
}
```

ASP.NET Core MVC スターター Web プロジェクトは、多くのカスタマイズのユーザー、または、ApplicationDbContext に含まれていません。 すべてのカスタム プロパティと、アプリケーションのユーザーと DbContext クラスだけでなく、アプリケーション (たとえば、ある場合、DbContext DbSetを利用して他のモデルクラスのメソッドに移行する必要がありますに実際のアプリケーションを移行する場合<Album>、アルバムのクラスを移行する必要がありますもちろん)。

これらのファイルの場所で、Startup.cs ファイルにできるを使用して更新することでコンパイルするステートメント。

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#"} -->

```csharp
using Microsoft.Framework.ConfigurationModel;
using Microsoft.AspNetCore.Hosting;
using NewMvc6Project.Models;
using Microsoft.AspNetCore.Identity;
```

アプリケーションは認証と id サービスをサポートする準備ができました - これらの機能をユーザーに公開するだけで済みます。

## <a name="migrate-registration-and-login-logic"></a>登録とログインのロジックを移行します。

Id サービスを使用するアプリケーションおよび Entity Framework および SQL Server を使用して構成データ アクセス用に構成されたおと、アプリケーションへの登録とログイン サポートを追加する準備ができました。 注意してください[移行プロセスの前半](mvc.md#migrate-layout-file)おをコメント アウト _Layout.cshtml で _LoginPartial への参照。 では、そのコードに返すコメントを解除し、必要なコント ローラーとログイン機能をサポートするためにビューに追加します。

_Layout.cshtml; の更新します。コメントを解除、@Html.Partial行。

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "none"} -->

```none
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

ビュー/共有フォルダーに _LoginPartial と呼ばれる新しい MVC ビュー ページを追加します。

次のコードで _LoginPartial.cshtml を更新 (すべての内容を置き換えます)。

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#"} -->

```csharp
@inject SignInManager<User> SignInManager
@inject UserManager<User> UserManager

@if (SignInManager.IsSignedIn(User))
{
    <form asp-area="" asp-controller="Account" asp-action="LogOff" method="post" id="logoutForm" class="navbar-right">
        <ul class="nav navbar-nav navbar-right">
            <li>
                <a asp-area="" asp-controller="Manage" asp-action="Index" title="Manage">Hello @UserManager.GetUserName(User)!</a>
            </li>
            <li>
                <button type="submit" class="btn btn-link navbar-btn navbar-link">Log off</button>
            </li>
        </ul>
    </form>
}
else
{
    <ul class="nav navbar-nav navbar-right">
        <li><a asp-area="" asp-controller="Account" asp-action="Register">Register</a></li>
        <li><a asp-area="" asp-controller="Account" asp-action="Login">Log in</a></li>
    </ul>
}
```

この時点では、ブラウザーでサイトを更新することができます。

## <a name="summary"></a>概要

ASP.NET Core では、ASP.NET Identity の機能への変更について説明します。 この記事では、ASP.NET Core に ASP.NET Id の認証とユーザー管理機能を移行する方法を説明しました。
