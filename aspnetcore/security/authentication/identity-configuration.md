---
title: "ASP.NET Core Id を構成します。"
author: AdrienTorris
description: "ASP.NET Core Id で、既定値を把握し、カスタム値を使用するさまざまな Id プロパティを構成します。"
manager: wpickett
ms.author: scaddie
ms.date: 01/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity-configuration
ms.openlocfilehash: cf7dcdb80f5edf9e10960cb08957793c36829a69
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="configure-identity"></a><span data-ttu-id="a65b3-103">Id を構成します。</span><span class="sxs-lookup"><span data-stu-id="a65b3-103">Configure Identity</span></span>

<span data-ttu-id="a65b3-104">ASP.NET Core Id は、パスワード ポリシー、ロックアウト時間、およびアプリケーションの簡単にオーバーライドできる cookie の設定などのアプリケーションで共通の動作を持つ`Startup`クラスです。</span><span class="sxs-lookup"><span data-stu-id="a65b3-104">ASP.NET Core Identity has common behaviors in applications such as password policy, lockout time, and cookie settings that you can override easily in your application's `Startup` class.</span></span>

## <a name="passwords-policy"></a><span data-ttu-id="a65b3-105">パスワード ポリシー</span><span class="sxs-lookup"><span data-stu-id="a65b3-105">Passwords policy</span></span>

<span data-ttu-id="a65b3-106">既定では、Id は、パスワードに、大文字、小文字、数字、および英数字以外の文字を含めることが必要です。</span><span class="sxs-lookup"><span data-stu-id="a65b3-106">By default, Identity requires that passwords contain an uppercase character, lowercase character, a digit, and a non-alphanumeric character.</span></span> <span data-ttu-id="a65b3-107">その他のいくつかの制限もあります。</span><span class="sxs-lookup"><span data-stu-id="a65b3-107">There are also some other restrictions.</span></span> <span data-ttu-id="a65b3-108">パスワードの制限を簡略化の変更、`ConfigureServices`のメソッド、`Startup`アプリケーションのクラスです。</span><span class="sxs-lookup"><span data-stu-id="a65b3-108">To simplify password restrictions, modify the `ConfigureServices` method of the `Startup` class of your application.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a65b3-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a65b3-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="a65b3-110">ASP.NET Core 追加 2.0、`RequiredUniqueChars`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="a65b3-110">ASP.NET Core 2.0 added the `RequiredUniqueChars` property.</span></span> <span data-ttu-id="a65b3-111">それ以外の場合、オプションは、ASP.NET Core から同じ 1.x です。</span><span class="sxs-lookup"><span data-stu-id="a65b3-111">Otherwise, the options are the same from ASP.NET Core 1.x.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a65b3-112">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a65b3-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

<span data-ttu-id="a65b3-113">`IdentityOptions.Password`次のプロパティがあります。</span><span class="sxs-lookup"><span data-stu-id="a65b3-113">`IdentityOptions.Password` has the following properties:</span></span>

