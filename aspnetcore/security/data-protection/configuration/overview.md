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
ms.openlocfilehash: d35e0e806999ffd2e0f8f82e0adfc940ea2b503d
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/13/2017
---
# <a name="configuring-data-protection"></a><span data-ttu-id="01404-103">データ保護を構成します。</span><span class="sxs-lookup"><span data-stu-id="01404-103">Configuring data protection</span></span>

<a name="data-protection-configuring"></a>

<span data-ttu-id="01404-104">いくつか適用される、データ保護システムが初期化されたときに[既定の設定](default-settings.md#data-protection-default-settings)運用環境に基づきます。</span><span class="sxs-lookup"><span data-stu-id="01404-104">When the data protection system is initialized it applies some [default settings](default-settings.md#data-protection-default-settings) based on the operational environment.</span></span> <span data-ttu-id="01404-105">これらの設定は通常、1 台のコンピューターで実行されるアプリケーションに適しています。</span><span class="sxs-lookup"><span data-stu-id="01404-105">These settings are generally good for applications running on a single machine.</span></span> <span data-ttu-id="01404-106">開発者はこれらを変更する必要のある場合もあります (おそらく複数のコンピューターまたはコンプライアンス上の理由から、アプリケーションが分散しているので)、し、このようなシナリオは、データ保護システムは、豊富な構成 API を提供しています。</span><span class="sxs-lookup"><span data-stu-id="01404-106">There are some cases where a developer may want to change these (perhaps because their application is spread across multiple machines or for compliance reasons), and for these scenarios the data protection system offers a rich configuration API.</span></span>

<a name="data-protection-configuration-callback"></a>

<span data-ttu-id="01404-107">拡張メソッドを返す、IDataProtectionBuilder AddDataProtection 自体を連結できます。 さまざまなデータ保護を構成するオプションの拡張メソッドを公開します。</span><span class="sxs-lookup"><span data-stu-id="01404-107">There is an extension method AddDataProtection which returns an IDataProtectionBuilder which itself exposes extension methods that you can chain together to configure various data protection options.</span></span> <span data-ttu-id="01404-108">たとえば、キーを格納する %localappdata% (既定値) の代わりに UNC 共有は、次のようなシステムを構成します。</span><span class="sxs-lookup"><span data-stu-id="01404-108">For instance, to store keys at a UNC share instead of %LOCALAPPDATA% (the default), configure the system as follows:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

>[!WARNING]
> <span data-ttu-id="01404-109">キーの永続性の場所を変更する場合、システムは自動的に行われなくキーの暗号化休息 DPAPI は、適切な暗号化メカニズムであるかどうかを認識していないためです。</span><span class="sxs-lookup"><span data-stu-id="01404-109">If you change the key persistence location, the system will no longer automatically encrypt keys at rest since it doesn't know whether DPAPI is an appropriate encryption mechanism.</span></span>

<a name="configuring-x509-certificate"></a>

<span data-ttu-id="01404-110">呼び出して、ProtectKeysWith のいずれかの休息キーを保護するシステムを構成する\*Api を構成します。</span><span class="sxs-lookup"><span data-stu-id="01404-110">You can configure the system to protect keys at rest by calling any of the ProtectKeysWith\* configuration APIs.</span></span> <span data-ttu-id="01404-111">次の例では UNC 共有でキーを格納し、特定の X.509 証明書の残りの部分でこれらのキーの暗号化を検討してください。</span><span class="sxs-lookup"><span data-stu-id="01404-111">Consider the example below, which stores keys at a UNC share and encrypts those keys at rest with a specific X.509 certificate.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

<span data-ttu-id="01404-112">参照してください[静止したキーの暗号化](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest)例については、組み込みのキーの暗号化メカニズムの詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="01404-112">See [key encryption at rest](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) for more examples and for discussion on the built-in key encryption mechanisms.</span></span>

<span data-ttu-id="01404-113">90 日間ではなく既定キーの有効期間は 14 日間を使用するようシステムを構成するのには、次の例を考慮してください。</span><span class="sxs-lookup"><span data-stu-id="01404-113">To configure the system to use a default key lifetime of 14 days instead of 90 days, consider the following example:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

<span data-ttu-id="01404-114">既定では、データ保護システムは、同じ物理キーのリポジトリを共有している場合でも、別のアプリケーションを分離します。</span><span class="sxs-lookup"><span data-stu-id="01404-114">By default the data protection system isolates applications from one another, even if they're sharing the same physical key repository.</span></span> <span data-ttu-id="01404-115">これにより、互いの保護されているペイロードを理解するアプリケーション。</span><span class="sxs-lookup"><span data-stu-id="01404-115">This prevents the applications from understanding each other's protected payloads.</span></span> <span data-ttu-id="01404-116">2 つの異なるアプリケーション間で保護されているペイロードを共有するようにアプリケーションの両方に同じアプリケーションの名前に渡すことシステムの構成は次の例。</span><span class="sxs-lookup"><span data-stu-id="01404-116">To share protected payloads between two different applications, configure the system passing in the same application name for both applications as in the below example:</span></span>

<a name="data-protection-code-sample-application-name"></a>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("my application");
}
```

<a name="data-protection-configuring-disable-automatic-key-generation"></a>

<span data-ttu-id="01404-117">最後に、アプリケーションの有効期限が間近、キーを自動的にロールをしないシナリオがある可能性があります。</span><span class="sxs-lookup"><span data-stu-id="01404-117">Finally, you may have a scenario where you do not want an application to automatically roll keys as they approach expiration.</span></span> <span data-ttu-id="01404-118">この 1 つの例には、アプリケーションのプライマリ/セカンダリは、関係でのみ、アプリケーションのプライマリ キー管理に関連する要素を担当するあり、すべてのセカンダリ アプリケーションだけがある、キーのリングの読み取り専用ビューを設定する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="01404-118">One example of this might be applications set up in a primary / secondary relationship, where only the primary application is responsible for key management concerns, and all secondary applications simply have a read-only view of the key ring.</span></span> <span data-ttu-id="01404-119">次に示すように、システムを構成することによって、読み取り専用としてキー リングを処理する、セカンダリのアプリケーションを構成できます。</span><span class="sxs-lookup"><span data-stu-id="01404-119">The secondary applications can be configured to treat the key ring as read-only by configuring the system as below:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

<a name="data-protection-configuration-per-app-isolation"></a>

## <a name="per-application-isolation"></a><span data-ttu-id="01404-120">アプリケーションごとの分離</span><span class="sxs-lookup"><span data-stu-id="01404-120">Per-application isolation</span></span>

<span data-ttu-id="01404-121">ASP.NET Core ホストによって、データ保護システムが指定した場合、それらのアプリケーションが同じワーカー プロセス アカウントで実行されていると、同じマスター キー生成情報を使用している場合でも、別のアプリケーションを自動的に分離はします。</span><span class="sxs-lookup"><span data-stu-id="01404-121">When the data protection system is provided by an ASP.NET Core host, it will automatically isolate applications from one another, even if those applications are running under the same worker process account and are using the same master keying material.</span></span> <span data-ttu-id="01404-122">これは、System.Web から IsolateApps 修飾子に似た<machineKey>要素。</span><span class="sxs-lookup"><span data-stu-id="01404-122">This is somewhat similar to the IsolateApps modifier from System.Web's <machineKey> element.</span></span>

<span data-ttu-id="01404-123">独自のテナントとしてローカル コンピューター上の各アプリケーションの検討から分離メカニズムのしくみ、したがって IDataProtector 自動的に、特定のアプリケーションのルートにはが含まれますアプリケーション ID には識別子として。</span><span class="sxs-lookup"><span data-stu-id="01404-123">The isolation mechanism works by considering each application on the local machine as a unique tenant, thus the IDataProtector rooted for any given application automatically includes the application ID as a discriminator.</span></span> <span data-ttu-id="01404-124">アプリケーションの一意の ID は、2 つの場所から取得されます。</span><span class="sxs-lookup"><span data-stu-id="01404-124">The application's unique ID comes from one of two places.</span></span>

1. <span data-ttu-id="01404-125">アプリケーションが IIS でホストされている場合は、アプリケーションの構成パスは一意識別子です。</span><span class="sxs-lookup"><span data-stu-id="01404-125">If the application is hosted in IIS, the unique identifier is the application's configuration path.</span></span> <span data-ttu-id="01404-126">アプリケーションがファーム環境に展開されている場合は、IIS 環境は、ファーム内のすべてのマシン間で同様に構成されていると仮定するとこの値が安定したあります。</span><span class="sxs-lookup"><span data-stu-id="01404-126">If an application is deployed in a farm environment, this value should be stable assuming that the IIS environments are configured similarly across all machines in the farm.</span></span>

2. <span data-ttu-id="01404-127">アプリケーションが IIS でホストされていない場合、一意識別子は、アプリケーションの物理パスです。</span><span class="sxs-lookup"><span data-stu-id="01404-127">If the application is not hosted in IIS, the unique identifier is the physical path of the application.</span></span>

<span data-ttu-id="01404-128">リセットの個々 のアプリケーション、および、マシン自体からの復旧は、一意の識別子は設計されています。</span><span class="sxs-lookup"><span data-stu-id="01404-128">The unique identifier is designed to survive resets - both of the individual application and of the machine itself.</span></span>

<span data-ttu-id="01404-129">この分離メカニズムは、アプリケーションが悪意のあるいないことを前提とします。</span><span class="sxs-lookup"><span data-stu-id="01404-129">This isolation mechanism assumes that the applications are not malicious.</span></span> <span data-ttu-id="01404-130">悪意のあるアプリケーションは、同じワーカー プロセス アカウントで実行されているその他のアプリケーションに常に影響を与えることができます。</span><span class="sxs-lookup"><span data-stu-id="01404-130">A malicious application can always impact any other application running under the same worker process account.</span></span> <span data-ttu-id="01404-131">共有ホスティング環境でアプリケーションが相互に信頼されていない、ホスティング プロバイダーは、キーのリポジトリの基になるアプリケーションの分離を含む、アプリケーション間で OS レベルの分離を講じる必要があります。</span><span class="sxs-lookup"><span data-stu-id="01404-131">In a shared hosting environment where applications are mutually untrusted, the hosting provider should take steps to ensure OS-level isolation between applications, including separating the applications' underlying key repositories.</span></span>

<span data-ttu-id="01404-132">データ保護システムによって提供されていない ASP.NET Core ホスト場合 (たとえば、開発者は、それをインスタンス化自分自身 DataProtectionProvider 具象型を使用して)、既定では、アプリケーションの分離が無効になり、すべてのアプリケーションが同じキーの更新によってサポート適切な目的を提供する限り、マテリアルはペイロードを共有できます。</span><span class="sxs-lookup"><span data-stu-id="01404-132">If the data protection system is not provided by an ASP.NET Core host (e.g., if the developer instantiates it himself via the DataProtectionProvider concrete type), application isolation is disabled by default, and all applications backed by the same keying material can share payloads as long as they provide the appropriate purposes.</span></span> <span data-ttu-id="01404-133">この環境でアプリケーションの分離を提供するには、構成オブジェクトに対して SetApplicationName メソッドを呼び出すを参照してください、[コード サンプル](#data-protection-code-sample-application-name)上。</span><span class="sxs-lookup"><span data-stu-id="01404-133">To provide application isolation in this environment, call the SetApplicationName method on the configuration object, see the [code sample](#data-protection-code-sample-application-name) above.</span></span>

<a name="data-protection-changing-algorithms"></a>

## <a name="changing-algorithms"></a><span data-ttu-id="01404-134">アルゴリズムを変更します。</span><span class="sxs-lookup"><span data-stu-id="01404-134">Changing algorithms</span></span>

<span data-ttu-id="01404-135">データ保護スタックは、新しく生成されたキーで使用される既定のアルゴリズムを変更できます。</span><span class="sxs-lookup"><span data-stu-id="01404-135">The data protection stack allows changing the default algorithm used by newly-generated keys.</span></span> <span data-ttu-id="01404-136">これを行う最も簡単な方法はからを呼び出す UseCryptographicAlgorithms 構成コールバックとしてでは、次の例です。</span><span class="sxs-lookup"><span data-stu-id="01404-136">The simplest way to do this is to call UseCryptographicAlgorithms from the configuration callback, as in the below example.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="01404-137">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="01404-137">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(new AuthenticatedEncryptorConfiguration()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="01404-138">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="01404-138">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(new AuthenticatedEncryptionSettings()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

---

<span data-ttu-id="01404-139">既定 EncryptionAlgorithm ValidationAlgorithm は AES 256-CBC、HMACSHA256、それぞれします。</span><span class="sxs-lookup"><span data-stu-id="01404-139">The default EncryptionAlgorithm and ValidationAlgorithm are AES-256-CBC and HMACSHA256, respectively.</span></span> <span data-ttu-id="01404-140">使用して、システム管理者によって既定のポリシーを設定することができます[マシン全体のポリシー](machine-wide-policy.md)UseCryptographicAlgorithms を明示的に呼び出すには、既定のポリシーがよりも優先されますが、します。</span><span class="sxs-lookup"><span data-stu-id="01404-140">The default policy can be set by a system administrator via [Machine Wide Policy](machine-wide-policy.md), but an explicit call to UseCryptographicAlgorithms will override the default policy.</span></span>

<span data-ttu-id="01404-141">UseCryptographicAlgorithms を呼び出すことにより、開発者は (あらかじめ定義された組み込みの一覧から)、必要なアルゴリズムを指定して、開発者は、アルゴリズムの実装について心配する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="01404-141">Calling UseCryptographicAlgorithms will allow the developer to specify the desired algorithm (from a predefined built-in list), and the developer does not need to worry about the implementation of the algorithm.</span></span> <span data-ttu-id="01404-142">たとえば、データ保護システム上のシナリオでを試みます Windows で実行されている場合は、AES の CNG 実装を使用して、それ以外の場合に切り替えられますマネージ System.Security.Cryptography.Aes クラスにします。</span><span class="sxs-lookup"><span data-stu-id="01404-142">For instance, in the scenario above the data protection system will attempt to use the CNG implementation of AES if running on Windows, otherwise it will fall back to the managed System.Security.Cryptography.Aes class.</span></span>

<span data-ttu-id="01404-143">開発者は、手動で指定できます、実装でスライド ショーとして UseCustomCryptographicAlgorithms への呼び出しを使用して必要な場合は、次の例です。</span><span class="sxs-lookup"><span data-stu-id="01404-143">The developer can manually specify an implementation if desired via a call to UseCustomCryptographicAlgorithms, as show in the below examples.</span></span>

>[!TIP]
> <span data-ttu-id="01404-144">アルゴリズムを変更しても、キー リング内の既存のキーには影響しません。</span><span class="sxs-lookup"><span data-stu-id="01404-144">Changing algorithms does not affect existing keys in the key ring.</span></span> <span data-ttu-id="01404-145">新しく生成されたキーにのみ影響します。</span><span class="sxs-lookup"><span data-stu-id="01404-145">It only affects newly-generated keys.</span></span>

<a name="data-protection-changing-algorithms-custom-managed"></a>

### <a name="specifying-custom-managed-algorithms"></a><span data-ttu-id="01404-146">カスタムの管理されているアルゴリズムを指定します。</span><span class="sxs-lookup"><span data-stu-id="01404-146">Specifying custom managed algorithms</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="01404-147">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="01404-147">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="01404-148">カスタム マネージ アルゴリズムを指定するには、実装の種類を示す ManagedAuthenticatedEncryptorConfiguration インスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="01404-148">To specify custom managed algorithms, create a ManagedAuthenticatedEncryptorConfiguration instance that points to the implementation types.</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="01404-149">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="01404-149">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="01404-150">カスタム マネージ アルゴリズムを指定するには、実装の種類を示す ManagedAuthenticatedEncryptionSettings インスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="01404-150">To specify custom managed algorithms, create a ManagedAuthenticatedEncryptionSettings instance that points to the implementation types.</span></span>

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

<span data-ttu-id="01404-151">一般に、\*型のプロパティは、具象型を指す必要があります (パラメーターなしのパブリック コンストラクタ) 経由でインスタンス化可能な実装対称アルゴリズムの KeyedHashAlgorithm、いくつかの値などのシステムの特別なケースが便宜上 typeof(Aes).</span><span class="sxs-lookup"><span data-stu-id="01404-151">Generally the \*Type properties must point to concrete, instantiable (via a public parameterless ctor) implementations of SymmetricAlgorithm and KeyedHashAlgorithm, though the system special-cases some values like typeof(Aes) for convenience.</span></span>

> [!NOTE]
> <span data-ttu-id="01404-152">PKCS #7 パディング CBC モードの暗号化をサポートする必要があり、対称アルゴリズムは ≥ 128 ビットのキーの長さと ≥ の 64 ビットのブロック サイズにあります。</span><span class="sxs-lookup"><span data-stu-id="01404-152">The SymmetricAlgorithm must have a key length of ≥ 128 bits and a block size of ≥ 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="01404-153">ダイジェストのサイズをいる必要があります、KeyedHashAlgorithm > = 128 ビット、およびハッシュ アルゴリズムのダイジェストの長さと同じ長さのキーをサポートする必要があります。</span><span class="sxs-lookup"><span data-stu-id="01404-153">The KeyedHashAlgorithm must have a digest size of >= 128 bits, and it must support keys of length equal to the hash algorithm's digest length.</span></span> <span data-ttu-id="01404-154">KeyedHashAlgorithm は HMAC を必須ではありません。</span><span class="sxs-lookup"><span data-stu-id="01404-154">The KeyedHashAlgorithm is not strictly required to be HMAC.</span></span>

<a name="data-protection-changing-algorithms-cng"></a>

### <a name="specifying-custom-windows-cng-algorithms"></a><span data-ttu-id="01404-155">カスタムの Windows CNG アルゴリズムを指定します。</span><span class="sxs-lookup"><span data-stu-id="01404-155">Specifying custom Windows CNG algorithms</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="01404-156">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="01404-156">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="01404-157">CBC モードの暗号化 + HMAC 検証を使用して、カスタム Windows CNG アルゴリズムを指定するには、アルゴリズムの情報を含む CngCbcAuthenticatedEncryptorConfiguration インスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="01404-157">To specify a custom Windows CNG algorithm using CBC-mode encryption + HMAC validation, create a CngCbcAuthenticatedEncryptorConfiguration instance that contains the algorithmic information.</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="01404-158">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="01404-158">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="01404-159">CBC モードの暗号化 + HMAC 検証を使用して、カスタム Windows CNG アルゴリズムを指定するには、アルゴリズムの情報を含む CngCbcAuthenticatedEncryptionSettings インスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="01404-159">To specify a custom Windows CNG algorithm using CBC-mode encryption + HMAC validation, create a CngCbcAuthenticatedEncryptionSettings instance that contains the algorithmic information.</span></span>

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
> <span data-ttu-id="01404-160">対称ブロック暗号アルゴリズムが必要 ≥ 128 ビットのキーの長さと ≥ の 64 ビットのブロック サイズを PKCS #7 パディング CBC モードの暗号化をサポートする必要があります。</span><span class="sxs-lookup"><span data-stu-id="01404-160">The symmetric block cipher algorithm must have a key length of ≥ 128 bits and a block size of ≥ 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="01404-161">ハッシュ アルゴリズムのダイジェストのサイズが必要 > = 128 ビットと BCRYPT_ALG_HANDLE_HMAC_FLAG フラグで開かれるをサポートする必要があります。</span><span class="sxs-lookup"><span data-stu-id="01404-161">The hash algorithm must have a digest size of >= 128 bits and must support being opened with the BCRYPT_ALG_HANDLE_HMAC_FLAG flag.</span></span> <span data-ttu-id="01404-162">\*プロバイダーのプロパティは、指定したアルゴリズムの既定のプロバイダーを使用するのには、null に設定できます。</span><span class="sxs-lookup"><span data-stu-id="01404-162">The \*Provider properties can be set to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="01404-163">参照してください、[アンロード](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx)詳細についてはドキュメントです。</span><span class="sxs-lookup"><span data-stu-id="01404-163">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="01404-164">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="01404-164">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="01404-165">Galois/カウンター モードの暗号化と検証を使用して、カスタム Windows CNG アルゴリズムを指定するには、アルゴリズム情報を含む CngGcmAuthenticatedEncryptorConfiguration インスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="01404-165">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption + validation, create a CngGcmAuthenticatedEncryptorConfiguration instance that contains the algorithmic information.</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="01404-166">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="01404-166">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="01404-167">Galois/カウンター モードの暗号化と検証を使用して、カスタム Windows CNG アルゴリズムを指定するには、アルゴリズム情報を含む CngGcmAuthenticatedEncryptionSettings インスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="01404-167">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption + validation, create a CngGcmAuthenticatedEncryptionSettings instance that contains the algorithmic information.</span></span>

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
> <span data-ttu-id="01404-168">対称ブロック暗号アルゴリズムは ≥ 128 ビットのキーの長さと正確に 128 ビットのブロック サイズが必要し、GCM 暗号化をサポートする必要があります。</span><span class="sxs-lookup"><span data-stu-id="01404-168">The symmetric block cipher algorithm must have a key length of ≥ 128 bits and a block size of exactly 128 bits, and it must support GCM encryption.</span></span> <span data-ttu-id="01404-169">EncryptionAlgorithmProvider プロパティは、指定したアルゴリズムの既定のプロバイダーを使用する場合は null に設定できます。</span><span class="sxs-lookup"><span data-stu-id="01404-169">The EncryptionAlgorithmProvider property can be set to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="01404-170">参照してください、[アンロード](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx)詳細についてはドキュメントです。</span><span class="sxs-lookup"><span data-stu-id="01404-170">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

### <a name="specifying-other-custom-algorithms"></a><span data-ttu-id="01404-171">その他のカスタム アルゴリズムを指定します。</span><span class="sxs-lookup"><span data-stu-id="01404-171">Specifying other custom algorithms</span></span>

<span data-ttu-id="01404-172">ファースト クラスの API として公開されていない、データ保護システムはほとんどのアルゴリズムの種類を指定することを許可するのに十分な拡張可能です。</span><span class="sxs-lookup"><span data-stu-id="01404-172">Though not exposed as a first-class API, the data protection system is extensible enough to allow specifying almost any kind of algorithm.</span></span> <span data-ttu-id="01404-173">たとえば、HSM 内で格納されているすべてのキーを保持し、実装を提供する、カスタムのコアの暗号化と暗号化解除ルーチンも可能です。</span><span class="sxs-lookup"><span data-stu-id="01404-173">For example, it is possible to keep all keys contained within an HSM and to provide a custom implementation of the core encryption and decryption routines.</span></span> <span data-ttu-id="01404-174">詳細については、コア暗号化機能拡張セクションで IAuthenticatedEncryptorConfiguration を参照してください。</span><span class="sxs-lookup"><span data-stu-id="01404-174">See IAuthenticatedEncryptorConfiguration in the core cryptography extensibility section for more information.</span></span>

### <a name="see-also"></a><span data-ttu-id="01404-175">参照</span><span class="sxs-lookup"><span data-stu-id="01404-175">See also</span></span>

* [<span data-ttu-id="01404-176">DI に対応しないシナリオ</span><span class="sxs-lookup"><span data-stu-id="01404-176">Non DI Aware Scenarios</span></span>](non-di-scenarios.md)
* [<span data-ttu-id="01404-177">コンピューター全体のポリシー</span><span class="sxs-lookup"><span data-stu-id="01404-177">Machine Wide Policy</span></span>](machine-wide-policy.md)
