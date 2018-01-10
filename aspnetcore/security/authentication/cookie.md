---
title: "ASP.NET Core Identity なしで認証に Cookie を使用します。"
author: rick-anderson
description: "ASP.NET Core Id なしの cookie 認証を使用しての説明"
keywords: "ASP.NET Core、cookie"
ms.author: riande
manager: wpickett
ms.date: 10/11/2017
ms.topic: article
ms.assetid: 2bdcbf95-8d9d-4537-a4a0-a5ee439dcb62
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/cookie
ms.openlocfilehash: ee660667251ec4a64f2b3e83f39214e9defcea03
ms.sourcegitcommit: 2d23ea501e0213bbacf65298acf1c8bd17209540
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/09/2018
---
# <a name="using-cookie-authentication-without-aspnet-core-identity"></a><span data-ttu-id="02936-104">ASP.NET Core Identity なしで認証に Cookie を使用します。</span><span class="sxs-lookup"><span data-stu-id="02936-104">Using Cookie Authentication without ASP.NET Core Identity</span></span>

<span data-ttu-id="02936-105">によって[Rick Anderson](https://twitter.com/RickAndMSFT)と[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="02936-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="02936-106">以前の認証のトピックで説明したように[ASP.NET Core Id](xref:security/authentication/identity) 、フル機能の完全な認証プロバイダーを作成してログインを維持するため、します。</span><span class="sxs-lookup"><span data-stu-id="02936-106">As you've seen in the earlier authentication topics, [ASP.NET Core Identity](xref:security/authentication/identity) is a complete, full-featured authentication provider for creating and maintaining logins.</span></span> <span data-ttu-id="02936-107">ただし、cookie ベースの認証時に、独自のカスタム認証ロジックを使用することがあります。</span><span class="sxs-lookup"><span data-stu-id="02936-107">However, you may want to use your own custom authentication logic with cookie-based authentication at times.</span></span> <span data-ttu-id="02936-108">ASP.NET Core Id せず、スタンドアロンの認証プロバイダーとしては、クッキー ベースの認証を使用できます。</span><span class="sxs-lookup"><span data-stu-id="02936-108">You can use cookie-based authentication as a standalone authentication provider without ASP.NET Core Identity.</span></span>

<span data-ttu-id="02936-109">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="02936-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="02936-110">ASP.NET Core から移行する cookie ベースの認証の詳細について 1.x 2.0 を参照してください[移行認証および ASP.NET Core 2.0 トピック (Cookie ベースの認証) を Id](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication)です。</span><span class="sxs-lookup"><span data-stu-id="02936-110">For information on migrating cookie-based authentication from ASP.NET Core 1.x to 2.0, see [Migrating Authentication and Identity to ASP.NET Core 2.0 topic (Cookie-based Authentication)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).</span></span>

## <a name="configuration"></a><span data-ttu-id="02936-111">構成</span><span class="sxs-lookup"><span data-stu-id="02936-111">Configuration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="02936-112">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="02936-112">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="02936-113">使用しない場合、 [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage)、バージョン 2.0 以降のインストール、 [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) NuGet パッケージです。</span><span class="sxs-lookup"><span data-stu-id="02936-113">If you aren't using the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), install version 2.0+ of the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) NuGet package.</span></span>

<span data-ttu-id="02936-114">`ConfigureServices`メソッドを使用して、認証ミドルウェア サービスを作成、`AddAuthentication`と`AddCookie`メソッド。</span><span class="sxs-lookup"><span data-stu-id="02936-114">In the `ConfigureServices` method, create the Authentication Middleware service with the `AddAuthentication` and `AddCookie` methods:</span></span>

[!code-csharp[Main](cookie/sample/Startup.cs?name=snippet1)]

<span data-ttu-id="02936-115">`AuthenticationScheme`渡される`AddAuthentication`アプリの既定の認証スキームを設定します。</span><span class="sxs-lookup"><span data-stu-id="02936-115">`AuthenticationScheme` passed to `AddAuthentication` sets the default authentication scheme for the app.</span></span> <span data-ttu-id="02936-116">`AuthenticationScheme`cookie 認証の複数のインスタンスがあるし、する場合に便利です[、特定のスキームの承認](xref:security/authorization/limitingidentitybyscheme)です。</span><span class="sxs-lookup"><span data-stu-id="02936-116">`AuthenticationScheme` is useful when there are multiple instances of cookie authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="02936-117">設定、`AuthenticationScheme`に`CookieAuthenticationDefaults.AuthenticationScheme`スキームの"Cookie"の値を提供します。</span><span class="sxs-lookup"><span data-stu-id="02936-117">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="02936-118">スキームを区別する任意の文字列値を指定することができます。</span><span class="sxs-lookup"><span data-stu-id="02936-118">You can supply any string value that distinguishes the scheme.</span></span>

<span data-ttu-id="02936-119">`Configure`メソッドを使用して、`UseAuthentication`設定認証ミドルウェアを呼び出すメソッドを`HttpContext.User`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="02936-119">In the `Configure` method, use the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="02936-120">呼び出す、`UseAuthentication`メソッドを呼び出す前に`UseMvcWithDefaultRoute`または`UseMvc`:</span><span class="sxs-lookup"><span data-stu-id="02936-120">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[Main](cookie/sample/Startup.cs?name=snippet2)]

<span data-ttu-id="02936-121">**AddCookie オプション**</span><span class="sxs-lookup"><span data-stu-id="02936-121">**AddCookie Options**</span></span>

