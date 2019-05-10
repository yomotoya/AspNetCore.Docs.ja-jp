---
title: その他の ASP.NET Core データ保護 Api
author: rick-anderson
description: ASP.NET Core データ保護 ISecret インターフェイスについて説明します。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 114cdd6209970e46b827e403fbe79b95692d0242
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2019
ms.locfileid: "64896619"
---
# <a name="miscellaneous-aspnet-core-data-protection-apis"></a>その他の ASP.NET Core データ保護 Api

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> 次のインターフェイスのいずれかを実装する型がスレッド セーフにする必要があります複数の呼び出し元の。

## <a name="isecret"></a>ISecret

`ISecret`インターフェイスは、暗号化キー マテリアルなどのシークレットの値を表します。 次の API サーフェスが含まれています。

* `Length`: `int`

* `Dispose()`: `void`

* `WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`

`WriteSecretIntoBuffer`メソッドは生のシークレットの値で指定されたバッファーを設定します。 この API は、パラメーターとして、バッファーの理由を返すのではなく、`byte[]`バッファー オブジェクトは、管理対象のガベージ コレクターへのシークレットの露出を制限することをピン留めする営業案件これにより、呼び出し元を直接は。

`Secret`型の具象実装は、`ISecret`プロセスでメモリ内にシークレットの値が格納されます。 Windows のプラットフォームで、シークレットの値が使用して暗号化されて[CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx)します。
