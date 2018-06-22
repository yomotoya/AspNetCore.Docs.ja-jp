---
title: ASP.NET Core のキー管理の拡張機能
author: rick-anderson
description: ASP.NET Core データ保護キーの管理の拡張性について説明します。
ms.author: riande
ms.date: 11/22/2017
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: 3ebde889d207e02aff8c042b1d80884210a68ff4
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274753"
---
# <a name="key-management-extensibility-in-aspnet-core"></a><span data-ttu-id="bb890-103">ASP.NET Core のキー管理の拡張機能</span><span class="sxs-lookup"><span data-stu-id="bb890-103">Key management extensibility in ASP.NET Core</span></span>

<a name="data-protection-extensibility-key-management"></a>

>[!TIP]
> <span data-ttu-id="bb890-104">読み取り、[キー管理](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)ようにこれらの Api の背後にある基本的な概念について説明します、このセクションを読む前にセクションです。</span><span class="sxs-lookup"><span data-stu-id="bb890-104">Read the [key management](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) section before reading this section, as it explains some of the fundamental concepts behind these APIs.</span></span>

>[!WARNING]
> <span data-ttu-id="bb890-105">次のインターフェイスのいずれかを実装する型はスレッド セーフである必要があります複数の呼び出し元のです。</span><span class="sxs-lookup"><span data-stu-id="bb890-105">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="key"></a><span data-ttu-id="bb890-106">キー</span><span class="sxs-lookup"><span data-stu-id="bb890-106">Key</span></span>

<span data-ttu-id="bb890-107">`IKey`インターフェイスは、キー暗号方式での基本的な形式です。</span><span class="sxs-lookup"><span data-stu-id="bb890-107">The `IKey` interface is the basic representation of a key in cryptosystem.</span></span> <span data-ttu-id="bb890-108">用語キーは、抽象の意味での「暗号キー マテリアル」文字どおりの意味ではなくここで使用されます。</span><span class="sxs-lookup"><span data-stu-id="bb890-108">The term key is used here in the abstract sense, not in the literal sense of "cryptographic key material".</span></span> <span data-ttu-id="bb890-109">キーには、次のプロパティがあります。</span><span class="sxs-lookup"><span data-stu-id="bb890-109">A key has the following properties:</span></span>

* <span data-ttu-id="bb890-110">アクティブ化、作成、および有効期限の日付</span><span class="sxs-lookup"><span data-stu-id="bb890-110">Activation, creation, and expiration dates</span></span>

* <span data-ttu-id="bb890-111">失効状態</span><span class="sxs-lookup"><span data-stu-id="bb890-111">Revocation status</span></span>

* <span data-ttu-id="bb890-112">キー識別子 (GUID)</span><span class="sxs-lookup"><span data-stu-id="bb890-112">Key identifier (a GUID)</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="bb890-113">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="bb890-113">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="bb890-114">さらに、`IKey`公開、`CreateEncryptor`を作成するために使用するメソッド、 [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor)インスタンスは、このキーに関連付けられています。</span><span class="sxs-lookup"><span data-stu-id="bb890-114">Additionally, `IKey` exposes a `CreateEncryptor` method which can be used to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="bb890-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="bb890-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="bb890-116">さらに、`IKey`公開、`CreateEncryptorInstance`を作成するために使用するメソッド、 [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor)インスタンスは、このキーに関連付けられています。</span><span class="sxs-lookup"><span data-stu-id="bb890-116">Additionally, `IKey` exposes a `CreateEncryptorInstance` method which can be used to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

---

> [!NOTE]
> <span data-ttu-id="bb890-117">未加工の暗号化マテリアルを取得する API はありません、`IKey`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="bb890-117">There's no API to retrieve the raw cryptographic material from an `IKey` instance.</span></span>

## <a name="ikeymanager"></a><span data-ttu-id="bb890-118">IKeyManager</span><span class="sxs-lookup"><span data-stu-id="bb890-118">IKeyManager</span></span>

<span data-ttu-id="bb890-119">`IKeyManager`インターフェイスは、一般的なキーの格納、取得、および操作を担当するオブジェクトを表します。</span><span class="sxs-lookup"><span data-stu-id="bb890-119">The `IKeyManager` interface represents an object responsible for general key storage, retrieval, and manipulation.</span></span> <span data-ttu-id="bb890-120">3 つの高度な操作を公開します。</span><span class="sxs-lookup"><span data-stu-id="bb890-120">It exposes three high-level operations:</span></span>

