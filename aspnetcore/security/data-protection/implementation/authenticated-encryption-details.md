---
title: "暗号化の詳細を認証します。"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 826e6d5d-9620-44e6-ad93-3b1d9969b70b
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/authenticated-encryption-details
ms.openlocfilehash: 4d0e63d7722071ab8806a217e96ee7ad7bf10286
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/28/2017
---
# <a name="authenticated-encryption-details"></a>暗号化の詳細を認証します。

<a name=data-protection-implementation-authenticated-encryption-details></a>

IDataProtector.Protect への呼び出しは、認証済み暗号化操作です。 保護する方法は、機密性と完全性の両方を提供し、IDataProtectionProvider、ルートから派生してこの特定の IDataProtector インスタンスに使用された目的のチェーンに関連付けられています。

IDataProtector.Protect は byte[] プレーン テキスト パラメーターを受け取りし、バイトに保護されているペイロード、その形式は以下の説明を生成します。 (も文字列のプレーン テキスト パラメーターを受け取りを文字列の保護されているペイロードを返す拡張メソッド オーバー ロードします。 この API を使用する場合、保護されているペイロード形式が残っている、構造体、以下がなります[base64url でエンコードされた](https://tools.ietf.org/html/rfc4648#section-5))。

## <a name="protected-payload-format"></a>保護されたペイロードの形式

保護されたペイロードの形式は、次の 3 つの主要なコンポーネントで構成されます。

* データ保護システムのバージョンを識別する 32 ビット マジック ヘッダー。

* この特定のペイロードを保護するために使用するキーを識別する 128 ビット キー id です。

* 保護されたペイロードの残りの部分は[このキーによってカプセル化された暗号化機能に固有](subkeyderivation.md#data-protection-implementation-subkey-derivation)です。 次の例で、キー、AES 256-CBC + HMACSHA256 暗号化機能を表し、ペイロードは次のようにさらに分割します。 * A 128 ビット キー修飾子です。 * は、128 ビットの初期化ベクトルです。 * AES 256-CBC 出力の 48 バイトです。 * HMACSHA256 認証タグ。

サンプルの保護されているペイロードを次に示します。

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

最初の 32 ビット、または 4 バイトの上、ペイロード形式マジック ヘッダー バージョン (09 F0 C9 F0) を識別するには

[次へ] の 128 ビットまたは 16 バイトは、キー識別子 (80 9 C 81 0 C 19 66 19 40 95 36 53 F8 AA FF EE 57)

残りの部分では、ペイロードを含むあり、使用する形式に固有です。

>[!WARNING]
> 指定されたキーに対して保護されているすべてのペイロードは、同じ 20 バイト (マジック値、キー id) のヘッダーで開始されます。 管理者は、およそのペイロードが生成されたときに、診断目的のためには、このファクトを使用できます。 たとえば、上記のペイロードは、{0c819c80-6619-4019-9536-53f8aaffee57} をキーに対応します。 キーのリポジトリを確認した後は、この特定のキーのアクティブ化された日付が 2015-01-01 と有効期限の日付が 2015-03-01、そのことがあると想定する不適切なを見つけられない場合、ペイロード (改ざんされない) 場合により、そのウィンドウ内で生成されたか、小さないずれかの側の一種です。
