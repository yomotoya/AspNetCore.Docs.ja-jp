---
title: その他の要求と ASP.NET Core での外部プロバイダーからのトークンを保存します。
author: guardrex
description: その他の要求と外部プロバイダーからトークンを確立する方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/14/2019
uid: security/authentication/social/additional-claims
ms.openlocfilehash: e18287e5a4171b3f7a6daa122111448b8447c1da
ms.sourcegitcommit: ccbb84ae307a5bc527441d3d509c20b5c1edde05
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/19/2019
ms.locfileid: "65874846"
---
# <a name="persist-additional-claims-and-tokens-from-external-providers-in-aspnet-core"></a><span data-ttu-id="b370d-103">その他の要求と ASP.NET Core での外部プロバイダーからのトークンを保存します。</span><span class="sxs-lookup"><span data-stu-id="b370d-103">Persist additional claims and tokens from external providers in ASP.NET Core</span></span>

<span data-ttu-id="b370d-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="b370d-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="b370d-105">ASP.NET Core アプリは、追加の要求および Facebook、Google、Microsoft、Twitter などの外部認証プロバイダーからのトークンを確立できます。</span><span class="sxs-lookup"><span data-stu-id="b370d-105">An ASP.NET Core app can establish additional claims and tokens from external authentication providers, such as Facebook, Google, Microsoft, and Twitter.</span></span> <span data-ttu-id="b370d-106">各プロバイダーが、そのプラットフォームでユーザーに関するさまざまな情報が表示されますが、受信し、その他の要求にユーザー データを変換するためのパターンは同じです。</span><span class="sxs-lookup"><span data-stu-id="b370d-106">Each provider reveals different information about users on its platform, but the pattern for receiving and transforming user data into additional claims is the same.</span></span>

<span data-ttu-id="b370d-107">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="b370d-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b370d-108">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="b370d-108">Prerequisites</span></span>

<span data-ttu-id="b370d-109">アプリでサポートするには、どの外部認証プロバイダーを決定します。</span><span class="sxs-lookup"><span data-stu-id="b370d-109">Decide which external authentication providers to support in the app.</span></span> <span data-ttu-id="b370d-110">プロバイダーごとにアプリを登録して、クライアント ID とクライアント シークレットを取得します。</span><span class="sxs-lookup"><span data-stu-id="b370d-110">For each provider, register the app and obtain a client ID and client secret.</span></span> <span data-ttu-id="b370d-111">詳細については、「 <xref:security/authentication/social/index> 」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="b370d-111">For more information, see <xref:security/authentication/social/index>.</span></span> <span data-ttu-id="b370d-112">サンプル アプリでは、 [Google の認証プロバイダー](xref:security/authentication/google-logins)します。</span><span class="sxs-lookup"><span data-stu-id="b370d-112">The sample app uses the [Google authentication provider](xref:security/authentication/google-logins).</span></span>

## <a name="set-the-client-id-and-client-secret"></a><span data-ttu-id="b370d-113">クライアント ID とクライアント シークレットを設定します。</span><span class="sxs-lookup"><span data-stu-id="b370d-113">Set the client ID and client secret</span></span>

<span data-ttu-id="b370d-114">OAuth 認証プロバイダーは、クライアント ID とクライアント シークレットを使用するアプリとの信頼関係を確立します。</span><span class="sxs-lookup"><span data-stu-id="b370d-114">The OAuth authentication provider establishes a trust relationship with an app using a client ID and client secret.</span></span> <span data-ttu-id="b370d-115">クライアント ID とクライアント シークレットの値はアプリ用に作成、外部認証プロバイダーによって、アプリが、プロバイダーに登録されている場合。</span><span class="sxs-lookup"><span data-stu-id="b370d-115">Client ID and client secret values are created for the app by the external authentication provider when the app is registered with the provider.</span></span> <span data-ttu-id="b370d-116">各外部プロバイダー、アプリが使用するは、プロバイダーのクライアント ID とクライアント シークレット個別に構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b370d-116">Each external provider that the app uses must be configured independently with the provider's client ID and client secret.</span></span> <span data-ttu-id="b370d-117">詳細については、実際のシナリオに適用される、外部認証プロバイダーについてのトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="b370d-117">For more information, see the external authentication provider topics that apply to your scenario:</span></span>