<span data-ttu-id="02936-122">[CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0)認証プロバイダー オプションを構成するクラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="02936-122">The [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="02936-123">オプション</span><span class="sxs-lookup"><span data-stu-id="02936-123">Option</span></span> | <span data-ttu-id="02936-124">説明</span><span class="sxs-lookup"><span data-stu-id="02936-124">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="02936-125">AccessDeniedPath</span><span class="sxs-lookup"><span data-stu-id="02936-125">AccessDeniedPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath?view=aspnetcore-2.0) | <span data-ttu-id="02936-126">302 検出 (URL リダイレクト) を指定するパスを提供によってトリガーされたときに`HttpContext.ForbidAsync`です。</span><span class="sxs-lookup"><span data-stu-id="02936-126">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ForbidAsync`.</span></span> <span data-ttu-id="02936-127">既定値は `/Account/AccessDenied` です。</span><span class="sxs-lookup"><span data-stu-id="02936-127">The default value is `/Account/AccessDenied`.</span></span> |
| [<span data-ttu-id="02936-128">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="02936-128">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer?view=aspnetcore-2.0) | <span data-ttu-id="02936-129">使用する発行者、[発行者](/dotnet/api/system.security.claims.claim.issuer)cookie 認証サービスによって作成された任意の信頼性情報のプロパティです。</span><span class="sxs-lookup"><span data-stu-id="02936-129">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication service.</span></span> |
| [<span data-ttu-id="02936-130">Cookie.Domain</span><span class="sxs-lookup"><span data-stu-id="02936-130">Cookie.Domain</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain?view=aspnetcore-2.0) | <span data-ttu-id="02936-131">Cookie が提供されたドメイン名です。</span><span class="sxs-lookup"><span data-stu-id="02936-131">The domain name where the cookie is served.</span></span> <span data-ttu-id="02936-132">既定では、これは、要求のホスト名です。</span><span class="sxs-lookup"><span data-stu-id="02936-132">By default, this is the host name of the request.</span></span> <span data-ttu-id="02936-133">ブラウザーはのみ、一致するホスト名に、要求で cookie を送信します。</span><span class="sxs-lookup"><span data-stu-id="02936-133">The browser only sends the cookie in requests to a matching host name.</span></span> <span data-ttu-id="02936-134">任意のホストに使用できる cookie は、ドメイン内にこれを調整することもできます。</span><span class="sxs-lookup"><span data-stu-id="02936-134">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="02936-135">たとえば、cookie のドメインに設定`.contoso.com`で使用できるように`contoso.com`、 `www.contoso.com`、および`staging.www.contoso.com`です。</span><span class="sxs-lookup"><span data-stu-id="02936-135">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="02936-136">Cookie.Expiration</span><span class="sxs-lookup"><span data-stu-id="02936-136">Cookie.Expiration</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.expiration?view=aspnetcore-2.0) | <span data-ttu-id="02936-137">取得または cookie の有効期間を設定します。</span><span class="sxs-lookup"><span data-stu-id="02936-137">Gets or sets the lifespan of a cookie.</span></span> <span data-ttu-id="02936-138">現時点では、このキャッシュなしでのオプションし、ASP.NET Core 2.1 以降で廃止になります。</span><span class="sxs-lookup"><span data-stu-id="02936-138">Currently, this option no-ops and will become obsolete in ASP.NET Core 2.1+.</span></span> <span data-ttu-id="02936-139">使用して、 `ExpireTimeSpan` cookie の有効期限を設定するオプションです。</span><span class="sxs-lookup"><span data-stu-id="02936-139">Use the `ExpireTimeSpan` option to set cookie expiration.</span></span> <span data-ttu-id="02936-140">詳細については、次を参照してください。 [CookieAuthenticationOptions.Cookie.Expiration の動作が明確](https://github.com/aspnet/Security/issues/1293)です。</span><span class="sxs-lookup"><span data-stu-id="02936-140">For more information, see [Clarify behavior of CookieAuthenticationOptions.Cookie.Expiration](https://github.com/aspnet/Security/issues/1293).</span></span> |
| [<span data-ttu-id="02936-141">Cookie.HttpOnly</span><span class="sxs-lookup"><span data-stu-id="02936-141">Cookie.HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly?view=aspnetcore-2.0) | <span data-ttu-id="02936-142">クッキーはサーバーにのみアクセスできるかどうかを示すフラグ。</span><span class="sxs-lookup"><span data-stu-id="02936-142">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="02936-143">この値を変更する`false`cookie にアクセスするクライアント側のスクリプトを許可し、アプリが必要 cookie の盗難にアプリを開きます可能性があります、[クロス サイト スクリプト (XSS)](xref:security/cross-site-scripting)脆弱性。</span><span class="sxs-lookup"><span data-stu-id="02936-143">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="02936-144">既定値は `true` です。</span><span class="sxs-lookup"><span data-stu-id="02936-144">The default value is `true`.</span></span> |
| [<span data-ttu-id="02936-145">Cookie.Name</span><span class="sxs-lookup"><span data-stu-id="02936-145">Cookie.Name</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name?view=aspnetcore-2.0) | <span data-ttu-id="02936-146">Cookie の名前を設定します。</span><span class="sxs-lookup"><span data-stu-id="02936-146">Sets the name of the cookie.</span></span> |
| [<span data-ttu-id="02936-147">Cookie.Path</span><span class="sxs-lookup"><span data-stu-id="02936-147">Cookie.Path</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path?view=aspnetcore-2.0) | <span data-ttu-id="02936-148">同じホスト名で実行されているアプリを分離するために使用します。</span><span class="sxs-lookup"><span data-stu-id="02936-148">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="02936-149">実行されているアプリがあれば`/app1`cookie をそのアプリに制限を設定して、`CookiePath`プロパティを`/app1`です。</span><span class="sxs-lookup"><span data-stu-id="02936-149">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="02936-150">これにより、cookie が要求に使用可能なのみ`/app1`とその下のすべてのアプリです。</span><span class="sxs-lookup"><span data-stu-id="02936-150">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="02936-151">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="02936-151">Cookie.SameSite</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite?view=aspnetcore-2.0) | <span data-ttu-id="02936-152">ブラウザーが同じサイトの要求のみに接続する cookie を許可するかどうかを指定 (`SameSiteMode.Strict`) またはセーフである HTTP メソッドと同じサイトの要求を使用してクロスサイト リクエスト (`SameSiteMode.Lax`)。</span><span class="sxs-lookup"><span data-stu-id="02936-152">Indicates whether the browser should allow the cookie to be attached to same-site requests only (`SameSiteMode.Strict`) or cross-site requests using safe HTTP methods and same-site requests (`SameSiteMode.Lax`).</span></span> <span data-ttu-id="02936-153">設定すると`SameSiteMode.None`cookie のヘッダーの値が設定されていません。</span><span class="sxs-lookup"><span data-stu-id="02936-153">When set to `SameSiteMode.None`, the cookie header value isn't set.</span></span> <span data-ttu-id="02936-154">なお[Cookie ポリシー ミドルウェア](#cookie-policy-middleware)指定した値を上書きする可能性があります。</span><span class="sxs-lookup"><span data-stu-id="02936-154">Note that [Cookie Policy Middleware](#cookie-policy-middleware) might overwrite the value that you provide.</span></span> <span data-ttu-id="02936-155">既定値は、OAuth 認証をサポートする`SameSiteMode.Lax`です。</span><span class="sxs-lookup"><span data-stu-id="02936-155">To support OAuth authentication, the default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="02936-156">詳細については、次を参照してください。 [SameSite クッキーのポリシーのための別の OAuth 認証](https://github.com/aspnet/Security/issues/1231)です。</span><span class="sxs-lookup"><span data-stu-id="02936-156">For more information, see [OAuth authentication broken due to SameSite cookie policy](https://github.com/aspnet/Security/issues/1231).</span></span> |
| [<span data-ttu-id="02936-157">Cookie.SecurePolicy</span><span class="sxs-lookup"><span data-stu-id="02936-157">Cookie.SecurePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy?view=aspnetcore-2.0) | <span data-ttu-id="02936-158">作成された cookie を HTTPS に限られますかどうかを示すフラグ (`CookieSecurePolicy.Always`)、HTTP または HTTPS (`CookieSecurePolicy.None`)、または要求と同じプロトコル (`CookieSecurePolicy.SameAsRequest`)。</span><span class="sxs-lookup"><span data-stu-id="02936-158">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="02936-159">既定値は `CookieSecurePolicy.SameAsRequest` です。</span><span class="sxs-lookup"><span data-stu-id="02936-159">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="02936-160">DataProtectionProvider</span><span class="sxs-lookup"><span data-stu-id="02936-160">DataProtectionProvider</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0) | <span data-ttu-id="02936-161">セット、`DataProtectionProvider`の既定の作成に使用される`TicketDataFormat`です。</span><span class="sxs-lookup"><span data-stu-id="02936-161">Sets the `DataProtectionProvider` that's used to create the default `TicketDataFormat`.</span></span> <span data-ttu-id="02936-162">場合、`TicketDataFormat`プロパティが設定されて、`DataProtectionProvider`オプションは使用されません。</span><span class="sxs-lookup"><span data-stu-id="02936-162">If the `TicketDataFormat` property is set, the `DataProtectionProvider` option isn't used.</span></span> <span data-ttu-id="02936-163">指定しないと、アプリの既定のデータ保護プロバイダーが使用されます。</span><span class="sxs-lookup"><span data-stu-id="02936-163">If not provided, the app's default data protection provider is used.</span></span> |
| [<span data-ttu-id="02936-164">イベント</span><span class="sxs-lookup"><span data-stu-id="02936-164">Events</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events?view=aspnetcore-2.0) | <span data-ttu-id="02936-165">ハンドラーは、特定の処理時点で、アプリのコントロールを提供するプロバイダーのメソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="02936-165">The handler calls methods on the provider that give the app control at certain processing points.</span></span> <span data-ttu-id="02936-166">場合`Events`は提供された場合、既定のインスタンスが指定されたメソッドが呼び出されたときに何も実行しません。</span><span class="sxs-lookup"><span data-stu-id="02936-166">If `Events` aren't provided, a default instance is supplied that does nothing when the methods are called.</span></span> |
| [<span data-ttu-id="02936-167">EventsType</span><span class="sxs-lookup"><span data-stu-id="02936-167">EventsType</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype?view=aspnetcore-2.0) | <span data-ttu-id="02936-168">取得するサービスの種類として使用される、`Events`プロパティではなくインスタンス。</span><span class="sxs-lookup"><span data-stu-id="02936-168">Used as the service type to get the `Events` instance instead of the property.</span></span> |
| [<span data-ttu-id="02936-169">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="02936-169">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan?view=aspnetcore-2.0) | <span data-ttu-id="02936-170">`TimeSpan` Cookie 内に格納する認証チケットが期限切れの後にします。</span><span class="sxs-lookup"><span data-stu-id="02936-170">The `TimeSpan` after which the authentication ticket stored inside the cookie expires.</span></span> <span data-ttu-id="02936-171">`ExpireTimeSpan`チケットの有効期限を作成するには、現在の時刻に追加されます。</span><span class="sxs-lookup"><span data-stu-id="02936-171">`ExpireTimeSpan` is added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="02936-172">`ExpiredTimeSpan`値は常に暗号化された認証チケットがサーバーで検証に入ります。</span><span class="sxs-lookup"><span data-stu-id="02936-172">The `ExpiredTimeSpan` value always goes into the encrypted AuthTicket verified by the server.</span></span> <span data-ttu-id="02936-173">入りますが、 [Set-cookie](https://tools.ietf.org/html/rfc6265#section-4.1)場合にのみ、ヘッダー、`IsPersistent`が設定されています。</span><span class="sxs-lookup"><span data-stu-id="02936-173">It may also go into the [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) header, but only if `IsPersistent` is set.</span></span> <span data-ttu-id="02936-174">設定する`IsPersistent`に`true`、構成、 [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties)に渡される`SignInAsync`です。</span><span class="sxs-lookup"><span data-stu-id="02936-174">To set `IsPersistent` to `true`, configure the [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) passed to `SignInAsync`.</span></span> <span data-ttu-id="02936-175">既定値の`ExpireTimeSpan`は 14 日間です。</span><span class="sxs-lookup"><span data-stu-id="02936-175">The default value of `ExpireTimeSpan` is 14 days.</span></span> |
| [<span data-ttu-id="02936-176">LoginPath</span><span class="sxs-lookup"><span data-stu-id="02936-176">LoginPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath?view=aspnetcore-2.0) | <span data-ttu-id="02936-177">302 検出 (URL リダイレクト) を指定するパスを提供によってトリガーされたときに`HttpContext.ChallengeAsync`です。</span><span class="sxs-lookup"><span data-stu-id="02936-177">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ChallengeAsync`.</span></span> <span data-ttu-id="02936-178">401 を生成した現在の URL に追加、`LoginPath`によってという名前のクエリ文字列パラメーターとして、`ReturnUrlParameter`です。</span><span class="sxs-lookup"><span data-stu-id="02936-178">The current URL that generated the 401 is added to the `LoginPath` as a query string parameter named by the `ReturnUrlParameter`.</span></span> <span data-ttu-id="02936-179">要求を 1 回、`LoginPath`新しいサインイン ユーザーの許可、`ReturnUrlParameter`値を使用して、元の unauthorized ステータス コードの原因となった URL にブラウザーをリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="02936-179">Once a request to the `LoginPath` grants a new sign-in identity, the `ReturnUrlParameter` value is used to redirect the browser back to the URL that caused the original unauthorized status code.</span></span> <span data-ttu-id="02936-180">既定値は `/Account/Login` です。</span><span class="sxs-lookup"><span data-stu-id="02936-180">The default value is `/Account/Login`.</span></span> |
| [<span data-ttu-id="02936-181">LogoutPath</span><span class="sxs-lookup"><span data-stu-id="02936-181">LogoutPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath?view=aspnetcore-2.0) | <span data-ttu-id="02936-182">場合、`LogoutPath`が、そのパスへの要求をリダイレクトし、ハンドラーに提供されるの値に基づいて、`ReturnUrlParameter`です。</span><span class="sxs-lookup"><span data-stu-id="02936-182">If the `LogoutPath` is provided to the handler, then a request to that path redirects based on the value of the `ReturnUrlParameter`.</span></span> <span data-ttu-id="02936-183">既定値は `/Account/Logout` です。</span><span class="sxs-lookup"><span data-stu-id="02936-183">The default value is `/Account/Logout`.</span></span> |
| [<span data-ttu-id="02936-184">ReturnUrlParameter</span><span class="sxs-lookup"><span data-stu-id="02936-184">ReturnUrlParameter</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter?view=aspnetcore-2.0) | <span data-ttu-id="02936-185">302 Found (URL リダイレクト) 応答のハンドラーで追加されるクエリ文字列パラメーターの名前を決定します。</span><span class="sxs-lookup"><span data-stu-id="02936-185">Determines the name of the query string parameter that's appended by the handler for a 302 Found (URL redirect) response.</span></span> <span data-ttu-id="02936-186">`ReturnUrlParameter`要求を受信したときに使用、`LoginPath`または`LogoutPath`ログインまたはログアウト アクションが実行された後、元の URL をブラウザーに戻ります。</span><span class="sxs-lookup"><span data-stu-id="02936-186">`ReturnUrlParameter` is used when a request arrives on the `LoginPath` or `LogoutPath` to return the browser to the original URL after the login or logout action is performed.</span></span> <span data-ttu-id="02936-187">既定値は `ReturnUrl` です。</span><span class="sxs-lookup"><span data-stu-id="02936-187">The default value is `ReturnUrl`.</span></span> |
| [<span data-ttu-id="02936-188">SessionStore</span><span class="sxs-lookup"><span data-stu-id="02936-188">SessionStore</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore?view=aspnetcore-2.0) | <span data-ttu-id="02936-189">要求間で id を格納するために使用、省略可能なコンテナーです。</span><span class="sxs-lookup"><span data-stu-id="02936-189">An optional container used to store identity across requests.</span></span> <span data-ttu-id="02936-190">使用すると、セッション識別子のみがクライアントに送信されます。</span><span class="sxs-lookup"><span data-stu-id="02936-190">When used, only a session identifier is sent to the client.</span></span> <span data-ttu-id="02936-191">`SessionStore`大規模な id を持つ潜在的な問題を軽減するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="02936-191">`SessionStore` can be used to mitigate potential problems with large identities.</span></span> |
| [<span data-ttu-id="02936-192">SlidingExpiration</span><span class="sxs-lookup"><span data-stu-id="02936-192">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-2.0) | <span data-ttu-id="02936-193">かどうかは、更新された期限で新しい cookie を動的に発行する必要がありますを示すフラグ。</span><span class="sxs-lookup"><span data-stu-id="02936-193">A flag indicating if a new cookie with an updated expiration time should be issued dynamically.</span></span> <span data-ttu-id="02936-194">ここで、現在 cookie の有効期間が 50% を超える有効期限が切れてすべての要求の可能性があります。</span><span class="sxs-lookup"><span data-stu-id="02936-194">This can happen on any request where the current cookie expiration period is more than 50% expired.</span></span> <span data-ttu-id="02936-195">新しい有効期限が前方に移動する、現在の日付と`ExpireTimespan`です。</span><span class="sxs-lookup"><span data-stu-id="02936-195">The new expiration date is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="02936-196">[絶対 cookie の有効期限](xref:security/authentication/cookie#absolute-cookie-expiration)を使用して設定することができます、`AuthenticationProperties`クラスを呼び出すときに`SignInAsync`です。</span><span class="sxs-lookup"><span data-stu-id="02936-196">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="02936-197">絶対有効期限は、認証 cookie が有効な時間を制限することによって、アプリのセキュリティを強化できます。</span><span class="sxs-lookup"><span data-stu-id="02936-197">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="02936-198">既定値は `true` です。</span><span class="sxs-lookup"><span data-stu-id="02936-198">The default value is `true`.</span></span> |
| [<span data-ttu-id="02936-199">TicketDataFormat</span><span class="sxs-lookup"><span data-stu-id="02936-199">TicketDataFormat</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat?view=aspnetcore-2.0) | <span data-ttu-id="02936-200">`TicketDataFormat`を保護し、id および cookie の値に格納されているその他のプロパティの保護を解除するために使用します。</span><span class="sxs-lookup"><span data-stu-id="02936-200">The `TicketDataFormat` is used to protect and unprotect the identity and other properties that are stored in the cookie value.</span></span> <span data-ttu-id="02936-201">指定されていない場合、`TicketDataFormat`を使用して作成されて、 [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0)です。</span><span class="sxs-lookup"><span data-stu-id="02936-201">If not provided, a `TicketDataFormat` is created using the [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0).</span></span> |
| [<span data-ttu-id="02936-202">検証</span><span class="sxs-lookup"><span data-stu-id="02936-202">Validate</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.validate?view=aspnetcore-2.0) | <span data-ttu-id="02936-203">オプションが有効なことを確認するメソッド。</span><span class="sxs-lookup"><span data-stu-id="02936-203">Method that checks that the options are valid.</span></span> |

<span data-ttu-id="02936-204">設定`CookieAuthenticationOptions`での認証サービスの構成で、`ConfigureServices`メソッド。</span><span class="sxs-lookup"><span data-stu-id="02936-204">Set `CookieAuthenticationOptions` in the service configuration for authentication in the `ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="02936-205">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="02936-205">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="02936-206">ASP.NET Core 1.x 使用 cookie[ミドルウェア](xref:fundamentals/middleware)暗号化された cookie にユーザー プリンシパルをシリアル化します。</span><span class="sxs-lookup"><span data-stu-id="02936-206">ASP.NET Core 1.x uses cookie [middleware](xref:fundamentals/middleware) that serializes a user principal into an encrypted cookie.</span></span> <span data-ttu-id="02936-207">以降の要求でクッキーが検証され、プリンシパルが再作成されに割り当てられている、`HttpContext.User`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="02936-207">On subsequent requests, the cookie is validated, and the principal is recreated and assigned to the `HttpContext.User` property.</span></span>

<span data-ttu-id="02936-208">インストール、 [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/)プロジェクトでの NuGet パッケージです。</span><span class="sxs-lookup"><span data-stu-id="02936-208">Install the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) NuGet package in your project.</span></span> <span data-ttu-id="02936-209">このパッケージには、cookie ミドルウェアが含まれています。</span><span class="sxs-lookup"><span data-stu-id="02936-209">This package contains the cookie middleware.</span></span>

<span data-ttu-id="02936-210">使用して、`UseCookieAuthentication`メソッドで、`Configure`メソッドで、 *Startup.cs*する前にファイル`UseMvc`または`UseMvcWithDefaultRoute`:</span><span class="sxs-lookup"><span data-stu-id="02936-210">Use the `UseCookieAuthentication` method in the `Configure` method in your *Startup.cs* file before `UseMvc` or `UseMvcWithDefaultRoute`:</span></span>

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

<span data-ttu-id="02936-211">**CookieAuthenticationOptions オプション**</span><span class="sxs-lookup"><span data-stu-id="02936-211">**CookieAuthenticationOptions Options**</span></span>

<span data-ttu-id="02936-212">[CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1)認証プロバイダー オプションを構成するクラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="02936-212">The [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="02936-213">オプション</span><span class="sxs-lookup"><span data-stu-id="02936-213">Option</span></span> | <span data-ttu-id="02936-214">説明</span><span class="sxs-lookup"><span data-stu-id="02936-214">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="02936-215">AuthenticationScheme</span><span class="sxs-lookup"><span data-stu-id="02936-215">AuthenticationScheme</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme?view=aspnetcore-1.1) | <span data-ttu-id="02936-216">認証方式を設定します。</span><span class="sxs-lookup"><span data-stu-id="02936-216">Sets the authentication scheme.</span></span> <span data-ttu-id="02936-217">`AuthenticationScheme`認証の複数のインスタンスがあるし、する場合に便利です[、特定のスキームの承認](xref:security/authorization/limitingidentitybyscheme)です。</span><span class="sxs-lookup"><span data-stu-id="02936-217">`AuthenticationScheme` is useful when there are multiple instances of authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="02936-218">設定、`AuthenticationScheme`に`CookieAuthenticationDefaults.AuthenticationScheme`スキームの"Cookie"の値を提供します。</span><span class="sxs-lookup"><span data-stu-id="02936-218">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="02936-219">スキームを区別する任意の文字列値を指定することができます。</span><span class="sxs-lookup"><span data-stu-id="02936-219">You can supply any string value that distinguishes the scheme.</span></span> |
| [<span data-ttu-id="02936-220">AutomaticAuthenticate</span><span class="sxs-lookup"><span data-stu-id="02936-220">AutomaticAuthenticate</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate?view=aspnetcore-1.1) | <span data-ttu-id="02936-221">Cookie 認証の要求ごとに実行、および検証し、作成された任意のシリアル化されたプリンシパルを再構築しようとしています。 ことを示す値を設定します。</span><span class="sxs-lookup"><span data-stu-id="02936-221">Sets a value to indicate that the cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span> |
| [<span data-ttu-id="02936-222">AutomaticChallenge</span><span class="sxs-lookup"><span data-stu-id="02936-222">AutomaticChallenge</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge?view=aspnetcore-1.1) | <span data-ttu-id="02936-223">True の場合、認証ミドルウェアは自動の課題を処理します。</span><span class="sxs-lookup"><span data-stu-id="02936-223">If true, the authentication middleware handles automatic challenges.</span></span> <span data-ttu-id="02936-224">かどうかは false の場合、認証ミドルウェアが変更されるだけ応答によって明示的に示されたとき、`AuthenticationScheme`です。</span><span class="sxs-lookup"><span data-stu-id="02936-224">If false, the authentication middleware only alters responses when explicitly indicated by the `AuthenticationScheme`.</span></span> |
| [<span data-ttu-id="02936-225">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="02936-225">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.claimsissuer?view=aspnetcore-1.1) | <span data-ttu-id="02936-226">使用する発行者、[発行者](/dotnet/api/system.security.claims.claim.issuer)cookie 認証ミドルウェアによって作成された任意の信頼性情報のプロパティです。</span><span class="sxs-lookup"><span data-stu-id="02936-226">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication middleware.</span></span> |
| [<span data-ttu-id="02936-227">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="02936-227">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain?view=aspnetcore-1.1) | <span data-ttu-id="02936-228">Cookie が提供されたドメイン名です。</span><span class="sxs-lookup"><span data-stu-id="02936-228">The domain name where the cookie is served.</span></span> <span data-ttu-id="02936-229">既定では、これは、要求のホスト名です。</span><span class="sxs-lookup"><span data-stu-id="02936-229">By default, this is the host name of the request.</span></span> <span data-ttu-id="02936-230">ブラウザーでは、cookie、一致するホスト名にのみ機能します。</span><span class="sxs-lookup"><span data-stu-id="02936-230">The browser only serves the cookie to a matching host name.</span></span> <span data-ttu-id="02936-231">任意のホストに使用できる cookie は、ドメイン内にこれを調整することもできます。</span><span class="sxs-lookup"><span data-stu-id="02936-231">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="02936-232">たとえば、cookie のドメインに設定`.contoso.com`で使用できるように`contoso.com`、 `www.contoso.com`、および`staging.www.contoso.com`です。</span><span class="sxs-lookup"><span data-stu-id="02936-232">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="02936-233">CookieHttpOnly</span><span class="sxs-lookup"><span data-stu-id="02936-233">CookieHttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly?view=aspnetcore-1.1) | <span data-ttu-id="02936-234">クッキーはサーバーにのみアクセスできるかどうかを示すフラグ。</span><span class="sxs-lookup"><span data-stu-id="02936-234">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="02936-235">この値を変更する`false`cookie にアクセスするクライアント側のスクリプトを許可し、アプリが必要 cookie の盗難にアプリを開きます可能性があります、[クロス サイト スクリプト (XSS)](xref:security/cross-site-scripting)脆弱性。</span><span class="sxs-lookup"><span data-stu-id="02936-235">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="02936-236">既定値は `true` です。</span><span class="sxs-lookup"><span data-stu-id="02936-236">The default value is `true`.</span></span> |
| [<span data-ttu-id="02936-237">CookiePath</span><span class="sxs-lookup"><span data-stu-id="02936-237">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath?view=aspnetcore-1.1) | <span data-ttu-id="02936-238">同じホスト名で実行されているアプリを分離するために使用します。</span><span class="sxs-lookup"><span data-stu-id="02936-238">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="02936-239">実行されているアプリがあれば`/app1`cookie をそのアプリに制限を設定して、`CookiePath`プロパティを`/app1`です。</span><span class="sxs-lookup"><span data-stu-id="02936-239">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="02936-240">これにより、cookie が要求に使用可能なのみ`/app1`とその下のすべてのアプリです。</span><span class="sxs-lookup"><span data-stu-id="02936-240">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="02936-241">CookieSecure</span><span class="sxs-lookup"><span data-stu-id="02936-241">CookieSecure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure?view=aspnetcore-1.1) | <span data-ttu-id="02936-242">作成された cookie を HTTPS に限られますかどうかを示すフラグ (`CookieSecurePolicy.Always`)、HTTP または HTTPS (`CookieSecurePolicy.None`)、または要求と同じプロトコル (`CookieSecurePolicy.SameAsRequest`)。</span><span class="sxs-lookup"><span data-stu-id="02936-242">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="02936-243">既定値は `CookieSecurePolicy.SameAsRequest` です。</span><span class="sxs-lookup"><span data-stu-id="02936-243">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="02936-244">説明</span><span class="sxs-lookup"><span data-stu-id="02936-244">Description</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description?view=aspnetcore-1.1) | <span data-ttu-id="02936-245">アプリに使用できる認証の種類に関する追加情報。</span><span class="sxs-lookup"><span data-stu-id="02936-245">Additional information about the authentication type which is made available to the app.</span></span> |
| [<span data-ttu-id="02936-246">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="02936-246">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.expiretimespan?view=aspnetcore-1.1) | <span data-ttu-id="02936-247">`TimeSpan`認証チケットが期限切れの後にします。</span><span class="sxs-lookup"><span data-stu-id="02936-247">The `TimeSpan` after which the authentication ticket expires.</span></span> <span data-ttu-id="02936-248">チケットの有効期限を作成するのには、現在の時刻に追加されます。</span><span class="sxs-lookup"><span data-stu-id="02936-248">It's added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="02936-249">使用する`ExpireTimeSpan`、設定する必要があります`IsPersistent`に`true`で、`AuthenticationProperties`に渡される`SignInAsync`です。</span><span class="sxs-lookup"><span data-stu-id="02936-249">To use `ExpireTimeSpan`, you must set `IsPersistent` to `true` in the `AuthenticationProperties` passed to `SignInAsync`.</span></span> <span data-ttu-id="02936-250">既定値は、14 日間です。</span><span class="sxs-lookup"><span data-stu-id="02936-250">The default value is 14 days.</span></span> |
| [<span data-ttu-id="02936-251">SlidingExpiration</span><span class="sxs-lookup"><span data-stu-id="02936-251">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-1.1) | <span data-ttu-id="02936-252">複数の半分の cookie の有効期限の日付をリセットするかどうかを示すフラグ、`ExpireTimeSpan`間隔が経過します。</span><span class="sxs-lookup"><span data-stu-id="02936-252">A flag indicating whether the cookie expiration date resets when more than half of the `ExpireTimeSpan` interval has passed.</span></span> <span data-ttu-id="02936-253">新しい exipiration 時刻が前方に移動する、現在の日付と`ExpireTimespan`です。</span><span class="sxs-lookup"><span data-stu-id="02936-253">The new exipiration time is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="02936-254">[絶対 cookie の有効期限](xref:security/authentication/cookie#absolute-cookie-expiration)を使用して設定することができます、`AuthenticationProperties`クラスを呼び出すときに`SignInAsync`です。</span><span class="sxs-lookup"><span data-stu-id="02936-254">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="02936-255">絶対有効期限は、認証 cookie が有効な時間を制限することによって、アプリのセキュリティを強化できます。</span><span class="sxs-lookup"><span data-stu-id="02936-255">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="02936-256">既定値は `true` です。</span><span class="sxs-lookup"><span data-stu-id="02936-256">The default value is `true`.</span></span> |

<span data-ttu-id="02936-257">設定`CookieAuthenticationOptions`で Cookie 認証ミドルウェアの`Configure`メソッド。</span><span class="sxs-lookup"><span data-stu-id="02936-257">Set `CookieAuthenticationOptions` for the Cookie Authentication Middleware in the `Configure` method:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    ...
});
```

---

## <a name="cookie-policy-middleware"></a><span data-ttu-id="02936-258">Cookie ポリシー ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="02936-258">Cookie Policy Middleware</span></span>

<span data-ttu-id="02936-259">[Cookie ポリシー ミドルウェア](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware)アプリ内で cookie ポリシーの機能を有効にします。</span><span class="sxs-lookup"><span data-stu-id="02936-259">[Cookie Policy Middleware](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) enables cookie policy capabilities in an app.</span></span> <span data-ttu-id="02936-260">機密性の高い; 順序は、アプリの処理パイプラインにミドルウェアを追加します。パイプラインの後に登録されているコンポーネントのみに影響します。</span><span class="sxs-lookup"><span data-stu-id="02936-260">Adding the middleware to the app processing pipeline is order sensitive; it only affects components registered after it in the pipeline.</span></span>

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

 <span data-ttu-id="02936-261">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) Cookie ポリシー ミドルウェアに渡された cookie を追加または削除されたときに、cookie 処理ハンドラーに cookie の処理とフックのグローバルの特性を制御することができます。</span><span class="sxs-lookup"><span data-stu-id="02936-261">The [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) provided to the Cookie Policy Middleware allow you to control global characteristics of cookie processing and hook into cookie processing handlers when cookies are appended or deleted.</span></span>

