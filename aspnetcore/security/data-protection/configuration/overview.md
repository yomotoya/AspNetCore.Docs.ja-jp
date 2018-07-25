---
title: ASP.NET Core データ保護を構成します。
author: rick-anderson
description: ASP.NET Core でのデータ保護を構成する方法について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 07/17/2017
uid: security/data-protection/configuration/overview
ms.openlocfilehash: fd3e5dff114c81eae07186e735a15314d984dcc3
ms.sourcegitcommit: 5338b1ed9e2ef225ab565d6cba072b474fd9324d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/25/2018
ms.locfileid: "39243111"
---
# <a name="configure-aspnet-core-data-protection"></a><span data-ttu-id="177af-103">ASP.NET Core データ保護を構成します。</span><span class="sxs-lookup"><span data-stu-id="177af-103">Configure ASP.NET Core Data Protection</span></span>

<span data-ttu-id="177af-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="177af-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="177af-105">データ保護システムが初期化されると、適用[既定の設定](xref:security/data-protection/configuration/default-settings)運用環境に基づきます。</span><span class="sxs-lookup"><span data-stu-id="177af-105">When the Data Protection system is initialized, it applies [default settings](xref:security/data-protection/configuration/default-settings) based on the operational environment.</span></span> <span data-ttu-id="177af-106">これらの設定は、1 台のコンピューターで実行されているアプリの一般的に適しています。</span><span class="sxs-lookup"><span data-stu-id="177af-106">These settings are generally appropriate for apps running on a single machine.</span></span> <span data-ttu-id="177af-107">開発者は、既定の設定を変更する必要のあるケースがあります。</span><span class="sxs-lookup"><span data-stu-id="177af-107">There are cases where a developer may want to change the default settings:</span></span>

* <span data-ttu-id="177af-108">複数のコンピューターで、アプリの負荷が分散されます。</span><span class="sxs-lookup"><span data-stu-id="177af-108">The app is spread across multiple machines.</span></span>
* <span data-ttu-id="177af-109">コンプライアンス上の理由からです。</span><span class="sxs-lookup"><span data-stu-id="177af-109">For compliance reasons.</span></span>

<span data-ttu-id="177af-110">これらのシナリオについては、データ保護システムは、高度な構成 API を提供します。</span><span class="sxs-lookup"><span data-stu-id="177af-110">For these scenarios, the Data Protection system offers a rich configuration API.</span></span>

> [!WARNING]
> <span data-ttu-id="177af-111">構成ファイルと同様に、データ保護キー リングする必要があります保護する適切なアクセス許可を使用します。</span><span class="sxs-lookup"><span data-stu-id="177af-111">Similar to configuration files, the data protection key ring should be protected using appropriate permissions.</span></span> <span data-ttu-id="177af-112">、保存時のキーを暗号化することもできますが、攻撃者を防ぐ新しいキーを作成するがこれです。</span><span class="sxs-lookup"><span data-stu-id="177af-112">You can choose to encrypt keys at rest, but this doesn't prevent attackers from creating new keys.</span></span> <span data-ttu-id="177af-113">その結果、アプリのセキュリティに影響します。</span><span class="sxs-lookup"><span data-stu-id="177af-113">Consequently, your app's security is impacted.</span></span> <span data-ttu-id="177af-114">データ保護で構成されている記憶域の場所のアクセスは、アプリ自体と同じように構成ファイルを保護するように制限があります。</span><span class="sxs-lookup"><span data-stu-id="177af-114">The storage location configured with Data Protection should have its access limited to the app itself, similar to the way you would protect configuration files.</span></span> <span data-ttu-id="177af-115">たとえば、ディスク上のキー リングを格納する場合は、ファイル システム権限を使用します。</span><span class="sxs-lookup"><span data-stu-id="177af-115">For example, if you choose to store your key ring on disk, use file system permissions.</span></span> <span data-ttu-id="177af-116">Id のみを確認します。 web アプリの実行に読み取り、書き込み、およびそのディレクトリへのアクセスを作成します。</span><span class="sxs-lookup"><span data-stu-id="177af-116">Ensure only the identity under which your web app runs has read, write, and create access to that directory.</span></span> <span data-ttu-id="177af-117">Azure Table Storage を使用する場合、web アプリのみが読み取り、書き込み、またはテーブル ストアなどの新しいエントリを作成する機能に必要です。</span><span class="sxs-lookup"><span data-stu-id="177af-117">If you use Azure Table Storage, only the web app should have the ability to read, write, or create new entries in the table store, etc.</span></span>
>
> <span data-ttu-id="177af-118">拡張メソッド[AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection)を返します、 [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder)します。</span><span class="sxs-lookup"><span data-stu-id="177af-118">The extension method [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) returns an [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder).</span></span> <span data-ttu-id="177af-119">`IDataProtectionBuilder` 連結できますデータ保護を構成するオプションの拡張メソッドを公開します。</span><span class="sxs-lookup"><span data-stu-id="177af-119">`IDataProtectionBuilder` exposes extension methods that you can chain together to configure Data Protection options.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="protectkeyswithazurekeyvault"></a><span data-ttu-id="177af-120">ProtectKeysWithAzureKeyVault</span><span class="sxs-lookup"><span data-stu-id="177af-120">ProtectKeysWithAzureKeyVault</span></span>

