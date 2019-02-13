---
title: ASP.NET Core Identity を構成します。
author: AdrienTorris
description: ASP.NET Core Identity の既定値を理解し、カスタム値を使用する Id プロパティを構成する方法について説明します。
ms.author: riande
ms.date: 02/11/2019
uid: security/authentication/identity-configuration
ms.openlocfilehash: 3213f669cbfccdcda7cc7c0142b8101e696678e6
ms.sourcegitcommit: af8a6eb5375ef547a52ffae22465e265837aa82b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/12/2019
ms.locfileid: "56159514"
---
# <a name="configure-aspnet-core-identity"></a>ASP.NET Core Identity を構成します。

ASP.NET Core Identity では、パスワード ポリシー、によってロックアウトされた場合、cookie の構成などの設定の既定値が使用されます。 これらの設定を上書きすることができます、`Startup`クラス。

## <a name="identity-options"></a>Id オプション

[IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions)クラスは、Id システムを構成するために使用できるオプションを表します。 `IdentityOptions` 設定する必要があります**後**呼び出す`AddIdentity`または`AddDefaultIdentity`します。

### <a name="claims-identity"></a>要求の Id

[IdentityOptions.ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity)を指定します、 [ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions)次の表に示したプロパティを使用します。

| プロパティ | 説明 | 既定値 |
| -------- | ----------- | :-----: |
| [RoleClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.roleclaimtype) | 取得または使用ロール クレームのクレームの種類を設定します。 | [ClaimTypes.Role](/dotnet/api/system.security.claims.claimtypes.role) |
| [SecurityStampClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.securitystampclaimtype) | 取得または設定要求の種類のセキュリティ スタンプ要求のために使用します。 | `AspNet.Identity.SecurityStamp` |
| [UserIdClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.useridclaimtype) | 取得または使用ユーザー識別子要求の要求の種類を設定します。 | [ClaimTypes.NameIdentifier](/dotnet/api/system.security.claims.claimtypes.nameidentifier) |
| [UserNameClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.usernameclaimtype) | 取得またはユーザー名要求のために使用するクレームの種類を設定します。 | [ClaimTypes.Name](/dotnet/api/system.security.claims.claimtypes.name) |

### <a name="lockout"></a>ロックアウト

ロックアウト設定されている、 [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync#Microsoft_AspNetCore_Identity_SignInManager_1_PasswordSignInAsync_System_String_System_String_System_Boolean_System_Boolean_)メソッド。

[!code-csharp[](identity-configuration/sample/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=9)]

上記のコードがに基づいて、 `Login` Id テンプレート。 

ロックアウト オプションで設定`StartUp.ConfigureServices`:

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_lock)]

上記のコード セット、 [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions)既定値を使用します。

成功した認証は失敗したアクセス試行数がリセットされ、時計をリセットします。

[IdentityOptions.Lockout](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout)を指定します、 [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions)表に示したプロパティを使用します。

| プロパティ | 説明 | 既定値 |
| -------- | ----------- | :-----: |
| [AllowedForNewUsers](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.allowedfornewusers) | 新しいユーザーをロックアウトできるかどうかを決定します。 | `true` |
| [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan) | 時間数、ユーザーがロックアウト ロックアウトが発生した場合。 | 5 分 |
| [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) | ロックアウトが有効な場合、ユーザーがロックされるまで、失敗したアクセス試行の数。 | 5 |

### <a name="password"></a>[Password]

既定では、Id は、パスワードが、大文字、小文字、数字、および英数字以外の文字を含めることが必要です。 パスワードは、少なくとも 6 文字以上である必要があります。 [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions)で設定できる`Startup.ConfigureServices`します。

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_pw)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

::: moniker-end

[IdentityOptions.Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password)を指定します、 [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions)表に示したプロパティを使用します。

::: moniker range=">= aspnetcore-2.0"

