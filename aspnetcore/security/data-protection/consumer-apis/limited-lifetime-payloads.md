---
title: "保護されたペイロードの有効期間を制限します。"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 000175e2-10fc-43dd-bfc2-51e004b97b44
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/limited-lifetime-payloads
ms.openlocfilehash: 4ff13803b328c1e9dd2934c38c88b43f5798de03
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/11/2017
---
# <a name="limiting-the-lifetime-of-protected-payloads"></a><span data-ttu-id="d96aa-103">保護されたペイロードの有効期間を制限します。</span><span class="sxs-lookup"><span data-stu-id="d96aa-103">Limiting the lifetime of protected payloads</span></span>

<span data-ttu-id="d96aa-104">アプリケーション開発者が一定期間の後に期限切れになる保護されているペイロードを作成しようとした、シナリオがあります。</span><span class="sxs-lookup"><span data-stu-id="d96aa-104">There are scenarios where the application developer wants to create a protected payload that expires after a set period of time.</span></span> <span data-ttu-id="d96aa-105">たとえば、保護されたペイロードは必要があるだけ有効な 1 つの 1 時間以内のパスワード リセット トークンを表す場合があります。</span><span class="sxs-lookup"><span data-stu-id="d96aa-105">For instance, the protected payload might represent a password reset token that should only be valid for one hour.</span></span> <span data-ttu-id="d96aa-106">ことが確かに、埋め込みの有効期限日を含む独自のペイロード形式を作成する開発者およびこれを行うか、上級開発者が望まけれども、開発者の大部分の有効期限のこれらの管理を拡張できる面倒</span><span class="sxs-lookup"><span data-stu-id="d96aa-106">It is certainly possible for the developer to create their own payload format that contains an embedded expiration date, and advanced developers may wish to do this anyway, but for the majority of developers managing these expirations can grow tedious.</span></span>

<span data-ttu-id="d96aa-107">この簡単にするため、開発者向けの Microsoft.AspNetCore.DataProtection.Extensions パッケージには一定期間の後に自動的に期限切れのペイロードを作成するためのユーティリティ Api が含まれています。</span><span class="sxs-lookup"><span data-stu-id="d96aa-107">To make this easier for our developer audience, the package Microsoft.AspNetCore.DataProtection.Extensions contains utility APIs for creating payloads that automatically expire after a set period of time.</span></span> <span data-ttu-id="d96aa-108">これらの Api は、ITimeLimitedDataProtector 型から切断します。</span><span class="sxs-lookup"><span data-stu-id="d96aa-108">These APIs hang off of the ITimeLimitedDataProtector type.</span></span>

## <a name="api-usage"></a><span data-ttu-id="d96aa-109">API の使用方法</span><span class="sxs-lookup"><span data-stu-id="d96aa-109">API usage</span></span>

<span data-ttu-id="d96aa-110">ITimeLimitedDataProtector インターフェイスは、保護し、時間制限を復号化/ペイロードを自己期限切れのコア インターフェイスです。</span><span class="sxs-lookup"><span data-stu-id="d96aa-110">The ITimeLimitedDataProtector interface is the core interface for protecting and unprotecting time-limited / self-expiring payloads.</span></span> <span data-ttu-id="d96aa-111">ITimeLimitedDataProtector のインスタンスを作成するには、最初、通常のインスタンスを必要がありますが[IDataProtector](overview.md)特定の目的で構築します。</span><span class="sxs-lookup"><span data-stu-id="d96aa-111">To create an instance of an ITimeLimitedDataProtector, you'll first need an instance of a regular [IDataProtector](overview.md) constructed with a specific purpose.</span></span> <span data-ttu-id="d96aa-112">IDataProtector インスタンスが使用可能、メソッドを呼び出して IDataProtector.ToTimeLimitedDataProtector 拡張機能を有効期限の組み込み機能を備えた、保護機能を取得します。</span><span class="sxs-lookup"><span data-stu-id="d96aa-112">Once the IDataProtector instance is available, call the IDataProtector.ToTimeLimitedDataProtector extension method to get back a protector with built-in expiration capabilities.</span></span>