* <span data-ttu-id="bb890-121">新しいキーを作成し、記憶域に保存します。</span><span class="sxs-lookup"><span data-stu-id="bb890-121">Create a new key and persist it to storage.</span></span>

* <span data-ttu-id="bb890-122">記憶域からすべてのキーを取得します。</span><span class="sxs-lookup"><span data-stu-id="bb890-122">Get all keys from storage.</span></span>

* <span data-ttu-id="bb890-123">1 つまたは複数のキーを取り消すし、記憶域に失効情報の保持されます。</span><span class="sxs-lookup"><span data-stu-id="bb890-123">Revoke one or more keys and persist the revocation information to storage.</span></span>

>[!WARNING]
> <span data-ttu-id="bb890-124">書き込み、`IKeyManager`は非常に高度なタスクの場合であり、開発者の大部分が試行しないでください。</span><span class="sxs-lookup"><span data-stu-id="bb890-124">Writing an `IKeyManager` is a very advanced task, and the majority of developers shouldn't attempt it.</span></span> <span data-ttu-id="bb890-125">代わりに、ほとんどの開発者を活用してによって提供される機能、 [XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager)クラスです。</span><span class="sxs-lookup"><span data-stu-id="bb890-125">Instead, most developers should take advantage of the facilities offered by the [XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager) class.</span></span>

<a name="data-protection-extensibility-key-management-xmlkeymanager"></a>

## <a name="xmlkeymanager"></a><span data-ttu-id="bb890-126">XmlKeyManager</span><span class="sxs-lookup"><span data-stu-id="bb890-126">XmlKeyManager</span></span>

