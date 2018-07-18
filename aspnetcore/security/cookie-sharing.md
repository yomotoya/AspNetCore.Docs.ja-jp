---
title: ASP.NET と ASP.NET Core でのアプリ間での cookie を共有します。
author: rick-anderson
description: ASP.NET 認証 cookie を共有する方法について説明します 4.x および ASP.NET Core アプリ。
ms.author: riande
ms.custom: mvc
ms.date: 01/19/2017
uid: security/cookie-sharing
ms.openlocfilehash: f8347b52f68165cdbe4ab77a76664e4767bc4cdf
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095478"
---
# <a name="share-cookies-among-apps-with-aspnet-and-aspnet-core"></a>ASP.NET と ASP.NET Core でのアプリ間での cookie を共有します。

によって[Rick Anderson](https://twitter.com/RickAndMSFT)と[Luke Latham](https://github.com/guardrex)

Web サイトは多くの場合、連携して、個々 の web アプリで構成されます。 シングル サインオン (SSO) エクスペリエンスを提供するには、サイト内で web アプリは、認証 cookie を共有する必要があります。 このシナリオをサポートするためには、データ保護スタックは、Katana cookie 認証と ASP.NET Core の cookie 認証チケットの共有を許可します。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

このサンプルは、cookie の cookie 認証を使用する 3 つのアプリ間で共有を示しています。

* 使用せずに ASP.NET Core 2.0 の Razor ページ アプリ[ASP.NET Core Identity](xref:security/authentication/identity)
* ASP.NET Core Identity を使用して ASP.NET Core 2.0 MVC アプリ
* ASP.NET Identity による ASP.NET Framework 4.6.1 の MVC アプリ

次の例。

* 認証の cookie 名は、共通の値に設定されて`.AspNet.SharedCookie`します。
* `AuthenticationType`に設定されている`Identity.Application`明示的にまたは既定のいずれか。
* 一般的なアプリ名を使用して、データ保護キーを共有するデータ保護システムを有効に (`SharedCookieApp`)。
* `Identity.Application` 認証方式として使用されます。 どのようなスキームが使用される、一貫して使用する必要があります*内および間*cookie の共有アプリいずれかまたは明示的に設定することで、既定のスキームとして。 スキームは、アプリ間で一貫性のあるスキームを使用する必要がありますので、cookie を暗号化および暗号化のときに使用されます。
* 共通[データ保護キー](xref:security/data-protection/implementation/key-management)記憶域の場所を使用します。 サンプル アプリで使用するという名前のフォルダー*キーリング*データ保護キーを保持するために、ソリューションのルートにあります。
* ASP.NET Core アプリで[PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem)キー記憶域の場所を設定するために使用します。 [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname)一般的なアプリの共有名を構成するために使用します。
* .NET Framework アプリで、cookie 認証ミドルウェアの実装を使用して[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider)します。 `DataProtectionProvider` 暗号化と認証 cookie のペイロード データの復号化用のデータ保護サービスを提供します。 `DataProtectionProvider`インスタンスは、アプリの他の部分で使用されるデータ保護システムから分離されます。
  * [DataProtectionProvider.Create(System.IO.DirectoryInfo, Action\<IDataProtectionBuilder>)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_System_Action_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder__) accepts a [DirectoryInfo](/dotnet/api/system.io.directoryinfo) to specify the location for data protection key storage. サンプル アプリのパスを提供する、*キーリング*フォルダー`DirectoryInfo`します。 [DataProtectionBuilderExtensions.SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionBuilderExtensions_SetApplicationName_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_)共通のアプリ名を設定します。
  * [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider)が必要です、 [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet パッケージ。 ASP.NET Core 2.1 以降のアプリをこのパッケージを取得するには参照、 [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)します。 .NET Framework を対象とするときにパッケージ参照を追加`Microsoft.AspNetCore.DataProtection.Extensions`します。

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a>ASP.NET Core アプリ間での認証 cookie を共有します。

ASP.NET Core Identity を使用する: 場合

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

`ConfigureServices`メソッドを使用して、 [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) cookie のデータ保護サービスを設定する拡張メソッド。

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.Core/Startup.cs?name=snippet1)]

データ保護キーと、アプリ名は、アプリ間で共有する必要があります。 サンプル アプリで`GetKeyRingDirInfo`に共通のキー記憶域の場所を返します、 [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem)メソッド。 使用[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname)一般的なアプリの共有名を構成する (`SharedCookieApp`サンプル)。 詳細については、次を参照してください。[データ保護を構成する](xref:security/data-protection/configuration/overview)します。

