---
title: "アプリ間で cookie を共有"
author: rick-anderson
description: "ASP.NET 認証 cookie を共有する方法を説明 4.x および ASP.NET Core アプリケーションです。"
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 01/19/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/compatibility/cookie-sharing
ms.openlocfilehash: 034c080c3459cd3a9e6b412492d1b19d9e6348a4
ms.sourcegitcommit: 3f491f887074310fc0f145cd01a670aa63b969e3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/22/2018
---
# <a name="sharing-cookies-among-apps"></a><span data-ttu-id="b08de-103">アプリ間で cookie を共有</span><span class="sxs-lookup"><span data-stu-id="b08de-103">Sharing cookies among apps</span></span>

<span data-ttu-id="b08de-104">によって[Rick Anderson](https://twitter.com/RickAndMSFT)と[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="b08de-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="b08de-105">Web サイトは多くの場合、連携して、個々 の web アプリで構成されます。</span><span class="sxs-lookup"><span data-stu-id="b08de-105">Websites often consist of individual web apps working together.</span></span> <span data-ttu-id="b08de-106">シングル サインオン (SSO) エクスペリエンスを提供するには、サイト内で web アプリは認証クッキーを共有する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b08de-106">To provide a single sign-on (SSO) experience, web apps within a site must share authentication cookies.</span></span> <span data-ttu-id="b08de-107">このシナリオをサポートするには、データ保護スタックは、共有 Katana cookie 認証および ASP.NET Core cookie 認証チケットを許可します。</span><span class="sxs-lookup"><span data-stu-id="b08de-107">To support this scenario, the data protection stack allows sharing Katana cookie authentication and ASP.NET Core cookie authentication tickets.</span></span>

<span data-ttu-id="b08de-108">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="b08de-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="b08de-109">このサンプルは、cookie の cookie 認証を使用する 3 つのアプリ間で共有を示しています。</span><span class="sxs-lookup"><span data-stu-id="b08de-109">The sample illustrates cookie sharing across three apps that use cookie authentication:</span></span>

* <span data-ttu-id="b08de-110">ASP.NET Core 2.0 Razor ページのアプリを使用せず[ASP.NET Core Id](xref:security/authentication/identity)</span><span class="sxs-lookup"><span data-stu-id="b08de-110">ASP.NET Core 2.0 Razor Pages app without using [ASP.NET Core Identity](xref:security/authentication/identity)</span></span>
* <span data-ttu-id="b08de-111">ASP.NET Core Id を持つ ASP.NET Core 2.0 MVC アプリ</span><span class="sxs-lookup"><span data-stu-id="b08de-111">ASP.NET Core 2.0 MVC app with ASP.NET Core Identity</span></span>
* <span data-ttu-id="b08de-112">ASP.NET Identity Framework 4.6.1 の ASP.NET MVC アプリ</span><span class="sxs-lookup"><span data-stu-id="b08de-112">ASP.NET Framework 4.6.1 MVC app with ASP.NET Identity</span></span>

<span data-ttu-id="b08de-113">次の例。</span><span class="sxs-lookup"><span data-stu-id="b08de-113">In the examples that follow:</span></span>

* <span data-ttu-id="b08de-114">認証 cookie の名前がの共通の値に設定されている`.AspNet.SharedCookie`です。</span><span class="sxs-lookup"><span data-stu-id="b08de-114">The authentication cookie name is set to a common value of `.AspNet.SharedCookie`.</span></span>
* <span data-ttu-id="b08de-115">`AuthenticationType`に設定されている`Identity.Application`明示的にまたは既定のいずれか。</span><span class="sxs-lookup"><span data-stu-id="b08de-115">The `AuthenticationType` is set to `Identity.Application` either explicitly or by default.</span></span>
* <span data-ttu-id="b08de-116">[CookieAuthenticationDefaults.AuthenticationScheme](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationdefaults.authenticationscheme) is used as the authentication scheme.</span><span class="sxs-lookup"><span data-stu-id="b08de-116">[CookieAuthenticationDefaults.AuthenticationScheme](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationdefaults.authenticationscheme) is used as the authentication scheme.</span></span> <span data-ttu-id="b08de-117">値に解決される定数`Cookies`です。</span><span class="sxs-lookup"><span data-stu-id="b08de-117">The constant resolves to a value of `Cookies`.</span></span>
* <span data-ttu-id="b08de-118">Cookie 認証ミドルウェアの実装を使用して[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider)です。</span><span class="sxs-lookup"><span data-stu-id="b08de-118">The cookie authentication middleware uses an implementation of [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider).</span></span> <span data-ttu-id="b08de-119">`DataProtectionProvider`暗号化と認証 cookie のペイロード データの暗号化解除用のデータ保護サービスを提供します。</span><span class="sxs-lookup"><span data-stu-id="b08de-119">`DataProtectionProvider` provides data protection services for the encryption and decryption of authentication cookie payload data.</span></span> <span data-ttu-id="b08de-120">`DataProtectionProvider`インスタンスは、アプリの他の部分で使用するデータ保護システムから分離します。</span><span class="sxs-lookup"><span data-stu-id="b08de-120">The `DataProtectionProvider` instance is isolated from the data protection system used by other parts of the app.</span></span>
  * <span data-ttu-id="b08de-121">一般的に使われる[データ保護キー](xref:security/data-protection/implementation/key-management)記憶域の場所を使用します。</span><span class="sxs-lookup"><span data-stu-id="b08de-121">A common [data protection key](xref:security/data-protection/implementation/key-management) storage location is used.</span></span> <span data-ttu-id="b08de-122">サンプル アプリで使用するという名前のフォルダー*キーリング*データ保護キーを保持するために、ソリューションのルートに位置します。</span><span class="sxs-lookup"><span data-stu-id="b08de-122">The sample app uses a folder named *KeyRing* at the root of the solution to hold the data protection keys.</span></span>
  * <span data-ttu-id="b08de-123">[DataProtectionProvider.Create(System.IO.DirectoryInfo)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_) accepts a [DirectoryInfo](/dotnet/api/system.io.directoryinfo) for use with authentication cookies.</span><span class="sxs-lookup"><span data-stu-id="b08de-123">[DataProtectionProvider.Create(System.IO.DirectoryInfo)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_) accepts a [DirectoryInfo](/dotnet/api/system.io.directoryinfo) for use with authentication cookies.</span></span> <span data-ttu-id="b08de-124">サンプル アプリのパスを提供する、*キーリング*フォルダー`DirectoryInfo`です。</span><span class="sxs-lookup"><span data-stu-id="b08de-124">The sample app provides the path of the *KeyRing* folder to `DirectoryInfo`.</span></span>
  * <span data-ttu-id="b08de-125">[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider)が必要です、 [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet パッケージです。</span><span class="sxs-lookup"><span data-stu-id="b08de-125">[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) requires the [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet package.</span></span> <span data-ttu-id="b08de-126">このパッケージの ASP.NET Core 2.0 と以降のアプリの入手、参照、 [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage です。</span><span class="sxs-lookup"><span data-stu-id="b08de-126">To obtain this package for ASP.NET Core 2.0 and later apps, reference the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage.</span></span> <span data-ttu-id="b08de-127">.NET Framework を対象とする場合にパッケージ参照の追加`Microsoft.AspNetCore.DataProtection.Extensions`です。</span><span class="sxs-lookup"><span data-stu-id="b08de-127">When targeting .NET Framework, add a package reference to `Microsoft.AspNetCore.DataProtection.Extensions`.</span></span>

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a><span data-ttu-id="b08de-128">ASP.NET Core アプリケーション間での認証クッキーを共有します。</span><span class="sxs-lookup"><span data-stu-id="b08de-128">Share authentication cookies among ASP.NET Core apps</span></span>

<span data-ttu-id="b08de-129">ASP.NET Core の Id を使用する: 場合</span><span class="sxs-lookup"><span data-stu-id="b08de-129">When using ASP.NET Core Identity:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b08de-130">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b08de-130">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b08de-131">`ConfigureServices`メソッドを使用して、 [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) cookie のデータ保護サービスをセットアップする拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="b08de-131">In the `ConfigureServices` method, use the [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) extension method to set up the data protection service for cookies.</span></span>

[!code-csharp[Main](cookie-sharing/sample/CookieAuthWithIdentity.Core/Startup.cs?name=snippet1)]

<span data-ttu-id="b08de-132">参照してください、 *CookieAuthWithIdentity.Core*プロジェクトで、[のサンプル コード](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/)([をダウンロードする方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="b08de-132">See the *CookieAuthWithIdentity.Core* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b08de-133">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b08de-133">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b08de-134">`Configure`メソッドを使用して、 [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions)を設定します。</span><span class="sxs-lookup"><span data-stu-id="b08de-134">In the `Configure` method, use the [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) to set up:</span></span>

* <span data-ttu-id="b08de-135">Cookie のデータ保護サービスです。</span><span class="sxs-lookup"><span data-stu-id="b08de-135">The data protection service for cookies.</span></span>
* <span data-ttu-id="b08de-136">`AuthenticationScheme`合わせて ASP.NET 4.x です。</span><span class="sxs-lookup"><span data-stu-id="b08de-136">The `AuthenticationScheme` to match ASP.NET 4.x.</span></span>

```csharp
app.AddIdentity<ApplicationUser, IdentityRole>(options =>
{
    options.Cookies.ApplicationCookie.AuthenticationScheme = 
        "ApplicationCookie";

    var protectionProvider = 
        DataProtectionProvider.Create(
            new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"));

    options.Cookies.ApplicationCookie.DataProtectionProvider = 
        protectionProvider;

    options.Cookies.ApplicationCookie.TicketDataFormat = 
        new TicketDataFormat(protectionProvider.CreateProtector(
            "Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware", 
            "Cookies", 
            "v2"));
});
```

---

<span data-ttu-id="b08de-137">ときに、cookie を直接使用するには。</span><span class="sxs-lookup"><span data-stu-id="b08de-137">When using cookies directly:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b08de-138">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b08de-138">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](cookie-sharing/sample/CookieAuth.Core/Startup.cs?name=snippet1)]

<span data-ttu-id="b08de-139">参照してください、 *CookieAuth.Core*プロジェクトで、[のサンプル コード](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/)([をダウンロードする方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="b08de-139">See the *CookieAuth.Core* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b08de-140">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b08de-140">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = 
        DataProtectionProvider.Create(
            new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"))
});
```

---

## <a name="encrypting-data-protection-keys-at-rest"></a><span data-ttu-id="b08de-141">静止したデータ保護キーの暗号化</span><span class="sxs-lookup"><span data-stu-id="b08de-141">Encrypting data protection keys at rest</span></span>

<span data-ttu-id="b08de-142">運用環境の配置、構成、 `DataProtectionProvider` DPAPI または X509Certificate で残りの部分でキーを暗号化します。</span><span class="sxs-lookup"><span data-stu-id="b08de-142">For production deployments, configure the `DataProtectionProvider` to encrypt keys at rest with DPAPI or an X509Certificate.</span></span> <span data-ttu-id="b08de-143">参照してください[キー時の暗号化](xref:security/data-protection/implementation/key-encryption-at-rest)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="b08de-143">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more information.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b08de-144">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b08de-144">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("thumbprint");
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b08de-145">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b08de-145">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = DataProtectionProvider.Create(
        new DirectoryInfo(@"PATH_TO_KEY_RING"),
        configure =>
        {
            configure.ProtectKeysWithCertificate("thumbprint");
        })
});
```

---

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a><span data-ttu-id="b08de-146">ASP.NET との間の認証クッキーを共有 4.x および ASP.NET Core アプリケーション</span><span class="sxs-lookup"><span data-stu-id="b08de-146">Sharing authentication cookies between ASP.NET 4.x and ASP.NET Core apps</span></span>

<span data-ttu-id="b08de-147">ASP.NET Core cookie 認証ミドルウェアと互換性がある認証 cookie を生成するを Katana cookie 認証ミドルウェアを使用する ASP.NET 4.x アプリを構成することができます。</span><span class="sxs-lookup"><span data-stu-id="b08de-147">ASP.NET 4.x apps which use Katana cookie authentication middleware can be configured to generate authentication cookies that are compatible with the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="b08de-148">これにより、サイト全体で滑らかな SSO エクスペリエンスを提供しつつ、段階的な部分、大規模なサイトの個々 のアプリケーションをアップグレードできます。</span><span class="sxs-lookup"><span data-stu-id="b08de-148">This allows upgrading a large site's individual apps piecemeal while providing a smooth SSO experience across the site.</span></span>

> [!TIP]
> <span data-ttu-id="b08de-149">アプリは、Katana cookie 認証ミドルウェアを使用しているときに呼び出す`UseCookieAuthentication`プロジェクトの*Startup.Auth.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="b08de-149">When an app uses Katana cookie authentication middleware, it calls `UseCookieAuthentication` in the project's *Startup.Auth.cs* file.</span></span> <span data-ttu-id="b08de-150">ASP.NET 4.x web アプリのプロジェクトでは、Visual Studio 2013 で作成され、後で、既定では、Katana cookie 認証ミドルウェアを使用しています。</span><span class="sxs-lookup"><span data-stu-id="b08de-150">ASP.NET 4.x web app projects created with Visual Studio 2013 and later use the Katana cookie authentication middleware by default.</span></span>

> [!NOTE]
> <span data-ttu-id="b08de-151">ASP.NET 4.x アプリが .NET Framework 4.5.1 を対象する必要がありますまたはそれ以降。</span><span class="sxs-lookup"><span data-stu-id="b08de-151">An ASP.NET 4.x app must target .NET Framework 4.5.1 or higher.</span></span> <span data-ttu-id="b08de-152">それ以外の場合、必要な NuGet パッケージは、インストールに失敗します。</span><span class="sxs-lookup"><span data-stu-id="b08de-152">Otherwise, the necessary NuGet packages fail to install.</span></span>

<span data-ttu-id="b08de-153">ASP.NET 4.x 用アプリと ASP.NET Core の間での認証クッキーを共有するには、前述のように ASP.NET Core アプリケーションを構成するし、以下の手順に従って、ASP.NET 4.x アプリを構成します。</span><span class="sxs-lookup"><span data-stu-id="b08de-153">To share authentication cookies among ASP.NET 4.x apps and ASP.NET Core apps, configure the ASP.NET Core app as stated above, then configure the ASP.NET 4.x apps by following the steps below.</span></span>

1. <span data-ttu-id="b08de-154">パッケージをインストール[Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/)各 ASP.NET 4.x アプリにします。</span><span class="sxs-lookup"><span data-stu-id="b08de-154">Install the package [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) into each ASP.NET 4.x app.</span></span>

2. <span data-ttu-id="b08de-155">*Startup.Auth.cs*への呼び出しを見つける`UseCookieAuthentication`し、次のように変更します。</span><span class="sxs-lookup"><span data-stu-id="b08de-155">In *Startup.Auth.cs*, locate the call to `UseCookieAuthentication` and modify it as follows.</span></span> <span data-ttu-id="b08de-156">ASP.NET Core cookie 認証ミドルウェアで使用される名前と一致する cookie の名前を変更します。</span><span class="sxs-lookup"><span data-stu-id="b08de-156">Change the cookie name to match the name used by the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="b08de-157">インスタンスを提供する`DataProtectionProvider`一般的なデータ保護キー記憶域の場所を初期化します。</span><span class="sxs-lookup"><span data-stu-id="b08de-157">Provide an instance of a `DataProtectionProvider` initialized to the common data protection key storage location.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b08de-158">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b08de-158">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/App_Start/Startup.Auth.cs?name=snippet1)]

<span data-ttu-id="b08de-159">参照してください、 *CookieAuthWithIdentity.NETFramework*プロジェクトで、[のサンプル コード](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/)([をダウンロードする方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="b08de-159">See the *CookieAuthWithIdentity.NETFramework* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="b08de-160">ユーザー id を生成するには、認証の種類で定義された型と一致しなければならない`AuthenticationType`で設定された`UseCookieAuthentication`です。</span><span class="sxs-lookup"><span data-stu-id="b08de-160">When generating a user identity, the authentication type must match the type defined in `AuthenticationType` set with `UseCookieAuthentication`.</span></span>

<span data-ttu-id="b08de-161">*Models/IdentityModels.cs*:</span><span class="sxs-lookup"><span data-stu-id="b08de-161">*Models/IdentityModels.cs*:</span></span>

[!code-csharp[Main](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/Models/IdentityModels.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b08de-162">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b08de-162">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b08de-163">設定、`CookieManager`相互運用`ChunkingCookieManager`チャンク形式は互換性のあるようにします。</span><span class="sxs-lookup"><span data-stu-id="b08de-163">Set the `CookieManager` to interop `ChunkingCookieManager` so the chunking format is compatible.</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    AuthenticationType = DefaultAuthenticationTypes.ApplicationCookie,
    CookieName = ".AspNetCore.Cookies",
    // CookieName = ".AspNetCore.ApplicationCookie", (if using ASP.NET Identity)
    // CookiePath = "...", (if necessary)
    // ...
    TicketDataFormat = new AspNetTicketDataFormat(
        new DataProtectorShim(
            DataProtectionProvider.Create(
                new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"))
            .CreateProtector(
                "Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware",
                "Cookies", 
                "v2"))),
    CookieManager = new ChunkingCookieManager()
});
```

---

## <a name="use-a-common-user-database"></a><span data-ttu-id="b08de-164">一般的なユーザー データベースを使用します。</span><span class="sxs-lookup"><span data-stu-id="b08de-164">Use a common user database</span></span>

<span data-ttu-id="b08de-165">各アプリケーションの id システムは、同じユーザー データベースを指すことを確認します。</span><span class="sxs-lookup"><span data-stu-id="b08de-165">Confirm that the identity system for each app is pointed at the same user database.</span></span> <span data-ttu-id="b08de-166">それ以外の場合、id システムでは、そのデータベース内の情報に対する認証クッキーの情報と一致しようとしたときに実行時にエラーが生成されます。</span><span class="sxs-lookup"><span data-stu-id="b08de-166">Otherwise, the identity system produces failures at runtime when it attempts to match the information in the authentication cookie against the information in its database.</span></span>
