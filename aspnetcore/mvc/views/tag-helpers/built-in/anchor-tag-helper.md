---
title: "アンカー タグ ヘルパー |Microsoft ドキュメント"
author: pkellner
description: "アンカー タグ ヘルパーを使用する方法を示しています。"
ms.author: riande
manager: wpickett
ms.date: 12/20/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: 74609b515936ec7da8bfc133c27cb69f51311924
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
# <a name="anchor-tag-helper"></a><span data-ttu-id="168cc-103">アンカー タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="168cc-103">Anchor Tag Helper</span></span>

<span data-ttu-id="168cc-104">著者: [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="168cc-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="168cc-105">アンカー タグ ヘルパーの強化 HTML アンカー (`<a ... ></a>`) 新しい属性を追加してタグ。</span><span class="sxs-lookup"><span data-stu-id="168cc-105">The Anchor Tag Helper enhances the HTML anchor (`<a ... ></a>`) tag by adding new attributes.</span></span> <span data-ttu-id="168cc-106">生成されたリンク (上、`href`タグ)、新しい属性を使用して作成します。</span><span class="sxs-lookup"><span data-stu-id="168cc-106">The link generated (on the `href` tag) is created using the new attributes.</span></span> <span data-ttu-id="168cc-107">その URL には、https などの省略可能なプロトコルを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="168cc-107">That URL can include an optional protocol such as https.</span></span>

<span data-ttu-id="168cc-108">下の スピーカー コント ローラーは、このドキュメントのサンプルで使用されます。</span><span class="sxs-lookup"><span data-stu-id="168cc-108">The speaker controller below is used in samples in this document.</span></span>

<span data-ttu-id="168cc-109">**SpeakerController.cs**</span><span class="sxs-lookup"><span data-stu-id="168cc-109">**SpeakerController.cs**</span></span> 

[!code-csharp[SpeakerController](sample/TagHelpersBuiltInAspNetCore/src/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs)]


## <a name="anchor-tag-helper-attributes"></a><span data-ttu-id="168cc-110">アンカー タグ ヘルパー属性</span><span class="sxs-lookup"><span data-stu-id="168cc-110">Anchor Tag Helper Attributes</span></span>

### <a name="asp-controller"></a><span data-ttu-id="168cc-111">asp コント ローラー</span><span class="sxs-lookup"><span data-stu-id="168cc-111">asp-controller</span></span>

<span data-ttu-id="168cc-112">`asp-controller`URL の生成に使用するコント ローラーを関連付けるために使用します。</span><span class="sxs-lookup"><span data-stu-id="168cc-112">`asp-controller` is used to associate which controller will be used to generate the URL.</span></span> <span data-ttu-id="168cc-113">指定されたコント ローラーは、現在のプロジェクトに存在する必要があります。</span><span class="sxs-lookup"><span data-stu-id="168cc-113">The controllers specified must exist in the current project.</span></span> <span data-ttu-id="168cc-114">次のコードには、すべてのスピーカーが一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="168cc-114">The following code lists all speakers:</span></span> 

```cshtml
<a asp-controller="Speaker" asp-action="Index">All Speakers</a>
```

<span data-ttu-id="168cc-115">生成されたマークアップになります。</span><span class="sxs-lookup"><span data-stu-id="168cc-115">The generated markup will be:</span></span>

```html
<a href="/Speaker">All Speakers</a>
```

<span data-ttu-id="168cc-116">場合、`asp-controller`が指定されていると`asp-action`いない場合は、既定値`asp-action`現在実行中のビューの既定のコント ローラーのメソッドになります。</span><span class="sxs-lookup"><span data-stu-id="168cc-116">If the `asp-controller` is specified and `asp-action` isn't, the default `asp-action` will be the default controller method of the currently executing view.</span></span> <span data-ttu-id="168cc-117">上記の例では場合、 `asp-action` 、左からこのアンカー タグ ヘルパーが生成されると*HomeController*の`Index`ビュー (**ホーム/**)、生成されたマークアップになります。</span><span class="sxs-lookup"><span data-stu-id="168cc-117">That is, in the above example, if `asp-action` is left out, and this Anchor Tag Helper is generated from *HomeController*'s `Index` view (**/Home**), the generated markup will be:</span></span>

```html
<a href="/Home">All Speakers</a>
```

### <a name="asp-action"></a><span data-ttu-id="168cc-118">asp アクション</span><span class="sxs-lookup"><span data-stu-id="168cc-118">asp-action</span></span>

<span data-ttu-id="168cc-119">`asp-action`含まれているコント ローラー アクション メソッドの名前を生成された`href`です。</span><span class="sxs-lookup"><span data-stu-id="168cc-119">`asp-action` is the name of the action method in the controller that will be included in the generated `href`.</span></span> <span data-ttu-id="168cc-120">たとえば、次のコードは、生成された設定`href`スピーカー詳細ページを指します。</span><span class="sxs-lookup"><span data-stu-id="168cc-120">For example, the following code set the generated `href` to point to the speaker detail page:</span></span>

```html
<a asp-controller="Speaker" asp-action="Detail">Speaker Detail</a>
```

<span data-ttu-id="168cc-121">生成されたマークアップになります。</span><span class="sxs-lookup"><span data-stu-id="168cc-121">The generated markup will be:</span></span>

```html
<a href="/Speaker/Detail">Speaker Detail</a>
```

<span data-ttu-id="168cc-122">ない場合は`asp-controller`属性が指定されて、現在のビューを実行するビューを呼び出す既定のコント ローラーが使用されます。</span><span class="sxs-lookup"><span data-stu-id="168cc-122">If no `asp-controller` attribute is specified, the default controller calling the view executing the current view will be used.</span></span>  
 
<span data-ttu-id="168cc-123">場合、属性`asp-action`は`Index`、url、既定値に先行する操作は追加されず、`Index`呼び出されるメソッド。</span><span class="sxs-lookup"><span data-stu-id="168cc-123">If the attribute `asp-action` is `Index`, then no action is appended to the URL, leading to the default `Index` method being called.</span></span> <span data-ttu-id="168cc-124">アクションを指定 (または既定値) で参照されているコント ローラーに存在する必要があります`asp-controller`です。</span><span class="sxs-lookup"><span data-stu-id="168cc-124">The action specified (or defaulted), must exist in the controller referenced in `asp-controller`.</span></span>

### <a name="asp-page"></a><span data-ttu-id="168cc-125">asp ページ</span><span class="sxs-lookup"><span data-stu-id="168cc-125">asp-page</span></span>

<span data-ttu-id="168cc-126">使用して、`asp-page`特定ページを指す URL を設定するアンカー タグ内の属性です。</span><span class="sxs-lookup"><span data-stu-id="168cc-126">Use the `asp-page` attribute in an anchor tag to set its URL to point to a specific page.</span></span> <span data-ttu-id="168cc-127">スラッシュでページ名の前に付ける「/」の URL を作成します。</span><span class="sxs-lookup"><span data-stu-id="168cc-127">Prefixing the page name with a forward slash "/" creates the URL.</span></span> <span data-ttu-id="168cc-128">以下のサンプル内の URL は、現在のディレクトリに「スピーカー」ページを指します。</span><span class="sxs-lookup"><span data-stu-id="168cc-128">The URL in the sample below points to the "Speaker" page in the current directory.</span></span>

```cshtml
<a asp-page="/Speakers">All Speakers</a>
```

<span data-ttu-id="168cc-129">`asp-page`上記のコード サンプルでの属性が次のスニペットは、次のようなビューでの HTML 出力を表示します。</span><span class="sxs-lookup"><span data-stu-id="168cc-129">The `asp-page` attribute in the previous code sample renders HTML output in the view that's similar to the following snippet:</span></span>

```html
<a href="/items?page=%2FSpeakers">Speakers</a>
```

<span data-ttu-id="168cc-130">`asp-page`属性と同時に、 `asp-route`、 `asp-controller`、および`asp-action`属性。</span><span class="sxs-lookup"><span data-stu-id="168cc-130">The `asp-page` attribute is mutually exclusive with the `asp-route`, `asp-controller`, and `asp-action` attributes.</span></span> <span data-ttu-id="168cc-131">ただし、`asp-page`で使用できる`asp-route-id`を制御するルーティングでは、次のコード サンプルを示します。</span><span class="sxs-lookup"><span data-stu-id="168cc-131">However, `asp-page` can be used with `asp-route-id` to control routing, as the following code sample demonstrates:</span></span>

```cshtml
<a asp-page="/Speaker" asp-route-id="@speaker.Id">View Speaker</a>
```

<span data-ttu-id="168cc-132">`asp-route-id`次の出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="168cc-132">The `asp-route-id` produces the following output:</span></span>

```html
https://localhost:44399/Speakers/Index/2?page=%2FSpeaker
```

> [!NOTE]
> <span data-ttu-id="168cc-133">使用する、 `asp-page` Razor ページで、Url 属性があります相対パスでは、たとえば`"./Speaker"`します。</span><span class="sxs-lookup"><span data-stu-id="168cc-133">To use the `asp-page` attribute in Razor Pages, the URLs must be a relative path, for example `"./Speaker"`.</span></span> <span data-ttu-id="168cc-134">内の相対パス、`asp-page`属性は MVC ビューでは使用できません。</span><span class="sxs-lookup"><span data-stu-id="168cc-134">Relative paths in the `asp-page` attribute are not available in MVC views.</span></span> <span data-ttu-id="168cc-135">代わりに、MVC ビューの「/」の構文を使用します。</span><span class="sxs-lookup"><span data-stu-id="168cc-135">Use the "/" syntax for MVC views instead.</span></span>

### <a name="asp-route-value"></a><span data-ttu-id="168cc-136">asp-route-{value}</span><span class="sxs-lookup"><span data-stu-id="168cc-136">asp-route-{value}</span></span>

<span data-ttu-id="168cc-137">`asp-route-`次のワイルドカード ルート プレフィックス。</span><span class="sxs-lookup"><span data-stu-id="168cc-137">`asp-route-` is a wild card route prefix.</span></span> <span data-ttu-id="168cc-138">任意の値は、潜在的なルートのパラメーターとして解釈されます末尾ダッシュ後に配置します。</span><span class="sxs-lookup"><span data-stu-id="168cc-138">Any value you put after the trailing dash will be interpreted as a potential route parameter.</span></span> <span data-ttu-id="168cc-139">既定のルートが見つからない場合は、要求のパラメーターと値として生成された href にこのルート プレフィックスが追加されます。</span><span class="sxs-lookup"><span data-stu-id="168cc-139">If a default route isn't found, this route prefix will be appended to the generated href as a request parameter and value.</span></span> <span data-ttu-id="168cc-140">それ以外の場合、ルート テンプレートで置き換えられますされます。</span><span class="sxs-lookup"><span data-stu-id="168cc-140">Otherwise it will be substituted in the route template.</span></span>

<span data-ttu-id="168cc-141">前提とするとがコント ローラー メソッドように定義します。</span><span class="sxs-lookup"><span data-stu-id="168cc-141">Assuming you have a controller method defined as follows:</span></span>

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

<span data-ttu-id="168cc-142">定義されている既定のルート テンプレートがあると、 *Startup.cs*次のようにします。</span><span class="sxs-lookup"><span data-stu-id="168cc-142">And have the default route template defined in your *Startup.cs* as follows:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
});

