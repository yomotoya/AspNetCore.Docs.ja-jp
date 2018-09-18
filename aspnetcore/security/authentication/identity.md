---
title: ASP.NET core Identity の概要
author: rick-anderson
description: ASP.NET Core アプリでは、Id を使用します。 パスワードの要件 (RequireDigit、RequiredLength、RequiredUniqueChars など) を設定する方法について説明します。
ms.author: riande
ms.date: 08/08/2018
uid: security/authentication/identity
ms.openlocfilehash: ca83d07f7d93bd0cc61cd17cc373096b8e6aa2e1
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/18/2018
ms.locfileid: "46010976"
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="b4a31-104">ASP.NET core Identity の概要</span><span class="sxs-lookup"><span data-stu-id="b4a31-104">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="b4a31-105">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b4a31-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b4a31-106">ASP.NET Core Identity は、ASP.NET Core アプリにログイン機能を追加するメンバーシップ システムです。</span><span class="sxs-lookup"><span data-stu-id="b4a31-106">ASP.NET Core Identity is a membership system that adds login functionality to ASP.NET Core apps.</span></span> <span data-ttu-id="b4a31-107">Id に格納されているログイン情報で、ユーザーがアカウントの作成または外部ログイン プロバイダーを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="b4a31-107">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="b4a31-108">サポートされている外部ログイン プロバイダーには、 [Facebook、Google、Microsoft アカウント、および Twitter](xref:security/authentication/social/index)します。</span><span class="sxs-lookup"><span data-stu-id="b4a31-108">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="b4a31-109">Id は、ユーザー名、パスワード、およびプロファイル データを格納する SQL Server データベースを使用して構成できます。</span><span class="sxs-lookup"><span data-stu-id="b4a31-109">Identity can be configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="b4a31-110">または、別の永続ストア使用できます、たとえば、Azure Table Storage。</span><span class="sxs-lookup"><span data-stu-id="b4a31-110">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

