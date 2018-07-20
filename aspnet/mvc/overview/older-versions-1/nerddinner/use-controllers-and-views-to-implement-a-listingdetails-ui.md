---
uid: mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
title: コント ローラーとビューを使用して、リスティング/詳細 UI を実装する |Microsoft Docs
author: microsoft
description: 手順 4 では、コント ローラーのユーザー データのリスティング/詳細ナビゲーション エクスペリエンスを提供するには、このモデルを利用するアプリケーションを追加する方法を示します.
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: 64116e56-1c9a-4f07-8097-bb36cbb6e57f
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
msc.type: authoredcontent
ms.openlocfilehash: 7a0a057efb52a869a72722b24d7283cb883db858
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838586"
---
<a name="use-controllers-and-views-to-implement-a-listingdetails-ui"></a><span data-ttu-id="04ed3-103">コント ローラーとビューを使用して、リスティング/詳細 UI を実装するには</span><span class="sxs-lookup"><span data-stu-id="04ed3-103">Use Controllers and Views to Implement a Listing/Details UI</span></span>
====================
<span data-ttu-id="04ed3-104">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="04ed3-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="04ed3-105">PDF のダウンロード</span><span class="sxs-lookup"><span data-stu-id="04ed3-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="04ed3-106">これは、無料の手順 4 ["NerdDinner"アプリケーションのチュートリアル](introducing-the-nerddinner-tutorial.md)をウォーク スルーの小さなをビルドしても、ASP.NET MVC 1 を使用して web アプリケーションを実行する方法。</span><span class="sxs-lookup"><span data-stu-id="04ed3-106">This is step 4 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="04ed3-107">手順 4 では、コント ローラーのユーザー データ リスティング/詳細ナビゲーション エクスペリエンスを dinners NerdDinner サイトに提供するには、このモデルを利用するアプリケーションを追加する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="04ed3-107">Step 4 shows how to add a Controller to the application that takes advantage of our model to provide users with a data listing/details navigation experience for dinners on our NerdDinner site.</span></span>
> 
> <span data-ttu-id="04ed3-108">次のことをお勧め ASP.NET MVC 3 を使用している場合、 [MVC 3 の開始と取得](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)または[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)チュートリアル。</span><span class="sxs-lookup"><span data-stu-id="04ed3-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-4-controllers-and-views"></a><span data-ttu-id="04ed3-109">NerdDinner 手順 4: コント ローラーとビュー</span><span class="sxs-lookup"><span data-stu-id="04ed3-109">NerdDinner Step 4: Controllers and Views</span></span>

<span data-ttu-id="04ed3-110">従来の web フレームワーク (classic ASP、PHP、ASP.NET Web フォームなど)、受信した Url は通常ディスク上のファイルにマップされます。</span><span class="sxs-lookup"><span data-stu-id="04ed3-110">With traditional web frameworks (classic ASP, PHP, ASP.NET Web Forms, etc), incoming URLs are typically mapped to files on disk.</span></span> <span data-ttu-id="04ed3-111">例: のような要求 URL の"/ディスク上のファイル"または"/されず"、「繰り返し」または「されず」ファイルで処理される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="04ed3-111">For example: a request for a URL like "/Products.aspx" or "/Products.php" might be processed by a "Products.aspx" or "Products.php" file.</span></span>

<span data-ttu-id="04ed3-112">Web ベースの MVC フレームワークは、少し異なる方法で Url をサーバー コードにマップします。</span><span class="sxs-lookup"><span data-stu-id="04ed3-112">Web-based MVC frameworks map URLs to server code in a slightly different way.</span></span> <span data-ttu-id="04ed3-113">受信した Url をファイルにマップするには、代わりには、代わりに、クラスのメソッドに Url をマップします。</span><span class="sxs-lookup"><span data-stu-id="04ed3-113">Instead of mapping incoming URLs to files, they instead map URLs to methods on classes.</span></span> <span data-ttu-id="04ed3-114">これらのクラスは「コント ローラー」と呼ばれますとユーザー入力を処理する受信 HTTP 要求を処理する責任が、クライアント バックアップを取得して、データの保存と送信する応答を決定する (HTML を表示、ダウンロード ファイルを別にリダイレクトURL など)。</span><span class="sxs-lookup"><span data-stu-id="04ed3-114">These classes are called "Controllers" and they are responsible for processing incoming HTTP requests, handling user input, retrieving and saving data, and determining the response to send back to the client (display HTML, download a file, redirect to a different URL, etc).</span></span>

<span data-ttu-id="04ed3-115">NerdDinner アプリケーションの基本的なモデルを構築しましたが、次の手順はコント ローラーのユーザー データ リスティング/詳細ナビゲーション エクスペリエンスを Dinners サイトに提供することを利用するアプリケーションを追加するとします。</span><span class="sxs-lookup"><span data-stu-id="04ed3-115">Now that we have built up a basic model for our NerdDinner application, our next step will be to add a Controller to the application that takes advantage of it to provide users with a data listing/details navigation experience for Dinners on our site.</span></span>

### <a name="adding-a-dinnerscontroller-controller"></a><span data-ttu-id="04ed3-116">DinnersController コント ローラーの追加</span><span class="sxs-lookup"><span data-stu-id="04ed3-116">Adding a DinnersController Controller</span></span>

<span data-ttu-id="04ed3-117">当社の web プロジェクト内でコント ローラー」フォルダーを右クリックして開始し、いますが、**追加 -&gt;コント ローラー** (もコマンドを実行できるこの Ctrl M, Ctrl + C を入力して) メニューのコマンド。</span><span class="sxs-lookup"><span data-stu-id="04ed3-117">We'll begin by right-clicking on the "Controllers" folder within our web project, and then select the **Add-&gt;Controller** menu command (you can also execute this command by typing Ctrl-M, Ctrl-C):</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image1.png)

<span data-ttu-id="04ed3-118">「コント ローラーの追加」ダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="04ed3-118">This will bring up the "Add Controller" dialog:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image2.png)

<span data-ttu-id="04ed3-119">新しいコント ローラー"DinnersController"という名前がされ、[追加] ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="04ed3-119">We'll name the new controller "DinnersController" and click the "Add" button.</span></span> <span data-ttu-id="04ed3-120">Visual Studio は、\Controllers ディレクトリの下に DinnersController.cs ファイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="04ed3-120">Visual Studio will then add a DinnersController.cs file under our \Controllers directory:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image3.png)

<span data-ttu-id="04ed3-121">コード エディター内で新しい DinnersController クラスも開きます。</span><span class="sxs-lookup"><span data-stu-id="04ed3-121">It will also open up the new DinnersController class within the code-editor.</span></span>

### <a name="adding-index-and-details-action-methods-to-the-dinnerscontroller-class"></a><span data-ttu-id="04ed3-122">DinnersController クラスへの Index() と Details() アクション メソッドの追加</span><span class="sxs-lookup"><span data-stu-id="04ed3-122">Adding Index() and Details() Action Methods to the DinnersController Class</span></span>

<span data-ttu-id="04ed3-123">アプリケーションを使用して今後の dinners の一覧を参照し、固有の詳細については、それを表示する一覧で任意の Dinner をクリックすることを許可するユーザーを有効にします。</span><span class="sxs-lookup"><span data-stu-id="04ed3-123">We want to enable visitors using our application to browse a list of upcoming dinners, and allow them to click on any Dinner in the list to see specific details about it.</span></span> <span data-ttu-id="04ed3-124">アプリケーションから次の Url を発行することで実施します。</span><span class="sxs-lookup"><span data-stu-id="04ed3-124">We'll do this by publishing the following URLs from our application:</span></span>

