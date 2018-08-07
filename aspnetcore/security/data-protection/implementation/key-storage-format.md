---
title: ASP.NET Core でのキー ストレージの形式
author: rick-anderson
description: ASP.NET Core データ保護キー ストレージの形式の実装の詳細について説明します。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-storage-format
ms.openlocfilehash: bca19ad001dd20b5d02ae5470f7d928082496037
ms.sourcegitcommit: 8f8924ce4eb9effeaf489f177fb01b66867da16f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/24/2018
ms.locfileid: "39219278"
---
# <a name="key-storage-format-in-aspnet-core"></a><span data-ttu-id="eedba-103">ASP.NET Core でのキー ストレージの形式</span><span class="sxs-lookup"><span data-stu-id="eedba-103">Key storage format in ASP.NET Core</span></span>

<a name="data-protection-implementation-key-storage-format"></a>

<span data-ttu-id="eedba-104">オブジェクトは、XML 表記で残りの部分に保存されます。</span><span class="sxs-lookup"><span data-stu-id="eedba-104">Objects are stored at rest in XML representation.</span></span> <span data-ttu-id="eedba-105">キー記憶域の既定のディレクトリには、%localappdata%\asp.net\dataprotection-keys\ です。</span><span class="sxs-lookup"><span data-stu-id="eedba-105">The default directory for key storage is %LOCALAPPDATA%\ASP.NET\DataProtection-Keys\.</span></span>

## <a name="the-key-element"></a><span data-ttu-id="eedba-106">\<キー > 要素</span><span class="sxs-lookup"><span data-stu-id="eedba-106">The \<key> element</span></span>

<span data-ttu-id="eedba-107">キーは、キーのリポジトリ内の最上位レベルのオブジェクトとして存在します。</span><span class="sxs-lookup"><span data-stu-id="eedba-107">Keys exist as top-level objects in the key repository.</span></span> <span data-ttu-id="eedba-108">規則によりキーは、ファイル名をある**キー - {guid} .xml**{guid} はキーの id です。</span><span class="sxs-lookup"><span data-stu-id="eedba-108">By convention keys have the filename **key-{guid}.xml**, where {guid} is the id of the key.</span></span> <span data-ttu-id="eedba-109">このような各ファイルには、1 つのキーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="eedba-109">Each such file contains a single key.</span></span> <span data-ttu-id="eedba-110">ファイルの形式は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="eedba-110">The format of the file is as follows.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<key id="80732141-ec8f-4b80-af9c-c4d2d1ff8901" version="1">
  <creationDate>2015-03-19T23:32:02.3949887Z</creationDate>
  <activationDate>2015-03-19T23:32:02.3839429Z</activationDate>
  <expirationDate>2015-06-17T23:32:02.3839429Z</expirationDate>
  <descriptor deserializerType="{deserializerType}">
    <descriptor>
      <encryption algorithm="AES_256_CBC" />
      <validation algorithm="HMACSHA256" />
      <enc:encryptedSecret decryptorType="{decryptorType}" xmlns:enc="...">
        <encryptedKey>
          <!-- This key is encrypted with Windows DPAPI. -->
          <value>AQAAANCM...8/zeP8lcwAg==</value>
        </encryptedKey>
      </enc:encryptedSecret>
    </descriptor>
  </descriptor>
