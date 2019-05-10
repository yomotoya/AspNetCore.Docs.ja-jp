---
title: ASP.NET Core への認証と Id を移行します。
author: ardalis
description: ASP.NET MVC プロジェクトから ASP.NET Core MVC プロジェクトに認証と id を移行する方法について説明します。
ms.author: riande
ms.date: 10/14/2016
uid: migration/identity
ms.openlocfilehash: 72e62e78e37325ec47d54abbc11a875ae87fb63a
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2019
ms.locfileid: "64891879"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core"></a><span data-ttu-id="63876-103">ASP.NET Core への認証と Id を移行します。</span><span class="sxs-lookup"><span data-stu-id="63876-103">Migrate Authentication and Identity to ASP.NET Core</span></span>

<span data-ttu-id="63876-104">作成者: [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="63876-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="63876-105">前の記事では[、ASP.NET MVC プロジェクトから ASP.NET Core MVC に構成を移行](xref:migration/configuration)します。</span><span class="sxs-lookup"><span data-stu-id="63876-105">In the previous article, we [migrated configuration from an ASP.NET MVC project to ASP.NET Core MVC](xref:migration/configuration).</span></span> <span data-ttu-id="63876-106">この記事では、登録、ログイン、およびユーザー管理機能を移行します。</span><span class="sxs-lookup"><span data-stu-id="63876-106">In this article, we migrate the registration, login, and user management features.</span></span>

## <a name="configure-identity-and-membership"></a><span data-ttu-id="63876-107">Id とメンバーシップを構成します。</span><span class="sxs-lookup"><span data-stu-id="63876-107">Configure Identity and Membership</span></span>

<span data-ttu-id="63876-108">ASP.NET Identity を使用して認証と id の機能が構成で ASP.NET MVC、 *Startup.Auth.cs*と*IdentityConfig.cs*にある、 *App_Start*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="63876-108">In ASP.NET MVC, authentication and identity features are configured using ASP.NET Identity in *Startup.Auth.cs* and *IdentityConfig.cs*, located in the *App_Start* folder.</span></span> <span data-ttu-id="63876-109">ASP.NET Core MVC では、これらの機能が構成されている*Startup.cs*します。</span><span class="sxs-lookup"><span data-stu-id="63876-109">In ASP.NET Core MVC, these features are configured in *Startup.cs*.</span></span>

<span data-ttu-id="63876-110">インストール、`Microsoft.AspNetCore.Identity.EntityFrameworkCore`と`Microsoft.AspNetCore.Authentication.Cookies`NuGet パッケージ。</span><span class="sxs-lookup"><span data-stu-id="63876-110">Install the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` and `Microsoft.AspNetCore.Authentication.Cookies` NuGet packages.</span></span>

<span data-ttu-id="63876-111">次に、開きます*Startup.cs*し、更新、 `Startup.ConfigureServices` Entity Framework と Id サービスを使用するメソッド。</span><span class="sxs-lookup"><span data-stu-id="63876-111">Then, open *Startup.cs* and update the `Startup.ConfigureServices` method to use Entity Framework and Identity services:</span></span>

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

<span data-ttu-id="63876-112">この時点では、ASP.NET MVC プロジェクトからまだ移行していない上記のコードで参照されている 2 つの種類があります:`ApplicationDbContext`と`ApplicationUser`します。</span><span class="sxs-lookup"><span data-stu-id="63876-112">At this point, there are two types referenced in the above code that we haven't yet migrated from the ASP.NET MVC project: `ApplicationDbContext` and `ApplicationUser`.</span></span> <span data-ttu-id="63876-113">新規作成*モデル*フォルダー、ASP.NET core プロジェクト、およびこれらの型に対応するを 2 つのクラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="63876-113">Create a new *Models* folder in the ASP.NET Core project, and add two classes to it corresponding to these types.</span></span> <span data-ttu-id="63876-114">ASP.NET MVC のバージョンのこれらのクラスを検索は */Models/IdentityModels.cs*がより明確では、移行したプロジェクトのクラスごとに 1 つのファイルが使用されます。</span><span class="sxs-lookup"><span data-stu-id="63876-114">You will find the ASP.NET MVC versions of these classes in */Models/IdentityModels.cs*, but we will use one file per class in the migrated project since that's more clear.</span></span>

<span data-ttu-id="63876-115">*ApplicationUser.cs*:</span><span class="sxs-lookup"><span data-stu-id="63876-115">*ApplicationUser.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvcProject.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

<span data-ttu-id="63876-116">*ApplicationDbContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="63876-116">*ApplicationDbContext.cs*:</span></span>

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

<span data-ttu-id="63876-117">ASP.NET Core MVC Starter Web プロジェクトには、ユーザーの多くのカスタマイズが含まれていない、または`ApplicationDbContext`します。</span><span class="sxs-lookup"><span data-stu-id="63876-117">The ASP.NET Core MVC Starter Web project doesn't include much customization of users, or the `ApplicationDbContext`.</span></span> <span data-ttu-id="63876-118">実際のアプリを移行するときにも移行に必要なすべてのカスタム プロパティと、アプリのユーザーのメソッドと`DbContext`クラスと他のモデル クラス、アプリで使用します。</span><span class="sxs-lookup"><span data-stu-id="63876-118">When migrating a real app, you also need to migrate all of the custom properties and methods of your app's user and `DbContext` classes, as well as any other Model classes your app utilizes.</span></span> <span data-ttu-id="63876-119">たとえば場合、`DbContext`が、 `DbSet<Album>`、移行に必要な`Album`クラス。</span><span class="sxs-lookup"><span data-stu-id="63876-119">For example, if your `DbContext` has a `DbSet<Album>`, you need to migrate the `Album` class.</span></span>

<span data-ttu-id="63876-120">これらのファイルの場所で、 *Startup.cs*ファイルを更新することでコンパイルできるその`using`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="63876-120">With these files in place, the *Startup.cs* file can be made to compile by updating its `using` statements:</span></span>

```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Identity;
using Microsoft.AspNetCore.Hosting;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
```

<span data-ttu-id="63876-121">アプリは、認証と Id サービスをサポートする準備ができました。</span><span class="sxs-lookup"><span data-stu-id="63876-121">Our app is now ready to support authentication and Identity services.</span></span> <span data-ttu-id="63876-122">ユーザーに公開される機能を備えること必要があります。</span><span class="sxs-lookup"><span data-stu-id="63876-122">It just needs to have these features exposed to users.</span></span>

## <a name="migrate-registration-and-login-logic"></a><span data-ttu-id="63876-123">登録、ログインのロジックを移行します。</span><span class="sxs-lookup"><span data-stu-id="63876-123">Migrate registration and login logic</span></span>

<span data-ttu-id="63876-124">アプリの構成の Id サービスと Entity Framework と SQL Server を使用して構成されているデータ アクセスでは、登録、ログインのサポートをアプリに追加する準備ができました。</span><span class="sxs-lookup"><span data-stu-id="63876-124">With Identity services configured for the app and data access configured using Entity Framework and SQL Server, we're ready to add support for registration and login to the app.</span></span> <span data-ttu-id="63876-125">いることを思い出してください[移行プロセスで以前](xref:migration/mvc#migrate-the-layout-file)私たちはコメント アウトへの参照を *_LoginPartial*で *_Layout.cshtml*します。</span><span class="sxs-lookup"><span data-stu-id="63876-125">Recall that [earlier in the migration process](xref:migration/mvc#migrate-the-layout-file) we commented out a reference to *_LoginPartial* in *_Layout.cshtml*.</span></span> <span data-ttu-id="63876-126">ここは、そのコードに戻りのコメントを解除し、必要なコント ローラーおよびビューのログイン機能をサポートするために追加してみましょう。</span><span class="sxs-lookup"><span data-stu-id="63876-126">Now it's time to return to that code, uncomment it, and add in the necessary controllers and views to support login functionality.</span></span>

<span data-ttu-id="63876-127">コメントを解除、`@Html.Partial`行 *_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="63876-127">Uncomment the `@Html.Partial` line in *_Layout.cshtml*:</span></span>

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

<span data-ttu-id="63876-128">という新しい Razor ビューを次に、追加 *_LoginPartial*を*Views/shared*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="63876-128">Now, add a new Razor view called *_LoginPartial* to the *Views/Shared* folder:</span></span>

<span data-ttu-id="63876-129">Update *_LoginPartial.cshtml*を次のコード (すべての内容を置き換えます)。</span><span class="sxs-lookup"><span data-stu-id="63876-129">Update *_LoginPartial.cshtml* with the following code (replace all of its contents):</span></span>

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

<span data-ttu-id="63876-130">この時点では、お使いのブラウザーでサイトを更新できる必要があります。</span><span class="sxs-lookup"><span data-stu-id="63876-130">At this point, you should be able to refresh the site in your browser.</span></span>

## <a name="summary"></a><span data-ttu-id="63876-131">まとめ</span><span class="sxs-lookup"><span data-stu-id="63876-131">Summary</span></span>

<span data-ttu-id="63876-132">ASP.NET Core では、ASP.NET Identity 機能への変更が導入されています。</span><span class="sxs-lookup"><span data-stu-id="63876-132">ASP.NET Core introduces changes to the ASP.NET Identity features.</span></span> <span data-ttu-id="63876-133">この記事では、ASP.NET Identity の認証とユーザーの管理機能を ASP.NET Core に移行する方法を説明しました。</span><span class="sxs-lookup"><span data-stu-id="63876-133">In this article, you have seen how to migrate the authentication and user management features of ASP.NET Identity to ASP.NET Core.</span></span>
