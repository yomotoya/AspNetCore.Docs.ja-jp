---
title: キーの ASP.NET Core の不変性とキーの設定
author: rick-anderson
description: ASP.NET Core データ保護キー不変 Api の実装の詳細を説明します。
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: e918b00562aca9821de87c38f10242177517d8a5
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/22/2018
ms.locfileid: "30075636"
---
# <a name="key-immutability-and-key-settings-in-aspnet-core"></a><span data-ttu-id="02686-103">キーの ASP.NET Core の不変性とキーの設定</span><span class="sxs-lookup"><span data-stu-id="02686-103">Key immutability and key settings in ASP.NET Core</span></span>

<span data-ttu-id="02686-104">オブジェクトは、バッキング ストアに永続化した後、形式は永久に固定されます。</span><span class="sxs-lookup"><span data-stu-id="02686-104">Once an object is persisted to the backing store, its representation is forever fixed.</span></span> <span data-ttu-id="02686-105">バッキング ストアに新しいデータを追加することができますが、既存のデータを変更しないことができます。</span><span class="sxs-lookup"><span data-stu-id="02686-105">New data can be added to the backing store, but existing data can never be mutated.</span></span> <span data-ttu-id="02686-106">この動作の主な目的は、データの破損を防ぐためです。</span><span class="sxs-lookup"><span data-stu-id="02686-106">The primary purpose of this behavior is to prevent data corruption.</span></span>

<span data-ttu-id="02686-107">この動作の 1 つの結果は、こと、キーは、バッキング ストアに書き込まれますが後、は変更できません。</span><span class="sxs-lookup"><span data-stu-id="02686-107">One consequence of this behavior is that once a key is written to the backing store, it's immutable.</span></span> <span data-ttu-id="02686-108">作成、アクティブ化、および有効期限の日付は変更不可能を使用して取り消すことができますが、`IKeyManager`です。</span><span class="sxs-lookup"><span data-stu-id="02686-108">Its creation, activation, and expiration dates can never be changed, though it can revoked by using `IKeyManager`.</span></span> <span data-ttu-id="02686-109">さらに、その基になるアルゴリズム情報、マスター_キー生成情報、および rest プロパティでの暗号化も変更できません。</span><span class="sxs-lookup"><span data-stu-id="02686-109">Additionally, its underlying algorithmic information, master keying material, and encryption at rest properties are also immutable.</span></span>

<span data-ttu-id="02686-110">これらの変更はありません、キーを生成するのいずれかに明示的な呼び出しによって次の時間まで有効に移動する、開発者は、キーの永続性に影響する設定を変更する場合`IKeyManager.CreateNewKey`またはシステム経由で、データ保護の独自[自動キー生成](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)動作します。</span><span class="sxs-lookup"><span data-stu-id="02686-110">If the developer changes any setting that affects key persistence, those changes won't go into effect until the next time a key is generated, either via an explicit call to `IKeyManager.CreateNewKey` or via the data protection system's own [automatic key generation](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) behavior.</span></span> <span data-ttu-id="02686-111">キーの永続性に影響する設定は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="02686-111">The settings that affect key persistence are as follows:</span></span>

* [<span data-ttu-id="02686-112">既定キーの有効期間</span><span class="sxs-lookup"><span data-stu-id="02686-112">The default key lifetime</span></span>](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)

* [<span data-ttu-id="02686-113">Rest メカニズムでキーの暗号化</span><span class="sxs-lookup"><span data-stu-id="02686-113">The key encryption at rest mechanism</span></span>](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest)

* [<span data-ttu-id="02686-114">キーに含まれるアルゴリズム情報</span><span class="sxs-lookup"><span data-stu-id="02686-114">The algorithmic information contained within the key</span></span>](xref:security/data-protection/configuration/overview#changing-algorithms-with-usecryptographicalgorithms)

<span data-ttu-id="02686-115">をこれらの設定、次の自動キーのローリング時間よりも前に開始する必要がある場合は、明示的な呼び出しを行うことを検討してください`IKeyManager.CreateNewKey`に新しいキーの作成を強制します。</span><span class="sxs-lookup"><span data-stu-id="02686-115">If you need these settings to kick in earlier than the next automatic key rolling time, consider making an explicit call to `IKeyManager.CreateNewKey` to force the creation of a new key.</span></span> <span data-ttu-id="02686-116">明示的なライセンス認証日を指定してください ({今すぐ + 2 日間} は、目安変更が反映されるまでの時間を許可する) と呼び出しの有効期限。</span><span class="sxs-lookup"><span data-stu-id="02686-116">Remember to provide an explicit activation date ({ now + 2 days } is a good rule of thumb to allow time for the change to propagate) and expiration date in the call.</span></span>

>[!TIP]
> <span data-ttu-id="02686-117">リポジトリと接触しているすべてのアプリケーションで、同じ設定を指定する必要があります、`IDataProtectionBuilder`拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="02686-117">All applications touching the repository should specify the same settings with the `IDataProtectionBuilder` extension methods.</span></span> <span data-ttu-id="02686-118">それ以外の場合、永続化されたキーのプロパティは、キーの生成のルーチンを呼び出す特定のアプリケーションに依存するされます。</span><span class="sxs-lookup"><span data-stu-id="02686-118">Otherwise, the properties of the persisted key will be dependent on the particular application that invoked the key generation routines.</span></span>
