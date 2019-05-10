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
# <a name="miscellaneous-aspnet-core-data-protection-apis"></a><span data-ttu-id="d40c7-103">その他の ASP.NET Core データ保護 Api</span><span class="sxs-lookup"><span data-stu-id="d40c7-103">Miscellaneous ASP.NET Core Data Protection APIs</span></span>

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> <span data-ttu-id="d40c7-104">次のインターフェイスのいずれかを実装する型がスレッド セーフにする必要があります複数の呼び出し元の。</span><span class="sxs-lookup"><span data-stu-id="d40c7-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="isecret"></a><span data-ttu-id="d40c7-105">ISecret</span><span class="sxs-lookup"><span data-stu-id="d40c7-105">ISecret</span></span>

<span data-ttu-id="d40c7-106">`ISecret`インターフェイスは、暗号化キー マテリアルなどのシークレットの値を表します。</span><span class="sxs-lookup"><span data-stu-id="d40c7-106">The `ISecret` interface represents a secret value, such as cryptographic key material.</span></span> <span data-ttu-id="d40c7-107">次の API サーフェスが含まれています。</span><span class="sxs-lookup"><span data-stu-id="d40c7-107">It contains the following API surface:</span></span>

* <span data-ttu-id="d40c7-108">`Length`: `int`</span><span class="sxs-lookup"><span data-stu-id="d40c7-108">`Length`: `int`</span></span>

* <span data-ttu-id="d40c7-109">`Dispose()`: `void`</span><span class="sxs-lookup"><span data-stu-id="d40c7-109">`Dispose()`: `void`</span></span>

* <span data-ttu-id="d40c7-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span><span class="sxs-lookup"><span data-stu-id="d40c7-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span></span>

<span data-ttu-id="d40c7-111">`WriteSecretIntoBuffer`メソッドは生のシークレットの値で指定されたバッファーを設定します。</span><span class="sxs-lookup"><span data-stu-id="d40c7-111">The `WriteSecretIntoBuffer` method populates the supplied buffer with the raw secret value.</span></span> <span data-ttu-id="d40c7-112">この API は、パラメーターとして、バッファーの理由を返すのではなく、`byte[]`バッファー オブジェクトは、管理対象のガベージ コレクターへのシークレットの露出を制限することをピン留めする営業案件これにより、呼び出し元を直接は。</span><span class="sxs-lookup"><span data-stu-id="d40c7-112">The reason this API takes the buffer as a parameter rather than returning a `byte[]` directly is that this gives the caller the opportunity to pin the buffer object, limiting secret exposure to the managed garbage collector.</span></span>

<span data-ttu-id="d40c7-113">`Secret`型の具象実装は、`ISecret`プロセスでメモリ内にシークレットの値が格納されます。</span><span class="sxs-lookup"><span data-stu-id="d40c7-113">The `Secret` type is a concrete implementation of `ISecret` where the secret value is stored in in-process memory.</span></span> <span data-ttu-id="d40c7-114">Windows のプラットフォームで、シークレットの値が使用して暗号化されて[CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx)します。</span><span class="sxs-lookup"><span data-stu-id="d40c7-114">On Windows platforms, the secret value is encrypted via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span></span>
