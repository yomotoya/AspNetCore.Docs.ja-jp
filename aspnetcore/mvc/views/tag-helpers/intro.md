---
title: "ASP.NET Core のタグ ヘルパー"
author: rick-anderson
description: "タグ ヘルパーと、ASP.NET Core でのその使用方法について説明します。"
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 7/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/intro
ms.openlocfilehash: 939eccd45ec437f379fb9349c24246cc0683528b
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-tag-helpers-in-aspnet-core"></a><span data-ttu-id="b3854-103">ASP.NET Core のタグ ヘルパーの概要</span><span class="sxs-lookup"><span data-stu-id="b3854-103">Introduction to Tag Helpers in ASP.NET Core</span></span> 

<span data-ttu-id="b3854-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b3854-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="what-are-tag-helpers"></a><span data-ttu-id="b3854-105">タグ ヘルパーとは</span><span class="sxs-lookup"><span data-stu-id="b3854-105">What are Tag Helpers?</span></span>

<span data-ttu-id="b3854-106">タグ ヘルパーを使用することで、Razor ファイルでの HTML 要素の作成とレンダリングに、サーバー側コードを組み込むことができます。</span><span class="sxs-lookup"><span data-stu-id="b3854-106">Tag Helpers enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="b3854-107">たとえば、組み込みの `ImageTagHelper` では、イメージ名にバージョン番号を追加することができます。</span><span class="sxs-lookup"><span data-stu-id="b3854-107">For example, the built-in `ImageTagHelper` can append a version number to the image name.</span></span> <span data-ttu-id="b3854-108">イメージが変更されるたびに、サーバーはイメージに対して新しい一意のバージョンを生成するため、クライアントは (キャッシュされた古いイメージではなく) 現在のイメージを確実に取得できます。</span><span class="sxs-lookup"><span data-stu-id="b3854-108">Whenever the image changes, the server generates a new unique version for the image, so clients are guaranteed to get the current image (instead of a stale cached image).</span></span> <span data-ttu-id="b3854-109">フォームやリンクの作成、資産の読み込みなど、一般的なタスクの組み込みのタグ ヘルパーは数多くあります。パブリック GitHub リポジトリで NuGet パッケージとして使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="b3854-109">There are many built-in Tag Helpers for common tasks - such as creating forms, links, loading assets and more - and even more available in public GitHub repositories and as NuGet packages.</span></span> <span data-ttu-id="b3854-110">タグ ヘルパーは C# で作成され、要素名、属性名、または親タグに基づいて HTML 要素をターゲットとします。</span><span class="sxs-lookup"><span data-stu-id="b3854-110">Tag Helpers are authored in C#, and they target HTML elements based on element name, attribute name, or parent tag.</span></span> <span data-ttu-id="b3854-111">たとえば、`LabelTagHelper` 属性が適用されている場合、組み込みの `LabelTagHelper` では HTML `<label>` 要素をターゲットとすることができます。</span><span class="sxs-lookup"><span data-stu-id="b3854-111">For example, the built-in `LabelTagHelper` can target the HTML `<label>` element when the `LabelTagHelper` attributes are applied.</span></span> <span data-ttu-id="b3854-112">[HTML ヘルパー](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers)に慣れていれば、タグ ヘルパーを使用したときに、Razor ビューでの HTML と C# の間の明示的な切り替えが減ることがわかります。</span><span class="sxs-lookup"><span data-stu-id="b3854-112">If you're familiar with [HTML Helpers](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers), Tag Helpers reduce the explicit transitions between HTML and C# in Razor views.</span></span> <span data-ttu-id="b3854-113">多くの場合、HTML ヘルパーでは特定のタグ ヘルパーの代替方法が提供されますが、タグ ヘルパーを HTML ヘルパーの代わりに使用することはできないことと、各 HTML ヘルパーに対応するタグ ヘルパーがないことを認識することが重要です。</span><span class="sxs-lookup"><span data-stu-id="b3854-113">In many cases, HTML Helpers provide an alternative approach to a specific Tag Helper, but it's important to recognize that Tag Helpers don't replace HTML Helpers and there's not a Tag Helper for each HTML Helper.</span></span> <span data-ttu-id="b3854-114">2 つの違いの詳細については、「[タグ ヘルパーと HTML ヘルパーの比較](#tag-helpers-compared-to-html-helpers)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="b3854-114">[Tag Helpers compared to HTML Helpers](#tag-helpers-compared-to-html-helpers) explains the differences in more detail.</span></span>

## <a name="what-tag-helpers-provide"></a><span data-ttu-id="b3854-115">タグ ヘルパーで提供されるもの</span><span class="sxs-lookup"><span data-stu-id="b3854-115">What Tag Helpers provide</span></span>

<span data-ttu-id="b3854-116">**HTML に適した開発エクスペリエンス** タグ ヘルパーを使用する Razor マークアップの大部分は、標準の HTML のように見えます。</span><span class="sxs-lookup"><span data-stu-id="b3854-116">**An HTML-friendly development experience** For the most part, Razor markup using Tag Helpers looks like standard HTML.</span></span> <span data-ttu-id="b3854-117">HTML/CSS/JavaScript に慣れているフロントエンドのデザイナーは、C# Razor 構文について学習しなくても Razor を編集できます。</span><span class="sxs-lookup"><span data-stu-id="b3854-117">Front-end designers conversant with HTML/CSS/JavaScript can edit Razor without learning C# Razor syntax.</span></span>

