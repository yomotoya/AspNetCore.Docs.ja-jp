---
title: その他の要求と ASP.NET Core での外部プロバイダーからのトークンを保存します。
author: guardrex
description: その他の要求と外部プロバイダーからトークンを確立する方法について説明します。
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/28/2018
uid: security/authentication/social/additional-claims
ms.openlocfilehash: dc8b3e32141466a12e4eff0c86d2d4bed689afe5
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206358"
---
# <a name="persist-additional-claims-and-tokens-from-external-providers-in-aspnet-core"></a><span data-ttu-id="e6cc2-103">その他の要求と ASP.NET Core での外部プロバイダーからのトークンを保存します。</span><span class="sxs-lookup"><span data-stu-id="e6cc2-103">Persist additional claims and tokens from external providers in ASP.NET Core</span></span>

<span data-ttu-id="e6cc2-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e6cc2-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="e6cc2-105">ASP.NET Core アプリは、追加の要求および Facebook、Google、Microsoft、Twitter などの外部認証プロバイダーからのトークンを確立できます。</span><span class="sxs-lookup"><span data-stu-id="e6cc2-105">An ASP.NET Core app can establish additional claims and tokens from external authentication providers, such as Facebook, Google, Microsoft, and Twitter.</span></span> <span data-ttu-id="e6cc2-106">各プロバイダーが、そのプラットフォームでユーザーに関するさまざまな情報が表示されますが、受信し、その他の要求にユーザー データを変換するためのパターンは同じです。</span><span class="sxs-lookup"><span data-stu-id="e6cc2-106">Each provider reveals different information about users on its platform, but the pattern for receiving and transforming user data into additional claims is the same.</span></span>

<span data-ttu-id="e6cc2-107">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="e6cc2-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisite"></a><span data-ttu-id="e6cc2-108">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="e6cc2-108">Prerequisite</span></span>

<span data-ttu-id="e6cc2-109">アプリでサポートするには、どの外部認証プロバイダーを決定します。</span><span class="sxs-lookup"><span data-stu-id="e6cc2-109">Decide which external authentication providers to support in the app.</span></span> <span data-ttu-id="e6cc2-110">プロバイダーごとにアプリを登録して、クライアント ID とクライアント シークレットを取得します。</span><span class="sxs-lookup"><span data-stu-id="e6cc2-110">For each provider, register the app and obtain a client ID and client secret.</span></span> <span data-ttu-id="e6cc2-111">詳細については、「 <xref:security/authentication/social/index> 」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e6cc2-111">For more information, see <xref:security/authentication/social/index>.</span></span> <span data-ttu-id="e6cc2-112">[サンプル アプリ](#sample-app-instructions)を使用して、 [Google の認証プロバイダー](xref:security/authentication/google-logins)します。</span><span class="sxs-lookup"><span data-stu-id="e6cc2-112">The [sample app](#sample-app-instructions) uses the [Google authentication provider](xref:security/authentication/google-logins).</span></span>

## <a name="authentication-provider-configuration"></a><span data-ttu-id="e6cc2-113">認証プロバイダーの構成</span><span class="sxs-lookup"><span data-stu-id="e6cc2-113">Authentication provider configuration</span></span>

### <a name="set-the-client-id-and-client-secret"></a><span data-ttu-id="e6cc2-114">クライアント ID とクライアント シークレットを設定します。</span><span class="sxs-lookup"><span data-stu-id="e6cc2-114">Set the client ID and client secret</span></span>

<span data-ttu-id="e6cc2-115">OAuth 認証プロバイダーは、クライアント ID とクライアント シークレットを使用するアプリとの信頼関係を確立します。</span><span class="sxs-lookup"><span data-stu-id="e6cc2-115">The OAuth authentication provider establishes a trust relationship with an app using a client ID and client secret.</span></span> <span data-ttu-id="e6cc2-116">クライアント ID とクライアント シークレットの値はアプリ用に作成、外部認証プロバイダーによって、アプリが、プロバイダーに登録されている場合。</span><span class="sxs-lookup"><span data-stu-id="e6cc2-116">Client ID and client secret values are created for the app by the external authentication provider when the app is registered with the provider.</span></span> <span data-ttu-id="e6cc2-117">各外部プロバイダー、アプリが使用するは、プロバイダーのクライアント ID とクライアント シークレット個別に構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e6cc2-117">Each external provider that the app uses must be configured independently with the provider's client ID and client secret.</span></span> <span data-ttu-id="e6cc2-118">詳細については、実際のシナリオに適用される、外部認証プロバイダーについてのトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="e6cc2-118">For more information, see the external authentication provider topics that apply to your scenario:</span></span>