| <span data-ttu-id="a65b3-114">プロパティ</span><span class="sxs-lookup"><span data-stu-id="a65b3-114">Property</span></span>                | <span data-ttu-id="a65b3-115">説明</span><span class="sxs-lookup"><span data-stu-id="a65b3-115">Description</span></span>                       | <span data-ttu-id="a65b3-116">既定値</span><span class="sxs-lookup"><span data-stu-id="a65b3-116">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireDigit`          | <span data-ttu-id="a65b3-117">0 ~ 9 では、パスワードに数字が必要です。</span><span class="sxs-lookup"><span data-stu-id="a65b3-117">Requires a number between 0-9 in the password.</span></span> | <span data-ttu-id="a65b3-118">true</span><span class="sxs-lookup"><span data-stu-id="a65b3-118">true</span></span> |
| `RequiredLength`        | <span data-ttu-id="a65b3-119">パスワードの最小長。</span><span class="sxs-lookup"><span data-stu-id="a65b3-119">The minimum length of the password.</span></span> | <span data-ttu-id="a65b3-120">6</span><span class="sxs-lookup"><span data-stu-id="a65b3-120">6</span></span> |
| `RequireNonAlphanumeric`| <span data-ttu-id="a65b3-121">パスワードに英数字以外の文字が必要です。</span><span class="sxs-lookup"><span data-stu-id="a65b3-121">Requires a non-alphanumeric character in the password.</span></span> | <span data-ttu-id="a65b3-122">true</span><span class="sxs-lookup"><span data-stu-id="a65b3-122">true</span></span> |
| `RequireUppercase`      | <span data-ttu-id="a65b3-123">パスワードに大文字が必要です。</span><span class="sxs-lookup"><span data-stu-id="a65b3-123">Requires an upper case character in the password.</span></span> | <span data-ttu-id="a65b3-124">true</span><span class="sxs-lookup"><span data-stu-id="a65b3-124">true</span></span> |
| `RequireLowercase`      | <span data-ttu-id="a65b3-125">パスワードに小文字が必要です。</span><span class="sxs-lookup"><span data-stu-id="a65b3-125">Requires a lower case character in the password.</span></span> | <span data-ttu-id="a65b3-126">true</span><span class="sxs-lookup"><span data-stu-id="a65b3-126">true</span></span> |
| `RequiredUniqueChars`   | <span data-ttu-id="a65b3-127">パスワードに個別の文字数が必要です。</span><span class="sxs-lookup"><span data-stu-id="a65b3-127">Requires the number of distinct characters in the password.</span></span> | <span data-ttu-id="a65b3-128">1</span><span class="sxs-lookup"><span data-stu-id="a65b3-128">1</span></span> |


## <a name="users-lockout"></a><span data-ttu-id="a65b3-129">ユーザーのロックアウト</span><span class="sxs-lookup"><span data-stu-id="a65b3-129">User's lockout</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

<span data-ttu-id="a65b3-130">`IdentityOptions.Lockout`次のプロパティがあります。</span><span class="sxs-lookup"><span data-stu-id="a65b3-130">`IdentityOptions.Lockout` has the following properties:</span></span>

| <span data-ttu-id="a65b3-131">プロパティ</span><span class="sxs-lookup"><span data-stu-id="a65b3-131">Property</span></span>                | <span data-ttu-id="a65b3-132">説明</span><span class="sxs-lookup"><span data-stu-id="a65b3-132">Description</span></span>                       | <span data-ttu-id="a65b3-133">既定値</span><span class="sxs-lookup"><span data-stu-id="a65b3-133">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `DefaultLockoutTimeSpan` | <span data-ttu-id="a65b3-134">時間数、ユーザーがロックアウト ロックアウトが発生するとします。</span><span class="sxs-lookup"><span data-stu-id="a65b3-134">The amount of time a user is locked out when a lockout occurs.</span></span>  | <span data-ttu-id="a65b3-135">5 分</span><span class="sxs-lookup"><span data-stu-id="a65b3-135">5 minutes</span></span>  |
| `MaxFailedAccessAttempts` | <span data-ttu-id="a65b3-136">ロックアウトが有効になっている場合、ユーザーがロックされるまで、失敗したアクセス試行の数。</span><span class="sxs-lookup"><span data-stu-id="a65b3-136">The number of failed access attempts until a user is locked out, if lockout is enabled.</span></span>  | <span data-ttu-id="a65b3-137">5</span><span class="sxs-lookup"><span data-stu-id="a65b3-137">5</span></span> |
| `AllowedForNewUsers` | <span data-ttu-id="a65b3-138">新しいユーザーをロックアウトできるかどうかを判断します。</span><span class="sxs-lookup"><span data-stu-id="a65b3-138">Determines if a new user can be locked out.</span></span>  | <span data-ttu-id="a65b3-139">true</span><span class="sxs-lookup"><span data-stu-id="a65b3-139">true</span></span> |

## <a name="sign-in-settings"></a><span data-ttu-id="a65b3-140">サインイン設定</span><span class="sxs-lookup"><span data-stu-id="a65b3-140">Sign in settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

<span data-ttu-id="a65b3-141">`IdentityOptions.SignIn`次のプロパティがあります。</span><span class="sxs-lookup"><span data-stu-id="a65b3-141">`IdentityOptions.SignIn` has the following properties:</span></span>

| <span data-ttu-id="a65b3-142">プロパティ</span><span class="sxs-lookup"><span data-stu-id="a65b3-142">Property</span></span>                | <span data-ttu-id="a65b3-143">説明</span><span class="sxs-lookup"><span data-stu-id="a65b3-143">Description</span></span>                       | <span data-ttu-id="a65b3-144">既定値</span><span class="sxs-lookup"><span data-stu-id="a65b3-144">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireConfirmedEmail` | <span data-ttu-id="a65b3-145">確認の電子メールにサインインする必要があります。</span><span class="sxs-lookup"><span data-stu-id="a65b3-145">Requires a confirmed email to sign in.</span></span> | <span data-ttu-id="a65b3-146">False</span><span class="sxs-lookup"><span data-stu-id="a65b3-146">false</span></span>  |
| `RequireConfirmedPhoneNumber` |  <span data-ttu-id="a65b3-147">確認の電話番号にサインインする必要があります。</span><span class="sxs-lookup"><span data-stu-id="a65b3-147">Requires a confirmed phone number to sign in.</span></span> | <span data-ttu-id="a65b3-148">False</span><span class="sxs-lookup"><span data-stu-id="a65b3-148">false</span></span>  |

