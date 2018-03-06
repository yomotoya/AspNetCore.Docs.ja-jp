---
title: "ASP.NET Core での Razor ページの概要"
author: Rick-Anderson
description: "ASP.NET Core の Razor ページを使用して、ページのコーディングに重点を置いたシナリオをより簡略化して、MVC を使用する場合よりも生産性を高める方法について説明します。"
manager: wpickett
ms.author: riande
ms.date: 09/12/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: mvc/razor-pages/index
ms.openlocfilehash: f24de7ab12a3bbd7915ce6c3c93a107eb47fe864
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/02/2018
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="35dc6-103">ASP.NET Core での Razor ページの概要</span><span class="sxs-lookup"><span data-stu-id="35dc6-103">Introduction to Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="35dc6-104">[Rick Anderson](https://twitter.com/RickAndMSFT) および [Ryan Nowak](https://github.com/rynowak) 著</span><span class="sxs-lookup"><span data-stu-id="35dc6-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="35dc6-105">Razor ページは、ページ コーディングに重点を置いたシナリオをより簡略化し、生産性を高める ASP.NET Core MVC の新機能です。</span><span class="sxs-lookup"><span data-stu-id="35dc6-105">Razor Pages is a new feature of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="35dc6-106">モデル ビュー コントローラーのアプローチを使用するチュートリアルをお探しの場合は、「[Getting started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc)」 (ASP.NET Core MVC の概要) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="35dc6-106">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Getting started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="35dc6-107">このドキュメントでは、Razor ページの概要について説明します。</span><span class="sxs-lookup"><span data-stu-id="35dc6-107">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="35dc6-108">手順を追って説明するチュートリアルではありません。</span><span class="sxs-lookup"><span data-stu-id="35dc6-108">It's not a step by step tutorial.</span></span> <span data-ttu-id="35dc6-109">セクションの一部を理解できない場合は、[Razor ページの概要](xref:tutorials/razor-pages/razor-pages-start)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="35dc6-109">If you find some of the sections difficult to follow, see [Getting started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span>

<a name="prerequisites"></a>

## <a name="aspnet-core-20-prerequisites"></a><span data-ttu-id="35dc6-110">ASP.NET Core 2.0 の前提条件</span><span class="sxs-lookup"><span data-stu-id="35dc6-110">ASP.NET Core 2.0 prerequisites</span></span>

<span data-ttu-id="35dc6-111">[.NET Core](https://www.microsoft.com/net/core) 2.0.0 以降をインストールします。</span><span class="sxs-lookup"><span data-stu-id="35dc6-111">Install [.NET Core](https://www.microsoft.com/net/core) 2.0.0 or later.</span></span>

<span data-ttu-id="35dc6-112">Visual Studio を使用している場合は、以下のワークロードで [Visual Studio](https://www.visualstudio.com/vs/) 2017 Version 15.3 以降をインストールします。</span><span class="sxs-lookup"><span data-stu-id="35dc6-112">If you're using Visual Studio, install [Visual Studio](https://www.visualstudio.com/vs/) 2017 version 15.3 or later with the following workloads:</span></span>

* <span data-ttu-id="35dc6-113">**ASP.NET と Web 開発**</span><span class="sxs-lookup"><span data-stu-id="35dc6-113">**ASP.NET and web development**</span></span>
* <span data-ttu-id="35dc6-114">**.NET Core クロスプラットフォームの開発**</span><span class="sxs-lookup"><span data-stu-id="35dc6-114">**.NET Core cross-platform development**</span></span>

<a name="rpvs17"></a>

## <a name="creating-a-razor-pages-project"></a><span data-ttu-id="35dc6-115">Razor ページ プロジェクトの作成</span><span class="sxs-lookup"><span data-stu-id="35dc6-115">Creating a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="35dc6-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="35dc6-116">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="35dc6-117">Visual Studio を使用して Razor ページ プロジェクトを作成する詳細な手順については、「[Getting started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start)」 (Razor ページの概要) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="35dc6-117">See [Getting started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project using Visual Studio.</span></span>

#   <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="35dc6-118">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="35dc6-118">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="35dc6-119">コマンド ラインから `dotnet new razor` を実行します。</span><span class="sxs-lookup"><span data-stu-id="35dc6-119">Run `dotnet new razor` from the command line.</span></span>

<span data-ttu-id="35dc6-120">Visual Studio for Mac から生成された *.csproj* ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="35dc6-120">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="35dc6-121">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="35dc6-121">Visual Studio Code</span></span>](#tab/visual-studio-code) 

<span data-ttu-id="35dc6-122">コマンド ラインから `dotnet new razor` を実行します。</span><span class="sxs-lookup"><span data-stu-id="35dc6-122">Run `dotnet new razor` from the command line.</span></span>

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="35dc6-123">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="35dc6-123">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="35dc6-124">コマンド ラインから `dotnet new razor` を実行します。</span><span class="sxs-lookup"><span data-stu-id="35dc6-124">Run `dotnet new razor` from the command line.</span></span>

---

## <a name="razor-pages"></a><span data-ttu-id="35dc6-125">Razor ページ</span><span class="sxs-lookup"><span data-stu-id="35dc6-125">Razor Pages</span></span>

<span data-ttu-id="35dc6-126">Razor ページは *Startup.cs* で有効になっています。</span><span class="sxs-lookup"><span data-stu-id="35dc6-126">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

<span data-ttu-id="35dc6-127">基本ページを検討します。<a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="35dc6-127">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

<span data-ttu-id="35dc6-128">上記のコードは Razor ビュー ファイルに非常によく似ています。</span><span class="sxs-lookup"><span data-stu-id="35dc6-128">The preceding code looks a lot like a Razor view file.</span></span> <span data-ttu-id="35dc6-129">違いは、`@page` ディレクティブにあります。</span><span class="sxs-lookup"><span data-stu-id="35dc6-129">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="35dc6-130">`@page` はファイルを MVC アクションにします。つまり、コントローラーを経由せずに要求を直接処理します。</span><span class="sxs-lookup"><span data-stu-id="35dc6-130">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="35dc6-131">`@page` はページで最初の Razor ディレクティブである必要があります。</span><span class="sxs-lookup"><span data-stu-id="35dc6-131">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="35dc6-132">`@page` はその他の Razor コンストラクトの動作に影響します。</span><span class="sxs-lookup"><span data-stu-id="35dc6-132">`@page` affects the behavior of other Razor constructs.</span></span>

<span data-ttu-id="35dc6-133">`PageModel` クラスを使用している類似したページが、次の 2 つのファイルにあります。</span><span class="sxs-lookup"><span data-stu-id="35dc6-133">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="35dc6-134">*Pages/Index2.cshtml* ファイル:</span><span class="sxs-lookup"><span data-stu-id="35dc6-134">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="35dc6-135">*Pages/Index2.cshtml.cs* ページ モデル:</span><span class="sxs-lookup"><span data-stu-id="35dc6-135">The *Pages/Index2.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="35dc6-136">規則により、`PageModel` クラス ファイルは、Razor ページ ファイルと同じ名前に *.cs* が付加された名前になります。</span><span class="sxs-lookup"><span data-stu-id="35dc6-136">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="35dc6-137">たとえば、上の Razor ページは *Pages/Index2.cshtml* になります。</span><span class="sxs-lookup"><span data-stu-id="35dc6-137">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="35dc6-138">`PageModel` クラスを含むファイル名は、*Pages/Index2.cshtml.cs* になります。</span><span class="sxs-lookup"><span data-stu-id="35dc6-138">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="35dc6-139">URL パスのページへの関連付けは、ファイル システム内のページの場所によって決定されます。</span><span class="sxs-lookup"><span data-stu-id="35dc6-139">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="35dc6-140">次の表に、Razor ページ パスと一致 URL を示します。</span><span class="sxs-lookup"><span data-stu-id="35dc6-140">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="35dc6-141">ファイル名とパス</span><span class="sxs-lookup"><span data-stu-id="35dc6-141">File name and path</span></span>               | <span data-ttu-id="35dc6-142">一致 URL</span><span class="sxs-lookup"><span data-stu-id="35dc6-142">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="35dc6-143">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="35dc6-143">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="35dc6-144">`/` または `/Index`</span><span class="sxs-lookup"><span data-stu-id="35dc6-144">`/` or `/Index`</span></span> |
| <span data-ttu-id="35dc6-145">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="35dc6-145">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="35dc6-146">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="35dc6-146">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="35dc6-147">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="35dc6-147">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="35dc6-148">`/Store` または `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="35dc6-148">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="35dc6-149">メモ:</span><span class="sxs-lookup"><span data-stu-id="35dc6-149">Notes:</span></span>

* <span data-ttu-id="35dc6-150">既定では、ランタイムが *Pages* フォルダー内で Razor ページ ファイルを検索します。</span><span class="sxs-lookup"><span data-stu-id="35dc6-150">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="35dc6-151">`Index` は、URL にページが含まれない場合の既定のページになります。</span><span class="sxs-lookup"><span data-stu-id="35dc6-151">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="writing-a-basic-form"></a><span data-ttu-id="35dc6-152">基本フォームの作成</span><span class="sxs-lookup"><span data-stu-id="35dc6-152">Writing a basic form</span></span>

<span data-ttu-id="35dc6-153">Razor ページ機能は、Web ブラウザーで使用される一般的なパターンを簡単にするために設計されています。</span><span class="sxs-lookup"><span data-stu-id="35dc6-153">Razor Pages features are designed to make common patterns used with web browsers easy.</span></span> <span data-ttu-id="35dc6-154">[モデル バインド](xref:mvc/models/model-binding)、[タグ ヘルパー](xref:mvc/views/tag-helpers/intro)、および HTML ヘルパーはすべて、Razor ページ クラスで定義されたプロパティで*機能します*。</span><span class="sxs-lookup"><span data-stu-id="35dc6-154">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="35dc6-155">`Contact` モデルの基本的な "お問い合わせ" フォームを実装するページを考察します。</span><span class="sxs-lookup"><span data-stu-id="35dc6-155">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="35dc6-156">このドキュメントのサンプルでは、[Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) ファイルで `DbContext` が初期化されます。</span><span class="sxs-lookup"><span data-stu-id="35dc6-156">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

<span data-ttu-id="35dc6-157">データ モデル:</span><span class="sxs-lookup"><span data-stu-id="35dc6-157">The data model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/Customer.cs)]

<span data-ttu-id="35dc6-158">db コンテキスト:</span><span class="sxs-lookup"><span data-stu-id="35dc6-158">The db context:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

<span data-ttu-id="35dc6-159">*Pages/Create.cshtml* ビュー ファイル:</span><span class="sxs-lookup"><span data-stu-id="35dc6-159">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

<span data-ttu-id="35dc6-160">*Pages/Create.cshtml.cs* ページ モデル:</span><span class="sxs-lookup"><span data-stu-id="35dc6-160">The *Pages/Create.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="35dc6-161">規則により、`PageModel` クラスは `<PageName>Model` と呼ばれ、ページと同じ名前空間にあります。</span><span class="sxs-lookup"><span data-stu-id="35dc6-161">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="35dc6-162">`PageModel` クラスでは、ページの表示からロジックを分離できます。</span><span class="sxs-lookup"><span data-stu-id="35dc6-162">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="35dc6-163">これは、ページに送信される要求のページ ハンドラーと、ページのレンダリングに使用されるデータを定義します。</span><span class="sxs-lookup"><span data-stu-id="35dc6-163">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="35dc6-164">この分離により、ユーザーは[依存関係の挿入](xref:fundamentals/dependency-injection)を通じてページの依存関係を管理し、ページの[単体テスト](xref:testing/razor-pages-testing)を実行できます。</span><span class="sxs-lookup"><span data-stu-id="35dc6-164">This separation allows you to manage page dependencies through [dependency injection](xref:fundamentals/dependency-injection) and to [unit test](xref:testing/razor-pages-testing) the pages.</span></span>

<span data-ttu-id="35dc6-165">このページには、(ユーザーがフォームを投稿したときに) `POST` 要求で実行される `OnPostAsync` *ハンドラー メソッド*があります。</span><span class="sxs-lookup"><span data-stu-id="35dc6-165">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="35dc6-166">任意の HTTP 動詞のハンドラー メソッドを追加できます。</span><span class="sxs-lookup"><span data-stu-id="35dc6-166">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="35dc6-167">最も一般的なハンドラーは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="35dc6-167">The most common handlers are:</span></span>

* <span data-ttu-id="35dc6-168">ページに必要な状態を初期化するための `OnGet`。</span><span class="sxs-lookup"><span data-stu-id="35dc6-168">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="35dc6-169">[OnGet](#OnGet) サンプル。</span><span class="sxs-lookup"><span data-stu-id="35dc6-169">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="35dc6-170">フォームの送信を処理するための `OnPost`。</span><span class="sxs-lookup"><span data-stu-id="35dc6-170">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="35dc6-171">`Async` 名前付けサフィックスは省略可能ですが、非同期関数の規則でよく使用されます。</span><span class="sxs-lookup"><span data-stu-id="35dc6-171">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="35dc6-172">上記の例の `OnPostAsync` コードは、コントローラーで通常に記述するものに似ています。</span><span class="sxs-lookup"><span data-stu-id="35dc6-172">The `OnPostAsync` code in the preceding example looks similar to what you would normally write in a controller.</span></span> <span data-ttu-id="35dc6-173">上記のコードは、Razor ページでは一般的です。</span><span class="sxs-lookup"><span data-stu-id="35dc6-173">The preceding code is typical for Razor Pages.</span></span> <span data-ttu-id="35dc6-174">[モデル バインド](xref:mvc/models/model-binding)、[検証](xref:mvc/models/validation)、およびアクションの結果などのほとんどの MVC プリミティブは共有されます。</span><span class="sxs-lookup"><span data-stu-id="35dc6-174">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results are shared.</span></span>  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

<span data-ttu-id="35dc6-175">上記の `OnPostAsync` メソッド:</span><span class="sxs-lookup"><span data-stu-id="35dc6-175">The previous `OnPostAsync` method:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="35dc6-176">`OnPostAsync` の基本的な流れは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="35dc6-176">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="35dc6-177">検証エラーを確認します。</span><span class="sxs-lookup"><span data-stu-id="35dc6-177">Check for validation errors.</span></span>

*  <span data-ttu-id="35dc6-178">エラーがない場合は、データを保存し、リダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="35dc6-178">If there are no errors, save the data and redirect.</span></span>
*  <span data-ttu-id="35dc6-179">エラーがある場合は、検証メッセージとともにページをもう一度表示します。</span><span class="sxs-lookup"><span data-stu-id="35dc6-179">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="35dc6-180">クライアント側の検証は、従来の ASP.NET Core MVC アプリケーションと同じです。</span><span class="sxs-lookup"><span data-stu-id="35dc6-180">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="35dc6-181">多くの場合、検証エラーはクライアントで検出され、サーバーには送信されません。</span><span class="sxs-lookup"><span data-stu-id="35dc6-181">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="35dc6-182">データが正常に入力されると、`OnPostAsync` ハンドラー メソッドが `RedirectToPage` ヘルパー メソッドを呼び出して `RedirectToPageResult` のインスタンスを返します。</span><span class="sxs-lookup"><span data-stu-id="35dc6-182">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="35dc6-183">`RedirectToPage` は、`RedirectToAction` や `RedirectToRoute` と同じような新しいアクション結果ですが、ページ用にカスタマイズされています。</span><span class="sxs-lookup"><span data-stu-id="35dc6-183">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="35dc6-184">上記のサンプルでは、ルート インデックス ページ (`/Index`) にリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="35dc6-184">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="35dc6-185">`RedirectToPage` については、「[ページの URL の生成](#url_gen)」セクションで詳しく説明されています。</span><span class="sxs-lookup"><span data-stu-id="35dc6-185">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="35dc6-186">送信されたフォームに検証エラー (サーバーに渡される) があると、`OnPostAsync` ハンドラー メソッドが `Page` ヘルパー メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="35dc6-186">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="35dc6-187">`Page` は `PageResult` のインスタンスを返します。</span><span class="sxs-lookup"><span data-stu-id="35dc6-187">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="35dc6-188">`Page` を返すのは、コントローラーのアクションが `View` を返す方法に似ています。</span><span class="sxs-lookup"><span data-stu-id="35dc6-188">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="35dc6-189">`PageResult` はハンドラー メソッドの既定の<!-- Review  -->戻り値の型です。</span><span class="sxs-lookup"><span data-stu-id="35dc6-189">`PageResult` is the default <!-- Review  --> return type for a handler method.</span></span> <span data-ttu-id="35dc6-190">`void` を返すハンドラー メソッドがページをレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="35dc6-190">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="35dc6-191">`Customer` プロパティは `[BindProperty]` 属性を使用してモデル バインドにオプトインします。</span><span class="sxs-lookup"><span data-stu-id="35dc6-191">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

<span data-ttu-id="35dc6-192">既定では、Razor ページはプロパティを非 GET 動詞とのみバインドします。</span><span class="sxs-lookup"><span data-stu-id="35dc6-192">Razor Pages, by default, bind properties only with non-GET verbs.</span></span> <span data-ttu-id="35dc6-193">プロパティをバインドすることで、記述すべきコードの量を削減できます。</span><span class="sxs-lookup"><span data-stu-id="35dc6-193">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="35dc6-194">同じプロパティを使用してバインドすることでコードを減らし、フィールド (`<input asp-for="Customer.Name" />`) からレンダリングして入力を受け入れます。</span><span class="sxs-lookup"><span data-stu-id="35dc6-194">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name" />`) and accept the input.</span></span>

<span data-ttu-id="35dc6-195">ホーム ページ (*Index.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="35dc6-195">The home page (*Index.cshtml*):</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

<span data-ttu-id="35dc6-196">分離コード *Index.cshtml.cs* ファイル:</span><span class="sxs-lookup"><span data-stu-id="35dc6-196">The code behind *Index.cshtml.cs* file:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

<span data-ttu-id="35dc6-197">*Index.cshtml* ファイルには、各連絡先の編集リンクを作成するために次のマークアップが含まれています。</span><span class="sxs-lookup"><span data-stu-id="35dc6-197">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

<span data-ttu-id="35dc6-198">[アンカー タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)は `asp-route-{value}` 属性を使用して編集ページへのリンクを生成しました。</span><span class="sxs-lookup"><span data-stu-id="35dc6-198">The [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="35dc6-199">リンクには、連絡先 ID とともにルート データが含まれています。</span><span class="sxs-lookup"><span data-stu-id="35dc6-199">The link contains route data with the contact ID.</span></span> <span data-ttu-id="35dc6-200">たとえば、`http://localhost:5000/Edit/1` のようにします。</span><span class="sxs-lookup"><span data-stu-id="35dc6-200">For example, `http://localhost:5000/Edit/1`.</span></span>

<span data-ttu-id="35dc6-201">*Pages/Edit.cshtml* ファイル:</span><span class="sxs-lookup"><span data-stu-id="35dc6-201">The *Pages/Edit.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

<span data-ttu-id="35dc6-202">最初の行には `@page "{id:int}"` ディレクティブが含まれています。</span><span class="sxs-lookup"><span data-stu-id="35dc6-202">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="35dc6-203">ルーティングの制約 `"{id:int}"` は、`int` ルート データを含むページへの要求を受け入れるようにページに指示します。</span><span class="sxs-lookup"><span data-stu-id="35dc6-203">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="35dc6-204">ページへの要求に `int` に変換できるルート データが含まれていない場合は、ランタイムで HTTP 404 (見つかりません) エラーが返されます。</span><span class="sxs-lookup"><span data-stu-id="35dc6-204">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span>

<span data-ttu-id="35dc6-205">*Pages/Edit.cshtml.cs* ファイル:</span><span class="sxs-lookup"><span data-stu-id="35dc6-205">The *Pages/Edit.cshtml.cs* file:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

<span data-ttu-id="35dc6-206">*Index.cshtml* ファイルには、各顧客の連絡先の削除ボタンを作成するマークアップも含まれています。</span><span class="sxs-lookup"><span data-stu-id="35dc6-206">The *Index.cshtml* file also contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

<span data-ttu-id="35dc6-207">HTML で削除ボタンがレンダリングされる場合、その `formaction` には次のパラメーターが含まれています。</span><span class="sxs-lookup"><span data-stu-id="35dc6-207">When the delete button is rendered in HTML, its `formaction` includes parameters for:</span></span>

* <span data-ttu-id="35dc6-208">`asp-route-id` 属性によって指定された顧客の連絡先 ID。</span><span class="sxs-lookup"><span data-stu-id="35dc6-208">The customer contact ID specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="35dc6-209">`asp-page-handler` 属性によって指定された `handler`。</span><span class="sxs-lookup"><span data-stu-id="35dc6-209">The `handler` specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="35dc6-210">顧客の連絡先 ID `1` でレンダリングされた削除ボタンの例を示します。</span><span class="sxs-lookup"><span data-stu-id="35dc6-210">Here is an example of a rendered delete button with a customer contact ID of `1`:</span></span>

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="35dc6-211">ボタンが選択されると、フォームの `POST` 要求がサーバーに送信されます。</span><span class="sxs-lookup"><span data-stu-id="35dc6-211">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="35dc6-212">慣例により、ハンドラー メソッドの名前はスキーム `OnPost[handler]Async` に従った `handler` パラメーターの値に基づいて選択されます。</span><span class="sxs-lookup"><span data-stu-id="35dc6-212">By convention, the name of the handler method is selected based the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="35dc6-213">この例では `handler` が `delete` であるため、`OnPostDeleteAsync` ハンドラー メソッドを使用して `POST` 要求が処理されます。</span><span class="sxs-lookup"><span data-stu-id="35dc6-213">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="35dc6-214">`asp-page-handler` が `remove` などの別の値に設定されている場合、名前が `OnPostRemoveAsync` のページ ハンドラー メソッドが選択されます。</span><span class="sxs-lookup"><span data-stu-id="35dc6-214">If the `asp-page-handler` is set to a different value, such as `remove`, a page handler method with the name `OnPostRemoveAsync` is selected.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

<span data-ttu-id="35dc6-215">`OnPostDeleteAsync` メソッド:</span><span class="sxs-lookup"><span data-stu-id="35dc6-215">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="35dc6-216">クエリ文字列から `id` を受け入れます。</span><span class="sxs-lookup"><span data-stu-id="35dc6-216">Accepts the `id` from the query string.</span></span>
* <span data-ttu-id="35dc6-217">`FindAsync` を使用してデータベースから顧客の連絡先を照会します。</span><span class="sxs-lookup"><span data-stu-id="35dc6-217">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="35dc6-218">顧客の連絡先が見つかった場合、その連絡先は顧客の連絡先の一覧から削除されています。</span><span class="sxs-lookup"><span data-stu-id="35dc6-218">If the customer contact is found, they're removed from the list of customer contacts.</span></span> <span data-ttu-id="35dc6-219">データベースが更新されます。</span><span class="sxs-lookup"><span data-stu-id="35dc6-219">The database is updated.</span></span>
* <span data-ttu-id="35dc6-220">ルート インデックス ページ (`/Index`) にリダイレクトされるように、`RedirectToPage` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="35dc6-220">Calls `RedirectToPage` to redirect to the root Index page (`/Index`).</span></span>

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="35dc6-221">XSRF/CSRF と Razor ページ</span><span class="sxs-lookup"><span data-stu-id="35dc6-221">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="35dc6-222">[偽造防止検証](xref:security/anti-request-forgery)のためにコードを記述する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="35dc6-222">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="35dc6-223">偽造防止トークンの生成と検証は、自動的に Razor ページに含まれます。</span><span class="sxs-lookup"><span data-stu-id="35dc6-223">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>
## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="35dc6-224">Razor ページでのレイアウト、パーシャル、テンプレート、およびタグ ヘルパーの使用</span><span class="sxs-lookup"><span data-stu-id="35dc6-224">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="35dc6-225">ページは、Razor ビュー エンジンのすべての機能で機能します。</span><span class="sxs-lookup"><span data-stu-id="35dc6-225">Pages work with all the features of the Razor view engine.</span></span> <span data-ttu-id="35dc6-226">レイアウト、パーシャル、テンプレート、タグ ヘルパー、*_ViewStart.cshtml*、*_ViewImports.cshtml* は、従来の Razor ビューと同じように動作します。</span><span class="sxs-lookup"><span data-stu-id="35dc6-226">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="35dc6-227">これらの機能の一部を利用してこのページをまとめてみましょう。</span><span class="sxs-lookup"><span data-stu-id="35dc6-227">Let's declutter this page by taking advantage of some of those features.</span></span>

<span data-ttu-id="35dc6-228">[レイアウト ページ](xref:mvc/views/layout)を *Pages/_Layout.cshtml* に追加します。</span><span class="sxs-lookup"><span data-stu-id="35dc6-228">Add a [layout page](xref:mvc/views/layout) to *Pages/_Layout.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

<span data-ttu-id="35dc6-229">[レイアウト](xref:mvc/views/layout)は次のことを行います。</span><span class="sxs-lookup"><span data-stu-id="35dc6-229">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="35dc6-230">(ページでレイアウトを止めない限り) 各ページのレイアウトを制御します。</span><span class="sxs-lookup"><span data-stu-id="35dc6-230">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="35dc6-231">JavaScript やスタイルシートなどの HTML 構造をインポートします。</span><span class="sxs-lookup"><span data-stu-id="35dc6-231">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="35dc6-232">詳細については、[レイアウトのページ](xref:mvc/views/layout)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="35dc6-232">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="35dc6-233">[Layout](xref:mvc/views/layout#specifying-a-layout) プロパティは *Pages/_ViewStart.cshtml* で設定されています。</span><span class="sxs-lookup"><span data-stu-id="35dc6-233">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

<span data-ttu-id="35dc6-234">**注:** レイアウトは、*Pages* フォルダー内にあります。</span><span class="sxs-lookup"><span data-stu-id="35dc6-234">**Note:** The layout is in the *Pages* folder.</span></span> <span data-ttu-id="35dc6-235">ページは現在のページと同じフォルダーから開始して、階層的に他のビュー (レイアウト、テンプレート、パーシャル) を検索します。</span><span class="sxs-lookup"><span data-stu-id="35dc6-235">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="35dc6-236">*Pages* フォルダー内のレイアウトは、*Pages* フォルダー配下の任意の Razor ページから使用できます。</span><span class="sxs-lookup"><span data-stu-id="35dc6-236">A layout in the *Pages* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="35dc6-237">レイアウト ファイルを *Views/Shared* フォルダー内に配置**しない**ことをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="35dc6-237">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="35dc6-238">*Views/Shared* は MVC ビュー パターンです。</span><span class="sxs-lookup"><span data-stu-id="35dc6-238">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="35dc6-239">Razor ページは、パス規則ではなく、フォルダー階層に依存することを意図しています。</span><span class="sxs-lookup"><span data-stu-id="35dc6-239">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="35dc6-240">Razor ページからのビュー検索には、*Pages* フォルダーが含まれます。</span><span class="sxs-lookup"><span data-stu-id="35dc6-240">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="35dc6-241">MVC コントローラーで使用しているレイアウト、テンプレート、およびパーシャルと、従来の Razor ビューは*機能します*。</span><span class="sxs-lookup"><span data-stu-id="35dc6-241">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="35dc6-242">*Pages/_ViewImports.cshtml* ファイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="35dc6-242">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="35dc6-243">`@namespace` はこのチュートリアルで後ほど説明します。</span><span class="sxs-lookup"><span data-stu-id="35dc6-243">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="35dc6-244">`@addTagHelper` ディレクティブにより、[組み込みタグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/Index)が *Pages* フォルダー内のすべてのページにもたらされます。</span><span class="sxs-lookup"><span data-stu-id="35dc6-244">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="35dc6-245">ページで `@namespace` ディレクティブが明示的に使用されている場合:</span><span class="sxs-lookup"><span data-stu-id="35dc6-245">When the `@namespace` directive is used explicitly on a page:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="35dc6-246">ディレクティブは、ページの名前空間を設定します。</span><span class="sxs-lookup"><span data-stu-id="35dc6-246">The directive sets the namespace for the page.</span></span> <span data-ttu-id="35dc6-247">`@model` ディレクティブには、名前空間を含める必要はありません。</span><span class="sxs-lookup"><span data-stu-id="35dc6-247">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="35dc6-248">`@namespace` ディレクティブが *_ViewImports.cshtml* に含まれていると、指定した名前空間が `@namespace` ディレクティブをインポートするページで生成された名前空間のプレフィックスを提供します。</span><span class="sxs-lookup"><span data-stu-id="35dc6-248">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="35dc6-249">生成された名前空間の残りの部分 (サフィックスの部分) は、*_ViewImports.cshtml* を含むフォルダーとページを含むフォルダー間のドットで区切られた相対パスです。</span><span class="sxs-lookup"><span data-stu-id="35dc6-249">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="35dc6-250">たとえば、分離コード ファイル *Pages/Customers/Edit.cshtml.cs* は名前空間を明示的に設定します。</span><span class="sxs-lookup"><span data-stu-id="35dc6-250">For example, the code behind file *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="35dc6-251">*Pages/_ViewImports.cshtml* ファイルは次の名前空間を設定します。</span><span class="sxs-lookup"><span data-stu-id="35dc6-251">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="35dc6-252">*Pages/Customers/Edit.cshtml* Razor ページの生成された名前空間は、分離コード ファイルと同じです。</span><span class="sxs-lookup"><span data-stu-id="35dc6-252">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the code behind file.</span></span> <span data-ttu-id="35dc6-253">`@namespace` ディレクティブは、分離コード ファイルの `@using` ディレクティブを追加しなくても、プロジェクトに追加された C# クラスと、ページ生成したコードが*機能する*ように設計されました。</span><span class="sxs-lookup"><span data-stu-id="35dc6-253">The `@namespace` directive was designed so the C# classes added to a project and pages-generated code *just work* without having to add an `@using` directive for the code behind file.</span></span>

<span data-ttu-id="35dc6-254">**注:** `@namespace`は従来の Razor ビューでも機能します。</span><span class="sxs-lookup"><span data-stu-id="35dc6-254">**Note:** `@namespace` also works with conventional Razor views.</span></span>

<span data-ttu-id="35dc6-255">元の *Pages/Create.cshtml* ビュー ファイル:</span><span class="sxs-lookup"><span data-stu-id="35dc6-255">The original *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

<span data-ttu-id="35dc6-256">更新された *Pages/Create.cshtml* ビュー ファイル:</span><span class="sxs-lookup"><span data-stu-id="35dc6-256">The updated *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

<span data-ttu-id="35dc6-257">[Razor ページのスタート プロジェクト](#rpvs17)には、クライアント側の検証をフックする *Pages/_ValidationScriptsPartial.cshtml* が含まれています。</span><span class="sxs-lookup"><span data-stu-id="35dc6-257">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="35dc6-258">ページの URL の生成</span><span class="sxs-lookup"><span data-stu-id="35dc6-258">URL generation for Pages</span></span>

<span data-ttu-id="35dc6-259">上に示した `Create` ページでは、`RedirectToPage` を使用します。</span><span class="sxs-lookup"><span data-stu-id="35dc6-259">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

<span data-ttu-id="35dc6-260">アプリには次のファイル/フォルダー構造があります。</span><span class="sxs-lookup"><span data-stu-id="35dc6-260">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="35dc6-261">*/Pages*</span><span class="sxs-lookup"><span data-stu-id="35dc6-261">*/Pages*</span></span>

  * <span data-ttu-id="35dc6-262">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="35dc6-262">*Index.cshtml*</span></span>
  * <span data-ttu-id="35dc6-263">*/Customer*</span><span class="sxs-lookup"><span data-stu-id="35dc6-263">*/Customer*</span></span>

    * <span data-ttu-id="35dc6-264">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="35dc6-264">*Create.cshtml*</span></span>
    * <span data-ttu-id="35dc6-265">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="35dc6-265">*Edit.cshtml*</span></span>
    * <span data-ttu-id="35dc6-266">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="35dc6-266">*Index.cshtml*</span></span>

<span data-ttu-id="35dc6-267">成功すると、*Pages/Customers/Create.cshtml* ページと *Pages/Customers/Edit.cshtml* ページが *Pages/Index.cshtml* にリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="35dc6-267">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="35dc6-268">文字列 `/Index` は前のページにアクセスするための URI の一部です。</span><span class="sxs-lookup"><span data-stu-id="35dc6-268">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="35dc6-269">文字列 `/Index` は、*Pages/Index.cshtml* ページへの URI を生成するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="35dc6-269">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="35dc6-270">例:</span><span class="sxs-lookup"><span data-stu-id="35dc6-270">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="35dc6-271">ページ名は、ルート */Pages* フォルダーからページへのパスです (たとえば `/Index` の場合、先頭の `/` を含みます)。</span><span class="sxs-lookup"><span data-stu-id="35dc6-271">The page name is the path to the page from the root */Pages* folder (including a leading `/`, for example `/Index`).</span></span> <span data-ttu-id="35dc6-272">上記の URL の生成サンプルは、単に URL をハードコーディングするよりもはるかに多機能です。</span><span class="sxs-lookup"><span data-stu-id="35dc6-272">The preceding URL generation samples are much more feature rich than just hardcoding a URL.</span></span> <span data-ttu-id="35dc6-273">URL の生成は[ルーティング](xref:mvc/controllers/routing)を使用し、ターゲット パスで定義されたルート方法に従って、パラメーターの生成とエンコードができます。</span><span class="sxs-lookup"><span data-stu-id="35dc6-273">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="35dc6-274">ページの URL 生成は、相対名をサポートします。</span><span class="sxs-lookup"><span data-stu-id="35dc6-274">URL generation for pages supports relative names.</span></span> <span data-ttu-id="35dc6-275">次の表に、*Pages/Customers/Create.cshtml* の異なる `RedirectToPage` パラメーターで選択されたインデックス ページを示します。</span><span class="sxs-lookup"><span data-stu-id="35dc6-275">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="35dc6-276">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="35dc6-276">RedirectToPage(x)</span></span>| <span data-ttu-id="35dc6-277">ページ</span><span class="sxs-lookup"><span data-stu-id="35dc6-277">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="35dc6-278">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="35dc6-278">RedirectToPage("/Index")</span></span> | <span data-ttu-id="35dc6-279">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="35dc6-279">*Pages/Index*</span></span> |
| <span data-ttu-id="35dc6-280">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="35dc6-280">RedirectToPage("./Index");</span></span> | <span data-ttu-id="35dc6-281">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="35dc6-281">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="35dc6-282">RedirectToPage("../Index")</span><span class="sxs-lookup"><span data-stu-id="35dc6-282">RedirectToPage("../Index")</span></span> | <span data-ttu-id="35dc6-283">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="35dc6-283">*Pages/Index*</span></span> |
| <span data-ttu-id="35dc6-284">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="35dc6-284">RedirectToPage("Index")</span></span>  | <span data-ttu-id="35dc6-285">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="35dc6-285">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="35dc6-286">`RedirectToPage("Index")`、`RedirectToPage("./Index")`、および `RedirectToPage("../Index")` は*相対名*です。</span><span class="sxs-lookup"><span data-stu-id="35dc6-286">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are *relative names*.</span></span> <span data-ttu-id="35dc6-287">`RedirectToPage` パラメーターは現在のページのパスと*組み合わされて*、ターゲット ページの名前を計算します。</span><span class="sxs-lookup"><span data-stu-id="35dc6-287">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page. -- page name, not page path -->

<span data-ttu-id="35dc6-288">相対名のリンクは、複雑な構造を持つサイトを構築する際に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="35dc6-288">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="35dc6-289">相対名を使用してフォルダー内のページ間をリンクする場合、そのフォルダー名を変更することができます。</span><span class="sxs-lookup"><span data-stu-id="35dc6-289">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="35dc6-290">すべてのリンクは引き続き機能します (リンクにはフォルダー名が含まれていないため)。</span><span class="sxs-lookup"><span data-stu-id="35dc6-290">All the links still work (because they didn't include the folder name).</span></span>

## <a name="tempdata"></a><span data-ttu-id="35dc6-291">TempData</span><span class="sxs-lookup"><span data-stu-id="35dc6-291">TempData</span></span>

<span data-ttu-id="35dc6-292">ASP.NET Core は [コントローラー](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller)上で [TempData](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) プロパティを公開します。</span><span class="sxs-lookup"><span data-stu-id="35dc6-292">ASP.NET Core exposes the [TempData](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="35dc6-293">このプロパティは、読み取られるまでデータを格納します。</span><span class="sxs-lookup"><span data-stu-id="35dc6-293">This property stores data until it's read.</span></span> <span data-ttu-id="35dc6-294">`Keep` メソッドと `Peek` メソッドは、削除せずにデータを確認するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="35dc6-294">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="35dc6-295">`TempData` は、複数の要求にデータが必要な場合のリダイレクトに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="35dc6-295">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="35dc6-296">`[TempData]` は ASP.NET Core 2.0 の新しい属性で、コントローラーとページでサポートされています。</span><span class="sxs-lookup"><span data-stu-id="35dc6-296">The `[TempData]` attribute is new in ASP.NET Core 2.0 and is supported on controllers and pages.</span></span>

<span data-ttu-id="35dc6-297">次のコードは、`TempData` を使用して `Message` の値を設定します。</span><span class="sxs-lookup"><span data-stu-id="35dc6-297">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="35dc6-298">*Pages/Customers/Index.cshtml* ファイル内の次のマークアップは、`TempData` を使用して `Message` の値を表示します。</span><span class="sxs-lookup"><span data-stu-id="35dc6-298">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="35dc6-299">*Pages/Customers/Index.cshtml.cs* ページは、`[TempData]` 属性を `Message` プロパティに適用します。</span><span class="sxs-lookup"><span data-stu-id="35dc6-299">The *Pages/Customers/Index.cshtml.cs* page model applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="35dc6-300">詳細については、「[TempData](xref:fundamentals/app-state#temp)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="35dc6-300">See [TempData](xref:fundamentals/app-state#temp) for more information.</span></span>

<a name="mhpp"></a>
## <a name="multiple-handlers-per-page"></a><span data-ttu-id="35dc6-301">ページあたり複数のハンドラー</span><span class="sxs-lookup"><span data-stu-id="35dc6-301">Multiple handlers per page</span></span>

<span data-ttu-id="35dc6-302">次のページは、`asp-page-handler` タグ ヘルパーを使用して 2 つのページ ハンドラーにマークアップを生成します。</span><span class="sxs-lookup"><span data-stu-id="35dc6-302">The following page generates markup for two page handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there's no `asp-` attribute   -->

<span data-ttu-id="35dc6-303">前の例のフォームには、それぞれが `FormActionTagHelper` を使用して異なる URL に送信する 2 つの送信ボタンがあります。</span><span class="sxs-lookup"><span data-stu-id="35dc6-303">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="35dc6-304">`asp-page-handler` 属性は、`asp-page` のコンパニオンです。</span><span class="sxs-lookup"><span data-stu-id="35dc6-304">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="35dc6-305">`asp-page-handler` はページごとに定義されている各ハンドラー メソッドに送信する URL を生成します。</span><span class="sxs-lookup"><span data-stu-id="35dc6-305">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="35dc6-306">サンプルは現在のページにリンクしているため、`asp-page` は指定されません。</span><span class="sxs-lookup"><span data-stu-id="35dc6-306">`asp-page` isn't specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="35dc6-307">ページ モデル: </span><span class="sxs-lookup"><span data-stu-id="35dc6-307">The page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="35dc6-308">上記のコードは、*名前付きハンドラー メソッド*を使用しています。</span><span class="sxs-lookup"><span data-stu-id="35dc6-308">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="35dc6-309">名前付きハンドラー メソッドは、名前の `On<HTTP Verb>` の後および `Async` の前 (ある場合) のテキストを取得して作成されます。</span><span class="sxs-lookup"><span data-stu-id="35dc6-309">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="35dc6-310">前の例では、ページ メソッドは OnPost**JoinList**Async と OnPost**JoinListUC**Async です。</span><span class="sxs-lookup"><span data-stu-id="35dc6-310">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="35dc6-311">*OnPost* と *Async* を削除すると、ハンドラー名は `JoinList` と `JoinListUC` になります。</span><span class="sxs-lookup"><span data-stu-id="35dc6-311">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="35dc6-312">上記のコードを使用すると、`OnPostJoinListAsync` に送信される URL パスは `http://localhost:5000/Customers/CreateFATH?handler=JoinList` になります。</span><span class="sxs-lookup"><span data-stu-id="35dc6-312">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="35dc6-313">`OnPostJoinListUCAsync` に送信される URL パスは `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC` です。</span><span class="sxs-lookup"><span data-stu-id="35dc6-313">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="customizing-routing"></a><span data-ttu-id="35dc6-314">ルーティングのカスタマイズ</span><span class="sxs-lookup"><span data-stu-id="35dc6-314">Customizing Routing</span></span>

<span data-ttu-id="35dc6-315">URL 内のクエリ文字列 `?handler=JoinList` が気に入らない場合は、ルートを変更して URL のパス部分にハンドラー名を挿入することができます。</span><span class="sxs-lookup"><span data-stu-id="35dc6-315">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="35dc6-316">`@page` ディレクティブの後に二重引用符で囲んだルート テンプレートを追加して、ルートをカスタマイズすることができます。</span><span class="sxs-lookup"><span data-stu-id="35dc6-316">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="35dc6-317">上記のルートでは、クエリ文字列の代わりにハンドラー名を URL パスに挿入しています。</span><span class="sxs-lookup"><span data-stu-id="35dc6-317">The preceding route puts the handler name in the URL path instead of the query string.</span></span> <span data-ttu-id="35dc6-318">`handler` の後の `?` は、ルート パラメーターが省略可能なことを意味します。</span><span class="sxs-lookup"><span data-stu-id="35dc6-318">The `?` following `handler` means the route parameter is optional.</span></span>

<span data-ttu-id="35dc6-319">`@page` を使用して、ページのルートに追加のセグメントとパラメーターを追加できます。</span><span class="sxs-lookup"><span data-stu-id="35dc6-319">You can use `@page` to add additional segments and parameters to a page's route.</span></span> <span data-ttu-id="35dc6-320">ここにあるものはすべて、既定のルートに**追加**されます。</span><span class="sxs-lookup"><span data-stu-id="35dc6-320">Whatever's there's **appended** to the default route of the page.</span></span> <span data-ttu-id="35dc6-321">絶対パスまたは仮想パスを使用してページのルート (`"~/Some/Other/Path"` など) を変更することはサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="35dc6-321">Using an absolute or virtual path to change the page's route (like `"~/Some/Other/Path"`) isn't supported.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="35dc6-322">構成と設定</span><span class="sxs-lookup"><span data-stu-id="35dc6-322">Configuration and settings</span></span>

<span data-ttu-id="35dc6-323">高度なオプションを構成するには、MVC ビルダーで拡張メソッド `AddRazorPagesOptions` を使用します。</span><span class="sxs-lookup"><span data-stu-id="35dc6-323">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

<span data-ttu-id="35dc6-324">現在は、`RazorPagesOptions` を使用してページのルート ディレクトリを設定したり、ページのアプリケーション モデルの規則を追加したりすることができます。</span><span class="sxs-lookup"><span data-stu-id="35dc6-324">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="35dc6-325">将来、この方法でより多くの機能拡張を可能にしたいと考えています。</span><span class="sxs-lookup"><span data-stu-id="35dc6-325">We'll enable more extensibility this way in the future.</span></span>

<span data-ttu-id="35dc6-326">ビューをプリコンパイルするには、「[Razor view compilation](xref:mvc/views/view-compilation)」 (Razor ビュー コンパイル) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="35dc6-326">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="35dc6-327">[サンプル コードをダウンロードまたは表示します](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample)。</span><span class="sxs-lookup"><span data-stu-id="35dc6-327">[Download or view sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span></span>

<span data-ttu-id="35dc6-328">この概要に基づく、「[Getting started with Razor Pages in ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start)」 (ASP.NET Core での Razor ページの概要) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="35dc6-328">See [Getting started with Razor Pages in ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="35dc6-329">Razor ページをコンテンツのルートに指定する</span><span class="sxs-lookup"><span data-stu-id="35dc6-329">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="35dc6-330">Razor ページのルートは既定で */Pages* ディレクトリです。</span><span class="sxs-lookup"><span data-stu-id="35dc6-330">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="35dc6-331">Razor ページをアプリのコンテンツ ルート ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) に指定するには、[WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) を [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) に追加します。</span><span class="sxs-lookup"><span data-stu-id="35dc6-331">Add [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at the content root ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) of the app:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="35dc6-332">Razor ページをカスタム ルート ディレクトリに指定する</span><span class="sxs-lookup"><span data-stu-id="35dc6-332">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="35dc6-333">(相対パスを指定して) Razor ページをアプリのカスタム ルート ディレクトリに指定するには、[WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) を [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) に追加します。</span><span class="sxs-lookup"><span data-stu-id="35dc6-333">Add [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="see-also"></a><span data-ttu-id="35dc6-334">関連項目</span><span class="sxs-lookup"><span data-stu-id="35dc6-334">See also</span></span>

* [<span data-ttu-id="35dc6-335">Razor ページの概要</span><span class="sxs-lookup"><span data-stu-id="35dc6-335">Getting started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="35dc6-336">Razor ページの承認規則</span><span class="sxs-lookup"><span data-stu-id="35dc6-336">Razor Pages authorization conventions</span></span>](xref:security/authorization/razor-pages-authorization)
* [<span data-ttu-id="35dc6-337">Razor ページのカスタム ルートとページ モデル プロバイダー</span><span class="sxs-lookup"><span data-stu-id="35dc6-337">Razor Pages custom route and page model providers</span></span>](xref:mvc/razor-pages/razor-pages-convention-features)
* [<span data-ttu-id="35dc6-338">Razor ページの単体テストと統合テスト</span><span class="sxs-lookup"><span data-stu-id="35dc6-338">Razor Pages unit and integration testing</span></span>](xref:testing/razor-pages-testing)
