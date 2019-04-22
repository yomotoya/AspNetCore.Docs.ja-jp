---
title: ASP.NET Core データ保護を構成します。
author: rick-anderson
description: ASP.NET Core でのデータ保護を構成する方法について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 04/11/2019
uid: security/data-protection/configuration/overview
ms.openlocfilehash: ee43427fa1e82a365d49df50567b4ca7afb5a5d3
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/17/2019
ms.locfileid: "59516249"
---
# <a name="configure-aspnet-core-data-protection"></a><span data-ttu-id="dd7b1-103">ASP.NET Core データ保護を構成します。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-103">Configure ASP.NET Core Data Protection</span></span>

<span data-ttu-id="dd7b1-104">データ保護システムが初期化されると、適用[既定の設定](xref:security/data-protection/configuration/default-settings)運用環境に基づきます。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-104">When the Data Protection system is initialized, it applies [default settings](xref:security/data-protection/configuration/default-settings) based on the operational environment.</span></span> <span data-ttu-id="dd7b1-105">これらの設定は、1 台のコンピューターで実行されているアプリの一般的に適しています。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-105">These settings are generally appropriate for apps running on a single machine.</span></span> <span data-ttu-id="dd7b1-106">開発者は、既定の設定を変更する必要のあるケースがあります。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-106">There are cases where a developer may want to change the default settings:</span></span>

* <span data-ttu-id="dd7b1-107">複数のコンピューターで、アプリの負荷が分散されます。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-107">The app is spread across multiple machines.</span></span>
* <span data-ttu-id="dd7b1-108">コンプライアンス上の理由からです。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-108">For compliance reasons.</span></span>

<span data-ttu-id="dd7b1-109">これらのシナリオについては、データ保護システムは、高度な構成 API を提供します。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-109">For these scenarios, the Data Protection system offers a rich configuration API.</span></span>

> [!WARNING]
> <span data-ttu-id="dd7b1-110">構成ファイルと同様に、データ保護キー リングする必要があります保護する適切なアクセス許可を使用します。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-110">Similar to configuration files, the data protection key ring should be protected using appropriate permissions.</span></span> <span data-ttu-id="dd7b1-111">、保存時のキーを暗号化することもできますが、攻撃者を防ぐ新しいキーを作成するがこれです。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-111">You can choose to encrypt keys at rest, but this doesn't prevent attackers from creating new keys.</span></span> <span data-ttu-id="dd7b1-112">その結果、アプリのセキュリティに影響します。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-112">Consequently, your app's security is impacted.</span></span> <span data-ttu-id="dd7b1-113">データ保護で構成されている記憶域の場所のアクセスは、アプリ自体と同じように構成ファイルを保護するように制限があります。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-113">The storage location configured with Data Protection should have its access limited to the app itself, similar to the way you would protect configuration files.</span></span> <span data-ttu-id="dd7b1-114">たとえば、ディスク上のキー リングを格納する場合は、ファイル システム権限を使用します。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-114">For example, if you choose to store your key ring on disk, use file system permissions.</span></span> <span data-ttu-id="dd7b1-115">Id のみを確認します。 web アプリの実行に読み取り、書き込み、およびそのディレクトリへのアクセスを作成します。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-115">Ensure only the identity under which your web app runs has read, write, and create access to that directory.</span></span> <span data-ttu-id="dd7b1-116">Azure Table Storage を使用する場合、web アプリのみが読み取り、書き込み、またはテーブル ストアなどの新しいエントリを作成する機能に必要です。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-116">If you use Azure Table Storage, only the web app should have the ability to read, write, or create new entries in the table store, etc.</span></span>
>
> <span data-ttu-id="dd7b1-117">拡張メソッド[AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection)を返します、 [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder)します。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-117">The extension method [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) returns an [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder).</span></span> <span data-ttu-id="dd7b1-118">`IDataProtectionBuilder` 連結できますデータ保護を構成するオプションの拡張メソッドを公開します。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-118">`IDataProtectionBuilder` exposes extension methods that you can chain together to configure Data Protection options.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="protectkeyswithazurekeyvault"></a><span data-ttu-id="dd7b1-119">ProtectKeysWithAzureKeyVault</span><span class="sxs-lookup"><span data-stu-id="dd7b1-119">ProtectKeysWithAzureKeyVault</span></span>

