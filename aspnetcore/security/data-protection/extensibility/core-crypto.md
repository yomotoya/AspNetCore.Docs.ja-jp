---
title: "コア暗号化の拡張性"
author: rick-anderson
description: "IAuthenticatedEncryptor、IAuthenticatedEncryptorDescriptor、IAuthenticatedEncryptorDescriptorDeserializer、および最上位のファクトリについて説明します。"
ms.author: riande
manager: wpickett
ms.date: 8/11/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/core-crypto
ms.openlocfilehash: b82c30fe40c4badc74645dafa9f0d13f6ffae031
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="core-cryptography-extensibility"></a>コア暗号化の拡張性

<a name="data-protection-extensibility-core-crypto"></a>

>[!WARNING]
> 次のインターフェイスのいずれかを実装する型はスレッド セーフである必要があります複数の呼び出し元のです。

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptor"></a>

## <a name="iauthenticatedencryptor"></a>IAuthenticatedEncryptor

**IAuthenticatedEncryptor**インターフェイスは、暗号化サブシステムの基本的なビルド ブロックです。 一般には、キー、ごとの 1 つの IAuthenticatedEncryptor と IAuthenticatedEncryptor インスタンスは、すべての暗号化キー マテリアルと暗号化操作の実行に必要なアルゴリズムの情報をラップします。

その名前からわかるように、型、認証済みの暗号化と復号化サービスを提供します。 これは、次の 2 つの Api を公開します。

* 復号化 (ArraySegment<byte>暗号文、ArraySegment<byte> additionalAuthenticatedData): byte[]

* 暗号化 (ArraySegment<byte>プレーン テキスト、ArraySegment<byte> additionalAuthenticatedData): byte[]

暗号化メソッドは、enciphered プレーン テキストとな認証タグを含む blob を返します。 認証タグは、AAD 自体は、最終的なペイロードから回復可能な必要はありませんが、追加の認証済みデータ (AAD) を囲んでいる必要があります。 復号化メソッドは、認証タグを検証し、解読はペイロードを返します。 すべてのエラー (ArgumentNullException を除くと似ています) は、CryptographicException に homogenized 必要があります。

> [!NOTE]
> IAuthenticatedEncryptor インスタンス自体実際には、キー マテリアルを格納する必要はありません。 たとえば、実装では、すべての操作の HSM に委任します。

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptorfactory"></a>
<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor"></a>

## <a name="how-to-create-an-iauthenticatedencryptor"></a>IAuthenticatedEncryptor を作成する方法

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

**IAuthenticatedEncryptorFactory**インターフェイスを作成する方法を認識する型を表す、 [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor)インスタンス。 その API は次のとおりです。

* CreateEncryptorInstance (IKey キー): IAuthenticatedEncryptor

指定された IKey インスタンスに対してその CreateEncryptorInstance メソッドによって作成された任意の認証済み受け付け必要があります等価と見なされる、ように、次のコード例です。

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

**IAuthenticatedEncryptorDescriptor**インターフェイスを作成する方法を認識する型を表す、 [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor)インスタンス。 その API は次のとおりです。

* CreateEncryptorInstance(): IAuthenticatedEncryptor

* ExportToXml(): XmlSerializedDescriptorInfo

IAuthenticatedEncryptor と同様に IAuthenticatedEncryptorDescriptor のインスタンスが 1 つの特定のキーをラップすると見なされます。 つまりが、特定の IAuthenticatedEncryptorDescriptor インスタンス、その CreateEncryptorInstance メソッドによって作成された任意の認証済み受け付け必要がありますと見なされるように、同等の次のコード例です。

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

## <a name="iauthenticatedencryptordescriptor-aspnet-core-2x-only"></a>IAuthenticatedEncryptorDescriptor (ASP.NET のコア 2.x のみ)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

**IAuthenticatedEncryptorDescriptor**インターフェイスは、型自体を XML にエクスポートする方法を把握していることを表します。 その API は次のとおりです。

* ExportToXml(): XmlSerializedDescriptorInfo

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

---

## <a name="xml-serialization"></a>XML シリアル化

IAuthenticatedEncryptor と IAuthenticatedEncryptorDescriptor の主な違いは、記述子が、暗号化機能を作成し、有効な引数で指定する方法を知っていることです。 これらの実装では、対称アルゴリズムおよび KeyedHashAlgorithm 依存、IAuthenticatedEncryptor を検討してください。 暗号化機能のジョブが、これらの型を使用するが、必ずしもを認識していないこれらの型が送信元アプリケーションが再起動した場合、それ自体を再作成する方法の適切な説明を本当に書き込むことができないようにします。 記述子は、さらに高いレベルとして機能します。 記述子は、暗号化機能のインスタンスを作成する方法を認識しているため (たとえば、認識必要なアルゴリズムを作成する方法)、アプリケーションをリセットした後、暗号化機能のインスタンスが再作成できるようには XML 形式でそのナレッジがシリアル化します。

<a name="data-protection-extensibility-core-crypto-exporttoxml"></a>

記述子は、その ExportToXml ルーチンを使用してシリアル化できます。 このルーチンは、次の 2 つのプロパティが含まれて XmlSerializedDescriptorInfo を返します: 記述子とを表す型の XElement 形式、 [IAuthenticatedEncryptorDescriptorDeserializer](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer)があります指定された対応する XElement この記述子を再生するために使用します。

