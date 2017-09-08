---
title: "その他の API"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 512c6ba7-88ec-47e4-a656-6b30350b34e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 541dd721a00495632f0d633577b55933c9be03fa
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/11/2017
---
# <a name="miscellaneous-apis"></a>その他の API

<a name=data-protection-extensibility-mics-apis></a>

>[!WARNING]
> 次のインターフェイスのいずれかを実装する型はスレッド セーフである必要があります複数の呼び出し元のです。

## <a name="isecret"></a>ISecret

ISecret インターフェイスでは、暗号化キー マテリアルなどの秘密の値を表します。 次の API サーフェスが含まれています。

* 長さ: int

* Dispose(): void

* WriteSecretIntoBuffer (ArraySegment<byte>バッファー): void

WriteSecretIntoBuffer メソッドは、生の秘密の値で指定されたバッファーを設定します。 この API は、管理対象のガベージ コレクターにシークレットの露出を制限することのこれにより、呼び出し元は、バッファー オブジェクトをピン留めする機会は直接、byte[] を返すのではなく、パラメーターとして、バッファーを受け取る理由です。

シークレット型は、プロセス内のメモリで秘密の値が格納 ISecret の具象実装です。 使用して Windows プラットフォームでは、秘密の値が暗号化されて[CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx)です。
