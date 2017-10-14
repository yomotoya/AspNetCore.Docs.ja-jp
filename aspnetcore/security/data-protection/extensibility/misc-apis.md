---
title: "その他の API"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 512c6ba7-88ec-47e4-a656-6b30350b34e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 72274a0da94a14bcd5af11d245ba626214fa2607
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/13/2017
---
# <a name="miscellaneous-apis"></a><span data-ttu-id="562ff-103">その他の API</span><span class="sxs-lookup"><span data-stu-id="562ff-103">Miscellaneous APIs</span></span>

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> <span data-ttu-id="562ff-104">次のインターフェイスのいずれかを実装する型はスレッド セーフである必要があります複数の呼び出し元のです。</span><span class="sxs-lookup"><span data-stu-id="562ff-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="isecret"></a><span data-ttu-id="562ff-105">ISecret</span><span class="sxs-lookup"><span data-stu-id="562ff-105">ISecret</span></span>

<span data-ttu-id="562ff-106">ISecret インターフェイスでは、暗号化キー マテリアルなどの秘密の値を表します。</span><span class="sxs-lookup"><span data-stu-id="562ff-106">The ISecret interface represents a secret value, such as cryptographic key material.</span></span> <span data-ttu-id="562ff-107">次の API サーフェスが含まれています。</span><span class="sxs-lookup"><span data-stu-id="562ff-107">It contains the following API surface.</span></span>

* <span data-ttu-id="562ff-108">長さ: int</span><span class="sxs-lookup"><span data-stu-id="562ff-108">Length : int</span></span>

* <span data-ttu-id="562ff-109">Dispose(): void</span><span class="sxs-lookup"><span data-stu-id="562ff-109">Dispose() : void</span></span>

* <span data-ttu-id="562ff-110">WriteSecretIntoBuffer (ArraySegment<byte>バッファー): void</span><span class="sxs-lookup"><span data-stu-id="562ff-110">WriteSecretIntoBuffer(ArraySegment<byte> buffer) : void</span></span>

<span data-ttu-id="562ff-111">WriteSecretIntoBuffer メソッドは、生の秘密の値で指定されたバッファーを設定します。</span><span class="sxs-lookup"><span data-stu-id="562ff-111">The WriteSecretIntoBuffer method populates the supplied buffer with the raw secret value.</span></span> <span data-ttu-id="562ff-112">この API は、管理対象のガベージ コレクターにシークレットの露出を制限することのこれにより、呼び出し元は、バッファー オブジェクトをピン留めする機会は直接、byte[] を返すのではなく、パラメーターとして、バッファーを受け取る理由です。</span><span class="sxs-lookup"><span data-stu-id="562ff-112">The reason this API takes the buffer as a parameter rather than returning a byte[] directly is that this gives the caller the opportunity to pin the buffer object, limiting secret exposure to the managed garbage collector.</span></span>

<span data-ttu-id="562ff-113">シークレット型は、プロセス内のメモリで秘密の値が格納 ISecret の具象実装です。</span><span class="sxs-lookup"><span data-stu-id="562ff-113">The Secret type is a concrete implementation of ISecret where the secret value is stored in in-process memory.</span></span> <span data-ttu-id="562ff-114">使用して Windows プラットフォームでは、秘密の値が暗号化されて[CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx)です。</span><span class="sxs-lookup"><span data-stu-id="562ff-114">On Windows platforms, the secret value is encrypted via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span></span>
