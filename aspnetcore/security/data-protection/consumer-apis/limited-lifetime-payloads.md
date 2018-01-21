---
title: "保護されたペイロードの有効期間を制限します。"
author: rick-anderson
description: "このドキュメントでは、ASP.NET Core データ保護 Api を使用して保護されているペイロードの有効期間を制限する方法について説明します。"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/limited-lifetime-payloads
ms.openlocfilehash: 144097cd1551c1d0aece5df20ce01e14146a41d1
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="limiting-the-lifetime-of-protected-payloads"></a><span data-ttu-id="5a55f-103">保護されたペイロードの有効期間を制限します。</span><span class="sxs-lookup"><span data-stu-id="5a55f-103">Limiting the lifetime of protected payloads</span></span>

<span data-ttu-id="5a55f-104">アプリケーション開発者が一定期間の後に期限切れになる保護されているペイロードを作成しようとした、シナリオがあります。</span><span class="sxs-lookup"><span data-stu-id="5a55f-104">There are scenarios where the application developer wants to create a protected payload that expires after a set period of time.</span></span> <span data-ttu-id="5a55f-105">たとえば、保護されたペイロードは必要があるだけ有効な 1 つの 1 時間以内のパスワード リセット トークンを表す場合があります。</span><span class="sxs-lookup"><span data-stu-id="5a55f-105">For instance, the protected payload might represent a password reset token that should only be valid for one hour.</span></span> <span data-ttu-id="5a55f-106">ことが確かに、埋め込みの有効期限日を含む独自のペイロード形式を作成する開発者およびこれを行うか、上級開発者が望まけれども、開発者の大部分の有効期限のこれらの管理を拡張できる面倒</span><span class="sxs-lookup"><span data-stu-id="5a55f-106">It is certainly possible for the developer to create their own payload format that contains an embedded expiration date, and advanced developers may wish to do this anyway, but for the majority of developers managing these expirations can grow tedious.</span></span>

<span data-ttu-id="5a55f-107">開発者、ユーザー向けのパッケージを簡単に作成する[Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/)一定期間の後に自動的に期限切れのペイロードを作成するためのユーティリティ Api が含まれています。</span><span class="sxs-lookup"><span data-stu-id="5a55f-107">To make this easier for our developer audience, the package [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) contains utility APIs for creating payloads that automatically expire after a set period of time.</span></span> <span data-ttu-id="5a55f-108">これらの Api がオフのハング、`ITimeLimitedDataProtector`型です。</span><span class="sxs-lookup"><span data-stu-id="5a55f-108">These APIs hang off of the `ITimeLimitedDataProtector` type.</span></span>

## <a name="api-usage"></a><span data-ttu-id="5a55f-109">API の使用方法</span><span class="sxs-lookup"><span data-stu-id="5a55f-109">API usage</span></span>

<span data-ttu-id="5a55f-110">`ITimeLimitedDataProtector`インターフェイスは、コア インターフェイスの保護や、時間制限/自己期限切れ間近のペイロードを復号化します。</span><span class="sxs-lookup"><span data-stu-id="5a55f-110">The `ITimeLimitedDataProtector` interface is the core interface for protecting and unprotecting time-limited / self-expiring payloads.</span></span> <span data-ttu-id="5a55f-111">インスタンスを作成する、 `ITimeLimitedDataProtector`、最初、通常のインスタンスを必要があります[IDataProtector](overview.md)特定の目的で構築します。</span><span class="sxs-lookup"><span data-stu-id="5a55f-111">To create an instance of an `ITimeLimitedDataProtector`, you'll first need an instance of a regular [IDataProtector](overview.md) constructed with a specific purpose.</span></span> <span data-ttu-id="5a55f-112">1 回、`IDataProtector`インスタンスが使用可能な呼び出し、`IDataProtector.ToTimeLimitedDataProtector`有効期限の組み込み機能を備えたの保護機能を取得する拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="5a55f-112">Once the `IDataProtector` instance is available, call the `IDataProtector.ToTimeLimitedDataProtector` extension method to get back a protector with built-in expiration capabilities.</span></span>

<span data-ttu-id="5a55f-113">`ITimeLimitedDataProtector`以下の API サーフェスと拡張メソッドを公開します。</span><span class="sxs-lookup"><span data-stu-id="5a55f-113">`ITimeLimitedDataProtector` exposes the following API surface and extension methods:</span></span>

* <span data-ttu-id="5a55f-114">CreateProtector (文字列目的): ITimeLimitedDataProtector - この API は、既存のような`IDataProtectionProvider.CreateProtector`を作成するために使用できる点で[チェーンを目的](purpose-strings.md)ルート期間限定の保護機能からです。</span><span class="sxs-lookup"><span data-stu-id="5a55f-114">CreateProtector(string purpose) : ITimeLimitedDataProtector - This API is similar to the existing `IDataProtectionProvider.CreateProtector` in that it can be used to create [purpose chains](purpose-strings.md) from a root time-limited protector.</span></span>

* <span data-ttu-id="5a55f-115">Protect(byte[] plaintext, DateTimeOffset expiration) : byte[]</span><span class="sxs-lookup"><span data-stu-id="5a55f-115">Protect(byte[] plaintext, DateTimeOffset expiration) : byte[]</span></span>

* <span data-ttu-id="5a55f-116">Protect(byte[] plaintext, TimeSpan lifetime) : byte[]</span><span class="sxs-lookup"><span data-stu-id="5a55f-116">Protect(byte[] plaintext, TimeSpan lifetime) : byte[]</span></span>

* <span data-ttu-id="5a55f-117">Protect(byte[] plaintext) : byte[]</span><span class="sxs-lookup"><span data-stu-id="5a55f-117">Protect(byte[] plaintext) : byte[]</span></span>