* [<span data-ttu-id="b370d-118">Facebook での認証</span><span class="sxs-lookup"><span data-stu-id="b370d-118">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="b370d-119">Google での認証</span><span class="sxs-lookup"><span data-stu-id="b370d-119">Google authentication</span></span>](xref:security/authentication/google-logins)
* [<span data-ttu-id="b370d-120">Microsoft での認証</span><span class="sxs-lookup"><span data-stu-id="b370d-120">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="b370d-121">Twitter での認証</span><span class="sxs-lookup"><span data-stu-id="b370d-121">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="b370d-122">他の認証プロバイダー</span><span class="sxs-lookup"><span data-stu-id="b370d-122">Other authentication providers</span></span>](xref:security/authentication/otherlogins)
* [<span data-ttu-id="b370d-123">OpenIdConnect</span><span class="sxs-lookup"><span data-stu-id="b370d-123">OpenIdConnect</span></span>](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

<span data-ttu-id="b370d-124">サンプル アプリは、クライアント ID とクライアント シークレットが Google から提供されると、Google の認証プロバイダーを構成します。</span><span class="sxs-lookup"><span data-stu-id="b370d-124">The sample app configures the Google authentication provider with a client ID and client secret provided by Google:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,9)]

## <a name="establish-the-authentication-scope"></a><span data-ttu-id="b370d-125">認証のスコープを確立します。</span><span class="sxs-lookup"><span data-stu-id="b370d-125">Establish the authentication scope</span></span>

<span data-ttu-id="b370d-126">指定することで、プロバイダーから取得するアクセス許可の一覧を指定、<xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>します。</span><span class="sxs-lookup"><span data-stu-id="b370d-126">Specify the list of permissions to retrieve from the provider by specifying the <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span></span> <span data-ttu-id="b370d-127">一般的な外部プロバイダーの認証のスコープは、次の表に表示されます。</span><span class="sxs-lookup"><span data-stu-id="b370d-127">Authentication scopes for common external providers appear in the following table.</span></span>

| <span data-ttu-id="b370d-128">プロバイダー</span><span class="sxs-lookup"><span data-stu-id="b370d-128">Provider</span></span>  | <span data-ttu-id="b370d-129">スコープ</span><span class="sxs-lookup"><span data-stu-id="b370d-129">Scope</span></span>                                                            |
| --------- | ---------------------------------------------------------------- |
| <span data-ttu-id="b370d-130">Facebook</span><span class="sxs-lookup"><span data-stu-id="b370d-130">Facebook</span></span>  | `https://www.facebook.com/dialog/oauth`                          |
| <span data-ttu-id="b370d-131">Google</span><span class="sxs-lookup"><span data-stu-id="b370d-131">Google</span></span>    | `https://www.googleapis.com/auth/userinfo.profile`               |
| <span data-ttu-id="b370d-132">Microsoft</span><span class="sxs-lookup"><span data-stu-id="b370d-132">Microsoft</span></span> | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| <span data-ttu-id="b370d-133">Twitter</span><span class="sxs-lookup"><span data-stu-id="b370d-133">Twitter</span></span>   | `https://api.twitter.com/oauth/authenticate`                     |

<span data-ttu-id="b370d-134">サンプル アプリで、Google の`userinfo.profile`スコープは、フレームワークによって自動的に追加時に<xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*>で呼び出される、<xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>します。</span><span class="sxs-lookup"><span data-stu-id="b370d-134">In the sample app, Google's `userinfo.profile` scope is automatically added by the framework when <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> is called on the <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>.</span></span> <span data-ttu-id="b370d-135">アプリは、追加のスコープを必要とする場合は、オプションを追加します。</span><span class="sxs-lookup"><span data-stu-id="b370d-135">If the app requires additional scopes, add them to the options.</span></span> <span data-ttu-id="b370d-136">次の例では、Google`https://www.googleapis.com/auth/user.birthday.read`スコープは、ユーザーの誕生日を取得するために追加されます。</span><span class="sxs-lookup"><span data-stu-id="b370d-136">In the following example, the Google `https://www.googleapis.com/auth/user.birthday.read` scope is added in order to retrieve a user's birthday:</span></span>

```csharp
options.Scope.Add("https://www.googleapis.com/auth/user.birthday.read");
```

