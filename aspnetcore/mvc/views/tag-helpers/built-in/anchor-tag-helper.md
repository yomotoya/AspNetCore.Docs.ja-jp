---
title: "ASP.NET Core のアンカー タグ ヘルパー"
author: pkellner
description: "アンカー タグ ヘルパーを使用する方法を示します"
manager: wpickett
ms.author: riande
ms.date: 12/20/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: 404fc7bc3b35114066f035e1ac28d10a8279ccbc
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="anchor-tag-helper"></a><span data-ttu-id="a2e2a-103">アンカー タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="a2e2a-103">Anchor Tag Helper</span></span>

<span data-ttu-id="a2e2a-104">著者: [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="a2e2a-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="a2e2a-105">アンカー タグ ヘルパーは、新しい属性を追加することで HTML アンカー (`<a ... ></a>`) タグを拡張します。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-105">The Anchor Tag Helper enhances the HTML anchor (`<a ... ></a>`) tag by adding new attributes.</span></span> <span data-ttu-id="a2e2a-106">(`href` タグで) 生成されるリンクは、新しい属性を使用して作成されます。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-106">The link generated (on the `href` tag) is created using the new attributes.</span></span> <span data-ttu-id="a2e2a-107">その URL には、https などの省略可能なプロトコルを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-107">That URL can include an optional protocol such as https.</span></span>

<span data-ttu-id="a2e2a-108">このドキュメントのサンプルでは、以下のスピーカー コントローラーを使用します。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-108">The speaker controller below is used in samples in this document.</span></span>

<span data-ttu-id="a2e2a-109">**SpeakerController.cs**</span><span class="sxs-lookup"><span data-stu-id="a2e2a-109">**SpeakerController.cs**</span></span> 

[!code-csharp[SpeakerController](sample/TagHelpersBuiltInAspNetCore/src/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs)]


## <a name="anchor-tag-helper-attributes"></a><span data-ttu-id="a2e2a-110">アンカー タグ ヘルパーの属性</span><span class="sxs-lookup"><span data-stu-id="a2e2a-110">Anchor Tag Helper Attributes</span></span>

### <a name="asp-controller"></a><span data-ttu-id="a2e2a-111">asp-controller</span><span class="sxs-lookup"><span data-stu-id="a2e2a-111">asp-controller</span></span>

<span data-ttu-id="a2e2a-112">`asp-controller` は URL の生成に使用されるコントローラーとの関連付けに使用されます。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-112">`asp-controller` is used to associate which controller will be used to generate the URL.</span></span> <span data-ttu-id="a2e2a-113">指定されたコントローラーが、現在のプロジェクトに存在する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-113">The controllers specified must exist in the current project.</span></span> <span data-ttu-id="a2e2a-114">次のコードでは、すべてのスピーカーが一覧表示されます。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-114">The following code lists all speakers:</span></span> 

```cshtml
<a asp-controller="Speaker" asp-action="Index">All Speakers</a>
```

<span data-ttu-id="a2e2a-115">次のようなマークアップが生成されます。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-115">The generated markup will be:</span></span>

```html
<a href="/Speaker">All Speakers</a>
```

<span data-ttu-id="a2e2a-116">`asp-controller` が指定され、`asp-action` が指定されない場合は、既定の `asp-action` が現在実行されているビューの既定のコントローラーのメソッドになります。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-116">If the `asp-controller` is specified and `asp-action` isn't, the default `asp-action` will be the default controller method of the currently executing view.</span></span> <span data-ttu-id="a2e2a-117">つまり、上記の例で、`asp-action` が除外されていて、このアンカー タグ ヘルパーが *HomeController* の `Index` ビュー (**/Home**) から生成される場合、次のようなマークアップが生成されます。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-117">That is, in the above example, if `asp-action` is left out, and this Anchor Tag Helper is generated from *HomeController*'s `Index` view (**/Home**), the generated markup will be:</span></span>

```html
<a href="/Home">All Speakers</a>
```

### <a name="asp-action"></a><span data-ttu-id="a2e2a-118">asp-action</span><span class="sxs-lookup"><span data-stu-id="a2e2a-118">asp-action</span></span>

<span data-ttu-id="a2e2a-119">`asp-action` は、生成される `href` に含まれるコントローラーのアクション メソッドの名前です。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-119">`asp-action` is the name of the action method in the controller that will be included in the generated `href`.</span></span> <span data-ttu-id="a2e2a-120">たとえば、次のコードは、スピーカーの詳細ページを示すために生成される `href` を設定します。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-120">For example, the following code set the generated `href` to point to the speaker detail page:</span></span>

```html
<a asp-controller="Speaker" asp-action="Detail">Speaker Detail</a>
```

<span data-ttu-id="a2e2a-121">次のようなマークアップが生成されます。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-121">The generated markup will be:</span></span>

```html
<a href="/Speaker/Detail">Speaker Detail</a>
```

<span data-ttu-id="a2e2a-122">`asp-controller` 属性が指定されていない場合、現在のビューを実行しているビューを呼び出す既定のコントローラーが使用されます。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-122">If no `asp-controller` attribute is specified, the default controller calling the view executing the current view will be used.</span></span>  
 
<span data-ttu-id="a2e2a-123">属性 `asp-action` が `Index` で、URL に操作が何も追加されていない場合、既定の `Index` メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-123">If the attribute `asp-action` is `Index`, then no action is appended to the URL, leading to the default `Index` method being called.</span></span> <span data-ttu-id="a2e2a-124">指定された (または既定の) 操作が、`asp-controller` で参照されるコントローラーに存在する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-124">The action specified (or defaulted), must exist in the controller referenced in `asp-controller`.</span></span>

### <a name="asp-page"></a><span data-ttu-id="a2e2a-125">asp-page</span><span class="sxs-lookup"><span data-stu-id="a2e2a-125">asp-page</span></span>

<span data-ttu-id="a2e2a-126">`asp-page` 属性をアンカー タグで使用すると、URL が特定のページを示すように設定されます。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-126">Use the `asp-page` attribute in an anchor tag to set its URL to point to a specific page.</span></span> <span data-ttu-id="a2e2a-127">ページ名の前にスラッシュ "/" を付けると URL が作成されます。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-127">Prefixing the page name with a forward slash "/" creates the URL.</span></span> <span data-ttu-id="a2e2a-128">以下のサンプルの URL は、現在のディレクトリの "スピーカー" ページを示しています。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-128">The URL in the sample below points to the "Speaker" page in the current directory.</span></span>

```cshtml
<a asp-page="/Speakers">All Speakers</a>
```

<span data-ttu-id="a2e2a-129">前のコード サンプルの `asp-page` 属性は、ビューに次のスニペットに似た HTML 出力を表示します。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-129">The `asp-page` attribute in the previous code sample renders HTML output in the view that's similar to the following snippet:</span></span>

```html
<a href="/items?page=%2FSpeakers">Speakers</a>
```

<span data-ttu-id="a2e2a-130">`asp-page` 属性は `asp-route`、`asp-controller`、`asp-action` の各属性と相互に排他的です。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-130">The `asp-page` attribute is mutually exclusive with the `asp-route`, `asp-controller`, and `asp-action` attributes.</span></span> <span data-ttu-id="a2e2a-131">ただし、`asp-page` は、次のコード サンプルで示されているように、`asp-route-id` とともに使用してルーティングを制御できます。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-131">However, `asp-page` can be used with `asp-route-id` to control routing, as the following code sample demonstrates:</span></span>

```cshtml
<a asp-page="/Speaker" asp-route-id="@speaker.Id">View Speaker</a>
```

<span data-ttu-id="a2e2a-132">`asp-route-id` で生成される出力は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-132">The `asp-route-id` produces the following output:</span></span>

```html
https://localhost:44399/Speakers/Index/2?page=%2FSpeaker
```

> [!NOTE]
> <span data-ttu-id="a2e2a-133">Razor ページで `asp-page` 属性を使用するには、`"./Speaker"` のように、URL が相対パスである必要があります。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-133">To use the `asp-page` attribute in Razor Pages, the URLs must be a relative path, for example `"./Speaker"`.</span></span> <span data-ttu-id="a2e2a-134">`asp-page` 属性の相対パスは MVC ビューでは使用できません。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-134">Relative paths in the `asp-page` attribute are not available in MVC views.</span></span> <span data-ttu-id="a2e2a-135">MVC ビューでは、代わりに "/" の構文を使用します。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-135">Use the "/" syntax for MVC views instead.</span></span>

### <a name="asp-route-value"></a><span data-ttu-id="a2e2a-136">asp-route-{value}</span><span class="sxs-lookup"><span data-stu-id="a2e2a-136">asp-route-{value}</span></span>

<span data-ttu-id="a2e2a-137">`asp-route-` はワイルド カード ルート プレフィックスです。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-137">`asp-route-` is a wild card route prefix.</span></span> <span data-ttu-id="a2e2a-138">末尾ダッシュの後ろにある値はすべて、潜在的なルート パラメーターと解釈されます。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-138">Any value you put after the trailing dash will be interpreted as a potential route parameter.</span></span> <span data-ttu-id="a2e2a-139">既定のルートが見つからない場合は、このルート プレフィックスが生成された href に要求パラメーターおよび値として追加されます。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-139">If a default route isn't found, this route prefix will be appended to the generated href as a request parameter and value.</span></span> <span data-ttu-id="a2e2a-140">見つかった場合は、そのルートがルート テンプレートで置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-140">Otherwise it will be substituted in the route template.</span></span>

<span data-ttu-id="a2e2a-141">次のようにコントローラー メソッドが定義されていることが前提です。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-141">Assuming you have a controller method defined as follows:</span></span>

```csharp
public IActionResult AnchorTagHelper(string id)
{
    var speaker = new SpeakerData()
    {
        SpeakerId = 12
    };
    return View(viewName, speaker);
}
```

<span data-ttu-id="a2e2a-142">さらに、既定のルート テンプレートが次のように *Startup.cs* に定義されている必要もあります。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-142">And have the default route template defined in your *Startup.cs* as follows:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
});

