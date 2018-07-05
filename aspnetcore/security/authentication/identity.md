---
title: ASP.NET core Identity の概要
author: rick-anderson
description: ASP.NET Core アプリでは、Id を使用します。 含まれている (RequireDigit、RequiredLength、RequiredUniqueChars など) にパスワードの要件を設定します。
ms.author: riande
ms.date: 01/24/2018
uid: security/authentication/identity
ms.openlocfilehash: c231a7619a4433ce004342ce68564e4c3892e702
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829303"
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="b18d8-104">ASP.NET core Identity の概要</span><span class="sxs-lookup"><span data-stu-id="b18d8-104">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="b18d8-105">によって[Pranav Rastogi](https://github.com/rustd)、 [Rick Anderson](https://twitter.com/RickAndMSFT)、 [Tom Dykstra](https://github.com/tdykstra)、Jon Galloway、 [Erik Reitan](https://github.com/Erikre)、および[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="b18d8-105">By [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Jon Galloway, [Erik Reitan](https://github.com/Erikre), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="b18d8-106">ASP.NET Core Identity は、メンバーシップ システム、アプリケーションにログイン機能を追加することができます。</span><span class="sxs-lookup"><span data-stu-id="b18d8-106">ASP.NET Core Identity is a membership system which allows you to add login functionality to your application.</span></span> <span data-ttu-id="b18d8-107">ユーザーはユーザー名を持つアカウントとログインを作成でき、パスワード、またはこれらには、Facebook、Google、Microsoft アカウント、Twitter またはなど、外部ログイン プロバイダーを使用できます。</span><span class="sxs-lookup"><span data-stu-id="b18d8-107">Users can create an account and login with a user name and password or they can use an external login provider such as Facebook, Google, Microsoft Account, Twitter or others.</span></span>

<span data-ttu-id="b18d8-108">ユーザー名、パスワード、およびプロファイル データを格納する SQL Server データベースを使用する ASP.NET Core Identity を構成することができます。</span><span class="sxs-lookup"><span data-stu-id="b18d8-108">You can configure ASP.NET Core Identity to use a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="b18d8-109">または、独自の永続的なストア、Azure Table Storage などを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="b18d8-109">Alternatively, you can use your own persistent store, for example, an Azure Table Storage.</span></span> <span data-ttu-id="b18d8-110">このドキュメントでは、Visual Studio と CLI を使用するための手順を説明します。</span><span class="sxs-lookup"><span data-stu-id="b18d8-110">This document contains instructions for Visual Studio and for using the CLI.</span></span>

[<span data-ttu-id="b18d8-111">表示またはサンプル コードをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="b18d8-111">View or download the sample code.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) [<span data-ttu-id="b18d8-112">(ダウンロードする方法)</span><span class="sxs-lookup"><span data-stu-id="b18d8-112">(How to download)</span></span>](xref:tutorials/index#how-to-download-a-sample)

## <a name="overview-of-identity"></a><span data-ttu-id="b18d8-113">Id の概要</span><span class="sxs-lookup"><span data-stu-id="b18d8-113">Overview of Identity</span></span>

<span data-ttu-id="b18d8-114">このトピックでは、ASP.NET Core Identity を使用して、ログインを登録する機能を追加する方法について説明し、ユーザーをログアウトします。</span><span class="sxs-lookup"><span data-stu-id="b18d8-114">In this topic, you'll learn how to use ASP.NET Core Identity to add functionality to register, log in, and log out a user.</span></span> <span data-ttu-id="b18d8-115">ASP.NET Core Identity を使用してアプリの作成について詳細な手順については、この記事の最後に次の手順を参照してください。</span><span class="sxs-lookup"><span data-stu-id="b18d8-115">For more detailed instructions about creating apps using ASP.NET Core Identity, see the Next Steps section at the end of this article.</span></span>

1. <span data-ttu-id="b18d8-116">個々 のユーザー アカウントを使って、ASP.NET Core Web アプリケーション プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="b18d8-116">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b18d8-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b18d8-117">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="b18d8-118">Visual Studio で、次のように選択します。**ファイル** > **新規** > **プロジェクト**します。</span><span class="sxs-lookup"><span data-stu-id="b18d8-118">In Visual Studio, select **File** > **New** > **Project**.</span></span> <span data-ttu-id="b18d8-119">選択**ASP.NET Core Web アプリケーション** をクリック**OK**します。</span><span class="sxs-lookup"><span data-stu-id="b18d8-119">Select **ASP.NET Core Web Application** and click **OK**.</span></span>

   ![[新しいプロジェクト] ダイアログ](identity/_static/01-new-project.png)

   <span data-ttu-id="b18d8-121">ASP.NET Core を選択します。 **Web アプリケーション (モデル-ビュー-コント ローラー)** for ASP.NET Core 2.x、し、選択**認証の変更**します。</span><span class="sxs-lookup"><span data-stu-id="b18d8-121">Select an ASP.NET Core **Web Application (Model-View-Controller)** for ASP.NET Core 2.x, then select **Change Authentication**.</span></span>

   ![[新しいプロジェクト] ダイアログ](identity/_static/02-new-project.png)

   <span data-ttu-id="b18d8-123">ダイアログがサービス認証の選択肢です。</span><span class="sxs-lookup"><span data-stu-id="b18d8-123">A dialog appears offering authentication choices.</span></span> <span data-ttu-id="b18d8-124">選択**個々 のユーザー アカウント** をクリック**OK**前のダイアログに戻ります。</span><span class="sxs-lookup"><span data-stu-id="b18d8-124">Select **Individual User Accounts** and click **OK** to return to the previous dialog.</span></span>

   ![[新しいプロジェクト] ダイアログ](identity/_static/03-new-project-auth.png)

   <span data-ttu-id="b18d8-126">選択**個々 のユーザー アカウント**モデル、ビューモデル、ビュー、コント ローラー、およびプロジェクト テンプレートの一部として、認証に必要なその他の資産を作成する Visual Studio に指示します。</span><span class="sxs-lookup"><span data-stu-id="b18d8-126">Selecting **Individual User Accounts** directs Visual Studio to create Models, ViewModels, Views, Controllers, and other assets required for authentication as part of the project template.</span></span>

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b18d8-127">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b18d8-127">.NET Core CLI</span></span>](#tab/netcore-cli)

   <span data-ttu-id="b18d8-128">.NET Core CLI を使用する場合を使用して新しいプロジェクトを作成`dotnet new mvc --auth Individual`です。</span><span class="sxs-lookup"><span data-stu-id="b18d8-128">If using the .NET Core CLI, create the new project using `dotnet new mvc --auth Individual`.</span></span> <span data-ttu-id="b18d8-129">このコマンドは、同じ Id テンプレート コードが Visual Studio によって作成、新しいプロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="b18d8-129">This command creates a new project with the same Identity template code Visual Studio creates.</span></span>

   <span data-ttu-id="b18d8-130">作成したプロジェクトが含まれています、`Microsoft.AspNetCore.Identity.EntityFrameworkCore`パッケージで、Id データと SQL Server を使用するスキーマが解決しない[Entity Framework Core](https://docs.microsoft.com/ef/)します。</span><span class="sxs-lookup"><span data-stu-id="b18d8-130">The created project contains the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` package, which persists the Identity data and schema to SQL Server using [Entity Framework Core](https://docs.microsoft.com/ef/).</span></span>

   ---

2. <span data-ttu-id="b18d8-131">Id サービスを構成しでミドルウェアを追加`Startup`します。</span><span class="sxs-lookup"><span data-stu-id="b18d8-131">Configure Identity services and add middleware in `Startup`.</span></span>

   <span data-ttu-id="b18d8-132">Identity サービスでアプリケーションに追加されます、`ConfigureServices`メソッドで、`Startup`クラス。</span><span class="sxs-lookup"><span data-stu-id="b18d8-132">The Identity services are added to the application in the `ConfigureServices` method in the `Startup` class:</span></span>

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b18d8-133">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b18d8-133">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   <span data-ttu-id="b18d8-134">これらのサービスを使用して、アプリケーションで利用できる[依存関係の注入](xref:fundamentals/dependency-injection)します。</span><span class="sxs-lookup"><span data-stu-id="b18d8-134">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="b18d8-135">呼び出すことによって、アプリケーションの id が有効に`UseAuthentication`で、`Configure`メソッド。</span><span class="sxs-lookup"><span data-stu-id="b18d8-135">Identity is enabled for the application by calling `UseAuthentication` in the `Configure` method.</span></span> <span data-ttu-id="b18d8-136">`UseAuthentication` 認証を追加します。[ミドルウェア](xref:fundamentals/middleware/index)要求パイプラインにします。</span><span class="sxs-lookup"><span data-stu-id="b18d8-136">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b18d8-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b18d8-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   <span data-ttu-id="b18d8-138">これらのサービスを使用して、アプリケーションで利用できる[依存関係の注入](xref:fundamentals/dependency-injection)します。</span><span class="sxs-lookup"><span data-stu-id="b18d8-138">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="b18d8-139">呼び出すことによって、アプリケーションの id が有効に`UseIdentity`で、`Configure`メソッド。</span><span class="sxs-lookup"><span data-stu-id="b18d8-139">Identity is enabled for the application by calling `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="b18d8-140">`UseIdentity` cookie ベースの認証を追加します。[ミドルウェア](xref:fundamentals/middleware/index)要求パイプラインにします。</span><span class="sxs-lookup"><span data-stu-id="b18d8-140">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

   ---

   <span data-ttu-id="b18d8-141">アプリケーション起動プロセスの詳細については、次を参照してください。[アプリケーションの起動](xref:fundamentals/startup)します。</span><span class="sxs-lookup"><span data-stu-id="b18d8-141">For more information about the application start up process, see [Application Startup](xref:fundamentals/startup).</span></span>

3. <span data-ttu-id="b18d8-142">ユーザーを作成します。</span><span class="sxs-lookup"><span data-stu-id="b18d8-142">Create a user.</span></span>

   <span data-ttu-id="b18d8-143">アプリケーションを起動し、をクリックして、**登録**リンク。</span><span class="sxs-lookup"><span data-stu-id="b18d8-143">Launch the application and then click on the **Register** link.</span></span>

   <span data-ttu-id="b18d8-144">初めてこの操作を実行している場合は、移行を実行するために必要する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b18d8-144">If this is the first time you're performing this action, you may be required to run migrations.</span></span> <span data-ttu-id="b18d8-145">アプリケーションでは、するように求められます**適用移行**します。</span><span class="sxs-lookup"><span data-stu-id="b18d8-145">The application prompts you to **Apply Migrations**.</span></span> <span data-ttu-id="b18d8-146">必要な場合は、ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="b18d8-146">Refresh the page if needed.</span></span>

   ![移行の Web ページを適用します。](identity/_static/apply-migrations.png)

   <span data-ttu-id="b18d8-148">または、永続的なデータベースを使用せず、アプリでメモリ内データベースを使用して ASP.NET Core Identity を使用してテストできます。</span><span class="sxs-lookup"><span data-stu-id="b18d8-148">Alternately, you can test using ASP.NET Core Identity with your app without a persistent database by using an in-memory database.</span></span> <span data-ttu-id="b18d8-149">メモリ内データベースを使用する追加、`Microsoft.EntityFrameworkCore.InMemory`をアプリにパッケージ化し、アプリの呼び出しを変更`AddDbContext`で`ConfigureServices`次のようにします。</span><span class="sxs-lookup"><span data-stu-id="b18d8-149">To use an in-memory database, add the `Microsoft.EntityFrameworkCore.InMemory` package to your app and modify your app's call to `AddDbContext` in `ConfigureServices` as follows:</span></span>

   ```csharp
   services.AddDbContext<ApplicationDbContext>(options =>
       options.UseInMemoryDatabase(Guid.NewGuid().ToString()));
   ```

   <span data-ttu-id="b18d8-150">ユーザーがクリックすると、**登録**、リンク、`Register`でアクションが呼び出された`AccountController`。</span><span class="sxs-lookup"><span data-stu-id="b18d8-150">When the user clicks the **Register** link, the `Register` action is invoked on `AccountController`.</span></span> <span data-ttu-id="b18d8-151">`Register`アクションを呼び出して、ユーザーを作成します`CreateAsync`上、`_userManager`オブジェクト (に提供される`AccountController`依存関係の挿入によって)。</span><span class="sxs-lookup"><span data-stu-id="b18d8-151">The `Register` action creates the user by calling `CreateAsync` on the `_userManager` object (provided to `AccountController` by dependency injection):</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

   <span data-ttu-id="b18d8-152">ユーザーが正常に作成された場合、ユーザーがログインへの呼び出しによって`_signInManager.SignInAsync`します。</span><span class="sxs-lookup"><span data-stu-id="b18d8-152">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

   <span data-ttu-id="b18d8-153">**注:** を参照してください[アカウントの確認](xref:security/authentication/accconfirm#prevent-login-at-registration)登録時の即時のログインを防止する手順についてはします。</span><span class="sxs-lookup"><span data-stu-id="b18d8-153">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

4. <span data-ttu-id="b18d8-154">ログイン。</span><span class="sxs-lookup"><span data-stu-id="b18d8-154">Log in.</span></span>

   <span data-ttu-id="b18d8-155">ユーザーがクリックしてサインインできる、**ログイン**サイトの上部にあるリンクまたは承認を必要とするサイトの一部にアクセスする場合、ログイン ページにナビゲート可能性があります。</span><span class="sxs-lookup"><span data-stu-id="b18d8-155">Users can sign in by clicking the **Log in** link at the top of the site, or they may be navigated to the Login page if they attempt to access a part of the site that requires authorization.</span></span> <span data-ttu-id="b18d8-156">ユーザーがログイン ページで、フォームを送信するときに、 `AccountController` `Login`アクションが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="b18d8-156">When the user submits the form on the Login page, the `AccountController` `Login` action is called.</span></span>

   <span data-ttu-id="b18d8-157">`Login`アクション呼び出し`PasswordSignInAsync`上、`_signInManager`オブジェクト (に提供される`AccountController`依存関係の挿入によって)。</span><span class="sxs-lookup"><span data-stu-id="b18d8-157">The `Login` action calls `PasswordSignInAsync` on the `_signInManager` object (provided to `AccountController` by dependency injection).</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

   <span data-ttu-id="b18d8-158">基本`Controller`クラスでは、`User`コント ローラー メソッドからアクセスできるプロパティです。</span><span class="sxs-lookup"><span data-stu-id="b18d8-158">The base `Controller` class exposes a `User` property that you can access from controller methods.</span></span> <span data-ttu-id="b18d8-159">たとえば、列挙することができます`User.Claims`承認決定を行うとします。</span><span class="sxs-lookup"><span data-stu-id="b18d8-159">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="b18d8-160">詳細については、次を参照してください。[承認](xref:security/authorization/index)します。</span><span class="sxs-lookup"><span data-stu-id="b18d8-160">For more information, see [Authorization](xref:security/authorization/index).</span></span>

5. <span data-ttu-id="b18d8-161">ログアウトします。</span><span class="sxs-lookup"><span data-stu-id="b18d8-161">Log out.</span></span>

   <span data-ttu-id="b18d8-162">クリックすると、**ログアウト**呼び出しのリンク、`LogOut`アクション。</span><span class="sxs-lookup"><span data-stu-id="b18d8-162">Clicking the **Log out** link calls the `LogOut` action.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   <span data-ttu-id="b18d8-163">上記のコードの呼び出しの上、`_signInManager.SignOutAsync`メソッド。</span><span class="sxs-lookup"><span data-stu-id="b18d8-163">The preceding code above calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="b18d8-164">`SignOutAsync`メソッドは、cookie に格納されているユーザーの要求をクリアします。</span><span class="sxs-lookup"><span data-stu-id="b18d8-164">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>

<a name="pw"></a>
6. <span data-ttu-id="b18d8-165">構成します。</span><span class="sxs-lookup"><span data-stu-id="b18d8-165">Configuration.</span></span>

   <span data-ttu-id="b18d8-166">Id は、アプリのスタートアップ クラスでオーバーライドすることがいくつかの既定の動作を持っています。</span><span class="sxs-lookup"><span data-stu-id="b18d8-166">Identity has some default behaviors that can be overridden in the app's startup class.</span></span> <span data-ttu-id="b18d8-167">`IdentityOptions` 既定の動作を使用する場合に構成する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="b18d8-167">`IdentityOptions` don't need to be configured when using the default behaviors.</span></span> <span data-ttu-id="b18d8-168">次のコードは、パスワードの強度のいくつかのオプションを設定します。</span><span class="sxs-lookup"><span data-stu-id="b18d8-168">The following code sets several password strength options:</span></span>

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b18d8-169">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b18d8-169">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b18d8-170">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b18d8-170">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=13-33)]

   ---

   <span data-ttu-id="b18d8-171">Id を構成する方法の詳細については、次を参照してください。[構成 Identity](xref:security/authentication/identity-configuration)します。</span><span class="sxs-lookup"><span data-stu-id="b18d8-171">For more information about how to configure Identity, see [Configure Identity](xref:security/authentication/identity-configuration).</span></span>

   <span data-ttu-id="b18d8-172">構成することもできます、主キーのデータ型を参照してください[Id の構成の主キーのデータ型](xref:security/authentication/identity-primary-key-configuration)します。</span><span class="sxs-lookup"><span data-stu-id="b18d8-172">You also can configure the data type of the primary key, see [Configure Identity primary keys data type](xref:security/authentication/identity-primary-key-configuration).</span></span>

7. <span data-ttu-id="b18d8-173">データベースを表示します。</span><span class="sxs-lookup"><span data-stu-id="b18d8-173">View the database.</span></span>

   <span data-ttu-id="b18d8-174">アプリは、SQL Server データベース (既定では、Windows と Visual Studio のユーザー) を使用している場合は、データベースに作成したアプリを表示できます。</span><span class="sxs-lookup"><span data-stu-id="b18d8-174">If your app is using a SQL Server database (the default on Windows and for Visual Studio users), you can view the database the app created.</span></span> <span data-ttu-id="b18d8-175">使用することができます**SQL Server Management Studio**します。</span><span class="sxs-lookup"><span data-stu-id="b18d8-175">You can use **SQL Server Management Studio**.</span></span> <span data-ttu-id="b18d8-176">または、Visual studio で、次のように選択します。**ビュー** > **SQL Server オブジェクト エクスプ ローラー**します。</span><span class="sxs-lookup"><span data-stu-id="b18d8-176">Alternatively, from Visual Studio, select **View** > **SQL Server Object Explorer**.</span></span> <span data-ttu-id="b18d8-177">接続する **(localdb) \MSSQLLocalDB**します。</span><span class="sxs-lookup"><span data-stu-id="b18d8-177">Connect to **(localdb)\MSSQLLocalDB**.</span></span> <span data-ttu-id="b18d8-178">一致する名前を持つデータベース`aspnet-<name of your project>-<guid>`が表示されます。</span><span class="sxs-lookup"><span data-stu-id="b18d8-178">The database with a name matching `aspnet-<name of your project>-<guid>` is displayed.</span></span>

   ![AspNetUsers データベース テーブルのコンテキスト メニュー](identity/_static/04-db.png)

   <span data-ttu-id="b18d8-180">データベースを展開し、その**テーブル**、右クリックし、 **dbo します。AspNetUsers**テーブルを選択**ビュー データ**します。</span><span class="sxs-lookup"><span data-stu-id="b18d8-180">Expand the database and its **Tables**, then right-click the **dbo.AspNetUsers** table and select **View Data**.</span></span>

8. <span data-ttu-id="b18d8-181">Identity の動作を確認します</span><span class="sxs-lookup"><span data-stu-id="b18d8-181">Verify Identity works</span></span>

    <span data-ttu-id="b18d8-182">既定の*ASP.NET Core Web アプリケーション*プロジェクト テンプレートでは必要なく、アプリケーションで任意のアクションへのアクセスにログインします。</span><span class="sxs-lookup"><span data-stu-id="b18d8-182">The default *ASP.NET Core Web Application* project template allows users to access any action in the application without having to login.</span></span> <span data-ttu-id="b18d8-183">ASP.NET Identity が動作することを確認するには追加、`[Authorize]`属性を`About`のアクション、`Home`コント ローラー。</span><span class="sxs-lookup"><span data-stu-id="b18d8-183">To verify that ASP.NET Identity works, add an`[Authorize]` attribute to the `About` action of the `Home` Controller.</span></span>

    ```csharp
    [Authorize]
    public IActionResult About()
    {
        ViewData["Message"] = "Your application description page.";
        return View();
    }
    ```

    # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b18d8-184">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b18d8-184">Visual Studio</span></span>](#tab/visual-studio)

    <span data-ttu-id="b18d8-185">使用してプロジェクトを実行**Ctrl** + **f5 キーを押して**に移動し、**について**ページ。</span><span class="sxs-lookup"><span data-stu-id="b18d8-185">Run the project using **Ctrl** + **F5** and navigate to the **About** page.</span></span> <span data-ttu-id="b18d8-186">認証されたユーザーのみがアクセス可能性があります、**について**ASP.NET ログインまたは登録するには、ログイン ページにリダイレクトするため、ここで、ページします。</span><span class="sxs-lookup"><span data-stu-id="b18d8-186">Only authenticated users may access the **About** page now, so ASP.NET redirects you to the login page to login or register.</span></span>

    # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b18d8-187">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b18d8-187">.NET Core CLI</span></span>](#tab/netcore-cli)

    <span data-ttu-id="b18d8-188">コマンド ウィンドウを開き、プロジェクトのルートに移動しますディレクトリを含む、`.csproj`ファイル。</span><span class="sxs-lookup"><span data-stu-id="b18d8-188">Open a command window and navigate to the project's root directory containing the `.csproj` file.</span></span> <span data-ttu-id="b18d8-189">実行、 [「dotnet run」](/dotnet/core/tools/dotnet-run)アプリを実行するコマンド。</span><span class="sxs-lookup"><span data-stu-id="b18d8-189">Run the [dotnet run](/dotnet/core/tools/dotnet-run) command to run the app:</span></span>

    ```csharp
    dotnet run 
    ```

    <span data-ttu-id="b18d8-190">参照からの出力で指定された URL、 [「dotnet run」](/dotnet/core/tools/dotnet-run)コマンド。</span><span class="sxs-lookup"><span data-stu-id="b18d8-190">Browse the URL specified in the output from the [dotnet run](/dotnet/core/tools/dotnet-run) command.</span></span> <span data-ttu-id="b18d8-191">URL を指す必要があります`localhost`生成されたポート番号。</span><span class="sxs-lookup"><span data-stu-id="b18d8-191">The URL should point to `localhost` with a generated port number.</span></span> <span data-ttu-id="b18d8-192">移動し、**について**ページ。</span><span class="sxs-lookup"><span data-stu-id="b18d8-192">Navigate to the **About** page.</span></span> <span data-ttu-id="b18d8-193">認証されたユーザーのみがアクセス可能性があります、**について**ASP.NET ログインまたは登録するには、ログイン ページにリダイレクトするため、ここで、ページします。</span><span class="sxs-lookup"><span data-stu-id="b18d8-193">Only authenticated users may access the **About** page now, so ASP.NET redirects you to the login page to login or register.</span></span>

    ---

## <a name="identity-components"></a><span data-ttu-id="b18d8-194">Id コンポーネント</span><span class="sxs-lookup"><span data-stu-id="b18d8-194">Identity Components</span></span>

<span data-ttu-id="b18d8-195">Id システムの主な参照アセンブリが`Microsoft.AspNetCore.Identity`します。</span><span class="sxs-lookup"><span data-stu-id="b18d8-195">The primary reference assembly for the Identity system is `Microsoft.AspNetCore.Identity`.</span></span> <span data-ttu-id="b18d8-196">このパッケージは、ASP.NET Core Identity のインターフェイスのコア セットが含まれていてに含まれている`Microsoft.AspNetCore.Identity.EntityFrameworkCore`します。</span><span class="sxs-lookup"><span data-stu-id="b18d8-196">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

<span data-ttu-id="b18d8-197">ASP.NET Core アプリケーションで、Id システムを使用するには、これらの依存関係が必要です。</span><span class="sxs-lookup"><span data-stu-id="b18d8-197">These dependencies are needed to use the Identity system in ASP.NET Core applications:</span></span>

* <span data-ttu-id="b18d8-198">`Microsoft.AspNetCore.Identity.EntityFrameworkCore` -Entity Framework Core での Id を使用する必要な型が含まれています。</span><span class="sxs-lookup"><span data-stu-id="b18d8-198">`Microsoft.AspNetCore.Identity.EntityFrameworkCore` - Contains the required types to use Identity with Entity Framework Core.</span></span>

* <span data-ttu-id="b18d8-199">`Microsoft.EntityFrameworkCore.SqlServer` -Entity Framework Core は、SQL Server のようなリレーショナル データベース用の Microsoft の推奨されるデータ アクセス テクノロジです。</span><span class="sxs-lookup"><span data-stu-id="b18d8-199">`Microsoft.EntityFrameworkCore.SqlServer` - Entity Framework Core is Microsoft's recommended data access technology for relational databases like SQL Server.</span></span> <span data-ttu-id="b18d8-200">テストは、使用することができます`Microsoft.EntityFrameworkCore.InMemory`します。</span><span class="sxs-lookup"><span data-stu-id="b18d8-200">For testing, you can use `Microsoft.EntityFrameworkCore.InMemory`.</span></span>

* <span data-ttu-id="b18d8-201">`Microsoft.AspNetCore.Authentication.Cookies` -Cookie ベースの認証を使用するアプリができるミドルウェア。</span><span class="sxs-lookup"><span data-stu-id="b18d8-201">`Microsoft.AspNetCore.Authentication.Cookies` - Middleware that enables an app to use cookie-based authentication.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="b18d8-202">ASP.NET Core Identity に移行します。</span><span class="sxs-lookup"><span data-stu-id="b18d8-202">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="b18d8-203">追加情報と、既存の Id の移行のガイダンスについては、「」を参照をストア[移行の認証と Id](xref:migration/identity)します。</span><span class="sxs-lookup"><span data-stu-id="b18d8-203">For additional information and guidance on migrating your existing Identity store see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="b18d8-204">パスワードの強度を設定</span><span class="sxs-lookup"><span data-stu-id="b18d8-204">Setting password strength</span></span>

<span data-ttu-id="b18d8-205">参照してください[構成](#pw)のパスワードの最小要件を設定するサンプルです。</span><span class="sxs-lookup"><span data-stu-id="b18d8-205">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b18d8-206">次の手順</span><span class="sxs-lookup"><span data-stu-id="b18d8-206">Next Steps</span></span>

* [<span data-ttu-id="b18d8-207">認証と Id を移行します。</span><span class="sxs-lookup"><span data-stu-id="b18d8-207">Migrate Authentication and Identity</span></span>](xref:migration/identity)
* [<span data-ttu-id="b18d8-208">アカウントの確認とパスワードの回復</span><span class="sxs-lookup"><span data-stu-id="b18d8-208">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="b18d8-209">SMS での 2 要素認証</span><span class="sxs-lookup"><span data-stu-id="b18d8-209">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="b18d8-210">Facebook、Google、および外部プロバイダーの認証</span><span class="sxs-lookup"><span data-stu-id="b18d8-210">Facebook, Google, and external provider authentication</span></span>](xref:security/authentication/social/index)
