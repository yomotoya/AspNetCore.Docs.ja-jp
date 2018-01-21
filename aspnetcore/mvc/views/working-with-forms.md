---
title: "ASP.NET Core でのフォームにタグ ヘルパー"
author: rick-anderson
description: "組み込みのフォームでタグ ヘルパーの使用について説明します。"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/working-with-forms
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9fbe2c5cb495aabee0e1f0bdb3871641efa03599
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="introduction-to-using-tag-helpers-in-forms-in-aspnet-core"></a><span data-ttu-id="b9879-103">ASP.NET Core でのフォームにタグ ヘルパーの使用の概要</span><span class="sxs-lookup"><span data-stu-id="b9879-103">Introduction to using tag helpers in forms in ASP.NET Core</span></span>

<span data-ttu-id="b9879-104">によって[Rick Anderson](https://twitter.com/RickAndMSFT)、 [Dave Paquette](https://twitter.com/Dave_Paquette)、および[Jerrie Pelser](https://github.com/jerriep)</span><span class="sxs-lookup"><span data-stu-id="b9879-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Dave Paquette](https://twitter.com/Dave_Paquette), and [Jerrie Pelser](https://github.com/jerriep)</span></span>

<span data-ttu-id="b9879-105">このドキュメントでは、フォームとフォームでよく使用される HTML 要素の操作を示します。</span><span class="sxs-lookup"><span data-stu-id="b9879-105">This document demonstrates working with Forms and the HTML elements commonly used on a Form.</span></span> <span data-ttu-id="b9879-106">HTML[フォーム](https://www.w3.org/TR/html401/interact/forms.html)要素は、主要なメカニズム web アプリを使用してポストバックをサーバーにデータを提供します。</span><span class="sxs-lookup"><span data-stu-id="b9879-106">The HTML [Form](https://www.w3.org/TR/html401/interact/forms.html) element provides the primary mechanism web apps use to post back data to the server.</span></span> <span data-ttu-id="b9879-107">このドキュメントのほとんどについて説明します[タグ ヘルパー](tag-helpers/intro.md)とどのように役立つ生産的に堅牢な HTML フォームを作成します。</span><span class="sxs-lookup"><span data-stu-id="b9879-107">Most of this document describes [Tag Helpers](tag-helpers/intro.md) and how they can help you productively create robust HTML forms.</span></span> <span data-ttu-id="b9879-108">読むことをお勧め[タグ ヘルパーの概要](tag-helpers/intro.md)このドキュメントを読む前にします。</span><span class="sxs-lookup"><span data-stu-id="b9879-108">We recommend you read [Introduction to Tag Helpers](tag-helpers/intro.md) before you read this document.</span></span>

<span data-ttu-id="b9879-109">多くの場合、HTML ヘルパー、別の方法を特定のタグ ヘルパーに提供するが、タグ ヘルパーの HTML ヘルパーを置き換えないし、各 HTML ヘルパーのタグ ヘルパーがないことを認識することが重要です。</span><span class="sxs-lookup"><span data-stu-id="b9879-109">In many cases, HTML Helpers provide an alternative approach to a specific Tag Helper, but it's important to recognize that Tag Helpers do not replace HTML Helpers and there is not a Tag Helper for each HTML Helper.</span></span> <span data-ttu-id="b9879-110">代わりに HTML ヘルパーが存在する場合が指定されています。</span><span class="sxs-lookup"><span data-stu-id="b9879-110">When an HTML Helper alternative exists, it is mentioned.</span></span>

<a name="my-asp-route-param-ref-label"></a>

## <a name="the-form-tag-helper"></a><span data-ttu-id="b9879-111">フォーム タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="b9879-111">The Form Tag Helper</span></span>

<span data-ttu-id="b9879-112">[フォーム](https://www.w3.org/TR/html401/interact/forms.html)タグ ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="b9879-112">The [Form](https://www.w3.org/TR/html401/interact/forms.html) Tag Helper:</span></span>

* <span data-ttu-id="b9879-113">HTML を生成[\<フォーム >](https://www.w3.org/TR/html401/interact/forms.html) `action` MVC コント ローラーのアクションまたは名前付きのルートの属性の値</span><span class="sxs-lookup"><span data-stu-id="b9879-113">Generates the HTML [\<FORM>](https://www.w3.org/TR/html401/interact/forms.html) `action` attribute value for a MVC controller action or named route</span></span>

* <span data-ttu-id="b9879-114">非表示を生成[要求の検証トークン](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages)クロスサイト リクエスト フォージェリを防ぐために (を使用すると、 `[ValidateAntiForgeryToken]` HTTP Post のアクション メソッドの属性)</span><span class="sxs-lookup"><span data-stu-id="b9879-114">Generates a hidden [Request Verification Token](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) to prevent cross-site request forgery (when used with the `[ValidateAntiForgeryToken]` attribute in the HTTP Post action method)</span></span>

* <span data-ttu-id="b9879-115">提供、`asp-route-<Parameter Name>`属性に、ここで`<Parameter Name>`がルートの値に追加します。</span><span class="sxs-lookup"><span data-stu-id="b9879-115">Provides the `asp-route-<Parameter Name>` attribute, where `<Parameter Name>` is added to the route values.</span></span> <span data-ttu-id="b9879-116">`routeValues`パラメーター`Html.BeginForm`と`Html.BeginRouteForm`同様の機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="b9879-116">The  `routeValues` parameters to `Html.BeginForm` and `Html.BeginRouteForm` provide similar functionality.</span></span>

* <span data-ttu-id="b9879-117">HTML ヘルパーの代替手段を持つ`Html.BeginForm`と`Html.BeginRouteForm`</span><span class="sxs-lookup"><span data-stu-id="b9879-117">Has an HTML Helper alternative `Html.BeginForm` and `Html.BeginRouteForm`</span></span>

<span data-ttu-id="b9879-118">例:</span><span class="sxs-lookup"><span data-stu-id="b9879-118">Sample:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/RegisterFormOnly.cshtml)]

<span data-ttu-id="b9879-119">フォーム タグ ヘルパーの上には、次の HTML が生成されます。</span><span class="sxs-lookup"><span data-stu-id="b9879-119">The Form Tag Helper above generates the following HTML:</span></span>

```HTML
<form method="post" action="/Demo/Register">
     <!-- Input and Submit elements -->
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
    </form>
```

<span data-ttu-id="b9879-120">MVC ランタイムによって生成、`action`フォーム タグ ヘルパーの属性から値を属性`asp-controller`と`asp-action`です。</span><span class="sxs-lookup"><span data-stu-id="b9879-120">The MVC runtime generates the `action` attribute value from the Form Tag Helper attributes `asp-controller` and `asp-action`.</span></span> <span data-ttu-id="b9879-121">フォーム タグ ヘルパー生成も非表示[要求の検証トークン](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages)クロスサイト リクエスト フォージェリを防ぐために (を使用すると、 `[ValidateAntiForgeryToken]` HTTP Post のアクション メソッドの属性)。</span><span class="sxs-lookup"><span data-stu-id="b9879-121">The Form Tag Helper also generates a hidden [Request Verification Token](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) to prevent cross-site request forgery (when used with the `[ValidateAntiForgeryToken]` attribute in the HTTP Post action method).</span></span> <span data-ttu-id="b9879-122">フォーム タグ ヘルパーがこのサービスを提供、クロスサイト リクエスト フォージェリから純粋な HTML フォームを保護するは困難です。</span><span class="sxs-lookup"><span data-stu-id="b9879-122">Protecting a pure HTML Form from cross-site request forgery is difficult, the Form Tag Helper provides this service for you.</span></span>

### <a name="using-a-named-route"></a><span data-ttu-id="b9879-123">名前付きのルートを使用します。</span><span class="sxs-lookup"><span data-stu-id="b9879-123">Using a named route</span></span>

<span data-ttu-id="b9879-124">`asp-route`タグ ヘルパー属性、html マークアップを生成できますも`action`属性。</span><span class="sxs-lookup"><span data-stu-id="b9879-124">The `asp-route` Tag Helper attribute can also generate markup for the HTML `action` attribute.</span></span> <span data-ttu-id="b9879-125">使用したアプリ、[ルート](../../fundamentals/routing.md)という`register`登録ページで、次のマークアップを使用でした。</span><span class="sxs-lookup"><span data-stu-id="b9879-125">An app with a [route](../../fundamentals/routing.md)  named `register` could use the following markup for the registration page:</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterRoute.cshtml)]

