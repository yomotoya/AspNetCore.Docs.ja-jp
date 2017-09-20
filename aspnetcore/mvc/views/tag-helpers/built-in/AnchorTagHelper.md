---
title: "アンカー タグ ヘルパー |Microsoft ドキュメント"
author: pkellner
description: "アンカー タグ ヘルパーを使用する方法を示しています。"
keywords: "ASP.NET Core,タグ ヘルパー"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: c045d485-d1dc-4cea-a675-46be83b7a011
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/AnchorTagHelper
ms.openlocfilehash: fdb91836699b4dd334499cffa6c4c3961c5c020f
ms.sourcegitcommit: 74a8ad9c1ba5c155d7c4303e67632a0922c38e86
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/20/2017
---
# <a name="anchor-tag-helper"></a><span data-ttu-id="4791d-104">アンカー タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="4791d-104">Anchor Tag Helper</span></span>

<span data-ttu-id="4791d-105">著者: [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="4791d-105">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="4791d-106">アンカー タグ ヘルパーの強化 HTML アンカー (`<a ... ></a>`) 新しい属性を追加してタグ。</span><span class="sxs-lookup"><span data-stu-id="4791d-106">The Anchor Tag Helper enhances the HTML anchor (`<a ... ></a>`) tag by adding new attributes.</span></span> <span data-ttu-id="4791d-107">生成されたリンク (上、`href`タグ)、新しい属性を使用して作成します。</span><span class="sxs-lookup"><span data-stu-id="4791d-107">The link generated (on the `href` tag) is created using the new attributes.</span></span> <span data-ttu-id="4791d-108">その URL には、https などの省略可能なプロトコルを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="4791d-108">That URL can include an optional protocol such as https.</span></span>

<span data-ttu-id="4791d-109">下の スピーカー コント ローラーは、このドキュメントのサンプルで使用されます。</span><span class="sxs-lookup"><span data-stu-id="4791d-109">The speaker controller below is used in samples in this document.</span></span>

<br/><span data-ttu-id="4791d-110">
**SpeakerController.cs**</span><span class="sxs-lookup"><span data-stu-id="4791d-110">
**SpeakerController.cs**</span></span> 

[!code-csharp[SpeakerController](sample/TagHelpersBuiltInAspNetCore/src/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs)]


## <a name="anchor-tag-helper-attributes"></a><span data-ttu-id="4791d-111">アンカー タグ ヘルパー属性</span><span class="sxs-lookup"><span data-stu-id="4791d-111">Anchor Tag Helper Attributes</span></span>

- - -

### <a name="asp-controller"></a><span data-ttu-id="4791d-112">asp コント ローラー</span><span class="sxs-lookup"><span data-stu-id="4791d-112">asp-controller</span></span>

<span data-ttu-id="4791d-113">`asp-controller`URL の生成に使用するコント ローラーを関連付けるために使用します。</span><span class="sxs-lookup"><span data-stu-id="4791d-113">`asp-controller` is used to associate which controller will be used to generate the URL.</span></span> <span data-ttu-id="4791d-114">指定されたコント ローラーは、現在のプロジェクトに存在する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4791d-114">The controllers specified must exist in the current project.</span></span> <span data-ttu-id="4791d-115">次のコードには、すべてのスピーカーが一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="4791d-115">The following code lists all speakers:</span></span> 

```cshtml
<a asp-controller="Speaker" asp-action="Index">All Speakers</a>
```

<span data-ttu-id="4791d-116">生成されたマークアップになります。</span><span class="sxs-lookup"><span data-stu-id="4791d-116">The generated markup will be:</span></span>

```html
<a href="/Speaker">All Speakers</a>
```

<span data-ttu-id="4791d-117">場合、`asp-controller`が指定されていると`asp-action`既定ではない、`asp-action`現在実行中のビューの既定のコント ローラーのメソッドになります。</span><span class="sxs-lookup"><span data-stu-id="4791d-117">If the `asp-controller` is specified and `asp-action` is not, the default `asp-action` will be the default controller method of the currently executing view.</span></span> <span data-ttu-id="4791d-118">上記の例では場合、 `asp-action` 、左からこのアンカー タグ ヘルパーが生成されると*HomeController*の`Index`ビュー (**ホーム/**)、生成されたマークアップになります。</span><span class="sxs-lookup"><span data-stu-id="4791d-118">That is, in the above example, if `asp-action` is left out, and this Anchor Tag Helper is generated from *HomeController*'s `Index` view (**/Home**), the generated markup will be:</span></span>


```html
<a href="/Home">All Speakers</a>
```

- - -
  
### <a name="asp-action"></a><span data-ttu-id="4791d-119">asp アクション</span><span class="sxs-lookup"><span data-stu-id="4791d-119">asp-action</span></span>

<span data-ttu-id="4791d-120">`asp-action`含まれているコント ローラー アクション メソッドの名前を生成された`href`です。</span><span class="sxs-lookup"><span data-stu-id="4791d-120">`asp-action` is the name of the action method in the controller that will be included in the generated `href`.</span></span> <span data-ttu-id="4791d-121">たとえば、次のコードは、生成された設定`href`スピーカー詳細ページを指します。</span><span class="sxs-lookup"><span data-stu-id="4791d-121">For example, the following code set the generated `href` to point to the speaker detail page:</span></span>

```html
<a asp-controller="Speaker" asp-action="Detail">Speaker Detail</a>
```

<span data-ttu-id="4791d-122">生成されたマークアップになります。</span><span class="sxs-lookup"><span data-stu-id="4791d-122">The generated markup will be:</span></span>

```html
<a href="/Speaker/Detail">Speaker Detail</a>
```

<span data-ttu-id="4791d-123">ない場合は`asp-controller`属性が指定されて、現在のビューを実行するビューを呼び出す既定のコント ローラーが使用されます。</span><span class="sxs-lookup"><span data-stu-id="4791d-123">If no `asp-controller` attribute is specified, the default controller calling the view executing the current view will be used.</span></span>  
 
<span data-ttu-id="4791d-124">場合、属性`asp-action`は`Index`、url、既定値に先行する操作は追加されず、`Index`呼び出されるメソッド。</span><span class="sxs-lookup"><span data-stu-id="4791d-124">If the attribute `asp-action` is `Index`, then no action is appended to the URL, leading to the default `Index` method being called.</span></span> <span data-ttu-id="4791d-125">アクションを指定 (または既定値) で参照されているコント ローラーに存在する必要があります`asp-controller`です。</span><span class="sxs-lookup"><span data-stu-id="4791d-125">The action specified (or defaulted), must exist in the controller referenced in `asp-controller`.</span></span>

- - -
  
<a name="route"></a>
### <a name="asp-route-value"></a><span data-ttu-id="4791d-126">asp - ルート - {value}</span><span class="sxs-lookup"><span data-stu-id="4791d-126">asp-route-{value}</span></span>

<span data-ttu-id="4791d-127">`asp-route-`次のワイルドカード ルート プレフィックス。</span><span class="sxs-lookup"><span data-stu-id="4791d-127">`asp-route-` is a wild card route prefix.</span></span> <span data-ttu-id="4791d-128">任意の値は、潜在的なルートのパラメーターとして解釈されます末尾ダッシュ後に配置します。</span><span class="sxs-lookup"><span data-stu-id="4791d-128">Any value you put after the trailing dash will be interpreted as a potential route parameter.</span></span> <span data-ttu-id="4791d-129">既定のルートが見つからない場合は、要求のパラメーターと値として生成された href にこのルート プレフィックスが追加されます。</span><span class="sxs-lookup"><span data-stu-id="4791d-129">If a default route is not found, this route prefix will be appended to the generated href as a request parameter and value.</span></span> <span data-ttu-id="4791d-130">それ以外の場合、ルート テンプレートで置き換えられますされます。</span><span class="sxs-lookup"><span data-stu-id="4791d-130">Otherwise it will be substituted in the route template.</span></span>

<span data-ttu-id="4791d-131">前提とするとがコント ローラー メソッドように定義します。</span><span class="sxs-lookup"><span data-stu-id="4791d-131">Assuming you have a controller method defined as follows:</span></span>

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

<span data-ttu-id="4791d-132">定義されている既定のルート テンプレートがあると、 *Startup.cs*次のようにします。</span><span class="sxs-lookup"><span data-stu-id="4791d-132">And have the default route template defined in your *Startup.cs* as follows:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
});