[<span data-ttu-id="b4a31-111">表示またはサンプル コードをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="b4a31-111">View or download the sample code.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) [<span data-ttu-id="b4a31-112">(ダウンロードする方法)</span><span class="sxs-lookup"><span data-stu-id="b4a31-112">(How to download)</span></span>](xref:tutorials/index#how-to-download-a-sample)

<span data-ttu-id="b4a31-113">このトピックを登録するには、ログイン Id を使用する方法について説明し、ユーザーをログアウトします。</span><span class="sxs-lookup"><span data-stu-id="b4a31-113">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="b4a31-114">Id を使用するアプリの作成について詳細な手順については、この記事の最後に次の手順を参照してください。</span><span class="sxs-lookup"><span data-stu-id="b4a31-114">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

::: moniker range=">= aspnetcore-2.1"

<a name="adi"></a>
## <a name="adddefaultidentity-and-addidentity"></a><span data-ttu-id="b4a31-115">AddDefaultIdentity と AddIdentity</span><span class="sxs-lookup"><span data-stu-id="b4a31-115">AddDefaultIdentity and AddIdentity</span></span>

<span data-ttu-id="b4a31-116">[AddDefaultIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionUIExtensions_AddDefaultIdentity__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__) ASP で導入されました。Core 2.1 です。</span><span class="sxs-lookup"><span data-stu-id="b4a31-116">[AddDefaultIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionUIExtensions_AddDefaultIdentity__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__) was introduced in ASP.Core 2.1.</span></span> <span data-ttu-id="b4a31-117">呼び出す`AddDefaultIdentity`は、次の呼び出しに似ています。</span><span class="sxs-lookup"><span data-stu-id="b4a31-117">Calling `AddDefaultIdentity` is similar to calling the following:</span></span>

* [<span data-ttu-id="b4a31-118">AddIdentity</span><span class="sxs-lookup"><span data-stu-id="b4a31-118">AddIdentity</span></span>](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_AddIdentity__2_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__)
* [<span data-ttu-id="b4a31-119">AddDefaultUI</span><span class="sxs-lookup"><span data-stu-id="b4a31-119">AddDefaultUI</span></span>](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderuiextensions.adddefaultui?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderUIExtensions_AddDefaultUI_Microsoft_AspNetCore_Identity_IdentityBuilder_)
* [<span data-ttu-id="b4a31-120">AddDefaultTokenProviders</span><span class="sxs-lookup"><span data-stu-id="b4a31-120">AddDefaultTokenProviders</span></span>](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderextensions.adddefaulttokenproviders?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderExtensions_AddDefaultTokenProviders_Microsoft_AspNetCore_Identity_IdentityBuilder_)

<span data-ttu-id="b4a31-121">参照してください[AddDefaultIdentity ソース](https://github.com/aspnet/Identity/blob/2634637fd535b229762b5e4a49cdd128f4d8f12e/src/UI/IdentityServiceCollectionUIExtensions.cs#L47-L64)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="b4a31-121">See [AddDefaultIdentity source](https://github.com/aspnet/Identity/blob/2634637fd535b229762b5e4a49cdd128f4d8f12e/src/UI/IdentityServiceCollectionUIExtensions.cs#L47-L64) for more information.</span></span>

::: moniker-end

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="b4a31-122">認証を使用した Web アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="b4a31-122">Create a Web app with authentication</span></span>

<span data-ttu-id="b4a31-123">個々 のユーザー アカウントを使って、ASP.NET Core Web アプリケーション プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="b4a31-123">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b4a31-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b4a31-124">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b4a31-125">**[ファイル]** > **[新規作成]** > **[プロジェクト]** を順に選択します。</span><span class="sxs-lookup"><span data-stu-id="b4a31-125">Select **File** > **New** > **Project**.</span></span> 
* <span data-ttu-id="b4a31-126">**[ASP.NET Core Web アプリケーション]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="b4a31-126">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="b4a31-127">プロジェクトに名前を**WebApp1**にプロジェクトのダウンロードとして同じ名前空間。</span><span class="sxs-lookup"><span data-stu-id="b4a31-127">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="b4a31-128">**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="b4a31-128">Click **OK**.</span></span>
* <span data-ttu-id="b4a31-129">ASP.NET Core を選択します。 **Web アプリケーション**ASP.NET Core 2.1 では、を選択し、**認証の変更**します。</span><span class="sxs-lookup"><span data-stu-id="b4a31-129">Select an ASP.NET Core **Web Application** for ASP.NET Core 2.1, then select **Change Authentication**.</span></span>
* <span data-ttu-id="b4a31-130">選択**個々 のユーザー アカウント** をクリック**OK**します。</span><span class="sxs-lookup"><span data-stu-id="b4a31-130">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b4a31-131">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b4a31-131">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet new webapp --auth Individual -o WebApp1
```

---

<span data-ttu-id="b4a31-132">生成されたプロジェクトは、 [ASP.NET Core Identity](xref:security/authentication/identity)として、 [Razor クラス ライブラリ](xref:razor-pages/ui-class)します。</span><span class="sxs-lookup"><span data-stu-id="b4a31-132">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span>

### <a name="test-register-and-login"></a><span data-ttu-id="b4a31-133">テスト登録とログイン</span><span class="sxs-lookup"><span data-stu-id="b4a31-133">Test Register and Login</span></span>

<span data-ttu-id="b4a31-134">アプリを実行し、ユーザーを登録します。</span><span class="sxs-lookup"><span data-stu-id="b4a31-134">Run the app and register a user.</span></span> <span data-ttu-id="b4a31-135">参照するナビゲーションのトグル ボタンを選択する必要があります、画面サイズに応じて、**登録**と**ログイン**リンク。</span><span class="sxs-lookup"><span data-stu-id="b4a31-135">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

![ナビゲーション バーのトグル ボタン](identity/_static/navToggle.png)

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>
### <a name="configure-identity-services"></a><span data-ttu-id="b4a31-137">Id サービスを構成します。</span><span class="sxs-lookup"><span data-stu-id="b4a31-137">Configure Identity services</span></span>

<span data-ttu-id="b4a31-138">サービスを追加`ConfigureServices`します。</span><span class="sxs-lookup"><span data-stu-id="b4a31-138">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="b4a31-139">一般的なパターンは、すべての `Add{Service}` メソッドを呼び出した後、すべての `services.Configure{Service}` メソッドを呼び出すことです。</span><span class="sxs-lookup"><span data-stu-id="b4a31-139">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span> <span data-ttu-id="b4a31-140">次のコードに生成されたテンプレートが含まれていない`CookiePolicyOptions`:</span><span class="sxs-lookup"><span data-stu-id="b4a31-140">The following code doesn't include the template generated `CookiePolicyOptions`:</span></span>

::: moniker range=">= aspnetcore-2.1"

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Startup.cs?name=snippet_configureservices)]

<span data-ttu-id="b4a31-141">上記のコードでは、オプションの既定値で Identity を構成します。</span><span class="sxs-lookup"><span data-stu-id="b4a31-141">The preceding code configures Identity with default option values.</span></span> <span data-ttu-id="b4a31-142">サービスを使用してアプリで利用できる[依存関係の注入](xref:fundamentals/dependency-injection)します。</span><span class="sxs-lookup"><span data-stu-id="b4a31-142">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="b4a31-143">Id を呼び出すことによって有効に[UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_)します。</span><span class="sxs-lookup"><span data-stu-id="b4a31-143">Identity is enabled by calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span> <span data-ttu-id="b4a31-144">`UseAuthentication` 認証を追加します。[ミドルウェア](xref:fundamentals/middleware/index)要求パイプラインにします。</span><span class="sxs-lookup"><span data-stu-id="b4a31-144">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Startup.cs?name=snippet_configure&highlight=18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   <span data-ttu-id="b4a31-145">使用して、アプリケーションで利用できるサービス[依存関係の注入](xref:fundamentals/dependency-injection)します。</span><span class="sxs-lookup"><span data-stu-id="b4a31-145">Services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="b4a31-146">呼び出すことによって、アプリケーションの id が有効に`UseAuthentication`で、`Configure`メソッド。</span><span class="sxs-lookup"><span data-stu-id="b4a31-146">Identity is enabled for the application by calling `UseAuthentication` in the `Configure` method.</span></span> <span data-ttu-id="b4a31-147">`UseAuthentication` 認証を追加します。[ミドルウェア](xref:fundamentals/middleware/index)要求パイプラインにします。</span><span class="sxs-lookup"><span data-stu-id="b4a31-147">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   <span data-ttu-id="b4a31-148">これらのサービスを使用して、アプリケーションで利用できる[依存関係の注入](xref:fundamentals/dependency-injection)します。</span><span class="sxs-lookup"><span data-stu-id="b4a31-148">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="b4a31-149">呼び出すことによって、アプリケーションの id が有効に`UseIdentity`で、`Configure`メソッド。</span><span class="sxs-lookup"><span data-stu-id="b4a31-149">Identity is enabled for the application by calling `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="b4a31-150">`UseIdentity` cookie ベースの認証を追加します。[ミドルウェア](xref:fundamentals/middleware/index)要求パイプラインにします。</span><span class="sxs-lookup"><span data-stu-id="b4a31-150">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

::: moniker-end

<span data-ttu-id="b4a31-151">詳細については、次を参照してください。、 [IdentityOptions クラス](/dotnet/api/microsoft.aspnetcore.identity.identityoptions)と[アプリケーションの起動](xref:fundamentals/startup)します。</span><span class="sxs-lookup"><span data-stu-id="b4a31-151">For more information, see the [IdentityOptions Class](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="b4a31-152">登録、ログイン、およびログアウトをスキャフォールディングします。</span><span class="sxs-lookup"><span data-stu-id="b4a31-152">Scaffold Register, Login, and LogOut</span></span>

<span data-ttu-id="b4a31-153">に従って、[の id の権限を持つ Razor プロジェクトにスキャフォールディング](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization)指示します。</span><span class="sxs-lookup"><span data-stu-id="b4a31-153">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b4a31-154">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b4a31-154">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="b4a31-155">登録、ログイン、ログアウト ファイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="b4a31-155">Add the Register, Login, and LogOut files.</span></span>


# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b4a31-156">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b4a31-156">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="b4a31-157">名前のプロジェクトを作成する場合**WebApp1**、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="b4a31-157">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="b4a31-158">それ以外の場合、正しい名前空間を使用して、 `ApplicationDbContext`:</span><span class="sxs-lookup"><span data-stu-id="b4a31-158">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>


```cli
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"

```

<span data-ttu-id="b4a31-159">PowerShell では、コマンドの区切り記号としてセミコロンを使用します。</span><span class="sxs-lookup"><span data-stu-id="b4a31-159">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="b4a31-160">PowerShell を使用する場合は、ファイルの一覧でセミコロンをエスケープまたはファイルのリストを前の例のように、二重引用符に配置します。</span><span class="sxs-lookup"><span data-stu-id="b4a31-160">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="b4a31-161">登録を確認します。</span><span class="sxs-lookup"><span data-stu-id="b4a31-161">Examine Register</span></span>

::: moniker range=">= aspnetcore-2.1"

   <span data-ttu-id="b4a31-162">ユーザーがクリックすると、**登録**、リンク、`RegisterModel.OnPostAsync`アクションが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="b4a31-162">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="b4a31-163">によって、ユーザーが作成された[CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_)上、`_userManager`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="b4a31-163">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object.</span></span> <span data-ttu-id="b4a31-164">`_userManager` によって提供される依存関係の挿入)。</span><span class="sxs-lookup"><span data-stu-id="b4a31-164">`_userManager` is provided by dependency injection):</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Register.cshtml.cs?name=snippet&highlight=7,22)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   <span data-ttu-id="b4a31-165">ユーザーがクリックすると、**登録**、リンク、`Register`でアクションが呼び出された`AccountController`。</span><span class="sxs-lookup"><span data-stu-id="b4a31-165">When a user clicks the **Register** link, the `Register` action is invoked on `AccountController`.</span></span> <span data-ttu-id="b4a31-166">`Register`アクションを呼び出して、ユーザーを作成します`CreateAsync`上、`_userManager`オブジェクト (に提供される`AccountController`依存関係の挿入によって)。</span><span class="sxs-lookup"><span data-stu-id="b4a31-166">The `Register` action creates the user by calling `CreateAsync` on the `_userManager` object (provided to `AccountController` by dependency injection):</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

