---
title: ASP.NET Core でのキー管理の拡張性
author: rick-anderson
description: ASP.NET Core データ保護キー管理の拡張性について説明します。
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: 28932cbef1cc797338980f3e0de8b09caee324c0
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2019
ms.locfileid: "64896909"
---
# <a name="key-management-extensibility-in-aspnet-core"></a><span data-ttu-id="56b05-103">ASP.NET Core でのキー管理の拡張性</span><span class="sxs-lookup"><span data-stu-id="56b05-103">Key management extensibility in ASP.NET Core</span></span>

> [!TIP]
> <span data-ttu-id="56b05-104">読み取り、[キー管理](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)これらの Api の背後にある基本的な概念の一部について説明します、このセクションを読む前にセクション。</span><span class="sxs-lookup"><span data-stu-id="56b05-104">Read the [key management](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) section before reading this section, as it explains some of the fundamental concepts behind these APIs.</span></span>

> [!WARNING]
> <span data-ttu-id="56b05-105">次のインターフェイスのいずれかを実装する型がスレッド セーフにする必要があります複数の呼び出し元の。</span><span class="sxs-lookup"><span data-stu-id="56b05-105">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="key"></a><span data-ttu-id="56b05-106">キー</span><span class="sxs-lookup"><span data-stu-id="56b05-106">Key</span></span>

<span data-ttu-id="56b05-107">`IKey`インターフェイスは基本的なキー暗号方式で表したものです。</span><span class="sxs-lookup"><span data-stu-id="56b05-107">The `IKey` interface is the basic representation of a key in cryptosystem.</span></span> <span data-ttu-id="56b05-108">用語のキーは、「暗号化キー マテリアル」の文字どおりの意味ではなく、抽象という意味では、ここに使用されます。</span><span class="sxs-lookup"><span data-stu-id="56b05-108">The term key is used here in the abstract sense, not in the literal sense of "cryptographic key material".</span></span> <span data-ttu-id="56b05-109">キーには、次のプロパティがあります。</span><span class="sxs-lookup"><span data-stu-id="56b05-109">A key has the following properties:</span></span>

* <span data-ttu-id="56b05-110">アクティブ化、作成、および有効期限の日付</span><span class="sxs-lookup"><span data-stu-id="56b05-110">Activation, creation, and expiration dates</span></span>

* <span data-ttu-id="56b05-111">失効状態</span><span class="sxs-lookup"><span data-stu-id="56b05-111">Revocation status</span></span>

* <span data-ttu-id="56b05-112">キー識別子 (GUID)</span><span class="sxs-lookup"><span data-stu-id="56b05-112">Key identifier (a GUID)</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="56b05-113">さらに、`IKey`公開、`CreateEncryptor`メソッドを作成するために使用できる、 [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor)インスタンスは、このキーに関連付けられています。</span><span class="sxs-lookup"><span data-stu-id="56b05-113">Additionally, `IKey` exposes a `CreateEncryptor` method which can be used to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="56b05-114">さらに、`IKey`公開、`CreateEncryptorInstance`メソッドを作成するために使用できる、 [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor)インスタンスは、このキーに関連付けられています。</span><span class="sxs-lookup"><span data-stu-id="56b05-114">Additionally, `IKey` exposes a `CreateEncryptorInstance` method which can be used to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

::: moniker-end

> [!NOTE]
> <span data-ttu-id="56b05-115">生の暗号化マテリアルを取得する API はありません、`IKey`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="56b05-115">There's no API to retrieve the raw cryptographic material from an `IKey` instance.</span></span>

## <a name="ikeymanager"></a><span data-ttu-id="56b05-116">IKeyManager</span><span class="sxs-lookup"><span data-stu-id="56b05-116">IKeyManager</span></span>

