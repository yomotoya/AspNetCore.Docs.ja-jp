---
title: "ASP.NET Core での Id の概要"
author: rick-anderson
description: "ASP.NET Core アプリケーションの Id を使用してください。"
keywords: "ASP.NET Core、Identity、承認、セキュリティ"
ms.author: riande
manager: wpickett
ms.date: 7/7/2017
ms.topic: article
ms.assetid: cf119f21-1a2b-49a2-b052-547ccb66ee83
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity
ms.openlocfilehash: 72802830660ddcf479e540de7cfc33a07c49dc23
ms.sourcegitcommit: b02db6da115e55140da91b67355aaf56aae1703f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/11/2017
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="711b7-104">ASP.NET Core での Id の概要</span><span class="sxs-lookup"><span data-stu-id="711b7-104">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="711b7-105">によって[Pranav Rastogi](https://github.com/rustd)、 [Rick Anderson](https://twitter.com/RickAndMSFT)、 [Tom Dykstra](https://github.com/tdykstra)、Jon Galloway [Erik Reitan](https://github.com/Erikre)、および[Steve Smith](http://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="711b7-105">By [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Jon Galloway, [Erik Reitan](https://github.com/Erikre), and [Steve Smith](http://ardalis.com)</span></span>

<span data-ttu-id="711b7-106">ASP.NET Core Id は、アプリケーションにログイン機能を追加することができるメンバーシップ システムがします。</span><span class="sxs-lookup"><span data-stu-id="711b7-106">ASP.NET Core Identity is a membership system which allows you to add login functionality to your application.</span></span> <span data-ttu-id="711b7-107">ユーザーは、ユーザー名でアカウントやログインを作成できます、パスワード、またはこれらには、Facebook、Google、Microsoft アカウント、Twitter またはなど、外部ログイン プロバイダーを使用できます。</span><span class="sxs-lookup"><span data-stu-id="711b7-107">Users can create an account and login with a user name and password or they can use an external login provider such as Facebook, Google, Microsoft Account, Twitter or others.</span></span>

<span data-ttu-id="711b7-108">ユーザー名、パスワード、およびプロファイル データを格納する SQL Server データベースを使用する ASP.NET Core Id を構成することができます。</span><span class="sxs-lookup"><span data-stu-id="711b7-108">You can configure ASP.NET Core Identity to use a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="711b7-109">代わりに、独自の永続的なストア、Azure テーブル ストレージの例を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="711b7-109">Alternatively, you can use your own persistent store, for example Azure Table Storage.</span></span> <span data-ttu-id="711b7-110">このドキュメントでは、Visual Studio と CLI を使用するための手順を説明します。</span><span class="sxs-lookup"><span data-stu-id="711b7-110">This document contains instructions for Visual Studio and for using the CLI.</span></span>

## <a name="overview-of-identity"></a><span data-ttu-id="711b7-111">Id の概要</span><span class="sxs-lookup"><span data-stu-id="711b7-111">Overview of Identity</span></span>

<span data-ttu-id="711b7-112">このトピックでを ASP.NET Core Id を使用して、ログインを登録する機能を追加する方法について説明し、ユーザーをログアウトします。</span><span class="sxs-lookup"><span data-stu-id="711b7-112">In this topic, you'll learn how to use ASP.NET Core Identity to add functionality to register, log in, and log out a user.</span></span> <span data-ttu-id="711b7-113">ASP.NET Core の Id を使用してアプリの作成に関する詳細な手順については、この記事の最後に次の手順を参照してください。</span><span class="sxs-lookup"><span data-stu-id="711b7-113">For more detailed instructions about creating apps using ASP.NET Core Identity, see the Next Steps section at the end of this article.</span></span>

1.  <span data-ttu-id="711b7-114">個々 のユーザー アカウントを使って ASP.NET Core Web アプリケーション プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="711b7-114">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

    # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="711b7-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="711b7-115">Visual Studio</span></span>](#tab/visual-studio)
    <span data-ttu-id="711b7-116">Visual Studio で、次のように選択します。**ファイル** -> **新規** -> **プロジェクト**です。</span><span class="sxs-lookup"><span data-stu-id="711b7-116">In Visual Studio, select **File** -> **New** -> **Project**.</span></span> <span data-ttu-id="711b7-117">選択、 **ASP.NET Web アプリケーション**から、**新しいプロジェクト** ダイアログ ボックス。</span><span class="sxs-lookup"><span data-stu-id="711b7-117">Select the **ASP.NET Web Application** from the **New Project** dialog box.</span></span> <span data-ttu-id="711b7-118">ASP.NET Core を選択すると**Web アプリケーション**で**個々 のユーザー アカウント**の認証方法として。</span><span class="sxs-lookup"><span data-stu-id="711b7-118">Selecting an ASP.NET Core **Web Application** with **Individual User Accounts** as the authentication method.</span></span>

    <span data-ttu-id="711b7-119">注: 選択する必要があります**個々 のユーザー アカウント**です。</span><span class="sxs-lookup"><span data-stu-id="711b7-119">Note: You must select **Individual User Accounts**.</span></span>
 
    ![[新しいプロジェクト] ダイアログ](identity/_static/01-mvc.png)
    
    # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="711b7-121">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="711b7-121">.NET Core CLI</span></span>](#tab/netcore-cli)
    <span data-ttu-id="711b7-122">.NET Core CLI を使用する場合を使用して新しいプロジェクトを作成``dotnet new mvc --auth Individual``です。</span><span class="sxs-lookup"><span data-stu-id="711b7-122">If using the .NET Core CLI, create the new project using ``dotnet new mvc --auth Individual``.</span></span> <span data-ttu-id="711b7-123">これにより、Visual Studio によって作成された同じ Identity テンプレート コードで新しいプロジェクトが作成されます。</span><span class="sxs-lookup"><span data-stu-id="711b7-123">This will create a new project with the same Identity template code Visual Studio creates.</span></span>
 
    <span data-ttu-id="711b7-124">作成されたプロジェクトが含まれています、`Microsoft.AspNetCore.Identity.EntityFrameworkCore`パッケージ Id データおよび SQL Server を使用するスキーマに保持されますが[Entity Framework Core](https://docs.efproject.net)です。</span><span class="sxs-lookup"><span data-stu-id="711b7-124">The created project contains the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` package, which will persist the Identity data and schema to SQL Server using [Entity Framework Core](https://docs.efproject.net).</span></span>
    
    ---
 
2.  <span data-ttu-id="711b7-125">Id サービスを構成しのミドルウェアを追加`Startup`です。</span><span class="sxs-lookup"><span data-stu-id="711b7-125">Configure Identity services and add middleware in `Startup`.</span></span>

    <span data-ttu-id="711b7-126">Id サービスが、アプリケーションに追加されます、`ConfigureServices`メソッドで、`Startup`クラス。</span><span class="sxs-lookup"><span data-stu-id="711b7-126">The Identity services are added to the application in the `ConfigureServices` method in the `Startup` class:</span></span>

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="711b7-127">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="711b7-127">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)
    
    <span data-ttu-id="711b7-128">[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]</span><span class="sxs-lookup"><span data-stu-id="711b7-128">[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]</span></span>
    
    <span data-ttu-id="711b7-129">これらのサービスを使用して、アプリケーションで利用できる[依存性の注入](xref:fundamentals/dependency-injection)です。</span><span class="sxs-lookup"><span data-stu-id="711b7-129">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
    
    <span data-ttu-id="711b7-130">呼び出して、アプリケーションの識別情報が有効になっている`UseAuthentication`で、`Configure`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="711b7-130">Identity is enabled for the application by calling `UseAuthentication` in the `Configure` method.</span></span> <span data-ttu-id="711b7-131">`UseAuthentication`認証を追加[ミドルウェア](xref:fundamentals/middleware)要求パイプラインにします。</span><span class="sxs-lookup"><span data-stu-id="711b7-131">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware) to the request pipeline.</span></span>
    
    <span data-ttu-id="711b7-132">[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]</span><span class="sxs-lookup"><span data-stu-id="711b7-132">[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]</span></span>
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="711b7-133">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="711b7-133">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
    
    <span data-ttu-id="711b7-134">[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-34)]</span><span class="sxs-lookup"><span data-stu-id="711b7-134">[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-34)]</span></span>
    
    <span data-ttu-id="711b7-135">これらのサービスを使用して、アプリケーションで利用できる[依存性の注入](xref:fundamentals/dependency-injection)です。</span><span class="sxs-lookup"><span data-stu-id="711b7-135">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
    
    <span data-ttu-id="711b7-136">呼び出して、アプリケーションの識別情報が有効になっている`UseIdentity`で、`Configure`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="711b7-136">Identity is enabled for the application by calling `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="711b7-137">`UseIdentity`cookie ベースの認証を追加[ミドルウェア](xref:fundamentals/middleware)要求パイプラインにします。</span><span class="sxs-lookup"><span data-stu-id="711b7-137">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware) to the request pipeline.</span></span>
        
    <span data-ttu-id="711b7-138">[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]</span><span class="sxs-lookup"><span data-stu-id="711b7-138">[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]</span></span>
    
    ---
     
    <span data-ttu-id="711b7-139">アプリケーション起動プロセスの詳細については、次を参照してください。[アプリケーションの起動](xref:fundamentals/startup)です。</span><span class="sxs-lookup"><span data-stu-id="711b7-139">For more information about the application start up process, see [Application Startup](xref:fundamentals/startup).</span></span>

3.  <span data-ttu-id="711b7-140">ユーザーを作成します。</span><span class="sxs-lookup"><span data-stu-id="711b7-140">Create a user.</span></span>

    <span data-ttu-id="711b7-141">アプリケーションを起動し、をクリックして、**登録**リンクします。</span><span class="sxs-lookup"><span data-stu-id="711b7-141">Launch the application and then click on the **Register** link.</span></span>

    <span data-ttu-id="711b7-142">初めてこの操作を実行している場合は、移行を実行する必要はあります。</span><span class="sxs-lookup"><span data-stu-id="711b7-142">If this is the first time you're performing this action, you may be required to run migrations.</span></span> <span data-ttu-id="711b7-143">アプリケーションでは、するように求められます**適用移行**:</span><span class="sxs-lookup"><span data-stu-id="711b7-143">The application prompts you to **Apply Migrations**:</span></span>
    
    ![移行の Web ページを適用します。](identity/_static/apply-migrations.png)
    
    <span data-ttu-id="711b7-145">代わりに、メモリ内データベースを使用してアプリを永続的なデータベースを使用しない ASP.NET Core の Id を使用してテストできます。</span><span class="sxs-lookup"><span data-stu-id="711b7-145">Alternately, you can test using ASP.NET Core Identity with your app without a persistent database by using an in-memory database.</span></span> <span data-ttu-id="711b7-146">メモリ内のデータベースを使用するのには、追加、``Microsoft.EntityFrameworkCore.InMemory``アプリをパッケージ化し、アプリの呼び出しを変更``AddDbContext``で``ConfigureServices``次のようにします。</span><span class="sxs-lookup"><span data-stu-id="711b7-146">To use an in-memory database, add the ``Microsoft.EntityFrameworkCore.InMemory`` package to your app and modify your app's call to ``AddDbContext`` in ``ConfigureServices`` as follows:</span></span>

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseInMemoryDatabase(Guid.NewGuid().ToString()));
    ```
    
    <span data-ttu-id="711b7-147">ユーザーがクリックしたとき、**登録**、リンク、``Register``でアクションが呼び出される``AccountController``です。</span><span class="sxs-lookup"><span data-stu-id="711b7-147">When the user clicks the **Register** link, the ``Register`` action is invoked on ``AccountController``.</span></span> <span data-ttu-id="711b7-148">``Register``アクションが呼び出すことによって、ユーザーを作成`CreateAsync`上、`_userManager`オブジェクト (に提供される``AccountController``依存関係の挿入によって)。</span><span class="sxs-lookup"><span data-stu-id="711b7-148">The ``Register`` action creates the user by calling `CreateAsync` on the  `_userManager` object (provided to ``AccountController`` by dependency injection):</span></span>
 
    <span data-ttu-id="711b7-149">[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]</span><span class="sxs-lookup"><span data-stu-id="711b7-149">[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]</span></span>

    <span data-ttu-id="711b7-150">ユーザーが正常に作成されている場合、ユーザーがログインへの呼び出しによって``_signInManager.SignInAsync``です。</span><span class="sxs-lookup"><span data-stu-id="711b7-150">If the user was created successfully, the user is logged in by the call to ``_signInManager.SignInAsync``.</span></span>

    <span data-ttu-id="711b7-151">**注:**を参照してください[アカウント確認](xref:security/authentication/accconfirm#prevent-login-at-registration)手順については、登録時に即時にログインできません。</span><span class="sxs-lookup"><span data-stu-id="711b7-151">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>
 
4.  <span data-ttu-id="711b7-152">ログイン。</span><span class="sxs-lookup"><span data-stu-id="711b7-152">Log in.</span></span>
 
    <span data-ttu-id="711b7-153">をクリックしてユーザーがサインインできるよう、**ログイン**サイトの上部にあるリンクの承認を必要とするサイトの一部にアクセスする場合、ログイン ページに移動可能性がありますか。</span><span class="sxs-lookup"><span data-stu-id="711b7-153">Users can sign in by clicking the **Log in** link at the top of the site, or they may be navigated to the Login page if they attempt to access a part of the site that requires authorization.</span></span> <span data-ttu-id="711b7-154">ユーザーがログイン ページのフォームを送信するときに、 ``AccountController`` ``Login``アクションが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="711b7-154">When the user submits the form on the Login page, the ``AccountController`` ``Login`` action is called.</span></span>

    <span data-ttu-id="711b7-155">[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]</span><span class="sxs-lookup"><span data-stu-id="711b7-155">[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]</span></span>
 
    <span data-ttu-id="711b7-156">``Login``アクション呼び出し``PasswordSignInAsync``上、``_signInManager``オブジェクト (に提供される``AccountController``依存関係の挿入によって)。</span><span class="sxs-lookup"><span data-stu-id="711b7-156">The ``Login`` action calls ``PasswordSignInAsync`` on the ``_signInManager`` object (provided to ``AccountController`` by dependency injection).</span></span>
 
    <span data-ttu-id="711b7-157">基本``Controller``クラスが公開する``User``コント ローラーのメソッドからアクセスできるプロパティです。</span><span class="sxs-lookup"><span data-stu-id="711b7-157">The base ``Controller`` class exposes a ``User`` property that you can access from controller methods.</span></span> <span data-ttu-id="711b7-158">インスタンスを列挙できます`User.Claims`承認決定を行うとします。</span><span class="sxs-lookup"><span data-stu-id="711b7-158">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="711b7-159">詳細については、次を参照してください。[承認](xref:security/authorization/index)です。</span><span class="sxs-lookup"><span data-stu-id="711b7-159">For more information, see [Authorization](xref:security/authorization/index).</span></span>
 
5.  <span data-ttu-id="711b7-160">ログアウトします。</span><span class="sxs-lookup"><span data-stu-id="711b7-160">Log out.</span></span>
 
    <span data-ttu-id="711b7-161">クリックすると、**ログアウト**呼び出しのリンク、`LogOut`アクション。</span><span class="sxs-lookup"><span data-stu-id="711b7-161">Clicking the **Log out** link calls the `LogOut` action.</span></span>
 
    <span data-ttu-id="711b7-162">[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]</span><span class="sxs-lookup"><span data-stu-id="711b7-162">[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]</span></span>
 
    <span data-ttu-id="711b7-163">上記のコードの呼び出しの上、`_signInManager.SignOutAsync`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="711b7-163">The preceding code above calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="711b7-164">`SignOutAsync`メソッドは、cookie に格納されているユーザーの信頼性情報をクリアします。</span><span class="sxs-lookup"><span data-stu-id="711b7-164">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>
 
6.  <span data-ttu-id="711b7-165">構成します。</span><span class="sxs-lookup"><span data-stu-id="711b7-165">Configuration.</span></span>

    <span data-ttu-id="711b7-166">Id は、アプリケーションのスタートアップ クラスでオーバーライドすることがいくつかの既定の動作を持っています。</span><span class="sxs-lookup"><span data-stu-id="711b7-166">Identity has some default behaviors that you can override in your application's startup class.</span></span> <span data-ttu-id="711b7-167">構成する必要はありません``IdentityOptions``の既定の動作を使用している場合。</span><span class="sxs-lookup"><span data-stu-id="711b7-167">You do not need to configure ``IdentityOptions`` if you are using the default behaviors.</span></span>

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="711b7-168">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="711b7-168">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)
    
    <span data-ttu-id="711b7-169">[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]</span><span class="sxs-lookup"><span data-stu-id="711b7-169">[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]</span></span>
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="711b7-170">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="711b7-170">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
    
    <span data-ttu-id="711b7-171">[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=13-34)]</span><span class="sxs-lookup"><span data-stu-id="711b7-171">[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=13-34)]</span></span>

    ---
    
    <span data-ttu-id="711b7-172">Id を構成する方法の詳細については、次を参照してください。[構成 Identity](xref:security/authentication/identity-configuration)です。</span><span class="sxs-lookup"><span data-stu-id="711b7-172">For more information about how to configure Identity, see [Configure Identity](xref:security/authentication/identity-configuration).</span></span>
    
    <span data-ttu-id="711b7-173">構成することもできます、主キーのデータ型を参照してください[Id の構成の主キーのデータ型](xref:security/authentication/identity-primary-key-configuration)です。</span><span class="sxs-lookup"><span data-stu-id="711b7-173">You also can configure the data type of the primary key, see [Configure Identity primary keys data type](xref:security/authentication/identity-primary-key-configuration).</span></span>
 
