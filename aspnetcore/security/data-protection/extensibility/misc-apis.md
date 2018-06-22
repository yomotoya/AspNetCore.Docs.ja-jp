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
# <a name="miscellaneous-aspnet-core-data-protection-apis"></a><span data-ttu-id="f270e-103">その他の ASP.NET Core データ保護 Api</span><span class="sxs-lookup"><span data-stu-id="f270e-103">Miscellaneous ASP.NET Core Data Protection APIs</span></span>

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> <span data-ttu-id="f270e-104">次のインターフェイスのいずれかを実装する型はスレッド セーフである必要があります複数の呼び出し元のです。</span><span class="sxs-lookup"><span data-stu-id="f270e-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="isecret"></a><span data-ttu-id="f270e-105">ISecret</span><span class="sxs-lookup"><span data-stu-id="f270e-105">ISecret</span></span>

<span data-ttu-id="f270e-106">`ISecret`インターフェイスは、暗号化キー マテリアルなどの秘密の値を表します。</span><span class="sxs-lookup"><span data-stu-id="f270e-106">The `ISecret` interface represents a secret value, such as cryptographic key material.</span></span> <span data-ttu-id="f270e-107">次の API サーフェスが含まれています。</span><span class="sxs-lookup"><span data-stu-id="f270e-107">It contains the following API surface:</span></span>

* <span data-ttu-id="f270e-108">`Length`: `int`</span><span class="sxs-lookup"><span data-stu-id="f270e-108">`Length`: `int`</span></span>

* <span data-ttu-id="f270e-109">`Dispose()`: `void`</span><span class="sxs-lookup"><span data-stu-id="f270e-109">`Dispose()`: `void`</span></span>

* <span data-ttu-id="f270e-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span><span class="sxs-lookup"><span data-stu-id="f270e-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span></span>

<span data-ttu-id="f270e-111">`WriteSecretIntoBuffer`メソッドは、生の秘密の値で指定されたバッファーを設定します。</span><span class="sxs-lookup"><span data-stu-id="f270e-111">The `WriteSecretIntoBuffer` method populates the supplied buffer with the raw secret value.</span></span> <span data-ttu-id="f270e-112">この API は、パラメーターとして、バッファーの理由が返されず、`byte[]`直接が、これにより、呼び出し元は、マネージ ガベージ コレクターのシークレットの露出を制限する、バッファー オブジェクトをピン留めすることです。</span><span class="sxs-lookup"><span data-stu-id="f270e-112">The reason this API takes the buffer as a parameter rather than returning a `byte[]` directly is that this gives the caller the opportunity to pin the buffer object, limiting secret exposure to the managed garbage collector.</span></span>

<span data-ttu-id="f270e-113">`Secret`型の具象実装は、`ISecret`のプロセスでメモリに秘密の値が格納されます。</span><span class="sxs-lookup"><span data-stu-id="f270e-113">The `Secret` type is a concrete implementation of `ISecret` where the secret value is stored in in-process memory.</span></span> <span data-ttu-id="f270e-114">使用して Windows プラットフォームでは、秘密の値が暗号化されて[CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx)です。</span><span class="sxs-lookup"><span data-stu-id="f270e-114">On Windows platforms, the secret value is encrypted via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span></span>
