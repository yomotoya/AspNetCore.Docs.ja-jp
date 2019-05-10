---
title: ASP.NET と ASP.NET Core でのアプリ間での cookie を共有します。
author: rick-anderson
description: ASP.NET 認証 cookie を共有する方法について説明します 4.x および ASP.NET Core アプリ。
ms.author: riande
ms.custom: mvc
ms.date: 04/06/2019
uid: security/cookie-sharing
ms.openlocfilehash: 94fafc91012b5a7e0888a6ebf37f517c129af2ac
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2019
ms.locfileid: "64892799"
---
# <a name="share-cookies-among-apps-with-aspnet-and-aspnet-core"></a><span data-ttu-id="798e9-103">ASP.NET と ASP.NET Core でのアプリ間での cookie を共有します。</span><span class="sxs-lookup"><span data-stu-id="798e9-103">Share cookies among apps with ASP.NET and ASP.NET Core</span></span>

<span data-ttu-id="798e9-104">によって[Rick Anderson](https://twitter.com/RickAndMSFT)と[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="798e9-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="798e9-105">Web サイトは多くの場合、連携して、個々 の web アプリで構成されます。</span><span class="sxs-lookup"><span data-stu-id="798e9-105">Websites often consist of individual web apps working together.</span></span> <span data-ttu-id="798e9-106">シングル サインオン (SSO) エクスペリエンスを提供するには、サイト内で web アプリは、認証 cookie を共有する必要があります。</span><span class="sxs-lookup"><span data-stu-id="798e9-106">To provide a single sign-on (SSO) experience, web apps within a site must share authentication cookies.</span></span> <span data-ttu-id="798e9-107">このシナリオをサポートするためには、データ保護スタックは、Katana cookie 認証と ASP.NET Core の cookie 認証チケットの共有を許可します。</span><span class="sxs-lookup"><span data-stu-id="798e9-107">To support this scenario, the data protection stack allows sharing Katana cookie authentication and ASP.NET Core cookie authentication tickets.</span></span>

<span data-ttu-id="798e9-108">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cookie-sharing/sample/)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="798e9-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="798e9-109">このサンプルは、cookie の cookie 認証を使用する 3 つのアプリ間で共有を示しています。</span><span class="sxs-lookup"><span data-stu-id="798e9-109">The sample illustrates cookie sharing across three apps that use cookie authentication:</span></span>

* <span data-ttu-id="798e9-110">使用せずに ASP.NET Core 2.0 の Razor ページ アプリ[ASP.NET Core Identity](xref:security/authentication/identity)</span><span class="sxs-lookup"><span data-stu-id="798e9-110">ASP.NET Core 2.0 Razor Pages app without using [ASP.NET Core Identity](xref:security/authentication/identity)</span></span>
* <span data-ttu-id="798e9-111">ASP.NET Core Identity を使用して ASP.NET Core 2.0 MVC アプリ</span><span class="sxs-lookup"><span data-stu-id="798e9-111">ASP.NET Core 2.0 MVC app with ASP.NET Core Identity</span></span>
* <span data-ttu-id="798e9-112">ASP.NET Identity による ASP.NET Framework 4.6.1 の MVC アプリ</span><span class="sxs-lookup"><span data-stu-id="798e9-112">ASP.NET Framework 4.6.1 MVC app with ASP.NET Identity</span></span>

<span data-ttu-id="798e9-113">次の例。</span><span class="sxs-lookup"><span data-stu-id="798e9-113">In the examples that follow:</span></span>

