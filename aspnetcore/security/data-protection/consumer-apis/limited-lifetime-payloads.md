---
title: ASP.NET Core で保護されたペイロードの有効期間を制限します。
author: rick-anderson
description: ASP.NET Core データ保護 Api を使用して保護されたペイロードの有効期間を制限する方法について説明します。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/limited-lifetime-payloads
ms.openlocfilehash: 8dc3b856ec67477ec8ae777749c9bf3107eb4eda
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897119"
---
# <a name="limit-the-lifetime-of-protected-payloads-in-aspnet-core"></a><span data-ttu-id="34315-103">ASP.NET Core で保護されたペイロードの有効期間を制限します。</span><span class="sxs-lookup"><span data-stu-id="34315-103">Limit the lifetime of protected payloads in ASP.NET Core</span></span>

<span data-ttu-id="34315-104">一定期間の後に期限切れになる保護されているペイロードを作成するアプリケーション開発者が必要があるシナリオがあります。</span><span class="sxs-lookup"><span data-stu-id="34315-104">There are scenarios where the application developer wants to create a protected payload that expires after a set period of time.</span></span> <span data-ttu-id="34315-105">たとえば、保護されたペイロードは、必ず有効な 1 時間分のパスワード リセット トークンを表すことができます。</span><span class="sxs-lookup"><span data-stu-id="34315-105">For instance, the protected payload might represent a password reset token that should only be valid for one hour.</span></span> <span data-ttu-id="34315-106">確かに、埋め込みの有効期限日を含む独自のペイロード形式を作成する開発者可能性がありますと高度な開発者はとにかく、そうことができますが、大多数の開発者のこれらの有効期限の管理を拡張できる面倒です。</span><span class="sxs-lookup"><span data-stu-id="34315-106">It's certainly possible for the developer to create their own payload format that contains an embedded expiration date, and advanced developers may wish to do this anyway, but for the majority of developers managing these expirations can grow tedious.</span></span>

<span data-ttu-id="34315-107">マイクロソフト開発者向け、パッケージを簡単に確認する[Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/)一定期間の後に自動的に期限切れのペイロードを作成するためのユーティリティ Api が含まれています。</span><span class="sxs-lookup"><span data-stu-id="34315-107">To make this easier for our developer audience, the package [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) contains utility APIs for creating payloads that automatically expire after a set period of time.</span></span> <span data-ttu-id="34315-108">これらの Api がオフのハング、`ITimeLimitedDataProtector`型。</span><span class="sxs-lookup"><span data-stu-id="34315-108">These APIs hang off of the `ITimeLimitedDataProtector` type.</span></span>

## <a name="api-usage"></a><span data-ttu-id="34315-109">API の使用方法</span><span class="sxs-lookup"><span data-stu-id="34315-109">API usage</span></span>

<span data-ttu-id="34315-110">`ITimeLimitedDataProtector`インターフェイスは、主要なインターフェイスの保護と時間制限付き/期限切れ間近自己のペイロードの保護を解除します。</span><span class="sxs-lookup"><span data-stu-id="34315-110">The `ITimeLimitedDataProtector` interface is the core interface for protecting and unprotecting time-limited / self-expiring payloads.</span></span> <span data-ttu-id="34315-111">インスタンスを作成する、 `ITimeLimitedDataProtector`、通常のインスタンスを必要がありますまず[IDataProtector](xref:security/data-protection/consumer-apis/overview)特定の目的で構築します。</span><span class="sxs-lookup"><span data-stu-id="34315-111">To create an instance of an `ITimeLimitedDataProtector`, you'll first need an instance of a regular [IDataProtector](xref:security/data-protection/consumer-apis/overview) constructed with a specific purpose.</span></span> <span data-ttu-id="34315-112">1 回、`IDataProtector`インスタンスが使用可能な呼び出し、`IDataProtector.ToTimeLimitedDataProtector`有効期限の組み込み機能を備えたのプロテクターを取得する拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="34315-112">Once the `IDataProtector` instance is available, call the `IDataProtector.ToTimeLimitedDataProtector` extension method to get back a protector with built-in expiration capabilities.</span></span>

<span data-ttu-id="34315-113">`ITimeLimitedDataProtector` 次の API サーフェスと拡張機能メソッドを公開します。</span><span class="sxs-lookup"><span data-stu-id="34315-113">`ITimeLimitedDataProtector` exposes the following API surface and extension methods:</span></span>

* <span data-ttu-id="34315-114">CreateProtector(string purpose) :ITimeLimitedDataProtector - この API は、既存のような`IDataProtectionProvider.CreateProtector`を作成するために使用できる点で[チェーンを目的](xref:security/data-protection/consumer-apis/purpose-strings)ルート期間限定の保護機能からです。</span><span class="sxs-lookup"><span data-stu-id="34315-114">CreateProtector(string purpose) : ITimeLimitedDataProtector - This API is similar to the existing `IDataProtectionProvider.CreateProtector` in that it can be used to create [purpose chains](xref:security/data-protection/consumer-apis/purpose-strings) from a root time-limited protector.</span></span>

* <span data-ttu-id="34315-115">Protect(byte[] plaintext, DateTimeOffset expiration) : byte[]</span><span class="sxs-lookup"><span data-stu-id="34315-115">Protect(byte[] plaintext, DateTimeOffset expiration) : byte[]</span></span>

* <span data-ttu-id="34315-116">Protect(byte[] plaintext, TimeSpan lifetime) : byte[]</span><span class="sxs-lookup"><span data-stu-id="34315-116">Protect(byte[] plaintext, TimeSpan lifetime) : byte[]</span></span>

