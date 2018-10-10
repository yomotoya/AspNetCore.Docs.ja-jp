---
title: ASP.NET Core Identity を構成します。
author: AdrienTorris
description: ASP.NET Core Identity の既定値を理解し、カスタム値を使用する Id プロパティを構成する方法について説明します。
ms.author: riande
ms.date: 08/14/2018
uid: security/authentication/identity-configuration
ms.openlocfilehash: 02441cd28c2a99eda7b50ed54f4437d4b52ca5d9
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/10/2018
ms.locfileid: "48911946"
---
# <a name="configure-aspnet-core-identity"></a><span data-ttu-id="65473-103">ASP.NET Core Identity を構成します。</span><span class="sxs-lookup"><span data-stu-id="65473-103">Configure ASP.NET Core Identity</span></span>

<span data-ttu-id="65473-104">ASP.NET Core Identity では、パスワード ポリシー、によってロックアウトされた場合、cookie の構成などの設定の既定値が使用されます。</span><span class="sxs-lookup"><span data-stu-id="65473-104">ASP.NET Core Identity uses default values for settings such as password policy, lockout, and cookie configuration.</span></span> <span data-ttu-id="65473-105">これらの設定を上書きすることができます、`Startup`クラス。</span><span class="sxs-lookup"><span data-stu-id="65473-105">These settings can be overridden in the `Startup` class.</span></span>

## <a name="identity-options"></a><span data-ttu-id="65473-106">Id オプション</span><span class="sxs-lookup"><span data-stu-id="65473-106">Identity options</span></span>

<span data-ttu-id="65473-107">[IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions)クラスは、Id システムを構成するために使用できるオプションを表します。</span><span class="sxs-lookup"><span data-stu-id="65473-107">The [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) class represents the options that can be used to configure the Identity system.</span></span> <span data-ttu-id="65473-108">`IdentityOptions` 設定する必要があります**後**呼び出す`AddIdentity`または`AddDefaultIdentity`します。</span><span class="sxs-lookup"><span data-stu-id="65473-108">`IdentityOptions` must be set **after** calling `AddIdentity` or `AddDefaultIdentity`.</span></span>

### <a name="claims-identity"></a><span data-ttu-id="65473-109">要求の Id</span><span class="sxs-lookup"><span data-stu-id="65473-109">Claims Identity</span></span>

<span data-ttu-id="65473-110">[IdentityOptions.ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity)を指定します、 [ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions)次の表に示したプロパティを使用します。</span><span class="sxs-lookup"><span data-stu-id="65473-110">[IdentityOptions.ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity) specifies the [ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions) with the properties shown in the following table.</span></span>

| <span data-ttu-id="65473-111">プロパティ</span><span class="sxs-lookup"><span data-stu-id="65473-111">Property</span></span> | <span data-ttu-id="65473-112">説明</span><span class="sxs-lookup"><span data-stu-id="65473-112">Description</span></span> | <span data-ttu-id="65473-113">既定値</span><span class="sxs-lookup"><span data-stu-id="65473-113">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="65473-114">RoleClaimType</span><span class="sxs-lookup"><span data-stu-id="65473-114">RoleClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.roleclaimtype) | <span data-ttu-id="65473-115">取得または使用ロール クレームのクレームの種類を設定します。</span><span class="sxs-lookup"><span data-stu-id="65473-115">Gets or sets the claim type used for a role claim.</span></span> | [<span data-ttu-id="65473-116">ClaimTypes.Role</span><span class="sxs-lookup"><span data-stu-id="65473-116">ClaimTypes.Role</span></span>](/dotnet/api/system.security.claims.claimtypes.role) |
| [<span data-ttu-id="65473-117">SecurityStampClaimType</span><span class="sxs-lookup"><span data-stu-id="65473-117">SecurityStampClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.securitystampclaimtype) | <span data-ttu-id="65473-118">取得または設定要求の種類のセキュリティ スタンプ要求のために使用します。</span><span class="sxs-lookup"><span data-stu-id="65473-118">Gets or sets the claim type used for the security stamp claim.</span></span> | `AspNet.Identity.SecurityStamp` |
| [<span data-ttu-id="65473-119">UserIdClaimType</span><span class="sxs-lookup"><span data-stu-id="65473-119">UserIdClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.useridclaimtype) | <span data-ttu-id="65473-120">取得または使用ユーザー識別子要求の要求の種類を設定します。</span><span class="sxs-lookup"><span data-stu-id="65473-120">Gets or sets the claim type used for the user identifier claim.</span></span> | [<span data-ttu-id="65473-121">ClaimTypes.NameIdentifier</span><span class="sxs-lookup"><span data-stu-id="65473-121">ClaimTypes.NameIdentifier</span></span>](/dotnet/api/system.security.claims.claimtypes.nameidentifier) |
| [<span data-ttu-id="65473-122">UserNameClaimType</span><span class="sxs-lookup"><span data-stu-id="65473-122">UserNameClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.usernameclaimtype) | <span data-ttu-id="65473-123">取得またはユーザー名要求のために使用するクレームの種類を設定します。</span><span class="sxs-lookup"><span data-stu-id="65473-123">Gets or sets the claim type used for the user name claim.</span></span> | [<span data-ttu-id="65473-124">ClaimTypes.Name</span><span class="sxs-lookup"><span data-stu-id="65473-124">ClaimTypes.Name</span></span>](/dotnet/api/system.security.claims.claimtypes.name) |