シリアル化された記述子には、暗号化キー マテリアルなどの機密情報を含めることがあります。 データ保護システムには、これが記憶域に保存される前に、情報を暗号化するための組み込みサポートがあります。 これを利用するには、記述子は (xmlns"http://schemas.asp.net/2015/03/dataProtection")、値"true"を属性名"requiresEncryption"で機密情報を含む要素をマークする必要があります。

>[!TIP]
> この属性を設定するヘルパー API があります。 XElement.MarkAsRequiresEncryption() が Microsoft.AspNetCore.DataProtection.AuthenticatedEncryption.ConfigurationModel 名前空間にある拡張メソッドを呼び出します。

ここで、シリアル化された記述子が含まれていない機密情報には場合もあります。 HSM に格納されている暗号化キーの大文字と小文字を再度検討してください。 記述子は、HSM はプレーン テキスト形式の内容を公開しないために、自体シリアル化するとき、キー マテリアルを書き込むことはできません。 代わりに、記述子 (HSM は、この方法でエクスポートを許可する) 場合、キーまたはキーを HSM の一意識別子のキーによってラップされたバージョンが書き込まれる可能性があります。

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer"></a>

## <a name="iauthenticatedencryptordescriptordeserializer"></a>IAuthenticatedEncryptorDescriptorDeserializer

**IAuthenticatedEncryptorDescriptorDeserializer**インターフェイスは XElement から IAuthenticatedEncryptorDescriptor インスタンスを逆シリアル化する方法を認識する型を表します。 1 つのメソッドを公開します。

* ImportFromXml (XElement 要素): IAuthenticatedEncryptorDescriptor

ImportFromXml メソッドは、返された XElement [IAuthenticatedEncryptorDescriptor.ExportToXml](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-exporttoxml)され元 IAuthenticatedEncryptorDescriptor の同等なが作成されます。

IAuthenticatedEncryptorDescriptorDeserializer を実装する型は、次の 2 つのパブリック コンス トラクターのいずれかが必要です。

* .ctor(IServiceProvider)

* .ctor()

> [!NOTE]
> コンス トラクターに渡される IServiceProvider を null にすることがあります。

## <a name="the-top-level-factory"></a>最上位のファクトリ

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

**AlgorithmConfiguration**クラスを作成する方法を知っている型を表す[IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor)インスタンス。 これは、単一の API を公開します。

* CreateNewDescriptor(): IAuthenticatedEncryptorDescriptor

AlgorithmConfiguration の最上位のファクトリとして考えます。 構成は、テンプレートとして機能します。 アルゴリズムの情報をラップ (例: この構成によって生成される値、AES 128-GCM マスター _ キーを持つ記述子) がまだ特定のキーでに関連付けられていません。

CreateNewDescriptor し、呼ばれ、この呼び出しのためだけに最新のキー マテリアルが作成された新しい IAuthenticatedEncryptorDescriptor が生成されるときに、このキー マテリアル、および、マテリアルを使用する必要な情報をアルゴリズムをラップします。 キー マテリアルでしたするソフトウェアで作成された (メモリ内に保持) と作成され、HSM およびよびな内で保持されている可能性があります。 重要な点は、CreateNewDescriptor への呼び出しの 2 つが等価 IAuthenticatedEncryptorDescriptor インスタンスを作成しないでことです。

AlgorithmConfiguration 型ポイントとして機能、エントリのキーの作成ルーチンなど[自動キーのローリング](../implementation/key-management.md#key-expiration-and-rolling)です。 将来のすべてのキーの実装を変更するには、KeyManagementOptions で AuthenticatedEncryptorConfiguration プロパティを設定します。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

**IAuthenticatedEncryptorConfiguration**インターフェイスを作成する方法を知っている型を表す[IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor)インスタンス。 これは、単一の API を公開します。

* CreateNewDescriptor(): IAuthenticatedEncryptorDescriptor

IAuthenticatedEncryptorConfiguration の最上位のファクトリとして考えます。 構成は、テンプレートとして機能します。 アルゴリズムの情報をラップ (例: この構成によって生成される値、AES 128-GCM マスター _ キーを持つ記述子) がまだ特定のキーでに関連付けられていません。

CreateNewDescriptor し、呼ばれ、この呼び出しのためだけに最新のキー マテリアルが作成された新しい IAuthenticatedEncryptorDescriptor が生成されるときに、このキー マテリアル、および、マテリアルを使用する必要な情報をアルゴリズムをラップします。 キー マテリアルでしたするソフトウェアで作成された (メモリ内に保持) と作成され、HSM およびよびな内で保持されている可能性があります。 重要な点は、CreateNewDescriptor への呼び出しの 2 つが等価 IAuthenticatedEncryptorDescriptor インスタンスを作成しないでことです。

IAuthenticatedEncryptorConfiguration 型ポイントとして機能、エントリのキーの作成ルーチンなど[自動キーのローリング](../implementation/key-management.md#key-expiration-and-rolling)です。 将来のすべてのキーの実装を変更するには、サービス コンテナーに IAuthenticatedEncryptorConfiguration シングルトンを登録します。

---
