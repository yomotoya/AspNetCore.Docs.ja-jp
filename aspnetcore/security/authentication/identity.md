---
title: "ASP.NET Core での Id の概要"
author: rick-anderson
description: "ASP.NET Core アプリケーションの Id を使用します。"
keywords: "ASP.NET Core、Identity、承認、セキュリティ"
ms.author: riande
manager: wpickett
ms.date: 01/02/2018
ms.topic: article
ms.assetid: cf119f21-1a2b-49a2-b052-547ccb66ee83
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity
ms.openlocfilehash: fc8e076af92bd8f9a95e73abb66ce32cae8ab9cf
ms.sourcegitcommit: 2d23ea501e0213bbacf65298acf1c8bd17209540
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/09/2018
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="f79b1-104">ASP.NET Core での Id の概要</span><span class="sxs-lookup"><span data-stu-id="f79b1-104">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="f79b1-105">によって[Pranav Rastogi](https://github.com/rustd)、 [Rick Anderson](https://twitter.com/RickAndMSFT)、 [Tom Dykstra](https://github.com/tdykstra)、Jon Galloway [Erik Reitan](https://github.com/Erikre)、および[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="f79b1-105">By [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Jon Galloway, [Erik Reitan](https://github.com/Erikre), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="f79b1-106">ASP.NET Core Id は、アプリケーションにログイン機能を追加することができるメンバーシップ システムがします。</span><span class="sxs-lookup"><span data-stu-id="f79b1-106">ASP.NET Core Identity is a membership system which allows you to add login functionality to your application.</span></span> <span data-ttu-id="f79b1-107">ユーザーは、ユーザー名でアカウントやログインを作成できます、パスワード、またはこれらには、Facebook、Google、Microsoft アカウント、Twitter またはなど、外部ログイン プロバイダーを使用できます。</span><span class="sxs-lookup"><span data-stu-id="f79b1-107">Users can create an account and login with a user name and password or they can use an external login provider such as Facebook, Google, Microsoft Account, Twitter or others.</span></span>

<span data-ttu-id="f79b1-108">ユーザー名、パスワード、およびプロファイル データを格納する SQL Server データベースを使用する ASP.NET Core Id を構成することができます。</span><span class="sxs-lookup"><span data-stu-id="f79b1-108">You can configure ASP.NET Core Identity to use a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="f79b1-109">代わりに、独自の永続的なストア、たとえば、Azure テーブル ストレージを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="f79b1-109">Alternatively, you can use your own persistent store, for example, an Azure Table Storage.</span></span> <span data-ttu-id="f79b1-110">このドキュメントでは、Visual Studio と CLI を使用するための手順を説明します。</span><span class="sxs-lookup"><span data-stu-id="f79b1-110">This document contains instructions for Visual Studio and for using the CLI.</span></span>

[<span data-ttu-id="f79b1-111">表示または、サンプル コードをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="f79b1-111">View or download the sample code.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) [<span data-ttu-id="f79b1-112">(ダウンロードする方法)</span><span class="sxs-lookup"><span data-stu-id="f79b1-112">(How to download)</span></span>](https://docs.microsoft.com/en-us/aspnet/core/tutorials/index#how-to-download-a-sample)

## <a name="overview-of-identity"></a><span data-ttu-id="f79b1-113">Id の概要</span><span class="sxs-lookup"><span data-stu-id="f79b1-113">Overview of Identity</span></span>

<span data-ttu-id="f79b1-114">このトピックでを ASP.NET Core Id を使用して、ログインを登録する機能を追加する方法について説明し、ユーザーをログアウトします。</span><span class="sxs-lookup"><span data-stu-id="f79b1-114">In this topic, you'll learn how to use ASP.NET Core Identity to add functionality to register, log in, and log out a user.</span></span> <span data-ttu-id="f79b1-115">ASP.NET Core の Id を使用してアプリの作成に関する詳細な手順については、この記事の最後に次の手順を参照してください。</span><span class="sxs-lookup"><span data-stu-id="f79b1-115">For more detailed instructions about creating apps using ASP.NET Core Identity, see the Next Steps section at the end of this article.</span></span>

1.  <span data-ttu-id="f79b1-116">個々 のユーザー アカウントを使って ASP.NET Core Web アプリケーション プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="f79b1-116">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

    # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f79b1-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f79b1-117">Visual Studio</span></span>](#tab/visual-studio)

    <span data-ttu-id="f79b1-118">Visual Studio で、次のように選択します。**ファイル** > **新規** > **プロジェクト**です。</span><span class="sxs-lookup"><span data-stu-id="f79b1-118">In Visual Studio, select **File** > **New** > **Project**.</span></span> <span data-ttu-id="f79b1-119">選択**ASP.NET Core Web アプリケーション** をクリック**OK**です。</span><span class="sxs-lookup"><span data-stu-id="f79b1-119">Select **ASP.NET Core Web Application** and click **OK**.</span></span>

    ![[新しいプロジェクト] ダイアログ](identity/_static/01-new-project.png)

    <span data-ttu-id="f79b1-121">ASP.NET Core の選択**Web アプリケーション (モデル-ビュー-コント ローラー)** for ASP.NET 2.x、コアし、選択**認証の変更**です。</span><span class="sxs-lookup"><span data-stu-id="f79b1-121">Select an ASP.NET Core **Web Application (Model-View-Controller)** for ASP.NET Core 2.x, then select **Change Authentication**.</span></span>

    ![[新しいプロジェクト] ダイアログ](identity/_static/02-new-project.png)

    <span data-ttu-id="f79b1-123">オファー、ダイアログを表示認証オプション。</span><span class="sxs-lookup"><span data-stu-id="f79b1-123">A dialog appears offering authentication choices.</span></span> <span data-ttu-id="f79b1-124">選択**個々 のユーザー アカウント** をクリック**OK**前のダイアログに戻ります。</span><span class="sxs-lookup"><span data-stu-id="f79b1-124">Select **Individual User Accounts** and click **OK** to return to the previous dialog.</span></span>

    ![[新しいプロジェクト] ダイアログ](identity/_static/03-new-project-auth.png)

    <span data-ttu-id="f79b1-126">選択すると**個々 のユーザー アカウント**モデル、ViewModels、ビュー、コント ローラー、およびプロジェクト テンプレートの一部として、認証に必要なその他のアセットを作成する Visual Studio に指示します。</span><span class="sxs-lookup"><span data-stu-id="f79b1-126">Selecting **Individual User Accounts** directs Visual Studio to create Models, ViewModels, Views, Controllers, and other assets required for authentication as part of the project template.</span></span>

    # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f79b1-127">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="f79b1-127">.NET Core CLI</span></span>](#tab/netcore-cli)

    <span data-ttu-id="f79b1-128">.NET Core CLI を使用する場合を使用して新しいプロジェクトを作成``dotnet new mvc --auth Individual``です。</span><span class="sxs-lookup"><span data-stu-id="f79b1-128">If using the .NET Core CLI, create the new project using ``dotnet new mvc --auth Individual``.</span></span> <span data-ttu-id="f79b1-129">このコマンドは、Visual Studio によって作成された同じ Identity テンプレート コードを新しいプロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="f79b1-129">This command creates a new project with the same Identity template code Visual Studio creates.</span></span>

    <span data-ttu-id="f79b1-130">作成されたプロジェクトが含まれています、 `Microsoft.AspNetCore.Identity.EntityFrameworkCore` Id データおよび SQL Server を使用するスキーマが引き続き発生する、パッケージ[Entity Framework Core](https://docs.microsoft.com/ef/)です。</span><span class="sxs-lookup"><span data-stu-id="f79b1-130">The created project contains the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` package, which persists the Identity data and schema to SQL Server using [Entity Framework Core](https://docs.microsoft.com/ef/).</span></span>

    ---

2.  <span data-ttu-id="f79b1-131">Id サービスを構成しのミドルウェアを追加`Startup`です。</span><span class="sxs-lookup"><span data-stu-id="f79b1-131">Configure Identity services and add middleware in `Startup`.</span></span>

    <span data-ttu-id="f79b1-132">Id サービスが、アプリケーションに追加されます、`ConfigureServices`メソッドで、`Startup`クラス。</span><span class="sxs-lookup"><span data-stu-id="f79b1-132">The Identity services are added to the application in the `ConfigureServices` method in the `Startup` class:</span></span>

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f79b1-133">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f79b1-133">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]
    
    <span data-ttu-id="f79b1-134">これらのサービスを使用して、アプリケーションで利用できる[依存性の注入](xref:fundamentals/dependency-injection)です。</span><span class="sxs-lookup"><span data-stu-id="f79b1-134">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
    
    <span data-ttu-id="f79b1-135">呼び出して、アプリケーションの識別情報が有効になっている`UseAuthentication`で、`Configure`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="f79b1-135">Identity is enabled for the application by calling `UseAuthentication` in the `Configure` method.</span></span> <span data-ttu-id="f79b1-136">`UseAuthentication`認証を追加[ミドルウェア](xref:fundamentals/middleware)要求パイプラインにします。</span><span class="sxs-lookup"><span data-stu-id="f79b1-136">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware) to the request pipeline.</span></span>
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f79b1-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f79b1-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-34)]
    
    <span data-ttu-id="f79b1-138">これらのサービスを使用して、アプリケーションで利用できる[依存性の注入](xref:fundamentals/dependency-injection)です。</span><span class="sxs-lookup"><span data-stu-id="f79b1-138">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
    
    <span data-ttu-id="f79b1-139">呼び出して、アプリケーションの識別情報が有効になっている`UseIdentity`で、`Configure`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="f79b1-139">Identity is enabled for the application by calling `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="f79b1-140">`UseIdentity`cookie ベースの認証を追加[ミドルウェア](xref:fundamentals/middleware)要求パイプラインにします。</span><span class="sxs-lookup"><span data-stu-id="f79b1-140">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware) to the request pipeline.</span></span>
        
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]
    
    ---
     
    <span data-ttu-id="f79b1-141">アプリケーション起動プロセスの詳細については、次を参照してください。[アプリケーションの起動](xref:fundamentals/startup)です。</span><span class="sxs-lookup"><span data-stu-id="f79b1-141">For more information about the application start up process, see [Application Startup](xref:fundamentals/startup).</span></span>

3.  <span data-ttu-id="f79b1-142">ユーザーを作成します。</span><span class="sxs-lookup"><span data-stu-id="f79b1-142">Create a user.</span></span>

    <span data-ttu-id="f79b1-143">アプリケーションを起動し、をクリックして、**登録**リンクします。</span><span class="sxs-lookup"><span data-stu-id="f79b1-143">Launch the application and then click on the **Register** link.</span></span>

    <span data-ttu-id="f79b1-144">初めてこの操作を実行している場合は、移行を実行する必要はあります。</span><span class="sxs-lookup"><span data-stu-id="f79b1-144">If this is the first time you're performing this action, you may be required to run migrations.</span></span> <span data-ttu-id="f79b1-145">アプリケーションでは、するように求められます**適用移行**です。</span><span class="sxs-lookup"><span data-stu-id="f79b1-145">The application prompts you to **Apply Migrations**.</span></span> <span data-ttu-id="f79b1-146">必要な場合は、ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="f79b1-146">Refresh the page if needed.</span></span>
    
    ![移行の Web ページを適用します。](identity/_static/apply-migrations.png)
    
    <span data-ttu-id="f79b1-148">代わりに、メモリ内データベースを使用してアプリを永続的なデータベースを使用しない ASP.NET Core の Id を使用してテストできます。</span><span class="sxs-lookup"><span data-stu-id="f79b1-148">Alternately, you can test using ASP.NET Core Identity with your app without a persistent database by using an in-memory database.</span></span> <span data-ttu-id="f79b1-149">メモリ内のデータベースを使用するのには、追加、``Microsoft.EntityFrameworkCore.InMemory``アプリをパッケージ化し、アプリの呼び出しを変更``AddDbContext``で``ConfigureServices``次のようにします。</span><span class="sxs-lookup"><span data-stu-id="f79b1-149">To use an in-memory database, add the ``Microsoft.EntityFrameworkCore.InMemory`` package to your app and modify your app's call to ``AddDbContext`` in ``ConfigureServices`` as follows:</span></span>

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseInMemoryDatabase(Guid.NewGuid().ToString()));
    ```
    
    <span data-ttu-id="f79b1-150">ユーザーがクリックしたとき、**登録**、リンク、``Register``でアクションが呼び出される``AccountController``です。</span><span class="sxs-lookup"><span data-stu-id="f79b1-150">When the user clicks the **Register** link, the ``Register`` action is invoked on ``AccountController``.</span></span> <span data-ttu-id="f79b1-151">``Register``アクションが呼び出すことによって、ユーザーを作成`CreateAsync`上、`_userManager`オブジェクト (に提供される``AccountController``依存関係の挿入によって)。</span><span class="sxs-lookup"><span data-stu-id="f79b1-151">The ``Register`` action creates the user by calling `CreateAsync` on the  `_userManager` object (provided to ``AccountController`` by dependency injection):</span></span>
 
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

    <span data-ttu-id="f79b1-152">ユーザーが正常に作成されている場合、ユーザーがログインへの呼び出しによって``_signInManager.SignInAsync``です。</span><span class="sxs-lookup"><span data-stu-id="f79b1-152">If the user was created successfully, the user is logged in by the call to ``_signInManager.SignInAsync``.</span></span>

    <span data-ttu-id="f79b1-153">**注:**を参照してください[アカウント確認](xref:security/authentication/accconfirm#prevent-login-at-registration)手順については、登録時に即時にログインできません。</span><span class="sxs-lookup"><span data-stu-id="f79b1-153">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>
 
4.  <span data-ttu-id="f79b1-154">ログイン。</span><span class="sxs-lookup"><span data-stu-id="f79b1-154">Log in.</span></span>
 
    <span data-ttu-id="f79b1-155">をクリックしてユーザーがサインインできるよう、**ログイン**サイトの上部にあるリンクの承認を必要とするサイトの一部にアクセスする場合、ログイン ページに移動可能性がありますか。</span><span class="sxs-lookup"><span data-stu-id="f79b1-155">Users can sign in by clicking the **Log in** link at the top of the site, or they may be navigated to the Login page if they attempt to access a part of the site that requires authorization.</span></span> <span data-ttu-id="f79b1-156">ユーザーがログイン ページのフォームを送信するときに、 ``AccountController`` ``Login``アクションが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="f79b1-156">When the user submits the form on the Login page, the ``AccountController`` ``Login`` action is called.</span></span>

    <span data-ttu-id="f79b1-157">``Login``アクション呼び出し``PasswordSignInAsync``上、``_signInManager``オブジェクト (に提供される``AccountController``依存関係の挿入によって)。</span><span class="sxs-lookup"><span data-stu-id="f79b1-157">The ``Login`` action calls ``PasswordSignInAsync`` on the ``_signInManager`` object (provided to ``AccountController`` by dependency injection).</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]
 
    <span data-ttu-id="f79b1-158">基本``Controller``クラスが公開する``User``コント ローラーのメソッドからアクセスできるプロパティです。</span><span class="sxs-lookup"><span data-stu-id="f79b1-158">The base ``Controller`` class exposes a ``User`` property that you can access from controller methods.</span></span> <span data-ttu-id="f79b1-159">インスタンスを列挙できます`User.Claims`承認決定を行うとします。</span><span class="sxs-lookup"><span data-stu-id="f79b1-159">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="f79b1-160">詳細については、次を参照してください。[承認](xref:security/authorization/index)です。</span><span class="sxs-lookup"><span data-stu-id="f79b1-160">For more information, see [Authorization](xref:security/authorization/index).</span></span>
 