* <span data-ttu-id="5a55f-118">(文字列プレーン テキスト、DateTimeOffset の有効期限) を保護する: 文字列</span><span class="sxs-lookup"><span data-stu-id="5a55f-118">Protect(string plaintext, DateTimeOffset expiration) : string</span></span>

* <span data-ttu-id="5a55f-119">保護 (文字列のプレーン テキスト、TimeSpan の有効期間): 文字列</span><span class="sxs-lookup"><span data-stu-id="5a55f-119">Protect(string plaintext, TimeSpan lifetime) : string</span></span>

* <span data-ttu-id="5a55f-120">(文字列のプレーン テキスト) を保護する: 文字列</span><span class="sxs-lookup"><span data-stu-id="5a55f-120">Protect(string plaintext) : string</span></span>

<span data-ttu-id="5a55f-121">コアに加えて`Protect`プレーン テキストのみを実行するメソッド、ペイロードの有効期限の日付を指定することを許可する新しいオーバー ロードがあります。</span><span class="sxs-lookup"><span data-stu-id="5a55f-121">In addition to the core `Protect` methods which take only the plaintext, there are new overloads which allow specifying the payload's expiration date.</span></span> <span data-ttu-id="5a55f-122">絶対日付として有効期限の日付を指定することができます (を使用して、 `DateTimeOffset`) または相対時刻として (現在のシステムからを使用して、後で、 `TimeSpan`)。</span><span class="sxs-lookup"><span data-stu-id="5a55f-122">The expiration date can be specified as an absolute date (via a `DateTimeOffset`) or as a relative time (from the current system time, via a `TimeSpan`).</span></span> <span data-ttu-id="5a55f-123">有効期限を受け取らない指定できるオーバー ロードが呼び出されると、ペイロードが期限切れにしないと見なされます。</span><span class="sxs-lookup"><span data-stu-id="5a55f-123">If an overload which doesn't take an expiration is called, the payload is assumed never to expire.</span></span>

* <span data-ttu-id="5a55f-124">Unprotect(byte[] protectedData, out DateTimeOffset expiration) : byte[]</span><span class="sxs-lookup"><span data-stu-id="5a55f-124">Unprotect(byte[] protectedData, out DateTimeOffset expiration) : byte[]</span></span>

* <span data-ttu-id="5a55f-125">Unprotect(byte[] protectedData) : byte[]</span><span class="sxs-lookup"><span data-stu-id="5a55f-125">Unprotect(byte[] protectedData) : byte[]</span></span>

* <span data-ttu-id="5a55f-126">(DateTimeOffset の有効期限を文字列 protectedData) の保護を解除: 文字列</span><span class="sxs-lookup"><span data-stu-id="5a55f-126">Unprotect(string protectedData, out DateTimeOffset expiration) : string</span></span>

* <span data-ttu-id="5a55f-127">(文字列 protectedData) の保護を解除: 文字列</span><span class="sxs-lookup"><span data-stu-id="5a55f-127">Unprotect(string protectedData) : string</span></span>

<span data-ttu-id="5a55f-128">`Unprotect`メソッドには、元の保護されていないデータが返されます。</span><span class="sxs-lookup"><span data-stu-id="5a55f-128">The `Unprotect` methods return the original unprotected data.</span></span> <span data-ttu-id="5a55f-129">ペイロードがまだ失効しておらず、絶対有効期限が省略可能な出力元の保護されていないデータと共にパラメーターとして返されます。</span><span class="sxs-lookup"><span data-stu-id="5a55f-129">If the payload hasn't yet expired, the absolute expiration is returned as an optional out parameter along with the original unprotected data.</span></span> <span data-ttu-id="5a55f-130">ペイロードが有効期限が切れて Unprotect メソッドのすべてのオーバー ロードは CryptographicException をスローします。</span><span class="sxs-lookup"><span data-stu-id="5a55f-130">If the payload is expired, all overloads of the Unprotect method will throw CryptographicException.</span></span>

>[!WARNING]
> <span data-ttu-id="5a55f-131">これらの Api を使用して、永続的または長期の永続化を必要とするペイロードを保護することはお勧めしません。</span><span class="sxs-lookup"><span data-stu-id="5a55f-131">It is not advised to use these APIs to protect payloads which require long-term or indefinite persistence.</span></span> <span data-ttu-id="5a55f-132">「ことを許容できるは回復不能に永続的に、1 か月後に保護されているペイロードですか?」</span><span class="sxs-lookup"><span data-stu-id="5a55f-132">"Can I afford for the protected payloads to be permanently unrecoverable after a month?"</span></span> <span data-ttu-id="5a55f-133">適切な経験則; として使用できます。回答がある場合、開発者検討してくださいありません代替 Api。</span><span class="sxs-lookup"><span data-stu-id="5a55f-133">can serve as a good rule of thumb; if the answer is no then developers should consider alternative APIs.</span></span>

<span data-ttu-id="5a55f-134">使用して下記のサンプル、 [DI 以外のコード パス](../configuration/non-di-scenarios.md)データ保護システムをインスタンス化するためです。</span><span class="sxs-lookup"><span data-stu-id="5a55f-134">The sample below uses the [non-DI code paths](../configuration/non-di-scenarios.md) for instantiating the data protection system.</span></span> <span data-ttu-id="5a55f-135">このサンプルを実行するには、Microsoft.AspNetCore.DataProtection.Extensions パッケージへの参照が最初に追加されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="5a55f-135">To run this sample, ensure that you have first added a reference to the Microsoft.AspNetCore.DataProtection.Extensions package.</span></span>

[!code-csharp[Main](limited-lifetime-payloads/samples/limitedlifetimepayloads.cs)]
