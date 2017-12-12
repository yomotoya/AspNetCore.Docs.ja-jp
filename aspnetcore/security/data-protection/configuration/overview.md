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
# <a name="configuring-data-protection-in-aspnet-core"></a><span data-ttu-id="90604-104">ASP.NET Core でのデータ保護の構成</span><span class="sxs-lookup"><span data-stu-id="90604-104">Configuring Data Protection in ASP.NET Core</span></span>

<span data-ttu-id="90604-105">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="90604-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="90604-106">データ保護システムが初期化されたときに適用される[既定の設定](xref:security/data-protection/configuration/default-settings)運用環境に基づきます。</span><span class="sxs-lookup"><span data-stu-id="90604-106">When the Data Protection system is initialized, it applies [default settings](xref:security/data-protection/configuration/default-settings) based on the operational environment.</span></span> <span data-ttu-id="90604-107">これらの設定に適した一般的に 1 台のコンピューターで実行されるアプリです。</span><span class="sxs-lookup"><span data-stu-id="90604-107">These settings are generally appropriate for apps running on a single machine.</span></span> <span data-ttu-id="90604-108">開発者は複数のコンピューターまたはコンプライアンス上の理由から、アプリが分散しているので、おそらく、既定の設定を変更する必要のあるケースがあります。</span><span class="sxs-lookup"><span data-stu-id="90604-108">There are cases where a developer may want to change the default settings, perhaps because their app is spread across multiple machines or for compliance reasons.</span></span> <span data-ttu-id="90604-109">このようなシナリオは、データ保護システムは、豊富な構成 API を提供します。</span><span class="sxs-lookup"><span data-stu-id="90604-109">For these scenarios, the Data Protection system offers a rich configuration API.</span></span>

<span data-ttu-id="90604-110">拡張メソッドがある[AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection)を返す、 [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder)です。</span><span class="sxs-lookup"><span data-stu-id="90604-110">There's an extension method [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) that returns an [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder).</span></span> <span data-ttu-id="90604-111">`IDataProtectionBuilder`連結できます。 データ保護を構成するオプションの拡張メソッドを公開します。</span><span class="sxs-lookup"><span data-stu-id="90604-111">`IDataProtectionBuilder` exposes extension methods that you can chain together to configure Data Protection options.</span></span>

## <a name="persistkeystofilesystem"></a><span data-ttu-id="90604-112">PersistKeysToFileSystem</span><span class="sxs-lookup"><span data-stu-id="90604-112">PersistKeysToFileSystem</span></span>

