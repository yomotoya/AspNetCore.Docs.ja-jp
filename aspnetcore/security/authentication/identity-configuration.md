---
title: "ASP.NET Core Id を構成します。"
author: AdrienTorris
description: "ASP.NET Core Id で、既定値を把握し、カスタム値を使用するさまざまな Id プロパティを構成します。"
keywords: "ASP.NET Core、Identity、認証、セキュリティ"
ms.author: scaddie
manager: wpickett
ms.date: 01/11/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-configuration
ms.openlocfilehash: ac204cb89aac1f90adc64c4f0bec4e946cb8c4d9
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/11/2018
---
# <a name="configure-identity"></a><span data-ttu-id="201b6-104">Id を構成します。</span><span class="sxs-lookup"><span data-stu-id="201b6-104">Configure Identity</span></span>

<span data-ttu-id="201b6-105">ASP.NET Core Id は、パスワード ポリシー、ロックアウト時間、およびアプリケーションの簡単にオーバーライドできる cookie の設定などのアプリケーションで共通の動作を持つ`Startup`クラスです。</span><span class="sxs-lookup"><span data-stu-id="201b6-105">ASP.NET Core Identity has common behaviors in applications such as password policy, lockout time, and cookie settings that you can override easily in your application's `Startup` class.</span></span>

## <a name="passwords-policy"></a><span data-ttu-id="201b6-106">パスワード ポリシー</span><span class="sxs-lookup"><span data-stu-id="201b6-106">Passwords policy</span></span>

<span data-ttu-id="201b6-107">既定では、Id は、パスワードに、大文字、小文字、数字、および英数字以外の文字を含めることが必要です。</span><span class="sxs-lookup"><span data-stu-id="201b6-107">By default, Identity requires that passwords contain an uppercase character, lowercase character, a digit, and a non-alphanumeric character.</span></span> <span data-ttu-id="201b6-108">その他のいくつかの制限もあります。</span><span class="sxs-lookup"><span data-stu-id="201b6-108">There are also some other restrictions.</span></span> <span data-ttu-id="201b6-109">パスワードの制限を簡略化の変更、`ConfigureServices`のメソッド、`Startup`アプリケーションのクラスです。</span><span class="sxs-lookup"><span data-stu-id="201b6-109">To simplify password restrictions, modify the `ConfigureServices` method of the `Startup` class of your application.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="201b6-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="201b6-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="201b6-111">ASP.NET Core 追加 2.0、`RequiredUniqueChars`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="201b6-111">ASP.NET Core 2.0 added the `RequiredUniqueChars` property.</span></span> <span data-ttu-id="201b6-112">それ以外の場合、オプションは、ASP.NET Core から同じ 1.x です。</span><span class="sxs-lookup"><span data-stu-id="201b6-112">Otherwise, the options are the same from ASP.NET Core 1.x.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="201b6-113">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="201b6-113">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

<span data-ttu-id="201b6-114">`IdentityOptions.Password`次のプロパティがあります。</span><span class="sxs-lookup"><span data-stu-id="201b6-114">`IdentityOptions.Password` has the following properties:</span></span>

