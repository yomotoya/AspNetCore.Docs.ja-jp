---
title: "キー記憶域の形式"
author: tdykstra
description: "このドキュメントでは、ASP.NET Core データ保護キーの格納形式の実装の詳細について説明します。"
keywords: "ASP.NET Core、データ保護、キー ストレージ"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: e8996478-f7bf-4b58-bab4-7fdb5d8556c5
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-storage-format
ms.openlocfilehash: 4ebad05f7d55e954463ce5e277b419a7d6f773c0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="key-storage-format"></a><span data-ttu-id="b2fd1-104">キー記憶域の形式</span><span class="sxs-lookup"><span data-stu-id="b2fd1-104">Key Storage Format</span></span>

<a name="data-protection-implementation-key-storage-format"></a>

<span data-ttu-id="b2fd1-105">オブジェクトは、XML 表現で残りの部分に保存されます。</span><span class="sxs-lookup"><span data-stu-id="b2fd1-105">Objects are stored at rest in XML representation.</span></span> <span data-ttu-id="b2fd1-106">キー記憶域の既定のディレクトリには、%localappdata%\asp.net\dataprotection-keys\ です。</span><span class="sxs-lookup"><span data-stu-id="b2fd1-106">The default directory for key storage is %LOCALAPPDATA%\ASP.NET\DataProtection-Keys\.</span></span>

## <a name="the-key-element"></a><span data-ttu-id="b2fd1-107">\<キー > 要素</span><span class="sxs-lookup"><span data-stu-id="b2fd1-107">The \<key> element</span></span>

<span data-ttu-id="b2fd1-108">キーは、キーのリポジトリ内の最上位レベルのオブジェクトとして存在します。</span><span class="sxs-lookup"><span data-stu-id="b2fd1-108">Keys exist as top-level objects in the key repository.</span></span> <span data-ttu-id="b2fd1-109">キーが、ファイル名をある慣例**キー - {guid} .xml**{guid} はキーの id。</span><span class="sxs-lookup"><span data-stu-id="b2fd1-109">By convention keys have the filename **key-{guid}.xml**, where {guid} is the id of the key.</span></span> <span data-ttu-id="b2fd1-110">このような各ファイルには、1 つのキーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="b2fd1-110">Each such file contains a single key.</span></span> <span data-ttu-id="b2fd1-111">ファイルの形式は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="b2fd1-111">The format of the file is as follows.</span></span>

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

<span data-ttu-id="b2fd1-112">\<キー > 要素には、次の属性と子要素が含まれています。</span><span class="sxs-lookup"><span data-stu-id="b2fd1-112">The \<key> element contains the following attributes and child elements:</span></span>

* <span data-ttu-id="b2fd1-113">キーの id です。この値は権限を持つよう; 扱われますファイル名は、人間にとって読み終わりましただけです。</span><span class="sxs-lookup"><span data-stu-id="b2fd1-113">The key id. This value is treated as authoritative; the filename is simply a nicety for human readability.</span></span>

* <span data-ttu-id="b2fd1-114">バージョン、\<キー > 要素を 1 に現在固定します。</span><span class="sxs-lookup"><span data-stu-id="b2fd1-114">The version of the \<key> element, currently fixed at 1.</span></span>

* <span data-ttu-id="b2fd1-115">キーの作成、アクティブ化、および有効期限の日付。</span><span class="sxs-lookup"><span data-stu-id="b2fd1-115">The key's creation, activation, and expiration dates.</span></span>

* <span data-ttu-id="b2fd1-116">A\<記述子 > 要素は、このキー内に含まれる認証済み暗号化の実装について説明します。</span><span class="sxs-lookup"><span data-stu-id="b2fd1-116">A \<descriptor> element, which contains information on the authenticated encryption implementation contained within this key.</span></span>

