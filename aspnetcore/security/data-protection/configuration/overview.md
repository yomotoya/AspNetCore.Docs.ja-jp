---
title: ASP.NET Core データ保護を構成します。
author: rick-anderson
description: ASP.NET Core でのデータ保護を構成する方法について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 07/17/2017
uid: security/data-protection/configuration/overview
ms.openlocfilehash: 0377fe9fbe5a1eeddb384443370751baa3c0ee43
ms.sourcegitcommit: 8bf4dff3069e62972c1b0839a93fb444e502afe7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/20/2018
ms.locfileid: "46482997"
---
# <a name="configure-aspnet-core-data-protection"></a>ASP.NET Core データ保護を構成します。

データ保護システムが初期化されると、適用[既定の設定](xref:security/data-protection/configuration/default-settings)運用環境に基づきます。 これらの設定は、1 台のコンピューターで実行されているアプリの一般的に適しています。 開発者は、既定の設定を変更する必要のあるケースがあります。

* 複数のコンピューターで、アプリの負荷が分散されます。
* コンプライアンス上の理由からです。

これらのシナリオについては、データ保護システムは、高度な構成 API を提供します。

> [!WARNING]
> 構成ファイルと同様に、データ保護キー リングする必要があります保護する適切なアクセス許可を使用します。 、保存時のキーを暗号化することもできますが、攻撃者を防ぐ新しいキーを作成するがこれです。 その結果、アプリのセキュリティに影響します。 データ保護で構成されている記憶域の場所のアクセスは、アプリ自体と同じように構成ファイルを保護するように制限があります。 たとえば、ディスク上のキー リングを格納する場合は、ファイル システム権限を使用します。 Id のみを確認します。 web アプリの実行に読み取り、書き込み、およびそのディレクトリへのアクセスを作成します。 Azure Table Storage を使用する場合、web アプリのみが読み取り、書き込み、またはテーブル ストアなどの新しいエントリを作成する機能に必要です。
>
> 拡張メソッド[AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection)を返します、 [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder)します。 `IDataProtectionBuilder` 連結できますデータ保護を構成するオプションの拡張メソッドを公開します。

::: moniker range=">= aspnetcore-2.1"

## <a name="protectkeyswithazurekeyvault"></a>ProtectKeysWithAzureKeyVault

キーを保管する[Azure Key Vault](https://azure.microsoft.com/services/key-vault/)、構成を使用してシステム[ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault)で、`Startup`クラス。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blobUriWithSasToken>"))
        .ProtectKeysWithAzureKeyVault("<keyIdentifier>", "<clientId>", "<clientSecret>");
}
```

設定キー リング記憶域の場所 (たとえば、 [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage))。 呼び出すために、場所を設定する必要があります`ProtectKeysWithAzureKeyVault`実装、 [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor)キー リング記憶域の場所を含む、データの自動保護設定を無効にします。 前の例では、Azure Blob Storage を使用して、キー リングを保持します。 詳細については、次を参照してください。[キー記憶域プロバイダー: Azure と Redis](xref:security/data-protection/implementation/key-storage-providers#azure-and-redis)します。 ローカルにキー リングを保存することもできます。 [PersistKeysToFileSystem](xref:security/data-protection/implementation/key-storage-providers#file-system)します。

`keyIdentifier`はキーの暗号化に使用されるキー コンテナーのキー識別子です (たとえば、 `https://contosokeyvault.vault.azure.net/keys/dataprotection/`)。

`ProtectKeysWithAzureKeyVault` オーバー ロードします。

* [ProtectKeysWithAzureKeyVault (IDataProtectionBuilder、KeyVaultClient、文字列)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_Microsoft_Azure_KeyVault_KeyVaultClient_System_String_)の使用を許可、 [KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient) key vault を使用するデータ保護システムを有効にします。
* [(IDataProtectionBuilder、文字列、文字列、X509Certificate2) ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_Security_Cryptography_X509Certificates_X509Certificate2_)の使用を許可、`ClientId`と[X509Certificate](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) key vault を使用するデータ保護システムを有効にします。
* [(IDataProtectionBuilder、文字列、String, String) ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_String_)の使用を許可、`ClientId`と`ClientSecret`key vault を使用するデータ保護システムを有効にします。

::: moniker-end

## <a name="persistkeystofilesystem"></a>PersistKeysToFileSystem

