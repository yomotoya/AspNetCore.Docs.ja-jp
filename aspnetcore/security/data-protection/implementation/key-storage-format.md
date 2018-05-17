---
title: ASP.NET Core でのキー記憶域の形式
author: rick-anderson
description: ASP.NET Core データ保護キーの格納形式の実装の詳細を説明します。
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/key-storage-format
ms.openlocfilehash: 1a5912f246708355e6677c60034d982d053c3938
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/14/2018
---
# <a name="key-storage-format-in-aspnet-core"></a>ASP.NET Core でのキー記憶域の形式

<a name="data-protection-implementation-key-storage-format"></a>

オブジェクトは、XML 表現で残りの部分に保存されます。 キー記憶域の既定のディレクトリには、%localappdata%\asp.net\dataprotection-keys\ です。

## <a name="the-key-element"></a>\<キー > 要素

キーは、キーのリポジトリ内の最上位レベルのオブジェクトとして存在します。 キーが、ファイル名をある慣例**キー - {guid} .xml**{guid} はキーの id。 このような各ファイルには、1 つのキーが含まれています。 ファイルの形式は次のとおりです。

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

\<キー > 要素には、次の属性と子要素が含まれています。

* キーの id です。この値は権限を持つよう; 扱われますファイル名は、人間にとって読み終わりましただけです。

* バージョン、\<キー > 要素を 1 に現在固定します。

* キーの作成、アクティブ化、および有効期限の日付。

* A\<記述子 > 要素は、このキー内に含まれる認証済み暗号化の実装について説明します。

上記の例では、キーの id {80732141-ec8f-4b80-af9c-c4d2d1ff8901}、作成日時、2015 年 3 月 19 日にアクティブ化、90 日間の有効期間があります。 (場合によっては、アクティブ化された日付があります若干例のように、作成日の前にします。 これは、Api の動作とは実際には問題ありません nit のため) です。

## <a name="the-descriptor-element"></a>\<記述子 > 要素

外側\<記述子 > 要素には、属性 deserializerType、IAuthenticatedEncryptorDescriptorDeserializer を実装する型のアセンブリ修飾名が含まれています。 この型は、内部の読み取りを行う\<記述子 > 要素内に含まれる情報を解析しています。

特定の形式、\<記述子 > 要素が、キーによってカプセル化された認証済み暗号化機能の実装に依存し、各種のデシリアライザーはこのわずかに異なる形式が必要です。 一般に、ただし、この要素情報が含まれますアルゴリズム (名前、種類、Oid、または類似した) と秘密キー マテリアル。 上記の例では、記述子は、このキーが、AES 256-CBC 暗号化 + HMACSHA256 検証をラップするを指定します。

## <a name="the-encryptedsecret-element"></a>\<EncryptedSecret > 要素

<encryptedSecret>秘密キー マテリアルの暗号化されたフォームが含まれる要素が存在する場合[静止したシークレットの暗号化が有効になっている](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest)です。 属性 decryptorType IXmlDecryptor を実装する型のアセンブリ修飾名になります。 この型は、内部の読み取りを行う<encryptedKey>要素と回復元のプレーン テキストに復号化します。

同様に\<記述子 > の特定の形式、<encryptedSecret>要素が使用されて残りの部分での暗号化メカニズムに依存します。 上記の例では、Windows DPAPI を使用して、コメント、マスター _ キーが暗号化されます。

## <a name="the-revocation-element"></a>\<失効 > 要素

取り消した場合は、キーのリポジトリ内の最上位レベルのオブジェクトとして存在します。 失効慣例により、ファイル名がある**失効-{のタイムスタンプ} .xml** (特定の日付までのすべてのキーを取り消す) 用または**失効 - {guid} .xml** (を取り消す場合に特定のキー)。 各ファイルには、1 つ\<失効 > 要素。

個々 のキーを取り消した場合、ファイルの内容になります次のようです。

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T22:45:30.2616742Z</revocationDate>
  <key id="eb4fc299-8808-409d-8a34-23fc83d026c9" />
  <reason>human-readable reason</reason>
</revocation>
```

この場合、指定したキーのみが失効します。 場合は、キー id は"*"、ただし、ように、次の例では、作成日付を持つ、指定された失効日より前のすべてのキーが失効しています。

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T15:45:45.7366491-07:00</revocationDate>
  <!-- All keys created before the revocation date are revoked. -->
  <key id="*" />
  <reason>human-readable reason</reason>
</revocation>
```

\<理由 > 要素が、システムによって読み取られることはありません。 人間が判読できる失効理由を格納する便利な場所では単純にすることをお勧めします。