<span data-ttu-id="b3854-118">**HTML および Razor マークアップを作成するための豊富な IntelliSense 環境** これは HTML ヘルパー (Razor ビューのマークアップをサーバー側で作成する前述の方法) とは大きく異なります。</span><span class="sxs-lookup"><span data-stu-id="b3854-118">**A rich IntelliSense environment for creating HTML and Razor markup** This is in sharp contrast to HTML Helpers, the previous approach to server-side creation of markup in Razor views.</span></span> <span data-ttu-id="b3854-119">2 つの違いの詳細については、「[タグ ヘルパーと HTML ヘルパーの比較](#tag-helpers-compared-to-html-helpers)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="b3854-119">[Tag Helpers compared to HTML Helpers](#tag-helpers-compared-to-html-helpers) explains the differences in more detail.</span></span> <span data-ttu-id="b3854-120">IntelliSense 環境については、「[Intellisense でのタグ ヘルパーのサポート](#intellisense-support-for-tag-helpers)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="b3854-120">[IntelliSense support for Tag Helpers](#intellisense-support-for-tag-helpers) explains the IntelliSense environment.</span></span> <span data-ttu-id="b3854-121">Razor C# 構文に慣れている開発者でも、C# Razor マークアップを書き込むより、タグ ヘルパーを使用するほうが生産性を高めることができます。</span><span class="sxs-lookup"><span data-stu-id="b3854-121">Even developers experienced with Razor C# syntax are more productive using Tag Helpers than writing C# Razor markup.</span></span>

<span data-ttu-id="b3854-122">**生産性を高め、サーバーでのみ使用可能な情報を使用することで、より堅牢で信頼できる保守しやすいコードを生成できる方法** たとえば、これまでは、イメージを変更する際にそのイメージ名を変更することがイメージ更新の理念でした。</span><span class="sxs-lookup"><span data-stu-id="b3854-122">**A way to make you more productive and able to produce more robust, reliable, and maintainable code using information only available on the server** For example, historically the mantra on updating images was to change the name of the image when you change the image.</span></span> <span data-ttu-id="b3854-123">パフォーマンス上の理由から、イメージを積極的にキャッシュする必要があり、イメージの名前を変更しない限り、クライアントが古いコピーを取得する危険性があります。</span><span class="sxs-lookup"><span data-stu-id="b3854-123">Images should be aggressively cached for performance reasons, and unless you change the name of an image, you risk clients getting a stale copy.</span></span> <span data-ttu-id="b3854-124">これまでは、イメージの編集後に、名前を変更する必要があり、Web アプリでのイメージへの各参照を更新する必要がありました。</span><span class="sxs-lookup"><span data-stu-id="b3854-124">Historically, after an image was edited, the name had to be changed and each reference to the image in the web app needed to be updated.</span></span> <span data-ttu-id="b3854-125">これは非常に労力がかかるだけでなく、エラーが発生しやくなります (参照が見つからなかったり、誤って間違った文字列を入力したりする場合がある)。組み込みの `ImageTagHelper` ではこれを自動的に行うことができます。</span><span class="sxs-lookup"><span data-stu-id="b3854-125">Not only is this very labor intensive, it's also error prone (you could miss a reference, accidentally enter the wrong string, etc.) The built-in `ImageTagHelper` can do this for you automatically.</span></span> <span data-ttu-id="b3854-126">`ImageTagHelper` ではイメージ名にバージョン番号を追加できるため、イメージが変更されるたびに、サーバーはそのイメージに対して新しい一意のバージョンを自動的に生成します。</span><span class="sxs-lookup"><span data-stu-id="b3854-126">The `ImageTagHelper` can append a version number to the image name, so whenever the image changes, the server automatically generates a new unique version for the image.</span></span> <span data-ttu-id="b3854-127">クライアントは現在のイメージを確実に取得できます。</span><span class="sxs-lookup"><span data-stu-id="b3854-127">Clients are guaranteed to get the current image.</span></span> <span data-ttu-id="b3854-128">`ImageTagHelper` を使用することで、このような堅牢性と省力化を基本的に自由に実現できます。</span><span class="sxs-lookup"><span data-stu-id="b3854-128">This robustness and labor savings comes essentially free by using the `ImageTagHelper`.</span></span>

<span data-ttu-id="b3854-129">組み込みのタグ ヘルパーのほとんどは既存の HTML 要素をターゲットとし、要素に対してサーバー側の属性を提供します。</span><span class="sxs-lookup"><span data-stu-id="b3854-129">Most of the built-in Tag Helpers target existing HTML elements and provide server-side attributes for the element.</span></span> <span data-ttu-id="b3854-130">たとえば、*Views/Account* フォルダーの多くのビューで使用される `<input>` 要素には `asp-for` 属性が含まれており、指定されたモデル プロパティの名前をレンダリングされる HTML に抽出します。</span><span class="sxs-lookup"><span data-stu-id="b3854-130">For example, the `<input>` element used in many of the views in the *Views/Account* folder contains the `asp-for` attribute, which extracts the name of the specified model property into the rendered HTML.</span></span> <span data-ttu-id="b3854-131">次の Razor マークアップでは、</span><span class="sxs-lookup"><span data-stu-id="b3854-131">The following Razor markup:</span></span>

```cshtml
<label asp-for="Email"></label>
```

<span data-ttu-id="b3854-132">次の HTML が生成されます。</span><span class="sxs-lookup"><span data-stu-id="b3854-132">Generates the following HTML:</span></span>

```cshtml
<label for="Email">Email</label>
```

<span data-ttu-id="b3854-133">`asp-for` 属性は、`LabelTagHelper` の `For` プロパティで使用できます。</span><span class="sxs-lookup"><span data-stu-id="b3854-133">The `asp-for` attribute is made available by the `For` property in the `LabelTagHelper`.</span></span> <span data-ttu-id="b3854-134">詳細については、「[タグ ヘルパーの作成](authoring.md)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="b3854-134">See [Authoring Tag Helpers](authoring.md) for more information.</span></span>

## <a name="managing-tag-helper-scope"></a><span data-ttu-id="b3854-135">タグ ヘルパーのスコープの管理</span><span class="sxs-lookup"><span data-stu-id="b3854-135">Managing Tag Helper scope</span></span>

<span data-ttu-id="b3854-136">タグ ヘルパーのスコープは、`@addTagHelper`、`@removeTagHelper`、"!" オプトアウト文字の組み合わせによって制御されます。</span><span class="sxs-lookup"><span data-stu-id="b3854-136">Tag Helpers scope is controlled by a combination of `@addTagHelper`, `@removeTagHelper`, and the "!" opt-out character.</span></span>

<a name="add-helper-label"></a>

### <a name="addtaghelper-makes-tag-helpers-available"></a><span data-ttu-id="b3854-137">`@addTagHelper` でタグ ヘルパーを使用可能にする</span><span class="sxs-lookup"><span data-stu-id="b3854-137">`@addTagHelper` makes Tag Helpers available</span></span>

<span data-ttu-id="b3854-138">*AuthoringTagHelpers* という名前の新しい ASP.NET Core Web アプリを作成する場合 (認証なし)、以下の *Views/_ViewImports.cshtml* ファイルがプロジェクトに追加されます。</span><span class="sxs-lookup"><span data-stu-id="b3854-138">If you create a new ASP.NET Core web app named *AuthoringTagHelpers* (with no authentication), the following *Views/_ViewImports.cshtml* file will be added to your project:</span></span>

[!code-cshtml[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=2&range=2-3)]

<span data-ttu-id="b3854-139">`@addTagHelper` ディレクティブにより、ビューでタグ ヘルパーが使用可能になります。</span><span class="sxs-lookup"><span data-stu-id="b3854-139">The `@addTagHelper` directive makes Tag Helpers available to the view.</span></span> <span data-ttu-id="b3854-140">この場合、ビュー ファイルは *Views/_ViewImports.cshtml* であり、既定では *Views* フォルダーとサブディレクトリのすべてのビュー ファイルによって継承され、タグ ヘルパーが使用可能になります。</span><span class="sxs-lookup"><span data-stu-id="b3854-140">In this case, the view file is *Views/_ViewImports.cshtml*, which by default is inherited by all view files in the *Views* folder and sub-directories; making Tag Helpers available.</span></span> <span data-ttu-id="b3854-141">上記のコードではワイルドカード構文 ("\*") を使用して、指定されたアセンブリ (*Microsoft.AspNetCore.Mvc.TagHelpers*) 内のすべてのタグ ヘルパーが、*Views* ディレクトリまたはサブディレクトリ内のすべてのビュー ファイルで使用可能になるように指定します。</span><span class="sxs-lookup"><span data-stu-id="b3854-141">The code above uses the wildcard syntax ("\*") to specify that all Tag Helpers in the specified assembly (*Microsoft.AspNetCore.Mvc.TagHelpers*) will be available to every view file in the *Views* directory or sub-directory.</span></span> <span data-ttu-id="b3854-142">`@addTagHelper` の後の最初のパラメーターでは読み込むタグ ヘルパーを指定し (すべてのタグ ヘルパーに対して "\*" を使用)、2 番目のパラメーター "Microsoft.AspNetCore.Mvc.TagHelpers" ではタグ ヘルパーを含むアセンブリを指定します。</span><span class="sxs-lookup"><span data-stu-id="b3854-142">The first parameter after `@addTagHelper` specifies the Tag Helpers to load (we are using "\*" for all Tag Helpers), and the second parameter "Microsoft.AspNetCore.Mvc.TagHelpers" specifies the assembly containing the Tag Helpers.</span></span> <span data-ttu-id="b3854-143">*Microsoft.AspNetCore.Mvc.TagHelpers* は、組み込み ASP.NET Core タグ ヘルパーのアセンブリです。</span><span class="sxs-lookup"><span data-stu-id="b3854-143">*Microsoft.AspNetCore.Mvc.TagHelpers* is the assembly for the built-in ASP.NET Core Tag Helpers.</span></span>

<span data-ttu-id="b3854-144">すべてのタグ ヘルパーをこのプロジェクト (*AuthoringTagHelpers* という名前のアセンブリを作成する) で公開するには、以下を使用します。</span><span class="sxs-lookup"><span data-stu-id="b3854-144">To expose all of the Tag Helpers in this project (which creates an assembly named *AuthoringTagHelpers*), you would use the following:</span></span>

[!code-cshtml[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=3)]

<span data-ttu-id="b3854-145">プロジェクトに既定の名前空間がある `EmailTagHelper` が含まれている場合は (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`)、タグ ヘルパーの完全修飾名 (FQN) を指定できます。</span><span class="sxs-lookup"><span data-stu-id="b3854-145">If your project contains an `EmailTagHelper` with the default namespace (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), you can provide the fully qualified name (FQN) of the Tag Helper:</span></span>

```cshtml
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```

<span data-ttu-id="b3854-146">FQN を使用してビューにタグ ヘルパーを追加するには、最初に FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`) を追加してから、アセンブリ名 (*AuthoringTagHelpers*) を追加します。</span><span class="sxs-lookup"><span data-stu-id="b3854-146">To add a Tag Helper to a view using an FQN, you first add the FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), and then the assembly name (*AuthoringTagHelpers*).</span></span> <span data-ttu-id="b3854-147">ほとんどの開発者は、"\*" ワイルドカード構文を使用するほうを選びます。</span><span class="sxs-lookup"><span data-stu-id="b3854-147">Most developers prefer to use the  "\*" wildcard syntax.</span></span> <span data-ttu-id="b3854-148">ワイルドカード構文を使用することで、FQN のサフィックスとしてワイルドカード文字 "\*" を挿入できます。</span><span class="sxs-lookup"><span data-stu-id="b3854-148">The wildcard syntax allows you to insert the wildcard character "\*" as the suffix in an FQN.</span></span> <span data-ttu-id="b3854-149">たとえば、以下のディレクティブのいずれかで `EmailTagHelper` を取り込みます。</span><span class="sxs-lookup"><span data-stu-id="b3854-149">For example, any of the following directives will bring in the `EmailTagHelper`:</span></span>

