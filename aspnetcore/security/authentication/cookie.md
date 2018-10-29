---
title: ASP.NET Core Identity なしでの cookie 認証を使用します。
author: rick-anderson
description: ASP.NET Core Identity なしでの cookie 認証を使用しての説明
ms.author: riande
ms.date: 10/11/2017
uid: security/authentication/cookie
ms.openlocfilehash: 8add7559557d505397c3be8d8a48aa2e9d9e45e8
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207421"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a><span data-ttu-id="f22d1-103">ASP.NET Core Identity なしでの cookie 認証を使用します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-103">Use cookie authentication without ASP.NET Core Identity</span></span>

<span data-ttu-id="f22d1-104">によって[Rick Anderson](https://twitter.com/RickAndMSFT)と[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f22d1-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="f22d1-105">以前の認証に関するトピックで説明したように[ASP.NET Core Identity](xref:security/authentication/identity)の作成とログインの管理、フル機能の完全な認証プロバイダーは、します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-105">As you've seen in the earlier authentication topics, [ASP.NET Core Identity](xref:security/authentication/identity) is a complete, full-featured authentication provider for creating and maintaining logins.</span></span> <span data-ttu-id="f22d1-106">ただし、cookie ベースの認証時に、独自のカスタム認証ロジックを使用する場合があります。</span><span class="sxs-lookup"><span data-stu-id="f22d1-106">However, you may want to use your own custom authentication logic with cookie-based authentication at times.</span></span> <span data-ttu-id="f22d1-107">ASP.NET Core Identity なしのスタンドアロンの認証プロバイダーとしては、cookie ベースの認証を使用できます。</span><span class="sxs-lookup"><span data-stu-id="f22d1-107">You can use cookie-based authentication as a standalone authentication provider without ASP.NET Core Identity.</span></span>

<span data-ttu-id="f22d1-108">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/samples)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="f22d1-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="f22d1-109">サンプル アプリでは、デモンストレーションのための Maria Rodriguez、架空のユーザーのユーザー アカウント、アプリにハードコードしています。</span><span class="sxs-lookup"><span data-stu-id="f22d1-109">For demonstration purposes in the sample app, the user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="f22d1-110">電子メールのユーザー名を使用して、"maria.rodriguez@contoso.com"と、ユーザーがサインインするすべてのパスワード。</span><span class="sxs-lookup"><span data-stu-id="f22d1-110">Use the Email username "maria.rodriguez@contoso.com" and any password to sign in the user.</span></span> <span data-ttu-id="f22d1-111">における、ユーザーの認証、`AuthenticateUser`メソッドで、 *Pages/Account/Login.cshtml.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="f22d1-111">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="f22d1-112">実際の例では、ユーザーは、データベースに対して認証は。</span><span class="sxs-lookup"><span data-stu-id="f22d1-112">In a real-world example, the user would be authenticated against a database.</span></span>

<span data-ttu-id="f22d1-113">ASP.NET Core から移行する cookie ベースの認証の詳細について 1.x から 2.0 への参照[認証の移行と ASP.NET Core 2.0 のトピック (Cookie ベースの認証) を Id](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication)します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-113">For information on migrating cookie-based authentication from ASP.NET Core 1.x to 2.0, see [Migrate Authentication and Identity to ASP.NET Core 2.0 topic (Cookie-based Authentication)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).</span></span>

<span data-ttu-id="f22d1-114">ASP.NET Core Identity を使用する、次を参照してください。、 [Id の概要](xref:security/authentication/identity)トピック。</span><span class="sxs-lookup"><span data-stu-id="f22d1-114">To use ASP.NET Core Identity, see the [Introduction to Identity](xref:security/authentication/identity) topic.</span></span>

## <a name="configuration"></a><span data-ttu-id="f22d1-115">構成</span><span class="sxs-lookup"><span data-stu-id="f22d1-115">Configuration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f22d1-116">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f22d1-116">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="f22d1-117">アプリを使用しない場合、 [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)、プロジェクト ファイルでパッケージの参照を作成、 [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/)パッケージ (バージョン 2.1.0 または後で)。</span><span class="sxs-lookup"><span data-stu-id="f22d1-117">If the app doesn't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference in the project file for the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) package (version 2.1.0 or later).</span></span>

<span data-ttu-id="f22d1-118">`ConfigureServices`メソッドを使用して、認証ミドルウェア サービスを作成、`AddAuthentication`と`AddCookie`メソッド。</span><span class="sxs-lookup"><span data-stu-id="f22d1-118">In the `ConfigureServices` method, create the Authentication Middleware service with the `AddAuthentication` and `AddCookie` methods:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

<span data-ttu-id="f22d1-119">`AuthenticationScheme` 渡される`AddAuthentication`アプリの既定の認証スキームを設定します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-119">`AuthenticationScheme` passed to `AddAuthentication` sets the default authentication scheme for the app.</span></span> <span data-ttu-id="f22d1-120">`AuthenticationScheme` cookie 認証の複数のインスタンスがあるし、する場合に便利です[特定のスキームで承認](xref:security/authorization/limitingidentitybyscheme)します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-120">`AuthenticationScheme` is useful when there are multiple instances of cookie authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="f22d1-121">設定、`AuthenticationScheme`に`CookieAuthenticationDefaults.AuthenticationScheme`スキームの「クッキー」の値を指定します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-121">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="f22d1-122">スキームを識別する任意の文字列値を指定することができます。</span><span class="sxs-lookup"><span data-stu-id="f22d1-122">You can supply any string value that distinguishes the scheme.</span></span>

<span data-ttu-id="f22d1-123">アプリの認証スキームは、アプリの cookie 認証スキームと異なります。</span><span class="sxs-lookup"><span data-stu-id="f22d1-123">The app's authentication scheme is different from the app's cookie authentication scheme.</span></span> <span data-ttu-id="f22d1-124">Cookie の認証スキームがときに指定されていません<xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>を使用して[CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) (「クッキー」)。</span><span class="sxs-lookup"><span data-stu-id="f22d1-124">When a cookie authentication scheme isn't provided to <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, it uses [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("Cookies").</span></span>

<span data-ttu-id="f22d1-125">`Configure`メソッドを使用して、`UseAuthentication`を設定する、認証ミドルウェアを呼び出すメソッドを`HttpContext.User`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="f22d1-125">In the `Configure` method, use the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="f22d1-126">呼び出す、`UseAuthentication`メソッドを呼び出す前に`UseMvcWithDefaultRoute`または`UseMvc`:</span><span class="sxs-lookup"><span data-stu-id="f22d1-126">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

<span data-ttu-id="f22d1-127">**AddCookie オプション**</span><span class="sxs-lookup"><span data-stu-id="f22d1-127">**AddCookie Options**</span></span>

