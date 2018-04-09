---
uid: mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-vb
title: ビュー マスター ページ (VB) にデータを渡す |Microsoft ドキュメント
author: microsoft
description: このチュートリアルの目的では、ビュー マスター ページへのコント ローラーからデータを渡す方法について説明します。 ビュー m にデータを渡すための 2 つの方法を説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/16/2008
ms.topic: article
ms.assetid: 37a1ebae-8773-408f-8645-d21da7ff9ae1
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: fcd7c5baacc00490720d1f82252d81e40c097c88
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="passing-data-to-view-master-pages-vb"></a><span data-ttu-id="0ccca-104">ビュー マスター ページ (VB) にデータを渡す</span><span class="sxs-lookup"><span data-stu-id="0ccca-104">Passing Data to View Master Pages (VB)</span></span>
====================
<span data-ttu-id="0ccca-105">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="0ccca-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="0ccca-106">PDF をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="0ccca-106">Download PDF</span></span>](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_13_VB.pdf)

> <span data-ttu-id="0ccca-107">このチュートリアルの目的では、ビュー マスター ページへのコント ローラーからデータを渡す方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="0ccca-107">The goal of this tutorial is to explain how you can pass data from a controller to a view master page.</span></span> <span data-ttu-id="0ccca-108">ビュー マスター ページにデータを渡すための 2 つの方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="0ccca-108">We examine two strategies for passing data to a view master page.</span></span> <span data-ttu-id="0ccca-109">最初に、保守が困難なアプリケーションで発生するための簡単な解決策について説明します。</span><span class="sxs-lookup"><span data-stu-id="0ccca-109">First, we discuss an easy solution that results in an application that is difficult to maintain.</span></span> <span data-ttu-id="0ccca-110">次に、少しより保守が容易なアプリケーションで結果より多くの初期作業を必要とするより良いソリューションを調べます。</span><span class="sxs-lookup"><span data-stu-id="0ccca-110">Next, we examine a much better solution that requires a little more initial work but results in a much more maintainable application.</span></span>


## <a name="passing-data-to-view-master-pages"></a><span data-ttu-id="0ccca-111">マスター ページの表示にデータを渡す</span><span class="sxs-lookup"><span data-stu-id="0ccca-111">Passing Data to View Master Pages</span></span>

<span data-ttu-id="0ccca-112">このチュートリアルの目的では、ビュー マスター ページへのコント ローラーからデータを渡す方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="0ccca-112">The goal of this tutorial is to explain how you can pass data from a controller to a view master page.</span></span> <span data-ttu-id="0ccca-113">ビュー マスター ページにデータを渡すための 2 つの方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="0ccca-113">We examine two strategies for passing data to a view master page.</span></span> <span data-ttu-id="0ccca-114">最初に、保守が困難なアプリケーションで発生するための簡単な解決策について説明します。</span><span class="sxs-lookup"><span data-stu-id="0ccca-114">First, we discuss an easy solution that results in an application that is difficult to maintain.</span></span> <span data-ttu-id="0ccca-115">次に、少しより保守が容易なアプリケーションで結果より多くの初期作業を必要とするより良いソリューションを調べます。</span><span class="sxs-lookup"><span data-stu-id="0ccca-115">Next, we examine a much better solution that requires a little more initial work but results in a much more maintainable application.</span></span>

### <a name="the-problem"></a><span data-ttu-id="0ccca-116">問題を</span><span class="sxs-lookup"><span data-stu-id="0ccca-116">The Problem</span></span>

