---
title: "ASP.NET Core Id を構成します。"
author: AdrienTorris
description: "ASP.NET Core Id で、既定値を把握し、カスタム値を使用するさまざまな Id プロパティを構成します。"
keywords: "ASP.NET Core、Identity、認証、セキュリティ"
ms.author: scaddie
manager: wpickett
ms.date: 09/18/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-configuration
ms.openlocfilehash: adf577ae1e1c752c3b1a332ec94a7a7627a7a4b4
ms.sourcegitcommit: 76d42f09f3e0dd2f2105493eca6b29994aa47706
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/19/2017
---
# <a name="configure-identity"></a><span data-ttu-id="6b16d-104">Id を構成します。</span><span class="sxs-lookup"><span data-stu-id="6b16d-104">Configure Identity</span></span>

<span data-ttu-id="6b16d-105">ASP.NET Core Id が、アプリケーションで簡単にオーバーライドできる既定の動作によって`Startup`クラスです。</span><span class="sxs-lookup"><span data-stu-id="6b16d-105">ASP.NET Core Identity has some default behaviors that you can override easily in your application's `Startup` class.</span></span>

## <a name="passwords-policy"></a><span data-ttu-id="6b16d-106">パスワード ポリシー</span><span class="sxs-lookup"><span data-stu-id="6b16d-106">Passwords policy</span></span>

<span data-ttu-id="6b16d-107">既定では、Id は、パスワードに、大文字、小文字、数字、および英数字 1 文字を含めることが必要です。</span><span class="sxs-lookup"><span data-stu-id="6b16d-107">By default, Identity requires that passwords contain an uppercase character, lowercase character, a digit, and an alphanumeric character.</span></span> <span data-ttu-id="6b16d-108">その他のいくつかの制限もあります。</span><span class="sxs-lookup"><span data-stu-id="6b16d-108">There are also some other restrictions.</span></span> <span data-ttu-id="6b16d-109">パスワードの制限を簡略化する場合するには、`Startup`アプリケーションのクラスです。</span><span class="sxs-lookup"><span data-stu-id="6b16d-109">If you want to simplify password restrictions, you can do that in the `Startup` class of your application.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6b16d-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6b16d-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="6b16d-111">ASP.NET Core 追加 2.0、`RequiredUniqueChars`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="6b16d-111">ASP.NET Core 2.0 added the `RequiredUniqueChars` property.</span></span> <span data-ttu-id="6b16d-112">それ以外の場合、オプションは、ASP.NET Core から同じ 1.x です。</span><span class="sxs-lookup"><span data-stu-id="6b16d-112">Otherwise, the options are the same from ASP.NET Core 1.x.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6b16d-113">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6b16d-113">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