<span data-ttu-id="f22d1-128">[CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0)認証プロバイダーのオプションを構成するクラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-128">The [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="f22d1-129">オプション</span><span class="sxs-lookup"><span data-stu-id="f22d1-129">Option</span></span> | <span data-ttu-id="f22d1-130">説明</span><span class="sxs-lookup"><span data-stu-id="f22d1-130">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="f22d1-131">AccessDeniedPath</span><span class="sxs-lookup"><span data-stu-id="f22d1-131">AccessDeniedPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath?view=aspnetcore-2.0) | <span data-ttu-id="f22d1-132">302 検出 (URL リダイレクト) を指定するパスを提供してトリガーされたときに`HttpContext.ForbidAsync`します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-132">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ForbidAsync`.</span></span> <span data-ttu-id="f22d1-133">既定値は `/Account/AccessDenied` です。</span><span class="sxs-lookup"><span data-stu-id="f22d1-133">The default value is `/Account/AccessDenied`.</span></span> |
| [<span data-ttu-id="f22d1-134">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="f22d1-134">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer?view=aspnetcore-2.0) | <span data-ttu-id="f22d1-135">使用する発行者、[発行者](/dotnet/api/system.security.claims.claim.issuer)cookie 認証サービスによって作成されたすべての要求のプロパティ。</span><span class="sxs-lookup"><span data-stu-id="f22d1-135">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication service.</span></span> |
| [<span data-ttu-id="f22d1-136">Cookie.Domain</span><span class="sxs-lookup"><span data-stu-id="f22d1-136">Cookie.Domain</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain?view=aspnetcore-2.0) | <span data-ttu-id="f22d1-137">ドメイン名は、cookie が処理される場所です。</span><span class="sxs-lookup"><span data-stu-id="f22d1-137">The domain name where the cookie is served.</span></span> <span data-ttu-id="f22d1-138">既定では、要求のホスト名です。</span><span class="sxs-lookup"><span data-stu-id="f22d1-138">By default, this is the host name of the request.</span></span> <span data-ttu-id="f22d1-139">ブラウザーはのみ、一致するホスト名に、要求で cookie を送信します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-139">The browser only sends the cookie in requests to a matching host name.</span></span> <span data-ttu-id="f22d1-140">このドメインの任意のホストに使用できる cookie を調整することがあります。</span><span class="sxs-lookup"><span data-stu-id="f22d1-140">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="f22d1-141">たとえば、クッキーのドメインに設定`.contoso.com`で使用できるように`contoso.com`、 `www.contoso.com`、および`staging.www.contoso.com`します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-141">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="f22d1-142">Cookie.Expiration</span><span class="sxs-lookup"><span data-stu-id="f22d1-142">Cookie.Expiration</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.expiration?view=aspnetcore-2.0) | <span data-ttu-id="f22d1-143">取得または cookie の有効期間を設定します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-143">Gets or sets the lifespan of a cookie.</span></span> <span data-ttu-id="f22d1-144">現在、このオプションは機能しませんは ASP.NET Core 2.1 以降では無効になります。</span><span class="sxs-lookup"><span data-stu-id="f22d1-144">Currently, this option no-ops and will become obsolete in ASP.NET Core 2.1+.</span></span> <span data-ttu-id="f22d1-145">使用して、`ExpireTimeSpan`クッキーの有効期限を設定するオプション。</span><span class="sxs-lookup"><span data-stu-id="f22d1-145">Use the `ExpireTimeSpan` option to set cookie expiration.</span></span> <span data-ttu-id="f22d1-146">詳細については、次を参照してください。 [CookieAuthenticationOptions.Cookie.Expiration の動作が明確](https://github.com/aspnet/Security/issues/1293)します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-146">For more information, see [Clarify behavior of CookieAuthenticationOptions.Cookie.Expiration](https://github.com/aspnet/Security/issues/1293).</span></span> |
| [<span data-ttu-id="f22d1-147">Cookie.HttpOnly</span><span class="sxs-lookup"><span data-stu-id="f22d1-147">Cookie.HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly?view=aspnetcore-2.0) | <span data-ttu-id="f22d1-148">クッキーをサーバーにのみアクセスできる必要があるかどうかを示すフラグです。</span><span class="sxs-lookup"><span data-stu-id="f22d1-148">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="f22d1-149">この値を変更する`false`クッキーにアクセスするクライアント側スクリプトを許可し、アプリが必要 cookie の盗難に、アプリを開くことがあります、[クロス サイト スクリプティング (XSS)](xref:security/cross-site-scripting)脆弱性。</span><span class="sxs-lookup"><span data-stu-id="f22d1-149">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="f22d1-150">既定値は `true` です。</span><span class="sxs-lookup"><span data-stu-id="f22d1-150">The default value is `true`.</span></span> |
| [<span data-ttu-id="f22d1-151">Cookie.Name</span><span class="sxs-lookup"><span data-stu-id="f22d1-151">Cookie.Name</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name?view=aspnetcore-2.0) | <span data-ttu-id="f22d1-152">クッキーの名前を設定します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-152">Sets the name of the cookie.</span></span> |
| [<span data-ttu-id="f22d1-153">Cookie.Path</span><span class="sxs-lookup"><span data-stu-id="f22d1-153">Cookie.Path</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path?view=aspnetcore-2.0) | <span data-ttu-id="f22d1-154">同じホスト名で実行されているアプリを分離するために使用します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-154">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="f22d1-155">実行中のアプリがあれば`/app1`cookie をそのアプリに制限を設定して、`CookiePath`プロパティを`/app1`します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-155">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="f22d1-156">これにより、クッキーが要求に使用可能なだけ`/app1`とその下にあるすべてのアプリ。</span><span class="sxs-lookup"><span data-stu-id="f22d1-156">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="f22d1-157">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="f22d1-157">Cookie.SameSite</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite?view=aspnetcore-2.0) | <span data-ttu-id="f22d1-158">ブラウザーが同じサイトの要求のみに接続する cookie を許可するかどうかを指定 (`SameSiteMode.Strict`) または安全な HTTP メソッドと同じサイトの要求を使用してサイト間の要求 (`SameSiteMode.Lax`)。</span><span class="sxs-lookup"><span data-stu-id="f22d1-158">Indicates whether the browser should allow the cookie to be attached to same-site requests only (`SameSiteMode.Strict`) or cross-site requests using safe HTTP methods and same-site requests (`SameSiteMode.Lax`).</span></span> <span data-ttu-id="f22d1-159">設定すると`SameSiteMode.None`cookie のヘッダーの値が設定されていません。</span><span class="sxs-lookup"><span data-stu-id="f22d1-159">When set to `SameSiteMode.None`, the cookie header value isn't set.</span></span> <span data-ttu-id="f22d1-160">なお[Cookie ポリシー ミドルウェア](#cookie-policy-middleware)指定した値を上書きする可能性があります。</span><span class="sxs-lookup"><span data-stu-id="f22d1-160">Note that [Cookie Policy Middleware](#cookie-policy-middleware) might overwrite the value that you provide.</span></span> <span data-ttu-id="f22d1-161">既定値は、OAuth 認証をサポートする`SameSiteMode.Lax`します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-161">To support OAuth authentication, the default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="f22d1-162">詳細については、次を参照してください。 [SameSite cookie のポリシーにより分割 OAuth 認証](https://github.com/aspnet/Security/issues/1231)します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-162">For more information, see [OAuth authentication broken due to SameSite cookie policy](https://github.com/aspnet/Security/issues/1231).</span></span> |
| [<span data-ttu-id="f22d1-163">Cookie.SecurePolicy</span><span class="sxs-lookup"><span data-stu-id="f22d1-163">Cookie.SecurePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy?view=aspnetcore-2.0) | <span data-ttu-id="f22d1-164">作成された cookie を HTTPS に制限するかどうかを示すフラグ (`CookieSecurePolicy.Always`)、HTTP または HTTPS (`CookieSecurePolicy.None`)、または、要求と同じプロトコル (`CookieSecurePolicy.SameAsRequest`)。</span><span class="sxs-lookup"><span data-stu-id="f22d1-164">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="f22d1-165">既定値は `CookieSecurePolicy.SameAsRequest` です。</span><span class="sxs-lookup"><span data-stu-id="f22d1-165">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="f22d1-166">DataProtectionProvider</span><span class="sxs-lookup"><span data-stu-id="f22d1-166">DataProtectionProvider</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0) | <span data-ttu-id="f22d1-167">セット、`DataProtectionProvider`の既定の作成に使用される`TicketDataFormat`します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-167">Sets the `DataProtectionProvider` that's used to create the default `TicketDataFormat`.</span></span> <span data-ttu-id="f22d1-168">場合、`TicketDataFormat`プロパティが設定されて、`DataProtectionProvider`オプションは使用されません。</span><span class="sxs-lookup"><span data-stu-id="f22d1-168">If the `TicketDataFormat` property is set, the `DataProtectionProvider` option isn't used.</span></span> <span data-ttu-id="f22d1-169">指定されていない場合は、アプリの既定のデータ保護プロバイダーが使用されます。</span><span class="sxs-lookup"><span data-stu-id="f22d1-169">If not provided, the app's default data protection provider is used.</span></span> |
| [<span data-ttu-id="f22d1-170">イベント</span><span class="sxs-lookup"><span data-stu-id="f22d1-170">Events</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events?view=aspnetcore-2.0) | <span data-ttu-id="f22d1-171">ハンドラーは、特定の処理ポイントでアプリの制御を提供するプロバイダーのメソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-171">The handler calls methods on the provider that give the app control at certain processing points.</span></span> <span data-ttu-id="f22d1-172">場合`Events`メソッドが呼び出されたときに何もしない既定のインスタンスが指定されて、指定されていません。</span><span class="sxs-lookup"><span data-stu-id="f22d1-172">If `Events` aren't provided, a default instance is supplied that does nothing when the methods are called.</span></span> |
| [<span data-ttu-id="f22d1-173">EventsType</span><span class="sxs-lookup"><span data-stu-id="f22d1-173">EventsType</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype?view=aspnetcore-2.0) | <span data-ttu-id="f22d1-174">サービスの種類として取得するために使用、`Events`インスタンス プロパティの代わりにします。</span><span class="sxs-lookup"><span data-stu-id="f22d1-174">Used as the service type to get the `Events` instance instead of the property.</span></span> |
| [<span data-ttu-id="f22d1-175">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="f22d1-175">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan?view=aspnetcore-2.0) | <span data-ttu-id="f22d1-176">`TimeSpan` Cookie 内に格納された認証チケットが期限切れ後。</span><span class="sxs-lookup"><span data-stu-id="f22d1-176">The `TimeSpan` after which the authentication ticket stored inside the cookie expires.</span></span> <span data-ttu-id="f22d1-177">`ExpireTimeSpan` チケットの有効期限を作成するには、現在の時刻に追加されます。</span><span class="sxs-lookup"><span data-stu-id="f22d1-177">`ExpireTimeSpan` is added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="f22d1-178">`ExpiredTimeSpan`値が常に検証サーバーで暗号化された認証チケットに移動します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-178">The `ExpiredTimeSpan` value always goes into the encrypted AuthTicket verified by the server.</span></span> <span data-ttu-id="f22d1-179">少し可能性があります、 [Set-cookie](https://tools.ietf.org/html/rfc6265#section-4.1)ヘッダーが場合にのみ、`IsPersistent`設定されます。</span><span class="sxs-lookup"><span data-stu-id="f22d1-179">It may also go into the [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) header, but only if `IsPersistent` is set.</span></span> <span data-ttu-id="f22d1-180">設定する`IsPersistent`に`true`、構成、 [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties)に渡される`SignInAsync`します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-180">To set `IsPersistent` to `true`, configure the [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) passed to `SignInAsync`.</span></span> <span data-ttu-id="f22d1-181">既定値の`ExpireTimeSpan`は 14 日間です。</span><span class="sxs-lookup"><span data-stu-id="f22d1-181">The default value of `ExpireTimeSpan` is 14 days.</span></span> |
| [<span data-ttu-id="f22d1-182">LoginPath</span><span class="sxs-lookup"><span data-stu-id="f22d1-182">LoginPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath?view=aspnetcore-2.0) | <span data-ttu-id="f22d1-183">302 検出 (URL リダイレクト) を指定するパスを提供してトリガーされたときに`HttpContext.ChallengeAsync`します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-183">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ChallengeAsync`.</span></span> <span data-ttu-id="f22d1-184">401 を生成した現在の URL を追加、`LoginPath`によってという名前のクエリ文字列パラメーターとして、`ReturnUrlParameter`します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-184">The current URL that generated the 401 is added to the `LoginPath` as a query string parameter named by the `ReturnUrlParameter`.</span></span> <span data-ttu-id="f22d1-185">要求を 1 回、`LoginPath`新しいサインイン id、付与、`ReturnUrlParameter`値を使用して、元の unauthorized ステータス コードの原因となった URL にブラウザーをリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="f22d1-185">Once a request to the `LoginPath` grants a new sign-in identity, the `ReturnUrlParameter` value is used to redirect the browser back to the URL that caused the original unauthorized status code.</span></span> <span data-ttu-id="f22d1-186">既定値は `/Account/Login` です。</span><span class="sxs-lookup"><span data-stu-id="f22d1-186">The default value is `/Account/Login`.</span></span> |
| [<span data-ttu-id="f22d1-187">LogoutPath</span><span class="sxs-lookup"><span data-stu-id="f22d1-187">LogoutPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath?view=aspnetcore-2.0) | <span data-ttu-id="f22d1-188">場合、 `LogoutPath` 、そのパスに対する要求をリダイレクトし、ハンドラーに提供の値に基づいて、`ReturnUrlParameter`します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-188">If the `LogoutPath` is provided to the handler, then a request to that path redirects based on the value of the `ReturnUrlParameter`.</span></span> <span data-ttu-id="f22d1-189">既定値は `/Account/Logout` です。</span><span class="sxs-lookup"><span data-stu-id="f22d1-189">The default value is `/Account/Logout`.</span></span> |
| [<span data-ttu-id="f22d1-190">ReturnUrlParameter</span><span class="sxs-lookup"><span data-stu-id="f22d1-190">ReturnUrlParameter</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter?view=aspnetcore-2.0) | <span data-ttu-id="f22d1-191">302 Found (URL リダイレクト) 応答のハンドラーによって追加されるクエリ文字列パラメーターの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-191">Determines the name of the query string parameter that's appended by the handler for a 302 Found (URL redirect) response.</span></span> <span data-ttu-id="f22d1-192">`ReturnUrlParameter` 要求を受信したときに使用されます、`LoginPath`または`LogoutPath`ログインまたはログアウト アクションが実行された後、元の URL にブラウザーに戻ります。</span><span class="sxs-lookup"><span data-stu-id="f22d1-192">`ReturnUrlParameter` is used when a request arrives on the `LoginPath` or `LogoutPath` to return the browser to the original URL after the login or logout action is performed.</span></span> <span data-ttu-id="f22d1-193">既定値は `ReturnUrl` です。</span><span class="sxs-lookup"><span data-stu-id="f22d1-193">The default value is `ReturnUrl`.</span></span> |
| [<span data-ttu-id="f22d1-194">SessionStore</span><span class="sxs-lookup"><span data-stu-id="f22d1-194">SessionStore</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore?view=aspnetcore-2.0) | <span data-ttu-id="f22d1-195">要求間で id を格納するために使用するオプションのコンテナー。</span><span class="sxs-lookup"><span data-stu-id="f22d1-195">An optional container used to store identity across requests.</span></span> <span data-ttu-id="f22d1-196">使用すると、セッション識別子のみがクライアントに送信されます。</span><span class="sxs-lookup"><span data-stu-id="f22d1-196">When used, only a session identifier is sent to the client.</span></span> <span data-ttu-id="f22d1-197">`SessionStore` 大規模な id を持つ潜在的な問題を軽減するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="f22d1-197">`SessionStore` can be used to mitigate potential problems with large identities.</span></span> |
| [<span data-ttu-id="f22d1-198">SlidingExpiration</span><span class="sxs-lookup"><span data-stu-id="f22d1-198">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-2.0) | <span data-ttu-id="f22d1-199">更新された期限で新しい cookie を動的に発行するかどうかを示すフラグです。</span><span class="sxs-lookup"><span data-stu-id="f22d1-199">A flag indicating if a new cookie with an updated expiration time should be issued dynamically.</span></span> <span data-ttu-id="f22d1-200">これは、現在 cookie の有効期間が 50% 以上の有効期限が切れたすべての要求に対して発生します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-200">This can happen on any request where the current cookie expiration period is more than 50% expired.</span></span> <span data-ttu-id="f22d1-201">新しい有効期限は順方向に移動は、現在の日付を指定するだけでなく、`ExpireTimespan`します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-201">The new expiration date is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="f22d1-202">[絶対クッキーの有効期限](xref:security/authentication/cookie#absolute-cookie-expiration)を使用して設定することができます、`AuthenticationProperties`クラスを呼び出すときに`SignInAsync`します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-202">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="f22d1-203">絶対有効期限は、認証 cookie が有効な時間を制限することで、アプリのセキュリティを強化できます。</span><span class="sxs-lookup"><span data-stu-id="f22d1-203">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="f22d1-204">既定値は `true` です。</span><span class="sxs-lookup"><span data-stu-id="f22d1-204">The default value is `true`.</span></span> |
| [<span data-ttu-id="f22d1-205">TicketDataFormat</span><span class="sxs-lookup"><span data-stu-id="f22d1-205">TicketDataFormat</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat?view=aspnetcore-2.0) | <span data-ttu-id="f22d1-206">`TicketDataFormat`を保護し、id と cookie の値に格納されているその他のプロパティの保護を解除するために使用します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-206">The `TicketDataFormat` is used to protect and unprotect the identity and other properties that are stored in the cookie value.</span></span> <span data-ttu-id="f22d1-207">指定しない場合、`TicketDataFormat`を使用して作成されて、 [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0)します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-207">If not provided, a `TicketDataFormat` is created using the [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0).</span></span> |
| [<span data-ttu-id="f22d1-208">検証</span><span class="sxs-lookup"><span data-stu-id="f22d1-208">Validate</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.validate?view=aspnetcore-2.0) | <span data-ttu-id="f22d1-209">オプションが有効であるかをチェックするメソッド。</span><span class="sxs-lookup"><span data-stu-id="f22d1-209">Method that checks that the options are valid.</span></span> |

<span data-ttu-id="f22d1-210">設定`CookieAuthenticationOptions`での認証サービスの構成で、`ConfigureServices`メソッド。</span><span class="sxs-lookup"><span data-stu-id="f22d1-210">Set `CookieAuthenticationOptions` in the service configuration for authentication in the `ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f22d1-211">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f22d1-211">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="f22d1-212">ASP.NET Core 1.x は cookie[ミドルウェア](xref:fundamentals/middleware/index)ユーザー プリンシパルに暗号化された cookie をシリアル化します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-212">ASP.NET Core 1.x uses cookie [middleware](xref:fundamentals/middleware/index) that serializes a user principal into an encrypted cookie.</span></span> <span data-ttu-id="f22d1-213">プリンシパルが再作成されに割り当てられているし、後続の要求で cookie が有効な`HttpContext.User`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="f22d1-213">On subsequent requests, the cookie is validated, and the principal is recreated and assigned to the `HttpContext.User` property.</span></span>

<span data-ttu-id="f22d1-214">インストール、 [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/)プロジェクトに NuGet パッケージ。</span><span class="sxs-lookup"><span data-stu-id="f22d1-214">Install the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) NuGet package in your project.</span></span> <span data-ttu-id="f22d1-215">このパッケージには、cookie ミドルウェアが含まれています。</span><span class="sxs-lookup"><span data-stu-id="f22d1-215">This package contains the cookie middleware.</span></span>

<span data-ttu-id="f22d1-216">使用して、`UseCookieAuthentication`メソッドで、`Configure`メソッドで、 *Startup.cs*する前にファイル`UseMvc`または`UseMvcWithDefaultRoute`:</span><span class="sxs-lookup"><span data-stu-id="f22d1-216">Use the `UseCookieAuthentication` method in the `Configure` method in your *Startup.cs* file before `UseMvc` or `UseMvcWithDefaultRoute`:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions()
{
    AccessDeniedPath = "/Account/Forbidden/",
    AuthenticationScheme = CookieAuthenticationDefaults.AuthenticationScheme,
    AutomaticAuthenticate = true,
    AutomaticChallenge = true,
    LoginPath = "/Account/Unauthorized/"
});
```

<span data-ttu-id="f22d1-217">**CookieAuthenticationOptions オプション**</span><span class="sxs-lookup"><span data-stu-id="f22d1-217">**CookieAuthenticationOptions Options**</span></span>

<span data-ttu-id="f22d1-218">[CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1)認証プロバイダーのオプションを構成するクラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-218">The [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="f22d1-219">オプション</span><span class="sxs-lookup"><span data-stu-id="f22d1-219">Option</span></span> | <span data-ttu-id="f22d1-220">説明</span><span class="sxs-lookup"><span data-stu-id="f22d1-220">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="f22d1-221">AuthenticationScheme</span><span class="sxs-lookup"><span data-stu-id="f22d1-221">AuthenticationScheme</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme?view=aspnetcore-1.1) | <span data-ttu-id="f22d1-222">認証スキームを設定します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-222">Sets the authentication scheme.</span></span> <span data-ttu-id="f22d1-223">`AuthenticationScheme` 認証の複数のインスタンスがあるし、する場合に便利です[特定のスキームで承認](xref:security/authorization/limitingidentitybyscheme)します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-223">`AuthenticationScheme` is useful when there are multiple instances of authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="f22d1-224">設定、`AuthenticationScheme`に`CookieAuthenticationDefaults.AuthenticationScheme`スキームの「クッキー」の値を指定します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-224">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="f22d1-225">スキームを識別する任意の文字列値を指定することができます。</span><span class="sxs-lookup"><span data-stu-id="f22d1-225">You can supply any string value that distinguishes the scheme.</span></span> |
| [<span data-ttu-id="f22d1-226">AutomaticAuthenticate</span><span class="sxs-lookup"><span data-stu-id="f22d1-226">AutomaticAuthenticate</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate?view=aspnetcore-1.1) | <span data-ttu-id="f22d1-227">Cookie 認証が要求ごとに実行、検証し、作成された任意のシリアル化されたプリンシパルを再構築することを示す値を設定します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-227">Sets a value to indicate that the cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span> |
| [<span data-ttu-id="f22d1-228">AutomaticChallenge</span><span class="sxs-lookup"><span data-stu-id="f22d1-228">AutomaticChallenge</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge?view=aspnetcore-1.1) | <span data-ttu-id="f22d1-229">True の場合、認証ミドルウェアは、自動の課題を処理します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-229">If true, the authentication middleware handles automatic challenges.</span></span> <span data-ttu-id="f22d1-230">かどうかは false の場合、認証ミドルウェアが変更されるだけ応答によって明示的に示されたとき、`AuthenticationScheme`します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-230">If false, the authentication middleware only alters responses when explicitly indicated by the `AuthenticationScheme`.</span></span> |
| [<span data-ttu-id="f22d1-231">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="f22d1-231">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.claimsissuer?view=aspnetcore-1.1) | <span data-ttu-id="f22d1-232">使用する発行者、[発行者](/dotnet/api/system.security.claims.claim.issuer)cookie 認証ミドルウェアによって作成されたすべての要求のプロパティ。</span><span class="sxs-lookup"><span data-stu-id="f22d1-232">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication middleware.</span></span> |
| [<span data-ttu-id="f22d1-233">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="f22d1-233">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain?view=aspnetcore-1.1) | <span data-ttu-id="f22d1-234">ドメイン名は、cookie が処理される場所です。</span><span class="sxs-lookup"><span data-stu-id="f22d1-234">The domain name where the cookie is served.</span></span> <span data-ttu-id="f22d1-235">既定では、要求のホスト名です。</span><span class="sxs-lookup"><span data-stu-id="f22d1-235">By default, this is the host name of the request.</span></span> <span data-ttu-id="f22d1-236">ブラウザーでは、一致するホスト名のクッキーのみ機能します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-236">The browser only serves the cookie to a matching host name.</span></span> <span data-ttu-id="f22d1-237">このドメインの任意のホストに使用できる cookie を調整することがあります。</span><span class="sxs-lookup"><span data-stu-id="f22d1-237">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="f22d1-238">たとえば、クッキーのドメインに設定`.contoso.com`で使用できるように`contoso.com`、 `www.contoso.com`、および`staging.www.contoso.com`します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-238">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="f22d1-239">CookieHttpOnly</span><span class="sxs-lookup"><span data-stu-id="f22d1-239">CookieHttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly?view=aspnetcore-1.1) | <span data-ttu-id="f22d1-240">クッキーをサーバーにのみアクセスできる必要があるかどうかを示すフラグです。</span><span class="sxs-lookup"><span data-stu-id="f22d1-240">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="f22d1-241">この値を変更する`false`クッキーにアクセスするクライアント側スクリプトを許可し、アプリが必要 cookie の盗難に、アプリを開くことがあります、[クロス サイト スクリプティング (XSS)](xref:security/cross-site-scripting)脆弱性。</span><span class="sxs-lookup"><span data-stu-id="f22d1-241">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="f22d1-242">既定値は `true` です。</span><span class="sxs-lookup"><span data-stu-id="f22d1-242">The default value is `true`.</span></span> |
| [<span data-ttu-id="f22d1-243">CookiePath</span><span class="sxs-lookup"><span data-stu-id="f22d1-243">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath?view=aspnetcore-1.1) | <span data-ttu-id="f22d1-244">同じホスト名で実行されているアプリを分離するために使用します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-244">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="f22d1-245">実行中のアプリがあれば`/app1`cookie をそのアプリに制限を設定して、`CookiePath`プロパティを`/app1`します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-245">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="f22d1-246">これにより、クッキーが要求に使用可能なだけ`/app1`とその下にあるすべてのアプリ。</span><span class="sxs-lookup"><span data-stu-id="f22d1-246">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="f22d1-247">CookieSecure</span><span class="sxs-lookup"><span data-stu-id="f22d1-247">CookieSecure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure?view=aspnetcore-1.1) | <span data-ttu-id="f22d1-248">作成された cookie を HTTPS に制限するかどうかを示すフラグ (`CookieSecurePolicy.Always`)、HTTP または HTTPS (`CookieSecurePolicy.None`)、または、要求と同じプロトコル (`CookieSecurePolicy.SameAsRequest`)。</span><span class="sxs-lookup"><span data-stu-id="f22d1-248">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="f22d1-249">既定値は `CookieSecurePolicy.SameAsRequest` です。</span><span class="sxs-lookup"><span data-stu-id="f22d1-249">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="f22d1-250">説明</span><span class="sxs-lookup"><span data-stu-id="f22d1-250">Description</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description?view=aspnetcore-1.1) | <span data-ttu-id="f22d1-251">アプリに利用可能になっている認証の種類に関する追加情報。</span><span class="sxs-lookup"><span data-stu-id="f22d1-251">Additional information about the authentication type which is made available to the app.</span></span> |
| [<span data-ttu-id="f22d1-252">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="f22d1-252">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.expiretimespan?view=aspnetcore-1.1) | <span data-ttu-id="f22d1-253">`TimeSpan`認証チケットが期限切れ後。</span><span class="sxs-lookup"><span data-stu-id="f22d1-253">The `TimeSpan` after which the authentication ticket expires.</span></span> <span data-ttu-id="f22d1-254">チケットの有効期限を作成するには、現在の時刻に追加されます。</span><span class="sxs-lookup"><span data-stu-id="f22d1-254">It's added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="f22d1-255">使用する`ExpireTimeSpan`を設定する必要があります`IsPersistent`に`true`で、`AuthenticationProperties`に渡される`SignInAsync`します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-255">To use `ExpireTimeSpan`, you must set `IsPersistent` to `true` in the `AuthenticationProperties` passed to `SignInAsync`.</span></span> <span data-ttu-id="f22d1-256">既定値は 14 日間です。</span><span class="sxs-lookup"><span data-stu-id="f22d1-256">The default value is 14 days.</span></span> |
| [<span data-ttu-id="f22d1-257">SlidingExpiration</span><span class="sxs-lookup"><span data-stu-id="f22d1-257">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-1.1) | <span data-ttu-id="f22d1-258">複数の半分のクッキーの有効期限の日時をリセットするかどうかを示すフラグ、`ExpireTimeSpan`間隔が渡されます。</span><span class="sxs-lookup"><span data-stu-id="f22d1-258">A flag indicating whether the cookie expiration date resets when more than half of the `ExpireTimeSpan` interval has passed.</span></span> <span data-ttu-id="f22d1-259">新しい exipiration 時間は順方向に移動は、現在の日付を指定するだけでなく、`ExpireTimespan`します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-259">The new exipiration time is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="f22d1-260">[絶対クッキーの有効期限](xref:security/authentication/cookie#absolute-cookie-expiration)を使用して設定することができます、`AuthenticationProperties`クラスを呼び出すときに`SignInAsync`します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-260">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="f22d1-261">絶対有効期限は、認証 cookie が有効な時間を制限することで、アプリのセキュリティを強化できます。</span><span class="sxs-lookup"><span data-stu-id="f22d1-261">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="f22d1-262">既定値は `true` です。</span><span class="sxs-lookup"><span data-stu-id="f22d1-262">The default value is `true`.</span></span> |

