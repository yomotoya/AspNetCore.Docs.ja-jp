---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
title: ASP.NET Web API OData 5.3 の新機能 |Microsoft ドキュメント
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/16/2014
ms.topic: article
ms.assetid: e39eaa25-83ff-41dc-869d-3818d59a88ae
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
msc.type: authoredcontent
ms.openlocfilehash: e918f86ebd813f31ad0c21566e617482fc7dd963
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
ms.locfileid: "26508111"
---
<a name="whats-new-in-aspnet-web-api-odata-53"></a><span data-ttu-id="d591a-102">ASP.NET Web API OData 5.3 の新機能</span><span class="sxs-lookup"><span data-stu-id="d591a-102">What's New in ASP.NET Web API OData 5.3</span></span>
====================
<span data-ttu-id="d591a-103">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="d591a-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="d591a-104">このトピックでは、ASP.NET Web API OData 5.3 の新機能について説明します。</span><span class="sxs-lookup"><span data-stu-id="d591a-104">This topic describes what's new for ASP.NET Web API OData 5.3.</span></span>

- [<span data-ttu-id="d591a-105">ダウンロード</span><span class="sxs-lookup"><span data-stu-id="d591a-105">Download</span></span>](#download)
- [<span data-ttu-id="d591a-106">ドキュメント</span><span class="sxs-lookup"><span data-stu-id="d591a-106">Documentation</span></span>](#documentation)
- [<span data-ttu-id="d591a-107">OData のコア ライブラリ</span><span class="sxs-lookup"><span data-stu-id="d591a-107">OData Core Libraries</span></span>](#corelib)
- [<span data-ttu-id="d591a-108">新機能</span><span class="sxs-lookup"><span data-stu-id="d591a-108">New Features</span></span>](#newf)
- [<span data-ttu-id="d591a-109">既知の問題と重大な変更</span><span class="sxs-lookup"><span data-stu-id="d591a-109">Known Issues and Breaking Changes</span></span>](#known-issues)
- [<span data-ttu-id="d591a-110">バグの修正</span><span class="sxs-lookup"><span data-stu-id="d591a-110">Bug Fixes</span></span>](#bug-fixes)
- [<span data-ttu-id="d591a-111">ASP.NET Web API OData 5.3.1</span><span class="sxs-lookup"><span data-stu-id="d591a-111">ASP.NET Web API OData 5.3.1</span></span>](#OD)

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="d591a-112">ダウンロード</span><span class="sxs-lookup"><span data-stu-id="d591a-112">Download</span></span>

<span data-ttu-id="d591a-113">ランタイムの機能は、NuGet ギャラリー上の NuGet パッケージとしてリリースされています。</span><span class="sxs-lookup"><span data-stu-id="d591a-113">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="d591a-114">インストールまたは NuGet パッケージ マネージャー コンソールを使用してリリースされた NuGet パッケージを更新できます。</span><span class="sxs-lookup"><span data-stu-id="d591a-114">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="d591a-115">ドキュメント</span><span class="sxs-lookup"><span data-stu-id="d591a-115">Documentation</span></span>

<span data-ttu-id="d591a-116">チュートリアルおよび ASP.NET Web API OData については、その他のドキュメントを見つけることができます、 [ASP.NET web サイト](../odata-support-in-aspnet-web-api/index.md)です。</span><span class="sxs-lookup"><span data-stu-id="d591a-116">You can find tutorials and other documentation about ASP.NET Web API OData at the [ASP.NET web site](../odata-support-in-aspnet-web-api/index.md).</span></span>

<a id="corelib"></a>
## <a name="odata-core-libraries"></a><span data-ttu-id="d591a-117">OData のコア ライブラリ</span><span class="sxs-lookup"><span data-stu-id="d591a-117">OData Core Libraries</span></span>

<span data-ttu-id="d591a-118">OData v4 の Web API 今すぐ使用 ODataLib バージョン 6.5.0</span><span class="sxs-lookup"><span data-stu-id="d591a-118">For OData v4, Web API now uses ODataLib version 6.5.0</span></span>

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-odata-53"></a><span data-ttu-id="d591a-119">ASP.NET で新機能の Web API OData 5.3</span><span class="sxs-lookup"><span data-stu-id="d591a-119">New Features in ASP.NET Web API OData 5.3</span></span>

### <a name="support-for-levels-in-expand"></a><span data-ttu-id="d591a-120">$Levels $a 内の展開のサポートします。</span><span class="sxs-lookup"><span data-stu-id="d591a-120">Support for $levels in $expand</span></span>

<span data-ttu-id="d591a-121">$Levels を使用するクエリが $ でオプションがクエリを展開します。</span><span class="sxs-lookup"><span data-stu-id="d591a-121">You can use the $levels query option in $expand queries.</span></span> <span data-ttu-id="d591a-122">例:</span><span class="sxs-lookup"><span data-stu-id="d591a-122">For example:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample2.cmd)]

<span data-ttu-id="d591a-123">このクエリと同じです。</span><span class="sxs-lookup"><span data-stu-id="d591a-123">This query is equivalent to:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample3.cmd)]

<a id="open-entity-types"></a>
### <a name="support-for-open-entity-types"></a><span data-ttu-id="d591a-124">開いているエンティティ型のサポート</span><span class="sxs-lookup"><span data-stu-id="d591a-124">Support for Open Entity Types</span></span>

<span data-ttu-id="d591a-125">*オープン型*種類の定義で宣言されているプロパティに加え、動的なプロパティを含む stuctured 型です。</span><span class="sxs-lookup"><span data-stu-id="d591a-125">An *open type* is a stuctured type that contains dynamic properties, in addition to any properties that are declared in the type definition.</span></span> <span data-ttu-id="d591a-126">オープン型では、データ モデルに柔軟性を追加できます。</span><span class="sxs-lookup"><span data-stu-id="d591a-126">Open types let you add flexibility to your data models.</span></span> <span data-ttu-id="d591a-127">詳細については、xxxx を参照してください。</span><span class="sxs-lookup"><span data-stu-id="d591a-127">For more information, see xxxx.</span></span>

### <a name="support-for-dynamic-collection-properties-in-open-types"></a><span data-ttu-id="d591a-128">オープン型での動的なコレクション プロパティのサポート</span><span class="sxs-lookup"><span data-stu-id="d591a-128">Support for dynamic collection properties in open types</span></span>

<span data-ttu-id="d591a-129">以前は、動的プロパティは、1 つの値を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d591a-129">Previously, a dynamic property had to be a single value.</span></span> <span data-ttu-id="d591a-130">5.3、動的なプロパティはコレクションの値を持つことができます。</span><span class="sxs-lookup"><span data-stu-id="d591a-130">In 5.3, dynamic properties can have collection values.</span></span> <span data-ttu-id="d591a-131">たとえば、次の JSON ペイロードで、`Emails`プロパティは文字列型のコレクションは、動的なプロパティ。</span><span class="sxs-lookup"><span data-stu-id="d591a-131">For example, in the following JSON payload, the `Emails` property is a dynamic property and is of collection of string type:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample4.cmd)]

### <a name="support-for-inheritance-for-complex-types"></a><span data-ttu-id="d591a-132">複合型の継承のサポート</span><span class="sxs-lookup"><span data-stu-id="d591a-132">Support for inheritance for complex types</span></span>

<span data-ttu-id="d591a-133">今すぐ複合型を基本型から継承できます。</span><span class="sxs-lookup"><span data-stu-id="d591a-133">Now complex types can inherit from a base type.</span></span> <span data-ttu-id="d591a-134">たとえば、OData サービスは、次の複合型を定義できます。</span><span class="sxs-lookup"><span data-stu-id="d591a-134">For example, an OData service could define the following complex types:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample5.cs)]

<span data-ttu-id="d591a-135">この例については、EDM を次に示します。</span><span class="sxs-lookup"><span data-stu-id="d591a-135">Here is the EDM for this example:</span></span>

[!code-xml[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample6.xml?highlight=8,15)]

<span data-ttu-id="d591a-136">詳細については、次を参照してください。[複合型の継承のサンプルの OData](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt)です。</span><span class="sxs-lookup"><span data-stu-id="d591a-136">For more information, see [OData Complex Type Inheritance Sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span></span>

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="d591a-137">既知の問題と重大な変更</span><span class="sxs-lookup"><span data-stu-id="d591a-137">Known Issues and Breaking Changes</span></span>

<span data-ttu-id="d591a-138">このセクションでは、既知の問題と、ASP.NET Web API OData バージョン 5.3 以降で重大な変更について説明します。</span><span class="sxs-lookup"><span data-stu-id="d591a-138">This section describes known issues and breaking changes in the ASP.NET Web API OData 5.3.</span></span>

### <a name="odata-v4"></a><span data-ttu-id="d591a-139">OData v4</span><span class="sxs-lookup"><span data-stu-id="d591a-139">OData v4</span></span>

#### <a name="query-options"></a><span data-ttu-id="d591a-140">Query Options (クエリ オプション)</span><span class="sxs-lookup"><span data-stu-id="d591a-140">Query Options</span></span>

<span data-ttu-id="d591a-141">問題点: 入れ子になった $ を使用する $levels と展開、不適切な展開の深さの最大の結果を = です。</span><span class="sxs-lookup"><span data-stu-id="d591a-141">Issue: Using nested $expand with $levels=max results in an incorrect expansion depth.</span></span>

<span data-ttu-id="d591a-142">たとえば、次の要求を指定します。</span><span class="sxs-lookup"><span data-stu-id="d591a-142">For example, given the following request:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample7.cmd)]