<span data-ttu-id="177af-121">キーを保管する[Azure Key Vault](https://azure.microsoft.com/services/key-vault/)、構成を使用してシステム[ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault)で、`Startup`クラス。</span><span class="sxs-lookup"><span data-stu-id="177af-121">To store keys in [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), configure the system with [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) in the `Startup` class:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blobUriWithSasToken>"))
        .ProtectKeysWithAzureKeyVault("<keyIdentifier>", "<clientId>", "<clientSecret>");
}
```

<span data-ttu-id="177af-122">設定キー リング記憶域の場所 (たとえば、 [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage))。</span><span class="sxs-lookup"><span data-stu-id="177af-122">Set the key ring storage location (for example, [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage)).</span></span> <span data-ttu-id="177af-123">呼び出すために、場所を設定する必要があります`ProtectKeysWithAzureKeyVault`実装、 [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor)キー リング記憶域の場所を含む、データの自動保護設定を無効にします。</span><span class="sxs-lookup"><span data-stu-id="177af-123">The location must be set because calling `ProtectKeysWithAzureKeyVault` implements an [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor) that disables automatic data protection settings, including the key ring storage location.</span></span> <span data-ttu-id="177af-124">前の例では、Azure Blob Storage を使用して、キー リングを保持します。</span><span class="sxs-lookup"><span data-stu-id="177af-124">The preceding example uses Azure Blob Storage to persist the key ring.</span></span> <span data-ttu-id="177af-125">詳細については、次を参照してください。[キー記憶域プロバイダー: Azure と Redis](xref:security/data-protection/implementation/key-storage-providers#azure-and-redis)します。</span><span class="sxs-lookup"><span data-stu-id="177af-125">For more information, see [Key storage providers: Azure and Redis](xref:security/data-protection/implementation/key-storage-providers#azure-and-redis).</span></span> <span data-ttu-id="177af-126">ローカルにキー リングを保存することもできます。 [PersistKeysToFileSystem](xref:security/data-protection/implementation/key-storage-providers#file-system)します。</span><span class="sxs-lookup"><span data-stu-id="177af-126">You can also persist the key ring locally with [PersistKeysToFileSystem](xref:security/data-protection/implementation/key-storage-providers#file-system).</span></span>

<span data-ttu-id="177af-127">`keyIdentifier`はキーの暗号化に使用されるキー コンテナーのキー識別子です (たとえば、 `https://contosokeyvault.vault.azure.net/keys/dataprotection/`)。</span><span class="sxs-lookup"><span data-stu-id="177af-127">The `keyIdentifier` is the key vault key identifier used for key encryption (for example, `https://contosokeyvault.vault.azure.net/keys/dataprotection/`).</span></span>

<span data-ttu-id="177af-128">`ProtectKeysWithAzureKeyVault` オーバー ロードします。</span><span class="sxs-lookup"><span data-stu-id="177af-128">`ProtectKeysWithAzureKeyVault` overloads:</span></span>

