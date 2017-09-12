---
title: "ASP.NET Core での Razor ページの概要"
author: Rick-Anderson
description: "ASP.NET Core での Razor ページの概要"
keywords: "ASP.NET Core、Razor ページ"
ms.author: riande
manager: wpickett
ms.date: 08/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/razor-pages/index
ms.openlocfilehash: 543399d99af127f943f7e9119fb5d84c8c5bc499
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/12/2017
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="48016-104">ASP.NET Core での Razor ページの概要</span><span class="sxs-lookup"><span data-stu-id="48016-104">Introduction to Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="48016-105">[Rick Anderson](https://twitter.com/RickAndMSFT) および [Ryan Nowak](https://github.com/rynowak) 著</span><span class="sxs-lookup"><span data-stu-id="48016-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="48016-106">Razor ページは、ページ コーディングに重点を置いたシナリオをより簡略化し、生産性を高める ASP.NET Core MVC の新機能です。</span><span class="sxs-lookup"><span data-stu-id="48016-106">Razor Pages is a new feature of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="48016-107">モデル ビュー コントローラーのアプローチを使用するチュートリアルをお探しの場合は、「[Getting started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc)」 (ASP.NET Core MVC の概要) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="48016-107">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Getting started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<a name="prerequisites"></a>

## <a name="aspnet-core-20-prerequisites"></a><span data-ttu-id="48016-108">ASP.NET Core 2.0 の前提条件</span><span class="sxs-lookup"><span data-stu-id="48016-108">ASP.NET Core 2.0 prerequisites</span></span>

<span data-ttu-id="48016-109">[.NET Core](https://www.microsoft.com/net/core) 2.0.0 以降をインストールします。</span><span class="sxs-lookup"><span data-stu-id="48016-109">Install [.NET Core](https://www.microsoft.com/net/core) 2.0.0 or later.</span></span>

<span data-ttu-id="48016-110">Visual Studio を使用している場合は、以下のワークロードで [Visual Studio](https://www.visualstudio.com/vs/) 15.3 以降をインストールします。</span><span class="sxs-lookup"><span data-stu-id="48016-110">If you're using Visual Studio, install [Visual Studio](https://www.visualstudio.com/vs/) 15.3 or later with the following workloads:</span></span>

* <span data-ttu-id="48016-111">**ASP.NET と Web 開発**</span><span class="sxs-lookup"><span data-stu-id="48016-111">**ASP.NET and web development**</span></span>
* <span data-ttu-id="48016-112">**.NET Core クロスプラットフォームの開発**</span><span class="sxs-lookup"><span data-stu-id="48016-112">**.NET Core cross-platform development**</span></span>

<a name="rpvs17"></a>

## <a name="creating-a-razor-pages-project"></a><span data-ttu-id="48016-113">Razor ページ プロジェクトの作成</span><span class="sxs-lookup"><span data-stu-id="48016-113">Creating a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="48016-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="48016-114">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="48016-115">Visual Studio を使用して Razor ページ プロジェクトを作成する詳細な手順については、「[Getting started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start)」 (Razor ページの概要) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="48016-115">See [Getting started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project using Visual Studio.</span></span>

#   <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="48016-116">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="48016-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="48016-117">コマンド ラインから `dotnet new razor` を実行します。</span><span class="sxs-lookup"><span data-stu-id="48016-117">Run `dotnet new razor` from the command line.</span></span>

<span data-ttu-id="48016-118">Visual Studio for Mac から生成された *.csproj* ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="48016-118">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="48016-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="48016-119">Visual Studio Code</span></span>](#tab/visual-studio-code) 

<span data-ttu-id="48016-120">コマンド ラインから `dotnet new razor` を実行します。</span><span class="sxs-lookup"><span data-stu-id="48016-120">Run `dotnet new razor` from the command line.</span></span>

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="48016-121">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="48016-121">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="48016-122">コマンド ラインから `dotnet new razor` を実行します。</span><span class="sxs-lookup"><span data-stu-id="48016-122">Run `dotnet new razor` from the command line.</span></span>

---

## <a name="razor-pages"></a><span data-ttu-id="48016-123">Razor ページ</span><span class="sxs-lookup"><span data-stu-id="48016-123">Razor Pages</span></span>

<span data-ttu-id="48016-124">Razor ページは *Startup.cs* で有効になっています。</span><span class="sxs-lookup"><span data-stu-id="48016-124">Razor Pages is enabled in *Startup.cs*:</span></span>

<span data-ttu-id="48016-125">[!code-cs[main](index/sample/RazorPagesIntro/Startup.cs?name=Startup)]</span><span class="sxs-lookup"><span data-stu-id="48016-125">[!code-cs[main](index/sample/RazorPagesIntro/Startup.cs?name=Startup)]</span></span>

<span data-ttu-id="48016-126">基本ページを検討します。<a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="48016-126">Consider a basic page: <a name="OnGet"></a></span></span>

<span data-ttu-id="48016-127">[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Index.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="48016-127">[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Index.cshtml)]</span></span>

<span data-ttu-id="48016-128">上記のコードは Razor ビュー ファイルに非常によく似ています。</span><span class="sxs-lookup"><span data-stu-id="48016-128">The preceding code looks a lot like a Razor view file.</span></span> <span data-ttu-id="48016-129">違いは、`@page` ディレクティブにあります。</span><span class="sxs-lookup"><span data-stu-id="48016-129">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="48016-130">`@page` はファイルを MVC アクションにします。つまり、コントローラーを経由せずに要求を直接処理します。</span><span class="sxs-lookup"><span data-stu-id="48016-130">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="48016-131">`@page` はページで最初の Razor ディレクティブである必要があります。</span><span class="sxs-lookup"><span data-stu-id="48016-131">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="48016-132">`@page` はその他の Razor コンストラクトの動作に影響します。</span><span class="sxs-lookup"><span data-stu-id="48016-132">`@page` affects the behavior of other Razor constructs.</span></span> <span data-ttu-id="48016-133">[@functions](xref:mvc/views/razor#functions) ディレクティブは、関数レベルのコンテンツを有効にします。</span><span class="sxs-lookup"><span data-stu-id="48016-133">The [@functions](xref:mvc/views/razor#functions) directive enables function-level content.</span></span>

<span data-ttu-id="48016-134">次の 2 つのファイルは、別のファイルで `PageModel` を使用した同様のページを示しています。</span><span class="sxs-lookup"><span data-stu-id="48016-134">A similar page, with the `PageModel` in a separate file, is shown in the following two files.</span></span> <span data-ttu-id="48016-135">*Pages/Index2.cshtml* ファイル:</span><span class="sxs-lookup"><span data-stu-id="48016-135">The *Pages/Index2.cshtml* file:</span></span>

<span data-ttu-id="48016-136">[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="48016-136">[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]</span></span>

<span data-ttu-id="48016-137">*Pages/Index2.cshtml.cs* '分離コード' ファイル:</span><span class="sxs-lookup"><span data-stu-id="48016-137">The *Pages/Index2.cshtml.cs* 'code-behind' file:</span></span>

<span data-ttu-id="48016-138">[!code-cs[main](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]</span><span class="sxs-lookup"><span data-stu-id="48016-138">[!code-cs[main](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]</span></span>

<span data-ttu-id="48016-139">規則により、`PageModel` クラス ファイルは、Razor ページ ファイルと同じ名前に *.cs* が付加された名前になります。</span><span class="sxs-lookup"><span data-stu-id="48016-139">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="48016-140">たとえば、上の Razor ページは *Pages/Index2.cshtml* になります。</span><span class="sxs-lookup"><span data-stu-id="48016-140">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="48016-141">`PageModel` クラスを含むファイル名は、*Pages/Index2.cshtml.cs* になります。</span><span class="sxs-lookup"><span data-stu-id="48016-141">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="48016-142">単純なページの場合、`PageModel` クラスと Razor マークアップが混在していても問題ありません。</span><span class="sxs-lookup"><span data-stu-id="48016-142">For simple pages, mixing the `PageModel` class with the Razor markup is fine.</span></span> <span data-ttu-id="48016-143">より複雑なコードの場合は、ページ モデル コードを別々に保持するのがベスト プラクティスです。</span><span class="sxs-lookup"><span data-stu-id="48016-143">For more complex code, it's a best practice to keep the page model code separate.</span></span>

<span data-ttu-id="48016-144">URL パスのページへの関連付けは、ファイル システム内のページの場所によって決定されます。</span><span class="sxs-lookup"><span data-stu-id="48016-144">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="48016-145">次の表に、Razor ページ パスと一致 URL を示します。</span><span class="sxs-lookup"><span data-stu-id="48016-145">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="48016-146">ファイル名とパス</span><span class="sxs-lookup"><span data-stu-id="48016-146">File name and path</span></span>               | <span data-ttu-id="48016-147">一致 URL</span><span class="sxs-lookup"><span data-stu-id="48016-147">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="48016-148">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="48016-148">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="48016-149">`/` または `/Index`</span><span class="sxs-lookup"><span data-stu-id="48016-149">`/` or `/Index`</span></span> |
| <span data-ttu-id="48016-150">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="48016-150">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="48016-151">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="48016-151">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="48016-152">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="48016-152">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="48016-153">`/Store` または `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="48016-153">`/Store` or `/Store/Index`</span></span>  |

<span data-ttu-id="48016-154">メモ:</span><span class="sxs-lookup"><span data-stu-id="48016-154">Notes:</span></span>

* <span data-ttu-id="48016-155">既定では、ランタイムが *Pages* フォルダー内で Razor ページ ファイルを検索します。</span><span class="sxs-lookup"><span data-stu-id="48016-155">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="48016-156">`Index` は、URL にページが含まれない場合の既定のページになります。</span><span class="sxs-lookup"><span data-stu-id="48016-156">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="writing-a-basic-form"></a><span data-ttu-id="48016-157">基本フォームの作成</span><span class="sxs-lookup"><span data-stu-id="48016-157">Writing a basic form</span></span>

<span data-ttu-id="48016-158">Razor ページ機能は、Web ブラウザーで使用される一般的なパターンを簡単にするために設計されています。</span><span class="sxs-lookup"><span data-stu-id="48016-158">Razor Pages features are designed to make common patterns used with web browsers easy.</span></span> <span data-ttu-id="48016-159">[モデル バインド](xref:mvc/models/model-binding)、[タグ ヘルパー](xref:mvc/views/tag-helpers/intro)、および HTML ヘルパーはすべて、Razor ページ クラスで定義されたプロパティで*機能します*。</span><span class="sxs-lookup"><span data-stu-id="48016-159">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="48016-160">`Contact` モデルの基本的な "お問い合わせ" フォームを実装するページを考察します。</span><span class="sxs-lookup"><span data-stu-id="48016-160">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="48016-161">このドキュメントのサンプルでは、[Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) ファイルで `DbContext` が初期化されます。</span><span class="sxs-lookup"><span data-stu-id="48016-161">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

<span data-ttu-id="48016-162">[!code-cs[main](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]</span><span class="sxs-lookup"><span data-stu-id="48016-162">[!code-cs[main](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]</span></span>

<span data-ttu-id="48016-163">データ モデル:</span><span class="sxs-lookup"><span data-stu-id="48016-163">The data model:</span></span>

<span data-ttu-id="48016-164">[!code-cs[main](index/sample/RazorPagesContacts/Data/Customer.cs)]</span><span class="sxs-lookup"><span data-stu-id="48016-164">[!code-cs[main](index/sample/RazorPagesContacts/Data/Customer.cs)]</span></span>

<span data-ttu-id="48016-165">*Pages/Create.cshtml* ビュー ファイル:</span><span class="sxs-lookup"><span data-stu-id="48016-165">The *Pages/Create.cshtml* view file:</span></span>

<span data-ttu-id="48016-166">[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Create.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="48016-166">[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Create.cshtml)]</span></span>

<span data-ttu-id="48016-167">ビューの *Pages/Create.cshtml.cs* 分離コード ファイル:</span><span class="sxs-lookup"><span data-stu-id="48016-167">The *Pages/Create.cshtml.cs* code-behind file for the view:</span></span>

<span data-ttu-id="48016-168">[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=ALL)]</span><span class="sxs-lookup"><span data-stu-id="48016-168">[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=ALL)]</span></span>

<span data-ttu-id="48016-169">規則により、`PageModel` クラスは `<PageName>Model` と呼ばれ、ページと同じ名前空間にあります。</span><span class="sxs-lookup"><span data-stu-id="48016-169">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span> <span data-ttu-id="48016-170">`@functions` を使用してページから変換し、`PageModel` クラスを使用してハンドラーとページを変換するには、それほど多くの変更は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="48016-170">Not much change is needed to convert from a page using `@functions` to define handlers and a page using a `PageModel` class.</span></span>

<span data-ttu-id="48016-171">`PageModel` 分離コード ファイルを使用すると単体テストがサポートされますが、明示的なコンストラクターとクラスを記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="48016-171">Using a `PageModel` code-behind file supports unit testing, but requires you to write an explicit constructor and class.</span></span> <span data-ttu-id="48016-172">`PageModel` 分離コード ファイルのないページは、ランタイムのコンパイルをサポートします。これは開発で有利になる場合があります。</span><span class="sxs-lookup"><span data-stu-id="48016-172">Pages without `PageModel` code-behind files support runtime compilation, which can be an advantage in development.</span></span>  <!-- review: advantage because you can make changes and refresh the browser without explicitly compiling the app -->

<span data-ttu-id="48016-173">このページには、(ユーザーがフォームを投稿したときに) `POST` 要求で実行される `OnPostAsync` *ハンドラー メソッド*があります。</span><span class="sxs-lookup"><span data-stu-id="48016-173">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="48016-174">任意の HTTP 動詞のハンドラー メソッドを追加できます。</span><span class="sxs-lookup"><span data-stu-id="48016-174">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="48016-175">最も一般的なハンドラーは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="48016-175">The most common handlers are:</span></span>

* <span data-ttu-id="48016-176">ページに必要な状態を初期化するための `OnGet`。</span><span class="sxs-lookup"><span data-stu-id="48016-176">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="48016-177">[OnGet](#OnGet) サンプル。</span><span class="sxs-lookup"><span data-stu-id="48016-177">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="48016-178">フォームの送信を処理するための `OnPost`。</span><span class="sxs-lookup"><span data-stu-id="48016-178">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="48016-179">`Async` 名前付けサフィックスは省略可能ですが、非同期関数の規則でよく使用されます。</span><span class="sxs-lookup"><span data-stu-id="48016-179">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="48016-180">上記の例の `OnPostAsync` コードは、コントローラーで通常に記述するものに似ています。</span><span class="sxs-lookup"><span data-stu-id="48016-180">The `OnPostAsync` code in the preceding example looks similar to what you would normally write in a controller.</span></span> <span data-ttu-id="48016-181">上記のコードは、Razor ページでは一般的です。</span><span class="sxs-lookup"><span data-stu-id="48016-181">The preceding code is typical for Razor Pages.</span></span> <span data-ttu-id="48016-182">[モデル バインド](xref:mvc/models/model-binding)、[検証](xref:mvc/models/validation)、およびアクションの結果などのほとんどの MVC プリミティブは共有されます。</span><span class="sxs-lookup"><span data-stu-id="48016-182">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results are shared.</span></span>  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

<span data-ttu-id="48016-183">上記の `OnPostAsync` メソッド:</span><span class="sxs-lookup"><span data-stu-id="48016-183">The previous `OnPostAsync` method:</span></span>

<span data-ttu-id="48016-184">[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=OnPostAsync)]</span><span class="sxs-lookup"><span data-stu-id="48016-184">[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=OnPostAsync)]</span></span>

<span data-ttu-id="48016-185">`OnPostAsync` の基本的な流れは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="48016-185">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="48016-186">検証エラーを確認します。</span><span class="sxs-lookup"><span data-stu-id="48016-186">Check for validation errors.</span></span>

*  <span data-ttu-id="48016-187">エラーがない場合は、データを保存し、リダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="48016-187">If there are no errors, save the data and redirect.</span></span>
*  <span data-ttu-id="48016-188">エラーがある場合は、検証メッセージとともにページをもう一度表示します。</span><span class="sxs-lookup"><span data-stu-id="48016-188">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="48016-189">クライアント側の検証は、従来の ASP.NET Core MVC アプリケーションと同じです。</span><span class="sxs-lookup"><span data-stu-id="48016-189">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="48016-190">多くの場合、検証エラーはクライアントで検出され、サーバーには送信されません。</span><span class="sxs-lookup"><span data-stu-id="48016-190">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="48016-191">データが正常に入力されると、`OnPostAsync` ハンドラー メソッドが `RedirectToPage` ヘルパー メソッドを呼び出して `RedirectToPageResult` のインスタンスを返します。</span><span class="sxs-lookup"><span data-stu-id="48016-191">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="48016-192">`RedirectToPage` は、`RedirectToAction` や `RedirectToRoute` と同じような新しいアクション結果ですが、ページ用にカスタマイズされています。</span><span class="sxs-lookup"><span data-stu-id="48016-192">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="48016-193">上記のサンプルでは、ルート インデックス ページ (`/Index`) にリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="48016-193">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="48016-194">`RedirectToPage` については、「[ページの URL の生成](#url_gen)」セクションで詳しく説明されています。</span><span class="sxs-lookup"><span data-stu-id="48016-194">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="48016-195">送信されたフォームに検証エラー (サーバーに渡される) があると、`OnPostAsync` ハンドラー メソッドが `Page` ヘルパー メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="48016-195">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="48016-196">`Page` は `PageResult` のインスタンスを返します。</span><span class="sxs-lookup"><span data-stu-id="48016-196">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="48016-197">`Page` を返すのは、コントローラーのアクションが `View` を返す方法に似ています。</span><span class="sxs-lookup"><span data-stu-id="48016-197">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="48016-198">`PageResult` はハンドラー メソッドの既定の<!-- Review  -->戻り値の型です。</span><span class="sxs-lookup"><span data-stu-id="48016-198">`PageResult` is the default <!-- Review  --> return type for a handler method.</span></span> <span data-ttu-id="48016-199">`void` を返すハンドラー メソッドがページをレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="48016-199">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="48016-200">`Customer` プロパティは `[BindProperty]` 属性を使用してモデル バインドにオプトインします。</span><span class="sxs-lookup"><span data-stu-id="48016-200">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

<span data-ttu-id="48016-201">[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=PageModel&highlight=10-11)]</span><span class="sxs-lookup"><span data-stu-id="48016-201">[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=PageModel&highlight=10-11)]</span></span>

<span data-ttu-id="48016-202">既定では、Razor ページはプロパティを非 GET 動詞とのみバインドします。</span><span class="sxs-lookup"><span data-stu-id="48016-202">Razor Pages, by default, bind properties only with non-GET verbs.</span></span> <span data-ttu-id="48016-203">プロパティをバインドすることで、記述すべきコードの量を削減できます。</span><span class="sxs-lookup"><span data-stu-id="48016-203">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="48016-204">同じプロパティを使用してバインドすることでコードを減らし、フィールド (`<input asp-for="Customer.Name" />`) からレンダリングして入力を受け入れます。</span><span class="sxs-lookup"><span data-stu-id="48016-204">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name" />`) and accept the input.</span></span>

<span data-ttu-id="48016-205">次のコードは、ページの作成の結合したバージョンを示しています。</span><span class="sxs-lookup"><span data-stu-id="48016-205">The following code shows the combined version of the create page:</span></span>

<span data-ttu-id="48016-206">[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/CreateCombined.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="48016-206">[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/CreateCombined.cshtml)]</span></span>

<span data-ttu-id="48016-207">`@model` を使用するのではなく、ページの新機能を利用しています。</span><span class="sxs-lookup"><span data-stu-id="48016-207">Rather than using `@model`, we're taking advantage of a new feature for Pages.</span></span> <span data-ttu-id="48016-208">既定では、生成された `Page` 派生クラス*は*モデルです。</span><span class="sxs-lookup"><span data-stu-id="48016-208">By default, the generated `Page`-derived class *is* the model.</span></span> <span data-ttu-id="48016-209">Razor ビューで*ビュー モデル*を使用することがベスト プラクティスです。</span><span class="sxs-lookup"><span data-stu-id="48016-209">Using a *view model* with Razor views is a best practice.</span></span> <span data-ttu-id="48016-210">ページでは、ビュー モデルが*自動的に*取得されます。</span><span class="sxs-lookup"><span data-stu-id="48016-210">With Pages, you get a view model *automatically*.</span></span>

<span data-ttu-id="48016-211">主な変更点は、コンストラクターの挿入を挿入された (`@inject`) プロパティと置き換えることです。</span><span class="sxs-lookup"><span data-stu-id="48016-211">The main change is replacing constructor injection with injected (`@inject`) properties.</span></span> <span data-ttu-id="48016-212">このページは[コンストラクターの依存性の注入](xref:mvc/controllers/dependency-injection#constructor-injection)に [@inject](xref:mvc/views/razor#inject) を使用します。</span><span class="sxs-lookup"><span data-stu-id="48016-212">This page uses [@inject](xref:mvc/views/razor#inject) for [constructor dependency injection](xref:mvc/controllers/dependency-injection#constructor-injection).</span></span> <span data-ttu-id="48016-213">`@inject` ステートメントは `OnPostAsync` で使用される `Db` プロパティを生成し、初期化します。</span><span class="sxs-lookup"><span data-stu-id="48016-213">The `@inject` statement generates and initializes the `Db` property that is used in `OnPostAsync`.</span></span> <span data-ttu-id="48016-214">挿入された (`@inject`) プロパティは、ハンドラー メソッドの実行前に設定されます。</span><span class="sxs-lookup"><span data-stu-id="48016-214">Injected (`@inject`) properties are set before handler methods run.</span></span>


<span data-ttu-id="48016-215">ホーム ページ (*Index.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="48016-215">The home page (*Index.cshtml*):</span></span>

<span data-ttu-id="48016-216">[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="48016-216">[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml)]</span></span>

<span data-ttu-id="48016-217">分離コード *Index.cshtml.cs* ファイル:</span><span class="sxs-lookup"><span data-stu-id="48016-217">The code behind *Index.cshtml.cs* file:</span></span>

<span data-ttu-id="48016-218">[!code-cs[main](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]</span><span class="sxs-lookup"><span data-stu-id="48016-218">[!code-cs[main](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]</span></span>

<span data-ttu-id="48016-219">*Index.cshtml* ファイルには、各連絡先の編集リンクを作成するために次のマークアップが含まれています。</span><span class="sxs-lookup"><span data-stu-id="48016-219">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

```html
<a asp-page="./Edit" asp-route-id="@contact.Id">edit</a>
```

<span data-ttu-id="48016-220">[アンカー タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/AnchorTagHelper)は [asp-route-{value}](xref:mvc/views/tag-helpers/builtin-th/AnchorTagHelper#route) 属性を使用して編集ページへのリンクを生成しました。</span><span class="sxs-lookup"><span data-stu-id="48016-220">The [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/AnchorTagHelper) used the [asp-route-{value}](xref:mvc/views/tag-helpers/builtin-th/AnchorTagHelper#route) attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="48016-221">リンクには、連絡先 ID とともにルート データが含まれています。</span><span class="sxs-lookup"><span data-stu-id="48016-221">The link contains route data with the contact ID.</span></span> <span data-ttu-id="48016-222">たとえば、`http://localhost:5000/Edit/1` のようにします。</span><span class="sxs-lookup"><span data-stu-id="48016-222">For example, `http://localhost:5000/Edit/1`.</span></span>

<span data-ttu-id="48016-223">*Pages/Edit.cshtml* ファイル:</span><span class="sxs-lookup"><span data-stu-id="48016-223">The *Pages/Edit.cshtml* file:</span></span>

<span data-ttu-id="48016-224">[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]</span><span class="sxs-lookup"><span data-stu-id="48016-224">[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]</span></span>

<span data-ttu-id="48016-225">最初の行には `@page "{id:int}"` ディレクティブが含まれています。</span><span class="sxs-lookup"><span data-stu-id="48016-225">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="48016-226">ルーティングの制約 `"{id:int}"` は、`int` ルート データを含むページへの要求を受け入れるようにページに指示します。</span><span class="sxs-lookup"><span data-stu-id="48016-226">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="48016-227">ページへの要求に `int` に変換できるルート データが含まれていない場合は、ランタイムで HTTP 404 (見つかりません) エラーが返されます。</span><span class="sxs-lookup"><span data-stu-id="48016-227">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span>

<span data-ttu-id="48016-228">*Pages/Edit.cshtml.cs* ファイル:</span><span class="sxs-lookup"><span data-stu-id="48016-228">The *Pages/Edit.cshtml.cs* file:</span></span>

<span data-ttu-id="48016-229">[!code-cs[main](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]</span><span class="sxs-lookup"><span data-stu-id="48016-229">[!code-cs[main](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]</span></span>

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="48016-230">XSRF/CSRF と Razor ページ</span><span class="sxs-lookup"><span data-stu-id="48016-230">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="48016-231">[偽造防止検証](xref:security/anti-request-forgery)のためにコードを記述する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="48016-231">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="48016-232">偽造防止トークンの生成と検証は、自動的に Razor ページに含まれます。</span><span class="sxs-lookup"><span data-stu-id="48016-232">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>
## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="48016-233">Razor ページでのレイアウト、パーシャル、テンプレート、およびタグ ヘルパーの使用</span><span class="sxs-lookup"><span data-stu-id="48016-233">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="48016-234">ページは、Razor ビュー エンジンのすべての機能で機能します。</span><span class="sxs-lookup"><span data-stu-id="48016-234">Pages work with all the features of the Razor view engine.</span></span> <span data-ttu-id="48016-235">レイアウト、パーシャル、テンプレート、タグ ヘルパー、*_ViewStart.cshtml*、*_ViewImports.cshtml* は、従来の Razor ビューと同じように動作します。</span><span class="sxs-lookup"><span data-stu-id="48016-235">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="48016-236">これらの機能の一部を利用してこのページをまとめてみましょう。</span><span class="sxs-lookup"><span data-stu-id="48016-236">Let's declutter this page by taking advantage of some of those features.</span></span>

<span data-ttu-id="48016-237">[レイアウト ページ](xref:mvc/views/layout)を *Pages/_Layout.cshtml* に追加します。</span><span class="sxs-lookup"><span data-stu-id="48016-237">Add a [layout page](xref:mvc/views/layout) to *Pages/_Layout.cshtml*:</span></span>

<span data-ttu-id="48016-238">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="48016-238">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]</span></span>

<span data-ttu-id="48016-239">[レイアウト](xref:mvc/views/layout)は次のことを行います。</span><span class="sxs-lookup"><span data-stu-id="48016-239">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="48016-240">(ページでレイアウトを止めない限り) 各ページのレイアウトを制御します。</span><span class="sxs-lookup"><span data-stu-id="48016-240">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="48016-241">JavaScript やスタイルシートなどの HTML 構造をインポートします。</span><span class="sxs-lookup"><span data-stu-id="48016-241">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="48016-242">詳細については、[レイアウトのページ](xref:mvc/views/layout)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="48016-242">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="48016-243">[Layout](xref:mvc/views/layout#specifying-a-layout) プロパティは *Pages/_ViewStart.cshtml* で設定されています。</span><span class="sxs-lookup"><span data-stu-id="48016-243">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

<span data-ttu-id="48016-244">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="48016-244">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]</span></span>

<span data-ttu-id="48016-245">注: レイアウトは、*Pages* フォルダー内にあります。</span><span class="sxs-lookup"><span data-stu-id="48016-245">Note: The layout is in the *Pages* folder.</span></span> <span data-ttu-id="48016-246">ページは現在のページと同じフォルダーから開始して、階層的に他のビュー (レイアウト、テンプレート、パーシャル) を検索します。</span><span class="sxs-lookup"><span data-stu-id="48016-246">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="48016-247">*Pages* フォルダー内のレイアウトは、*Pages* フォルダー配下の任意の Razor ページから使用できます。</span><span class="sxs-lookup"><span data-stu-id="48016-247">A layout in the *Pages* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="48016-248">レイアウト ファイルを *Views/Shared* フォルダー内に配置**しない**ことをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="48016-248">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="48016-249">*Views/Shared* は MVC ビュー パターンです。</span><span class="sxs-lookup"><span data-stu-id="48016-249">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="48016-250">Razor ページは、パス規則ではなく、フォルダー階層に依存することを意図しています。</span><span class="sxs-lookup"><span data-stu-id="48016-250">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="48016-251">Razor ページからのビュー検索には、*Pages* フォルダーが含まれます。</span><span class="sxs-lookup"><span data-stu-id="48016-251">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="48016-252">MVC コントローラーで使用しているレイアウト、テンプレート、およびパーシャルと、従来の Razor ビューは*機能します*。</span><span class="sxs-lookup"><span data-stu-id="48016-252">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="48016-253">*Pages/_ViewImports.cshtml* ファイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="48016-253">Add a *Pages/_ViewImports.cshtml* file:</span></span>

<span data-ttu-id="48016-254">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="48016-254">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]</span></span>

<span data-ttu-id="48016-255">`@namespace` はこのチュートリアルで後ほど説明します。</span><span class="sxs-lookup"><span data-stu-id="48016-255">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="48016-256">`@addTagHelper` ディレクティブにより、[組み込みタグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/Index)が *Pages* フォルダー内のすべてのページにもたらされます。</span><span class="sxs-lookup"><span data-stu-id="48016-256">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="48016-257">ページで `@namespace` ディレクティブが明示的に使用されている場合:</span><span class="sxs-lookup"><span data-stu-id="48016-257">When the `@namespace` directive is used explicitly on a page:</span></span>

<span data-ttu-id="48016-258">[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="48016-258">[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]</span></span>

<span data-ttu-id="48016-259">ディレクティブは、ページの名前空間を設定します。</span><span class="sxs-lookup"><span data-stu-id="48016-259">The directive sets the namespace for the page.</span></span> <span data-ttu-id="48016-260">`@model` ディレクティブには、名前空間を含める必要はありません。</span><span class="sxs-lookup"><span data-stu-id="48016-260">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="48016-261">`@namespace` ディレクティブが *_ViewImports.cshtml* に含まれていると、指定した名前空間が `@namespace` ディレクティブをインポートするページで生成された名前空間のプレフィックスを提供します。</span><span class="sxs-lookup"><span data-stu-id="48016-261">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="48016-262">生成された名前空間の残りの部分 (サフィックスの部分) は、*_ViewImports.cshtml* を含むフォルダーとページを含むフォルダー間のドットで区切られた相対パスです。</span><span class="sxs-lookup"><span data-stu-id="48016-262">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="48016-263">たとえば、分離コード ファイル *Pages/Customers/Edit.cshtml.cs* は名前空間を明示的に設定します。</span><span class="sxs-lookup"><span data-stu-id="48016-263">For example, the code behind file *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

<span data-ttu-id="48016-264">[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=namespace)]</span><span class="sxs-lookup"><span data-stu-id="48016-264">[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=namespace)]</span></span>

<span data-ttu-id="48016-265">*Pages/_ViewImports.cshtml* ファイルは次の名前空間を設定します。</span><span class="sxs-lookup"><span data-stu-id="48016-265">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

<span data-ttu-id="48016-266">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]</span><span class="sxs-lookup"><span data-stu-id="48016-266">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]</span></span>

<span data-ttu-id="48016-267">*Pages/Customers/Edit.cshtml* Razor ページの生成された名前空間は、分離コード ファイルと同じです。</span><span class="sxs-lookup"><span data-stu-id="48016-267">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the code behind file.</span></span> <span data-ttu-id="48016-268">`@namespace` ディレクティブは、分離コード ファイルの `@using` ディレクティブを追加しなくても、プロジェクトに追加された C# クラスと、ページ生成したコードが*機能する*ように設計されました。</span><span class="sxs-lookup"><span data-stu-id="48016-268">The `@namespace` directive was designed so the C# classes added to a project and pages-generated code *just work* without having to add an `@using` directive for the code behind file.</span></span>

<span data-ttu-id="48016-269">注: `@namespace` は従来の Razor ビューでも機能します。</span><span class="sxs-lookup"><span data-stu-id="48016-269">Note: `@namespace` also works with conventional Razor views.</span></span>

<span data-ttu-id="48016-270">元の *Pages/Create.cshtml* ビュー ファイル:</span><span class="sxs-lookup"><span data-stu-id="48016-270">The original *Pages/Create.cshtml* view file:</span></span>

<span data-ttu-id="48016-271">[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="48016-271">[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]</span></span>

<span data-ttu-id="48016-272">更新されたページ:</span><span class="sxs-lookup"><span data-stu-id="48016-272">The updated page:</span></span>

<span data-ttu-id="48016-273">*Pages/Create.cshtml* ビュー ファイル:</span><span class="sxs-lookup"><span data-stu-id="48016-273">The *Pages/Create.cshtml* view file:</span></span>

<span data-ttu-id="48016-274">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="48016-274">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]</span></span>

<span data-ttu-id="48016-275">[Razor ページのスタート プロジェクト](#rpvs17)には、クライアント側の検証をフックする *Pages/_ValidationScriptsPartial.cshtml* が含まれています。</span><span class="sxs-lookup"><span data-stu-id="48016-275">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="48016-276">ページの URL の生成</span><span class="sxs-lookup"><span data-stu-id="48016-276">URL generation for Pages</span></span>

<span data-ttu-id="48016-277">上に示した `Create` ページでは、`RedirectToPage` を使用します。</span><span class="sxs-lookup"><span data-stu-id="48016-277">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

<span data-ttu-id="48016-278">[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=OnPostAsync&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="48016-278">[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=OnPostAsync&highlight=10)]</span></span>

<span data-ttu-id="48016-279">アプリには次のファイル/フォルダー構造があります。</span><span class="sxs-lookup"><span data-stu-id="48016-279">The app has the following file/folder structure</span></span>

* <span data-ttu-id="48016-280">*/Pages*</span><span class="sxs-lookup"><span data-stu-id="48016-280">*/Pages*</span></span>

  * <span data-ttu-id="48016-281">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="48016-281">*Index.cshtml*</span></span>
  * <span data-ttu-id="48016-282">*/Customer*</span><span class="sxs-lookup"><span data-stu-id="48016-282">*/Customer*</span></span>

    * <span data-ttu-id="48016-283">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="48016-283">*Create.cshtml*</span></span>
    * <span data-ttu-id="48016-284">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="48016-284">*Edit.cshtml*</span></span>
    * <span data-ttu-id="48016-285">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="48016-285">*Index.cshtml*</span></span>

<span data-ttu-id="48016-286">成功すると、*Pages/Customers/Create.cshtml* ページと *Pages/Customers/Edit.cshtml* ページが *Pages/Index.cshtml* にリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="48016-286">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="48016-287">文字列 `/Index` は前のページにアクセスするための URI の一部です。</span><span class="sxs-lookup"><span data-stu-id="48016-287">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="48016-288">文字列 `/Index` は、*Pages/Index.cshtml* ページへの URI を生成するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="48016-288">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="48016-289">例:</span><span class="sxs-lookup"><span data-stu-id="48016-289">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="48016-290">ページ名は、ルート */Pages* フォルダーからページへのパスです (たとえば `/Index` の場合、先頭の `/` を含みます)。</span><span class="sxs-lookup"><span data-stu-id="48016-290">The page name is the path to the page from the root */Pages* folder (including a leading `/`, for example `/Index`).</span></span> <span data-ttu-id="48016-291">上記の URL の生成サンプルは、単に URL をハードコーディングするよりもはるかに多機能です。</span><span class="sxs-lookup"><span data-stu-id="48016-291">The preceding URL generation samples are much more feature rich than just hardcoding a URL.</span></span> <span data-ttu-id="48016-292">URL の生成は[ルーティング](xref:mvc/controllers/routing)を使用し、ターゲット パスで定義されたルート方法に従って、パラメーターの生成とエンコードができます。</span><span class="sxs-lookup"><span data-stu-id="48016-292">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="48016-293">ページの URL 生成は、相対名をサポートします。</span><span class="sxs-lookup"><span data-stu-id="48016-293">URL generation for pages supports relative names.</span></span> <span data-ttu-id="48016-294">次の表に、*Pages/Customers/Create.cshtml* の異なる `RedirectToPage` パラメーターで選択されたインデックス ページを示します。</span><span class="sxs-lookup"><span data-stu-id="48016-294">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="48016-295">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="48016-295">RedirectToPage(x)</span></span>| <span data-ttu-id="48016-296">ページ</span><span class="sxs-lookup"><span data-stu-id="48016-296">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="48016-297">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="48016-297">RedirectToPage("/Index")</span></span> | <span data-ttu-id="48016-298">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="48016-298">*Pages/Index*</span></span> |
| <span data-ttu-id="48016-299">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="48016-299">RedirectToPage("./Index");</span></span> | <span data-ttu-id="48016-300">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="48016-300">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="48016-301">RedirectToPage("../Index")</span><span class="sxs-lookup"><span data-stu-id="48016-301">RedirectToPage("../Index")</span></span> | <span data-ttu-id="48016-302">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="48016-302">*Pages/Index*</span></span> |
| <span data-ttu-id="48016-303">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="48016-303">RedirectToPage("Index")</span></span>  | <span data-ttu-id="48016-304">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="48016-304">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="48016-305">`RedirectToPage("Index")`、`RedirectToPage("./Index")`、および `RedirectToPage("../Index")` は*相対名*です。</span><span class="sxs-lookup"><span data-stu-id="48016-305">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are *relative names*.</span></span> <span data-ttu-id="48016-306">`RedirectToPage` パラメーターは現在のページのパスと*組み合わされて*、ターゲット ページの名前を計算します。</span><span class="sxs-lookup"><span data-stu-id="48016-306">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page. -- page name, not page path -->

<span data-ttu-id="48016-307">相対名のリンクは、複雑な構造を持つサイトを構築する際に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="48016-307">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="48016-308">相対名を使用してフォルダー内のページ間をリンクする場合、そのフォルダー名を変更することができます。</span><span class="sxs-lookup"><span data-stu-id="48016-308">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="48016-309">すべてのリンクは引き続き機能します (リンクにはフォルダー名が含まれていないため)。</span><span class="sxs-lookup"><span data-stu-id="48016-309">All the links still work (because they didn't include the folder name).</span></span>

## <a name="tempdata"></a><span data-ttu-id="48016-310">TempData</span><span class="sxs-lookup"><span data-stu-id="48016-310">TempData</span></span>

<span data-ttu-id="48016-311">ASP.NET Core は [コントローラー](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller)上で [TempData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller#Microsoft_AspNetCore_Mvc_Controller_TempData) プロパティを公開します。</span><span class="sxs-lookup"><span data-stu-id="48016-311">ASP.NET Core exposes the [TempData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="48016-312">このプロパティは、読み取られるまでデータを格納します。</span><span class="sxs-lookup"><span data-stu-id="48016-312">This property stores data until it is read.</span></span> <span data-ttu-id="48016-313">`Keep` メソッドと `Peek` メソッドは、削除せずにデータを確認するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="48016-313">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="48016-314">`TempData` は、複数の要求にデータが必要な場合のリダイレクトに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="48016-314">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="48016-315">`[TempData]` は ASP.NET Core 2.0 の新しい属性で、コントローラーとページでサポートされています。</span><span class="sxs-lookup"><span data-stu-id="48016-315">The `[TempData]` attribute is new in ASP.NET Core 2.0 and is supported on controllers and pages.</span></span>

<span data-ttu-id="48016-316">次のコードは、`TempData` を使用して `Message` の値を設定します。</span><span class="sxs-lookup"><span data-stu-id="48016-316">The following code sets the value of `Message` using `TempData`.</span></span>
<span data-ttu-id="48016-317">[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,27-28&name=snippetTemp)]</span><span class="sxs-lookup"><span data-stu-id="48016-317">[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,27-28&name=snippetTemp)]</span></span>

<span data-ttu-id="48016-318">*Pages/Customers/Index.cshtml* ファイル内の次のマークアップは、`TempData` を使用して `Message` の値を表示します。</span><span class="sxs-lookup"><span data-stu-id="48016-318">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="48016-319">*Pages/Customers/Index.cshtml.cs* 分離コード ファイルは、`[TempData]` 属性を `Message` プロパティに適用します。</span><span class="sxs-lookup"><span data-stu-id="48016-319">The *Pages/Customers/Index.cshtml.cs* code-behind file applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="48016-320">詳細については、「[TempData](xref:fundamentals/app-state#temp)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="48016-320">See [TempData](xref:fundamentals/app-state#temp) for more information.</span></span>

<a name="mhpp"></a>
## <a name="multiple-handlers-per-page"></a><span data-ttu-id="48016-321">ページあたり複数のハンドラー</span><span class="sxs-lookup"><span data-stu-id="48016-321">Multiple handlers per page</span></span>

<span data-ttu-id="48016-322">次のページは、`asp-page-handler` タグ ヘルパーを使用して 2 つのページ ハンドラーにマークアップを生成します。</span><span class="sxs-lookup"><span data-stu-id="48016-322">The following page generates markup for two page handlers using the `asp-page-handler` Tag Helper:</span></span>

<span data-ttu-id="48016-323">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]</span><span class="sxs-lookup"><span data-stu-id="48016-323">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]</span></span>

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there is no `asp-` attribute   -->

<span data-ttu-id="48016-324">前の例のフォームには、それぞれが `FormActionTagHelper` を使用して異なる URL に送信する 2 つの送信ボタンがあります。</span><span class="sxs-lookup"><span data-stu-id="48016-324">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="48016-325">`asp-page-handler` 属性は、`asp-page` のコンパニオンです。</span><span class="sxs-lookup"><span data-stu-id="48016-325">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="48016-326">`asp-page-handler` はページごとに定義されている各ハンドラー メソッドに送信する URL を生成します。</span><span class="sxs-lookup"><span data-stu-id="48016-326">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="48016-327">サンプルは現在のページにリンクしているため、`asp-page` は指定されません。</span><span class="sxs-lookup"><span data-stu-id="48016-327">`asp-page` is not specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="48016-328">分離コード ファイル:</span><span class="sxs-lookup"><span data-stu-id="48016-328">The code-behind file:</span></span>

<span data-ttu-id="48016-329">[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]</span><span class="sxs-lookup"><span data-stu-id="48016-329">[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]</span></span>

<span data-ttu-id="48016-330">上記のコードは、*名前付きハンドラー メソッド*を使用しています。</span><span class="sxs-lookup"><span data-stu-id="48016-330">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="48016-331">名前付きハンドラー メソッドは、名前の `On<HTTP Verb>` の後および `Async` の前 (ある場合) のテキストを取得して作成されます。</span><span class="sxs-lookup"><span data-stu-id="48016-331">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="48016-332">前の例では、ページ メソッドは OnPost**JoinList**Async と OnPost**JoinListUC**Async です。</span><span class="sxs-lookup"><span data-stu-id="48016-332">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="48016-333">*OnPost* と *Async* を削除すると、ハンドラー名は `JoinList` と `JoinListUC` になります。</span><span class="sxs-lookup"><span data-stu-id="48016-333">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

<span data-ttu-id="48016-334">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]</span><span class="sxs-lookup"><span data-stu-id="48016-334">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]</span></span>

<span data-ttu-id="48016-335">上記のコードを使用すると、`OnPostJoinListAsync` に送信される URL パスは `http://localhost:5000/Customers/CreateFATH?handler=JoinList` になります。</span><span class="sxs-lookup"><span data-stu-id="48016-335">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="48016-336">`OnPostJoinListUCAsync` に送信される URL パスは `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC` です。</span><span class="sxs-lookup"><span data-stu-id="48016-336">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="customizing-routing"></a><span data-ttu-id="48016-337">ルーティングのカスタマイズ</span><span class="sxs-lookup"><span data-stu-id="48016-337">Customizing Routing</span></span>

<span data-ttu-id="48016-338">URL 内のクエリ文字列 `?handler=JoinList` が気に入らない場合は、ルートを変更して URL のパス部分にハンドラー名を挿入することができます。</span><span class="sxs-lookup"><span data-stu-id="48016-338">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="48016-339">`@page` ディレクティブの後に二重引用符で囲んだルート テンプレートを追加して、ルートをカスタマイズすることができます。</span><span class="sxs-lookup"><span data-stu-id="48016-339">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

<span data-ttu-id="48016-340">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]</span><span class="sxs-lookup"><span data-stu-id="48016-340">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]</span></span>


<span data-ttu-id="48016-341">上記のルートでは、クエリ文字列の代わりにハンドラー名を URL パスに挿入しています。</span><span class="sxs-lookup"><span data-stu-id="48016-341">The preceding route puts the handler name in the URL path instead of the query string.</span></span> <span data-ttu-id="48016-342">`handler` の後の `?` は、ルート パラメーターが省略可能なことを意味します。</span><span class="sxs-lookup"><span data-stu-id="48016-342">The `?` following `handler` means the route parameter is optional.</span></span>

<span data-ttu-id="48016-343">`@page` を使用して、ページのルートに追加のセグメントとパラメーターを追加できます。</span><span class="sxs-lookup"><span data-stu-id="48016-343">You can use `@page` to add additional segments and parameters to a page's route.</span></span> <span data-ttu-id="48016-344">ここにあるものは何でもページの既定のルートに**追加**されます。</span><span class="sxs-lookup"><span data-stu-id="48016-344">Whatever's there is **appended** to the default route of the page.</span></span> <span data-ttu-id="48016-345">絶対パスまたは仮想パスを使用してページのルート (`"~/Some/Other/Path"` など) を変更することはサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="48016-345">Using an absolute or virtual path to change the page's route (like `"~/Some/Other/Path"`) is not supported.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="48016-346">構成と設定</span><span class="sxs-lookup"><span data-stu-id="48016-346">Configuration and settings</span></span>

<span data-ttu-id="48016-347">高度なオプションを構成するには、MVC ビルダーで拡張メソッド `AddRazorPagesOptions` を使用します。</span><span class="sxs-lookup"><span data-stu-id="48016-347">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

<span data-ttu-id="48016-348">[!code-cs[main](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="48016-348">[!code-cs[main](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet1)]</span></span>

<span data-ttu-id="48016-349">現在は、`RazorPagesOptions` を使用してページのルート ディレクトリを設定したり、ページのアプリケーション モデルの規則を追加したりすることができます。</span><span class="sxs-lookup"><span data-stu-id="48016-349">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="48016-350">将来、この方法でより多くの機能拡張を可能にしたいと考えています。</span><span class="sxs-lookup"><span data-stu-id="48016-350">We hope to enable more extensibility this way in the future.</span></span>

<span data-ttu-id="48016-351">ビューをプリコンパイルするには、「[Razor view compilation](xref:mvc/views/view-compilation)」 (Razor ビュー コンパイル) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="48016-351">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="48016-352">[サンプル コードをダウンロードまたは表示します](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample)。</span><span class="sxs-lookup"><span data-stu-id="48016-352">[Download or view sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span></span>

<span data-ttu-id="48016-353">この概要に基づく、「[Getting started with Razor Pages in ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start)」 (ASP.NET Core での Razor ページの概要) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="48016-353">See [Getting started with Razor Pages in ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>