::: moniker-end

   <span data-ttu-id="b4a31-167">ユーザーが正常に作成された場合、ユーザーがログインへの呼び出しによって`_signInManager.SignInAsync`します。</span><span class="sxs-lookup"><span data-stu-id="b4a31-167">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

   <span data-ttu-id="b4a31-168">**注:** を参照してください[アカウントの確認](xref:security/authentication/accconfirm#prevent-login-at-registration)登録時の即時のログインを防止する手順についてはします。</span><span class="sxs-lookup"><span data-stu-id="b4a31-168">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="b4a31-169">ログイン</span><span class="sxs-lookup"><span data-stu-id="b4a31-169">Log in</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="b4a31-170">ログイン フォームが表示されるとき。</span><span class="sxs-lookup"><span data-stu-id="b4a31-170">The Login form is displayed when:</span></span>

* <span data-ttu-id="b4a31-171">**ログイン** リンクを選択します。</span><span class="sxs-lookup"><span data-stu-id="b4a31-171">The **Log in** link  is selected.</span></span>
* <span data-ttu-id="b4a31-172">ユーザーがいないに認証されるページにアクセスするときに**または**承認されると、ログイン ページにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="b4a31-172">When a user accesses a page where they are not authenticated **or** authorized, they are redirected to the Login page.</span></span> 

<span data-ttu-id="b4a31-173">ログイン ページのフォームが送信されたときに、`OnPostAsync`アクションが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="b4a31-173">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="b4a31-174">`PasswordSignInAsync` 呼び出される、 `_signInManager` (依存関係の挿入によって提供される) オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="b4a31-174">`PasswordSignInAsync` is called on the `_signInManager` object (provided by dependency injection).</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Login.cshtml.cs?name=snippet&highlight=10-11)]

   <span data-ttu-id="b4a31-175">基本`Controller`クラスでは、`User`コント ローラー メソッドからアクセスできるプロパティです。</span><span class="sxs-lookup"><span data-stu-id="b4a31-175">The base `Controller` class exposes a `User` property that you can access from controller methods.</span></span> <span data-ttu-id="b4a31-176">たとえば、列挙することができます`User.Claims`承認決定を行うとします。</span><span class="sxs-lookup"><span data-stu-id="b4a31-176">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="b4a31-177">詳細については、次を参照してください。[承認](xref:security/authorization/index)します。</span><span class="sxs-lookup"><span data-stu-id="b4a31-177">For more information, see [Authorization](xref:security/authorization/index).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="b4a31-178">ユーザー選択すると、ログイン フォームが表示されます、**ログイン**リンクまたは認証が必要なページにアクセスするときにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="b4a31-178">The Login form is displayed when users select the **Log in** link or are redirected when accessing a page that requires authentication.</span></span> <span data-ttu-id="b4a31-179">ユーザーがログイン ページで、フォームを送信するときに、 `AccountController` `Login`アクションが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="b4a31-179">When the user submits the form on the Login page, the `AccountController` `Login` action is called.</span></span>