| <span data-ttu-id="02936-262">プロパティ</span><span class="sxs-lookup"><span data-stu-id="02936-262">Property</span></span> | <span data-ttu-id="02936-263">説明</span><span class="sxs-lookup"><span data-stu-id="02936-263">Description</span></span> |
| -------- | ----------- |
| [<span data-ttu-id="02936-264">HttpOnly</span><span class="sxs-lookup"><span data-stu-id="02936-264">HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.httponly) | <span data-ttu-id="02936-265">Cookie が HttpOnly であるかどうかをフラグを示すですクッキーはサーバーにのみアクセスできるかどうかに影響します。</span><span class="sxs-lookup"><span data-stu-id="02936-265">Affects whether cookies must be HttpOnly, which is a flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="02936-266">既定値は `HttpOnlyPolicy.None` です。</span><span class="sxs-lookup"><span data-stu-id="02936-266">The default value is `HttpOnlyPolicy.None`.</span></span> |
| [<span data-ttu-id="02936-267">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="02936-267">MinimumSameSitePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.minimumsamesitepolicy) | <span data-ttu-id="02936-268">Cookie の同じサイト属性 (下記参照) に影響します。</span><span class="sxs-lookup"><span data-stu-id="02936-268">Affects the cookie's same-site attribute (see below).</span></span> <span data-ttu-id="02936-269">既定値は `SameSiteMode.Lax` です。</span><span class="sxs-lookup"><span data-stu-id="02936-269">The default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="02936-270">このオプションは、ASP.NET Core 2.0 以降を使用します。</span><span class="sxs-lookup"><span data-stu-id="02936-270">This option is available for ASP.NET Core 2.0+.</span></span> |
| [<span data-ttu-id="02936-271">OnAppendCookie</span><span class="sxs-lookup"><span data-stu-id="02936-271">OnAppendCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.onappendcookie) | <span data-ttu-id="02936-272">Cookie を追加するときに呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="02936-272">Called when a cookie is appended.</span></span> |
| [<span data-ttu-id="02936-273">OnDeleteCookie</span><span class="sxs-lookup"><span data-stu-id="02936-273">OnDeleteCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.ondeletecookie) | <span data-ttu-id="02936-274">Cookie が削除されたときに呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="02936-274">Called when a cookie is deleted.</span></span> |
| [<span data-ttu-id="02936-275">セキュリティで保護されました。</span><span class="sxs-lookup"><span data-stu-id="02936-275">Secure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.secure) | <span data-ttu-id="02936-276">クッキーはセキュリティで保護する必要があるかどうかに影響します。</span><span class="sxs-lookup"><span data-stu-id="02936-276">Affects whether cookies must be Secure.</span></span> <span data-ttu-id="02936-277">既定値は `CookieSecurePolicy.None` です。</span><span class="sxs-lookup"><span data-stu-id="02936-277">The default value is `CookieSecurePolicy.None`.</span></span> |