* <span data-ttu-id="34315-117">Protect(byte[] plaintext) : byte[]</span><span class="sxs-lookup"><span data-stu-id="34315-117">Protect(byte[] plaintext) : byte[]</span></span>

* <span data-ttu-id="34315-118">(文字列プレーン テキスト、DateTimeOffset の有効期限) の保護: 文字列</span><span class="sxs-lookup"><span data-stu-id="34315-118">Protect(string plaintext, DateTimeOffset expiration) : string</span></span>

* <span data-ttu-id="34315-119">Protect(string plaintext, TimeSpan lifetime) : string</span><span class="sxs-lookup"><span data-stu-id="34315-119">Protect(string plaintext, TimeSpan lifetime) : string</span></span>

* <span data-ttu-id="34315-120">Protect(string plaintext) : string</span><span class="sxs-lookup"><span data-stu-id="34315-120">Protect(string plaintext) : string</span></span>

<span data-ttu-id="34315-121">コアだけでなく`Protect`プレーン テキストのみを実行するメソッドが、ペイロードの有効期限の日付を指定することは新しいオーバー ロードがあります。</span><span class="sxs-lookup"><span data-stu-id="34315-121">In addition to the core `Protect` methods which take only the plaintext, there are new overloads which allow specifying the payload's expiration date.</span></span> <span data-ttu-id="34315-122">有効期限の日付を検証する絶対的日付として指定できます (を使用して、 `DateTimeOffset`) または相対時刻として (現在のシステムからを使用して、後で、 `TimeSpan`)。</span><span class="sxs-lookup"><span data-stu-id="34315-122">The expiration date can be specified as an absolute date (via a `DateTimeOffset`) or as a relative time (from the current system time, via a `TimeSpan`).</span></span> <span data-ttu-id="34315-123">有効期限を受け取りません。 指定できるオーバー ロードを呼び出すと、ペイロードが決して期限切れになると見なされます。</span><span class="sxs-lookup"><span data-stu-id="34315-123">If an overload which doesn't take an expiration is called, the payload is assumed never to expire.</span></span>

* <span data-ttu-id="34315-124">(Byte protectedData、DateTimeOffset の有効期限を) の保護を解除: byte[]</span><span class="sxs-lookup"><span data-stu-id="34315-124">Unprotect(byte[] protectedData, out DateTimeOffset expiration) : byte[]</span></span>

* <span data-ttu-id="34315-125">Unprotect(byte[] protectedData) : byte[]</span><span class="sxs-lookup"><span data-stu-id="34315-125">Unprotect(byte[] protectedData) : byte[]</span></span>

* <span data-ttu-id="34315-126">(DateTimeOffset の有効期限を文字列 protectedData) 保護の解除: 文字列</span><span class="sxs-lookup"><span data-stu-id="34315-126">Unprotect(string protectedData, out DateTimeOffset expiration) : string</span></span>

* <span data-ttu-id="34315-127">Unprotect(string protectedData) : string</span><span class="sxs-lookup"><span data-stu-id="34315-127">Unprotect(string protectedData) : string</span></span>

<span data-ttu-id="34315-128">`Unprotect`メソッドは、元の保護されていないデータを返します。</span><span class="sxs-lookup"><span data-stu-id="34315-128">The `Unprotect` methods return the original unprotected data.</span></span> <span data-ttu-id="34315-129">ペイロードがまだ失効しておらず、絶対有効期限が省略可能な出力元の保護されていないデータと共にパラメーターとして返されます。</span><span class="sxs-lookup"><span data-stu-id="34315-129">If the payload hasn't yet expired, the absolute expiration is returned as an optional out parameter along with the original unprotected data.</span></span> <span data-ttu-id="34315-130">ペイロードが有効期限が切れて Unprotect メソッドのすべてのオーバー ロードは CryptographicException をスローします。</span><span class="sxs-lookup"><span data-stu-id="34315-130">If the payload is expired, all overloads of the Unprotect method will throw CryptographicException.</span></span>

>[!WARNING]
> <span data-ttu-id="34315-131">これらの Api を使用して、長期間または無期限の永続化を必要とするペイロードを保護することをお勧めはできません。</span><span class="sxs-lookup"><span data-stu-id="34315-131">It's not advised to use these APIs to protect payloads which require long-term or indefinite persistence.</span></span> <span data-ttu-id="34315-132">「ことを許容できる 1 か月後にある回復可能な完全に保護されたペイロードのでしょうか。」</span><span class="sxs-lookup"><span data-stu-id="34315-132">"Can I afford for the protected payloads to be permanently unrecoverable after a month?"</span></span> <span data-ttu-id="34315-133">目安; として使用できます。場合、答えは、開発者検討してくださいない代替の Api。</span><span class="sxs-lookup"><span data-stu-id="34315-133">can serve as a good rule of thumb; if the answer is no then developers should consider alternative APIs.</span></span>

<span data-ttu-id="34315-134">使用して次のサンプル、 [DI 以外のコード パス](xref:security/data-protection/configuration/non-di-scenarios)データ保護システムをインスタンス化するためです。</span><span class="sxs-lookup"><span data-stu-id="34315-134">The sample below uses the [non-DI code paths](xref:security/data-protection/configuration/non-di-scenarios) for instantiating the data protection system.</span></span> <span data-ttu-id="34315-135">このサンプルを実行するには、まず Microsoft.AspNetCore.DataProtection.Extensions パッケージへの参照を追加したことを確認します。</span><span class="sxs-lookup"><span data-stu-id="34315-135">To run this sample, ensure that you have first added a reference to the Microsoft.AspNetCore.DataProtection.Extensions package.</span></span>

[!code-csharp[](limited-lifetime-payloads/samples/limitedlifetimepayloads.cs)]
