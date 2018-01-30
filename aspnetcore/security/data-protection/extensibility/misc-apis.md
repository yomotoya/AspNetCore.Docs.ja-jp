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
# <a name="miscellaneous-apis"></a><span data-ttu-id="8f779-103">その他の API</span><span class="sxs-lookup"><span data-stu-id="8f779-103">Miscellaneous APIs</span></span>

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> <span data-ttu-id="8f779-104">次のインターフェイスのいずれかを実装する型はスレッド セーフである必要があります複数の呼び出し元のです。</span><span class="sxs-lookup"><span data-stu-id="8f779-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="isecret"></a><span data-ttu-id="8f779-105">ISecret</span><span class="sxs-lookup"><span data-stu-id="8f779-105">ISecret</span></span>

<span data-ttu-id="8f779-106">`ISecret`インターフェイスは、暗号化キー マテリアルなどの秘密の値を表します。</span><span class="sxs-lookup"><span data-stu-id="8f779-106">The `ISecret` interface represents a secret value, such as cryptographic key material.</span></span> <span data-ttu-id="8f779-107">次の API サーフェスが含まれています。</span><span class="sxs-lookup"><span data-stu-id="8f779-107">It contains the following API surface:</span></span>

* <span data-ttu-id="8f779-108">`Length`: `int`</span><span class="sxs-lookup"><span data-stu-id="8f779-108">`Length`: `int`</span></span>

* <span data-ttu-id="8f779-109">`Dispose()`: `void`</span><span class="sxs-lookup"><span data-stu-id="8f779-109">`Dispose()`: `void`</span></span>

* <span data-ttu-id="8f779-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span><span class="sxs-lookup"><span data-stu-id="8f779-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span></span>

<span data-ttu-id="8f779-111">`WriteSecretIntoBuffer`メソッドは、生の秘密の値で指定されたバッファーを設定します。</span><span class="sxs-lookup"><span data-stu-id="8f779-111">The `WriteSecretIntoBuffer` method populates the supplied buffer with the raw secret value.</span></span> <span data-ttu-id="8f779-112">この API は、パラメーターとして、バッファーの理由が返されず、`byte[]`直接が、これにより、呼び出し元は、マネージ ガベージ コレクターのシークレットの露出を制限する、バッファー オブジェクトをピン留めすることです。</span><span class="sxs-lookup"><span data-stu-id="8f779-112">The reason this API takes the buffer as a parameter rather than returning a `byte[]` directly is that this gives the caller the opportunity to pin the buffer object, limiting secret exposure to the managed garbage collector.</span></span>

<span data-ttu-id="8f779-113">`Secret`型の具象実装は、`ISecret`のプロセスでメモリに秘密の値が格納されます。</span><span class="sxs-lookup"><span data-stu-id="8f779-113">The `Secret` type is a concrete implementation of `ISecret` where the secret value is stored in in-process memory.</span></span> <span data-ttu-id="8f779-114">使用して Windows プラットフォームでは、秘密の値が暗号化されて[CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx)です。</span><span class="sxs-lookup"><span data-stu-id="8f779-114">On Windows platforms, the secret value is encrypted via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span></span>