## <a name="map-user-data-keys-and-create-claims"></a><span data-ttu-id="b370d-137">ユーザー データのキーをマップし、要求の作成</span><span class="sxs-lookup"><span data-stu-id="b370d-137">Map user data keys and create claims</span></span>

<span data-ttu-id="b370d-138">プロバイダーのオプションでは、指定、<xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*>または<xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*>の外部プロバイダーのユーザー データを JSON のサインイン時の読み取りをアプリ id の各キー サブキー/。</span><span class="sxs-lookup"><span data-stu-id="b370d-138">In the provider's options, specify a <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> or <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> for each key/subkey in the external provider's JSON user data for the app identity to read on sign in.</span></span> <span data-ttu-id="b370d-139">要求の種類の詳細については、次を参照してください。<xref:System.Security.Claims.ClaimTypes>します。</span><span class="sxs-lookup"><span data-stu-id="b370d-139">For more information on claim types, see <xref:System.Security.Claims.ClaimTypes>.</span></span>

<span data-ttu-id="b370d-140">サンプル アプリは、ロケールを作成します (`urn:google:locale`) と画像 (`urn:google:picture`) からの要求、`locale`と`picture`Google ユーザー データ内のキー。</span><span class="sxs-lookup"><span data-stu-id="b370d-140">The sample app creates locale (`urn:google:locale`) and picture (`urn:google:picture`) claims from the `locale` and `picture` keys in Google user data:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=13-14)]

<span data-ttu-id="b370d-141"><xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>、 <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) が使用して、アプリにサインイン<xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>します。</span><span class="sxs-lookup"><span data-stu-id="b370d-141">In <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>, an <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) is signed into the app with <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>.</span></span> <span data-ttu-id="b370d-142">プロセスでは、サインアップ時に、<xref:Microsoft.AspNetCore.Identity.UserManager%601>格納できる、`ApplicationUser`から使用可能なユーザー データの要求、<xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>します。</span><span class="sxs-lookup"><span data-stu-id="b370d-142">During the sign in process, the <xref:Microsoft.AspNetCore.Identity.UserManager%601> can store an `ApplicationUser` claims for user data available from the <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span></span>