<span data-ttu-id="dd7b1-120">キーを保管する[Azure Key Vault](https://azure.microsoft.com/services/key-vault/)、構成を使用してシステム[ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault)で、`Startup`クラス。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-120">To store keys in [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), configure the system with [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) in the `Startup` class:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blobUriWithSasToken>"))
        .ProtectKeysWithAzureKeyVault("<keyIdentifier>", "<clientId>", "<clientSecret>");
}
```

<span data-ttu-id="dd7b1-121">設定キー リング記憶域の場所 (たとえば、 [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage))。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-121">Set the key ring storage location (for example, [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage)).</span></span> <span data-ttu-id="dd7b1-122">呼び出すために、場所を設定する必要があります`ProtectKeysWithAzureKeyVault`実装、 [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor)キー リング記憶域の場所を含む、データの自動保護設定を無効にします。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-122">The location must be set because calling `ProtectKeysWithAzureKeyVault` implements an [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor) that disables automatic data protection settings, including the key ring storage location.</span></span> <span data-ttu-id="dd7b1-123">前の例では、Azure Blob Storage を使用して、キー リングを保持します。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-123">The preceding example uses Azure Blob Storage to persist the key ring.</span></span> <span data-ttu-id="dd7b1-124">詳細については、次を参照してください。[キー記憶域プロバイダー。Azure と Redis](xref:security/data-protection/implementation/key-storage-providers#azure-and-redis)します。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-124">For more information, see [Key storage providers: Azure and Redis](xref:security/data-protection/implementation/key-storage-providers#azure-and-redis).</span></span> <span data-ttu-id="dd7b1-125">ローカルにキー リングを保存することもできます。 [PersistKeysToFileSystem](xref:security/data-protection/implementation/key-storage-providers#file-system)します。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-125">You can also persist the key ring locally with [PersistKeysToFileSystem](xref:security/data-protection/implementation/key-storage-providers#file-system).</span></span>

<span data-ttu-id="dd7b1-126">`keyIdentifier`はキーの暗号化に使用されるキー コンテナーのキー識別子です。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-126">The `keyIdentifier` is the key vault key identifier used for key encryption.</span></span> <span data-ttu-id="dd7b1-127">たとえば、という名前の key vault に作成されたキー`dataprotection`で、`contosokeyvault`したキー識別子を持つ`https://contosokeyvault.vault.azure.net/keys/dataprotection/`します。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-127">For example, a key created in key vault named `dataprotection` in the `contosokeyvault` has the key identifier `https://contosokeyvault.vault.azure.net/keys/dataprotection/`.</span></span> <span data-ttu-id="dd7b1-128">使用してアプリを提供**Unwrap Key**と**Wrap Key** key vault へのアクセスを許可します。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-128">Provide the app with **Unwrap Key** and **Wrap Key** permissions to the key vault.</span></span>

<span data-ttu-id="dd7b1-129">`ProtectKeysWithAzureKeyVault` オーバー ロードします。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-129">`ProtectKeysWithAzureKeyVault` overloads:</span></span>

* <span data-ttu-id="dd7b1-130">[ProtectKeysWithAzureKeyVault (IDataProtectionBuilder、KeyVaultClient、文字列)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_Microsoft_Azure_KeyVault_KeyVaultClient_System_String_)の使用を許可、 [KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient) key vault を使用するデータ保護システムを有効にします。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-130">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, KeyVaultClient, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_Microsoft_Azure_KeyVault_KeyVaultClient_System_String_) permits the use of a [KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient) to enable the data protection system to use the key vault.</span></span>
* <span data-ttu-id="dd7b1-131">[(IDataProtectionBuilder、文字列、文字列、X509Certificate2) ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_Security_Cryptography_X509Certificates_X509Certificate2_)の使用を許可、`ClientId`と[X509Certificate](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) key vault を使用するデータ保護システムを有効にします。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-131">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, String, String, X509Certificate2)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_Security_Cryptography_X509Certificates_X509Certificate2_) permits the use of a `ClientId` and [X509Certificate](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) to enable the data protection system to use the key vault.</span></span>
* <span data-ttu-id="dd7b1-132">[(IDataProtectionBuilder、文字列、String, String) ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_String_)の使用を許可、`ClientId`と`ClientSecret`key vault を使用するデータ保護システムを有効にします。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-132">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, String, String, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_String_) permits the use of a `ClientId` and `ClientSecret` to enable the data protection system to use the key vault.</span></span>