```

<span data-ttu-id="a2e2a-143">コントローラーからビューに渡される**スピーカー** モデル パラメーターを使用するために必要なアンカー タグ ヘルパーが含まれる **cshtml** ファイルは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-143">The **cshtml** file that contains the Anchor Tag Helper necessary to use the **speaker** model parameter passed in from the controller to the view is as follows:</span></span>

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-id=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

<span data-ttu-id="a2e2a-144">**id** が既定のルートで見つかったため、次のような HTML が生成されます。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-144">The generated HTML will then be as follows because **id** was found in the default route.</span></span>

```html
<a href='/Speaker/Detail/12'>SpeakerId: 12</a>
```

<span data-ttu-id="a2e2a-145">ルート プレフィックスが見つかったルーティング テンプレートに含まれていない場合、**cshtml** ファイルは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-145">If the route prefix isn't part of the routing template found, which is the case with the following **cshtml** file:</span></span>

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-speakerid=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

<span data-ttu-id="a2e2a-146">**speakerid** が一致するルートで見つからなかったため、次のような HTML が生成されます。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-146">The generated HTML will then be as follows because **speakerid** wasn't found in the route matched:</span></span>

```html
<a href='/Speaker/Detail?speakerid=12'>SpeakerId: 12</a>
```

<span data-ttu-id="a2e2a-147">`asp-controller` または `asp-action` のどちらかが指定されていない場合は、`asp-route` 属性の場合と同じ既定の処理に従います。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-147">If either `asp-controller` or `asp-action` are not specified, then the same default processing is followed as is in the `asp-route` attribute.</span></span>

### <a name="asp-route"></a><span data-ttu-id="a2e2a-148">asp-route</span><span class="sxs-lookup"><span data-stu-id="a2e2a-148">asp-route</span></span>

<span data-ttu-id="a2e2a-149">`asp-route` により、名前付きルートに直接リンクする URL を作成することができます。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-149">`asp-route` provides a way to create a URL that links directly to a named route.</span></span> <span data-ttu-id="a2e2a-150">ルーティング属性を使用すると、`SpeakerController` に示されているようにルートに名前を付け、その `Evaluations` メソッドで使用することができます。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-150">Using routing attributes, a route can be named as shown in the `SpeakerController` and used in its `Evaluations` method.</span></span>

<span data-ttu-id="a2e2a-151">`Name = "speakerevals"` からアンカー タグ ヘルパーに、URL `/Speaker/Evaluations` を使用してそのコントローラー メソッドに直接ルートを生成するように指示されます。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-151">`Name = "speakerevals"` tells the Anchor Tag Helper to generate a route directly to that controller method using the URL `/Speaker/Evaluations`.</span></span> <span data-ttu-id="a2e2a-152">`asp-controller` または `asp-action` が `asp-route` の他に指定されていると、想定と異なるルートが生成される場合があります。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-152">If `asp-controller` or `asp-action` is specified in addition to `asp-route`, the route generated may not be what you expect.</span></span> <span data-ttu-id="a2e2a-153">ルートの競合を防ぐには、`asp-route` を属性 `asp-controller` または `asp-action` のどちらかとともに使用してはいけません。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-153">`asp-route` shouldn't be used with either of the attributes `asp-controller` or `asp-action` to avoid a route conflict.</span></span>

### <a name="asp-all-route-data"></a><span data-ttu-id="a2e2a-154">asp-all-route-data</span><span class="sxs-lookup"><span data-stu-id="a2e2a-154">asp-all-route-data</span></span>

<span data-ttu-id="a2e2a-155">`asp-all-route-data` では、キーがパラメーター名で、値がそのキーに関連付けられた値であるキー値ペアのディクショナリを生成できます。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-155">`asp-all-route-data` allows creating a dictionary of key value pairs where the key is the parameter name and the value is the value associated with that key.</span></span>

<span data-ttu-id="a2e2a-156">以下の例に示すように、インライン ディクショナリが作成され、Razor ビューにデータが渡されます。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-156">As the example below shows, an inline dictionary is created and the data is passed to the razor view.</span></span> <span data-ttu-id="a2e2a-157">または、データがモデルによって渡される場合もあります。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-157">As an alternative, the data could also be passed in with your model.</span></span>

```cshtml
@{
    var dict =
        new Dictionary<string, string>
        {
            {"speakerId", "11"},
            {"currentYear", "true"}
        };
}
<a asp-route="speakerevalscurrent"
asp-all-route-data="dict">SpeakerEvals</a>
```

<span data-ttu-id="a2e2a-158">上記のコードでは、次の URL が生成されます: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true</span><span class="sxs-lookup"><span data-stu-id="a2e2a-158">The code above generates the following URL: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true</span></span>

<span data-ttu-id="a2e2a-159">リンクをクリックすると、コントローラー メソッド `EvaluationsCurrent` が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-159">When the link is clicked, the controller method `EvaluationsCurrent` is called.</span></span> <span data-ttu-id="a2e2a-160">これが呼び出されるのは、そのコントローラーに `asp-all-route-data` ディクショナリから作成されたものと一致する 2 つの文字列パラメーターが含まれているためです。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-160">It's called because that controller has two string parameters that match what has been created from the `asp-all-route-data` dictionary.</span></span>

<span data-ttu-id="a2e2a-161">ディクショナリのいずれかのキーがルート パラメーターと一致している場合、その値はルートで適宜置き換えられ、一致しない他の値が要求パラメーターとして生成されます。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-161">If any keys in the dictionary match route parameters, those values will be substituted in the route as appropriate and the other non-matching values will be generated as request parameters.</span></span>

### <a name="asp-fragment"></a><span data-ttu-id="a2e2a-162">asp-fragment</span><span class="sxs-lookup"><span data-stu-id="a2e2a-162">asp-fragment</span></span>

<span data-ttu-id="a2e2a-163">`asp-fragment` では URL に追加される URL フラグメントが定義されます。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-163">`asp-fragment` defines a URL fragment to append to the URL.</span></span> <span data-ttu-id="a2e2a-164">アンカー タグ ヘルパーにより、ハッシュ文字 (#) が追加されます。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-164">The Anchor Tag Helper will add the hash character (#).</span></span> <span data-ttu-id="a2e2a-165">タグを作成する場合: </span><span class="sxs-lookup"><span data-stu-id="a2e2a-165">If you create a tag:</span></span>

```cshtml
<a asp-action="Evaluations" asp-controller="Speaker"  
   asp-fragment="SpeakerEvaluations">About Speaker Evals</a>
