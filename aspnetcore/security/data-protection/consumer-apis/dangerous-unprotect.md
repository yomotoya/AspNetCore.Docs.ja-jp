---
title: "キーを持つが失効している保護を解除するペイロード"
author: rick-anderson
description: "このドキュメントでは、ASP.NET Core アプリケーションで、失効させたのでキーで保護されたデータの保護を解除する方法について説明します。"
keywords: "ASP.NET Core、データ保護がキー、IPersistedDataProtector を取り消す"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 6c4e6591-45d2-4d25-855e-062ad352d648
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/dangerous-unprotect
ms.openlocfilehash: 5d431f0bbe7152525c9a360a6e90bccbd26be93d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="unprotecting-payloads-whose-keys-have-been-revoked"></a><span data-ttu-id="d0f20-104">キーを持つが失効している保護を解除するペイロード</span><span class="sxs-lookup"><span data-stu-id="d0f20-104">Unprotecting payloads whose keys have been revoked</span></span>

<a name="data-protection-consumer-apis-dangerous-unprotect"></a>

<span data-ttu-id="d0f20-105">主に、ASP.NET Core データ保護 Api は社外秘のペイロードの無期限の持続性の目的ではありません。</span><span class="sxs-lookup"><span data-stu-id="d0f20-105">The ASP.NET Core data protection APIs are not primarily intended for indefinite persistence of confidential payloads.</span></span> <span data-ttu-id="d0f20-106">他のテクノロジと同様に[Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx)と[Azure Rights Management](https://docs.microsoft.com/rights-management/)より永続的ストレージのシナリオに適している、およびこれに対応して強力なキー管理機能があります。</span><span class="sxs-lookup"><span data-stu-id="d0f20-106">Other technologies like [Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) and [Azure Rights Management](https://docs.microsoft.com/rights-management/) are more suited to the scenario of indefinite storage, and they have correspondingly strong key management capabilities.</span></span> <span data-ttu-id="d0f20-107">ただし、機密データの長期的な保護の ASP.NET Core データ保護 Api を使用して、開発者を禁止することは nothing を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d0f20-107">That said, there is nothing prohibiting a developer from using the ASP.NET Core data protection APIs for long-term protection of confidential data.</span></span> <span data-ttu-id="d0f20-108">キーからは削除されませんキー リングのため`IDataProtector.Unprotect`キーが使用可能な有効な限り、常に、既存のペイロードを回復できます。</span><span class="sxs-lookup"><span data-stu-id="d0f20-108">Keys are never removed from the key ring, so `IDataProtector.Unprotect` can always recover existing payloads as long as the keys are available and valid.</span></span>

<span data-ttu-id="d0f20-109">開発者として、失効したキーで保護されているデータの保護を解除しようとしました。 ただし、問題が発生した`IDataProtector.Unprotect`ここで例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="d0f20-109">However, an issue arises when the developer tries to unprotect data that has been protected with a revoked key, as `IDataProtector.Unprotect` will throw an exception in this case.</span></span> <span data-ttu-id="d0f20-110">で、システムによってこれらの種類のペイロードを簡単に再作成するし、最悪の場合、サイトの利用者しなければならない場合あります再度ログインするのにはのような認証トークンの場合)、短期間または一時的なペイロードには適してにあります。</span><span class="sxs-lookup"><span data-stu-id="d0f20-110">This might be fine for short-lived or transient payloads (like authentication tokens), as these kinds of payloads can easily be recreated by the system, and at worst the site visitor might be required to log in again.</span></span> <span data-ttu-id="d0f20-111">永続化されたペイロードを持つ、 `Unprotect` throw が許容できないデータ損失につながる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="d0f20-111">But for persisted payloads, having `Unprotect` throw could lead to unacceptable data loss.</span></span>

## <a name="ipersisteddataprotector"></a><span data-ttu-id="d0f20-112">IPersistedDataProtector</span><span class="sxs-lookup"><span data-stu-id="d0f20-112">IPersistedDataProtector</span></span>

<span data-ttu-id="d0f20-113">データ保護システムに含まれるキーが失効した場合でも保護を解除されるペイロードを許可するというシナリオをサポートするために、`IPersistedDataProtector`型です。</span><span class="sxs-lookup"><span data-stu-id="d0f20-113">To support the scenario of allowing payloads to be unprotected even in the face of revoked keys, the data protection system contains an `IPersistedDataProtector` type.</span></span> <span data-ttu-id="d0f20-114">インスタンスを取得する`IPersistedDataProtector`のインスタンスを取得するだけ`IDataProtector`try キャストと通常の方法で、`IDataProtector`に`IPersistedDataProtector`です。</span><span class="sxs-lookup"><span data-stu-id="d0f20-114">To get an instance of `IPersistedDataProtector`, simply get an instance of `IDataProtector` in the normal fashion and try casting the `IDataProtector` to `IPersistedDataProtector`.</span></span>