::: moniker-end

## <a name="persistkeystofilesystem"></a><span data-ttu-id="dd7b1-133">PersistKeysToFileSystem</span><span class="sxs-lookup"><span data-stu-id="dd7b1-133">PersistKeysToFileSystem</span></span>

<span data-ttu-id="dd7b1-134">代わりに UNC 共有でのキーを格納する、 *%localappdata%* 既定の場所、構成を使用してシステム[PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span><span class="sxs-lookup"><span data-stu-id="dd7b1-134">To store keys on a UNC share instead of at the *%LOCALAPPDATA%* default location, configure the system with [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

> [!WARNING]
> <span data-ttu-id="dd7b1-135">キーの永続性の場所を変更する場合、システムは DPAPI は、適切な暗号化メカニズムであるかどうかを認識していないため、保存時のキーを暗号化する自動的にします。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-135">If you change the key persistence location, the system no longer automatically encrypts keys at rest, since it doesn't know whether DPAPI is an appropriate encryption mechanism.</span></span>

## <a name="protectkeyswith"></a><span data-ttu-id="dd7b1-136">ProtectKeysWith\*</span><span class="sxs-lookup"><span data-stu-id="dd7b1-136">ProtectKeysWith\*</span></span>

<span data-ttu-id="dd7b1-137">いずれかを呼び出すことによって、保存時のキーを保護するシステムを構成することができます、 [ProtectKeysWith\* ](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) Api を構成します。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-137">You can configure the system to protect keys at rest by calling any of the [ProtectKeysWith\*](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) configuration APIs.</span></span> <span data-ttu-id="dd7b1-138">UNC 共有にキーを格納し、特定の X.509 証明書での保存時に、そのキーを暗号化する次の例を検討してください。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-138">Consider the example below, which stores keys on a UNC share and encrypts those keys at rest with a specific X.509 certificate:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="dd7b1-139">ASP.NET Core 2.1 以降では、行うことができます、 [X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2)に[ProtectKeysWithCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithcertificate)ファイルから読み込まれた証明書など。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-139">In ASP.NET Core 2.1 or later, you can provide an [X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) to [ProtectKeysWithCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithcertificate), such as a certificate loaded from a file:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate(
            new X509Certificate2("certificate.pfx", "password"));
}
```

::: moniker-end

<span data-ttu-id="dd7b1-140">参照してください[キーの暗号化に](xref:security/data-protection/implementation/key-encryption-at-rest)詳細の例と、組み込みのキーの暗号化メカニズムについて説明します。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-140">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more examples and discussion on the built-in key encryption mechanisms.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="unprotectkeyswithanycertificate"></a><span data-ttu-id="dd7b1-141">UnprotectKeysWithAnyCertificate</span><span class="sxs-lookup"><span data-stu-id="dd7b1-141">UnprotectKeysWithAnyCertificate</span></span>

<span data-ttu-id="dd7b1-142">ASP.NET Core 2.1 以降では、証明書の回転し、配列を使用して保存時のキーを復号化[X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2)で証明書[UnprotectKeysWithAnyCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.unprotectkeyswithanycertificate):</span><span class="sxs-lookup"><span data-stu-id="dd7b1-142">In ASP.NET Core 2.1 or later, you can rotate certificates and decrypt keys at rest using an array of [X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) certificates with [UnprotectKeysWithAnyCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.unprotectkeyswithanycertificate):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate(
            new X509Certificate2("certificate.pfx", "password"));
        .UnprotectKeysWithAnyCertificate(
            new X509Certificate2("certificate_old_1.pfx", "password_1"),
            new X509Certificate2("certificate_old_2.pfx", "password_2"));
}
```

