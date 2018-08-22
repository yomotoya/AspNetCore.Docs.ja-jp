---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
title: ASP.NET Web API OData 5.3 の新機能新機能 |Microsoft Docs
author: microsoft
description: ''
ms.author: riande
ms.date: 09/16/2014
ms.assetid: e39eaa25-83ff-41dc-869d-3818d59a88ae
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
msc.type: authoredcontent
ms.openlocfilehash: 20ef263ce7cbaef40c16937eaa91d34239fc2ace
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829289"
---
<a name="whats-new-in-aspnet-web-api-odata-53"></a><span data-ttu-id="d6c2d-102">ASP.NET Web API OData 5.3 の新機能新機能</span><span class="sxs-lookup"><span data-stu-id="d6c2d-102">What's New in ASP.NET Web API OData 5.3</span></span>
====================
<span data-ttu-id="d6c2d-103">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="d6c2d-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="d6c2d-104">このトピックでは、ASP.NET Web API OData 5.3 の新機能新機能について説明します。</span><span class="sxs-lookup"><span data-stu-id="d6c2d-104">This topic describes what's new for ASP.NET Web API OData 5.3.</span></span>

- [<span data-ttu-id="d6c2d-105">ダウンロード</span><span class="sxs-lookup"><span data-stu-id="d6c2d-105">Download</span></span>](#download)
- [<span data-ttu-id="d6c2d-106">ドキュメント</span><span class="sxs-lookup"><span data-stu-id="d6c2d-106">Documentation</span></span>](#documentation)
- [<span data-ttu-id="d6c2d-107">OData のコア ライブラリ</span><span class="sxs-lookup"><span data-stu-id="d6c2d-107">OData Core Libraries</span></span>](#corelib)
- [<span data-ttu-id="d6c2d-108">新機能</span><span class="sxs-lookup"><span data-stu-id="d6c2d-108">New Features</span></span>](#newf)
- [<span data-ttu-id="d6c2d-109">既知の問題と重大な変更</span><span class="sxs-lookup"><span data-stu-id="d6c2d-109">Known Issues and Breaking Changes</span></span>](#known-issues)
- [<span data-ttu-id="d6c2d-110">バグの修正</span><span class="sxs-lookup"><span data-stu-id="d6c2d-110">Bug Fixes</span></span>](#bug-fixes)
- [<span data-ttu-id="d6c2d-111">ASP.NET Web API OData 5.3.1</span><span class="sxs-lookup"><span data-stu-id="d6c2d-111">ASP.NET Web API OData 5.3.1</span></span>](#OD)

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="d6c2d-112">ダウンロード</span><span class="sxs-lookup"><span data-stu-id="d6c2d-112">Download</span></span>

<span data-ttu-id="d6c2d-113">ランタイムの機能は、NuGet ギャラリーでの NuGet パッケージとしてリリースされます。</span><span class="sxs-lookup"><span data-stu-id="d6c2d-113">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="d6c2d-114">インストールまたは NuGet パッケージ マネージャー コンソールを使用して、リリースされた NuGet パッケージを更新できます。</span><span class="sxs-lookup"><span data-stu-id="d6c2d-114">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="d6c2d-115">ドキュメント</span><span class="sxs-lookup"><span data-stu-id="d6c2d-115">Documentation</span></span>

<span data-ttu-id="d6c2d-116">チュートリアルなどで ASP.NET Web API OData の詳細については、ドキュメントを見つけることができます、 [ASP.NET web サイト](../odata-support-in-aspnet-web-api/index.md)します。</span><span class="sxs-lookup"><span data-stu-id="d6c2d-116">You can find tutorials and other documentation about ASP.NET Web API OData at the [ASP.NET web site](../odata-support-in-aspnet-web-api/index.md).</span></span>

<a id="corelib"></a>
## <a name="odata-core-libraries"></a><span data-ttu-id="d6c2d-117">OData のコア ライブラリ</span><span class="sxs-lookup"><span data-stu-id="d6c2d-117">OData Core Libraries</span></span>

<span data-ttu-id="d6c2d-118">OData v4 の Web API では、ODataLib バージョン 6.5.0</span><span class="sxs-lookup"><span data-stu-id="d6c2d-118">For OData v4, Web API now uses ODataLib version 6.5.0</span></span>

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-odata-53"></a><span data-ttu-id="d6c2d-119">ASP.NET の新機能の Web API OData 5.3</span><span class="sxs-lookup"><span data-stu-id="d6c2d-119">New Features in ASP.NET Web API OData 5.3</span></span>

### <a name="support-for-levels-in-expand"></a><span data-ttu-id="d6c2d-120">$ で $levels 展開のサポートします。</span><span class="sxs-lookup"><span data-stu-id="d6c2d-120">Support for $levels in $expand</span></span>

<span data-ttu-id="d6c2d-121">$Levels を使用するクエリが $ でオプションがクエリを展開します。</span><span class="sxs-lookup"><span data-stu-id="d6c2d-121">You can use the $levels query option in $expand queries.</span></span> <span data-ttu-id="d6c2d-122">例えば:</span><span class="sxs-lookup"><span data-stu-id="d6c2d-122">For example:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample2.cmd)]

<span data-ttu-id="d6c2d-123">このクエリと同等です。</span><span class="sxs-lookup"><span data-stu-id="d6c2d-123">This query is equivalent to:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample3.cmd)]

<a id="open-entity-types"></a>
### <a name="support-for-open-entity-types"></a><span data-ttu-id="d6c2d-124">オープンなエンティティ型のサポート</span><span class="sxs-lookup"><span data-stu-id="d6c2d-124">Support for Open Entity Types</span></span>

<span data-ttu-id="d6c2d-125">*オープン型*は型定義で宣言されている任意のプロパティだけでなく、動的プロパティを含む stuctured 型です。</span><span class="sxs-lookup"><span data-stu-id="d6c2d-125">An *open type* is a stuctured type that contains dynamic properties, in addition to any properties that are declared in the type definition.</span></span> <span data-ttu-id="d6c2d-126">オープン型では、データ モデルに柔軟性を追加できます。</span><span class="sxs-lookup"><span data-stu-id="d6c2d-126">Open types let you add flexibility to your data models.</span></span> <span data-ttu-id="d6c2d-127">詳細については、xxxx を参照してください。</span><span class="sxs-lookup"><span data-stu-id="d6c2d-127">For more information, see xxxx.</span></span>

### <a name="support-for-dynamic-collection-properties-in-open-types"></a><span data-ttu-id="d6c2d-128">オープン型での動的コレクションのプロパティのサポート</span><span class="sxs-lookup"><span data-stu-id="d6c2d-128">Support for dynamic collection properties in open types</span></span>

<span data-ttu-id="d6c2d-129">以前は、動的プロパティは、1 つの値を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d6c2d-129">Previously, a dynamic property had to be a single value.</span></span> <span data-ttu-id="d6c2d-130">5.3、動的プロパティはコレクションの値を持つことができます。</span><span class="sxs-lookup"><span data-stu-id="d6c2d-130">In 5.3, dynamic properties can have collection values.</span></span> <span data-ttu-id="d6c2d-131">たとえば、次の JSON ペイロードで、`Emails`プロパティは動的なプロパティで、文字列型のコレクションです。</span><span class="sxs-lookup"><span data-stu-id="d6c2d-131">For example, in the following JSON payload, the `Emails` property is a dynamic property and is of collection of string type:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample4.cmd)]

### <a name="support-for-inheritance-for-complex-types"></a><span data-ttu-id="d6c2d-132">複合型の継承のサポート</span><span class="sxs-lookup"><span data-stu-id="d6c2d-132">Support for inheritance for complex types</span></span>

<span data-ttu-id="d6c2d-133">これで複合型は、基本型から継承できます。</span><span class="sxs-lookup"><span data-stu-id="d6c2d-133">Now complex types can inherit from a base type.</span></span> <span data-ttu-id="d6c2d-134">たとえば、OData サービスでは、次の複合型を定義できます。</span><span class="sxs-lookup"><span data-stu-id="d6c2d-134">For example, an OData service could define the following complex types:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample5.cs)]

<span data-ttu-id="d6c2d-135">この例では、EDM を次に示します。</span><span class="sxs-lookup"><span data-stu-id="d6c2d-135">Here is the EDM for this example:</span></span>

[!code-xml[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample6.xml?highlight=8,15)]

<span data-ttu-id="d6c2d-136">詳細については、次を参照してください。 [OData の複合型継承サンプル](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt)します。</span><span class="sxs-lookup"><span data-stu-id="d6c2d-136">For more information, see [OData Complex Type Inheritance Sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span></span>

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="d6c2d-137">既知の問題と重大な変更</span><span class="sxs-lookup"><span data-stu-id="d6c2d-137">Known Issues and Breaking Changes</span></span>

<span data-ttu-id="d6c2d-138">このセクションでは、既知の問題と ASP.NET Web API OData 5.3 における重大な変更について説明します。</span><span class="sxs-lookup"><span data-stu-id="d6c2d-138">This section describes known issues and breaking changes in the ASP.NET Web API OData 5.3.</span></span>

### <a name="odata-v4"></a><span data-ttu-id="d6c2d-139">OData v4</span><span class="sxs-lookup"><span data-stu-id="d6c2d-139">OData v4</span></span>

#### <a name="query-options"></a><span data-ttu-id="d6c2d-140">Query Options (クエリ オプション)</span><span class="sxs-lookup"><span data-stu-id="d6c2d-140">Query Options</span></span>

<span data-ttu-id="d6c2d-141">問題点: $levels と入れ子になった $ を使用して展開し、正しくない展開の深さの最大の結果を = です。</span><span class="sxs-lookup"><span data-stu-id="d6c2d-141">Issue: Using nested $expand with $levels=max results in an incorrect expansion depth.</span></span>

<span data-ttu-id="d6c2d-142">たとえば、次の要求を考えてみます。</span><span class="sxs-lookup"><span data-stu-id="d6c2d-142">For example, given the following request:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample7.cmd)]

