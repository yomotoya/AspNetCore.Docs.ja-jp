---
title: ASP.NET Core のフォームのタグ ヘルパー
author: rick-anderson
description: フォームで使用される組み込みのタグ ヘルパーについて説明します。
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/working-with-forms
ms.openlocfilehash: 9155bd54bc211c8be0678065e857f73d8a139365
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/03/2018
ms.locfileid: "32741129"
---
# <a name="tag-helpers-in-forms-in-aspnet-core"></a><span data-ttu-id="571b4-103">ASP.NET Core のフォームのタグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="571b4-103">Tag Helpers in forms in ASP.NET Core</span></span>

<span data-ttu-id="571b4-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)、[Dave Paquette](https://twitter.com/Dave_Paquette)、[Jerrie Pelser](https://github.com/jerriep)</span><span class="sxs-lookup"><span data-stu-id="571b4-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Dave Paquette](https://twitter.com/Dave_Paquette), and [Jerrie Pelser](https://github.com/jerriep)</span></span>

<span data-ttu-id="571b4-105">このドキュメントでは、フォームとフォームでよく使用される HTML 要素の使用方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="571b4-105">This document demonstrates working with Forms and the HTML elements commonly used on a Form.</span></span> <span data-ttu-id="571b4-106">HTML の [Form](https://www.w3.org/TR/html401/interact/forms.html) 要素には、Web アプリケーションからサーバーにデータをポスト バックするために使用する主要なメカニズムがあります。</span><span class="sxs-lookup"><span data-stu-id="571b4-106">The HTML [Form](https://www.w3.org/TR/html401/interact/forms.html) element provides the primary mechanism web apps use to post back data to the server.</span></span> <span data-ttu-id="571b4-107">このドキュメントでは、[タグ ヘルパー](tag-helpers/intro.md)と、タグ ヘルパーを利用して堅牢な HTML フォームを生産的に作成する方法について主に説明します。</span><span class="sxs-lookup"><span data-stu-id="571b4-107">Most of this document describes [Tag Helpers](tag-helpers/intro.md) and how they can help you productively create robust HTML forms.</span></span> <span data-ttu-id="571b4-108">このドキュメントを読む前に、[タグ ヘルパーの概要](tag-helpers/intro.md)に関するページを読むことをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="571b4-108">We recommend you read [Introduction to Tag Helpers](tag-helpers/intro.md) before you read this document.</span></span>

<span data-ttu-id="571b4-109">多くの場合、HTML ヘルパーには特定のタグ ヘルパーの代替方法が用意されていますが、タグ ヘルパーは HTML ヘルパーの置き換えではない点と、各 HTML ヘルパーに対応するタグ ヘルパーがない点を認識することが重要です。</span><span class="sxs-lookup"><span data-stu-id="571b4-109">In many cases, HTML Helpers provide an alternative approach to a specific Tag Helper, but it's important to recognize that Tag Helpers don't replace HTML Helpers and there's not a Tag Helper for each HTML Helper.</span></span> <span data-ttu-id="571b4-110">HTML ヘルパーの代替が存在する場合は記載します。</span><span class="sxs-lookup"><span data-stu-id="571b4-110">When an HTML Helper alternative exists, it's mentioned.</span></span>

<a name="my-asp-route-param-ref-label"></a>

## <a name="the-form-tag-helper"></a><span data-ttu-id="571b4-111">フォーム タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="571b4-111">The Form Tag Helper</span></span>

<span data-ttu-id="571b4-112">[フォーム](https://www.w3.org/TR/html401/interact/forms.html) タグ ヘルパー:</span><span class="sxs-lookup"><span data-stu-id="571b4-112">The [Form](https://www.w3.org/TR/html401/interact/forms.html) Tag Helper:</span></span>

* <span data-ttu-id="571b4-113">MVC コントローラー アクションまたは名前付きルートについて HTML の [\<FORM>](https://www.w3.org/TR/html401/interact/forms.html) `action` 属性値を生成します。</span><span class="sxs-lookup"><span data-stu-id="571b4-113">Generates the HTML [\<FORM>](https://www.w3.org/TR/html401/interact/forms.html) `action` attribute value for a MVC controller action or named route</span></span>

* <span data-ttu-id="571b4-114">クロスサイト リクエスト フォージェリを防ぐために、非表示の[要求検証トークン](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages)を生成します (HTTP POST アクション メソッドで `[ValidateAntiForgeryToken]` 属性と共に使用する場合)</span><span class="sxs-lookup"><span data-stu-id="571b4-114">Generates a hidden [Request Verification Token](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) to prevent cross-site request forgery (when used with the `[ValidateAntiForgeryToken]` attribute in the HTTP Post action method)</span></span>

* <span data-ttu-id="571b4-115">`asp-route-<Parameter Name>` 属性を提供します (`<Parameter Name>` がルート値に追加される場合)。</span><span class="sxs-lookup"><span data-stu-id="571b4-115">Provides the `asp-route-<Parameter Name>` attribute, where `<Parameter Name>` is added to the route values.</span></span> <span data-ttu-id="571b4-116">`Html.BeginForm` および `Html.BeginRouteForm` に `routeValues` パラメーターを指定すると、同様の機能が提供されます。</span><span class="sxs-lookup"><span data-stu-id="571b4-116">The  `routeValues` parameters to `Html.BeginForm` and `Html.BeginRouteForm` provide similar functionality.</span></span>

* <span data-ttu-id="571b4-117">HTML ヘルパーの代替の `Html.BeginForm` と `Html.BeginRouteForm` があります</span><span class="sxs-lookup"><span data-stu-id="571b4-117">Has an HTML Helper alternative `Html.BeginForm` and `Html.BeginRouteForm`</span></span>

<span data-ttu-id="571b4-118">サンプル:</span><span class="sxs-lookup"><span data-stu-id="571b4-118">Sample:</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Demo/RegisterFormOnly.cshtml)]

<span data-ttu-id="571b4-119">上記のフォーム タグ ヘルパーで、次の HTML が生成されます。</span><span class="sxs-lookup"><span data-stu-id="571b4-119">The Form Tag Helper above generates the following HTML:</span></span>

```HTML
<form method="post" action="/Demo/Register">
    <!-- Input and Submit elements -->
    <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

<span data-ttu-id="571b4-120">MVC ランタイムで、フォーム タグ ヘルパーの属性 `asp-controller` と `asp-action` から `action` 属性値が生成されます。</span><span class="sxs-lookup"><span data-stu-id="571b4-120">The MVC runtime generates the `action` attribute value from the Form Tag Helper attributes `asp-controller` and `asp-action`.</span></span> <span data-ttu-id="571b4-121">また、フォーム タグ ヘルパーも、クロスサイト リクエスト フォージェリを防ぐために、非表示の[要求検証トークン](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages)を生成します (HTTP POST アクション メソッドで `[ValidateAntiForgeryToken]` 属性と共に使用する場合)。</span><span class="sxs-lookup"><span data-stu-id="571b4-121">The Form Tag Helper also generates a hidden [Request Verification Token](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) to prevent cross-site request forgery (when used with the `[ValidateAntiForgeryToken]` attribute in the HTTP Post action method).</span></span> <span data-ttu-id="571b4-122">純粋な HTML フォームをクロスサイト リクエスト フォージェリから保護することは難しいため、フォーム タグ ヘルパーが提供するサービスを利用してください。</span><span class="sxs-lookup"><span data-stu-id="571b4-122">Protecting a pure HTML Form from cross-site request forgery is difficult, the Form Tag Helper provides this service for you.</span></span>

### <a name="using-a-named-route"></a><span data-ttu-id="571b4-123">名前付きのルートの使用</span><span class="sxs-lookup"><span data-stu-id="571b4-123">Using a named route</span></span>

<span data-ttu-id="571b4-124">`asp-route` タグ ヘルパー属性で、HTML `action` 属性のマークアップを生成することもできます。</span><span class="sxs-lookup"><span data-stu-id="571b4-124">The `asp-route` Tag Helper attribute can also generate markup for the HTML `action` attribute.</span></span> <span data-ttu-id="571b4-125">`register` という名前の[ルート](../../fundamentals/routing.md)を持つアプリケーションは、登録ページに次のマークアップを使用できます。</span><span class="sxs-lookup"><span data-stu-id="571b4-125">An app with a [route](../../fundamentals/routing.md)  named `register` could use the following markup for the registration page:</span></span>

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterRoute.cshtml)]

<span data-ttu-id="571b4-126">*Views/Account* フォルダー (*個々のユーザー アカウント*を使用して新しい Web アプリケーションを作成するときに生成されるフォルダー) のビューの多くには、[asp-route-returnurl](xref:mvc/views/working-with-forms) 属性が含まれています。</span><span class="sxs-lookup"><span data-stu-id="571b4-126">Many of the views in the *Views/Account* folder (generated when you create a new web app with *Individual User Accounts*) contain the [asp-route-returnurl](xref:mvc/views/working-with-forms) attribute:</span></span>

```cshtml
<form asp-controller="Account" asp-action="Login"
     asp-route-returnurl="@ViewData["ReturnUrl"]"
     method="post" class="form-horizontal" role="form">
```

>[!NOTE]
><span data-ttu-id="571b4-127">承認済みで認証されていないリソースまたは承認されていないリソースにアクセスしようとすると、組み込みのテンプレートを使用して、`returnUrl` のみが自動的に設定されます。</span><span class="sxs-lookup"><span data-stu-id="571b4-127">With the built in templates, `returnUrl` is only populated automatically when you try to access an authorized resource but are not authenticated or authorized.</span></span> <span data-ttu-id="571b4-128">未承認のアクセスを試行すると、セキュリティ ミドルウェアによって、`returnUrl` が設定されたログイン ページにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="571b4-128">When you attempt an unauthorized access, the security middleware redirects you to the login page with the `returnUrl` set.</span></span>

## <a name="the-input-tag-helper"></a><span data-ttu-id="571b4-129">入力タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="571b4-129">The Input Tag Helper</span></span>

<span data-ttu-id="571b4-130">入力タグ ヘルパーは、HTML の [\<input>](https://www.w3.org/wiki/HTML/Elements/input) 要素を Razor ビューのモデル式にバインドします。</span><span class="sxs-lookup"><span data-stu-id="571b4-130">The Input Tag Helper binds an HTML [\<input>](https://www.w3.org/wiki/HTML/Elements/input) element to a model expression in your razor view.</span></span>

<span data-ttu-id="571b4-131">構文:</span><span class="sxs-lookup"><span data-stu-id="571b4-131">Syntax:</span></span>

```HTML
<input asp-for="<Expression Name>" />
```

<span data-ttu-id="571b4-132">入力タグ ヘルパー:</span><span class="sxs-lookup"><span data-stu-id="571b4-132">The Input Tag Helper:</span></span>

* <span data-ttu-id="571b4-133">`asp-for` 属性で指定された式の名前の `id` および `name` HTML 属性を生成します。</span><span class="sxs-lookup"><span data-stu-id="571b4-133">Generates the `id` and `name` HTML attributes for the expression name specified in the `asp-for` attribute.</span></span> <span data-ttu-id="571b4-134">`asp-for="Property1.Property2"` は `m => m.Property1.Property2` と同じです。</span><span class="sxs-lookup"><span data-stu-id="571b4-134">`asp-for="Property1.Property2"` is equivalent to `m => m.Property1.Property2`.</span></span> <span data-ttu-id="571b4-135">式の名前は、`asp-for` 属性値に使用されるものです。</span><span class="sxs-lookup"><span data-stu-id="571b4-135">The name of the expression is what is used for the `asp-for` attribute value.</span></span> <span data-ttu-id="571b4-136">詳細については、「[式の名前](#expression-names)」セクションを参照してください。</span><span class="sxs-lookup"><span data-stu-id="571b4-136">See the [Expression names](#expression-names) section for additional information.</span></span>

* <span data-ttu-id="571b4-137">モデル プロパティに適用されているモデル型と[データ注釈](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter)に基づいて HTML `type` 属性値を設定します</span><span class="sxs-lookup"><span data-stu-id="571b4-137">Sets the HTML `type` attribute value based on the model type and  [data annotation](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) attributes applied to the model property</span></span>

* <span data-ttu-id="571b4-138">HTML `type` 属性値が指定されている場合は、上書きしません</span><span class="sxs-lookup"><span data-stu-id="571b4-138">Won't overwrite the HTML `type` attribute value when one is specified</span></span>

* <span data-ttu-id="571b4-139">モデル プロパティに適用された[データ注釈](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter)属性から [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) 検証属性を生成します</span><span class="sxs-lookup"><span data-stu-id="571b4-139">Generates [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5)  validation attributes from [data annotation](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) attributes applied to model properties</span></span>

* <span data-ttu-id="571b4-140">`Html.TextBoxFor` および `Html.EditorFor` と重複する HTML ヘルパー機能があります。</span><span class="sxs-lookup"><span data-stu-id="571b4-140">Has an HTML Helper feature overlap with `Html.TextBoxFor` and `Html.EditorFor`.</span></span> <span data-ttu-id="571b4-141">詳細については、「**入力タグ ヘルパーの代替となる HTML ヘルパー**」セクションを参照してください。</span><span class="sxs-lookup"><span data-stu-id="571b4-141">See the **HTML Helper alternatives to Input Tag Helper** section for details.</span></span>

* <span data-ttu-id="571b4-142">厳密な型指定を提供します。</span><span class="sxs-lookup"><span data-stu-id="571b4-142">Provides strong typing.</span></span> <span data-ttu-id="571b4-143">プロパティの名前が変更され、タグ ヘルパーを更新しない場合は、次のようなエラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="571b4-143">If the name of the property changes and you don't update the Tag Helper you'll get an error similar to the following:</span></span>

```HTML
An error occurred during the compilation of a resource required to process
this request. Please review the following specific error details and modify
your source code appropriately.

Type expected
 'RegisterViewModel' does not contain a definition for 'Email' and no
 extension method 'Email' accepting a first argument of type 'RegisterViewModel'
 could be found (are you missing a using directive or an assembly reference?)
```

<span data-ttu-id="571b4-144">`Input` タグ ヘルパーは、.NET 型に基づいて HTML `type` 属性を設定します。</span><span class="sxs-lookup"><span data-stu-id="571b4-144">The `Input` Tag Helper sets the HTML `type` attribute based on the .NET type.</span></span> <span data-ttu-id="571b4-145">次の表は、一般的な.NET 型と生成される HTML 型の一部をまとめたものです (すべての .NET 型を網羅した一覧ではありません)。</span><span class="sxs-lookup"><span data-stu-id="571b4-145">The following table lists some common .NET types and generated HTML type (not every .NET type is listed).</span></span>

|<span data-ttu-id="571b4-146">.NET 型</span><span class="sxs-lookup"><span data-stu-id="571b4-146">.NET type</span></span>|<span data-ttu-id="571b4-147">入力の型</span><span class="sxs-lookup"><span data-stu-id="571b4-147">Input Type</span></span>|
|---|---|
|<span data-ttu-id="571b4-148">Bool</span><span class="sxs-lookup"><span data-stu-id="571b4-148">Bool</span></span>|<span data-ttu-id="571b4-149">type=”checkbox”</span><span class="sxs-lookup"><span data-stu-id="571b4-149">type=”checkbox”</span></span>|
|<span data-ttu-id="571b4-150">String</span><span class="sxs-lookup"><span data-stu-id="571b4-150">String</span></span>|<span data-ttu-id="571b4-151">type=”text”</span><span class="sxs-lookup"><span data-stu-id="571b4-151">type=”text”</span></span>|
|<span data-ttu-id="571b4-152">DateTime</span><span class="sxs-lookup"><span data-stu-id="571b4-152">DateTime</span></span>|<span data-ttu-id="571b4-153">type=”datetime”</span><span class="sxs-lookup"><span data-stu-id="571b4-153">type=”datetime”</span></span>|
|<span data-ttu-id="571b4-154">Byte</span><span class="sxs-lookup"><span data-stu-id="571b4-154">Byte</span></span>|<span data-ttu-id="571b4-155">type=”number”</span><span class="sxs-lookup"><span data-stu-id="571b4-155">type=”number”</span></span>|
|<span data-ttu-id="571b4-156">Int</span><span class="sxs-lookup"><span data-stu-id="571b4-156">Int</span></span>|<span data-ttu-id="571b4-157">type=”number”</span><span class="sxs-lookup"><span data-stu-id="571b4-157">type=”number”</span></span>|
|<span data-ttu-id="571b4-158">Single、Double</span><span class="sxs-lookup"><span data-stu-id="571b4-158">Single, Double</span></span>|<span data-ttu-id="571b4-159">type=”number”</span><span class="sxs-lookup"><span data-stu-id="571b4-159">type=”number”</span></span>|


<span data-ttu-id="571b4-160">次の表は、入力タグ ヘルパーが特定の入力の型にマップする一般的な[データ注釈](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter)属性の一部をまとめたものです (すべての検証属性を網羅した一覧ではありません)。</span><span class="sxs-lookup"><span data-stu-id="571b4-160">The following table shows some common [data annotations](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) attributes that the input tag helper will map to specific input types (not every validation attribute is listed):</span></span>


|<span data-ttu-id="571b4-161">属性</span><span class="sxs-lookup"><span data-stu-id="571b4-161">Attribute</span></span>|<span data-ttu-id="571b4-162">入力の型</span><span class="sxs-lookup"><span data-stu-id="571b4-162">Input Type</span></span>|
|---|---|
|<span data-ttu-id="571b4-163">[EmailAddress]</span><span class="sxs-lookup"><span data-stu-id="571b4-163">[EmailAddress]</span></span>|<span data-ttu-id="571b4-164">type=”email”</span><span class="sxs-lookup"><span data-stu-id="571b4-164">type=”email”</span></span>|
|<span data-ttu-id="571b4-165">[Url]</span><span class="sxs-lookup"><span data-stu-id="571b4-165">[Url]</span></span>|<span data-ttu-id="571b4-166">type=”url”</span><span class="sxs-lookup"><span data-stu-id="571b4-166">type=”url”</span></span>|
|<span data-ttu-id="571b4-167">[HiddenInput]</span><span class="sxs-lookup"><span data-stu-id="571b4-167">[HiddenInput]</span></span>|<span data-ttu-id="571b4-168">type=”hidden”</span><span class="sxs-lookup"><span data-stu-id="571b4-168">type=”hidden”</span></span>|
|<span data-ttu-id="571b4-169">[Phone]</span><span class="sxs-lookup"><span data-stu-id="571b4-169">[Phone]</span></span>|<span data-ttu-id="571b4-170">type=”tel”</span><span class="sxs-lookup"><span data-stu-id="571b4-170">type=”tel”</span></span>|
|<span data-ttu-id="571b4-171">[DataType(DataType.Password)]</span><span class="sxs-lookup"><span data-stu-id="571b4-171">[DataType(DataType.Password)]</span></span>| <span data-ttu-id="571b4-172">type=”password”</span><span class="sxs-lookup"><span data-stu-id="571b4-172">type=”password”</span></span>|
|<span data-ttu-id="571b4-173">[DataType(DataType.Date)]</span><span class="sxs-lookup"><span data-stu-id="571b4-173">[DataType(DataType.Date)]</span></span>| <span data-ttu-id="571b4-174">type=”date”</span><span class="sxs-lookup"><span data-stu-id="571b4-174">type=”date”</span></span>|
|<span data-ttu-id="571b4-175">[DataType(DataType.Time)]</span><span class="sxs-lookup"><span data-stu-id="571b4-175">[DataType(DataType.Time)]</span></span>| <span data-ttu-id="571b4-176">type=”time”</span><span class="sxs-lookup"><span data-stu-id="571b4-176">type=”time”</span></span>|


<span data-ttu-id="571b4-177">サンプル:</span><span class="sxs-lookup"><span data-stu-id="571b4-177">Sample:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[](working-with-forms/sample/final/Views/Demo/RegisterInput.cshtml)]

<span data-ttu-id="571b4-178">上記のコードで、次の HTML が生成されます。</span><span class="sxs-lookup"><span data-stu-id="571b4-178">The code above generates the following HTML:</span></span>

```HTML
  <form method="post" action="/Demo/RegisterInput">
       Email:
       <input type="email" data-val="true"
              data-val-email="The Email Address field is not a valid email address."
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

<span data-ttu-id="571b4-179">`Email` および `Password` プロパティに適用されたデータ注釈によって、モデルに関するメタデータが生成されます。</span><span class="sxs-lookup"><span data-stu-id="571b4-179">The data annotations applied to the `Email` and `Password` properties generate metadata on the model.</span></span> <span data-ttu-id="571b4-180">入力タグ ヘルパーはモデルのメタデータを使用し、[HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-val-*` 属性を生成します ([モデルの検証](../models/validation.md)に関するページを参照してください)。</span><span class="sxs-lookup"><span data-stu-id="571b4-180">The Input Tag Helper consumes the model metadata and produces [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-val-*` attributes (see [Model Validation](../models/validation.md)).</span></span> <span data-ttu-id="571b4-181">これらの属性に、入力フィールドにアタッチする検証コントロールを記述します。</span><span class="sxs-lookup"><span data-stu-id="571b4-181">These attributes describe the validators to attach to the input fields.</span></span> <span data-ttu-id="571b4-182">これで、控えめな HTML5 と [jQuery](https://jquery.com/) の検証機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="571b4-182">This provides unobtrusive HTML5 and [jQuery](https://jquery.com/) validation.</span></span> <span data-ttu-id="571b4-183">控えめな属性の形式は `data-val-rule="Error Message"` です。この rule は検証ルールの名前です (`data-val-required`、`data-val-email`、`data-val-maxlength` など)。属性にエラー メッセージが指定されている場合は、`data-val-rule` 属性の値として表示されます。</span><span class="sxs-lookup"><span data-stu-id="571b4-183">The unobtrusive attributes have the format `data-val-rule="Error Message"`, where rule is the name of the validation rule (such as `data-val-required`, `data-val-email`, `data-val-maxlength`, etc.) If an error message is provided in the attribute, it's displayed as the value for the `data-val-rule` attribute.</span></span> <span data-ttu-id="571b4-184">`data-val-maxlength-max="1024"` など、ルールに関する追加の詳細情報を提供するフォームの属性 `data-val-ruleName-argumentName="argumentValue"` もあります。</span><span class="sxs-lookup"><span data-stu-id="571b4-184">There are also attributes of the form `data-val-ruleName-argumentName="argumentValue"` that provide additional details about the rule, for example, `data-val-maxlength-max="1024"` .</span></span>

### <a name="html-helper-alternatives-to-input-tag-helper"></a><span data-ttu-id="571b4-185">入力タグ ヘルパーの代替となる HTML ヘルパー</span><span class="sxs-lookup"><span data-stu-id="571b4-185">HTML Helper alternatives to Input Tag Helper</span></span>

<span data-ttu-id="571b4-186">`Html.TextBox`、`Html.TextBoxFor`、`Html.Editor`、`Html.EditorFor` には、入力タグ ヘルパーと重複する機能があります。</span><span class="sxs-lookup"><span data-stu-id="571b4-186">`Html.TextBox`, `Html.TextBoxFor`, `Html.Editor` and `Html.EditorFor` have overlapping features with the Input Tag Helper.</span></span> <span data-ttu-id="571b4-187">入力タグ ヘルパーでは `type` 属性が自動的に設定されますが、`Html.TextBox` と `Html.TextBoxFor` では自動設定されません。</span><span class="sxs-lookup"><span data-stu-id="571b4-187">The Input Tag Helper will automatically set the `type` attribute; `Html.TextBox` and `Html.TextBoxFor` won't.</span></span> <span data-ttu-id="571b4-188">`Html.Editor` と `Html.EditorFor` はコレクション、複雑なオブジェクト、テンプレートを処理しますが、入力タグ ヘルパーは処理しません。</span><span class="sxs-lookup"><span data-stu-id="571b4-188">`Html.Editor` and `Html.EditorFor` handle collections, complex objects and templates; the Input Tag Helper doesn't.</span></span> <span data-ttu-id="571b4-189">入力タグ ヘルパーの `Html.EditorFor` と `Html.TextBoxFor` は厳密に型指定されていますが (ラムダ式を使用します)、`Html.TextBox` と `Html.Editor` は厳密に型指定されていません (式の名前を使用します)。</span><span class="sxs-lookup"><span data-stu-id="571b4-189">The Input Tag Helper, `Html.EditorFor`  and  `Html.TextBoxFor` are strongly typed (they use lambda expressions); `Html.TextBox` and `Html.Editor` are not (they use expression names).</span></span>

### <a name="htmlattributes"></a><span data-ttu-id="571b4-190">HtmlAttributes</span><span class="sxs-lookup"><span data-stu-id="571b4-190">HtmlAttributes</span></span>

<span data-ttu-id="571b4-191">`@Html.Editor()` と `@Html.EditorFor()` は、既定のテンプレートを実行するときに `htmlAttributes` という名前の特殊な `ViewDataDictionary` エントリを使用します。</span><span class="sxs-lookup"><span data-stu-id="571b4-191">`@Html.Editor()` and `@Html.EditorFor()` use a special `ViewDataDictionary` entry named `htmlAttributes` when executing their default templates.</span></span> <span data-ttu-id="571b4-192">この動作は、必要に応じて `additionalViewData` パラメーターを使用して拡張されます。</span><span class="sxs-lookup"><span data-stu-id="571b4-192">This behavior is optionally augmented using `additionalViewData` parameters.</span></span> <span data-ttu-id="571b4-193">キー "htmlAttributes" は大文字と小文字が区別されません。</span><span class="sxs-lookup"><span data-stu-id="571b4-193">The key "htmlAttributes" is case-insensitive.</span></span> <span data-ttu-id="571b4-194">キー "htmlAttributes" は、`htmlAttributes` のような入力ヘルパーに渡される `@Html.TextBox()` オブジェクトと同様に処理されます。</span><span class="sxs-lookup"><span data-stu-id="571b4-194">The key "htmlAttributes" is handled similarly to the `htmlAttributes` object passed to input helpers like `@Html.TextBox()`.</span></span>

```HTML
@Html.EditorFor(model => model.YourProperty, 
  new { htmlAttributes = new { @class="myCssClass", style="Width:100px" } })
```

### <a name="expression-names"></a><span data-ttu-id="571b4-195">式の名前</span><span class="sxs-lookup"><span data-stu-id="571b4-195">Expression names</span></span>

<span data-ttu-id="571b4-196">`asp-for` 属性値は `ModelExpression` であり、ラムダ式の右辺です。</span><span class="sxs-lookup"><span data-stu-id="571b4-196">The `asp-for` attribute value is a `ModelExpression` and the right hand side of a lambda expression.</span></span> <span data-ttu-id="571b4-197">そのため、生成されるコードで `asp-for="Property1"` は `m => m.Property1` になります。したがって、`Model` をプレフィックスとして付加する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="571b4-197">Therefore, `asp-for="Property1"` becomes `m => m.Property1` in the generated code which is why you don't need to prefix with `Model`.</span></span> <span data-ttu-id="571b4-198">"@" 文字を使用してインライン式を開始し、`m.` の前に移動できます。</span><span class="sxs-lookup"><span data-stu-id="571b4-198">You can use the "@" character to start an inline expression and move before the `m.`:</span></span>

```HTML
@{
       var joe = "Joe";
   }
   <input asp-for="@joe" />
```

<span data-ttu-id="571b4-199">以下が生成されます。</span><span class="sxs-lookup"><span data-stu-id="571b4-199">Generates the following:</span></span>

```HTML
<input type="text" id="joe" name="joe" value="Joe" />
```

<span data-ttu-id="571b4-200">`i` の値が `23` の場合、`asp-for="CollectionProperty[23].Member"` はコレクションのプロパティを使用して、`asp-for="CollectionProperty[i].Member"` と同じ名前を生成します。</span><span class="sxs-lookup"><span data-stu-id="571b4-200">With collection properties, `asp-for="CollectionProperty[23].Member"` generates the same name as `asp-for="CollectionProperty[i].Member"` when `i` has the value `23`.</span></span>

<span data-ttu-id="571b4-201">ASP.NET Core MVC で `ModelExpression` の値が計算されるとき、`ModelState` を含む、いくつかのソースが検査されます。</span><span class="sxs-lookup"><span data-stu-id="571b4-201">When ASP.NET Core MVC calculates the value of `ModelExpression`, it inspects several sources, including `ModelState`.</span></span> <span data-ttu-id="571b4-202">`<input type="text" asp-for="@Name" />` を検討します。</span><span class="sxs-lookup"><span data-stu-id="571b4-202">Consider `<input type="text" asp-for="@Name" />`.</span></span> <span data-ttu-id="571b4-203">計算された `value` 属性は、次のうちの最初の非 null 値です。</span><span class="sxs-lookup"><span data-stu-id="571b4-203">The calculated `value` attribute is the first non-null value from:</span></span>

* <span data-ttu-id="571b4-204">キー "Name" を持つ `ModelState` エントリ。</span><span class="sxs-lookup"><span data-stu-id="571b4-204">`ModelState` entry with key "Name".</span></span>
* <span data-ttu-id="571b4-205">式 `Model.Name` の結果。</span><span class="sxs-lookup"><span data-stu-id="571b4-205">Result of the expression `Model.Name`.</span></span>

### <a name="navigating-child-properties"></a><span data-ttu-id="571b4-206">子プロパティの移動</span><span class="sxs-lookup"><span data-stu-id="571b4-206">Navigating child properties</span></span>

<span data-ttu-id="571b4-207">ビュー モデルのプロパティ パスを使用して、子プロパティに移動することもできます。</span><span class="sxs-lookup"><span data-stu-id="571b4-207">You can also navigate to child properties using the property path of the view model.</span></span> <span data-ttu-id="571b4-208">子 `Address` プロパティを含むより複雑なモデル クラスを考えてみましょう。</span><span class="sxs-lookup"><span data-stu-id="571b4-208">Consider a more complex model class that contains a child `Address` property.</span></span>

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/AddressViewModel.cs?highlight=1,2,3,4&range=5-8)]

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/RegisterAddressViewModel.cs?highlight=8&range=5-13)]