<span data-ttu-id="0ccca-117">ムービーのデータベース アプリケーションを構築して、アプリケーション内の各ページにムービーのカテゴリの一覧を表示することを想像してください (図 1 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="0ccca-117">Imagine that you are building a movie database application and you want to display the list of movie categories on every page in your application (see Figure 1).</span></span> <span data-ttu-id="0ccca-118">さらに、ムービーのカテゴリの一覧が、データベース テーブルに格納される想像してみてください。</span><span class="sxs-lookup"><span data-stu-id="0ccca-118">Imagine, furthermore, that the list of movie categories is stored in a database table.</span></span> <span data-ttu-id="0ccca-119">その場合は、データベースからカテゴリを取得し、ビューのマスター ページ内のムービーのカテゴリの一覧を表示します。</span><span class="sxs-lookup"><span data-stu-id="0ccca-119">In that case, it would make sense to retrieve the categories from the database and render the list of movie categories within a view master page.</span></span>


<span data-ttu-id="0ccca-120">[![ビュー マスター ページで、ムービーのカテゴリを表示します。](passing-data-to-view-master-pages-vb/_static/image2.png)](passing-data-to-view-master-pages-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="0ccca-120">[![Displaying movie categories in a view master page](passing-data-to-view-master-pages-vb/_static/image2.png)](passing-data-to-view-master-pages-vb/_static/image1.png)</span></span>

<span data-ttu-id="0ccca-121">**図 01**: ビュー マスター ページで、ムービーのカテゴリを表示する ([フルサイズのイメージを表示するをクリックして](passing-data-to-view-master-pages-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="0ccca-121">**Figure 01**: Displaying movie categories in a view master page ([Click to view full-size image](passing-data-to-view-master-pages-vb/_static/image3.png))</span></span>


<span data-ttu-id="0ccca-122">この問題を次に示します。</span><span class="sxs-lookup"><span data-stu-id="0ccca-122">Here's the problem.</span></span> <span data-ttu-id="0ccca-123">マスター ページ内のムービーのカテゴリの一覧を取得する方法</span><span class="sxs-lookup"><span data-stu-id="0ccca-123">How do you retrieve the list of movie categories in the master page?</span></span> <span data-ttu-id="0ccca-124">ことは避けて、マスター ページで、モデル クラスのメソッドを直接呼び出します。</span><span class="sxs-lookup"><span data-stu-id="0ccca-124">It is tempting to call methods of your model classes in the master page directly.</span></span> <span data-ttu-id="0ccca-125">つまりは、マスター ページの右側にデータベースからデータを取得するためのコードを含めます。</span><span class="sxs-lookup"><span data-stu-id="0ccca-125">In other words, it is tempting to include the code for retrieving the data from the database right in your master page.</span></span> <span data-ttu-id="0ccca-126">ただし、データベースへのアクセス、MVC コント ローラーをバイパスする違反となる、クリーン関心の分離は、MVC アプリケーションの構築の主な利点の 1 つです。</span><span class="sxs-lookup"><span data-stu-id="0ccca-126">However, bypassing your MVC controllers to access the database would violate the clean separation of concerns that is one of the primary benefits of building an MVC application.</span></span>

<span data-ttu-id="0ccca-127">MVC アプリケーション、MVC コント ローラーによって処理するには、MVC ビューと、MVC モデルの間のすべての対話できるようにします。</span><span class="sxs-lookup"><span data-stu-id="0ccca-127">In an MVC application, you want all interaction between your MVC views and your MVC model to be handled by your MVC controllers.</span></span> <span data-ttu-id="0ccca-128">この関心の分離は、保守が容易な適応性、およびテストが容易なアプリケーションで発生します。</span><span class="sxs-lookup"><span data-stu-id="0ccca-128">This separation of concerns results in a more maintainable, adaptable, and testable application.</span></span>

<span data-ttu-id="0ccca-129">MVC アプリケーションで、ビュー マスター ページを含むビューに渡されるすべてのデータをコント ローラーのアクションでのビューに渡される必要があります。</span><span class="sxs-lookup"><span data-stu-id="0ccca-129">In an MVC application, all data passed to a view – including a view master page – should be passed to a view by a controller action.</span></span> <span data-ttu-id="0ccca-130">さらに、データは、データの表示を活用して渡されます。</span><span class="sxs-lookup"><span data-stu-id="0ccca-130">Furthermore, the data should be passed by taking advantage of view data.</span></span> <span data-ttu-id="0ccca-131">このチュートリアルの残りの部分では、ビューのマスター ページ ビューのデータを渡すことの 2 つのメソッドを調べます。</span><span class="sxs-lookup"><span data-stu-id="0ccca-131">In the remainder of this tutorial, I examine two methods of passing view data to a view master page.</span></span>

### <a name="the-simple-solution"></a><span data-ttu-id="0ccca-132">単純なソリューション</span><span class="sxs-lookup"><span data-stu-id="0ccca-132">The Simple Solution</span></span>

<span data-ttu-id="0ccca-133">ビュー マスター ページに、コント ローラーからビューのデータを渡す最も簡単なソリューションから始めましょう。</span><span class="sxs-lookup"><span data-stu-id="0ccca-133">Let's start with the simplest solution to passing view data from a controller to a view master page.</span></span> <span data-ttu-id="0ccca-134">最も簡単なソリューションでは、各コント ローラーのアクションで、マスター ページのビュー データを渡します。</span><span class="sxs-lookup"><span data-stu-id="0ccca-134">The simplest solution is to pass the view data for the master page in each and every controller action.</span></span>

<span data-ttu-id="0ccca-135">リスト 1 のコント ローラーを検討してください。</span><span class="sxs-lookup"><span data-stu-id="0ccca-135">Consider the controller in Listing 1.</span></span> <span data-ttu-id="0ccca-136">という名前の 2 つのアクションを公開`Index()`と`Details()`です。</span><span class="sxs-lookup"><span data-stu-id="0ccca-136">It exposes two actions named `Index()` and `Details()`.</span></span> <span data-ttu-id="0ccca-137">`Index()`アクション メソッドは、映画データベース テーブルですべてのムービーを返します。</span><span class="sxs-lookup"><span data-stu-id="0ccca-137">The `Index()` action method returns every movie in the Movies database table.</span></span> <span data-ttu-id="0ccca-138">`Details()`アクション メソッドは、特定のムービーのカテゴリにどのムービーを返します。</span><span class="sxs-lookup"><span data-stu-id="0ccca-138">The `Details()` action method returns every movie in a particular movie category.</span></span>

<span data-ttu-id="0ccca-139">**1 – を一覧表示します。 `Controllers\HomeController.vb`**</span><span class="sxs-lookup"><span data-stu-id="0ccca-139">**Listing 1 – `Controllers\HomeController.vb`**</span></span>

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample1.vb)]

<span data-ttu-id="0ccca-140">注意して両方の`Index()`と`Details()`アクションは、データを表示する 2 つの項目を追加します。</span><span class="sxs-lookup"><span data-stu-id="0ccca-140">Notice that both the `Index()` and the `Details()` actions add two items to view data.</span></span> <span data-ttu-id="0ccca-141">`Index()`操作は、2 つのキーを追加: カテゴリおよびビデオ。</span><span class="sxs-lookup"><span data-stu-id="0ccca-141">The `Index()` action adds two keys: categories and movies.</span></span> <span data-ttu-id="0ccca-142">カテゴリのキーでは、ビュー マスター ページで表示されるビデオ カテゴリの一覧を表します。</span><span class="sxs-lookup"><span data-stu-id="0ccca-142">The categories key represents the list of movie categories displayed by the view master page.</span></span> <span data-ttu-id="0ccca-143">映画キーでは、インデックスの表示 ページで表示されるムービーの一覧を表します。</span><span class="sxs-lookup"><span data-stu-id="0ccca-143">The movies key represents the list of movies displayed by the Index view page.</span></span>

<span data-ttu-id="0ccca-144">`Details()`アクションもカテゴリや映画をという名前の 2 つのキーを追加します。</span><span class="sxs-lookup"><span data-stu-id="0ccca-144">The `Details()` action also adds two keys named categories and movies.</span></span> <span data-ttu-id="0ccca-145">カテゴリ キーは、もう一度ビュー マスター ページで表示されるビデオ カテゴリの一覧を表します。</span><span class="sxs-lookup"><span data-stu-id="0ccca-145">The categories key, once again, represents the list of movie categories displayed by the view master page.</span></span> <span data-ttu-id="0ccca-146">映画キーは、詳細ビュー ページで表示される特定のカテゴリにムービーの一覧を表します (図 2 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="0ccca-146">The movies key represents the list of movies in a particular category displayed by the Details view page (see Figure 2).</span></span>


<span data-ttu-id="0ccca-147">[![詳細ビュー](passing-data-to-view-master-pages-vb/_static/image5.png)](passing-data-to-view-master-pages-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="0ccca-147">[![The Details view](passing-data-to-view-master-pages-vb/_static/image5.png)](passing-data-to-view-master-pages-vb/_static/image4.png)</span></span>

<span data-ttu-id="0ccca-148">**図 02**: [詳細] ビュー ([フルサイズのイメージを表示するをクリックして](passing-data-to-view-master-pages-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="0ccca-148">**Figure 02**: The Details view ([Click to view full-size image](passing-data-to-view-master-pages-vb/_static/image6.png))</span></span>


<span data-ttu-id="0ccca-149">インデックス ビューは、2 のリストに含まれます。</span><span class="sxs-lookup"><span data-stu-id="0ccca-149">The Index view is contained in Listing 2.</span></span> <span data-ttu-id="0ccca-150">データの表示の動画アイテムで表されるムービーの一覧を反復処理するだけです。</span><span class="sxs-lookup"><span data-stu-id="0ccca-150">It simply iterates through the list of movies represented by the movies item in view data.</span></span>

<span data-ttu-id="0ccca-151">**2 – を一覧表示します。 `Views\Home\Index.aspx`**</span><span class="sxs-lookup"><span data-stu-id="0ccca-151">**Listing 2 – `Views\Home\Index.aspx`**</span></span>

[!code-aspx[Main](passing-data-to-view-master-pages-vb/samples/sample2.aspx)]

<span data-ttu-id="0ccca-152">ビュー マスター ページは、3 の一覧に含まれます。</span><span class="sxs-lookup"><span data-stu-id="0ccca-152">The view master page is contained in Listing 3.</span></span> <span data-ttu-id="0ccca-153">ビュー マスター ページでは、反復処理し、すべてのデータの表示からカテゴリ項目によって表される映画カテゴリを表示します。</span><span class="sxs-lookup"><span data-stu-id="0ccca-153">The view master page iterates and renders all of the movie categories represented by the categories item from view data.</span></span>

<span data-ttu-id="0ccca-154">**3 – を一覧表示します。 `Views\Shared\Site.master`**</span><span class="sxs-lookup"><span data-stu-id="0ccca-154">**Listing 3 – `Views\Shared\Site.master`**</span></span>

[!code-aspx[Main](passing-data-to-view-master-pages-vb/samples/sample3.aspx)]

<span data-ttu-id="0ccca-155">すべてのデータは、ビュー データをビューとビューのマスター ページに渡されます。</span><span class="sxs-lookup"><span data-stu-id="0ccca-155">All data is passed to the view and the view master page through view data.</span></span> <span data-ttu-id="0ccca-156">マスター ページにデータを渡すための正しい方法です。</span><span class="sxs-lookup"><span data-stu-id="0ccca-156">That is the correct way to pass data to the master page.</span></span>

<span data-ttu-id="0ccca-157">そのため、このソリューションに関する問題ですか。</span><span class="sxs-lookup"><span data-stu-id="0ccca-157">So, what's wrong with this solution?</span></span> <span data-ttu-id="0ccca-158">この問題は、このソリューションがドライ (しないことを繰り返さない) の原則に違反します。</span><span class="sxs-lookup"><span data-stu-id="0ccca-158">The problem is that this solution violates the DRY (Don't Repeat Yourself) principle.</span></span> <span data-ttu-id="0ccca-159">各コント ローラーのアクションは、非常に同じデータを表示するムービーのカテゴリの一覧を追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0ccca-159">Each and every controller action must add the very same list of movie categories to view data.</span></span> <span data-ttu-id="0ccca-160">アプリケーションで重複するコードを持つによってにより、アプリケーションの管理、改変、および変更する非常に困難です。</span><span class="sxs-lookup"><span data-stu-id="0ccca-160">Having duplicate code in your application makes your application much more difficult to maintain, adapt, and modify.</span></span>

### <a name="the-good-solution"></a><span data-ttu-id="0ccca-161">適切なソリューション</span><span class="sxs-lookup"><span data-stu-id="0ccca-161">The Good Solution</span></span>

<span data-ttu-id="0ccca-162">このセクションでは、代替、およびより優れたソリューションがコント ローラーのアクションからビュー マスター ページにデータを渡すことを確認します。</span><span class="sxs-lookup"><span data-stu-id="0ccca-162">In this section, we examine an alternative, and better, solution to passing data from a controller action to a view master page.</span></span> <span data-ttu-id="0ccca-163">マスター ページのムービーのカテゴリを追加する、各コント ローラーのアクションで、代わりにカテゴリを追加、ムービー ビュー データを 1 回だけ。</span><span class="sxs-lookup"><span data-stu-id="0ccca-163">Instead of adding the movie categories for the master page in each and every controller action, we add the movie categories to the view data only once.</span></span> <span data-ttu-id="0ccca-164">ビュー マスター ページで使用されるすべてのビュー データは、アプリケーションのコント ローラーに追加されます。</span><span class="sxs-lookup"><span data-stu-id="0ccca-164">All view data used by the view master page is added in an Application controller.</span></span>

<span data-ttu-id="0ccca-165">ApplicationController クラスは、4 の一覧に含まれます。</span><span class="sxs-lookup"><span data-stu-id="0ccca-165">The ApplicationController class is contained in Listing 4.</span></span>

<span data-ttu-id="0ccca-166">ApplicationController クラスは、4 の一覧に含まれます。</span><span class="sxs-lookup"><span data-stu-id="0ccca-166">The ApplicationController class is contained in Listing 4.</span></span>

<span data-ttu-id="0ccca-167">**4 – を一覧表示します。 `Controllers\ApplicationController.vb`**</span><span class="sxs-lookup"><span data-stu-id="0ccca-167">**Listing 4 – `Controllers\ApplicationController.vb`**</span></span>

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample4.vb)]

<span data-ttu-id="0ccca-168">一覧表示する 4 でアプリケーションのコント ローラーに関する注意すべき次の 3 つの点があります。</span><span class="sxs-lookup"><span data-stu-id="0ccca-168">There are three things that you should notice about the Application controller in Listing 4.</span></span> <span data-ttu-id="0ccca-169">最初に、クラスが基本 System.Web.Mvc.Controller クラスから継承されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="0ccca-169">First, notice that the class inherits from the base System.Web.Mvc.Controller class.</span></span> <span data-ttu-id="0ccca-170">アプリケーションのコント ローラーは、コント ローラー クラスです。</span><span class="sxs-lookup"><span data-stu-id="0ccca-170">The Application controller is a controller class.</span></span>

<span data-ttu-id="0ccca-171">第二に、アプリケーションのコント ローラー クラスが MustInherit クラスであるに注意してください。</span><span class="sxs-lookup"><span data-stu-id="0ccca-171">Second, notice that the Application controller class is a MustInherit class.</span></span> <span data-ttu-id="0ccca-172">MustInherit クラスは、具象クラスで実装される必要があるクラスです。</span><span class="sxs-lookup"><span data-stu-id="0ccca-172">An MustInherit class is a class that must be implemented by a concrete class.</span></span> <span data-ttu-id="0ccca-173">アプリケーションのコント ローラーは MustInherit クラスであるため、クラスで直接定義されているすべてのメソッドを呼び出せませんことはできません。</span><span class="sxs-lookup"><span data-stu-id="0ccca-173">Because the Application controller is an MustInherit class, you cannot not invoke any methods defined in the class directly.</span></span> <span data-ttu-id="0ccca-174">アプリケーションのクラスを直接呼び出すしようとすると、リソースが検出されないエラー メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="0ccca-174">If you attempt to invoke the Application class directly then you'll get a Resource Cannot Be Found error message.</span></span>

<span data-ttu-id="0ccca-175">3 番目に、アプリケーション コント ローラーにデータを表示するムービーのカテゴリの一覧に追加するコンス トラクターが含まれていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="0ccca-175">Third, notice that the Application controller contains a constructor that adds the list of movie categories to view data.</span></span> <span data-ttu-id="0ccca-176">アプリケーションのコント ローラーから継承するすべてのコント ローラー クラスでは、自動的にアプリケーションのコント ローラーのコンス トラクターを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="0ccca-176">Every controller class that inherits from the Application controller calls the Application controller's constructor automatically.</span></span> <span data-ttu-id="0ccca-177">アプリケーションのコント ローラーから継承する任意のコント ローラーで任意のアクションを呼び出すと、ときに、ムービーのカテゴリがビュー データに自動的に追加します。</span><span class="sxs-lookup"><span data-stu-id="0ccca-177">Whenever you call any action on any controller that inherits from the Application controller, the movie categories is included in the view data automatically.</span></span>

<span data-ttu-id="0ccca-178">リスト 5 のビデオ コント ローラーは、アプリケーションのコント ローラーから継承します。</span><span class="sxs-lookup"><span data-stu-id="0ccca-178">The Movies controller in Listing 5 inherits from the Application controller.</span></span>

<span data-ttu-id="0ccca-179">**5 – を一覧表示します。 `Controllers\MoviesController.vb`**</span><span class="sxs-lookup"><span data-stu-id="0ccca-179">**Listing 5 – `Controllers\MoviesController.vb`**</span></span>

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample5.vb)]

<span data-ttu-id="0ccca-180">前のセクションで説明されている、Home コント ローラーと同じように、ムービーのコント ローラーという 2 つのアクション メソッドを公開する`Index()`と`Details()`です。</span><span class="sxs-lookup"><span data-stu-id="0ccca-180">The Movies controller, just like the Home controller discussed in the previous section, exposes two action methods named `Index()` and `Details()`.</span></span> <span data-ttu-id="0ccca-181">いずれかでデータを表示するビュー マスター ページで表示されるビデオ カテゴリの一覧ではない通知が追加された、`Index()`または`Details()`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="0ccca-181">Notice that the list of movie categories displayed by the view master page is not added to view data in either the `Index()` or `Details()` method.</span></span> <span data-ttu-id="0ccca-182">ビデオ コント ローラーは、アプリケーションのコント ローラーから継承、するために自動的にデータを表示するムービーのカテゴリの一覧が追加されます。</span><span class="sxs-lookup"><span data-stu-id="0ccca-182">Because the Movies controller inherits from the Application controller, the list of movie categories is added to view data automatically.</span></span>

<span data-ttu-id="0ccca-183">ビュー マスター ページのビュー データを追加するには、このソリューションにドライ (しないことを繰り返さない) の原則に違反していないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="0ccca-183">Notice that this solution to adding view data for a view master page does not violate the DRY (Don't Repeat Yourself) principle.</span></span> <span data-ttu-id="0ccca-184">1 つの場所にデータを表示するムービーのカテゴリの一覧を追加するためのコードが含まれている: アプリケーションのコント ローラーのコンス トラクターです。</span><span class="sxs-lookup"><span data-stu-id="0ccca-184">The code for adding the list of movie categories to view data is contained in only one location: the constructor for the Application controller.</span></span>

### <a name="summary"></a><span data-ttu-id="0ccca-185">まとめ</span><span class="sxs-lookup"><span data-stu-id="0ccca-185">Summary</span></span>

<span data-ttu-id="0ccca-186">このチュートリアルでは、コント ローラーからビュー マスター ページ ビュー データを渡す 2 つの方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="0ccca-186">In this tutorial, we discussed two approaches to passing view data from a controller to a view master page.</span></span> <span data-ttu-id="0ccca-187">まず、アプローチを維持するが困難ですが、単純なを調査します。</span><span class="sxs-lookup"><span data-stu-id="0ccca-187">First, we examined a simple, but difficult to maintain approach.</span></span> <span data-ttu-id="0ccca-188">最初のセクションでは、アプリケーションで各すべてコント ローラーのアクションでビュー マスター ページのビュー データを追加する方法について説明しました。</span><span class="sxs-lookup"><span data-stu-id="0ccca-188">In the first section, we discussed how you can add view data for a view master page in each every controller action in your application.</span></span> <span data-ttu-id="0ccca-189">という結論に達しました (しないことを繰り返さない) ドライ原則に違反しているために、不適切なアプローチがこれことです。</span><span class="sxs-lookup"><span data-stu-id="0ccca-189">We concluded that this was a bad approach because it violates the DRY (Don't Repeat Yourself) principle.</span></span>

<span data-ttu-id="0ccca-190">次に、データを表示するビュー マスター ページで必要なデータを追加するためより適切な程度戦略を調査します。</span><span class="sxs-lookup"><span data-stu-id="0ccca-190">Next, we examined a much better strategy for adding data required by a view master page to view data.</span></span> <span data-ttu-id="0ccca-191">ビュー データを追加する、各コント ローラーのアクションで、代わりに、アプリケーションのコント ローラー内では、ビュー データを 1 回だけを追加しています。</span><span class="sxs-lookup"><span data-stu-id="0ccca-191">Instead of adding the view data in each and every controller action, we added the view data only once within an Application controller.</span></span> <span data-ttu-id="0ccca-192">ようにして、ビュー マスター ページ、ASP.NET MVC アプリケーションでデータを渡す場合は、重複するコードを回避できます。</span><span class="sxs-lookup"><span data-stu-id="0ccca-192">That way, you can avoid duplicate code when passing data to a view master page in an ASP.NET MVC application.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="0ccca-193">前へ</span><span class="sxs-lookup"><span data-stu-id="0ccca-193">Previous</span></span>](creating-page-layouts-with-view-master-pages-vb.md)
