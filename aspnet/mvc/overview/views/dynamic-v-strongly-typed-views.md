---
uid: mvc/overview/views/dynamic-v-strongly-typed-views
title: Dynamic v します。 ビューを厳密に型指定 |Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 01/27/2011
ms.assetid: 0cbd88da-0da6-4605-b222-2835c6478304
msc.legacyurl: /mvc/overview/views/dynamic-v-strongly-typed-views
msc.type: authoredcontent
ms.openlocfilehash: 941cb24b81721eb75a8f7150ddb17acf71287da3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37822183"
---
<a name="dynamic-v-strongly-typed-views"></a><span data-ttu-id="758fa-103">Dynamic v します。</span><span class="sxs-lookup"><span data-stu-id="758fa-103">Dynamic v.</span></span> <span data-ttu-id="758fa-104">厳密に型指定されたビュー</span><span class="sxs-lookup"><span data-stu-id="758fa-104">Strongly Typed Views</span></span>
====================
<span data-ttu-id="758fa-105">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="758fa-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

<span data-ttu-id="758fa-106">ASP.NET MVC 3 でビュー コント ローラーから情報を渡すための 3 つの方法はあります。</span><span class="sxs-lookup"><span data-stu-id="758fa-106">There are three ways to pass information from a controller to a view in ASP.NET MVC 3:</span></span>

1. <span data-ttu-id="758fa-107">厳密に型指定されたモデル オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="758fa-107">As a strongly typed model object.</span></span>
2. <span data-ttu-id="758fa-108">動的な型として (を使用して@model動的)</span><span class="sxs-lookup"><span data-stu-id="758fa-108">As a dynamic type (using @model dynamic)</span></span>
3. <span data-ttu-id="758fa-109">ViewBag を使用します。</span><span class="sxs-lookup"><span data-stu-id="758fa-109">Using the ViewBag</span></span>

<span data-ttu-id="758fa-110">動的かつ厳密に型指定されたビューを比較して単純な MVC 3 上位ブログ アプリケーション作成しました。</span><span class="sxs-lookup"><span data-stu-id="758fa-110">I've written a simple MVC 3 Top Blog application to compare and contrast dynamic and strongly typed views.</span></span> <span data-ttu-id="758fa-111">ブログの単純なリストから始まって、コント ローラー。</span><span class="sxs-lookup"><span data-stu-id="758fa-111">The controller starts out with a simple list of blogs:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample1.cs)]

<span data-ttu-id="758fa-112">IndexNotStonglyTyped() メソッド内を右クリックし、Razor ビューを追加します。</span><span class="sxs-lookup"><span data-stu-id="758fa-112">Right click in the IndexNotStonglyTyped() method and add a Razor view.</span></span>

<span data-ttu-id="758fa-113">[![8475.NotStronglyTypedView [1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="758fa-113">[![8475.NotStronglyTypedView[1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)</span></span>

<span data-ttu-id="758fa-114">確認、**厳密に型指定されたビューを作成**ボックスがオンになっていません。</span><span class="sxs-lookup"><span data-stu-id="758fa-114">Make sure the **Create a strongly-typed view** box is not checked.</span></span> <span data-ttu-id="758fa-115">結果のビューには、ほとんどが含まれていません。</span><span class="sxs-lookup"><span data-stu-id="758fa-115">The resulting view doesn't contain much:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample2.cshtml)]

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample3.cshtml)]

<span data-ttu-id="758fa-116">動的なと厳密に型指定されたビューではなくを使用するため、intellisense が役立ちます。</span><span class="sxs-lookup"><span data-stu-id="758fa-116">Because we're using a dynamic and not a strongly typed view, intellisense doesn't help us.</span></span> <span data-ttu-id="758fa-117">完成したコードは、以下に示します。</span><span class="sxs-lookup"><span data-stu-id="758fa-117">The completed code is shown below:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample4.cshtml)]

<span data-ttu-id="758fa-118">[![6646.NotStronglyTypedView_5F00_IE [1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="758fa-118">[![6646.NotStronglyTypedView_5F00_IE[1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)</span></span>

<span data-ttu-id="758fa-119">厳密に型指定されたビューを追加しましょう。</span><span class="sxs-lookup"><span data-stu-id="758fa-119">Now we'll add a strongly typed view.</span></span> <span data-ttu-id="758fa-120">コント ローラーには、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="758fa-120">Add the following code to the controller:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample5.cs)]


<span data-ttu-id="758fa-121">正確に、同じ戻り View(topBlogs); に注目してください。非厳密に型指定されたビューとして呼び出します。</span><span class="sxs-lookup"><span data-stu-id="758fa-121">Notice it's exactly the same return View(topBlogs); call as the non-strongly typed view.</span></span> <span data-ttu-id="758fa-122">内側を右クリックして*StonglyTypedIndex()* 選択**ビューの追加**します。</span><span class="sxs-lookup"><span data-stu-id="758fa-122">Right click inside of *StonglyTypedIndex()* and select **Add View**.</span></span> <span data-ttu-id="758fa-123">この時間を選択、**ブログ**モデル クラスと選択**一覧**スキャフォールディング テンプレートとして。</span><span class="sxs-lookup"><span data-stu-id="758fa-123">This time select the **Blog** Model class and select **List** as the Scaffold template.</span></span>

<span data-ttu-id="758fa-124">[![5658.StrongView [1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="758fa-124">[![5658.StrongView[1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)</span></span>

<span data-ttu-id="758fa-125">新しいビュー テンプレートの内部では、intellisense サポートを取得します。</span><span class="sxs-lookup"><span data-stu-id="758fa-125">Inside the new view template we get intellisense support.</span></span>

<span data-ttu-id="758fa-126">[![7002.intellesince [1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="758fa-126">[![7002.intellesince[1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)</span></span>

<span data-ttu-id="758fa-127">C# プロジェクトをダウンロードできます[ここ](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip)します。</span><span class="sxs-lookup"><span data-stu-id="758fa-127">The c# project can be downloaded [here](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).</span></span>