* <span data-ttu-id="798e9-114">認証の cookie 名は、共通の値に設定されて`.AspNet.SharedCookie`します。</span><span class="sxs-lookup"><span data-stu-id="798e9-114">The authentication cookie name is set to a common value of `.AspNet.SharedCookie`.</span></span>
* <span data-ttu-id="798e9-115">`AuthenticationType`に設定されている`Identity.Application`明示的にまたは既定のいずれか。</span><span class="sxs-lookup"><span data-stu-id="798e9-115">The `AuthenticationType` is set to `Identity.Application` either explicitly or by default.</span></span>
* <span data-ttu-id="798e9-116">一般的なアプリ名を使用して、データ保護キーを共有するデータ保護システムを有効に (`SharedCookieApp`)。</span><span class="sxs-lookup"><span data-stu-id="798e9-116">A common app name is used to enable the data protection system to share data protection keys (`SharedCookieApp`).</span></span>
* <span data-ttu-id="798e9-117">`Identity.Application` 認証方式として使用されます。</span><span class="sxs-lookup"><span data-stu-id="798e9-117">`Identity.Application` is used as the authentication scheme.</span></span> <span data-ttu-id="798e9-118">どのようなスキームが使用される、一貫して使用する必要があります*内および間*cookie の共有アプリいずれかまたは明示的に設定することで、既定のスキームとして。</span><span class="sxs-lookup"><span data-stu-id="798e9-118">Whatever scheme is used, it must be used consistently *within and across* the shared cookie apps either as the default scheme or by explicitly setting it.</span></span> <span data-ttu-id="798e9-119">スキームは、アプリ間で一貫性のあるスキームを使用する必要がありますので、cookie を暗号化および暗号化のときに使用されます。</span><span class="sxs-lookup"><span data-stu-id="798e9-119">The scheme is used when encrypting and decrypting cookies, so a consistent scheme must be used across apps.</span></span>
* <span data-ttu-id="798e9-120">共通[データ保護キー](xref:security/data-protection/implementation/key-management)記憶域の場所を使用します。</span><span class="sxs-lookup"><span data-stu-id="798e9-120">A common [data protection key](xref:security/data-protection/implementation/key-management) storage location is used.</span></span> <span data-ttu-id="798e9-121">サンプル アプリで使用するという名前のフォルダー*キーリング*データ保護キーを保持するために、ソリューションのルートにあります。</span><span class="sxs-lookup"><span data-stu-id="798e9-121">The sample app uses a folder named *KeyRing* at the root of the solution to hold the data protection keys.</span></span>
* <span data-ttu-id="798e9-122">ASP.NET Core アプリで[PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem)キー記憶域の場所を設定するために使用します。</span><span class="sxs-lookup"><span data-stu-id="798e9-122">In the ASP.NET Core apps, [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) is used to set the key storage location.</span></span> <span data-ttu-id="798e9-123">[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname)一般的なアプリの共有名を構成するために使用します。</span><span class="sxs-lookup"><span data-stu-id="798e9-123">[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) is used to configure a common shared app name.</span></span>
* <span data-ttu-id="798e9-124">.NET Framework アプリで、cookie 認証ミドルウェアの実装を使用して[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider)します。</span><span class="sxs-lookup"><span data-stu-id="798e9-124">In the .NET Framework app, the cookie authentication middleware uses an implementation of [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider).</span></span> <span data-ttu-id="798e9-125">`DataProtectionProvider` 暗号化と認証 cookie のペイロード データの復号化用のデータ保護サービスを提供します。</span><span class="sxs-lookup"><span data-stu-id="798e9-125">`DataProtectionProvider` provides data protection services for the encryption and decryption of authentication cookie payload data.</span></span> <span data-ttu-id="798e9-126">`DataProtectionProvider`インスタンスは、アプリの他の部分で使用されるデータ保護システムから分離されます。</span><span class="sxs-lookup"><span data-stu-id="798e9-126">The `DataProtectionProvider` instance is isolated from the data protection system used by other parts of the app.</span></span>
  * <span data-ttu-id="798e9-127">[DataProtectionProvider.Create(System.IO.DirectoryInfo, Action\<IDataProtectionBuilder>)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_System_Action_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder__) accepts a [DirectoryInfo](/dotnet/api/system.io.directoryinfo) to specify the location for data protection key storage.</span><span class="sxs-lookup"><span data-stu-id="798e9-127">[DataProtectionProvider.Create(System.IO.DirectoryInfo, Action\<IDataProtectionBuilder>)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_System_Action_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder__) accepts a [DirectoryInfo](/dotnet/api/system.io.directoryinfo) to specify the location for data protection key storage.</span></span> <span data-ttu-id="798e9-128">サンプル アプリのパスを提供する、*キーリング*フォルダー`DirectoryInfo`します。</span><span class="sxs-lookup"><span data-stu-id="798e9-128">The sample app provides the path of the *KeyRing* folder to `DirectoryInfo`.</span></span> <span data-ttu-id="798e9-129">[DataProtectionBuilderExtensions.SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionBuilderExtensions_SetApplicationName_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_)共通のアプリ名を設定します。</span><span class="sxs-lookup"><span data-stu-id="798e9-129">[DataProtectionBuilderExtensions.SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionBuilderExtensions_SetApplicationName_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_) sets the common app name.</span></span>
  * <span data-ttu-id="798e9-130">[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider)が必要です、 [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet パッケージ。</span><span class="sxs-lookup"><span data-stu-id="798e9-130">[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) requires the [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet package.</span></span> <span data-ttu-id="798e9-131">ASP.NET Core 2.1 以降のアプリをこのパッケージを取得するには参照、 [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)します。</span><span class="sxs-lookup"><span data-stu-id="798e9-131">To obtain this package for ASP.NET Core 2.1 and later apps, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="798e9-132">.NET Framework を対象とするときにパッケージ参照を追加`Microsoft.AspNetCore.DataProtection.Extensions`します。</span><span class="sxs-lookup"><span data-stu-id="798e9-132">When targeting the .NET Framework, add a package reference to `Microsoft.AspNetCore.DataProtection.Extensions`.</span></span>

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a><span data-ttu-id="798e9-133">ASP.NET Core アプリ間での認証 cookie を共有します。</span><span class="sxs-lookup"><span data-stu-id="798e9-133">Share authentication cookies among ASP.NET Core apps</span></span>

<span data-ttu-id="798e9-134">ASP.NET Core Identity を使用する: 場合</span><span class="sxs-lookup"><span data-stu-id="798e9-134">When using ASP.NET Core Identity:</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="798e9-135">`ConfigureServices`メソッドを使用して、 [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) cookie のデータ保護サービスを設定する拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="798e9-135">In the `ConfigureServices` method, use the [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) extension method to set up the data protection service for cookies.</span></span>

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.Core/Startup.cs?name=snippet1)]