<span data-ttu-id="56b05-117">`IKeyManager`インターフェイスは、一般的なキーの格納、取得、および操作を担当するオブジェクトを表します。</span><span class="sxs-lookup"><span data-stu-id="56b05-117">The `IKeyManager` interface represents an object responsible for general key storage, retrieval, and manipulation.</span></span> <span data-ttu-id="56b05-118">次の 3 つの高度な操作が公開しています。</span><span class="sxs-lookup"><span data-stu-id="56b05-118">It exposes three high-level operations:</span></span>

* <span data-ttu-id="56b05-119">新しいキーを作成し、ストレージに保存します。</span><span class="sxs-lookup"><span data-stu-id="56b05-119">Create a new key and persist it to storage.</span></span>

* <span data-ttu-id="56b05-120">記憶域からすべてのキーを取得します。</span><span class="sxs-lookup"><span data-stu-id="56b05-120">Get all keys from storage.</span></span>

* <span data-ttu-id="56b05-121">1 つまたは複数のキーを取り消すし、記憶域に失効情報を保持します。</span><span class="sxs-lookup"><span data-stu-id="56b05-121">Revoke one or more keys and persist the revocation information to storage.</span></span>

>[!WARNING]
> <span data-ttu-id="56b05-122">書き込み、`IKeyManager`非常に高度なタスクは、開発者の大部分が試行しないでください。</span><span class="sxs-lookup"><span data-stu-id="56b05-122">Writing an `IKeyManager` is a very advanced task, and the majority of developers shouldn't attempt it.</span></span> <span data-ttu-id="56b05-123">代わりに、ほとんどの開発者がによって提供される機能の利点を実行する必要があります、 [XmlKeyManager](#xmlkeymanager)クラス。</span><span class="sxs-lookup"><span data-stu-id="56b05-123">Instead, most developers should take advantage of the facilities offered by the [XmlKeyManager](#xmlkeymanager) class.</span></span>

## <a name="xmlkeymanager"></a><span data-ttu-id="56b05-124">XmlKeyManager</span><span class="sxs-lookup"><span data-stu-id="56b05-124">XmlKeyManager</span></span>