<span data-ttu-id="f22d1-263">設定`CookieAuthenticationOptions`で Cookie 認証ミドルウェアの`Configure`メソッド。</span><span class="sxs-lookup"><span data-stu-id="f22d1-263">Set `CookieAuthenticationOptions` for the Cookie Authentication Middleware in the `Configure` method:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    ...
});
```

---

## <a name="cookie-policy-middleware"></a><span data-ttu-id="f22d1-264">Cookie のポリシーのミドルウェア</span><span class="sxs-lookup"><span data-stu-id="f22d1-264">Cookie Policy Middleware</span></span>

<span data-ttu-id="f22d1-265">[Cookie のポリシーのミドルウェア](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware)アプリでの cookie ポリシー機能を使用します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-265">[Cookie Policy Middleware](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) enables cookie policy capabilities in an app.</span></span> <span data-ttu-id="f22d1-266">機密性の高い; 順序は、アプリの処理パイプラインにミドルウェアを追加します。その後に、パイプラインに登録されたコンポーネントのみに影響します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-266">Adding the middleware to the app processing pipeline is order sensitive; it only affects components registered after it in the pipeline.</span></span>

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

 <span data-ttu-id="f22d1-267">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) Cookie のポリシーのミドルウェアに提供される cookie を追加または削除されたときに、cookie 処理ハンドラーにクッキーの処理とフックのグローバルの特性を制御することです。</span><span class="sxs-lookup"><span data-stu-id="f22d1-267">The [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) provided to the Cookie Policy Middleware allow you to control global characteristics of cookie processing and hook into cookie processing handlers when cookies are appended or deleted.</span></span>

| <span data-ttu-id="f22d1-268">プロパティ</span><span class="sxs-lookup"><span data-stu-id="f22d1-268">Property</span></span> | <span data-ttu-id="f22d1-269">説明</span><span class="sxs-lookup"><span data-stu-id="f22d1-269">Description</span></span> |
| -------- | ----------- |
| [<span data-ttu-id="f22d1-270">HttpOnly</span><span class="sxs-lookup"><span data-stu-id="f22d1-270">HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.httponly) | <span data-ttu-id="f22d1-271">Cookie は HttpOnly である必要があります、かどうかをフラグを示すです、クッキーをサーバーにのみアクセスできる必要があるかどうかに影響します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-271">Affects whether cookies must be HttpOnly, which is a flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="f22d1-272">既定値は `HttpOnlyPolicy.None` です。</span><span class="sxs-lookup"><span data-stu-id="f22d1-272">The default value is `HttpOnlyPolicy.None`.</span></span> |
| [<span data-ttu-id="f22d1-273">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="f22d1-273">MinimumSameSitePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.minimumsamesitepolicy) | <span data-ttu-id="f22d1-274">Cookie の同じサイト属性 (下記参照) に影響します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-274">Affects the cookie's same-site attribute (see below).</span></span> <span data-ttu-id="f22d1-275">既定値は `SameSiteMode.Lax` です。</span><span class="sxs-lookup"><span data-stu-id="f22d1-275">The default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="f22d1-276">このオプションでは、ASP.NET Core 2.0 以降を使用します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-276">This option is available for ASP.NET Core 2.0+.</span></span> |
| [<span data-ttu-id="f22d1-277">OnAppendCookie</span><span class="sxs-lookup"><span data-stu-id="f22d1-277">OnAppendCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.onappendcookie) | <span data-ttu-id="f22d1-278">Cookie を追加するときに呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="f22d1-278">Called when a cookie is appended.</span></span> |
| [<span data-ttu-id="f22d1-279">OnDeleteCookie</span><span class="sxs-lookup"><span data-stu-id="f22d1-279">OnDeleteCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.ondeletecookie) | <span data-ttu-id="f22d1-280">Cookie が削除されたときに呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="f22d1-280">Called when a cookie is deleted.</span></span> |
| [<span data-ttu-id="f22d1-281">セキュリティで保護します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-281">Secure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.secure) | <span data-ttu-id="f22d1-282">Cookie はセキュリティで保護する必要があるかどうかに影響します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-282">Affects whether cookies must be Secure.</span></span> <span data-ttu-id="f22d1-283">既定値は `CookieSecurePolicy.None` です。</span><span class="sxs-lookup"><span data-stu-id="f22d1-283">The default value is `CookieSecurePolicy.None`.</span></span> |

<span data-ttu-id="f22d1-284">**MinimumSameSitePolicy** (ASP.NET Core 2.0 以降のみ)</span><span class="sxs-lookup"><span data-stu-id="f22d1-284">**MinimumSameSitePolicy** (ASP.NET Core 2.0+ only)</span></span>

<span data-ttu-id="f22d1-285">既定の`MinimumSameSitePolicy`値は`SameSiteMode.Lax`OAuth2 認証を許可するようにします。</span><span class="sxs-lookup"><span data-stu-id="f22d1-285">The default `MinimumSameSitePolicy` value is `SameSiteMode.Lax` to permit OAuth2 authentication.</span></span> <span data-ttu-id="f22d1-286">厳密にポリシーを適用する同じサイトの`SameSiteMode.Strict`、設定、`MinimumSameSitePolicy`します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-286">To strictly enforce a same-site policy of `SameSiteMode.Strict`, set the `MinimumSameSitePolicy`.</span></span> <span data-ttu-id="f22d1-287">ただし、この設定は、OAuth2 やその他のクロス オリジンの認証スキームを区切り、クロス オリジン要求の処理に依存しないようにするアプリの他の種類の cookie のセキュリティのレベルを高めます。</span><span class="sxs-lookup"><span data-stu-id="f22d1-287">Although this setting breaks OAuth2 and other cross-origin authentication schemes, it elevates the level of cookie security for other types of apps that don't rely on cross-origin request processing.</span></span>

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

<span data-ttu-id="f22d1-288">Cookie のポリシーのミドルウェアの設定`MinimumSameSitePolicy`の設定に影響を与える`Cookie.SameSite`で`CookieAuthenticationOptions`次の表に従って設定します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-288">The Cookie Policy Middleware setting for `MinimumSameSitePolicy` can affect your setting of `Cookie.SameSite` in `CookieAuthenticationOptions` settings according to the matrix below.</span></span>

| <span data-ttu-id="f22d1-289">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="f22d1-289">MinimumSameSitePolicy</span></span> | <span data-ttu-id="f22d1-290">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="f22d1-290">Cookie.SameSite</span></span> | <span data-ttu-id="f22d1-291">最終的な Cookie.SameSite 設定</span><span class="sxs-lookup"><span data-stu-id="f22d1-291">Resultant Cookie.SameSite setting</span></span> |
| --------------------- | --------------- | --------------------------------- |
| <span data-ttu-id="f22d1-292">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="f22d1-292">SameSiteMode.None</span></span>     | <span data-ttu-id="f22d1-293">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="f22d1-293">SameSiteMode.None</span></span><br><span data-ttu-id="f22d1-294">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="f22d1-294">SameSiteMode.Lax</span></span><br><span data-ttu-id="f22d1-295">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="f22d1-295">SameSiteMode.Strict</span></span> | <span data-ttu-id="f22d1-296">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="f22d1-296">SameSiteMode.None</span></span><br><span data-ttu-id="f22d1-297">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="f22d1-297">SameSiteMode.Lax</span></span><br><span data-ttu-id="f22d1-298">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="f22d1-298">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="f22d1-299">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="f22d1-299">SameSiteMode.Lax</span></span>      | <span data-ttu-id="f22d1-300">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="f22d1-300">SameSiteMode.None</span></span><br><span data-ttu-id="f22d1-301">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="f22d1-301">SameSiteMode.Lax</span></span><br><span data-ttu-id="f22d1-302">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="f22d1-302">SameSiteMode.Strict</span></span> | <span data-ttu-id="f22d1-303">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="f22d1-303">SameSiteMode.Lax</span></span><br><span data-ttu-id="f22d1-304">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="f22d1-304">SameSiteMode.Lax</span></span><br><span data-ttu-id="f22d1-305">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="f22d1-305">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="f22d1-306">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="f22d1-306">SameSiteMode.Strict</span></span>   | <span data-ttu-id="f22d1-307">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="f22d1-307">SameSiteMode.None</span></span><br><span data-ttu-id="f22d1-308">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="f22d1-308">SameSiteMode.Lax</span></span><br><span data-ttu-id="f22d1-309">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="f22d1-309">SameSiteMode.Strict</span></span> | <span data-ttu-id="f22d1-310">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="f22d1-310">SameSiteMode.Strict</span></span><br><span data-ttu-id="f22d1-311">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="f22d1-311">SameSiteMode.Strict</span></span><br><span data-ttu-id="f22d1-312">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="f22d1-312">SameSiteMode.Strict</span></span> |

## <a name="create-an-authentication-cookie"></a><span data-ttu-id="f22d1-313">認証 cookie を作成します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-313">Create an authentication cookie</span></span>

<span data-ttu-id="f22d1-314">ユーザー情報を保持するクッキーを作成するには、構築する必要があります、 [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal)します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-314">To create a cookie holding user information, you must construct a [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal).</span></span> <span data-ttu-id="f22d1-315">ユーザー情報がシリアル化され、cookie に格納されています。</span><span class="sxs-lookup"><span data-stu-id="f22d1-315">The user information is serialized and stored in the cookie.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f22d1-316">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f22d1-316">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="f22d1-317">作成、 [ClaimsIdentity](/dotnet/api/system.security.claims.claimsidentity)必須[要求](/dotnet/api/system.security.claims.claim)s と呼び出し[SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0)ユーザーがサインインします。</span><span class="sxs-lookup"><span data-stu-id="f22d1-317">Create a [ClaimsIdentity](/dotnet/api/system.security.claims.claimsidentity) with any required [Claim](/dotnet/api/system.security.claims.claim)s and call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) to sign in the user:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f22d1-318">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f22d1-318">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="f22d1-319">呼び出す[SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1)ユーザーがサインインします。</span><span class="sxs-lookup"><span data-stu-id="f22d1-319">Call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) to sign in the user:</span></span>

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity));
```

