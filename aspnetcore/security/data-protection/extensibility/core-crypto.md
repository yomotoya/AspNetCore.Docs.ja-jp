---
title: ASP.NET Core の中核となる暗号化の拡張性
author: rick-anderson
description: IAuthenticatedEncryptor、IAuthenticatedEncryptorDescriptor、IAuthenticatedEncryptorDescriptorDeserializer、および最上位のファクトリについて説明します。
manager: wpickett
ms.author: riande
ms.date: 8/11/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/extensibility/core-crypto
ms.openlocfilehash: b5a0dbc9120a8032dbb8d8eee74684495a982ac1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30896827"
---
# <a name="core-cryptography-extensibility-in-aspnet-core"></a><span data-ttu-id="86a5b-103">ASP.NET Core の中核となる暗号化の拡張性</span><span class="sxs-lookup"><span data-stu-id="86a5b-103">Core cryptography extensibility in ASP.NET Core</span></span>

<a name="data-protection-extensibility-core-crypto"></a>

>[!WARNING]
> <span data-ttu-id="86a5b-104">次のインターフェイスのいずれかを実装する型はスレッド セーフである必要があります複数の呼び出し元のです。</span><span class="sxs-lookup"><span data-stu-id="86a5b-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptor"></a>

## <a name="iauthenticatedencryptor"></a><span data-ttu-id="86a5b-105">IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="86a5b-105">IAuthenticatedEncryptor</span></span>

<span data-ttu-id="86a5b-106">**IAuthenticatedEncryptor**インターフェイスは、暗号化サブシステムの基本的なビルド ブロックです。</span><span class="sxs-lookup"><span data-stu-id="86a5b-106">The **IAuthenticatedEncryptor** interface is the basic building block of the cryptographic subsystem.</span></span> <span data-ttu-id="86a5b-107">一般には、キー、ごとの 1 つの IAuthenticatedEncryptor と IAuthenticatedEncryptor インスタンスは、すべての暗号化キー マテリアルと暗号化操作の実行に必要なアルゴリズムの情報をラップします。</span><span class="sxs-lookup"><span data-stu-id="86a5b-107">There's generally one IAuthenticatedEncryptor per key, and the IAuthenticatedEncryptor instance wraps all cryptographic key material and algorithmic information necessary to perform cryptographic operations.</span></span>

<span data-ttu-id="86a5b-108">その名前からわかるように、型、認証済みの暗号化と復号化サービスを提供します。</span><span class="sxs-lookup"><span data-stu-id="86a5b-108">As its name suggests, the type is responsible for providing authenticated encryption and decryption services.</span></span> <span data-ttu-id="86a5b-109">これは、次の 2 つの Api を公開します。</span><span class="sxs-lookup"><span data-stu-id="86a5b-109">It exposes the following two APIs.</span></span>

* <span data-ttu-id="86a5b-110">復号化 (ArraySegment<byte>暗号文、ArraySegment<byte> additionalAuthenticatedData): byte[]</span><span class="sxs-lookup"><span data-stu-id="86a5b-110">Decrypt(ArraySegment<byte> ciphertext, ArraySegment<byte> additionalAuthenticatedData) : byte[]</span></span>

* <span data-ttu-id="86a5b-111">暗号化 (ArraySegment<byte>プレーン テキスト、ArraySegment<byte> additionalAuthenticatedData): byte[]</span><span class="sxs-lookup"><span data-stu-id="86a5b-111">Encrypt(ArraySegment<byte> plaintext, ArraySegment<byte> additionalAuthenticatedData) : byte[]</span></span>

