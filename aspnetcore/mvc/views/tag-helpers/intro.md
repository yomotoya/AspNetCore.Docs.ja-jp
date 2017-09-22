---
title: "ASP.NET Core のタグ ヘルパー"
author: rick-anderson
description: "タグ ヘルパーと ASP.NET Core での使用方法について説明します。"
keywords: "ASP.NET Core、タグ ヘルパー"
ms.author: riande
manager: wpickett
ms.date: 7/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/tag-helpers/intro
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 10471210075dc8a5366b7d5170d6594c2e66ce94
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/22/2017
---
# <a name="introduction-to-tag-helpers-in-aspnet-core"></a><span data-ttu-id="2c37b-104">ASP.NET Core のタグ ヘルパーの概要</span><span class="sxs-lookup"><span data-stu-id="2c37b-104">Introduction to Tag Helpers in ASP.NET Core</span></span> 

<span data-ttu-id="2c37b-105">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2c37b-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="what-are-tag-helpers"></a><span data-ttu-id="2c37b-106">タグ ヘルパーは?</span><span class="sxs-lookup"><span data-stu-id="2c37b-106">What are Tag Helpers?</span></span>

<span data-ttu-id="2c37b-107">タグ ヘルパーでは、サーバー側コードを作成して、Razor ファイルでの HTML 要素のレンダリングに参加を有効にします。</span><span class="sxs-lookup"><span data-stu-id="2c37b-107">Tag Helpers enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="2c37b-108">たとえば、組み込み`ImageTagHelper`イメージ名にバージョン番号を追加することができます。</span><span class="sxs-lookup"><span data-stu-id="2c37b-108">For example, the built-in `ImageTagHelper` can append a version number to the image name.</span></span> <span data-ttu-id="2c37b-109">イメージが変更されるたびに、サーバーは、クライアント (古いキャッシュされた画像) ではなく現在のイメージを取得することが保証されますので、イメージの新しい一意のバージョンを生成します。</span><span class="sxs-lookup"><span data-stu-id="2c37b-109">Whenever the image changes, the server generates a new unique version for the image, so clients are guaranteed to get the current image (instead of a stale cached image).</span></span> <span data-ttu-id="2c37b-110">これは、フォーム、リンク、読み込みの資産および詳細 - およびさらに高いで利用できるパブリックの GitHub リポジトリとして NuGet パッケージの作成などの一般的なタスクの多くの組み込みタグ ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="2c37b-110">There are many built-in Tag Helpers for common tasks - such as creating forms, links, loading assets and more - and even more available in public GitHub repositories and as NuGet packages.</span></span> <span data-ttu-id="2c37b-111">タグ ヘルパーは、C# の場合は、作成し、要素名、属性名、または親タグに基づく HTML 要素を対象にします。</span><span class="sxs-lookup"><span data-stu-id="2c37b-111">Tag Helpers are authored in C#, and they target HTML elements based on element name, attribute name, or parent tag.</span></span> <span data-ttu-id="2c37b-112">たとえば、組み込み`LabelTagHelper`HTML を対象にできます`<label>`要素ときに、`LabelTagHelper`属性が適用されます。</span><span class="sxs-lookup"><span data-stu-id="2c37b-112">For example, the built-in `LabelTagHelper` can target the HTML `<label>` element when the `LabelTagHelper` attributes are applied.</span></span> <span data-ttu-id="2c37b-113">慣れている場合[HTML ヘルパー](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers)、タグ ヘルパーは、Razor ビューでの HTML と c# の間で明示的な遷移を削減します。</span><span class="sxs-lookup"><span data-stu-id="2c37b-113">If you're familiar with [HTML Helpers](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers), Tag Helpers reduce the explicit transitions between HTML and C# in Razor views.</span></span> <span data-ttu-id="2c37b-114">多くの場合、HTML ヘルパー、別の方法を特定のタグ ヘルパーに提供するが、タグ ヘルパーの HTML ヘルパーを置き換えないし、各 HTML ヘルパーのタグ ヘルパーがないことを認識することが重要です。</span><span class="sxs-lookup"><span data-stu-id="2c37b-114">In many cases, HTML Helpers provide an alternative approach to a specific Tag Helper, but it's important to recognize that Tag Helpers do not replace HTML Helpers and there is not a Tag Helper for each HTML Helper.</span></span> <span data-ttu-id="2c37b-115">[タグ ヘルパーの HTML ヘルパーと比較して](#tag-helpers-compared-to-html-helpers)詳細の相違点について説明します。</span><span class="sxs-lookup"><span data-stu-id="2c37b-115">[Tag Helpers compared to HTML Helpers](#tag-helpers-compared-to-html-helpers) explains the differences in more detail.</span></span>

## <a name="what-tag-helpers-provide"></a><span data-ttu-id="2c37b-116">タグ ヘルパーの提供</span><span class="sxs-lookup"><span data-stu-id="2c37b-116">What Tag Helpers provide</span></span>

<span data-ttu-id="2c37b-117">**HTML に適した開発エクスペリエンス**大部分 Razor マークアップ タグ ヘルパーを使用して次のように標準 HTML。</span><span class="sxs-lookup"><span data-stu-id="2c37b-117">**An HTML-friendly development experience** For the most part, Razor markup using Tag Helpers looks like standard HTML.</span></span> <span data-ttu-id="2c37b-118">HTML と CSS/JavaScript に精通フロント エンドのデザイナーは、C# の場合は、Razor 構文を習得しなくても、Razor を編集できます。</span><span class="sxs-lookup"><span data-stu-id="2c37b-118">Front-end designers conversant with HTML/CSS/JavaScript can edit Razor without learning C# Razor syntax.</span></span>