7.  <span data-ttu-id="711b7-174">データベースを表示します。</span><span class="sxs-lookup"><span data-stu-id="711b7-174">View the database.</span></span>

    <span data-ttu-id="711b7-175">アプリは、SQL Server データベース (既定の windows と Visual Studio ユーザー用) を使用している場合は、データベース作成されたアプリを表示できます。</span><span class="sxs-lookup"><span data-stu-id="711b7-175">If your app is using a SQL Server database (the default on Windows and for Visual Studio users), you can view the database the app created.</span></span> <span data-ttu-id="711b7-176">使用することができます**SQL Server Management Studio**です。</span><span class="sxs-lookup"><span data-stu-id="711b7-176">You can use **SQL Server Management Studio**.</span></span> <span data-ttu-id="711b7-177">または、Visual Studio から、次のように選択します。**ビュー** -> **SQL Server オブジェクト エクスプ ローラー**です。</span><span class="sxs-lookup"><span data-stu-id="711b7-177">Alternatively, from Visual Studio, select **View** -> **SQL Server Object Explorer**.</span></span> <span data-ttu-id="711b7-178">接続**(localdb) \MSSQLLocalDB**です。</span><span class="sxs-lookup"><span data-stu-id="711b7-178">Connect to **(localdb)\MSSQLLocalDB**.</span></span> <span data-ttu-id="711b7-179">一致する名前を持つデータベース* *aspnet - <*、プロジェクトの名前*>-<*日付文字列*> * * が表示されます。</span><span class="sxs-lookup"><span data-stu-id="711b7-179">The database with a name matching **aspnet-<*name of your project*>-<*date string*>** is displayed.</span></span>

    ![AspNetUsers データベース テーブルのコンテキスト メニュー](identity/_static/04-db.png)
    
    <span data-ttu-id="711b7-181">データベースを展開し、その**テーブル**を右クリックし、 **dbo します。AspNetUsers**テーブルを選択して**ビュー データ**です。</span><span class="sxs-lookup"><span data-stu-id="711b7-181">Expand the database and its **Tables**, then right-click the **dbo.AspNetUsers** table and select **View Data**.</span></span>

