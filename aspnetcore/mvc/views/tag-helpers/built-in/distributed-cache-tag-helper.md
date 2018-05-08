---
title: ASP.NET Core の分散キャッシュ タグ ヘルパー
author: pkellner
description: キャッシュ タグ ヘルパーを使用する方法を示します
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: 929156633048b8ee68a66290f44b12026a08c8c9
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/22/2018
---
# <a name="distributed-cache-tag-helper-in-aspnet-core"></a><span data-ttu-id="3687a-103">ASP.NET Core の分散キャッシュ タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="3687a-103">Distributed Cache Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="3687a-104">著者: [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="3687a-104">By [Peter Kellner](http://peterkellner.net)</span></span> 


<span data-ttu-id="3687a-105">分散キャッシュ タグ ヘルパーは、ASP.NET Core アプリの内容を分散キャッシュ ソースにキャッシュすることによって、アプリのパフォーマンスを大幅に改善する機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="3687a-105">The Distributed Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to a distributed cache source.</span></span>

<span data-ttu-id="3687a-106">分散キャッシュ タグ ヘルパーは、キャッシュ タグ ヘルパーと同じ基本クラスから継承されます。</span><span class="sxs-lookup"><span data-stu-id="3687a-106">The Distributed Cache Tag Helper inherits from the same base class as the Cache Tag Helper.</span></span>  <span data-ttu-id="3687a-107">キャッシュ タグ ヘルパーに関連付けられているすべての属性は、分散タグ ヘルパーでも機能します。</span><span class="sxs-lookup"><span data-stu-id="3687a-107">All attributes associated with the Cache Tag Helper will also work on the Distributed Tag Helper.</span></span>


<span data-ttu-id="3687a-108">分散キャッシュ タグ ヘルパーは、**コンストラクター インジェクション**と呼ばれる**明示的な依存関係の原則**に従います。</span><span class="sxs-lookup"><span data-stu-id="3687a-108">The Distributed Cache Tag Helper follows the **Explicit Dependencies Principle** known as **Constructor Injection**.</span></span>  <span data-ttu-id="3687a-109">具体的には、`IDistributedCache` インターフェイス コンテナーは、分散キャッシュ タグ ヘルパーのコンストラクターに渡されます。</span><span class="sxs-lookup"><span data-stu-id="3687a-109">Specifically, the `IDistributedCache` interface container is passed into the Distributed Cache Tag Helper's constructor.</span></span>  <span data-ttu-id="3687a-110">`ConfigureServices` に `IDistributedCache` の具体的な実装が作成されていない場合 (通常は startup.cs にあります)、分散キャッシュ タグ ヘルパーはキャッシュされたデータの格納に基本キャッシュ タグ ヘルパーと同じメモリ内プロバイダーを使用します。</span><span class="sxs-lookup"><span data-stu-id="3687a-110">If no specific concrete implementation of `IDistributedCache` has been created in `ConfigureServices`, usually found in startup.cs, then the Distributed Cache Tag Helper will use the same in-memory provider for storing cached data as the basic Cache Tag Helper.</span></span>

## <a name="distributed-cache-tag-helper-attributes"></a><span data-ttu-id="3687a-111">分散キャッシュ タグ ヘルパーの属性</span><span class="sxs-lookup"><span data-stu-id="3687a-111">Distributed Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled-expires-on-expires-after-expires-sliding-vary-by-header-vary-by-query-vary-by-route-vary-by-cookie-vary-by-user-vary-by-priority"></a><span data-ttu-id="3687a-112">expires-on expires-after expires-sliding vary-by-header vary-by-query vary-by-route vary-by-cookie vary-by-user vary-by priority が有効</span><span class="sxs-lookup"><span data-stu-id="3687a-112">enabled expires-on expires-after expires-sliding vary-by-header vary-by-query vary-by-route vary-by-cookie vary-by-user vary-by priority</span></span>