<span data-ttu-id="b4a31-180">`Login`アクション呼び出し`PasswordSignInAsync`上、`_signInManager`オブジェクト (に提供される`AccountController`依存関係の挿入によって)。</span><span class="sxs-lookup"><span data-stu-id="b4a31-180">The `Login` action calls `PasswordSignInAsync` on the `_signInManager` object (provided to `AccountController` by dependency injection).</span></span>

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

<span data-ttu-id="b4a31-181">基本 (`Controller`または`PageModel`) クラスでは、`User`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="b4a31-181">The base (`Controller` or `PageModel`) class exposes a `User` property.</span></span> <span data-ttu-id="b4a31-182">たとえば、`User.Claims`承認決定を行うために列挙できることができます。</span><span class="sxs-lookup"><span data-stu-id="b4a31-182">For example, `User.Claims` can be enumerated to make authorization decisions.</span></span>

::: moniker-end

### <a name="log-out"></a><span data-ttu-id="b4a31-183">ログアウトします。</span><span class="sxs-lookup"><span data-stu-id="b4a31-183">Log out</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="b4a31-184">**ログアウト**リンクを呼び出す、`LogoutModel.OnPost`アクション。</span><span class="sxs-lookup"><span data-stu-id="b4a31-184">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Logout.cshtml.cs)]