::: moniker-end

## <a name="setdefaultkeylifetime"></a><span data-ttu-id="dd7b1-143">SetDefaultKeyLifetime</span><span class="sxs-lookup"><span data-stu-id="dd7b1-143">SetDefaultKeyLifetime</span></span>

<span data-ttu-id="dd7b1-144">90 日間、既定ではなくキーの有効期間は 14 日間を使用するシステムを構成するには使用[SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span><span class="sxs-lookup"><span data-stu-id="dd7b1-144">To configure the system to use a key lifetime of 14 days instead of the default 90 days, use [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

## <a name="setapplicationname"></a><span data-ttu-id="dd7b1-145">SetApplicationName</span><span class="sxs-lookup"><span data-stu-id="dd7b1-145">SetApplicationName</span></span>

<span data-ttu-id="dd7b1-146">既定では、データ保護システムは、同じ物理キーのリポジトリを共有している場合でも、コンテンツ ルート パスに基づいて、別のアプリを分離します。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-146">By default, the Data Protection system isolates apps from one another based on their content root paths, even if they're sharing the same physical key repository.</span></span> <span data-ttu-id="dd7b1-147">これは、アプリが互いの保護されたペイロードを理解することを防ぎます。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-147">This prevents the apps from understanding each other's protected payloads.</span></span>

<span data-ttu-id="dd7b1-148">アプリ間でのペイロードの保護された共有します。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-148">To share protected payloads among apps:</span></span>

* <span data-ttu-id="dd7b1-149">構成<xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*>と同じ値には、各アプリでします。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-149">Configure <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> in each app with the same value.</span></span>
* <span data-ttu-id="dd7b1-150">アプリ間で同じバージョンのデータ保護 API スタックを使用します。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-150">Use the same version of the Data Protection API stack across the apps.</span></span> <span data-ttu-id="dd7b1-151">実行**か**アプリのプロジェクト ファイルで、次の。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-151">Perform **either** of the following in the apps' project files:</span></span>
  * <span data-ttu-id="dd7b1-152">使用して同じ共有フレームワークのバージョンを参照、 [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)します。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-152">Reference the same shared framework version via the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
  * <span data-ttu-id="dd7b1-153">同じ参照[データ保護パッケージ](xref:security/data-protection/introduction#package-layout)バージョン。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-153">Reference the same [Data Protection package](xref:security/data-protection/introduction#package-layout) version.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("shared app name");
}
```

## <a name="disableautomatickeygeneration"></a><span data-ttu-id="dd7b1-154">DisableAutomaticKeyGeneration</span><span class="sxs-lookup"><span data-stu-id="dd7b1-154">DisableAutomaticKeyGeneration</span></span>

<span data-ttu-id="dd7b1-155">自動的に有効期限を実現するキー (新しいキーを作成する) を展開するアプリをたくないシナリオがあります。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-155">You may have a scenario where you don't want an app to automatically roll keys (create new keys) as they approach expiration.</span></span> <span data-ttu-id="dd7b1-156">この 1 つの例には、アプリでプライマリ アプリのみがキー管理の問題に責任を負います、セカンダリのアプリは、キー リングの読み取り専用ビューを単純にあるプライマリ/セカンダリの関係を設定する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-156">One example of this might be apps set up in a primary/secondary relationship, where only the primary app is responsible for key management concerns and secondary apps simply have a read-only view of the key ring.</span></span> <span data-ttu-id="dd7b1-157">使用してシステムを構成することによって、読み取り専用としてキー リングを処理するセカンダリのアプリを構成することができます<xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.DisableAutomaticKeyGeneration*>:</span><span class="sxs-lookup"><span data-stu-id="dd7b1-157">The secondary apps can be configured to treat the key ring as read-only by configuring the system with <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.DisableAutomaticKeyGeneration*>:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

## <a name="per-application-isolation"></a><span data-ttu-id="dd7b1-158">アプリケーションごとの分離</span><span class="sxs-lookup"><span data-stu-id="dd7b1-158">Per-application isolation</span></span>

<span data-ttu-id="dd7b1-159">データ保護システムを指定するには、ASP.NET Core のホストによって、自動的に分離され、そのアプリから、別場合でも、それらのアプリが同じワーカー プロセス アカウントで実行されていて、同じマスター キー生成情報を使用しています。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-159">When the Data Protection system is provided by an ASP.NET Core host, it automatically isolates apps from one another, even if those apps are running under the same worker process account and are using the same master keying material.</span></span> <span data-ttu-id="dd7b1-160">これは、System.Web から IsolateApps 修飾子に少し似ています`<machineKey>`要素。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-160">This is somewhat similar to the IsolateApps modifier from System.Web's `<machineKey>` element.</span></span>

<span data-ttu-id="dd7b1-161">したがって、一意のテナントとして、ローカル コンピューター上の各アプリを考慮して分離メカニズムの動作、<xref:Microsoft.AspNetCore.DataProtection.IDataProtector>ルートの指定したアプリに自動的に識別子としてアプリ ID が含まれています。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-161">The isolation mechanism works by considering each app on the local machine as a unique tenant, thus the <xref:Microsoft.AspNetCore.DataProtection.IDataProtector> rooted for any given app automatically includes the app ID as a discriminator.</span></span> <span data-ttu-id="dd7b1-162">アプリの一意の ID は、アプリの物理パスです。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-162">The app's unique ID is the app's physical path:</span></span>

* <span data-ttu-id="dd7b1-163">ホストされるアプリ[IIS](xref:fundamentals/servers/index#iis-http-server)、一意の ID は、アプリの IIS 物理パス。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-163">For apps hosted in [IIS](xref:fundamentals/servers/index#iis-http-server), the unique ID is the IIS physical path of the app.</span></span> <span data-ttu-id="dd7b1-164">場合は、web ファーム環境でアプリを展開すると、この値は、IIS 環境は web ファーム内のすべてのコンピューター間で同様に構成されていると仮定安定したは。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-164">If an app is deployed in a web farm environment, this value is stable assuming that the IIS environments are configured similarly across all machines in the web farm.</span></span>
* <span data-ttu-id="dd7b1-165">実行される自己ホスト型アプリ、 [Kestrel サーバー](xref:fundamentals/servers/index#kestrel)、一意の ID は、ディスク上のアプリへの物理パス。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-165">For self-hosted apps running on the [Kestrel server](xref:fundamentals/servers/index#kestrel), the unique ID is the physical path to the app on disk.</span></span>

<span data-ttu-id="dd7b1-166">一意の識別子がリセット後も存続するように設計&mdash;の個々 のアプリと、マシン自体の両方。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-166">The unique identifier is designed to survive resets&mdash;both of the individual app and of the machine itself.</span></span>

<span data-ttu-id="dd7b1-167">この分離メカニズムでは、アプリが悪意のあるいないことを前提としています。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-167">This isolation mechanism assumes that the apps are not malicious.</span></span> <span data-ttu-id="dd7b1-168">悪意のあるアプリを常に同じワーカー プロセス アカウントで実行されている他のアプリに影響する可能性です。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-168">A malicious app can always impact any other app running under the same worker process account.</span></span> <span data-ttu-id="dd7b1-169">アプリが相互に信頼されていない共有ホスティング環境、ホスティング プロバイダーは、アプリのキーのリポジトリを基になる分離を含む、アプリ間での OS レベルの分離を講じる必要があります。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-169">In a shared hosting environment where apps are mutually untrusted, the hosting provider should take steps to ensure OS-level isolation between apps, including separating the apps' underlying key repositories.</span></span>

<span data-ttu-id="dd7b1-170">データ保護システムを ASP.NET Core のホストによって提供されないかどうか (経由でインスタンス化する場合など、`DataProtectionProvider`具象型) アプリの分離が既定で無効になっています。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-170">If the Data Protection system isn't provided by an ASP.NET Core host (for example, if you instantiate it via the `DataProtectionProvider` concrete type) app isolation is disabled by default.</span></span> <span data-ttu-id="dd7b1-171">適切な提供される限り、アプリの分離が無効になっているときに、同じキー マテリアルに支えすべてのアプリではペイロードを共有できます[目的で](xref:security/data-protection/consumer-apis/purpose-strings)します。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-171">When app isolation is disabled, all apps backed by the same keying material can share payloads as long as they provide the appropriate [purposes](xref:security/data-protection/consumer-apis/purpose-strings).</span></span> <span data-ttu-id="dd7b1-172">この環境でアプリの分離を提供するには、呼び出し、 [SetApplicationName](#setapplicationname)メソッドを構成オブジェクトし、アプリごとに一意の名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-172">To provide app isolation in this environment, call the [SetApplicationName](#setapplicationname) method on the configuration object and provide a unique name for each app.</span></span>

## <a name="changing-algorithms-with-usecryptographicalgorithms"></a><span data-ttu-id="dd7b1-173">UseCryptographicAlgorithms でアルゴリズムを変更します。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-173">Changing algorithms with UseCryptographicAlgorithms</span></span>

<span data-ttu-id="dd7b1-174">データ保護スタックを使用して、新しく生成されたキーで使用される既定のアルゴリズムを変更できます。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-174">The Data Protection stack allows you to change the default algorithm used by newly-generated keys.</span></span> <span data-ttu-id="dd7b1-175">これを行う最も簡単な方法を呼び出すことです。 [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms)構成コールバックから。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-175">The simplest way to do this is to call [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) from the configuration callback:</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptorConfiguration()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptionSettings()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

::: moniker-end

<span data-ttu-id="dd7b1-176">既定の EncryptionAlgorithm は AES 256-CBC で、既定値 ValidationAlgorithm は HMACSHA256 です。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-176">The default EncryptionAlgorithm is AES-256-CBC, and the default ValidationAlgorithm is HMACSHA256.</span></span> <span data-ttu-id="dd7b1-177">経由でシステム管理者によって、既定のポリシーを設定することができます、[コンピューター レベル ポリシー](xref:security/data-protection/configuration/machine-wide-policy)を明示的に呼び出すが、`UseCryptographicAlgorithms`既定ポリシーによって上書きされます。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-177">The default policy can be set by a system administrator via a [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy), but an explicit call to `UseCryptographicAlgorithms` overrides the default policy.</span></span>

<span data-ttu-id="dd7b1-178">呼び出す`UseCryptographicAlgorithms`定義済みの組み込みの一覧から目的のアルゴリズムを指定することができます。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-178">Calling `UseCryptographicAlgorithms` allows you to specify the desired algorithm from a predefined built-in list.</span></span> <span data-ttu-id="dd7b1-179">アルゴリズムの実装について心配する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-179">You don't need to worry about the implementation of the algorithm.</span></span> <span data-ttu-id="dd7b1-180">上記のシナリオでは、データ保護システムは、Windows で実行されている場合は、AES の CNG 実装を使用しようとします。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-180">In the scenario above, the Data Protection system attempts to use the CNG implementation of AES if running on Windows.</span></span> <span data-ttu-id="dd7b1-181">それ以外の場合、フォールバックをマネージ[System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes)クラス。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-181">Otherwise, it falls back to the managed [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) class.</span></span>

<span data-ttu-id="dd7b1-182">呼び出しを使用して実装を手動で指定[UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms)します。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-182">You can manually specify an implementation via a call to [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).</span></span>

> [!TIP]
> <span data-ttu-id="dd7b1-183">アルゴリズムを変更すると、既存のキーをキー リングには影響しません。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-183">Changing algorithms doesn't affect existing keys in the key ring.</span></span> <span data-ttu-id="dd7b1-184">新しく生成されたキーのみに影響します。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-184">It only affects newly-generated keys.</span></span>

### <a name="specifying-custom-managed-algorithms"></a><span data-ttu-id="dd7b1-185">管理対象のカスタム アルゴリズムを指定します。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-185">Specifying custom managed algorithms</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="dd7b1-186">管理対象のカスタム アルゴリズムを指定するには、作成、 [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration)実装の種類を示すインスタンス。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-186">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) instance that points to the implementation types:</span></span>

```csharp
serviceCollection.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new ManagedAuthenticatedEncryptorConfiguration()
    {
        // A type that subclasses SymmetricAlgorithm
        EncryptionAlgorithmType = typeof(Aes),

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // A type that subclasses KeyedHashAlgorithm
        ValidationAlgorithmType = typeof(HMACSHA256)
    });
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="dd7b1-187">管理対象のカスタム アルゴリズムを指定するには、作成、 [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings)実装の種類を示すインスタンス。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-187">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) instance that points to the implementation types:</span></span>

```csharp
serviceCollection.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new ManagedAuthenticatedEncryptionSettings()
    {
        // A type that subclasses SymmetricAlgorithm
        EncryptionAlgorithmType = typeof(Aes),

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // A type that subclasses KeyedHashAlgorithm
        ValidationAlgorithmType = typeof(HMACSHA256)
    });
```

::: moniker-end

<span data-ttu-id="dd7b1-188">一般に、\*の Type プロパティは、具体的をポイントする必要があります (パラメーターなしコンス トラクターを公開) を使用してインスタンス化可能な実装の[SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm)と[KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm)できたとして、特殊なケースなどのいくつかの値のシステム`typeof(Aes)`利便性を考慮します。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-188">Generally the \*Type properties must point to concrete, instantiable (via a public parameterless ctor) implementations of [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) and [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), though the system special-cases some values like `typeof(Aes)` for convenience.</span></span>

> [!NOTE]
> <span data-ttu-id="dd7b1-189">PKCS #7 埋め込みで CBC モードの暗号化をサポートする必要があり、SymmetricAlgorithm は ≥ 128 ビットのキーの長さと ≥ の 64 ビットのブロック サイズにあります。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-189">The SymmetricAlgorithm must have a key length of ≥ 128 bits and a block size of ≥ 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="dd7b1-190">ダイジェストのサイズをいる必要があります、KeyedHashAlgorithm > = 128 ビット、およびハッシュ アルゴリズムのダイジェストの長さと同じ長さのキーをサポートする必要があります。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-190">The KeyedHashAlgorithm must have a digest size of >= 128 bits, and it must support keys of length equal to the hash algorithm's digest length.</span></span> <span data-ttu-id="dd7b1-191">HMAC を必須としたときに、KeyedHashAlgorithm はありません。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-191">The KeyedHashAlgorithm isn't strictly required to be HMAC.</span></span>

### <a name="specifying-custom-windows-cng-algorithms"></a><span data-ttu-id="dd7b1-192">カスタムの Windows CNG アルゴリズムを指定します。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-192">Specifying custom Windows CNG algorithms</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="dd7b1-193">CBC モードの暗号化を使用して、HMAC の検証にカスタム Windows CNG アルゴリズムを指定するには、作成、 [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration)アルゴリズム情報を格納するインスタンス。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-193">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngCbcAuthenticatedEncryptorConfiguration()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // Passed to BCryptOpenAlgorithmProvider
        HashAlgorithm = "SHA256",
        HashAlgorithmProvider = null
    });
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="dd7b1-194">CBC モードの暗号化を使用して、HMAC の検証にカスタム Windows CNG アルゴリズムを指定するには、作成、 [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings)アルゴリズム情報を格納するインスタンス。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-194">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngCbcAuthenticatedEncryptionSettings()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // Passed to BCryptOpenAlgorithmProvider
        HashAlgorithm = "SHA256",
        HashAlgorithmProvider = null
    });