```cshtml
@addTagHelper AuthoringTagHelpers.TagHelpers.E*, AuthoringTagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.Email*, AuthoringTagHelpers
```

<span data-ttu-id="b3854-150">前述のとおり、`@addTagHelper` ディレクティブを *Views/_ViewImports.cshtml* ファイルに追加すると、タグ ヘルパーを *Views* ディレクトリとサブディレクトリ内のすべてのビュー ファイルで使用できるようになります。</span><span class="sxs-lookup"><span data-stu-id="b3854-150">As mentioned previously, adding the `@addTagHelper` directive to the *Views/_ViewImports.cshtml* file makes the Tag Helper available to all view files in the *Views* directory and sub-directories.</span></span> <span data-ttu-id="b3854-151">これらのビューにのみタグ ヘルパーを公開することを選択する場合は、特定のビュー ファイルで `@addTagHelper` ディレクティブを使用できます。</span><span class="sxs-lookup"><span data-stu-id="b3854-151">You can use the `@addTagHelper` directive in specific view files if you want to opt-in to exposing the Tag Helper to only those views.</span></span>

<a name="remove-razor-directives-label"></a>

### <a name="removetaghelper-removes-tag-helpers"></a><span data-ttu-id="b3854-152">`@removeTagHelper` でタグ ヘルパーを削除する</span><span class="sxs-lookup"><span data-stu-id="b3854-152">`@removeTagHelper` removes Tag Helpers</span></span>