5.  <span data-ttu-id="f79b1-161">ログアウトします。</span><span class="sxs-lookup"><span data-stu-id="f79b1-161">Log out.</span></span>
 
    <span data-ttu-id="f79b1-162">クリックすると、**ログアウト**呼び出しのリンク、`LogOut`アクション。</span><span class="sxs-lookup"><span data-stu-id="f79b1-162">Clicking the **Log out** link calls the `LogOut` action.</span></span>
 
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]
 
    <span data-ttu-id="f79b1-163">上記のコードの呼び出しの上、`_signInManager.SignOutAsync`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="f79b1-163">The preceding code above calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="f79b1-164">`SignOutAsync`メソッドは、cookie に格納されているユーザーの信頼性情報をクリアします。</span><span class="sxs-lookup"><span data-stu-id="f79b1-164">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>
 
6.  <span data-ttu-id="f79b1-165">構成します。</span><span class="sxs-lookup"><span data-stu-id="f79b1-165">Configuration.</span></span>

    <span data-ttu-id="f79b1-166">Id は、アプリケーションのスタートアップ クラスでオーバーライドすることがいくつかの既定の動作を持っています。</span><span class="sxs-lookup"><span data-stu-id="f79b1-166">Identity has some default behaviors that you can override in your application's startup class.</span></span> <span data-ttu-id="f79b1-167">構成する必要はありません``IdentityOptions``の既定の動作を使用している場合。</span><span class="sxs-lookup"><span data-stu-id="f79b1-167">You do not need to configure ``IdentityOptions`` if you are using the default behaviors.</span></span>

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f79b1-168">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f79b1-168">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f79b1-169">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f79b1-169">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=13-34)]

    ---
    
    <span data-ttu-id="f79b1-170">Id を構成する方法の詳細については、次を参照してください。[構成 Identity](xref:security/authentication/identity-configuration)です。</span><span class="sxs-lookup"><span data-stu-id="f79b1-170">For more information about how to configure Identity, see [Configure Identity](xref:security/authentication/identity-configuration).</span></span>
    
    <span data-ttu-id="f79b1-171">構成することもできます、主キーのデータ型を参照してください[Id の構成の主キーのデータ型](xref:security/authentication/identity-primary-key-configuration)です。</span><span class="sxs-lookup"><span data-stu-id="f79b1-171">You also can configure the data type of the primary key, see [Configure Identity primary keys data type](xref:security/authentication/identity-primary-key-configuration).</span></span>
 