<span data-ttu-id="571b4-209">このビューでは、`Address.AddressLine1` にバインドしています。</span><span class="sxs-lookup"><span data-stu-id="571b4-209">In the view, we bind to `Address.AddressLine1`:</span></span>

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterAddress.cshtml?highlight=6)]

<span data-ttu-id="571b4-210">`Address.AddressLine1` に対して次の HTML が生成されます。</span><span class="sxs-lookup"><span data-stu-id="571b4-210">The following HTML is generated for `Address.AddressLine1`:</span></span>

```HTML
<input type="text" id="Address_AddressLine1" name="Address.AddressLine1" value="" />
```

### <a name="expression-names-and-collections"></a><span data-ttu-id="571b4-211">式の名前とコレクション</span><span class="sxs-lookup"><span data-stu-id="571b4-211">Expression names and Collections</span></span>

<span data-ttu-id="571b4-212">`Colors` の配列を含むサンプル モデル:</span><span class="sxs-lookup"><span data-stu-id="571b4-212">Sample, a model containing an array of `Colors`:</span></span>

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/Person.cs?highlight=3&range=5-10)]

<span data-ttu-id="571b4-213">アクション メソッド:</span><span class="sxs-lookup"><span data-stu-id="571b4-213">The action method:</span></span>

```csharp
public IActionResult Edit(int id, int colorIndex)
   {
       ViewData["Index"] = colorIndex;
       return View(GetPerson(id));
   }
```

