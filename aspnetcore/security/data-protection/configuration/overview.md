---
title: "データ保護を構成します。"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 0e4881a3-a94d-4e35-9c1c-f025d65dcff0
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/overview
ms.openlocfilehash: 9361dcec89a0f35067181523cc56637d629614ff
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/28/2017
---
# <a name="configuring-data-protection"></a>データ保護を構成します。

<a name=data-protection-configuring></a>

いくつか適用される、データ保護システムが初期化されたときに[既定の設定](default-settings.md#data-protection-default-settings)運用環境に基づきます。 これらの設定は通常、1 台のコンピューターで実行されるアプリケーションに適しています。 開発者はこれらを変更する必要のある場合もあります (おそらく複数のコンピューターまたはコンプライアンス上の理由から、アプリケーションが分散しているので)、し、このようなシナリオは、データ保護システムは、豊富な構成 API を提供しています。

<a name=data-protection-configuration-callback></a>

拡張メソッドを返す、IDataProtectionBuilder AddDataProtection 自体を連結できます。 さまざまなデータ保護を構成するオプションの拡張メソッドを公開します。 たとえば、キーを格納する %localappdata% (既定値) の代わりに UNC 共有は、次のようなシステムを構成します。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

>[!WARNING]
> キーの永続性の場所を変更する場合、システムは自動的に行われなくキーの暗号化休息 DPAPI は、適切な暗号化メカニズムであるかどうかを認識していないためです。

<a name=configuring-x509-certificate></a>

呼び出して、ProtectKeysWith のいずれかの休息キーを保護するシステムを構成する\*Api を構成します。 次の例では UNC 共有でキーを格納し、特定の X.509 証明書の残りの部分でこれらのキーの暗号化を検討してください。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

参照してください[静止したキーの暗号化](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest)例については、組み込みのキーの暗号化メカニズムの詳細についてはします。

90 日間ではなく既定キーの有効期間は 14 日間を使用するようシステムを構成するのには、次の例を考慮してください。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

既定では、データ保護システムは、同じ物理キーのリポジトリを共有している場合でも、別のアプリケーションを分離します。 これにより、互いの保護されているペイロードを理解するアプリケーション。 2 つの異なるアプリケーション間で保護されているペイロードを共有するようにアプリケーションの両方に同じアプリケーションの名前に渡すことシステムの構成は次の例。

<a name=data-protection-code-sample-application-name></a>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("my application");
}
```

<a name=data-protection-configuring-disable-automatic-key-generation></a>

最後に、アプリケーションの有効期限が間近、キーを自動的にロールをしないシナリオがある可能性があります。 この 1 つの例には、アプリケーションのプライマリ/セカンダリは、関係でのみ、アプリケーションのプライマリ キー管理に関連する要素を担当するあり、すべてのセカンダリ アプリケーションだけがある、キーのリングの読み取り専用ビューを設定する可能性があります。 次に示すように、システムを構成することによって、読み取り専用としてキー リングを処理する、セカンダリのアプリケーションを構成できます。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

<a name=data-protection-configuration-per-app-isolation></a>

## <a name="per-application-isolation"></a>アプリケーションごとの分離

ASP.NET Core ホストによって、データ保護システムが指定した場合、それらのアプリケーションが同じワーカー プロセス アカウントで実行されていると、同じマスター キー生成情報を使用している場合でも、別のアプリケーションを自動的に分離はします。 これは、System.Web から IsolateApps 修飾子に似た<machineKey>要素。

独自のテナントとしてローカル コンピューター上の各アプリケーションの検討から分離メカニズムのしくみ、したがって IDataProtector 自動的に、特定のアプリケーションのルートにはが含まれますアプリケーション ID には識別子として。 アプリケーションの一意の ID は、2 つの場所から取得されます。

1. アプリケーションが IIS でホストされている場合は、アプリケーションの構成パスは一意識別子です。 アプリケーションがファーム環境に展開されている場合は、IIS 環境は、ファーム内のすべてのマシン間で同様に構成されていると仮定するとこの値が安定したあります。

2. アプリケーションが IIS でホストされていない場合、一意識別子は、アプリケーションの物理パスです。

リセットの個々 のアプリケーション、および、マシン自体からの復旧は、一意の識別子は設計されています。

この分離メカニズムは、アプリケーションが悪意のあるいないことを前提とします。 悪意のあるアプリケーションは、同じワーカー プロセス アカウントで実行されているその他のアプリケーションに常に影響を与えることができます。 共有ホスティング環境でアプリケーションが相互に信頼されていない、ホスティング プロバイダーは、キーのリポジトリの基になるアプリケーションの分離を含む、アプリケーション間で OS レベルの分離を講じる必要があります。

データ保護システムによって提供されていない ASP.NET Core ホスト場合 (たとえば、開発者は、それをインスタンス化自分自身 DataProtectionProvider 具象型を使用して)、既定では、アプリケーションの分離が無効になり、すべてのアプリケーションが同じキーの更新によってサポート適切な目的を提供する限り、マテリアルはペイロードを共有できます。 この環境でアプリケーションの分離を提供するには、構成オブジェクトに対して SetApplicationName メソッドを呼び出すを参照してください、[コード サンプル](#data-protection-code-sample-application-name)上。

<a name=data-protection-changing-algorithms></a>

## <a name="changing-algorithms"></a>アルゴリズムを変更します。

データ保護スタックは、新しく生成されたキーで使用される既定のアルゴリズムを変更できます。 これを行う最も簡単な方法はからを呼び出す UseCryptographicAlgorithms 構成コールバックとしてでは、次の例です。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(new AuthenticatedEncryptorConfiguration()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(new AuthenticatedEncryptionSettings()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

---

既定 EncryptionAlgorithm ValidationAlgorithm は AES 256-CBC、HMACSHA256、それぞれします。 使用して、システム管理者によって既定のポリシーを設定することができます[マシン全体のポリシー](machine-wide-policy.md)UseCryptographicAlgorithms を明示的に呼び出すには、既定のポリシーがよりも優先されますが、します。

UseCryptographicAlgorithms を呼び出すことにより、開発者は (あらかじめ定義された組み込みの一覧から)、必要なアルゴリズムを指定して、開発者は、アルゴリズムの実装について心配する必要はありません。 たとえば、データ保護システム上のシナリオでを試みます Windows で実行されている場合は、AES の CNG 実装を使用して、それ以外の場合に切り替えられますマネージ System.Security.Cryptography.Aes クラスにします。

開発者は、手動で指定できます、実装でスライド ショーとして UseCustomCryptographicAlgorithms への呼び出しを使用して必要な場合は、次の例です。

>[!TIP]
> アルゴリズムを変更しても、キー リング内の既存のキーには影響しません。 新しく生成されたキーにのみ影響します。

<a name=data-protection-changing-algorithms-custom-managed></a>

### <a name="specifying-custom-managed-algorithms"></a>カスタムの管理されているアルゴリズムを指定します。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

カスタム マネージ アルゴリズムを指定するには、実装の種類を示す ManagedAuthenticatedEncryptorConfiguration インスタンスを作成します。

```csharp
serviceCollection.AddDataProtection()
    .UseCustomCryptographicAlgorithms(new ManagedAuthenticatedEncryptorConfiguration()
    {
        // a type that subclasses SymmetricAlgorithm
        EncryptionAlgorithmType = typeof(Aes),

        // specified in bits
        EncryptionAlgorithmKeySize = 256,

        // a type that subclasses KeyedHashAlgorithm
        ValidationAlgorithmType = typeof(HMACSHA256)
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

カスタム マネージ アルゴリズムを指定するには、実装の種類を示す ManagedAuthenticatedEncryptionSettings インスタンスを作成します。

```csharp
serviceCollection.AddDataProtection()
    .UseCustomCryptographicAlgorithms(new ManagedAuthenticatedEncryptionSettings()
    {
        // a type that subclasses SymmetricAlgorithm
        EncryptionAlgorithmType = typeof(Aes),

        // specified in bits
        EncryptionAlgorithmKeySize = 256,

        // a type that subclasses KeyedHashAlgorithm
        ValidationAlgorithmType = typeof(HMACSHA256)
    });
```

---

一般に、\*型のプロパティは、具象型を指す必要があります (パラメーターなしのパブリック コンストラクタ) 経由でインスタンス化可能な実装対称アルゴリズムの KeyedHashAlgorithm、いくつかの値などのシステムの特別なケースが便宜上 typeof(Aes).

> [!NOTE]
> PKCS #7 パディング CBC モードの暗号化をサポートする必要があり、対称アルゴリズムは ≥ 128 ビットのキーの長さと ≥ の 64 ビットのブロック サイズにあります。 ダイジェストのサイズをいる必要があります、KeyedHashAlgorithm > = 128 ビット、およびハッシュ アルゴリズムのダイジェストの長さと同じ長さのキーをサポートする必要があります。 KeyedHashAlgorithm は HMAC を必須ではありません。

<a name=data-protection-changing-algorithms-cng></a>

### <a name="specifying-custom-windows-cng-algorithms"></a>カスタムの Windows CNG アルゴリズムを指定します。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

CBC モードの暗号化 + HMAC 検証を使用して、カスタム Windows CNG アルゴリズムを指定するには、アルゴリズムの情報を含む CngCbcAuthenticatedEncryptorConfiguration インスタンスを作成します。

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(new CngCbcAuthenticatedEncryptorConfiguration()
    {
        // passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // specified in bits
        EncryptionAlgorithmKeySize = 256,

        // passed to BCryptOpenAlgorithmProvider
        HashAlgorithm = "SHA256",
        HashAlgorithmProvider = null
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

CBC モードの暗号化 + HMAC 検証を使用して、カスタム Windows CNG アルゴリズムを指定するには、アルゴリズムの情報を含む CngCbcAuthenticatedEncryptionSettings インスタンスを作成します。

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(new CngCbcAuthenticatedEncryptionSettings()
    {
        // passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // specified in bits
        EncryptionAlgorithmKeySize = 256,

        // passed to BCryptOpenAlgorithmProvider
        HashAlgorithm = "SHA256",
        HashAlgorithmProvider = null
    });
```

---

> [!NOTE]
> 対称ブロック暗号アルゴリズムが必要 ≥ 128 ビットのキーの長さと ≥ の 64 ビットのブロック サイズを PKCS #7 パディング CBC モードの暗号化をサポートする必要があります。 ハッシュ アルゴリズムのダイジェストのサイズが必要 > = 128 ビットと BCRYPT_ALG_HANDLE_HMAC_FLAG フラグで開かれるをサポートする必要があります。 \*プロバイダーのプロパティは、指定したアルゴリズムの既定のプロバイダーを使用するのには、null に設定できます。 参照してください、[アンロード](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx)詳細についてはドキュメントです。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Galois/カウンター モードの暗号化と検証を使用して、カスタム Windows CNG アルゴリズムを指定するには、アルゴリズム情報を含む CngGcmAuthenticatedEncryptorConfiguration インスタンスを作成します。

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(new CngGcmAuthenticatedEncryptorConfiguration()
    {
        // passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // specified in bits
        EncryptionAlgorithmKeySize = 256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Galois/カウンター モードの暗号化と検証を使用して、カスタム Windows CNG アルゴリズムを指定するには、アルゴリズム情報を含む CngGcmAuthenticatedEncryptionSettings インスタンスを作成します。

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(new CngGcmAuthenticatedEncryptionSettings()
    {
        // passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // specified in bits
        EncryptionAlgorithmKeySize = 256
    });
```

---

> [!NOTE]
> 対称ブロック暗号アルゴリズムは ≥ 128 ビットのキーの長さと正確に 128 ビットのブロック サイズが必要し、GCM 暗号化をサポートする必要があります。 EncryptionAlgorithmProvider プロパティは、指定したアルゴリズムの既定のプロバイダーを使用する場合は null に設定できます。 参照してください、[アンロード](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx)詳細についてはドキュメントです。

### <a name="specifying-other-custom-algorithms"></a>その他のカスタム アルゴリズムを指定します。

ファースト クラスの API として公開されていない、データ保護システムはほとんどのアルゴリズムの種類を指定することを許可するのに十分な拡張可能です。 たとえば、HSM 内で格納されているすべてのキーを保持し、実装を提供する、カスタムのコアの暗号化と暗号化解除ルーチンも可能です。 詳細については、コア暗号化機能拡張セクションで IAuthenticatedEncryptorConfiguration を参照してください。

### <a name="see-also"></a>関連項目

* [DI に対応しないシナリオ](non-di-scenarios.md)
* [コンピューター全体のポリシー](machine-wide-policy.md)