---

<span data-ttu-id="f22d1-320">`SignInAsync` 暗号化された cookie を作成し、それを現在の応答に追加します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-320">`SignInAsync` creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="f22d1-321">指定しない場合、`AuthenticationScheme`既定のスキームを使用します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-321">If you don't specify an `AuthenticationScheme`, the default scheme is used.</span></span>

<span data-ttu-id="f22d1-322">実際には、使用される暗号化とは、ASP.NET Core の[データ保護](xref:security/data-protection/using-data-protection#security-data-protection-getting-started)システム。</span><span class="sxs-lookup"><span data-stu-id="f22d1-322">Under the covers, the encryption used is ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) system.</span></span> <span data-ttu-id="f22d1-323">複数のマシン、アプリでの負荷分散または web ファームを使用してアプリをホストしているかどうかは、する必要があります[データ保護を構成する](xref:security/data-protection/configuration/overview)アプリ id と同じキー リングを使用します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-323">If you're hosting app on multiple machines, load balancing across apps, or using a web farm, then you must [configure data protection](xref:security/data-protection/configuration/overview) to use the same key ring and app identifier.</span></span>

## <a name="sign-out"></a><span data-ttu-id="f22d1-324">サインアウト</span><span class="sxs-lookup"><span data-stu-id="f22d1-324">Sign out</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f22d1-325">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f22d1-325">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="f22d1-326">現在のユーザーをサインアウトし、その cookie を削除、 [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span><span class="sxs-lookup"><span data-stu-id="f22d1-326">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f22d1-327">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f22d1-327">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="f22d1-328">現在のユーザーをサインアウトし、その cookie を削除、 [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span><span class="sxs-lookup"><span data-stu-id="f22d1-328">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span></span>

```csharp
await HttpContext.Authentication.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

---

<span data-ttu-id="f22d1-329">使用していない場合`CookieAuthenticationDefaults.AuthenticationScheme`(または「クッキー」)、スキーム (たとえば、"ContosoCookie") として、認証プロバイダーを構成するときに使用するスキームを指定します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-329">If you aren't using `CookieAuthenticationDefaults.AuthenticationScheme` (or "Cookies") as the scheme (for example, "ContosoCookie"), supply the scheme you used when configuring the authentication provider.</span></span> <span data-ttu-id="f22d1-330">それ以外の場合、既定のスキームが使用されます。</span><span class="sxs-lookup"><span data-stu-id="f22d1-330">Otherwise, the default scheme is used.</span></span>

## <a name="react-to-back-end-changes"></a><span data-ttu-id="f22d1-331">バックエンドの変更に対応します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-331">React to back-end changes</span></span>

<span data-ttu-id="f22d1-332">Cookie が作成されると、id の 1 つのソースになります。</span><span class="sxs-lookup"><span data-stu-id="f22d1-332">Once a cookie is created, it becomes the single source of identity.</span></span> <span data-ttu-id="f22d1-333">バックエンド システムでユーザーを無効にした場合でも、cookie 認証システムは、この知識を持たないし、ユーザーはまだその cookie が有効な限り、ログインしています。</span><span class="sxs-lookup"><span data-stu-id="f22d1-333">Even if you disable a user in your back-end systems, the cookie authentication system has no knowledge of this, and a user stays logged in as long as their cookie is valid.</span></span>

<span data-ttu-id="f22d1-334">[ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal)イベントでは、ASP.NET Core 2.x または[ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) 1.x をインターセプトし、cookie id の検証のオーバーライドに使用できる ASP.NET Core でのメソッド。</span><span class="sxs-lookup"><span data-stu-id="f22d1-334">The [ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) event in ASP.NET Core 2.x or the [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) method in ASP.NET Core 1.x can be used to intercept and override validation of the cookie identity.</span></span> <span data-ttu-id="f22d1-335">このアプローチでは、失効したユーザーがアプリにアクセスするリスクを軽減します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-335">This approach mitigates the risk of revoked users accessing the app.</span></span>

<span data-ttu-id="f22d1-336">Cookie を検証する方法の 1 つは、ユーザー データベースが変更されたときの追跡に基づいています。</span><span class="sxs-lookup"><span data-stu-id="f22d1-336">One approach to cookie validation is based on keeping track of when the user database has been changed.</span></span> <span data-ttu-id="f22d1-337">ユーザーのクッキーが発行されたので、データベースが変更されていない場合、その cookie が有効である場合、ユーザーを再認証する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="f22d1-337">If the database hasn't been changed since the user's cookie was issued, there's no need to re-authenticate the user if their cookie is still valid.</span></span> <span data-ttu-id="f22d1-338">このシナリオに実装されていると、データベースを実装する`IUserRepository`この例では、格納、`LastChanged`値。</span><span class="sxs-lookup"><span data-stu-id="f22d1-338">To implement this scenario, the database, which is implemented in `IUserRepository` for this example, stores a `LastChanged` value.</span></span> <span data-ttu-id="f22d1-339">すべてのユーザーが、データベースで更新されたときに、`LastChanged`値が現在の時刻に設定します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-339">When any user is updated in the database, the `LastChanged` value is set to the current time.</span></span>

<span data-ttu-id="f22d1-340">データベースの変更に基づいている場合は、cookie を無効にするには、`LastChanged`値には、cookie を作成、`LastChanged`現在を含む要求`LastChanged`データベースから値。</span><span class="sxs-lookup"><span data-stu-id="f22d1-340">In order to invalidate a cookie when the database changes based on the `LastChanged` value, create the cookie with a `LastChanged` claim containing the current `LastChanged` value from the database:</span></span>

```csharp
var claims = new List<Claim>
{
    new Claim(ClaimTypes.Name, user.Email),
    new Claim("LastChanged", {Database Value})
};

var claimsIdentity = new ClaimsIdentity(
    claims, 
    CookieAuthenticationDefaults.AuthenticationScheme);

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme, 
    new ClaimsPrincipal(claimsIdentity));