<span data-ttu-id="6b16d-114">`IdentityOptions.Password`次のプロパティがあります。</span><span class="sxs-lookup"><span data-stu-id="6b16d-114">`IdentityOptions.Password` has the following properties:</span></span>
* <span data-ttu-id="6b16d-115">`RequireDigit`: 0 ~ 9 では、パスワードに数字が必要です。</span><span class="sxs-lookup"><span data-stu-id="6b16d-115">`RequireDigit`: Requires a number between 0-9 in the password.</span></span> <span data-ttu-id="6b16d-116">既定値は true です。</span><span class="sxs-lookup"><span data-stu-id="6b16d-116">Defaults to true.</span></span>
* <span data-ttu-id="6b16d-117">`RequiredLength`: パスワードの最小長。</span><span class="sxs-lookup"><span data-stu-id="6b16d-117">`RequiredLength`: The minimum length of the password.</span></span> <span data-ttu-id="6b16d-118">既定値は 6 です。</span><span class="sxs-lookup"><span data-stu-id="6b16d-118">Defaults to 6.</span></span>
* <span data-ttu-id="6b16d-119">`RequireNonAlphanumeric`: パスワードの英数字以外の文字が必要です。</span><span class="sxs-lookup"><span data-stu-id="6b16d-119">`RequireNonAlphanumeric`: Requires a non-alphanumeric character in the password.</span></span> <span data-ttu-id="6b16d-120">既定値は true です。</span><span class="sxs-lookup"><span data-stu-id="6b16d-120">Defaults to true.</span></span>
* <span data-ttu-id="6b16d-121">`RequireUppercase`: パスワードに大文字が必要です。</span><span class="sxs-lookup"><span data-stu-id="6b16d-121">`RequireUppercase`: Requires an upper case character in the password.</span></span> <span data-ttu-id="6b16d-122">既定値は true です。</span><span class="sxs-lookup"><span data-stu-id="6b16d-122">Defaults to true.</span></span>
* <span data-ttu-id="6b16d-123">`RequireLowercase`: パスワードに小文字が必要です。</span><span class="sxs-lookup"><span data-stu-id="6b16d-123">`RequireLowercase`: Requires a lower case character in the password.</span></span> <span data-ttu-id="6b16d-124">既定値は true です。</span><span class="sxs-lookup"><span data-stu-id="6b16d-124">Defaults to true.</span></span>
* <span data-ttu-id="6b16d-125">`RequiredUniqueChars`: パスワードの個別の文字数が必要です。</span><span class="sxs-lookup"><span data-stu-id="6b16d-125">`RequiredUniqueChars`: Requires the number of distinct characters in the password.</span></span> <span data-ttu-id="6b16d-126">既定値は 1 です。</span><span class="sxs-lookup"><span data-stu-id="6b16d-126">Defaults to 1.</span></span>


## <a name="users-lockout"></a><span data-ttu-id="6b16d-127">ユーザーのロックアウト</span><span class="sxs-lookup"><span data-stu-id="6b16d-127">User's lockout</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

<span data-ttu-id="6b16d-128">`IdentityOptions.Lockout`次のプロパティがあります。</span><span class="sxs-lookup"><span data-stu-id="6b16d-128">`IdentityOptions.Lockout` has the following properties:</span></span>
* <span data-ttu-id="6b16d-129">`DefaultLockoutTimeSpan`: 時間数、ユーザーがロックアウト ロックアウトが発生するとします。</span><span class="sxs-lookup"><span data-stu-id="6b16d-129">`DefaultLockoutTimeSpan`: The amount of time a user is locked out when a lockout occurs.</span></span> <span data-ttu-id="6b16d-130">既定値は 5 分です。</span><span class="sxs-lookup"><span data-stu-id="6b16d-130">Defaults to 5 minutes.</span></span>
* <span data-ttu-id="6b16d-131">`MaxFailedAccessAttempts`: 失敗したアクセス試行回数を、ユーザーがロックアウトされるまでロックアウトが有効になっている場合。</span><span class="sxs-lookup"><span data-stu-id="6b16d-131">`MaxFailedAccessAttempts`: The number of failed access attempts until a user is locked out, if lockout is enabled.</span></span> <span data-ttu-id="6b16d-132">既定値は 5 です。</span><span class="sxs-lookup"><span data-stu-id="6b16d-132">Defaults to 5.</span></span>
* <span data-ttu-id="6b16d-133">`AllowedForNewUsers`: 新しいユーザーをロックアウトできるかどうかを判断します。既定値は true です。</span><span class="sxs-lookup"><span data-stu-id="6b16d-133">`AllowedForNewUsers`: Determines if a new user can be locked out. Defaults to true.</span></span>


## <a name="sign-in-settings"></a><span data-ttu-id="6b16d-134">サインイン設定</span><span class="sxs-lookup"><span data-stu-id="6b16d-134">Sign in settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