<span data-ttu-id="798e9-136">データ保護キーと、アプリ名は、アプリ間で共有する必要があります。</span><span class="sxs-lookup"><span data-stu-id="798e9-136">Data protection keys and the app name must be shared among apps.</span></span> <span data-ttu-id="798e9-137">サンプル アプリで`GetKeyRingDirInfo`に共通のキー記憶域の場所を返します、 [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem)メソッド。</span><span class="sxs-lookup"><span data-stu-id="798e9-137">In the sample apps, `GetKeyRingDirInfo` returns the common key storage location to the [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) method.</span></span> <span data-ttu-id="798e9-138">使用[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname)一般的なアプリの共有名を構成する (`SharedCookieApp`サンプル)。</span><span class="sxs-lookup"><span data-stu-id="798e9-138">Use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) to configure a common shared app name (`SharedCookieApp` in the sample).</span></span> <span data-ttu-id="798e9-139">詳細については、次を参照してください。[データ保護を構成する](xref:security/data-protection/configuration/overview)します。</span><span class="sxs-lookup"><span data-stu-id="798e9-139">For more information, see [Configuring Data Protection](xref:security/data-protection/configuration/overview).</span></span>

<span data-ttu-id="798e9-140">サブドメインで cookie を共有するアプリをホストする場合での一般的なドメインを指定してください。、 [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain)プロパティ。</span><span class="sxs-lookup"><span data-stu-id="798e9-140">When hosting apps that share cookies across subdomains, specify a common domain in the [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain) property.</span></span> <span data-ttu-id="798e9-141">アプリ間で cookie を共有する`contoso.com`など`first_subdomain.contoso.com`と`second_subdomain.contoso.com`、指定、`Cookie.Domain`として`.contoso.com`:</span><span class="sxs-lookup"><span data-stu-id="798e9-141">To share cookies across apps at `contoso.com`, such as `first_subdomain.contoso.com` and `second_subdomain.contoso.com`, specify the `Cookie.Domain` as `.contoso.com`:</span></span>