<span data-ttu-id="2c37b-119">**HTML および Razor マークアップを作成するための豊富な IntelliSense 環境**これは、HTML ヘルパー、Razor ビューのマークアップのサーバー側で作成する前のアプローチをコントラストをシャープにします。</span><span class="sxs-lookup"><span data-stu-id="2c37b-119">**A rich IntelliSense environment for creating HTML and Razor markup** This is in sharp contrast to HTML Helpers, the previous approach to server-side creation of markup in Razor views.</span></span> <span data-ttu-id="2c37b-120">[タグ ヘルパーの HTML ヘルパーと比較して](#tag-helpers-compared-to-html-helpers)詳細の相違点について説明します。</span><span class="sxs-lookup"><span data-stu-id="2c37b-120">[Tag Helpers compared to HTML Helpers](#tag-helpers-compared-to-html-helpers) explains the differences in more detail.</span></span> <span data-ttu-id="2c37b-121">[タグ ヘルパーの IntelliSense サポート](#intellisense-support-for-tag-helpers)IntelliSense 環境について説明します。</span><span class="sxs-lookup"><span data-stu-id="2c37b-121">[IntelliSense support for Tag Helpers](#intellisense-support-for-tag-helpers) explains the IntelliSense environment.</span></span> <span data-ttu-id="2c37b-122">Razor の c# の構文の使用経験が偶数の開発者は、c# Razor マークアップを記述するよりもタグ ヘルパーを使用して生産性を向上します。</span><span class="sxs-lookup"><span data-stu-id="2c37b-122">Even developers experienced with Razor C# syntax are more productive using Tag Helpers than writing C# Razor markup.</span></span>

<span data-ttu-id="2c37b-123">**作成する方法をより生産的かつ生成できるより堅牢で信頼性が高く、およびサーバーでのみ利用可能な情報を使用してコードを保守しやすい**たとえば、従来イメージを更新する方法の信念でしたを変更すると、イメージの名前を変更するにはイメージです。</span><span class="sxs-lookup"><span data-stu-id="2c37b-123">**A way to make you more productive and able to produce more robust, reliable, and maintainable code using information only available on the server** For example, historically the mantra on updating images was to change the name of the image when you change the image.</span></span> <span data-ttu-id="2c37b-124">パフォーマンス上の理由から、積極的にイメージをキャッシュする必要があり、クライアントの古いコピーを取得する危険性が画像の名前を変更すると、しない限り、します。</span><span class="sxs-lookup"><span data-stu-id="2c37b-124">Images should be aggressively cached for performance reasons, and unless you change the name of an image, you risk clients getting a stale copy.</span></span> <span data-ttu-id="2c37b-125">従来、イメージが編集された後に名前が変更されましたし、web アプリのイメージへの参照の更新が必要です。</span><span class="sxs-lookup"><span data-stu-id="2c37b-125">Historically, after an image was edited, the name had to be changed and each reference to the image in the web app needed to be updated.</span></span> <span data-ttu-id="2c37b-126">これは、処理を要するもエラーになります (する可能性がありますの参照を誤って間違った文字列などを入力します。) が発生しやすいが非常に労力だけでなく組み込み`ImageTagHelper`できますこの処理を自動的にします。</span><span class="sxs-lookup"><span data-stu-id="2c37b-126">Not only is this very labor intensive, it's also error prone (you could miss a reference, accidentally enter the wrong string, etc.) The built-in `ImageTagHelper` can do this for you automatically.</span></span> <span data-ttu-id="2c37b-127">`ImageTagHelper`バージョンに追加することができます、イメージが変更されるたびに、サーバーを自動的に生成イメージの一意の新しいバージョンようにをイメージ名番号。</span><span class="sxs-lookup"><span data-stu-id="2c37b-127">The `ImageTagHelper` can append a version number to the image name, so whenever the image changes, the server automatically generates a new unique version for the image.</span></span> <span data-ttu-id="2c37b-128">クライアントを現在のイメージを取得することが保証されます。</span><span class="sxs-lookup"><span data-stu-id="2c37b-128">Clients are guaranteed to get the current image.</span></span> <span data-ttu-id="2c37b-129">使用してこの堅牢性と人件費の節約は本質的には無料、`ImageTagHelper`です。</span><span class="sxs-lookup"><span data-stu-id="2c37b-129">This robustness and labor savings comes essentially free by using the `ImageTagHelper`.</span></span>

<span data-ttu-id="2c37b-130">組み込みタグ ヘルパーのほとんどは、既存の HTML 要素をターゲットし、要素のサーバー側の属性を指定します。</span><span class="sxs-lookup"><span data-stu-id="2c37b-130">Most of the built-in Tag Helpers target existing HTML elements and provide server-side attributes for the element.</span></span> <span data-ttu-id="2c37b-131">たとえば、`<input>`要素内のビューの多くで使用されて、*ビュー/アカウント*フォルダーが含まれています、`asp-for`属性は、レンダリングされる HTML に指定されたモデルのプロパティの名前を抽出します。</span><span class="sxs-lookup"><span data-stu-id="2c37b-131">For example, the `<input>` element used in many of the views in the *Views/Account* folder contains the `asp-for` attribute, which extracts the name of the specified model property into the rendered HTML.</span></span> <span data-ttu-id="2c37b-132">次の Razor マークアップ。</span><span class="sxs-lookup"><span data-stu-id="2c37b-132">The following Razor markup:</span></span>

```html
<label asp-for="Email"></label>
```

<span data-ttu-id="2c37b-133">次の HTML を生成します。</span><span class="sxs-lookup"><span data-stu-id="2c37b-133">Generates the following HTML:</span></span>

```html
<label for="Email">Email</label>
```

<span data-ttu-id="2c37b-134">`asp-for`属性を使用可能で、`For`プロパティに、`LabelTagHelper`です。</span><span class="sxs-lookup"><span data-stu-id="2c37b-134">The `asp-for` attribute is made available by the `For` property in the `LabelTagHelper`.</span></span> <span data-ttu-id="2c37b-135">参照してください[タグ ヘルパーのオーサリング](authoring.md)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="2c37b-135">See [Authoring Tag Helpers](authoring.md) for more information.</span></span>

## <a name="managing-tag-helper-scope"></a><span data-ttu-id="2c37b-136">タグ ヘルパーのスコープを管理します。</span><span class="sxs-lookup"><span data-stu-id="2c37b-136">Managing Tag Helper scope</span></span>

<span data-ttu-id="2c37b-137">タグ ヘルパーのスコープがの組み合わせによって制御される`@addTagHelper`、 `@removeTagHelper`、および"!"オプトアウト文字です。</span><span class="sxs-lookup"><span data-stu-id="2c37b-137">Tag Helpers scope is controlled by a combination of `@addTagHelper`, `@removeTagHelper`, and the "!" opt-out character.</span></span>

<a name=add-helper-label></a>

### <a name="addtaghelper-makes-tag-helpers-available"></a><span data-ttu-id="2c37b-138">`@addTagHelper`タグ ヘルパーを使用可能します。</span><span class="sxs-lookup"><span data-stu-id="2c37b-138">`@addTagHelper` makes Tag Helpers available</span></span>

<span data-ttu-id="2c37b-139">という名前の新しい ASP.NET Core web アプリを作成するかどうかは*AuthoringTagHelpers* (認証なしで)、次*Views/_ViewImports.cshtml*ファイルは、プロジェクトに追加されます。</span><span class="sxs-lookup"><span data-stu-id="2c37b-139">If you create a new ASP.NET Core web app named *AuthoringTagHelpers* (with no authentication), the following *Views/_ViewImports.cshtml* file will be added to your project:</span></span>

[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=2&range=2-3)]

