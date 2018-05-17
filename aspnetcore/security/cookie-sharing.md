---
title: ASP.NET および ASP.NET Core とアプリ間で cookie を共有します。
author: rick-anderson
description: ASP.NET 認証 cookie を共有する方法を説明 4.x および ASP.NET Core アプリケーションです。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/19/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/cookie-sharing
ms.openlocfilehash: 5f77377f168993d48686217adac54a75313766ec
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/12/2018
---
# <a name="share-cookies-among-apps-with-aspnet-and-aspnet-core"></a><span data-ttu-id="10637-103">ASP.NET および ASP.NET Core とアプリ間で cookie を共有します。</span><span class="sxs-lookup"><span data-stu-id="10637-103">Share cookies among apps with ASP.NET and ASP.NET Core</span></span>

<span data-ttu-id="10637-104">によって[Rick Anderson](https://twitter.com/RickAndMSFT)と[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="10637-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="10637-105">Web サイトは多くの場合、連携して、個々 の web アプリで構成されます。</span><span class="sxs-lookup"><span data-stu-id="10637-105">Websites often consist of individual web apps working together.</span></span> <span data-ttu-id="10637-106">シングル サインオン (SSO) エクスペリエンスを提供するには、サイト内で web アプリは認証クッキーを共有する必要があります。</span><span class="sxs-lookup"><span data-stu-id="10637-106">To provide a single sign-on (SSO) experience, web apps within a site must share authentication cookies.</span></span> <span data-ttu-id="10637-107">このシナリオをサポートするには、データ保護スタックは、共有 Katana cookie 認証および ASP.NET Core cookie 認証チケットを許可します。</span><span class="sxs-lookup"><span data-stu-id="10637-107">To support this scenario, the data protection stack allows sharing Katana cookie authentication and ASP.NET Core cookie authentication tickets.</span></span>

<span data-ttu-id="10637-108">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="10637-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="10637-109">このサンプルは、cookie の cookie 認証を使用する 3 つのアプリ間で共有を示しています。</span><span class="sxs-lookup"><span data-stu-id="10637-109">The sample illustrates cookie sharing across three apps that use cookie authentication:</span></span>

* <span data-ttu-id="10637-110">ASP.NET Core 2.0 Razor ページのアプリを使用せず[ASP.NET Core Id](xref:security/authentication/identity)</span><span class="sxs-lookup"><span data-stu-id="10637-110">ASP.NET Core 2.0 Razor Pages app without using [ASP.NET Core Identity](xref:security/authentication/identity)</span></span>
* <span data-ttu-id="10637-111">ASP.NET Core Id を持つ ASP.NET Core 2.0 MVC アプリ</span><span class="sxs-lookup"><span data-stu-id="10637-111">ASP.NET Core 2.0 MVC app with ASP.NET Core Identity</span></span>
* <span data-ttu-id="10637-112">ASP.NET Identity Framework 4.6.1 の ASP.NET MVC アプリ</span><span class="sxs-lookup"><span data-stu-id="10637-112">ASP.NET Framework 4.6.1 MVC app with ASP.NET Identity</span></span>

<span data-ttu-id="10637-113">次の例。</span><span class="sxs-lookup"><span data-stu-id="10637-113">In the examples that follow:</span></span>

* <span data-ttu-id="10637-114">認証 cookie の名前がの共通の値に設定されている`.AspNet.SharedCookie`です。</span><span class="sxs-lookup"><span data-stu-id="10637-114">The authentication cookie name is set to a common value of `.AspNet.SharedCookie`.</span></span>
* <span data-ttu-id="10637-115">`AuthenticationType`に設定されている`Identity.Application`明示的にまたは既定のいずれか。</span><span class="sxs-lookup"><span data-stu-id="10637-115">The `AuthenticationType` is set to `Identity.Application` either explicitly or by default.</span></span>
* <span data-ttu-id="10637-116">一般的なアプリ名を使用してデータ保護キーを共有するデータ保護システムを有効に (`SharedCookieApp`)。</span><span class="sxs-lookup"><span data-stu-id="10637-116">A common app name is used to enable the data protection system to share data protection keys (`SharedCookieApp`).</span></span>
* <span data-ttu-id="10637-117">`Identity.Application` 認証スキームとして使用されます。</span><span class="sxs-lookup"><span data-stu-id="10637-117">`Identity.Application` is used as the authentication scheme.</span></span> <span data-ttu-id="10637-118">どのような式を使用して、一貫して使用する必要があります*内外*共有 cookie アプリか、または明示的に設定することで、既定のスキームとして。</span><span class="sxs-lookup"><span data-stu-id="10637-118">Whatever scheme is used, it must be used consistently *within and across* the shared cookie apps either as the default scheme or by explicitly setting it.</span></span> <span data-ttu-id="10637-119">ためのアプリ間で一貫したスキームを使用する必要があります、cookie を暗号化および暗号化スキームが使用されます。</span><span class="sxs-lookup"><span data-stu-id="10637-119">The scheme is used when encrypting and decrypting cookies, so a consistent scheme must be used across apps.</span></span>
* <span data-ttu-id="10637-120">一般的に使われる[データ保護キー](xref:security/data-protection/implementation/key-management)記憶域の場所を使用します。</span><span class="sxs-lookup"><span data-stu-id="10637-120">A common [data protection key](xref:security/data-protection/implementation/key-management) storage location is used.</span></span> <span data-ttu-id="10637-121">サンプル アプリで使用するという名前のフォルダー*キーリング*データ保護キーを保持するために、ソリューションのルートに位置します。</span><span class="sxs-lookup"><span data-stu-id="10637-121">The sample app uses a folder named *KeyRing* at the root of the solution to hold the data protection keys.</span></span>
* <span data-ttu-id="10637-122">アプリでは、ASP.NET Core、 [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem)キー記憶域の場所を設定するために使用します。</span><span class="sxs-lookup"><span data-stu-id="10637-122">In the ASP.NET Core apps, [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) is used to set the key storage location.</span></span> <span data-ttu-id="10637-123">[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname)一般的なアプリの共有名を構成するために使用します。</span><span class="sxs-lookup"><span data-stu-id="10637-123">[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) is used to configure a common shared app name.</span></span>
* <span data-ttu-id="10637-124">Cookie 認証ミドルウェアを .NET Framework アプリでの実装を使用して[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider)です。</span><span class="sxs-lookup"><span data-stu-id="10637-124">In the .NET Framework app, the cookie authentication middleware uses an implementation of [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider).</span></span> <span data-ttu-id="10637-125">`DataProtectionProvider` 暗号化と認証 cookie のペイロード データの暗号化解除用のデータ保護サービスを提供します。</span><span class="sxs-lookup"><span data-stu-id="10637-125">`DataProtectionProvider` provides data protection services for the encryption and decryption of authentication cookie payload data.</span></span> <span data-ttu-id="10637-126">`DataProtectionProvider`インスタンスは、アプリの他の部分で使用するデータ保護システムから分離します。</span><span class="sxs-lookup"><span data-stu-id="10637-126">The `DataProtectionProvider` instance is isolated from the data protection system used by other parts of the app.</span></span>
  * <span data-ttu-id="10637-127">[DataProtectionProvider.Create(System.IO.DirectoryInfo, Action\<IDataProtectionBuilder>)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_System_Action_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder__) accepts a [DirectoryInfo](/dotnet/api/system.io.directoryinfo) to specify the location for data protection key storage.</span><span class="sxs-lookup"><span data-stu-id="10637-127">[DataProtectionProvider.Create(System.IO.DirectoryInfo, Action\<IDataProtectionBuilder>)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_System_Action_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder__) accepts a [DirectoryInfo](/dotnet/api/system.io.directoryinfo) to specify the location for data protection key storage.</span></span> <span data-ttu-id="10637-128">サンプル アプリのパスを提供する、*キーリング*フォルダー`DirectoryInfo`です。</span><span class="sxs-lookup"><span data-stu-id="10637-128">The sample app provides the path of the *KeyRing* folder to `DirectoryInfo`.</span></span> <span data-ttu-id="10637-129">[DataProtectionBuilderExtensions.SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionBuilderExtensions_SetApplicationName_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_)共通のアプリ名を設定します。</span><span class="sxs-lookup"><span data-stu-id="10637-129">[DataProtectionBuilderExtensions.SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionBuilderExtensions_SetApplicationName_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_) sets the common app name.</span></span>
  * <span data-ttu-id="10637-130">[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider)が必要です、 [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet パッケージです。</span><span class="sxs-lookup"><span data-stu-id="10637-130">[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) requires the [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet package.</span></span> <span data-ttu-id="10637-131">このパッケージの ASP.NET Core 2.0 と以降のアプリの入手、参照、 [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage です。</span><span class="sxs-lookup"><span data-stu-id="10637-131">To obtain this package for ASP.NET Core 2.0 and later apps, reference the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage.</span></span> <span data-ttu-id="10637-132">.NET Framework を対象とする場合にパッケージ参照の追加`Microsoft.AspNetCore.DataProtection.Extensions`です。</span><span class="sxs-lookup"><span data-stu-id="10637-132">When targeting the .NET Framework, add a package reference to `Microsoft.AspNetCore.DataProtection.Extensions`.</span></span>

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a><span data-ttu-id="10637-133">ASP.NET Core アプリケーション間での認証クッキーを共有します。</span><span class="sxs-lookup"><span data-stu-id="10637-133">Share authentication cookies among ASP.NET Core apps</span></span>

<span data-ttu-id="10637-134">ASP.NET Core の Id を使用する: 場合</span><span class="sxs-lookup"><span data-stu-id="10637-134">When using ASP.NET Core Identity:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="10637-135">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="10637-135">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="10637-136">`ConfigureServices`メソッドを使用して、 [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) cookie のデータ保護サービスをセットアップする拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="10637-136">In the `ConfigureServices` method, use the [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) extension method to set up the data protection service for cookies.</span></span>

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.Core/Startup.cs?name=snippet1)]

<span data-ttu-id="10637-137">データ保護キーとアプリ名は、アプリ間で共有する必要があります。</span><span class="sxs-lookup"><span data-stu-id="10637-137">Data protection keys and the app name must be shared among apps.</span></span> <span data-ttu-id="10637-138">アプリでは、サンプル、`GetKeyRingDirInfo`に共通のキー記憶域の場所を返します、 [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem)メソッドです。</span><span class="sxs-lookup"><span data-stu-id="10637-138">In the sample apps, `GetKeyRingDirInfo` returns the common key storage location to the [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) method.</span></span> <span data-ttu-id="10637-139">使用して[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname)一般的なアプリの共有名を構成する (`SharedCookieApp`サンプル)。</span><span class="sxs-lookup"><span data-stu-id="10637-139">Use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) to configure a common shared app name (`SharedCookieApp` in the sample).</span></span> <span data-ttu-id="10637-140">詳細については、次を参照してください。[データ保護を構成する](xref:security/data-protection/configuration/overview)です。</span><span class="sxs-lookup"><span data-stu-id="10637-140">For more information, see [Configuring Data Protection](xref:security/data-protection/configuration/overview).</span></span>

<span data-ttu-id="10637-141">参照してください、 *CookieAuthWithIdentity.Core*プロジェクトで、[のサンプル コード](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/)([をダウンロードする方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="10637-141">See the *CookieAuthWithIdentity.Core* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="10637-142">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="10637-142">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="10637-143">`Configure`メソッドを使用して、 [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions)を設定します。</span><span class="sxs-lookup"><span data-stu-id="10637-143">In the `Configure` method, use the [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) to set up:</span></span>

* <span data-ttu-id="10637-144">Cookie のデータ保護サービスです。</span><span class="sxs-lookup"><span data-stu-id="10637-144">The data protection service for cookies.</span></span>
* <span data-ttu-id="10637-145">`AuthenticationScheme`合わせて ASP.NET 4.x です。</span><span class="sxs-lookup"><span data-stu-id="10637-145">The `AuthenticationScheme` to match ASP.NET 4.x.</span></span>

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

<span data-ttu-id="10637-146">ときに、cookie を直接使用するには。</span><span class="sxs-lookup"><span data-stu-id="10637-146">When using cookies directly:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="10637-147">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="10637-147">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](cookie-sharing/sample/CookieAuth.Core/Startup.cs?name=snippet1)]

<span data-ttu-id="10637-148">データ保護キーとアプリ名は、アプリ間で共有する必要があります。</span><span class="sxs-lookup"><span data-stu-id="10637-148">Data protection keys and the app name must be shared among apps.</span></span> <span data-ttu-id="10637-149">アプリでは、サンプル、`GetKeyRingDirInfo`に共通のキー記憶域の場所を返します、 [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem)メソッドです。</span><span class="sxs-lookup"><span data-stu-id="10637-149">In the sample apps, `GetKeyRingDirInfo` returns the common key storage location to the [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) method.</span></span> <span data-ttu-id="10637-150">使用して[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname)一般的なアプリの共有名を構成する (`SharedCookieApp`サンプル)。</span><span class="sxs-lookup"><span data-stu-id="10637-150">Use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) to configure a common shared app name (`SharedCookieApp` in the sample).</span></span> <span data-ttu-id="10637-151">詳細については、次を参照してください。[データ保護を構成する](xref:security/data-protection/configuration/overview)です。</span><span class="sxs-lookup"><span data-stu-id="10637-151">For more information, see [Configuring Data Protection](xref:security/data-protection/configuration/overview).</span></span> 