<span data-ttu-id="b2fd1-117">上記の例では、キーの id {80732141-ec8f-4b80-af9c-c4d2d1ff8901}、作成日時、2015 年 3 月 19 日にアクティブ化、90 日間の有効期間があります。</span><span class="sxs-lookup"><span data-stu-id="b2fd1-117">In the above example, the key's id is {80732141-ec8f-4b80-af9c-c4d2d1ff8901}, it was created and activated on March 19, 2015, and it has a lifetime of 90 days.</span></span> <span data-ttu-id="b2fd1-118">(場合によっては、アクティブ化された日付があります若干例のように、作成日の前にします。</span><span class="sxs-lookup"><span data-stu-id="b2fd1-118">(Occasionally the activation date might be slightly before the creation date as in this example.</span></span> <span data-ttu-id="b2fd1-119">これは、Api の動作とは実際には問題ありません nit のため) です。</span><span class="sxs-lookup"><span data-stu-id="b2fd1-119">This is due to a nit in how the APIs work and is harmless in practice.)</span></span>

## <a name="the-descriptor-element"></a><span data-ttu-id="b2fd1-120">\<記述子 > 要素</span><span class="sxs-lookup"><span data-stu-id="b2fd1-120">The \<descriptor> element</span></span>

<span data-ttu-id="b2fd1-121">外側\<記述子 > 要素には、属性 deserializerType、IAuthenticatedEncryptorDescriptorDeserializer を実装する型のアセンブリ修飾名が含まれています。</span><span class="sxs-lookup"><span data-stu-id="b2fd1-121">The outer \<descriptor> element contains an attribute deserializerType, which is the assembly-qualified name of a type which implements IAuthenticatedEncryptorDescriptorDeserializer.</span></span> <span data-ttu-id="b2fd1-122">この型は、内部の読み取りを行う\<記述子 > 要素内に含まれる情報を解析しています。</span><span class="sxs-lookup"><span data-stu-id="b2fd1-122">This type is responsible for reading the inner \<descriptor> element and for parsing the information contained within.</span></span>

<span data-ttu-id="b2fd1-123">特定の形式、\<記述子 > 要素が、キーによってカプセル化された認証済み暗号化機能の実装に依存し、各種のデシリアライザーはこのわずかに異なる形式が必要です。</span><span class="sxs-lookup"><span data-stu-id="b2fd1-123">The particular format of the \<descriptor> element depends on the authenticated encryptor implementation encapsulated by the key, and each deserializer type expects a slightly different format for this.</span></span> <span data-ttu-id="b2fd1-124">一般に、ただし、この要素情報が含まれますアルゴリズム (名前、種類、Oid、または類似した) と秘密キー マテリアル。</span><span class="sxs-lookup"><span data-stu-id="b2fd1-124">In general, though, this element will contain algorithmic information (names, types, OIDs, or similar) and secret key material.</span></span> <span data-ttu-id="b2fd1-125">上記の例では、記述子は、このキーが、AES 256-CBC 暗号化 + HMACSHA256 検証をラップするを指定します。</span><span class="sxs-lookup"><span data-stu-id="b2fd1-125">In the above example, the descriptor specifies that this key wraps AES-256-CBC encryption + HMACSHA256 validation.</span></span>

## <a name="the-encryptedsecret-element"></a><span data-ttu-id="b2fd1-126">\<EncryptedSecret > 要素</span><span class="sxs-lookup"><span data-stu-id="b2fd1-126">The \<encryptedSecret> element</span></span>

<span data-ttu-id="b2fd1-127"><encryptedSecret>秘密キー マテリアルの暗号化されたフォームが含まれる要素が存在する場合[静止したシークレットの暗号化が有効になっている](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest)です。</span><span class="sxs-lookup"><span data-stu-id="b2fd1-127">An <encryptedSecret> element which contains the encrypted form of the secret key material may be present if [encryption of secrets at rest is enabled](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest).</span></span> <span data-ttu-id="b2fd1-128">属性 decryptorType IXmlDecryptor を実装する型のアセンブリ修飾名になります。</span><span class="sxs-lookup"><span data-stu-id="b2fd1-128">The attribute decryptorType will be the assembly-qualified name of a type which implements IXmlDecryptor.</span></span> <span data-ttu-id="b2fd1-129">この型は、内部の読み取りを行う<encryptedKey>要素と回復元のプレーン テキストに復号化します。</span><span class="sxs-lookup"><span data-stu-id="b2fd1-129">This type is responsible for reading the inner <encryptedKey> element and decrypting it to recover the original plaintext.</span></span>

