---
title: "その他の API"
author: rick-anderson
description: "このドキュメントでは、ASP.NET Core データ保護 ISecret インターフェイスについて説明します。"
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 26474de3a1c681a8c7f8b7812e9ef859f5d448bb
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="miscellaneous-apis"></a>その他の API

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> 次のインターフェイスのいずれかを実装する型はスレッド セーフである必要があります複数の呼び出し元のです。

## <a name="isecret"></a>ISecret

`ISecret`インターフェイスは、暗号化キー マテリアルなどの秘密の値を表します。 次の API サーフェスが含まれています。

* `Length`: `int`

* `Dispose()`: `void`

* `WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`

`WriteSecretIntoBuffer`メソッドは、生の秘密の値で指定されたバッファーを設定します。 この API は、パラメーターとして、バッファーの理由が返されず、`byte[]`直接が、これにより、呼び出し元は、マネージ ガベージ コレクターのシークレットの露出を制限する、バッファー オブジェクトをピン留めすることです。

`Secret`型の具象実装は、`ISecret`のプロセスでメモリに秘密の値が格納されます。 使用して Windows プラットフォームでは、秘密の値が暗号化されて[CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx)です。