<span data-ttu-id="2c37b-140">`@addTagHelper`ディレクティブにより、タグ ヘルパーのビューに表示します。</span><span class="sxs-lookup"><span data-stu-id="2c37b-140">The `@addTagHelper` directive makes Tag Helpers available to the view.</span></span> <span data-ttu-id="2c37b-141">ここでは、ファイルの表示は*Views/_ViewImports.cshtml*、内のすべてのビュー ファイルによって継承される既定で、*ビュー*フォルダーとサブディレクトリ; タグ ヘルパーを使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="2c37b-141">In this case, the view file is *Views/_ViewImports.cshtml*, which by default is inherited by all view files in the *Views* folder and sub-directories; making Tag Helpers available.</span></span> <span data-ttu-id="2c37b-142">上記のコードでは、ワイルドカードの構文を使用して ("\*") ことを指定する、指定したアセンブリ内のすべてのタグ ヘルパー (*Microsoft.AspNetCore.Mvc.TagHelpers*) できるように、すべてのファイルの表示になります、*ビュー*ディレクトリまたはサブディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="2c37b-142">The code above uses the wildcard syntax ("\*") to specify that all Tag Helpers in the specified assembly (*Microsoft.AspNetCore.Mvc.TagHelpers*) will be available to every view file in the *Views* directory or sub-directory.</span></span> <span data-ttu-id="2c37b-143">最初のパラメーターの後に`@addTagHelper`を読み込むタグ ヘルパーを指定します (を使用して"\*"すべてのタグ ヘルパーの)、し"Microsoft.AspNetCore.Mvc.TagHelpers"の 2 番目のパラメーターは、タグ ヘルパーを含むアセンブリを指定します。</span><span class="sxs-lookup"><span data-stu-id="2c37b-143">The first parameter after `@addTagHelper` specifies the Tag Helpers to load (we are using "\*" for all Tag Helpers), and the second parameter "Microsoft.AspNetCore.Mvc.TagHelpers" specifies the assembly containing the Tag Helpers.</span></span> <span data-ttu-id="2c37b-144">*Microsoft.AspNetCore.Mvc.TagHelpers*組み込みの ASP.NET Core タグ ヘルパーのアセンブリです。</span><span class="sxs-lookup"><span data-stu-id="2c37b-144">*Microsoft.AspNetCore.Mvc.TagHelpers* is the assembly for the built-in ASP.NET Core Tag Helpers.</span></span>

<span data-ttu-id="2c37b-145">このプロジェクトでタグ ヘルパーのすべてを公開するために (という名前のアセンブリを作成する*AuthoringTagHelpers*)、以下を使用するとします。</span><span class="sxs-lookup"><span data-stu-id="2c37b-145">To expose all of the Tag Helpers in this project (which creates an assembly named *AuthoringTagHelpers*), you would use the following:</span></span>

[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=3)]

<span data-ttu-id="2c37b-146">プロジェクトが含まれている場合、`EmailTagHelper`既定の名前空間 (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`)、タグ ヘルパーの完全修飾名 (FQN) を指定することができます。</span><span class="sxs-lookup"><span data-stu-id="2c37b-146">If your project contains an `EmailTagHelper` with the default namespace (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), you can provide the fully qualified name (FQN) of the Tag Helper:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [3]}} -->

```html
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```

<span data-ttu-id="2c37b-147">タグ ヘルパーに追加する、FQN を使用するビューを最初に追加する、FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`)、およびアセンブリ名では、(*AuthoringTagHelpers*)。</span><span class="sxs-lookup"><span data-stu-id="2c37b-147">To add a Tag Helper to a view using an FQN, you first add the FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), and then the assembly name (*AuthoringTagHelpers*).</span></span> <span data-ttu-id="2c37b-148">ほとんどの開発者が使用を好む、"\*"ワイルドカードの構文。</span><span class="sxs-lookup"><span data-stu-id="2c37b-148">Most developers prefer to use the  "\*" wildcard syntax.</span></span> <span data-ttu-id="2c37b-149">ワイルドカードの構文では、ワイルドカード文字を挿入することができます"\*"、FQN のサフィックスとして。</span><span class="sxs-lookup"><span data-stu-id="2c37b-149">The wildcard syntax allows you to insert the wildcard character "\*" as the suffix in an FQN.</span></span> <span data-ttu-id="2c37b-150">表示されます、次のディレクティブのいずれかなど、 `EmailTagHelper`:</span><span class="sxs-lookup"><span data-stu-id="2c37b-150">For example, any of the following directives will bring in the `EmailTagHelper`:</span></span>

```csharp
@addTagHelper AuthoringTagHelpers.TagHelpers.E*, AuthoringTagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.Email*, AuthoringTagHelpers
```

<span data-ttu-id="2c37b-151">前述のように、追加、`@addTagHelper`ディレクティブを*Views/_ViewImports.cshtml*ファイル タグ ヘルパーに使用可能ですべてのファイルの表示、*ビュー*ディレクトリとサブディレクトリです。</span><span class="sxs-lookup"><span data-stu-id="2c37b-151">As mentioned previously, adding the `@addTagHelper` directive to the *Views/_ViewImports.cshtml* file makes the Tag Helper available to all view files in the *Views* directory and sub-directories.</span></span> <span data-ttu-id="2c37b-152">使用することができます、`@addTagHelper`ディレクティブでこれらのビューにタグ ヘルパーの公開にオプトインする場合に特定のビューはファイルです。</span><span class="sxs-lookup"><span data-stu-id="2c37b-152">You can use the `@addTagHelper` directive in specific view files if you want to opt-in to exposing the Tag Helper to only those views.</span></span>

<a name=remove-razor-directives-label></a>

### <a name="removetaghelper-removes-tag-helpers"></a><span data-ttu-id="2c37b-153">`@removeTagHelper`タグ ヘルパーを削除します。</span><span class="sxs-lookup"><span data-stu-id="2c37b-153">`@removeTagHelper` removes Tag Helpers</span></span>