<span data-ttu-id="b9879-126">内のビューの多くは、*ビュー/アカウント*フォルダー (で新しい web アプリを作成するときに生成される*個々 のユーザー アカウント*) が含まれて、 [asp ルート-returnurl](https://docs.microsoft.com/aspnet/core/mvc/views/working-with-forms)属性。</span><span class="sxs-lookup"><span data-stu-id="b9879-126">Many of the views in the *Views/Account* folder (generated when you create a new web app with *Individual User Accounts*) contain the [asp-route-returnurl](https://docs.microsoft.com/aspnet/core/mvc/views/working-with-forms) attribute:</span></span>

```cshtml
<form asp-controller="Account" asp-action="Login"
     asp-route-returnurl="@ViewData["ReturnUrl"]"
     method="post" class="form-horizontal" role="form">
```

>[!NOTE]
><span data-ttu-id="b9879-127">組み込みのテンプレートと`returnUrl`承認済みのリソースにアクセスしようとしていますが、ことがなく認証、承認されたときに、自動的に設定されるだけです。</span><span class="sxs-lookup"><span data-stu-id="b9879-127">With the built in templates, `returnUrl` is only populated automatically when you try to access an authorized resource but are not authenticated or authorized.</span></span> <span data-ttu-id="b9879-128">未承認のアクセスを試みると、セキュリティ ミドルウェアにリダイレクトする、ログイン ページで、`returnUrl`を設定します。</span><span class="sxs-lookup"><span data-stu-id="b9879-128">When you attempt an unauthorized access, the security middleware redirects you to the login page with the `returnUrl` set.</span></span>

## <a name="the-input-tag-helper"></a><span data-ttu-id="b9879-129">入力タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="b9879-129">The Input Tag Helper</span></span>

<span data-ttu-id="b9879-130">入力タグ ヘルパー バインド HTML [\<入力 >](https://www.w3.org/wiki/HTML/Elements/input)要素で、razor ビューのモデルの式をします。</span><span class="sxs-lookup"><span data-stu-id="b9879-130">The Input Tag Helper binds an HTML [\<input>](https://www.w3.org/wiki/HTML/Elements/input) element to a model expression in your razor view.</span></span>

<span data-ttu-id="b9879-131">構文:</span><span class="sxs-lookup"><span data-stu-id="b9879-131">Syntax:</span></span>

```HTML
<input asp-for="<Expression Name>" />
```

<span data-ttu-id="b9879-132">入力タグ ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="b9879-132">The Input Tag Helper:</span></span>

* <span data-ttu-id="b9879-133">生成、`id`と`name`で指定された式の名前の HTML 属性、`asp-for`属性。</span><span class="sxs-lookup"><span data-stu-id="b9879-133">Generates the `id` and `name` HTML attributes for the expression name specified in the `asp-for` attribute.</span></span> <span data-ttu-id="b9879-134">`asp-for="Property1.Property2"` は `m => m.Property1.Property2` と同じです。</span><span class="sxs-lookup"><span data-stu-id="b9879-134">`asp-for="Property1.Property2"` is equivalent to `m => m.Property1.Property2`.</span></span> <span data-ttu-id="b9879-135">式の名前は、の使用目的は、`asp-for`属性の値。</span><span class="sxs-lookup"><span data-stu-id="b9879-135">The name of the expression is what is used for the `asp-for` attribute value.</span></span> <span data-ttu-id="b9879-136">参照してください、[式名](#expression-names)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="b9879-136">See the [Expression names](#expression-names) section for additional information.</span></span>

* <span data-ttu-id="b9879-137">HTML を設定`type`モデルの種類に基づいて値の属性と[データ注釈](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter)モデル プロパティに適用される属性</span><span class="sxs-lookup"><span data-stu-id="b9879-137">Sets the HTML `type` attribute value based on the model type and  [data annotation](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) attributes applied to the model property</span></span>

* <span data-ttu-id="b9879-138">HTML は上書きされません`type`属性値のいずれかを指定します。</span><span class="sxs-lookup"><span data-stu-id="b9879-138">Will not overwrite the HTML `type` attribute value when one is specified</span></span>

* <span data-ttu-id="b9879-139">生成[HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5)から属性を検証[データ注釈](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter)モデルのプロパティに適用される属性</span><span class="sxs-lookup"><span data-stu-id="b9879-139">Generates [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5)  validation attributes from [data annotation](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) attributes applied to model properties</span></span>

* <span data-ttu-id="b9879-140">重複する HTML ヘルパー機能を持つ`Html.TextBoxFor`と`Html.EditorFor`です。</span><span class="sxs-lookup"><span data-stu-id="b9879-140">Has an HTML Helper feature overlap with `Html.TextBoxFor` and `Html.EditorFor`.</span></span> <span data-ttu-id="b9879-141">参照してください、**入力タグ ヘルパーの HTML ヘルパー代替**詳細セクションです。</span><span class="sxs-lookup"><span data-stu-id="b9879-141">See the **HTML Helper alternatives to Input Tag Helper** section for details.</span></span>

* <span data-ttu-id="b9879-142">厳密な型指定を提供します。</span><span class="sxs-lookup"><span data-stu-id="b9879-142">Provides strong typing.</span></span> <span data-ttu-id="b9879-143">プロパティの変更の名前は、タグ ヘルパーを更新しない場合、次のようなエラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="b9879-143">If the name of the property changes and you don't update the Tag Helper you'll get an error similar to the following:</span></span>

```HTML
An error occurred during the compilation of a resource required to process
this request. Please review the following specific error details and modify
your source code appropriately.

Type expected
 'RegisterViewModel' does not contain a definition for 'Email' and no
 extension method 'Email' accepting a first argument of type 'RegisterViewModel'
 could be found (are you missing a using directive or an assembly reference?)
```

<span data-ttu-id="b9879-144">`Input`タグ ヘルパーの設定、HTML`type`属性ベースの .NET 型にします。</span><span class="sxs-lookup"><span data-stu-id="b9879-144">The `Input` Tag Helper sets the HTML `type` attribute based on the .NET type.</span></span> <span data-ttu-id="b9879-145">次の表には、いくつかの一般的な .NET 型と (すべての .NET 型が一覧に) 生成された HTML の種類が一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="b9879-145">The following table lists some common .NET types and generated HTML type (not every .NET type is listed).</span></span>

|<span data-ttu-id="b9879-146">.NET 型</span><span class="sxs-lookup"><span data-stu-id="b9879-146">.NET type</span></span>|<span data-ttu-id="b9879-147">入力の型</span><span class="sxs-lookup"><span data-stu-id="b9879-147">Input Type</span></span>|
|---|---|
|<span data-ttu-id="b9879-148">Bool</span><span class="sxs-lookup"><span data-stu-id="b9879-148">Bool</span></span>|<span data-ttu-id="b9879-149">type=”checkbox”</span><span class="sxs-lookup"><span data-stu-id="b9879-149">type=”checkbox”</span></span>|
|<span data-ttu-id="b9879-150">String</span><span class="sxs-lookup"><span data-stu-id="b9879-150">String</span></span>|<span data-ttu-id="b9879-151">type=”text”</span><span class="sxs-lookup"><span data-stu-id="b9879-151">type=”text”</span></span>|
|<span data-ttu-id="b9879-152">DateTime</span><span class="sxs-lookup"><span data-stu-id="b9879-152">DateTime</span></span>|<span data-ttu-id="b9879-153">type=”datetime”</span><span class="sxs-lookup"><span data-stu-id="b9879-153">type=”datetime”</span></span>|
|<span data-ttu-id="b9879-154">Byte</span><span class="sxs-lookup"><span data-stu-id="b9879-154">Byte</span></span>|<span data-ttu-id="b9879-155">type=”number”</span><span class="sxs-lookup"><span data-stu-id="b9879-155">type=”number”</span></span>|
|<span data-ttu-id="b9879-156">Int</span><span class="sxs-lookup"><span data-stu-id="b9879-156">Int</span></span>|<span data-ttu-id="b9879-157">type=”number”</span><span class="sxs-lookup"><span data-stu-id="b9879-157">type=”number”</span></span>|
|<span data-ttu-id="b9879-158">Single、Double</span><span class="sxs-lookup"><span data-stu-id="b9879-158">Single, Double</span></span>|<span data-ttu-id="b9879-159">type=”number”</span><span class="sxs-lookup"><span data-stu-id="b9879-159">type=”number”</span></span>|


<span data-ttu-id="b9879-160">次の表に、いくつかの一般的な[データ注釈](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter)を (すべての検証属性が表示されている) 特定の入力の種類にマップする入力タグ ヘルパー属性。</span><span class="sxs-lookup"><span data-stu-id="b9879-160">The following table shows some common [data annotations](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) attributes that the input tag helper will map to specific input types (not every validation attribute is listed):</span></span>


|<span data-ttu-id="b9879-161">属性</span><span class="sxs-lookup"><span data-stu-id="b9879-161">Attribute</span></span>|<span data-ttu-id="b9879-162">入力の型</span><span class="sxs-lookup"><span data-stu-id="b9879-162">Input Type</span></span>|
|---|---|
|<span data-ttu-id="b9879-163">[EmailAddress]</span><span class="sxs-lookup"><span data-stu-id="b9879-163">[EmailAddress]</span></span>|<span data-ttu-id="b9879-164">type=”email”</span><span class="sxs-lookup"><span data-stu-id="b9879-164">type=”email”</span></span>|
|<span data-ttu-id="b9879-165">[Url]</span><span class="sxs-lookup"><span data-stu-id="b9879-165">[Url]</span></span>|<span data-ttu-id="b9879-166">type=”url”</span><span class="sxs-lookup"><span data-stu-id="b9879-166">type=”url”</span></span>|
|<span data-ttu-id="b9879-167">[HiddenInput]</span><span class="sxs-lookup"><span data-stu-id="b9879-167">[HiddenInput]</span></span>|<span data-ttu-id="b9879-168">type=”hidden”</span><span class="sxs-lookup"><span data-stu-id="b9879-168">type=”hidden”</span></span>|
|<span data-ttu-id="b9879-169">[Phone]</span><span class="sxs-lookup"><span data-stu-id="b9879-169">[Phone]</span></span>|<span data-ttu-id="b9879-170">type=”tel”</span><span class="sxs-lookup"><span data-stu-id="b9879-170">type=”tel”</span></span>|
|<span data-ttu-id="b9879-171">[DataType(DataType.Password)]</span><span class="sxs-lookup"><span data-stu-id="b9879-171">[DataType(DataType.Password)]</span></span>| <span data-ttu-id="b9879-172">type=”password”</span><span class="sxs-lookup"><span data-stu-id="b9879-172">type=”password”</span></span>|
|<span data-ttu-id="b9879-173">[DataType(DataType.Date)]</span><span class="sxs-lookup"><span data-stu-id="b9879-173">[DataType(DataType.Date)]</span></span>| <span data-ttu-id="b9879-174">type=”date”</span><span class="sxs-lookup"><span data-stu-id="b9879-174">type=”date”</span></span>|
|<span data-ttu-id="b9879-175">[DataType(DataType.Time)]</span><span class="sxs-lookup"><span data-stu-id="b9879-175">[DataType(DataType.Time)]</span></span>| <span data-ttu-id="b9879-176">type=”time”</span><span class="sxs-lookup"><span data-stu-id="b9879-176">type=”time”</span></span>|


<span data-ttu-id="b9879-177">例:</span><span class="sxs-lookup"><span data-stu-id="b9879-177">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/RegisterInput.cshtml)]

<span data-ttu-id="b9879-178">上記のコードは、次の HTML を生成します。</span><span class="sxs-lookup"><span data-stu-id="b9879-178">The code above generates the following HTML:</span></span>

```HTML
  <form method="post" action="/Demo/RegisterInput">
       Email:
       <input type="email" data-val="true"
              data-val-email="The Email Address field is not a valid e-mail address."
              data-val-required="The Email Address field is required."
              id="Email" name="Email" value="" /> <br>
       Password:
       <input type="password" data-val="true"
              data-val-required="The Password field is required."
              id="Password" name="Password" /><br>
       <button type="submit">Register</button>
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
   </form>
```

<span data-ttu-id="b9879-179">データの注釈に適用される、`Email`と`Password`プロパティは、モデルのメタデータを生成します。</span><span class="sxs-lookup"><span data-stu-id="b9879-179">The data annotations applied to the `Email` and `Password` properties generate metadata on the model.</span></span> <span data-ttu-id="b9879-180">入力タグ ヘルパーは、モデルのメタデータを消費し、生成[HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-val-*`属性 (を参照してください[モデルの検証](../models/validation.md))。</span><span class="sxs-lookup"><span data-stu-id="b9879-180">The Input Tag Helper consumes the model metadata and produces [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-val-*` attributes (see [Model Validation](../models/validation.md)).</span></span> <span data-ttu-id="b9879-181">これらの属性では、入力フィールドにアタッチする検証コントロールについて説明します。</span><span class="sxs-lookup"><span data-stu-id="b9879-181">These attributes describe the validators to attach to the input fields.</span></span> <span data-ttu-id="b9879-182">これにより、控えめな HTML5 および[jQuery](https://jquery.com/)検証します。</span><span class="sxs-lookup"><span data-stu-id="b9879-182">This provides unobtrusive HTML5 and [jQuery](https://jquery.com/) validation.</span></span> <span data-ttu-id="b9879-183">控えめな属性の形式である`data-val-rule="Error Message"`ここで、ルールは、検証規則の名前 (など`data-val-required`、 `data-val-email`、 `data-val-maxlength`, などです)。エラー メッセージは、属性で提供されるの値として表示されます、`data-val-rule`属性。</span><span class="sxs-lookup"><span data-stu-id="b9879-183">The unobtrusive attributes have the format `data-val-rule="Error Message"`, where rule is the name of the validation rule (such as `data-val-required`, `data-val-email`, `data-val-maxlength`, etc.) If an error message is provided in the attribute, it is displayed as the value for the `data-val-rule` attribute.</span></span> <span data-ttu-id="b9879-184">また、フォームの属性がある`data-val-ruleName-argumentName="argumentValue"`など、そのルールに関する追加情報を提供する`data-val-maxlength-max="1024"`です。</span><span class="sxs-lookup"><span data-stu-id="b9879-184">There are also attributes of the form `data-val-ruleName-argumentName="argumentValue"` that provide additional details about the rule, for example, `data-val-maxlength-max="1024"` .</span></span>

### <a name="html-helper-alternatives-to-input-tag-helper"></a><span data-ttu-id="b9879-185">入力タグ ヘルパーの代替 HTML ヘルパー</span><span class="sxs-lookup"><span data-stu-id="b9879-185">HTML Helper alternatives to Input Tag Helper</span></span>

<span data-ttu-id="b9879-186">`Html.TextBox`、 `Html.TextBoxFor`、`Html.Editor`と`Html.EditorFor`入力タグ ヘルパーの機能が重複しています。</span><span class="sxs-lookup"><span data-stu-id="b9879-186">`Html.TextBox`, `Html.TextBoxFor`, `Html.Editor` and `Html.EditorFor` have overlapping features with the Input Tag Helper.</span></span> <span data-ttu-id="b9879-187">入力タグ ヘルパーは自動的に設定されている、`type`属性です。`Html.TextBox`と`Html.TextBoxFor`は表示されません。</span><span class="sxs-lookup"><span data-stu-id="b9879-187">The Input Tag Helper will automatically set the `type` attribute; `Html.TextBox` and `Html.TextBoxFor` will not.</span></span> <span data-ttu-id="b9879-188">`Html.Editor``Html.EditorFor`コレクション、複合オブジェクト、テンプレートの処理、入力タグ ヘルパーはありません。</span><span class="sxs-lookup"><span data-stu-id="b9879-188">`Html.Editor` and `Html.EditorFor` handle collections, complex objects and templates; the Input Tag Helper does not.</span></span> <span data-ttu-id="b9879-189">入力タグ ヘルパー`Html.EditorFor`と`Html.TextBoxFor`厳密に型指定された (、ラムダ式を使用)。`Html.TextBox`と`Html.Editor`されない (式の名前を使用)。</span><span class="sxs-lookup"><span data-stu-id="b9879-189">The Input Tag Helper, `Html.EditorFor`  and  `Html.TextBoxFor` are strongly typed (they use lambda expressions); `Html.TextBox` and `Html.Editor` are not (they use expression names).</span></span>

### <a name="htmlattributes"></a><span data-ttu-id="b9879-190">HtmlAttributes</span><span class="sxs-lookup"><span data-stu-id="b9879-190">HtmlAttributes</span></span>

<span data-ttu-id="b9879-191">`@Html.Editor()`および`@Html.EditorFor()`特殊なを使用して`ViewDataDictionary`という名前のエントリ`htmlAttributes`既定のテンプレートを実行するときにします。</span><span class="sxs-lookup"><span data-stu-id="b9879-191">`@Html.Editor()` and `@Html.EditorFor()` use a special `ViewDataDictionary` entry named `htmlAttributes` when executing their default templates.</span></span> <span data-ttu-id="b9879-192">この動作は、必要に応じてを補う`additionalViewData`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="b9879-192">This behavior is optionally augmented using `additionalViewData` parameters.</span></span> <span data-ttu-id="b9879-193">キー"htmlAttributes"は区別されません。</span><span class="sxs-lookup"><span data-stu-id="b9879-193">The key "htmlAttributes" is case-insensitive.</span></span> <span data-ttu-id="b9879-194">キー"htmlAttributes"と同じように処理される、`htmlAttributes`入力のようなヘルパーにオブジェクトが渡される`@Html.TextBox()`です。</span><span class="sxs-lookup"><span data-stu-id="b9879-194">The key "htmlAttributes" is handled similarly to the `htmlAttributes` object passed to input helpers like `@Html.TextBox()`.</span></span>

```HTML
@Html.EditorFor(model => model.YourProperty, 
  new { htmlAttributes = new { @class="myCssClass", style="Width:100px" } })
```

### <a name="expression-names"></a><span data-ttu-id="b9879-195">式の名前</span><span class="sxs-lookup"><span data-stu-id="b9879-195">Expression names</span></span>

<span data-ttu-id="b9879-196">`asp-for`属性値は、`ModelExpression`ラムダ式の右側にあるとします。</span><span class="sxs-lookup"><span data-stu-id="b9879-196">The `asp-for` attribute value is a `ModelExpression` and the right hand side of a lambda expression.</span></span> <span data-ttu-id="b9879-197">したがって、`asp-for="Property1"`なります`m => m.Property1`は生成されたコード内でプレフィックスする必要はありません`Model`です。</span><span class="sxs-lookup"><span data-stu-id="b9879-197">Therefore, `asp-for="Property1"` becomes `m => m.Property1` in the generated code which is why you don't need to prefix with `Model`.</span></span> <span data-ttu-id="b9879-198">使用することができます、"@"文字の前に移動して、インライン式、 `m.`:</span><span class="sxs-lookup"><span data-stu-id="b9879-198">You can use the "@" character to start an inline expression and move before the `m.`:</span></span>

```HTML
@{
       var joe = "Joe";
   }
   <input asp-for="@joe" />
```

<span data-ttu-id="b9879-199">次が生成されます。</span><span class="sxs-lookup"><span data-stu-id="b9879-199">Generates the following:</span></span>

```HTML
<input type="text" id="joe" name="joe" value="Joe" />
```

<span data-ttu-id="b9879-200">コレクションのプロパティを持つ`asp-for="CollectionProperty[23].Member"`と同じ名前が生成されます`asp-for="CollectionProperty[i].Member"`とき`i`プロパティ値を持つ`23`します。</span><span class="sxs-lookup"><span data-stu-id="b9879-200">With collection properties, `asp-for="CollectionProperty[23].Member"` generates the same name as `asp-for="CollectionProperty[i].Member"` when `i` has the value `23`.</span></span>

### <a name="navigating-child-properties"></a><span data-ttu-id="b9879-201">子のプロパティを移動します。</span><span class="sxs-lookup"><span data-stu-id="b9879-201">Navigating child properties</span></span>

<span data-ttu-id="b9879-202">ビュー モデルのプロパティのパスを使用して子プロパティに移動することもできます。</span><span class="sxs-lookup"><span data-stu-id="b9879-202">You can also navigate to child properties using the property path of the view model.</span></span> <span data-ttu-id="b9879-203">子を含む複雑なモデル クラスを考えます`Address`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="b9879-203">Consider a more complex model class that contains a child `Address` property.</span></span>

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/AddressViewModel.cs?highlight=1,2,3,4&range=5-8)]

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/RegisterAddressViewModel.cs?highlight=8&range=5-13)]