7.  <span data-ttu-id="f79b1-172">データベースを表示します。</span><span class="sxs-lookup"><span data-stu-id="f79b1-172">View the database.</span></span>

    <span data-ttu-id="f79b1-173">アプリは、SQL Server データベース (既定の windows と Visual Studio ユーザー用) を使用している場合は、データベース作成されたアプリを表示できます。</span><span class="sxs-lookup"><span data-stu-id="f79b1-173">If your app is using a SQL Server database (the default on Windows and for Visual Studio users), you can view the database the app created.</span></span> <span data-ttu-id="f79b1-174">使用することができます**SQL Server Management Studio**です。</span><span class="sxs-lookup"><span data-stu-id="f79b1-174">You can use **SQL Server Management Studio**.</span></span> <span data-ttu-id="f79b1-175">または、Visual Studio から、次のように選択します。**ビュー** > **SQL Server オブジェクト エクスプ ローラー**です。</span><span class="sxs-lookup"><span data-stu-id="f79b1-175">Alternatively, from Visual Studio, select **View** > **SQL Server Object Explorer**.</span></span> <span data-ttu-id="f79b1-176">接続**(localdb) \MSSQLLocalDB**です。</span><span class="sxs-lookup"><span data-stu-id="f79b1-176">Connect to **(localdb)\MSSQLLocalDB**.</span></span> <span data-ttu-id="f79b1-177">一致する名前を持つデータベース **aspnet - <*、プロジェクトの名前*>-<*日付文字列*> * * が表示されます。</span><span class="sxs-lookup"><span data-stu-id="f79b1-177">The database with a name matching **aspnet-<*name of your project*>-<*date string*>** is displayed.</span></span>

    ![AspNetUsers データベース テーブルのコンテキスト メニュー](identity/_static/04-db.png)
    
    <span data-ttu-id="f79b1-179">データベースを展開し、その**テーブル**を右クリックし、 **dbo します。AspNetUsers**テーブルを選択して**ビュー データ**です。</span><span class="sxs-lookup"><span data-stu-id="f79b1-179">Expand the database and its **Tables**, then right-click the **dbo.AspNetUsers** table and select **View Data**.</span></span>

