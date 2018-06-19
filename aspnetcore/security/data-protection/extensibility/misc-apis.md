---
title: その他の ASP.NET Core データ保護 Api
author: rick-anderson
description: ASP.NET Core データ保護 ISecret インターフェイスについて説明します。
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 484c6a0979a10e7cf2b801873655caa99a42532c
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/22/2018
ms.locfileid: "30072765"
---
# <a name="miscellaneous-aspnet-core-data-protection-apis"></a><span data-ttu-id="6628f-103">その他の ASP.NET Core データ保護 Api</span><span class="sxs-lookup"><span data-stu-id="6628f-103">Miscellaneous ASP.NET Core Data Protection APIs</span></span>

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> <span data-ttu-id="6628f-104">次のインターフェイスのいずれかを実装する型はスレッド セーフである必要があります複数の呼び出し元のです。</span><span class="sxs-lookup"><span data-stu-id="6628f-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="isecret"></a><span data-ttu-id="6628f-105">ISecret</span><span class="sxs-lookup"><span data-stu-id="6628f-105">ISecret</span></span>

<span data-ttu-id="6628f-106">`ISecret`インターフェイスは、暗号化キー マテリアルなどの秘密の値を表します。</span><span class="sxs-lookup"><span data-stu-id="6628f-106">The `ISecret` interface represents a secret value, such as cryptographic key material.</span></span> <span data-ttu-id="6628f-107">次の API サーフェスが含まれています。</span><span class="sxs-lookup"><span data-stu-id="6628f-107">It contains the following API surface:</span></span>

* <span data-ttu-id="6628f-108">`Length`: `int`</span><span class="sxs-lookup"><span data-stu-id="6628f-108">`Length`: `int`</span></span>

* <span data-ttu-id="6628f-109">`Dispose()`: `void`</span><span class="sxs-lookup"><span data-stu-id="6628f-109">`Dispose()`: `void`</span></span>

* <span data-ttu-id="6628f-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span><span class="sxs-lookup"><span data-stu-id="6628f-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span></span>

<span data-ttu-id="6628f-111">`WriteSecretIntoBuffer`メソッドは、生の秘密の値で指定されたバッファーを設定します。</span><span class="sxs-lookup"><span data-stu-id="6628f-111">The `WriteSecretIntoBuffer` method populates the supplied buffer with the raw secret value.</span></span> <span data-ttu-id="6628f-112">この API は、パラメーターとして、バッファーの理由が返されず、`byte[]`直接が、これにより、呼び出し元は、マネージ ガベージ コレクターのシークレットの露出を制限する、バッファー オブジェクトをピン留めすることです。</span><span class="sxs-lookup"><span data-stu-id="6628f-112">The reason this API takes the buffer as a parameter rather than returning a `byte[]` directly is that this gives the caller the opportunity to pin the buffer object, limiting secret exposure to the managed garbage collector.</span></span>

<span data-ttu-id="6628f-113">`Secret`型の具象実装は、`ISecret`のプロセスでメモリに秘密の値が格納されます。</span><span class="sxs-lookup"><span data-stu-id="6628f-113">The `Secret` type is a concrete implementation of `ISecret` where the secret value is stored in in-process memory.</span></span> <span data-ttu-id="6628f-114">使用して Windows プラットフォームでは、秘密の値が暗号化されて[CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx)です。</span><span class="sxs-lookup"><span data-stu-id="6628f-114">On Windows platforms, the secret value is encrypted via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span></span>
