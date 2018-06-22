---
title: ASP.NET Core への認証と Id を移行します。
author: ardalis
description: ASP.NET MVC プロジェクトから ASP.NET Core MVC プロジェクトに認証と id を移行する方法を説明します。
ms.author: riande
ms.date: 10/14/2016
uid: migration/identity
ms.openlocfilehash: e05d72ca78c7b8191a47f78cda31ee40e04d0706
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275698"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core"></a><span data-ttu-id="80b89-103">ASP.NET Core への認証と Id を移行します。</span><span class="sxs-lookup"><span data-stu-id="80b89-103">Migrate Authentication and Identity to ASP.NET Core</span></span>

<span data-ttu-id="80b89-104">作成者: [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="80b89-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="80b89-105">前の記事お[ASP.NET MVC プロジェクトから ASP.NET Core MVC に構成を移行した](xref:migration/configuration)です。</span><span class="sxs-lookup"><span data-stu-id="80b89-105">In the previous article, we [migrated configuration from an ASP.NET MVC project to ASP.NET Core MVC](xref:migration/configuration).</span></span> <span data-ttu-id="80b89-106">この記事では、登録、ログイン、およびユーザー管理機能を移行します。</span><span class="sxs-lookup"><span data-stu-id="80b89-106">In this article, we migrate the registration, login, and user management features.</span></span>

## <a name="configure-identity-and-membership"></a><span data-ttu-id="80b89-107">Id およびメンバーシップを構成します。</span><span class="sxs-lookup"><span data-stu-id="80b89-107">Configure Identity and Membership</span></span>

<span data-ttu-id="80b89-108">ASP.NET mvc での認証と id 機能が構成で ASP.NET の Id を使用して*Startup.Auth.cs*と*IdentityConfig.cs*内にある、 *App_Start*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="80b89-108">In ASP.NET MVC, authentication and identity features are configured using ASP.NET Identity in *Startup.Auth.cs* and *IdentityConfig.cs*, located in the *App_Start* folder.</span></span> <span data-ttu-id="80b89-109">ASP.NET Core mvc でこれらの機能が構成されている*Startup.cs*です。</span><span class="sxs-lookup"><span data-stu-id="80b89-109">In ASP.NET Core MVC, these features are configured in *Startup.cs*.</span></span>

<span data-ttu-id="80b89-110">インストール、`Microsoft.AspNetCore.Identity.EntityFrameworkCore`と`Microsoft.AspNetCore.Authentication.Cookies`NuGet パッケージの管理。</span><span class="sxs-lookup"><span data-stu-id="80b89-110">Install the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` and `Microsoft.AspNetCore.Authentication.Cookies` NuGet packages.</span></span>

<span data-ttu-id="80b89-111">を開き*Startup.cs*し、更新、 `Startup.ConfigureServices` Entity Framework と Id サービスを使用する方法。</span><span class="sxs-lookup"><span data-stu-id="80b89-111">Then, open *Startup.cs* and update the `Startup.ConfigureServices` method to use Entity Framework and Identity services:</span></span>

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

<span data-ttu-id="80b89-112">この時点では、ASP.NET MVC プロジェクトから移行していないまだ上記のコードで参照されている 2 つの種類があります:`ApplicationDbContext`と`ApplicationUser`です。</span><span class="sxs-lookup"><span data-stu-id="80b89-112">At this point, there are two types referenced in the above code that we haven't yet migrated from the ASP.NET MVC project: `ApplicationDbContext` and `ApplicationUser`.</span></span> <span data-ttu-id="80b89-113">新しい*モデル*ASP.NET Core フォルダー プロジェクト、およびこれらの型に対応する 2 つのクラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="80b89-113">Create a new *Models* folder in the ASP.NET Core project, and add two classes to it corresponding to these types.</span></span> <span data-ttu-id="80b89-114">見つかります ASP.NET MVC のバージョンのこれらのクラスで */Models/IdentityModels.cs*がより明確であるため移行対象のプロジェクト内のクラスごとに 1 ファイルが使用されます。</span><span class="sxs-lookup"><span data-stu-id="80b89-114">You will find the ASP.NET MVC versions of these classes in */Models/IdentityModels.cs*, but we will use one file per class in the migrated project since that's more clear.</span></span>

<span data-ttu-id="80b89-115">*ApplicationUser.cs*:</span><span class="sxs-lookup"><span data-stu-id="80b89-115">*ApplicationUser.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvcProject.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

<span data-ttu-id="80b89-116">*ApplicationDbContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="80b89-116">*ApplicationDbContext.cs*:</span></span>

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

<span data-ttu-id="80b89-117">ASP.NET Core MVC スターター Web プロジェクトには、ユーザーの多くのカスタマイズが含まれていない、または`ApplicationDbContext`です。</span><span class="sxs-lookup"><span data-stu-id="80b89-117">The ASP.NET Core MVC Starter Web project doesn't include much customization of users, or the `ApplicationDbContext`.</span></span> <span data-ttu-id="80b89-118">実際のアプリを移行するときにも移行する必要のすべてのカスタム プロパティと、アプリのユーザーのメソッドと`DbContext`クラスだけでなく、アプリが利用して他のモデル クラス。</span><span class="sxs-lookup"><span data-stu-id="80b89-118">When migrating a real app, you also need to migrate all of the custom properties and methods of your app's user and `DbContext` classes, as well as any other Model classes your app utilizes.</span></span> <span data-ttu-id="80b89-119">たとえば場合、`DbContext`が、 `DbSet<Album>`、移行する必要があります、`Album`クラスです。</span><span class="sxs-lookup"><span data-stu-id="80b89-119">For example, if your `DbContext` has a `DbSet<Album>`, you need to migrate the `Album` class.</span></span>

<span data-ttu-id="80b89-120">代わりに、これらのファイル、 *Startup.cs*を更新することでコンパイルするファイルができる、`using`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="80b89-120">With these files in place, the *Startup.cs* file can be made to compile by updating its `using` statements:</span></span>

```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Identity;
using Microsoft.AspNetCore.Hosting;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
```

<span data-ttu-id="80b89-121">このアプリは、認証と Id サービスをサポートする準備ができました。</span><span class="sxs-lookup"><span data-stu-id="80b89-121">Our app is now ready to support authentication and Identity services.</span></span> <span data-ttu-id="80b89-122">ユーザーに公開されるこれらの機能があること必要があります。</span><span class="sxs-lookup"><span data-stu-id="80b89-122">It just needs to have these features exposed to users.</span></span>

## <a name="migrate-registration-and-login-logic"></a><span data-ttu-id="80b89-123">登録とログインのロジックを移行します。</span><span class="sxs-lookup"><span data-stu-id="80b89-123">Migrate registration and login logic</span></span>

<span data-ttu-id="80b89-124">アプリの構成の Id サービスと Entity Framework および SQL Server を使用して構成データにアクセスでは、アプリへの登録とログイン サポートを追加する準備ができました。</span><span class="sxs-lookup"><span data-stu-id="80b89-124">With Identity services configured for the app and data access configured using Entity Framework and SQL Server, we're ready to add support for registration and login to the app.</span></span> <span data-ttu-id="80b89-125">注意してください[移行プロセスの前半](xref:migration/mvc#migrate-the-layout-file)への参照をコメントお *_LoginPartial*で *_Layout.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="80b89-125">Recall that [earlier in the migration process](xref:migration/mvc#migrate-the-layout-file) we commented out a reference to *_LoginPartial* in *_Layout.cshtml*.</span></span> <span data-ttu-id="80b89-126">では、そのコードに返すコメントを解除し、必要なコント ローラーとログイン機能をサポートするためにビューに追加します。</span><span class="sxs-lookup"><span data-stu-id="80b89-126">Now it's time to return to that code, uncomment it, and add in the necessary controllers and views to support login functionality.</span></span>

<span data-ttu-id="80b89-127">コメントを解除、`@Html.Partial`線 *_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="80b89-127">Uncomment the `@Html.Partial` line in *_Layout.cshtml*:</span></span>

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

<span data-ttu-id="80b89-128">ここと呼ばれる新しい Razor ビューを追加 *_LoginPartial*を*Views/shared*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="80b89-128">Now, add a new Razor view called *_LoginPartial* to the *Views/Shared* folder:</span></span>

<span data-ttu-id="80b89-129">更新 *_LoginPartial.cshtml*を次のコード (すべての内容を置き換えます)。</span><span class="sxs-lookup"><span data-stu-id="80b89-129">Update *_LoginPartial.cshtml* with the following code (replace all of its contents):</span></span>

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

<span data-ttu-id="80b89-130">この時点では、ブラウザーでサイトを更新することができます。</span><span class="sxs-lookup"><span data-stu-id="80b89-130">At this point, you should be able to refresh the site in your browser.</span></span>

## <a name="summary"></a><span data-ttu-id="80b89-131">まとめ</span><span class="sxs-lookup"><span data-stu-id="80b89-131">Summary</span></span>

<span data-ttu-id="80b89-132">ASP.NET Core では、ASP.NET Identity の機能への変更について説明します。</span><span class="sxs-lookup"><span data-stu-id="80b89-132">ASP.NET Core introduces changes to the ASP.NET Identity features.</span></span> <span data-ttu-id="80b89-133">この記事では、ASP.NET Core に ASP.NET Id の認証とユーザー管理機能を移行する方法を説明しました。</span><span class="sxs-lookup"><span data-stu-id="80b89-133">In this article, you have seen how to migrate the authentication and user management features of ASP.NET Identity to ASP.NET Core.</span></span>