<span data-ttu-id="571b4-214">次の Razor は、特定の `Color` 要素にアクセスする方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="571b4-214">The following Razor shows how you access a specific `Color` element:</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Demo/EditColor.cshtml)]

<span data-ttu-id="571b4-215">*Views/Shared/EditorTemplates/String.cshtml* テンプレート:</span><span class="sxs-lookup"><span data-stu-id="571b4-215">The *Views/Shared/EditorTemplates/String.cshtml* template:</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Shared/EditorTemplates/String.cshtml)]

<span data-ttu-id="571b4-216">`List<T>` を使用するサンプル:</span><span class="sxs-lookup"><span data-stu-id="571b4-216">Sample using `List<T>`:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/ToDoItem.cs?range=3-8)]

<span data-ttu-id="571b4-217">次の Razor は、コレクションに対して反復処理を実行する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="571b4-217">The following Razor shows how to iterate over a collection:</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Demo/Edit.cshtml)]

<span data-ttu-id="571b4-218">*Views/Shared/EditorTemplates/ToDoItem.cshtml* テンプレート:</span><span class="sxs-lookup"><span data-stu-id="571b4-218">The *Views/Shared/EditorTemplates/ToDoItem.cshtml* template:</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Shared/EditorTemplates/ToDoItem.cshtml)]