```csharp
options.Cookie.Domain = ".contoso.com";
```

<span data-ttu-id="798e9-142">参照してください、 *CookieAuthWithIdentity.Core*プロジェクト、[サンプル コード](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cookie-sharing/sample/)([をダウンロードする方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="798e9-142">See the *CookieAuthWithIdentity.Core* project in the [sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="798e9-143">`Configure`メソッドを使用して、 [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions)を設定します。</span><span class="sxs-lookup"><span data-stu-id="798e9-143">In the `Configure` method, use the [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) to set up:</span></span>

* <span data-ttu-id="798e9-144">Cookie のデータ保護サービス。</span><span class="sxs-lookup"><span data-stu-id="798e9-144">The data protection service for cookies.</span></span>
* <span data-ttu-id="798e9-145">`AuthenticationScheme`に合わせて ASP.NET 4.x です。</span><span class="sxs-lookup"><span data-stu-id="798e9-145">The `AuthenticationScheme` to match ASP.NET 4.x.</span></span>

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

::: moniker-end

<span data-ttu-id="798e9-146">ときに、cookie を直接使用するには。</span><span class="sxs-lookup"><span data-stu-id="798e9-146">When using cookies directly:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](cookie-sharing/sample/CookieAuth.Core/Startup.cs?name=snippet1)]

<span data-ttu-id="798e9-147">データ保護キーと、アプリ名は、アプリ間で共有する必要があります。</span><span class="sxs-lookup"><span data-stu-id="798e9-147">Data protection keys and the app name must be shared among apps.</span></span> <span data-ttu-id="798e9-148">サンプル アプリで`GetKeyRingDirInfo`に共通のキー記憶域の場所を返します、 [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem)メソッド。</span><span class="sxs-lookup"><span data-stu-id="798e9-148">In the sample apps, `GetKeyRingDirInfo` returns the common key storage location to the [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) method.</span></span> <span data-ttu-id="798e9-149">使用[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname)一般的なアプリの共有名を構成する (`SharedCookieApp`サンプル)。</span><span class="sxs-lookup"><span data-stu-id="798e9-149">Use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) to configure a common shared app name (`SharedCookieApp` in the sample).</span></span> <span data-ttu-id="798e9-150">詳細については、次を参照してください。[データ保護を構成する](xref:security/data-protection/configuration/overview)します。</span><span class="sxs-lookup"><span data-stu-id="798e9-150">For more information, see [Configuring Data Protection](xref:security/data-protection/configuration/overview).</span></span>

<span data-ttu-id="798e9-151">サブドメインで cookie を共有するアプリをホストする場合での一般的なドメインを指定してください。、 [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain)プロパティ。</span><span class="sxs-lookup"><span data-stu-id="798e9-151">When hosting apps that share cookies across subdomains, specify a common domain in the [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain) property.</span></span> <span data-ttu-id="798e9-152">アプリ間で cookie を共有する`contoso.com`など`first_subdomain.contoso.com`と`second_subdomain.contoso.com`、指定、`Cookie.Domain`として`.contoso.com`:</span><span class="sxs-lookup"><span data-stu-id="798e9-152">To share cookies across apps at `contoso.com`, such as `first_subdomain.contoso.com` and `second_subdomain.contoso.com`, specify the `Cookie.Domain` as `.contoso.com`:</span></span>

```csharp
options.Cookie.Domain = ".contoso.com";
```