| <span data-ttu-id="04ed3-125">**URL**</span><span class="sxs-lookup"><span data-stu-id="04ed3-125">**URL**</span></span> | <span data-ttu-id="04ed3-126">**目的**</span><span class="sxs-lookup"><span data-stu-id="04ed3-126">**Purpose**</span></span> |
| --- | --- |
| <span data-ttu-id="04ed3-127">*/Dinners/*</span><span class="sxs-lookup"><span data-stu-id="04ed3-127">*/Dinners/*</span></span> | <span data-ttu-id="04ed3-128">今後の dinners の HTML リストを表示します。</span><span class="sxs-lookup"><span data-stu-id="04ed3-128">Display an HTML list of upcoming dinners</span></span> |
| <span data-ttu-id="04ed3-129">*/Dinners 詳細/[id]*</span><span class="sxs-lookup"><span data-stu-id="04ed3-129">*/Dinners/Details/[id]*</span></span> | <span data-ttu-id="04ed3-130">これが一致するデータベースの dinner の DinnerID、URL 内に埋め込まれて、"id"パラメーターで指定された特定の dinner に関する詳細を表示します。</span><span class="sxs-lookup"><span data-stu-id="04ed3-130">Display details about a specific dinner indicated by an "id" parameter embedded within the URL – which will match the DinnerID of the dinner in the database.</span></span> <span data-ttu-id="04ed3-131">例:/Dinners/Details/2 DinnerID 値 2 は、Dinner の詳細を含む HTML ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="04ed3-131">For example: /Dinners/Details/2 would display an HTML page with details about the Dinner whose DinnerID value is 2.</span></span> |

<span data-ttu-id="04ed3-132">次のように、DinnersController クラスに 2 つのパブリック「操作方法」を追加することでこれらの Url の最初の実装を発行します。</span><span class="sxs-lookup"><span data-stu-id="04ed3-132">We will publish initial implementations of these URLs by adding two public "action methods" to our DinnersController class like below:</span></span>

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample1.cs)]

<span data-ttu-id="04ed3-133">NerdDinner アプリケーションを実行し、呼び出し、ブラウザーを使用します。</span><span class="sxs-lookup"><span data-stu-id="04ed3-133">We'll then run the NerdDinner application and use our browser to invoke them.</span></span> <span data-ttu-id="04ed3-134">入力、 *「Dinners/」* により、URL、 *Index()* 実行するメソッドが戻り、次の応答で送信は。</span><span class="sxs-lookup"><span data-stu-id="04ed3-134">Typing in the *"/Dinners/"* URL will cause our *Index()* method to run, and it will send back the following response:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image4.png)

<span data-ttu-id="04ed3-135">入力、 *「/Dinners/詳細/2」* により、URL、 *Details()* メソッドを実行し、次の応答を返信します。</span><span class="sxs-lookup"><span data-stu-id="04ed3-135">Typing in the *"/Dinners/Details/2"* URL will cause our *Details()* method to run, and send back the following response:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image5.png)

<span data-ttu-id="04ed3-136">かもしれません。-ASP.NET MVC 知った、DinnersController クラスを作成し、これらのメソッドを呼び出しますか。</span><span class="sxs-lookup"><span data-stu-id="04ed3-136">You might be wondering - how did ASP.NET MVC know to create our DinnersController class and invoke those methods?</span></span> <span data-ttu-id="04ed3-137">みましょうを理解するルーティングの動作の概要を確認します。</span><span class="sxs-lookup"><span data-stu-id="04ed3-137">To understand that let's take a quick look at how routing works.</span></span>

### <a name="understanding-aspnet-mvc-routing"></a><span data-ttu-id="04ed3-138">Understanding ASP.NET MVC のルーティング</span><span class="sxs-lookup"><span data-stu-id="04ed3-138">Understanding ASP.NET MVC Routing</span></span>

<span data-ttu-id="04ed3-139">ASP.NET MVC には、強力な URL ルーティング エンジンで Url をコント ローラー クラスにマップする方法を制御する柔軟性を提供するが含まれています。</span><span class="sxs-lookup"><span data-stu-id="04ed3-139">ASP.NET MVC includes a powerful URL routing engine that provides a lot of flexibility in controlling how URLs are mapped to controller classes.</span></span> <span data-ttu-id="04ed3-140">ASP.NET MVC がどのメソッドを呼び出すには、できるだけでなくのさまざまな方法を変数できます。 が自動的に URL/クエリ文字列から解析されたパラメーターとしてメソッドに渡される構成を作成するには、どのコント ローラー クラスを選択する方法を完全にカスタマイズすることができます。引数。</span><span class="sxs-lookup"><span data-stu-id="04ed3-140">It allows us to completely customize how ASP.NET MVC chooses which controller class to create, which method to invoke on it, as well as configure different ways that variables can be automatically parsed from the URL/Querystring and passed to the method as parameter arguments.</span></span> <span data-ttu-id="04ed3-141">まったく SEO (検索エンジンの最適化) のサイトを最適化するだけでなく、アプリケーションからすべての URL 構造を公開する柔軟性を提供します。</span><span class="sxs-lookup"><span data-stu-id="04ed3-141">It delivers the flexibility to totally optimize a site for SEO (search engine optimization) as well as publish any URL structure we want from an application.</span></span>

<span data-ttu-id="04ed3-142">既定では、新しい ASP.NET MVC プロジェクトの URL ルーティング ルールが既に登録されている事前構成済みのセットが付属します。</span><span class="sxs-lookup"><span data-stu-id="04ed3-142">By default, new ASP.NET MVC projects come with a preconfigured set of URL routing rules already registered.</span></span> <span data-ttu-id="04ed3-143">これにより、アプリケーションで明示的に何も構成することがなく簡単に開始します。</span><span class="sxs-lookup"><span data-stu-id="04ed3-143">This enables us to easily get started on an application without having to explicitly configure anything.</span></span> <span data-ttu-id="04ed3-144">既定のルーティング ルールの登録は、このプロジェクトのルートに"Global.asax"ファイルをダブルクリックして開くことができます: プロジェクトの"Application"クラス内で確認できます。</span><span class="sxs-lookup"><span data-stu-id="04ed3-144">The default routing rule registrations can be found within the "Application" class of our projects – which we can open by double-clicking the "Global.asax" file in the root of our project:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image6.png)

<span data-ttu-id="04ed3-145">既定の ASP.NET MVC ルーティング ルールは、このクラスの"RegisterRoutes"メソッド内で登録されます。</span><span class="sxs-lookup"><span data-stu-id="04ed3-145">The default ASP.NET MVC routing rules are registered within the "RegisterRoutes" method of this class:</span></span>

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample2.cs)]

<span data-ttu-id="04ed3-146">"ルート。MapRoute()"上記のメソッド呼び出しが受信した Url を URL の形式を使用して、コント ローラー クラスにマップされる既定のルーティング規則を登録します:"/{controller}/{action}/{id}"、インスタンス化するコント ローラー クラスの名前が"controller"–"action"はの名前をパブリック メソッドを呼び出すし、"id"は、メソッドに引数として渡すことができる URL に埋め込まれた省略可能なパラメーターです。</span><span class="sxs-lookup"><span data-stu-id="04ed3-146">The "routes.MapRoute()" method call above registers a default routing rule that maps incoming URLs to controller classes using the URL format: "/{controller}/{action}/{id}" – where "controller" is the name of the controller class to instantiate, "action" is the name of a public method to invoke on it, and "id" is an optional parameter embedded within the URL that can be passed as an argument to the method.</span></span> <span data-ttu-id="04ed3-147">"MapRoute()"メソッドの呼び出しに渡される 3 番目のパラメーターは、一連の既定値は URL に存在しないことに、コント ローラーとアクション/id 値を使用する (コント ローラー アクションを"Home"= ="Index"、Id ="")。</span><span class="sxs-lookup"><span data-stu-id="04ed3-147">The third parameter passed to the "MapRoute()" method call is a set of default values to use for the controller/action/id values in the event that they are not present in the URL (Controller = "Home", Action="Index", Id="").</span></span>

<span data-ttu-id="04ed3-148">以下は Url のさまざまな方法を示すテーブルが既定値を使用してマップされている"<em>/{コント ローラー}/{action}/{id}"</em>ルート ルール。</span><span class="sxs-lookup"><span data-stu-id="04ed3-148">Below is a table that demonstrates how a variety of URLs are mapped using the default "<em>/{controllers}/{action}/{id}"</em>route rule:</span></span>