<span data-ttu-id="86a5b-112">暗号化メソッドは、enciphered プレーン テキストとな認証タグを含む blob を返します。</span><span class="sxs-lookup"><span data-stu-id="86a5b-112">The Encrypt method returns a blob that includes the enciphered plaintext and an authentication tag.</span></span> <span data-ttu-id="86a5b-113">認証タグは、AAD 自体は、最終的なペイロードから回復可能な必要はありませんが、追加の認証済みデータ (AAD) を囲んでいる必要があります。</span><span class="sxs-lookup"><span data-stu-id="86a5b-113">The authentication tag must encompass the additional authenticated data (AAD), though the AAD itself need not be recoverable from the final payload.</span></span> <span data-ttu-id="86a5b-114">復号化メソッドは、認証タグを検証し、解読はペイロードを返します。</span><span class="sxs-lookup"><span data-stu-id="86a5b-114">The Decrypt method validates the authentication tag and returns the deciphered payload.</span></span> <span data-ttu-id="86a5b-115">すべてのエラー (ArgumentNullException を除くと似ています) は、CryptographicException に homogenized 必要があります。</span><span class="sxs-lookup"><span data-stu-id="86a5b-115">All failures (except ArgumentNullException and similar) should be homogenized to CryptographicException.</span></span>

> [!NOTE]
> <span data-ttu-id="86a5b-116">IAuthenticatedEncryptor インスタンス自体実際には、キー マテリアルを格納する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="86a5b-116">The IAuthenticatedEncryptor instance itself doesn't actually need to contain the key material.</span></span> <span data-ttu-id="86a5b-117">たとえば、実装では、すべての操作の HSM に委任します。</span><span class="sxs-lookup"><span data-stu-id="86a5b-117">For example, the implementation could delegate to an HSM for all operations.</span></span>

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptorfactory"></a>
<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor"></a>

## <a name="how-to-create-an-iauthenticatedencryptor"></a><span data-ttu-id="86a5b-118">IAuthenticatedEncryptor を作成する方法</span><span class="sxs-lookup"><span data-stu-id="86a5b-118">How to create an IAuthenticatedEncryptor</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="86a5b-119">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="86a5b-119">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="86a5b-120">**IAuthenticatedEncryptorFactory**インターフェイスを作成する方法を認識する型を表す、 [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor)インスタンス。</span><span class="sxs-lookup"><span data-stu-id="86a5b-120">The **IAuthenticatedEncryptorFactory** interface represents a type that knows how to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance.</span></span> <span data-ttu-id="86a5b-121">その API は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="86a5b-121">Its API is as follows.</span></span>

* <span data-ttu-id="86a5b-122">CreateEncryptorInstance (IKey キー): IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="86a5b-122">CreateEncryptorInstance(IKey key) : IAuthenticatedEncryptor</span></span>

<span data-ttu-id="86a5b-123">指定された IKey インスタンスに対してその CreateEncryptorInstance メソッドによって作成された任意の認証済み受け付け必要があります等価と見なされる、ように、次のコード例です。</span><span class="sxs-lookup"><span data-stu-id="86a5b-123">For any given IKey instance, any authenticated encryptors created by its CreateEncryptorInstance method should be considered equivalent, as in the below code sample.</span></span>