<span data-ttu-id="b370d-143">サンプル アプリで`OnPostConfirmationAsync`(*Account/ExternalLogin.cshtml.cs*)、ロケールの確立 (`urn:google:locale`) と画像 (`urn:google:picture`)、署名付きの要求で`ApplicationUser`、クレームを含む<xref:System.Security.Claims.ClaimTypes.GivenName>:</span><span class="sxs-lookup"><span data-stu-id="b370d-143">In the sample app, `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) establishes the locale (`urn:google:locale`) and picture (`urn:google:picture`) claims for the signed in `ApplicationUser`, including a claim for <xref:System.Security.Claims.ClaimTypes.GivenName>:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=35-51)]

<span data-ttu-id="b370d-144">既定では、ユーザーの要求は認証 cookie に格納されます。</span><span class="sxs-lookup"><span data-stu-id="b370d-144">By default, a user's claims are stored in the authentication cookie.</span></span> <span data-ttu-id="b370d-145">認証 cookie が大きすぎる場合、アプリケーションのために失敗する可能性します。</span><span class="sxs-lookup"><span data-stu-id="b370d-145">If the authentication cookie is too large, it can cause the app to fail because:</span></span>

* <span data-ttu-id="b370d-146">ブラウザーでは、cookie のヘッダーが長すぎることを検出します。</span><span class="sxs-lookup"><span data-stu-id="b370d-146">The browser detects that the cookie header is too long.</span></span>
* <span data-ttu-id="b370d-147">要求の全体的なサイズが大きすぎます。</span><span class="sxs-lookup"><span data-stu-id="b370d-147">The overall size of the request is too large.</span></span>

<span data-ttu-id="b370d-148">大量のユーザー データがユーザーの要求を処理するために必要な場合。</span><span class="sxs-lookup"><span data-stu-id="b370d-148">If a large amount of user data is required for processing user requests:</span></span>

* <span data-ttu-id="b370d-149">要求にのみ、アプリが必要な処理のユーザー要求のサイズと数を制限します。</span><span class="sxs-lookup"><span data-stu-id="b370d-149">Limit the number and size of user claims for request processing to only what the app requires.</span></span>
* <span data-ttu-id="b370d-150">使用して、カスタム<xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore>の Cookie 認証ミドルウェアの<xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore>要求間で id を格納します。</span><span class="sxs-lookup"><span data-stu-id="b370d-150">Use a custom <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> for the Cookie Authentication Middleware's <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> to store identity across requests.</span></span> <span data-ttu-id="b370d-151">大量ののみ、小規模のセッション識別子のキーをクライアントに送信中に、サーバー上の id 情報が保持されます。</span><span class="sxs-lookup"><span data-stu-id="b370d-151">Preserve large quantities of identity information on the server while only sending a small session identifier key to the client.</span></span>

## <a name="save-the-access-token"></a><span data-ttu-id="b370d-152">アクセス トークンを保存します。</span><span class="sxs-lookup"><span data-stu-id="b370d-152">Save the access token</span></span>

<span data-ttu-id="b370d-153"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> アクセスと更新トークンを保存するかどうかを定義、<xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties>成功した認証が完了後します。</span><span class="sxs-lookup"><span data-stu-id="b370d-153"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> defines whether access and refresh tokens should be stored in the <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> after a successful authorization.</span></span> <span data-ttu-id="b370d-154">`SaveTokens` 設定されている`false`既定では、最終的な認証 cookie のサイズを小さくします。</span><span class="sxs-lookup"><span data-stu-id="b370d-154">`SaveTokens` is set to `false` by default to reduce the size of the final authentication cookie.</span></span>

<span data-ttu-id="b370d-155">サンプル アプリの値を設定する`SaveTokens`に`true`で<xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span><span class="sxs-lookup"><span data-stu-id="b370d-155">The sample app sets the value of `SaveTokens` to `true` in <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=15)]

<span data-ttu-id="b370d-156">ときに`OnPostConfirmationAsync`を実行するアクセス トークンを保存 ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) で外部プロバイダーから、`ApplicationUser`の`AuthenticationProperties`します。</span><span class="sxs-lookup"><span data-stu-id="b370d-156">When `OnPostConfirmationAsync` executes, store the access token ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) from the external provider in the `ApplicationUser`'s `AuthenticationProperties`.</span></span>

<span data-ttu-id="b370d-157">サンプル アプリでアクセス トークンを保存します`OnPostConfirmationAsync`(新しいユーザーの登録) と`OnGetCallbackAsync`(以前に登録したユーザー) で*Account/ExternalLogin.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="b370d-157">The sample app saves the access token in `OnPostConfirmationAsync` (new user registration) and `OnGetCallbackAsync` (previously registered user) in *Account/ExternalLogin.cshtml.cs*:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=54-56)]

## <a name="how-to-add-additional-custom-tokens"></a><span data-ttu-id="b370d-158">追加のカスタム トークンを追加する方法</span><span class="sxs-lookup"><span data-stu-id="b370d-158">How to add additional custom tokens</span></span>

<span data-ttu-id="b370d-159">一部として格納されているカスタムのトークンを追加する方法を説明するために`SaveTokens`、サンプル アプリを追加、<xref:Microsoft.AspNetCore.Authentication.AuthenticationToken>を現在<xref:System.DateTime>の[AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*)の`TicketCreated`:</span><span class="sxs-lookup"><span data-stu-id="b370d-159">To demonstrate how to add a custom token, which is stored as part of `SaveTokens`, the sample app adds an <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> with the current <xref:System.DateTime> for an [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) of `TicketCreated`:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=17-28)]

## <a name="creating-and-adding-claims"></a><span data-ttu-id="b370d-160">作成して、要求の追加</span><span class="sxs-lookup"><span data-stu-id="b370d-160">Creating and adding claims</span></span>

<span data-ttu-id="b370d-161">フレームワークは、一般的な操作と、コレクションにクレームを追加する拡張メソッドを提供します。</span><span class="sxs-lookup"><span data-stu-id="b370d-161">The framework provides common actions and extension methods for creating and adding claims to the collection.</span></span> <span data-ttu-id="b370d-162">詳細については、「 <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> 」および「 <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="b370d-162">For more information, see the <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> and <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>.</span></span>

<span data-ttu-id="b370d-163">派生することによって、ユーザーがカスタム アクションを定義できます<xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction>抽象の実装と<xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*>メソッド。</span><span class="sxs-lookup"><span data-stu-id="b370d-163">Users can define custom actions by deriving from <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> and implementing the abstract <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*> method.</span></span>

<span data-ttu-id="b370d-164">詳細については、「 <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims> 」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="b370d-164">For more information, see <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.</span></span>

## <a name="removal-of-claim-actions-and-claims"></a><span data-ttu-id="b370d-165">要求アクションと要求の削除</span><span class="sxs-lookup"><span data-stu-id="b370d-165">Removal of claim actions and claims</span></span>

<span data-ttu-id="b370d-166">[ClaimActionCollection.Remove(String)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*)すべて削除要求のアクションを指定された<xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType>コレクションから。</span><span class="sxs-lookup"><span data-stu-id="b370d-166">[ClaimActionCollection.Remove(String)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) removes all claim actions for the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the collection.</span></span> <span data-ttu-id="b370d-167">[ClaimActionCollectionMapExtensions.DeleteClaim (ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*)のクレームを削除する、指定された<xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType>id からです。</span><span class="sxs-lookup"><span data-stu-id="b370d-167">[ClaimActionCollectionMapExtensions.DeleteClaim(ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) deletes a claim of the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the identity.</span></span> <span data-ttu-id="b370d-168"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> 主に併用[OpenID Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc)プロトコルによって生成されたクレームを削除します。</span><span class="sxs-lookup"><span data-stu-id="b370d-168"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> is primarily used with [OpenID Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc) to remove protocol-generated claims.</span></span>

## <a name="sample-app-output"></a><span data-ttu-id="b370d-169">アプリのサンプル出力</span><span class="sxs-lookup"><span data-stu-id="b370d-169">Sample app output</span></span>

```
User Claims