<span data-ttu-id="2c37b-154">`@removeTagHelper` 、同じ 2 つのパラメーターとして`@addTagHelper`、既に追加されているタグ ヘルパーを削除します。</span><span class="sxs-lookup"><span data-stu-id="2c37b-154">The `@removeTagHelper` has the same two parameters as `@addTagHelper`, and it removes a Tag Helper that was previously added.</span></span> <span data-ttu-id="2c37b-155">たとえば、`@removeTagHelper`ビューから特定のビューを削除指定されたタグ ヘルパーに適用します。</span><span class="sxs-lookup"><span data-stu-id="2c37b-155">For example, `@removeTagHelper` applied to a specific view removes the specified Tag Helper from the view.</span></span> <span data-ttu-id="2c37b-156">使用して`@removeTagHelper`で、 *Views/Folder/_ViewImports.cshtml*ファイルからすべてのビューの指定されたタグ ヘルパーの削除*フォルダー*です。</span><span class="sxs-lookup"><span data-stu-id="2c37b-156">Using `@removeTagHelper` in a *Views/Folder/_ViewImports.cshtml* file removes the specified Tag Helper from all of the views in *Folder*.</span></span>

### <a name="controlling-tag-helper-scope-with-the-viewimportscshtml-file"></a><span data-ttu-id="2c37b-157">タグ ヘルパーのスコープを制御する、 *_ViewImports.cshtml*ファイル</span><span class="sxs-lookup"><span data-stu-id="2c37b-157">Controlling Tag Helper scope with the *_ViewImports.cshtml* file</span></span>

<span data-ttu-id="2c37b-158">追加することができます、 *_ViewImports.cshtml*エンジンがその両方のファイルのディレクティブを適用するビューの任意のフォルダーおよびビューと*Views/_ViewImports.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="2c37b-158">You can add a *_ViewImports.cshtml* to any view folder, and the view engine applies the directives from both that file and the *Views/_ViewImports.cshtml* file.</span></span> <span data-ttu-id="2c37b-159">場合は、空の追加*Views/Home/_ViewImports.cshtml*ファイルで、*ホーム*ビュー、あるは変更されないため、 *_ViewImports.cshtml*ファイルが加法です。</span><span class="sxs-lookup"><span data-stu-id="2c37b-159">If you added an empty *Views/Home/_ViewImports.cshtml* file for the *Home* views, there would be no change because the *_ViewImports.cshtml* file is additive.</span></span> <span data-ttu-id="2c37b-160">任意`@addTagHelper`ディレクティブを追加する、 *Views/Home/_ViewImports.cshtml*ファイル (既定値に含まれていない*Views/_ViewImports.cshtml*ファイル) が公開されますをビューには、そのタグ ヘルパーでのみ、*ホーム*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="2c37b-160">Any `@addTagHelper` directives you add to the *Views/Home/_ViewImports.cshtml* file (that are not in the default *Views/_ViewImports.cshtml* file) would expose those Tag Helpers to views only in the *Home* folder.</span></span>

<a name="opt-out"></a>

### <a name="opting-out-of-individual-elements"></a><span data-ttu-id="2c37b-161">個々 の要素を除外</span><span class="sxs-lookup"><span data-stu-id="2c37b-161">Opting out of individual elements</span></span>

<span data-ttu-id="2c37b-162">タグ ヘルパーのオプトアウト文字で、要素レベルでタグ ヘルパーを無効にすることができます ("!") です。</span><span class="sxs-lookup"><span data-stu-id="2c37b-162">You can disable a Tag Helper at the element level with the Tag Helper opt-out character ("!").</span></span> <span data-ttu-id="2c37b-163">たとえば、`Email`で検証が無効になっている、`<span>`タグ ヘルパーのオプトアウト文字。</span><span class="sxs-lookup"><span data-stu-id="2c37b-163">For example, `Email` validation is disabled in the `<span>` with the Tag Helper opt-out character:</span></span>

```csharp
<!span asp-validation-for="Email" class="text-danger"></!span>
```

<span data-ttu-id="2c37b-164">開始タグと終了タグにタグ ヘルパーのオプトアウト文字を適用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2c37b-164">You must apply the Tag Helper opt-out character to the opening and closing tag.</span></span> <span data-ttu-id="2c37b-165">(Visual Studio エディターに自動的にオプトアウトに文字を追加、終了タグ開始タグに追加する場合)。</span><span class="sxs-lookup"><span data-stu-id="2c37b-165">(The Visual Studio editor automatically adds the opt-out character to the closing tag when you add one to the opening tag).</span></span> <span data-ttu-id="2c37b-166">オプトアウト文字を追加した後、要素とタグ ヘルパー属性は印象的なフォントで表示されません。</span><span class="sxs-lookup"><span data-stu-id="2c37b-166">After you add the opt-out character, the element and Tag Helper attributes are no longer displayed in a distinctive font.</span></span>

<a name=prefix-razor-directives-label></a>

### <a name="using-taghelperprefix-to-make-tag-helper-usage-explicit"></a><span data-ttu-id="2c37b-167">使用して`@tagHelperPrefix`タグ ヘルパーの使用状況を明示するには</span><span class="sxs-lookup"><span data-stu-id="2c37b-167">Using `@tagHelperPrefix` to make Tag Helper usage explicit</span></span>

<span data-ttu-id="2c37b-168">`@tagHelperPrefix`ディレクティブでは、タグ ヘルパーのサポートを有効にして、タグ ヘルパーの使用法を明確にするタグ プレフィックス文字列を指定することができます。</span><span class="sxs-lookup"><span data-stu-id="2c37b-168">The `@tagHelperPrefix` directive allows you to specify a tag prefix string to enable Tag Helper support and to make Tag Helper usage explicit.</span></span> <span data-ttu-id="2c37b-169">たとえば、次に示すマークアップを追加、 *Views/_ViewImports.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="2c37b-169">For example, you could add the following markup to the *Views/_ViewImports.cshtml* file:</span></span>

