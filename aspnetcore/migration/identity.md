---
title: ASP.NET Core への認証と Id を移行します。
author: ardalis
description: ASP.NET MVC プロジェクトから ASP.NET Core MVC プロジェクトに認証と id を移行する方法を説明します。
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/identity
ms.openlocfilehash: 2a80274e9056b41e370f199c7d41865db5fcedd7
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/08/2018
---
# <a name="migrate-authentication-and-identity-to-aspnet-core"></a>ASP.NET Core への認証と Id を移行します。

作成者: [Steve Smith](https://ardalis.com/)

前の記事お[ASP.NET MVC プロジェクトから ASP.NET Core MVC に構成を移行した](xref:migration/configuration)です。 この記事では、登録、ログイン、およびユーザー管理機能を移行します。

## <a name="configure-identity-and-membership"></a>Id およびメンバーシップを構成します。

ASP.NET mvc での認証と id 機能が構成で ASP.NET の Id を使用して*Startup.Auth.cs*と*IdentityConfig.cs*内にある、 *App_Start*フォルダーです。 ASP.NET Core mvc でこれらの機能が構成されている*Startup.cs*です。

インストール、`Microsoft.AspNetCore.Identity.EntityFrameworkCore`と`Microsoft.AspNetCore.Authentication.Cookies`NuGet パッケージの管理。

を開き*Startup.cs*し、更新、 `Startup.ConfigureServices` Entity Framework と Id サービスを使用する方法。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add EF services to the services container.
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));

    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

     services.AddMvc();
}
```

この時点では、ASP.NET MVC プロジェクトから移行していないまだ上記のコードで参照されている 2 つの種類があります:`ApplicationDbContext`と`ApplicationUser`です。 新しい*モデル*ASP.NET Core フォルダー プロジェクト、およびこれらの型に対応する 2 つのクラスを追加します。 見つかります ASP.NET MVC のバージョンのこれらのクラスで */Models/IdentityModels.cs*がより明確であるため移行対象のプロジェクト内のクラスごとに 1 ファイルが使用されます。

*ApplicationUser.cs*:

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvcProject.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

*ApplicationDbContext.cs*:

```csharp
using Microsoft.AspNetCore.Identity.EntityFramework;
using Microsoft.Data.Entity;

namespace NewMvcProject.Models
{
    public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
    {
        public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
            : base(options)
        {
        }

        protected override void OnModelCreating(ModelBuilder builder)
        {
            base.OnModelCreating(builder);
            // Customize the ASP.NET Identity model and override the defaults if needed.
            // For example, you can rename the ASP.NET Identity table names and more.
            // Add your customizations after calling base.OnModelCreating(builder);
        }
    }
}
```

ASP.NET Core MVC スターター Web プロジェクトには、ユーザーの多くのカスタマイズが含まれていない、または`ApplicationDbContext`です。 実際のアプリを移行するときにも移行する必要のすべてのカスタム プロパティと、アプリのユーザーのメソッドと`DbContext`クラスだけでなく、アプリが利用して他のモデル クラス。 たとえば場合、`DbContext`が、 `DbSet<Album>`、移行する必要があります、`Album`クラスです。

代わりに、これらのファイル、 *Startup.cs*を更新することでコンパイルするファイルができる、`using`ステートメント。

```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Identity;
using Microsoft.AspNetCore.Hosting;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
```

このアプリは、認証と Id サービスをサポートする準備ができました。 ユーザーに公開されるこれらの機能があること必要があります。

## <a name="migrate-registration-and-login-logic"></a>登録とログインのロジックを移行します。

アプリの構成の Id サービスと Entity Framework および SQL Server を使用して構成データにアクセスでは、アプリへの登録とログイン サポートを追加する準備ができました。 注意してください[移行プロセスの前半](xref:migration/mvc#migrate-the-layout-file)への参照をコメントお *_LoginPartial*で *_Layout.cshtml*です。 では、そのコードに返すコメントを解除し、必要なコント ローラーとログイン機能をサポートするためにビューに追加します。

コメントを解除、`@Html.Partial`線 *_Layout.cshtml*:

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

ここと呼ばれる新しい Razor ビューを追加 *_LoginPartial*を*Views/shared*フォルダー。

更新 *_LoginPartial.cshtml*を次のコード (すべての内容を置き換えます)。

```cshtml
@inject SignInManager<ApplicationUser> SignInManager
@inject UserManager<ApplicationUser> UserManager

@if (SignInManager.IsSignedIn(User))
{
    <form asp-area="" asp-controller="Account" asp-action="Logout" method="post" id="logoutForm" class="navbar-right">
        <ul class="nav navbar-nav navbar-right">
            <li>
                <a asp-area="" asp-controller="Manage" asp-action="Index" title="Manage">Hello @UserManager.GetUserName(User)!</a>
            </li>
            <li>
                <button type="submit" class="btn btn-link navbar-btn navbar-link">Log out</button>
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

## <a name="summary"></a>まとめ

ASP.NET Core では、ASP.NET Identity の機能への変更について説明します。 この記事では、ASP.NET Core に ASP.NET Id の認証とユーザー管理機能を移行する方法を説明しました。