<span data-ttu-id="b3854-153">`@removeTagHelper` には `@addTagHelper` と同じ 2 つのパラメーターがあり、以前に追加されたタグ ヘルパーを削除します。</span><span class="sxs-lookup"><span data-stu-id="b3854-153">The `@removeTagHelper` has the same two parameters as `@addTagHelper`, and it removes a Tag Helper that was previously added.</span></span> <span data-ttu-id="b3854-154">たとえば、特定のビューに適用された `@removeTagHelper` では、指定されたタグ ヘルパーをビューから削除します。</span><span class="sxs-lookup"><span data-stu-id="b3854-154">For example, `@removeTagHelper` applied to a specific view removes the specified Tag Helper from the view.</span></span> <span data-ttu-id="b3854-155">*Views/Folder/_ViewImports.cshtml* ファイルで `@removeTagHelper` を使用すると、*Folder* 内のすべてのビューから指定されたタグ ヘルパーが削除されます。</span><span class="sxs-lookup"><span data-stu-id="b3854-155">Using `@removeTagHelper` in a *Views/Folder/_ViewImports.cshtml* file removes the specified Tag Helper from all of the views in *Folder*.</span></span>

### <a name="controlling-tag-helper-scope-with-the-viewimportscshtml-file"></a><span data-ttu-id="b3854-156">*_ViewImports.cshtml* ファイルによるタグ ヘルパー スコープの制御</span><span class="sxs-lookup"><span data-stu-id="b3854-156">Controlling Tag Helper scope with the *_ViewImports.cshtml* file</span></span>

<span data-ttu-id="b3854-157">*_ViewImports.cshtml* を任意のビュー フォルダーに追加することができ、ビュー エンジンではそのファイルと *Views/_ViewImports.cshtml* ファイルの両方のディレクティブが適用されます。</span><span class="sxs-lookup"><span data-stu-id="b3854-157">You can add a *_ViewImports.cshtml* to any view folder, and the view engine applies the directives from both that file and the *Views/_ViewImports.cshtml* file.</span></span> <span data-ttu-id="b3854-158">*Home* ビューに対して空の *Views/Home/_ViewImports.cshtml* ファイルを追加した場合、*_ViewImports.cshtml* ファイルは付加的なものであるため、何も変わりません。</span><span class="sxs-lookup"><span data-stu-id="b3854-158">If you added an empty *Views/Home/_ViewImports.cshtml* file for the *Home* views, there would be no change because the *_ViewImports.cshtml* file is additive.</span></span> <span data-ttu-id="b3854-159">*Views/Home/_ViewImports.cshtml* ファイルに追加する `@addTagHelper` ディレクティブ (既定の *Views/_ViewImports.cshtml* ファイルにはない) では、これらのタグ ヘルパーが *Home* フォルダー内のビューにのみ公開されます。</span><span class="sxs-lookup"><span data-stu-id="b3854-159">Any `@addTagHelper` directives you add to the *Views/Home/_ViewImports.cshtml* file (that are not in the default *Views/_ViewImports.cshtml* file) would expose those Tag Helpers to views only in the *Home* folder.</span></span>

<a name="opt-out"></a>

### <a name="opting-out-of-individual-elements"></a><span data-ttu-id="b3854-160">個々の要素の無効化</span><span class="sxs-lookup"><span data-stu-id="b3854-160">Opting out of individual elements</span></span>

<span data-ttu-id="b3854-161">タグ ヘルパーのオプトアウト文字 ("!") を使用して、要素レベルでタグ ヘルパーを無効にすることができます。</span><span class="sxs-lookup"><span data-stu-id="b3854-161">You can disable a Tag Helper at the element level with the Tag Helper opt-out character ("!").</span></span> <span data-ttu-id="b3854-162">たとえば、`Email` 検証は、タグ ヘルパーのオプトアウト文字を使用して `<span>` で無効にすることができます。</span><span class="sxs-lookup"><span data-stu-id="b3854-162">For example, `Email` validation is disabled in the `<span>` with the Tag Helper opt-out character:</span></span>

```cshtml
<!span asp-validation-for="Email" class="text-danger"></!span>
```

<span data-ttu-id="b3854-163">開始タグと終了タグにタグ ヘルパーのオプトアウト文字を適用する必要があります </span><span class="sxs-lookup"><span data-stu-id="b3854-163">You must apply the Tag Helper opt-out character to the opening and closing tag.</span></span> <span data-ttu-id="b3854-164">(Visual Studio エディターでは、ユーザーがオプトアウト文字を開始タグに追加すると、自動的にオプトアウト文字が終了タグに追加されます)。</span><span class="sxs-lookup"><span data-stu-id="b3854-164">(The Visual Studio editor automatically adds the opt-out character to the closing tag when you add one to the opening tag).</span></span> <span data-ttu-id="b3854-165">オプトアウト文字を追加した後、要素とタグ ヘルパーの属性は独特なフォントで表示されなくなります。</span><span class="sxs-lookup"><span data-stu-id="b3854-165">After you add the opt-out character, the element and Tag Helper attributes are no longer displayed in a distinctive font.</span></span>

<a name="prefix-razor-directives-label"></a>

### <a name="using-taghelperprefix-to-make-tag-helper-usage-explicit"></a><span data-ttu-id="b3854-166">`@tagHelperPrefix` を使用してタグ ヘルパーの使用状況を明確にする</span><span class="sxs-lookup"><span data-stu-id="b3854-166">Using `@tagHelperPrefix` to make Tag Helper usage explicit</span></span>

<span data-ttu-id="b3854-167">`@tagHelperPrefix` ディレクティブでは、タグ プレフィックス文字列を指定して、タグ ヘルパーのサポートを有効にしたり、タグ ヘルパーの使用状況を明確にすることができます。</span><span class="sxs-lookup"><span data-stu-id="b3854-167">The `@tagHelperPrefix` directive allows you to specify a tag prefix string to enable Tag Helper support and to make Tag Helper usage explicit.</span></span> <span data-ttu-id="b3854-168">たとえば、*Views/_ViewImports.cshtml* ファイルに次のマークアップを追加できます。</span><span class="sxs-lookup"><span data-stu-id="b3854-168">For example, you could add the following markup to the *Views/_ViewImports.cshtml* file:</span></span>