| <span data-ttu-id="04ed3-149">**URL**</span><span class="sxs-lookup"><span data-stu-id="04ed3-149">**URL**</span></span> | <span data-ttu-id="04ed3-150">**コント ローラー クラス**</span><span class="sxs-lookup"><span data-stu-id="04ed3-150">**Controller Class**</span></span> | <span data-ttu-id="04ed3-151">**アクション メソッド**</span><span class="sxs-lookup"><span data-stu-id="04ed3-151">**Action Method**</span></span> | <span data-ttu-id="04ed3-152">**渡されたパラメーター**</span><span class="sxs-lookup"><span data-stu-id="04ed3-152">**Parameters Passed**</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="04ed3-153">*/Dinners/Details/2*</span><span class="sxs-lookup"><span data-stu-id="04ed3-153">*/Dinners/Details/2*</span></span> | <span data-ttu-id="04ed3-154">DinnersController</span><span class="sxs-lookup"><span data-stu-id="04ed3-154">DinnersController</span></span> | <span data-ttu-id="04ed3-155">Details(id)</span><span class="sxs-lookup"><span data-stu-id="04ed3-155">Details(id)</span></span> | <span data-ttu-id="04ed3-156">id=2</span><span class="sxs-lookup"><span data-stu-id="04ed3-156">id=2</span></span> |
| <span data-ttu-id="04ed3-157">*/Dinners/Edit/5*</span><span class="sxs-lookup"><span data-stu-id="04ed3-157">*/Dinners/Edit/5*</span></span> | <span data-ttu-id="04ed3-158">DinnersController</span><span class="sxs-lookup"><span data-stu-id="04ed3-158">DinnersController</span></span> | <span data-ttu-id="04ed3-159">Edit(id)</span><span class="sxs-lookup"><span data-stu-id="04ed3-159">Edit(id)</span></span> | <span data-ttu-id="04ed3-160">id=5</span><span class="sxs-lookup"><span data-stu-id="04ed3-160">id=5</span></span> |
| <span data-ttu-id="04ed3-161">*/Dinners/Create*</span><span class="sxs-lookup"><span data-stu-id="04ed3-161">*/Dinners/Create*</span></span> | <span data-ttu-id="04ed3-162">DinnersController</span><span class="sxs-lookup"><span data-stu-id="04ed3-162">DinnersController</span></span> | <span data-ttu-id="04ed3-163">Create()</span><span class="sxs-lookup"><span data-stu-id="04ed3-163">Create()</span></span> | <span data-ttu-id="04ed3-164">N/A</span><span class="sxs-lookup"><span data-stu-id="04ed3-164">N/A</span></span> |
| <span data-ttu-id="04ed3-165">*/Dinners*</span><span class="sxs-lookup"><span data-stu-id="04ed3-165">*/Dinners*</span></span> | <span data-ttu-id="04ed3-166">DinnersController</span><span class="sxs-lookup"><span data-stu-id="04ed3-166">DinnersController</span></span> | <span data-ttu-id="04ed3-167">Index()</span><span class="sxs-lookup"><span data-stu-id="04ed3-167">Index()</span></span> | <span data-ttu-id="04ed3-168">N/A</span><span class="sxs-lookup"><span data-stu-id="04ed3-168">N/A</span></span> |
| <span data-ttu-id="04ed3-169">*/Home*</span><span class="sxs-lookup"><span data-stu-id="04ed3-169">*/Home*</span></span> | <span data-ttu-id="04ed3-170">HomeController</span><span class="sxs-lookup"><span data-stu-id="04ed3-170">HomeController</span></span> | <span data-ttu-id="04ed3-171">Index()</span><span class="sxs-lookup"><span data-stu-id="04ed3-171">Index()</span></span> | <span data-ttu-id="04ed3-172">N/A</span><span class="sxs-lookup"><span data-stu-id="04ed3-172">N/A</span></span> |
| */* | <span data-ttu-id="04ed3-173">HomeController</span><span class="sxs-lookup"><span data-stu-id="04ed3-173">HomeController</span></span> | <span data-ttu-id="04ed3-174">Index()</span><span class="sxs-lookup"><span data-stu-id="04ed3-174">Index()</span></span> | <span data-ttu-id="04ed3-175">N/A</span><span class="sxs-lookup"><span data-stu-id="04ed3-175">N/A</span></span> |

<span data-ttu-id="04ed3-176">最後の 3 つの行が既定値を表示する (コント ローラー = アクションでは、インデックス、Id を = ="") 使用されています。</span><span class="sxs-lookup"><span data-stu-id="04ed3-176">The last three rows show the default values (Controller = Home, Action = Index, Id = "") being used.</span></span> <span data-ttu-id="04ed3-177">1 つ指定されていない場合、既定のアクション名として"Index"メソッドが登録されているため、"/Dinners"し、そのコント ローラー クラスで呼び出される Url 原因 Index() アクション メソッドを"/home"。</span><span class="sxs-lookup"><span data-stu-id="04ed3-177">Because the "Index" method is registered as the default action name if one isn't specified, the "/Dinners" and "/Home" URLs cause the Index() action method to be invoked on their Controller classes.</span></span> <span data-ttu-id="04ed3-178">いずれかが指定されていない場合に既定のコント ローラーとして"Home"コント ローラーが登録されているため、「/」URL と、作成するテンプレートを使用して、Index() アクション メソッドを呼び出すことが。</span><span class="sxs-lookup"><span data-stu-id="04ed3-178">Because the "Home" controller is registered as the default controller if one isn't specified, the "/" URL causes the HomeController to be created, and the Index() action method on it to be invoked.</span></span>

<span data-ttu-id="04ed3-179">これら既定の URL ルーティング ルールしない場合は、良いニュースは簡単に変更するだけ、上記の RegisterRoutes メソッド内で編集されるは。</span><span class="sxs-lookup"><span data-stu-id="04ed3-179">If you don't like these default URL routing rules, the good news is that they are easy to change - just edit them within the RegisterRoutes method above.</span></span> <span data-ttu-id="04ed3-180">NerdDinner アプリケーションでは、既定の URL ルーティング ルールのいずれかを変更するつもり – 代わりにだけとして使用します-です。</span><span class="sxs-lookup"><span data-stu-id="04ed3-180">For our NerdDinner application, though, we aren't going to change any of the default URL routing rules – instead we'll just use them as-is.</span></span>

### <a name="using-the-dinnerrepository-from-our-dinnerscontroller"></a><span data-ttu-id="04ed3-181">DinnersController のときは必ず DinnerRepository を使用します。</span><span class="sxs-lookup"><span data-stu-id="04ed3-181">Using the DinnerRepository from our DinnersController</span></span>

<span data-ttu-id="04ed3-182">みましょう DinnersController の現在の実装に置き換える Index() と Details() のアクション メソッドをモデルを使用する実装。</span><span class="sxs-lookup"><span data-stu-id="04ed3-182">Let's now replace our current implementation of the DinnersController's Index() and Details() action methods with implementations that use our model.</span></span>

<span data-ttu-id="04ed3-183">動作を実装する前に作成したときは必ず DinnerRepository クラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="04ed3-183">We'll use the DinnerRepository class we built earlier to implement the behavior.</span></span> <span data-ttu-id="04ed3-184">"NerdDinner.Models"名前空間を参照する"using"ステートメントを追加することで開始して、フィールドとして、DinnerController クラスのときは、必ず DinnerRepository インスタンスを宣言しました行います。</span><span class="sxs-lookup"><span data-stu-id="04ed3-184">We'll begin by adding a "using" statement that references the "NerdDinner.Models" namespace, and then declare an instance of our DinnerRepository as a field on our DinnerController class.</span></span>

<span data-ttu-id="04ed3-185">この章の「依存関係の挿入」の概念を紹介およびのときは、必ず DinnerRepository インスタンスを作成しますので、– テストが、右側の見方をするときは必ず DinnerRepository より良い単体できるへの参照を取得するコント ローラーを表示しましたします次のようにインラインです。</span><span class="sxs-lookup"><span data-stu-id="04ed3-185">Later in this chapter we'll introduce the concept of "Dependency Injection" and show another way for our Controllers to obtain a reference to a DinnerRepository that enables better unit testing – but for right now we'll just create an instance of our DinnerRepository inline like below.</span></span>

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample3.cs)]

<span data-ttu-id="04ed3-186">取得したデータ モデル オブジェクトを使用して HTML 応答を生成する準備が整いました。</span><span class="sxs-lookup"><span data-stu-id="04ed3-186">Now we are ready to generate a HTML response back using our retrieved data model objects.</span></span>

### <a name="using-views-with-our-controller"></a><span data-ttu-id="04ed3-187">コント ローラーとビューを使用します。</span><span class="sxs-lookup"><span data-stu-id="04ed3-187">Using Views with our Controller</span></span>

<span data-ttu-id="04ed3-188">HTML をアセンブルし、使用して、アクション メソッド内でコードを記述することはできますが、 *Response.Write()* アプローチが迅速にかなり扱いにくくなります、クライアントに送信するヘルパー メソッド。</span><span class="sxs-lookup"><span data-stu-id="04ed3-188">While it is possible to write code within our action methods to assemble HTML and then use the *Response.Write()* helper method to send it back to the client, that approach becomes fairly unwieldy quickly.</span></span> <span data-ttu-id="04ed3-189">はるかに優れた方法がアプリケーションとデータ ロジック、DinnersController のアクション メソッド内でのみを実行して、HTML 形式の出力を担当する個別の「ビュー」テンプレートへの HTML 応答を表示するために必要なデータを渡すためにはことです。</span><span class="sxs-lookup"><span data-stu-id="04ed3-189">A much better approach is for us to only perform application and data logic inside our DinnersController action methods, and to then pass the data needed to render a HTML response to a separate "view" template that is responsible for outputting the HTML representation of it.</span></span> <span data-ttu-id="04ed3-190">すぐに表示されるよう、「表示」テンプレートは、通常の HTML マークアップとレンダリングの埋め込みコードの組み合わせを含むテキスト ファイルになります。</span><span class="sxs-lookup"><span data-stu-id="04ed3-190">As we'll see in a moment, a "view" template is a text file that typically contains a combination of HTML markup and embedded rendering code.</span></span>

<span data-ttu-id="04ed3-191">このビューの表示から、コント ローラー ロジックを分離するには、いくつかの大きな利点が表示されます。</span><span class="sxs-lookup"><span data-stu-id="04ed3-191">Separating our controller logic from our view rendering brings several big benefits.</span></span> <span data-ttu-id="04ed3-192">具体的には、アプリケーション コードと UI の書式設定/レンダリング コード間をクリア"関心の分離"を適用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="04ed3-192">In particular it helps enforce a clear "separation of concerns" between the application code and UI formatting/rendering code.</span></span> <span data-ttu-id="04ed3-193">これがずっと簡単単体テスト アプリケーション ロジックを分離する UI のレンダリング ロジックから。</span><span class="sxs-lookup"><span data-stu-id="04ed3-193">This makes it much easier to unit-test application logic in isolation from UI rendering logic.</span></span> <span data-ttu-id="04ed3-194">簡単にアプリケーション コードを変更することがなく UI レンダリング テンプレートを後で変更します。</span><span class="sxs-lookup"><span data-stu-id="04ed3-194">It makes it easier to later modify the UI rendering templates without having to make application code changes.</span></span> <span data-ttu-id="04ed3-195">そのやすく開発者および設計者は、プロジェクトの共同します。</span><span class="sxs-lookup"><span data-stu-id="04ed3-195">And it can make it easier for developers and designers to collaborate together on projects.</span></span>

<span data-ttu-id="04ed3-196">"Void"に代わりに"ActionResult"の戻り値の型の戻り値の型のことから、2 つのアクション メソッドのメソッド シグネチャを変更することで、HTML UI の応答を返信するビュー テンプレートを使用することを示す、DinnersController クラスを更新できます。</span><span class="sxs-lookup"><span data-stu-id="04ed3-196">We can update our DinnersController class to indicate that we want to use a view template to send back an HTML UI response by changing the method signatures of our two action methods from having a return type of "void" to instead have a return type of "ActionResult".</span></span> <span data-ttu-id="04ed3-197">呼び出し、 *View()* 以下のようなヘルパー メソッドを返すコント ローラーの基本クラスを"ViewResult"オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="04ed3-197">We can then call the *View()* helper method on the Controller base class to return back a "ViewResult" object like below:</span></span>

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample4.cs)]

<span data-ttu-id="04ed3-198">署名、 *View()* 上を使用しているヘルパー メソッドは以下のようになります。</span><span class="sxs-lookup"><span data-stu-id="04ed3-198">The signature of the *View()* helper method we are using above looks like below:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image7.png)

<span data-ttu-id="04ed3-199">最初のパラメーター、 *View()* ヘルパー メソッドは、HTML 応答を表示するために使用するビュー テンプレート ファイルの名前です。</span><span class="sxs-lookup"><span data-stu-id="04ed3-199">The first parameter to the *View()* helper method is the name of the view template file we want to use to render the HTML response.</span></span> <span data-ttu-id="04ed3-200">2 番目のパラメーターは、テンプレートの表示が HTML 応答を表示するために必要なデータを含むモデル オブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="04ed3-200">The second parameter is a model object that contains the data that the view template needs in order to render the HTML response.</span></span>

<span data-ttu-id="04ed3-201">呼び出す、Index() アクション メソッド内で、 *View()* ヘルパー メソッドとの dinners"Index"ビュー テンプレートを使用して、HTML の一覧を表示することを指定します。</span><span class="sxs-lookup"><span data-stu-id="04ed3-201">Within our Index() action method we are calling the *View()* helper method and indicating that we want to render an HTML listing of dinners using an "Index" view template.</span></span> <span data-ttu-id="04ed3-202">ビュー テンプレートからリストを生成する Dinner オブジェクトのシーケンスを渡しています。</span><span class="sxs-lookup"><span data-stu-id="04ed3-202">We are passing the view template a sequence of Dinner objects to generate the list from:</span></span>

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample5.cs)]

<span data-ttu-id="04ed3-203">Details() アクション メソッド内には、URL 内で指定された id を使用して Dinner オブジェクトを取得しようとします。</span><span class="sxs-lookup"><span data-stu-id="04ed3-203">Within our Details() action method we attempt to retrieve a Dinner object using the id provided within the URL.</span></span> <span data-ttu-id="04ed3-204">有効な Dinner のいわゆるが見つかった場合、 *View()* "Details"ビュー テンプレートを使用して取得した Dinner オブジェクトを表示することを示す、ヘルパー メソッド。</span><span class="sxs-lookup"><span data-stu-id="04ed3-204">If a valid Dinner is found we call the *View()* helper method, indicating we want to use a "Details" view template to render the retrieved Dinner object.</span></span> <span data-ttu-id="04ed3-205">役に立つエラー メッセージ"NotFound"のビュー テンプレートを使用して、Dinner が存在しないことを示す場合は、無効な夕食を要求すると、レンダリング (とオーバー ロードされたバージョンの*View()* ヘルパー メソッドをテンプレート名を受け取るだけです):</span><span class="sxs-lookup"><span data-stu-id="04ed3-205">If an invalid dinner is requested, we render a helpful error message that indicates that the Dinner doesn't exist using a "NotFound" view template (and an overloaded version of the *View()* helper method that just takes the template name):</span></span>

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample6.cs)]

<span data-ttu-id="04ed3-206">これで、"NotFound"、「詳細」と"Index"ビュー テンプレートを実装してみましょう。</span><span class="sxs-lookup"><span data-stu-id="04ed3-206">Let's now implement the "NotFound", "Details", and "Index" view templates.</span></span>

### <a name="implementing-the-notfound-view-template"></a><span data-ttu-id="04ed3-207">"NotFound"のビュー テンプレートの実装</span><span class="sxs-lookup"><span data-stu-id="04ed3-207">Implementing the "NotFound" View Template</span></span>

<span data-ttu-id="04ed3-208">まず、要求された dinner が見つからないことを示すわかりやすいエラー メッセージを表示する –"NotFound"のビュー テンプレートの実装します。</span><span class="sxs-lookup"><span data-stu-id="04ed3-208">We'll begin by implementing the "NotFound" view template – which displays a friendly error message indicating that the requested dinner can't be found.</span></span>

<span data-ttu-id="04ed3-209">コント ローラー アクション メソッド内にテキスト カーソルを配置し、新しいテンプレートの表示を作成し、右クリックし、(私たちも実行できるこのコマンド Ctrl M, ctrl + V」と入力して)「ビューの追加」メニュー コマンドを選択します。</span><span class="sxs-lookup"><span data-stu-id="04ed3-209">We'll create a new view template by positioning our text cursor within a controller action method, and then right click and choose the "Add View" menu command (we can also execute this command by typing Ctrl-M, Ctrl-V):</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image8.png)

<span data-ttu-id="04ed3-210">これは、次のような「ビューの追加」ダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="04ed3-210">This will bring up an "Add View" dialog like below.</span></span> <span data-ttu-id="04ed3-211">既定では、ダイアログ ボックスが作成するビューの名前を事前に設定のアクション メソッドの名前と一致する、カーソル時の (この例では「詳細」) では、ダイアログが起動されました。</span><span class="sxs-lookup"><span data-stu-id="04ed3-211">By default the dialog will pre-populate the name of the view to create to match the name of the action method the cursor was in when the dialog was launched (in this case "Details").</span></span> <span data-ttu-id="04ed3-212">まず、"NotFound"テンプレートを実装するため、このビューの名前をオーバーライドし、"NotFound"のように変更するように設定しましたします。</span><span class="sxs-lookup"><span data-stu-id="04ed3-212">Because we want to first implement the "NotFound" template, we'll override this view name and set it to instead be "NotFound":</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image9.png)

<span data-ttu-id="04ed3-213">[追加] ボタンをクリックすると、ときに Visual Studio は新しい"NotFound.aspx"ビュー テンプレートを (が、ディレクトリが既に存在しない場合も作成されます)、"\Views\Dinners"ディレクトリ内の作成します。</span><span class="sxs-lookup"><span data-stu-id="04ed3-213">When we click the "Add" button, Visual Studio will create a new "NotFound.aspx" view template for us within the "\Views\Dinners" directory (which it will also create if the directory doesn't already exist):</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image10.png)

<span data-ttu-id="04ed3-214">コード エディター内に、新しい"NotFound.aspx"ビュー テンプレートも開きます。</span><span class="sxs-lookup"><span data-stu-id="04ed3-214">It will also open up our new "NotFound.aspx" view template within the code-editor:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image11.png)

<span data-ttu-id="04ed3-215">既定ではテンプレートの表示がある 2 つ「コンテンツ領域」のコンテンツとコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="04ed3-215">View templates by default have two "content regions" where we can add content and code.</span></span> <span data-ttu-id="04ed3-216">1 つ目では、「タイトル」、HTML ページの返信をカスタマイズできます。</span><span class="sxs-lookup"><span data-stu-id="04ed3-216">The first allows us to customize the "title" of the HTML page sent back.</span></span> <span data-ttu-id="04ed3-217">2 番目の内容をカスタマイズする"メイン"の HTML ページに送信できます。</span><span class="sxs-lookup"><span data-stu-id="04ed3-217">The second allows us to customize the "main content" of the HTML page sent back.</span></span>

<span data-ttu-id="04ed3-218">"NotFound"のビュー テンプレートを実装するために、いくつかの基本的なコンテンツを追加します。</span><span class="sxs-lookup"><span data-stu-id="04ed3-218">To implement our "NotFound" view template we'll add some basic content:</span></span>

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample7.aspx)]

<span data-ttu-id="04ed3-219">試すことができますし、ブラウザー内で。</span><span class="sxs-lookup"><span data-stu-id="04ed3-219">We can then try it out within the browser.</span></span> <span data-ttu-id="04ed3-220">これを行うみましょう要求、 *「/Dinners/詳細/9999」* URL。</span><span class="sxs-lookup"><span data-stu-id="04ed3-220">To do this let's request the *"/Dinners/Details/9999"* URL.</span></span> <span data-ttu-id="04ed3-221">これは、データベースに現在存在しないし、"NotFound"のビュー テンプレートを表示するために、DinnersController.Details() アクション メソッドが発生する dinner をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="04ed3-221">This will refer to a dinner that doesn't currently exist in the database, and will cause our DinnersController.Details() action method to render our "NotFound" view template:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image12.png)

<span data-ttu-id="04ed3-222">1 つに気付くことで、上記のスクリーン ショットは、基本的なビュー テンプレートには、一連の画面にメイン コンテンツを囲む HTML が継承されてます。</span><span class="sxs-lookup"><span data-stu-id="04ed3-222">One thing you'll notice in the screen-shot above is that our basic view template has inherited a bunch of HTML that surrounds the main content on the screen.</span></span> <span data-ttu-id="04ed3-223">これは、このビュー テンプレートが、サイト上のすべてのビューで一貫性のあるレイアウトを適用できる「マスター ページ」テンプレートを使用しているためにです。</span><span class="sxs-lookup"><span data-stu-id="04ed3-223">This is because our view-template is using a "master page" template that enables us to apply a consistent layout across all views on the site.</span></span> <span data-ttu-id="04ed3-224">マスター ページのこのチュートリアルの後半で複数のしくみについて説明します。</span><span class="sxs-lookup"><span data-stu-id="04ed3-224">We'll discuss how master pages work more in a later part of this tutorial.</span></span>

### <a name="implementing-the-details-view-template"></a><span data-ttu-id="04ed3-225">「詳細」のビュー テンプレートの実装</span><span class="sxs-lookup"><span data-stu-id="04ed3-225">Implementing the "Details" View Template</span></span>

<span data-ttu-id="04ed3-226">「詳細」のビュー テンプレート – Dinner モデルが 1 つの HTML が生成されますを今すぐ実装してみましょう。</span><span class="sxs-lookup"><span data-stu-id="04ed3-226">Let's now implement the "Details" view template – which will generate HTML for a single Dinner model.</span></span>

<span data-ttu-id="04ed3-227">私たちします詳細アクション メソッド内でテキスト カーソルを配置し、右クリックし"ビューの追加 メニューのコマンドを選択するか Ctrl M, ctrl + V キーを押します)。</span><span class="sxs-lookup"><span data-stu-id="04ed3-227">We'll do this by positioning our text cursor within the Details action method, and then right click and choose the "Add View" menu command (or press Ctrl-M, Ctrl-V):</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image13.png)

<span data-ttu-id="04ed3-228">「ビューの追加」ダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="04ed3-228">This will bring up the "Add View" dialog.</span></span> <span data-ttu-id="04ed3-229">既定のビュー名 (「詳細」) にしておきます。</span><span class="sxs-lookup"><span data-stu-id="04ed3-229">We'll keep the default view name ("Details").</span></span> <span data-ttu-id="04ed3-230">ダイアログ ボックスで [厳密に型指定されたビューを作成する] チェック ボックスをオンもされ、コント ローラーからビューに渡されるモデルの種類の名前を (コンボ ボックスのドロップダウンを使用して) を選択します。</span><span class="sxs-lookup"><span data-stu-id="04ed3-230">We'll also select the "Create a strongly-typed View" checkbox in the dialog and select (using the combobox dropdown) the name of the model type we are passing from the Controller to the View.</span></span> <span data-ttu-id="04ed3-231">このビューは、Dinner オブジェクトを渡しています (この型の完全修飾名です:"NerdDinner.Models.Dinner")。</span><span class="sxs-lookup"><span data-stu-id="04ed3-231">For this view we are passing a Dinner object (the fully qualified name for this type is: "NerdDinner.Models.Dinner"):</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image14.png)

<span data-ttu-id="04ed3-232">私たちが付けた"空"ビューを作成する、以前のテンプレートとは異なりを自動的に選択して、この時間「スキャフォールディング」"Details"テンプレートを使用して表示します。</span><span class="sxs-lookup"><span data-stu-id="04ed3-232">Unlike the previous template, where we chose to create an "Empty View", this time we will choose to automatically "scaffold" the view using a "Details" template.</span></span> <span data-ttu-id="04ed3-233">これは、前述のダイアログ ボックスで「コンテンツの表示」のドロップダウン リストを変更することで指定できます。</span><span class="sxs-lookup"><span data-stu-id="04ed3-233">We can indicate this by changing the "View content" drop-down in the dialog above.</span></span>

<span data-ttu-id="04ed3-234">「スキャフォールディング」を渡している Dinner オブジェクトに基づいて、詳細ビュー テンプレートの最初の実装が生成されます。</span><span class="sxs-lookup"><span data-stu-id="04ed3-234">"Scaffolding" will generate an initial implementation of our details view template based on the Dinner object we are passing to it.</span></span> <span data-ttu-id="04ed3-235">これは、すぐに、ビュー テンプレートの実装を開始するための簡単な方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="04ed3-235">This provides an easy way for us to quickly get started on our view template implementation.</span></span>

<span data-ttu-id="04ed3-236">[追加] ボタンをクリックすると、ときに Visual Studio は新しい"Details.aspx"ビュー テンプレート ファイルを"\Views\Dinners"ディレクトリ内の作成します。</span><span class="sxs-lookup"><span data-stu-id="04ed3-236">When we click the "Add" button, Visual Studio will create a new "Details.aspx" view template file for us within our "\Views\Dinners" directory:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image15.png)

<span data-ttu-id="04ed3-237">コード エディター内で、新しい"Details.aspx"ビュー テンプレートも開きます。</span><span class="sxs-lookup"><span data-stu-id="04ed3-237">It will also open up our new "Details.aspx" view template within the code-editor.</span></span> <span data-ttu-id="04ed3-238">Dinner モデルに基づいて詳細ビューのスキャフォールディングの初期実装が含まれます。</span><span class="sxs-lookup"><span data-stu-id="04ed3-238">It will contain an initial scaffold implementation of a details view based on a Dinner model.</span></span> <span data-ttu-id="04ed3-239">スキャフォールディング エンジンは、.NET リフレクションを使用して、渡されたクラスで公開されているパブリック プロパティを確認し、見つけたの各種類に基づく適切なコンテンツを追加します。</span><span class="sxs-lookup"><span data-stu-id="04ed3-239">The scaffolding engine uses .NET reflection to look at the public properties exposed on the class passed it, and will add appropriate content based on each type it finds:</span></span>

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample8.aspx)]

<span data-ttu-id="04ed3-240">*「/1/Dinners/詳細」* ブラウザーでこの「詳細」のスキャフォールディングの実装がどのように表示する URL。</span><span class="sxs-lookup"><span data-stu-id="04ed3-240">We can request the *"/Dinners/Details/1"* URL to see what this "details" scaffold implementation looks like in the browser.</span></span> <span data-ttu-id="04ed3-241">この URL を使用して手動で追加したデータベースに最初に作成したときに、dinners の 1 つ表示されます。</span><span class="sxs-lookup"><span data-stu-id="04ed3-241">Using this URL will display one of the dinners we manually added to our database when we first created it:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image16.png)

<span data-ttu-id="04ed3-242">これにより、すばやく作業を開始、私たちを取得し、この Details.aspx ビューの最初の実装を提供します。</span><span class="sxs-lookup"><span data-stu-id="04ed3-242">This gets us up and running quickly, and provides us with an initial implementation of our Details.aspx view.</span></span> <span data-ttu-id="04ed3-243">移動しを満足度に UI をカスタマイズすることを調整できます。</span><span class="sxs-lookup"><span data-stu-id="04ed3-243">We can then go and tweak it to customize the UI to our satisfaction.</span></span>

<span data-ttu-id="04ed3-244">もっとよく Details.aspx テンプレートを見てみると、静的な HTML が含まれていますと、レンダリング コードを埋め込みわかります。</span><span class="sxs-lookup"><span data-stu-id="04ed3-244">When we look at the Details.aspx template more closely, we'll find that it contains static HTML as well as embedded rendering code.</span></span> <span data-ttu-id="04ed3-245">&lt;%%&gt;コード ナゲットはビュー テンプレートをレンダリングするときにコードを実行し、 &lt;% = %&gt;コード ナゲットがそれに含まれるコードを実行し、テンプレートの出力ストリームに結果をレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="04ed3-245">&lt;% %&gt; code nuggets execute code when the view template renders, and &lt;%= %&gt; code nuggets execute the code contained within them and then render the result to the output stream of the template.</span></span>

<span data-ttu-id="04ed3-246">厳密に型指定された"Model"プロパティを使用して、コント ローラーから渡された"Dinner"モデル オブジェクトにアクセスする、ビュー内でコードを記述できます。</span><span class="sxs-lookup"><span data-stu-id="04ed3-246">We can write code within our View that accesses the "Dinner" model object that was passed from our controller using a strongly-typed "Model" property.</span></span> <span data-ttu-id="04ed3-247">Visual Studio により、完全なコードの intellisense、エディター内での「モデル」このプロパティにアクセスする場合。</span><span class="sxs-lookup"><span data-stu-id="04ed3-247">Visual Studio provides us with full code-intellisense when accessing this "Model" property within the editor:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image17.png)

<span data-ttu-id="04ed3-248">いくつかの修正は、最後の詳細ビュー テンプレートのソースは次のようになりますようにしましょう。</span><span class="sxs-lookup"><span data-stu-id="04ed3-248">Let's make some tweaks so that the source for our final Details view template looks like below:</span></span>

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample9.aspx)]

<span data-ttu-id="04ed3-249">アクセスして、 *「/1/Dinners/詳細」* URL もう一度これは、次のようなレンダリング。</span><span class="sxs-lookup"><span data-stu-id="04ed3-249">When we access the *"/Dinners/Details/1"* URL again it will now render like below:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image18.png)

### <a name="implementing-the-index-view-template"></a><span data-ttu-id="04ed3-250">"Index"のビュー テンプレートの実装</span><span class="sxs-lookup"><span data-stu-id="04ed3-250">Implementing the "Index" View Template</span></span>

<span data-ttu-id="04ed3-251">今すぐ – 今後 Dinners の一覧を生成する"Index"ビュー テンプレートを実装してみましょう。</span><span class="sxs-lookup"><span data-stu-id="04ed3-251">Let's now implement the "Index" view template – which will generate a listing of upcoming Dinners.</span></span> <span data-ttu-id="04ed3-252">To-do これをインデックス アクション メソッド内にテキスト カーソルを配置し、右 をクリックし、"ビューの追加 メニューのコマンドを選択 (または Ctrl M, ctrl + V キーを押します)。</span><span class="sxs-lookup"><span data-stu-id="04ed3-252">To-do this we'll position our text cursor within the Index action method, and then right click and choose the "Add View" menu command (or press Ctrl-M, Ctrl-V).</span></span>

<span data-ttu-id="04ed3-253">"ビューの追加 ダイアログ ボックスで"Index"という名前のテンプレートの表示の維持がされ"厳密に型指定されたビューを作成する チェック ボックスを選択します。</span><span class="sxs-lookup"><span data-stu-id="04ed3-253">Within the "Add View" dialog we'll keep the view template named "Index" and select the "Create a strongly-typed view" checkbox.</span></span> <span data-ttu-id="04ed3-254">この時間を自動的に"List"ビュー テンプレートを生成し、"NerdDinner.Models.Dinner"モデルの種類として、ビューに渡されるを選択します (このスキャフォールディングと、ビューの追加ダイアログが前提としていますが、"List"を作成することによって示されたがためシーケンスを渡して Dinner オブジェクトのコント ローラーからビューに)。</span><span class="sxs-lookup"><span data-stu-id="04ed3-254">This time we will choose to automatically generate a "List" view template, and select "NerdDinner.Models.Dinner" as the model type passed to the view (which because we have indicated we are creating a "List" scaffold will cause the Add View dialog to assume we are passing a sequence of Dinner objects from our Controller to the View):</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image19.png)

<span data-ttu-id="04ed3-255">[追加] ボタンをクリックすると、ときに Visual Studio は新しい"Index.aspx"ビュー テンプレート ファイルを"\Views\Dinners"ディレクトリ内を作成します。</span><span class="sxs-lookup"><span data-stu-id="04ed3-255">When we click the "Add" button, Visual Studio will create a new "Index.aspx" view template file for us within our "\Views\Dinners" directory.</span></span> <span data-ttu-id="04ed3-256">これは「スキャフォールディング」ビューに渡す Dinners の HTML テーブルの一覧を提供する内部に初期実装。</span><span class="sxs-lookup"><span data-stu-id="04ed3-256">It will "scaffold" an initial implementation within it that provides an HTML table listing of the Dinners we pass to the view.</span></span>

<span data-ttu-id="04ed3-257">アプリケーションとアクセスを実行したときに、 *「Dinners/」* URL dinners の一覧を表示するようになります。</span><span class="sxs-lookup"><span data-stu-id="04ed3-257">When we run the application and access the *"/Dinners/"* URL it will render our list of dinners like so:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image20.png)

<span data-ttu-id="04ed3-258">上記のテーブル ソリューションでは、Dinner データ – これは非常に必要な Dinner リストに接続する、コンシューマーのグリッドのようなレイアウトをによりします。</span><span class="sxs-lookup"><span data-stu-id="04ed3-258">The table solution above gives us a grid-like layout of our Dinner data – which isn't quite what we want for our consumer facing Dinner listing.</span></span> <span data-ttu-id="04ed3-259">Index.aspx ビュー テンプレートを更新し、データのより少ない列を一覧表示して使用するように変更を&lt;ul&gt;レンダリングして、次のコードを使用してテーブルではなく要素。</span><span class="sxs-lookup"><span data-stu-id="04ed3-259">We can update the Index.aspx view template and modify it to list fewer columns of data, and use a &lt;ul&gt; element to render them instead of a table using the code below:</span></span>

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample10.aspx)]

<span data-ttu-id="04ed3-260">このモデルで各 dinner 経由でループと上記の foreach ステートメント内で"の var"キーワードを使用しています。</span><span class="sxs-lookup"><span data-stu-id="04ed3-260">We are using the "var" keyword within the above foreach statement as we loop over each dinner in our Model.</span></span> <span data-ttu-id="04ed3-261">Dinner オブジェクトがある遅延バインディングを意味"の var"を使用して c# 3.0 で未知のものと考えるかもしれません。</span><span class="sxs-lookup"><span data-stu-id="04ed3-261">Those unfamiliar with C# 3.0 might think that using "var" means that the dinner object is late-bound.</span></span> <span data-ttu-id="04ed3-262">コンパイラが厳密に型指定された「モデル」プロパティに対して型推論を使用している代わりに意味 (型の"IEnumerable&lt;Dinner&gt;") といっぱいになることを意味する – Dinner の種類としてローカル"dinner"変数のコンパイルintellisense とコンパイル時のコード ブロック内でチェックします。</span><span class="sxs-lookup"><span data-stu-id="04ed3-262">It instead means that the compiler is using type-inference against the strongly typed "Model" property (which is of type "IEnumerable&lt;Dinner&gt;") and compiling the local "dinner" variable as a Dinner type – which means we get full intellisense and compile-time checking for it within code blocks:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image21.png)

<span data-ttu-id="04ed3-263">更新をヒットしました、 */Dinners*以下のような更新されたビューの現時点では、ブラウザーで URL:</span><span class="sxs-lookup"><span data-stu-id="04ed3-263">When we hit refresh on the */Dinners* URL in our browser our updated view now looks like below:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image22.png)

