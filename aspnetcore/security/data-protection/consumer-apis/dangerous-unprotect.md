---
title: ASP.NET Core でキーを持つが取り消されたペイロードの保護を解除します。
author: rick-anderson
description: ASP.NET Core アプリで、取り消されたためキーで保護されているデータの保護を解除する方法について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: security/data-protection/consumer-apis/dangerous-unprotect
ms.openlocfilehash: 26061d048dcd9c1e3d8909e9388d8b565376fa2f
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897109"
---
# <a name="unprotect-payloads-whose-keys-have-been-revoked-in-aspnet-core"></a><span data-ttu-id="5f60b-103">ASP.NET Core でキーを持つが取り消されたペイロードの保護を解除します。</span><span class="sxs-lookup"><span data-stu-id="5f60b-103">Unprotect payloads whose keys have been revoked in ASP.NET Core</span></span>

<a name="data-protection-consumer-apis-dangerous-unprotect"></a>

<span data-ttu-id="5f60b-104">ASP.NET Core データ保護 Api が機密のペイロードの無期限の永続化の主に意図されていません。</span><span class="sxs-lookup"><span data-stu-id="5f60b-104">The ASP.NET Core data protection APIs are not primarily intended for indefinite persistence of confidential payloads.</span></span> <span data-ttu-id="5f60b-105">などの他のテクノロジ[Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx)と[Azure Rights Management](/rights-management/)無制限のストレージのシナリオに適したこれに応じて拡大縮小の強力なキー管理機能があるとします。</span><span class="sxs-lookup"><span data-stu-id="5f60b-105">Other technologies like [Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) and [Azure Rights Management](/rights-management/) are more suited to the scenario of indefinite storage, and they have correspondingly strong key management capabilities.</span></span> <span data-ttu-id="5f60b-106">ただし、機密データの長期的な保護の ASP.NET Core データ保護 Api を使用してから、開発者を禁止すること何もないです。</span><span class="sxs-lookup"><span data-stu-id="5f60b-106">That said, there's nothing prohibiting a developer from using the ASP.NET Core data protection APIs for long-term protection of confidential data.</span></span> <span data-ttu-id="5f60b-107">キーがので、キー リングから削除しない`IDataProtector.Unprotect`キーが使用可能で有効な限り常に、既存のペイロードを回復できます。</span><span class="sxs-lookup"><span data-stu-id="5f60b-107">Keys are never removed from the key ring, so `IDataProtector.Unprotect` can always recover existing payloads as long as the keys are available and valid.</span></span>

<span data-ttu-id="5f60b-108">開発者が、失効したキーを持つとして保護されているデータの保護を解除しようとしています。 ただし、問題が発生した`IDataProtector.Unprotect`ここで例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="5f60b-108">However, an issue arises when the developer tries to unprotect data that has been protected with a revoked key, as `IDataProtector.Unprotect` will throw an exception in this case.</span></span> <span data-ttu-id="5f60b-109">システムでこれらの種類のペイロードを簡単に再作成するし、サイト訪問者をもう一度ログインする必要な最悪の場合の (認証トークン) のようなペイロードを短期間または一時的な問題があります。</span><span class="sxs-lookup"><span data-stu-id="5f60b-109">This might be fine for short-lived or transient payloads (like authentication tokens), as these kinds of payloads can easily be recreated by the system, and at worst the site visitor might be required to log in again.</span></span> <span data-ttu-id="5f60b-110">持つ、永続化されたペイロードの`Unprotect`throw が許容できないデータ損失につながる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="5f60b-110">But for persisted payloads, having `Unprotect` throw could lead to unacceptable data loss.</span></span>

## <a name="ipersisteddataprotector"></a><span data-ttu-id="5f60b-111">IPersistedDataProtector</span><span class="sxs-lookup"><span data-stu-id="5f60b-111">IPersistedDataProtector</span></span>

<span data-ttu-id="5f60b-112">失効したキーが発生しても保護するペイロードを許可する際のシナリオをサポートするデータ保護システムに含まれる、`IPersistedDataProtector`型。</span><span class="sxs-lookup"><span data-stu-id="5f60b-112">To support the scenario of allowing payloads to be unprotected even in the face of revoked keys, the data protection system contains an `IPersistedDataProtector` type.</span></span> <span data-ttu-id="5f60b-113">インスタンスを取得する`IPersistedDataProtector`のインスタンスが表示されるだけ`IDataProtector`通常の方法でお試しくださいキャスト、`IDataProtector`に`IPersistedDataProtector`します。</span><span class="sxs-lookup"><span data-stu-id="5f60b-113">To get an instance of `IPersistedDataProtector`, simply get an instance of `IDataProtector` in the normal fashion and try casting the `IDataProtector` to `IPersistedDataProtector`.</span></span>