```csharp
// we have an IAuthenticatedEncryptorFactory instance and an IKey instance
IAuthenticatedEncryptorFactory factory = ...;
IKey key = ...;

// get an encryptor instance and perform an authenticated encryption operation
ArraySegment<byte> plaintext = new ArraySegment<byte>(Encoding.UTF8.GetBytes("plaintext"));
ArraySegment<byte> aad = new ArraySegment<byte>(Encoding.UTF8.GetBytes("AAD"));
var encryptor1 = factory.CreateEncryptorInstance(key);
byte[] ciphertext = encryptor1.Encrypt(plaintext, aad);

// get another encryptor instance and perform an authenticated decryption operation
var encryptor2 = factory.CreateEncryptorInstance(key);
byte[] roundTripped = encryptor2.Decrypt(new ArraySegment<byte>(ciphertext), aad);


// the 'roundTripped' and 'plaintext' buffers should be equivalent
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="86a5b-124">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="86a5b-124">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="86a5b-125">**IAuthenticatedEncryptorDescriptor**インターフェイスを作成する方法を認識する型を表す、 [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor)インスタンス。</span><span class="sxs-lookup"><span data-stu-id="86a5b-125">The **IAuthenticatedEncryptorDescriptor** interface represents a type that knows how to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance.</span></span> <span data-ttu-id="86a5b-126">その API は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="86a5b-126">Its API is as follows.</span></span>

* <span data-ttu-id="86a5b-127">CreateEncryptorInstance(): IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="86a5b-127">CreateEncryptorInstance() : IAuthenticatedEncryptor</span></span>

* <span data-ttu-id="86a5b-128">ExportToXml(): XmlSerializedDescriptorInfo</span><span class="sxs-lookup"><span data-stu-id="86a5b-128">ExportToXml() : XmlSerializedDescriptorInfo</span></span>

<span data-ttu-id="86a5b-129">IAuthenticatedEncryptor と同様に IAuthenticatedEncryptorDescriptor のインスタンスが 1 つの特定のキーをラップすると見なされます。</span><span class="sxs-lookup"><span data-stu-id="86a5b-129">Like IAuthenticatedEncryptor, an instance of IAuthenticatedEncryptorDescriptor is assumed to wrap one specific key.</span></span> <span data-ttu-id="86a5b-130">つまりが、特定の IAuthenticatedEncryptorDescriptor インスタンス、その CreateEncryptorInstance メソッドによって作成された任意の認証済み受け付け必要がありますと見なされるように、同等の次のコード例です。</span><span class="sxs-lookup"><span data-stu-id="86a5b-130">This means that for any given IAuthenticatedEncryptorDescriptor instance, any authenticated encryptors created by its CreateEncryptorInstance method should be considered equivalent, as in the below code sample.</span></span>

```csharp
// we have an IAuthenticatedEncryptorDescriptor instance
IAuthenticatedEncryptorDescriptor descriptor = ...;

// get an encryptor instance and perform an authenticated encryption operation
ArraySegment<byte> plaintext = new ArraySegment<byte>(Encoding.UTF8.GetBytes("plaintext"));
ArraySegment<byte> aad = new ArraySegment<byte>(Encoding.UTF8.GetBytes("AAD"));
var encryptor1 = descriptor.CreateEncryptorInstance();
byte[] ciphertext = encryptor1.Encrypt(plaintext, aad);

// get another encryptor instance and perform an authenticated decryption operation
var encryptor2 = descriptor.CreateEncryptorInstance();
byte[] roundTripped = encryptor2.Decrypt(new ArraySegment<byte>(ciphertext), aad);