```

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f22d1-341">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f22d1-341">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="f22d1-342">オーバーライドを実装するために、`ValidatePrincipal`から派生したクラスでは、次のシグネチャを持つメソッドを書き込み、イベント[CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span><span class="sxs-lookup"><span data-stu-id="f22d1-342">To implement an override for the `ValidatePrincipal` event, write a method with the following signature in a class that you derive from [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span></span>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

<span data-ttu-id="f22d1-343">例では、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="f22d1-343">An example looks like the following:</span></span>

```csharp
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Authentication;
using Microsoft.AspNetCore.Authentication.Cookies;

public class CustomCookieAuthenticationEvents : CookieAuthenticationEvents
{
    private readonly IUserRepository _userRepository;

    public CustomCookieAuthenticationEvents(IUserRepository userRepository)
    {
        // Get the database from registered DI services.
        _userRepository = userRepository;
    }

    public override async Task ValidatePrincipal(CookieValidatePrincipalContext context)
    {
        var userPrincipal = context.Principal;

        // Look for the LastChanged claim.
        var lastChanged = (from c in userPrincipal.Claims
                           where c.Type == "LastChanged"
                           select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !_userRepository.ValidateLastChanged(lastChanged))
        {
            context.RejectPrincipal();

            await context.HttpContext.SignOutAsync(
                CookieAuthenticationDefaults.AuthenticationScheme);
        }
    }
}
```

<span data-ttu-id="f22d1-344">クッキーにサービスを登録中に、イベント インスタンスを登録、`ConfigureServices`メソッド。</span><span class="sxs-lookup"><span data-stu-id="f22d1-344">Register the events instance during cookie service registration in the `ConfigureServices` method.</span></span> <span data-ttu-id="f22d1-345">スコープを持つサービスの登録を提供、`CustomCookieAuthenticationEvents`クラス。</span><span class="sxs-lookup"><span data-stu-id="f22d1-345">Provide a scoped service registration for your `CustomCookieAuthenticationEvents` class:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f22d1-346">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f22d1-346">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="f22d1-347">オーバーライドを実装するために、`ValidateAsync`イベントでは、次のシグネチャを持つメソッドを作成します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-347">To implement an override for the `ValidateAsync` event, write a method with the following signature:</span></span>

```csharp
ValidateAsync(CookieValidatePrincipalContext)
```

<span data-ttu-id="f22d1-348">ASP.NET Core Identity では、このチェックを実装の一部としてその[SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync)します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-348">ASP.NET Core Identity implements this check as part of its [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync).</span></span> <span data-ttu-id="f22d1-349">例では、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="f22d1-349">An example looks like the following:</span></span>

```csharp
public static class LastChangedValidator
{
    public static async Task ValidateAsync(CookieValidatePrincipalContext context)
    {
        // Pull database from registered DI services.
        var userRepository = 
            context.HttpContext.RequestServices
                .GetRequiredService<IUserRepository>();
        var userPrincipal = context.Principal;

        // Look for the last changed claim.
        var lastChanged = (from c in userPrincipal.Claims
                           where c.Type == "LastChanged"
                           select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !userRepository.ValidateLastChanged(lastChanged))
        {
            context.RejectPrincipal();

            await context.HttpContext.SignOutAsync(
                CookieAuthenticationDefaults.AuthenticationScheme);
        }
    }
}
```

<span data-ttu-id="f22d1-350">Cookie 認証の構成時にイベントを登録、`Configure`メソッド。</span><span class="sxs-lookup"><span data-stu-id="f22d1-350">Register the event during cookie authentication configuration in the `Configure` method:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    Events = new CookieAuthenticationEvents
    {
        OnValidatePrincipal = LastChangedValidator.ValidateAsync
    }
});
```