<span data-ttu-id="04ed3-264">これにより、ほうが検索しているはまだ完全にあります。</span><span class="sxs-lookup"><span data-stu-id="04ed3-264">This is looking better – but isn't entirely there yet.</span></span> <span data-ttu-id="04ed3-265">最後に、エンドユーザー、リスト内の個々 の Dinners をクリックし、それらについての詳細を参照してください。</span><span class="sxs-lookup"><span data-stu-id="04ed3-265">Our last step is to enable end-users to click individual Dinners in the list and see details about them.</span></span> <span data-ttu-id="04ed3-266">これで、DinnersController の詳細アクション メソッドにリンクする HTML ハイパーリンク要素をレンダリングすることにより実装します。</span><span class="sxs-lookup"><span data-stu-id="04ed3-266">We'll implement this by rendering HTML hyperlink elements that link to the Details action method on our DinnersController.</span></span>

<span data-ttu-id="04ed3-267">2 つの方法のいずれかで、インデックス ビュー内でこれらのハイパーリンクを生成しています。</span><span class="sxs-lookup"><span data-stu-id="04ed3-267">We can generate these hyperlinks within our Index view in one of two ways.</span></span> <span data-ttu-id="04ed3-268">1 つは、HTML を手動で作成する&lt;、&gt;などの要素以下、場所を埋め込む&lt;%%&gt;ブロック内で、 &lt;、&gt; HTML 要素。</span><span class="sxs-lookup"><span data-stu-id="04ed3-268">The first is to manually create HTML &lt;a&gt; elements like below, where we embed &lt;% %&gt; blocks within the &lt;a&gt; HTML element:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image23.png)