```

<span data-ttu-id="168cc-143">**Cshtml** 、アンカー タグ ヘルパーを使用する必要のあるファイル、**スピーカー**ビューに、コント ローラーから渡されたモデル パラメーターを次に示します。</span><span class="sxs-lookup"><span data-stu-id="168cc-143">The **cshtml** file that contains the Anchor Tag Helper necessary to use the **speaker** model parameter passed in from the controller to the view is as follows:</span></span>

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-id=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

<span data-ttu-id="168cc-144">生成される HTML れます次のように**id**で既定のルートが見つかりました。</span><span class="sxs-lookup"><span data-stu-id="168cc-144">The generated HTML will then be as follows because **id** was found in the default route.</span></span>

```html
<a href='/Speaker/Detail/12'>SpeakerId: 12</a>
```

<span data-ttu-id="168cc-145">ルート プレフィックスがあるルーティング テンプレートの一部でない場合は、次の場合と**cshtml**ファイル。</span><span class="sxs-lookup"><span data-stu-id="168cc-145">If the route prefix isn't part of the routing template found, which is the case with the following **cshtml** file:</span></span>

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-speakerid=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

<span data-ttu-id="168cc-146">生成される HTML れます次のように**speakerid**に一致するルートが見つかりませんでした。</span><span class="sxs-lookup"><span data-stu-id="168cc-146">The generated HTML will then be as follows because **speakerid** wasn't found in the route matched:</span></span>

```html
<a href='/Speaker/Detail?speakerid=12'>SpeakerId: 12</a>
```

<span data-ttu-id="168cc-147">いずれか`asp-controller`または`asp-action`が指定されていないがで同じ既定の処理の後に、`asp-route`属性。</span><span class="sxs-lookup"><span data-stu-id="168cc-147">If either `asp-controller` or `asp-action` are not specified, then the same default processing is followed as is in the `asp-route` attribute.</span></span>

### <a name="asp-route"></a><span data-ttu-id="168cc-148">asp ルート</span><span class="sxs-lookup"><span data-stu-id="168cc-148">asp-route</span></span>

<span data-ttu-id="168cc-149">`asp-route`名前付きのルートに直接リンクする URL を作成する方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="168cc-149">`asp-route` provides a way to create a URL that links directly to a named route.</span></span> <span data-ttu-id="168cc-150">ルーティング属性を使用して、ように、ルートをということができます、`SpeakerController`で使用されると、`Evaluations`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="168cc-150">Using routing attributes, a route can be named as shown in the `SpeakerController` and used in its `Evaluations` method.</span></span>

<span data-ttu-id="168cc-151">`Name = "speakerevals"`アンカー タグ ヘルパーのルート URL を使用してそのコント ローラーのメソッドを直接生成するように指示`/Speaker/Evaluations`です。</span><span class="sxs-lookup"><span data-stu-id="168cc-151">`Name = "speakerevals"` tells the Anchor Tag Helper to generate a route directly to that controller method using the URL `/Speaker/Evaluations`.</span></span> <span data-ttu-id="168cc-152">場合`asp-controller`または`asp-action`が他に指定されている`asp-route`、生成されるルートできない可能性がありますが予期したものとします。</span><span class="sxs-lookup"><span data-stu-id="168cc-152">If `asp-controller` or `asp-action` is specified in addition to `asp-route`, the route generated may not be what you expect.</span></span> <span data-ttu-id="168cc-153">`asp-route`属性のいずれかで使用できない`asp-controller`または`asp-action`ルート競合を回避します。</span><span class="sxs-lookup"><span data-stu-id="168cc-153">`asp-route` shouldn't be used with either of the attributes `asp-controller` or `asp-action` to avoid a route conflict.</span></span>

### <a name="asp-all-route-data"></a><span data-ttu-id="168cc-154">asp-all-route-data</span><span class="sxs-lookup"><span data-stu-id="168cc-154">asp-all-route-data</span></span>

<span data-ttu-id="168cc-155">`asp-all-route-data`ここで、キーは、パラメーター名と値は、そのキーに関連付けられている値のキー値のペアのディクショナリを作成できます。</span><span class="sxs-lookup"><span data-stu-id="168cc-155">`asp-all-route-data` allows creating a dictionary of key value pairs where the key is the parameter name and the value is the value associated with that key.</span></span>

<span data-ttu-id="168cc-156">次の例に、としてインライン ディクショナリが作成され、razor ビューにデータが渡されます。</span><span class="sxs-lookup"><span data-stu-id="168cc-156">As the example below shows, an inline dictionary is created and the data is passed to the razor view.</span></span> <span data-ttu-id="168cc-157">代わりに、データも、モデルを使用して渡される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="168cc-157">As an alternative, the data could also be passed in with your model.</span></span>

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

<span data-ttu-id="168cc-158">上記のコードには、次の URL が生成されます: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true</span><span class="sxs-lookup"><span data-stu-id="168cc-158">The code above generates the following URL: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true</span></span>

<span data-ttu-id="168cc-159">ときに、リンクをクリックして、コント ローラー メソッド`EvaluationsCurrent`と呼びます。</span><span class="sxs-lookup"><span data-stu-id="168cc-159">When the link is clicked, the controller method `EvaluationsCurrent` is called.</span></span> <span data-ttu-id="168cc-160">これは、そのコント ローラーがある 2 つの文字列パラメーターから作成したものと一致するために呼び出されますが、`asp-all-route-data`ディクショナリ。</span><span class="sxs-lookup"><span data-stu-id="168cc-160">It's called because that controller has two string parameters that match what has been created from the `asp-all-route-data` dictionary.</span></span>

<span data-ttu-id="168cc-161">ディクショナリ一致しているすべてのキーは、パラメーターをルーティングする場合として適切なルート上でそれらの値が置き換えられるし、要求パラメーターと一致しない値が生成されます。</span><span class="sxs-lookup"><span data-stu-id="168cc-161">If any keys in the dictionary match route parameters, those values will be substituted in the route as appropriate and the other non-matching values will be generated as request parameters.</span></span>

### <a name="asp-fragment"></a><span data-ttu-id="168cc-162">asp-fragment</span><span class="sxs-lookup"><span data-stu-id="168cc-162">asp-fragment</span></span>

<span data-ttu-id="168cc-163">`asp-fragment`URL に追加する URL フラグメントを定義します。</span><span class="sxs-lookup"><span data-stu-id="168cc-163">`asp-fragment` defines a URL fragment to append to the URL.</span></span> <span data-ttu-id="168cc-164">アンカー タグ ヘルパー追加ハッシュ文字 (#)。</span><span class="sxs-lookup"><span data-stu-id="168cc-164">The Anchor Tag Helper will add the hash character (#).</span></span> <span data-ttu-id="168cc-165">場合は、タグを作成します。</span><span class="sxs-lookup"><span data-stu-id="168cc-165">If you create a tag:</span></span>

```cshtml
<a asp-action="Evaluations" asp-controller="Speaker"  
   asp-fragment="SpeakerEvaluations">About Speaker Evals</a>