<span data-ttu-id="02936-278">**MinimumSameSitePolicy** (ASP.NET Core 2.0 以降のみ)</span><span class="sxs-lookup"><span data-stu-id="02936-278">**MinimumSameSitePolicy** (ASP.NET Core 2.0+ only)</span></span>

<span data-ttu-id="02936-279">既定値`MinimumSameSitePolicy`値は`SameSiteMode.Lax`OAuth2 認証を許可するようにします。</span><span class="sxs-lookup"><span data-stu-id="02936-279">The default `MinimumSameSitePolicy` value is `SameSiteMode.Lax` to permit OAuth2 authentication.</span></span> <span data-ttu-id="02936-280">厳密にポリシーを適用する、同じサイトの`SameSiteMode.Strict`、設定、`MinimumSameSitePolicy`です。</span><span class="sxs-lookup"><span data-stu-id="02936-280">To strictly enforce a same-site policy of `SameSiteMode.Strict`, set the `MinimumSameSitePolicy`.</span></span> <span data-ttu-id="02936-281">この設定は、OAuth2 と他のクロス オリジン認証スキームを破損が他の種類のクロス オリジン要求の処理に依存しないようにするアプリの cookie のセキュリティのレベルを高めます。</span><span class="sxs-lookup"><span data-stu-id="02936-281">Although this setting breaks OAuth2 and other cross-origin authentication schemes, it elevates the level of cookie security for other types of apps that don't rely on cross-origin request processing.</span></span>

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