代わりに UNC 共有でのキーを格納する、 *%localappdata%* 既定の場所、構成を使用してシステム[PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

> [!WARNING]
> キーの永続性の場所を変更する場合、システムは DPAPI は、適切な暗号化メカニズムであるかどうかを認識していないため、保存時のキーを暗号化する自動的にします。

## <a name="protectkeyswith"></a>ProtectKeysWith\*

いずれかを呼び出すことによって、保存時のキーを保護するシステムを構成することができます、 [ProtectKeysWith\* ](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) Api を構成します。 UNC 共有にキーを格納し、特定の X.509 証明書での保存時に、そのキーを暗号化する次の例を検討してください。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

::: moniker range=">= aspnetcore-2.1"

ASP.NET Core 2.1 以降では、行うことができます、 [X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2)に[ProtectKeysWithCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithcertificate)ファイルから読み込まれた証明書など。

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

参照してください[キーの暗号化に](xref:security/data-protection/implementation/key-encryption-at-rest)詳細の例と、組み込みのキーの暗号化メカニズムについて説明します。

::: moniker range=">= aspnetcore-2.1"

## <a name="unprotectkeyswithanycertificate"></a>UnprotectKeysWithAnyCertificate

ASP.NET Core 2.1 以降では、証明書の回転し、配列を使用して保存時のキーを復号化[X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2)で証明書[UnprotectKeysWithAnyCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.unprotectkeyswithanycertificate):

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

## <a name="setdefaultkeylifetime"></a>SetDefaultKeyLifetime

90 日間、既定ではなくキーの有効期間は 14 日間を使用するシステムを構成するには使用[SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

## <a name="setapplicationname"></a>SetApplicationName

既定では、データ保護システムは、同じ物理キーのリポジトリを共有している場合でも、別のアプリを分離します。 これは、アプリが互いの保護されたペイロードを理解することを防ぎます。 2 つのアプリ間で保護されたペイロードを共有する使用[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname)アプリごとに同じ値を持つ。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("shared app name");
}
```

## <a name="disableautomatickeygeneration"></a>DisableAutomaticKeyGeneration

自動的に有効期限を実現するキー (新しいキーを作成する) を展開するアプリをたくないシナリオがあります。 この 1 つの例には、アプリでプライマリ アプリのみがキー管理の問題に責任を負います、セカンダリのアプリは、キー リングの読み取り専用ビューを単純にあるプライマリ/セカンダリの関係を設定する可能性があります。 使用してシステムを構成することによって、読み取り専用としてキー リングを処理するセカンダリのアプリを構成することができます[DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

## <a name="per-application-isolation"></a>アプリケーションごとの分離

データ保護システムを指定するには、ASP.NET Core のホストによって、自動的に分離され、そのアプリから、別場合でも、それらのアプリが同じワーカー プロセス アカウントで実行されていて、同じマスター キー生成情報を使用しています。 これは、System.Web から IsolateApps 修飾子に少し似ています **\<machineKey >** 要素。

したがって、一意のテナントとして、ローカル コンピューター上の各アプリを考慮して分離メカニズムの動作、 [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector)ルートの指定したアプリに自動的に識別子としてアプリ ID が含まれています。 アプリの一意の ID では、2 つの場所のいずれかのです。

1. アプリが IIS でホストされる、一意の識別子は、アプリの構成パスです。 Web ファーム環境でアプリを展開する場合は、IIS 環境は web ファーム内のすべてのコンピューター間で同様に構成されていると仮定するとこの値が安定したなります。

2. アプリは、IIS でホストされていない、一意の識別子は、アプリの物理パスです。

一意の識別子がリセット後も存続するように設計&mdash;の個々 のアプリと、マシン自体の両方。

この分離メカニズムでは、アプリが悪意のあるいないことを前提としています。 悪意のあるアプリを常に同じワーカー プロセス アカウントで実行されている他のアプリに影響する可能性です。 アプリが相互に信頼されていない共有ホスティング環境、ホスティング プロバイダーは、アプリのキーのリポジトリを基になる分離を含む、アプリ間での OS レベルの分離を講じる必要があります。

データ保護システムを ASP.NET Core のホストによって提供されないかどうか (経由でインスタンス化する場合など、`DataProtectionProvider`具象型) アプリの分離が既定で無効になっています。 適切な提供される限り、アプリの分離が無効になっているときに、同じキー マテリアルに支えすべてのアプリではペイロードを共有できます[目的で](xref:security/data-protection/consumer-apis/purpose-strings)します。 この環境でアプリの分離を提供するには、呼び出し、 [SetApplicationName](#setapplicationname)メソッドを構成オブジェクトし、アプリごとに一意の名前を指定します。

## <a name="changing-algorithms-with-usecryptographicalgorithms"></a>UseCryptographicAlgorithms でアルゴリズムを変更します。

データ保護スタックを使用して、新しく生成されたキーで使用される既定のアルゴリズムを変更できます。 これを行う最も簡単な方法を呼び出すことです。 [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms)構成コールバックから。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptorConfiguration()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

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

既定の EncryptionAlgorithm は AES 256-CBC で、既定値 ValidationAlgorithm は HMACSHA256 です。 経由でシステム管理者によって、既定のポリシーを設定することができます、[コンピューター レベル ポリシー](xref:security/data-protection/configuration/machine-wide-policy)を明示的に呼び出すが、`UseCryptographicAlgorithms`既定ポリシーによって上書きされます。

呼び出す`UseCryptographicAlgorithms`定義済みの組み込みの一覧から目的のアルゴリズムを指定することができます。 アルゴリズムの実装について心配する必要はありません。 上記のシナリオでは、データ保護システムは、Windows で実行されている場合は、AES の CNG 実装を使用しようとします。 それ以外の場合、フォールバックをマネージ[System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes)クラス。

呼び出しを使用して実装を手動で指定[UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms)します。

> [!TIP]
> アルゴリズムを変更すると、既存のキーをキー リングには影響しません。 新しく生成されたキーのみに影響します。

### <a name="specifying-custom-managed-algorithms"></a>管理対象のカスタム アルゴリズムを指定します。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

管理対象のカスタム アルゴリズムを指定するには、作成、 [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration)実装の種類を示すインスタンス。

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

管理対象のカスタム アルゴリズムを指定するには、作成、 [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings)実装の種類を示すインスタンス。

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

一般に、\*の Type プロパティは、具体的をポイントする必要があります (パラメーターなしコンス トラクターを公開) を使用してインスタンス化可能な実装の[SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm)と[KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm)できたとして、特殊なケースなどのいくつかの値のシステム`typeof(Aes)`利便性を考慮します。

> [!NOTE]
> PKCS #7 埋め込みで CBC モードの暗号化をサポートする必要があり、SymmetricAlgorithm は ≥ 128 ビットのキーの長さと ≥ の 64 ビットのブロック サイズにあります。 ダイジェストのサイズをいる必要があります、KeyedHashAlgorithm > = 128 ビット、およびハッシュ アルゴリズムのダイジェストの長さと同じ長さのキーをサポートする必要があります。 HMAC を必須としたときに、KeyedHashAlgorithm はありません。

### <a name="specifying-custom-windows-cng-algorithms"></a>カスタムの Windows CNG アルゴリズムを指定します。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

CBC モードの暗号化を使用して、HMAC の検証にカスタム Windows CNG アルゴリズムを指定するには、作成、 [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration)アルゴリズム情報を格納するインスタンス。

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

CBC モードの暗号化を使用して、HMAC の検証にカスタム Windows CNG アルゴリズムを指定するには、作成、 [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings)アルゴリズム情報を格納するインスタンス。

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
> 対称ブロック暗号アルゴリズム、キーの長さをいる必要があります > = 128 ビットのブロック サイズ > = 64 ビット、および PKCS #7 埋め込みで CBC モードの暗号化をサポートする必要があります。 ハッシュ アルゴリズムのダイジェストのサイズをいる必要があります > = 128 ビットと、BCRYPT で開かれるをサポートする必要があります\_ALG\_処理\_HMAC\_フラグ フラグ。 \*プロバイダーのプロパティは、指定したアルゴリズムの既定のプロバイダーを使用するには null に設定できます。 参照してください、 [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx)詳細についてはドキュメントです。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

検証で Galois/カウンター モード暗号化を使用するカスタムの Windows CNG アルゴリズムを指定するには、作成、 [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration)アルゴリズム情報を格納するインスタンス。

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

検証で Galois/カウンター モード暗号化を使用するカスタムの Windows CNG アルゴリズムを指定するには、作成、 [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings)アルゴリズム情報を格納するインスタンス。

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
> 対称ブロック暗号アルゴリズム、キーの長さをいる必要があります > =、正確に 128 ビットのブロック サイズを 128 ビットと GCM の暗号化をサポートする必要があります。 設定することができます、 [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider)プロパティを指定したアルゴリズムの既定のプロバイダーを使用する場合は null です。 参照してください、 [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx)詳細についてはドキュメントです。

### <a name="specifying-other-custom-algorithms"></a>その他のカスタム アルゴリズムを指定します。

ファースト クラスの API として公開されている、データ保護システムはほぼすべての種類のアルゴリズムを指定することは拡張性を備えています。 たとえば、ハードウェア セキュリティ モジュール (HSM) 内に含まれるすべてのキーを保持し、core のカスタム実装の暗号化と復号化ルーチンを提供するも可能です。 参照してください[IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor)で[暗号化の拡張性をコア](xref:security/data-protection/extensibility/core-crypto)詳細についてはします。

## <a name="persisting-keys-when-hosting-in-a-docker-container"></a>Docker コンテナーでホストする場合、永続化キー

ホストする場合、 [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/)コンテナー、キーは、いずれかで保持する必要があります。

* Docker ボリュームの共有ボリュームまたはホストにマウントされたボリュームなど、コンテナーの有効期間を超えて保持するフォルダー。
* 外部プロバイダーなど[Azure Key Vault](https://azure.microsoft.com/services/key-vault/)または[Redis](https://redis.io/)します。

## <a name="see-also"></a>関連項目

* <xref:security/data-protection/configuration/non-di-scenarios>
* <xref:security/data-protection/configuration/machine-wide-policy>
* <xref:host-and-deploy/web-farm>