```

<span data-ttu-id="168cc-166">生成された URL になります: http://localhost/Speaker/Evaluations#SpeakerEvaluations</span><span class="sxs-lookup"><span data-stu-id="168cc-166">The generated URL will be: http://localhost/Speaker/Evaluations#SpeakerEvaluations</span></span>

<span data-ttu-id="168cc-167">ハッシュのタグは、クライアント側アプリケーションを構築するときに便利です。</span><span class="sxs-lookup"><span data-stu-id="168cc-167">Hash tags are useful when building client-side applications.</span></span> <span data-ttu-id="168cc-168">簡単なマークを付け、たとえば、JavaScript では、検索、使用できます。</span><span class="sxs-lookup"><span data-stu-id="168cc-168">They can be used for easy marking and searching in JavaScript, for example.</span></span>

### <a name="asp-area"></a><span data-ttu-id="168cc-169">asp 領域</span><span class="sxs-lookup"><span data-stu-id="168cc-169">asp-area</span></span>

<span data-ttu-id="168cc-170">`asp-area`ASP.NET Core は適切なルートの設定を使用して領域の名前を設定します。</span><span class="sxs-lookup"><span data-stu-id="168cc-170">`asp-area` sets the area name that ASP.NET Core uses to set the appropriate route.</span></span> <span data-ttu-id="168cc-171">どの区分属性によって、ルートの再マップの例を以下に示します。</span><span class="sxs-lookup"><span data-stu-id="168cc-171">Below are examples of how the area attribute causes a remapping of routes.</span></span> <span data-ttu-id="168cc-172">設定`asp-area`ブログに、ディレクトリをプレフィックス`Areas/Blogs`関連付けられているコント ローラーとこのアンカー タグのビューのルートにします。</span><span class="sxs-lookup"><span data-stu-id="168cc-172">Setting `asp-area` to Blogs prefixes the directory `Areas/Blogs` to the routes of the associated controllers and views for this anchor tag.</span></span>

* <span data-ttu-id="168cc-173">プロジェクト名</span><span class="sxs-lookup"><span data-stu-id="168cc-173">Project name</span></span>
  * <span data-ttu-id="168cc-174">wwwroot</span><span class="sxs-lookup"><span data-stu-id="168cc-174">wwwroot</span></span>
  * <span data-ttu-id="168cc-175">区分</span><span class="sxs-lookup"><span data-stu-id="168cc-175">Areas</span></span>
    * <span data-ttu-id="168cc-176">ブログ</span><span class="sxs-lookup"><span data-stu-id="168cc-176">Blogs</span></span>
      * <span data-ttu-id="168cc-177">コント ローラー</span><span class="sxs-lookup"><span data-stu-id="168cc-177">Controllers</span></span>
        * <span data-ttu-id="168cc-178">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="168cc-178">HomeController.cs</span></span>
      * <span data-ttu-id="168cc-179">ビュー</span><span class="sxs-lookup"><span data-stu-id="168cc-179">Views</span></span>
        * <span data-ttu-id="168cc-180">Home</span><span class="sxs-lookup"><span data-stu-id="168cc-180">Home</span></span>
          * <span data-ttu-id="168cc-181">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="168cc-181">Index.cshtml</span></span>
          * <span data-ttu-id="168cc-182">AboutBlog.cshtml</span><span class="sxs-lookup"><span data-stu-id="168cc-182">AboutBlog.cshtml</span></span>
  * <span data-ttu-id="168cc-183">コント ローラー</span><span class="sxs-lookup"><span data-stu-id="168cc-183">Controllers</span></span>

<span data-ttu-id="168cc-184">などの無効である area タグを指定する```area="Blogs"```参照するときに、```AboutBlog.cshtml```ファイルは、次のようになりますアンカー タグ ヘルパーの使用します。</span><span class="sxs-lookup"><span data-stu-id="168cc-184">Specifying an area tag that's valid, such as ```area="Blogs"``` when referencing the ```AboutBlog.cshtml``` file will look like the following using the Anchor Tag Helper.</span></span>

```cshtml
<a asp-action="AboutBlog" asp-controller="Home" asp-area="Blogs">Blogs About</a>
```

<span data-ttu-id="168cc-185">生成される HTML では、領域セグメントが含まれますされ、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="168cc-185">The generated HTML will include the areas segment and will be as follows:</span></span>

```html
<a href="/Blogs/Home/AboutBlog">Blogs About</a>
```

> [!TIP]
> <span data-ttu-id="168cc-186">Web アプリケーションで作業する MVC 区分のルート テンプレートは存在する場合領域への参照を含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="168cc-186">For MVC areas to work in a web application, the route template must include a reference to the area if it exists.</span></span> <span data-ttu-id="168cc-187">そのテンプレートでは、2 番目のパラメーターの`routes.MapRoute`メソッド呼び出しとして表示されます。`template: '"{area:exists}/{controller=Home}/{action=Index}"'`</span><span class="sxs-lookup"><span data-stu-id="168cc-187">That template, which is the second parameter of the `routes.MapRoute` method call, will appear as: `template: '"{area:exists}/{controller=Home}/{action=Index}"'`</span></span>

### <a name="asp-protocol"></a><span data-ttu-id="168cc-188">asp-protocol</span><span class="sxs-lookup"><span data-stu-id="168cc-188">asp-protocol</span></span>

<span data-ttu-id="168cc-189">`asp-protocol` 、プロトコルを指定するのには (など`https`)、URL にします。</span><span class="sxs-lookup"><span data-stu-id="168cc-189">The `asp-protocol` is for specifying a protocol (such as `https`) in your URL.</span></span> <span data-ttu-id="168cc-190">プロトコルを含むアンカー タグ ヘルパーの使用例は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="168cc-190">An example Anchor Tag Helper that includes the protocol will look as follows:</span></span>

```<a asp-protocol="https" asp-action="About" asp-controller="Home">About</a>```

<span data-ttu-id="168cc-191">次のように HTML を生成します。</span><span class="sxs-lookup"><span data-stu-id="168cc-191">and will generate HTML as follows:</span></span>

```<a href="https://localhost/Home/About">About</a>```

<span data-ttu-id="168cc-192">ドメインの例では、localhost がアンカー タグ ヘルパーは、URL を生成するときに、web サイトのパブリック ドメインを使用します。</span><span class="sxs-lookup"><span data-stu-id="168cc-192">The domain in the example is localhost, but the Anchor Tag Helper uses the website's public domain when generating the URL.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="168cc-193">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="168cc-193">Additional resources</span></span>

* [<span data-ttu-id="168cc-194">領域</span><span class="sxs-lookup"><span data-stu-id="168cc-194">Areas</span></span>](xref:mvc/controllers/areas)