>[!NOTE]
><span data-ttu-id="571b4-219">(`foreach` *ではなく*) 常に `for` を使用して一覧に対して反復処理を実行します。</span><span class="sxs-lookup"><span data-stu-id="571b4-219">Always use `for` (and *not* `foreach`) to iterate over a list.</span></span> <span data-ttu-id="571b4-220">LINQ 式でインデクサーを評価する処理はコストが高くなる可能性があるので、最小限に抑えることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="571b4-220">Evaluating an indexer in a LINQ expression can be expensive and should be minimized.</span></span>

&nbsp;

>[!NOTE]
><span data-ttu-id="571b4-221">上記のコメント付きサンプル コードは、ラムダ式を `@` 演算子に置き換えて、リスト内の各 `ToDoItem` にアクセスする方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="571b4-221">The commented sample code above shows how you would replace the lambda expression with the `@` operator to access each `ToDoItem` in the list.</span></span>

## <a name="the-textarea-tag-helper"></a><span data-ttu-id="571b4-222">Textarea タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="571b4-222">The Textarea Tag Helper</span></span>

<span data-ttu-id="571b4-223">`Textarea Tag Helper` タグ ヘルパーは、入力タグ ヘルパーと似ています。</span><span class="sxs-lookup"><span data-stu-id="571b4-223">The `Textarea Tag Helper` tag helper is  similar to the Input Tag Helper.</span></span>