```

::: moniker-end

> [!NOTE]
> <span data-ttu-id="dd7b1-195">対称ブロック暗号アルゴリズム、キーの長さをいる必要があります > = 128 ビットのブロック サイズ > = 64 ビット、および PKCS #7 埋め込みで CBC モードの暗号化をサポートする必要があります。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-195">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of >= 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="dd7b1-196">ハッシュ アルゴリズムのダイジェストのサイズをいる必要があります > = 128 ビットと、BCRYPT で開かれるをサポートする必要があります\_ALG\_処理\_HMAC\_フラグ フラグ。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-196">The hash algorithm must have a digest size of >= 128 bits and must support being opened with the BCRYPT\_ALG\_HANDLE\_HMAC\_FLAG flag.</span></span> <span data-ttu-id="dd7b1-197">\*プロバイダーのプロパティは、指定したアルゴリズムの既定のプロバイダーを使用するには null に設定できます。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-197">The \*Provider properties can be set to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="dd7b1-198">参照してください、 [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx)詳細についてはドキュメントです。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-198">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="dd7b1-199">検証で Galois/カウンター モード暗号化を使用するカスタムの Windows CNG アルゴリズムを指定するには、作成、 [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration)アルゴリズム情報を格納するインスタンス。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-199">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngGcmAuthenticatedEncryptorConfiguration()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256
    });
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="dd7b1-200">検証で Galois/カウンター モード暗号化を使用するカスタムの Windows CNG アルゴリズムを指定するには、作成、 [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings)アルゴリズム情報を格納するインスタンス。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-200">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngGcmAuthenticatedEncryptionSettings()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256
    });
```