* <span data-ttu-id="177af-129">[ProtectKeysWithAzureKeyVault (IDataProtectionBuilder、KeyVaultClient、文字列)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_Microsoft_Azure_KeyVault_KeyVaultClient_System_String_)の使用を許可、 [KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient) key vault を使用するデータ保護システムを有効にします。</span><span class="sxs-lookup"><span data-stu-id="177af-129">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, KeyVaultClient, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_Microsoft_Azure_KeyVault_KeyVaultClient_System_String_) permits the use of a [KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient) to enable the data protection system to use the key vault.</span></span>
* <span data-ttu-id="177af-130">[(IDataProtectionBuilder、文字列、文字列、X509Certificate2) ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_Security_Cryptography_X509Certificates_X509Certificate2_)の使用を許可、`ClientId`と[X509Certificate](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) key vault を使用するデータ保護システムを有効にします。</span><span class="sxs-lookup"><span data-stu-id="177af-130">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, String, String, X509Certificate2)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_Security_Cryptography_X509Certificates_X509Certificate2_) permits the use of a `ClientId` and [X509Certificate](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) to enable the data protection system to use the key vault.</span></span>
* <span data-ttu-id="177af-131">[(IDataProtectionBuilder、文字列、String, String) ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_String_)の使用を許可、`ClientId`と`ClientSecret`key vault を使用するデータ保護システムを有効にします。</span><span class="sxs-lookup"><span data-stu-id="177af-131">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, String, String, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_String_) permits the use of a `ClientId` and `ClientSecret` to enable the data protection system to use the key vault.</span></span>

::: moniker-end

## <a name="persistkeystofilesystem"></a><span data-ttu-id="177af-132">PersistKeysToFileSystem</span><span class="sxs-lookup"><span data-stu-id="177af-132">PersistKeysToFileSystem</span></span>