> [!NOTE]
> <span data-ttu-id="d0f20-115">すべて`IDataProtector`インスタンスにキャストできる`IPersistedDataProtector`です。</span><span class="sxs-lookup"><span data-stu-id="d0f20-115">Not all `IDataProtector` instances can be cast to `IPersistedDataProtector`.</span></span> <span data-ttu-id="d0f20-116">演算子として、c# の開発者が使用する必要があります。 または障害の場合を適切に処理する準備をほぼ同じ結果が、無効なキャストにより、ランタイム例外を回避する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d0f20-116">Developers should use the C# as operator or similar to avoid runtime exceptions caused by invalid casts, and they should be prepared to handle the failure case appropriately.</span></span>

<span data-ttu-id="d0f20-117">`IPersistedDataProtector`次の API サーフェスを公開します。</span><span class="sxs-lookup"><span data-stu-id="d0f20-117">`IPersistedDataProtector` exposes the following API surface:</span></span>

```csharp
DangerousUnprotect(byte[] protectedData, bool ignoreRevocationErrors,
     out bool requiresMigration, out bool wasRevoked) : byte[]
```

<span data-ttu-id="d0f20-118">この API は、(バイト配列) として保護されているペイロードを取得し、保護されていないペイロードを返します。</span><span class="sxs-lookup"><span data-stu-id="d0f20-118">This API takes the protected payload (as a byte array) and returns the unprotected payload.</span></span> <span data-ttu-id="d0f20-119">文字列ベースのオーバー ロードはありません。</span><span class="sxs-lookup"><span data-stu-id="d0f20-119">There is no string-based overload.</span></span> <span data-ttu-id="d0f20-120">Out パラメーター 2 は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="d0f20-120">The two out parameters are as follows.</span></span>

* <span data-ttu-id="d0f20-121">`requiresMigration`: 実行される場所をこのペイロードを保護するためのキーが古いおよび以降の操作をローリング キーになどのこのペイロードを保護するためのキーは、アクティブな既定のキーでは不要になった場合は true に設定されます。</span><span class="sxs-lookup"><span data-stu-id="d0f20-121">`requiresMigration`: will be set to true if the key used to protect this payload is no longer the active default key, e.g., the key used to protect this payload is old and a key rolling operation has since taken place.</span></span> <span data-ttu-id="d0f20-122">呼び出し元は、ビジネス ニーズに応じて、ペイロードの再保護を考慮する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d0f20-122">The caller may wish to consider reprotecting the payload depending on their business needs.</span></span>

* <span data-ttu-id="d0f20-123">`wasRevoked`: このペイロードを保護するためのキーが取り消された場合は true に設定されます。</span><span class="sxs-lookup"><span data-stu-id="d0f20-123">`wasRevoked`: will be set to true if the key used to protect this payload was revoked.</span></span>

>[!WARNING]
> <span data-ttu-id="d0f20-124">慎重に渡す際に`ignoreRevocationErrors: true`を`DangerousUnprotect`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="d0f20-124">Exercise extreme caution when passing `ignoreRevocationErrors: true` to the `DangerousUnprotect` method.</span></span> <span data-ttu-id="d0f20-125">このメソッドを呼び出した後の場合、`wasRevoked`値が true の場合、このペイロードを保護するためのキーが取り消された、し、ペイロードの信頼性を問題ありとして扱う必要があります。</span><span class="sxs-lookup"><span data-stu-id="d0f20-125">If after calling this method the `wasRevoked` value is true, then the key used to protect this payload was revoked, and the payload's authenticity should be treated as suspect.</span></span> <span data-ttu-id="d0f20-126">この場合、のみ操作が続行保護されていないペイロードでいくつか別の保証がある場合、本物である、ことなどの信頼されていない web クライアントによって送信されるのではなく、セキュリティで保護されたデータベースから受信します。</span><span class="sxs-lookup"><span data-stu-id="d0f20-126">In this case, only continue operating on the unprotected payload if you have some separate assurance that it is authentic, e.g. that it's coming from a secure database rather than being sent by an untrusted web client.</span></span>

[!code-csharp[Main](dangerous-unprotect/samples/dangerous-unprotect.cs)]