8. <span data-ttu-id="f79b1-180">Identity 動作を確認します。</span><span class="sxs-lookup"><span data-stu-id="f79b1-180">Verify Identity works</span></span>

    <span data-ttu-id="f79b1-181">既定値*ASP.NET Core Web アプリケーション*プロジェクト テンプレートを使用してしなくても、アプリケーションでのアクションにアクセスするユーザーにログインします。</span><span class="sxs-lookup"><span data-stu-id="f79b1-181">The default *ASP.NET Core Web Application* project template allows users to access any action in the application without having to login.</span></span> <span data-ttu-id="f79b1-182">ASP.NET Identity が動作することを確認するには追加、`[Authorize]`属性を`About`のアクション、`Home`コント ローラー。</span><span class="sxs-lookup"><span data-stu-id="f79b1-182">To verify that ASP.NET Identity works, add an`[Authorize]` attribute to the `About` action of the `Home` Controller.</span></span>
 
    ```cs
    [Authorize]
    public IActionResult About()
    {
        ViewData["Message"] = "Your application description page.";
        return View();
    }
    ```
    
    # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f79b1-183">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f79b1-183">Visual Studio</span></span>](#tab/visual-studio)

    <span data-ttu-id="f79b1-184">使用してプロジェクトを実行**Ctrl** + **f5 キーを押して**に移動し、**に関する**ページ。</span><span class="sxs-lookup"><span data-stu-id="f79b1-184">Run the project using **Ctrl** + **F5** and navigate to the **About** page.</span></span> <span data-ttu-id="f79b1-185">認証されたユーザーのみがアクセス、**に関する** ページが表示、ASP.NET ログインまたは登録するには、ログイン ページにリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="f79b1-185">Only authenticated users may access the **About** page now, so ASP.NET redirects you to the login page to login or register.</span></span>

    # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f79b1-186">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="f79b1-186">.NET Core CLI</span></span>](#tab/netcore-cli)

    <span data-ttu-id="f79b1-187">コマンド ウィンドウを開き、プロジェクトのルートに移動ディレクトリを含む、`.csproj`ファイル。</span><span class="sxs-lookup"><span data-stu-id="f79b1-187">Open a command window and navigate to the project's root directory containing the `.csproj` file.</span></span> <span data-ttu-id="f79b1-188">実行、`dotnet run`アプリを実行するコマンド。</span><span class="sxs-lookup"><span data-stu-id="f79b1-188">Run the `dotnet run` command to run the app:</span></span>

    ```cs
    dotnet run 
    ```

    <span data-ttu-id="f79b1-189">出力で指定された URL を参照、`dotnet run`コマンド。</span><span class="sxs-lookup"><span data-stu-id="f79b1-189">Browse the URL specified in the output from the `dotnet run` command.</span></span> <span data-ttu-id="f79b1-190">URL を指す必要があります`localhost`生成されたポート番号。</span><span class="sxs-lookup"><span data-stu-id="f79b1-190">The URL should point to `localhost` with a generated port number.</span></span> <span data-ttu-id="f79b1-191">移動し、**に関する**ページ。</span><span class="sxs-lookup"><span data-stu-id="f79b1-191">Navigate to the **About** page.</span></span> <span data-ttu-id="f79b1-192">認証されたユーザーのみがアクセス、**に関する** ページが表示、ASP.NET ログインまたは登録するには、ログイン ページにリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="f79b1-192">Only authenticated users may access the **About** page now, so ASP.NET redirects you to the login page to login or register.</span></span>

    ---