<span data-ttu-id="b9879-204">バインド ビューでは、 `Address.AddressLine1`:</span><span class="sxs-lookup"><span data-stu-id="b9879-204">In the view, we bind to `Address.AddressLine1`:</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterAddress.cshtml?highlight=6)]

<span data-ttu-id="b9879-205">次の HTML が生成`Address.AddressLine1`:</span><span class="sxs-lookup"><span data-stu-id="b9879-205">The following HTML is generated for `Address.AddressLine1`:</span></span>

```HTML
<input type="text" id="Address_AddressLine1" name="Address.AddressLine1" value="" />
```

### <a name="expression-names-and-collections"></a><span data-ttu-id="b9879-206">式の名前とコレクション</span><span class="sxs-lookup"><span data-stu-id="b9879-206">Expression names and Collections</span></span>

<span data-ttu-id="b9879-207">サンプルは、配列を含むモデル`Colors`:</span><span class="sxs-lookup"><span data-stu-id="b9879-207">Sample, a model containing an array of `Colors`:</span></span>

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/Person.cs?highlight=3&range=5-10)]

<span data-ttu-id="b9879-208">操作方法:</span><span class="sxs-lookup"><span data-stu-id="b9879-208">The action method:</span></span>

```csharp
public IActionResult Edit(int id, int colorIndex)
   {
       ViewData["Index"] = colorIndex;
       return View(GetPerson(id));
   }
```