<span data-ttu-id="02936-282">Cookie のポリシーのミドルウェアの設定`MinimumSameSitePolicy`の設定に影響を与える`Cookie.SameSite`で`CookieAuthenticationOptions`次の表に従って設定します。</span><span class="sxs-lookup"><span data-stu-id="02936-282">The Cookie Policy Middleware setting for `MinimumSameSitePolicy` can affect your setting of `Cookie.SameSite` in `CookieAuthenticationOptions` settings according to the matrix below.</span></span>

| <span data-ttu-id="02936-283">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="02936-283">MinimumSameSitePolicy</span></span> | <span data-ttu-id="02936-284">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="02936-284">Cookie.SameSite</span></span> | <span data-ttu-id="02936-285">最終的な Cookie.SameSite 設定</span><span class="sxs-lookup"><span data-stu-id="02936-285">Resultant Cookie.SameSite setting</span></span> |
| --------------------- | --------------- | --------------------------------- |
| <span data-ttu-id="02936-286">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="02936-286">SameSiteMode.None</span></span>     | <span data-ttu-id="02936-287">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="02936-287">SameSiteMode.None</span></span><br><span data-ttu-id="02936-288">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="02936-288">SameSiteMode.Lax</span></span><br><span data-ttu-id="02936-289">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="02936-289">SameSiteMode.Strict</span></span> | <span data-ttu-id="02936-290">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="02936-290">SameSiteMode.None</span></span><br><span data-ttu-id="02936-291">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="02936-291">SameSiteMode.Lax</span></span><br><span data-ttu-id="02936-292">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="02936-292">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="02936-293">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="02936-293">SameSiteMode.Lax</span></span>      | <span data-ttu-id="02936-294">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="02936-294">SameSiteMode.None</span></span><br><span data-ttu-id="02936-295">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="02936-295">SameSiteMode.Lax</span></span><br><span data-ttu-id="02936-296">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="02936-296">SameSiteMode.Strict</span></span> | <span data-ttu-id="02936-297">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="02936-297">SameSiteMode.Lax</span></span><br><span data-ttu-id="02936-298">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="02936-298">SameSiteMode.Lax</span></span><br><span data-ttu-id="02936-299">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="02936-299">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="02936-300">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="02936-300">SameSiteMode.Strict</span></span>   | <span data-ttu-id="02936-301">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="02936-301">SameSiteMode.None</span></span><br><span data-ttu-id="02936-302">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="02936-302">SameSiteMode.Lax</span></span><br><span data-ttu-id="02936-303">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="02936-303">SameSiteMode.Strict</span></span> | <span data-ttu-id="02936-304">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="02936-304">SameSiteMode.Strict</span></span><br><span data-ttu-id="02936-305">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="02936-305">SameSiteMode.Strict</span></span><br><span data-ttu-id="02936-306">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="02936-306">SameSiteMode.Strict</span></span> |

