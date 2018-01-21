---
title: "キーの不変性と設定を変更します。"
author: rick-anderson
description: "このドキュメントでは、ASP.NET Core データ保護キー不変 Api の実装の詳細について説明します。"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: 8e46e634266fa5f082c47f3be306009eb54bcbcc
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="key-immutability-and-changing-settings"></a><span data-ttu-id="deba6-103">キーの不変性と設定を変更します。</span><span class="sxs-lookup"><span data-stu-id="deba6-103">Key Immutability and changing settings</span></span>

<span data-ttu-id="deba6-104">オブジェクトは、バッキング ストアに永続化した後、形式は永久に固定されます。</span><span class="sxs-lookup"><span data-stu-id="deba6-104">Once an object is persisted to the backing store, its representation is forever fixed.</span></span> <span data-ttu-id="deba6-105">バッキング ストアに新しいデータを追加することができますが、既存のデータを変更しないことができます。</span><span class="sxs-lookup"><span data-stu-id="deba6-105">New data can be added to the backing store, but existing data can never be mutated.</span></span> <span data-ttu-id="deba6-106">この動作の主な目的は、データの破損を防ぐためです。</span><span class="sxs-lookup"><span data-stu-id="deba6-106">The primary purpose of this behavior is to prevent data corruption.</span></span>

<span data-ttu-id="deba6-107">この動作の 1 つの結果は、こと、キーは、バッキング ストアに書き込まれますが後、は変更できません。</span><span class="sxs-lookup"><span data-stu-id="deba6-107">One consequence of this behavior is that once a key is written to the backing store, it is immutable.</span></span> <span data-ttu-id="deba6-108">作成、アクティブ化、および有効期限の日付は変更不可能を使用して取り消すことができますが、`IKeyManager`です。</span><span class="sxs-lookup"><span data-stu-id="deba6-108">Its creation, activation, and expiration dates can never be changed, though it can revoked by using `IKeyManager`.</span></span> <span data-ttu-id="deba6-109">さらに、その基になるアルゴリズム情報、マスター_キー生成情報、および rest プロパティでの暗号化も変更できません。</span><span class="sxs-lookup"><span data-stu-id="deba6-109">Additionally, its underlying algorithmic information, master keying material, and encryption at rest properties are also immutable.</span></span>

<span data-ttu-id="deba6-110">場合は、開発者は、キーの永続性に影響する設定を変更、それらの変更にならない効果、キーを生成するのいずれかに明示的な呼び出しによって次の時間まで`IKeyManager.CreateNewKey`またはシステム経由で、データ保護の独自[自動キー生成](key-management.md#data-protection-implementation-key-management)動作します。</span><span class="sxs-lookup"><span data-stu-id="deba6-110">If the developer changes any setting that affects key persistence, those changes will not go into effect until the next time a key is generated, either via an explicit call to `IKeyManager.CreateNewKey` or via the data protection system's own [automatic key generation](key-management.md#data-protection-implementation-key-management) behavior.</span></span> <span data-ttu-id="deba6-111">キーの永続性に影響する設定は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="deba6-111">The settings that affect key persistence are as follows:</span></span>

* [<span data-ttu-id="deba6-112">既定キーの有効期間</span><span class="sxs-lookup"><span data-stu-id="deba6-112">The default key lifetime</span></span>](key-management.md#data-protection-implementation-key-management)

* [<span data-ttu-id="deba6-113">Rest メカニズムでキーの暗号化</span><span class="sxs-lookup"><span data-stu-id="deba6-113">The key encryption at rest mechanism</span></span>](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest)

* [<span data-ttu-id="deba6-114">キーに含まれるアルゴリズム情報</span><span class="sxs-lookup"><span data-stu-id="deba6-114">The algorithmic information contained within the key</span></span>](xref:security/data-protection/configuration/overview#changing-algorithms-with-usecryptographicalgorithms)

<span data-ttu-id="deba6-115">をこれらの設定、次の自動キーのローリング時間よりも前に開始する必要がある場合は、明示的な呼び出しを行うことを検討してください`IKeyManager.CreateNewKey`に新しいキーの作成を強制します。</span><span class="sxs-lookup"><span data-stu-id="deba6-115">If you need these settings to kick in earlier than the next automatic key rolling time, consider making an explicit call to `IKeyManager.CreateNewKey` to force the creation of a new key.</span></span> <span data-ttu-id="deba6-116">明示的なライセンス認証日を指定してください ({今すぐ + 2 日間} は、目安変更が反映されるまでの時間を許可する) と呼び出しの有効期限。</span><span class="sxs-lookup"><span data-stu-id="deba6-116">Remember to provide an explicit activation date ({ now + 2 days } is a good rule of thumb to allow time for the change to propagate) and expiration date in the call.</span></span>

>[!TIP]
> <span data-ttu-id="deba6-117">リポジトリと接触しているすべてのアプリケーションで、同じ設定を指定する必要があります、`IDataProtectionBuilder`拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="deba6-117">All applications touching the repository should specify the same settings with the `IDataProtectionBuilder` extension methods.</span></span> <span data-ttu-id="deba6-118">それ以外の場合、永続化されたキーのプロパティは、キーの生成のルーチンを呼び出す特定のアプリケーションに依存するされます。</span><span class="sxs-lookup"><span data-stu-id="deba6-118">Otherwise, the properties of the persisted key will be dependent on the particular application that invoked the key generation routines.</span></span>