<span data-ttu-id="b4a31-185">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) cookie に格納されているユーザーの要求をクリアします。</span><span class="sxs-lookup"><span data-stu-id="b4a31-185">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span> <span data-ttu-id="b4a31-186">呼び出した後にリダイレクトしない`SignOutAsync`ユーザーまたは**いない**サインアウトします。</span><span class="sxs-lookup"><span data-stu-id="b4a31-186">Don't redirect after calling `SignOutAsync` or the user will **not** be signed out.</span></span>

<span data-ttu-id="b4a31-187">投稿がで指定された、 *Pages/Shared/_LoginPartial.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="b4a31-187">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/_LoginPartial.cshtml?highlight=10)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   <span data-ttu-id="b4a31-188">クリックすると、**ログアウト**呼び出しのリンク、`LogOut`アクション。</span><span class="sxs-lookup"><span data-stu-id="b4a31-188">Clicking the **Log out** link calls the `LogOut` action.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   <span data-ttu-id="b4a31-189">上記のコードの呼び出し、`_signInManager.SignOutAsync`メソッド。</span><span class="sxs-lookup"><span data-stu-id="b4a31-189">The preceding code calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="b4a31-190">`SignOutAsync`メソッドは、cookie に格納されているユーザーの要求をクリアします。</span><span class="sxs-lookup"><span data-stu-id="b4a31-190">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>