* <span data-ttu-id="571b4-224">[\<textarea>](https://www.w3.org/wiki/HTML/Elements/textarea) 要素のモデルから `id` および `name` 属性と、データ検証属性を生成します。</span><span class="sxs-lookup"><span data-stu-id="571b4-224">Generates the `id` and `name` attributes, and the data validation attributes from the model for a [\<textarea>](https://www.w3.org/wiki/HTML/Elements/textarea) element.</span></span>

* <span data-ttu-id="571b4-225">厳密な型指定を提供します。</span><span class="sxs-lookup"><span data-stu-id="571b4-225">Provides strong typing.</span></span>

* <span data-ttu-id="571b4-226">HTML ヘルパーの代替: `Html.TextAreaFor`</span><span class="sxs-lookup"><span data-stu-id="571b4-226">HTML Helper alternative: `Html.TextAreaFor`</span></span>

<span data-ttu-id="571b4-227">サンプル:</span><span class="sxs-lookup"><span data-stu-id="571b4-227">Sample:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/DescriptionViewModel.cs)]

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterTextArea.cshtml?highlight=4)]

<span data-ttu-id="571b4-228">次の HTML が生成されます。</span><span class="sxs-lookup"><span data-stu-id="571b4-228">The following HTML is generated:</span></span>

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

## <a name="the-label-tag-helper"></a><span data-ttu-id="571b4-229">ラベル タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="571b4-229">The Label Tag Helper</span></span>

* <span data-ttu-id="571b4-230">式の名前の [<label>](https://www.w3.org/wiki/HTML/Elements/label) 要素に対してラベルのキャプションと `for` 属性を生成します。</span><span class="sxs-lookup"><span data-stu-id="571b4-230">Generates the label caption and `for` attribute on a [<label>](https://www.w3.org/wiki/HTML/Elements/label) element for an expression name</span></span>

* <span data-ttu-id="571b4-231">HTML ヘルパーの代替: `Html.LabelFor`。</span><span class="sxs-lookup"><span data-stu-id="571b4-231">HTML Helper alternative: `Html.LabelFor`.</span></span>

<span data-ttu-id="571b4-232">`Label Tag Helper` は、純粋な HTML の label 要素よりも次の点で優れています。</span><span class="sxs-lookup"><span data-stu-id="571b4-232">The `Label Tag Helper`  provides the following benefits over a pure HTML label element:</span></span>

* <span data-ttu-id="571b4-233">`Display` 属性からわかりやすいラベル値が自動的に取得されます。</span><span class="sxs-lookup"><span data-stu-id="571b4-233">You automatically get the descriptive label value from the `Display` attribute.</span></span> <span data-ttu-id="571b4-234">意図した表示名は時間と共に変化する可能性があります。また、`Display` 属性とラベル タグ ヘルパーの組み合わせによって、使用されているあらゆる場所に `Display` が適用されます。</span><span class="sxs-lookup"><span data-stu-id="571b4-234">The intended display name might change over time, and the combination of `Display` attribute and Label Tag Helper will apply the `Display` everywhere it's used.</span></span>

* <span data-ttu-id="571b4-235">ソース コードのマークアップが少ない</span><span class="sxs-lookup"><span data-stu-id="571b4-235">Less markup in source code</span></span>

* <span data-ttu-id="571b4-236">モデルのプロパティを使用した厳密な型指定。</span><span class="sxs-lookup"><span data-stu-id="571b4-236">Strong typing with the model property.</span></span>

<span data-ttu-id="571b4-237">サンプル:</span><span class="sxs-lookup"><span data-stu-id="571b4-237">Sample:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/SimpleViewModel.cs)]

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterLabel.cshtml?highlight=4)]

<span data-ttu-id="571b4-238">`<label>` 要素に対して次の HTML が生成されます。</span><span class="sxs-lookup"><span data-stu-id="571b4-238">The following HTML is generated for the `<label>` element:</span></span>