```

<span data-ttu-id="4791d-133">**Cshtml** 、アンカー タグ ヘルパーを使用する必要のあるファイル、**スピーカー**ビューに、コント ローラーから渡されたモデル パラメーターを次に示します。</span><span class="sxs-lookup"><span data-stu-id="4791d-133">The **cshtml** file that contains the Anchor Tag Helper necessary to use the **speaker** model parameter passed in from the controller to the view is as follows:</span></span>

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-id=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

<span data-ttu-id="4791d-134">生成される HTML れます次のように**id**で既定のルートが見つかりました。</span><span class="sxs-lookup"><span data-stu-id="4791d-134">The generated HTML will then be as follows because **id** was found in the default route.</span></span>

```html
<a href='/Speaker/Detail/12'>SpeakerId: 12</a>
```

<span data-ttu-id="4791d-135">次の場合はそのルート プレフィックスがあるルーティング テンプレートの一部でない場合は、 **cshtml**ファイル。</span><span class="sxs-lookup"><span data-stu-id="4791d-135">If the route prefix is not part of the routing template found, which is the case with the following **cshtml** file:</span></span>

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-speakerid=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

<span data-ttu-id="4791d-136">生成される HTML れます次のように**speakerid**が一致するルート上で見つかりませんでした。</span><span class="sxs-lookup"><span data-stu-id="4791d-136">The generated HTML will then be as follows because **speakerid** was not found in the route matched:</span></span>


```html
<a href='/Speaker/Detail?speakerid=12'>SpeakerId: 12</a>
```

<span data-ttu-id="4791d-137">いずれか`asp-controller`または`asp-action`が指定されていないがで同じ既定の処理の後に、`asp-route`属性。</span><span class="sxs-lookup"><span data-stu-id="4791d-137">If either `asp-controller` or `asp-action` are not specified, then the same default processing is followed as is in the `asp-route` attribute.</span></span>

- - -

### <a name="asp-route"></a><span data-ttu-id="4791d-138">asp ルート</span><span class="sxs-lookup"><span data-stu-id="4791d-138">asp-route</span></span>

<span data-ttu-id="4791d-139">`asp-route`名前付きのルートに直接リンクする URL を作成する方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="4791d-139">`asp-route` provides a way to create a URL that links directly to a named route.</span></span> <span data-ttu-id="4791d-140">ルーティング属性を使用して、ように、ルートをということができます、`SpeakerController`で使用されると、`Evaluations`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="4791d-140">Using routing attributes, a route can be named as shown in the `SpeakerController` and used in its `Evaluations` method.</span></span>

<span data-ttu-id="4791d-141">`Name = "speakerevals"`アンカー タグ ヘルパーのルート URL を使用してそのコント ローラーのメソッドを直接生成するように指示`/Speaker/Evaluations`です。</span><span class="sxs-lookup"><span data-stu-id="4791d-141">`Name = "speakerevals"` tells the Anchor Tag Helper to generate a route directly to that controller method using the URL `/Speaker/Evaluations`.</span></span> <span data-ttu-id="4791d-142">場合`asp-controller`または`asp-action`が他に指定されている`asp-route`、生成されるルートできない可能性がありますが予期したものとします。</span><span class="sxs-lookup"><span data-stu-id="4791d-142">If `asp-controller` or `asp-action` is specified in addition to `asp-route`, the route generated may not be what you expect.</span></span> <span data-ttu-id="4791d-143">`asp-route`属性のいずれかで使用しないで`asp-controller`または`asp-action`ルート競合を回避します。</span><span class="sxs-lookup"><span data-stu-id="4791d-143">`asp-route` should not be used with either of the attributes `asp-controller` or `asp-action` to avoid a route conflict.</span></span>

- - -

### <a name="asp-all-route-data"></a><span data-ttu-id="4791d-144">asp すべてルート データ</span><span class="sxs-lookup"><span data-stu-id="4791d-144">asp-all-route-data</span></span>

<span data-ttu-id="4791d-145">`asp-all-route-data`ここで、キーは、パラメーター名と値は、そのキーに関連付けられている値のキー値のペアのディクショナリを作成できます。</span><span class="sxs-lookup"><span data-stu-id="4791d-145">`asp-all-route-data` allows creating a dictionary of key value pairs where the key is the parameter name and the value is the value associated with that key.</span></span>

<span data-ttu-id="4791d-146">次の例に、としてインライン ディクショナリが作成され、razor ビューにデータが渡されます。</span><span class="sxs-lookup"><span data-stu-id="4791d-146">As the example below shows, an inline dictionary is created and the data is passed to the razor view.</span></span> <span data-ttu-id="4791d-147">代わりに、データも、モデルを使用して渡される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="4791d-147">As an alternative, the data could also be passed in with your model.</span></span>

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

<span data-ttu-id="4791d-148">上記のコードには、次の URL が生成されます: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true</span><span class="sxs-lookup"><span data-stu-id="4791d-148">The code above generates the following URL: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true</span></span>

<span data-ttu-id="4791d-149">ときに、リンクをクリックして、コント ローラー メソッド`EvaluationsCurrent`と呼びます。</span><span class="sxs-lookup"><span data-stu-id="4791d-149">When the link is clicked, the controller method `EvaluationsCurrent` is called.</span></span> <span data-ttu-id="4791d-150">これは、そのコント ローラーがある 2 つの文字列パラメーターから作成したものと一致するために呼び出されますが、`asp-all-route-data`ディクショナリ。</span><span class="sxs-lookup"><span data-stu-id="4791d-150">It is called because that controller has two string parameters that match what has been created from the `asp-all-route-data` dictionary.</span></span>

<span data-ttu-id="4791d-151">ディクショナリ一致しているすべてのキーは、パラメーターをルーティングする場合として適切なルート上でそれらの値が置き換えられるし、要求パラメーターと一致しない値が生成されます。</span><span class="sxs-lookup"><span data-stu-id="4791d-151">If any keys in the dictionary match route parameters, those values will be substituted in the route as appropriate and the other non-matching values will be generated as request parameters.</span></span>

- - -

### <a name="asp-fragment"></a><span data-ttu-id="4791d-152">asp フラグメント</span><span class="sxs-lookup"><span data-stu-id="4791d-152">asp-fragment</span></span>

<span data-ttu-id="4791d-153">`asp-fragment`URL に追加する URL フラグメントを定義します。</span><span class="sxs-lookup"><span data-stu-id="4791d-153">`asp-fragment` defines a URL fragment to append to the URL.</span></span> <span data-ttu-id="4791d-154">アンカー タグ ヘルパー追加ハッシュ文字 (#)。</span><span class="sxs-lookup"><span data-stu-id="4791d-154">The Anchor Tag Helper will add the hash character (#).</span></span> <span data-ttu-id="4791d-155">場合は、タグを作成します。</span><span class="sxs-lookup"><span data-stu-id="4791d-155">If you create a tag:</span></span>

```cshtml
<a asp-action="Evaluations" asp-controller="Speaker"  
   asp-fragment="SpeakerEvaluations">About Speaker Evals</a>