<span data-ttu-id="d591a-143">場合`MaxExpansionDepth`は 5、6 の展開の深さはこのクエリになります。</span><span class="sxs-lookup"><span data-stu-id="d591a-143">If `MaxExpansionDepth` is 5, this query would result in an expansion depth of 6.</span></span>

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a><span data-ttu-id="d591a-144">バグの修正およびマイナー機能の更新</span><span class="sxs-lookup"><span data-stu-id="d591a-144">Bug Fixes and Minor Feature Updates</span></span>

<span data-ttu-id="d591a-145">このリリースではいくつかのバグ修正とマイナー機能も更新します。</span><span class="sxs-lookup"><span data-stu-id="d591a-145">This release also includes several bug fixes and minor feature updates.</span></span> <span data-ttu-id="d591a-146">完全な一覧は、ここをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="d591a-146">You can find the complete list here:</span></span>

- [<span data-ttu-id="d591a-147">バグ修正</span><span class="sxs-lookup"><span data-stu-id="d591a-147">Bug fixes</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.3%20Beta&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="OD"></a>
## <a name="aspnet-web-api-odata-531"></a><span data-ttu-id="d591a-148">ASP.NET Web API OData 5.3.1</span><span class="sxs-lookup"><span data-stu-id="d591a-148">ASP.NET Web API OData 5.3.1</span></span>

<span data-ttu-id="d591a-149">このリリースでは行われた、[のバグ修正](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.3.1%20Beta&amp;assignedTo=All&amp;component=Web%20API%20OData&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=All)AllowedFunctions 列挙型の一部にします。</span><span class="sxs-lookup"><span data-stu-id="d591a-149">In this release we made a [bug fix](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.3.1%20Beta&amp;assignedTo=All&amp;component=Web%20API%20OData&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=All) to some of the AllowedFunctions enums.</span></span> <span data-ttu-id="d591a-150">このリリースには、他のバグの修正や新機能がありません。</span><span class="sxs-lookup"><span data-stu-id="d591a-150">This release doesn't have any other bug fixes or new features.</span></span>