| <span data-ttu-id="201b6-115">プロパティ</span><span class="sxs-lookup"><span data-stu-id="201b6-115">Property</span></span>                | <span data-ttu-id="201b6-116">説明</span><span class="sxs-lookup"><span data-stu-id="201b6-116">Description</span></span>                       | <span data-ttu-id="201b6-117">既定値</span><span class="sxs-lookup"><span data-stu-id="201b6-117">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireDigit`          | <span data-ttu-id="201b6-118">0 ~ 9 では、パスワードに数字が必要です。</span><span class="sxs-lookup"><span data-stu-id="201b6-118">Requires a number between 0-9 in the password.</span></span> | <span data-ttu-id="201b6-119">true</span><span class="sxs-lookup"><span data-stu-id="201b6-119">true</span></span> |
| `RequiredLength`        | <span data-ttu-id="201b6-120">パスワードの最小長。</span><span class="sxs-lookup"><span data-stu-id="201b6-120">The minimum length of the password.</span></span> | <span data-ttu-id="201b6-121">6</span><span class="sxs-lookup"><span data-stu-id="201b6-121">6</span></span> |
| `RequireNonAlphanumeric`| <span data-ttu-id="201b6-122">パスワードに英数字以外の文字が必要です。</span><span class="sxs-lookup"><span data-stu-id="201b6-122">Requires a non-alphanumeric character in the password.</span></span> | <span data-ttu-id="201b6-123">true</span><span class="sxs-lookup"><span data-stu-id="201b6-123">true</span></span> |
| `RequireUppercase`      | <span data-ttu-id="201b6-124">パスワードに大文字が必要です。</span><span class="sxs-lookup"><span data-stu-id="201b6-124">Requires an upper case character in the password.</span></span> | <span data-ttu-id="201b6-125">true</span><span class="sxs-lookup"><span data-stu-id="201b6-125">true</span></span> |
| `RequireLowercase`      | <span data-ttu-id="201b6-126">パスワードに小文字が必要です。</span><span class="sxs-lookup"><span data-stu-id="201b6-126">Requires a lower case character in the password.</span></span> | <span data-ttu-id="201b6-127">true</span><span class="sxs-lookup"><span data-stu-id="201b6-127">true</span></span> |
| `RequiredUniqueChars`   | <span data-ttu-id="201b6-128">パスワードに個別の文字数が必要です。</span><span class="sxs-lookup"><span data-stu-id="201b6-128">Requires the number of distinct characters in the password.</span></span> | <span data-ttu-id="201b6-129">1</span><span class="sxs-lookup"><span data-stu-id="201b6-129">1</span></span> |


## <a name="users-lockout"></a><span data-ttu-id="201b6-130">ユーザーのロックアウト</span><span class="sxs-lookup"><span data-stu-id="201b6-130">User's lockout</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

<span data-ttu-id="201b6-131">`IdentityOptions.Lockout`次のプロパティがあります。</span><span class="sxs-lookup"><span data-stu-id="201b6-131">`IdentityOptions.Lockout` has the following properties:</span></span>

| <span data-ttu-id="201b6-132">プロパティ</span><span class="sxs-lookup"><span data-stu-id="201b6-132">Property</span></span>                | <span data-ttu-id="201b6-133">説明</span><span class="sxs-lookup"><span data-stu-id="201b6-133">Description</span></span>                       | <span data-ttu-id="201b6-134">既定値</span><span class="sxs-lookup"><span data-stu-id="201b6-134">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `DefaultLockoutTimeSpan` | <span data-ttu-id="201b6-135">時間数、ユーザーがロックアウト ロックアウトが発生するとします。</span><span class="sxs-lookup"><span data-stu-id="201b6-135">The amount of time a user is locked out when a lockout occurs.</span></span>  | <span data-ttu-id="201b6-136">5 分</span><span class="sxs-lookup"><span data-stu-id="201b6-136">5 minutes</span></span>  |
| `MaxFailedAccessAttempts` | <span data-ttu-id="201b6-137">ロックアウトが有効になっている場合、ユーザーがロックされるまで、失敗したアクセス試行の数。</span><span class="sxs-lookup"><span data-stu-id="201b6-137">The number of failed access attempts until a user is locked out, if lockout is enabled.</span></span>  | <span data-ttu-id="201b6-138">5</span><span class="sxs-lookup"><span data-stu-id="201b6-138">5</span></span> |
| `AllowedForNewUsers` | <span data-ttu-id="201b6-139">新しいユーザーをロックアウトできるかどうかを判断します。</span><span class="sxs-lookup"><span data-stu-id="201b6-139">Determines if a new user can be locked out.</span></span>  | <span data-ttu-id="201b6-140">true</span><span class="sxs-lookup"><span data-stu-id="201b6-140">true</span></span> |

## <a name="sign-in-settings"></a><span data-ttu-id="201b6-141">サインイン設定</span><span class="sxs-lookup"><span data-stu-id="201b6-141">Sign in settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

<span data-ttu-id="201b6-142">`IdentityOptions.SignIn`次のプロパティがあります。</span><span class="sxs-lookup"><span data-stu-id="201b6-142">`IdentityOptions.SignIn` has the following properties:</span></span>

| <span data-ttu-id="201b6-143">プロパティ</span><span class="sxs-lookup"><span data-stu-id="201b6-143">Property</span></span>                | <span data-ttu-id="201b6-144">説明</span><span class="sxs-lookup"><span data-stu-id="201b6-144">Description</span></span>                       | <span data-ttu-id="201b6-145">既定値</span><span class="sxs-lookup"><span data-stu-id="201b6-145">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireConfirmedEmail` | <span data-ttu-id="201b6-146">確認の電子メールにサインインする必要があります。</span><span class="sxs-lookup"><span data-stu-id="201b6-146">Requires a confirmed email to sign in.</span></span> | <span data-ttu-id="201b6-147">False</span><span class="sxs-lookup"><span data-stu-id="201b6-147">false</span></span>  |
| `RequireConfirmedPhoneNumber` |  <span data-ttu-id="201b6-148">確認の電話番号にサインインする必要があります。</span><span class="sxs-lookup"><span data-stu-id="201b6-148">Requires a confirmed phone number to sign in.</span></span> | <span data-ttu-id="201b6-149">False</span><span class="sxs-lookup"><span data-stu-id="201b6-149">false</span></span>  |