```

<span data-ttu-id="4791d-156">生成された URL になります: http://localhost/Speaker/Evaluations#SpeakerEvaluations</span><span class="sxs-lookup"><span data-stu-id="4791d-156">The generated URL will be: http://localhost/Speaker/Evaluations#SpeakerEvaluations</span></span>

<span data-ttu-id="4791d-157">ハッシュのタグは、クライアント側アプリケーションを構築するときに便利です。</span><span class="sxs-lookup"><span data-stu-id="4791d-157">Hash tags are useful when building client-side applications.</span></span> <span data-ttu-id="4791d-158">簡単なマークを付け、たとえば、JavaScript では、検索、使用できます。</span><span class="sxs-lookup"><span data-stu-id="4791d-158">They can be used for easy marking and searching in JavaScript, for example.</span></span>

- - -

### <a name="asp-area"></a><span data-ttu-id="4791d-159">asp 領域</span><span class="sxs-lookup"><span data-stu-id="4791d-159">asp-area</span></span>

<span data-ttu-id="4791d-160">`asp-area`ASP.NET Core は適切なルートの設定を使用して領域の名前を設定します。</span><span class="sxs-lookup"><span data-stu-id="4791d-160">`asp-area` sets the area name that ASP.NET Core uses to set the appropriate route.</span></span> <span data-ttu-id="4791d-161">どの区分属性によって、ルートの再マップの例を以下に示します。</span><span class="sxs-lookup"><span data-stu-id="4791d-161">Below are examples of how the area attribute causes a remapping of routes.</span></span> <span data-ttu-id="4791d-162">設定`asp-area`ブログに、ディレクトリをプレフィックス`Areas/Blogs`関連付けられているコント ローラーとこのアンカー タグのビューのルートにします。</span><span class="sxs-lookup"><span data-stu-id="4791d-162">Setting `asp-area` to Blogs prefixes the directory `Areas/Blogs` to the routes of the associated controllers and views for this anchor tag.</span></span>

* <span data-ttu-id="4791d-163">プロジェクト名</span><span class="sxs-lookup"><span data-stu-id="4791d-163">Project name</span></span>

  * <span data-ttu-id="4791d-164">*wwwroot*</span><span class="sxs-lookup"><span data-stu-id="4791d-164">*wwwroot*</span></span>

  * <span data-ttu-id="4791d-165">*領域*</span><span class="sxs-lookup"><span data-stu-id="4791d-165">*Areas*</span></span>

    * <span data-ttu-id="4791d-166">*ブログ*</span><span class="sxs-lookup"><span data-stu-id="4791d-166">*Blogs*</span></span>

      * <span data-ttu-id="4791d-167">*コントローラー*</span><span class="sxs-lookup"><span data-stu-id="4791d-167">*Controllers*</span></span>

        * <span data-ttu-id="4791d-168">*HomeController.cs*</span><span class="sxs-lookup"><span data-stu-id="4791d-168">*HomeController.cs*</span></span>

      * <span data-ttu-id="4791d-169">*ビュー*</span><span class="sxs-lookup"><span data-stu-id="4791d-169">*Views*</span></span>

        * <span data-ttu-id="4791d-170">*ホーム*</span><span class="sxs-lookup"><span data-stu-id="4791d-170">*Home*</span></span>

          * <span data-ttu-id="4791d-171">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="4791d-171">*Index.cshtml*</span></span>
          
          * <span data-ttu-id="4791d-172">*AboutBlog.cshtml*</span><span class="sxs-lookup"><span data-stu-id="4791d-172">*AboutBlog.cshtml*</span></span>
          
  * <span data-ttu-id="4791d-173">*コントローラー*</span><span class="sxs-lookup"><span data-stu-id="4791d-173">*Controllers*</span></span>
  

        
<span data-ttu-id="4791d-174">などの無効である area タグを指定する```area="Blogs"```参照するときに、```AboutBlog.cshtml```ファイルは、次のようになりますアンカー タグ ヘルパーの使用します。</span><span class="sxs-lookup"><span data-stu-id="4791d-174">Specifying an area tag that is valid, such as ```area="Blogs"``` when referencing the ```AboutBlog.cshtml``` file will look like the following using the Anchor Tag Helper.</span></span>

```cshtml
<a asp-action="AboutBlog" asp-controller="Home" asp-area="Blogs">Blogs About</a>
```

<span data-ttu-id="4791d-175">生成される HTML では、領域セグメントが含まれますされ、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="4791d-175">The generated HTML will include the areas segment and will be as follows:</span></span>

```html
<a href="/Blogs/Home/AboutBlog">Blogs About</a>
```

> [!TIP]
> <span data-ttu-id="4791d-176">Web アプリケーションで作業する MVC 区分のルート テンプレートは存在する場合領域への参照を含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="4791d-176">For MVC areas to work in a web application, the route template must include a reference to the area if it exists.</span></span> <span data-ttu-id="4791d-177">そのテンプレートでは、2 番目のパラメーターの`routes.MapRoute`メソッド呼び出しとして表示されます。`template: '"{area:exists}/{controller=Home}/{action=Index}"'`</span><span class="sxs-lookup"><span data-stu-id="4791d-177">That template, which is the second parameter of the `routes.MapRoute` method call, will appear as: `template: '"{area:exists}/{controller=Home}/{action=Index}"'`</span></span>

- - -

### <a name="asp-protocol"></a><span data-ttu-id="4791d-178">asp プロトコル</span><span class="sxs-lookup"><span data-stu-id="4791d-178">asp-protocol</span></span>

<span data-ttu-id="4791d-179">`asp-protocol` 、プロトコルを指定するのには (など`https`)、URL にします。</span><span class="sxs-lookup"><span data-stu-id="4791d-179">The `asp-protocol` is for specifying a protocol (such as `https`) in your URL.</span></span> <span data-ttu-id="4791d-180">プロトコルを含むアンカー タグ ヘルパーの使用例は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="4791d-180">An example Anchor Tag Helper that includes the protocol will look as follows:</span></span>

```<a asp-protocol="https" asp-action="About" asp-controller="Home">About</a>```

<span data-ttu-id="4791d-181">次のように HTML を生成します。</span><span class="sxs-lookup"><span data-stu-id="4791d-181">and will generate HTML as follows:</span></span>

```<a href="https://localhost/Home/About">About</a>```

<span data-ttu-id="4791d-182">ドメインの例では、localhost がアンカー タグ ヘルパーは、URL を生成するときに、web サイトのパブリック ドメインを使用します。</span><span class="sxs-lookup"><span data-stu-id="4791d-182">The domain in the example is localhost, but the Anchor Tag Helper uses the website's public domain when generating the URL.</span></span>

- - -

## <a name="additional-resources"></a><span data-ttu-id="4791d-183">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="4791d-183">Additional resources</span></span>

* [<span data-ttu-id="4791d-184">領域</span><span class="sxs-lookup"><span data-stu-id="4791d-184">Areas</span></span>](xref:mvc/controllers/areas)