> [!NOTE]
> <span data-ttu-id="5f60b-114">すべて`IDataProtector`インスタンスにキャストできる`IPersistedDataProtector`します。</span><span class="sxs-lookup"><span data-stu-id="5f60b-114">Not all `IDataProtector` instances can be cast to `IPersistedDataProtector`.</span></span> <span data-ttu-id="5f60b-115">開発者が使用する必要があります、C#演算子、または似ていますが無効なキャストで引き起こされたランタイムの例外を回避し、障害発生時に適切に処理する準備が必要があります。</span><span class="sxs-lookup"><span data-stu-id="5f60b-115">Developers should use the C# as operator or similar to avoid runtime exceptions caused by invalid casts, and they should be prepared to handle the failure case appropriately.</span></span>

<span data-ttu-id="5f60b-116">`IPersistedDataProtector` 次の API サーフェスを公開します。</span><span class="sxs-lookup"><span data-stu-id="5f60b-116">`IPersistedDataProtector` exposes the following API surface:</span></span>

```csharp
DangerousUnprotect(byte[] protectedData, bool ignoreRevocationErrors,
     out bool requiresMigration, out bool wasRevoked) : byte[]
```

<span data-ttu-id="5f60b-117">この API は、(バイト配列) として保護されているペイロードを受け取り、保護されていないペイロードを返します。</span><span class="sxs-lookup"><span data-stu-id="5f60b-117">This API takes the protected payload (as a byte array) and returns the unprotected payload.</span></span> <span data-ttu-id="5f60b-118">文字列ベースのオーバー ロードがありません。</span><span class="sxs-lookup"><span data-stu-id="5f60b-118">There's no string-based overload.</span></span> <span data-ttu-id="5f60b-119">Out パラメーター 2 は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="5f60b-119">The two out parameters are as follows.</span></span>

* <span data-ttu-id="5f60b-120">`requiresMigration`: 実行される場所をこのペイロードを保護するために使用するキーは、アクティブな既定のキーが不要になった、など、このペイロードを保護するために使用するキーが古いと、以降の操作をローリング キーがある場合は true に設定されます。</span><span class="sxs-lookup"><span data-stu-id="5f60b-120">`requiresMigration`: will be set to true if the key used to protect this payload is no longer the active default key, e.g., the key used to protect this payload is old and a key rolling operation has since taken place.</span></span> <span data-ttu-id="5f60b-121">呼び出し元は、ビジネス ニーズに応じて、ペイロードの再保護を検討することです。</span><span class="sxs-lookup"><span data-stu-id="5f60b-121">The caller may wish to consider reprotecting the payload depending on their business needs.</span></span>

* <span data-ttu-id="5f60b-122">`wasRevoked`: このペイロードを保護するために使用するキーが取り消された場合は true に設定されます。</span><span class="sxs-lookup"><span data-stu-id="5f60b-122">`wasRevoked`: will be set to true if the key used to protect this payload was revoked.</span></span>

>[!WARNING]
> <span data-ttu-id="5f60b-123">渡すときに十分注意`ignoreRevocationErrors: true`を`DangerousUnprotect`メソッド。</span><span class="sxs-lookup"><span data-stu-id="5f60b-123">Exercise extreme caution when passing `ignoreRevocationErrors: true` to the `DangerousUnprotect` method.</span></span> <span data-ttu-id="5f60b-124">このメソッドを呼び出した後の場合、`wasRevoked`値が true の場合、このペイロードを保護するために使用するキーが取り消されたし、ペイロードの信頼性が問題ありとして扱われます。</span><span class="sxs-lookup"><span data-stu-id="5f60b-124">If after calling this method the `wasRevoked` value is true, then the key used to protect this payload was revoked, and the payload's authenticity should be treated as suspect.</span></span> <span data-ttu-id="5f60b-125">この場合は、のみ操作が続行、保護されていないペイロードであるを導き出すことなどいくつか別の保証がある場合そのが、信頼されていない web クライアントによって送信されるのではなく、セキュリティで保護されたデータベースから入手します。</span><span class="sxs-lookup"><span data-stu-id="5f60b-125">In this case, only continue operating on the unprotected payload if you have some separate assurance that it's authentic, e.g. that it's coming from a secure database rather than being sent by an untrusted web client.</span></span>

[!code-csharp[](dangerous-unprotect/samples/dangerous-unprotect.cs)]