## <a name="identity-components"></a><span data-ttu-id="f79b1-193">Identity コンポーネント</span><span class="sxs-lookup"><span data-stu-id="f79b1-193">Identity Components</span></span>

<span data-ttu-id="f79b1-194">Id システムのプライマリ参照アセンブリが`Microsoft.AspNetCore.Identity`です。</span><span class="sxs-lookup"><span data-stu-id="f79b1-194">The primary reference assembly for the Identity system is `Microsoft.AspNetCore.Identity`.</span></span> <span data-ttu-id="f79b1-195">ASP.NET Core Id のインターフェイスのコア セットを含む、このパッケージをによって含まれる`Microsoft.AspNetCore.Identity.EntityFrameworkCore`です。</span><span class="sxs-lookup"><span data-stu-id="f79b1-195">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

<span data-ttu-id="f79b1-196">ASP.NET Core アプリケーションで Id システムを使用するには、これらの依存関係が必要です。</span><span class="sxs-lookup"><span data-stu-id="f79b1-196">These dependencies are needed to use the Identity system in ASP.NET Core applications:</span></span>

* <span data-ttu-id="f79b1-197">`Microsoft.AspNetCore.Identity.EntityFrameworkCore`-エンティティ フレームワークのコアで Id を使用する、必要な型が含まれています。</span><span class="sxs-lookup"><span data-stu-id="f79b1-197">`Microsoft.AspNetCore.Identity.EntityFrameworkCore` - Contains the required types to use Identity with Entity Framework Core.</span></span>