---

<span data-ttu-id="f22d1-351">ユーザーの名前が更新される場合を考えてみましょう&mdash;判断を任意の方法でセキュリティには影響しません。</span><span class="sxs-lookup"><span data-stu-id="f22d1-351">Consider a situation in which the user's name is updated &mdash; a decision that doesn't affect security in any way.</span></span> <span data-ttu-id="f22d1-352">非破壊的ユーザー プリンシパルを更新する場合は、呼び出す`context.ReplacePrincipal`設定と、`context.ShouldRenew`プロパティを`true`します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-352">If you want to non-destructively update the user principal, call `context.ReplacePrincipal` and set the `context.ShouldRenew` property to `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="f22d1-353">ここで説明したアプローチは、要求ごとにトリガーされます。</span><span class="sxs-lookup"><span data-stu-id="f22d1-353">The approach described here is triggered on every request.</span></span> <span data-ttu-id="f22d1-354">これにより、アプリのパフォーマンスが大幅に低下します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-354">This can result in a large performance penalty for the app.</span></span>

## <a name="persistent-cookies"></a><span data-ttu-id="f22d1-355">永続的な cookie</span><span class="sxs-lookup"><span data-stu-id="f22d1-355">Persistent cookies</span></span>

<span data-ttu-id="f22d1-356">Cookie がブラウザー セッション間で永続化することができます。</span><span class="sxs-lookup"><span data-stu-id="f22d1-356">You may want the cookie to persist across browser sessions.</span></span> <span data-ttu-id="f22d1-357">この永続化は、明示的なユーザーによる同意は、「記憶」チェック ボックスで、ログインまたは同様のメカニズムでのみ有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="f22d1-357">This persistence should only be enabled with explicit user consent with a "Remember Me" checkbox on login or a similar mechanism.</span></span> 

