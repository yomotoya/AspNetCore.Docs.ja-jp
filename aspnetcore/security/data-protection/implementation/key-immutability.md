---
title: キーの不変性と ASP.NET Core でのキーの設定
author: rick-anderson
description: ASP.NET Core データ保護キーの不変の Api の実装の詳細について説明します。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: 7796cb102c0f6f03809704016fd36b28c7a82438
ms.sourcegitcommit: 8f8924ce4eb9effeaf489f177fb01b66867da16f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/24/2018
ms.locfileid: "39219304"
---
# <a name="key-immutability-and-key-settings-in-aspnet-core"></a><span data-ttu-id="f74cf-103">キーの不変性と ASP.NET Core でのキーの設定</span><span class="sxs-lookup"><span data-stu-id="f74cf-103">Key immutability and key settings in ASP.NET Core</span></span>

<span data-ttu-id="f74cf-104">オブジェクトは、バッキング ストアに永続化されるとその表現は永久に修正されます。</span><span class="sxs-lookup"><span data-stu-id="f74cf-104">Once an object is persisted to the backing store, its representation is forever fixed.</span></span> <span data-ttu-id="f74cf-105">新しいデータは、バッキング ストアに追加できますが、既存のデータを変更しないことができます。</span><span class="sxs-lookup"><span data-stu-id="f74cf-105">New data can be added to the backing store, but existing data can never be mutated.</span></span> <span data-ttu-id="f74cf-106">この動作の主な目的は、データの破損を防ぐためです。</span><span class="sxs-lookup"><span data-stu-id="f74cf-106">The primary purpose of this behavior is to prevent data corruption.</span></span>

<span data-ttu-id="f74cf-107">この動作の 1 つの結果は、こと、キーは、バッキング ストアに書き込まれますが後、は変更できません。</span><span class="sxs-lookup"><span data-stu-id="f74cf-107">One consequence of this behavior is that once a key is written to the backing store, it's immutable.</span></span> <span data-ttu-id="f74cf-108">その作成、ライセンス認証、および有効期限の日付変更できないを使用して取り消すことができる、`IKeyManager`します。</span><span class="sxs-lookup"><span data-stu-id="f74cf-108">Its creation, activation, and expiration dates can never be changed, though it can revoked by using `IKeyManager`.</span></span> <span data-ttu-id="f74cf-109">さらに、その基になるアルゴリズム情報、マスター_キー生成情報、および rest プロパティでの暗号化も変更できません。</span><span class="sxs-lookup"><span data-stu-id="f74cf-109">Additionally, its underlying algorithmic information, master keying material, and encryption at rest properties are also immutable.</span></span>

<span data-ttu-id="f74cf-110">開発者は、キーの永続性に影響を与える設定を変更する場合これらの変更には触れません効果、キーを生成すると、いずれかを明示的に呼び出すによって次の時間まで`IKeyManager.CreateNewKey`またはシステム経由で、データ保護の独自[自動キー生成](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)動作します。</span><span class="sxs-lookup"><span data-stu-id="f74cf-110">If the developer changes any setting that affects key persistence, those changes won't go into effect until the next time a key is generated, either via an explicit call to `IKeyManager.CreateNewKey` or via the data protection system's own [automatic key generation](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) behavior.</span></span> <span data-ttu-id="f74cf-111">キーの永続性に影響する設定は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="f74cf-111">The settings that affect key persistence are as follows:</span></span>

* [<span data-ttu-id="f74cf-112">既定キーの有効期間</span><span class="sxs-lookup"><span data-stu-id="f74cf-112">The default key lifetime</span></span>](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)

* [<span data-ttu-id="f74cf-113">Rest のメカニズムでは、キーの暗号化</span><span class="sxs-lookup"><span data-stu-id="f74cf-113">The key encryption at rest mechanism</span></span>](xref:security/data-protection/implementation/key-encryption-at-rest)

* [<span data-ttu-id="f74cf-114">キー内に含まれているアルゴリズムの情報</span><span class="sxs-lookup"><span data-stu-id="f74cf-114">The algorithmic information contained within the key</span></span>](xref:security/data-protection/configuration/overview#changing-algorithms-with-usecryptographicalgorithms)

<span data-ttu-id="f74cf-115">これらの設定、次の自動キーのローリング時間よりも前に開始するために必要な場合は、明示的な呼び出しを行うことを検討してください`IKeyManager.CreateNewKey`新しいキーの作成を強制します。</span><span class="sxs-lookup"><span data-stu-id="f74cf-115">If you need these settings to kick in earlier than the next automatic key rolling time, consider making an explicit call to `IKeyManager.CreateNewKey` to force the creation of a new key.</span></span> <span data-ttu-id="f74cf-116">明示的にアクティブ化日付を指定してください ({0} 今すぐ + 2 日} は、変更が反映されるまでの時間を許可する適切な目安) と呼び出しの有効期限の日付。</span><span class="sxs-lookup"><span data-stu-id="f74cf-116">Remember to provide an explicit activation date ({ now + 2 days } is a good rule of thumb to allow time for the change to propagate) and expiration date in the call.</span></span>

>[!TIP]
> <span data-ttu-id="f74cf-117">リポジトリの手を加えることのすべてのアプリケーションが使用して、同じ設定を指定する必要があります、`IDataProtectionBuilder`拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="f74cf-117">All applications touching the repository should specify the same settings with the `IDataProtectionBuilder` extension methods.</span></span> <span data-ttu-id="f74cf-118">それ以外の場合、永続化されたキーのプロパティは、キーの生成のルーチンを呼び出す特定のアプリケーションに依存するされます。</span><span class="sxs-lookup"><span data-stu-id="f74cf-118">Otherwise, the properties of the persisted key will be dependent on the particular application that invoked the key generation routines.</span></span>
