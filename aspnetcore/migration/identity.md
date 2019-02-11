---
title: ASP.NET Core への認証と Id を移行します。
author: ardalis
description: ASP.NET MVC プロジェクトから ASP.NET Core MVC プロジェクトに認証と id を移行する方法について説明します。
ms.author: riande
ms.date: 10/14/2016
uid: migration/identity
ms.openlocfilehash: 72e62e78e37325ec47d54abbc11a875ae87fb63a
ms.sourcegitcommit: b72bbc9ae91e4bd37c9ea9b2d09ebf47afb25dd7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/11/2019
ms.locfileid: "41827922"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core"></a>ASP.NET Core への認証と Id を移行します。

作成者: [Steve Smith](https://ardalis.com/)

前の記事では[、ASP.NET MVC プロジェクトから ASP.NET Core MVC に構成を移行](xref:migration/configuration)します。 この記事では、登録、ログイン、およびユーザー管理機能を移行します。

## <a name="configure-identity-and-membership"></a>Id とメンバーシップを構成します。

ASP.NET Identity を使用して認証と id の機能が構成で ASP.NET MVC、 *Startup.Auth.cs*と*IdentityConfig.cs*にある、 *App_Start*フォルダー。 ASP.NET Core MVC では、これらの機能が構成されている*Startup.cs*します。

インストール、`Microsoft.AspNetCore.Identity.EntityFrameworkCore`と`Microsoft.AspNetCore.Authentication.Cookies`NuGet パッケージ。

次に、開きます*Startup.cs*し、更新、 `Startup.ConfigureServices` Entity Framework と Id サービスを使用するメソッド。

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

この時点では、ASP.NET MVC プロジェクトからまだ移行していない上記のコードで参照されている 2 つの種類があります:`ApplicationDbContext`と`ApplicationUser`します。 新規作成*モデル*フォルダー、ASP.NET core プロジェクト、およびこれらの型に対応するを 2 つのクラスを追加します。 ASP.NET MVC のバージョンのこれらのクラスを検索は */Models/IdentityModels.cs*がより明確では、移行したプロジェクトのクラスごとに 1 つのファイルが使用されます。

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
            // Customize the ASP.NET Core Identity model and override the defaults if needed.
            // For example, you can rename the ASP.NET Core Identity table names and more.
            // Add your customizations after calling base.OnModelCreating(builder);
        }
    }
}
```

ASP.NET Core MVC Starter Web プロジェクトには、ユーザーの多くのカスタマイズが含まれていない、または`ApplicationDbContext`します。 実際のアプリを移行するときにも移行に必要なすべてのカスタム プロパティと、アプリのユーザーのメソッドと`DbContext`クラスと他のモデル クラス、アプリで使用します。 たとえば場合、`DbContext`が、 `DbSet<Album>`、移行に必要な`Album`クラス。

これらのファイルの場所で、 *Startup.cs*ファイルを更新することでコンパイルできるその`using`ステートメント。

```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Identity;
using Microsoft.AspNetCore.Hosting;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
```

アプリは、認証と Id サービスをサポートする準備ができました。 ユーザーに公開される機能を備えること必要があります。

## <a name="migrate-registration-and-login-logic"></a>登録、ログインのロジックを移行します。

アプリの構成の Id サービスと Entity Framework と SQL Server を使用して構成されているデータ アクセスでは、登録、ログインのサポートをアプリに追加する準備ができました。 いることを思い出してください[移行プロセスで以前](xref:migration/mvc#migrate-the-layout-file)私たちはコメント アウトへの参照を *_LoginPartial*で *_Layout.cshtml*します。 ここは、そのコードに戻りのコメントを解除し、必要なコント ローラーおよびビューのログイン機能をサポートするために追加してみましょう。

コメントを解除、`@Html.Partial`行 *_Layout.cshtml*:

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

という新しい Razor ビューを次に、追加 *_LoginPartial*を*Views/shared*フォルダー。

Update *_LoginPartial.cshtml*を次のコード (すべての内容を置き換えます)。

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

この時点では、お使いのブラウザーでサイトを更新できる必要があります。

## <a name="summary"></a>まとめ

ASP.NET Core では、ASP.NET Identity 機能への変更が導入されています。 この記事では、ASP.NET Identity の認証とユーザーの管理機能を ASP.NET Core に移行する方法を説明しました。