<span data-ttu-id="f22d1-358">次のコード スニペットでは、id およびブラウザー クロージャでは存続する対応する cookie を作成します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-358">The following code snippet creates an identity and corresponding cookie that survives through browser closures.</span></span> <span data-ttu-id="f22d1-359">以前に構成された、スライド式有効期限の設定が受け入れられます。</span><span class="sxs-lookup"><span data-stu-id="f22d1-359">Any sliding expiration settings previously configured are honored.</span></span> <span data-ttu-id="f22d1-360">ブラウザーに cookie の期限切れのブラウザーが閉じられたときに、再起動の後、cookie をクリアします。</span><span class="sxs-lookup"><span data-stu-id="f22d1-360">If the cookie expires while the browser is closed, the browser clears the cookie once it's restarted.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f22d1-361">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f22d1-361">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="f22d1-362">[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0)でクラスが存在する、`Microsoft.AspNetCore.Authentication`名前空間。</span><span class="sxs-lookup"><span data-stu-id="f22d1-362">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) class resides in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f22d1-363">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f22d1-363">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="f22d1-364">[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1)でクラスが存在する、`Microsoft.AspNetCore.Http.Authentication`名前空間。</span><span class="sxs-lookup"><span data-stu-id="f22d1-364">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) class resides in the `Microsoft.AspNetCore.Http.Authentication` namespace.</span></span>