<span data-ttu-id="90604-113">代わりに UNC 共有にキーを格納する、 *%localappdata%*既定の場所、構成を使用してシステム[PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span><span class="sxs-lookup"><span data-stu-id="90604-113">To store keys on a UNC share instead of at the *%LOCALAPPDATA%* default location, configure the system with [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

> [!WARNING]
> <span data-ttu-id="90604-114">キーの永続性の場所を変更すると、システム不要になったによって自動的に暗号化キー、残りの部分を DPAPI は、適切な暗号化メカニズムであるかどうかを認識していないためです。</span><span class="sxs-lookup"><span data-stu-id="90604-114">If you change the key persistence location, the system no longer automatically encrypts keys at rest, since it doesn't know whether DPAPI is an appropriate encryption mechanism.</span></span>

## <a name="protectkeyswith"></a><span data-ttu-id="90604-115">ProtectKeysWith\*</span><span class="sxs-lookup"><span data-stu-id="90604-115">ProtectKeysWith\*</span></span>

<span data-ttu-id="90604-116">いずれかを呼び出すことによって残りの部分でキーを保護するシステムを構成することができます、 [ProtectKeysWith\* ](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) Api を構成します。</span><span class="sxs-lookup"><span data-stu-id="90604-116">You can configure the system to protect keys at rest by calling any of the [ProtectKeysWith\*](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) configuration APIs.</span></span> <span data-ttu-id="90604-117">UNC 共有にキーを格納し、特定の X.509 証明書の残りの部分では、そのキーを暗号化する、次の例を考えてみます。</span><span class="sxs-lookup"><span data-stu-id="90604-117">Consider the example below, which stores keys on a UNC share and encrypts those keys at rest with a specific X.509 certificate:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

<span data-ttu-id="90604-118">参照してください[キー時の暗号化](xref:security/data-protection/implementation/key-encryption-at-rest)詳細の例と組み込みのキーの暗号化メカニズムについて説明します。</span><span class="sxs-lookup"><span data-stu-id="90604-118">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more examples and discussion on the built-in key encryption mechanisms.</span></span>

## <a name="setdefaultkeylifetime"></a><span data-ttu-id="90604-119">SetDefaultKeyLifetime</span><span class="sxs-lookup"><span data-stu-id="90604-119">SetDefaultKeyLifetime</span></span>

<span data-ttu-id="90604-120">90 日間に、既定ではなくキーの有効期間は 14 日間を使用するようシステムを構成するのには、使用[SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span><span class="sxs-lookup"><span data-stu-id="90604-120">To configure the system to use a key lifetime of 14 days instead of the default 90 days, use [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

## <a name="setapplicationname"></a><span data-ttu-id="90604-121">SetApplicationName</span><span class="sxs-lookup"><span data-stu-id="90604-121">SetApplicationName</span></span>

<span data-ttu-id="90604-122">既定では、データ保護システムは、同じ物理キーのリポジトリを共有している場合でも、別のアプリを分離します。</span><span class="sxs-lookup"><span data-stu-id="90604-122">By default, the Data Protection system isolates apps from one another, even if they're sharing the same physical key repository.</span></span> <span data-ttu-id="90604-123">これは、アプリが互いの保護されているペイロードを理解することを防ぎます。</span><span class="sxs-lookup"><span data-stu-id="90604-123">This prevents the apps from understanding each other's protected payloads.</span></span> <span data-ttu-id="90604-124">2 つのアプリ間で保護されているペイロードを共有するには、使用[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname)各アプリに対して同じ値を持つ。</span><span class="sxs-lookup"><span data-stu-id="90604-124">To share protected payloads between two apps, use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) with the same value for each app:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("shared app name");
}
```

## <a name="disableautomatickeygeneration"></a><span data-ttu-id="90604-125">DisableAutomaticKeyGeneration</span><span class="sxs-lookup"><span data-stu-id="90604-125">DisableAutomaticKeyGeneration</span></span>

<span data-ttu-id="90604-126">有効期限が間近、(新しいキーを作成) キーを自動的にロールにアプリをたくないシナリオがあります。</span><span class="sxs-lookup"><span data-stu-id="90604-126">You may have a scenario where you don't want an app to automatically roll keys (create new keys) as they approach expiration.</span></span> <span data-ttu-id="90604-127">この 1 つの例は、プライマリ/セカンダリ関係、ここで、プライマリのアプリのみがキーの管理に関する注意事項を担当して、セカンダリ アプリだけがある、キーのリングの読み取り専用ビューを設定アプリにする可能性があります。</span><span class="sxs-lookup"><span data-stu-id="90604-127">One example of this might be apps set up in a primary/secondary relationship, where only the primary app is responsible for key management concerns and secondary apps simply have a read-only view of the key ring.</span></span> <span data-ttu-id="90604-128">セカンダリ アプリを使用してシステムを構成することによって、読み取り専用としてキー リングを処理するように構成できます[DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):</span><span class="sxs-lookup"><span data-stu-id="90604-128">The secondary apps can be configured to treat the key ring as read-only by configuring the system with [DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

## <a name="per-application-isolation"></a><span data-ttu-id="90604-129">アプリケーションごとの分離</span><span class="sxs-lookup"><span data-stu-id="90604-129">Per-application isolation</span></span>

<span data-ttu-id="90604-130">データ保護システムは、ASP.NET Core ホストによって提供される、ときに自動的に分離され、そのから、別のアプリの場合でも、それらのアプリが同じワーカー プロセス アカウントで実行されていて、同じマスター キー生成情報を使用しています。</span><span class="sxs-lookup"><span data-stu-id="90604-130">When the Data Protection system is provided by an ASP.NET Core host, it automatically isolates apps from one another, even if those apps are running under the same worker process account and are using the same master keying material.</span></span> <span data-ttu-id="90604-131">これは、System.Web から IsolateApps 修飾子に似た **\<machineKey >**要素。</span><span class="sxs-lookup"><span data-stu-id="90604-131">This is somewhat similar to the IsolateApps modifier from System.Web's **\<machineKey>** element.</span></span>

<span data-ttu-id="90604-132">したがって、独自のテナントとしてローカル コンピューターには、各アプリを考慮して分離メカニズムの動作、 [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector)ルートとする指定したアプリには、アプリ ID は識別子として自動的が含まれています。</span><span class="sxs-lookup"><span data-stu-id="90604-132">The isolation mechanism works by considering each app on the local machine as a unique tenant, thus the [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) rooted for any given app automatically includes the app ID as a discriminator.</span></span> <span data-ttu-id="90604-133">アプリの一意の ID は 2 つの場所から取得します。</span><span class="sxs-lookup"><span data-stu-id="90604-133">The app's unique ID comes from one of two places:</span></span>

1. <span data-ttu-id="90604-134">アプリが IIS でホストされている場合は、アプリの構成パスは一意識別子です。</span><span class="sxs-lookup"><span data-stu-id="90604-134">If the app is hosted in IIS, the unique identifier is the app's configuration path.</span></span> <span data-ttu-id="90604-135">アプリは、web ファーム環境で展開が場合、は、IIS 環境は、web ファーム内のすべてのマシン間で同様に構成されていると仮定するとこの値が安定したあります。</span><span class="sxs-lookup"><span data-stu-id="90604-135">If an app is deployed in a web farm environment, this value should be stable assuming that the IIS environments are configured similarly across all machines in the web farm.</span></span>

2. <span data-ttu-id="90604-136">場合は、アプリは、IIS でホストされていない、一意の識別子は、アプリの物理パスです。</span><span class="sxs-lookup"><span data-stu-id="90604-136">If the app isn't hosted in IIS, the unique identifier is the physical path of the app.</span></span>

<span data-ttu-id="90604-137">一意識別子がリセットで存続するように設計&mdash;個々 のアプリの両方に対してコンピューター自体とします。</span><span class="sxs-lookup"><span data-stu-id="90604-137">The unique identifier is designed to survive resets &mdash; both of the individual app and of the machine itself.</span></span>

<span data-ttu-id="90604-138">この分離メカニズムでは、アプリが悪意のあるになっていないことを前提としています。</span><span class="sxs-lookup"><span data-stu-id="90604-138">This isolation mechanism assumes that the apps are not malicious.</span></span> <span data-ttu-id="90604-139">悪意のあるアプリは、同じワーカー プロセス アカウントで実行されている他のアプリに常に影響を与えることができます。</span><span class="sxs-lookup"><span data-stu-id="90604-139">A malicious app can always impact any other app running under the same worker process account.</span></span> <span data-ttu-id="90604-140">共有ホスティング環境でアプリが相互に信頼された、ホスティング プロバイダーは、アプリのキーのリポジトリを基になるを分離することを含め、アプリ間で OS レベルの分離を講じる必要があります。</span><span class="sxs-lookup"><span data-stu-id="90604-140">In a shared hosting environment where apps are mutually untrusted, the hosting provider should take steps to ensure OS-level isolation between apps, including separating the apps' underlying key repositories.</span></span>

<span data-ttu-id="90604-141">ASP.NET Core ホストによって、データ保護システムが提供されないかどうか (たとえば、使用してそれをインスタンス化する場合、`DataProtectionProvider`具象型) アプリの分離が既定で無効になっています。</span><span class="sxs-lookup"><span data-stu-id="90604-141">If the Data Protection system isn't provided by an ASP.NET Core host (for example, if you instantiate it via the `DataProtectionProvider` concrete type) app isolation is disabled by default.</span></span> <span data-ttu-id="90604-142">適切な提供する限り、アプリの分離を無効にすると、同じキー生成情報によってサポートすべてのアプリケーションではペイロードを共有できます[目的](xref:security/data-protection/consumer-apis/purpose-strings)です。</span><span class="sxs-lookup"><span data-stu-id="90604-142">When app isolation is disabled, all apps backed by the same keying material can share payloads as long as they provide the appropriate [purposes](xref:security/data-protection/consumer-apis/purpose-strings).</span></span> <span data-ttu-id="90604-143">この環境でアプリの分離を提供するには、呼び出し、 [SetApplicationName](#setapplicationname)構成上のメソッド オブジェクトを各アプリに対して一意の名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="90604-143">To provide app isolation in this environment, call the [SetApplicationName](#setapplicationname) method on the configuration object and provide a unique name for each app.</span></span>

## <a name="changing-algorithms-with-usecryptographicalgorithms"></a><span data-ttu-id="90604-144">UseCryptographicAlgorithms でアルゴリズムを変更します。</span><span class="sxs-lookup"><span data-stu-id="90604-144">Changing algorithms with UseCryptographicAlgorithms</span></span>

<span data-ttu-id="90604-145">データ保護スタックでは、新しく生成されたキーで使用される既定のアルゴリズムを変更することができます。</span><span class="sxs-lookup"><span data-stu-id="90604-145">The Data Protection stack allows you to change the default algorithm used by newly-generated keys.</span></span> <span data-ttu-id="90604-146">これを行う最も簡単な方法を呼び出すことです。 [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms)構成コールバックから。</span><span class="sxs-lookup"><span data-stu-id="90604-146">The simplest way to do this is to call [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) from the configuration callback:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="90604-147">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="90604-147">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptorConfiguration()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="90604-148">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="90604-148">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="90604-149">既定の EncryptionAlgorithm は AES 256-CBC で、既定値 ValidationAlgorithm は HMACSHA256 です。</span><span class="sxs-lookup"><span data-stu-id="90604-149">The default EncryptionAlgorithm is AES-256-CBC, and the default ValidationAlgorithm is HMACSHA256.</span></span> <span data-ttu-id="90604-150">経由でシステム管理者によって既定のポリシーを設定することができます、[コンピューター レベル ポリシー](xref:security/data-protection/configuration/machine-wide-policy)を明示的に呼び出す`UseCryptographicAlgorithms`既定のポリシーをオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="90604-150">The default policy can be set by a system administrator via a [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy), but an explicit call to `UseCryptographicAlgorithms` overrides the default policy.</span></span>

<span data-ttu-id="90604-151">呼び出す`UseCryptographicAlgorithms`事前定義された組み込みの一覧から目的のアルゴリズムを指定することができます。</span><span class="sxs-lookup"><span data-stu-id="90604-151">Calling `UseCryptographicAlgorithms` allows you to specify the desired algorithm from a predefined built-in list.</span></span> <span data-ttu-id="90604-152">アルゴリズムの実装について心配する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="90604-152">You don't need to worry about the implementation of the algorithm.</span></span> <span data-ttu-id="90604-153">上記のシナリオでは、データ保護システムは Windows で実行されている場合は、AES の CNG 実装を使用しようとします。</span><span class="sxs-lookup"><span data-stu-id="90604-153">In the scenario above, the Data Protection system attempts to use the CNG implementation of AES if running on Windows.</span></span> <span data-ttu-id="90604-154">それ以外の場合にフォールバック マネージ[System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes)クラスです。</span><span class="sxs-lookup"><span data-stu-id="90604-154">Otherwise, it falls back to the managed [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) class.</span></span>

<span data-ttu-id="90604-155">呼び出しを使用して、実装を手動で指定することができます[UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms)です。</span><span class="sxs-lookup"><span data-stu-id="90604-155">You can manually specify an implementation via a call to [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).</span></span>

> [!TIP]
> <span data-ttu-id="90604-156">アルゴリズムを変更すると、キー リング内の既存のキーは影響しません。</span><span class="sxs-lookup"><span data-stu-id="90604-156">Changing algorithms doesn't affect existing keys in the key ring.</span></span> <span data-ttu-id="90604-157">新しく生成されたキーにのみ影響します。</span><span class="sxs-lookup"><span data-stu-id="90604-157">It only affects newly-generated keys.</span></span>

### <a name="specifying-custom-managed-algorithms"></a><span data-ttu-id="90604-158">カスタムの管理されているアルゴリズムを指定します。</span><span class="sxs-lookup"><span data-stu-id="90604-158">Specifying custom managed algorithms</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="90604-159">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="90604-159">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="90604-160">カスタムの管理されているアルゴリズムを指定するには、作成、 [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration)実装の種類を示すインスタンス。</span><span class="sxs-lookup"><span data-stu-id="90604-160">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) instance that points to the implementation types:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="90604-161">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="90604-161">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="90604-162">カスタムの管理されているアルゴリズムを指定するには、作成、 [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings)実装の種類を示すインスタンス。</span><span class="sxs-lookup"><span data-stu-id="90604-162">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) instance that points to the implementation types:</span></span>

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

<span data-ttu-id="90604-163">一般に、\*型のプロパティは、具象型を指す必要があります (パラメーターなしのパブリック コンストラクタ) 経由でインスタンス化可能な実装の[対称アルゴリズム](/dotnet/api/system.security.cryptography.symmetricalgorithm)と[KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm)が、特殊なケースなどのいくつかの値のシステム`typeof(Aes)`利便性を考慮します。</span><span class="sxs-lookup"><span data-stu-id="90604-163">Generally the \*Type properties must point to concrete, instantiable (via a public parameterless ctor) implementations of [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) and [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), though the system special-cases some values like `typeof(Aes)` for convenience.</span></span>

> [!NOTE]
> <span data-ttu-id="90604-164">`SymmetricAlgorithm`キーの長さが必要 > = 128 ビット単位のブロック サイズ > 64 ビットの場合は、以下が PKCS #7 埋め込み CBC モードの暗号化をサポートする必要があります。</span><span class="sxs-lookup"><span data-stu-id="90604-164">The `SymmetricAlgorithm` must have a key length of >= 128 bits, a block size of >= 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="90604-165">`KeyedHashAlgorithm`のダイジェストのサイズが必要 > = 128 ビット、およびハッシュ アルゴリズムのダイジェストの長さと同じ長さのキーをサポートする必要があります。</span><span class="sxs-lookup"><span data-stu-id="90604-165">The `KeyedHashAlgorithm` must have a digest size of >= 128 bits, and it must support keys of length equal to the hash algorithm's digest length.</span></span> <span data-ttu-id="90604-166">`KeyedHashAlgorithm` HMAC を厳密には必要はありません。</span><span class="sxs-lookup"><span data-stu-id="90604-166">The `KeyedHashAlgorithm` isn't strictly required to be HMAC.</span></span>
> <span data-ttu-id="90604-167">PKCS #7 パディング CBC モードの暗号化をサポートする必要があり、対称アルゴリズムは ≥ 128 ビットのキーの長さと ≥ の 64 ビットのブロック サイズにあります。</span><span class="sxs-lookup"><span data-stu-id="90604-167">The SymmetricAlgorithm must have a key length of ≥ 128 bits and a block size of ≥ 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="90604-168">ダイジェストのサイズをいる必要があります、KeyedHashAlgorithm > = 128 ビット、およびハッシュ アルゴリズムのダイジェストの長さと同じ長さのキーをサポートする必要があります。</span><span class="sxs-lookup"><span data-stu-id="90604-168">The KeyedHashAlgorithm must have a digest size of >= 128 bits, and it must support keys of length equal to the hash algorithm's digest length.</span></span> <span data-ttu-id="90604-169">KeyedHashAlgorithm は HMAC を必須ではありません。</span><span class="sxs-lookup"><span data-stu-id="90604-169">The KeyedHashAlgorithm is not strictly required to be HMAC.</span></span>

### <a name="specifying-custom-windows-cng-algorithms"></a><span data-ttu-id="90604-170">カスタムの Windows CNG アルゴリズムを指定します。</span><span class="sxs-lookup"><span data-stu-id="90604-170">Specifying custom Windows CNG algorithms</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="90604-171">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="90604-171">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="90604-172">HMAC 検証と CBC モードの暗号化を使用して、カスタム Windows CNG アルゴリズムを指定するには、作成、 [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration)アルゴリズム情報を含むインスタンス。</span><span class="sxs-lookup"><span data-stu-id="90604-172">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="90604-173">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="90604-173">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="90604-174">HMAC 検証と CBC モードの暗号化を使用して、カスタム Windows CNG アルゴリズムを指定するには、作成、 [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings)アルゴリズム情報を含むインスタンス。</span><span class="sxs-lookup"><span data-stu-id="90604-174">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

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
> <span data-ttu-id="90604-175">対称ブロック暗号アルゴリズム、キーの長さを持つ必要があります > = 128 ビット単位のブロック サイズ > の 64 ビットを = し、PKCS #7 パディング CBC モードの暗号化をサポートする必要があります。</span><span class="sxs-lookup"><span data-stu-id="90604-175">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of >= 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="90604-176">ハッシュ アルゴリズムのダイジェストのサイズが必要 > = 128 ビットと、BCRYPT で開かれるをサポートする必要があります\_ALG\_処理\_HMAC\_フラグ フラグ。</span><span class="sxs-lookup"><span data-stu-id="90604-176">The hash algorithm must have a digest size of >= 128 bits and must support being opened with the BCRYPT\_ALG\_HANDLE\_HMAC\_FLAG flag.</span></span> <span data-ttu-id="90604-177">\*プロバイダーのプロパティは、指定したアルゴリズムの既定のプロバイダーを使用するのには、null に設定できます。</span><span class="sxs-lookup"><span data-stu-id="90604-177">The \*Provider properties can be set to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="90604-178">参照してください、[アンロード](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx)詳細についてはドキュメントです。</span><span class="sxs-lookup"><span data-stu-id="90604-178">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="90604-179">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="90604-179">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="90604-180">検証で Galois/カウンター モードの暗号化を使用して、カスタム Windows CNG アルゴリズムを指定するには、作成、 [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration)アルゴリズム情報を含むインスタンス。</span><span class="sxs-lookup"><span data-stu-id="90604-180">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="90604-181">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="90604-181">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="90604-182">検証で Galois/カウンター モードの暗号化を使用して、カスタム Windows CNG アルゴリズムを指定するには、作成、 [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings)アルゴリズム情報を含むインスタンス。</span><span class="sxs-lookup"><span data-stu-id="90604-182">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

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
> <span data-ttu-id="90604-183">対称ブロック暗号アルゴリズム、キーの長さを持つ必要があります > = 128 ビット、正確に 128 ビットのブロック サイズと GCM 暗号化をサポートする必要があります。</span><span class="sxs-lookup"><span data-stu-id="90604-183">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of exactly 128 bits, and it must support GCM encryption.</span></span> <span data-ttu-id="90604-184">設定することができます、 [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider)プロパティを指定したアルゴリズムの既定のプロバイダーを使用する場合は null です。</span><span class="sxs-lookup"><span data-stu-id="90604-184">You can set the [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) property to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="90604-185">参照してください、[アンロード](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx)詳細についてはドキュメントです。</span><span class="sxs-lookup"><span data-stu-id="90604-185">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

### <a name="specifying-other-custom-algorithms"></a><span data-ttu-id="90604-186">その他のカスタム アルゴリズムを指定します。</span><span class="sxs-lookup"><span data-stu-id="90604-186">Specifying other custom algorithms</span></span>

<span data-ttu-id="90604-187">ファースト クラスの API として公開されていない、データ保護システムはほとんどのアルゴリズムの種類を指定することを許可するのに十分な拡張可能です。</span><span class="sxs-lookup"><span data-stu-id="90604-187">Though not exposed as a first-class API, the Data Protection system is extensible enough to allow specifying almost any kind of algorithm.</span></span> <span data-ttu-id="90604-188">たとえば、ハードウェア セキュリティ モジュール (HSM) 内に含まれるすべてのキーを保持し、実装を提供する、カスタムのコアの暗号化と暗号化解除ルーチンも可能です。</span><span class="sxs-lookup"><span data-stu-id="90604-188">For example, it's possible to keep all keys contained within a Hardware Security Module (HSM) and to provide a custom implementation of the core encryption and decryption routines.</span></span> <span data-ttu-id="90604-189">参照してください[IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor)で[暗号化の拡張性をコア](xref:security/data-protection/extensibility/core-crypto)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="90604-189">See [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) in [Core cryptography extensibility](xref:security/data-protection/extensibility/core-crypto) for more information.</span></span>

## <a name="persisting-keys-when-hosting-in-a-docker-container"></a><span data-ttu-id="90604-190">Docker コンテナーでホストする場合、永続化キー</span><span class="sxs-lookup"><span data-stu-id="90604-190">Persisting keys when hosting in a Docker container</span></span>

<span data-ttu-id="90604-191">ホストする場合、 [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/)コンテナー、いずれかでキーを保持する必要があります。</span><span class="sxs-lookup"><span data-stu-id="90604-191">When hosting in a [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) container, keys should be maintained in either:</span></span>

* <span data-ttu-id="90604-192">フォルダーの共有ボリュームまたはホストにマウントされたボリュームなど、コンテナーの有効期間を超えて保持 Docker ボリュームです。</span><span class="sxs-lookup"><span data-stu-id="90604-192">A folder that's a Docker volume that persists beyond the container's lifetime, such as a shared volume or a host-mounted volume.</span></span>
* <span data-ttu-id="90604-193">外部プロバイダーなど[Azure Key Vault](https://azure.microsoft.com/services/key-vault/)または[Redis](https://redis.io/)です。</span><span class="sxs-lookup"><span data-stu-id="90604-193">An external provider, such as [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) or [Redis](https://redis.io/).</span></span>

## <a name="see-also"></a><span data-ttu-id="90604-194">関連項目</span><span class="sxs-lookup"><span data-stu-id="90604-194">See also</span></span>

* [<span data-ttu-id="90604-195">DI に対応しないシナリオ</span><span class="sxs-lookup"><span data-stu-id="90604-195">Non DI Aware Scenarios</span></span>](xref:security/data-protection/configuration/non-di-scenarios)
* [<span data-ttu-id="90604-196">コンピューター全体のポリシー</span><span class="sxs-lookup"><span data-stu-id="90604-196">Machine Wide Policy</span></span>](xref:security/data-protection/configuration/machine-wide-policy)
