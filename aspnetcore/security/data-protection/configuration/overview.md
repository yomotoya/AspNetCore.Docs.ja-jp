---
title: "ASP.NET Core でのデータ保護の構成"
author: rick-anderson
description: "ASP.NET Core でのデータ保護を構成する方法を説明します。"
keywords: "ASP.NET Core、データ保護、構成"
ms.author: riande
manager: wpickett
ms.date: 07/17/2017
ms.topic: article
ms.assetid: 0e4881a3-a94d-4e35-9c1c-f025d65dcff0
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/overview
ms.openlocfilehash: 4713c2bed04af784e74586daa10ec847262a1345
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="configuring-data-protection-in-aspnet-core"></a>ASP.NET Core でのデータ保護の構成

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

データ保護システムが初期化されたときに適用される[既定の設定](xref:security/data-protection/configuration/default-settings)運用環境に基づきます。 これらの設定に適した一般的に 1 台のコンピューターで実行されるアプリです。 開発者は複数のコンピューターまたはコンプライアンス上の理由から、アプリが分散しているので、おそらく、既定の設定を変更する必要のあるケースがあります。 このようなシナリオは、データ保護システムは、豊富な構成 API を提供します。

拡張メソッドがある[AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection)を返す、 [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder)です。 `IDataProtectionBuilder`連結できます。 データ保護を構成するオプションの拡張メソッドを公開します。

## <a name="persistkeystofilesystem"></a>PersistKeysToFileSystem