<span data-ttu-id="6b16d-135">`IdentityOptions.SignIn`次のプロパティがあります。</span><span class="sxs-lookup"><span data-stu-id="6b16d-135">`IdentityOptions.SignIn` has the following properties:</span></span>
* <span data-ttu-id="6b16d-136">`RequireConfirmedEmail`: 未確認の電子メールにサインインする必要があります。</span><span class="sxs-lookup"><span data-stu-id="6b16d-136">`RequireConfirmedEmail`: Requires a confirmed email to sign in.</span></span> <span data-ttu-id="6b16d-137">既定値は false。</span><span class="sxs-lookup"><span data-stu-id="6b16d-137">Defaults to false.</span></span>
* <span data-ttu-id="6b16d-138">`RequireConfirmedPhoneNumber`: サインインに確認の電話番号が必要です。</span><span class="sxs-lookup"><span data-stu-id="6b16d-138">`RequireConfirmedPhoneNumber`: Requires a confirmed phone number to sign in.</span></span> <span data-ttu-id="6b16d-139">既定値は false。</span><span class="sxs-lookup"><span data-stu-id="6b16d-139">Defaults to false.</span></span>


## <a name="user-validation-settings"></a><span data-ttu-id="6b16d-140">ユーザーの検証の設定</span><span class="sxs-lookup"><span data-stu-id="6b16d-140">User validation settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

<span data-ttu-id="6b16d-141">`IdentityOptions.User`次のプロパティがあります。</span><span class="sxs-lookup"><span data-stu-id="6b16d-141">`IdentityOptions.User` has the following properties:</span></span>
* <span data-ttu-id="6b16d-142">`RequireUniqueEmail`: 各ユーザーを一意の電子メールを持っている必要があります。</span><span class="sxs-lookup"><span data-stu-id="6b16d-142">`RequireUniqueEmail`: Requires each User to have a unique email.</span></span> <span data-ttu-id="6b16d-143">既定値は false。</span><span class="sxs-lookup"><span data-stu-id="6b16d-143">Defaults to false.</span></span>
* <span data-ttu-id="6b16d-144">`AllowedUserNameCharacters`: ユーザー名に文字を指定できます。</span><span class="sxs-lookup"><span data-stu-id="6b16d-144">`AllowedUserNameCharacters`: Allowed characters in the username.</span></span> <span data-ttu-id="6b16d-145">既定値は abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+ です。</span><span class="sxs-lookup"><span data-stu-id="6b16d-145">Defaults to abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+.</span></span>

## <a name="applications-cookie-settings"></a><span data-ttu-id="6b16d-146">アプリケーションの cookie の設定</span><span class="sxs-lookup"><span data-stu-id="6b16d-146">Application's cookie settings</span></span>

<span data-ttu-id="6b16d-147">パスワード ポリシーと同様に、アプリケーションの cookie のすべての設定を変更できます、`Startup`クラスです。</span><span class="sxs-lookup"><span data-stu-id="6b16d-147">Like the passwords policy, all the settings of the application's cookie can be changed in the `Startup` class.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6b16d-148">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6b16d-148">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="6b16d-149">`ConfigureServices`で、`Startup`クラス、アプリケーションの cookie を構成することができます。</span><span class="sxs-lookup"><span data-stu-id="6b16d-149">Under `ConfigureServices` in the `Startup` class, you can configure the application's cookie.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6b16d-150">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6b16d-150">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

--- 

