---
title: ASP.NET Core でのキー ストレージの形式
author: rick-anderson
description: ASP.NET Core データ保護キー ストレージの形式の実装の詳細について説明します。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-storage-format
ms.openlocfilehash: 81df124f3dd0cadf8fd895ab55f66eec6415705f
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897479"
---
# <a name="key-storage-format-in-aspnet-core"></a>ASP.NET Core でのキー ストレージの形式

<a name="data-protection-implementation-key-storage-format"></a>

オブジェクトは、XML 表記で残りの部分に保存されます。 キー記憶域の既定のディレクトリには、%localappdata%\asp.net\dataprotection-keys\ です。

## <a name="the-key-element"></a>\<キー > 要素

キーは、キーのリポジトリ内の最上位レベルのオブジェクトとして存在します。 規則によりキーは、ファイル名をある**キー - {guid} .xml**{guid} はキーの id です。 このような各ファイルには、1 つのキーが含まれています。 ファイルの形式は次のとおりです。

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

* キーの id。この値は、権限のある; に扱われますファイル名は、人間にとって読み言えませんだけです。

* バージョン、\<キー > 要素を 1 に現在固定されています。

* キーの作成、ライセンス認証、および有効期限の日付。

* A\<記述子 > 要素で、このキー内に含まれる、認証された暗号化の実装について説明します。

上記の例では、キーの id に {80732141-ec8f-4b80-af9c-c4d2d1ff8901} が作成され、2015 年 3 月 19 日で有効になっても、90 日間の有効期間があります。 (場合によっては、アクティブ化する日があります少しこの例のように、作成日の前にします。 これは、Api の動作し、は、実際には問題でのつまらぬのため) です。

## <a name="the-descriptor-element"></a>\<記述子 > 要素

外側\<記述子 > 要素には、属性 deserializerType、IAuthenticatedEncryptorDescriptorDeserializer を実装する型のアセンブリ修飾名が含まれています。 この型は、内部の読み取りを行う\<記述子 > 要素に含まれる情報を解析しています。

特定の形式、\<記述子 > 要素をキーでカプセル化された認証済みの暗号化機能の実装に依存し、各デシリアライザ-がの型は、このわずかに異なる形式が必要です。 この要素が、アルゴリズムの情報に含めるは一般に、(名前、種類、Oid、または類似した) とシークレット キー マテリアル。 上記の例では、記述子は、このキーが、AES-CBC 256 暗号化 + HMACSHA256 検証をラップするを指定します。

## <a name="the-encryptedsecret-element"></a>\<EncryptedSecret > 要素

**&lt;EncryptedSecret&gt;** 秘密キー マテリアルの暗号化されたフォームが含まれる要素が存在する可能性がある場合[rest でのシークレットの暗号化が有効になっている](xref:security/data-protection/implementation/key-encryption-at-rest)します。 属性`decryptorType`を実装する型のアセンブリ修飾名は、 [IXmlDecryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmldecryptor)します。 この型は、内部の読み取りを行う **&lt;encryptedKey&gt;** 要素と、元のプレーン テキストを回復する復号化します。

同様`<descriptor>`の特定の形式、`<encryptedSecret>`要素が使用中の保存時暗号化メカニズムに依存します。 上記の例では、Windows DPAPI を使用して、コメント、マスター _ キーが暗号化します。

## <a name="the-revocation-element"></a>\<失効 > 要素

失効は、キーのリポジトリ内の最上位レベルのオブジェクトとして存在します。 失効規則により、ファイル名がある**失効-{のタイムスタンプ}.xml** (特定の日付までのすべてのキーを取り消す) 用または**失効-{guid}.xml** (特定のキーを取り消す) 用です。 各ファイルには、1 つ\<失効> 要素。

ファイルの内容になります個々 のキーを取り消した場合は、次のようです。

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T22:45:30.2616742Z</revocationDate>
  <key id="eb4fc299-8808-409d-8a34-23fc83d026c9" />
  <reason>human-readable reason</reason>
</revocation>
```

この場合、指定したキーのみが失効します。 場合は、キー id"*"、ただし、ように、次の例では、作成日付が指定された失効日より前のすべてのキーが失効しました。

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T15:45:45.7366491-07:00</revocationDate>
  <!-- All keys created before the revocation date are revoked. -->
  <key id="*" />
  <reason>human-readable reason</reason>
</revocation>
```

\<理由 > 要素が、システムによって読み取られることはありません。 失効の人間が判読できる理由を格納する便利な場所だけになります。
