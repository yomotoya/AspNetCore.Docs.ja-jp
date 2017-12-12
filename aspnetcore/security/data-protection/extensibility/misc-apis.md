---
title: "その他の API"
author: rick-anderson
description: "このドキュメントでは、ASP.NET Core データ保護 ISecret インターフェイスについて説明します。"
keywords: "ASP.NET Core、データ保護、ISecret"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 512c6ba7-88ec-47e4-a656-6b30350b34e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: f5d6920f9f229bd480a76c952dab30efb7d9eff5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="miscellaneous-apis"></a><span data-ttu-id="56b7c-104">その他の API</span><span class="sxs-lookup"><span data-stu-id="56b7c-104">Miscellaneous APIs</span></span>

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> <span data-ttu-id="56b7c-105">次のインターフェイスのいずれかを実装する型はスレッド セーフである必要があります複数の呼び出し元のです。</span><span class="sxs-lookup"><span data-stu-id="56b7c-105">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="isecret"></a><span data-ttu-id="56b7c-106">ISecret</span><span class="sxs-lookup"><span data-stu-id="56b7c-106">ISecret</span></span>

<span data-ttu-id="56b7c-107">`ISecret`インターフェイスは、暗号化キー マテリアルなどの秘密の値を表します。</span><span class="sxs-lookup"><span data-stu-id="56b7c-107">The `ISecret` interface represents a secret value, such as cryptographic key material.</span></span> <span data-ttu-id="56b7c-108">次の API サーフェスが含まれています。</span><span class="sxs-lookup"><span data-stu-id="56b7c-108">It contains the following API surface:</span></span>

* <span data-ttu-id="56b7c-109">`Length`: `int`</span><span class="sxs-lookup"><span data-stu-id="56b7c-109">`Length`: `int`</span></span>

* <span data-ttu-id="56b7c-110">`Dispose()`: `void`</span><span class="sxs-lookup"><span data-stu-id="56b7c-110">`Dispose()`: `void`</span></span>

* <span data-ttu-id="56b7c-111">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span><span class="sxs-lookup"><span data-stu-id="56b7c-111">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span></span>

<span data-ttu-id="56b7c-112">`WriteSecretIntoBuffer`メソッドは、生の秘密の値で指定されたバッファーを設定します。</span><span class="sxs-lookup"><span data-stu-id="56b7c-112">The `WriteSecretIntoBuffer` method populates the supplied buffer with the raw secret value.</span></span> <span data-ttu-id="56b7c-113">この API は、パラメーターとして、バッファーの理由が返されず、`byte[]`直接が、これにより、呼び出し元は、マネージ ガベージ コレクターのシークレットの露出を制限する、バッファー オブジェクトをピン留めすることです。</span><span class="sxs-lookup"><span data-stu-id="56b7c-113">The reason this API takes the buffer as a parameter rather than returning a `byte[]` directly is that this gives the caller the opportunity to pin the buffer object, limiting secret exposure to the managed garbage collector.</span></span>

<span data-ttu-id="56b7c-114">`Secret`型の具象実装は、`ISecret`のプロセスでメモリに秘密の値が格納されます。</span><span class="sxs-lookup"><span data-stu-id="56b7c-114">The `Secret` type is a concrete implementation of `ISecret` where the secret value is stored in in-process memory.</span></span> <span data-ttu-id="56b7c-115">使用して Windows プラットフォームでは、秘密の値が暗号化されて[CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx)です。</span><span class="sxs-lookup"><span data-stu-id="56b7c-115">On Windows platforms, the secret value is encrypted via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span></span>