// the 'roundTripped' and 'plaintext' buffers should be equivalent
```

---

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor"></a>

## <a name="iauthenticatedencryptordescriptor-aspnet-core-2x-only"></a><span data-ttu-id="86a5b-131">IAuthenticatedEncryptorDescriptor (ASP.NET のコア 2.x のみ)</span><span class="sxs-lookup"><span data-stu-id="86a5b-131">IAuthenticatedEncryptorDescriptor (ASP.NET Core 2.x only)</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="86a5b-132">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="86a5b-132">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="86a5b-133">**IAuthenticatedEncryptorDescriptor**インターフェイスは、型自体を XML にエクスポートする方法を把握していることを表します。</span><span class="sxs-lookup"><span data-stu-id="86a5b-133">The **IAuthenticatedEncryptorDescriptor** interface represents a type that knows how to export itself to XML.</span></span> <span data-ttu-id="86a5b-134">その API は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="86a5b-134">Its API is as follows.</span></span>

* <span data-ttu-id="86a5b-135">ExportToXml(): XmlSerializedDescriptorInfo</span><span class="sxs-lookup"><span data-stu-id="86a5b-135">ExportToXml() : XmlSerializedDescriptorInfo</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="86a5b-136">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="86a5b-136">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

---

## <a name="xml-serialization"></a><span data-ttu-id="86a5b-137">XML シリアル化</span><span class="sxs-lookup"><span data-stu-id="86a5b-137">XML Serialization</span></span>

<span data-ttu-id="86a5b-138">IAuthenticatedEncryptor と IAuthenticatedEncryptorDescriptor の主な違いは、記述子が、暗号化機能を作成し、有効な引数で指定する方法を知っていることです。</span><span class="sxs-lookup"><span data-stu-id="86a5b-138">The primary difference between IAuthenticatedEncryptor and IAuthenticatedEncryptorDescriptor is that the descriptor knows how to create the encryptor and supply it with valid arguments.</span></span> <span data-ttu-id="86a5b-139">これらの実装では、対称アルゴリズムおよび KeyedHashAlgorithm 依存、IAuthenticatedEncryptor を検討してください。</span><span class="sxs-lookup"><span data-stu-id="86a5b-139">Consider an IAuthenticatedEncryptor whose implementation relies on SymmetricAlgorithm and KeyedHashAlgorithm.</span></span> <span data-ttu-id="86a5b-140">暗号化機能のジョブが、これらの型を使用するが、必ずしもを認識していないこれらの型が送信元アプリケーションが再起動した場合、それ自体を再作成する方法の適切な説明を本当に書き込むことができないようにします。</span><span class="sxs-lookup"><span data-stu-id="86a5b-140">The encryptor's job is to consume these types, but it doesn't necessarily know where these types came from, so it can't really write out a proper description of how to recreate itself if the application restarts.</span></span> <span data-ttu-id="86a5b-141">記述子は、さらに高いレベルとして機能します。</span><span class="sxs-lookup"><span data-stu-id="86a5b-141">The descriptor acts as a higher level on top of this.</span></span> <span data-ttu-id="86a5b-142">記述子は、暗号化機能のインスタンスを作成する方法を認識しているため (たとえば、認識必要なアルゴリズムを作成する方法)、アプリケーションをリセットした後、暗号化機能のインスタンスが再作成できるようには XML 形式でそのナレッジがシリアル化します。</span><span class="sxs-lookup"><span data-stu-id="86a5b-142">Since the descriptor knows how to create the encryptor instance (e.g., it knows how to create the required algorithms), it can serialize that knowledge in XML form so that the encryptor instance can be recreated after an application reset.</span></span>

<a name="data-protection-extensibility-core-crypto-exporttoxml"></a>

<span data-ttu-id="86a5b-143">記述子は、その ExportToXml ルーチンを使用してシリアル化できます。</span><span class="sxs-lookup"><span data-stu-id="86a5b-143">The descriptor can be serialized via its ExportToXml routine.</span></span> <span data-ttu-id="86a5b-144">このルーチンは、次の 2 つのプロパティが含まれて XmlSerializedDescriptorInfo を返します: 記述子とを表す型の XElement 形式、 [IAuthenticatedEncryptorDescriptorDeserializer](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer)があります指定された対応する XElement この記述子を再生するために使用します。</span><span class="sxs-lookup"><span data-stu-id="86a5b-144">This routine returns an XmlSerializedDescriptorInfo which contains two properties: the XElement representation of the descriptor and the Type which represents an [IAuthenticatedEncryptorDescriptorDeserializer](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer) which can be used to resurrect this descriptor given the corresponding XElement.</span></span>

<span data-ttu-id="86a5b-145">シリアル化された記述子には、暗号化キー マテリアルなどの機密情報を含めることがあります。</span><span class="sxs-lookup"><span data-stu-id="86a5b-145">The serialized descriptor may contain sensitive information such as cryptographic key material.</span></span> <span data-ttu-id="86a5b-146">データ保護システムには、これが記憶域に保存される前に、情報を暗号化するための組み込みサポートがあります。</span><span class="sxs-lookup"><span data-stu-id="86a5b-146">The data protection system has built-in support for encrypting information before it's persisted to storage.</span></span> <span data-ttu-id="86a5b-147">記述子活用するためにこれを属性名"requiresEncryption"で機密情報を含む要素をマークする必要があります (xmlns"<http://schemas.asp.net/2015/03/dataProtection>")、"true"の値します。</span><span class="sxs-lookup"><span data-stu-id="86a5b-147">To take advantage of this, the descriptor should mark the element which contains sensitive information with the attribute name "requiresEncryption" (xmlns "<http://schemas.asp.net/2015/03/dataProtection>"), value "true".</span></span>

>[!TIP]
> <span data-ttu-id="86a5b-148">この属性を設定するヘルパー API があります。</span><span class="sxs-lookup"><span data-stu-id="86a5b-148">There's a helper API for setting this attribute.</span></span> <span data-ttu-id="86a5b-149">XElement.MarkAsRequiresEncryption() が Microsoft.AspNetCore.DataProtection.AuthenticatedEncryption.ConfigurationModel 名前空間にある拡張メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="86a5b-149">Call the extension method XElement.MarkAsRequiresEncryption() located in namespace Microsoft.AspNetCore.DataProtection.AuthenticatedEncryption.ConfigurationModel.</span></span>

<span data-ttu-id="86a5b-150">ここで、シリアル化された記述子が含まれていない機密情報には場合もあります。</span><span class="sxs-lookup"><span data-stu-id="86a5b-150">There can also be cases where the serialized descriptor doesn't contain sensitive information.</span></span> <span data-ttu-id="86a5b-151">HSM に格納されている暗号化キーの大文字と小文字を再度検討してください。</span><span class="sxs-lookup"><span data-stu-id="86a5b-151">Consider again the case of a cryptographic key stored in an HSM.</span></span> <span data-ttu-id="86a5b-152">HSM は、プレーン テキスト形式の内容を公開しないため自体をシリアル化した場合、キー マテリアルを記述子を書き込めません。</span><span class="sxs-lookup"><span data-stu-id="86a5b-152">The descriptor cannot write out the key material when serializing itself since the HSM won't expose the material in plaintext form.</span></span> <span data-ttu-id="86a5b-153">代わりに、記述子 (HSM は、この方法でエクスポートを許可する) 場合、キーまたはキーを HSM の一意識別子のキーによってラップされたバージョンが書き込まれる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="86a5b-153">Instead, the descriptor might write out the key-wrapped version of the key (if the HSM allows export in this fashion) or the HSM's own unique identifier for the key.</span></span>

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer"></a>

## <a name="iauthenticatedencryptordescriptordeserializer"></a><span data-ttu-id="86a5b-154">IAuthenticatedEncryptorDescriptorDeserializer</span><span class="sxs-lookup"><span data-stu-id="86a5b-154">IAuthenticatedEncryptorDescriptorDeserializer</span></span>

<span data-ttu-id="86a5b-155">**IAuthenticatedEncryptorDescriptorDeserializer**インターフェイスは XElement から IAuthenticatedEncryptorDescriptor インスタンスを逆シリアル化する方法を認識する型を表します。</span><span class="sxs-lookup"><span data-stu-id="86a5b-155">The **IAuthenticatedEncryptorDescriptorDeserializer** interface represents a type that knows how to deserialize an IAuthenticatedEncryptorDescriptor instance from an XElement.</span></span> <span data-ttu-id="86a5b-156">1 つのメソッドを公開します。</span><span class="sxs-lookup"><span data-stu-id="86a5b-156">It exposes a single method:</span></span>

* <span data-ttu-id="86a5b-157">ImportFromXml (XElement 要素): IAuthenticatedEncryptorDescriptor</span><span class="sxs-lookup"><span data-stu-id="86a5b-157">ImportFromXml(XElement element) : IAuthenticatedEncryptorDescriptor</span></span>

<span data-ttu-id="86a5b-158">ImportFromXml メソッドは、返された XElement [IAuthenticatedEncryptorDescriptor.ExportToXml](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-exporttoxml)され元 IAuthenticatedEncryptorDescriptor の同等なが作成されます。</span><span class="sxs-lookup"><span data-stu-id="86a5b-158">The ImportFromXml method takes the XElement that was returned by [IAuthenticatedEncryptorDescriptor.ExportToXml](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-exporttoxml) and creates an equivalent of the original IAuthenticatedEncryptorDescriptor.</span></span>

<span data-ttu-id="86a5b-159">IAuthenticatedEncryptorDescriptorDeserializer を実装する型は、次の 2 つのパブリック コンス トラクターのいずれかが必要です。</span><span class="sxs-lookup"><span data-stu-id="86a5b-159">Types which implement IAuthenticatedEncryptorDescriptorDeserializer should have one of the following two public constructors:</span></span>

* <span data-ttu-id="86a5b-160">.ctor(IServiceProvider)</span><span class="sxs-lookup"><span data-stu-id="86a5b-160">.ctor(IServiceProvider)</span></span>

* <span data-ttu-id="86a5b-161">.ctor()</span><span class="sxs-lookup"><span data-stu-id="86a5b-161">.ctor()</span></span>

> [!NOTE]
> <span data-ttu-id="86a5b-162">コンス トラクターに渡される IServiceProvider を null にすることがあります。</span><span class="sxs-lookup"><span data-stu-id="86a5b-162">The IServiceProvider passed to the constructor may be null.</span></span>

## <a name="the-top-level-factory"></a><span data-ttu-id="86a5b-163">最上位のファクトリ</span><span class="sxs-lookup"><span data-stu-id="86a5b-163">The top-level factory</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="86a5b-164">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="86a5b-164">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="86a5b-165">**AlgorithmConfiguration**クラスを作成する方法を知っている型を表す[IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor)インスタンス。</span><span class="sxs-lookup"><span data-stu-id="86a5b-165">The **AlgorithmConfiguration** class represents a type which knows how to create [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) instances.</span></span> <span data-ttu-id="86a5b-166">これは、単一の API を公開します。</span><span class="sxs-lookup"><span data-stu-id="86a5b-166">It exposes a single API.</span></span>

* <span data-ttu-id="86a5b-167">CreateNewDescriptor(): IAuthenticatedEncryptorDescriptor</span><span class="sxs-lookup"><span data-stu-id="86a5b-167">CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor</span></span>

<span data-ttu-id="86a5b-168">AlgorithmConfiguration の最上位のファクトリとして考えます。</span><span class="sxs-lookup"><span data-stu-id="86a5b-168">Think of AlgorithmConfiguration as the top-level factory.</span></span> <span data-ttu-id="86a5b-169">構成は、テンプレートとして機能します。</span><span class="sxs-lookup"><span data-stu-id="86a5b-169">The configuration serves as a template.</span></span> <span data-ttu-id="86a5b-170">それがラップ アルゴリズム情報 (例: この構成によって生成される値、AES 128-GCM マスター _ キーを持つ記述子) がまだ特定のキーに関連付けられているが、します。</span><span class="sxs-lookup"><span data-stu-id="86a5b-170">It wraps algorithmic information (e.g., this configuration produces descriptors with an AES-128-GCM master key), but it's not yet associated with a specific key.</span></span>

<span data-ttu-id="86a5b-171">CreateNewDescriptor し、呼ばれ、この呼び出しのためだけに最新のキー マテリアルが作成された新しい IAuthenticatedEncryptorDescriptor が生成されるときに、このキー マテリアル、および、マテリアルを使用する必要な情報をアルゴリズムをラップします。</span><span class="sxs-lookup"><span data-stu-id="86a5b-171">When CreateNewDescriptor is called, fresh key material is created solely for this call, and a new IAuthenticatedEncryptorDescriptor is produced which wraps this key material and the algorithmic information required to consume the material.</span></span> <span data-ttu-id="86a5b-172">キー マテリアルでしたするソフトウェアで作成された (メモリ内に保持) と作成され、HSM およびよびな内で保持されている可能性があります。</span><span class="sxs-lookup"><span data-stu-id="86a5b-172">The key material could be created in software (and held in memory), it could be created and held within an HSM, and so on.</span></span> <span data-ttu-id="86a5b-173">重要な点は、CreateNewDescriptor への呼び出しの 2 つが等価 IAuthenticatedEncryptorDescriptor インスタンスを作成しないでことです。</span><span class="sxs-lookup"><span data-stu-id="86a5b-173">The crucial point is that any two calls to CreateNewDescriptor should never create equivalent IAuthenticatedEncryptorDescriptor instances.</span></span>

<span data-ttu-id="86a5b-174">AlgorithmConfiguration 型ポイントとして機能、エントリのキーの作成ルーチンなど[自動キーのローリング](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling)です。</span><span class="sxs-lookup"><span data-stu-id="86a5b-174">The AlgorithmConfiguration type serves as the entry point for key creation routines such as [automatic key rolling](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling).</span></span> <span data-ttu-id="86a5b-175">将来のすべてのキーの実装を変更するには、KeyManagementOptions で AuthenticatedEncryptorConfiguration プロパティを設定します。</span><span class="sxs-lookup"><span data-stu-id="86a5b-175">To change the implementation for all future keys, set the AuthenticatedEncryptorConfiguration property in KeyManagementOptions.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="86a5b-176">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="86a5b-176">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="86a5b-177">**IAuthenticatedEncryptorConfiguration**インターフェイスを作成する方法を知っている型を表す[IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor)インスタンス。</span><span class="sxs-lookup"><span data-stu-id="86a5b-177">The **IAuthenticatedEncryptorConfiguration** interface represents a type which knows how to create [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) instances.</span></span> <span data-ttu-id="86a5b-178">これは、単一の API を公開します。</span><span class="sxs-lookup"><span data-stu-id="86a5b-178">It exposes a single API.</span></span>

* <span data-ttu-id="86a5b-179">CreateNewDescriptor(): IAuthenticatedEncryptorDescriptor</span><span class="sxs-lookup"><span data-stu-id="86a5b-179">CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor</span></span>

<span data-ttu-id="86a5b-180">IAuthenticatedEncryptorConfiguration の最上位のファクトリとして考えます。</span><span class="sxs-lookup"><span data-stu-id="86a5b-180">Think of IAuthenticatedEncryptorConfiguration as the top-level factory.</span></span> <span data-ttu-id="86a5b-181">構成は、テンプレートとして機能します。</span><span class="sxs-lookup"><span data-stu-id="86a5b-181">The configuration serves as a template.</span></span> <span data-ttu-id="86a5b-182">それがラップ アルゴリズム情報 (例: この構成によって生成される値、AES 128-GCM マスター _ キーを持つ記述子) がまだ特定のキーに関連付けられているが、します。</span><span class="sxs-lookup"><span data-stu-id="86a5b-182">It wraps algorithmic information (e.g., this configuration produces descriptors with an AES-128-GCM master key), but it's not yet associated with a specific key.</span></span>

<span data-ttu-id="86a5b-183">CreateNewDescriptor し、呼ばれ、この呼び出しのためだけに最新のキー マテリアルが作成された新しい IAuthenticatedEncryptorDescriptor が生成されるときに、このキー マテリアル、および、マテリアルを使用する必要な情報をアルゴリズムをラップします。</span><span class="sxs-lookup"><span data-stu-id="86a5b-183">When CreateNewDescriptor is called, fresh key material is created solely for this call, and a new IAuthenticatedEncryptorDescriptor is produced which wraps this key material and the algorithmic information required to consume the material.</span></span> <span data-ttu-id="86a5b-184">キー マテリアルでしたするソフトウェアで作成された (メモリ内に保持) と作成され、HSM およびよびな内で保持されている可能性があります。</span><span class="sxs-lookup"><span data-stu-id="86a5b-184">The key material could be created in software (and held in memory), it could be created and held within an HSM, and so on.</span></span> <span data-ttu-id="86a5b-185">重要な点は、CreateNewDescriptor への呼び出しの 2 つが等価 IAuthenticatedEncryptorDescriptor インスタンスを作成しないでことです。</span><span class="sxs-lookup"><span data-stu-id="86a5b-185">The crucial point is that any two calls to CreateNewDescriptor should never create equivalent IAuthenticatedEncryptorDescriptor instances.</span></span>

<span data-ttu-id="86a5b-186">IAuthenticatedEncryptorConfiguration 型ポイントとして機能、エントリのキーの作成ルーチンなど[自動キーのローリング](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling)です。</span><span class="sxs-lookup"><span data-stu-id="86a5b-186">The IAuthenticatedEncryptorConfiguration type serves as the entry point for key creation routines such as [automatic key rolling](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling).</span></span> <span data-ttu-id="86a5b-187">将来のすべてのキーの実装を変更するには、サービス コンテナーに IAuthenticatedEncryptorConfiguration シングルトンを登録します。</span><span class="sxs-lookup"><span data-stu-id="86a5b-187">To change the implementation for all future keys, register a singleton IAuthenticatedEncryptorConfiguration in the service container.</span></span>

---