* [<span data-ttu-id="e6cc2-119">Facebook での認証</span><span class="sxs-lookup"><span data-stu-id="e6cc2-119">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="e6cc2-120">Google での認証</span><span class="sxs-lookup"><span data-stu-id="e6cc2-120">Google authentication</span></span>](xref:security/authentication/google-logins)
* [<span data-ttu-id="e6cc2-121">Microsoft での認証</span><span class="sxs-lookup"><span data-stu-id="e6cc2-121">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="e6cc2-122">Twitter での認証</span><span class="sxs-lookup"><span data-stu-id="e6cc2-122">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="e6cc2-123">他の認証プロバイダー</span><span class="sxs-lookup"><span data-stu-id="e6cc2-123">Other authentication providers</span></span>](xref:security/authentication/otherlogins)
* [<span data-ttu-id="e6cc2-124">OpenIdConnect</span><span class="sxs-lookup"><span data-stu-id="e6cc2-124">OpenIdConnect</span></span>](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

<span data-ttu-id="e6cc2-125">サンプル アプリは、クライアント ID とクライアント シークレットが Google から提供されると、Google の認証プロバイダーを構成します。</span><span class="sxs-lookup"><span data-stu-id="e6cc2-125">The sample app configures the Google authentication provider with a client ID and client secret provided by Google:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,6)]

### <a name="establish-the-authentication-scope"></a><span data-ttu-id="e6cc2-126">認証のスコープを確立します。</span><span class="sxs-lookup"><span data-stu-id="e6cc2-126">Establish the authentication scope</span></span>

<span data-ttu-id="e6cc2-127">指定することで、プロバイダーから取得するアクセス許可の一覧を指定、<xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>します。</span><span class="sxs-lookup"><span data-stu-id="e6cc2-127">Specify the list of permissions to retrieve from the provider by specifying the <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span></span> <span data-ttu-id="e6cc2-128">一般的な外部プロバイダーの認証のスコープは、次の表に表示されます。</span><span class="sxs-lookup"><span data-stu-id="e6cc2-128">Authentication scopes for common external providers appear in the following table.</span></span>

| <span data-ttu-id="e6cc2-129">プロバイダー</span><span class="sxs-lookup"><span data-stu-id="e6cc2-129">Provider</span></span>  | <span data-ttu-id="e6cc2-130">スコープ</span><span class="sxs-lookup"><span data-stu-id="e6cc2-130">Scope</span></span>                                                            |
| --------- | ---------------------------------------------------------------- |
| <span data-ttu-id="e6cc2-131">Facebook</span><span class="sxs-lookup"><span data-stu-id="e6cc2-131">Facebook</span></span>  | `https://www.facebook.com/dialog/oauth`                          |
| <span data-ttu-id="e6cc2-132">Google</span><span class="sxs-lookup"><span data-stu-id="e6cc2-132">Google</span></span>    | `https://www.googleapis.com/auth/plus.login`                     |
| <span data-ttu-id="e6cc2-133">Microsoft</span><span class="sxs-lookup"><span data-stu-id="e6cc2-133">Microsoft</span></span> | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| <span data-ttu-id="e6cc2-134">Twitter</span><span class="sxs-lookup"><span data-stu-id="e6cc2-134">Twitter</span></span>   | `https://api.twitter.com/oauth/authenticate`                     |

<span data-ttu-id="e6cc2-135">サンプル アプリの追加、Google`plus.login`スコープ アクセス許可で Google + 記号を要求します。</span><span class="sxs-lookup"><span data-stu-id="e6cc2-135">The sample app adds the Google `plus.login` scope to request Google+ sign in permissions:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=7)]

### <a name="map-user-data-keys-and-create-claims"></a><span data-ttu-id="e6cc2-136">ユーザー データのキーをマップし、要求の作成</span><span class="sxs-lookup"><span data-stu-id="e6cc2-136">Map user data keys and create claims</span></span>

<span data-ttu-id="e6cc2-137">プロバイダーのオプションでは、指定、<xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*>外部プロバイダーのユーザー データを JSON のサインイン時の読み取りをアプリ id を内の各キー。</span><span class="sxs-lookup"><span data-stu-id="e6cc2-137">In the provider's options, specify a <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> for each key in the external provider's JSON user data for the app identity to read on sign in.</span></span> <span data-ttu-id="e6cc2-138">要求の種類の詳細については、次を参照してください。<xref:System.Security.Claims.ClaimTypes>します。</span><span class="sxs-lookup"><span data-stu-id="e6cc2-138">For more information on claim types, see <xref:System.Security.Claims.ClaimTypes>.</span></span>

<span data-ttu-id="e6cc2-139">サンプル アプリを作成、<xref:System.Security.Claims.ClaimTypes.Gender>からの要求を`gender`Google ユーザーのデータ キー。</span><span class="sxs-lookup"><span data-stu-id="e6cc2-139">The sample app creates a <xref:System.Security.Claims.ClaimTypes.Gender> claim from the `gender` key in Google user data:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=8)]