* <span data-ttu-id="f79b1-198">`Microsoft.EntityFrameworkCore.SqlServer`Entity Framework Core は、SQL Server などのリレーショナル データベースの Microsoft の推奨されるデータ アクセス テクノロジです。</span><span class="sxs-lookup"><span data-stu-id="f79b1-198">`Microsoft.EntityFrameworkCore.SqlServer` - Entity Framework Core is Microsoft's recommended data access technology for relational databases like SQL Server.</span></span> <span data-ttu-id="f79b1-199">使用することができます、テストは`Microsoft.EntityFrameworkCore.InMemory`します。</span><span class="sxs-lookup"><span data-stu-id="f79b1-199">For testing, you can use `Microsoft.EntityFrameworkCore.InMemory`.</span></span>

* <span data-ttu-id="f79b1-200">`Microsoft.AspNetCore.Authentication.Cookies`-を cookie ベースの認証を使用するアプリを有効にするミドルウェア。</span><span class="sxs-lookup"><span data-stu-id="f79b1-200">`Microsoft.AspNetCore.Authentication.Cookies` - Middleware that enables an app to use cookie-based authentication.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="f79b1-201">ASP.NET Core の Id への移行</span><span class="sxs-lookup"><span data-stu-id="f79b1-201">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="f79b1-202">ストアを参照の追加情報と、既存の Id の移行に関するガイダンスの[移行認証と Id](xref:migration/identity)です。</span><span class="sxs-lookup"><span data-stu-id="f79b1-202">For additional information and guidance on migrating your existing Identity store see [Migrating Authentication and Identity](xref:migration/identity).</span></span>

## <a name="next-steps"></a><span data-ttu-id="f79b1-203">次の手順</span><span class="sxs-lookup"><span data-stu-id="f79b1-203">Next Steps</span></span>

* [<span data-ttu-id="f79b1-204">認証と ID の移行</span><span class="sxs-lookup"><span data-stu-id="f79b1-204">Migrating Authentication and Identity</span></span>](xref:migration/identity)
* [<span data-ttu-id="f79b1-205">アカウントの確認とパスワードの回復</span><span class="sxs-lookup"><span data-stu-id="f79b1-205">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="f79b1-206">SMS での 2 要素認証</span><span class="sxs-lookup"><span data-stu-id="f79b1-206">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="f79b1-207">Facebook、Google、および他の外部プロバイダーを使用する認証の有効化</span><span class="sxs-lookup"><span data-stu-id="f79b1-207">Enabling authentication using Facebook, Google and other external providers</span></span>](xref:security/authentication/social/index)