<span data-ttu-id="04ed3-269">使用して別のアプローチは、HTML をプログラムで作成をサポートする ASP.NET MVC 内で組み込みの"Html.ActionLink()"ヘルパー メソッドを活用するために、 &lt;、&gt;で別のアクション メソッドにリンクする要素、コント ローラー:</span><span class="sxs-lookup"><span data-stu-id="04ed3-269">An alternative approach we can use is to take advantage of the built-in "Html.ActionLink()" helper method within ASP.NET MVC that supports programmatically creating an HTML &lt;a&gt; element that links to another action method on a Controller:</span></span>

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample11.aspx)]

<span data-ttu-id="04ed3-270">Html.ActionLink() のヘルパー メソッドの最初のパラメーターを表示するリンク テキストは、(ここでは、タイトル、dinner の) 2 番目のパラメーター (この場合、詳細メソッド) へのリンクを生成するコント ローラー アクション名は、3 番目のパラメーター(プロパティの名前/値を持つ匿名型として実装)、アクションに送信するパラメーターのセット。</span><span class="sxs-lookup"><span data-stu-id="04ed3-270">The first parameter to the Html.ActionLink() helper method is the link-text to display (in this case the title of the dinner), the second parameter is the Controller action name we want to generate the link to (in this case the Details method), and the third parameter is a set of parameters to send to the action (implemented as an anonymous type with property name/values).</span></span> <span data-ttu-id="04ed3-271">ここでおにリンクするため、既定の URL ルーティング ルールの ASP.NET MVC では、dinner の"id"パラメーターを指定することは"{controller}/{action}/{id}"Html.ActionLink() ヘルパー メソッドは、次の出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="04ed3-271">In this case we are specifying the "id" parameter of the dinner we want to link to, and because the default URL routing rule in ASP.NET MVC is "{Controller}/{Action}/{id}" the Html.ActionLink() helper method will generate the following output:</span></span>