<span data-ttu-id="3687a-113">定義については、キャッシュ タグ ヘルパーを参照してください。</span><span class="sxs-lookup"><span data-stu-id="3687a-113">See Cache Tag Helper for definitions.</span></span> <span data-ttu-id="3687a-114">分散キャッシュ タグ ヘルパーはキャッシュ タグ ヘルパーと同じクラスから継承されるため、これらすべての属性がキャッシュ タグ ヘルパーと共通しています。</span><span class="sxs-lookup"><span data-stu-id="3687a-114">Distributed Cache Tag Helper inherits from the same class as Cache Tag Helper so all these attributes are common from Cache Tag Helper.</span></span>

- - -

### <a name="name-required"></a><span data-ttu-id="3687a-115">名前 (必須)</span><span class="sxs-lookup"><span data-stu-id="3687a-115">name (required)</span></span>

| <span data-ttu-id="3687a-116">属性の型</span><span class="sxs-lookup"><span data-stu-id="3687a-116">Attribute Type</span></span>    | <span data-ttu-id="3687a-117">値の例</span><span class="sxs-lookup"><span data-stu-id="3687a-117">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="3687a-118">string</span><span class="sxs-lookup"><span data-stu-id="3687a-118">string</span></span>    | <span data-ttu-id="3687a-119">"my-distributed-cache-unique-key-101"</span><span class="sxs-lookup"><span data-stu-id="3687a-119">"my-distributed-cache-unique-key-101"</span></span>     |

<span data-ttu-id="3687a-120">必須の `name` 属性は、分散キャッシュ タグ ヘルパーのインスタンスごとに保存されているキャッシュのキーとして使用されます。</span><span class="sxs-lookup"><span data-stu-id="3687a-120">The required `name` attribute is used as a key to that cache stored for each instance of a Distributed Cache Tag Helper.</span></span>  <span data-ttu-id="3687a-121">基本キャッシュ タグ ヘルパーは Razor ページ名と、Razor ページのタグ ヘルパーの場所に基づいて各キャッシュ タグ ヘルパー インスタンスにキーを割り当てますが、分散キャッシュ タグ ヘルパーのキーが基準とするのは属性 `name` のみです</span><span class="sxs-lookup"><span data-stu-id="3687a-121">Unlike the basic Cache Tag Helper that assigns a key to each Cache Tag Helper instance based on the Razor page name and location of the tag helper in the razor page, the Distributed Cache Tag Helper only bases it's key on the attribute `name`</span></span>

<span data-ttu-id="3687a-122">使用例:</span><span class="sxs-lookup"><span data-stu-id="3687a-122">Usage Example:</span></span>

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a><span data-ttu-id="3687a-123">分散キャッシュ タグ ヘルパー IDistributedCache の実装</span><span class="sxs-lookup"><span data-stu-id="3687a-123">Distributed Cache Tag Helper IDistributedCache Implementations</span></span>

<span data-ttu-id="3687a-124">ASP.NET Core には `IDistributedCache` の 2 つの実装が組み込まれています。</span><span class="sxs-lookup"><span data-stu-id="3687a-124">There are two implementations of `IDistributedCache` built in to ASP.NET Core.</span></span>  <span data-ttu-id="3687a-125">1 つは **SQL Server** に基づき、もう 1 つは **Redis** に基づきます。</span><span class="sxs-lookup"><span data-stu-id="3687a-125">One is based on **Sql Server** and the other is based on **Redis**.</span></span> <span data-ttu-id="3687a-126">これらの実装の詳細については、下で参照されている "分散キャッシュの使用" のリソースで確認できます。</span><span class="sxs-lookup"><span data-stu-id="3687a-126">Details of these implementations can be found at the resource referenced below named "Working with a distributed cache".</span></span> <span data-ttu-id="3687a-127">どちらの実装も、ASP.NET Core の **startup.cs** で `IDistributedCache` のインスタンスの設定を伴います。</span><span class="sxs-lookup"><span data-stu-id="3687a-127">Both implementations involve setting an instance of `IDistributedCache` in ASP.NET Core's **startup.cs**.</span></span>

<span data-ttu-id="3687a-128">`IDistributedCache` のいずれかの具体的な実装の使用に、明確に関連付けられている属性はありません。</span><span class="sxs-lookup"><span data-stu-id="3687a-128">There are no tag attributes specifically associated with using any specific implementation of `IDistributedCache`.</span></span>



- - -



## <a name="additional-resources"></a><span data-ttu-id="3687a-129">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="3687a-129">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