<span data-ttu-id="e6cc2-140"><xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>、 <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) が使用して、アプリにサインイン<xref:Microsoft.AspNetCore.Identity.SignInManager`1.SignInAsync*>します。</span><span class="sxs-lookup"><span data-stu-id="e6cc2-140">In <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>, an <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) is signed into the app with <xref:Microsoft.AspNetCore.Identity.SignInManager`1.SignInAsync*>.</span></span> <span data-ttu-id="e6cc2-141">プロセスでは、サインアップ時に、<xref:Microsoft.AspNetCore.Identity.UserManager`1>格納できる、`ApplicationUser`から使用可能なユーザー データの要求、<xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>します。</span><span class="sxs-lookup"><span data-stu-id="e6cc2-141">During the sign in process, the <xref:Microsoft.AspNetCore.Identity.UserManager`1> can store an `ApplicationUser` claim for user data available from the <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span></span>

<span data-ttu-id="e6cc2-142">サンプル アプリで`OnPostConfirmationAsync`(*Account/ExternalLogin.cshtml.cs*) を確立、 <xref:System.Security.Claims.ClaimTypes.Gender> 、符号付きの要求で`ApplicationUser`:</span><span class="sxs-lookup"><span data-stu-id="e6cc2-142">In the sample app, `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) establishes a <xref:System.Security.Claims.ClaimTypes.Gender> claim for the signed in `ApplicationUser`:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=30-31)]

### <a name="save-the-access-token"></a><span data-ttu-id="e6cc2-143">アクセス トークンを保存します。</span><span class="sxs-lookup"><span data-stu-id="e6cc2-143">Save the access token</span></span>

<span data-ttu-id="e6cc2-144"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> アクセスと更新トークンを保存するかどうかを定義、<xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties>成功した認証が完了後します。</span><span class="sxs-lookup"><span data-stu-id="e6cc2-144"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> defines whether access and refresh tokens should be stored in the <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> after a successful authorization.</span></span> <span data-ttu-id="e6cc2-145">`SaveTokens` 設定されている`false`既定では、最終的な認証 cookie のサイズを小さくします。</span><span class="sxs-lookup"><span data-stu-id="e6cc2-145">`SaveTokens` is set to `false` by default to reduce the size of the final authentication cookie.</span></span>

<span data-ttu-id="e6cc2-146">サンプル アプリの値を設定する`SaveTokens`に`true`で<xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span><span class="sxs-lookup"><span data-stu-id="e6cc2-146">The sample app sets the value of `SaveTokens` to `true` in <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=9)]

<span data-ttu-id="e6cc2-147">ときに`OnPostConfirmationAsync`を実行するアクセス トークンを保存 ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) で外部プロバイダーから、`ApplicationUser`の`AuthenticationProperties`します。</span><span class="sxs-lookup"><span data-stu-id="e6cc2-147">When `OnPostConfirmationAsync` executes, store the access token ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) from the external provider in the `ApplicationUser`'s `AuthenticationProperties`.</span></span>

<span data-ttu-id="e6cc2-148">サンプル アプリでアクセス トークンを保存します。</span><span class="sxs-lookup"><span data-stu-id="e6cc2-148">The sample app saves the access token in:</span></span>

* <span data-ttu-id="e6cc2-149">`OnPostConfirmationAsync` &ndash; 新規ユーザー登録を実行します。</span><span class="sxs-lookup"><span data-stu-id="e6cc2-149">`OnPostConfirmationAsync` &ndash; Executes for new user registration.</span></span>
* <span data-ttu-id="e6cc2-150">`OnGetCallbackAsync` &ndash; 以前に登録されたユーザーがアプリにサインインを実行します。</span><span class="sxs-lookup"><span data-stu-id="e6cc2-150">`OnGetCallbackAsync` &ndash; Executes when a previously registered user signs into the app.</span></span>

<span data-ttu-id="e6cc2-151">*Account/ExternalLogin.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="e6cc2-151">*Account/ExternalLogin.cshtml.cs*:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=34-35)]

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnGetCallbackAsync&highlight=31-32)]

### <a name="how-to-add-additional-custom-tokens"></a><span data-ttu-id="e6cc2-152">追加のカスタム トークンを追加する方法</span><span class="sxs-lookup"><span data-stu-id="e6cc2-152">How to add additional custom tokens</span></span>

<span data-ttu-id="e6cc2-153">一部として格納されているカスタムのトークンを追加する方法を説明するために`SaveTokens`、サンプル アプリを追加、<xref:Microsoft.AspNetCore.Authentication.AuthenticationToken>を現在<xref:System.DateTime>の[AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*)の`TicketCreated`:</span><span class="sxs-lookup"><span data-stu-id="e6cc2-153">To demonstrate how to add a custom token, which is stored as part of `SaveTokens`, the sample app adds an <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> with the current <xref:System.DateTime> for an [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) of `TicketCreated`:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=10-21)]

## <a name="sample-app-instructions"></a><span data-ttu-id="e6cc2-154">サンプル アプリの手順</span><span class="sxs-lookup"><span data-stu-id="e6cc2-154">Sample app instructions</span></span>

<span data-ttu-id="e6cc2-155">サンプル アプリについて説明する方法。</span><span class="sxs-lookup"><span data-stu-id="e6cc2-155">The sample app demonstrates how to:</span></span>

* <span data-ttu-id="e6cc2-156">Google からユーザーの性別を取得し、値は、性別のクレームを格納します。</span><span class="sxs-lookup"><span data-stu-id="e6cc2-156">Obtain the user's gender from Google and store a gender claim with the value.</span></span>
* <span data-ttu-id="e6cc2-157">ユーザーの Google アクセス トークンは保存`AuthenticationProperties`します。</span><span class="sxs-lookup"><span data-stu-id="e6cc2-157">Store the Google access token in the user's `AuthenticationProperties`.</span></span>

<span data-ttu-id="e6cc2-158">サンプル アプリを使用します。</span><span class="sxs-lookup"><span data-stu-id="e6cc2-158">To use the sample app:</span></span>

1. <span data-ttu-id="e6cc2-159">アプリを登録し、有効なクライアント ID と Google 認証用のクライアント シークレットを取得します。</span><span class="sxs-lookup"><span data-stu-id="e6cc2-159">Register the app and obtain a valid client ID and client secret for Google authentication.</span></span> <span data-ttu-id="e6cc2-160">詳細については、「 <xref:security/authentication/google-logins> 」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e6cc2-160">For more information, see <xref:security/authentication/google-logins>.</span></span>
1. <span data-ttu-id="e6cc2-161">クライアント ID とクライアント シークレットをアプリに提供、<xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>の`Startup.ConfigureServices`します。</span><span class="sxs-lookup"><span data-stu-id="e6cc2-161">Provide the client ID and client secret to the app in the <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> of `Startup.ConfigureServices`.</span></span>
1. <span data-ttu-id="e6cc2-162">アプリを実行し、My 要求ページを要求します。</span><span class="sxs-lookup"><span data-stu-id="e6cc2-162">Run the app and request the My Claims page.</span></span> <span data-ttu-id="e6cc2-163">ユーザーが署名されていないときに、アプリを Google にリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="e6cc2-163">When the user isn't signed in, the app redirects to Google.</span></span> <span data-ttu-id="e6cc2-164">Google でサインインします。</span><span class="sxs-lookup"><span data-stu-id="e6cc2-164">Sign in with Google.</span></span> <span data-ttu-id="e6cc2-165">Google は、アプリにユーザーをリダイレクト (`/Home/MyClaims`)。</span><span class="sxs-lookup"><span data-stu-id="e6cc2-165">Google redirects the user back to the app (`/Home/MyClaims`).</span></span> <span data-ttu-id="e6cc2-166">ユーザーが認証されると、および My 要求ページが読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="e6cc2-166">The user is authenticated, and the My Claims page is loaded.</span></span> <span data-ttu-id="e6cc2-167">性別の要求は下**ユーザーの信頼性情報**Google から取得した値を使用します。</span><span class="sxs-lookup"><span data-stu-id="e6cc2-167">The gender claim is present under **User Claims** with the value obtained from Google.</span></span> <span data-ttu-id="e6cc2-168">表示されます、アクセス トークン、**認証プロパティ**します。</span><span class="sxs-lookup"><span data-stu-id="e6cc2-168">The access token appears in the **Authentication Properties**.</span></span>

```
User Claims

http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier
    b36a7b09-9135-4810-b7a5-78697ff23e99
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name
    username@gmail.com
AspNet.Identity.SecurityStamp
    29G2TB881ATCUQFJSRFG1S0QJ0OOAWVT
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/gender
    female
http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod
    Google

Authentication Properties

.Token.access_token
    bv42.Dgw...GQMv9ArLPs
.Token.token_type
    Bearer
.Token.expires_at
    2018-08-27T19:08:00.0000000+00:00
.Token.TicketCreated
    8/27/2018 6:08:00 PM
.TokenNames
    access_token;token_type;expires_at;TicketCreated
.issued
    Mon, 27 Aug 2018 18:08:05 GMT
.expires
    Mon, 10 Sep 2018 18:08:05 GMT
```