<span data-ttu-id="6b16d-151">`CookieAuthenticationOptions`次のプロパティがあります。</span><span class="sxs-lookup"><span data-stu-id="6b16d-151">`CookieAuthenticationOptions` has the following properties:</span></span>
* <span data-ttu-id="6b16d-152">`Cookie.Name`: Cookie の名前。</span><span class="sxs-lookup"><span data-stu-id="6b16d-152">`Cookie.Name`: The name of the cookie.</span></span> <span data-ttu-id="6b16d-153">既定値です。AspNetCore.Cookies です。</span><span class="sxs-lookup"><span data-stu-id="6b16d-153">Defaults to .AspNetCore.Cookies.</span></span>
* <span data-ttu-id="6b16d-154">`Cookie.HttpOnly`: True の場合、cookie はクライアント側スクリプトからアクセスできません。</span><span class="sxs-lookup"><span data-stu-id="6b16d-154">`Cookie.HttpOnly`: When true, the cookie is not accessible from client-side scripts.</span></span> <span data-ttu-id="6b16d-155">既定値は true です。</span><span class="sxs-lookup"><span data-stu-id="6b16d-155">Defaults to true.</span></span>
* <span data-ttu-id="6b16d-156">`ExpireTimeSpan`: 認証チケットがクッキーに格納されている時間は作成された時点から有効になるを制御します。</span><span class="sxs-lookup"><span data-stu-id="6b16d-156">`ExpireTimeSpan`: Controls how much time the authentication ticket stored in the cookie will remain valid from the point it is created.</span></span> <span data-ttu-id="6b16d-157">既定値は 14 日間です。</span><span class="sxs-lookup"><span data-stu-id="6b16d-157">Defaults to 14 days.</span></span>
* <span data-ttu-id="6b16d-158">`LoginPath`: ユーザーが承認されていない、ときに、ログインにこのパスにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="6b16d-158">`LoginPath`: When a user is unauthorized, they will be redirected to this path to login.</span></span> <span data-ttu-id="6b16d-159">ログイン/アカウント/既定値です。</span><span class="sxs-lookup"><span data-stu-id="6b16d-159">Defaults to /Account/Login.</span></span>
* <span data-ttu-id="6b16d-160">`LogoutPath`: ユーザーがログアウトされるときは、このパスにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="6b16d-160">`LogoutPath`: When a user is logged out, they will be redirected to this path.</span></span> <span data-ttu-id="6b16d-161">既定値は、ログアウト/アカウント/です。</span><span class="sxs-lookup"><span data-stu-id="6b16d-161">Defaults to /Account/Logout.</span></span>
* <span data-ttu-id="6b16d-162">`AccessDeniedPath`: ユーザーには、承認チェックが失敗すると、ときに、このパスにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="6b16d-162">`AccessDeniedPath`: When a user fails an authorization check, they will be redirected to this path.</span></span> <span data-ttu-id="6b16d-163">既定値は/アカウント/AccessDenied です。</span><span class="sxs-lookup"><span data-stu-id="6b16d-163">Defaults to /Account/AccessDenied.</span></span>
* <span data-ttu-id="6b16d-164">`SlidingExpiration`: True の場合、現在 cookie が有効期限 ウィンドウから複数のちょうど中間にあるときに新しい有効期限時刻に新しい cookie が発行されます。</span><span class="sxs-lookup"><span data-stu-id="6b16d-164">`SlidingExpiration`: When true, a new cookie will be issued with a new expiration time when the current cookie is more than halfway through the expiration window.</span></span> <span data-ttu-id="6b16d-165">既定値は true です。</span><span class="sxs-lookup"><span data-stu-id="6b16d-165">Defaults to true.</span></span>
* <span data-ttu-id="6b16d-166">`ReturnUrlParameter`:、ReturnUrlParameter は、は 401 Unauthorized ステータス コードがログイン パスへの 302 リダイレクトに変更されたときに、ミドルウェアによって付加されるクエリ文字列パラメーターの名前を決定します。</span><span class="sxs-lookup"><span data-stu-id="6b16d-166">`ReturnUrlParameter`: The ReturnUrlParameter determines the name of the query string parameter which is appended by the middleware when a 401 Unauthorized status code is changed to a 302 redirect onto the login path.</span></span>
* <span data-ttu-id="6b16d-167">`AuthenticationScheme`: ASP.NET Core これはのみ 1.x です。</span><span class="sxs-lookup"><span data-stu-id="6b16d-167">`AuthenticationScheme`: This is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="6b16d-168">特定の認証スキームの論理名。</span><span class="sxs-lookup"><span data-stu-id="6b16d-168">The logical name for a particular authentication scheme.</span></span>
* <span data-ttu-id="6b16d-169">`AutomaticAuthenticate`: このフラグにのみ関連 ASP.NET Core 1.x です。</span><span class="sxs-lookup"><span data-stu-id="6b16d-169">`AutomaticAuthenticate`: This flag is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="6b16d-170">True の場合、cookie 認証を要求ごとに実行しを検証し、作成された任意のシリアル化されたプリンシパルを再構築しようとします。</span><span class="sxs-lookup"><span data-stu-id="6b16d-170">When true, cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span>