::: moniker-end

## <a name="test-identity"></a><span data-ttu-id="b4a31-191">テスト Id</span><span class="sxs-lookup"><span data-stu-id="b4a31-191">Test Identity</span></span>

<span data-ttu-id="b4a31-192">既定の web プロジェクト テンプレートは、ホーム ページへの匿名アクセスを許可します。</span><span class="sxs-lookup"><span data-stu-id="b4a31-192">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="b4a31-193">Id をテストするには追加[ `[Authorize]` ](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) About ページにします。</span><span class="sxs-lookup"><span data-stu-id="b4a31-193">To test Identity, add [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) to the About page.</span></span>

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/About.cshtml.cs)]

<span data-ttu-id="b4a31-194">サインイン場合、サインアウトします。アプリを実行し、選択、**について**リンク。</span><span class="sxs-lookup"><span data-stu-id="b4a31-194">If you are signed in, sign out. Run the app and select the **About** link.</span></span> <span data-ttu-id="b4a31-195">ログイン ページにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="b4a31-195">You are redirected to the login page.</span></span>

::: moniker range=">= aspnetcore-2.1"

### <a name="explore-identity"></a><span data-ttu-id="b4a31-196">Id を調べる</span><span class="sxs-lookup"><span data-stu-id="b4a31-196">Explore Identity</span></span>

<span data-ttu-id="b4a31-197">Identity をさらに詳しく調査。</span><span class="sxs-lookup"><span data-stu-id="b4a31-197">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="b4a31-198">完全な id の UI のソースを作成します。</span><span class="sxs-lookup"><span data-stu-id="b4a31-198">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="b4a31-199">各ページと、デバッガーをステップのソースを確認します。</span><span class="sxs-lookup"><span data-stu-id="b4a31-199">Examine the source of each page and step through the debugger.</span></span>

::: moniker-end

## <a name="identity-components"></a><span data-ttu-id="b4a31-200">Id コンポーネント</span><span class="sxs-lookup"><span data-stu-id="b4a31-200">Identity Components</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="b4a31-201">すべての Id 依存の NuGet パッケージが含まれている、 [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)します。</span><span class="sxs-lookup"><span data-stu-id="b4a31-201">All the Identity dependent NuGet packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="b4a31-202">プライマリ パッケージの Id が[Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/)します。</span><span class="sxs-lookup"><span data-stu-id="b4a31-202">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="b4a31-203">このパッケージは、ASP.NET Core Identity のインターフェイスのコア セットが含まれていてに含まれている`Microsoft.AspNetCore.Identity.EntityFrameworkCore`します。</span><span class="sxs-lookup"><span data-stu-id="b4a31-203">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="b4a31-204">ASP.NET Core Identity に移行します。</span><span class="sxs-lookup"><span data-stu-id="b4a31-204">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="b4a31-205">詳細と、既存の Id ストアを移行のガイダンスについては、次を参照してください。[移行の認証と Id](xref:migration/identity)します。</span><span class="sxs-lookup"><span data-stu-id="b4a31-205">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="b4a31-206">パスワードの強度を設定</span><span class="sxs-lookup"><span data-stu-id="b4a31-206">Setting password strength</span></span>

<span data-ttu-id="b4a31-207">参照してください[構成](#pw)のパスワードの最小要件を設定するサンプルです。</span><span class="sxs-lookup"><span data-stu-id="b4a31-207">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b4a31-208">次の手順</span><span class="sxs-lookup"><span data-stu-id="b4a31-208">Next Steps</span></span>

* [<span data-ttu-id="b4a31-209">Identity の構成</span><span class="sxs-lookup"><span data-stu-id="b4a31-209">Configure Identity</span></span>](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <span data-ttu-id="b4a31-210">[Id の主キーのデータ型を構成する](xref:security/authentication/identity-primary-key-configuration)します。</span><span class="sxs-lookup"><span data-stu-id="b4a31-210">[Configure Identity primary keys data type](xref:security/authentication/identity-primary-key-configuration).</span></span>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>