</key>
```

<span data-ttu-id="eedba-111">\<キー > 要素には、次の属性と子要素が含まれています。</span><span class="sxs-lookup"><span data-stu-id="eedba-111">The \<key> element contains the following attributes and child elements:</span></span>

* <span data-ttu-id="eedba-112">キーの id。この値は、権限のある; に扱われますファイル名は、人間にとって読み言えませんだけです。</span><span class="sxs-lookup"><span data-stu-id="eedba-112">The key id. This value is treated as authoritative; the filename is simply a nicety for human readability.</span></span>

* <span data-ttu-id="eedba-113">バージョン、\<キー > 要素を 1 に現在固定されています。</span><span class="sxs-lookup"><span data-stu-id="eedba-113">The version of the \<key> element, currently fixed at 1.</span></span>

* <span data-ttu-id="eedba-114">キーの作成、ライセンス認証、および有効期限の日付。</span><span class="sxs-lookup"><span data-stu-id="eedba-114">The key's creation, activation, and expiration dates.</span></span>

* <span data-ttu-id="eedba-115">A\<記述子 > 要素で、このキー内に含まれる、認証された暗号化の実装について説明します。</span><span class="sxs-lookup"><span data-stu-id="eedba-115">A \<descriptor> element, which contains information on the authenticated encryption implementation contained within this key.</span></span>

<span data-ttu-id="eedba-116">上記の例では、キーの id に {80732141-ec8f-4b80-af9c-c4d2d1ff8901} が作成され、2015 年 3 月 19 日で有効になっても、90 日間の有効期間があります。</span><span class="sxs-lookup"><span data-stu-id="eedba-116">In the above example, the key's id is {80732141-ec8f-4b80-af9c-c4d2d1ff8901}, it was created and activated on March 19, 2015, and it has a lifetime of 90 days.</span></span> <span data-ttu-id="eedba-117">(場合によっては、アクティブ化する日があります少しこの例のように、作成日の前にします。</span><span class="sxs-lookup"><span data-stu-id="eedba-117">(Occasionally the activation date might be slightly before the creation date as in this example.</span></span> <span data-ttu-id="eedba-118">これは、Api の動作し、は、実際には問題でのつまらぬのため) です。</span><span class="sxs-lookup"><span data-stu-id="eedba-118">This is due to a nit in how the APIs work and is harmless in practice.)</span></span>

## <a name="the-descriptor-element"></a><span data-ttu-id="eedba-119">\<記述子 > 要素</span><span class="sxs-lookup"><span data-stu-id="eedba-119">The \<descriptor> element</span></span>

<span data-ttu-id="eedba-120">外側\<記述子 > 要素には、属性 deserializerType、IAuthenticatedEncryptorDescriptorDeserializer を実装する型のアセンブリ修飾名が含まれています。</span><span class="sxs-lookup"><span data-stu-id="eedba-120">The outer \<descriptor> element contains an attribute deserializerType, which is the assembly-qualified name of a type which implements IAuthenticatedEncryptorDescriptorDeserializer.</span></span> <span data-ttu-id="eedba-121">この型は、内部の読み取りを行う\<記述子 > 要素に含まれる情報を解析しています。</span><span class="sxs-lookup"><span data-stu-id="eedba-121">This type is responsible for reading the inner \<descriptor> element and for parsing the information contained within.</span></span>

<span data-ttu-id="eedba-122">特定の形式、\<記述子 > 要素をキーでカプセル化された認証済みの暗号化機能の実装に依存し、各デシリアライザ-がの型は、このわずかに異なる形式が必要です。</span><span class="sxs-lookup"><span data-stu-id="eedba-122">The particular format of the \<descriptor> element depends on the authenticated encryptor implementation encapsulated by the key, and each deserializer type expects a slightly different format for this.</span></span> <span data-ttu-id="eedba-123">この要素が、アルゴリズムの情報に含めるは一般に、(名前、種類、Oid、または類似した) とシークレット キー マテリアル。</span><span class="sxs-lookup"><span data-stu-id="eedba-123">In general, though, this element will contain algorithmic information (names, types, OIDs, or similar) and secret key material.</span></span> <span data-ttu-id="eedba-124">上記の例では、記述子は、このキーが、AES-CBC 256 暗号化 + HMACSHA256 検証をラップするを指定します。</span><span class="sxs-lookup"><span data-stu-id="eedba-124">In the above example, the descriptor specifies that this key wraps AES-256-CBC encryption + HMACSHA256 validation.</span></span>

## <a name="the-encryptedsecret-element"></a><span data-ttu-id="eedba-125">\<EncryptedSecret > 要素</span><span class="sxs-lookup"><span data-stu-id="eedba-125">The \<encryptedSecret> element</span></span>

<span data-ttu-id="eedba-126">**&lt;EncryptedSecret&gt;** 秘密キー マテリアルの暗号化されたフォームが含まれる要素が存在する可能性がある場合[rest でのシークレットの暗号化が有効になっている](xref:security/data-protection/implementation/key-encryption-at-rest)します。</span><span class="sxs-lookup"><span data-stu-id="eedba-126">An **&lt;encryptedSecret&gt;** element which contains the encrypted form of the secret key material may be present if [encryption of secrets at rest is enabled](xref:security/data-protection/implementation/key-encryption-at-rest).</span></span> <span data-ttu-id="eedba-127">属性`decryptorType`を実装する型のアセンブリ修飾名は、 [IXmlDecryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmldecryptor)します。</span><span class="sxs-lookup"><span data-stu-id="eedba-127">The attribute `decryptorType` is the assembly-qualified name of a type which implements [IXmlDecryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmldecryptor).</span></span> <span data-ttu-id="eedba-128">この型は、内部の読み取りを行う **&lt;encryptedKey&gt;** 要素と、元のプレーン テキストを回復する復号化します。</span><span class="sxs-lookup"><span data-stu-id="eedba-128">This type is responsible for reading the inner **&lt;encryptedKey&gt;** element and decrypting it to recover the original plaintext.</span></span>

<span data-ttu-id="eedba-129">同様\<記述子 > の特定の形式、<encryptedSecret>要素が使用中の保存時暗号化メカニズムに依存します。</span><span class="sxs-lookup"><span data-stu-id="eedba-129">As with \<descriptor>, the particular format of the <encryptedSecret> element depends on the at-rest encryption mechanism in use.</span></span> <span data-ttu-id="eedba-130">上記の例では、Windows DPAPI を使用して、コメント、マスター _ キーが暗号化します。</span><span class="sxs-lookup"><span data-stu-id="eedba-130">In the above example, the master key is encrypted using Windows DPAPI per the comment.</span></span>

## <a name="the-revocation-element"></a><span data-ttu-id="eedba-131">\<失効 > 要素</span><span class="sxs-lookup"><span data-stu-id="eedba-131">The \<revocation> element</span></span>

<span data-ttu-id="eedba-132">失効は、キーのリポジトリ内の最上位レベルのオブジェクトとして存在します。</span><span class="sxs-lookup"><span data-stu-id="eedba-132">Revocations exist as top-level objects in the key repository.</span></span> <span data-ttu-id="eedba-133">失効規則により、ファイル名がある**失効-{のタイムスタンプ}.xml** (特定の日付までのすべてのキーを取り消す) 用または**失効-{guid}.xml** (特定のキーを取り消す) 用です。</span><span class="sxs-lookup"><span data-stu-id="eedba-133">By convention revocations have the filename **revocation-{timestamp}.xml** (for revoking all keys before a specific date) or **revocation-{guid}.xml** (for revoking a specific key).</span></span> <span data-ttu-id="eedba-134">各ファイルには、1 つ\<失効> 要素。</span><span class="sxs-lookup"><span data-stu-id="eedba-134">Each file contains a single \<revocation> element.</span></span>

<span data-ttu-id="eedba-135">ファイルの内容になります個々 のキーを取り消した場合は、次のようです。</span><span class="sxs-lookup"><span data-stu-id="eedba-135">For revocations of individual keys, the file contents will be as below.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T22:45:30.2616742Z</revocationDate>
  <key id="eb4fc299-8808-409d-8a34-23fc83d026c9" />
  <reason>human-readable reason</reason>
</revocation>
```

<span data-ttu-id="eedba-136">この場合、指定したキーのみが失効します。</span><span class="sxs-lookup"><span data-stu-id="eedba-136">In this case, only the specified key is revoked.</span></span> <span data-ttu-id="eedba-137">場合は、キー id"\*"、ただし、ように、次の例では、作成日付が指定された失効日より前のすべてのキーが失効しました。</span><span class="sxs-lookup"><span data-stu-id="eedba-137">If the key id is "\*", however, as in the below example, all keys whose creation date is prior to the specified revocation date are revoked.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T15:45:45.7366491-07:00</revocationDate>
  <!-- All keys created before the revocation date are revoked. -->
  <key id="*" />
  <reason>human-readable reason</reason>
</revocation>
```

<span data-ttu-id="eedba-138">\<理由 > 要素が、システムによって読み取られることはありません。</span><span class="sxs-lookup"><span data-stu-id="eedba-138">The \<reason> element is never read by the system.</span></span> <span data-ttu-id="eedba-139">失効の人間が判読できる理由を格納する便利な場所だけになります。</span><span class="sxs-lookup"><span data-stu-id="eedba-139">It's simply a convenient place to store a human-readable reason for revocation.</span></span>
