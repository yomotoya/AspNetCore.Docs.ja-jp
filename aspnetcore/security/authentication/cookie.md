---
title: "ASP.NET Core Identity なしで認証に Cookie を使用します。"
author: rick-anderson
description: "ASP.NET Core Id なしの cookie 認証を使用しての説明"
keywords: "ASP.NET Core、cookie"
ms.author: riande
manager: wpickett
ms.date: 08/14/2017
ms.topic: article
ms.assetid: 2bdcbf95-8d9d-4537-a4a0-a5ee439dcb62
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/cookie
ms.openlocfilehash: af3ffe418521d5d97f5d14ca9c904c21b4d4ff89
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/28/2017
---
# <a name="using-cookie-authentication-without-aspnet-core-identity"></a><span data-ttu-id="e0898-104">ASP.NET Core Identity なしで認証に Cookie を使用します。</span><span class="sxs-lookup"><span data-stu-id="e0898-104">Using Cookie Authentication without ASP.NET Core Identity</span></span>

<a name="security-authentication-cookie-middleware"></a>

<span data-ttu-id="e0898-105">ASP.NET Core 1.x 提供 cookie[ミドルウェア](../../fundamentals/middleware.md#fundamentals-middleware)暗号化された cookie にユーザー プリンシパルをシリアル化し、次に、後続の要求の cookie を検証するプリンシパルを再作成し、それを`HttpContext.User`プロパティ.</span><span class="sxs-lookup"><span data-stu-id="e0898-105">ASP.NET Core 1.x provides cookie [middleware](../../fundamentals/middleware.md#fundamentals-middleware) which serializes a user principal into an encrypted cookie and then, on subsequent requests, validates the cookie, recreates the principal, and assigns it to the `HttpContext.User` property.</span></span> <span data-ttu-id="e0898-106">独自のログイン画面とユーザー データベースを提供する場合は、スタンドアロン機能として cookie ミドルウェアを使用できます。</span><span class="sxs-lookup"><span data-stu-id="e0898-106">If you want to provide your own login screens and user databases, you can use the cookie middleware as a standalone feature.</span></span>

<span data-ttu-id="e0898-107">重大な変更点、ASP.NET Core 2.x は cookie ミドルウェアが存在しないことです。</span><span class="sxs-lookup"><span data-stu-id="e0898-107">A major change in ASP.NET Core 2.x is that the cookie middleware is absent.</span></span> <span data-ttu-id="e0898-108">代わりに、`UseAuthentication`メソッドを呼び出し、`Configure`メソッドの*Startup.cs* AuthenticationMiddleware 設定を追加、`HttpContext.User`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="e0898-108">Instead, the `UseAuthentication` method invocation in the `Configure` method of *Startup.cs* adds the AuthenticationMiddleware which sets the `HttpContext.User` property.</span></span>

<a name="security-authentication-cookie-middleware-configuring"></a>

## <a name="adding-and-configuring"></a><span data-ttu-id="e0898-109">追加と構成</span><span class="sxs-lookup"><span data-stu-id="e0898-109">Adding and configuring</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e0898-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e0898-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="e0898-111">次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="e0898-111">Complete the following steps:</span></span>

- <span data-ttu-id="e0898-112">使用していない場合、 `Microsoft.AspNetCore.All` [metapackage](xref:fundamentals/metapackage)、バージョン 2.0 以降のインストール、`Microsoft.AspNetCore.Authentication.Cookies`プロジェクトでの NuGet パッケージです。</span><span class="sxs-lookup"><span data-stu-id="e0898-112">If not using the `Microsoft.AspNetCore.All` [metapackage](xref:fundamentals/metapackage), install version 2.0+ of the `Microsoft.AspNetCore.Authentication.Cookies` NuGet package in your project.</span></span>

- <span data-ttu-id="e0898-113">呼び出す、`UseAuthentication`メソッドで、`Configure`のメソッド、 *Startup.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="e0898-113">Invoke the `UseAuthentication` method in the `Configure` method of the *Startup.cs* file:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="e0898-114">呼び出す、`AddAuthentication`と`AddCookie`内のメソッド、`ConfigureServices`のメソッド、 *Startup.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="e0898-114">Invoke the `AddAuthentication` and `AddCookie` methods in the `ConfigureServices` method of the *Startup.cs* file:</span></span>

    ```csharp
    services.AddAuthentication("MyCookieAuthenticationScheme")
            .AddCookie("MyCookieAuthenticationScheme", options => {
                options.AccessDeniedPath = "/Account/Forbidden/";
                options.LoginPath = "/Account/Unauthorized/";
            });
    ```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e0898-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e0898-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="e0898-116">次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="e0898-116">Complete the following steps:</span></span>

- <span data-ttu-id="e0898-117">インストール、`Microsoft.AspNetCore.Authentication.Cookies`プロジェクトでの NuGet パッケージです。</span><span class="sxs-lookup"><span data-stu-id="e0898-117">Install the `Microsoft.AspNetCore.Authentication.Cookies` NuGet package in your project.</span></span> <span data-ttu-id="e0898-118">このパッケージには、cookie ミドルウェアが含まれています。</span><span class="sxs-lookup"><span data-stu-id="e0898-118">This package contains the cookie middleware.</span></span>

- <span data-ttu-id="e0898-119">次の行を追加、`Configure`メソッドで、 *Startup.cs*ファイルの前に、`app.UseMvc()`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="e0898-119">Add the following lines to the `Configure` method in your *Startup.cs* file before the `app.UseMvc()` statement:</span></span>

    ```csharp
    app.UseCookieAuthentication(new CookieAuthenticationOptions()
    {
        AccessDeniedPath = "/Account/Forbidden/",
        AuthenticationScheme = "MyCookieAuthenticationScheme",
        AutomaticAuthenticate = true,
        AutomaticChallenge = true,
        LoginPath = "/Account/Unauthorized/"
    });
    ```

---

<span data-ttu-id="e0898-120">上記のコード スニペットは、次のオプションの一部またはすべてを構成します。</span><span class="sxs-lookup"><span data-stu-id="e0898-120">The code snippets above configure some or all of the following options:</span></span>

* <span data-ttu-id="e0898-121">`AccessDeniedPath`-これは、ユーザーがリソースにアクセスしようとしています。 いずれかを渡さない場合に要求がリダイレクトする相対パス[承認ポリシー](xref:security/authorization/policies#security-authorization-policies-based)をそのリソース用です。</span><span class="sxs-lookup"><span data-stu-id="e0898-121">`AccessDeniedPath` - This is the relative path to which requests redirect when a user attempts to access a resource but does not pass any [authorization policies](xref:security/authorization/policies#security-authorization-policies-based) for that resource.</span></span>

* <span data-ttu-id="e0898-122">`AuthenticationScheme`-これは、特定の cookie 認証スキームを認識する値です。</span><span class="sxs-lookup"><span data-stu-id="e0898-122">`AuthenticationScheme` - This is a value by which a particular cookie authentication scheme is known.</span></span> <span data-ttu-id="e0898-123">これは、cookie 認証の複数のインスタンスがあるし、する場合に役立ちます[の承認を 1 つのインスタンスを制限する](xref:security/authorization/limitingidentitybyscheme#security-authorization-limiting-by-scheme)です。</span><span class="sxs-lookup"><span data-stu-id="e0898-123">This is useful when there are multiple instances of cookie authentication and you want to [limit authorization to one instance](xref:security/authorization/limitingidentitybyscheme#security-authorization-limiting-by-scheme).</span></span>

* <span data-ttu-id="e0898-124">`AutomaticAuthenticate`-このフラグは ASP.NET Core のみに関連 1.x です。</span><span class="sxs-lookup"><span data-stu-id="e0898-124">`AutomaticAuthenticate` - This flag is relevant only for ASP.NET Core 1.x.</span></span> <span data-ttu-id="e0898-125">Cookie 認証の要求ごとに実行、および検証し、作成された任意のシリアル化されたプリンシパルを再構築しようとしています。 ことを示します。</span><span class="sxs-lookup"><span data-stu-id="e0898-125">It indicates that the cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span>

* <span data-ttu-id="e0898-126">`AutomaticChallenge`-このフラグは ASP.NET Core のみに関連 1.x です。</span><span class="sxs-lookup"><span data-stu-id="e0898-126">`AutomaticChallenge` - This flag is relevant only for ASP.NET Core 1.x.</span></span> <span data-ttu-id="e0898-127">1.x cookie 認証がブラウザーをリダイレクトすることを示します、`LoginPath`または`AccessDeniedPath`承認に失敗するとします。</span><span class="sxs-lookup"><span data-stu-id="e0898-127">It indicates that the 1.x cookie authentication should redirect the browser to the `LoginPath` or `AccessDeniedPath` when authorization fails.</span></span>

* <span data-ttu-id="e0898-128">`LoginPath`-これは、ユーザーがリソースにアクセスしようとしていますが、認証されていないときに要求がリダイレクトする相対パスです。</span><span class="sxs-lookup"><span data-stu-id="e0898-128">`LoginPath` - This is the relative path to which requests redirect when a user attempts to access a resource but has not been authenticated.</span></span>

<span data-ttu-id="e0898-129">[その他のオプション](xref:security/authentication/cookie#security-authentication-cookie-options)cookie 認証を作成する任意のクレームの発行者を設定する機能は、認証 cookie の名前になると、ドメイン、cookie と、cookie でさまざまなセキュリティのプロパティをします。</span><span class="sxs-lookup"><span data-stu-id="e0898-129">[Other options](xref:security/authentication/cookie#security-authentication-cookie-options) include the ability to set the issuer for any claims the cookie authentication creates, the name of the cookie the authentication drops, the domain for the cookie and various security properties on the cookie.</span></span> <span data-ttu-id="e0898-130">既定では、cookie 認証は、クッキーを作成などの適切なセキュリティ オプションを使用します。</span><span class="sxs-lookup"><span data-stu-id="e0898-130">By default, the cookie authentication uses appropriate security options for any cookies it creates, such as:</span></span>
- <span data-ttu-id="e0898-131">クライアント側 JavaScript でクッキーのアクセスを防ぐため HttpOnly フラグの設定</span><span class="sxs-lookup"><span data-stu-id="e0898-131">Setting the HttpOnly flag to prevent cookie access in client-side JavaScript</span></span>
- <span data-ttu-id="e0898-132">要求が HTTPS 経由で旅行の場合、cookie を HTTPS を制限します。</span><span class="sxs-lookup"><span data-stu-id="e0898-132">Limiting the cookie to HTTPS if a request has traveled over HTTPS</span></span>

<a name="security-authentication-cookie-middleware-creating-a-cookie"></a>

## <a name="creating-an-identity-cookie"></a><span data-ttu-id="e0898-133">Id cookie を作成します。</span><span class="sxs-lookup"><span data-stu-id="e0898-133">Creating an Identity cookie</span></span>

<span data-ttu-id="e0898-134">構築する必要があります、ユーザー情報を保持する cookie を作成するには[ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal)クッキーにシリアル化したい情報を保持します。</span><span class="sxs-lookup"><span data-stu-id="e0898-134">To create a cookie holding your user information, you must construct a [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal) holding the information you wish to be serialized in the cookie.</span></span> <span data-ttu-id="e0898-135">作成したら、適切な`ClaimsPrincipal`オブジェクトを次の呼び出し、コント ローラー メソッドの内部。</span><span class="sxs-lookup"><span data-stu-id="e0898-135">Once you have a suitable `ClaimsPrincipal` object, call the following inside your controller method:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e0898-136">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e0898-136">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync("MyCookieAuthenticationScheme", principal);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e0898-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e0898-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync("MyCookieAuthenticationScheme", principal);
```

---

<span data-ttu-id="e0898-138">これにより、暗号化された cookie を作成し、現在の応答に追加します。</span><span class="sxs-lookup"><span data-stu-id="e0898-138">This creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="e0898-139">`AuthenticationScheme`中に指定された[構成](xref:security/authentication/cookie#security-authentication-cookie-middleware-configuring)を呼び出すときに使用する必要があります`SignInAsync`です。</span><span class="sxs-lookup"><span data-stu-id="e0898-139">The `AuthenticationScheme` specified during [configuration](xref:security/authentication/cookie#security-authentication-cookie-middleware-configuring) must be used when calling `SignInAsync`.</span></span>

<span data-ttu-id="e0898-140">背後で使用される暗号化は ASP.NET Core[データ保護](xref:security/data-protection/using-data-protection#security-data-protection-getting-started)システムです。</span><span class="sxs-lookup"><span data-stu-id="e0898-140">Under the covers, the encryption used is ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) system.</span></span> <span data-ttu-id="e0898-141">負荷分散、または web ファームを使用して、複数のコンピューターでホストしているかどうかは、する必要があります[データ保護を構成する](xref:security/data-protection/configuration/overview#data-protection-configuring)同じキーのリングとのアプリケーション識別子を使用します。</span><span class="sxs-lookup"><span data-stu-id="e0898-141">If you are hosting on multiple machines, load balancing, or using a web farm, then you need to [configure data protection](xref:security/data-protection/configuration/overview#data-protection-configuring) to use the same key ring and application identifier.</span></span>

## <a name="signing-out"></a><span data-ttu-id="e0898-142">サインアウト</span><span class="sxs-lookup"><span data-stu-id="e0898-142">Signing out</span></span>

<span data-ttu-id="e0898-143">現在のユーザーからサインアウトするか、クッキーを削除して、次を呼び出す内部コント ローラー。</span><span class="sxs-lookup"><span data-stu-id="e0898-143">To sign out the current user and delete their cookie, call the following inside your controller:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e0898-144">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e0898-144">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignOutAsync("MyCookieAuthenticationScheme");
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e0898-145">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e0898-145">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignOutAsync("MyCookieAuthenticationScheme");
```

---

## <a name="reacting-to-back-end-changes"></a><span data-ttu-id="e0898-146">バックエンドの変更に反応します。</span><span class="sxs-lookup"><span data-stu-id="e0898-146">Reacting to back-end changes</span></span>

>[!WARNING]
> <span data-ttu-id="e0898-147">プリンシパルの cookie が作成されたら、id の 1 つのソースになります。</span><span class="sxs-lookup"><span data-stu-id="e0898-147">Once a principal cookie has been created, it becomes the single source of identity.</span></span> <span data-ttu-id="e0898-148">バックエンド システムでユーザーを無効にする場合でも cookie 認証は、この情報を持たないし、状態を維持でログに記録されたその cookie が有効な限り、します。</span><span class="sxs-lookup"><span data-stu-id="e0898-148">Even if you disable a user in your back-end systems, the cookie authentication has no knowledge of this, and a user stays logged in as long as their cookie is valid.</span></span>

<span data-ttu-id="e0898-149">Cookie 認証では、一連のオプション クラスでイベントを提供します。</span><span class="sxs-lookup"><span data-stu-id="e0898-149">The cookie authentication provides a series of events in its option class.</span></span> <span data-ttu-id="e0898-150">`ValidateAsync()`イベントをインターセプトし、cookie id の検証のオーバーライドに使用できます。</span><span class="sxs-lookup"><span data-stu-id="e0898-150">The `ValidateAsync()` event can be used to intercept and override validation of the cookie identity.</span></span>

<span data-ttu-id="e0898-151">"LastChanged"列を持たない可能性のあるバック エンド ユーザー データベースを検討してください。</span><span class="sxs-lookup"><span data-stu-id="e0898-151">Consider a back-end user database that may have a "LastChanged" column.</span></span> <span data-ttu-id="e0898-152">データベースが変更されたときに、cookie を無効にするためにする必要があります最初、[クッキーを作成する](xref:security/authentication/cookie#security-authentication-cookie-middleware-creating-a-cookie)、現在の値を含む"LastChanged"クレームを追加します。</span><span class="sxs-lookup"><span data-stu-id="e0898-152">In order to invalidate a cookie when the database changes, you should first, when [creating the cookie](xref:security/authentication/cookie#security-authentication-cookie-middleware-creating-a-cookie), add a "LastChanged" claim containing the current value.</span></span> <span data-ttu-id="e0898-153">データベースが変更されたときに、"LastChanged"値を更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e0898-153">When the database changes, the "LastChanged" value should be updated.</span></span>

<span data-ttu-id="e0898-154">オーバーライドを実装する、`ValidateAsync()`イベントでは、次のシグネチャを持つメソッドを記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e0898-154">To implement an override for the `ValidateAsync()` event, you must write a method with the following signature:</span></span>

```csharp
Task ValidateAsync(CookieValidatePrincipalContext context);
```

<span data-ttu-id="e0898-155">ASP.NET Core Id では、このチェックを実装の一部としてその`SecurityStampValidator`です。</span><span class="sxs-lookup"><span data-stu-id="e0898-155">ASP.NET Core Identity implements this check as part of its `SecurityStampValidator`.</span></span> <span data-ttu-id="e0898-156">例は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="e0898-156">An example looks like the following:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e0898-157">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e0898-157">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
public static class LastChangedValidator
{
    public static async Task ValidateAsync(CookieValidatePrincipalContext context)
    {
        // Pull database from registered DI services.
        var userRepository = context.HttpContext.RequestServices.GetRequiredService<IUserRepository>();
        var userPrincipal = context.Principal;

        // Look for the last changed claim.
        string lastChanged;
        lastChanged = (from c in userPrincipal.Claims
                        where c.Type == "LastUpdated"
                        select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !userRepository.ValidateLastChanged(userPrincipal, lastChanged))
        {
            context.RejectPrincipal();
            await context.HttpContext.SignOutAsync("MyCookieAuthenticationScheme");
        }
    }
}
```

<span data-ttu-id="e0898-158">これは、するワイヤード (有線) を cookie にサービスを登録中に、`ConfigureServices`メソッドの*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="e0898-158">This would then be wired up during cookie service registration in the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication("MyCookieAuthenticationScheme")
        .AddCookie("MyCookieAuthenticationScheme", options =>
        {
            options.Events = new CookieAuthenticationEvents
            {
                OnValidatePrincipal = LastChangedValidator.ValidateAsync
            };
        });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e0898-159">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e0898-159">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
public static class LastChangedValidator
{
    public static async Task ValidateAsync(CookieValidatePrincipalContext context)
    {
        // Pull database from registered DI services.
        var userRepository = context.HttpContext.RequestServices.GetRequiredService<IUserRepository>();
        var userPrincipal = context.Principal;

        // Look for the last changed claim.
        string lastChanged;
        lastChanged = (from c in userPrincipal.Claims
                        where c.Type == "LastUpdated"
                        select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !userRepository.ValidateLastChanged(userPrincipal, lastChanged))
        {
            context.RejectPrincipal();
            await context.HttpContext.Authentication.SignOutAsync("MyCookieAuthenticationScheme");
        }
    }
}
```

<span data-ttu-id="e0898-160">これは、するワイヤード (有線) の cookie 認証の構成時に、`Configure`メソッドの*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="e0898-160">This would then be wired up during cookie authentication configuration in the `Configure` method of *Startup.cs*:</span></span>

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

<span data-ttu-id="e0898-161">その名前が更新された例を考えてみます&mdash;任意の方法でセキュリティが低下する可能性が決定します。</span><span class="sxs-lookup"><span data-stu-id="e0898-161">Consider the example in which their name has been updated &mdash; a decision which doesn't affect security in any way.</span></span> <span data-ttu-id="e0898-162">非破壊的ユーザー プリンシパルを更新する場合は、呼び出す`context.ReplacePrincipal()`設定と、`context.ShouldRenew`プロパティを`true`です。</span><span class="sxs-lookup"><span data-stu-id="e0898-162">If you want to non-destructively update the user principal, you can call `context.ReplacePrincipal()` and set the `context.ShouldRenew` property to `true`.</span></span>

<a name="security-authentication-cookie-options"></a>

## <a name="controlling-cookie-options"></a><span data-ttu-id="e0898-163">Cookie のオプションを制御します。</span><span class="sxs-lookup"><span data-stu-id="e0898-163">Controlling cookie options</span></span>

<span data-ttu-id="e0898-164">[CookieAuthenticationOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions)クラスの作成中の cookie を微調整する各種の構成オプションが付属します。</span><span class="sxs-lookup"><span data-stu-id="e0898-164">The [CookieAuthenticationOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) class comes with various configuration options to fine-tune the cookies being created.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e0898-165">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e0898-165">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="e0898-166">ASP.NET Core 2.x が cookie を構成するために使用される Api を統一します。</span><span class="sxs-lookup"><span data-stu-id="e0898-166">ASP.NET Core 2.x unifies the APIs used for configuring cookies.</span></span> <span data-ttu-id="e0898-167">Api は、不使用とマークされている 1.x され、新しい`Cookie`型のプロパティ`CookieBuilder`が導入されました、`CookieAuthenticationOptions`クラスです。</span><span class="sxs-lookup"><span data-stu-id="e0898-167">The 1.x APIs have been marked as obsolete, and a new `Cookie` property of type `CookieBuilder` has been introduced in the `CookieAuthenticationOptions` class.</span></span> <span data-ttu-id="e0898-168">2.x Api に移行することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="e0898-168">It's recommended that you migrate to the 2.x APIs.</span></span>

* <span data-ttu-id="e0898-169">`ClaimsIssuer`使用する発行者には、[発行者](https://docs.microsoft.com/dotnet/api/system.security.claims.claim.issuer)cookie 認証で作成された任意の信頼性情報のプロパティです。</span><span class="sxs-lookup"><span data-stu-id="e0898-169">`ClaimsIssuer` is the issuer to be used for the [Issuer](https://docs.microsoft.com/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication.</span></span>

* <span data-ttu-id="e0898-170">`CookieBuilder.Domain`cookie を提供するドメイン名です。</span><span class="sxs-lookup"><span data-stu-id="e0898-170">`CookieBuilder.Domain` is the domain name to which the cookie is served.</span></span> <span data-ttu-id="e0898-171">既定では、これは、要求に送信されました、ホスト名です。</span><span class="sxs-lookup"><span data-stu-id="e0898-171">By default, this is the host name the request was sent to.</span></span> <span data-ttu-id="e0898-172">ブラウザーでは、cookie、一致するホスト名にのみ機能します。</span><span class="sxs-lookup"><span data-stu-id="e0898-172">The browser only serves the cookie to a matching host name.</span></span> <span data-ttu-id="e0898-173">任意のホストに使用できる cookie は、ドメイン内にこれを調整することもできます。</span><span class="sxs-lookup"><span data-stu-id="e0898-173">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="e0898-174">たとえば、cookie のドメインに設定`.contoso.com`で使用できるように`contoso.com`、 `www.contoso.com`、 `staging.www.contoso.com`, などです。</span><span class="sxs-lookup"><span data-stu-id="e0898-174">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, `staging.www.contoso.com`, etc.</span></span>

* <span data-ttu-id="e0898-175">`CookieBuilder.HttpOnly`クッキーはサーバーにのみアクセスできるかどうかを示すフラグです。</span><span class="sxs-lookup"><span data-stu-id="e0898-175">`CookieBuilder.HttpOnly` is a flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="e0898-176">既定値は`true`します。</span><span class="sxs-lookup"><span data-stu-id="e0898-176">This defaults to `true`.</span></span> <span data-ttu-id="e0898-177">この値を変更する可能性がありますアプリケーションを開く、cookie の盗難にクロスサイト スクリプティングのバグがある、アプリケーションをする必要があります。</span><span class="sxs-lookup"><span data-stu-id="e0898-177">Changing this value may open your application to cookie theft should your application have a Cross-Site Scripting bug.</span></span>

* <span data-ttu-id="e0898-178">`CookieBuilder.Path`同じホスト名で実行されているアプリケーションを分離するために使用します。</span><span class="sxs-lookup"><span data-stu-id="e0898-178">`CookieBuilder.Path` can be used to isolate applications running on the same host name.</span></span> <span data-ttu-id="e0898-179">実行されているアプリがあれば`/app1`だけそのアプリケーションに送信される発行された cookie を制限するし、設定する必要があります、`CookiePath`プロパティを`/app1`です。</span><span class="sxs-lookup"><span data-stu-id="e0898-179">If you have an app running in `/app1` and want to limit the cookies issued to just be sent to that application, then you should set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="e0898-180">これにより、cookie が要求に使用可能なのみ`/app1`または下にあるものです。</span><span class="sxs-lookup"><span data-stu-id="e0898-180">By doing so, the cookie is only available to requests to `/app1` or anything underneath it.</span></span>

* <span data-ttu-id="e0898-181">`CookieBuilder.SameSite`ブラウザーが同じサイト内またはサイト間の要求にアタッチされている cookie を許可するかどうかを指定します。</span><span class="sxs-lookup"><span data-stu-id="e0898-181">`CookieBuilder.SameSite` indicates whether the browser should allow the cookie to be attached to same-site or cross-site requests.</span></span> <span data-ttu-id="e0898-182">既定値は`SameSiteMode.Lax`します。</span><span class="sxs-lookup"><span data-stu-id="e0898-182">This defaults to `SameSiteMode.Lax`.</span></span>

* <span data-ttu-id="e0898-183">`CookieBuilder.SecurePolicy`作成された cookie を HTTPS、HTTP または HTTPS、または要求と同じプロトコルに限られますかどうかを示すフラグです。</span><span class="sxs-lookup"><span data-stu-id="e0898-183">`CookieBuilder.SecurePolicy` is a flag indicating if the cookie created should be limited to HTTPS, HTTP or HTTPS, or the same protocol as the request.</span></span> <span data-ttu-id="e0898-184">既定値は`SameAsRequest`します。</span><span class="sxs-lookup"><span data-stu-id="e0898-184">This defaults to `SameAsRequest`.</span></span>

* <span data-ttu-id="e0898-185">`ExpireTimeSpan``TimeSpan` cookie が期限切れの後にします。</span><span class="sxs-lookup"><span data-stu-id="e0898-185">`ExpireTimeSpan` is the `TimeSpan` after which the cookie expires.</span></span> <span data-ttu-id="e0898-186">Cookie の有効期限日を作成するには、現在の日付と時刻に追加されます。</span><span class="sxs-lookup"><span data-stu-id="e0898-186">It's added to the current date and time to create the expiry date for the cookie.</span></span>

* <span data-ttu-id="e0898-187">`SlidingExpiration`複数の半分の cookie の有効期限の日付をリセットするかどうかを示すフラグです、`ExpireTimeSpan`間隔が経過します。</span><span class="sxs-lookup"><span data-stu-id="e0898-187">`SlidingExpiration` is a flag indicating whether the cookie expiration date resets when more than half of the `ExpireTimeSpan` interval has passed.</span></span> <span data-ttu-id="e0898-188">新しい有効期限日が前方に移動する、現在の日付と`ExpireTimespan`です。</span><span class="sxs-lookup"><span data-stu-id="e0898-188">The new expiry date is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="e0898-189">[絶対有効期限](xref:security/authentication/cookie#security-authentication-absolute-expiry)を使用して設定することができます、`AuthenticationProperties`クラスを呼び出すときに`SignInAsync`です。</span><span class="sxs-lookup"><span data-stu-id="e0898-189">An [absolute expiry time](xref:security/authentication/cookie#security-authentication-absolute-expiry) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="e0898-190">絶対有効期限は、認証 cookie の有効期限の量を制限することによって、アプリケーションのセキュリティを強化できます。</span><span class="sxs-lookup"><span data-stu-id="e0898-190">An absolute expiry can improve the security of your application by limiting the amount of time for which the authentication cookie is valid.</span></span>

<span data-ttu-id="e0898-191">使用する例`CookieAuthenticationOptions`で、`ConfigureServices`のメソッド*Startup.cs*に従います。</span><span class="sxs-lookup"><span data-stu-id="e0898-191">An example of using `CookieAuthenticationOptions` in the `ConfigureServices` method of *Startup.cs* follows:</span></span>

```csharp
services.AddAuthentication()
        .AddCookie(options =>
        {
            options.Cookie.Name = "AuthCookie";
            options.Cookie.Domain = "contoso.com";
            options.Cookie.Path = "/";
            options.Cookie.HttpOnly = true;
            options.Cookie.SameSite = SameSiteMode.Lax;
            options.Cookie.SecurePolicy = CookieSecurePolicy.Always;
        });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e0898-192">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e0898-192">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="e0898-193">`ClaimsIssuer`使用する発行者には、[発行者](https://docs.microsoft.com/dotnet/api/system.security.claims.claim.issuer)ミドルウェアによって作成された任意の信頼性情報のプロパティです。</span><span class="sxs-lookup"><span data-stu-id="e0898-193">`ClaimsIssuer` is the issuer to be used for the [Issuer](https://docs.microsoft.com/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the middleware.</span></span>

* <span data-ttu-id="e0898-194">`CookieDomain`cookie を提供するドメイン名です。</span><span class="sxs-lookup"><span data-stu-id="e0898-194">`CookieDomain` is the domain name to which the cookie is served.</span></span> <span data-ttu-id="e0898-195">既定では、これは、要求に送信されました、ホスト名です。</span><span class="sxs-lookup"><span data-stu-id="e0898-195">By default, this is the host name the request was sent to.</span></span> <span data-ttu-id="e0898-196">ブラウザーでは、cookie、一致するホスト名にのみ機能します。</span><span class="sxs-lookup"><span data-stu-id="e0898-196">The browser only serves the cookie to a matching host name.</span></span> <span data-ttu-id="e0898-197">任意のホストに使用できる cookie は、ドメイン内にこれを調整することもできます。</span><span class="sxs-lookup"><span data-stu-id="e0898-197">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="e0898-198">たとえば、cookie のドメインに設定`.contoso.com`で使用できるように`contoso.com`、 `www.contoso.com`、 `staging.www.contoso.com`, などです。</span><span class="sxs-lookup"><span data-stu-id="e0898-198">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, `staging.www.contoso.com`, etc.</span></span>

* <span data-ttu-id="e0898-199">`CookieHttpOnly`クッキーはサーバーにのみアクセスできるかどうかを示すフラグです。</span><span class="sxs-lookup"><span data-stu-id="e0898-199">`CookieHttpOnly` is a flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="e0898-200">既定値は`true`します。</span><span class="sxs-lookup"><span data-stu-id="e0898-200">This defaults to `true`.</span></span> <span data-ttu-id="e0898-201">この値を変更する可能性がありますアプリケーションを開く、cookie の盗難にクロスサイト スクリプティングのバグがある、アプリケーションをする必要があります。</span><span class="sxs-lookup"><span data-stu-id="e0898-201">Changing this value may open your application to cookie theft should your application have a Cross-Site Scripting bug.</span></span>

* <span data-ttu-id="e0898-202">`CookiePath`同じホスト名で実行されているアプリケーションを分離するために使用します。</span><span class="sxs-lookup"><span data-stu-id="e0898-202">`CookiePath` can be used to isolate applications running on the same host name.</span></span> <span data-ttu-id="e0898-203">実行されているアプリがあれば`/app1`だけそのアプリケーションに送信される発行された cookie を制限するし、設定する必要があります、`CookiePath`プロパティを`/app1`です。</span><span class="sxs-lookup"><span data-stu-id="e0898-203">If you have an app running in `/app1` and want to limit the cookies issued to just be sent to that application, then you should set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="e0898-204">これにより、cookie が要求に使用可能なのみ`/app1`または下にあるものです。</span><span class="sxs-lookup"><span data-stu-id="e0898-204">By doing so, the cookie is only available to requests to `/app1` or anything underneath it.</span></span>

* <span data-ttu-id="e0898-205">`CookieSecure`作成された cookie を HTTPS、HTTP または HTTPS、または要求と同じプロトコルに限られますかどうかを示すフラグです。</span><span class="sxs-lookup"><span data-stu-id="e0898-205">`CookieSecure` is a flag indicating if the cookie created should be limited to HTTPS, HTTP or HTTPS, or the same protocol as the request.</span></span> <span data-ttu-id="e0898-206">既定値は`SameAsRequest`します。</span><span class="sxs-lookup"><span data-stu-id="e0898-206">This defaults to `SameAsRequest`.</span></span>

* <span data-ttu-id="e0898-207">`ExpireTimeSpan``TimeSpan` cookie が期限切れの後にします。</span><span class="sxs-lookup"><span data-stu-id="e0898-207">`ExpireTimeSpan` is the `TimeSpan` after which the cookie expires.</span></span> <span data-ttu-id="e0898-208">Cookie の有効期限日を作成するには、現在の日付と時刻に追加されます。</span><span class="sxs-lookup"><span data-stu-id="e0898-208">It's added to the current date and time to create the expiry date for the cookie.</span></span>

* <span data-ttu-id="e0898-209">`SlidingExpiration`複数の半分の cookie の有効期限の日付をリセットするかどうかを示すフラグです、`ExpireTimeSpan`間隔が経過します。</span><span class="sxs-lookup"><span data-stu-id="e0898-209">`SlidingExpiration` is a flag indicating whether the cookie expiration date resets when more than half of the `ExpireTimeSpan` interval has passed.</span></span> <span data-ttu-id="e0898-210">新しい有効期限日が前方に移動する、現在の日付と`ExpireTimespan`です。</span><span class="sxs-lookup"><span data-stu-id="e0898-210">The new expiry date is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="e0898-211">[絶対有効期限](xref:security/authentication/cookie#security-authentication-absolute-expiry)を使用して設定することができます、`AuthenticationProperties`クラスを呼び出すときに`SignInAsync`です。</span><span class="sxs-lookup"><span data-stu-id="e0898-211">An [absolute expiry time](xref:security/authentication/cookie#security-authentication-absolute-expiry) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="e0898-212">絶対有効期限は、認証 cookie の有効期限の量を制限することによって、アプリケーションのセキュリティを強化できます。</span><span class="sxs-lookup"><span data-stu-id="e0898-212">An absolute expiry can improve the security of your application by limiting the amount of time for which the authentication cookie is valid.</span></span>

<span data-ttu-id="e0898-213">使用する例`CookieAuthenticationOptions`で、`Configure`のメソッド*Startup.cs*に従います。</span><span class="sxs-lookup"><span data-stu-id="e0898-213">An example of using `CookieAuthenticationOptions` in the `Configure` method of *Startup.cs* follows:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    CookieName = "AuthCookie",
    CookieDomain = "contoso.com",
    CookiePath = "/",
    CookieHttpOnly = true,
    CookieSecure = CookieSecurePolicy.Always
});
```

---

## <a name="persistent-cookies-and-absolute-expiry-times"></a><span data-ttu-id="e0898-214">永続的な cookie および絶対有効期限の時間</span><span class="sxs-lookup"><span data-stu-id="e0898-214">Persistent cookies and absolute expiry times</span></span>

<span data-ttu-id="e0898-215">ブラウザー セッション間で永続化し、id とそれを転送する cookie を絶対有効期限をクッキーの有効期限をする可能性があります。</span><span class="sxs-lookup"><span data-stu-id="e0898-215">You may want the cookie expiry to persist across browser sessions and want an absolute expiry to the identity and the cookie transporting it.</span></span> <span data-ttu-id="e0898-216">この永続化は、「次回」にあるチェック ボックス ログインまたは同様のメカニズムを使用して、明示的なユーザーの同意でのみ有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="e0898-216">This persistence should only be enabled with explicit user consent, via a "Remember Me" checkbox on login or a similar mechanism.</span></span> <span data-ttu-id="e0898-217">使用してこれらの操作を行うことができます、`AuthenticationProperties`上のパラメーター、`SignInAsync`メソッドを呼び出すときに[id でサインインし、cookie を作成する](xref:security/authentication/cookie#security-authentication-cookie-middleware-creating-a-cookie)です。</span><span class="sxs-lookup"><span data-stu-id="e0898-217">You can do these things by using the `AuthenticationProperties` parameter on the `SignInAsync` method called when [signing in an identity and creating the cookie](xref:security/authentication/cookie#security-authentication-cookie-middleware-creating-a-cookie).</span></span> <span data-ttu-id="e0898-218">例:</span><span class="sxs-lookup"><span data-stu-id="e0898-218">For example:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e0898-219">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e0898-219">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    "MyCookieAuthenticationScheme",
    principal,
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="e0898-220">`AuthenticationProperties`に上記のコード スニペットで使用されるクラスが存在する、`Microsoft.AspNetCore.Authentication`名前空間。</span><span class="sxs-lookup"><span data-stu-id="e0898-220">The `AuthenticationProperties` class, used in the preceding code snippet, resides in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e0898-221">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e0898-221">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    "MyCookieAuthenticationScheme",
    principal,
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="e0898-222">`AuthenticationProperties`に上記のコード スニペットで使用されるクラスが存在する、`Microsoft.AspNetCore.Http.Authentication`名前空間。</span><span class="sxs-lookup"><span data-stu-id="e0898-222">The `AuthenticationProperties` class, used in the preceding code snippet, resides in the `Microsoft.AspNetCore.Http.Authentication` namespace.</span></span>

---

<span data-ttu-id="e0898-223">上記のコード スニペットは、id およびブラウザー クロージャで回収されなかったを対応する cookie を作成します。</span><span class="sxs-lookup"><span data-stu-id="e0898-223">The preceding code snippet creates an identity and corresponding cookie which survives through browser closures.</span></span> <span data-ttu-id="e0898-224">使用して以前構成スライディング有効期限設定[cookie オプション](xref:security/authentication/cookie#security-authentication-cookie-options)がまだ受け入れられます。</span><span class="sxs-lookup"><span data-stu-id="e0898-224">Any sliding expiration settings previously configured via [cookie options](xref:security/authentication/cookie#security-authentication-cookie-options) are still honored.</span></span> <span data-ttu-id="e0898-225">Cookie には、ブラウザーを閉じる中期限が切れると、ブラウザーをクリアが再起動したらします。</span><span class="sxs-lookup"><span data-stu-id="e0898-225">If the cookie expires whilst the browser is closed, the browser clears it once it is restarted.</span></span>

<a name="security-authentication-absolute-expiry"></a>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e0898-226">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e0898-226">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    "MyCookieAuthenticationScheme",
    principal,
    new AuthenticationProperties
    {
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e0898-227">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e0898-227">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    "MyCookieAuthenticationScheme",
    principal,
    new AuthenticationProperties
    {
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

---

<span data-ttu-id="e0898-228">上記のコード スニペットは、id と対応する cookie が 20 分間は継続を作成します。</span><span class="sxs-lookup"><span data-stu-id="e0898-228">The preceding code snippet creates an identity and corresponding cookie which lasts for 20 minutes.</span></span> <span data-ttu-id="e0898-229">これは、àavhd スライディング有効期限を使用して以前構成[cookie オプション](xref:security/authentication/cookie#security-authentication-cookie-options)です。</span><span class="sxs-lookup"><span data-stu-id="e0898-229">This ignores any sliding expiration settings previously configured via [cookie options](xref:security/authentication/cookie#security-authentication-cookie-options).</span></span>

<span data-ttu-id="e0898-230">`ExpiresUtc`と`IsPersistent`プロパティが相互に排他的です。</span><span class="sxs-lookup"><span data-stu-id="e0898-230">The `ExpiresUtc` and `IsPersistent` properties are mutually exclusive.</span></span>