<span data-ttu-id="b9879-209">次の Razor は、特定のアクセス方法を示しています。`Color`要素。</span><span class="sxs-lookup"><span data-stu-id="b9879-209">The following Razor shows how you access a specific `Color` element:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/EditColor.cshtml)]

<span data-ttu-id="b9879-210">*Views/Shared/EditorTemplates/String.cshtml*テンプレート。</span><span class="sxs-lookup"><span data-stu-id="b9879-210">The *Views/Shared/EditorTemplates/String.cshtml* template:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/String.cshtml)]

<span data-ttu-id="b9879-211">サンプルを使用して`List<T>`:</span><span class="sxs-lookup"><span data-stu-id="b9879-211">Sample using `List<T>`:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/ToDoItem.cs?range=3-8)]

<span data-ttu-id="b9879-212">次の Razor は、コレクションに対する反復処理する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="b9879-212">The following Razor shows how to iterate over a collection:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/Edit.cshtml)]

<span data-ttu-id="b9879-213">*Views/Shared/EditorTemplates/ToDoItem.cshtml*テンプレート。</span><span class="sxs-lookup"><span data-stu-id="b9879-213">The *Views/Shared/EditorTemplates/ToDoItem.cshtml* template:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/ToDoItem.cshtml)]


>[!NOTE]
><span data-ttu-id="b9879-214">常に使用する`for`(および*いない* `foreach`) リストを反復処理します。</span><span class="sxs-lookup"><span data-stu-id="b9879-214">Always use `for` (and *not* `foreach`) to iterate over a list.</span></span> <span data-ttu-id="b9879-215">LINQ でインデクサーを評価する式は負荷のかかるし、最小限に抑える必要があります。</span><span class="sxs-lookup"><span data-stu-id="b9879-215">Evaluating an indexer in a LINQ expression can be expensive and should be minimized.</span></span>