参照してください、 *CookieAuthWithIdentity.Core*プロジェクト、[サンプル コード](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/)([をダウンロードする方法](xref:tutorials/index#how-to-download-a-sample))。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

`Configure`メソッドを使用して、 [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions)を設定します。

* Cookie のデータ保護サービス。
* `AuthenticationScheme`に合わせて ASP.NET 4.x です。

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

ときに、cookie を直接使用するには。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](cookie-sharing/sample/CookieAuth.Core/Startup.cs?name=snippet1)]

データ保護キーと、アプリ名は、アプリ間で共有する必要があります。 サンプル アプリで`GetKeyRingDirInfo`に共通のキー記憶域の場所を返します、 [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem)メソッド。 使用[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname)一般的なアプリの共有名を構成する (`SharedCookieApp`サンプル)。 詳細については、次を参照してください。[データ保護を構成する](xref:security/data-protection/configuration/overview)します。 

参照してください、 *CookieAuth.Core*プロジェクト、[サンプル コード](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/)([をダウンロードする方法](xref:tutorials/index#how-to-download-a-sample))。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = 
        DataProtectionProvider.Create(
            new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"))
});
```

---

## <a name="encrypting-data-protection-keys-at-rest"></a>保存時のデータ保護キーの暗号化

運用環境のデプロイ、構成、 `DataProtectionProvider` DPAPI または X509Certificate と保存時のキーを暗号化します。 参照してください[キーの暗号化に](xref:security/data-protection/implementation/key-encryption-at-rest)詳細についてはします。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("thumbprint");
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

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

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a>ASP.NET との間の認証 cookie の共有 4.x および ASP.NET Core アプリ

ASP.NET Core の cookie 認証ミドルウェアと互換性のある認証 cookie を生成する Katana cookie 認証ミドルウェアを使用して、ASP.NET 4.x アプリを構成できます。 これにより、サイト全体で滑らかな SSO エクスペリエンスを提供しながら、大規模なサイトの個々 のアプリを段階的な部分アップグレードできます。

アプリは、Katana cookie 認証ミドルウェアを使用しているときに呼び出す`UseCookieAuthentication`プロジェクトの*Startup.Auth.cs*ファイル。 ASP.NET 4.x web アプリ プロジェクトでは、Visual Studio 2013 で作成され、後で、既定では、Katana の cookie 認証ミドルウェアを使用しています。 `UseCookieAuthentication`は古い形式の ASP.NET Core アプリを呼び出すことのサポートされていない`UseCookieAuthentication`Katana を使用する ASP.NET 4.x アプリの cookie 認証ミドルウェアが無効です。

ASP.NET 4.x アプリが .NET Framework 4.5.1 をターゲットする必要がありますまたはそれ以降。 それ以外の場合、必要な NuGet パッケージは、インストールに失敗します。

ASP.NET 4.x アプリと ASP.NET Core アプリの間の認証クッキーを共有するには、前述のように、ASP.NET Core アプリを構成し、次の手順に従って、ASP.NET 4.x アプリを構成します。

1. パッケージをインストール[Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/)各 ASP.NET 4.x アプリにします。

2. *Startup.Auth.cs*への呼び出しを見つけます`UseCookieAuthentication`し、次のように変更します。 ASP.NET Core の cookie 認証ミドルウェアで使用される名前と一致する cookie の名前を変更します。 インスタンスを指定する`DataProtectionProvider`一般的なデータ保護キー記憶域の場所を初期化します。 アプリ名が、cookie を共有するすべてのアプリで使用される共通のアプリ名に設定されていることを確認`SharedCookieApp`サンプル アプリでします。

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/App_Start/Startup.Auth.cs?name=snippet1)]

参照してください、 *CookieAuthWithIdentity.NETFramework*プロジェクト、[サンプル コード](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/)([をダウンロードする方法](xref:tutorials/index#how-to-download-a-sample))。

ユーザー id を生成するときに、認証の種類がで定義された型を一致する必要があります`AuthenticationType`セット`UseCookieAuthentication`します。

*Models/IdentityModels.cs*:

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/Models/IdentityModels.cs?name=snippet1)]

## <a name="use-a-common-user-database"></a>一般的なユーザー データベースを使用します。

アプリごとの id システムが同じユーザー データベースで参照されていることを確認します。 それ以外の場合、id システムでは、そのデータベース内の情報に対して認証クッキーの情報と一致しようとしたときに実行時にエラーが生成されます。

## <a name="additional-resources"></a>その他の技術情報

<xref:host-and-deploy/web-farm>