[!code-html[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample12.html)]

<span data-ttu-id="04ed3-272">Index.aspx ビュー Html.ActionLink() ヘルパー メソッドのアプローチを使用して各 dinner が適切な詳細情報の URL を一覧のリンクであるなります。</span><span class="sxs-lookup"><span data-stu-id="04ed3-272">For our Index.aspx view we'll use the Html.ActionLink() helper method approach and have each dinner in the list link to the appropriate details URL:</span></span>

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample13.aspx)]

<span data-ttu-id="04ed3-273">今すぐおをヒットした時と、 */Dinners* dinner 一覧は、次のように次の URL:</span><span class="sxs-lookup"><span data-stu-id="04ed3-273">And now when we hit the */Dinners* URL our dinner list looks like below:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image24.png)

<span data-ttu-id="04ed3-274">一覧でディナーのいずれかをクリックしたとき詳細が表示に移動します。</span><span class="sxs-lookup"><span data-stu-id="04ed3-274">When we click any of the Dinners in the list we'll navigate to see details about it:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image25.png)

### <a name="convention-based-naming-and-the-views-directory-structure"></a><span data-ttu-id="04ed3-275">名前付け規則に基づくと \Views ディレクトリ構造</span><span class="sxs-lookup"><span data-stu-id="04ed3-275">Convention-based naming and the \Views directory structure</span></span>