```html
@tagHelperPrefix th:
```
<span data-ttu-id="2c37b-170">設定されているタグ ヘルパーのプレフィックス コード次の図、 `th:`、プレフィックスを使用して要素のみ`th:`タグ ヘルパー (タグ ヘルパーが有効な要素がある印象的なフォント) をサポートします。</span><span class="sxs-lookup"><span data-stu-id="2c37b-170">In the code image below, the Tag Helper prefix is set to `th:`, so only those elements using the prefix `th:` support Tag Helpers (Tag Helper-enabled elements have a distinctive font).</span></span> <span data-ttu-id="2c37b-171">`<label>`と`<input>`要素タグ ヘルパーのプレフィックス、タグ ヘルパーに対応している、ときに、`<span>`要素はありません。</span><span class="sxs-lookup"><span data-stu-id="2c37b-171">The `<label>` and `<input>` elements have the Tag Helper prefix and are Tag Helper-enabled, while the `<span>` element does not.</span></span>

![イメージ](intro/_static/thp.png)

<span data-ttu-id="2c37b-173">適用される、同じ規則が階層`@addTagHelper`にも適用`@tagHelperPrefix`です。</span><span class="sxs-lookup"><span data-stu-id="2c37b-173">The same hierarchy rules that apply to `@addTagHelper` also apply to `@tagHelperPrefix`.</span></span>

## <a name="intellisense-support-for-tag-helpers"></a><span data-ttu-id="2c37b-174">タグ ヘルパーの IntelliSense のサポート</span><span class="sxs-lookup"><span data-stu-id="2c37b-174">IntelliSense support for Tag Helpers</span></span>

<span data-ttu-id="2c37b-175">Visual Studio で新しい ASP.NET web アプリを作成するときに、NuGet パッケージ"Microsoft.AspNetCore.Razor.Tools"を追加します。</span><span class="sxs-lookup"><span data-stu-id="2c37b-175">When you create a new ASP.NET web app in Visual Studio, it adds the NuGet package "Microsoft.AspNetCore.Razor.Tools".</span></span> <span data-ttu-id="2c37b-176">これは、タグ ヘルパー ツールを追加するパッケージです。</span><span class="sxs-lookup"><span data-stu-id="2c37b-176">This is the package that adds Tag Helper tooling.</span></span>

<span data-ttu-id="2c37b-177">HTML の作成を検討`<label>`要素。</span><span class="sxs-lookup"><span data-stu-id="2c37b-177">Consider writing an HTML `<label>` element.</span></span> <span data-ttu-id="2c37b-178">入力するとすぐに`<l`Visual Studio エディターで IntelliSense が一致する要素を表示します。</span><span class="sxs-lookup"><span data-stu-id="2c37b-178">As soon as you enter `<l` in the Visual Studio editor, IntelliSense displays matching elements:</span></span>

![イメージ](intro/_static/label.png)

<span data-ttu-id="2c37b-180">アイコンは、HTML ヘルプを表示するだけでなく (、"@"のシンボルがその下にある"<>") です。</span><span class="sxs-lookup"><span data-stu-id="2c37b-180">Not only do you get HTML help, but the icon (the "@" symbol with "<>" under it).</span></span>

![イメージ](intro/_static/tagSym.png)

<span data-ttu-id="2c37b-182">タグ ヘルパーの対象となるように、要素を識別します。</span><span class="sxs-lookup"><span data-stu-id="2c37b-182">identifies the element as targeted by Tag Helpers.</span></span> <span data-ttu-id="2c37b-183">純粋な HTML 要素 (など、 `fieldset`)"<>"アイコンを表示します。</span><span class="sxs-lookup"><span data-stu-id="2c37b-183">Pure HTML elements (such as the `fieldset`) display the "<>" icon.</span></span>

<span data-ttu-id="2c37b-184">純粋な HTML`<label>`タグが、茶色のフォントが赤で属性に (既定の Visual Studio の色のテーマ) の HTML タグを表示し、属性は青色で示される値します。</span><span class="sxs-lookup"><span data-stu-id="2c37b-184">A pure HTML `<label>` tag displays the HTML tag (with the default Visual Studio color theme) in a brown font, the attributes in red, and the attribute values in blue.</span></span>

![イメージ](intro/_static/LableHtmlTag.png)

<span data-ttu-id="2c37b-186">入力した後`<label`IntelliSense には、使用可能な HTML と CSS とタグ ヘルパーの対象となる属性が一覧表示されます。</span><span class="sxs-lookup"><span data-stu-id="2c37b-186">After you enter `<label`, IntelliSense lists the available HTML/CSS attributes and the Tag Helper-targeted attributes:</span></span>

![イメージ](intro/_static/labelattr.png)

<span data-ttu-id="2c37b-188">IntelliSense 入力候補では、キーを入力します タブを選択した値を指定してステートメントを完了することができます。</span><span class="sxs-lookup"><span data-stu-id="2c37b-188">IntelliSense statement completion allows you to enter the tab key to complete the statement with the selected value:</span></span>

![イメージ](intro/_static/stmtcomplete.png)

<span data-ttu-id="2c37b-190">タグ ヘルパー属性を入力するとすぐ、タグおよび属性のフォントを変更します。</span><span class="sxs-lookup"><span data-stu-id="2c37b-190">As soon as a Tag Helper attribute is entered, the tag and attribute fonts change.</span></span> <span data-ttu-id="2c37b-191">既定の Visual Studio"Blue"または"Light"色のテーマを使用して、フォントは、太字紫です。</span><span class="sxs-lookup"><span data-stu-id="2c37b-191">Using the default Visual Studio "Blue" or "Light" color theme, the font is bold purple.</span></span> <span data-ttu-id="2c37b-192">[ダーク] テーマを使用している場合、フォントは太字青緑がします。</span><span class="sxs-lookup"><span data-stu-id="2c37b-192">If you're using the "Dark" theme the font is bold teal.</span></span> <span data-ttu-id="2c37b-193">このドキュメント内のイメージは、既定のテーマを使用して取得されました。</span><span class="sxs-lookup"><span data-stu-id="2c37b-193">The images in this document were taken using the default theme.</span></span>

![イメージ](intro/_static/labelaspfor2.png)