<span data-ttu-id="d96aa-113">ITimeLimitedDataProtector は、以下の API サーフェスと拡張メソッドを公開します。</span><span class="sxs-lookup"><span data-stu-id="d96aa-113">ITimeLimitedDataProtector exposes the following API surface and extension methods:</span></span>

* <span data-ttu-id="d96aa-114">CreateProtector (文字列目的): を作成するために使用できる点で、ITimeLimitedDataProtector この API は既存の IDataProtectionProvider.CreateProtector に似ています[チェーンを目的](purpose-strings.md)ルート期間限定の保護機能からです。</span><span class="sxs-lookup"><span data-stu-id="d96aa-114">CreateProtector(string purpose) : ITimeLimitedDataProtector This API is similar to the existing IDataProtectionProvider.CreateProtector in that it can be used to create [purpose chains](purpose-strings.md) from a root time-limited protector.</span></span>

* <span data-ttu-id="d96aa-115">(バイト:operator[] プレーン テキスト、DateTimeOffset の有効期限) を保護する: byte[]</span><span class="sxs-lookup"><span data-stu-id="d96aa-115">Protect(byte[] plaintext, DateTimeOffset expiration) : byte[]</span></span>

* <span data-ttu-id="d96aa-116">保護する (バイト:operator[] プレーン テキスト、TimeSpan の有効期間): byte[]</span><span class="sxs-lookup"><span data-stu-id="d96aa-116">Protect(byte[] plaintext, TimeSpan lifetime) : byte[]</span></span>

* <span data-ttu-id="d96aa-117">(バイト:operator[] プレーン テキスト) を保護する: byte[]</span><span class="sxs-lookup"><span data-stu-id="d96aa-117">Protect(byte[] plaintext) : byte[]</span></span>

* <span data-ttu-id="d96aa-118">(文字列プレーン テキスト、DateTimeOffset の有効期限) を保護する: 文字列</span><span class="sxs-lookup"><span data-stu-id="d96aa-118">Protect(string plaintext, DateTimeOffset expiration) : string</span></span>

* <span data-ttu-id="d96aa-119">保護 (文字列のプレーン テキスト、TimeSpan の有効期間): 文字列</span><span class="sxs-lookup"><span data-stu-id="d96aa-119">Protect(string plaintext, TimeSpan lifetime) : string</span></span>

* <span data-ttu-id="d96aa-120">(文字列のプレーン テキスト) を保護する: 文字列</span><span class="sxs-lookup"><span data-stu-id="d96aa-120">Protect(string plaintext) : string</span></span>

<span data-ttu-id="d96aa-121">プレーン テキストのみを実行するコア保護メソッド、ペイロードの有効期限の日付を指定することを許可する新しいオーバー ロードがあります。</span><span class="sxs-lookup"><span data-stu-id="d96aa-121">In addition to the core Protect methods which take only the plaintext, there are new overloads which allow specifying the payload's expiration date.</span></span> <span data-ttu-id="d96aa-122">(現在のシステム時刻、TimeSpan を使用して) からの相対的な時間として、または (、DateTimeOffset) を使用して絶対日付として、有効期限の日付を指定できます。</span><span class="sxs-lookup"><span data-stu-id="d96aa-122">The expiration date can be specified as an absolute date (via a DateTimeOffset) or as a relative time (from the current system time, via a TimeSpan).</span></span> <span data-ttu-id="d96aa-123">有効期限を受け取らない指定できるオーバー ロードが呼び出されると、ペイロードが期限切れにしないと見なされます。</span><span class="sxs-lookup"><span data-stu-id="d96aa-123">If an overload which doesn't take an expiration is called, the payload is assumed never to expire.</span></span>