&nbsp;

>[!NOTE]
><span data-ttu-id="b9879-216">上記のコメント付きのサンプル コードでラムダ式を交換する方法を示しています、`@`各にアクセスする演算子`ToDoItem`一覧にします。</span><span class="sxs-lookup"><span data-stu-id="b9879-216">The commented sample code above shows how you would replace the lambda expression with the `@` operator to access each `ToDoItem` in the list.</span></span>

## <a name="the-textarea-tag-helper"></a><span data-ttu-id="b9879-217">Textarea タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="b9879-217">The Textarea Tag Helper</span></span>

<span data-ttu-id="b9879-218">`Textarea Tag Helper`タグ ヘルパーは、入力タグ ヘルパーに似ています。</span><span class="sxs-lookup"><span data-stu-id="b9879-218">The `Textarea Tag Helper` tag helper is  similar to the Input Tag Helper.</span></span>

* <span data-ttu-id="b9879-219">生成、`id`と`name`属性、およびデータの検証属性をモデルから、 [ \<textarea >](https://www.w3.org/wiki/HTML/Elements/textarea)要素。</span><span class="sxs-lookup"><span data-stu-id="b9879-219">Generates the `id` and `name` attributes, and the data validation attributes from the model for a [\<textarea>](https://www.w3.org/wiki/HTML/Elements/textarea) element.</span></span>

* <span data-ttu-id="b9879-220">厳密な型指定を提供します。</span><span class="sxs-lookup"><span data-stu-id="b9879-220">Provides strong typing.</span></span>

* <span data-ttu-id="b9879-221">HTML ヘルパーの代替方法:`Html.TextAreaFor`</span><span class="sxs-lookup"><span data-stu-id="b9879-221">HTML Helper alternative: `Html.TextAreaFor`</span></span>

<span data-ttu-id="b9879-222">例:</span><span class="sxs-lookup"><span data-stu-id="b9879-222">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/DescriptionViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterTextArea.cshtml?highlight=4)]

<span data-ttu-id="b9879-223">次の HTML が生成されます。</span><span class="sxs-lookup"><span data-stu-id="b9879-223">The following HTML is generated:</span></span>