<span data-ttu-id="798e9-153">参照してください、 *CookieAuth.Core*プロジェクト、[サンプル コード](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cookie-sharing/sample/)([をダウンロードする方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="798e9-153">See the *CookieAuth.Core* project in the [sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = 
        DataProtectionProvider.Create(
            new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"))
});
```

::: moniker-end

## <a name="encrypting-data-protection-keys-at-rest"></a><span data-ttu-id="798e9-154">保存時のデータ保護キーの暗号化</span><span class="sxs-lookup"><span data-stu-id="798e9-154">Encrypting data protection keys at rest</span></span>

<span data-ttu-id="798e9-155">運用環境のデプロイ、構成、 `DataProtectionProvider` DPAPI または X509Certificate と保存時のキーを暗号化します。</span><span class="sxs-lookup"><span data-stu-id="798e9-155">For production deployments, configure the `DataProtectionProvider` to encrypt keys at rest with DPAPI or an X509Certificate.</span></span> <span data-ttu-id="798e9-156">参照してください[キーの暗号化に](xref:security/data-protection/implementation/key-encryption-at-rest)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="798e9-156">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more information.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("thumbprint");
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

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

::: moniker-end

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a><span data-ttu-id="798e9-157">ASP.NET との間の認証 cookie の共有 4.x および ASP.NET Core アプリ</span><span class="sxs-lookup"><span data-stu-id="798e9-157">Sharing authentication cookies between ASP.NET 4.x and ASP.NET Core apps</span></span>

<span data-ttu-id="798e9-158">ASP.NET Core の cookie 認証ミドルウェアと互換性のある認証 cookie を生成する Katana cookie 認証ミドルウェアを使用して、ASP.NET 4.x アプリを構成できます。</span><span class="sxs-lookup"><span data-stu-id="798e9-158">ASP.NET 4.x apps which use Katana cookie authentication middleware can be configured to generate authentication cookies that are compatible with the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="798e9-159">これにより、サイト全体で滑らかな SSO エクスペリエンスを提供しながら、大規模なサイトの個々 のアプリを段階的な部分アップグレードできます。</span><span class="sxs-lookup"><span data-stu-id="798e9-159">This allows upgrading a large site's individual apps piecemeal while providing a smooth SSO experience across the site.</span></span>

<span data-ttu-id="798e9-160">アプリは、Katana cookie 認証ミドルウェアを使用しているときに呼び出す`UseCookieAuthentication`プロジェクトの*Startup.Auth.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="798e9-160">When an app uses Katana cookie authentication middleware, it calls `UseCookieAuthentication` in the project's *Startup.Auth.cs* file.</span></span> <span data-ttu-id="798e9-161">ASP.NET 4.x web アプリ プロジェクトでは、Visual Studio 2013 で作成され、後で、既定では、Katana の cookie 認証ミドルウェアを使用しています。</span><span class="sxs-lookup"><span data-stu-id="798e9-161">ASP.NET 4.x web app projects created with Visual Studio 2013 and later use the Katana cookie authentication middleware by default.</span></span> <span data-ttu-id="798e9-162">`UseCookieAuthentication`は古い形式の ASP.NET Core アプリを呼び出すことのサポートされていない`UseCookieAuthentication`Katana を使用する ASP.NET 4.x アプリの cookie 認証ミドルウェアが無効です。</span><span class="sxs-lookup"><span data-stu-id="798e9-162">Although `UseCookieAuthentication` is obsolete and unsupported for ASP.NET Core apps, calling `UseCookieAuthentication` in an ASP.NET 4.x app that uses Katana cookie authentication middleware is valid.</span></span>

<span data-ttu-id="798e9-163">ASP.NET 4.x アプリが .NET Framework 4.5.1 をターゲットする必要がありますまたはそれ以降。</span><span class="sxs-lookup"><span data-stu-id="798e9-163">An ASP.NET 4.x app must target .NET Framework 4.5.1 or later.</span></span> <span data-ttu-id="798e9-164">それ以外の場合、必要な NuGet パッケージは、インストールに失敗します。</span><span class="sxs-lookup"><span data-stu-id="798e9-164">Otherwise, the necessary NuGet packages fail to install.</span></span>

<span data-ttu-id="798e9-165">ASP.NET 4.x アプリと ASP.NET Core アプリの間の認証クッキーを共有するには、前述のように、ASP.NET Core アプリを構成し、次の手順に従って、ASP.NET 4.x アプリを構成します。</span><span class="sxs-lookup"><span data-stu-id="798e9-165">To share authentication cookies between an ASP.NET 4.x app and an ASP.NET Core app, configure the ASP.NET Core app as stated above, then configure the ASP.NET 4.x app by following these steps:</span></span>

1. <span data-ttu-id="798e9-166">パッケージをインストール[Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/)各 ASP.NET 4.x アプリにします。</span><span class="sxs-lookup"><span data-stu-id="798e9-166">Install the package [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) into each ASP.NET 4.x app.</span></span>

2. <span data-ttu-id="798e9-167">*Startup.Auth.cs*への呼び出しを見つけます`UseCookieAuthentication`し、次のように変更します。</span><span class="sxs-lookup"><span data-stu-id="798e9-167">In *Startup.Auth.cs*, locate the call to `UseCookieAuthentication` and modify it as follows.</span></span> <span data-ttu-id="798e9-168">ASP.NET Core の cookie 認証ミドルウェアで使用される名前と一致する cookie の名前を変更します。</span><span class="sxs-lookup"><span data-stu-id="798e9-168">Change the cookie name to match the name used by the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="798e9-169">インスタンスを指定する`DataProtectionProvider`一般的なデータ保護キー記憶域の場所を初期化します。</span><span class="sxs-lookup"><span data-stu-id="798e9-169">Provide an instance of a `DataProtectionProvider` initialized to the common data protection key storage location.</span></span> <span data-ttu-id="798e9-170">アプリ名が、cookie を共有するすべてのアプリで使用される共通のアプリ名に設定されていることを確認`SharedCookieApp`サンプル アプリでします。</span><span class="sxs-lookup"><span data-stu-id="798e9-170">Make sure that the app name is set to the common app name used by all apps that share cookies, `SharedCookieApp` in the sample app.</span></span>

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/App_Start/Startup.Auth.cs?name=snippet1)]

<span data-ttu-id="798e9-171">参照してください、 *CookieAuthWithIdentity.NETFramework*プロジェクト、[サンプル コード](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cookie-sharing/sample/)([をダウンロードする方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="798e9-171">See the *CookieAuthWithIdentity.NETFramework* project in the [sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="798e9-172">ユーザー id を生成するときに、認証の種類がで定義された型を一致する必要があります`AuthenticationType`セット`UseCookieAuthentication`します。</span><span class="sxs-lookup"><span data-stu-id="798e9-172">When generating a user identity, the authentication type must match the type defined in `AuthenticationType` set with `UseCookieAuthentication`.</span></span>

<span data-ttu-id="798e9-173">*Models/IdentityModels.cs*:</span><span class="sxs-lookup"><span data-stu-id="798e9-173">*Models/IdentityModels.cs*:</span></span>

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/Models/IdentityModels.cs?name=snippet1)]

## <a name="use-a-common-user-database"></a><span data-ttu-id="798e9-174">一般的なユーザー データベースを使用します。</span><span class="sxs-lookup"><span data-stu-id="798e9-174">Use a common user database</span></span>

<span data-ttu-id="798e9-175">アプリごとの id システムが同じユーザー データベースで参照されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="798e9-175">Confirm that the identity system for each app is pointed at the same user database.</span></span> <span data-ttu-id="798e9-176">それ以外の場合、id システムでは、そのデータベース内の情報に対して認証クッキーの情報と一致しようとしたときに実行時にエラーが生成されます。</span><span class="sxs-lookup"><span data-stu-id="798e9-176">Otherwise, the identity system produces failures at runtime when it attempts to match the information in the authentication cookie against the information in its database.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="798e9-177">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="798e9-177">Additional resources</span></span>

<xref:host-and-deploy/web-farm>