```HTML
<label for="Email">Email Address</label>
```

<span data-ttu-id="571b4-239">ラベル タグ ヘルパーから "Email" の属性値 `for` が生成されました。これは、`<input>` に関連付けられた ID です。</span><span class="sxs-lookup"><span data-stu-id="571b4-239">The Label Tag Helper generated the `for` attribute value of "Email", which is the ID associated with the `<input>` element.</span></span> <span data-ttu-id="571b4-240">タグ ヘルパーでは一貫性のある `id` および `for` 要素が生成されるので、正しく関連付けられます。</span><span class="sxs-lookup"><span data-stu-id="571b4-240">The Tag Helpers generate consistent `id` and `for` elements so they can be correctly associated.</span></span> <span data-ttu-id="571b4-241">このサンプルのキャプションは `Display` 属性に由来します。</span><span class="sxs-lookup"><span data-stu-id="571b4-241">The caption in this sample comes from the `Display` attribute.</span></span> <span data-ttu-id="571b4-242">モデルに `Display` 属性を含めなかった場合、キャプションは式のプロパティ名になります。</span><span class="sxs-lookup"><span data-stu-id="571b4-242">If the model didn't contain a `Display` attribute, the caption would be the property name of the expression.</span></span>

## <a name="the-validation-tag-helpers"></a><span data-ttu-id="571b4-243">検証タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="571b4-243">The Validation Tag Helpers</span></span>

<span data-ttu-id="571b4-244">2 つの検証タグ ヘルパーがあります。</span><span class="sxs-lookup"><span data-stu-id="571b4-244">There are two Validation Tag Helpers.</span></span> <span data-ttu-id="571b4-245">`Validation Message Tag Helper` (モデルの単一のプロパティに関する検証メッセージを表示する) と `Validation Summary Tag Helper` (検証エラーの概要を表示する) です。</span><span class="sxs-lookup"><span data-stu-id="571b4-245">The `Validation Message Tag Helper` (which displays a validation message for a single property on your model), and the `Validation Summary Tag Helper` (which displays a summary of validation errors).</span></span> <span data-ttu-id="571b4-246">`Input Tag Helper` は、モデル クラスのデータ注釈属性に基づいて、HTML5 のクライアント側検証属性を input 要素に追加します。</span><span class="sxs-lookup"><span data-stu-id="571b4-246">The `Input Tag Helper` adds HTML5 client side validation attributes to input elements based on data annotation attributes on your model classes.</span></span> <span data-ttu-id="571b4-247">検証はサーバー側でも実行されます。</span><span class="sxs-lookup"><span data-stu-id="571b4-247">Validation is also performed on the server.</span></span> <span data-ttu-id="571b4-248">検証エラーが発生すると、検証タグ ヘルパーによってこれらのエラー メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="571b4-248">The Validation Tag Helper displays these error messages when a validation error occurs.</span></span>

### <a name="the-validation-message-tag-helper"></a><span data-ttu-id="571b4-249">検証メッセージ タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="571b4-249">The Validation Message Tag Helper</span></span>