## <a name="creating-an-authentication-cookie"></a><span data-ttu-id="02936-307">認証 cookie を作成します。</span><span class="sxs-lookup"><span data-stu-id="02936-307">Creating an authentication cookie</span></span>

<span data-ttu-id="02936-308">ユーザー情報を保持する cookie を作成、構築する必要があります、 [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal)です。</span><span class="sxs-lookup"><span data-stu-id="02936-308">To create a cookie holding user information, you must construct a [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal).</span></span> <span data-ttu-id="02936-309">ユーザー情報がシリアル化され、cookie に格納されています。</span><span class="sxs-lookup"><span data-stu-id="02936-309">The user information is serialized and stored in the cookie.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="02936-310">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="02936-310">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="02936-311">呼び出す[SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0)ユーザーにサインインします。</span><span class="sxs-lookup"><span data-stu-id="02936-311">Call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) to sign in the user:</span></span>

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme, 
    new ClaimsPrincipal(claimsIdentity));
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="02936-312">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="02936-312">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="02936-313">呼び出す[SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1)ユーザーにサインインします。</span><span class="sxs-lookup"><span data-stu-id="02936-313">Call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) to sign in the user:</span></span>

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity));
```

---

<span data-ttu-id="02936-314">`SignInAsync`暗号化された cookie を作成し、現在の応答に追加します。</span><span class="sxs-lookup"><span data-stu-id="02936-314">`SignInAsync` creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="02936-315">指定しない場合は、`AuthenticationScheme`既定のスキームを使用します。</span><span class="sxs-lookup"><span data-stu-id="02936-315">If you don't specify an `AuthenticationScheme`, the default scheme is used.</span></span>

<span data-ttu-id="02936-316">背後で使用される暗号化は ASP.NET Core[データ保護](xref:security/data-protection/using-data-protection#security-data-protection-getting-started)システムです。</span><span class="sxs-lookup"><span data-stu-id="02936-316">Under the covers, the encryption used is ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) system.</span></span> <span data-ttu-id="02936-317">複数のコンピューター、アプリでは、間における負荷分散または web ファーム上のアプリをホストしているかどうかは、する必要があります[データ保護を構成する](xref:security/data-protection/configuration/overview)同じキーのリングとアプリ id を使用します。</span><span class="sxs-lookup"><span data-stu-id="02936-317">If you're hosting app on multiple machines, load balancing across apps, or using a web farm, then you must [configure data protection](xref:security/data-protection/configuration/overview) to use the same key ring and app identifier.</span></span>

## <a name="signing-out"></a><span data-ttu-id="02936-318">サインアウト</span><span class="sxs-lookup"><span data-stu-id="02936-318">Signing out</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="02936-319">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="02936-319">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="02936-320">現在のユーザーをサインアウトし、その cookie を削除、 [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span><span class="sxs-lookup"><span data-stu-id="02936-320">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span></span>

```csharp
await HttpContext.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="02936-321">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="02936-321">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="02936-322">現在のユーザーをサインアウトし、その cookie を削除、 [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span><span class="sxs-lookup"><span data-stu-id="02936-322">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span></span>

```csharp
await HttpContext.Authentication.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

---

<span data-ttu-id="02936-323">使用しない場合`CookieAuthenticationDefaults.AuthenticationScheme`(または"Cookie")、スキーム (たとえば、"ContosoCookie") として認証プロバイダーを構成するときに使用するスキームを指定します。</span><span class="sxs-lookup"><span data-stu-id="02936-323">If you aren't using `CookieAuthenticationDefaults.AuthenticationScheme` (or "Cookies") as the scheme (for example, "ContosoCookie"), supply the scheme you used when configuring the authentication provider.</span></span> <span data-ttu-id="02936-324">それ以外の場合、既定のスキームが使用されます。</span><span class="sxs-lookup"><span data-stu-id="02936-324">Otherwise, the default scheme is used.</span></span>

## <a name="reacting-to-back-end-changes"></a><span data-ttu-id="02936-325">バックエンドの変更に反応します。</span><span class="sxs-lookup"><span data-stu-id="02936-325">Reacting to back-end changes</span></span>

<span data-ttu-id="02936-326">Cookie が作成されると、id の 1 つのソースになります。</span><span class="sxs-lookup"><span data-stu-id="02936-326">Once a cookie is created, it becomes the single source of identity.</span></span> <span data-ttu-id="02936-327">バックエンド システムでユーザーを無効にする場合でも cookie 認証システムは、この情報を持たないし、状態を維持でログに記録されたその cookie が有効な限り、します。</span><span class="sxs-lookup"><span data-stu-id="02936-327">Even if you disable a user in your back-end systems, the cookie authentication system has no knowledge of this, and a user stays logged in as long as their cookie is valid.</span></span>

<span data-ttu-id="02936-328">[ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) ASP.NET Core でのイベント 2.x または[ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) 1.x をインターセプトし、cookie id の検証のオーバーライドに使用できる ASP.NET Core のメソッドです。</span><span class="sxs-lookup"><span data-stu-id="02936-328">The [ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) event in ASP.NET Core 2.x or the [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) method in ASP.NET Core 1.x can be used to intercept and override validation of the cookie identity.</span></span> <span data-ttu-id="02936-329">この方法は、失効したユーザーがアプリにアクセスするリスクを軽減します。</span><span class="sxs-lookup"><span data-stu-id="02936-329">This approach mitigates the risk of revoked users accessing the app.</span></span>

<span data-ttu-id="02936-330">Cookie 検証方法の 1 つは、ユーザー データベースが変更されたときの追跡に基づいています。</span><span class="sxs-lookup"><span data-stu-id="02936-330">One approach to cookie validation is based on keeping track of when the user database has been changed.</span></span> <span data-ttu-id="02936-331">ユーザーの cookie が発行されてから、データベースが変更されていない場合、cookie が有効である場合、ユーザーを再認証する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="02936-331">If the database hasn't been changed since the user's cookie was issued, there is no need to re-authenticate the user if their cookie is still valid.</span></span> <span data-ttu-id="02936-332">このシナリオで実装されると、データベースを実装する`IUserRepository`この例では、格納、`LastChanged`値。</span><span class="sxs-lookup"><span data-stu-id="02936-332">To implement this scenario, the database, which is implemented in `IUserRepository` for this example, stores a `LastChanged` value.</span></span> <span data-ttu-id="02936-333">すべてのユーザーが、データベースで更新されたときに、`LastChanged`値は、現在の時刻に設定します。</span><span class="sxs-lookup"><span data-stu-id="02936-333">When any user is updated in the database, the `LastChanged` value is set to the current time.</span></span>

<span data-ttu-id="02936-334">に基づいて、データベースの変更時に、cookie を無効にするために、`LastChanged`値、cookie を作成、`LastChanged`を含む、現在の要求`LastChanged`データベースから値。</span><span class="sxs-lookup"><span data-stu-id="02936-334">In order to invalidate a cookie when the database changes based on the `LastChanged` value, create the cookie with a `LastChanged` claim containing the current `LastChanged` value from the database:</span></span>

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

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="02936-335">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="02936-335">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="02936-336">オーバーライドを実装する、`ValidatePrincipal`イベントから派生したクラスでは、次のシグネチャを持つメソッドを作成します[CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span><span class="sxs-lookup"><span data-stu-id="02936-336">To implement an override for the `ValidatePrincipal` event, write a method with the following signature in a class that you derive from [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span></span>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

<span data-ttu-id="02936-337">例は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="02936-337">An example looks like the following:</span></span>

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

<span data-ttu-id="02936-338">Cookie にサービスを登録中に、イベント インスタンスを登録する、`ConfigureServices`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="02936-338">Register the events instance during cookie service registration in the `ConfigureServices` method.</span></span> <span data-ttu-id="02936-339">スコープを持つサービスの登録を提供、`CustomCookieAuthenticationEvents`クラス。</span><span class="sxs-lookup"><span data-stu-id="02936-339">Provide a scoped service registration for your `CustomCookieAuthenticationEvents` class:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="02936-340">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="02936-340">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="02936-341">オーバーライドを実装する、`ValidateAsync`イベント、次のシグネチャを持つメソッドを作成します。</span><span class="sxs-lookup"><span data-stu-id="02936-341">To implement an override for the `ValidateAsync` event, write a method with the following signature:</span></span>

```csharp
ValidateAsync(CookieValidatePrincipalContext)
```

<span data-ttu-id="02936-342">ASP.NET Core Id では、このチェックを実装の一部としてその[SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync)です。</span><span class="sxs-lookup"><span data-stu-id="02936-342">ASP.NET Core Identity implements this check as part of its [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync).</span></span> <span data-ttu-id="02936-343">例は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="02936-343">An example looks like the following:</span></span>

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

<span data-ttu-id="02936-344">Cookie 認証の構成時にイベントを登録、`Configure`メソッド。</span><span class="sxs-lookup"><span data-stu-id="02936-344">Register the event during cookie authentication configuration in the `Configure` method:</span></span>

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

<span data-ttu-id="02936-345">ユーザーの名前を更新する場合を検討&mdash;任意の方法でセキュリティに影響しないする意思決定します。</span><span class="sxs-lookup"><span data-stu-id="02936-345">Consider a situation in which the user's name is updated &mdash; a decision that doesn't affect security in any way.</span></span> <span data-ttu-id="02936-346">非破壊的ユーザー プリンシパルを更新する場合は、呼び出す`context.ReplacePrincipal`設定と、`context.ShouldRenew`プロパティを`true`です。</span><span class="sxs-lookup"><span data-stu-id="02936-346">If you want to non-destructively update the user principal, call `context.ReplacePrincipal` and set the `context.ShouldRenew` property to `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="02936-347">ここで説明されているアプローチは、要求ごとにトリガーされます。</span><span class="sxs-lookup"><span data-stu-id="02936-347">The approach described here is triggered on every request.</span></span> <span data-ttu-id="02936-348">これにより、アプリのパフォーマンスを大幅に低下。</span><span class="sxs-lookup"><span data-stu-id="02936-348">This can result in a large performance penalty for the app.</span></span>

## <a name="persistent-cookies"></a><span data-ttu-id="02936-349">永続的な cookie</span><span class="sxs-lookup"><span data-stu-id="02936-349">Persistent cookies</span></span>

<span data-ttu-id="02936-350">Cookie をブラウザー セッション間で保持することができます。</span><span class="sxs-lookup"><span data-stu-id="02936-350">You may want the cookie to persist across browser sessions.</span></span> <span data-ttu-id="02936-351">この永続化は、「次回」にあるチェック ボックス ログインまたは同様のメカニズムを使用してユーザーの明示的な同意でのみ有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="02936-351">This persistence should only be enabled with explicit user consent with a "Remember Me" checkbox on login or a similar mechanism.</span></span> 

<span data-ttu-id="02936-352">次のコード スニペットは、id およびブラウザー クロージャで回収されなかったを対応する cookie を作成します。</span><span class="sxs-lookup"><span data-stu-id="02936-352">The following code snippet creates an identity and corresponding cookie that survives through browser closures.</span></span> <span data-ttu-id="02936-353">以前に構成された、スライド式有効期限の設定が無視されます。</span><span class="sxs-lookup"><span data-stu-id="02936-353">Any sliding expiration settings previously configured are honored.</span></span> <span data-ttu-id="02936-354">Cookie には、ブラウザーを閉じている間に期限が切れるが再起動したら、ブラウザーの cookie を消去します。</span><span class="sxs-lookup"><span data-stu-id="02936-354">If the cookie expires while the browser is closed, the browser clears the cookie once it's restarted.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="02936-355">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="02936-355">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="02936-356">[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0)に存在するクラス、`Microsoft.AspNetCore.Authentication`名前空間。</span><span class="sxs-lookup"><span data-stu-id="02936-356">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) class resides in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="02936-357">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="02936-357">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="02936-358">[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1)に存在するクラス、`Microsoft.AspNetCore.Http.Authentication`名前空間。</span><span class="sxs-lookup"><span data-stu-id="02936-358">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) class resides in the `Microsoft.AspNetCore.Http.Authentication` namespace.</span></span>

---

## <a name="absolute-cookie-expiration"></a><span data-ttu-id="02936-359">絶対 cookie の有効期限</span><span class="sxs-lookup"><span data-stu-id="02936-359">Absolute cookie expiration</span></span>

<span data-ttu-id="02936-360">絶対有効期限を設定することができます`ExpiresUtc`です。</span><span class="sxs-lookup"><span data-stu-id="02936-360">You can set an absolute expiration time with `ExpiresUtc`.</span></span> <span data-ttu-id="02936-361">設定する必要もあります`IsPersistent`、それ以外の`ExpiresUtc`は無視されますと、単一セッションの cookie を作成します。</span><span class="sxs-lookup"><span data-stu-id="02936-361">You must also set `IsPersistent`; otherwise, `ExpiresUtc` is ignored and a single-session cookie is created.</span></span> <span data-ttu-id="02936-362">ときに`ExpiresUtc`に設定されている`SignInAsync`の値をオーバーライドし、`ExpireTimeSpan`オプション`CookieAuthenticationOptions`場合は、設定します。</span><span class="sxs-lookup"><span data-stu-id="02936-362">When `ExpiresUtc` is set on `SignInAsync`, it overrides the value of the `ExpireTimeSpan` option of `CookieAuthenticationOptions`, if set.</span></span>

<span data-ttu-id="02936-363">次のコード スニペットは、id と 20 分間存続する対応する cookie を作成します。</span><span class="sxs-lookup"><span data-stu-id="02936-363">The following code snippet creates an identity and corresponding cookie that lasts for 20 minutes.</span></span> <span data-ttu-id="02936-364">これには、以前に構成された、スライド式有効期限の設定は無視されます。</span><span class="sxs-lookup"><span data-stu-id="02936-364">This ignores any sliding expiration settings previously configured.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="02936-365">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="02936-365">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="02936-366">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="02936-366">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

## <a name="see-also"></a><span data-ttu-id="02936-367">関連項目</span><span class="sxs-lookup"><span data-stu-id="02936-367">See also</span></span>

* [<span data-ttu-id="02936-368">Auth 2.0 変更/移行アナウンス</span><span class="sxs-lookup"><span data-stu-id="02936-368">Auth 2.0 Changes / Migration Announcement</span></span>](https://github.com/aspnet/Announcements/issues/262)
* [<span data-ttu-id="02936-369">スキームによる ID の制限</span><span class="sxs-lookup"><span data-stu-id="02936-369">Limiting identity by scheme</span></span>](xref:security/authorization/limitingidentitybyscheme)