```

<span data-ttu-id="a2e2a-166">次の URL が生成されます: http://localhost/Speaker/Evaluations#SpeakerEvaluations</span><span class="sxs-lookup"><span data-stu-id="a2e2a-166">The generated URL will be: http://localhost/Speaker/Evaluations#SpeakerEvaluations</span></span>

<span data-ttu-id="a2e2a-167">ハッシュ タグはクライアント側アプリケーションを構築するときに便利です。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-167">Hash tags are useful when building client-side applications.</span></span> <span data-ttu-id="a2e2a-168">たとえば、JavaScript でのマーク付けや検索を簡単にするために使用できます。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-168">They can be used for easy marking and searching in JavaScript, for example.</span></span>

### <a name="asp-area"></a><span data-ttu-id="a2e2a-169">asp-area</span><span class="sxs-lookup"><span data-stu-id="a2e2a-169">asp-area</span></span>

<span data-ttu-id="a2e2a-170">`asp-area` は ASP.NET Core が適切なルートを設定するために使用する区分名を設定します。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-170">`asp-area` sets the area name that ASP.NET Core uses to set the appropriate route.</span></span> <span data-ttu-id="a2e2a-171">区分の属性によって、どのようにルートの再マップが行われるかの例を以下に示します。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-171">Below are examples of how the area attribute causes a remapping of routes.</span></span> <span data-ttu-id="a2e2a-172">`asp-area` を [ブログ] に設定すると、このアンカー タグの関連付けられているコントローラーとビューのルートに、ディレクトリ `Areas/Blogs` のプレフィックスが付けられます。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-172">Setting `asp-area` to Blogs prefixes the directory `Areas/Blogs` to the routes of the associated controllers and views for this anchor tag.</span></span>

* <span data-ttu-id="a2e2a-173">プロジェクト名</span><span class="sxs-lookup"><span data-stu-id="a2e2a-173">Project name</span></span>
  * <span data-ttu-id="a2e2a-174">wwwroot</span><span class="sxs-lookup"><span data-stu-id="a2e2a-174">wwwroot</span></span>
  * <span data-ttu-id="a2e2a-175">区分</span><span class="sxs-lookup"><span data-stu-id="a2e2a-175">Areas</span></span>
    * <span data-ttu-id="a2e2a-176">ブログ</span><span class="sxs-lookup"><span data-stu-id="a2e2a-176">Blogs</span></span>
      * <span data-ttu-id="a2e2a-177">コントローラー</span><span class="sxs-lookup"><span data-stu-id="a2e2a-177">Controllers</span></span>
        * <span data-ttu-id="a2e2a-178">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="a2e2a-178">HomeController.cs</span></span>
      * <span data-ttu-id="a2e2a-179">ビュー</span><span class="sxs-lookup"><span data-stu-id="a2e2a-179">Views</span></span>
        * <span data-ttu-id="a2e2a-180">Home</span><span class="sxs-lookup"><span data-stu-id="a2e2a-180">Home</span></span>
          * <span data-ttu-id="a2e2a-181">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="a2e2a-181">Index.cshtml</span></span>
          * <span data-ttu-id="a2e2a-182">AboutBlog.cshtml</span><span class="sxs-lookup"><span data-stu-id="a2e2a-182">AboutBlog.cshtml</span></span>
  * <span data-ttu-id="a2e2a-183">コントローラー</span><span class="sxs-lookup"><span data-stu-id="a2e2a-183">Controllers</span></span>

<span data-ttu-id="a2e2a-184">```AboutBlog.cshtml``` ファイルを参照するときに ```area="Blogs"``` などの有効な区分タグを指定すると、アンカー タグ ヘルパーを使用した次のようなファイルになります。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-184">Specifying an area tag that's valid, such as ```area="Blogs"``` when referencing the ```AboutBlog.cshtml``` file will look like the following using the Anchor Tag Helper.</span></span>

```cshtml
<a asp-action="AboutBlog" asp-controller="Home" asp-area="Blogs">Blogs About</a>
```

<span data-ttu-id="a2e2a-185">生成される HTML には区分セグメントが含まれ、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-185">The generated HTML will include the areas segment and will be as follows:</span></span>

```html
<a href="/Blogs/Home/AboutBlog">Blogs About</a>
```

> [!TIP]
> <span data-ttu-id="a2e2a-186">MVC 区分が Web アプリケーションで動作するには、ルート テンプレートに区分への参照 (存在する場合) が含まれている必要があります。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-186">For MVC areas to work in a web application, the route template must include a reference to the area if it exists.</span></span> <span data-ttu-id="a2e2a-187">その `routes.MapRoute` メソッド呼び出しの 2 番目のパラメーターであるテンプレートは次のように表示されます: `template: '"{area:exists}/{controller=Home}/{action=Index}"'`</span><span class="sxs-lookup"><span data-stu-id="a2e2a-187">That template, which is the second parameter of the `routes.MapRoute` method call, will appear as: `template: '"{area:exists}/{controller=Home}/{action=Index}"'`</span></span>

### <a name="asp-protocol"></a><span data-ttu-id="a2e2a-188">asp-protocol</span><span class="sxs-lookup"><span data-stu-id="a2e2a-188">asp-protocol</span></span>

<span data-ttu-id="a2e2a-189">`asp-protocol` は URL に (`https` などの) プロトコルを指定するためにあります。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-189">The `asp-protocol` is for specifying a protocol (such as `https`) in your URL.</span></span> <span data-ttu-id="a2e2a-190">プロトコルを含むアンカー タグ ヘルパーの例は、次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-190">An example Anchor Tag Helper that includes the protocol will look as follows:</span></span>

```<a asp-protocol="https" asp-action="About" asp-controller="Home">About</a>```

<span data-ttu-id="a2e2a-191">HTML は次のように生成されます。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-191">and will generate HTML as follows:</span></span>

```<a href="https://localhost/Home/About">About</a>```

<span data-ttu-id="a2e2a-192">例ではドメインが localhost ですが、アンカー タグ ヘルパーは URL を生成するときに Web サイトのパブリック ドメインを使用します。</span><span class="sxs-lookup"><span data-stu-id="a2e2a-192">The domain in the example is localhost, but the Anchor Tag Helper uses the website's public domain when generating the URL.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a2e2a-193">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="a2e2a-193">Additional resources</span></span>

* [<span data-ttu-id="a2e2a-194">領域</span><span class="sxs-lookup"><span data-stu-id="a2e2a-194">Areas</span></span>](xref:mvc/controllers/areas)
