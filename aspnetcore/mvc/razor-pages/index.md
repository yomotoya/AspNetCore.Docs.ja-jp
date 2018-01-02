---
title: "ASP.NET Core での Razor ページの概要"
author: Rick-Anderson
description: "このドキュメントは、ページに重点を置いたシナリオの開発を容易にするために、ASP.NET Core での Razor ページを使用するための概要を提供します。"
keywords: "ASP.NET Core、Razor ページ"
ms.author: riande
manager: wpickett
ms.date: 09/12/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/razor-pages/index
ms.openlocfilehash: a66b5ea32c2090b9944cd61f90f7fe011a823e82
ms.sourcegitcommit: 3511552becb081fb860a23d6c9b6c4efcab74577
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/07/2017
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="2b420-104">ASP.NET Core での Razor ページの概要</span><span class="sxs-lookup"><span data-stu-id="2b420-104">Introduction to Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="2b420-105">[Rick Anderson](https://twitter.com/RickAndMSFT) および [Ryan Nowak](https://github.com/rynowak) 著</span><span class="sxs-lookup"><span data-stu-id="2b420-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="2b420-106">Razor ページは、ページ コーディングに重点を置いたシナリオをより簡略化し、生産性を高める ASP.NET Core MVC の新機能です。</span><span class="sxs-lookup"><span data-stu-id="2b420-106">Razor Pages is a new feature of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="2b420-107">モデル ビュー コントローラーのアプローチを使用するチュートリアルをお探しの場合は、「[Getting started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc)」 (ASP.NET Core MVC の概要) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="2b420-107">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Getting started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="2b420-108">このドキュメントでは、Razor ページの概要について説明します。</span><span class="sxs-lookup"><span data-stu-id="2b420-108">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="2b420-109">手順を追って説明するチュートリアルではありません。</span><span class="sxs-lookup"><span data-stu-id="2b420-109">It's not a step by step tutorial.</span></span> <span data-ttu-id="2b420-110">セクションの一部を理解できない場合は、[Razor ページの概要](xref:tutorials/razor-pages/razor-pages-start)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="2b420-110">If you find some of the sections difficult to follow, see [Getting started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span>

<a name="prerequisites"></a>

## <a name="aspnet-core-20-prerequisites"></a><span data-ttu-id="2b420-111">ASP.NET Core 2.0 の前提条件</span><span class="sxs-lookup"><span data-stu-id="2b420-111">ASP.NET Core 2.0 prerequisites</span></span>

<span data-ttu-id="2b420-112">[.NET Core](https://www.microsoft.com/net/core) 2.0.0 以降をインストールします。</span><span class="sxs-lookup"><span data-stu-id="2b420-112">Install [.NET Core](https://www.microsoft.com/net/core) 2.0.0 or later.</span></span>

<span data-ttu-id="2b420-113">Visual Studio を使用している場合は、以下のワークロードで [Visual Studio](https://www.visualstudio.com/vs/) 2017 Version 15.3 以降をインストールします。</span><span class="sxs-lookup"><span data-stu-id="2b420-113">If you're using Visual Studio, install [Visual Studio](https://www.visualstudio.com/vs/) 2017 version 15.3 or later with the following workloads:</span></span>

* <span data-ttu-id="2b420-114">**ASP.NET と Web 開発**</span><span class="sxs-lookup"><span data-stu-id="2b420-114">**ASP.NET and web development**</span></span>
* <span data-ttu-id="2b420-115">**.NET Core クロスプラットフォームの開発**</span><span class="sxs-lookup"><span data-stu-id="2b420-115">**.NET Core cross-platform development**</span></span>

<a name="rpvs17"></a>

## <a name="creating-a-razor-pages-project"></a><span data-ttu-id="2b420-116">Razor ページ プロジェクトの作成</span><span class="sxs-lookup"><span data-stu-id="2b420-116">Creating a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2b420-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2b420-117">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="2b420-118">Visual Studio を使用して Razor ページ プロジェクトを作成する詳細な手順については、「[Getting started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start)」 (Razor ページの概要) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="2b420-118">See [Getting started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project using Visual Studio.</span></span>

#   <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2b420-119">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="2b420-119">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="2b420-120">コマンド ラインから `dotnet new razor` を実行します。</span><span class="sxs-lookup"><span data-stu-id="2b420-120">Run `dotnet new razor` from the command line.</span></span>

<span data-ttu-id="2b420-121">Visual Studio for Mac から生成された *.csproj* ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="2b420-121">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2b420-122">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2b420-122">Visual Studio Code</span></span>](#tab/visual-studio-code) 

<span data-ttu-id="2b420-123">コマンド ラインから `dotnet new razor` を実行します。</span><span class="sxs-lookup"><span data-stu-id="2b420-123">Run `dotnet new razor` from the command line.</span></span>

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="2b420-124">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="2b420-124">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="2b420-125">コマンド ラインから `dotnet new razor` を実行します。</span><span class="sxs-lookup"><span data-stu-id="2b420-125">Run `dotnet new razor` from the command line.</span></span>

---

## <a name="razor-pages"></a><span data-ttu-id="2b420-126">Razor ページ</span><span class="sxs-lookup"><span data-stu-id="2b420-126">Razor Pages</span></span>

<span data-ttu-id="2b420-127">Razor ページは *Startup.cs* で有効になっています。</span><span class="sxs-lookup"><span data-stu-id="2b420-127">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[main](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

<span data-ttu-id="2b420-128">基本ページを検討します。<a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="2b420-128">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

<span data-ttu-id="2b420-129">上記のコードは Razor ビュー ファイルに非常によく似ています。</span><span class="sxs-lookup"><span data-stu-id="2b420-129">The preceding code looks a lot like a Razor view file.</span></span> <span data-ttu-id="2b420-130">違いは、`@page` ディレクティブにあります。</span><span class="sxs-lookup"><span data-stu-id="2b420-130">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="2b420-131">`@page` はファイルを MVC アクションにします。つまり、コントローラーを経由せずに要求を直接処理します。</span><span class="sxs-lookup"><span data-stu-id="2b420-131">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="2b420-132">`@page` はページで最初の Razor ディレクティブである必要があります。</span><span class="sxs-lookup"><span data-stu-id="2b420-132">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="2b420-133">`@page` はその他の Razor コンストラクトの動作に影響します。</span><span class="sxs-lookup"><span data-stu-id="2b420-133">`@page` affects the behavior of other Razor constructs.</span></span>

<span data-ttu-id="2b420-134">`PageModel` クラスを使用している類似したページが、次の 2 つのファイルにあります。</span><span class="sxs-lookup"><span data-stu-id="2b420-134">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="2b420-135">*Pages/Index2.cshtml* ファイル:</span><span class="sxs-lookup"><span data-stu-id="2b420-135">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="2b420-136">*Pages/Index2.cshtml.cs* "code-behind" ファイル:</span><span class="sxs-lookup"><span data-stu-id="2b420-136">The *Pages/Index2.cshtml.cs* "code-behind" file:</span></span>

[!code-cs[main](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="2b420-137">規則により、`PageModel` クラス ファイルは、Razor ページ ファイルと同じ名前に *.cs* が付加された名前になります。</span><span class="sxs-lookup"><span data-stu-id="2b420-137">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="2b420-138">たとえば、上の Razor ページは *Pages/Index2.cshtml* になります。</span><span class="sxs-lookup"><span data-stu-id="2b420-138">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="2b420-139">`PageModel` クラスを含むファイル名は、*Pages/Index2.cshtml.cs* になります。</span><span class="sxs-lookup"><span data-stu-id="2b420-139">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="2b420-140">URL パスのページへの関連付けは、ファイル システム内のページの場所によって決定されます。</span><span class="sxs-lookup"><span data-stu-id="2b420-140">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="2b420-141">次の表に、Razor ページ パスと一致 URL を示します。</span><span class="sxs-lookup"><span data-stu-id="2b420-141">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="2b420-142">ファイル名とパス</span><span class="sxs-lookup"><span data-stu-id="2b420-142">File name and path</span></span>               | <span data-ttu-id="2b420-143">一致 URL</span><span class="sxs-lookup"><span data-stu-id="2b420-143">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="2b420-144">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="2b420-144">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="2b420-145">`/` または `/Index`</span><span class="sxs-lookup"><span data-stu-id="2b420-145">`/` or `/Index`</span></span> |
| <span data-ttu-id="2b420-146">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="2b420-146">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="2b420-147">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="2b420-147">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="2b420-148">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="2b420-148">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="2b420-149">`/Store` または `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="2b420-149">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="2b420-150">メモ:</span><span class="sxs-lookup"><span data-stu-id="2b420-150">Notes:</span></span>

* <span data-ttu-id="2b420-151">既定では、ランタイムが *Pages* フォルダー内で Razor ページ ファイルを検索します。</span><span class="sxs-lookup"><span data-stu-id="2b420-151">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="2b420-152">`Index` は、URL にページが含まれない場合の既定のページになります。</span><span class="sxs-lookup"><span data-stu-id="2b420-152">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="writing-a-basic-form"></a><span data-ttu-id="2b420-153">基本フォームの作成</span><span class="sxs-lookup"><span data-stu-id="2b420-153">Writing a basic form</span></span>

<span data-ttu-id="2b420-154">Razor ページ機能は、Web ブラウザーで使用される一般的なパターンを簡単にするために設計されています。</span><span class="sxs-lookup"><span data-stu-id="2b420-154">Razor Pages features are designed to make common patterns used with web browsers easy.</span></span> <span data-ttu-id="2b420-155">[モデル バインド](xref:mvc/models/model-binding)、[タグ ヘルパー](xref:mvc/views/tag-helpers/intro)、および HTML ヘルパーはすべて、Razor ページ クラスで定義されたプロパティで*機能します*。</span><span class="sxs-lookup"><span data-stu-id="2b420-155">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="2b420-156">`Contact` モデルの基本的な "お問い合わせ" フォームを実装するページを考察します。</span><span class="sxs-lookup"><span data-stu-id="2b420-156">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="2b420-157">このドキュメントのサンプルでは、[Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) ファイルで `DbContext` が初期化されます。</span><span class="sxs-lookup"><span data-stu-id="2b420-157">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

<span data-ttu-id="2b420-158">データ モデル:</span><span class="sxs-lookup"><span data-stu-id="2b420-158">The data model:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Data/Customer.cs)]

<span data-ttu-id="2b420-159">db コンテキスト:</span><span class="sxs-lookup"><span data-stu-id="2b420-159">The db context:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

<span data-ttu-id="2b420-160">*Pages/Create.cshtml* ビュー ファイル:</span><span class="sxs-lookup"><span data-stu-id="2b420-160">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

<span data-ttu-id="2b420-161">ビューの *Pages/Create.cshtml.cs* 分離コード ファイル:</span><span class="sxs-lookup"><span data-stu-id="2b420-161">The *Pages/Create.cshtml.cs* code-behind file for the view:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="2b420-162">規則により、`PageModel` クラスは `<PageName>Model` と呼ばれ、ページと同じ名前空間にあります。</span><span class="sxs-lookup"><span data-stu-id="2b420-162">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="2b420-163">`PageModel` クラスでは、ページの表示からロジックを分離できます。</span><span class="sxs-lookup"><span data-stu-id="2b420-163">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="2b420-164">これは、ページに送信される要求のページ ハンドラーと、ページのレンダリングに使用されるデータを定義します。</span><span class="sxs-lookup"><span data-stu-id="2b420-164">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="2b420-165">この分離により、ユーザーは[依存関係の挿入](xref:fundamentals/dependency-injection)を通じてページの依存関係を管理し、ページの[単体テスト](xref:testing/razor-pages-testing)を実行できます。</span><span class="sxs-lookup"><span data-stu-id="2b420-165">This separation allows you to manage page dependencies through [dependency injection](xref:fundamentals/dependency-injection) and to [unit test](xref:testing/razor-pages-testing) the pages.</span></span>

<span data-ttu-id="2b420-166">このページには、(ユーザーがフォームを投稿したときに) `POST` 要求で実行される `OnPostAsync` *ハンドラー メソッド*があります。</span><span class="sxs-lookup"><span data-stu-id="2b420-166">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="2b420-167">任意の HTTP 動詞のハンドラー メソッドを追加できます。</span><span class="sxs-lookup"><span data-stu-id="2b420-167">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="2b420-168">最も一般的なハンドラーは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="2b420-168">The most common handlers are:</span></span>

* <span data-ttu-id="2b420-169">ページに必要な状態を初期化するための `OnGet`。</span><span class="sxs-lookup"><span data-stu-id="2b420-169">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="2b420-170">[OnGet](#OnGet) サンプル。</span><span class="sxs-lookup"><span data-stu-id="2b420-170">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="2b420-171">フォームの送信を処理するための `OnPost`。</span><span class="sxs-lookup"><span data-stu-id="2b420-171">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="2b420-172">`Async` 名前付けサフィックスは省略可能ですが、非同期関数の規則でよく使用されます。</span><span class="sxs-lookup"><span data-stu-id="2b420-172">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="2b420-173">上記の例の `OnPostAsync` コードは、コントローラーで通常に記述するものに似ています。</span><span class="sxs-lookup"><span data-stu-id="2b420-173">The `OnPostAsync` code in the preceding example looks similar to what you would normally write in a controller.</span></span> <span data-ttu-id="2b420-174">上記のコードは、Razor ページでは一般的です。</span><span class="sxs-lookup"><span data-stu-id="2b420-174">The preceding code is typical for Razor Pages.</span></span> <span data-ttu-id="2b420-175">[モデル バインド](xref:mvc/models/model-binding)、[検証](xref:mvc/models/validation)、およびアクションの結果などのほとんどの MVC プリミティブは共有されます。</span><span class="sxs-lookup"><span data-stu-id="2b420-175">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results are shared.</span></span>  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

<span data-ttu-id="2b420-176">上記の `OnPostAsync` メソッド:</span><span class="sxs-lookup"><span data-stu-id="2b420-176">The previous `OnPostAsync` method:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="2b420-177">`OnPostAsync` の基本的な流れは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="2b420-177">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="2b420-178">検証エラーを確認します。</span><span class="sxs-lookup"><span data-stu-id="2b420-178">Check for validation errors.</span></span>

*  <span data-ttu-id="2b420-179">エラーがない場合は、データを保存し、リダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="2b420-179">If there are no errors, save the data and redirect.</span></span>
*  <span data-ttu-id="2b420-180">エラーがある場合は、検証メッセージとともにページをもう一度表示します。</span><span class="sxs-lookup"><span data-stu-id="2b420-180">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="2b420-181">クライアント側の検証は、従来の ASP.NET Core MVC アプリケーションと同じです。</span><span class="sxs-lookup"><span data-stu-id="2b420-181">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="2b420-182">多くの場合、検証エラーはクライアントで検出され、サーバーには送信されません。</span><span class="sxs-lookup"><span data-stu-id="2b420-182">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="2b420-183">データが正常に入力されると、`OnPostAsync` ハンドラー メソッドが `RedirectToPage` ヘルパー メソッドを呼び出して `RedirectToPageResult` のインスタンスを返します。</span><span class="sxs-lookup"><span data-stu-id="2b420-183">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="2b420-184">`RedirectToPage` は、`RedirectToAction` や `RedirectToRoute` と同じような新しいアクション結果ですが、ページ用にカスタマイズされています。</span><span class="sxs-lookup"><span data-stu-id="2b420-184">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="2b420-185">上記のサンプルでは、ルート インデックス ページ (`/Index`) にリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="2b420-185">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="2b420-186">`RedirectToPage` については、「[ページの URL の生成](#url_gen)」セクションで詳しく説明されています。</span><span class="sxs-lookup"><span data-stu-id="2b420-186">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="2b420-187">送信されたフォームに検証エラー (サーバーに渡される) があると、`OnPostAsync` ハンドラー メソッドが `Page` ヘルパー メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="2b420-187">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="2b420-188">`Page` は `PageResult` のインスタンスを返します。</span><span class="sxs-lookup"><span data-stu-id="2b420-188">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="2b420-189">`Page` を返すのは、コントローラーのアクションが `View` を返す方法に似ています。</span><span class="sxs-lookup"><span data-stu-id="2b420-189">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="2b420-190">`PageResult` はハンドラー メソッドの既定の<!-- Review  -->戻り値の型です。</span><span class="sxs-lookup"><span data-stu-id="2b420-190">`PageResult` is the default <!-- Review  --> return type for a handler method.</span></span> <span data-ttu-id="2b420-191">`void` を返すハンドラー メソッドがページをレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="2b420-191">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="2b420-192">`Customer` プロパティは `[BindProperty]` 属性を使用してモデル バインドにオプトインします。</span><span class="sxs-lookup"><span data-stu-id="2b420-192">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

<span data-ttu-id="2b420-193">既定では、Razor ページはプロパティを非 GET 動詞とのみバインドします。</span><span class="sxs-lookup"><span data-stu-id="2b420-193">Razor Pages, by default, bind properties only with non-GET verbs.</span></span> <span data-ttu-id="2b420-194">プロパティをバインドすることで、記述すべきコードの量を削減できます。</span><span class="sxs-lookup"><span data-stu-id="2b420-194">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="2b420-195">同じプロパティを使用してバインドすることでコードを減らし、フィールド (`<input asp-for="Customer.Name" />`) からレンダリングして入力を受け入れます。</span><span class="sxs-lookup"><span data-stu-id="2b420-195">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name" />`) and accept the input.</span></span>

<span data-ttu-id="2b420-196">ホーム ページ (*Index.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="2b420-196">The home page (*Index.cshtml*):</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

<span data-ttu-id="2b420-197">分離コード *Index.cshtml.cs* ファイル:</span><span class="sxs-lookup"><span data-stu-id="2b420-197">The code behind *Index.cshtml.cs* file:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

<span data-ttu-id="2b420-198">*Index.cshtml* ファイルには、各連絡先の編集リンクを作成するために次のマークアップが含まれています。</span><span class="sxs-lookup"><span data-stu-id="2b420-198">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

<span data-ttu-id="2b420-199">[アンカー タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)は [asp-route-{value}](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#route) 属性を使用して編集ページへのリンクを生成しました。</span><span class="sxs-lookup"><span data-stu-id="2b420-199">The [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the [asp-route-{value}](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#route) attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="2b420-200">リンクには、連絡先 ID とともにルート データが含まれています。</span><span class="sxs-lookup"><span data-stu-id="2b420-200">The link contains route data with the contact ID.</span></span> <span data-ttu-id="2b420-201">たとえば、`http://localhost:5000/Edit/1` のようにします。</span><span class="sxs-lookup"><span data-stu-id="2b420-201">For example, `http://localhost:5000/Edit/1`.</span></span>

<span data-ttu-id="2b420-202">*Pages/Edit.cshtml* ファイル:</span><span class="sxs-lookup"><span data-stu-id="2b420-202">The *Pages/Edit.cshtml* file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

<span data-ttu-id="2b420-203">最初の行には `@page "{id:int}"` ディレクティブが含まれています。</span><span class="sxs-lookup"><span data-stu-id="2b420-203">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="2b420-204">ルーティングの制約 `"{id:int}"` は、`int` ルート データを含むページへの要求を受け入れるようにページに指示します。</span><span class="sxs-lookup"><span data-stu-id="2b420-204">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="2b420-205">ページへの要求に `int` に変換できるルート データが含まれていない場合は、ランタイムで HTTP 404 (見つかりません) エラーが返されます。</span><span class="sxs-lookup"><span data-stu-id="2b420-205">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span>

<span data-ttu-id="2b420-206">*Pages/Edit.cshtml.cs* ファイル:</span><span class="sxs-lookup"><span data-stu-id="2b420-206">The *Pages/Edit.cshtml.cs* file:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

<span data-ttu-id="2b420-207">*Index.cshtml* ファイルには、各顧客の連絡先の削除ボタンを作成するマークアップも含まれています。</span><span class="sxs-lookup"><span data-stu-id="2b420-207">The *Index.cshtml* file also contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

<span data-ttu-id="2b420-208">HTML で削除ボタンがレンダリングされる場合、その `formaction` には次のパラメーターが含まれています。</span><span class="sxs-lookup"><span data-stu-id="2b420-208">When the delete button is rendered in HTML, its `formaction` includes parameters for:</span></span>

* <span data-ttu-id="2b420-209">`asp-route-id` 属性によって指定された顧客の連絡先 ID。</span><span class="sxs-lookup"><span data-stu-id="2b420-209">The customer contact ID specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="2b420-210">`asp-page-handler` 属性によって指定された `handler`。</span><span class="sxs-lookup"><span data-stu-id="2b420-210">The `handler` specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="2b420-211">顧客の連絡先 ID `1` でレンダリングされた削除ボタンの例を示します。</span><span class="sxs-lookup"><span data-stu-id="2b420-211">Here is an example of a rendered delete button with a customer contact ID of `1`:</span></span>

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="2b420-212">ボタンが選択されると、フォームの `POST` 要求がサーバーに送信されます。</span><span class="sxs-lookup"><span data-stu-id="2b420-212">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="2b420-213">慣例により、ハンドラー メソッドの名前はスキーム `OnPost[handler]Async` に従った `handler` パラメーターの値に基づいて選択されます。</span><span class="sxs-lookup"><span data-stu-id="2b420-213">By convention, the name of the handler method is selected based the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="2b420-214">この例では `handler` が `delete` であるため、`OnPostDeleteAsync` ハンドラー メソッドを使用して `POST` 要求が処理されます。</span><span class="sxs-lookup"><span data-stu-id="2b420-214">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="2b420-215">`asp-page-handler` が `remove` などの別の値に設定されている場合、名前が `OnPostRemoveAsync` のページ ハンドラー メソッドが選択されます。</span><span class="sxs-lookup"><span data-stu-id="2b420-215">If the `asp-page-handler` is set to a different value, such as `remove`, a page handler method with the name `OnPostRemoveAsync` is selected.</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

<span data-ttu-id="2b420-216">`OnPostDeleteAsync` メソッド:</span><span class="sxs-lookup"><span data-stu-id="2b420-216">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="2b420-217">クエリ文字列から `id` を受け入れます。</span><span class="sxs-lookup"><span data-stu-id="2b420-217">Accepts the `id` from the query string.</span></span>
* <span data-ttu-id="2b420-218">`FindAsync` を使用してデータベースから顧客の連絡先を照会します。</span><span class="sxs-lookup"><span data-stu-id="2b420-218">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="2b420-219">顧客の連絡先が見つかった場合、その連絡先は顧客の連絡先の一覧から削除されています。</span><span class="sxs-lookup"><span data-stu-id="2b420-219">If the customer contact is found, they're removed from the list of customer contacts.</span></span> <span data-ttu-id="2b420-220">データベースが更新されます。</span><span class="sxs-lookup"><span data-stu-id="2b420-220">The database is updated.</span></span>
* <span data-ttu-id="2b420-221">ルート インデックス ページ (`/Index`) にリダイレクトされるように、`RedirectToPage` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="2b420-221">Calls `RedirectToPage` to redirect to the root Index page (`/Index`).</span></span>

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="2b420-222">XSRF/CSRF と Razor ページ</span><span class="sxs-lookup"><span data-stu-id="2b420-222">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="2b420-223">[偽造防止検証](xref:security/anti-request-forgery)のためにコードを記述する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="2b420-223">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="2b420-224">偽造防止トークンの生成と検証は、自動的に Razor ページに含まれます。</span><span class="sxs-lookup"><span data-stu-id="2b420-224">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>
## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="2b420-225">Razor ページでのレイアウト、パーシャル、テンプレート、およびタグ ヘルパーの使用</span><span class="sxs-lookup"><span data-stu-id="2b420-225">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="2b420-226">ページは、Razor ビュー エンジンのすべての機能で機能します。</span><span class="sxs-lookup"><span data-stu-id="2b420-226">Pages work with all the features of the Razor view engine.</span></span> <span data-ttu-id="2b420-227">レイアウト、パーシャル、テンプレート、タグ ヘルパー、*_ViewStart.cshtml*、*_ViewImports.cshtml* は、従来の Razor ビューと同じように動作します。</span><span class="sxs-lookup"><span data-stu-id="2b420-227">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="2b420-228">これらの機能の一部を利用してこのページをまとめてみましょう。</span><span class="sxs-lookup"><span data-stu-id="2b420-228">Let's declutter this page by taking advantage of some of those features.</span></span>

<span data-ttu-id="2b420-229">[レイアウト ページ](xref:mvc/views/layout)を *Pages/_Layout.cshtml* に追加します。</span><span class="sxs-lookup"><span data-stu-id="2b420-229">Add a [layout page](xref:mvc/views/layout) to *Pages/_Layout.cshtml*:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

<span data-ttu-id="2b420-230">[レイアウト](xref:mvc/views/layout)は次のことを行います。</span><span class="sxs-lookup"><span data-stu-id="2b420-230">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="2b420-231">(ページでレイアウトを止めない限り) 各ページのレイアウトを制御します。</span><span class="sxs-lookup"><span data-stu-id="2b420-231">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="2b420-232">JavaScript やスタイルシートなどの HTML 構造をインポートします。</span><span class="sxs-lookup"><span data-stu-id="2b420-232">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="2b420-233">詳細については、[レイアウトのページ](xref:mvc/views/layout)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="2b420-233">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="2b420-234">[Layout](xref:mvc/views/layout#specifying-a-layout) プロパティは *Pages/_ViewStart.cshtml* で設定されています。</span><span class="sxs-lookup"><span data-stu-id="2b420-234">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

<span data-ttu-id="2b420-235">**注:** レイアウトは、*Pages* フォルダー内にあります。</span><span class="sxs-lookup"><span data-stu-id="2b420-235">**Note:** The layout is in the *Pages* folder.</span></span> <span data-ttu-id="2b420-236">ページは現在のページと同じフォルダーから開始して、階層的に他のビュー (レイアウト、テンプレート、パーシャル) を検索します。</span><span class="sxs-lookup"><span data-stu-id="2b420-236">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="2b420-237">*Pages* フォルダー内のレイアウトは、*Pages* フォルダー配下の任意の Razor ページから使用できます。</span><span class="sxs-lookup"><span data-stu-id="2b420-237">A layout in the *Pages* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="2b420-238">レイアウト ファイルを *Views/Shared* フォルダー内に配置**しない**ことをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="2b420-238">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="2b420-239">*Views/Shared* は MVC ビュー パターンです。</span><span class="sxs-lookup"><span data-stu-id="2b420-239">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="2b420-240">Razor ページは、パス規則ではなく、フォルダー階層に依存することを意図しています。</span><span class="sxs-lookup"><span data-stu-id="2b420-240">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="2b420-241">Razor ページからのビュー検索には、*Pages* フォルダーが含まれます。</span><span class="sxs-lookup"><span data-stu-id="2b420-241">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="2b420-242">MVC コントローラーで使用しているレイアウト、テンプレート、およびパーシャルと、従来の Razor ビューは*機能します*。</span><span class="sxs-lookup"><span data-stu-id="2b420-242">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="2b420-243">*Pages/_ViewImports.cshtml* ファイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="2b420-243">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="2b420-244">`@namespace` はこのチュートリアルで後ほど説明します。</span><span class="sxs-lookup"><span data-stu-id="2b420-244">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="2b420-245">`@addTagHelper` ディレクティブにより、[組み込みタグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/Index)が *Pages* フォルダー内のすべてのページにもたらされます。</span><span class="sxs-lookup"><span data-stu-id="2b420-245">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="2b420-246">ページで `@namespace` ディレクティブが明示的に使用されている場合:</span><span class="sxs-lookup"><span data-stu-id="2b420-246">When the `@namespace` directive is used explicitly on a page:</span></span>

[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="2b420-247">ディレクティブは、ページの名前空間を設定します。</span><span class="sxs-lookup"><span data-stu-id="2b420-247">The directive sets the namespace for the page.</span></span> <span data-ttu-id="2b420-248">`@model` ディレクティブには、名前空間を含める必要はありません。</span><span class="sxs-lookup"><span data-stu-id="2b420-248">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="2b420-249">`@namespace` ディレクティブが *_ViewImports.cshtml* に含まれていると、指定した名前空間が `@namespace` ディレクティブをインポートするページで生成された名前空間のプレフィックスを提供します。</span><span class="sxs-lookup"><span data-stu-id="2b420-249">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="2b420-250">生成された名前空間の残りの部分 (サフィックスの部分) は、*_ViewImports.cshtml* を含むフォルダーとページを含むフォルダー間のドットで区切られた相対パスです。</span><span class="sxs-lookup"><span data-stu-id="2b420-250">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="2b420-251">たとえば、分離コード ファイル *Pages/Customers/Edit.cshtml.cs* は名前空間を明示的に設定します。</span><span class="sxs-lookup"><span data-stu-id="2b420-251">For example, the code behind file *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="2b420-252">*Pages/_ViewImports.cshtml* ファイルは次の名前空間を設定します。</span><span class="sxs-lookup"><span data-stu-id="2b420-252">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="2b420-253">*Pages/Customers/Edit.cshtml* Razor ページの生成された名前空間は、分離コード ファイルと同じです。</span><span class="sxs-lookup"><span data-stu-id="2b420-253">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the code behind file.</span></span> <span data-ttu-id="2b420-254">`@namespace` ディレクティブは、分離コード ファイルの `@using` ディレクティブを追加しなくても、プロジェクトに追加された C# クラスと、ページ生成したコードが*機能する*ように設計されました。</span><span class="sxs-lookup"><span data-stu-id="2b420-254">The `@namespace` directive was designed so the C# classes added to a project and pages-generated code *just work* without having to add an `@using` directive for the code behind file.</span></span>

<span data-ttu-id="2b420-255">**注:** `@namespace`は従来の Razor ビューでも機能します。</span><span class="sxs-lookup"><span data-stu-id="2b420-255">**Note:** `@namespace` also works with conventional Razor views.</span></span>

<span data-ttu-id="2b420-256">元の *Pages/Create.cshtml* ビュー ファイル:</span><span class="sxs-lookup"><span data-stu-id="2b420-256">The original *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

<span data-ttu-id="2b420-257">更新された *Pages/Create.cshtml* ビュー ファイル:</span><span class="sxs-lookup"><span data-stu-id="2b420-257">The updated *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

<span data-ttu-id="2b420-258">[Razor ページのスタート プロジェクト](#rpvs17)には、クライアント側の検証をフックする *Pages/_ValidationScriptsPartial.cshtml* が含まれています。</span><span class="sxs-lookup"><span data-stu-id="2b420-258">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="2b420-259">ページの URL の生成</span><span class="sxs-lookup"><span data-stu-id="2b420-259">URL generation for Pages</span></span>

<span data-ttu-id="2b420-260">上に示した `Create` ページでは、`RedirectToPage` を使用します。</span><span class="sxs-lookup"><span data-stu-id="2b420-260">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

<span data-ttu-id="2b420-261">アプリには次のファイル/フォルダー構造があります。</span><span class="sxs-lookup"><span data-stu-id="2b420-261">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="2b420-262">*/Pages*</span><span class="sxs-lookup"><span data-stu-id="2b420-262">*/Pages*</span></span>

  * <span data-ttu-id="2b420-263">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="2b420-263">*Index.cshtml*</span></span>
  * <span data-ttu-id="2b420-264">*/Customer*</span><span class="sxs-lookup"><span data-stu-id="2b420-264">*/Customer*</span></span>

    * <span data-ttu-id="2b420-265">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="2b420-265">*Create.cshtml*</span></span>
    * <span data-ttu-id="2b420-266">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="2b420-266">*Edit.cshtml*</span></span>
    * <span data-ttu-id="2b420-267">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="2b420-267">*Index.cshtml*</span></span>

<span data-ttu-id="2b420-268">成功すると、*Pages/Customers/Create.cshtml* ページと *Pages/Customers/Edit.cshtml* ページが *Pages/Index.cshtml* にリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="2b420-268">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="2b420-269">文字列 `/Index` は前のページにアクセスするための URI の一部です。</span><span class="sxs-lookup"><span data-stu-id="2b420-269">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="2b420-270">文字列 `/Index` は、*Pages/Index.cshtml* ページへの URI を生成するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="2b420-270">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="2b420-271">例:</span><span class="sxs-lookup"><span data-stu-id="2b420-271">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="2b420-272">ページ名は、ルート */Pages* フォルダーからページへのパスです (たとえば `/Index` の場合、先頭の `/` を含みます)。</span><span class="sxs-lookup"><span data-stu-id="2b420-272">The page name is the path to the page from the root */Pages* folder (including a leading `/`, for example `/Index`).</span></span> <span data-ttu-id="2b420-273">上記の URL の生成サンプルは、単に URL をハードコーディングするよりもはるかに多機能です。</span><span class="sxs-lookup"><span data-stu-id="2b420-273">The preceding URL generation samples are much more feature rich than just hardcoding a URL.</span></span> <span data-ttu-id="2b420-274">URL の生成は[ルーティング](xref:mvc/controllers/routing)を使用し、ターゲット パスで定義されたルート方法に従って、パラメーターの生成とエンコードができます。</span><span class="sxs-lookup"><span data-stu-id="2b420-274">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="2b420-275">ページの URL 生成は、相対名をサポートします。</span><span class="sxs-lookup"><span data-stu-id="2b420-275">URL generation for pages supports relative names.</span></span> <span data-ttu-id="2b420-276">次の表に、*Pages/Customers/Create.cshtml* の異なる `RedirectToPage` パラメーターで選択されたインデックス ページを示します。</span><span class="sxs-lookup"><span data-stu-id="2b420-276">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="2b420-277">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="2b420-277">RedirectToPage(x)</span></span>| <span data-ttu-id="2b420-278">ページ</span><span class="sxs-lookup"><span data-stu-id="2b420-278">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="2b420-279">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="2b420-279">RedirectToPage("/Index")</span></span> | <span data-ttu-id="2b420-280">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="2b420-280">*Pages/Index*</span></span> |
| <span data-ttu-id="2b420-281">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="2b420-281">RedirectToPage("./Index");</span></span> | <span data-ttu-id="2b420-282">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="2b420-282">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="2b420-283">RedirectToPage("../Index")</span><span class="sxs-lookup"><span data-stu-id="2b420-283">RedirectToPage("../Index")</span></span> | <span data-ttu-id="2b420-284">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="2b420-284">*Pages/Index*</span></span> |
| <span data-ttu-id="2b420-285">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="2b420-285">RedirectToPage("Index")</span></span>  | <span data-ttu-id="2b420-286">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="2b420-286">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="2b420-287">`RedirectToPage("Index")`、`RedirectToPage("./Index")`、および `RedirectToPage("../Index")` は*相対名*です。</span><span class="sxs-lookup"><span data-stu-id="2b420-287">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are *relative names*.</span></span> <span data-ttu-id="2b420-288">`RedirectToPage` パラメーターは現在のページのパスと*組み合わされて*、ターゲット ページの名前を計算します。</span><span class="sxs-lookup"><span data-stu-id="2b420-288">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page. -- page name, not page path -->

<span data-ttu-id="2b420-289">相対名のリンクは、複雑な構造を持つサイトを構築する際に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="2b420-289">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="2b420-290">相対名を使用してフォルダー内のページ間をリンクする場合、そのフォルダー名を変更することができます。</span><span class="sxs-lookup"><span data-stu-id="2b420-290">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="2b420-291">すべてのリンクは引き続き機能します (リンクにはフォルダー名が含まれていないため)。</span><span class="sxs-lookup"><span data-stu-id="2b420-291">All the links still work (because they didn't include the folder name).</span></span>

## <a name="tempdata"></a><span data-ttu-id="2b420-292">TempData</span><span class="sxs-lookup"><span data-stu-id="2b420-292">TempData</span></span>

<span data-ttu-id="2b420-293">ASP.NET Core は [コントローラー](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller)上で [TempData](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) プロパティを公開します。</span><span class="sxs-lookup"><span data-stu-id="2b420-293">ASP.NET Core exposes the [TempData](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="2b420-294">このプロパティは、読み取られるまでデータを格納します。</span><span class="sxs-lookup"><span data-stu-id="2b420-294">This property stores data until it is read.</span></span> <span data-ttu-id="2b420-295">`Keep` メソッドと `Peek` メソッドは、削除せずにデータを確認するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="2b420-295">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="2b420-296">`TempData` は、複数の要求にデータが必要な場合のリダイレクトに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="2b420-296">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="2b420-297">`[TempData]` は ASP.NET Core 2.0 の新しい属性で、コントローラーとページでサポートされています。</span><span class="sxs-lookup"><span data-stu-id="2b420-297">The `[TempData]` attribute is new in ASP.NET Core 2.0 and is supported on controllers and pages.</span></span>

<span data-ttu-id="2b420-298">次のコードは、`TempData` を使用して `Message` の値を設定します。</span><span class="sxs-lookup"><span data-stu-id="2b420-298">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="2b420-299">*Pages/Customers/Index.cshtml* ファイル内の次のマークアップは、`TempData` を使用して `Message` の値を表示します。</span><span class="sxs-lookup"><span data-stu-id="2b420-299">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="2b420-300">*Pages/Customers/Index.cshtml.cs* 分離コード ファイルは、`[TempData]` 属性を `Message` プロパティに適用します。</span><span class="sxs-lookup"><span data-stu-id="2b420-300">The *Pages/Customers/Index.cshtml.cs* code-behind file applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="2b420-301">詳細については、「[TempData](xref:fundamentals/app-state#temp)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="2b420-301">See [TempData](xref:fundamentals/app-state#temp) for more information.</span></span>

<a name="mhpp"></a>
## <a name="multiple-handlers-per-page"></a><span data-ttu-id="2b420-302">ページあたり複数のハンドラー</span><span class="sxs-lookup"><span data-stu-id="2b420-302">Multiple handlers per page</span></span>

<span data-ttu-id="2b420-303">次のページは、`asp-page-handler` タグ ヘルパーを使用して 2 つのページ ハンドラーにマークアップを生成します。</span><span class="sxs-lookup"><span data-stu-id="2b420-303">The following page generates markup for two page handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there is no `asp-` attribute   -->

<span data-ttu-id="2b420-304">前の例のフォームには、それぞれが `FormActionTagHelper` を使用して異なる URL に送信する 2 つの送信ボタンがあります。</span><span class="sxs-lookup"><span data-stu-id="2b420-304">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="2b420-305">`asp-page-handler` 属性は、`asp-page` のコンパニオンです。</span><span class="sxs-lookup"><span data-stu-id="2b420-305">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="2b420-306">`asp-page-handler` はページごとに定義されている各ハンドラー メソッドに送信する URL を生成します。</span><span class="sxs-lookup"><span data-stu-id="2b420-306">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="2b420-307">サンプルは現在のページにリンクしているため、`asp-page` は指定されません。</span><span class="sxs-lookup"><span data-stu-id="2b420-307">`asp-page` is not specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="2b420-308">分離コード ファイル:</span><span class="sxs-lookup"><span data-stu-id="2b420-308">The code-behind file:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="2b420-309">上記のコードは、*名前付きハンドラー メソッド*を使用しています。</span><span class="sxs-lookup"><span data-stu-id="2b420-309">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="2b420-310">名前付きハンドラー メソッドは、名前の `On<HTTP Verb>` の後および `Async` の前 (ある場合) のテキストを取得して作成されます。</span><span class="sxs-lookup"><span data-stu-id="2b420-310">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="2b420-311">前の例では、ページ メソッドは OnPost**JoinList**Async と OnPost**JoinListUC**Async です。</span><span class="sxs-lookup"><span data-stu-id="2b420-311">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="2b420-312">*OnPost* と *Async* を削除すると、ハンドラー名は `JoinList` と `JoinListUC` になります。</span><span class="sxs-lookup"><span data-stu-id="2b420-312">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="2b420-313">上記のコードを使用すると、`OnPostJoinListAsync` に送信される URL パスは `http://localhost:5000/Customers/CreateFATH?handler=JoinList` になります。</span><span class="sxs-lookup"><span data-stu-id="2b420-313">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="2b420-314">`OnPostJoinListUCAsync` に送信される URL パスは `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC` です。</span><span class="sxs-lookup"><span data-stu-id="2b420-314">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="customizing-routing"></a><span data-ttu-id="2b420-315">ルーティングのカスタマイズ</span><span class="sxs-lookup"><span data-stu-id="2b420-315">Customizing Routing</span></span>

<span data-ttu-id="2b420-316">URL 内のクエリ文字列 `?handler=JoinList` が気に入らない場合は、ルートを変更して URL のパス部分にハンドラー名を挿入することができます。</span><span class="sxs-lookup"><span data-stu-id="2b420-316">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="2b420-317">`@page` ディレクティブの後に二重引用符で囲んだルート テンプレートを追加して、ルートをカスタマイズすることができます。</span><span class="sxs-lookup"><span data-stu-id="2b420-317">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="2b420-318">上記のルートでは、クエリ文字列の代わりにハンドラー名を URL パスに挿入しています。</span><span class="sxs-lookup"><span data-stu-id="2b420-318">The preceding route puts the handler name in the URL path instead of the query string.</span></span> <span data-ttu-id="2b420-319">`handler` の後の `?` は、ルート パラメーターが省略可能なことを意味します。</span><span class="sxs-lookup"><span data-stu-id="2b420-319">The `?` following `handler` means the route parameter is optional.</span></span>

<span data-ttu-id="2b420-320">`@page` を使用して、ページのルートに追加のセグメントとパラメーターを追加できます。</span><span class="sxs-lookup"><span data-stu-id="2b420-320">You can use `@page` to add additional segments and parameters to a page's route.</span></span> <span data-ttu-id="2b420-321">ここにあるものは何でもページの既定のルートに**追加**されます。</span><span class="sxs-lookup"><span data-stu-id="2b420-321">Whatever's there is **appended** to the default route of the page.</span></span> <span data-ttu-id="2b420-322">絶対パスまたは仮想パスを使用してページのルート (`"~/Some/Other/Path"` など) を変更することはサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="2b420-322">Using an absolute or virtual path to change the page's route (like `"~/Some/Other/Path"`) is not supported.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="2b420-323">構成と設定</span><span class="sxs-lookup"><span data-stu-id="2b420-323">Configuration and settings</span></span>

<span data-ttu-id="2b420-324">高度なオプションを構成するには、MVC ビルダーで拡張メソッド `AddRazorPagesOptions` を使用します。</span><span class="sxs-lookup"><span data-stu-id="2b420-324">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

<span data-ttu-id="2b420-325">現在は、`RazorPagesOptions` を使用してページのルート ディレクトリを設定したり、ページのアプリケーション モデルの規則を追加したりすることができます。</span><span class="sxs-lookup"><span data-stu-id="2b420-325">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="2b420-326">将来、この方法でより多くの機能拡張を可能にしたいと考えています。</span><span class="sxs-lookup"><span data-stu-id="2b420-326">We'll enable more extensibility this way in the future.</span></span>

<span data-ttu-id="2b420-327">ビューをプリコンパイルするには、「[Razor view compilation](xref:mvc/views/view-compilation)」 (Razor ビュー コンパイル) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="2b420-327">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="2b420-328">[サンプル コードをダウンロードまたは表示します](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample)。</span><span class="sxs-lookup"><span data-stu-id="2b420-328">[Download or view sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span></span>

<span data-ttu-id="2b420-329">この概要に基づく、「[Getting started with Razor Pages in ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start)」 (ASP.NET Core での Razor ページの概要) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="2b420-329">See [Getting started with Razor Pages in ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="2b420-330">Razor ページをコンテンツのルートに指定する</span><span class="sxs-lookup"><span data-stu-id="2b420-330">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="2b420-331">Razor ページのルートは既定で */Pages* ディレクトリです。</span><span class="sxs-lookup"><span data-stu-id="2b420-331">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="2b420-332">Razor ページをアプリのコンテンツ ルート ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) に指定するには、[WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) を [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) に追加します。</span><span class="sxs-lookup"><span data-stu-id="2b420-332">Add [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at the content root ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) of the app:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="2b420-333">Razor ページをカスタム ルート ディレクトリに指定する</span><span class="sxs-lookup"><span data-stu-id="2b420-333">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="2b420-334">(相対パスを指定して) Razor ページをアプリのカスタム ルート ディレクトリに指定するには、[WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) を [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) に追加します。</span><span class="sxs-lookup"><span data-stu-id="2b420-334">Add [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="see-also"></a><span data-ttu-id="2b420-335">関連項目</span><span class="sxs-lookup"><span data-stu-id="2b420-335">See also</span></span>

* [<span data-ttu-id="2b420-336">Razor ページの概要</span><span class="sxs-lookup"><span data-stu-id="2b420-336">Getting started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="2b420-337">Razor ページの承認規則</span><span class="sxs-lookup"><span data-stu-id="2b420-337">Razor Pages authorization conventions</span></span>](xref:security/authorization/razor-pages-authorization)
* [<span data-ttu-id="2b420-338">Razor ページのカスタム ルートとページ モデル プロバイダー</span><span class="sxs-lookup"><span data-stu-id="2b420-338">Razor Pages custom route and page model providers</span></span>](xref:mvc/razor-pages/razor-pages-convention-features)
* [<span data-ttu-id="2b420-339">Razor ページの単体テストと統合テスト</span><span class="sxs-lookup"><span data-stu-id="2b420-339">Razor Pages unit and integration testing</span></span>](xref:testing/razor-pages-testing)
