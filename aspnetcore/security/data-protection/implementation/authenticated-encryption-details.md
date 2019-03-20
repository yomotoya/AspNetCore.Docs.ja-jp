---
title: ASP.NET Core での認証された暗号化の詳細
author: rick-anderson
description: ASP.NET Core データ保護が認証された暗号化の実装の詳細について説明します。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/authenticated-encryption-details
ms.openlocfilehash: 9def03e6b27e19fc34a839e923d6152e086889db
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/19/2019
ms.locfileid: "58208581"
---
# <a name="authenticated-encryption-details-in-aspnet-core"></a>ASP.NET Core での認証された暗号化の詳細

<a name="data-protection-implementation-authenticated-encryption-details"></a>

IDataProtector.Protect への呼び出しは、認証された暗号化操作です。 機密性と完全性、保護メソッドが提供および IDataProtectionProvider ルートからのこの特定の IDataProtector インスタンスの派生に使用された目的のチェーンに関連付けられていること。

IDataProtector.Protect は、byte[] プレーン テキスト パラメーターを受け取り、その形式は以下の説明をバイト ・ [保護されたペイロードを生成します。 (も、拡張機能メソッド オーバー ロードがプレーン テキストの文字列パラメーターを取得し、文字列の保護されたペイロードを返します。 この API を使用する場合、保護されたペイロード形式が残っている、構造体には、以下がそのは[base64url でエンコードされた](https://tools.ietf.org/html/rfc4648#section-5))。

## <a name="protected-payload-format"></a>保護されたペイロードの形式

保護されたペイロードの形式は、次の 3 つの主要なコンポーネントで構成されます。

* データ保護システムのバージョンを識別する 32 ビット マジック ヘッダー。

* この特定のペイロードを保護するために使用するキーを識別する 128 ビット キー id。

* 保護されたペイロードの残りの部分は[このキーによってカプセル化された暗号化機能に固有](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation)します。 次の例では、キーが表して、AES-CBC 256 + HMACSHA256 暗号化機能とペイロードは次のようにさらに分割します。
  * 128 ビット キー修飾子です。
  * 128 ビットの初期化ベクトルです。
  * AES-CBC 256 出力の 48 バイト数。
  * HMACSHA256 認証タグ。

保護されたペイロードのサンプルを次に示します。

```
09 F0 C9 F0 80 9C 81 0C 19 66 19 40 95 36 53 F8
AA FF EE 57 57 2F 40 4C 3F 7F CC 9D CC D9 32 3E
84 17 99 16 EC BA 1F 4A A1 18 45 1F 2D 13 7A 28
79 6B 86 9C F8 B7 84 F9 26 31 FC B1 86 0A F1 56
61 CF 14 58 D3 51 6F CF 36 50 85 82 08 2D 3F 73
5F B0 AD 9E 1A B2 AE 13 57 90 C8 F5 7C 95 4E 6A
8A AA 06 EF 43 CA 19 62 84 7C 11 B2 C8 71 9D AA
52 19 2E 5B 4C 1E 54 F0 55 BE 88 92 12 C1 4B 5E
52 C9 74 A0
```

最初の 32 ビット、または 4 バイトのペイロード形式からは、マジックのヘッダー バージョン (09 F0 C9 F0) を識別します。

次の 128 ビットまたは 16 バイトは、キー識別子 (80 9 の C 81 0 C 19 66 19 40 95 36 53 F8 AA FF EE 57)

残りの部分では、ペイロードを含むあり、使用する形式に固有です。

> [!WARNING]
> 指定されたキーに対して保護されているすべてのペイロードは、同じ 20 バイト (魔法の値、キー id) ヘッダーで開始されます。 管理者は、およそのペイロードが生成されたときに、診断のために、このファクトを使用できます。 たとえば、上記のペイロードは、{0c819c80-6619-4019-9536-53f8aaffee57} をキーに対応します。 キーのリポジトリを確認した後は、この特定のキーのアクティブ化日付が 2015-01-01 をし、その有効期限の日付が 2015-03-01、あると想定するが妥当を見つけられない場合、ペイロード (で改ざんされていない) 場合により、そのウィンドウ内で生成されたまたは小さないずれかの側の一種です。