<span data-ttu-id="04ed3-276">既定では ASP.NET MVC アプリケーションでは、テンプレートの表示を解決するときに、名前付け構造、規則ベースのディレクトリを使用します。</span><span class="sxs-lookup"><span data-stu-id="04ed3-276">ASP.NET MVC applications by default use a convention-based directory naming structure when resolving view templates.</span></span> <span data-ttu-id="04ed3-277">これにより、開発者は完全修飾の場所のパスから、コント ローラー クラス内でのビューを参照するときにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="04ed3-277">This allows developers to avoid having to fully-qualify a location path when referencing views from within a Controller class.</span></span> <span data-ttu-id="04ed3-278">既定では ASP.NET MVC ビュー テンプレート ファイル内の検索は、* \Views\[ControllerName]\*アプリケーションの下にあるディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="04ed3-278">By default ASP.NET MVC will look for the view template file within the *\Views\[ControllerName]\* directory underneath the application.</span></span>

<span data-ttu-id="04ed3-279">たとえば、次の 3 つのビュー テンプレートを明示的に参照する – DinnersController クラスに操作している:"Index"、「詳細」および"NotFound"。</span><span class="sxs-lookup"><span data-stu-id="04ed3-279">For example, we've been working on the DinnersController class – which explicitly references three view templates: "Index", "Details" and "NotFound".</span></span> <span data-ttu-id="04ed3-280">ASP.NET MVC は既定で検索内でこれらのビュー、 *\Views\Dinners*アプリケーションのルート ディレクトリの下にあるディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="04ed3-280">ASP.NET MVC will by default look for these views within the *\Views\Dinners* directory underneath our application root directory:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image26.png)

