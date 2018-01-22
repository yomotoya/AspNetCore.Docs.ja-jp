---
title: "その他の API"
author: rick-anderson
description: "このドキュメントでは、ASP.NET Core データ保護 ISecret インターフェイスについて説明します。"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 88a08a25abf4c4e1ba0746087b05b1cc8fa13024
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="miscellaneous-apis"></a><span data-ttu-id="ea943-103">その他の API</span><span class="sxs-lookup"><span data-stu-id="ea943-103">Miscellaneous APIs</span></span>

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> <span data-ttu-id="ea943-104">次のインターフェイスのいずれかを実装する型はスレッド セーフである必要があります複数の呼び出し元のです。</span><span class="sxs-lookup"><span data-stu-id="ea943-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="isecret"></a><span data-ttu-id="ea943-105">ISecret</span><span class="sxs-lookup"><span data-stu-id="ea943-105">ISecret</span></span>

<span data-ttu-id="ea943-106">`ISecret`インターフェイスは、暗号化キー マテリアルなどの秘密の値を表します。</span><span class="sxs-lookup"><span data-stu-id="ea943-106">The `ISecret` interface represents a secret value, such as cryptographic key material.</span></span> <span data-ttu-id="ea943-107">次の API サーフェスが含まれています。</span><span class="sxs-lookup"><span data-stu-id="ea943-107">It contains the following API surface:</span></span>

* <span data-ttu-id="ea943-108">`Length`: `int`</span><span class="sxs-lookup"><span data-stu-id="ea943-108">`Length`: `int`</span></span>

* <span data-ttu-id="ea943-109">`Dispose()`: `void`</span><span class="sxs-lookup"><span data-stu-id="ea943-109">`Dispose()`: `void`</span></span>

* <span data-ttu-id="ea943-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span><span class="sxs-lookup"><span data-stu-id="ea943-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span></span>

<span data-ttu-id="ea943-111">`WriteSecretIntoBuffer`メソッドは、生の秘密の値で指定されたバッファーを設定します。</span><span class="sxs-lookup"><span data-stu-id="ea943-111">The `WriteSecretIntoBuffer` method populates the supplied buffer with the raw secret value.</span></span> <span data-ttu-id="ea943-112">この API は、パラメーターとして、バッファーの理由が返されず、`byte[]`直接が、これにより、呼び出し元は、マネージ ガベージ コレクターのシークレットの露出を制限する、バッファー オブジェクトをピン留めすることです。</span><span class="sxs-lookup"><span data-stu-id="ea943-112">The reason this API takes the buffer as a parameter rather than returning a `byte[]` directly is that this gives the caller the opportunity to pin the buffer object, limiting secret exposure to the managed garbage collector.</span></span>

<span data-ttu-id="ea943-113">`Secret`型の具象実装は、`ISecret`のプロセスでメモリに秘密の値が格納されます。</span><span class="sxs-lookup"><span data-stu-id="ea943-113">The `Secret` type is a concrete implementation of `ISecret` where the secret value is stored in in-process memory.</span></span> <span data-ttu-id="ea943-114">使用して Windows プラットフォームでは、秘密の値が暗号化されて[CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx)です。</span><span class="sxs-lookup"><span data-stu-id="ea943-114">On Windows platforms, the secret value is encrypted via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span></span>
