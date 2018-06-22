---
title: その他の ASP.NET Core データ保護 Api
author: rick-anderson
description: ASP.NET Core データ保護 ISecret インターフェイスについて説明します。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 114cdd6209970e46b827e403fbe79b95692d0242
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279156"
---
# <a name="miscellaneous-aspnet-core-data-protection-apis"></a>その他の ASP.NET Core データ保護 Api

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