* <span data-ttu-id="571b4-250">[HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-valmsg-for="property"` 属性を [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) 要素に追加します。これによって、指定されたモデル プロパティの入力フィールドに検証エラー メッセージがアタッチされます。</span><span class="sxs-lookup"><span data-stu-id="571b4-250">Adds the [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5)  `data-valmsg-for="property"` attribute to the [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) element, which attaches the validation error messages on the input field of the specified model property.</span></span> <span data-ttu-id="571b4-251">クライアント側の検証エラーが発生すると、[jQuery](https://jquery.com/) によって `<span>` 要素のエラー メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="571b4-251">When a client side validation error occurs, [jQuery](https://jquery.com/) displays the error message in the `<span>` element.</span></span>

* <span data-ttu-id="571b4-252">検証はサーバー側でも実行されます。</span><span class="sxs-lookup"><span data-stu-id="571b4-252">Validation also takes place on the server.</span></span> <span data-ttu-id="571b4-253">クライアントで JavaScript が無効にされている場合や、一部の検証をサーバー側でのみ実行できる場合があります。</span><span class="sxs-lookup"><span data-stu-id="571b4-253">Clients may have JavaScript disabled and some validation can only be done on the server side.</span></span>

* <span data-ttu-id="571b4-254">HTML ヘルパーの代替: `Html.ValidationMessageFor`</span><span class="sxs-lookup"><span data-stu-id="571b4-254">HTML Helper alternative: `Html.ValidationMessageFor`</span></span>

<span data-ttu-id="571b4-255">`Validation Message Tag Helper` は、HTML の [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) 要素で `asp-validation-for` 属性と共に使用されます。</span><span class="sxs-lookup"><span data-stu-id="571b4-255">The `Validation Message Tag Helper`  is used with the `asp-validation-for` attribute on a HTML [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) element.</span></span>

```HTML
<span asp-validation-for="Email"></span>
```

<span data-ttu-id="571b4-256">検証メッセージ タグ ヘルパーから、次の HTML が生成されます。</span><span class="sxs-lookup"><span data-stu-id="571b4-256">The Validation Message Tag Helper will generate the following HTML:</span></span>

```HTML
<span class="field-validation-valid"
  data-valmsg-for="Email"
  data-valmsg-replace="true"></span>
```

<span data-ttu-id="571b4-257">一般的に、同じプロパティの場合は、`Input` タグ ヘルパーの後に `Validation Message Tag Helper` を使用します。</span><span class="sxs-lookup"><span data-stu-id="571b4-257">You generally use the `Validation Message Tag Helper`  after an `Input` Tag Helper for the same property.</span></span> <span data-ttu-id="571b4-258">こうすることで、エラーの原因となった入力の近くで検証エラー メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="571b4-258">Doing so displays any validation error messages near the input that caused the error.</span></span>

> [!NOTE]
> <span data-ttu-id="571b4-259">クライアント側の検証のために、正しい JavaScript および [jQuery](https://jquery.com/) のスクリプト参照を使用したビューを用意する必要があります。</span><span class="sxs-lookup"><span data-stu-id="571b4-259">You must have a view with the correct JavaScript and [jQuery](https://jquery.com/) script references in place for client side validation.</span></span> <span data-ttu-id="571b4-260">詳細については、[モデルの検証](../models/validation.md)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="571b4-260">See [Model Validation](../models/validation.md) for more information.</span></span>

<span data-ttu-id="571b4-261">サーバー側の検証エラーが発生した場合 (カスタムのサーバー側の検証がある場合や、クライアント側の検証が無効な場合など)、MVC はそのエラー メッセージを `<span>` 要素の本文として配置します。</span><span class="sxs-lookup"><span data-stu-id="571b4-261">When a server side validation error occurs (for example when you have custom server side validation or client-side validation is disabled), MVC places that error message as the body of the `<span>` element.</span></span>

```HTML
<span class="field-validation-error" data-valmsg-for="Email"
            data-valmsg-replace="true">
   The Email Address field is required.
</span>
```

### <a name="the-validation-summary-tag-helper"></a><span data-ttu-id="571b4-262">検証概要タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="571b4-262">The Validation Summary Tag Helper</span></span>

* <span data-ttu-id="571b4-263">`asp-validation-summary` 属性を持つ `<div>` 要素をターゲットとします</span><span class="sxs-lookup"><span data-stu-id="571b4-263">Targets `<div>` elements with the `asp-validation-summary` attribute</span></span>

* <span data-ttu-id="571b4-264">HTML ヘルパーの代替: `@Html.ValidationSummary`</span><span class="sxs-lookup"><span data-stu-id="571b4-264">HTML Helper alternative: `@Html.ValidationSummary`</span></span>

<span data-ttu-id="571b4-265">`Validation Summary Tag Helper` は、検証メッセージの概要を表示するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="571b4-265">The `Validation Summary Tag Helper`  is used to display a summary of validation messages.</span></span> <span data-ttu-id="571b4-266">`asp-validation-summary` 属性値には、次のいずれかを指定できます。</span><span class="sxs-lookup"><span data-stu-id="571b4-266">The `asp-validation-summary` attribute value can be any of the following:</span></span>

|<span data-ttu-id="571b4-267">asp-validation-summary</span><span class="sxs-lookup"><span data-stu-id="571b4-267">asp-validation-summary</span></span>|<span data-ttu-id="571b4-268">検証メッセージが表示されます</span><span class="sxs-lookup"><span data-stu-id="571b4-268">Validation messages displayed</span></span>|
|--- |--- |
|<span data-ttu-id="571b4-269">ValidationSummary.All</span><span class="sxs-lookup"><span data-stu-id="571b4-269">ValidationSummary.All</span></span>|<span data-ttu-id="571b4-270">プロパティとモデル レベル</span><span class="sxs-lookup"><span data-stu-id="571b4-270">Property and model level</span></span>|
|<span data-ttu-id="571b4-271">ValidationSummary.ModelOnly</span><span class="sxs-lookup"><span data-stu-id="571b4-271">ValidationSummary.ModelOnly</span></span>|<span data-ttu-id="571b4-272">モデル</span><span class="sxs-lookup"><span data-stu-id="571b4-272">Model</span></span>|
|<span data-ttu-id="571b4-273">ValidationSummary.None</span><span class="sxs-lookup"><span data-stu-id="571b4-273">ValidationSummary.None</span></span>|<span data-ttu-id="571b4-274">なし</span><span class="sxs-lookup"><span data-stu-id="571b4-274">None</span></span>|

### <a name="sample"></a><span data-ttu-id="571b4-275">サンプル</span><span class="sxs-lookup"><span data-stu-id="571b4-275">Sample</span></span>

<span data-ttu-id="571b4-276">次の例では、データ モデルは `DataAnnotation` 属性で修飾され、`<input>` 要素に関する検証エラー メッセージが生成されます。</span><span class="sxs-lookup"><span data-stu-id="571b4-276">In the following example, the data model is decorated with `DataAnnotation` attributes, which generates validation error messages on the `<input>` element.</span></span>  <span data-ttu-id="571b4-277">検証エラーが発生すると、検証タグ ヘルパーはエラー メッセージを表示します。</span><span class="sxs-lookup"><span data-stu-id="571b4-277">When a validation error occurs, the Validation Tag Helper displays the error message:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterValidation.cshtml?highlight=4,6,8&range=1-10)]

<span data-ttu-id="571b4-278">生成される HTML (モデルが有効な場合):</span><span class="sxs-lookup"><span data-stu-id="571b4-278">The generated HTML (when the model is valid):</span></span>

```HTML
<form action="/DemoReg/Register" method="post">
  <div class="validation-summary-valid" data-valmsg-summary="true">
  <ul><li style="display:none"></li></ul></div>
  Email:  <input name="Email" id="Email" type="email" value=""
   data-val-required="The Email field is required."
   data-val-email="The Email field is not a valid email address."
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

## <a name="the-select-tag-helper"></a><span data-ttu-id="571b4-279">選択タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="571b4-279">The Select Tag Helper</span></span>

* <span data-ttu-id="571b4-280">モデルのプロパティについて、[select](https://www.w3.org/wiki/HTML/Elements/select) 要素と、関連する [option](https://www.w3.org/wiki/HTML/Elements/option) 要素を生成します。</span><span class="sxs-lookup"><span data-stu-id="571b4-280">Generates [select](https://www.w3.org/wiki/HTML/Elements/select) and associated [option](https://www.w3.org/wiki/HTML/Elements/option) elements for properties of your model.</span></span>

* <span data-ttu-id="571b4-281">HTML ヘルパーの代替の `Html.DropDownListFor` と `Html.ListBoxFor` があります</span><span class="sxs-lookup"><span data-stu-id="571b4-281">Has an HTML Helper alternative `Html.DropDownListFor` and `Html.ListBoxFor`</span></span>

<span data-ttu-id="571b4-282">`Select Tag Helper` `asp-for` は [select](https://www.w3.org/wiki/HTML/Elements/select) 要素のモデル プロパティ名を指定し、`asp-items` は [option](https://www.w3.org/wiki/HTML/Elements/option) 要素を指定します。</span><span class="sxs-lookup"><span data-stu-id="571b4-282">The `Select Tag Helper` `asp-for` specifies the model property  name for the [select](https://www.w3.org/wiki/HTML/Elements/select) element  and `asp-items` specifies the [option](https://www.w3.org/wiki/HTML/Elements/option) elements.</span></span>  <span data-ttu-id="571b4-283">例:</span><span class="sxs-lookup"><span data-stu-id="571b4-283">For example:</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

<span data-ttu-id="571b4-284">サンプル:</span><span class="sxs-lookup"><span data-stu-id="571b4-284">Sample:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/CountryViewModel.cs)]

<span data-ttu-id="571b4-285">`Index` メソッドは `CountryViewModel` を初期化し、選択された国を設定し、それを `Index` ビューに渡します。</span><span class="sxs-lookup"><span data-stu-id="571b4-285">The `Index` method initializes the `CountryViewModel`, sets the selected country and passes it to the `Index` view.</span></span>

[!code-csharp[](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

<span data-ttu-id="571b4-286">HTTP POST `Index` メソッドによって選択内容が表示されます。</span><span class="sxs-lookup"><span data-stu-id="571b4-286">The HTTP POST `Index` method displays the selection:</span></span>

[!code-csharp[](working-with-forms/sample/final/Controllers/HomeController.cs?range=15-27)]

<span data-ttu-id="571b4-287">`Index` ビュー:</span><span class="sxs-lookup"><span data-stu-id="571b4-287">The `Index` view:</span></span>

[!code-cshtml[](working-with-forms/sample/final/Views/Home/Index.cshtml?highlight=4)]

<span data-ttu-id="571b4-288">次の HTML が生成されます ("CA" が選択されている場合)。</span><span class="sxs-lookup"><span data-stu-id="571b4-288">Which generates the following HTML (with "CA" selected):</span></span>

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
> <span data-ttu-id="571b4-289">選択タグ ヘルパーで `ViewBag` または `ViewData` を使用することはお勧めしません。</span><span class="sxs-lookup"><span data-stu-id="571b4-289">We don't recommend using `ViewBag` or `ViewData` with the Select Tag Helper.</span></span> <span data-ttu-id="571b4-290">ビュー モデルは、MVC メタデータを提供する場合に堅牢性が高くなり、一般的にあまり問題にはなりません。</span><span class="sxs-lookup"><span data-stu-id="571b4-290">A view model is more robust at providing MVC metadata and generally less problematic.</span></span>

<span data-ttu-id="571b4-291">`asp-for` 属性値は特殊なケースであり、(`asp-items` などの) 他のタグ ヘルパー属性とは異なり、`Model` プレフィックスは必須ではありません</span><span class="sxs-lookup"><span data-stu-id="571b4-291">The `asp-for` attribute value is a special case and doesn't require a `Model` prefix, the other Tag Helper attributes do (such as `asp-items`)</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

### <a name="enum-binding"></a><span data-ttu-id="571b4-292">Enum バインディング</span><span class="sxs-lookup"><span data-stu-id="571b4-292">Enum binding</span></span>

<span data-ttu-id="571b4-293">`<select>` を `enum` プロパティと組み合わせて使用し、`enum` 値から `SelectListItem` 要素を生成すると便利な場合がよくあります。</span><span class="sxs-lookup"><span data-stu-id="571b4-293">It's often convenient to use `<select>` with an `enum` property and generate the `SelectListItem` elements from the `enum` values.</span></span>

<span data-ttu-id="571b4-294">サンプル:</span><span class="sxs-lookup"><span data-stu-id="571b4-294">Sample:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/CountryEnumViewModel.cs?range=3-7)]

[!code-csharp[](working-with-forms/sample/final/ViewModels/CountryEnum.cs)]

<span data-ttu-id="571b4-295">`GetEnumSelectList` メソッドは列挙型の場合に `SelectList` オブジェクトを生成します。</span><span class="sxs-lookup"><span data-stu-id="571b4-295">The `GetEnumSelectList` method generates a `SelectList` object for an enum.</span></span>

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEnum.cshtml?highlight=5)]

