---
title: "キー管理の機能拡張"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 3606b251-8324-4485-8d52-582a2cd5cffb
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: ed84b6dc257d5fd9e4c1cf6106df3c8bd6e14f64
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/12/2017
---
# <a name="key-management-extensibility"></a><span data-ttu-id="e7782-103">キー管理の機能拡張</span><span class="sxs-lookup"><span data-stu-id="e7782-103">Key management extensibility</span></span>

<a name=data-protection-extensibility-key-management></a>

>[!TIP]
> <span data-ttu-id="e7782-104">読み取り、[キー管理](../implementation/key-management.md#data-protection-implementation-key-management)ようにこれらの Api の背後にある基本的な概念について説明します、このセクションを読む前にセクションです。</span><span class="sxs-lookup"><span data-stu-id="e7782-104">Read the [key management](../implementation/key-management.md#data-protection-implementation-key-management) section before reading this section, as it explains some of the fundamental concepts behind these APIs.</span></span>

>[!WARNING]
> <span data-ttu-id="e7782-105">次のインターフェイスのいずれかを実装する型はスレッド セーフである必要があります複数の呼び出し元のです。</span><span class="sxs-lookup"><span data-stu-id="e7782-105">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="key"></a><span data-ttu-id="e7782-106">キー</span><span class="sxs-lookup"><span data-stu-id="e7782-106">Key</span></span>

<span data-ttu-id="e7782-107">IKey インターフェイスは、暗号方式のキーの基本表現です。</span><span class="sxs-lookup"><span data-stu-id="e7782-107">The IKey interface is the basic representation of a key in cryptosystem.</span></span> <span data-ttu-id="e7782-108">用語キーは、抽象の意味での「暗号キー マテリアル」文字どおりの意味ではなくここで使用されます。</span><span class="sxs-lookup"><span data-stu-id="e7782-108">The term key is used here in the abstract sense, not in the literal sense of "cryptographic key material".</span></span> <span data-ttu-id="e7782-109">キーには、次のプロパティがあります。</span><span class="sxs-lookup"><span data-stu-id="e7782-109">A key has the following properties:</span></span>

* <span data-ttu-id="e7782-110">アクティブ化、作成、および有効期限の日付</span><span class="sxs-lookup"><span data-stu-id="e7782-110">Activation, creation, and expiration dates</span></span>

* <span data-ttu-id="e7782-111">失効状態</span><span class="sxs-lookup"><span data-stu-id="e7782-111">Revocation status</span></span>

* <span data-ttu-id="e7782-112">キー識別子 (GUID)</span><span class="sxs-lookup"><span data-stu-id="e7782-112">Key identifier (a GUID)</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e7782-113">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e7782-113">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="e7782-114">IKey が CreateEncryptor メソッドを作成するために使用できるを公開するさらに、 [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor)インスタンスは、このキーに関連付けられています。</span><span class="sxs-lookup"><span data-stu-id="e7782-114">Additionally, IKey exposes a CreateEncryptor method which can be used to create an [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e7782-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e7782-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="e7782-116">IKey が CreateEncryptorInstance メソッドを作成するために使用できるを公開するさらに、 [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor)インスタンスは、このキーに関連付けられています。</span><span class="sxs-lookup"><span data-stu-id="e7782-116">Additionally, IKey exposes a CreateEncryptorInstance method which can be used to create an [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

---

> [!NOTE]
> <span data-ttu-id="e7782-117">生の暗号化マテリアルを IKey インスタンスから取得する API はありません。</span><span class="sxs-lookup"><span data-stu-id="e7782-117">There is no API to retrieve the raw cryptographic material from an IKey instance.</span></span>

## <a name="ikeymanager"></a><span data-ttu-id="e7782-118">IKeyManager</span><span class="sxs-lookup"><span data-stu-id="e7782-118">IKeyManager</span></span>

<span data-ttu-id="e7782-119">IKeyManager インターフェイスは、一般的なキーの格納、取得、および操作を担当するオブジェクトを表します。</span><span class="sxs-lookup"><span data-stu-id="e7782-119">The IKeyManager interface represents an object responsible for general key storage, retrieval, and manipulation.</span></span> <span data-ttu-id="e7782-120">3 つの高度な操作を公開します。</span><span class="sxs-lookup"><span data-stu-id="e7782-120">It exposes three high-level operations:</span></span>

* <span data-ttu-id="e7782-121">新しいキーを作成し、記憶域に保存します。</span><span class="sxs-lookup"><span data-stu-id="e7782-121">Create a new key and persist it to storage.</span></span>

* <span data-ttu-id="e7782-122">記憶域からすべてのキーを取得します。</span><span class="sxs-lookup"><span data-stu-id="e7782-122">Get all keys from storage.</span></span>

* <span data-ttu-id="e7782-123">1 つまたは複数のキーを取り消すし、記憶域に失効情報の保持されます。</span><span class="sxs-lookup"><span data-stu-id="e7782-123">Revoke one or more keys and persist the revocation information to storage.</span></span>

>[!WARNING]
> <span data-ttu-id="e7782-124">IKeyManager を記述、非常に高度なタスクは、開発者の大部分しないで。</span><span class="sxs-lookup"><span data-stu-id="e7782-124">Writing an IKeyManager is a very advanced task, and the majority of developers should not attempt it.</span></span> <span data-ttu-id="e7782-125">代わりに、ほとんどの開発者を活用してによって提供される機能、 [XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager)クラスです。</span><span class="sxs-lookup"><span data-stu-id="e7782-125">Instead, most developers should take advantage of the facilities offered by the [XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager) class.</span></span>

<a name=data-protection-extensibility-key-management-xmlkeymanager></a>

## <a name="xmlkeymanager"></a><span data-ttu-id="e7782-126">XmlKeyManager</span><span class="sxs-lookup"><span data-stu-id="e7782-126">XmlKeyManager</span></span>

<span data-ttu-id="e7782-127">XmlKeyManager 型は、IKeyManager のボックスでの具象実装です。</span><span class="sxs-lookup"><span data-stu-id="e7782-127">The XmlKeyManager type is the in-box concrete implementation of IKeyManager.</span></span> <span data-ttu-id="e7782-128">キー エスクロー、残りの部分でのキーの暗号化など、いくつかに役立つ機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="e7782-128">It provides several useful facilities, including key escrow and encryption of keys at rest.</span></span> <span data-ttu-id="e7782-129">このシステム内のキーは、XML 要素として表現されます (具体的には、 [XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)です。</span><span class="sxs-lookup"><span data-stu-id="e7782-129">Keys in this system are represented as XML elements (specifically, [XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview).</span></span>

<span data-ttu-id="e7782-130">XmlKeyManager は、そのタスクを満たす操作の過程でその他のいくつかのコンポーネントに依存します。</span><span class="sxs-lookup"><span data-stu-id="e7782-130">XmlKeyManager depends on several other components in the course of fulfilling its tasks:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e7782-131">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e7782-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* <span data-ttu-id="e7782-132">AlgorithmConfiguration は、新しいキーで使用されるアルゴリズムを決定します。</span><span class="sxs-lookup"><span data-stu-id="e7782-132">AlgorithmConfiguration, which dictates the algorithms used by new keys.</span></span>

* <span data-ttu-id="e7782-133">IXmlRepository、どのコントロールのキーがストレージに保存されます。</span><span class="sxs-lookup"><span data-stu-id="e7782-133">IXmlRepository, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="e7782-134">IXmlEncryptor [省略可能] が静止したキーを暗号化できます。</span><span class="sxs-lookup"><span data-stu-id="e7782-134">IXmlEncryptor [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="e7782-135">IKeyEscrowSink [オプション]、キー エスクロー サービスを提供します。</span><span class="sxs-lookup"><span data-stu-id="e7782-135">IKeyEscrowSink [optional], which provides key escrow services.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e7782-136">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e7782-136">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="e7782-137">IXmlRepository、どのコントロールのキーがストレージに保存されます。</span><span class="sxs-lookup"><span data-stu-id="e7782-137">IXmlRepository, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="e7782-138">IXmlEncryptor [省略可能] が静止したキーを暗号化できます。</span><span class="sxs-lookup"><span data-stu-id="e7782-138">IXmlEncryptor [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="e7782-139">IKeyEscrowSink [オプション]、キー エスクロー サービスを提供します。</span><span class="sxs-lookup"><span data-stu-id="e7782-139">IKeyEscrowSink [optional], which provides key escrow services.</span></span>

---

<span data-ttu-id="e7782-140">どのように、これらのコンポーネントは XmlKeyManager 内で一緒にワイヤードを示す高度な図を次に示します。</span><span class="sxs-lookup"><span data-stu-id="e7782-140">Below are high-level diagrams which indicate how these components are wired together within XmlKeyManager.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e7782-141">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e7782-141">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ![キーの作成](key-management/_static/keycreation2.png)

   <span data-ttu-id="e7782-143">*キーの作成/CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="e7782-143">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="e7782-144">CreateNewKey の実装で、一意な IAuthenticatedEncryptorDescriptor は、XML としてシリアル化しを作成する AlgorithmConfiguration コンポーネントを使用します。</span><span class="sxs-lookup"><span data-stu-id="e7782-144">In the implementation of CreateNewKey, the AlgorithmConfiguration component is used to create a unique IAuthenticatedEncryptorDescriptor, which is then serialized as XML.</span></span> <span data-ttu-id="e7782-145">キー エスクロー シンクが存在する場合は、長期的な記憶域の生の (暗号化されていない) XML は、シンクに提供されます。</span><span class="sxs-lookup"><span data-stu-id="e7782-145">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="e7782-146">暗号化されていない XML は、実行、IXmlEncryptor を通じて (必要な場合)、暗号化された XML ドキュメントが生成されます。</span><span class="sxs-lookup"><span data-stu-id="e7782-146">The unencrypted XML is then run through an IXmlEncryptor (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="e7782-147">この暗号化されたドキュメントは、IXmlRepository 経由での長期的なストレージに保存されます。</span><span class="sxs-lookup"><span data-stu-id="e7782-147">This encrypted document is persisted to long-term storage via the IXmlRepository.</span></span> <span data-ttu-id="e7782-148">(IXmlEncryptor が構成されていない暗号化されていないドキュメントに保存するか、IXmlRepository。)</span><span class="sxs-lookup"><span data-stu-id="e7782-148">(If no IXmlEncryptor is configured, the unencrypted document is persisted in the IXmlRepository.)</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e7782-149">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e7782-149">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ![キーの作成](key-management/_static/keycreation1.png)

   <span data-ttu-id="e7782-151">*キーの作成/CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="e7782-151">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="e7782-152">CreateNewKey の実装で、一意な IAuthenticatedEncryptorDescriptor は、XML としてシリアル化しを作成する IAuthenticatedEncryptorConfiguration コンポーネントを使用します。</span><span class="sxs-lookup"><span data-stu-id="e7782-152">In the implementation of CreateNewKey, the IAuthenticatedEncryptorConfiguration component is used to create a unique IAuthenticatedEncryptorDescriptor, which is then serialized as XML.</span></span> <span data-ttu-id="e7782-153">キー エスクロー シンクが存在する場合は、長期的な記憶域の生の (暗号化されていない) XML は、シンクに提供されます。</span><span class="sxs-lookup"><span data-stu-id="e7782-153">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="e7782-154">暗号化されていない XML は、実行、IXmlEncryptor を通じて (必要な場合)、暗号化された XML ドキュメントが生成されます。</span><span class="sxs-lookup"><span data-stu-id="e7782-154">The unencrypted XML is then run through an IXmlEncryptor (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="e7782-155">この暗号化されたドキュメントは、IXmlRepository 経由での長期的なストレージに保存されます。</span><span class="sxs-lookup"><span data-stu-id="e7782-155">This encrypted document is persisted to long-term storage via the IXmlRepository.</span></span> <span data-ttu-id="e7782-156">(IXmlEncryptor が構成されていない暗号化されていないドキュメントに保存するか、IXmlRepository。)</span><span class="sxs-lookup"><span data-stu-id="e7782-156">(If no IXmlEncryptor is configured, the unencrypted document is persisted in the IXmlRepository.)</span></span>

---

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e7782-157">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e7782-157">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ![キーの取得](key-management/_static/keyretrieval2.png)
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e7782-159">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e7782-159">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ![キーの取得](key-management/_static/keyretrieval1.png)

---

   <span data-ttu-id="e7782-161">*キーの取得/GetAllKeys*</span><span class="sxs-lookup"><span data-stu-id="e7782-161">*Key Retrieval / GetAllKeys*</span></span>

<span data-ttu-id="e7782-162">GetAllKeys の実装では、キーと失効を表す XML ドキュメントは、基になる IXmlRepository から読み取られます。</span><span class="sxs-lookup"><span data-stu-id="e7782-162">In the implementation of GetAllKeys, the XML documents representing keys and revocations are read from the underlying IXmlRepository.</span></span> <span data-ttu-id="e7782-163">これらのドキュメントが暗号化されている場合、システムは自動的に復号化にします。</span><span class="sxs-lookup"><span data-stu-id="e7782-163">If these documents are encrypted, the system will automatically decrypt them.</span></span> <span data-ttu-id="e7782-164">XmlKeyManager では、個々 の IKey インスタンスで、ラップされている IAuthenticatedEncryptorDescriptor インスタンスに戻す、ドキュメントを逆シリアル化する適切な IAuthenticatedEncryptorDescriptorDeserializer インスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="e7782-164">XmlKeyManager creates the appropriate IAuthenticatedEncryptorDescriptorDeserializer instances to deserialize the documents back into IAuthenticatedEncryptorDescriptor instances, which are then wrapped in individual IKey instances.</span></span> <span data-ttu-id="e7782-165">この IKey インスタンスのコレクションは、呼び出し元に返されます。</span><span class="sxs-lookup"><span data-stu-id="e7782-165">This collection of IKey instances is returned to the caller.</span></span>

<span data-ttu-id="e7782-166">特定の XML 要素の詳細についてで参照できる、[キーの格納形式のドキュメント](../implementation/key-storage-format.md#data-protection-implementation-key-storage-format)です。</span><span class="sxs-lookup"><span data-stu-id="e7782-166">Further information on the particular XML elements can be found in the [key storage format document](../implementation/key-storage-format.md#data-protection-implementation-key-storage-format).</span></span>

## <a name="ixmlrepository"></a><span data-ttu-id="e7782-167">IXmlRepository</span><span class="sxs-lookup"><span data-stu-id="e7782-167">IXmlRepository</span></span>

<span data-ttu-id="e7782-168">IXmlRepository インターフェイスは、XML を永続化したり、バッキング ストアから XML を取得できる型を表します。</span><span class="sxs-lookup"><span data-stu-id="e7782-168">The IXmlRepository interface represents a type that can persist XML to and retrieve XML from a backing store.</span></span> <span data-ttu-id="e7782-169">2 つの Api を公開します。</span><span class="sxs-lookup"><span data-stu-id="e7782-169">It exposes two APIs:</span></span>

* <span data-ttu-id="e7782-170">GetAllElements(): IReadOnlyCollection<XElement></span><span class="sxs-lookup"><span data-stu-id="e7782-170">GetAllElements() : IReadOnlyCollection<XElement></span></span>

* <span data-ttu-id="e7782-171">StoreElement (XElement 要素、文字列の friendlyName)</span><span class="sxs-lookup"><span data-stu-id="e7782-171">StoreElement(XElement element, string friendlyName)</span></span>

<span data-ttu-id="e7782-172">IXmlRepository の実装は、それらに渡される XML の解析に必要ありません。</span><span class="sxs-lookup"><span data-stu-id="e7782-172">Implementations of IXmlRepository don't need to parse the XML passing through them.</span></span> <span data-ttu-id="e7782-173">上位のレイヤーを生成して、ドキュメントの解析について心配を非透過的に XML ドキュメントを扱うさせている必要があります。</span><span class="sxs-lookup"><span data-stu-id="e7782-173">They should treat the XML documents as opaque and let higher layers worry about generating and parsing the documents.</span></span>

<span data-ttu-id="e7782-174">IXmlRepository を実装する 2 つの組み込み具体的な種類があります: FileSystemXmlRepository と RegistryXmlRepository です。</span><span class="sxs-lookup"><span data-stu-id="e7782-174">There are two built-in concrete types which implement IXmlRepository: FileSystemXmlRepository and RegistryXmlRepository.</span></span> <span data-ttu-id="e7782-175">参照してください、[キー記憶域プロバイダーのドキュメント](../implementation/key-storage-providers.md#data-protection-implementation-key-storage-providers)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="e7782-175">See the [key storage providers document](../implementation/key-storage-providers.md#data-protection-implementation-key-storage-providers) for more information.</span></span> <span data-ttu-id="e7782-176">カスタム IXmlRepository を登録するとする別に使用する適切な方法バッキング ストア、Azure Blob ストレージなどです。</span><span class="sxs-lookup"><span data-stu-id="e7782-176">Registering a custom IXmlRepository would be the appropriate manner to use a different backing store, e.g., Azure Blob Storage.</span></span> <span data-ttu-id="e7782-177">既定リポジトリ アプリケーション全体を変更するには、サービス プロバイダーのカスタム シングルトン IXmlRepository を登録します。</span><span class="sxs-lookup"><span data-stu-id="e7782-177">To change the default repository application-wide, register a custom singleton IXmlRepository in the service provider.</span></span>

<a name=data-protection-extensibility-key-management-ixmlencryptor></a>

## <a name="ixmlencryptor"></a><span data-ttu-id="e7782-178">IXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="e7782-178">IXmlEncryptor</span></span>

<span data-ttu-id="e7782-179">IXmlEncryptor インターフェイスは、プレーン テキストの XML 要素を暗号化する型を表します。</span><span class="sxs-lookup"><span data-stu-id="e7782-179">The IXmlEncryptor interface represents a type that can encrypt a plaintext XML element.</span></span> <span data-ttu-id="e7782-180">単一の API を公開します。</span><span class="sxs-lookup"><span data-stu-id="e7782-180">It exposes a single API:</span></span>

* <span data-ttu-id="e7782-181">(XElement plaintextElement) の暗号化: EncryptedXmlInfo</span><span class="sxs-lookup"><span data-stu-id="e7782-181">Encrypt(XElement plaintextElement) : EncryptedXmlInfo</span></span>

<span data-ttu-id="e7782-182">XmlKeyManager が構成されている IXmlEncryptor の暗号化の方法でこれらの要素を実行し、シリアル化された IAuthenticatedEncryptorDescriptor「暗号化が必要」としてマークされているすべての要素が含まれている場合、および enciphered 要素ではなく保持されます。IXmlRepository にプレーン テキストの要素。</span><span class="sxs-lookup"><span data-stu-id="e7782-182">If a serialized IAuthenticatedEncryptorDescriptor contains any elements marked as "requires encryption", then XmlKeyManager will run those elements through the configured IXmlEncryptor's Encrypt method, and it will persist the enciphered element rather than the plaintext element to the IXmlRepository.</span></span> <span data-ttu-id="e7782-183">暗号化のメソッドの出力は、EncryptedXmlInfo オブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="e7782-183">The output of the Encrypt method is an EncryptedXmlInfo object.</span></span> <span data-ttu-id="e7782-184">このオブジェクトは、結果の enciphered XElement との対応する要素を復号化するために使用できる IXmlDecryptor を表す型の両方を含むラッパーです。</span><span class="sxs-lookup"><span data-stu-id="e7782-184">This object is a wrapper which contains both the resultant enciphered XElement and the Type which represents an IXmlDecryptor which can be used to decipher the corresponding element.</span></span>

<span data-ttu-id="e7782-185">IXmlEncryptor を実装している 4 つの組み込み具体的な種類があります: CertificateXmlEncryptor、DpapiNGXmlEncryptor、DpapiXmlEncryptor、および NullXmlEncryptor です。</span><span class="sxs-lookup"><span data-stu-id="e7782-185">There are four built-in concrete types which implement IXmlEncryptor: CertificateXmlEncryptor, DpapiNGXmlEncryptor, DpapiXmlEncryptor, and NullXmlEncryptor.</span></span> <span data-ttu-id="e7782-186">参照してください、 [rest ドキュメント キーの暗号化](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="e7782-186">See the [key encryption at rest document](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) for more information.</span></span> <span data-ttu-id="e7782-187">既定のキーの暗号化に rest メカニズム アプリケーション全体を変更するには、サービス プロバイダーのカスタム シングルトン IXmlEncryptor を登録します。</span><span class="sxs-lookup"><span data-stu-id="e7782-187">To change the default key-encryption-at-rest mechanism application-wide, register a custom singleton IXmlEncryptor in the service provider.</span></span>

## <a name="ixmldecryptor"></a><span data-ttu-id="e7782-188">IXmlDecryptor</span><span class="sxs-lookup"><span data-stu-id="e7782-188">IXmlDecryptor</span></span>

<span data-ttu-id="e7782-189">IXmlDecryptor インターフェイスは、型、IXmlEncryptor を介して enciphered が XElement の暗号化を解除する方法を把握していることを表します。</span><span class="sxs-lookup"><span data-stu-id="e7782-189">The IXmlDecryptor interface represents a type that knows how to decrypt an XElement that was enciphered via an IXmlEncryptor.</span></span> <span data-ttu-id="e7782-190">単一の API を公開します。</span><span class="sxs-lookup"><span data-stu-id="e7782-190">It exposes a single API:</span></span>

* <span data-ttu-id="e7782-191">(XElement encryptedElement) を暗号化解除: XElement</span><span class="sxs-lookup"><span data-stu-id="e7782-191">Decrypt(XElement encryptedElement) : XElement</span></span>

<span data-ttu-id="e7782-192">復号化メソッドは、IXmlEncryptor.Encrypt によって実行された暗号化を元に戻します。</span><span class="sxs-lookup"><span data-stu-id="e7782-192">The Decrypt method undoes the encryption performed by IXmlEncryptor.Encrypt.</span></span> <span data-ttu-id="e7782-193">通常、各具象 IXmlEncryptor 実装では、対応する IXmlDecryptor 具象があります。</span><span class="sxs-lookup"><span data-stu-id="e7782-193">Generally each concrete IXmlEncryptor implementation will have a corresponding concrete IXmlDecryptor implementation.</span></span>

<span data-ttu-id="e7782-194">IXmlDecryptor を実装する型は、次の 2 つのパブリック コンス トラクターのいずれかが必要です。</span><span class="sxs-lookup"><span data-stu-id="e7782-194">Types which implement IXmlDecryptor should have one of the following two public constructors:</span></span>

* <span data-ttu-id="e7782-195">.ctor(IServiceProvider)</span><span class="sxs-lookup"><span data-stu-id="e7782-195">.ctor(IServiceProvider)</span></span>

* <span data-ttu-id="e7782-196">.ctor()</span><span class="sxs-lookup"><span data-stu-id="e7782-196">.ctor()</span></span>

> [!NOTE]
> <span data-ttu-id="e7782-197">コンス トラクターに渡される IServiceProvider を null にすることがあります。</span><span class="sxs-lookup"><span data-stu-id="e7782-197">The IServiceProvider passed to the constructor may be null.</span></span>

## <a name="ikeyescrowsink"></a><span data-ttu-id="e7782-198">IKeyEscrowSink</span><span class="sxs-lookup"><span data-stu-id="e7782-198">IKeyEscrowSink</span></span>

<span data-ttu-id="e7782-199">IKeyEscrowSink インターフェイスは、機密情報のエスクローを実行できる型を表します。</span><span class="sxs-lookup"><span data-stu-id="e7782-199">The IKeyEscrowSink interface represents a type that can perform escrow of sensitive information.</span></span> <span data-ttu-id="e7782-200">導入にこれは、シリアル化された記述子は、(暗号化マテリアル) などの機密情報を含む可能性がありますに注意してください、 [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor)最初の場所を入力します。</span><span class="sxs-lookup"><span data-stu-id="e7782-200">Recall that serialized descriptors might contain sensitive information (such as cryptographic material), and this is what led to the introduction of the [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor) type in the first place.</span></span> <span data-ttu-id="e7782-201">ただし、事故発生する可能性があり、keyrings 削除できるか、または壊れています。</span><span class="sxs-lookup"><span data-stu-id="e7782-201">However, accidents happen, and keyrings can be deleted or become corrupted.</span></span>

<span data-ttu-id="e7782-202">エスクロー インターフェイスには、構成されているいずれかが変換される前に、生のシリアル化された XML へのアクセスを許可、緊急脱出口が用意されています[IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor)です。</span><span class="sxs-lookup"><span data-stu-id="e7782-202">The escrow interface provides an emergency escape hatch, allowing access to the raw serialized XML before it is transformed by any configured [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor).</span></span> <span data-ttu-id="e7782-203">インターフェイスは、単一の API を公開します。</span><span class="sxs-lookup"><span data-stu-id="e7782-203">The interface exposes a single API:</span></span>

* <span data-ttu-id="e7782-204">ストア (Guid keyId、XElement 要素)</span><span class="sxs-lookup"><span data-stu-id="e7782-204">Store(Guid keyId, XElement element)</span></span>

<span data-ttu-id="e7782-205">ビジネス ポリシーと一致するセキュリティで保護された方法で指定した要素を処理する IKeyEscrowSink 実装の責任です。</span><span class="sxs-lookup"><span data-stu-id="e7782-205">It is up to the IKeyEscrowSink implementation to handle the provided element in a secure manner consistent with business policy.</span></span> <span data-ttu-id="e7782-206">ここでは、証明書の秘密キーはエスクローされました。 既知の企業 X.509 証明書を使用して XML 要素を暗号化するエスクロー シンクの 1 つの可能な実装がある可能性があります。この CertificateXmlEncryptor 型に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="e7782-206">One possible implementation could be for the escrow sink to encrypt the XML element using a known corporate X.509 certificate where the certificate's private key has been escrowed; the CertificateXmlEncryptor type can assist with this.</span></span> <span data-ttu-id="e7782-207">IKeyEscrowSink 実装は、指定した要素を適切に保持する役割もできます。</span><span class="sxs-lookup"><span data-stu-id="e7782-207">The IKeyEscrowSink implementation is also responsible for persisting the provided element appropriately.</span></span>

<span data-ttu-id="e7782-208">既定ではエスクロー メカニズムが有効でない、サーバー管理者は、[これをグローバルに構成](../configuration/machine-wide-policy.md#data-protection-configuration-machinewidepolicy)です。</span><span class="sxs-lookup"><span data-stu-id="e7782-208">By default no escrow mechanism is enabled, though server administrators can [configure this globally](../configuration/machine-wide-policy.md#data-protection-configuration-machinewidepolicy).</span></span> <span data-ttu-id="e7782-209">構成することもプログラムを介して、 *IDataProtectionBuilder.AddKeyEscrowSink*メソッド以下のサンプルで示すようにします。</span><span class="sxs-lookup"><span data-stu-id="e7782-209">It can also be configured programmatically via the *IDataProtectionBuilder.AddKeyEscrowSink* method as shown in the sample below.</span></span> <span data-ttu-id="e7782-210">*AddKeyEscrowSink*メソッドのオーバー ロードのミラー、 *IServiceCollection.AddSingleton*と*IServiceCollection.AddInstance* IKeyEscrowSink とオーバー ロードシングルトン インスタンスを想定します。</span><span class="sxs-lookup"><span data-stu-id="e7782-210">The *AddKeyEscrowSink* method overloads mirror the *IServiceCollection.AddSingleton* and *IServiceCollection.AddInstance* overloads, as IKeyEscrowSink instances are intended to be singletons.</span></span> <span data-ttu-id="e7782-211">IKeyEscrowSink の複数のインスタンスが登録されている場合、キーを同時に複数のメカニズムをエスクローできるようにに、1 つずつはキーの生成中に呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="e7782-211">If multiple IKeyEscrowSink instances are registered, each one will be called during key generation, so keys can be escrowed to multiple mechanisms simultaneously.</span></span>

<span data-ttu-id="e7782-212">API IKeyEscrowSink インスタンスから情報を読み取るにはありません。</span><span class="sxs-lookup"><span data-stu-id="e7782-212">There is no API to read material from an IKeyEscrowSink instance.</span></span> <span data-ttu-id="e7782-213">これはエスクロー機構の設計理論と一致: キー マテリアルを信頼された機関にアクセスできるようにするためのものが、および独自 escrowed マテリアルへのアクセスを含めることはできませんので、アプリケーション自体がない信頼された証明機関でします。</span><span class="sxs-lookup"><span data-stu-id="e7782-213">This is consistent with the design theory of the escrow mechanism: it's intended to make the key material accessible to a trusted authority, and since the application is itself not a trusted authority, it shouldn't have access to its own escrowed material.</span></span>

<span data-ttu-id="e7782-214">次のサンプル コードは、作成、"CONTOSODomain Admins"のメンバーだけが回復できるようにキーはエスクロー場所、IKeyEscrowSink を登録する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="e7782-214">The following sample code demonstrates creating and registering an IKeyEscrowSink where keys are escrowed such that only members of "CONTOSODomain Admins" can recover them.</span></span>

> [!NOTE]
> <span data-ttu-id="e7782-215">このサンプルを実行するドメインに参加している Windows 8 で Windows Server 2012 マシン、およびドメイン コント ローラーが Windows Server 2012 以降にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="e7782-215">To run this sample, you must be on a domain-joined Windows 8 / Windows Server 2012 machine, and the domain controller must be Windows Server 2012 or later.</span></span>

<span data-ttu-id="e7782-216">[!code-none[Main](key-management/samples/key-management-extensibility.cs)]</span><span class="sxs-lookup"><span data-stu-id="e7782-216">[!code-none[Main](key-management/samples/key-management-extensibility.cs)]</span></span>