### <a name="lockout"></a><span data-ttu-id="65473-125">ロックアウト</span><span class="sxs-lookup"><span data-stu-id="65473-125">Lockout</span></span>

<span data-ttu-id="65473-126">ロックアウト設定されている、 [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync#Microsoft_AspNetCore_Identity_SignInManager_1_PasswordSignInAsync_System_String_System_String_System_Boolean_System_Boolean_)メソッド。</span><span class="sxs-lookup"><span data-stu-id="65473-126">Lockout is set in the [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync#Microsoft_AspNetCore_Identity_SignInManager_1_PasswordSignInAsync_System_String_System_String_System_Boolean_System_Boolean_) method:</span></span>

[!code-csharp[](identity-configuration/sample/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=9)]

<span data-ttu-id="65473-127">上記のコードがに基づいて、 `Login` Id テンプレート。</span><span class="sxs-lookup"><span data-stu-id="65473-127">The preceding code is based on the `Login` Identity template.</span></span> 

<span data-ttu-id="65473-128">ロックアウト オプションで設定`StartUp.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="65473-128">Lockout options are set in `StartUp.ConfigureServices`:</span></span>

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_lock)]

<span data-ttu-id="65473-129">上記のコード セット、 [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions)既定値を使用します。</span><span class="sxs-lookup"><span data-stu-id="65473-129">The preceding code sets the [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) with default values.</span></span>

<span data-ttu-id="65473-130">成功した認証は失敗したアクセス試行数がリセットされ、時計をリセットします。</span><span class="sxs-lookup"><span data-stu-id="65473-130">A successful authentication resets the failed access attempts count and resets the clock.</span></span>

<span data-ttu-id="65473-131">[IdentityOptions.Lockout](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout)を指定します、 [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions)表に示したプロパティを使用します。</span><span class="sxs-lookup"><span data-stu-id="65473-131">[IdentityOptions.Lockout](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout) specifies the [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="65473-132">プロパティ</span><span class="sxs-lookup"><span data-stu-id="65473-132">Property</span></span> | <span data-ttu-id="65473-133">説明</span><span class="sxs-lookup"><span data-stu-id="65473-133">Description</span></span> | <span data-ttu-id="65473-134">既定値</span><span class="sxs-lookup"><span data-stu-id="65473-134">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="65473-135">AllowedForNewUsers</span><span class="sxs-lookup"><span data-stu-id="65473-135">AllowedForNewUsers</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.allowedfornewusers) | <span data-ttu-id="65473-136">新しいユーザーをロックアウトできるかどうかを決定します。</span><span class="sxs-lookup"><span data-stu-id="65473-136">Determines if a new user can be locked out.</span></span> | `true` |
| [<span data-ttu-id="65473-137">DefaultLockoutTimeSpan</span><span class="sxs-lookup"><span data-stu-id="65473-137">DefaultLockoutTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan) | <span data-ttu-id="65473-138">時間数、ユーザーがロックアウト ロックアウトが発生した場合。</span><span class="sxs-lookup"><span data-stu-id="65473-138">The amount of time a user is locked out when a lockout occurs.</span></span> | <span data-ttu-id="65473-139">5 分</span><span class="sxs-lookup"><span data-stu-id="65473-139">5 minutes</span></span> |
| [<span data-ttu-id="65473-140">MaxFailedAccessAttempts</span><span class="sxs-lookup"><span data-stu-id="65473-140">MaxFailedAccessAttempts</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) | <span data-ttu-id="65473-141">ロックアウトが有効な場合、ユーザーがロックされるまで、失敗したアクセス試行の数。</span><span class="sxs-lookup"><span data-stu-id="65473-141">The number of failed access attempts until a user is locked out, if lockout is enabled.</span></span> | <span data-ttu-id="65473-142">5</span><span class="sxs-lookup"><span data-stu-id="65473-142">5</span></span> |

### <a name="password"></a><span data-ttu-id="65473-143">[Password]</span><span class="sxs-lookup"><span data-stu-id="65473-143">Password</span></span>

<span data-ttu-id="65473-144">既定では、Id は、パスワードが、大文字、小文字、数字、および英数字以外の文字を含めることが必要です。</span><span class="sxs-lookup"><span data-stu-id="65473-144">By default, Identity requires that passwords contain an uppercase character, lowercase character, a digit, and a non-alphanumeric character.</span></span> <span data-ttu-id="65473-145">パスワードは、少なくとも 6 文字以上である必要があります。</span><span class="sxs-lookup"><span data-stu-id="65473-145">Passwords must be at least six characters long.</span></span> <span data-ttu-id="65473-146">[PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions)で設定できる`Startup.ConfigureServices`します。</span><span class="sxs-lookup"><span data-stu-id="65473-146">[PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) can be set in `Startup.ConfigureServices`.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_pw)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

::: moniker-end

<span data-ttu-id="65473-147">[IdentityOptions.Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password)を指定します、 [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions)表に示したプロパティを使用します。</span><span class="sxs-lookup"><span data-stu-id="65473-147">[IdentityOptions.Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password) specifies the [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) with the properties shown in the table.</span></span>

::: moniker range=">= aspnetcore-2.0"

| <span data-ttu-id="65473-148">プロパティ</span><span class="sxs-lookup"><span data-stu-id="65473-148">Property</span></span> | <span data-ttu-id="65473-149">説明</span><span class="sxs-lookup"><span data-stu-id="65473-149">Description</span></span> | <span data-ttu-id="65473-150">既定値</span><span class="sxs-lookup"><span data-stu-id="65473-150">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="65473-151">RequireDigit</span><span class="sxs-lookup"><span data-stu-id="65473-151">RequireDigit</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | <span data-ttu-id="65473-152">パスワードには、0 ~ 9 までの数値が必要です。</span><span class="sxs-lookup"><span data-stu-id="65473-152">Requires a number between 0-9 in the password.</span></span> | `true` |
| [<span data-ttu-id="65473-153">RequiredLength</span><span class="sxs-lookup"><span data-stu-id="65473-153">RequiredLength</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | <span data-ttu-id="65473-154">パスワードの最小長。</span><span class="sxs-lookup"><span data-stu-id="65473-154">The minimum length of the password.</span></span> | <span data-ttu-id="65473-155">6</span><span class="sxs-lookup"><span data-stu-id="65473-155">6</span></span> |
| [<span data-ttu-id="65473-156">RequireLowercase</span><span class="sxs-lookup"><span data-stu-id="65473-156">RequireLowercase</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | <span data-ttu-id="65473-157">パスワードに小文字の文字が必要です。</span><span class="sxs-lookup"><span data-stu-id="65473-157">Requires a lowercase character in the password.</span></span> | `true` |
| [<span data-ttu-id="65473-158">RequireNonAlphanumeric</span><span class="sxs-lookup"><span data-stu-id="65473-158">RequireNonAlphanumeric</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | <span data-ttu-id="65473-159">パスワードに英数字以外の文字が必要です。</span><span class="sxs-lookup"><span data-stu-id="65473-159">Requires a non-alphanumeric character in the password.</span></span> | `true` |
| [<span data-ttu-id="65473-160">RequiredUniqueChars</span><span class="sxs-lookup"><span data-stu-id="65473-160">RequiredUniqueChars</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) | <span data-ttu-id="65473-161">ASP.NET Core 2.0 以降にのみ適用されます。</span><span class="sxs-lookup"><span data-stu-id="65473-161">Only applies to ASP.NET Core 2.0 or later.</span></span><br><br> <span data-ttu-id="65473-162">パスワードに個別の文字数が必要です。</span><span class="sxs-lookup"><span data-stu-id="65473-162">Requires the number of distinct characters in the password.</span></span> | <span data-ttu-id="65473-163">1</span><span class="sxs-lookup"><span data-stu-id="65473-163">1</span></span> |
| [<span data-ttu-id="65473-164">RequireUppercase</span><span class="sxs-lookup"><span data-stu-id="65473-164">RequireUppercase</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | <span data-ttu-id="65473-165">パスワードに大文字が必要です。</span><span class="sxs-lookup"><span data-stu-id="65473-165">Requires an uppercase character in the password.</span></span> | `true` |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

| <span data-ttu-id="65473-166">プロパティ</span><span class="sxs-lookup"><span data-stu-id="65473-166">Property</span></span> | <span data-ttu-id="65473-167">説明</span><span class="sxs-lookup"><span data-stu-id="65473-167">Description</span></span> | <span data-ttu-id="65473-168">既定値</span><span class="sxs-lookup"><span data-stu-id="65473-168">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="65473-169">RequireDigit</span><span class="sxs-lookup"><span data-stu-id="65473-169">RequireDigit</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | <span data-ttu-id="65473-170">パスワードには、0 ~ 9 までの数値が必要です。</span><span class="sxs-lookup"><span data-stu-id="65473-170">Requires a number between 0-9 in the password.</span></span> | `true` |
| [<span data-ttu-id="65473-171">RequiredLength</span><span class="sxs-lookup"><span data-stu-id="65473-171">RequiredLength</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | <span data-ttu-id="65473-172">パスワードの最小長。</span><span class="sxs-lookup"><span data-stu-id="65473-172">The minimum length of the password.</span></span> | <span data-ttu-id="65473-173">6</span><span class="sxs-lookup"><span data-stu-id="65473-173">6</span></span> |
| [<span data-ttu-id="65473-174">RequireLowercase</span><span class="sxs-lookup"><span data-stu-id="65473-174">RequireLowercase</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | <span data-ttu-id="65473-175">パスワードに小文字の文字が必要です。</span><span class="sxs-lookup"><span data-stu-id="65473-175">Requires a lowercase character in the password.</span></span> | `true` |
| [<span data-ttu-id="65473-176">RequireNonAlphanumeric</span><span class="sxs-lookup"><span data-stu-id="65473-176">RequireNonAlphanumeric</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | <span data-ttu-id="65473-177">パスワードに英数字以外の文字が必要です。</span><span class="sxs-lookup"><span data-stu-id="65473-177">Requires a non-alphanumeric character in the password.</span></span> | `true` |
| [<span data-ttu-id="65473-178">RequireUppercase</span><span class="sxs-lookup"><span data-stu-id="65473-178">RequireUppercase</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | <span data-ttu-id="65473-179">パスワードに大文字が必要です。</span><span class="sxs-lookup"><span data-stu-id="65473-179">Requires an uppercase character in the password.</span></span> | `true` |

::: moniker-end

### <a name="sign-in"></a><span data-ttu-id="65473-180">サインイン</span><span class="sxs-lookup"><span data-stu-id="65473-180">Sign-in</span></span>

<span data-ttu-id="65473-181">次のコード セット`SignIn`(既定値) に設定します。</span><span class="sxs-lookup"><span data-stu-id="65473-181">The following code sets `SignIn` settings (to default values):</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_si)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)] 

::: moniker-end

<span data-ttu-id="65473-182">[IdentityOptions.SignIn](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin)を指定します、 [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions)表に示したプロパティを使用します。</span><span class="sxs-lookup"><span data-stu-id="65473-182">[IdentityOptions.SignIn](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin) specifies the [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="65473-183">プロパティ</span><span class="sxs-lookup"><span data-stu-id="65473-183">Property</span></span> | <span data-ttu-id="65473-184">説明</span><span class="sxs-lookup"><span data-stu-id="65473-184">Description</span></span> | <span data-ttu-id="65473-185">既定値</span><span class="sxs-lookup"><span data-stu-id="65473-185">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="65473-186">RequireConfirmedEmail</span><span class="sxs-lookup"><span data-stu-id="65473-186">RequireConfirmedEmail</span></span>](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedemail) | <span data-ttu-id="65473-187">確認の電子メールにサインインする必要があります。</span><span class="sxs-lookup"><span data-stu-id="65473-187">Requires a confirmed email to sign in.</span></span> | `false` |
| [<span data-ttu-id="65473-188">RequireConfirmedPhoneNumber</span><span class="sxs-lookup"><span data-stu-id="65473-188">RequireConfirmedPhoneNumber</span></span>](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedphonenumber) | <span data-ttu-id="65473-189">サインインの確認済みの電話番号が必要です。</span><span class="sxs-lookup"><span data-stu-id="65473-189">Requires a confirmed phone number to sign in.</span></span> | `false` |

### <a name="tokens"></a><span data-ttu-id="65473-190">トークン</span><span class="sxs-lookup"><span data-stu-id="65473-190">Tokens</span></span>

<span data-ttu-id="65473-191">[IdentityOptions.Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens)を指定します、 [TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions)表に示したプロパティを使用します。</span><span class="sxs-lookup"><span data-stu-id="65473-191">[IdentityOptions.Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens) specifies the [TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) with the properties shown in the table.</span></span>


|                                                        <span data-ttu-id="65473-192">プロパティ</span><span class="sxs-lookup"><span data-stu-id="65473-192">Property</span></span>                                                         |                                                                                      <span data-ttu-id="65473-193">説明</span><span class="sxs-lookup"><span data-stu-id="65473-193">Description</span></span>                                                                                      |
|-------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     [<span data-ttu-id="65473-194">AuthenticatorTokenProvider</span><span class="sxs-lookup"><span data-stu-id="65473-194">AuthenticatorTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.authenticatortokenprovider)     |                                       <span data-ttu-id="65473-195">取得または設定します、`AuthenticatorTokenProvider`認証子とサインインを 2 つの要素を検証するために使用します。</span><span class="sxs-lookup"><span data-stu-id="65473-195">Gets or sets the `AuthenticatorTokenProvider` used to validate two-factor sign-ins with an authenticator.</span></span>                                       |
|       [<span data-ttu-id="65473-196">ChangeEmailTokenProvider</span><span class="sxs-lookup"><span data-stu-id="65473-196">ChangeEmailTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changeemailtokenprovider)       |                                     <span data-ttu-id="65473-197">取得または設定します、`ChangeEmailTokenProvider`電子メール変更の確認メールで使用されるトークンを生成するために使用します。</span><span class="sxs-lookup"><span data-stu-id="65473-197">Gets or sets the `ChangeEmailTokenProvider` used to generate tokens used in email change confirmation emails.</span></span>                                     |
| [<span data-ttu-id="65473-198">ChangePhoneNumberTokenProvider</span><span class="sxs-lookup"><span data-stu-id="65473-198">ChangePhoneNumberTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changephonenumbertokenprovider) |                                      <span data-ttu-id="65473-199">取得または設定します、`ChangePhoneNumberTokenProvider`電話番号を変更するときに使用されるトークンを生成するために使用します。</span><span class="sxs-lookup"><span data-stu-id="65473-199">Gets or sets the `ChangePhoneNumberTokenProvider` used to generate tokens used when changing phone numbers.</span></span>                                      |
| [<span data-ttu-id="65473-200">EmailConfirmationTokenProvider</span><span class="sxs-lookup"><span data-stu-id="65473-200">EmailConfirmationTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.emailconfirmationtokenprovider) |                                             <span data-ttu-id="65473-201">取得または確認メールのアカウントで使用されるトークンを生成するために使用するトークン プロバイダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="65473-201">Gets or sets the token provider used to generate tokens used in account confirmation emails.</span></span>                                              |
|     [<span data-ttu-id="65473-202">PasswordResetTokenProvider</span><span class="sxs-lookup"><span data-stu-id="65473-202">PasswordResetTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.passwordresettokenprovider)     | <span data-ttu-id="65473-203">取得または設定します、 [IUserTwoFactorTokenProvider<TUser> ](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1)パスワード リセット メールで使用されるトークンを生成するために使用します。</span><span class="sxs-lookup"><span data-stu-id="65473-203">Gets or sets the [IUserTwoFactorTokenProvider<TUser>](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1) used to generate tokens used in password reset emails.</span></span> |
|                    [<span data-ttu-id="65473-204">ProviderMap</span><span class="sxs-lookup"><span data-stu-id="65473-204">ProviderMap</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.providermap)                    |                <span data-ttu-id="65473-205">構築に使用される、[ユーザー トークン プロバイダー](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor)プロバイダーの名前として使用されるキーを持つ。</span><span class="sxs-lookup"><span data-stu-id="65473-205">Used to construct a [User Token Provider](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor) with the key used as the provider's name.</span></span>                 |

### <a name="user"></a><span data-ttu-id="65473-206">ユーザー</span><span class="sxs-lookup"><span data-stu-id="65473-206">User</span></span>

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_user)]

<span data-ttu-id="65473-207">[IdentityOptions.User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user)を指定します、 [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions)表に示したプロパティを使用します。</span><span class="sxs-lookup"><span data-stu-id="65473-207">[IdentityOptions.User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user) specifies the [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="65473-208">プロパティ</span><span class="sxs-lookup"><span data-stu-id="65473-208">Property</span></span> | <span data-ttu-id="65473-209">説明</span><span class="sxs-lookup"><span data-stu-id="65473-209">Description</span></span> | <span data-ttu-id="65473-210">既定値</span><span class="sxs-lookup"><span data-stu-id="65473-210">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="65473-211">AllowedUserNameCharacters</span><span class="sxs-lookup"><span data-stu-id="65473-211">AllowedUserNameCharacters</span></span>](/dotnet/api/microsoft.aspnetcore.identity.useroptions.allowedusernamecharacters) | <span data-ttu-id="65473-212">ユーザー名に許可される文字。</span><span class="sxs-lookup"><span data-stu-id="65473-212">Allowed characters in the username.</span></span> | <span data-ttu-id="65473-213">abcdefghijklmnopqrstuvwxyz</span><span class="sxs-lookup"><span data-stu-id="65473-213">abcdefghijklmnopqrstuvwxyz</span></span><br><span data-ttu-id="65473-214">ABCDEFGHIJKLMNOPQRSTUVWXYZ</span><span class="sxs-lookup"><span data-stu-id="65473-214">ABCDEFGHIJKLMNOPQRSTUVWXYZ</span></span><br><span data-ttu-id="65473-215">0123456789</span><span class="sxs-lookup"><span data-stu-id="65473-215">0123456789</span></span><br><span data-ttu-id="65473-216">-.\_@+</span><span class="sxs-lookup"><span data-stu-id="65473-216">-.\_@+</span></span> |
| [<span data-ttu-id="65473-217">RequireUniqueEmail</span><span class="sxs-lookup"><span data-stu-id="65473-217">RequireUniqueEmail</span></span>](/dotnet/api/microsoft.aspnetcore.identity.useroptions.requireuniqueemail) | <span data-ttu-id="65473-218">各ユーザーを一意の電子メールを持つ必要があります。</span><span class="sxs-lookup"><span data-stu-id="65473-218">Requires each user to have a unique email.</span></span> | `false` |

### <a name="cookie-settings"></a><span data-ttu-id="65473-219">Cookie の設定</span><span class="sxs-lookup"><span data-stu-id="65473-219">Cookie settings</span></span>

<span data-ttu-id="65473-220">内のアプリの cookie を構成する`Startup.ConfigureServices`します。</span><span class="sxs-lookup"><span data-stu-id="65473-220">Configure the app's cookie in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="65473-221">[ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_ConfigureApplicationCookie_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_Cookies_CookieAuthenticationOptions__)呼び出す必要がある**後**呼び出す`AddIdentity`または`AddDefaultIdentity`します。</span><span class="sxs-lookup"><span data-stu-id="65473-221">[ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_ConfigureApplicationCookie_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_Cookies_CookieAuthenticationOptions__) must be called **after** calling `AddIdentity` or `AddDefaultIdentity`.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_cookie)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

::: moniker-end

<span data-ttu-id="65473-222">詳細については、次を参照してください。 [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions)します。</span><span class="sxs-lookup"><span data-stu-id="65473-222">For more information, see [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions).</span></span>