```HTML
<form method="post" action="/Demo/RegisterTextArea">
  <textarea data-val="true"
   data-val-maxlength="The field Description must be a string or array type with a maximum length of &#x27;1024&#x27;."
   data-val-maxlength-max="1024"
   data-val-minlength="The field Description must be a string or array type with a minimum length of &#x27;5&#x27;."
   data-val-minlength-min="5"
   id="Description" name="Description">
  </textarea>
  <button type="submit">Test</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

## <a name="the-label-tag-helper"></a><span data-ttu-id="b9879-224">ラベル タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="b9879-224">The Label Tag Helper</span></span>

* <span data-ttu-id="b9879-225">ラベルのキャプションを生成し、`for`属性を[ <label> ](https://www.w3.org/wiki/HTML/Elements/label)式の名前の要素</span><span class="sxs-lookup"><span data-stu-id="b9879-225">Generates the label caption and `for` attribute on a [<label>](https://www.w3.org/wiki/HTML/Elements/label) element for an expression name</span></span>

* <span data-ttu-id="b9879-226">代わりに HTML ヘルパー:`Html.LabelFor`です。</span><span class="sxs-lookup"><span data-stu-id="b9879-226">HTML Helper alternative: `Html.LabelFor`.</span></span>

<span data-ttu-id="b9879-227">`Label Tag Helper`純粋な HTML label 要素は次の利点を提供します。</span><span class="sxs-lookup"><span data-stu-id="b9879-227">The `Label Tag Helper`  provides the following benefits over a pure HTML label element:</span></span>

* <span data-ttu-id="b9879-228">自動的に値を取得する、わかりやすいラベルから、`Display`属性。</span><span class="sxs-lookup"><span data-stu-id="b9879-228">You automatically get the descriptive label value from the `Display` attribute.</span></span> <span data-ttu-id="b9879-229">時間との組み合わせを意図した表示名を変更する可能性があります`Display`属性とタグ ヘルパーのラベルが適用されます、`Display`どこからでも使用されます。</span><span class="sxs-lookup"><span data-stu-id="b9879-229">The intended display name might change over time, and the combination of `Display` attribute and Label Tag Helper will apply the `Display` everywhere it's used.</span></span>

* <span data-ttu-id="b9879-230">ソース コードの少ないマークアップ</span><span class="sxs-lookup"><span data-stu-id="b9879-230">Less markup in source code</span></span>

* <span data-ttu-id="b9879-231">厳密なモデル プロパティを使用して型指定します。</span><span class="sxs-lookup"><span data-stu-id="b9879-231">Strong typing with the model property.</span></span>

<span data-ttu-id="b9879-232">例:</span><span class="sxs-lookup"><span data-stu-id="b9879-232">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/SimpleViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterLabel.cshtml?highlight=4)]

<span data-ttu-id="b9879-233">に対して次の HTML を生成、`<label>`要素。</span><span class="sxs-lookup"><span data-stu-id="b9879-233">The following HTML is generated for the `<label>` element:</span></span>

```HTML
<label for="Email">Email Address</label>
```

<span data-ttu-id="b9879-234">生成されたラベル タグ ヘルパーの`for`"Email"は、id の属性値に関連付けられている、`<input>`要素。</span><span class="sxs-lookup"><span data-stu-id="b9879-234">The Label Tag Helper generated the `for` attribute value of "Email", which is the ID associated with the `<input>` element.</span></span> <span data-ttu-id="b9879-235">タグ ヘルパーの一貫性のある生成`id`と`for`要素が正しく関連付けられている可能性があるようにします。</span><span class="sxs-lookup"><span data-stu-id="b9879-235">The Tag Helpers generate consistent `id` and `for` elements so they can be correctly associated.</span></span> <span data-ttu-id="b9879-236">このサンプルではキャプションを起源、`Display`属性。</span><span class="sxs-lookup"><span data-stu-id="b9879-236">The caption in this sample comes from the `Display` attribute.</span></span> <span data-ttu-id="b9879-237">モデルが含まれていなかった場合、`Display`属性、キャプションは、式のプロパティの名前になります。</span><span class="sxs-lookup"><span data-stu-id="b9879-237">If the model didn't contain a `Display` attribute, the caption would be the property name of the expression.</span></span>

## <a name="the-validation-tag-helpers"></a><span data-ttu-id="b9879-238">検証タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="b9879-238">The Validation Tag Helpers</span></span>

<span data-ttu-id="b9879-239">検証タグ ヘルパーの 2 つがあります。</span><span class="sxs-lookup"><span data-stu-id="b9879-239">There are two Validation Tag Helpers.</span></span> <span data-ttu-id="b9879-240">`Validation Message Tag Helper` (するメッセージが表示されます、検証プロパティを 1 つのモデルに)、および`Validation Summary Tag Helper`(検証エラーの概要が表示されます)。</span><span class="sxs-lookup"><span data-stu-id="b9879-240">The `Validation Message Tag Helper` (which displays a validation message for a single property on your model), and the `Validation Summary Tag Helper` (which displays a summary of validation errors).</span></span> <span data-ttu-id="b9879-241">`Input Tag Helper`入力要素に基づいてデータ モデルのクラスに注釈属性に HTML5 クライアント側の検証属性を追加します。</span><span class="sxs-lookup"><span data-stu-id="b9879-241">The `Input Tag Helper` adds HTML5 client side validation attributes to input elements based on data annotation attributes on your model classes.</span></span> <span data-ttu-id="b9879-242">検証は、サーバーも実行します。</span><span class="sxs-lookup"><span data-stu-id="b9879-242">Validation is also performed on the server.</span></span> <span data-ttu-id="b9879-243">検証タグ ヘルパーでは、検証エラーが発生したときに、これらのエラー メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="b9879-243">The Validation Tag Helper displays these error messages when a validation error occurs.</span></span>

### <a name="the-validation-message-tag-helper"></a><span data-ttu-id="b9879-244">検証メッセージ タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="b9879-244">The Validation Message Tag Helper</span></span>

* <span data-ttu-id="b9879-245">追加、 [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-valmsg-for="property"`属性を[span](https://developer.mozilla.org/docs/Web/HTML/Element/span)要素は、指定したモデルのプロパティの入力フィールドの検証エラー メッセージをアタッチします。</span><span class="sxs-lookup"><span data-stu-id="b9879-245">Adds the [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5)  `data-valmsg-for="property"` attribute to the [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) element, which attaches the validation error messages on the input field of the specified model property.</span></span> <span data-ttu-id="b9879-246">クライアント側の検証エラーが発生したときに[jQuery](https://jquery.com/)のエラー メッセージを表示、`<span>`要素。</span><span class="sxs-lookup"><span data-stu-id="b9879-246">When a client side validation error occurs, [jQuery](https://jquery.com/) displays the error message in the `<span>` element.</span></span>

* <span data-ttu-id="b9879-247">検証では、サーバー上の場所も受け取ります。</span><span class="sxs-lookup"><span data-stu-id="b9879-247">Validation also takes place on the server.</span></span> <span data-ttu-id="b9879-248">クライアント上で JavaScript が無効になる場合があり、いくつかの検証は、サーバー側でのみ実行できます。</span><span class="sxs-lookup"><span data-stu-id="b9879-248">Clients may have JavaScript disabled and some validation can only be done on the server side.</span></span>

* <span data-ttu-id="b9879-249">HTML ヘルパーの代替方法:`Html.ValidationMessageFor`</span><span class="sxs-lookup"><span data-stu-id="b9879-249">HTML Helper alternative: `Html.ValidationMessageFor`</span></span>

<span data-ttu-id="b9879-250">`Validation Message Tag Helper`と共に使用される、`asp-validation-for`された HTML 属性[span](https://developer.mozilla.org/docs/Web/HTML/Element/span)要素。</span><span class="sxs-lookup"><span data-stu-id="b9879-250">The `Validation Message Tag Helper`  is used with the `asp-validation-for` attribute on a HTML [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) element.</span></span>

```HTML
<span asp-validation-for="Email"></span>
```

<span data-ttu-id="b9879-251">検証メッセージ タグ ヘルパーは、次の HTML を生成します。</span><span class="sxs-lookup"><span data-stu-id="b9879-251">The Validation Message Tag Helper will generate the following HTML:</span></span>

```HTML
<span class="field-validation-valid"
  data-valmsg-for="Email"
  data-valmsg-replace="true"></span>
```

<span data-ttu-id="b9879-252">一般的に使用する、`Validation Message Tag Helper`後、`Input`同じプロパティ タグ ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="b9879-252">You generally use the `Validation Message Tag Helper`  after an `Input` Tag Helper for the same property.</span></span> <span data-ttu-id="b9879-253">これには、エラーが発生した入力近く検証エラー メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="b9879-253">Doing so displays any validation error messages near the input that caused the error.</span></span>

> [!NOTE]
> <span data-ttu-id="b9879-254">正しい JavaScript では、ビューが必要と[jQuery](https://jquery.com/)スクリプト クライアント側の検証のために参照します。</span><span class="sxs-lookup"><span data-stu-id="b9879-254">You must have a view with the correct JavaScript and [jQuery](https://jquery.com/) script references in place for client side validation.</span></span> <span data-ttu-id="b9879-255">参照してください[モデルの検証](../models/validation.md)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="b9879-255">See [Model Validation](../models/validation.md) for more information.</span></span>

<span data-ttu-id="b9879-256">本文として MVC がエラー メッセージが表示を配置する場合、サーバー側で検証エラー (たとえばとカスタム サーバー側の検証がある場合またはクライアント側の検証が無効になっています)、`<span>`要素。</span><span class="sxs-lookup"><span data-stu-id="b9879-256">When a server side validation error occurs (for example when you have custom server side validation or client-side validation is disabled), MVC places that error message as the body of the `<span>` element.</span></span>

```HTML
<span class="field-validation-error" data-valmsg-for="Email"
            data-valmsg-replace="true">
   The Email Address field is required.
</span>
```

### <a name="the-validation-summary-tag-helper"></a><span data-ttu-id="b9879-257">検証概要タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="b9879-257">The Validation Summary Tag Helper</span></span>

* <span data-ttu-id="b9879-258">ターゲット`<div>`要素を`asp-validation-summary`属性</span><span class="sxs-lookup"><span data-stu-id="b9879-258">Targets `<div>` elements with the `asp-validation-summary` attribute</span></span>

* <span data-ttu-id="b9879-259">HTML ヘルパーの代替方法:`@Html.ValidationSummary`</span><span class="sxs-lookup"><span data-stu-id="b9879-259">HTML Helper alternative: `@Html.ValidationSummary`</span></span>

<span data-ttu-id="b9879-260">`Validation Summary Tag Helper`検証メッセージの概要を表示するために使用します。</span><span class="sxs-lookup"><span data-stu-id="b9879-260">The `Validation Summary Tag Helper`  is used to display a summary of validation messages.</span></span> <span data-ttu-id="b9879-261">`asp-validation-summary`属性値は、次のいずれかを指定できます。</span><span class="sxs-lookup"><span data-stu-id="b9879-261">The `asp-validation-summary` attribute value can be any of the following:</span></span>

|<span data-ttu-id="b9879-262">asp-validation-summary</span><span class="sxs-lookup"><span data-stu-id="b9879-262">asp-validation-summary</span></span>|<span data-ttu-id="b9879-263">検証メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="b9879-263">Validation messages displayed</span></span>|
|--- |--- |
|<span data-ttu-id="b9879-264">ValidationSummary.All</span><span class="sxs-lookup"><span data-stu-id="b9879-264">ValidationSummary.All</span></span>|<span data-ttu-id="b9879-265">プロパティとモデルのレベル</span><span class="sxs-lookup"><span data-stu-id="b9879-265">Property and model level</span></span>|
|<span data-ttu-id="b9879-266">ValidationSummary.ModelOnly</span><span class="sxs-lookup"><span data-stu-id="b9879-266">ValidationSummary.ModelOnly</span></span>|<span data-ttu-id="b9879-267">モデル</span><span class="sxs-lookup"><span data-stu-id="b9879-267">Model</span></span>|
|<span data-ttu-id="b9879-268">ValidationSummary.None</span><span class="sxs-lookup"><span data-stu-id="b9879-268">ValidationSummary.None</span></span>|<span data-ttu-id="b9879-269">なし</span><span class="sxs-lookup"><span data-stu-id="b9879-269">None</span></span>|

### <a name="sample"></a><span data-ttu-id="b9879-270">サンプル</span><span class="sxs-lookup"><span data-stu-id="b9879-270">Sample</span></span>

<span data-ttu-id="b9879-271">次の例では、データ モデルで装飾されて`DataAnnotation`で検証エラー メッセージを生成する属性、`<input>`要素。</span><span class="sxs-lookup"><span data-stu-id="b9879-271">In the following example, the data model is decorated with `DataAnnotation` attributes, which generates validation error messages on the `<input>` element.</span></span>  <span data-ttu-id="b9879-272">検証エラーが発生したときに検証タグ ヘルパーには、エラー メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="b9879-272">When a validation error occurs, the Validation Tag Helper displays the error message:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterValidation.cshtml?highlight=4,6,8&range=1-10)]

<span data-ttu-id="b9879-273">生成された HTML (モデルが有効な場合):</span><span class="sxs-lookup"><span data-stu-id="b9879-273">The generated HTML (when the model is valid):</span></span>

```HTML
<form action="/DemoReg/Register" method="post">
  <div class="validation-summary-valid" data-valmsg-summary="true">
  <ul><li style="display:none"></li></ul></div>
  Email:  <input name="Email" id="Email" type="email" value=""
   data-val-required="The Email field is required."
   data-val-email="The Email field is not a valid e-mail address."
   data-val="true"> <br>
  <span class="field-validation-valid" data-valmsg-replace="true"
   data-valmsg-for="Email"></span><br>
  Password: <input name="Password" id="Password" type="password"
   data-val-required="The Password field is required." data-val="true"><br>
  <span class="field-validation-valid" data-valmsg-replace="true"
   data-valmsg-for="Password"></span><br>
  <button type="submit">Register</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

## <a name="the-select-tag-helper"></a><span data-ttu-id="b9879-274">Select タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="b9879-274">The Select Tag Helper</span></span>

* <span data-ttu-id="b9879-275">生成されます[選択](https://www.w3.org/wiki/HTML/Elements/select)と関連付けられた[オプション](https://www.w3.org/wiki/HTML/Elements/option)モデルのプロパティの要素。</span><span class="sxs-lookup"><span data-stu-id="b9879-275">Generates [select](https://www.w3.org/wiki/HTML/Elements/select) and associated [option](https://www.w3.org/wiki/HTML/Elements/option) elements for properties of your model.</span></span>

* <span data-ttu-id="b9879-276">HTML ヘルパーの代替手段を持つ`Html.DropDownListFor`と`Html.ListBoxFor`</span><span class="sxs-lookup"><span data-stu-id="b9879-276">Has an HTML Helper alternative `Html.DropDownListFor` and `Html.ListBoxFor`</span></span>

<span data-ttu-id="b9879-277">`Select Tag Helper` `asp-for` 、モデルのプロパティ名を指定します、[選択](https://www.w3.org/wiki/HTML/Elements/select)要素および`asp-items`を指定します、[オプション](https://www.w3.org/wiki/HTML/Elements/option)要素。</span><span class="sxs-lookup"><span data-stu-id="b9879-277">The `Select Tag Helper` `asp-for` specifies the model property  name for the [select](https://www.w3.org/wiki/HTML/Elements/select) element  and `asp-items` specifies the [option](https://www.w3.org/wiki/HTML/Elements/option) elements.</span></span>  <span data-ttu-id="b9879-278">例:</span><span class="sxs-lookup"><span data-stu-id="b9879-278">For example:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

<span data-ttu-id="b9879-279">例:</span><span class="sxs-lookup"><span data-stu-id="b9879-279">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryViewModel.cs)]

<span data-ttu-id="b9879-280">`Index`メソッドの初期化、 `CountryViewModel`、選択した国を設定およびに渡します、`Index`ビュー。</span><span class="sxs-lookup"><span data-stu-id="b9879-280">The `Index` method initializes the `CountryViewModel`, sets the selected country and passes it to the `Index` view.</span></span>

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

<span data-ttu-id="b9879-281">HTTP POST`Index`メソッドは、選択範囲を表示します。</span><span class="sxs-lookup"><span data-stu-id="b9879-281">The HTTP POST `Index` method displays the selection:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=15-27)]

<span data-ttu-id="b9879-282">`Index`ビュー。</span><span class="sxs-lookup"><span data-stu-id="b9879-282">The `Index` view:</span></span>

[!code-cshtml[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?highlight=4)]

<span data-ttu-id="b9879-283">これには、("CA"選択) で、次の HTML が生成されます。</span><span class="sxs-lookup"><span data-stu-id="b9879-283">Which generates the following HTML (with "CA" selected):</span></span>

```html
<form method="post" action="/">
     <select id="Country" name="Country">
       <option value="MX">Mexico</option>
       <option selected="selected" value="CA">Canada</option>
       <option value="US">USA</option>
     </select>
       <br /><button type="submit">Register</button>
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
   </form>
```

> [!NOTE]
> <span data-ttu-id="b9879-284">使用しないで`ViewBag`または`ViewData`選択タグ ヘルパーとします。</span><span class="sxs-lookup"><span data-stu-id="b9879-284">We do not recommend using `ViewBag` or `ViewData` with the Select Tag Helper.</span></span> <span data-ttu-id="b9879-285">ビュー モデルは、MVC のメタデータを提供することをより堅牢になり、通常それほど大きな問題です。</span><span class="sxs-lookup"><span data-stu-id="b9879-285">A view model is more robust at providing MVC metadata and generally less problematic.</span></span>

<span data-ttu-id="b9879-286">`asp-for`属性値は、特殊なケースを必要し、しない、`Model`プレフィックス、その他のタグ ヘルパー属性は (など`asp-items`)</span><span class="sxs-lookup"><span data-stu-id="b9879-286">The `asp-for` attribute value is a special case and doesn't require a `Model` prefix, the other Tag Helper attributes do (such as `asp-items`)</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

### <a name="enum-binding"></a><span data-ttu-id="b9879-287">列挙型のバインディング</span><span class="sxs-lookup"><span data-stu-id="b9879-287">Enum binding</span></span>

<span data-ttu-id="b9879-288">使用すると便利なことがよくあります`<select>`で、`enum`プロパティを生成し、`SelectListItem`からの要素、`enum`値。</span><span class="sxs-lookup"><span data-stu-id="b9879-288">It's often convenient to use `<select>` with an `enum` property and generate the `SelectListItem` elements from the `enum` values.</span></span>

<span data-ttu-id="b9879-289">例:</span><span class="sxs-lookup"><span data-stu-id="b9879-289">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnumViewModel.cs?range=3-7)]

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnum.cs)]

<span data-ttu-id="b9879-290">`GetEnumSelectList`メソッドを生成、`SelectList`列挙型のオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="b9879-290">The `GetEnumSelectList` method generates a `SelectList` object for an enum.</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEnum.cshtml?highlight=5)]

<span data-ttu-id="b9879-291">装飾できるは、列挙子リストが、`Display`豊富な UI を取得する属性。</span><span class="sxs-lookup"><span data-stu-id="b9879-291">You can decorate your enumerator list with the `Display` attribute to get a richer UI:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnum.cs?highlight=5,7)]

<span data-ttu-id="b9879-292">次の HTML が生成されます。</span><span class="sxs-lookup"><span data-stu-id="b9879-292">The following HTML is generated:</span></span>

```HTML
  <form method="post" action="/Home/IndexEnum">
         <select data-val="true" data-val-required="The EnumCountry field is required."
                 id="EnumCountry" name="EnumCountry">
             <option value="0">United Mexican States</option>
             <option value="1">United States of America</option>
             <option value="2">Canada</option>
             <option value="3">France</option>
             <option value="4">Germany</option>
             <option selected="selected" value="5">Spain</option>
         </select>
         <br /><button type="submit">Register</button>
         <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
    </form>
```

### <a name="option-group"></a><span data-ttu-id="b9879-293">オプション グループ</span><span class="sxs-lookup"><span data-stu-id="b9879-293">Option Group</span></span>

<span data-ttu-id="b9879-294">HTML [ \<optgroup >](https://www.w3.org/wiki/HTML/Elements/optgroup)ビュー モデルには、1 つまたは複数が含まれている場合、要素が生成される`SelectListGroup`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="b9879-294">The HTML  [\<optgroup>](https://www.w3.org/wiki/HTML/Elements/optgroup) element is generated when the view model contains one or more `SelectListGroup` objects.</span></span>

<span data-ttu-id="b9879-295">`CountryViewModelGroup`グループ、 `SelectListItem` "North America"および"Europe"グループに要素。</span><span class="sxs-lookup"><span data-stu-id="b9879-295">The `CountryViewModelGroup` groups the `SelectListItem` elements into the "North America" and "Europe" groups:</span></span>

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelGroup.cs?highlight=5,6,14,20,26,32,38,44&range=6-56)]

<span data-ttu-id="b9879-296">2 つのグループを次に示します。</span><span class="sxs-lookup"><span data-stu-id="b9879-296">The two groups are shown below:</span></span>

![オプション グループの例](working-with-forms/_static/grp.png)

<span data-ttu-id="b9879-298">生成される HTML。</span><span class="sxs-lookup"><span data-stu-id="b9879-298">The generated HTML:</span></span>

```HTML
 <form method="post" action="/Home/IndexGroup">
      <select id="Country" name="Country">
          <optgroup label="North America">
              <option value="MEX">Mexico</option>
              <option value="CAN">Canada</option>
              <option value="US">USA</option>
          </optgroup>
          <optgroup label="Europe">
              <option value="FR">France</option>
              <option value="ES">Spain</option>
              <option value="DE">Germany</option>
          </optgroup>
      </select>
      <br /><button type="submit">Register</button>
      <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
 </form>
```

### <a name="multiple-select"></a><span data-ttu-id="b9879-299">複数の選択</span><span class="sxs-lookup"><span data-stu-id="b9879-299">Multiple select</span></span>

<span data-ttu-id="b9879-300">タグの選択ヘルパーが自動的に生成、[複数 =「複数」](http://w3c.github.io/html-reference/select.html)属性でプロパティが指定された場合、`asp-for`属性は、`IEnumerable`です。</span><span class="sxs-lookup"><span data-stu-id="b9879-300">The Select Tag Helper  will automatically generate the [multiple = "multiple"](http://w3c.github.io/html-reference/select.html)  attribute if the property specified in the `asp-for` attribute is an `IEnumerable`.</span></span> <span data-ttu-id="b9879-301">たとえば、次のようなモデルを指定します。</span><span class="sxs-lookup"><span data-stu-id="b9879-301">For example, given the following model:</span></span>

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelIEnumerable.cs?highlight=6)]

<span data-ttu-id="b9879-302">次のビュー。</span><span class="sxs-lookup"><span data-stu-id="b9879-302">With the following view:</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexMultiSelect.cshtml?highlight=4)]

<span data-ttu-id="b9879-303">次の HTML を生成します。</span><span class="sxs-lookup"><span data-stu-id="b9879-303">Generates the following HTML:</span></span>

```HTML
<form method="post" action="/Home/IndexMultiSelect">
    <select id="CountryCodes"
    multiple="multiple"
    name="CountryCodes"><option value="MX">Mexico</option>
<option value="CA">Canada</option>
<option value="US">USA</option>
<option value="FR">France</option>
<option value="ES">Spain</option>
<option value="DE">Germany</option>
</select>
    <br /><button type="submit">Register</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

### <a name="no-selection"></a><span data-ttu-id="b9879-304">選択されていません</span><span class="sxs-lookup"><span data-stu-id="b9879-304">No selection</span></span>

<span data-ttu-id="b9879-305">複数のページで、「指定されていません」のオプションを使用して自分宛てを検索する場合は、HTML の繰り返しを排除するためのテンプレートを作成できます。</span><span class="sxs-lookup"><span data-stu-id="b9879-305">If you find yourself using the "not specified" option in multiple pages, you can create a template to eliminate repeating the HTML:</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEmptyTemplate.cshtml?highlight=5)]