* <span data-ttu-id="d96aa-124">(バイト:operator[] protectedData、DateTimeOffset の有効期限アウト) の保護を解除: byte[]</span><span class="sxs-lookup"><span data-stu-id="d96aa-124">Unprotect(byte[] protectedData, out DateTimeOffset expiration) : byte[]</span></span>

* <span data-ttu-id="d96aa-125">(バイト:operator[] protectedData) の保護を解除: byte[]</span><span class="sxs-lookup"><span data-stu-id="d96aa-125">Unprotect(byte[] protectedData) : byte[]</span></span>

* <span data-ttu-id="d96aa-126">(DateTimeOffset の有効期限を文字列 protectedData) の保護を解除: 文字列</span><span class="sxs-lookup"><span data-stu-id="d96aa-126">Unprotect(string protectedData, out DateTimeOffset expiration) : string</span></span>

* <span data-ttu-id="d96aa-127">(文字列 protectedData) の保護を解除: 文字列</span><span class="sxs-lookup"><span data-stu-id="d96aa-127">Unprotect(string protectedData) : string</span></span>

<span data-ttu-id="d96aa-128">Unprotect メソッドには、元の保護されていないデータが返されます。</span><span class="sxs-lookup"><span data-stu-id="d96aa-128">The Unprotect methods return the original unprotected data.</span></span> <span data-ttu-id="d96aa-129">ペイロードがまだ失効しておらず、絶対有効期限が省略可能な出力元の保護されていないデータと共にパラメーターとして返されます。</span><span class="sxs-lookup"><span data-stu-id="d96aa-129">If the payload hasn't yet expired, the absolute expiration is returned as an optional out parameter along with the original unprotected data.</span></span> <span data-ttu-id="d96aa-130">ペイロードが有効期限が切れて Unprotect メソッドのすべてのオーバー ロードは CryptographicException をスローします。</span><span class="sxs-lookup"><span data-stu-id="d96aa-130">If the payload is expired, all overloads of the Unprotect method will throw CryptographicException.</span></span>

>[!WARNING]
> <span data-ttu-id="d96aa-131">これらの Api を使用して、永続的または長期の永続化を必要とするペイロードを保護することはお勧めしません。</span><span class="sxs-lookup"><span data-stu-id="d96aa-131">It is not advised to use these APIs to protect payloads which require long-term or indefinite persistence.</span></span> <span data-ttu-id="d96aa-132">「ことを許容できるは回復不能に永続的に、1 か月後に保護されているペイロードですか?」</span><span class="sxs-lookup"><span data-stu-id="d96aa-132">"Can I afford for the protected payloads to be permanently unrecoverable after a month?"</span></span> <span data-ttu-id="d96aa-133">適切な経験則; として使用できます。回答がある場合、開発者検討してくださいありません代替 Api。</span><span class="sxs-lookup"><span data-stu-id="d96aa-133">can serve as a good rule of thumb; if the answer is no then developers should consider alternative APIs.</span></span>

<span data-ttu-id="d96aa-134">使用して下記のサンプル、 [DI 以外のコード パス](../configuration/non-di-scenarios.md)データ保護システムをインスタンス化するためです。</span><span class="sxs-lookup"><span data-stu-id="d96aa-134">The sample below uses the [non-DI code paths](../configuration/non-di-scenarios.md) for instantiating the data protection system.</span></span> <span data-ttu-id="d96aa-135">このサンプルを実行するには、Microsoft.AspNetCore.DataProtection.Extensions パッケージへの参照が最初に追加されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="d96aa-135">To run this sample, ensure that you have first added a reference to the Microsoft.AspNetCore.DataProtection.Extensions package.</span></span>

<span data-ttu-id="d96aa-136">[!code-none[Main](limited-lifetime-payloads/samples/limitedlifetimepayloads.cs)]</span><span class="sxs-lookup"><span data-stu-id="d96aa-136">[!code-none[Main](limited-lifetime-payloads/samples/limitedlifetimepayloads.cs)]</span></span>