---

## <a name="absolute-cookie-expiration"></a><span data-ttu-id="f22d1-365">絶対クッキーの有効期限</span><span class="sxs-lookup"><span data-stu-id="f22d1-365">Absolute cookie expiration</span></span>

<span data-ttu-id="f22d1-366">絶対有効期限を設定する`ExpiresUtc`します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-366">You can set an absolute expiration time with `ExpiresUtc`.</span></span> <span data-ttu-id="f22d1-367">設定する必要があります`IsPersistent`、それ以外の`ExpiresUtc`は無視され、単一セッション cookie が作成されます。</span><span class="sxs-lookup"><span data-stu-id="f22d1-367">You must also set `IsPersistent`; otherwise, `ExpiresUtc` is ignored and a single-session cookie is created.</span></span> <span data-ttu-id="f22d1-368">ときに`ExpiresUtc`が設定されて`SignInAsync`の値よりも優先、`ExpireTimeSpan`オプションの`CookieAuthenticationOptions`場合は、設定します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-368">When `ExpiresUtc` is set on `SignInAsync`, it overrides the value of the `ExpireTimeSpan` option of `CookieAuthenticationOptions`, if set.</span></span>

<span data-ttu-id="f22d1-369">次のコード スニペットは、id と対応するクッキーを 20 分間継続を作成します。</span><span class="sxs-lookup"><span data-stu-id="f22d1-369">The following code snippet creates an identity and corresponding cookie that lasts for 20 minutes.</span></span> <span data-ttu-id="f22d1-370">これには、以前に構成された、スライド式有効期限の設定が無視されます。</span><span class="sxs-lookup"><span data-stu-id="f22d1-370">This ignores any sliding expiration settings previously configured.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f22d1-371">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f22d1-371">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true,
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f22d1-372">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f22d1-372">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true,
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

---

## <a name="additional-resources"></a><span data-ttu-id="f22d1-373">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="f22d1-373">Additional resources</span></span>

* [<span data-ttu-id="f22d1-374">Auth 2.0 の変更/移行のお知らせ</span><span class="sxs-lookup"><span data-stu-id="f22d1-374">Auth 2.0 Changes / Migration Announcement</span></span>](https://github.com/aspnet/Announcements/issues/262)
* <xref:security/authorization/limitingidentitybyscheme>
* <xref:security/authorization/claims>
* [<span data-ttu-id="f22d1-375">ポリシー ベースのロールのチェック</span><span class="sxs-lookup"><span data-stu-id="f22d1-375">Policy-based role checks</span></span>](xref:security/authorization/roles#policy-based-role-checks)
* <xref:host-and-deploy/web-farm>