<span data-ttu-id="177af-133">代わりに UNC 共有でのキーを格納する、 *%localappdata%* 既定の場所、構成を使用してシステム[PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span><span class="sxs-lookup"><span data-stu-id="177af-133">To store keys on a UNC share instead of at the *%LOCALAPPDATA%* default location, configure the system with [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

> [!WARNING]
> <span data-ttu-id="177af-134">キーの永続性の場所を変更する場合、システムは DPAPI は、適切な暗号化メカニズムであるかどうかを認識していないため、保存時のキーを暗号化する自動的にします。</span><span class="sxs-lookup"><span data-stu-id="177af-134">If you change the key persistence location, the system no longer automatically encrypts keys at rest, since it doesn't know whether DPAPI is an appropriate encryption mechanism.</span></span>

## <a name="protectkeyswith"></a><span data-ttu-id="177af-135">ProtectKeysWith\*</span><span class="sxs-lookup"><span data-stu-id="177af-135">ProtectKeysWith\*</span></span>

<span data-ttu-id="177af-136">いずれかを呼び出すことによって、保存時のキーを保護するシステムを構成することができます、 [ProtectKeysWith\* ](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) Api を構成します。</span><span class="sxs-lookup"><span data-stu-id="177af-136">You can configure the system to protect keys at rest by calling any of the [ProtectKeysWith\*](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) configuration APIs.</span></span> <span data-ttu-id="177af-137">UNC 共有にキーを格納し、特定の X.509 証明書での保存時に、そのキーを暗号化する次の例を検討してください。</span><span class="sxs-lookup"><span data-stu-id="177af-137">Consider the example below, which stores keys on a UNC share and encrypts those keys at rest with a specific X.509 certificate:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="177af-138">ASP.NET Core 2.1 以降では、行うことができます、 [X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2)に[ProtectKeysWithCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithcertificate)ファイルから読み込まれた証明書など。</span><span class="sxs-lookup"><span data-stu-id="177af-138">In ASP.NET Core 2.1 or later, you can provide an [X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) to [ProtectKeysWithCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithcertificate), such as a certificate loaded from a file:</span></span>

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

<span data-ttu-id="177af-139">参照してください[キーの暗号化に](xref:security/data-protection/implementation/key-encryption-at-rest)詳細の例と、組み込みのキーの暗号化メカニズムについて説明します。</span><span class="sxs-lookup"><span data-stu-id="177af-139">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more examples and discussion on the built-in key encryption mechanisms.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="unprotectkeyswithanycertificate"></a><span data-ttu-id="177af-140">UnprotectKeysWithAnyCertificate</span><span class="sxs-lookup"><span data-stu-id="177af-140">UnprotectKeysWithAnyCertificate</span></span>

<span data-ttu-id="177af-141">ASP.NET Core 2.1 以降では、証明書の回転し、配列を使用して保存時のキーを復号化[X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2)で証明書[UnprotectKeysWithAnyCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.unprotectkeyswithanycertificate):</span><span class="sxs-lookup"><span data-stu-id="177af-141">In ASP.NET Core 2.1 or later, you can rotate certificates and decrypt keys at rest using an array of [X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) certificates with [UnprotectKeysWithAnyCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.unprotectkeyswithanycertificate):</span></span>

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

## <a name="setdefaultkeylifetime"></a><span data-ttu-id="177af-142">SetDefaultKeyLifetime</span><span class="sxs-lookup"><span data-stu-id="177af-142">SetDefaultKeyLifetime</span></span>

<span data-ttu-id="177af-143">90 日間、既定ではなくキーの有効期間は 14 日間を使用するシステムを構成するには使用[SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span><span class="sxs-lookup"><span data-stu-id="177af-143">To configure the system to use a key lifetime of 14 days instead of the default 90 days, use [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

## <a name="setapplicationname"></a><span data-ttu-id="177af-144">SetApplicationName</span><span class="sxs-lookup"><span data-stu-id="177af-144">SetApplicationName</span></span>

<span data-ttu-id="177af-145">既定では、データ保護システムは、同じ物理キーのリポジトリを共有している場合でも、別のアプリを分離します。</span><span class="sxs-lookup"><span data-stu-id="177af-145">By default, the Data Protection system isolates apps from one another, even if they're sharing the same physical key repository.</span></span> <span data-ttu-id="177af-146">これは、アプリが互いの保護されたペイロードを理解することを防ぎます。</span><span class="sxs-lookup"><span data-stu-id="177af-146">This prevents the apps from understanding each other's protected payloads.</span></span> <span data-ttu-id="177af-147">2 つのアプリ間で保護されたペイロードを共有する使用[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname)アプリごとに同じ値を持つ。</span><span class="sxs-lookup"><span data-stu-id="177af-147">To share protected payloads between two apps, use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) with the same value for each app:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("shared app name");
}
```

## <a name="disableautomatickeygeneration"></a><span data-ttu-id="177af-148">DisableAutomaticKeyGeneration</span><span class="sxs-lookup"><span data-stu-id="177af-148">DisableAutomaticKeyGeneration</span></span>

<span data-ttu-id="177af-149">自動的に有効期限を実現するキー (新しいキーを作成する) を展開するアプリをたくないシナリオがあります。</span><span class="sxs-lookup"><span data-stu-id="177af-149">You may have a scenario where you don't want an app to automatically roll keys (create new keys) as they approach expiration.</span></span> <span data-ttu-id="177af-150">この 1 つの例には、アプリでプライマリ アプリのみがキー管理の問題に責任を負います、セカンダリのアプリは、キー リングの読み取り専用ビューを単純にあるプライマリ/セカンダリの関係を設定する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="177af-150">One example of this might be apps set up in a primary/secondary relationship, where only the primary app is responsible for key management concerns and secondary apps simply have a read-only view of the key ring.</span></span> <span data-ttu-id="177af-151">使用してシステムを構成することによって、読み取り専用としてキー リングを処理するセカンダリのアプリを構成することができます[DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):</span><span class="sxs-lookup"><span data-stu-id="177af-151">The secondary apps can be configured to treat the key ring as read-only by configuring the system with [DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

## <a name="per-application-isolation"></a><span data-ttu-id="177af-152">アプリケーションごとの分離</span><span class="sxs-lookup"><span data-stu-id="177af-152">Per-application isolation</span></span>

<span data-ttu-id="177af-153">データ保護システムを指定するには、ASP.NET Core のホストによって、自動的に分離され、そのアプリから、別場合でも、それらのアプリが同じワーカー プロセス アカウントで実行されていて、同じマスター キー生成情報を使用しています。</span><span class="sxs-lookup"><span data-stu-id="177af-153">When the Data Protection system is provided by an ASP.NET Core host, it automatically isolates apps from one another, even if those apps are running under the same worker process account and are using the same master keying material.</span></span> <span data-ttu-id="177af-154">これは、System.Web から IsolateApps 修飾子に少し似ています **\<machineKey >** 要素。</span><span class="sxs-lookup"><span data-stu-id="177af-154">This is somewhat similar to the IsolateApps modifier from System.Web's **\<machineKey>** element.</span></span>

<span data-ttu-id="177af-155">したがって、一意のテナントとして、ローカル コンピューター上の各アプリを考慮して分離メカニズムの動作、 [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector)ルートの指定したアプリに自動的に識別子としてアプリ ID が含まれています。</span><span class="sxs-lookup"><span data-stu-id="177af-155">The isolation mechanism works by considering each app on the local machine as a unique tenant, thus the [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) rooted for any given app automatically includes the app ID as a discriminator.</span></span> <span data-ttu-id="177af-156">アプリの一意の ID では、2 つの場所のいずれかのです。</span><span class="sxs-lookup"><span data-stu-id="177af-156">The app's unique ID comes from one of two places:</span></span>

1. <span data-ttu-id="177af-157">アプリが IIS でホストされる、一意の識別子は、アプリの構成パスです。</span><span class="sxs-lookup"><span data-stu-id="177af-157">If the app is hosted in IIS, the unique identifier is the app's configuration path.</span></span> <span data-ttu-id="177af-158">Web ファーム環境でアプリを展開する場合は、IIS 環境は web ファーム内のすべてのコンピューター間で同様に構成されていると仮定するとこの値が安定したなります。</span><span class="sxs-lookup"><span data-stu-id="177af-158">If an app is deployed in a web farm environment, this value should be stable assuming that the IIS environments are configured similarly across all machines in the web farm.</span></span>

2. <span data-ttu-id="177af-159">アプリは、IIS でホストされていない、一意の識別子は、アプリの物理パスです。</span><span class="sxs-lookup"><span data-stu-id="177af-159">If the app isn't hosted in IIS, the unique identifier is the physical path of the app.</span></span>

<span data-ttu-id="177af-160">一意の識別子がリセット後も存続するように設計&mdash;の個々 のアプリと、マシン自体の両方。</span><span class="sxs-lookup"><span data-stu-id="177af-160">The unique identifier is designed to survive resets &mdash; both of the individual app and of the machine itself.</span></span>

<span data-ttu-id="177af-161">この分離メカニズムでは、アプリが悪意のあるいないことを前提としています。</span><span class="sxs-lookup"><span data-stu-id="177af-161">This isolation mechanism assumes that the apps are not malicious.</span></span> <span data-ttu-id="177af-162">悪意のあるアプリを常に同じワーカー プロセス アカウントで実行されている他のアプリに影響する可能性です。</span><span class="sxs-lookup"><span data-stu-id="177af-162">A malicious app can always impact any other app running under the same worker process account.</span></span> <span data-ttu-id="177af-163">アプリが相互に信頼されていない共有ホスティング環境、ホスティング プロバイダーは、アプリのキーのリポジトリを基になる分離を含む、アプリ間での OS レベルの分離を講じる必要があります。</span><span class="sxs-lookup"><span data-stu-id="177af-163">In a shared hosting environment where apps are mutually untrusted, the hosting provider should take steps to ensure OS-level isolation between apps, including separating the apps' underlying key repositories.</span></span>

<span data-ttu-id="177af-164">データ保護システムを ASP.NET Core のホストによって提供されないかどうか (経由でインスタンス化する場合など、`DataProtectionProvider`具象型) アプリの分離が既定で無効になっています。</span><span class="sxs-lookup"><span data-stu-id="177af-164">If the Data Protection system isn't provided by an ASP.NET Core host (for example, if you instantiate it via the `DataProtectionProvider` concrete type) app isolation is disabled by default.</span></span> <span data-ttu-id="177af-165">適切な提供される限り、アプリの分離が無効になっているときに、同じキー マテリアルに支えすべてのアプリではペイロードを共有できます[目的で](xref:security/data-protection/consumer-apis/purpose-strings)します。</span><span class="sxs-lookup"><span data-stu-id="177af-165">When app isolation is disabled, all apps backed by the same keying material can share payloads as long as they provide the appropriate [purposes](xref:security/data-protection/consumer-apis/purpose-strings).</span></span> <span data-ttu-id="177af-166">この環境でアプリの分離を提供するには、呼び出し、 [SetApplicationName](#setapplicationname)メソッドを構成オブジェクトし、アプリごとに一意の名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="177af-166">To provide app isolation in this environment, call the [SetApplicationName](#setapplicationname) method on the configuration object and provide a unique name for each app.</span></span>

## <a name="changing-algorithms-with-usecryptographicalgorithms"></a><span data-ttu-id="177af-167">UseCryptographicAlgorithms でアルゴリズムを変更します。</span><span class="sxs-lookup"><span data-stu-id="177af-167">Changing algorithms with UseCryptographicAlgorithms</span></span>

<span data-ttu-id="177af-168">データ保護スタックを使用して、新しく生成されたキーで使用される既定のアルゴリズムを変更できます。</span><span class="sxs-lookup"><span data-stu-id="177af-168">The Data Protection stack allows you to change the default algorithm used by newly-generated keys.</span></span> <span data-ttu-id="177af-169">これを行う最も簡単な方法を呼び出すことです。 [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms)構成コールバックから。</span><span class="sxs-lookup"><span data-stu-id="177af-169">The simplest way to do this is to call [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) from the configuration callback:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="177af-170">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="177af-170">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptorConfiguration()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="177af-171">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="177af-171">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptionSettings()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

---

<span data-ttu-id="177af-172">既定の EncryptionAlgorithm は AES 256-CBC で、既定値 ValidationAlgorithm は HMACSHA256 です。</span><span class="sxs-lookup"><span data-stu-id="177af-172">The default EncryptionAlgorithm is AES-256-CBC, and the default ValidationAlgorithm is HMACSHA256.</span></span> <span data-ttu-id="177af-173">経由でシステム管理者によって、既定のポリシーを設定することができます、[コンピューター レベル ポリシー](xref:security/data-protection/configuration/machine-wide-policy)を明示的に呼び出すが、`UseCryptographicAlgorithms`既定ポリシーによって上書きされます。</span><span class="sxs-lookup"><span data-stu-id="177af-173">The default policy can be set by a system administrator via a [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy), but an explicit call to `UseCryptographicAlgorithms` overrides the default policy.</span></span>

<span data-ttu-id="177af-174">呼び出す`UseCryptographicAlgorithms`定義済みの組み込みの一覧から目的のアルゴリズムを指定することができます。</span><span class="sxs-lookup"><span data-stu-id="177af-174">Calling `UseCryptographicAlgorithms` allows you to specify the desired algorithm from a predefined built-in list.</span></span> <span data-ttu-id="177af-175">アルゴリズムの実装について心配する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="177af-175">You don't need to worry about the implementation of the algorithm.</span></span> <span data-ttu-id="177af-176">上記のシナリオでは、データ保護システムは、Windows で実行されている場合は、AES の CNG 実装を使用しようとします。</span><span class="sxs-lookup"><span data-stu-id="177af-176">In the scenario above, the Data Protection system attempts to use the CNG implementation of AES if running on Windows.</span></span> <span data-ttu-id="177af-177">それ以外の場合、フォールバックをマネージ[System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes)クラス。</span><span class="sxs-lookup"><span data-stu-id="177af-177">Otherwise, it falls back to the managed [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) class.</span></span>

<span data-ttu-id="177af-178">呼び出しを使用して実装を手動で指定[UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms)します。</span><span class="sxs-lookup"><span data-stu-id="177af-178">You can manually specify an implementation via a call to [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).</span></span>

> [!TIP]
> <span data-ttu-id="177af-179">アルゴリズムを変更すると、既存のキーをキー リングには影響しません。</span><span class="sxs-lookup"><span data-stu-id="177af-179">Changing algorithms doesn't affect existing keys in the key ring.</span></span> <span data-ttu-id="177af-180">新しく生成されたキーのみに影響します。</span><span class="sxs-lookup"><span data-stu-id="177af-180">It only affects newly-generated keys.</span></span>

### <a name="specifying-custom-managed-algorithms"></a><span data-ttu-id="177af-181">管理対象のカスタム アルゴリズムを指定します。</span><span class="sxs-lookup"><span data-stu-id="177af-181">Specifying custom managed algorithms</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="177af-182">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="177af-182">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="177af-183">管理対象のカスタム アルゴリズムを指定するには、作成、 [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration)実装の種類を示すインスタンス。</span><span class="sxs-lookup"><span data-stu-id="177af-183">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) instance that points to the implementation types:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="177af-184">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="177af-184">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="177af-185">管理対象のカスタム アルゴリズムを指定するには、作成、 [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings)実装の種類を示すインスタンス。</span><span class="sxs-lookup"><span data-stu-id="177af-185">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) instance that points to the implementation types:</span></span>

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

---

<span data-ttu-id="177af-186">一般に、\*の Type プロパティは、具体的をポイントする必要があります (パラメーターなしコンス トラクターを公開) を使用してインスタンス化可能な実装の[SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm)と[KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm)できたとして、特殊なケースなどのいくつかの値のシステム`typeof(Aes)`利便性を考慮します。</span><span class="sxs-lookup"><span data-stu-id="177af-186">Generally the \*Type properties must point to concrete, instantiable (via a public parameterless ctor) implementations of [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) and [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), though the system special-cases some values like `typeof(Aes)` for convenience.</span></span>

> [!NOTE]
> <span data-ttu-id="177af-187">PKCS #7 埋め込みで CBC モードの暗号化をサポートする必要があり、SymmetricAlgorithm は ≥ 128 ビットのキーの長さと ≥ の 64 ビットのブロック サイズにあります。</span><span class="sxs-lookup"><span data-stu-id="177af-187">The SymmetricAlgorithm must have a key length of ≥ 128 bits and a block size of ≥ 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="177af-188">ダイジェストのサイズをいる必要があります、KeyedHashAlgorithm > = 128 ビット、およびハッシュ アルゴリズムのダイジェストの長さと同じ長さのキーをサポートする必要があります。</span><span class="sxs-lookup"><span data-stu-id="177af-188">The KeyedHashAlgorithm must have a digest size of >= 128 bits, and it must support keys of length equal to the hash algorithm's digest length.</span></span> <span data-ttu-id="177af-189">HMAC を必須としたときに、KeyedHashAlgorithm はありません。</span><span class="sxs-lookup"><span data-stu-id="177af-189">The KeyedHashAlgorithm isn't strictly required to be HMAC.</span></span>

### <a name="specifying-custom-windows-cng-algorithms"></a><span data-ttu-id="177af-190">カスタムの Windows CNG アルゴリズムを指定します。</span><span class="sxs-lookup"><span data-stu-id="177af-190">Specifying custom Windows CNG algorithms</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="177af-191">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="177af-191">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="177af-192">CBC モードの暗号化を使用して、HMAC の検証にカスタム Windows CNG アルゴリズムを指定するには、作成、 [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration)アルゴリズム情報を格納するインスタンス。</span><span class="sxs-lookup"><span data-stu-id="177af-192">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="177af-193">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="177af-193">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="177af-194">CBC モードの暗号化を使用して、HMAC の検証にカスタム Windows CNG アルゴリズムを指定するには、作成、 [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings)アルゴリズム情報を格納するインスタンス。</span><span class="sxs-lookup"><span data-stu-id="177af-194">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

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

---

> [!NOTE]
> <span data-ttu-id="177af-195">対称ブロック暗号アルゴリズム、キーの長さをいる必要があります > = 128 ビットのブロック サイズ > = 64 ビット、および PKCS #7 埋め込みで CBC モードの暗号化をサポートする必要があります。</span><span class="sxs-lookup"><span data-stu-id="177af-195">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of >= 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="177af-196">ハッシュ アルゴリズムのダイジェストのサイズをいる必要があります > = 128 ビットと、BCRYPT で開かれるをサポートする必要があります\_ALG\_処理\_HMAC\_フラグ フラグ。</span><span class="sxs-lookup"><span data-stu-id="177af-196">The hash algorithm must have a digest size of >= 128 bits and must support being opened with the BCRYPT\_ALG\_HANDLE\_HMAC\_FLAG flag.</span></span> <span data-ttu-id="177af-197">\*プロバイダーのプロパティは、指定したアルゴリズムの既定のプロバイダーを使用するには null に設定できます。</span><span class="sxs-lookup"><span data-stu-id="177af-197">The \*Provider properties can be set to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="177af-198">参照してください、 [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx)詳細についてはドキュメントです。</span><span class="sxs-lookup"><span data-stu-id="177af-198">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="177af-199">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="177af-199">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="177af-200">検証で Galois/カウンター モード暗号化を使用するカスタムの Windows CNG アルゴリズムを指定するには、作成、 [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration)アルゴリズム情報を格納するインスタンス。</span><span class="sxs-lookup"><span data-stu-id="177af-200">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="177af-201">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="177af-201">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="177af-202">検証で Galois/カウンター モード暗号化を使用するカスタムの Windows CNG アルゴリズムを指定するには、作成、 [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings)アルゴリズム情報を格納するインスタンス。</span><span class="sxs-lookup"><span data-stu-id="177af-202">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

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

---

> [!NOTE]
> <span data-ttu-id="177af-203">対称ブロック暗号アルゴリズム、キーの長さをいる必要があります > =、正確に 128 ビットのブロック サイズを 128 ビットと GCM の暗号化をサポートする必要があります。</span><span class="sxs-lookup"><span data-stu-id="177af-203">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of exactly 128 bits, and it must support GCM encryption.</span></span> <span data-ttu-id="177af-204">設定することができます、 [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider)プロパティを指定したアルゴリズムの既定のプロバイダーを使用する場合は null です。</span><span class="sxs-lookup"><span data-stu-id="177af-204">You can set the [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) property to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="177af-205">参照してください、 [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx)詳細についてはドキュメントです。</span><span class="sxs-lookup"><span data-stu-id="177af-205">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

### <a name="specifying-other-custom-algorithms"></a><span data-ttu-id="177af-206">その他のカスタム アルゴリズムを指定します。</span><span class="sxs-lookup"><span data-stu-id="177af-206">Specifying other custom algorithms</span></span>

<span data-ttu-id="177af-207">ファースト クラスの API として公開されている、データ保護システムはほぼすべての種類のアルゴリズムを指定することは拡張性を備えています。</span><span class="sxs-lookup"><span data-stu-id="177af-207">Though not exposed as a first-class API, the Data Protection system is extensible enough to allow specifying almost any kind of algorithm.</span></span> <span data-ttu-id="177af-208">たとえば、ハードウェア セキュリティ モジュール (HSM) 内に含まれるすべてのキーを保持し、core のカスタム実装の暗号化と復号化ルーチンを提供するも可能です。</span><span class="sxs-lookup"><span data-stu-id="177af-208">For example, it's possible to keep all keys contained within a Hardware Security Module (HSM) and to provide a custom implementation of the core encryption and decryption routines.</span></span> <span data-ttu-id="177af-209">参照してください[IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor)で[暗号化の拡張性をコア](xref:security/data-protection/extensibility/core-crypto)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="177af-209">See [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) in [Core cryptography extensibility](xref:security/data-protection/extensibility/core-crypto) for more information.</span></span>

## <a name="persisting-keys-when-hosting-in-a-docker-container"></a><span data-ttu-id="177af-210">Docker コンテナーでホストする場合、永続化キー</span><span class="sxs-lookup"><span data-stu-id="177af-210">Persisting keys when hosting in a Docker container</span></span>

<span data-ttu-id="177af-211">ホストする場合、 [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/)コンテナー、キーは、いずれかで保持する必要があります。</span><span class="sxs-lookup"><span data-stu-id="177af-211">When hosting in a [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) container, keys should be maintained in either:</span></span>

* <span data-ttu-id="177af-212">Docker ボリュームの共有ボリュームまたはホストにマウントされたボリュームなど、コンテナーの有効期間を超えて保持するフォルダー。</span><span class="sxs-lookup"><span data-stu-id="177af-212">A folder that's a Docker volume that persists beyond the container's lifetime, such as a shared volume or a host-mounted volume.</span></span>
* <span data-ttu-id="177af-213">外部プロバイダーなど[Azure Key Vault](https://azure.microsoft.com/services/key-vault/)または[Redis](https://redis.io/)します。</span><span class="sxs-lookup"><span data-stu-id="177af-213">An external provider, such as [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) or [Redis](https://redis.io/).</span></span>

## <a name="see-also"></a><span data-ttu-id="177af-214">関連項目</span><span class="sxs-lookup"><span data-stu-id="177af-214">See also</span></span>

* <xref:security/data-protection/configuration/non-di-scenarios>
* <xref:security/data-protection/configuration/machine-wide-policy>
* <xref:host-and-deploy/web-farm>