<span data-ttu-id="b2fd1-130">同様に\<記述子 > の特定の形式、<encryptedSecret>要素が使用されて残りの部分での暗号化メカニズムに依存します。</span><span class="sxs-lookup"><span data-stu-id="b2fd1-130">As with \<descriptor>, the particular format of the <encryptedSecret> element depends on the at-rest encryption mechanism in use.</span></span> <span data-ttu-id="b2fd1-131">上記の例では、Windows DPAPI を使用して、コメント、マスター _ キーが暗号化されます。</span><span class="sxs-lookup"><span data-stu-id="b2fd1-131">In the above example, the master key is encrypted using Windows DPAPI per the comment.</span></span>

## <a name="the-revocation-element"></a><span data-ttu-id="b2fd1-132">\<失効 > 要素</span><span class="sxs-lookup"><span data-stu-id="b2fd1-132">The \<revocation> element</span></span>

<span data-ttu-id="b2fd1-133">取り消した場合は、キーのリポジトリ内の最上位レベルのオブジェクトとして存在します。</span><span class="sxs-lookup"><span data-stu-id="b2fd1-133">Revocations exist as top-level objects in the key repository.</span></span> <span data-ttu-id="b2fd1-134">失効慣例により、ファイル名がある**失効-{のタイムスタンプ} .xml** (特定の日付までのすべてのキーを取り消す) 用または**失効 - {guid} .xml** (を取り消す場合に特定のキー)。</span><span class="sxs-lookup"><span data-stu-id="b2fd1-134">By convention revocations have the filename **revocation-{timestamp}.xml** (for revoking all keys before a specific date) or **revocation-{guid}.xml** (for revoking a specific key).</span></span> <span data-ttu-id="b2fd1-135">各ファイルには、1 つ\<失効 > 要素。</span><span class="sxs-lookup"><span data-stu-id="b2fd1-135">Each file contains a single \<revocation> element.</span></span>

<span data-ttu-id="b2fd1-136">個々 のキーを取り消した場合、ファイルの内容になります次のようです。</span><span class="sxs-lookup"><span data-stu-id="b2fd1-136">For revocations of individual keys, the file contents will be as below.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T22:45:30.2616742Z</revocationDate>
  <key id="eb4fc299-8808-409d-8a34-23fc83d026c9" />
  <reason>human-readable reason</reason>
</revocation>
```

<span data-ttu-id="b2fd1-137">この場合、指定したキーのみが失効します。</span><span class="sxs-lookup"><span data-stu-id="b2fd1-137">In this case, only the specified key is revoked.</span></span> <span data-ttu-id="b2fd1-138">場合は、キー id は"*"、ただし、ように、次の例では、作成日付を持つ、指定された失効日より前のすべてのキーが失効しています。</span><span class="sxs-lookup"><span data-stu-id="b2fd1-138">If the key id is "*", however, as in the below example, all keys whose creation date is prior to the specified revocation date are revoked.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T15:45:45.7366491-07:00</revocationDate>
  <!-- All keys created before the revocation date are revoked. -->
  <key id="*" />
  <reason>human-readable reason</reason>
</revocation>
```

<span data-ttu-id="b2fd1-139">\<理由 > 要素が、システムによって読み取られることはありません。</span><span class="sxs-lookup"><span data-stu-id="b2fd1-139">The \<reason> element is never read by the system.</span></span> <span data-ttu-id="b2fd1-140">人間が判読できる失効理由を格納する便利な場所では単純にすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="b2fd1-140">It is simply a convenient place to store a human-readable reason for revocation.</span></span>