<span data-ttu-id="d6c2d-143">場合`MaxExpansionDepth`は 5、6 の展開の深さでこのクエリになります。</span><span class="sxs-lookup"><span data-stu-id="d6c2d-143">If `MaxExpansionDepth` is 5, this query would result in an expansion depth of 6.</span></span>

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a><span data-ttu-id="d6c2d-144">バグの修正と軽微な機能の更新</span><span class="sxs-lookup"><span data-stu-id="d6c2d-144">Bug Fixes and Minor Feature Updates</span></span>

<span data-ttu-id="d6c2d-145">このリリースではいくつかのバグ修正と軽微な機能も更新します。</span><span class="sxs-lookup"><span data-stu-id="d6c2d-145">This release also includes several bug fixes and minor feature updates.</span></span> <span data-ttu-id="d6c2d-146">完全な一覧をここにあります。</span><span class="sxs-lookup"><span data-stu-id="d6c2d-146">You can find the complete list here:</span></span>

- [<span data-ttu-id="d6c2d-147">バグ修正</span><span class="sxs-lookup"><span data-stu-id="d6c2d-147">Bug fixes</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.3%20Beta&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="OD"></a>
## <a name="aspnet-web-api-odata-531"></a><span data-ttu-id="d6c2d-148">ASP.NET Web API OData 5.3.1</span><span class="sxs-lookup"><span data-stu-id="d6c2d-148">ASP.NET Web API OData 5.3.1</span></span>

<span data-ttu-id="d6c2d-149">このリリースで行った、[バグを修正](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.3.1%20Beta&amp;assignedTo=All&amp;component=Web%20API%20OData&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=All)AllowedFunctions 列挙型の一部にします。</span><span class="sxs-lookup"><span data-stu-id="d6c2d-149">In this release we made a [bug fix](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.3.1%20Beta&amp;assignedTo=All&amp;component=Web%20API%20OData&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=All) to some of the AllowedFunctions enums.</span></span> <span data-ttu-id="d6c2d-150">このリリースには、その他のバグ修正や新機能が含まれていません。</span><span class="sxs-lookup"><span data-stu-id="d6c2d-150">This release doesn't have any other bug fixes or new features.</span></span>