<span data-ttu-id="571b4-296">より高機能な UI にするために、列挙子リストを `Display` 属性で修飾することができます。</span><span class="sxs-lookup"><span data-stu-id="571b4-296">You can decorate your enumerator list with the `Display` attribute to get a richer UI:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/CountryEnum.cs?highlight=5,7)]

<span data-ttu-id="571b4-297">次の HTML が生成されます。</span><span class="sxs-lookup"><span data-stu-id="571b4-297">The following HTML is generated:</span></span>

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

### <a name="option-group"></a><span data-ttu-id="571b4-298">オプション グループ</span><span class="sxs-lookup"><span data-stu-id="571b4-298">Option Group</span></span>

<span data-ttu-id="571b4-299">HTML の [\<optgroup>](https://www.w3.org/wiki/HTML/Elements/optgroup) 要素は、ビュー モデルに 1 つ以上の `SelectListGroup` オブジェクトが含まれている場合に生成されます。</span><span class="sxs-lookup"><span data-stu-id="571b4-299">The HTML  [\<optgroup>](https://www.w3.org/wiki/HTML/Elements/optgroup) element is generated when the view model contains one or more `SelectListGroup` objects.</span></span>

<span data-ttu-id="571b4-300">`CountryViewModelGroup` は `SelectListItem` 要素を "北米" グループと "ヨーロッパ" グループに分けます。</span><span class="sxs-lookup"><span data-stu-id="571b4-300">The `CountryViewModelGroup` groups the `SelectListItem` elements into the "North America" and "Europe" groups:</span></span>

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelGroup.cs?highlight=5,6,14,20,26,32,38,44&range=6-56)]

<span data-ttu-id="571b4-301">この 2 つのグループを次に示します。</span><span class="sxs-lookup"><span data-stu-id="571b4-301">The two groups are shown below:</span></span>

![オプション グループの例](working-with-forms/_static/grp.png)

<span data-ttu-id="571b4-303">生成される HTML:</span><span class="sxs-lookup"><span data-stu-id="571b4-303">The generated HTML:</span></span>

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

### <a name="multiple-select"></a><span data-ttu-id="571b4-304">複数選択</span><span class="sxs-lookup"><span data-stu-id="571b4-304">Multiple select</span></span>

<span data-ttu-id="571b4-305">`asp-for` 属性に指定されているプロパティが `IEnumerable` の場合、選択タグ ヘルパーは [multiple = "multiple"](http://w3c.github.io/html-reference/select.html) 属性を自動的に生成します。</span><span class="sxs-lookup"><span data-stu-id="571b4-305">The Select Tag Helper  will automatically generate the [multiple = "multiple"](http://w3c.github.io/html-reference/select.html)  attribute if the property specified in the `asp-for` attribute is an `IEnumerable`.</span></span> <span data-ttu-id="571b4-306">たとえば、次のようなモデルがあるとします。</span><span class="sxs-lookup"><span data-stu-id="571b4-306">For example, given the following model:</span></span>

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelIEnumerable.cs?highlight=6)]

<span data-ttu-id="571b4-307">次のビューを対象にします。</span><span class="sxs-lookup"><span data-stu-id="571b4-307">With the following view:</span></span>

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexMultiSelect.cshtml?highlight=4)]

<span data-ttu-id="571b4-308">次の HTML が生成されます。</span><span class="sxs-lookup"><span data-stu-id="571b4-308">Generates the following HTML:</span></span>

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

### <a name="no-selection"></a><span data-ttu-id="571b4-309">選択なし</span><span class="sxs-lookup"><span data-stu-id="571b4-309">No selection</span></span>

<span data-ttu-id="571b4-310">複数のページで "未指定" オプションを使用しているとわかった場合は、テンプレートを作成して HTML の繰り返しを除去することができます。</span><span class="sxs-lookup"><span data-stu-id="571b4-310">If you find yourself using the "not specified" option in multiple pages, you can create a template to eliminate repeating the HTML:</span></span>

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEmptyTemplate.cshtml?highlight=5)]

<span data-ttu-id="571b4-311">*Views/Shared/EditorTemplates/CountryViewModel.cshtml* テンプレート:</span><span class="sxs-lookup"><span data-stu-id="571b4-311">The *Views/Shared/EditorTemplates/CountryViewModel.cshtml* template:</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Shared/EditorTemplates/CountryViewModel.cshtml)]

<span data-ttu-id="571b4-312">HTML の [\<option>](https://www.w3.org/wiki/HTML/Elements/option) 要素の追加は、*選択なし*の場合に限定されません。</span><span class="sxs-lookup"><span data-stu-id="571b4-312">Adding HTML [\<option>](https://www.w3.org/wiki/HTML/Elements/option) elements isn't limited to the *No selection* case.</span></span> <span data-ttu-id="571b4-313">たとえば、次のビューおよびアクション メソッドで、上記のコードのような HTML が生成されます。</span><span class="sxs-lookup"><span data-stu-id="571b4-313">For example, the following view and action method will generate HTML similar to the code above:</span></span>

[!code-csharp[](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

[!code-HTML[](working-with-forms/sample/final/Views/Home/IndexOption.cshtml)]

<span data-ttu-id="571b4-314">現在の `Country` 値に応じて、(`selected="selected"` 属性を含む) 正しい `<option>` 要素が選択されます。</span><span class="sxs-lookup"><span data-stu-id="571b4-314">The correct `<option>` element will be selected ( contain the `selected="selected"` attribute) depending on the current `Country` value.</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="571b4-315">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="571b4-315">Additional resources</span></span>

* [<span data-ttu-id="571b4-316">タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="571b4-316">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="571b4-317">HTML の Form 要素</span><span class="sxs-lookup"><span data-stu-id="571b4-317">HTML Form element</span></span>](https://www.w3.org/TR/html401/interact/forms.html)
* [<span data-ttu-id="571b4-318">要求検証トークン</span><span class="sxs-lookup"><span data-stu-id="571b4-318">Request Verification Token</span></span>](/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages)
* [<span data-ttu-id="571b4-319">モデル バインド</span><span class="sxs-lookup"><span data-stu-id="571b4-319">Model Binding</span></span>](xref:mvc/models/model-binding)
* [<span data-ttu-id="571b4-320">モデルの検証</span><span class="sxs-lookup"><span data-stu-id="571b4-320">Model Validation</span></span>](xref:mvc/models/validation)
* [<span data-ttu-id="571b4-321">IAttributeAdapter インターフェイス</span><span class="sxs-lookup"><span data-stu-id="571b4-321">IAttributeAdapter Interface</span></span>](/dotnet/api/Microsoft.AspNetCore.Mvc.DataAnnotations.IAttributeAdapter)
* [<span data-ttu-id="571b4-322">このドキュメントのコード スニペット</span><span class="sxs-lookup"><span data-stu-id="571b4-322">Code snippets for this document</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/working-with-forms/sample/final)