```cshtml
@tagHelperPrefix th:
```
<span data-ttu-id="b3854-169">以下のコード イメージでは、タグ ヘルパーのプレフィックスが `th:` に設定されているため、プレフィックス `th:` を使用する要素のみでタグ ヘルパーがサポートされます (タグ ヘルパーが有効な要素には独特なフォントがあります)。</span><span class="sxs-lookup"><span data-stu-id="b3854-169">In the code image below, the Tag Helper prefix is set to `th:`, so only those elements using the prefix `th:` support Tag Helpers (Tag Helper-enabled elements have a distinctive font).</span></span> <span data-ttu-id="b3854-170">`<label>` 要素と `<input>` 要素にはタグ ヘルパー プレフィックスがあり、タグ ヘルパーが有効ですが、`<span>` 要素にはありません。</span><span class="sxs-lookup"><span data-stu-id="b3854-170">The `<label>` and `<input>` elements have the Tag Helper prefix and are Tag Helper-enabled, while the `<span>` element doesn't.</span></span>

![イメージ](intro/_static/thp.png)

<span data-ttu-id="b3854-172">`@addTagHelper` に適用されるものと同じ階層規則が `@tagHelperPrefix` にも適用されます。</span><span class="sxs-lookup"><span data-stu-id="b3854-172">The same hierarchy rules that apply to `@addTagHelper` also apply to `@tagHelperPrefix`.</span></span>

## <a name="intellisense-support-for-tag-helpers"></a><span data-ttu-id="b3854-173">Intellisense でのタグ ヘルパーのサポート</span><span class="sxs-lookup"><span data-stu-id="b3854-173">IntelliSense support for Tag Helpers</span></span>

<span data-ttu-id="b3854-174">Visual Studio で新しい ASP.NET Web アプリを作成するときに、NuGet パッケージ "Microsoft.AspNetCore.Razor.Tools" が追加されます。</span><span class="sxs-lookup"><span data-stu-id="b3854-174">When you create a new ASP.NET web app in Visual Studio, it adds the NuGet package "Microsoft.AspNetCore.Razor.Tools".</span></span> <span data-ttu-id="b3854-175">これは、タグ ヘルパー ツールを追加するパッケージです。</span><span class="sxs-lookup"><span data-stu-id="b3854-175">This is the package that adds Tag Helper tooling.</span></span>

<span data-ttu-id="b3854-176">HTML `<label>` 要素を書き込むことを検討してください。</span><span class="sxs-lookup"><span data-stu-id="b3854-176">Consider writing an HTML `<label>` element.</span></span> <span data-ttu-id="b3854-177">Visual Studio エディターで `<l` を入力するとすぐに、IntelliSense で一致する要素が表示されます。</span><span class="sxs-lookup"><span data-stu-id="b3854-177">As soon as you enter `<l` in the Visual Studio editor, IntelliSense displays matching elements:</span></span>

![イメージ](intro/_static/label.png)

<span data-ttu-id="b3854-179">HTML ヘルプだけでなく、アイコン ("@" symbol with "<>") も表示されます。</span><span class="sxs-lookup"><span data-stu-id="b3854-179">Not only do you get HTML help, but the icon (the "@" symbol with "<>" under it).</span></span>

![イメージ](intro/_static/tagSym.png)

<span data-ttu-id="b3854-181">これは、タグ ヘルパーのターゲットとなる要素を示します。</span><span class="sxs-lookup"><span data-stu-id="b3854-181">identifies the element as targeted by Tag Helpers.</span></span> <span data-ttu-id="b3854-182">純粋な HTML 要素 (`fieldset` など) では "<>" アイコンが表示されます。</span><span class="sxs-lookup"><span data-stu-id="b3854-182">Pure HTML elements (such as the `fieldset`) display the "<>" icon.</span></span>

<span data-ttu-id="b3854-183">純粋な HTML `<label>` タグの場合、HTML タグ (既定の Visual Studio の配色テーマ) が茶色のフォントで、属性が赤で、属性値が青で表示されます。</span><span class="sxs-lookup"><span data-stu-id="b3854-183">A pure HTML `<label>` tag displays the HTML tag (with the default Visual Studio color theme) in a brown font, the attributes in red, and the attribute values in blue.</span></span>

![イメージ](intro/_static/LableHtmlTag.png)

<span data-ttu-id="b3854-185">`<label` を入力した後、IntelliSense では次のように使用可能な HTML/CSS 属性とタグ ヘルパーのターゲットとなる属性がリストされます。</span><span class="sxs-lookup"><span data-stu-id="b3854-185">After you enter `<label`, IntelliSense lists the available HTML/CSS attributes and the Tag Helper-targeted attributes:</span></span>

![イメージ](intro/_static/labelattr.png)

<span data-ttu-id="b3854-187">IntelliSense ステートメント入力候補では、Tab キーを入力して、選択した値を使用してステートメントを完了することができます。</span><span class="sxs-lookup"><span data-stu-id="b3854-187">IntelliSense statement completion allows you to enter the tab key to complete the statement with the selected value:</span></span>

![イメージ](intro/_static/stmtcomplete.png)

<span data-ttu-id="b3854-189">タグ ヘルパー属性が入力されるとすぐに、タグと属性のフォントが変わります。</span><span class="sxs-lookup"><span data-stu-id="b3854-189">As soon as a Tag Helper attribute is entered, the tag and attribute fonts change.</span></span> <span data-ttu-id="b3854-190">既定の Visual Studio の "青" または "ライト" の配色テーマを使用すると、フォントが太字の紫になります。</span><span class="sxs-lookup"><span data-stu-id="b3854-190">Using the default Visual Studio "Blue" or "Light" color theme, the font is bold purple.</span></span> <span data-ttu-id="b3854-191">"ダーク" テーマを使用する場合、フォントは太字の青緑になります。</span><span class="sxs-lookup"><span data-stu-id="b3854-191">If you're using the "Dark" theme the font is bold teal.</span></span> <span data-ttu-id="b3854-192">このドキュメントのイメージは、既定のテーマを使用して取得されたものです。</span><span class="sxs-lookup"><span data-stu-id="b3854-192">The images in this document were taken using the default theme.</span></span>

![イメージ](intro/_static/labelaspfor2.png)