## <a name="identity-components"></a><span data-ttu-id="711b7-182">Identity コンポーネント</span><span class="sxs-lookup"><span data-stu-id="711b7-182">Identity Components</span></span>

<span data-ttu-id="711b7-183">Id システムのプライマリ参照アセンブリが`Microsoft.AspNetCore.Identity`です。</span><span class="sxs-lookup"><span data-stu-id="711b7-183">The primary reference assembly for the Identity system is `Microsoft.AspNetCore.Identity`.</span></span> <span data-ttu-id="711b7-184">ASP.NET Core Id のインターフェイスのコア セットを含む、このパッケージをによって含まれる`Microsoft.AspNetCore.Identity.EntityFrameworkCore`です。</span><span class="sxs-lookup"><span data-stu-id="711b7-184">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

<span data-ttu-id="711b7-185">ASP.NET Core アプリケーションで Id システムを使用するには、これらの依存関係が必要です。</span><span class="sxs-lookup"><span data-stu-id="711b7-185">These dependencies are needed to use the Identity system in ASP.NET Core applications:</span></span>

* <span data-ttu-id="711b7-186">`Microsoft.AspNetCore.Identity.EntityFrameworkCore`-エンティティ フレームワークのコアで Id を使用する、必要な型が含まれています。</span><span class="sxs-lookup"><span data-stu-id="711b7-186">`Microsoft.AspNetCore.Identity.EntityFrameworkCore` - Contains the required types to use Identity with Entity Framework Core.</span></span>