::: moniker-end

> [!NOTE]
> <span data-ttu-id="dd7b1-201">対称ブロック暗号アルゴリズム、キーの長さをいる必要があります > =、正確に 128 ビットのブロック サイズを 128 ビットと GCM の暗号化をサポートする必要があります。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-201">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of exactly 128 bits, and it must support GCM encryption.</span></span> <span data-ttu-id="dd7b1-202">設定することができます、 [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider)プロパティを指定したアルゴリズムの既定のプロバイダーを使用する場合は null です。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-202">You can set the [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) property to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="dd7b1-203">参照してください、 [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx)詳細についてはドキュメントです。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-203">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

### <a name="specifying-other-custom-algorithms"></a><span data-ttu-id="dd7b1-204">その他のカスタム アルゴリズムを指定します。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-204">Specifying other custom algorithms</span></span>

<span data-ttu-id="dd7b1-205">ファースト クラスの API として公開されている、データ保護システムはほぼすべての種類のアルゴリズムを指定することは拡張性を備えています。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-205">Though not exposed as a first-class API, the Data Protection system is extensible enough to allow specifying almost any kind of algorithm.</span></span> <span data-ttu-id="dd7b1-206">たとえば、ハードウェア セキュリティ モジュール (HSM) 内に含まれるすべてのキーを保持し、core のカスタム実装の暗号化と復号化ルーチンを提供するも可能です。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-206">For example, it's possible to keep all keys contained within a Hardware Security Module (HSM) and to provide a custom implementation of the core encryption and decryption routines.</span></span> <span data-ttu-id="dd7b1-207">参照してください[IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor)で[暗号化の拡張性をコア](xref:security/data-protection/extensibility/core-crypto)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-207">See [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) in [Core cryptography extensibility](xref:security/data-protection/extensibility/core-crypto) for more information.</span></span>