代わりに UNC 共有にキーを格納する、 *%localappdata%*既定の場所、構成を使用してシステム[PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

> [!WARNING]
> キーの永続性の場所を変更すると、システム不要になったによって自動的に暗号化キー、残りの部分を DPAPI は、適切な暗号化メカニズムであるかどうかを認識していないためです。

## <a name="protectkeyswith"></a>ProtectKeysWith\*

いずれかを呼び出すことによって残りの部分でキーを保護するシステムを構成することができます、 [ProtectKeysWith\* ](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) Api を構成します。 UNC 共有にキーを格納し、特定の X.509 証明書の残りの部分では、そのキーを暗号化する、次の例を考えてみます。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

参照してください[キー時の暗号化](xref:security/data-protection/implementation/key-encryption-at-rest)詳細の例と組み込みのキーの暗号化メカニズムについて説明します。

## <a name="setdefaultkeylifetime"></a>SetDefaultKeyLifetime

90 日間に、既定ではなくキーの有効期間は 14 日間を使用するようシステムを構成するのには、使用[SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

## <a name="setapplicationname"></a>SetApplicationName

既定では、データ保護システムは、同じ物理キーのリポジトリを共有している場合でも、別のアプリを分離します。 これは、アプリが互いの保護されているペイロードを理解することを防ぎます。 2 つのアプリ間で保護されているペイロードを共有するには、使用[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname)各アプリに対して同じ値を持つ。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("shared app name");
}
```

## <a name="disableautomatickeygeneration"></a>DisableAutomaticKeyGeneration

有効期限が間近、(新しいキーを作成) キーを自動的にロールにアプリをたくないシナリオがあります。 この 1 つの例は、プライマリ/セカンダリ関係、ここで、プライマリのアプリのみがキーの管理に関する注意事項を担当して、セカンダリ アプリだけがある、キーのリングの読み取り専用ビューを設定アプリにする可能性があります。 セカンダリ アプリを使用してシステムを構成することによって、読み取り専用としてキー リングを処理するように構成できます[DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

## <a name="per-application-isolation"></a>アプリケーションごとの分離

データ保護システムは、ASP.NET Core ホストによって提供される、ときに自動的に分離され、そのから、別のアプリの場合でも、それらのアプリが同じワーカー プロセス アカウントで実行されていて、同じマスター キー生成情報を使用しています。 これは、System.Web から IsolateApps 修飾子に似た **\<machineKey >**要素。

したがって、独自のテナントとしてローカル コンピューターには、各アプリを考慮して分離メカニズムの動作、 [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector)ルートとする指定したアプリには、アプリ ID は識別子として自動的が含まれています。 アプリの一意の ID は 2 つの場所から取得します。

1. アプリが IIS でホストされている場合は、アプリの構成パスは一意識別子です。 アプリは、web ファーム環境で展開が場合、は、IIS 環境は、web ファーム内のすべてのマシン間で同様に構成されていると仮定するとこの値が安定したあります。

2. 場合は、アプリは、IIS でホストされていない、一意の識別子は、アプリの物理パスです。

一意識別子がリセットで存続するように設計&mdash;個々 のアプリの両方に対してコンピューター自体とします。

この分離メカニズムでは、アプリが悪意のあるになっていないことを前提としています。 悪意のあるアプリは、同じワーカー プロセス アカウントで実行されている他のアプリに常に影響を与えることができます。 共有ホスティング環境でアプリが相互に信頼された、ホスティング プロバイダーは、アプリのキーのリポジトリを基になるを分離することを含め、アプリ間で OS レベルの分離を講じる必要があります。

ASP.NET Core ホストによって、データ保護システムが提供されないかどうか (たとえば、使用してそれをインスタンス化する場合、`DataProtectionProvider`具象型) アプリの分離が既定で無効になっています。 適切な提供する限り、アプリの分離を無効にすると、同じキー生成情報によってサポートすべてのアプリケーションではペイロードを共有できます[目的](xref:security/data-protection/consumer-apis/purpose-strings)です。 この環境でアプリの分離を提供するには、呼び出し、 [SetApplicationName](#setapplicationname)構成上のメソッド オブジェクトを各アプリに対して一意の名前を指定します。

## <a name="changing-algorithms-with-usecryptographicalgorithms"></a>UseCryptographicAlgorithms でアルゴリズムを変更します。

データ保護スタックでは、新しく生成されたキーで使用される既定のアルゴリズムを変更することができます。 これを行う最も簡単な方法を呼び出すことです。 [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms)構成コールバックから。

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

既定の EncryptionAlgorithm は AES 256-CBC で、既定値 ValidationAlgorithm は HMACSHA256 です。 経由でシステム管理者によって既定のポリシーを設定することができます、[コンピューター レベル ポリシー](xref:security/data-protection/configuration/machine-wide-policy)を明示的に呼び出す`UseCryptographicAlgorithms`既定のポリシーをオーバーライドします。

呼び出す`UseCryptographicAlgorithms`事前定義された組み込みの一覧から目的のアルゴリズムを指定することができます。 アルゴリズムの実装について心配する必要はありません。 上記のシナリオでは、データ保護システムは Windows で実行されている場合は、AES の CNG 実装を使用しようとします。 それ以外の場合にフォールバック マネージ[System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes)クラスです。

呼び出しを使用して、実装を手動で指定することができます[UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms)です。

> [!TIP]
> アルゴリズムを変更すると、キー リング内の既存のキーは影響しません。 新しく生成されたキーにのみ影響します。

### <a name="specifying-custom-managed-algorithms"></a>カスタムの管理されているアルゴリズムを指定します。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

カスタムの管理されているアルゴリズムを指定するには、作成、 [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration)実装の種類を示すインスタンス。

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

カスタムの管理されているアルゴリズムを指定するには、作成、 [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings)実装の種類を示すインスタンス。

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

一般に、\*型のプロパティは、具象型を指す必要があります (パラメーターなしのパブリック コンストラクタ) 経由でインスタンス化可能な実装の[対称アルゴリズム](/dotnet/api/system.security.cryptography.symmetricalgorithm)と[KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm)が、特殊なケースなどのいくつかの値のシステム`typeof(Aes)`利便性を考慮します。

> [!NOTE]
> `SymmetricAlgorithm`キーの長さが必要 > = 128 ビット単位のブロック サイズ > 64 ビットの場合は、以下が PKCS #7 埋め込み CBC モードの暗号化をサポートする必要があります。 `KeyedHashAlgorithm`のダイジェストのサイズが必要 > = 128 ビット、およびハッシュ アルゴリズムのダイジェストの長さと同じ長さのキーをサポートする必要があります。 `KeyedHashAlgorithm` HMAC を厳密には必要はありません。
> PKCS #7 パディング CBC モードの暗号化をサポートする必要があり、対称アルゴリズムは ≥ 128 ビットのキーの長さと ≥ の 64 ビットのブロック サイズにあります。 ダイジェストのサイズをいる必要があります、KeyedHashAlgorithm > = 128 ビット、およびハッシュ アルゴリズムのダイジェストの長さと同じ長さのキーをサポートする必要があります。 KeyedHashAlgorithm は HMAC を必須ではありません。

### <a name="specifying-custom-windows-cng-algorithms"></a>カスタムの Windows CNG アルゴリズムを指定します。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

HMAC 検証と CBC モードの暗号化を使用して、カスタム Windows CNG アルゴリズムを指定するには、作成、 [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration)アルゴリズム情報を含むインスタンス。

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

HMAC 検証と CBC モードの暗号化を使用して、カスタム Windows CNG アルゴリズムを指定するには、作成、 [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings)アルゴリズム情報を含むインスタンス。

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
> 対称ブロック暗号アルゴリズム、キーの長さを持つ必要があります > = 128 ビット単位のブロック サイズ > の 64 ビットを = し、PKCS #7 パディング CBC モードの暗号化をサポートする必要があります。 ハッシュ アルゴリズムのダイジェストのサイズが必要 > = 128 ビットと、BCRYPT で開かれるをサポートする必要があります\_ALG\_処理\_HMAC\_フラグ フラグ。 \*プロバイダーのプロパティは、指定したアルゴリズムの既定のプロバイダーを使用するのには、null に設定できます。 参照してください、[アンロード](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx)詳細についてはドキュメントです。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

検証で Galois/カウンター モードの暗号化を使用して、カスタム Windows CNG アルゴリズムを指定するには、作成、 [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration)アルゴリズム情報を含むインスタンス。

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

検証で Galois/カウンター モードの暗号化を使用して、カスタム Windows CNG アルゴリズムを指定するには、作成、 [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings)アルゴリズム情報を含むインスタンス。

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
> 対称ブロック暗号アルゴリズム、キーの長さを持つ必要があります > = 128 ビット、正確に 128 ビットのブロック サイズと GCM 暗号化をサポートする必要があります。 設定することができます、 [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider)プロパティを指定したアルゴリズムの既定のプロバイダーを使用する場合は null です。 参照してください、[アンロード](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx)詳細についてはドキュメントです。

### <a name="specifying-other-custom-algorithms"></a>その他のカスタム アルゴリズムを指定します。

ファースト クラスの API として公開されていない、データ保護システムはほとんどのアルゴリズムの種類を指定することを許可するのに十分な拡張可能です。 たとえば、ハードウェア セキュリティ モジュール (HSM) 内に含まれるすべてのキーを保持し、実装を提供する、カスタムのコアの暗号化と暗号化解除ルーチンも可能です。 参照してください[IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor)で[暗号化の拡張性をコア](xref:security/data-protection/extensibility/core-crypto)詳細についてはします。

## <a name="persisting-keys-when-hosting-in-a-docker-container"></a>Docker コンテナーでホストする場合、永続化キー

ホストする場合、 [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/)コンテナー、いずれかでキーを保持する必要があります。

* フォルダーの共有ボリュームまたはホストにマウントされたボリュームなど、コンテナーの有効期間を超えて保持 Docker ボリュームです。
* 外部プロバイダーなど[Azure Key Vault](https://azure.microsoft.com/services/key-vault/)または[Redis](https://redis.io/)です。

## <a name="see-also"></a>関連項目

* [DI に対応しないシナリオ](xref:security/data-protection/configuration/non-di-scenarios)
* [コンピューター全体のポリシー](xref:security/data-protection/configuration/machine-wide-policy)