<span data-ttu-id="2c37b-195">Visual Studio を入力することができます*CompleteWord*ショートカット (ctrl キーを押しながら space キーは、[既定](https://docs.microsoft.com/visualstudio/ide/default-keyboard-shortcuts-in-visual-studio)二重引用符の内側 ("")、実行され、今すぐ、C# の場合は、c# のクラスにする場合と同じようにします。</span><span class="sxs-lookup"><span data-stu-id="2c37b-195">You can enter the Visual Studio *CompleteWord* shortcut (Ctrl +spacebar is the [default](https://docs.microsoft.com/visualstudio/ide/default-keyboard-shortcuts-in-visual-studio) inside the double quotes (""), and you are now in C#, just like you would be in a C# class.</span></span> <span data-ttu-id="2c37b-196">IntelliSense は、ページのモデルのすべてのメソッドとプロパティを表示します。</span><span class="sxs-lookup"><span data-stu-id="2c37b-196">IntelliSense displays all the methods and properties on the page model.</span></span> <span data-ttu-id="2c37b-197">メソッドとプロパティが使用可能なプロパティの型があるため`ModelExpression`です。</span><span class="sxs-lookup"><span data-stu-id="2c37b-197">The methods and properties are available because the property type is `ModelExpression`.</span></span> <span data-ttu-id="2c37b-198">次の図では編集して、`Register`ビュー、ため、`RegisterViewModel`は使用できます。</span><span class="sxs-lookup"><span data-stu-id="2c37b-198">In the image below, I'm editing the `Register` view, so the `RegisterViewModel` is available.</span></span>

![イメージ](intro/_static/intellemail.png)

<span data-ttu-id="2c37b-200">IntelliSense は、プロパティと、ページ上のモデルが使用できる方法を示します。</span><span class="sxs-lookup"><span data-stu-id="2c37b-200">IntelliSense lists the properties and methods available to the model on the page.</span></span> <span data-ttu-id="2c37b-201">豊富な IntelliSense 環境では、CSS クラスを選択できます。</span><span class="sxs-lookup"><span data-stu-id="2c37b-201">The rich IntelliSense environment helps you select the CSS class:</span></span>

![イメージ](intro/_static/iclass.png)

![イメージ](intro/_static/intel3.png)

## <a name="tag-helpers-compared-to-html-helpers"></a><span data-ttu-id="2c37b-204">HTML ヘルパーと比較してタグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="2c37b-204">Tag Helpers compared to HTML Helpers</span></span>

<span data-ttu-id="2c37b-205">Razor ビューでの HTML 要素にタグ ヘルパーのアタッチ中に[HTML ヘルパー](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers) Razor ビューの HTML が混在してメソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="2c37b-205">Tag Helpers attach to HTML elements in Razor views, while [HTML Helpers](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers) are invoked as methods interspersed with HTML in Razor views.</span></span> <span data-ttu-id="2c37b-206">次の Razor のマークアップは、HTML ラベルを「キャプション」の CSS クラスの作成を検討してください。</span><span class="sxs-lookup"><span data-stu-id="2c37b-206">Consider the following Razor markup, which creates an HTML label with the CSS class "caption":</span></span>

```html
@Html.Label("FirstName", "First Name:", new {@class="caption"})
```

<span data-ttu-id="2c37b-207">で (`@`) 記号は、これは、コードの先頭を Razor に指示します。</span><span class="sxs-lookup"><span data-stu-id="2c37b-207">The at (`@`) symbol tells Razor this is the start of code.</span></span> <span data-ttu-id="2c37b-208">次の 2 つのパラメーター ("FirstName"と"名:")、文字列のためは[IntelliSense](https://docs.microsoft.com/visualstudio/ide/using-intellisense)ヘルプことはできません。</span><span class="sxs-lookup"><span data-stu-id="2c37b-208">The next two parameters ("FirstName" and "First Name:") are strings, so [IntelliSense](https://docs.microsoft.com/visualstudio/ide/using-intellisense) can't help.</span></span> <span data-ttu-id="2c37b-209">最後の引数。</span><span class="sxs-lookup"><span data-stu-id="2c37b-209">The last argument:</span></span>

```html
new {@class="caption"}
```

<span data-ttu-id="2c37b-210">属性を表すために使用する匿名オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="2c37b-210">Is an anonymous object used to represent attributes.</span></span> <span data-ttu-id="2c37b-211">**クラス**を予約済みキーワードを使用する (C#)、 `@` c# の解釈を強制する記号"@class="(プロパティ名) のシンボルとして。</span><span class="sxs-lookup"><span data-stu-id="2c37b-211">Because **class** is a reserved keyword in C#, you use the `@` symbol to force C# to interpret "@class=" as a symbol (property name).</span></span> <span data-ttu-id="2c37b-212">フロント エンドのデザイナーを (他のユーザー/Html/javascript CSS およびその他のクライアント テクノロジに慣れているが、c# と Razor に精通していない)、行のほとんどが外部です。</span><span class="sxs-lookup"><span data-stu-id="2c37b-212">To a front-end designer (someone familiar with HTML/CSS/JavaScript and other client technologies but not familiar with C# and Razor), most of the line is foreign.</span></span> <span data-ttu-id="2c37b-213">ヘルプは、intellisense では、行全体を作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2c37b-213">The entire line must be authored with no help from IntelliSense.</span></span>

<span data-ttu-id="2c37b-214">使用して、 `LabelTagHelper`、として同じマークアップを記述できます。</span><span class="sxs-lookup"><span data-stu-id="2c37b-214">Using the `LabelTagHelper`, the same markup can be written as:</span></span>

![イメージ](intro/_static/label2.png)

<span data-ttu-id="2c37b-216">入力するとすぐには、タグ ヘルパーのバージョンで`<l`Visual Studio エディターで IntelliSense が一致する要素を表示します。</span><span class="sxs-lookup"><span data-stu-id="2c37b-216">With the Tag Helper version, as soon as you enter `<l` in the Visual Studio editor, IntelliSense displays matching elements:</span></span>

![イメージ](intro/_static/label.png)

<span data-ttu-id="2c37b-218">IntelliSense では、行全体を記述するのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="2c37b-218">IntelliSense helps you write the entire line.</span></span> <span data-ttu-id="2c37b-219">`LabelTagHelper`ものコンテンツの設定に既定値は、 `asp-for` 「姓」を属性値 ("FirstName")Camel 形式のプロパティを各新しい大文字が発生する領域を持つプロパティ名から成る文に変換します。</span><span class="sxs-lookup"><span data-stu-id="2c37b-219">The `LabelTagHelper` also defaults to setting the content of the `asp-for` attribute value ("FirstName") to "First Name"; It converts camel-cased properties to a sentence composed of the property name with a space where each new upper-case letter occurs.</span></span> <span data-ttu-id="2c37b-220">次のマークアップ。</span><span class="sxs-lookup"><span data-stu-id="2c37b-220">In the following markup:</span></span>

![イメージ](intro/_static/label2.png)

<span data-ttu-id="2c37b-222">生成されます。</span><span class="sxs-lookup"><span data-stu-id="2c37b-222">generates:</span></span>

```html
<label class="caption" for="FirstName">First Name</label>
```

<span data-ttu-id="2c37b-223">Camel 形式の大文字小文字の文のコンテンツには使用されませんにコンテンツを追加する場合、`<label>`です。</span><span class="sxs-lookup"><span data-stu-id="2c37b-223">The camel-cased to sentence-cased content is not used if you add content to the `<label>`.</span></span> <span data-ttu-id="2c37b-224">例:</span><span class="sxs-lookup"><span data-stu-id="2c37b-224">For example:</span></span>

![イメージ](intro/_static/1stName.png)

<span data-ttu-id="2c37b-226">生成されます。</span><span class="sxs-lookup"><span data-stu-id="2c37b-226">generates:</span></span>

```html
<label class="caption" for="FirstName">Name First</label>
```

<span data-ttu-id="2c37b-227">次のコードの画像、フォームの部分を示します、 *Views/Account/Register.cshtml* Visual Studio 2015 に含まれているレガシ ASP.NET 4.5.x MVC テンプレートから生成された Razor ビュー。</span><span class="sxs-lookup"><span data-stu-id="2c37b-227">The following code image shows the Form portion of the *Views/Account/Register.cshtml* Razor view generated from the legacy ASP.NET 4.5.x MVC template included with Visual Studio 2015.</span></span>

![イメージ](intro/_static/regCS.png)

<span data-ttu-id="2c37b-229">Visual Studio エディターでは、背景が灰色の c# コードを表示します。</span><span class="sxs-lookup"><span data-stu-id="2c37b-229">The Visual Studio editor displays C# code with a grey background.</span></span> <span data-ttu-id="2c37b-230">たとえば、 `AntiForgeryToken` HTML ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="2c37b-230">For example, the `AntiForgeryToken` HTML Helper:</span></span>

```html
@Html.AntiForgeryToken()
```

<span data-ttu-id="2c37b-231">グレーの背景が表示されます。</span><span class="sxs-lookup"><span data-stu-id="2c37b-231">is displayed with a grey background.</span></span> <span data-ttu-id="2c37b-232">レジスタ ビュー内のマークアップのほとんどは c# です。</span><span class="sxs-lookup"><span data-stu-id="2c37b-232">Most of the markup in the Register view is C#.</span></span> <span data-ttu-id="2c37b-233">タグ ヘルパーの使用と同じアプローチを比較します。</span><span class="sxs-lookup"><span data-stu-id="2c37b-233">Compare that to the equivalent approach using Tag Helpers:</span></span>

![イメージ](intro/_static/regTH.png)

<span data-ttu-id="2c37b-235">マークアップ方がクリーナー簡単に読み取り、編集、および HTML ヘルパー アプローチよりも管理します。</span><span class="sxs-lookup"><span data-stu-id="2c37b-235">The markup is much cleaner and easier to read, edit, and maintain than the HTML Helpers approach.</span></span> <span data-ttu-id="2c37b-236">C# コードは、について知っておく必要があるサーバーに最低限に短縮されます。</span><span class="sxs-lookup"><span data-stu-id="2c37b-236">The C# code is reduced to the minimum that the server needs to know about.</span></span> <span data-ttu-id="2c37b-237">Visual Studio エディターでは、印象的なフォントでタグ ヘルパーの対象とするマークアップを表示します。</span><span class="sxs-lookup"><span data-stu-id="2c37b-237">The Visual Studio editor displays markup targeted by a Tag Helper in a distinctive font.</span></span>

<span data-ttu-id="2c37b-238">検討してください、*電子メール*グループ。</span><span class="sxs-lookup"><span data-stu-id="2c37b-238">Consider the *Email* group:</span></span>

[!code-csharp[Main](intro/sample/Register.cshtml?range=12-18)]

<span data-ttu-id="2c37b-239">各「asp-」属性には"Email"の値が"Email"は文字列ではありません。</span><span class="sxs-lookup"><span data-stu-id="2c37b-239">Each of the "asp-" attributes has a value of "Email", but "Email" is not a string.</span></span> <span data-ttu-id="2c37b-240">ここでは、"Email"は、c# 式のモデル プロパティ、`RegisterViewModel`です。</span><span class="sxs-lookup"><span data-stu-id="2c37b-240">In this context, "Email" is the C# model expression property for the `RegisterViewModel`.</span></span>

<span data-ttu-id="2c37b-241">Visual Studio エディターでは、作成できます。**すべて**の Visual Studio で HTML ヘルパーの方法でコードのほとんどのヘルプはありません、登録フォームのタグ ヘルパーの方法でマークアップ。</span><span class="sxs-lookup"><span data-stu-id="2c37b-241">The Visual Studio editor helps you write **all** of the markup in the Tag Helper approach of the register form, while Visual Studio provides no help for most of the code in the HTML Helpers approach.</span></span> <span data-ttu-id="2c37b-242">[タグ ヘルパーの IntelliSense サポート](#intellisense-support-for-tag-helpers)Visual Studio エディターでタグ ヘルパーの使用方法の詳細を見ていきます。</span><span class="sxs-lookup"><span data-stu-id="2c37b-242">[IntelliSense support for Tag Helpers](#intellisense-support-for-tag-helpers) goes into detail on working with Tag Helpers in the Visual Studio editor.</span></span>

## <a name="tag-helpers-compared-to-web-server-controls"></a><span data-ttu-id="2c37b-243">Web サーバー コントロールと比較してタグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="2c37b-243">Tag Helpers compared to Web Server Controls</span></span>

* <span data-ttu-id="2c37b-244">関連付けられている要素を所有していないタグ ヘルパーそれらの要素およびコンテンツのレンダリングに単に参加します。</span><span class="sxs-lookup"><span data-stu-id="2c37b-244">Tag Helpers don't own the element they're associated with; they simply participate in the rendering of the element and content.</span></span> <span data-ttu-id="2c37b-245">ASP.NET [Web サーバー コントロール](https://msdn.microsoft.com/library/7698y1f0.aspx)の宣言し、ページに呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="2c37b-245">ASP.NET [Web Server controls](https://msdn.microsoft.com/library/7698y1f0.aspx) are declared and invoked on a page.</span></span>

* <span data-ttu-id="2c37b-246">[Web サーバー コントロール](https://msdn.microsoft.com/library/zsyt68f1.aspx)が困難に開発およびデバッグする重要なライフ サイクルがあります。</span><span class="sxs-lookup"><span data-stu-id="2c37b-246">[Web Server controls](https://msdn.microsoft.com/library/zsyt68f1.aspx) have a non-trivial lifecycle that can make developing and debugging difficult.</span></span>

* <span data-ttu-id="2c37b-247">Web サーバー コントロールを使用すると、クライアントのコントロールを使用して、クライアントのドキュメント オブジェクト モデル (DOM) 要素に機能を追加できます。</span><span class="sxs-lookup"><span data-stu-id="2c37b-247">Web Server controls allow you to add functionality to the client Document Object Model (DOM) elements by using a client control.</span></span> <span data-ttu-id="2c37b-248">タグ ヘルパーが DOM. をあるなし</span><span class="sxs-lookup"><span data-stu-id="2c37b-248">Tag Helpers have no DOM.</span></span>

* <span data-ttu-id="2c37b-249">Web サーバー コントロールには、ブラウザーの自動検出が含まれます。</span><span class="sxs-lookup"><span data-stu-id="2c37b-249">Web Server controls include automatic browser detection.</span></span> <span data-ttu-id="2c37b-250">タグ ヘルパーは、ブラウザーの知識を持っていません。</span><span class="sxs-lookup"><span data-stu-id="2c37b-250">Tag Helpers have no knowledge of the browser.</span></span>

* <span data-ttu-id="2c37b-251">複数のタグ ヘルパーが同じ要素を処理できる (を参照してください[タグ ヘルパーを回避する競合](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/authoring#avoiding-tag-helper-conflicts)) 中に、通常 Web サーバー コントロールを構成できません。</span><span class="sxs-lookup"><span data-stu-id="2c37b-251">Multiple Tag Helpers can act on the same element (see [Avoiding Tag Helper conflicts](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/authoring#avoiding-tag-helper-conflicts) ) while you typically can't compose Web Server controls.</span></span>

* <span data-ttu-id="2c37b-252">タグ ヘルパーは、タグおよびこれらを対象としている HTML 要素の内容を変更できますが、ページ上の他のものを直接変更しないで。</span><span class="sxs-lookup"><span data-stu-id="2c37b-252">Tag Helpers can modify the tag and content of HTML elements that they're scoped to, but don't directly modify anything else on a page.</span></span> <span data-ttu-id="2c37b-253">Web サーバー コントロールより限定スコープが存在し、ページの他の部分に影響を与えるアクションを実行できます。意図しない副作用を有効にします。</span><span class="sxs-lookup"><span data-stu-id="2c37b-253">Web Server controls have a less specific scope and can perform actions that affect other parts of your page; enabling unintended side effects.</span></span>

* <span data-ttu-id="2c37b-254">Web サーバー コントロールでは、型コンバーターを使用して、文字列をオブジェクトに変換します。</span><span class="sxs-lookup"><span data-stu-id="2c37b-254">Web Server controls use type converters to convert strings into objects.</span></span> <span data-ttu-id="2c37b-255">タグ ヘルパーで作業するネイティブ C# の場合、入力は、変換する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="2c37b-255">With Tag Helpers, you work natively in C#, so you don't need to do type conversion.</span></span>

* <span data-ttu-id="2c37b-256">Web サーバー コントロールを使用して[System.ComponentModel](https://docs.microsoft.com/dotnet/api/system.componentmodel)コンポーネントやコントロールの実行時およびデザイン時の動作を実装します。</span><span class="sxs-lookup"><span data-stu-id="2c37b-256">Web Server controls use [System.ComponentModel](https://docs.microsoft.com/dotnet/api/system.componentmodel) to implement the run-time and design-time behavior of components and controls.</span></span> <span data-ttu-id="2c37b-257">`System.ComponentModel`基本クラスとインターフェイス、属性と型コンバーターの実装、ソース、データへのバインド、およびコンポーネントのライセンスが含まれます。</span><span class="sxs-lookup"><span data-stu-id="2c37b-257">`System.ComponentModel` includes the base classes and interfaces for implementing attributes and type converters, binding to data sources, and licensing components.</span></span> <span data-ttu-id="2c37b-258">タグ ヘルパーは、通常から派生するコントラストを`TagHelper`、および`TagHelper`基底クラスのみに 2 つのメソッドを公開する`Process`と`ProcessAsync`です。</span><span class="sxs-lookup"><span data-stu-id="2c37b-258">Contrast that to Tag Helpers, which typically derive from `TagHelper`, and the `TagHelper` base class exposes only two methods, `Process` and `ProcessAsync`.</span></span>

## <a name="customizing-the-tag-helper-element-font"></a><span data-ttu-id="2c37b-259">タグ ヘルパー要素のフォントのカスタマイズ</span><span class="sxs-lookup"><span data-stu-id="2c37b-259">Customizing the Tag Helper element font</span></span>

<span data-ttu-id="2c37b-260">色付けとフォントをカスタマイズすることができます**ツール** > **オプション** > **環境** > **フォントおよび色**:</span><span class="sxs-lookup"><span data-stu-id="2c37b-260">You can customize the font and colorization from **Tools** > **Options** > **Environment** > **Fonts and Colors**:</span></span>

![イメージ](intro/_static/fontoptions2.png)

## <a name="additional-resources"></a><span data-ttu-id="2c37b-262">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="2c37b-262">Additional Resources</span></span>

* [<span data-ttu-id="2c37b-263">タグ ヘルパーの作成</span><span class="sxs-lookup"><span data-stu-id="2c37b-263">Authoring Tag Helpers</span></span>](authoring.md)
* [<span data-ttu-id="2c37b-264">フォームの使用</span><span class="sxs-lookup"><span data-stu-id="2c37b-264">Working with Forms </span></span>](../working-with-forms.md)
* <span data-ttu-id="2c37b-265">[GitHub の TagHelperSamples](https://github.com/dpaquette/TagHelperSamples)を操作するためのタグ ヘルパーのサンプルが含まれます[ブートス トラップ](http://getbootstrap.com/)です。</span><span class="sxs-lookup"><span data-stu-id="2c37b-265">[TagHelperSamples on GitHub](https://github.com/dpaquette/TagHelperSamples) contains Tag Helper samples for working with [Bootstrap](http://getbootstrap.com/).</span></span>

<!--
* [Working with Forms ](xref:mvc/views/working-with-forms)
-->