<span data-ttu-id="56b05-125">`XmlKeyManager`型がボックス内の具象実装の`IKeyManager`します。</span><span class="sxs-lookup"><span data-stu-id="56b05-125">The `XmlKeyManager` type is the in-box concrete implementation of `IKeyManager`.</span></span> <span data-ttu-id="56b05-126">キー エスクローおよび保存時のキーの暗号化を含むいくつかの便利な機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="56b05-126">It provides several useful facilities, including key escrow and encryption of keys at rest.</span></span> <span data-ttu-id="56b05-127">このシステム内のキーは XML 要素として表されます (具体的には、 [XElement](/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview))。</span><span class="sxs-lookup"><span data-stu-id="56b05-127">Keys in this system are represented as XML elements (specifically, [XElement](/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).</span></span>

<span data-ttu-id="56b05-128">`XmlKeyManager` そのタスクを実行する過程でその他のいくつかのコンポーネントに依存します。</span><span class="sxs-lookup"><span data-stu-id="56b05-128">`XmlKeyManager` depends on several other components in the course of fulfilling its tasks:</span></span>

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="56b05-129">`AlgorithmConfiguration`を新しいキーで使用されるアルゴリズムを決定します。</span><span class="sxs-lookup"><span data-stu-id="56b05-129">`AlgorithmConfiguration`, which dictates the algorithms used by new keys.</span></span>

* <span data-ttu-id="56b05-130">`IXmlRepository`、ストレージ キーが永続化するコントロール。</span><span class="sxs-lookup"><span data-stu-id="56b05-130">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="56b05-131">`IXmlEncryptor` [オプション]、保存時のキーを暗号化することができます。</span><span class="sxs-lookup"><span data-stu-id="56b05-131">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="56b05-132">`IKeyEscrowSink` [オプション]、キー エスクローのサービスを提供します。</span><span class="sxs-lookup"><span data-stu-id="56b05-132">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

* <span data-ttu-id="56b05-133">`IXmlRepository`、ストレージ キーが永続化するコントロール。</span><span class="sxs-lookup"><span data-stu-id="56b05-133">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="56b05-134">`IXmlEncryptor` [オプション]、保存時のキーを暗号化することができます。</span><span class="sxs-lookup"><span data-stu-id="56b05-134">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="56b05-135">`IKeyEscrowSink` [オプション]、キー エスクローのサービスを提供します。</span><span class="sxs-lookup"><span data-stu-id="56b05-135">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

::: moniker-end

<span data-ttu-id="56b05-136">内でこれらのコンポーネントをまとめてワイヤードは方法を示す高度な図を次に示します`XmlKeyManager`します。</span><span class="sxs-lookup"><span data-stu-id="56b05-136">Below are high-level diagrams which indicate how these components are wired together within `XmlKeyManager`.</span></span>

::: moniker range=">= aspnetcore-2.0"

![キーの作成](key-management/_static/keycreation2.png)

<span data-ttu-id="56b05-138">*キーの作成/CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="56b05-138">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="56b05-139">実装で`CreateNewKey`、`AlgorithmConfiguration`コンポーネントの作成に使用する一意`IAuthenticatedEncryptorDescriptor`が XML としてシリアル化し、先。</span><span class="sxs-lookup"><span data-stu-id="56b05-139">In the implementation of `CreateNewKey`, the `AlgorithmConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="56b05-140">キー エスクロー シンクが存在する場合は、長期的なストレージ、未処理の XML を (暗号化されていない) が、シンクに提供されます。</span><span class="sxs-lookup"><span data-stu-id="56b05-140">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="56b05-141">暗号化されていない XML 実行し、 `IXmlEncryptor` (必要な) 場合、暗号化された XML ドキュメントを生成します。</span><span class="sxs-lookup"><span data-stu-id="56b05-141">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="56b05-142">この暗号化されたドキュメントが使用して長期的なストレージに永続化、`IXmlRepository`します。</span><span class="sxs-lookup"><span data-stu-id="56b05-142">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="56b05-143">(ない場合は`IXmlEncryptor`が構成されている場合、暗号化されていないドキュメントに保存、 `IXmlRepository`)。</span><span class="sxs-lookup"><span data-stu-id="56b05-143">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

![キーの取得](key-management/_static/keyretrieval2.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

![キーの作成](key-management/_static/keycreation1.png)

<span data-ttu-id="56b05-146">*キーの作成/CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="56b05-146">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="56b05-147">実装で`CreateNewKey`、`IAuthenticatedEncryptorConfiguration`コンポーネントの作成に使用する一意`IAuthenticatedEncryptorDescriptor`が XML としてシリアル化し、先。</span><span class="sxs-lookup"><span data-stu-id="56b05-147">In the implementation of `CreateNewKey`, the `IAuthenticatedEncryptorConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="56b05-148">キー エスクロー シンクが存在する場合は、長期的なストレージ、未処理の XML を (暗号化されていない) が、シンクに提供されます。</span><span class="sxs-lookup"><span data-stu-id="56b05-148">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="56b05-149">暗号化されていない XML 実行し、 `IXmlEncryptor` (必要な) 場合、暗号化された XML ドキュメントを生成します。</span><span class="sxs-lookup"><span data-stu-id="56b05-149">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="56b05-150">この暗号化されたドキュメントが使用して長期的なストレージに永続化、`IXmlRepository`します。</span><span class="sxs-lookup"><span data-stu-id="56b05-150">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="56b05-151">(ない場合は`IXmlEncryptor`が構成されている場合、暗号化されていないドキュメントに保存、 `IXmlRepository`)。</span><span class="sxs-lookup"><span data-stu-id="56b05-151">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

![キーの取得](key-management/_static/keyretrieval1.png)

::: moniker-end

<span data-ttu-id="56b05-153">*キーの取得/GetAllKeys*</span><span class="sxs-lookup"><span data-stu-id="56b05-153">*Key Retrieval / GetAllKeys*</span></span>

<span data-ttu-id="56b05-154">実装で`GetAllKeys`、XML ドキュメントのキーを表すおよび失効は、基になるから読み取られる`IXmlRepository`します。</span><span class="sxs-lookup"><span data-stu-id="56b05-154">In the implementation of `GetAllKeys`, the XML documents representing keys and revocations are read from the underlying `IXmlRepository`.</span></span> <span data-ttu-id="56b05-155">これらのドキュメントが暗号化されている場合、システムは自動的に復号化します。</span><span class="sxs-lookup"><span data-stu-id="56b05-155">If these documents are encrypted, the system will automatically decrypt them.</span></span> <span data-ttu-id="56b05-156">`XmlKeyManager` 適切な作成`IAuthenticatedEncryptorDescriptorDeserializer`にドキュメントを逆シリアル化するインスタンスが再度`IAuthenticatedEncryptorDescriptor`インスタンスで、個々 にラップし、`IKey`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="56b05-156">`XmlKeyManager` creates the appropriate `IAuthenticatedEncryptorDescriptorDeserializer` instances to deserialize the documents back into `IAuthenticatedEncryptorDescriptor` instances, which are then wrapped in individual `IKey` instances.</span></span> <span data-ttu-id="56b05-157">このコレクションの`IKey`のインスタンスが、呼び出し元に返されます。</span><span class="sxs-lookup"><span data-stu-id="56b05-157">This collection of `IKey` instances is returned to the caller.</span></span>

<span data-ttu-id="56b05-158">特定の XML 要素の詳細についてで参照できる、[キー記憶域形式のドキュメント](xref:security/data-protection/implementation/key-storage-format#data-protection-implementation-key-storage-format)します。</span><span class="sxs-lookup"><span data-stu-id="56b05-158">Further information on the particular XML elements can be found in the [key storage format document](xref:security/data-protection/implementation/key-storage-format#data-protection-implementation-key-storage-format).</span></span>

## <a name="ixmlrepository"></a><span data-ttu-id="56b05-159">IXmlRepository</span><span class="sxs-lookup"><span data-stu-id="56b05-159">IXmlRepository</span></span>

<span data-ttu-id="56b05-160">`IXmlRepository`インターフェイスは、XML を永続化および XML をバッキング ストアから取得できる型を表します。</span><span class="sxs-lookup"><span data-stu-id="56b05-160">The `IXmlRepository` interface represents a type that can persist XML to and retrieve XML from a backing store.</span></span> <span data-ttu-id="56b05-161">2 つの Api が公開しています。</span><span class="sxs-lookup"><span data-stu-id="56b05-161">It exposes two APIs:</span></span>

* <span data-ttu-id="56b05-162">`GetAllElements` :`IReadOnlyCollection<XElement>`</span><span class="sxs-lookup"><span data-stu-id="56b05-162">`GetAllElements` :`IReadOnlyCollection<XElement>`</span></span>

* `StoreElement(XElement element, string friendlyName)`

<span data-ttu-id="56b05-163">実装`IXmlRepository`通過して XML を解析する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="56b05-163">Implementations of `IXmlRepository` don't need to parse the XML passing through them.</span></span> <span data-ttu-id="56b05-164">上位のレイヤーを生成して、ドキュメントの解析について心配を不透明として XML ドキュメントを処理させている必要があります。</span><span class="sxs-lookup"><span data-stu-id="56b05-164">They should treat the XML documents as opaque and let higher layers worry about generating and parsing the documents.</span></span>

<span data-ttu-id="56b05-165">実装している 4 つの組み込み具象型がある`IXmlRepository`:</span><span class="sxs-lookup"><span data-stu-id="56b05-165">There are four built-in concrete types which implement `IXmlRepository`:</span></span>

::: moniker range=">= aspnetcore-2.2"

* [<span data-ttu-id="56b05-166">FileSystemXmlRepository</span><span class="sxs-lookup"><span data-stu-id="56b05-166">FileSystemXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [<span data-ttu-id="56b05-167">RegistryXmlRepository</span><span class="sxs-lookup"><span data-stu-id="56b05-167">RegistryXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [<span data-ttu-id="56b05-168">AzureStorage.AzureBlobXmlRepository</span><span class="sxs-lookup"><span data-stu-id="56b05-168">AzureStorage.AzureBlobXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [<span data-ttu-id="56b05-169">RedisXmlRepository</span><span class="sxs-lookup"><span data-stu-id="56b05-169">RedisXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.stackexchangeredis.redisxmlrepository)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [<span data-ttu-id="56b05-170">FileSystemXmlRepository</span><span class="sxs-lookup"><span data-stu-id="56b05-170">FileSystemXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [<span data-ttu-id="56b05-171">RegistryXmlRepository</span><span class="sxs-lookup"><span data-stu-id="56b05-171">RegistryXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [<span data-ttu-id="56b05-172">AzureStorage.AzureBlobXmlRepository</span><span class="sxs-lookup"><span data-stu-id="56b05-172">AzureStorage.AzureBlobXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [<span data-ttu-id="56b05-173">RedisXmlRepository</span><span class="sxs-lookup"><span data-stu-id="56b05-173">RedisXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.redisxmlrepository)

::: moniker-end

<span data-ttu-id="56b05-174">参照してください、[キー記憶域プロバイダーのドキュメント](xref:security/data-protection/implementation/key-storage-providers)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="56b05-174">See the [key storage providers document](xref:security/data-protection/implementation/key-storage-providers) for more information.</span></span>

<span data-ttu-id="56b05-175">カスタムの登録`IXmlRepository`さまざまなバッキング ストア (たとえば、Azure テーブル ストレージ) を使用する場合に適しています。</span><span class="sxs-lookup"><span data-stu-id="56b05-175">Registering a custom `IXmlRepository` is appropriate when using a different backing store (for example, Azure Table Storage).</span></span>

<span data-ttu-id="56b05-176">アプリケーション全体の既定のリポジトリを変更するには、登録、カスタム`IXmlRepository`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="56b05-176">To change the default repository application-wide, register a custom `IXmlRepository` instance:</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
services.Configure<KeyManagementOptions>(options => options.XmlRepository = new MyCustomXmlRepository());
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
services.AddSingleton<IXmlRepository>(new MyCustomXmlRepository());
```

::: moniker-end

## <a name="ixmlencryptor"></a><span data-ttu-id="56b05-177">IXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="56b05-177">IXmlEncryptor</span></span>

<span data-ttu-id="56b05-178">`IXmlEncryptor`インターフェイスは、プレーン テキストの XML 要素を暗号化する型を表します。</span><span class="sxs-lookup"><span data-stu-id="56b05-178">The `IXmlEncryptor` interface represents a type that can encrypt a plaintext XML element.</span></span> <span data-ttu-id="56b05-179">1 つの API を公開しています。</span><span class="sxs-lookup"><span data-stu-id="56b05-179">It exposes a single API:</span></span>

* <span data-ttu-id="56b05-180">Encrypt(XElement plaintextElement) :EncryptedXmlInfo</span><span class="sxs-lookup"><span data-stu-id="56b05-180">Encrypt(XElement plaintextElement) : EncryptedXmlInfo</span></span>

<span data-ttu-id="56b05-181">場合、シリアル化された`IAuthenticatedEncryptorDescriptor`し、「暗号化を必要とする」としてマークされているすべての要素が含まれています`XmlKeyManager`経由、構成されたこれらの要素で実行される`IXmlEncryptor`の`Encrypt`メソッドをおよびそれにしたり、要素を保持ではなく、プレーン テキスト要素を`IXmlRepository`します。</span><span class="sxs-lookup"><span data-stu-id="56b05-181">If a serialized `IAuthenticatedEncryptorDescriptor` contains any elements marked as "requires encryption", then `XmlKeyManager` will run those elements through the configured `IXmlEncryptor`'s `Encrypt` method, and it will persist the enciphered element rather than the plaintext element to the `IXmlRepository`.</span></span> <span data-ttu-id="56b05-182">出力、`Encrypt`メソッドは、`EncryptedXmlInfo`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="56b05-182">The output of the `Encrypt` method is an `EncryptedXmlInfo` object.</span></span> <span data-ttu-id="56b05-183">このオブジェクトはしたり、両方の結果を含むラッパー`XElement`と型を表す、`IXmlDecryptor`の対応する要素を復号化に使用できます。</span><span class="sxs-lookup"><span data-stu-id="56b05-183">This object is a wrapper which contains both the resultant enciphered `XElement` and the Type which represents an `IXmlDecryptor` which can be used to decipher the corresponding element.</span></span>

<span data-ttu-id="56b05-184">実装している 4 つの組み込み具象型がある`IXmlEncryptor`:</span><span class="sxs-lookup"><span data-stu-id="56b05-184">There are four built-in concrete types which implement `IXmlEncryptor`:</span></span>

* [<span data-ttu-id="56b05-185">CertificateXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="56b05-185">CertificateXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.certificatexmlencryptor)
* [<span data-ttu-id="56b05-186">DpapiNGXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="56b05-186">DpapiNGXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapingxmlencryptor)
* [<span data-ttu-id="56b05-187">DpapiXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="56b05-187">DpapiXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapixmlencryptor)
* [<span data-ttu-id="56b05-188">NullXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="56b05-188">NullXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.nullxmlencryptor)

<span data-ttu-id="56b05-189">参照してください、 [rest のドキュメントでキーの暗号化](xref:security/data-protection/implementation/key-encryption-at-rest)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="56b05-189">See the [key encryption at rest document](xref:security/data-protection/implementation/key-encryption-at-rest) for more information.</span></span>

<span data-ttu-id="56b05-190">既定の保存にキーの暗号化メカニズム アプリケーション全体を変更するに登録するカスタム`IXmlEncryptor`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="56b05-190">To change the default key-encryption-at-rest mechanism application-wide, register a custom `IXmlEncryptor` instance:</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
services.Configure<KeyManagementOptions>(options => options.XmlEncryptor = new MyCustomXmlEncryptor());
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
services.AddSingleton<IXmlEncryptor>(new MyCustomXmlEncryptor());
```

::: moniker-end

## <a name="ixmldecryptor"></a><span data-ttu-id="56b05-191">IXmlDecryptor</span><span class="sxs-lookup"><span data-stu-id="56b05-191">IXmlDecryptor</span></span>

<span data-ttu-id="56b05-192">`IXmlDecryptor`インターフェイスは、暗号化を解除する方法を認識する型を表す、`XElement`がしたりを使用して、`IXmlEncryptor`します。</span><span class="sxs-lookup"><span data-stu-id="56b05-192">The `IXmlDecryptor` interface represents a type that knows how to decrypt an `XElement` that was enciphered via an `IXmlEncryptor`.</span></span> <span data-ttu-id="56b05-193">1 つの API を公開しています。</span><span class="sxs-lookup"><span data-stu-id="56b05-193">It exposes a single API:</span></span>

* <span data-ttu-id="56b05-194">(XElement encryptedElement) の暗号化を解除します。XElement</span><span class="sxs-lookup"><span data-stu-id="56b05-194">Decrypt(XElement encryptedElement) : XElement</span></span>

<span data-ttu-id="56b05-195">`Decrypt`メソッドによって実行された暗号化を元に戻します`IXmlEncryptor.Encrypt`します。</span><span class="sxs-lookup"><span data-stu-id="56b05-195">The `Decrypt` method undoes the encryption performed by `IXmlEncryptor.Encrypt`.</span></span> <span data-ttu-id="56b05-196">一般に、各具象`IXmlEncryptor`対応する具体的な実装が必要があります`IXmlDecryptor`実装します。</span><span class="sxs-lookup"><span data-stu-id="56b05-196">Generally, each concrete `IXmlEncryptor` implementation will have a corresponding concrete `IXmlDecryptor` implementation.</span></span>

<span data-ttu-id="56b05-197">実装する型`IXmlDecryptor`次の 2 つのパブリック コンストラクターのいずれかである必要があります。</span><span class="sxs-lookup"><span data-stu-id="56b05-197">Types which implement `IXmlDecryptor` should have one of the following two public constructors:</span></span>

* <span data-ttu-id="56b05-198">.ctor(IServiceProvider)</span><span class="sxs-lookup"><span data-stu-id="56b05-198">.ctor(IServiceProvider)</span></span>
* <span data-ttu-id="56b05-199">.ctor()</span><span class="sxs-lookup"><span data-stu-id="56b05-199">.ctor()</span></span>

> [!NOTE]
> <span data-ttu-id="56b05-200">`IServiceProvider`に渡されるコンストラクターを null にすることがあります。</span><span class="sxs-lookup"><span data-stu-id="56b05-200">The `IServiceProvider` passed to the constructor may be null.</span></span>

## <a name="ikeyescrowsink"></a><span data-ttu-id="56b05-201">IKeyEscrowSink</span><span class="sxs-lookup"><span data-stu-id="56b05-201">IKeyEscrowSink</span></span>

<span data-ttu-id="56b05-202">`IKeyEscrowSink`インターフェイスは、機密情報のエスクローを実行できる型を表します。</span><span class="sxs-lookup"><span data-stu-id="56b05-202">The `IKeyEscrowSink` interface represents a type that can perform escrow of sensitive information.</span></span> <span data-ttu-id="56b05-203">シリアル化された記述子は、(暗号化マテリアルの場合) などの機密情報を含む可能性があります、これは、導入に至った経緯を思い出してください、 [IXmlEncryptor](#ixmlencryptor)最初に入力します。</span><span class="sxs-lookup"><span data-stu-id="56b05-203">Recall that serialized descriptors might contain sensitive information (such as cryptographic material), and this is what led to the introduction of the [IXmlEncryptor](#ixmlencryptor) type in the first place.</span></span> <span data-ttu-id="56b05-204">ただし、事故が発生して、キー リングが削除できるは、または破損します。</span><span class="sxs-lookup"><span data-stu-id="56b05-204">However, accidents happen, and key rings can be deleted or become corrupted.</span></span>

<span data-ttu-id="56b05-205">エスクロー インターフェイスを使用する、いずれかの構成によって変換される前に、生のシリアル化された XML へのアクセスを許可、緊急のエスケープ ハッチ[IXmlEncryptor](#ixmlencryptor)します。</span><span class="sxs-lookup"><span data-stu-id="56b05-205">The escrow interface provides an emergency escape hatch, allowing access to the raw serialized XML before it's transformed by any configured [IXmlEncryptor](#ixmlencryptor).</span></span> <span data-ttu-id="56b05-206">インターフェイスは、1 つの API を公開します。</span><span class="sxs-lookup"><span data-stu-id="56b05-206">The interface exposes a single API:</span></span>

* <span data-ttu-id="56b05-207">ストア (Guid keyId、XElement 要素)</span><span class="sxs-lookup"><span data-stu-id="56b05-207">Store(Guid keyId, XElement element)</span></span>

<span data-ttu-id="56b05-208">`IKeyEscrowSink`ビジネス ポリシーを使用して一貫性のある安全な方法で、指定した要素を処理するために実装します。</span><span class="sxs-lookup"><span data-stu-id="56b05-208">It's up to the `IKeyEscrowSink` implementation to handle the provided element in a secure manner consistent with business policy.</span></span> <span data-ttu-id="56b05-209">場所は、証明書の秘密キーはエスクローされました。 既知の企業 X.509 証明書を使用して XML 要素を暗号化するエスクロー シンクの 1 つの考えられる実装可能性があります。`CertificateXmlEncryptor`型がこれを支援するためです。</span><span class="sxs-lookup"><span data-stu-id="56b05-209">One possible implementation could be for the escrow sink to encrypt the XML element using a known corporate X.509 certificate where the certificate's private key has been escrowed; the `CertificateXmlEncryptor` type can assist with this.</span></span> <span data-ttu-id="56b05-210">`IKeyEscrowSink`実装は、指定した要素を適切に永続化することもできます。</span><span class="sxs-lookup"><span data-stu-id="56b05-210">The `IKeyEscrowSink` implementation is also responsible for persisting the provided element appropriately.</span></span>

<span data-ttu-id="56b05-211">既定ではエスクロー メカニズムが有効でない、サーバー管理者は、[これをグローバルに構成](xref:security/data-protection/configuration/machine-wide-policy)します。</span><span class="sxs-lookup"><span data-stu-id="56b05-211">By default no escrow mechanism is enabled, though server administrators can [configure this globally](xref:security/data-protection/configuration/machine-wide-policy).</span></span> <span data-ttu-id="56b05-212">構成することもプログラムを使用して、`IDataProtectionBuilder.AddKeyEscrowSink`メソッドを次の例で示すようにします。</span><span class="sxs-lookup"><span data-stu-id="56b05-212">It can also be configured programmatically via the `IDataProtectionBuilder.AddKeyEscrowSink` method as shown in the sample below.</span></span> <span data-ttu-id="56b05-213">`AddKeyEscrowSink`メソッドのオーバー ロードのミラー、`IServiceCollection.AddSingleton`と`IServiceCollection.AddInstance`オーバー ロードとして`IKeyEscrowSink`インスタンスはシングルトンとして意図されています。</span><span class="sxs-lookup"><span data-stu-id="56b05-213">The `AddKeyEscrowSink` method overloads mirror the `IServiceCollection.AddSingleton` and `IServiceCollection.AddInstance` overloads, as `IKeyEscrowSink` instances are intended to be singletons.</span></span> <span data-ttu-id="56b05-214">複数`IKeyEscrowSink`インスタンスが登録されて、キーを同時に複数のメカニズムをエスクローできるようにそのそれぞれがキーの生成中に呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="56b05-214">If multiple `IKeyEscrowSink` instances are registered, each one will be called during key generation, so keys can be escrowed to multiple mechanisms simultaneously.</span></span>

<span data-ttu-id="56b05-215">API から情報を読み取るにはありません、`IKeyEscrowSink`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="56b05-215">There's no API to read material from an `IKeyEscrowSink` instance.</span></span> <span data-ttu-id="56b05-216">これはエスクロー メカニズムの設計理論と一致します。 キー マテリアルを信頼された証明機関にアクセスできるように意図したものと独自 escrowed マテリアルへのアクセスを必要はありません、アプリケーション自体がない信頼された証明機関であるためです。</span><span class="sxs-lookup"><span data-stu-id="56b05-216">This is consistent with the design theory of the escrow mechanism: it's intended to make the key material accessible to a trusted authority, and since the application is itself not a trusted authority, it shouldn't have access to its own escrowed material.</span></span>

<span data-ttu-id="56b05-217">次のサンプル コードを示しますの作成と登録、 `IKeyEscrowSink` "CONTOSODomain Admins"のメンバーだけが回復できるようにキーをエスクローする場所。</span><span class="sxs-lookup"><span data-stu-id="56b05-217">The following sample code demonstrates creating and registering an `IKeyEscrowSink` where keys are escrowed such that only members of "CONTOSODomain Admins" can recover them.</span></span>

> [!NOTE]
> <span data-ttu-id="56b05-218">このサンプルを実行する Windows 8 のドメインに参加していることが必要]、[Windows Server 2012 マシン、およびドメイン コント ローラーが Windows Server 2012 またはそれ以降にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="56b05-218">To run this sample, you must be on a domain-joined Windows 8 / Windows Server 2012 machine, and the domain controller must be Windows Server 2012 or later.</span></span>

[!code-csharp[](key-management/samples/key-management-extensibility.cs)]