<span data-ttu-id="04ed3-281">プロジェクト内の 3 つのコント ローラー クラスのいる方法がありますの上に注意してください (DinnersController、HomeController と AccountController – プロジェクトを作成したときに、最後の 2 つが既定で追加された)、3 つのサブディレクトリ (1 つずつ、コント ローラー) \Views ディレクトリ内。</span><span class="sxs-lookup"><span data-stu-id="04ed3-281">Notice above how there are currently three controller classes within the project (DinnersController, HomeController and AccountController – the last two were added by default when we created the project), and there are three sub-directories (one for each controller) within the \Views directory.</span></span>

<span data-ttu-id="04ed3-282">ホームおよびアカウント コント ローラーから参照されているビューは、それぞれから、ビュー テンプレートを自動的に解決*\Views\Home*と*\Views\Account*ディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="04ed3-282">Views referenced from the Home and Accounts controllers will automatically resolve their view templates from the respective *\Views\Home* and *\Views\Account* directories.</span></span> <span data-ttu-id="04ed3-283">*\Views\Shared*サブディレクトリが、アプリケーション内で複数のコント ローラー間で再利用されるビュー テンプレートを格納する方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="04ed3-283">The *\Views\Shared* sub-directory provides a way to store view templates that are re-used across multiple controllers within the application.</span></span> <span data-ttu-id="04ed3-284">内でチェックが最初に ASP.NET MVC がビュー テンプレートを解決しようとした場合、 *\Views\[コント ローラー]* 特定のディレクトリにビュー テンプレートを見つけられない場合がありますよ内で、 *\Views\共有*ディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="04ed3-284">When ASP.NET MVC attempts to resolve a view template, it will first check within the *\Views\[Controller]* specific directory, and if it can't find the view template there it will look within the *\Views\Shared* directory.</span></span>

<span data-ttu-id="04ed3-285">個々 のビュー テンプレートの名前付けに関しては、推奨されるガイダンスが、ビュー テンプレートを表示するためにその原因となったアクション メソッドと同じ名前を共有します。</span><span class="sxs-lookup"><span data-stu-id="04ed3-285">When it comes to naming individual view templates, the recommended guidance is to have the view template share the same name as the action method that caused it to render.</span></span> <span data-ttu-id="04ed3-286">たとえば、「インデックス」上のアクション メソッドがビューの結果を表示するために"Index"ビューを使用して、"Details"アクション メソッドは、その結果を表示するために「詳細」ビューを使用しています。</span><span class="sxs-lookup"><span data-stu-id="04ed3-286">For example, above our "Index" action method is using the "Index" view to render the view result, and the "Details" action method is using the "Details" view to render its results.</span></span> <span data-ttu-id="04ed3-287">これにより、簡単にすばやく参照するテンプレートは、各アクションに関連付けられています。</span><span class="sxs-lookup"><span data-stu-id="04ed3-287">This makes it easy to quickly see which template is associated with each action.</span></span>

<span data-ttu-id="04ed3-288">開発者は、ビュー テンプレートがコント ローラーで呼び出されるアクション メソッドと同じ名前を持つときにビュー テンプレートの名前を明示的に指定する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="04ed3-288">Developers do not need to explicitly specify the view template name when the view template has the same name as the action method being invoked on the controller.</span></span> <span data-ttu-id="04ed3-289">代わりにだけ、モデル オブジェクトに渡す"View()"のヘルパー メソッド (ビューの名前を指定)、なしと ASP.NET MVC を使用することが自動的に推測、 *\Views\[ControllerName]\[ActionName]* ビュー テンプレートのレンダリングのディスクにします。</span><span class="sxs-lookup"><span data-stu-id="04ed3-289">We can instead just pass the model object to the "View()" helper method (without specifying the view name), and ASP.NET MVC will automatically infer that we want to use the *\Views\[ControllerName]\[ActionName]* view template on disk to render it.</span></span>

<span data-ttu-id="04ed3-290">これにより、コント ローラーのコードを少しクリーンアップし、2 回、コード内の名前の重複を避けます。</span><span class="sxs-lookup"><span data-stu-id="04ed3-290">This allows us to clean up our controller code a little, and avoid duplicating the name twice in our code:</span></span>

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample14.cs)]

<span data-ttu-id="04ed3-291">上記のコードは、サイトの便利な Dinner リスティング/詳細を実装するために必要なすべてが発生します。</span><span class="sxs-lookup"><span data-stu-id="04ed3-291">The above code is all that is needed to implement a nice Dinner listing/details experience for the site.</span></span>

#### <a name="next-step"></a><span data-ttu-id="04ed3-292">次の手順</span><span class="sxs-lookup"><span data-stu-id="04ed3-292">Next Step</span></span>

<span data-ttu-id="04ed3-293">ブラウズ エクスペリエンスの構築の優れた Dinner があるようになりました。</span><span class="sxs-lookup"><span data-stu-id="04ed3-293">We now have a nice Dinner browsing experience built.</span></span>

<span data-ttu-id="04ed3-294">みましょう CRUD (作成、読み取り、更新、削除) データ フォームの編集のサポートを今すぐ有効にします。</span><span class="sxs-lookup"><span data-stu-id="04ed3-294">Let's now enable CRUD (Create, Read, Update, Delete) data form editing support.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="04ed3-295">[前へ](build-a-model-with-business-rule-validations.md)
> [次へ](provide-crud-create-read-update-delete-data-form-entry-support.md)</span><span class="sxs-lookup"><span data-stu-id="04ed3-295">[Previous](build-a-model-with-business-rule-validations.md)
[Next](provide-crud-create-read-update-delete-data-form-entry-support.md)</span></span>