| プロパティ | 説明 | 既定値 |
| -------- | ----------- | :-----: |
| [RequireDigit](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | パスワードには、0 ~ 9 までの数値が必要です。 | `true` |
| [RequiredLength](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | パスワードの最小長。 | 6 |
| [RequireLowercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | パスワードに小文字の文字が必要です。 | `true` |
| [RequireNonAlphanumeric](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | パスワードに英数字以外の文字が必要です。 | `true` |
| [RequiredUniqueChars](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) | ASP.NET Core 2.0 以降にのみ適用されます。<br><br> パスワードに個別の文字数が必要です。 | 1 |
| [RequireUppercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | パスワードに大文字が必要です。 | `true` |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

| プロパティ | 説明 | 既定値 |
| -------- | ----------- | :-----: |
| [RequireDigit](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | パスワードには、0 ~ 9 までの数値が必要です。 | `true` |
| [RequiredLength](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | パスワードの最小長。 | 6 |
| [RequireLowercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | パスワードに小文字の文字が必要です。 | `true` |
| [RequireNonAlphanumeric](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | パスワードに英数字以外の文字が必要です。 | `true` |
| [RequireUppercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | パスワードに大文字が必要です。 | `true` |

::: moniker-end

### <a name="sign-in"></a>サインイン

次のコード セット`SignIn`(既定値) に設定します。

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_si)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)] 

::: moniker-end

[IdentityOptions.SignIn](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin)を指定します、 [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions)表に示したプロパティを使用します。

| プロパティ | 説明 | 既定値 |
| -------- | ----------- | :-----: |
| [RequireConfirmedEmail](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedemail) | 確認の電子メールにサインインする必要があります。 | `false` |
| [RequireConfirmedPhoneNumber](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedphonenumber) | サインインの確認済みの電話番号が必要です。 | `false` |

### <a name="tokens"></a>トークン

[IdentityOptions.Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens)を指定します、 [TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions)表に示したプロパティを使用します。


|                                                        プロパティ                                                         |                                                                                      説明                                                                                      |
|-------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     [AuthenticatorTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.authenticatortokenprovider)     |                                       取得または設定します、`AuthenticatorTokenProvider`認証子とサインインを 2 つの要素を検証するために使用します。                                       |
|       [ChangeEmailTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changeemailtokenprovider)       |                                     取得または設定します、`ChangeEmailTokenProvider`電子メール変更の確認メールで使用されるトークンを生成するために使用します。                                     |
| [ChangePhoneNumberTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changephonenumbertokenprovider) |                                      取得または設定します、`ChangePhoneNumberTokenProvider`電話番号を変更するときに使用されるトークンを生成するために使用します。                                      |
| [EmailConfirmationTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.emailconfirmationtokenprovider) |                                             取得または確認メールのアカウントで使用されるトークンを生成するために使用するトークン プロバイダーを設定します。                                              |
|     [PasswordResetTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.passwordresettokenprovider)     | 取得または設定します、 [IUserTwoFactorTokenProvider<TUser> ](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1)パスワード リセット メールで使用されるトークンを生成するために使用します。 |
|                    [ProviderMap](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.providermap)                    |                構築に使用される、[ユーザー トークン プロバイダー](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor)プロバイダーの名前として使用されるキーを持つ。                 |

### <a name="user"></a>ユーザー

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_user)]

[IdentityOptions.User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user)を指定します、 [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions)表に示したプロパティを使用します。

| プロパティ | 説明 | 既定値 |
| -------- | ----------- | :-----: |
| [AllowedUserNameCharacters](/dotnet/api/microsoft.aspnetcore.identity.useroptions.allowedusernamecharacters) | ユーザー名に許可される文字。 | abcdefghijklmnopqrstuvwxyz<br>ABCDEFGHIJKLMNOPQRSTUVWXYZ<br>0123456789<br>-.\_@+ |
| [RequireUniqueEmail](/dotnet/api/microsoft.aspnetcore.identity.useroptions.requireuniqueemail) | 各ユーザーを一意の電子メールを持つ必要があります。 | `false` |

### <a name="cookie-settings"></a>Cookie の設定

内のアプリの cookie を構成する`Startup.ConfigureServices`します。 [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_ConfigureApplicationCookie_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_Cookies_CookieAuthenticationOptions__)呼び出す必要がある**後**呼び出す`AddIdentity`または`AddDefaultIdentity`します。

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_cookie)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

::: moniker-end

詳細については、次を参照してください。 [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions)します。

## <a name="password-hasher-options"></a>パスワード Hasher オプション

<xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions> 取得し、パスワードをハッシュ化するためのオプションを設定します。

| オプション | 説明 |
| ------ | ----------- |
| <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.CompatibilityMode> | 新しいパスワードをハッシュするときに使用される互換モード。 既定値は <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3> です。 呼ばれる、ハッシュされたパスワードの最初のバイトを*形式マーカー*パスワードのハッシュに使用されるハッシュ アルゴリズムのバージョンを指定します。 ハッシュ、パスワードを検証するときに、<xref:Microsoft.AspNetCore.Identity.PasswordHasher`1.VerifyHashedPassword*>メソッドは、最初のバイトに基づいて、適切なアルゴリズムを選択します。 クライアントは、パスワードのハッシュに使用されたアルゴリズムのバージョンのうちに関係なく認証できません。 互換モードを設定すると影響のハッシュ*新しいパスワード*します。 |
| <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.IterationCount> | PBKDF2 を使用してパスワードをハッシュするときに使用するイテレーションの数。 この値が使用されている場合にのみ、<xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.CompatibilityMode>に設定されている<xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3>します。 値は正の整数で、既定値でなければなりません`10000`します。 |

次の例では、<xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.IterationCount>に設定されている`12000`で`Startup.ConfigureServices`:

```csharp
// using Microsoft.AspNetCore.Identity;

services.Configure<PasswordHasherOptions>(option =>
{
    option.IterationCount = 12000;
});
```
