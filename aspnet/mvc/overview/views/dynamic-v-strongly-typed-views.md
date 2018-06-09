---
uid: mvc/overview/views/dynamic-v-strongly-typed-views
title: Dynamic v します。 ビューを厳密に型指定 |Microsoft ドキュメント
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2011
ms.topic: article
ms.assetid: 0cbd88da-0da6-4605-b222-2835c6478304
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/views/dynamic-v-strongly-typed-views
msc.type: authoredcontent
ms.openlocfilehash: 8a96d43e04a0a50d5176c10c26aa49918a0e56ef
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "26504091"
---
<a name="dynamic-v-strongly-typed-views"></a><span data-ttu-id="8ff95-103">Dynamic v します。</span><span class="sxs-lookup"><span data-stu-id="8ff95-103">Dynamic v.</span></span> <span data-ttu-id="8ff95-104">厳密に型指定されたビュー</span><span class="sxs-lookup"><span data-stu-id="8ff95-104">Strongly Typed Views</span></span>
====================
<span data-ttu-id="8ff95-105">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="8ff95-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

<span data-ttu-id="8ff95-106">ASP.NET MVC 3 でビューに、コント ローラーから情報を渡すための 3 つの方法があります。</span><span class="sxs-lookup"><span data-stu-id="8ff95-106">There are three ways to pass information from a controller to a view in ASP.NET MVC 3:</span></span>

1. <span data-ttu-id="8ff95-107">として厳密に型指定されたモデル オブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="8ff95-107">As a strongly typed model object.</span></span>
2. <span data-ttu-id="8ff95-108">動的な型として (を使用して@model動的)</span><span class="sxs-lookup"><span data-stu-id="8ff95-108">As a dynamic type (using @model dynamic)</span></span>
3. <span data-ttu-id="8ff95-109">ViewBag を使用します。</span><span class="sxs-lookup"><span data-stu-id="8ff95-109">Using the ViewBag</span></span>

<span data-ttu-id="8ff95-110">比較検討して動的および厳密に型指定されたビューの単純な MVC 3 上位ブログ アプリケーションを作成しました。</span><span class="sxs-lookup"><span data-stu-id="8ff95-110">I've written a simple MVC 3 Top Blog application to compare and contrast dynamic and strongly typed views.</span></span> <span data-ttu-id="8ff95-111">ブログの単純なリストから始まり、コント ローラー。</span><span class="sxs-lookup"><span data-stu-id="8ff95-111">The controller starts out with a simple list of blogs:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample1.cs)]

<span data-ttu-id="8ff95-112">IndexNotStonglyTyped() メソッドを右クリックし、Razor ビューを追加します。</span><span class="sxs-lookup"><span data-stu-id="8ff95-112">Right click in the IndexNotStonglyTyped() method and add a Razor view.</span></span>

<span data-ttu-id="8ff95-113">[![8475.NotStronglyTypedView [1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8ff95-113">[![8475.NotStronglyTypedView[1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)</span></span>

<span data-ttu-id="8ff95-114">確認してください、**厳密に型指定されたビューを作成する**ボックスをオフになっています。</span><span class="sxs-lookup"><span data-stu-id="8ff95-114">Make sure the **Create a strongly-typed view** box is not checked.</span></span> <span data-ttu-id="8ff95-115">結果のビューには、大部分が含まれていません。</span><span class="sxs-lookup"><span data-stu-id="8ff95-115">The resulting view doesn't contain much:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample2.cshtml)]

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample3.cshtml)]

<span data-ttu-id="8ff95-116">動的なと厳密に型指定されたビューではなくを使用しているため intellisense しないにご協力します。</span><span class="sxs-lookup"><span data-stu-id="8ff95-116">Because we're using a dynamic and not a strongly typed view, intellisense doesn't help us.</span></span> <span data-ttu-id="8ff95-117">完成したコードは、次に示します。</span><span class="sxs-lookup"><span data-stu-id="8ff95-117">The completed code is shown below:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample4.cshtml)]

<span data-ttu-id="8ff95-118">[![6646.NotStronglyTypedView_5F00_IE [1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="8ff95-118">[![6646.NotStronglyTypedView_5F00_IE[1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)</span></span>

<span data-ttu-id="8ff95-119">厳密に型指定されたビューを追加します。</span><span class="sxs-lookup"><span data-stu-id="8ff95-119">Now we'll add a strongly typed view.</span></span> <span data-ttu-id="8ff95-120">コント ローラーに次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="8ff95-120">Add the following code to the controller:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample5.cs)]


<span data-ttu-id="8ff95-121">正確に、同じ戻り View(topBlogs); であることを確認します。非厳密に型指定されたビューを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="8ff95-121">Notice it's exactly the same return View(topBlogs); call as the non-strongly typed view.</span></span> <span data-ttu-id="8ff95-122">内側を右クリックして*StonglyTypedIndex()* 選択**ビューの追加**です。</span><span class="sxs-lookup"><span data-stu-id="8ff95-122">Right click inside of *StonglyTypedIndex()* and select **Add View**.</span></span> <span data-ttu-id="8ff95-123">この時間を選択、**ブログ**モデル クラスを選択**リスト**Scaffold テンプレートとして。</span><span class="sxs-lookup"><span data-stu-id="8ff95-123">This time select the **Blog** Model class and select **List** as the Scaffold template.</span></span>

<span data-ttu-id="8ff95-124">[![5658.StrongView [1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="8ff95-124">[![5658.StrongView[1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)</span></span>

<span data-ttu-id="8ff95-125">新しいビュー テンプレートの内部は、intellisense のサポートを取得します。</span><span class="sxs-lookup"><span data-stu-id="8ff95-125">Inside the new view template we get intellisense support.</span></span>

<span data-ttu-id="8ff95-126">[![7002.intellesince [1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="8ff95-126">[![7002.intellesince[1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)</span></span>

<span data-ttu-id="8ff95-127">C# プロジェクトをダウンロードできる[ここ](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip)です。</span><span class="sxs-lookup"><span data-stu-id="8ff95-127">The c# project can be downloaded [here](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).</span></span>