## <a name="user-validation-settings"></a><span data-ttu-id="201b6-150">ユーザーの検証の設定</span><span class="sxs-lookup"><span data-stu-id="201b6-150">User validation settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

<span data-ttu-id="201b6-151">`IdentityOptions.User`次のプロパティがあります。</span><span class="sxs-lookup"><span data-stu-id="201b6-151">`IdentityOptions.User` has the following properties:</span></span>

| <span data-ttu-id="201b6-152">プロパティ</span><span class="sxs-lookup"><span data-stu-id="201b6-152">Property</span></span>                | <span data-ttu-id="201b6-153">説明</span><span class="sxs-lookup"><span data-stu-id="201b6-153">Description</span></span>                       | <span data-ttu-id="201b6-154">既定値</span><span class="sxs-lookup"><span data-stu-id="201b6-154">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireUniqueEmail`  | <span data-ttu-id="201b6-155">各ユーザーを一意の電子メールを持っている必要があります。</span><span class="sxs-lookup"><span data-stu-id="201b6-155">Requires each User to have a unique email.</span></span> | <span data-ttu-id="201b6-156">False</span><span class="sxs-lookup"><span data-stu-id="201b6-156">false</span></span>  |
| `AllowedUserNameCharacters`  | <span data-ttu-id="201b6-157">ユーザー名に許容される文字。</span><span class="sxs-lookup"><span data-stu-id="201b6-157">Allowed characters in the username.</span></span> | <span data-ttu-id="201b6-158">abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+</span><span class="sxs-lookup"><span data-stu-id="201b6-158">abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+</span></span> |


## <a name="applications-cookie-settings"></a><span data-ttu-id="201b6-159">アプリケーションの cookie の設定</span><span class="sxs-lookup"><span data-stu-id="201b6-159">Application's cookie settings</span></span>

<span data-ttu-id="201b6-160">パスワード ポリシーと同様に、アプリケーションの cookie のすべての設定を変更できます、`Startup`クラスです。</span><span class="sxs-lookup"><span data-stu-id="201b6-160">Like the passwords policy, all the settings of the application's cookie can be changed in the `Startup` class.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="201b6-161">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="201b6-161">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="201b6-162">`ConfigureServices`で、`Startup`クラス、アプリケーションの cookie を構成することができます。</span><span class="sxs-lookup"><span data-stu-id="201b6-162">Under `ConfigureServices` in the `Startup` class, you can configure the application's cookie.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="201b6-163">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="201b6-163">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

---

<span data-ttu-id="201b6-164">`CookieAuthenticationOptions`次のプロパティがあります。</span><span class="sxs-lookup"><span data-stu-id="201b6-164">`CookieAuthenticationOptions` has the following properties:</span></span>

| <span data-ttu-id="201b6-165">プロパティ</span><span class="sxs-lookup"><span data-stu-id="201b6-165">Property</span></span>                | <span data-ttu-id="201b6-166">説明</span><span class="sxs-lookup"><span data-stu-id="201b6-166">Description</span></span>                       | <span data-ttu-id="201b6-167">既定値</span><span class="sxs-lookup"><span data-stu-id="201b6-167">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `Cookie.Name`  | <span data-ttu-id="201b6-168">Cookie の名前。</span><span class="sxs-lookup"><span data-stu-id="201b6-168">The name of the cookie.</span></span>  | <span data-ttu-id="201b6-169">.AspNetCore.Cookies です。</span><span class="sxs-lookup"><span data-stu-id="201b6-169">.AspNetCore.Cookies.</span></span>  |
| `Cookie.HttpOnly`  | <span data-ttu-id="201b6-170">True の場合、cookie はクライアント側スクリプトからアクセスできません。</span><span class="sxs-lookup"><span data-stu-id="201b6-170">When true, the cookie is not accessible from client-side scripts.</span></span>  |  <span data-ttu-id="201b6-171">true</span><span class="sxs-lookup"><span data-stu-id="201b6-171">true</span></span> |
| `ExpireTimeSpan`  | <span data-ttu-id="201b6-172">認証チケットがクッキーに格納されている時間は作成された時点から有効になるかを制御します。</span><span class="sxs-lookup"><span data-stu-id="201b6-172">Controls how much time the authentication ticket stored in the cookie will remain valid from the point it is created.</span></span>  | <span data-ttu-id="201b6-173">14 日間</span><span class="sxs-lookup"><span data-stu-id="201b6-173">14 days</span></span>  |
| `LoginPath`  | <span data-ttu-id="201b6-174">ユーザーが承認されていない、ときに、ログインにこのパスにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="201b6-174">When a user is unauthorized, they will be redirected to this path to login.</span></span> | <span data-ttu-id="201b6-175">/アカウント/ログイン</span><span class="sxs-lookup"><span data-stu-id="201b6-175">/Account/Login</span></span>  |
| `LogoutPath`  | <span data-ttu-id="201b6-176">ユーザーがログアウトするときは、このパスにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="201b6-176">When a user is logged out, they will be redirected to this path.</span></span>  | <span data-ttu-id="201b6-177">/アカウント/ログアウト</span><span class="sxs-lookup"><span data-stu-id="201b6-177">/Account/Logout</span></span>  |
| `AccessDeniedPath`  | <span data-ttu-id="201b6-178">ユーザーには、承認チェックが失敗した場合は、このパスにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="201b6-178">When a user fails an authorization check, they will be redirected to this path.</span></span>  |   |
| `SlidingExpiration`  | <span data-ttu-id="201b6-179">True の場合、現在 cookie が有効期限 ウィンドウから複数のちょうど中間にあるときに新しい有効期限時刻に新しい cookie が発行されます。</span><span class="sxs-lookup"><span data-stu-id="201b6-179">When true, a new cookie will be issued with a new expiration time when the current cookie is more than halfway through the expiration window.</span></span>  | <span data-ttu-id="201b6-180">/アカウント/AccessDenied</span><span class="sxs-lookup"><span data-stu-id="201b6-180">/Account/AccessDenied</span></span> |
| `ReturnUrlParameter`  | <span data-ttu-id="201b6-181">401 Unauthorized ステータス コードがログイン パスへの 302 リダイレクトに変更されたときに、ミドルウェアによって付加されるクエリ文字列パラメーターの名前を決定します。</span><span class="sxs-lookup"><span data-stu-id="201b6-181">Determines the name of the query string parameter which is appended by the middleware when a 401 Unauthorized status code is changed to a 302 redirect onto the login path.</span></span>  |  <span data-ttu-id="201b6-182">true</span><span class="sxs-lookup"><span data-stu-id="201b6-182">true</span></span> |
| `AuthenticationScheme`  | <span data-ttu-id="201b6-183">これにのみ関連 ASP.NET Core 1.x です。</span><span class="sxs-lookup"><span data-stu-id="201b6-183">This is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="201b6-184">特定の認証スキームの論理名。</span><span class="sxs-lookup"><span data-stu-id="201b6-184">The logical name for a particular authentication scheme.</span></span> |  |
| `AutomaticAuthenticate`  | <span data-ttu-id="201b6-185">このフラグにのみ関連 ASP.NET Core 1.x です。</span><span class="sxs-lookup"><span data-stu-id="201b6-185">This flag is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="201b6-186">True の場合、cookie 認証を要求ごとに実行しを検証し、作成された任意のシリアル化されたプリンシパルを再構築しようとします。</span><span class="sxs-lookup"><span data-stu-id="201b6-186">When true, cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span>  |  |