* <span data-ttu-id="711b7-187">`Microsoft.EntityFrameworkCore.SqlServer`Entity Framework Core は、SQL Server などのリレーショナル データベースの Microsoft の推奨されるデータ アクセス テクノロジです。</span><span class="sxs-lookup"><span data-stu-id="711b7-187">`Microsoft.EntityFrameworkCore.SqlServer` - Entity Framework Core is Microsoft's recommended data access technology for relational databases like SQL Server.</span></span> <span data-ttu-id="711b7-188">使用することができます、テストは`Microsoft.EntityFrameworkCore.InMemory`します。</span><span class="sxs-lookup"><span data-stu-id="711b7-188">For testing, you can use `Microsoft.EntityFrameworkCore.InMemory`.</span></span>

* <span data-ttu-id="711b7-189">`Microsoft.AspNetCore.Authentication.Cookies`-を cookie ベースの認証を使用するアプリを有効にするミドルウェア。</span><span class="sxs-lookup"><span data-stu-id="711b7-189">`Microsoft.AspNetCore.Authentication.Cookies` - Middleware that enables an app to use cookie-based authentication.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="711b7-190">ASP.NET Core の Id への移行</span><span class="sxs-lookup"><span data-stu-id="711b7-190">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="711b7-191">ストアを参照の追加情報と、既存の Id の移行に関するガイダンスの[移行認証と Id](xref:migration/identity)です。</span><span class="sxs-lookup"><span data-stu-id="711b7-191">For additional information and guidance on migrating your existing Identity store see [Migrating Authentication and Identity](xref:migration/identity).</span></span>

## <a name="next-steps"></a><span data-ttu-id="711b7-192">次の手順</span><span class="sxs-lookup"><span data-stu-id="711b7-192">Next Steps</span></span>

* [<span data-ttu-id="711b7-193">認証と ID の移行</span><span class="sxs-lookup"><span data-stu-id="711b7-193">Migrating Authentication and Identity</span></span>](xref:migration/identity)
* [<span data-ttu-id="711b7-194">アカウントの確認とパスワードの回復</span><span class="sxs-lookup"><span data-stu-id="711b7-194">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="711b7-195">SMS での 2 要素認証</span><span class="sxs-lookup"><span data-stu-id="711b7-195">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="711b7-196">Facebook、Google、他の外部プロバイダーを使用する認証の有効化</span><span class="sxs-lookup"><span data-stu-id="711b7-196">Enabling authentication using Facebook, Google and other external providers</span></span>](xref:security/authentication/social/index)