<span data-ttu-id="b9879-306">*Views/Shared/EditorTemplates/CountryViewModel.cshtml*テンプレート。</span><span class="sxs-lookup"><span data-stu-id="b9879-306">The *Views/Shared/EditorTemplates/CountryViewModel.cshtml* template:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/CountryViewModel.cshtml)]

<span data-ttu-id="b9879-307">HTML を追加する[\<オプション >](https://www.w3.org/wiki/HTML/Elements/option)要素に限定されません、*選択されていない*ケースです。</span><span class="sxs-lookup"><span data-stu-id="b9879-307">Adding HTML [\<option>](https://www.w3.org/wiki/HTML/Elements/option) elements is not limited to the *No selection* case.</span></span> <span data-ttu-id="b9879-308">たとえば、次のビューとアクション メソッド上のコードに似た HTML が生成されます。</span><span class="sxs-lookup"><span data-stu-id="b9879-308">For example, the following view and action method will generate HTML similar to the code above:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/IndexOption.cshtml)]

<span data-ttu-id="b9879-309">正しい`<option>`要素が選択されます (含まれて、`selected="selected"`属性) によっては、現在`Country`値。</span><span class="sxs-lookup"><span data-stu-id="b9879-309">The correct `<option>` element will be selected ( contain the `selected="selected"` attribute) depending on the current `Country` value.</span></span>

```HTML
 <form method="post" action="/Home/IndexEmpty">
      <select id="Country" name="Country">
          <option value="">&lt;none&gt;</option>
          <option value="MX">Mexico</option>
          <option value="CA" selected="selected">Canada</option>
          <option value="US">USA</option>
      </select>
      <br /><button type="submit">Register</button>
   <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
 </form>
 ```

## <a name="additional-resources"></a><span data-ttu-id="b9879-310">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="b9879-310">Additional Resources</span></span>

* [<span data-ttu-id="b9879-311">タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="b9879-311">Tag Helpers</span></span>](tag-helpers/intro.md)

* [<span data-ttu-id="b9879-312">HTML フォーム要素</span><span class="sxs-lookup"><span data-stu-id="b9879-312">HTML Form element</span></span>](https://www.w3.org/TR/html401/interact/forms.html)

* [<span data-ttu-id="b9879-313">検証トークンの要求</span><span class="sxs-lookup"><span data-stu-id="b9879-313">Request Verification Token</span></span>](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages)

* [<span data-ttu-id="b9879-314">モデル バインド</span><span class="sxs-lookup"><span data-stu-id="b9879-314">Model Binding</span></span>](../models/model-binding.md)

* [<span data-ttu-id="b9879-315">モデルの検証</span><span class="sxs-lookup"><span data-stu-id="b9879-315">Model Validation</span></span>](../models/validation.md)

* [<span data-ttu-id="b9879-316">データ注釈</span><span class="sxs-lookup"><span data-stu-id="b9879-316">data annotations</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter)

* <span data-ttu-id="b9879-317">[このドキュメントのスニペットをコード](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/forms/sample)です。</span><span class="sxs-lookup"><span data-stu-id="b9879-317">[Code snippets for this document](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/forms/sample).</span></span>
