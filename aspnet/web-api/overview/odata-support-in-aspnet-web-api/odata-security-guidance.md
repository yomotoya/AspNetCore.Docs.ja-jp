---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
title: ASP.NET Web API 2 のセキュリティ ガイダンス OData |Microsoft ドキュメント
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/06/2013
ms.topic: article
ms.assetid: b91e6424-1544-4747-bd0b-d1f8418c9653
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
msc.type: authoredcontent
ms.openlocfilehash: 41b05f2a2f8247853d8358e6cc1246c8b438a6db
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="security-guidance-for-aspnet-web-api-2-odata"></a><span data-ttu-id="35924-102">ASP.NET Web API 2 のセキュリティ ガイダンス OData</span><span class="sxs-lookup"><span data-stu-id="35924-102">Security Guidance for ASP.NET Web API 2 OData</span></span>
====================
<span data-ttu-id="35924-103">作成者 [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="35924-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="35924-104">このトピックでは、OData を介してデータセットを公開するときに考慮すべきセキュリティの問題について説明します。</span><span class="sxs-lookup"><span data-stu-id="35924-104">This topic describes some of the security issues that you should consider when exposing a dataset through OData.</span></span>

## <a name="edm-security"></a><span data-ttu-id="35924-105">EDM のセキュリティ</span><span class="sxs-lookup"><span data-stu-id="35924-105">EDM Security</span></span>

<span data-ttu-id="35924-106">クエリのセマンティクスは、基になるモデル型ではなく entity data model (EDM) に基づきます。</span><span class="sxs-lookup"><span data-stu-id="35924-106">The query semantics are based on the entity data model (EDM), not the underlying model types.</span></span> <span data-ttu-id="35924-107">EDM からプロパティを除外することし、クエリに表示されません。</span><span class="sxs-lookup"><span data-stu-id="35924-107">You can exclude a property from the EDM and it will not be visible to the query.</span></span> <span data-ttu-id="35924-108">たとえば、モデルには、給与プロパティを持つ従業員タイプが含まれるとします。</span><span class="sxs-lookup"><span data-stu-id="35924-108">For example, suppose your model includes an Employee type with a Salary property.</span></span> <span data-ttu-id="35924-109">クライアントから非表示にする、EDM からこのプロパティを除外することがあります。</span><span class="sxs-lookup"><span data-stu-id="35924-109">You might want to exclude this property from the EDM to hide it from clients.</span></span>

<span data-ttu-id="35924-110">除外する 2 つの方法 EDM からプロパティ。</span><span class="sxs-lookup"><span data-stu-id="35924-110">There are two ways to exlude a property from the EDM.</span></span> <span data-ttu-id="35924-111">設定することができます、 **[IgnoreDataMember]**モデル クラスのプロパティの属性。</span><span class="sxs-lookup"><span data-stu-id="35924-111">You can set the **[IgnoreDataMember]** attribute on the property in the model class:</span></span>

[!code-csharp[Main](odata-security-guidance/samples/sample1.cs)]

<span data-ttu-id="35924-112">削除することも、プロパティ、EDM からプログラムで。</span><span class="sxs-lookup"><span data-stu-id="35924-112">You can also remove the property from the EDM programmatically:</span></span>

[!code-csharp[Main](odata-security-guidance/samples/sample2.cs)]

## <a name="query-security"></a><span data-ttu-id="35924-113">クエリのセキュリティ</span><span class="sxs-lookup"><span data-stu-id="35924-113">Query Security</span></span>

<span data-ttu-id="35924-114">悪意のある、または単純なクライアントを実行する非常に長い時間がかかるクエリを作成することがあります。</span><span class="sxs-lookup"><span data-stu-id="35924-114">A malicious or naive client may be able to construct a query that takes a very long time to execute.</span></span> <span data-ttu-id="35924-115">最悪の場合に、サービスへのアクセスをこのが中断されることができます。</span><span class="sxs-lookup"><span data-stu-id="35924-115">In the worst case this can disrupt access to your service.</span></span>

<span data-ttu-id="35924-116">**[Queryable]**属性は、アクション フィルターを解析して、検証し、クエリを適用します。</span><span class="sxs-lookup"><span data-stu-id="35924-116">The **[Queryable]** attribute is an action filter that parses, validates, and applies the query.</span></span> <span data-ttu-id="35924-117">フィルターは、クエリ オプションを LINQ 式に変換します。</span><span class="sxs-lookup"><span data-stu-id="35924-117">The filter converts the query options into a LINQ expression.</span></span> <span data-ttu-id="35924-118">OData コント ローラーを返す場合、 **IQueryable**の種類、 **IQueryable** LINQ プロバイダーは、クエリを LINQ 式に変換します。</span><span class="sxs-lookup"><span data-stu-id="35924-118">When the OData controller returns an **IQueryable** type, the **IQueryable** LINQ provider converts the LINQ expression into a query.</span></span> <span data-ttu-id="35924-119">そのため、パフォーマンスは、使用されている LINQ プロバイダーとも、データセットやデータベース スキーマの特定の特性によって異なります。</span><span class="sxs-lookup"><span data-stu-id="35924-119">Therefore, performance depends on the LINQ provider that is used, and also on the particular characteristics of your dataset or database schema.</span></span>

<span data-ttu-id="35924-120">詳細については、ASP.NET Web API での OData クエリ オプションを使用して、次を参照してください。 [OData クエリ オプションをサポートする](supporting-odata-query-options.md)です。</span><span class="sxs-lookup"><span data-stu-id="35924-120">For more information about using OData query options in ASP.NET Web API, see [Supporting OData Query Options](supporting-odata-query-options.md).</span></span>

<span data-ttu-id="35924-121">(たとえば、エンタープライズ環境で)、信頼されるすべてのクライアントであることがわかっている場合、またはデータセットが小さい場合は、クエリのパフォーマンスが問題をできないがあります。</span><span class="sxs-lookup"><span data-stu-id="35924-121">If you know that all clients are trusted (for example, in an enterprise environment), or if your dataset is small, query performance might not be an issue.</span></span> <span data-ttu-id="35924-122">それ以外の場合、次の推奨事項を検討する必要があります。</span><span class="sxs-lookup"><span data-stu-id="35924-122">Otherwise, you should consider the following recommendations.</span></span>

- <span data-ttu-id="35924-123">さまざまなクエリで、サービスをテストし、DB をプロファイルします。</span><span class="sxs-lookup"><span data-stu-id="35924-123">Test your service with various queries and profile the DB.</span></span>
- <span data-ttu-id="35924-124">大きなデータ セットを 1 つのクエリで返されないようにする、サーバー ドリブン ページングを有効にします。</span><span class="sxs-lookup"><span data-stu-id="35924-124">Enable server-driven paging, to avoid returning a large data set in one query.</span></span> <span data-ttu-id="35924-125">詳細については、次を参照してください。 [Server-Driven ページング](supporting-odata-query-options.md#server-paging)です。</span><span class="sxs-lookup"><span data-stu-id="35924-125">For more information, see [Server-Driven Paging](supporting-odata-query-options.md#server-paging).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample3.cs)]
- <span data-ttu-id="35924-126">$Filter、$orderby が必要ですか。</span><span class="sxs-lookup"><span data-stu-id="35924-126">Do you need $filter and $orderby?</span></span> <span data-ttu-id="35924-127">一部のアプリケーションは、ページング、$top、$skip を使用してクライアントを許可するが、他のクエリ オプションを無効にする可能性があります。</span><span class="sxs-lookup"><span data-stu-id="35924-127">Some applications might allow client paging, using $top and $skip, but disable the other query options.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample4.cs)]
- <span data-ttu-id="35924-128">$Orderby をクラスター化インデックスのプロパティに制限することを検討してください。</span><span class="sxs-lookup"><span data-stu-id="35924-128">Consider restricting $orderby to properties in a clustered index.</span></span> <span data-ttu-id="35924-129">クラスター化インデックスを大規模なデータの並べ替えは低速です。</span><span class="sxs-lookup"><span data-stu-id="35924-129">Sorting large data without a clustered index is slow.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample5.cs)]
- <span data-ttu-id="35924-130">ノードの最大数: **MaxNodeCount**プロパティ**[Queryable]** $filter 構文ツリー内で許可されている最大の数値ノードを設定します。</span><span class="sxs-lookup"><span data-stu-id="35924-130">Maximum node count: The **MaxNodeCount** property on **[Queryable]** sets the maximum number nodes allowed in the $filter syntax tree.</span></span> <span data-ttu-id="35924-131">既定値は 100、ですが、多数のノードをコンパイルに時間がかかることができるのでを下限の値を設定することがあります。</span><span class="sxs-lookup"><span data-stu-id="35924-131">The default value is 100, but you may want to set a lower value, because a large number of nodes can be slow to compile.</span></span> <span data-ttu-id="35924-132">これは LINQ to Objects (中級者向けの LINQ プロバイダーを使用せず、メモリ内コレクションに対して LINQ クエリなど) を使用している場合に特に当てはまります。</span><span class="sxs-lookup"><span data-stu-id="35924-132">This is particularly true if you are using LINQ to Objects (i.e., LINQ queries on a collection in memory, without the use of an intermediate LINQ provider).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample6.cs)]
- <span data-ttu-id="35924-133">遅くなることが、any() と all() と関数の無効化を検討します。</span><span class="sxs-lookup"><span data-stu-id="35924-133">Consider disabling the any() and all() functions, as these can be slow.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample7.cs)]
- <span data-ttu-id="35924-134">場合は、文字列プロパティには、大きな文字列 & #8212for 例、製品の説明またはブログ エントリ & #8212consider 文字列関数の無効化が含まれます。</span><span class="sxs-lookup"><span data-stu-id="35924-134">If any string properties contain large strings&#8212for example, a product description or a blog entry&#8212consider disabling the string functions.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample8.cs)]
- <span data-ttu-id="35924-135">ナビゲーション プロパティのフィルター処理を禁止することを検討してください。</span><span class="sxs-lookup"><span data-stu-id="35924-135">Consider disallowing filtering on navigation properties.</span></span> <span data-ttu-id="35924-136">ナビゲーション プロパティのフィルター処理結果、結合は、データベース スキーマに応じて、遅くなる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="35924-136">Filtering on navigation properties can result in a join, which might be slow, depending on your database schema.</span></span> <span data-ttu-id="35924-137">次のコードでは、ナビゲーション プロパティのフィルター処理しないようにクエリ検証コントロールを示します。</span><span class="sxs-lookup"><span data-stu-id="35924-137">The following code shows a query validator that prevents filtering on navigation properties.</span></span> <span data-ttu-id="35924-138">クエリ検証コントロールの詳細については、次を参照してください。[クエリ検証](supporting-odata-query-options.md#query-validation)です。</span><span class="sxs-lookup"><span data-stu-id="35924-138">For more information about query validators, see [Query Validation](supporting-odata-query-options.md#query-validation).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample9.cs)]
- <span data-ttu-id="35924-139">データベース用にカスタマイズされている検証コントロールを記述して $filter のクエリを制限することを検討してください。</span><span class="sxs-lookup"><span data-stu-id="35924-139">Consider restricting $filter queries by writing a validator that is customized for your database.</span></span> <span data-ttu-id="35924-140">たとえば、これら 2 つのクエリについて考えてみます。</span><span class="sxs-lookup"><span data-stu-id="35924-140">For example, consider these two queries:</span></span> 

  - <span data-ttu-id="35924-141">すべてのムービーで最後の名前 'A' で始まるアクターとします。</span><span class="sxs-lookup"><span data-stu-id="35924-141">All movies with actors whose last name starts with ‘A'.</span></span>
  - <span data-ttu-id="35924-142">1994 年にリリースされたすべてのムービーします。</span><span class="sxs-lookup"><span data-stu-id="35924-142">All movies released in 1994.</span></span>

    <span data-ttu-id="35924-143">ムービーがアクターによってインデックスが作成しない限り、最初のクエリは、ムービーの一覧全体をスキャンする DB エンジンを必要があります。</span><span class="sxs-lookup"><span data-stu-id="35924-143">Unless movies are indexed by actors, the first query might require the DB engine to scan the entire list of movies.</span></span> <span data-ttu-id="35924-144">許容可能な 2 番目のクエリでありと仮定してムービーはリリース年によってインデックスが作成されます。</span><span class="sxs-lookup"><span data-stu-id="35924-144">Whereas the second query might be acceptable, assuming movies are indexed by release year.</span></span>

    <span data-ttu-id="35924-145">次のコードでは、"ReleaseYear"と"Title"プロパティがないその他のプロパティでフィルター処理するバリデーターを示しています。</span><span class="sxs-lookup"><span data-stu-id="35924-145">The following code shows a validator that allows filtering on the "ReleaseYear" and "Title" properties but no other properties.</span></span>

    [!code-csharp[Main](odata-security-guidance/samples/sample10.cs)]
- <span data-ttu-id="35924-146">一般に、必要などの $filter 機能を検討してください。</span><span class="sxs-lookup"><span data-stu-id="35924-146">In general, consider which $filter functions you need.</span></span> <span data-ttu-id="35924-147">クライアントで必要としない $filter の完全表現力場合は、使用可能な関数を制限できます。</span><span class="sxs-lookup"><span data-stu-id="35924-147">If your clients do not need the full expressiveness of $filter, you can limit the allowed functions.</span></span>