## <a name="user-validation-settings"></a><span data-ttu-id="a65b3-149">ユーザーの検証の設定</span><span class="sxs-lookup"><span data-stu-id="a65b3-149">User validation settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

<span data-ttu-id="a65b3-150">`IdentityOptions.User`次のプロパティがあります。</span><span class="sxs-lookup"><span data-stu-id="a65b3-150">`IdentityOptions.User` has the following properties:</span></span>

| <span data-ttu-id="a65b3-151">プロパティ</span><span class="sxs-lookup"><span data-stu-id="a65b3-151">Property</span></span>                | <span data-ttu-id="a65b3-152">説明</span><span class="sxs-lookup"><span data-stu-id="a65b3-152">Description</span></span>                       | <span data-ttu-id="a65b3-153">既定値</span><span class="sxs-lookup"><span data-stu-id="a65b3-153">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireUniqueEmail`  | <span data-ttu-id="a65b3-154">各ユーザーを一意の電子メールを持っている必要があります。</span><span class="sxs-lookup"><span data-stu-id="a65b3-154">Requires each User to have a unique email.</span></span> | <span data-ttu-id="a65b3-155">False</span><span class="sxs-lookup"><span data-stu-id="a65b3-155">false</span></span>  |
| `AllowedUserNameCharacters`  | <span data-ttu-id="a65b3-156">ユーザー名に許容される文字。</span><span class="sxs-lookup"><span data-stu-id="a65b3-156">Allowed characters in the username.</span></span> | <span data-ttu-id="a65b3-157">abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+</span><span class="sxs-lookup"><span data-stu-id="a65b3-157">abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+</span></span> |


## <a name="applications-cookie-settings"></a><span data-ttu-id="a65b3-158">アプリケーションの cookie の設定</span><span class="sxs-lookup"><span data-stu-id="a65b3-158">Application's cookie settings</span></span>

<span data-ttu-id="a65b3-159">パスワード ポリシーと同様に、アプリケーションの cookie のすべての設定を変更できます、`Startup`クラスです。</span><span class="sxs-lookup"><span data-stu-id="a65b3-159">Like the passwords policy, all the settings of the application's cookie can be changed in the `Startup` class.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a65b3-160">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a65b3-160">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="a65b3-161">`ConfigureServices`で、`Startup`クラス、アプリケーションの cookie を構成することができます。</span><span class="sxs-lookup"><span data-stu-id="a65b3-161">Under `ConfigureServices` in the `Startup` class, you can configure the application's cookie.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a65b3-162">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a65b3-162">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

---

<span data-ttu-id="a65b3-163">`CookieAuthenticationOptions`次のプロパティがあります。</span><span class="sxs-lookup"><span data-stu-id="a65b3-163">`CookieAuthenticationOptions` has the following properties:</span></span>

| <span data-ttu-id="a65b3-164">プロパティ</span><span class="sxs-lookup"><span data-stu-id="a65b3-164">Property</span></span>                | <span data-ttu-id="a65b3-165">説明</span><span class="sxs-lookup"><span data-stu-id="a65b3-165">Description</span></span>                       | <span data-ttu-id="a65b3-166">既定値</span><span class="sxs-lookup"><span data-stu-id="a65b3-166">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `Cookie.Name`  | <span data-ttu-id="a65b3-167">Cookie の名前。</span><span class="sxs-lookup"><span data-stu-id="a65b3-167">The name of the cookie.</span></span>  | <span data-ttu-id="a65b3-168">.AspNetCore.Cookies.</span><span class="sxs-lookup"><span data-stu-id="a65b3-168">.AspNetCore.Cookies.</span></span>  |
| `Cookie.HttpOnly`  | <span data-ttu-id="a65b3-169">True の場合、cookie はクライアント側スクリプトからアクセスできません。</span><span class="sxs-lookup"><span data-stu-id="a65b3-169">When true, the cookie isn't accessible from client-side scripts.</span></span>  |  <span data-ttu-id="a65b3-170">true</span><span class="sxs-lookup"><span data-stu-id="a65b3-170">true</span></span> |
| `ExpireTimeSpan`  | <span data-ttu-id="a65b3-171">認証チケットがクッキーに格納されている時間は作成された時点から有効になるかを制御します。</span><span class="sxs-lookup"><span data-stu-id="a65b3-171">Controls how much time the authentication ticket stored in the cookie will remain valid from the point it's created.</span></span>  | <span data-ttu-id="a65b3-172">14 日間</span><span class="sxs-lookup"><span data-stu-id="a65b3-172">14 days</span></span>  |
| `LoginPath`  | <span data-ttu-id="a65b3-173">ユーザーが承認されていない、ときに、ログインにこのパスにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="a65b3-173">When a user is unauthorized, they will be redirected to this path to login.</span></span> | <span data-ttu-id="a65b3-174">/Account/Login</span><span class="sxs-lookup"><span data-stu-id="a65b3-174">/Account/Login</span></span>  |
| `LogoutPath`  | <span data-ttu-id="a65b3-175">ユーザーがログアウトするときは、このパスにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="a65b3-175">When a user is logged out, they will be redirected to this path.</span></span>  | <span data-ttu-id="a65b3-176">/Account/Logout</span><span class="sxs-lookup"><span data-stu-id="a65b3-176">/Account/Logout</span></span>  |
| `AccessDeniedPath`  | <span data-ttu-id="a65b3-177">ユーザーには、承認チェックが失敗した場合は、このパスにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="a65b3-177">When a user fails an authorization check, they will be redirected to this path.</span></span>  |   |
| `SlidingExpiration`  | <span data-ttu-id="a65b3-178">True の場合、現在 cookie が有効期限 ウィンドウから複数のちょうど中間にあるときに新しい有効期限時刻に新しい cookie が発行されます。</span><span class="sxs-lookup"><span data-stu-id="a65b3-178">When true, a new cookie will be issued with a new expiration time when the current cookie is more than halfway through the expiration window.</span></span>  | <span data-ttu-id="a65b3-179">/Account/AccessDenied</span><span class="sxs-lookup"><span data-stu-id="a65b3-179">/Account/AccessDenied</span></span> |
| `ReturnUrlParameter`  | <span data-ttu-id="a65b3-180">401 Unauthorized ステータス コードがログイン パスへの 302 リダイレクトに変更されたときに、ミドルウェアによって付加されるクエリ文字列パラメーターの名前を決定します。</span><span class="sxs-lookup"><span data-stu-id="a65b3-180">Determines the name of the query string parameter which is appended by the middleware when a 401 Unauthorized status code is changed to a 302 redirect onto the login path.</span></span>  |  <span data-ttu-id="a65b3-181">true</span><span class="sxs-lookup"><span data-stu-id="a65b3-181">true</span></span> |
| `AuthenticationScheme`  | <span data-ttu-id="a65b3-182">これにのみ関連 ASP.NET Core 1.x です。</span><span class="sxs-lookup"><span data-stu-id="a65b3-182">This is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="a65b3-183">特定の認証スキームの論理名。</span><span class="sxs-lookup"><span data-stu-id="a65b3-183">The logical name for a particular authentication scheme.</span></span> |  |
| `AutomaticAuthenticate`  | <span data-ttu-id="a65b3-184">このフラグにのみ関連 ASP.NET Core 1.x です。</span><span class="sxs-lookup"><span data-stu-id="a65b3-184">This flag is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="a65b3-185">True の場合、cookie 認証を要求ごとに実行しを検証し、作成された任意のシリアル化されたプリンシパルを再構築しようとします。</span><span class="sxs-lookup"><span data-stu-id="a65b3-185">When true, cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span>  |  |