## <a name="persisting-keys-when-hosting-in-a-docker-container"></a><span data-ttu-id="dd7b1-208">Docker コンテナーでホストする場合、永続化キー</span><span class="sxs-lookup"><span data-stu-id="dd7b1-208">Persisting keys when hosting in a Docker container</span></span>

<span data-ttu-id="dd7b1-209">ホストする場合、 [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/)コンテナー、キーは、いずれかで保持する必要があります。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-209">When hosting in a [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) container, keys should be maintained in either:</span></span>

* <span data-ttu-id="dd7b1-210">Docker ボリュームの共有ボリュームまたはホストにマウントされたボリュームなど、コンテナーの有効期間を超えて保持するフォルダー。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-210">A folder that's a Docker volume that persists beyond the container's lifetime, such as a shared volume or a host-mounted volume.</span></span>
* <span data-ttu-id="dd7b1-211">外部プロバイダーなど[Azure Key Vault](https://azure.microsoft.com/services/key-vault/)または[Redis](https://redis.io/)します。</span><span class="sxs-lookup"><span data-stu-id="dd7b1-211">An external provider, such as [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) or [Redis](https://redis.io/).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="dd7b1-212">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="dd7b1-212">Additional resources</span></span>

* <xref:security/data-protection/configuration/non-di-scenarios>
* <xref:security/data-protection/configuration/machine-wide-policy>
* <xref:host-and-deploy/web-farm>
* <xref:security/data-protection/implementation/key-storage-providers>