http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier
    9b342344f-7aab-43c2-1ac1-ba75912ca999
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name
    someone@gmail.com
AspNet.Identity.SecurityStamp
    7D4312MOWRYYBFI1KXRPHGOSTBVWSFDE
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname
    Judy
urn:google:locale
    en
urn:google:picture
    https://lh4.googleusercontent.com/-XXXXXX/XXXXXX/XXXXXX/XXXXXX/photo.jpg

Authentication Properties

.Token.access_token
    yc23.AlvoZqz56...1lxltXV7D-ZWP9
.Token.token_type
    Bearer
.Token.expires_at
    2019-04-11T22:14:51.0000000+00:00
.Token.TicketCreated
    4/11/2019 9:14:52 PM
.TokenNames
    access_token;token_type;expires_at;TicketCreated
.persistent
.issued
    Thu, 11 Apr 2019 20:51:06 GMT
.expires
    Thu, 25 Apr 2019 20:51:06 GMT

```

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="additional-resources"></a><span data-ttu-id="b370d-170">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="b370d-170">Additional resources</span></span>

* <span data-ttu-id="b370d-171">[aspnet/AspNetCore エンジニア リング SocialSample アプリ](https://github.com/aspnet/AspNetCore/tree/master/src/Security/Authentication/samples/SocialSample)&ndash;リンクされているサンプル アプリが上に、 [aspnet/AspNetCore GitHub リポジトリの](https://github.com/aspnet/AspNetCore)`master`エンジニア リングの分岐。</span><span class="sxs-lookup"><span data-stu-id="b370d-171">[aspnet/AspNetCore engineering SocialSample app](https://github.com/aspnet/AspNetCore/tree/master/src/Security/Authentication/samples/SocialSample) &ndash; The linked sample app is on the [aspnet/AspNetCore GitHub repo's](https://github.com/aspnet/AspNetCore) `master` engineering branch.</span></span> <span data-ttu-id="b370d-172">`master`ブランチには、次のリリースの ASP.NET Core の開発中のコードが含まれています。</span><span class="sxs-lookup"><span data-stu-id="b370d-172">The `master` branch contains code under active development for the next release of ASP.NET Core.</span></span> <span data-ttu-id="b370d-173">ASP.NET Core のリリース バージョンのサンプル アプリのバージョンを表示する、**ブランチ**ドロップダウン リストをリリース ブランチを選択します (たとえば`release/2.2`)。</span><span class="sxs-lookup"><span data-stu-id="b370d-173">To see a version of the sample app for a released version of ASP.NET Core, use the **Branch** drop down list to select a release branch (for example `release/2.2`).</span></span>