<span data-ttu-id="10637-152">参照してください、 *CookieAuth.Core*プロジェクトで、[のサンプル コード](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/)([をダウンロードする方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="10637-152">See the *CookieAuth.Core* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="10637-153">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="10637-153">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = 
        DataProtectionProvider.Create(
            new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"))
});
```

---

## <a name="encrypting-data-protection-keys-at-rest"></a><span data-ttu-id="10637-154">静止したデータ保護キーの暗号化</span><span class="sxs-lookup"><span data-stu-id="10637-154">Encrypting data protection keys at rest</span></span>

<span data-ttu-id="10637-155">運用環境の配置、構成、 `DataProtectionProvider` DPAPI または X509Certificate で残りの部分でキーを暗号化します。</span><span class="sxs-lookup"><span data-stu-id="10637-155">For production deployments, configure the `DataProtectionProvider` to encrypt keys at rest with DPAPI or an X509Certificate.</span></span> <span data-ttu-id="10637-156">参照してください[キー時の暗号化](xref:security/data-protection/implementation/key-encryption-at-rest)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="10637-156">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more information.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="10637-157">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="10637-157">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("thumbprint");
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="10637-158">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="10637-158">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a><span data-ttu-id="10637-159">ASP.NET との間の認証クッキーを共有 4.x および ASP.NET Core アプリケーション</span><span class="sxs-lookup"><span data-stu-id="10637-159">Sharing authentication cookies between ASP.NET 4.x and ASP.NET Core apps</span></span>

<span data-ttu-id="10637-160">ASP.NET Core cookie 認証ミドルウェアと互換性がある認証 cookie を生成するを Katana cookie 認証ミドルウェアを使用する ASP.NET 4.x アプリを構成することができます。</span><span class="sxs-lookup"><span data-stu-id="10637-160">ASP.NET 4.x apps which use Katana cookie authentication middleware can be configured to generate authentication cookies that are compatible with the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="10637-161">これにより、サイト全体で滑らかな SSO エクスペリエンスを提供しつつ、段階的な部分、大規模なサイトの個々 のアプリケーションをアップグレードできます。</span><span class="sxs-lookup"><span data-stu-id="10637-161">This allows upgrading a large site's individual apps piecemeal while providing a smooth SSO experience across the site.</span></span>

> [!TIP]
> <span data-ttu-id="10637-162">アプリは、Katana cookie 認証ミドルウェアを使用しているときに呼び出す`UseCookieAuthentication`プロジェクトの*Startup.Auth.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="10637-162">When an app uses Katana cookie authentication middleware, it calls `UseCookieAuthentication` in the project's *Startup.Auth.cs* file.</span></span> <span data-ttu-id="10637-163">ASP.NET 4.x web アプリのプロジェクトでは、Visual Studio 2013 で作成され、後で、既定では、Katana cookie 認証ミドルウェアを使用しています。</span><span class="sxs-lookup"><span data-stu-id="10637-163">ASP.NET 4.x web app projects created with Visual Studio 2013 and later use the Katana cookie authentication middleware by default.</span></span>

> [!NOTE]
> <span data-ttu-id="10637-164">ASP.NET 4.x アプリが .NET Framework 4.5.1 を対象する必要がありますまたはそれ以降。</span><span class="sxs-lookup"><span data-stu-id="10637-164">An ASP.NET 4.x app must target .NET Framework 4.5.1 or higher.</span></span> <span data-ttu-id="10637-165">それ以外の場合、必要な NuGet パッケージは、インストールに失敗します。</span><span class="sxs-lookup"><span data-stu-id="10637-165">Otherwise, the necessary NuGet packages fail to install.</span></span>

<span data-ttu-id="10637-166">ASP.NET 4.x 用アプリと ASP.NET Core の間での認証クッキーを共有するには、前述のように ASP.NET Core アプリケーションを構成するし、以下の手順に従って、ASP.NET 4.x アプリを構成します。</span><span class="sxs-lookup"><span data-stu-id="10637-166">To share authentication cookies among ASP.NET 4.x apps and ASP.NET Core apps, configure the ASP.NET Core app as stated above, then configure the ASP.NET 4.x apps by following the steps below.</span></span>

1. <span data-ttu-id="10637-167">パッケージをインストール[Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/)各 ASP.NET 4.x アプリにします。</span><span class="sxs-lookup"><span data-stu-id="10637-167">Install the package [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) into each ASP.NET 4.x app.</span></span>

2. <span data-ttu-id="10637-168">*Startup.Auth.cs*への呼び出しを見つける`UseCookieAuthentication`し、次のように変更します。</span><span class="sxs-lookup"><span data-stu-id="10637-168">In *Startup.Auth.cs*, locate the call to `UseCookieAuthentication` and modify it as follows.</span></span> <span data-ttu-id="10637-169">ASP.NET Core cookie 認証ミドルウェアで使用される名前と一致する cookie の名前を変更します。</span><span class="sxs-lookup"><span data-stu-id="10637-169">Change the cookie name to match the name used by the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="10637-170">インスタンスを提供する`DataProtectionProvider`一般的なデータ保護キー記憶域の場所を初期化します。</span><span class="sxs-lookup"><span data-stu-id="10637-170">Provide an instance of a `DataProtectionProvider` initialized to the common data protection key storage location.</span></span> <span data-ttu-id="10637-171">アプリ名が、cookie を共有しているすべてのアプリで使用される共通のアプリ名 に設定されていることを確認してください`SharedCookieApp`サンプル アプリでします。</span><span class="sxs-lookup"><span data-stu-id="10637-171">Make sure that the app name is set to the common app name used by all apps that share cookies, `SharedCookieApp` in the sample app.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="10637-172">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="10637-172">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/App_Start/Startup.Auth.cs?name=snippet1)]

<span data-ttu-id="10637-173">参照してください、 *CookieAuthWithIdentity.NETFramework*プロジェクトで、[のサンプル コード](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/)([をダウンロードする方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="10637-173">See the *CookieAuthWithIdentity.NETFramework* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="10637-174">ユーザー id を生成するには、認証の種類で定義された型と一致しなければならない`AuthenticationType`で設定された`UseCookieAuthentication`です。</span><span class="sxs-lookup"><span data-stu-id="10637-174">When generating a user identity, the authentication type must match the type defined in `AuthenticationType` set with `UseCookieAuthentication`.</span></span>

<span data-ttu-id="10637-175">*Models/IdentityModels.cs*:</span><span class="sxs-lookup"><span data-stu-id="10637-175">*Models/IdentityModels.cs*:</span></span>

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/Models/IdentityModels.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="10637-176">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="10637-176">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="10637-177">設定、`CookieManager`相互運用`ChunkingCookieManager`チャンク形式は互換性のあるようにします。</span><span class="sxs-lookup"><span data-stu-id="10637-177">Set the `CookieManager` to interop `ChunkingCookieManager` so the chunking format is compatible.</span></span>

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

## <a name="use-a-common-user-database"></a><span data-ttu-id="10637-178">一般的なユーザー データベースを使用します。</span><span class="sxs-lookup"><span data-stu-id="10637-178">Use a common user database</span></span>

<span data-ttu-id="10637-179">各アプリケーションの id システムは、同じユーザー データベースを指すことを確認します。</span><span class="sxs-lookup"><span data-stu-id="10637-179">Confirm that the identity system for each app is pointed at the same user database.</span></span> <span data-ttu-id="10637-180">それ以外の場合、id システムでは、そのデータベース内の情報に対する認証クッキーの情報と一致しようとしたときに実行時にエラーが生成されます。</span><span class="sxs-lookup"><span data-stu-id="10637-180">Otherwise, the identity system produces failures at runtime when it attempts to match the information in the authentication cookie against the information in its database.</span></span>