<span data-ttu-id="b3854-194">Visual Studio の *CompleteWord* ショートカット ([既定](https://docs.microsoft.com/visualstudio/ide/default-keyboard-shortcuts-in-visual-studio)は Ctrl + Space) を二重引用符 ("") 内に入力することができ、C# クラスの場合と同じように C# で使用できるようになりました。</span><span class="sxs-lookup"><span data-stu-id="b3854-194">You can enter the Visual Studio *CompleteWord* shortcut (Ctrl +spacebar is the [default](https://docs.microsoft.com/visualstudio/ide/default-keyboard-shortcuts-in-visual-studio) inside the double quotes (""), and you are now in C#, just like you would be in a C# class.</span></span> <span data-ttu-id="b3854-195">IntelliSense では、ページ モデルですべてのメソッドとプロパティが表示されます。</span><span class="sxs-lookup"><span data-stu-id="b3854-195">IntelliSense displays all the methods and properties on the page model.</span></span> <span data-ttu-id="b3854-196">プロパティの種類が `ModelExpression` であるため、メソッドとプロパティを使用できます。</span><span class="sxs-lookup"><span data-stu-id="b3854-196">The methods and properties are available because the property type is `ModelExpression`.</span></span> <span data-ttu-id="b3854-197">次のイメージでは、`Register` ビューを編集するため、`RegisterViewModel` を使用できます。</span><span class="sxs-lookup"><span data-stu-id="b3854-197">In the image below, I'm editing the `Register` view, so the `RegisterViewModel` is available.</span></span>

![イメージ](intro/_static/intellemail.png)

<span data-ttu-id="b3854-199">IntelliSense では、ページのモデルで使用可能なプロパティとメソッドがリストされます。</span><span class="sxs-lookup"><span data-stu-id="b3854-199">IntelliSense lists the properties and methods available to the model on the page.</span></span> <span data-ttu-id="b3854-200">豊富な IntelliSense 環境は、CSS クラスを選択する場合に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="b3854-200">The rich IntelliSense environment helps you select the CSS class:</span></span>

![イメージ](intro/_static/iclass.png)

![イメージ](intro/_static/intel3.png)

## <a name="tag-helpers-compared-to-html-helpers"></a><span data-ttu-id="b3854-203">タグ ヘルパーと HTML ヘルパーの比較</span><span class="sxs-lookup"><span data-stu-id="b3854-203">Tag Helpers compared to HTML Helpers</span></span>

<span data-ttu-id="b3854-204">タグ ヘルパーは Razor ビューで HTML 要素にアタッチされますが、[HTML ヘルパー](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers)は Razor ビューで HTML が混在するメソッドとして呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="b3854-204">Tag Helpers attach to HTML elements in Razor views, while [HTML Helpers](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers) are invoked as methods interspersed with HTML in Razor views.</span></span> <span data-ttu-id="b3854-205">たとえば、CSS クラス "caption" を指定して HTML ラベルを作成する、次のような Razor マークアップがあるとします。</span><span class="sxs-lookup"><span data-stu-id="b3854-205">Consider the following Razor markup, which creates an HTML label with the CSS class "caption":</span></span>

```cshtml
@Html.Label("FirstName", "First Name:", new {@class="caption"})
```

<span data-ttu-id="b3854-206">アットマーク (`@`) は、Razor にこれがコードの先頭であることを示します。</span><span class="sxs-lookup"><span data-stu-id="b3854-206">The at (`@`) symbol tells Razor this is the start of code.</span></span> <span data-ttu-id="b3854-207">次の 2 つのパラメーター ("FirstName" と "First Name:") は文字列であるため、[IntelliSense](https://docs.microsoft.com/visualstudio/ide/using-intellisense) では対応できません。</span><span class="sxs-lookup"><span data-stu-id="b3854-207">The next two parameters ("FirstName" and "First Name:") are strings, so [IntelliSense](https://docs.microsoft.com/visualstudio/ide/using-intellisense) can't help.</span></span> <span data-ttu-id="b3854-208">以下が最後の引数です。</span><span class="sxs-lookup"><span data-stu-id="b3854-208">The last argument:</span></span>

```cshtml
new {@class="caption"}
```

<span data-ttu-id="b3854-209">これは属性を表すために使用される匿名オブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="b3854-209">Is an anonymous object used to represent attributes.</span></span> <span data-ttu-id="b3854-210">**class** は C# の予約済みのキーワードであるため、`@` シンボルを使用して、C# で強制的に "@class=" をシンボル (プロパティ名) として解釈するようにします。</span><span class="sxs-lookup"><span data-stu-id="b3854-210">Because **class** is a reserved keyword in C#, you use the `@` symbol to force C# to interpret "@class=" as a symbol (property name).</span></span> <span data-ttu-id="b3854-211">フロント エンドのデザイナー (HTML/CSS/JavaScript とその他のクライアント テクノロジには慣れているものの、C# や Razor には慣れていないデザイナー) には、行のほとんどが馴染みのないものです。</span><span class="sxs-lookup"><span data-stu-id="b3854-211">To a front-end designer (someone familiar with HTML/CSS/JavaScript and other client technologies but not familiar with C# and Razor), most of the line is foreign.</span></span> <span data-ttu-id="b3854-212">IntelliSense に頼らずに行全体を作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b3854-212">The entire line must be authored with no help from IntelliSense.</span></span>

<span data-ttu-id="b3854-213">`LabelTagHelper` を使用すれば、以下と同じマークアップを書き込むことができます。</span><span class="sxs-lookup"><span data-stu-id="b3854-213">Using the `LabelTagHelper`, the same markup can be written as:</span></span>

![イメージ](intro/_static/label2.png)

<span data-ttu-id="b3854-215">タグ ヘルパー バージョンを使用して Visual Studio エディターで `<l` を入力するとすぐに、IntelliSense で一致する要素が表示されます。</span><span class="sxs-lookup"><span data-stu-id="b3854-215">With the Tag Helper version, as soon as you enter `<l` in the Visual Studio editor, IntelliSense displays matching elements:</span></span>

![イメージ](intro/_static/label.png)

<span data-ttu-id="b3854-217">IntelliSense は行全体を書き込む場合に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="b3854-217">IntelliSense helps you write the entire line.</span></span> <span data-ttu-id="b3854-218">`LabelTagHelper` でも既定で `asp-for` 属性値のコンテンツ ("FirstName") が "First Name" に設定されます。キャメル ケースのプロパティは、新しい大文字がそれぞれ示されるスペースがあるプロパティ名で構成された文に変換されます。</span><span class="sxs-lookup"><span data-stu-id="b3854-218">The `LabelTagHelper` also defaults to setting the content of the `asp-for` attribute value ("FirstName") to "First Name"; It converts camel-cased properties to a sentence composed of the property name with a space where each new upper-case letter occurs.</span></span> <span data-ttu-id="b3854-219">次のマークアップでは、</span><span class="sxs-lookup"><span data-stu-id="b3854-219">In the following markup:</span></span>

![イメージ](intro/_static/label2.png)

<span data-ttu-id="b3854-221">次のように生成されます。</span><span class="sxs-lookup"><span data-stu-id="b3854-221">generates:</span></span>

```cshtml
<label class="caption" for="FirstName">First Name</label>
```

<span data-ttu-id="b3854-222">`<label>` にコンテンツを追加する場合、キャメル ケースからセンテンス ケースのコンテンツは使用されません。</span><span class="sxs-lookup"><span data-stu-id="b3854-222">The camel-cased to sentence-cased content isn't used if you add content to the `<label>`.</span></span> <span data-ttu-id="b3854-223">例:</span><span class="sxs-lookup"><span data-stu-id="b3854-223">For example:</span></span>

![イメージ](intro/_static/1stName.png)

<span data-ttu-id="b3854-225">この場合、次のように生成されます。</span><span class="sxs-lookup"><span data-stu-id="b3854-225">generates:</span></span>

```cshtml
<label class="caption" for="FirstName">Name First</label>
```

<span data-ttu-id="b3854-226">次のコード イメージは、Visual Studio 2015 に含まれているレガシ ASP.NET 4.5.x MVC テンプレートから生成される *Views/Account/Register.cshtml* Razor ビューの Form 部分を示しています。</span><span class="sxs-lookup"><span data-stu-id="b3854-226">The following code image shows the Form portion of the *Views/Account/Register.cshtml* Razor view generated from the legacy ASP.NET 4.5.x MVC template included with Visual Studio 2015.</span></span>

![イメージ](intro/_static/regCS.png)

<span data-ttu-id="b3854-228">Visual Studio エディターでは、背景が灰色の C# コードが表示されます。</span><span class="sxs-lookup"><span data-stu-id="b3854-228">The Visual Studio editor displays C# code with a grey background.</span></span> <span data-ttu-id="b3854-229">たとえば、次の `AntiForgeryToken` HTML ヘルパーの場合、</span><span class="sxs-lookup"><span data-stu-id="b3854-229">For example, the `AntiForgeryToken` HTML Helper:</span></span>

```cshtml
@Html.AntiForgeryToken()
```

<span data-ttu-id="b3854-230">灰色の背景で表示されます。</span><span class="sxs-lookup"><span data-stu-id="b3854-230">is displayed with a grey background.</span></span> <span data-ttu-id="b3854-231">Register ビューのマークアップのほとんどは C# です。</span><span class="sxs-lookup"><span data-stu-id="b3854-231">Most of the markup in the Register view is C#.</span></span> <span data-ttu-id="b3854-232">以下のタグ ヘルパーを使用する同じ方法と比較します。</span><span class="sxs-lookup"><span data-stu-id="b3854-232">Compare that to the equivalent approach using Tag Helpers:</span></span>

![イメージ](intro/_static/regTH.png)

<span data-ttu-id="b3854-234">HTML ヘルパーの方法よりも、マークアップがわかりやすく、読み取り、編集、保持が簡単になります。</span><span class="sxs-lookup"><span data-stu-id="b3854-234">The markup is much cleaner and easier to read, edit, and maintain than the HTML Helpers approach.</span></span> <span data-ttu-id="b3854-235">C# コードは、サーバーで認識する必要がある最低限まで減っています。</span><span class="sxs-lookup"><span data-stu-id="b3854-235">The C# code is reduced to the minimum that the server needs to know about.</span></span> <span data-ttu-id="b3854-236">Visual Studio エディターでは、独特なフォントでタグ ヘルパーのターゲットとなるマークアップが表示されます。</span><span class="sxs-lookup"><span data-stu-id="b3854-236">The Visual Studio editor displays markup targeted by a Tag Helper in a distinctive font.</span></span>

<span data-ttu-id="b3854-237">たとえば、以下の *Email* グループについて考えてみます。</span><span class="sxs-lookup"><span data-stu-id="b3854-237">Consider the *Email* group:</span></span>

[!code-csharp[Main](intro/sample/Register.cshtml?range=12-18)]

<span data-ttu-id="b3854-238">"asp-" 属性にはそれぞれ "Email" の値がありますが、"Email" は文字列ではありません。</span><span class="sxs-lookup"><span data-stu-id="b3854-238">Each of the "asp-" attributes has a value of "Email", but "Email" isn't a string.</span></span> <span data-ttu-id="b3854-239">このコンテキストでは、"Email" は `RegisterViewModel` の C# モデル式のプロパティとなります。</span><span class="sxs-lookup"><span data-stu-id="b3854-239">In this context, "Email" is the C# model expression property for the `RegisterViewModel`.</span></span>

<span data-ttu-id="b3854-240">Visual Studio エディターは登録フォームのタグ ヘルパーの方法で**すべて**のマークアップを書き込む場合に役立ちますが、Visual Studio は HTML ヘルパーの方法ではほとんどのコードに対応できません。</span><span class="sxs-lookup"><span data-stu-id="b3854-240">The Visual Studio editor helps you write **all** of the markup in the Tag Helper approach of the register form, while Visual Studio provides no help for most of the code in the HTML Helpers approach.</span></span> <span data-ttu-id="b3854-241">Visual Studio エディターでのタグ ヘルパーの操作の詳細については、「[IntelliSense でのタグ ヘルパーのサポート](#intellisense-support-for-tag-helpers)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="b3854-241">[IntelliSense support for Tag Helpers](#intellisense-support-for-tag-helpers) goes into detail on working with Tag Helpers in the Visual Studio editor.</span></span>

## <a name="tag-helpers-compared-to-web-server-controls"></a><span data-ttu-id="b3854-242">タグ ヘルパーと Web サーバー コントロールの比較</span><span class="sxs-lookup"><span data-stu-id="b3854-242">Tag Helpers compared to Web Server Controls</span></span>

* <span data-ttu-id="b3854-243">タグ ヘルパーには関連付けられている要素はなく、要素とコンテンツのレンダリングに参加するだけです。</span><span class="sxs-lookup"><span data-stu-id="b3854-243">Tag Helpers don't own the element they're associated with; they simply participate in the rendering of the element and content.</span></span> <span data-ttu-id="b3854-244">ASP.NET [Web サーバー コントロール](https://msdn.microsoft.com/library/7698y1f0.aspx)はページで宣言され、呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="b3854-244">ASP.NET [Web Server controls](https://msdn.microsoft.com/library/7698y1f0.aspx) are declared and invoked on a page.</span></span>

* <span data-ttu-id="b3854-245">[Web サーバー コントロール](https://msdn.microsoft.com/library/zsyt68f1.aspx)には複雑なライフサイクルがあり、これが開発とデバッグを困難にする場合があります。</span><span class="sxs-lookup"><span data-stu-id="b3854-245">[Web Server controls](https://msdn.microsoft.com/library/zsyt68f1.aspx) have a non-trivial lifecycle that can make developing and debugging difficult.</span></span>

* <span data-ttu-id="b3854-246">Web サーバー コントロールではクライアント コントロールを使用して、クライアントのドキュメント オブジェクト モデル (DOM) 要素に機能を追加することができます。</span><span class="sxs-lookup"><span data-stu-id="b3854-246">Web Server controls allow you to add functionality to the client Document Object Model (DOM) elements by using a client control.</span></span> <span data-ttu-id="b3854-247">タグ ヘルパーには DOM がありません。</span><span class="sxs-lookup"><span data-stu-id="b3854-247">Tag Helpers have no DOM.</span></span>

* <span data-ttu-id="b3854-248">Web サーバー コントロールには自動ブラウザー検出が含まれます。</span><span class="sxs-lookup"><span data-stu-id="b3854-248">Web Server controls include automatic browser detection.</span></span> <span data-ttu-id="b3854-249">タグ ヘルパーではブラウザーが認識されません。</span><span class="sxs-lookup"><span data-stu-id="b3854-249">Tag Helpers have no knowledge of the browser.</span></span>

* <span data-ttu-id="b3854-250">複数のタグ ヘルパーを同じ要素で実行できますが ([タグ ヘルパーの競合の回避](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/authoring#avoiding-tag-helper-conflicts)に関するページを参照)、通常は Web サーバー コントロールを構成できません。</span><span class="sxs-lookup"><span data-stu-id="b3854-250">Multiple Tag Helpers can act on the same element (see [Avoiding Tag Helper conflicts](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/authoring#avoiding-tag-helper-conflicts) ) while you typically can't compose Web Server controls.</span></span>

* <span data-ttu-id="b3854-251">タグ ヘルパーではスコープとなる HTML 要素のタグとコンテンツを変更できますが、ページ上の他のものを直接変更しません。</span><span class="sxs-lookup"><span data-stu-id="b3854-251">Tag Helpers can modify the tag and content of HTML elements that they're scoped to, but don't directly modify anything else on a page.</span></span> <span data-ttu-id="b3854-252">Web サーバー コントロールではスコープが限定されないため、ページの他の部分に影響を与えるアクションを実行して、予想外の結果となる場合があります。</span><span class="sxs-lookup"><span data-stu-id="b3854-252">Web Server controls have a less specific scope and can perform actions that affect other parts of your page; enabling unintended side effects.</span></span>

* <span data-ttu-id="b3854-253">Web サーバー コントロールでは型コンバーターを使用して、文字列をオブジェクトに変換します。</span><span class="sxs-lookup"><span data-stu-id="b3854-253">Web Server controls use type converters to convert strings into objects.</span></span> <span data-ttu-id="b3854-254">タグ ヘルパーの場合、C# でネイティブに作業するため、型を変換する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="b3854-254">With Tag Helpers, you work natively in C#, so you don't need to do type conversion.</span></span>

* <span data-ttu-id="b3854-255">Web サーバー コントロールでは [System.ComponentModel](https://docs.microsoft.com/dotnet/api/system.componentmodel) 名前空間を使用して、コンポーネントやコントロールの実行時動作およびデザイン時動作を実装します。</span><span class="sxs-lookup"><span data-stu-id="b3854-255">Web Server controls use [System.ComponentModel](https://docs.microsoft.com/dotnet/api/system.componentmodel) to implement the run-time and design-time behavior of components and controls.</span></span> <span data-ttu-id="b3854-256">`System.ComponentModel` には、属性と型コンバーターの実装、データ ソースへのバインド、コンポーネントのライセンス処理のための基底クラスと基底インターフェイスが含まれています。</span><span class="sxs-lookup"><span data-stu-id="b3854-256">`System.ComponentModel` includes the base classes and interfaces for implementing attributes and type converters, binding to data sources, and licensing components.</span></span> <span data-ttu-id="b3854-257">通常は `TagHelper` から派生するタグ ヘルパーとは異なり、`TagHelper` 基底クラスでは `Process` と `ProcessAsync` の 2 つのメソッドのみを公開します。</span><span class="sxs-lookup"><span data-stu-id="b3854-257">Contrast that to Tag Helpers, which typically derive from `TagHelper`, and the `TagHelper` base class exposes only two methods, `Process` and `ProcessAsync`.</span></span>

## <a name="customizing-the-tag-helper-element-font"></a><span data-ttu-id="b3854-258">タグ ヘルパー要素のフォントのカスタマイズ</span><span class="sxs-lookup"><span data-stu-id="b3854-258">Customizing the Tag Helper element font</span></span>

<span data-ttu-id="b3854-259">次のように、**[ツール]** > **[オプション]** > **[環境]** > **[フォントおよび色]** からフォントと色づけをカスタマイズすることができます。</span><span class="sxs-lookup"><span data-stu-id="b3854-259">You can customize the font and colorization from **Tools** > **Options** > **Environment** > **Fonts and Colors**:</span></span>

![イメージ](intro/_static/fontoptions2.png)

## <a name="additional-resources"></a><span data-ttu-id="b3854-261">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="b3854-261">Additional resources</span></span>

* [<span data-ttu-id="b3854-262">タグ ヘルパーの作成</span><span class="sxs-lookup"><span data-stu-id="b3854-262">Authoring Tag Helpers</span></span>](xref:mvc/views/tag-helpers/authoring)
* [<span data-ttu-id="b3854-263">フォームの操作</span><span class="sxs-lookup"><span data-stu-id="b3854-263">Working with Forms </span></span>](xref:mvc/views/working-with-forms)
* <span data-ttu-id="b3854-264">[GitHub の TagHelperSamples](https://github.com/dpaquette/TagHelperSamples) には、[ブートストラップ](http://getbootstrap.com/)を操作するためのタグ ヘルパーのサンプルが含まれています。</span><span class="sxs-lookup"><span data-stu-id="b3854-264">[TagHelperSamples on GitHub](https://github.com/dpaquette/TagHelperSamples) contains Tag Helper samples for working with [Bootstrap](http://getbootstrap.com/).</span></span>
* [<span data-ttu-id="b3854-265">フォームの操作</span><span class="sxs-lookup"><span data-stu-id="b3854-265">Working with Forms </span></span>](xref:mvc/views/working-with-forms)
