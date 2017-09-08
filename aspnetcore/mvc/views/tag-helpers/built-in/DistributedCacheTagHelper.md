---
title: "キャッシュ タグ ヘルパーの分散 |Microsoft ドキュメント"
author: pkellner
description: "キャッシュ タグ ヘルパーを使用する方法を示しています。"
keywords: "ASP.NET Core、タグ ヘルパー"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: c045d485-d1dc-4cea-a675-46be83b7a022
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/DistributedCacheTagHelper
ms.openlocfilehash: b6e0beca0833b1dbe0843e8f8848b976726cc7b0
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/11/2017
---
# <a name="distributed-cache-tag-helper"></a><span data-ttu-id="c28f6-104">分散キャッシュ タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="c28f6-104">Distributed Cache Tag Helper</span></span>

<span data-ttu-id="c28f6-105">によって[Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="c28f6-105">By [Peter Kellner](http://peterkellner.net)</span></span> 


<span data-ttu-id="c28f6-106">分散キャッシュ タグ ヘルパーは、分散キャッシュ ソースにそのコンテンツをキャッシュすることによって、ASP.NET Core アプリケーションのパフォーマンスを大幅に改善する機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="c28f6-106">The Distributed Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to a distributed cache source.</span></span>

<span data-ttu-id="c28f6-107">分散キャッシュ タグ ヘルパーは、キャッシュ タグ ヘルパーと同じ基本クラスから継承します。</span><span class="sxs-lookup"><span data-stu-id="c28f6-107">The Distributed Cache Tag Helper inherits from the same base class as the Cache Tag Helper.</span></span>  <span data-ttu-id="c28f6-108">キャッシュ タグ ヘルパーに関連付けられているすべての属性は、分散タグ ヘルパーでも機能します。</span><span class="sxs-lookup"><span data-stu-id="c28f6-108">All attributes associated with the Cache Tag Helper will also work on the Distributed Tag Helper.</span></span>


<span data-ttu-id="c28f6-109">分散キャッシュ タグ ヘルパーの後、**明示的な依存関係の原則**と呼ばれる**コンス トラクター インジェクション**です。</span><span class="sxs-lookup"><span data-stu-id="c28f6-109">The Distributed Cache Tag Helper follows the **Explicit Dependencies Principle** known as **Constructor Injection**.</span></span>  <span data-ttu-id="c28f6-110">具体的には、`IDistributedCache`インターフェイス コンテナーは、分散キャッシュ タグ ヘルパーのコンス トラクターに渡されます。</span><span class="sxs-lookup"><span data-stu-id="c28f6-110">Specifically, the `IDistributedCache` interface container is passed into the Distributed Cache Tag Helper's constructor.</span></span>  <span data-ttu-id="c28f6-111">場合のない特定の具象実装`IDistributedCache`内で作成された`ConfigureServices`、通常、startup.cs の分散キャッシュ タグ ヘルパーは基本のキャッシュ タグ ヘルパーとしてキャッシュされたデータを格納するため同じメモリ内のプロバイダーを使用します。</span><span class="sxs-lookup"><span data-stu-id="c28f6-111">If no specific concrete implementation of `IDistributedCache` has been created in `ConfigureServices`, usually found in startup.cs, then the Distributed Cache Tag Helper will use the same in-memory provider for storing cached data as the basic Cache Tag Helper.</span></span>

## <a name="distributed-cache-tag-helper-attributes"></a><span data-ttu-id="c28f6-112">分散キャッシュ タグ ヘルパー属性</span><span class="sxs-lookup"><span data-stu-id="c28f6-112">Distributed Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled-expires-on-expires-after-expires-sliding-vary-by-header-vary-by-query-vary-by-route-vary-by-cookie-vary-by-user-vary-by-priority"></a><span data-ttu-id="c28f6-113">有効期限が切れるの有効期限が切れた後に有効期限が切れる-スライディング有効になっているヘッダーの異なるクエリによって異なりますルートによって異なります cookie の異なるユーザーによって異なる別の優先順位</span><span class="sxs-lookup"><span data-stu-id="c28f6-113">enabled expires-on expires-after expires-sliding vary-by-header vary-by-query vary-by-route vary-by-cookie vary-by-user vary-by priority</span></span>

<span data-ttu-id="c28f6-114">定義については、タグ ヘルパーのキャッシュを参照してください。</span><span class="sxs-lookup"><span data-stu-id="c28f6-114">See Cache Tag Helper for definitions.</span></span> <span data-ttu-id="c28f6-115">分散キャッシュ タグ ヘルパーは、これらすべての属性はキャッシュ タグ ヘルパーから一般的なので、キャッシュ タグ ヘルパーと同じクラスから継承します。</span><span class="sxs-lookup"><span data-stu-id="c28f6-115">Distributed Cache Tag Helper inherits from the same class as Cache Tag Helper so all these attributes are common from Cache Tag Helper.</span></span>

- - -

### <a name="name-required"></a><span data-ttu-id="c28f6-116">名前 (必須)</span><span class="sxs-lookup"><span data-stu-id="c28f6-116">name (required)</span></span>

| <span data-ttu-id="c28f6-117">属性の型</span><span class="sxs-lookup"><span data-stu-id="c28f6-117">Attribute Type</span></span>    | <span data-ttu-id="c28f6-118">値の例</span><span class="sxs-lookup"><span data-stu-id="c28f6-118">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="c28f6-119">string</span><span class="sxs-lookup"><span data-stu-id="c28f6-119">string</span></span>    | <span data-ttu-id="c28f6-120">"my-distributed-cache-unique-key-101"</span><span class="sxs-lookup"><span data-stu-id="c28f6-120">"my-distributed-cache-unique-key-101"</span></span>     |

<span data-ttu-id="c28f6-121">必要な`name`属性は、分散キャッシュ タグ ヘルパーのインスタンスごとに格納されているキャッシュへのキーとして使用します。</span><span class="sxs-lookup"><span data-stu-id="c28f6-121">The required `name` attribute is used as a key to that cache stored for each instance of a Distributed Cache Tag Helper.</span></span>  <span data-ttu-id="c28f6-122">Razor ページで、タグ ヘルパーの場所と Razor ページ名に基づいて各タグ ヘルパーのキャッシュ インスタンスをキーを代入する基本的なキャッシュ タグ ヘルパーとは異なり、分散キャッシュ タグ ヘルパーのみに基づいて、キーの属性`name`</span><span class="sxs-lookup"><span data-stu-id="c28f6-122">Unlike the basic Cache Tag Helper that assigns a key to each Cache Tag Helper instance based on the Razor page name and location of the tag helper in the razor page, the Distributed Cache Tag Helper only bases it's key on the attribute `name`</span></span>

<span data-ttu-id="c28f6-123">使用例:</span><span class="sxs-lookup"><span data-stu-id="c28f6-123">Usage Example:</span></span>

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a><span data-ttu-id="c28f6-124">キャッシュ タグ ヘルパー IDistributedCache 実装の分散</span><span class="sxs-lookup"><span data-stu-id="c28f6-124">Distributed Cache Tag Helper IDistributedCache Implementations</span></span>

<span data-ttu-id="c28f6-125">2 つの実装がある`IDistributedCache`ASP.NET Core に組み込まれています。</span><span class="sxs-lookup"><span data-stu-id="c28f6-125">There are two implementations of `IDistributedCache` built in to ASP.NET Core.</span></span>  <span data-ttu-id="c28f6-126">いずれかに基づきます**Sql Server** 、もう一方に基づいて**Redis**です。</span><span class="sxs-lookup"><span data-stu-id="c28f6-126">One is based on **Sql Server** and the other is based on **Redis**.</span></span> <span data-ttu-id="c28f6-127">これらの実装の詳細については、名前付き「での作業を分散キャッシュ」の下で参照されているリソースで確認できます。</span><span class="sxs-lookup"><span data-stu-id="c28f6-127">Details of these implementations can be found at the resource referenced below named "Working with a distributed cache".</span></span> <span data-ttu-id="c28f6-128">どちらの実装では、インスタンスの設定が関係`IDistributedCache`ASP.NET Core で**startup.cs**です。</span><span class="sxs-lookup"><span data-stu-id="c28f6-128">Both implementations involve setting an instance of `IDistributedCache` in ASP.NET Core's **startup.cs**.</span></span>

<span data-ttu-id="c28f6-129">あるタグの属性具体的に関連付けられたの特定の実装を使用して`IDistributedCache`です。</span><span class="sxs-lookup"><span data-stu-id="c28f6-129">There no tag attributes specifically associated with using any specific implementation of `IDistributedCache`.</span></span>



- - -



## <a name="additional-resources"></a><span data-ttu-id="c28f6-130">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="c28f6-130">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/builtin-th/CacheTagHelper>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>