<span data-ttu-id="bb890-127">`XmlKeyManager`型は、インボックスの具象`IKeyManager`です。</span><span class="sxs-lookup"><span data-stu-id="bb890-127">The `XmlKeyManager` type is the in-box concrete implementation of `IKeyManager`.</span></span> <span data-ttu-id="bb890-128">キー エスクロー、残りの部分でのキーの暗号化など、いくつかに役立つ機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="bb890-128">It provides several useful facilities, including key escrow and encryption of keys at rest.</span></span> <span data-ttu-id="bb890-129">このシステム内のキーは、XML 要素として表現されます (具体的には、 [XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview))。</span><span class="sxs-lookup"><span data-stu-id="bb890-129">Keys in this system are represented as XML elements (specifically, [XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).</span></span>

<span data-ttu-id="bb890-130">`XmlKeyManager` そのタスクを満たす操作の過程でその他のいくつかのコンポーネントに依存します。</span><span class="sxs-lookup"><span data-stu-id="bb890-130">`XmlKeyManager` depends on several other components in the course of fulfilling its tasks:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="bb890-131">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="bb890-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* <span data-ttu-id="bb890-132">`AlgorithmConfiguration`、新しいキーで使用されるアルゴリズムを決定します。</span><span class="sxs-lookup"><span data-stu-id="bb890-132">`AlgorithmConfiguration`, which dictates the algorithms used by new keys.</span></span>

* <span data-ttu-id="bb890-133">`IXmlRepository`、どのコントロールのキーがストレージに保存されます。</span><span class="sxs-lookup"><span data-stu-id="bb890-133">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="bb890-134">`IXmlEncryptor` [オプション]、静止したキーを暗号化できるようにします。</span><span class="sxs-lookup"><span data-stu-id="bb890-134">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="bb890-135">`IKeyEscrowSink` [オプション] をキー エスクロー サービスを提供します。</span><span class="sxs-lookup"><span data-stu-id="bb890-135">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="bb890-136">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="bb890-136">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="bb890-137">`IXmlRepository`、どのコントロールのキーがストレージに保存されます。</span><span class="sxs-lookup"><span data-stu-id="bb890-137">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="bb890-138">`IXmlEncryptor` [オプション]、静止したキーを暗号化できるようにします。</span><span class="sxs-lookup"><span data-stu-id="bb890-138">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="bb890-139">`IKeyEscrowSink` [オプション] をキー エスクロー サービスを提供します。</span><span class="sxs-lookup"><span data-stu-id="bb890-139">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

---

<span data-ttu-id="bb890-140">内でこれらのコンポーネントを一緒にワイヤードは方法を示す高度な図を次に示します`XmlKeyManager`です。</span><span class="sxs-lookup"><span data-stu-id="bb890-140">Below are high-level diagrams which indicate how these components are wired together within `XmlKeyManager`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="bb890-141">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="bb890-141">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ![キーの作成](key-management/_static/keycreation2.png)

   <span data-ttu-id="bb890-143">*キーの作成/CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="bb890-143">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="bb890-144">実装で`CreateNewKey`、`AlgorithmConfiguration`コンポーネントの作成に使用する一意`IAuthenticatedEncryptorDescriptor`、し、XML としてシリアル化されます。</span><span class="sxs-lookup"><span data-stu-id="bb890-144">In the implementation of `CreateNewKey`, the `AlgorithmConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="bb890-145">キー エスクロー シンクが存在する場合は、長期的な記憶域の生の (暗号化されていない) XML は、シンクに提供されます。</span><span class="sxs-lookup"><span data-stu-id="bb890-145">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="bb890-146">暗号化されていない XML を実行し、 `IXmlEncryptor` (必要な場合) を暗号化された XML ドキュメントを生成します。</span><span class="sxs-lookup"><span data-stu-id="bb890-146">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="bb890-147">この暗号化されたドキュメントが経由での長期的なストレージに保存され、`IXmlRepository`です。</span><span class="sxs-lookup"><span data-stu-id="bb890-147">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="bb890-148">(ない場合は`IXmlEncryptor`が構成されている場合、暗号化されていないドキュメントで永続化されて、 `IXmlRepository`)。</span><span class="sxs-lookup"><span data-stu-id="bb890-148">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="bb890-149">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="bb890-149">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ![キーの作成](key-management/_static/keycreation1.png)

   <span data-ttu-id="bb890-151">*キーの作成/CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="bb890-151">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="bb890-152">実装で`CreateNewKey`、`IAuthenticatedEncryptorConfiguration`コンポーネントの作成に使用する一意`IAuthenticatedEncryptorDescriptor`、し、XML としてシリアル化されます。</span><span class="sxs-lookup"><span data-stu-id="bb890-152">In the implementation of `CreateNewKey`, the `IAuthenticatedEncryptorConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="bb890-153">キー エスクロー シンクが存在する場合は、長期的な記憶域の生の (暗号化されていない) XML は、シンクに提供されます。</span><span class="sxs-lookup"><span data-stu-id="bb890-153">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="bb890-154">暗号化されていない XML を実行し、 `IXmlEncryptor` (必要な場合) を暗号化された XML ドキュメントを生成します。</span><span class="sxs-lookup"><span data-stu-id="bb890-154">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="bb890-155">この暗号化されたドキュメントが経由での長期的なストレージに保存され、`IXmlRepository`です。</span><span class="sxs-lookup"><span data-stu-id="bb890-155">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="bb890-156">(ない場合は`IXmlEncryptor`が構成されている場合、暗号化されていないドキュメントで永続化されて、 `IXmlRepository`)。</span><span class="sxs-lookup"><span data-stu-id="bb890-156">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

---

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="bb890-157">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="bb890-157">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ![キーの取得](key-management/_static/keyretrieval2.png)
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="bb890-159">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="bb890-159">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ![キーの取得](key-management/_static/keyretrieval1.png)

---

   <span data-ttu-id="bb890-161">*キーの取得/GetAllKeys*</span><span class="sxs-lookup"><span data-stu-id="bb890-161">*Key Retrieval / GetAllKeys*</span></span>

<span data-ttu-id="bb890-162">実装で`GetAllKeys`、XML ドキュメントを表すキーおよび取り消した場合は、基になるから読み取ら`IXmlRepository`です。</span><span class="sxs-lookup"><span data-stu-id="bb890-162">In the implementation of `GetAllKeys`, the XML documents representing keys and revocations are read from the underlying `IXmlRepository`.</span></span> <span data-ttu-id="bb890-163">これらのドキュメントが暗号化されている場合、システムは自動的に復号化にします。</span><span class="sxs-lookup"><span data-stu-id="bb890-163">If these documents are encrypted, the system will automatically decrypt them.</span></span> <span data-ttu-id="bb890-164">`XmlKeyManager` 適切な作成`IAuthenticatedEncryptorDescriptorDeserializer`にドキュメントを逆シリアル化するインスタンスを戻します`IAuthenticatedEncryptorDescriptor`インスタンスで、個々 にラップし、`IKey`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="bb890-164">`XmlKeyManager` creates the appropriate `IAuthenticatedEncryptorDescriptorDeserializer` instances to deserialize the documents back into `IAuthenticatedEncryptorDescriptor` instances, which are then wrapped in individual `IKey` instances.</span></span> <span data-ttu-id="bb890-165">このコレクションの`IKey`インスタンスが、呼び出し元に返されます。</span><span class="sxs-lookup"><span data-stu-id="bb890-165">This collection of `IKey` instances is returned to the caller.</span></span>

<span data-ttu-id="bb890-166">特定の XML 要素の詳細についてで参照できる、[キーの格納形式のドキュメント](xref:security/data-protection/implementation/key-storage-format#data-protection-implementation-key-storage-format)です。</span><span class="sxs-lookup"><span data-stu-id="bb890-166">Further information on the particular XML elements can be found in the [key storage format document](xref:security/data-protection/implementation/key-storage-format#data-protection-implementation-key-storage-format).</span></span>

## <a name="ixmlrepository"></a><span data-ttu-id="bb890-167">IXmlRepository</span><span class="sxs-lookup"><span data-stu-id="bb890-167">IXmlRepository</span></span>

<span data-ttu-id="bb890-168">`IXmlRepository`インターフェイスは、XML を永続化したり、バッキング ストアから XML を取得できる型を表します。</span><span class="sxs-lookup"><span data-stu-id="bb890-168">The `IXmlRepository` interface represents a type that can persist XML to and retrieve XML from a backing store.</span></span> <span data-ttu-id="bb890-169">2 つの Api を公開します。</span><span class="sxs-lookup"><span data-stu-id="bb890-169">It exposes two APIs:</span></span>

* <span data-ttu-id="bb890-170">GetAllElements(): IReadOnlyCollection<XElement></span><span class="sxs-lookup"><span data-stu-id="bb890-170">GetAllElements() : IReadOnlyCollection<XElement></span></span>

* <span data-ttu-id="bb890-171">StoreElement (XElement 要素、文字列の friendlyName)</span><span class="sxs-lookup"><span data-stu-id="bb890-171">StoreElement(XElement element, string friendlyName)</span></span>

<span data-ttu-id="bb890-172">実装`IXmlRepository`それらに渡される XML を解析する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="bb890-172">Implementations of `IXmlRepository` don't need to parse the XML passing through them.</span></span> <span data-ttu-id="bb890-173">上位のレイヤーを生成して、ドキュメントの解析について心配を非透過的に XML ドキュメントを扱うさせている必要があります。</span><span class="sxs-lookup"><span data-stu-id="bb890-173">They should treat the XML documents as opaque and let higher layers worry about generating and parsing the documents.</span></span>

<span data-ttu-id="bb890-174">実装する 2 つの組み込み具象型がある`IXmlRepository`:`FileSystemXmlRepository`と`RegistryXmlRepository`です。</span><span class="sxs-lookup"><span data-stu-id="bb890-174">There are two built-in concrete types which implement `IXmlRepository`: `FileSystemXmlRepository` and `RegistryXmlRepository`.</span></span> <span data-ttu-id="bb890-175">参照してください、[キー記憶域プロバイダーのドキュメント](xref:security/data-protection/implementation/key-storage-providers#data-protection-implementation-key-storage-providers)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="bb890-175">See the [key storage providers document](xref:security/data-protection/implementation/key-storage-providers#data-protection-implementation-key-storage-providers) for more information.</span></span> <span data-ttu-id="bb890-176">カスタムの登録`IXmlRepository`Azure Blob ストレージなど、各種のバッキング ストアを使用する適切な方法になります。</span><span class="sxs-lookup"><span data-stu-id="bb890-176">Registering a custom `IXmlRepository` would be the appropriate manner to use a different backing store, e.g., Azure Blob Storage.</span></span>

<span data-ttu-id="bb890-177">アプリケーション全体の既定のリポジトリを変更するにはカスタム コントロールの登録`IXmlRepository`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="bb890-177">To change the default repository application-wide, register a custom `IXmlRepository` instance:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="bb890-178">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="bb890-178">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ```csharp
   services.Configure<KeyManagementOptions>(options => options.XmlRepository = new MyCustomXmlRepository());
   ```
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="bb890-179">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="bb890-179">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ```csharp
   services.AddSingleton<IXmlRepository>(new MyCustomXmlRepository());
   ```

---

<a name="data-protection-extensibility-key-management-ixmlencryptor"></a>

## <a name="ixmlencryptor"></a><span data-ttu-id="bb890-180">IXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="bb890-180">IXmlEncryptor</span></span>

<span data-ttu-id="bb890-181">`IXmlEncryptor`インターフェイスは、プレーン テキストの XML 要素を暗号化する型を表します。</span><span class="sxs-lookup"><span data-stu-id="bb890-181">The `IXmlEncryptor` interface represents a type that can encrypt a plaintext XML element.</span></span> <span data-ttu-id="bb890-182">単一の API を公開します。</span><span class="sxs-lookup"><span data-stu-id="bb890-182">It exposes a single API:</span></span>

* <span data-ttu-id="bb890-183">(XElement plaintextElement) の暗号化: EncryptedXmlInfo</span><span class="sxs-lookup"><span data-stu-id="bb890-183">Encrypt(XElement plaintextElement) : EncryptedXmlInfo</span></span>

<span data-ttu-id="bb890-184">場合は、シリアル化された`IAuthenticatedEncryptorDescriptor`し、「暗号化を必要とする」としてマークされているすべての要素を含む`XmlKeyManager`を通じて、構成されたこれらの要素を実行`IXmlEncryptor`の`Encrypt`メソッド、およびそれに enciphered 要素を保持ではなく、プレーン テキスト要素を`IXmlRepository`です。</span><span class="sxs-lookup"><span data-stu-id="bb890-184">If a serialized `IAuthenticatedEncryptorDescriptor` contains any elements marked as "requires encryption", then `XmlKeyManager` will run those elements through the configured `IXmlEncryptor`'s `Encrypt` method, and it will persist the enciphered element rather than the plaintext element to the `IXmlRepository`.</span></span> <span data-ttu-id="bb890-185">出力、`Encrypt`メソッドは、`EncryptedXmlInfo`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="bb890-185">The output of the `Encrypt` method is an `EncryptedXmlInfo` object.</span></span> <span data-ttu-id="bb890-186">このオブジェクトは enciphered、両方の結果を含むラッパー`XElement`と型を表す、`IXmlDecryptor`の対応する要素を復号化に使用できます。</span><span class="sxs-lookup"><span data-stu-id="bb890-186">This object is a wrapper which contains both the resultant enciphered `XElement` and the Type which represents an `IXmlDecryptor` which can be used to decipher the corresponding element.</span></span>

<span data-ttu-id="bb890-187">実装している 4 つの組み込み具象型がある`IXmlEncryptor`:</span><span class="sxs-lookup"><span data-stu-id="bb890-187">There are four built-in concrete types which implement `IXmlEncryptor`:</span></span>
* `CertificateXmlEncryptor`
* `DpapiNGXmlEncryptor`
* `DpapiXmlEncryptor`
* `NullXmlEncryptor`

<span data-ttu-id="bb890-188">参照してください、 [rest ドキュメント キーの暗号化](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="bb890-188">See the [key encryption at rest document](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest) for more information.</span></span>

<span data-ttu-id="bb890-189">既定のキーの暗号化に rest メカニズム アプリケーション全体を変更するにはカスタム コントロールの登録`IXmlEncryptor`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="bb890-189">To change the default key-encryption-at-rest mechanism application-wide, register a custom `IXmlEncryptor` instance:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="bb890-190">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="bb890-190">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ```csharp
   services.Configure<KeyManagementOptions>(options => options.XmlEncryptor = new MyCustomXmlEncryptor());
   ```
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="bb890-191">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="bb890-191">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ```csharp
   services.AddSingleton<IXmlEncryptor>(new MyCustomXmlEncryptor());
   ```

---

## <a name="ixmldecryptor"></a><span data-ttu-id="bb890-192">IXmlDecryptor</span><span class="sxs-lookup"><span data-stu-id="bb890-192">IXmlDecryptor</span></span>

<span data-ttu-id="bb890-193">`IXmlDecryptor`インターフェイスが暗号化を解除する方法を認識する型を表す、`XElement`を介して enciphered ですが、`IXmlEncryptor`です。</span><span class="sxs-lookup"><span data-stu-id="bb890-193">The `IXmlDecryptor` interface represents a type that knows how to decrypt an `XElement` that was enciphered via an `IXmlEncryptor`.</span></span> <span data-ttu-id="bb890-194">単一の API を公開します。</span><span class="sxs-lookup"><span data-stu-id="bb890-194">It exposes a single API:</span></span>

* <span data-ttu-id="bb890-195">(XElement encryptedElement) を暗号化解除: XElement</span><span class="sxs-lookup"><span data-stu-id="bb890-195">Decrypt(XElement encryptedElement) : XElement</span></span>

<span data-ttu-id="bb890-196">`Decrypt`メソッドによって実行された暗号化を元に戻します`IXmlEncryptor.Encrypt`です。</span><span class="sxs-lookup"><span data-stu-id="bb890-196">The `Decrypt` method undoes the encryption performed by `IXmlEncryptor.Encrypt`.</span></span> <span data-ttu-id="bb890-197">各具象型では一般に、`IXmlEncryptor`実装は、対応する具象型を持つは`IXmlDecryptor`実装します。</span><span class="sxs-lookup"><span data-stu-id="bb890-197">Generally, each concrete `IXmlEncryptor` implementation will have a corresponding concrete `IXmlDecryptor` implementation.</span></span>

<span data-ttu-id="bb890-198">実装する型`IXmlDecryptor`次の 2 つのパブリック コンス トラクターのいずれかである必要があります。</span><span class="sxs-lookup"><span data-stu-id="bb890-198">Types which implement `IXmlDecryptor` should have one of the following two public constructors:</span></span>

* <span data-ttu-id="bb890-199">.ctor(IServiceProvider)</span><span class="sxs-lookup"><span data-stu-id="bb890-199">.ctor(IServiceProvider)</span></span>
* <span data-ttu-id="bb890-200">.ctor()</span><span class="sxs-lookup"><span data-stu-id="bb890-200">.ctor()</span></span>

> [!NOTE]
> <span data-ttu-id="bb890-201">`IServiceProvider`に渡されるコンス トラクターを null にすることがあります。</span><span class="sxs-lookup"><span data-stu-id="bb890-201">The `IServiceProvider` passed to the constructor may be null.</span></span>

## <a name="ikeyescrowsink"></a><span data-ttu-id="bb890-202">IKeyEscrowSink</span><span class="sxs-lookup"><span data-stu-id="bb890-202">IKeyEscrowSink</span></span>

<span data-ttu-id="bb890-203">`IKeyEscrowSink`インターフェイスは、機密情報のエスクローを実行できる型を表します。</span><span class="sxs-lookup"><span data-stu-id="bb890-203">The `IKeyEscrowSink` interface represents a type that can perform escrow of sensitive information.</span></span> <span data-ttu-id="bb890-204">導入にこれは、シリアル化された記述子は、(暗号化マテリアル) などの機密情報を含む可能性がありますに注意してください、 [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor)最初の場所を入力します。</span><span class="sxs-lookup"><span data-stu-id="bb890-204">Recall that serialized descriptors might contain sensitive information (such as cryptographic material), and this is what led to the introduction of the [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor) type in the first place.</span></span> <span data-ttu-id="bb890-205">ただし、事故が発生する、キーのリングが削除できるか、または壊れています。</span><span class="sxs-lookup"><span data-stu-id="bb890-205">However, accidents happen, and key rings can be deleted or become corrupted.</span></span>

<span data-ttu-id="bb890-206">エスクロー インターフェイスには、構成されているいずれかが変換される前に、生のシリアル化された XML へのアクセスを許可、緊急脱出口が用意されています[IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor)です。</span><span class="sxs-lookup"><span data-stu-id="bb890-206">The escrow interface provides an emergency escape hatch, allowing access to the raw serialized XML before it's transformed by any configured [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor).</span></span> <span data-ttu-id="bb890-207">インターフェイスは、単一の API を公開します。</span><span class="sxs-lookup"><span data-stu-id="bb890-207">The interface exposes a single API:</span></span>

* <span data-ttu-id="bb890-208">ストア (Guid keyId、XElement 要素)</span><span class="sxs-lookup"><span data-stu-id="bb890-208">Store(Guid keyId, XElement element)</span></span>

<span data-ttu-id="bb890-209">`IKeyEscrowSink`ビジネス ポリシーと一致するセキュリティで保護された方法では、指定した要素の処理を実装します。</span><span class="sxs-lookup"><span data-stu-id="bb890-209">It's up to the `IKeyEscrowSink` implementation to handle the provided element in a secure manner consistent with business policy.</span></span> <span data-ttu-id="bb890-210">ここでは、証明書の秘密キーはエスクローされました。 既知の企業 X.509 証明書を使用して XML 要素を暗号化するエスクロー シンクの 1 つの可能な実装がある可能性があります。`CertificateXmlEncryptor`型がこれを支援できます。</span><span class="sxs-lookup"><span data-stu-id="bb890-210">One possible implementation could be for the escrow sink to encrypt the XML element using a known corporate X.509 certificate where the certificate's private key has been escrowed; the `CertificateXmlEncryptor` type can assist with this.</span></span> <span data-ttu-id="bb890-211">`IKeyEscrowSink`実装は、指定した要素を適切に保持する役割もします。</span><span class="sxs-lookup"><span data-stu-id="bb890-211">The `IKeyEscrowSink` implementation is also responsible for persisting the provided element appropriately.</span></span>

<span data-ttu-id="bb890-212">既定ではエスクロー メカニズムが有効でない、サーバー管理者は、[これをグローバルに構成](xref:security/data-protection/configuration/machine-wide-policy)です。</span><span class="sxs-lookup"><span data-stu-id="bb890-212">By default no escrow mechanism is enabled, though server administrators can [configure this globally](xref:security/data-protection/configuration/machine-wide-policy).</span></span> <span data-ttu-id="bb890-213">構成することもプログラムを介して、`IDataProtectionBuilder.AddKeyEscrowSink`メソッド以下のサンプルで示すようにします。</span><span class="sxs-lookup"><span data-stu-id="bb890-213">It can also be configured programmatically via the `IDataProtectionBuilder.AddKeyEscrowSink` method as shown in the sample below.</span></span> <span data-ttu-id="bb890-214">`AddKeyEscrowSink`メソッドのオーバー ロードのミラー、`IServiceCollection.AddSingleton`と`IServiceCollection.AddInstance`、オーバー ロードとして`IKeyEscrowSink`インスタンスはシングルトンとして意図されています。</span><span class="sxs-lookup"><span data-stu-id="bb890-214">The `AddKeyEscrowSink` method overloads mirror the `IServiceCollection.AddSingleton` and `IServiceCollection.AddInstance` overloads, as `IKeyEscrowSink` instances are intended to be singletons.</span></span> <span data-ttu-id="bb890-215">複数`IKeyEscrowSink`インスタンスを登録すると、キーを同時に複数のメカニズムをエスクローできるようにそのそれぞれがキーの生成中に呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="bb890-215">If multiple `IKeyEscrowSink` instances are registered, each one will be called during key generation, so keys can be escrowed to multiple mechanisms simultaneously.</span></span>

<span data-ttu-id="bb890-216">API から情報を読み取るにはありません、`IKeyEscrowSink`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="bb890-216">There's no API to read material from an `IKeyEscrowSink` instance.</span></span> <span data-ttu-id="bb890-217">これはエスクロー機構の設計理論と一致: キー マテリアルを信頼された機関にアクセスできるようにするためのものが、および独自 escrowed マテリアルへのアクセスを含めることはできませんので、アプリケーション自体がない信頼された証明機関でします。</span><span class="sxs-lookup"><span data-stu-id="bb890-217">This is consistent with the design theory of the escrow mechanism: it's intended to make the key material accessible to a trusted authority, and since the application is itself not a trusted authority, it shouldn't have access to its own escrowed material.</span></span>

<span data-ttu-id="bb890-218">次のサンプル コードは、作成、および登録を示しています、 `IKeyEscrowSink` "CONTOSODomain Admins"のメンバーだけが回復できるようにキーはエスクロー場所。</span><span class="sxs-lookup"><span data-stu-id="bb890-218">The following sample code demonstrates creating and registering an `IKeyEscrowSink` where keys are escrowed such that only members of "CONTOSODomain Admins" can recover them.</span></span>

> [!NOTE]
> <span data-ttu-id="bb890-219">このサンプルを実行するドメインに参加している Windows 8 で Windows Server 2012 マシン、およびドメイン コント ローラーが Windows Server 2012 以降にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="bb890-219">To run this sample, you must be on a domain-joined Windows 8 / Windows Server 2012 machine, and the domain controller must be Windows Server 2012 or later.</span></span>

[!code-csharp[](key-management/samples/key-management-extensibility.cs)]
