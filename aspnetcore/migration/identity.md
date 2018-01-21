---
title: "移行の認証と Id"
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/identity
ms.openlocfilehash: a3aa976578d8db089c69bf888f492f9cd7df824b
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="migrating-authentication-and-identity"></a><span data-ttu-id="5b049-102">移行の認証と Id</span><span class="sxs-lookup"><span data-stu-id="5b049-102">Migrating Authentication and Identity</span></span>

<a name="migration-identity"></a>

<span data-ttu-id="5b049-103">によって[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="5b049-103">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="5b049-104">前の記事お[ASP.NET MVC プロジェクトから ASP.NET Core MVC に構成を移行した](configuration.md)です。</span><span class="sxs-lookup"><span data-stu-id="5b049-104">In the previous article we [migrated configuration from an ASP.NET MVC project to ASP.NET Core MVC](configuration.md).</span></span> <span data-ttu-id="5b049-105">この記事では、登録、ログイン、およびユーザー管理機能を移行します。</span><span class="sxs-lookup"><span data-stu-id="5b049-105">In this article, we migrate the registration, login, and user management features.</span></span>

## <a name="configure-identity-and-membership"></a><span data-ttu-id="5b049-106">Id およびメンバーシップを構成します。</span><span class="sxs-lookup"><span data-stu-id="5b049-106">Configure Identity and Membership</span></span>

<span data-ttu-id="5b049-107">ASP.NET mvc で Startup.Auth.cs および IdentityConfig.cs、App_Start フォルダーにある ASP.NET Id を使用して認証と id の機能が構成されます。</span><span class="sxs-lookup"><span data-stu-id="5b049-107">In ASP.NET MVC, authentication and identity features are configured using ASP.NET Identity in Startup.Auth.cs and IdentityConfig.cs, located in the App_Start folder.</span></span> <span data-ttu-id="5b049-108">ASP.NET Core mvc でこれらの機能が構成されている*Startup.cs*です。</span><span class="sxs-lookup"><span data-stu-id="5b049-108">In ASP.NET Core MVC, these features are configured in *Startup.cs*.</span></span>

<span data-ttu-id="5b049-109">インストール、`Microsoft.AspNetCore.Identity.EntityFrameworkCore`と`Microsoft.AspNetCore.Authentication.Cookies`NuGet パッケージの管理。</span><span class="sxs-lookup"><span data-stu-id="5b049-109">Install the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` and `Microsoft.AspNetCore.Authentication.Cookies` NuGet packages.</span></span>

<span data-ttu-id="5b049-110">その後、Startup.cs と更新プログラムを開く、 `ConfigureServices()` Entity Framework と Id サービスを使用する方法。</span><span class="sxs-lookup"><span data-stu-id="5b049-110">Then, open Startup.cs and update the `ConfigureServices()` method to use Entity Framework and Identity services:</span></span>

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

<span data-ttu-id="5b049-111">この時点では、ASP.NET MVC プロジェクトから移行していないまだ上記のコードで参照されている 2 つの種類があります:`ApplicationDbContext`と`ApplicationUser`です。</span><span class="sxs-lookup"><span data-stu-id="5b049-111">At this point, there are two types referenced in the above code that we haven't yet migrated from the ASP.NET MVC project: `ApplicationDbContext` and `ApplicationUser`.</span></span> <span data-ttu-id="5b049-112">新しい*モデル*ASP.NET Core フォルダー プロジェクト、およびこれらの型に対応する 2 つのクラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="5b049-112">Create a new *Models* folder in the ASP.NET Core project, and add two classes to it corresponding to these types.</span></span> <span data-ttu-id="5b049-113">見つかります ASP.NET MVC のバージョンでこれらのクラスの`/Models/IdentityModels.cs`がより明確であるため移行対象のプロジェクト内のクラスごとに 1 ファイルが使用されます。</span><span class="sxs-lookup"><span data-stu-id="5b049-113">You will find the ASP.NET MVC versions of these classes in `/Models/IdentityModels.cs`, but we will use one file per class in the migrated project since that's more clear.</span></span>

<span data-ttu-id="5b049-114">ApplicationUser.cs:</span><span class="sxs-lookup"><span data-stu-id="5b049-114">ApplicationUser.cs:</span></span>

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvc6Project.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

<span data-ttu-id="5b049-115">ApplicationDbContext.cs:</span><span class="sxs-lookup"><span data-stu-id="5b049-115">ApplicationDbContext.cs:</span></span>

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

<span data-ttu-id="5b049-116">ASP.NET Core MVC スターター Web プロジェクトは、多くのカスタマイズのユーザー、または、ApplicationDbContext に含まれていません。</span><span class="sxs-lookup"><span data-stu-id="5b049-116">The ASP.NET Core MVC Starter Web project doesn't include much customization of users, or the ApplicationDbContext.</span></span> <span data-ttu-id="5b049-117">すべてのカスタム プロパティと、アプリケーションのユーザーと DbContext クラスだけでなく、アプリケーション (たとえば、ある場合、DbContext DbSetを利用して他のモデルクラスのメソッドに移行する必要がありますに実際のアプリケーションを移行する場合<Album>、アルバムのクラスを移行する必要がありますもちろん)。</span><span class="sxs-lookup"><span data-stu-id="5b049-117">When migrating a real application, you will also need to migrate all of the custom properties and methods of your application's user and DbContext classes, as well as any other Model classes your application utilizes (for example, if your DbContext has a DbSet<Album>, you will of course need to migrate the Album class).</span></span>

<span data-ttu-id="5b049-118">これらのファイルの場所で、Startup.cs ファイルにできるを使用して更新することでコンパイルするステートメント。</span><span class="sxs-lookup"><span data-stu-id="5b049-118">With these files in place, the Startup.cs file can be made to compile by updating its using statements:</span></span>

```csharp
using Microsoft.Framework.ConfigurationModel;
using Microsoft.AspNetCore.Hosting;
using NewMvc6Project.Models;
using Microsoft.AspNetCore.Identity;
```

<span data-ttu-id="5b049-119">アプリケーションは認証と id サービスをサポートする準備ができました - これらの機能をユーザーに公開するだけで済みます。</span><span class="sxs-lookup"><span data-stu-id="5b049-119">Our application is now ready to support authentication and identity services - it just needs to have these features exposed to users.</span></span>

## <a name="migrate-registration-and-login-logic"></a><span data-ttu-id="5b049-120">登録とログインのロジックを移行します。</span><span class="sxs-lookup"><span data-stu-id="5b049-120">Migrate Registration and Login Logic</span></span>

<span data-ttu-id="5b049-121">Id サービスを使用するアプリケーションおよび Entity Framework および SQL Server を使用して構成データ アクセス用に構成されたおと、アプリケーションへの登録とログイン サポートを追加する準備ができました。</span><span class="sxs-lookup"><span data-stu-id="5b049-121">With identity services configured for the application and data access configured using Entity Framework and SQL Server, we are now ready to add support for registration and login to the application.</span></span> <span data-ttu-id="5b049-122">注意してください[移行プロセスの前半](mvc.md#migrate-layout-file)おをコメント アウト _Layout.cshtml で _LoginPartial への参照。</span><span class="sxs-lookup"><span data-stu-id="5b049-122">Recall that [earlier in the migration process](mvc.md#migrate-layout-file) we commented out a reference to _LoginPartial in _Layout.cshtml.</span></span> <span data-ttu-id="5b049-123">では、そのコードに返すコメントを解除し、必要なコント ローラーとログイン機能をサポートするためにビューに追加します。</span><span class="sxs-lookup"><span data-stu-id="5b049-123">Now it's time to return to that code, uncomment it, and add in the necessary controllers and views to support login functionality.</span></span>

<span data-ttu-id="5b049-124">_Layout.cshtml; の更新します。コメントを解除、@Html.Partial行。</span><span class="sxs-lookup"><span data-stu-id="5b049-124">Update _Layout.cshtml; uncomment the @Html.Partial line:</span></span>

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

<span data-ttu-id="5b049-125">ビュー/共有フォルダーに _LoginPartial と呼ばれる新しい MVC ビュー ページを追加します。</span><span class="sxs-lookup"><span data-stu-id="5b049-125">Now, add a new MVC View Page called _LoginPartial to the Views/Shared folder:</span></span>

<span data-ttu-id="5b049-126">次のコードで _LoginPartial.cshtml を更新 (すべての内容を置き換えます)。</span><span class="sxs-lookup"><span data-stu-id="5b049-126">Update _LoginPartial.cshtml with the following code (replace all of its contents):</span></span>

```cshtml
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

<span data-ttu-id="5b049-127">この時点では、ブラウザーでサイトを更新することができます。</span><span class="sxs-lookup"><span data-stu-id="5b049-127">At this point, you should be able to refresh the site in your browser.</span></span>

## <a name="summary"></a><span data-ttu-id="5b049-128">まとめ</span><span class="sxs-lookup"><span data-stu-id="5b049-128">Summary</span></span>

<span data-ttu-id="5b049-129">ASP.NET Core では、ASP.NET Identity の機能への変更について説明します。</span><span class="sxs-lookup"><span data-stu-id="5b049-129">ASP.NET Core introduces changes to the ASP.NET Identity features.</span></span> <span data-ttu-id="5b049-130">この記事では、ASP.NET Core に ASP.NET Id の認証とユーザー管理機能を移行する方法を説明しました。</span><span class="sxs-lookup"><span data-stu-id="5b049-130">In this article, you have seen how to migrate the authentication and user management features of an ASP.NET Identity to ASP.NET Core.</span></span>
