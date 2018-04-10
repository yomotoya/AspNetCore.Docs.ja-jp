---
uid: mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
title: コント ローラーとビューを使用して、一覧と詳細の UI を実装する |Microsoft ドキュメント
author: microsoft
description: 手順 4 では、ユーザー データの一覧/詳細ナビゲーション エクスペリエンスを提供する、モデルの活用したアプリケーションに、コント ローラーを追加する方法を示します.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 64116e56-1c9a-4f07-8097-bb36cbb6e57f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
msc.type: authoredcontent
ms.openlocfilehash: ac3568941eeef24bd9857c5787471aadea15fc7f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="use-controllers-and-views-to-implement-a-listingdetails-ui"></a><span data-ttu-id="86597-103">コント ローラーとビューを使用して、一覧と詳細の UI を実装するには</span><span class="sxs-lookup"><span data-stu-id="86597-103">Use Controllers and Views to Implement a Listing/Details UI</span></span>
====================
<span data-ttu-id="86597-104">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="86597-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="86597-105">PDF をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="86597-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="86597-106">これは、無料の手順 4 ["NerdDinner"アプリケーションのチュートリアル](introducing-the-nerddinner-tutorial.md)をウォーク-にする方法を小規模の構築が完了すると、ASP.NET MVC 1 を使用して web アプリケーションです。</span><span class="sxs-lookup"><span data-stu-id="86597-106">This is step 4 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="86597-107">手順 4 では、ユーザーについては、NerdDinner サイト ディナー用のデータ一覧/詳細ナビゲーション操作に提供する、モデルの活用したアプリケーションに、コント ローラーを追加する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="86597-107">Step 4 shows how to add a Controller to the application that takes advantage of our model to provide users with a data listing/details navigation experience for dinners on our NerdDinner site.</span></span>
> 
> <span data-ttu-id="86597-108">ASP.NET MVC 3 を使用している場合ことをお勧めする、 [MVC 3 の開始と取得](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)または[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)チュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="86597-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-4-controllers-and-views"></a><span data-ttu-id="86597-109">NerdDinner 手順 4: コント ローラーとビュー</span><span class="sxs-lookup"><span data-stu-id="86597-109">NerdDinner Step 4: Controllers and Views</span></span>

<span data-ttu-id="86597-110">従来の web フレームワーク (classic ASP、PHP、ASP.NET Web フォームなど) では、受信 Url は通常、ディスク上のファイルにマップします。</span><span class="sxs-lookup"><span data-stu-id="86597-110">With traditional web frameworks (classic ASP, PHP, ASP.NET Web Forms, etc), incoming URLs are typically mapped to files on disk.</span></span> <span data-ttu-id="86597-111">例: などの URL に対する要求"/繰り返し"または"/Products.php"、「繰り返し」または"Products.php"ファイルによって処理される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="86597-111">For example: a request for a URL like "/Products.aspx" or "/Products.php" might be processed by a "Products.aspx" or "Products.php" file.</span></span>

<span data-ttu-id="86597-112">MVC フレームワークの web ベースでは、少し異なる方法で、Url がサーバー コードをマップします。</span><span class="sxs-lookup"><span data-stu-id="86597-112">Web-based MVC frameworks map URLs to server code in a slightly different way.</span></span> <span data-ttu-id="86597-113">受信 Url をファイルにマップするには、代わりには、代わりに、クラスのメソッドに Url をマップします。</span><span class="sxs-lookup"><span data-stu-id="86597-113">Instead of mapping incoming URLs to files, they instead map URLs to methods on classes.</span></span> <span data-ttu-id="86597-114">これらのクラスは「コント ローラー」と呼ばれますとユーザー入力の処理、入力方向の HTTP 要求の処理を担当している、クライアント バックアップを取得して、データを保存および送信する応答を決定する (HTML を表示、ファイルをダウンロードする、別のサーバーに対するリダイレクトURL など)。</span><span class="sxs-lookup"><span data-stu-id="86597-114">These classes are called "Controllers" and they are responsible for processing incoming HTTP requests, handling user input, retrieving and saving data, and determining the response to send back to the client (display HTML, download a file, redirect to a different URL, etc).</span></span>

<span data-ttu-id="86597-115">これで、NerdDinner アプリケーションの基本的なモデルを作成した、次の手順はディナーのデータの一覧/詳細ナビゲーション操作については、サイトをユーザーに提供するの利用するアプリケーションに、コント ローラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="86597-115">Now that we have built up a basic model for our NerdDinner application, our next step will be to add a Controller to the application that takes advantage of it to provide users with a data listing/details navigation experience for Dinners on our site.</span></span>

### <a name="adding-a-dinnerscontroller-controller"></a><span data-ttu-id="86597-116">DinnersController コント ローラーの追加</span><span class="sxs-lookup"><span data-stu-id="86597-116">Adding a DinnersController Controller</span></span>

<span data-ttu-id="86597-117">当社の web プロジェクト内の「コント ローラー」フォルダーを右クリックして作業を開始し、[おします、**追加 -&gt;コント ローラー** (もコマンドを実行できるこの Ctrl M, ctrl + C を入力して)] メニューのコマンド。</span><span class="sxs-lookup"><span data-stu-id="86597-117">We'll begin by right-clicking on the "Controllers" folder within our web project, and then select the **Add-&gt;Controller** menu command (you can also execute this command by typing Ctrl-M, Ctrl-C):</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image1.png)

<span data-ttu-id="86597-118">「コント ローラーの追加」ダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="86597-118">This will bring up the "Add Controller" dialog:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image2.png)

<span data-ttu-id="86597-119">新しいコント ローラー"DinnersController"という名前を「追加」ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="86597-119">We'll name the new controller "DinnersController" and click the "Add" button.</span></span> <span data-ttu-id="86597-120">Visual Studio によっては、追加してから、\Controllers ディレクトリの下の DinnersController.cs ファイルは。</span><span class="sxs-lookup"><span data-stu-id="86597-120">Visual Studio will then add a DinnersController.cs file under our \Controllers directory:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image3.png)

<span data-ttu-id="86597-121">コード エディター内で新しい DinnersController クラスが開放されるもします。</span><span class="sxs-lookup"><span data-stu-id="86597-121">It will also open up the new DinnersController class within the code-editor.</span></span>

### <a name="adding-index-and-details-action-methods-to-the-dinnerscontroller-class"></a><span data-ttu-id="86597-122">DinnersController クラスに Index() と Details() アクション メソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="86597-122">Adding Index() and Details() Action Methods to the DinnersController Class</span></span>

<span data-ttu-id="86597-123">アプリケーションを使用して今後ディナーの一覧を参照し、それに関する特定の詳細を表示する一覧で任意の Dinner をクリックすることの訪問者を有効にします。</span><span class="sxs-lookup"><span data-stu-id="86597-123">We want to enable visitors using our application to browse a list of upcoming dinners, and allow them to click on any Dinner in the list to see specific details about it.</span></span> <span data-ttu-id="86597-124">アプリケーションから、次の Url を発行することによってこの作業を行うします。</span><span class="sxs-lookup"><span data-stu-id="86597-124">We'll do this by publishing the following URLs from our application:</span></span>

| <span data-ttu-id="86597-125">**URL**</span><span class="sxs-lookup"><span data-stu-id="86597-125">**URL**</span></span> | <span data-ttu-id="86597-126">**目的**</span><span class="sxs-lookup"><span data-stu-id="86597-126">**Purpose**</span></span> |
| --- | --- |
| <span data-ttu-id="86597-127">*/Dinners/*</span><span class="sxs-lookup"><span data-stu-id="86597-127">*/Dinners/*</span></span> | <span data-ttu-id="86597-128">今後ディナーの HTML の一覧を表示します。</span><span class="sxs-lookup"><span data-stu-id="86597-128">Display an HTML list of upcoming dinners</span></span> |
| <span data-ttu-id="86597-129">*/Dinners/Details/[id]*</span><span class="sxs-lookup"><span data-stu-id="86597-129">*/Dinners/Details/[id]*</span></span> | <span data-ttu-id="86597-130">これが一致するデータベースに dinner の DinnerID – URL に埋め込まれた"id"パラメーターで指定された特定 dinner に関する詳細を表示します。</span><span class="sxs-lookup"><span data-stu-id="86597-130">Display details about a specific dinner indicated by an "id" parameter embedded within the URL – which will match the DinnerID of the dinner in the database.</span></span> <span data-ttu-id="86597-131">例:/Dinners/Details/2 DinnerID 値が 2 Dinner に関する詳細情報を HTML ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="86597-131">For example: /Dinners/Details/2 would display an HTML page with details about the Dinner whose DinnerID value is 2.</span></span> |

<span data-ttu-id="86597-132">次のように、DinnersController クラスに次の 2 つのパブリック「操作方法」を追加することによってこれらの Url の最初の実装が公開します。</span><span class="sxs-lookup"><span data-stu-id="86597-132">We will publish initial implementations of these URLs by adding two public "action methods" to our DinnersController class like below:</span></span>

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample1.cs)]

<span data-ttu-id="86597-133">NerdDinner アプリケーションを実行し、それらを呼び出すため、ブラウザーを使用しておをし、します。</span><span class="sxs-lookup"><span data-stu-id="86597-133">We'll then run the NerdDinner application and use our browser to invoke them.</span></span> <span data-ttu-id="86597-134">入力、 *「ディナー/」* URL と、当社*Index()*メソッドを実行し、返信、次の応答。</span><span class="sxs-lookup"><span data-stu-id="86597-134">Typing in the *"/Dinners/"* URL will cause our *Index()* method to run, and it will send back the following response:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image4.png)

<span data-ttu-id="86597-135">入力、 *「/ディナー/詳細/2」* URL になります、 *Details()*メソッドを実行し、次の応答を返信します。</span><span class="sxs-lookup"><span data-stu-id="86597-135">Typing in the *"/Dinners/Details/2"* URL will cause our *Details()* method to run, and send back the following response:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image5.png)

<span data-ttu-id="86597-136">思うかもしれません - どの ASP.NET MVC ご存知、DinnersController クラスを作成し、これらのメソッドを呼び出しますか。</span><span class="sxs-lookup"><span data-stu-id="86597-136">You might be wondering - how did ASP.NET MVC know to create our DinnersController class and invoke those methods?</span></span> <span data-ttu-id="86597-137">理解してみましょうルーティング機能の仕組みを簡単に説明します。</span><span class="sxs-lookup"><span data-stu-id="86597-137">To understand that let's take a quick look at how routing works.</span></span>

### <a name="understanding-aspnet-mvc-routing"></a><span data-ttu-id="86597-138">Understanding ASP.NET MVC ルーティング</span><span class="sxs-lookup"><span data-stu-id="86597-138">Understanding ASP.NET MVC Routing</span></span>

<span data-ttu-id="86597-139">ASP.NET MVC には、多数の Url をコント ローラー クラスにマップする方法の制御の柔軟性を提供する強力な URL ルーティング エンジンが含まれています。</span><span class="sxs-lookup"><span data-stu-id="86597-139">ASP.NET MVC includes a powerful URL routing engine that provides a lot of flexibility in controlling how URLs are mapped to controller classes.</span></span> <span data-ttu-id="86597-140">ASP.NET MVC を呼び出すだけでなく、変数に自動的に URL/クエリ文字列から解析してパラメーターとしてメソッドに渡されるさまざまな方法を構成する方法を作成するには、どのコント ローラー クラスを選択する方法を完全にカスタマイズすることができます。引数。</span><span class="sxs-lookup"><span data-stu-id="86597-140">It allows us to completely customize how ASP.NET MVC chooses which controller class to create, which method to invoke on it, as well as configure different ways that variables can be automatically parsed from the URL/Querystring and passed to the method as parameter arguments.</span></span> <span data-ttu-id="86597-141">完全に SEO (検索エンジンの最適化) のサイトを最適化するだけでなくアプリケーションから必要なすべての URL 構造を公開する柔軟性を提供します。</span><span class="sxs-lookup"><span data-stu-id="86597-141">It delivers the flexibility to totally optimize a site for SEO (search engine optimization) as well as publish any URL structure we want from an application.</span></span>

<span data-ttu-id="86597-142">既定では、新しい ASP.NET MVC プロジェクトにあらかじめ構成された一連の URL ルーティングの規則は既に登録されています。</span><span class="sxs-lookup"><span data-stu-id="86597-142">By default, new ASP.NET MVC projects come with a preconfigured set of URL routing rules already registered.</span></span> <span data-ttu-id="86597-143">これにより、簡単に作業を開始するアプリケーションで明示的に何も構成することがなくことができます。</span><span class="sxs-lookup"><span data-stu-id="86597-143">This enables us to easily get started on an application without having to explicitly configure anything.</span></span> <span data-ttu-id="86597-144">既定のルーティング ルール登録は、プロジェクトのルートに"Global.asax"ファイルをダブルクリックして開くことができます、プロジェクトの"Application"のクラス内で確認できます。</span><span class="sxs-lookup"><span data-stu-id="86597-144">The default routing rule registrations can be found within the "Application" class of our projects – which we can open by double-clicking the "Global.asax" file in the root of our project:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image6.png)

<span data-ttu-id="86597-145">既定の ASP.NET MVC ルーティング ルールは、このクラスの"RegisterRoutes"メソッド内で登録されます。</span><span class="sxs-lookup"><span data-stu-id="86597-145">The default ASP.NET MVC routing rules are registered within the "RegisterRoutes" method of this class:</span></span>

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample2.cs)]

<span data-ttu-id="86597-146">"ルート。MapRoute()"メソッドの呼び出しが上記の登録 URL の形式を使用して、コント ローラー クラスを受信した Url にマップする既定のルーティング規則:"/{controller}/{controller}/{id}""controller"には、インスタンス化するコント ローラー クラスの名前:"action"はの名前、パブリック メソッドを呼び出すと、"id"では、メソッドに引数として渡すことが URL に埋め込まれた省略可能なパラメーターです。</span><span class="sxs-lookup"><span data-stu-id="86597-146">The "routes.MapRoute()" method call above registers a default routing rule that maps incoming URLs to controller classes using the URL format: "/{controller}/{action}/{id}" – where "controller" is the name of the controller class to instantiate, "action" is the name of a public method to invoke on it, and "id" is an optional parameter embedded within the URL that can be passed as an argument to the method.</span></span> <span data-ttu-id="86597-147">"MapRoute()"メソッドの呼び出しに渡される 3 番目のパラメーターは、一連の既定値は URL に存在しないことに、コント ローラーとアクション/id 値に使用する (コント ローラー アクションで =「ホーム」、"Index"、Id = ="") です。</span><span class="sxs-lookup"><span data-stu-id="86597-147">The third parameter passed to the "MapRoute()" method call is a set of default values to use for the controller/action/id values in the event that they are not present in the URL (Controller = "Home", Action="Index", Id="").</span></span>

<span data-ttu-id="86597-148">以下は Url のさまざまな方法を示すテーブルが既定値を使用してマップされている"<em>/{コント ローラー}/{controller}/{id}"</em>ルート ルール。</span><span class="sxs-lookup"><span data-stu-id="86597-148">Below is a table that demonstrates how a variety of URLs are mapped using the default "<em>/{controllers}/{action}/{id}"</em>route rule:</span></span>

| <span data-ttu-id="86597-149">**URL**</span><span class="sxs-lookup"><span data-stu-id="86597-149">**URL**</span></span> | <span data-ttu-id="86597-150">**コント ローラー クラス**</span><span class="sxs-lookup"><span data-stu-id="86597-150">**Controller Class**</span></span> | <span data-ttu-id="86597-151">**アクション メソッド**</span><span class="sxs-lookup"><span data-stu-id="86597-151">**Action Method**</span></span> | <span data-ttu-id="86597-152">**渡されたパラメーター**</span><span class="sxs-lookup"><span data-stu-id="86597-152">**Parameters Passed**</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="86597-153">*/Dinners/Details/2*</span><span class="sxs-lookup"><span data-stu-id="86597-153">*/Dinners/Details/2*</span></span> | <span data-ttu-id="86597-154">DinnersController</span><span class="sxs-lookup"><span data-stu-id="86597-154">DinnersController</span></span> | <span data-ttu-id="86597-155">Details(id)</span><span class="sxs-lookup"><span data-stu-id="86597-155">Details(id)</span></span> | <span data-ttu-id="86597-156">id=2</span><span class="sxs-lookup"><span data-stu-id="86597-156">id=2</span></span> |
| <span data-ttu-id="86597-157">*/Dinners/Edit/5*</span><span class="sxs-lookup"><span data-stu-id="86597-157">*/Dinners/Edit/5*</span></span> | <span data-ttu-id="86597-158">DinnersController</span><span class="sxs-lookup"><span data-stu-id="86597-158">DinnersController</span></span> | <span data-ttu-id="86597-159">Edit(id)</span><span class="sxs-lookup"><span data-stu-id="86597-159">Edit(id)</span></span> | <span data-ttu-id="86597-160">id=5</span><span class="sxs-lookup"><span data-stu-id="86597-160">id=5</span></span> |
| <span data-ttu-id="86597-161">*/Dinners/Create*</span><span class="sxs-lookup"><span data-stu-id="86597-161">*/Dinners/Create*</span></span> | <span data-ttu-id="86597-162">DinnersController</span><span class="sxs-lookup"><span data-stu-id="86597-162">DinnersController</span></span> | <span data-ttu-id="86597-163">Create()</span><span class="sxs-lookup"><span data-stu-id="86597-163">Create()</span></span> | <span data-ttu-id="86597-164">N/A</span><span class="sxs-lookup"><span data-stu-id="86597-164">N/A</span></span> |
| <span data-ttu-id="86597-165">*/Dinners*</span><span class="sxs-lookup"><span data-stu-id="86597-165">*/Dinners*</span></span> | <span data-ttu-id="86597-166">DinnersController</span><span class="sxs-lookup"><span data-stu-id="86597-166">DinnersController</span></span> | <span data-ttu-id="86597-167">Index()</span><span class="sxs-lookup"><span data-stu-id="86597-167">Index()</span></span> | <span data-ttu-id="86597-168">N/A</span><span class="sxs-lookup"><span data-stu-id="86597-168">N/A</span></span> |
| <span data-ttu-id="86597-169">*/Home*</span><span class="sxs-lookup"><span data-stu-id="86597-169">*/Home*</span></span> | <span data-ttu-id="86597-170">HomeController</span><span class="sxs-lookup"><span data-stu-id="86597-170">HomeController</span></span> | <span data-ttu-id="86597-171">Index()</span><span class="sxs-lookup"><span data-stu-id="86597-171">Index()</span></span> | <span data-ttu-id="86597-172">N/A</span><span class="sxs-lookup"><span data-stu-id="86597-172">N/A</span></span> |
| */* | <span data-ttu-id="86597-173">HomeController</span><span class="sxs-lookup"><span data-stu-id="86597-173">HomeController</span></span> | <span data-ttu-id="86597-174">Index()</span><span class="sxs-lookup"><span data-stu-id="86597-174">Index()</span></span> | <span data-ttu-id="86597-175">N/A</span><span class="sxs-lookup"><span data-stu-id="86597-175">N/A</span></span> |

<span data-ttu-id="86597-176">最後の 3 つの行が既定値を表示 (コント ローラー アクションで = ホーム、インデックス、Id = ="") 使用されています。</span><span class="sxs-lookup"><span data-stu-id="86597-176">The last three rows show the default values (Controller = Home, Action = Index, Id = "") being used.</span></span> <span data-ttu-id="86597-177">1 つ指定されていない場合に、既定のアクション名として"Index"メソッドが登録されているため、"/ディナー"、"/home"Url が Index() アクション メソッドのコント ローラー クラスで呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="86597-177">Because the "Index" method is registered as the default action name if one isn't specified, the "/Dinners" and "/Home" URLs cause the Index() action method to be invoked on their Controller classes.</span></span> <span data-ttu-id="86597-178">いずれかが指定されていない場合に既定のコント ローラーとして「ホーム」コント ローラーが登録されているため、「/」URL を作成するテンプレートを使用して、Index() アクション メソッドを呼び出すことで。</span><span class="sxs-lookup"><span data-stu-id="86597-178">Because the "Home" controller is registered as the default controller if one isn't specified, the "/" URL causes the HomeController to be created, and the Index() action method on it to be invoked.</span></span>

<span data-ttu-id="86597-179">これら既定の URL ルーティング ルールがたくない場合良いニュースは、簡単に変更 - だけ上の RegisterRoutes メソッド内で編集されます。</span><span class="sxs-lookup"><span data-stu-id="86597-179">If you don't like these default URL routing rules, the good news is that they are easy to change - just edit them within the RegisterRoutes method above.</span></span> <span data-ttu-id="86597-180">NerdDinner アプリケーションでは、は、既定の URL ルーティング規則のいずれかを変更することにしましょう – 代わりにだけそのまま使用します-はします。</span><span class="sxs-lookup"><span data-stu-id="86597-180">For our NerdDinner application, though, we aren't going to change any of the default URL routing rules – instead we'll just use them as-is.</span></span>

### <a name="using-the-dinnerrepository-from-our-dinnerscontroller"></a><span data-ttu-id="86597-181">当社 DinnersController から DinnerRepository を使用します。</span><span class="sxs-lookup"><span data-stu-id="86597-181">Using the DinnerRepository from our DinnersController</span></span>

<span data-ttu-id="86597-182">みましょう DinnersController の現在の実装に置き換える Index() と Details() のアクション メソッドをモデルを使用する実装。</span><span class="sxs-lookup"><span data-stu-id="86597-182">Let's now replace our current implementation of the DinnersController's Index() and Details() action methods with implementations that use our model.</span></span>

<span data-ttu-id="86597-183">動作を実装する前に作成した DinnerRepository クラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="86597-183">We'll use the DinnerRepository class we built earlier to implement the behavior.</span></span> <span data-ttu-id="86597-184">おによりを"NerdDinner.Models"名前空間を参照している"using"ステートメントを追加することによって開始し、フィールドとして、DinnerController クラス、DinnerRepository のインスタンスを宣言します。</span><span class="sxs-lookup"><span data-stu-id="86597-184">We'll begin by adding a "using" statement that references the "NerdDinner.Models" namespace, and then declare an instance of our DinnerRepository as a field on our DinnerController class.</span></span>

<span data-ttu-id="86597-185">この章で後でうまく「依存関係の挿入」の概念を紹介し、表示を向上単位を有効にする DinnerRepository への参照を取得する、コント ローラーを別の方法: テストですが右のこれで、当社 DinnerRepository のインスタンスを作成しましょう次のようにインラインです。</span><span class="sxs-lookup"><span data-stu-id="86597-185">Later in this chapter we'll introduce the concept of "Dependency Injection" and show another way for our Controllers to obtain a reference to a DinnerRepository that enables better unit testing – but for right now we'll just create an instance of our DinnerRepository inline like below.</span></span>

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample3.cs)]

<span data-ttu-id="86597-186">取得したデータ モデル オブジェクトを使用して HTML 応答を生成する準備が整いました。</span><span class="sxs-lookup"><span data-stu-id="86597-186">Now we are ready to generate a HTML response back using our retrieved data model objects.</span></span>

### <a name="using-views-with-our-controller"></a><span data-ttu-id="86597-187">このコント ローラーとビューの使用</span><span class="sxs-lookup"><span data-stu-id="86597-187">Using Views with our Controller</span></span>

<span data-ttu-id="86597-188">HTML をアセンブルし、使用して、アクション メソッド内でコードを記述することができますが、 *Response.Write()*いるアプローチ手に負えなくかなり簡単に、クライアントに送信するヘルパー メソッドです。</span><span class="sxs-lookup"><span data-stu-id="86597-188">While it is possible to write code within our action methods to assemble HTML and then use the *Response.Write()* helper method to send it back to the client, that approach becomes fairly unwieldy quickly.</span></span> <span data-ttu-id="86597-189">のみアプリケーションと、DinnersController アクション メソッドの内部データ ロジックを実行し、個別"view"テンプレートは HTML 形式の出力を担当する HTML 応答を表示するために必要なデータを渡すは、はるかに優れた方法ことです。</span><span class="sxs-lookup"><span data-stu-id="86597-189">A much better approach is for us to only perform application and data logic inside our DinnersController action methods, and to then pass the data needed to render a HTML response to a separate "view" template that is responsible for outputting the HTML representation of it.</span></span> <span data-ttu-id="86597-190">しばらくおわかりのよう"view"テンプレートは、通常の HTML マークアップと埋め込みレンダリング コードの組み合わせを含むテキスト ファイルです。</span><span class="sxs-lookup"><span data-stu-id="86597-190">As we'll see in a moment, a "view" template is a text file that typically contains a combination of HTML markup and embedded rendering code.</span></span>

<span data-ttu-id="86597-191">ビュー レンダリングから、コント ローラー ロジックを分離すると、いくつかの大きな利点が表示されます。</span><span class="sxs-lookup"><span data-stu-id="86597-191">Separating our controller logic from our view rendering brings several big benefits.</span></span> <span data-ttu-id="86597-192">具体的には、明確"に分離する懸案事項"間で、アプリケーション コードと UI コードを書式設定または表示に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="86597-192">In particular it helps enforce a clear "separation of concerns" between the application code and UI formatting/rendering code.</span></span> <span data-ttu-id="86597-193">これはより簡単単体テスト アプリケーション ロジックを分離する UI のレンダリング ロジックからです。</span><span class="sxs-lookup"><span data-stu-id="86597-193">This makes it much easier to unit-test application logic in isolation from UI rendering logic.</span></span> <span data-ttu-id="86597-194">やすく後で UI 表示テンプレートを変更するアプリケーション コードの変更を加える必要はありません。</span><span class="sxs-lookup"><span data-stu-id="86597-194">It makes it easier to later modify the UI rendering templates without having to make application code changes.</span></span> <span data-ttu-id="86597-195">できるように開発者とデザイナー プロジェクトで一緒に共同作業を容易にします。</span><span class="sxs-lookup"><span data-stu-id="86597-195">And it can make it easier for developers and designers to collaborate together on projects.</span></span>

<span data-ttu-id="86597-196">代わりに、戻り値の型がある"ActionResult"の「無効」の戻り値の型を持つから、2 つのアクション メソッドのメソッド シグネチャを変更すると、HTML UI の応答を返信するビュー テンプレートを使用することを示すために、DinnersController クラスを更新することができます。</span><span class="sxs-lookup"><span data-stu-id="86597-196">We can update our DinnersController class to indicate that we want to use a view template to send back an HTML UI response by changing the method signatures of our two action methods from having a return type of "void" to instead have a return type of "ActionResult".</span></span> <span data-ttu-id="86597-197">呼び出してことができます、 *View()* "ViewResult"オブジェクトなどの下に返すコント ローラーの基本クラスのヘルパー メソッド。</span><span class="sxs-lookup"><span data-stu-id="86597-197">We can then call the *View()* helper method on the Controller base class to return back a "ViewResult" object like below:</span></span>

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample4.cs)]

<span data-ttu-id="86597-198">署名、 *View()*上を使用するヘルパー メソッドが次のようになります。</span><span class="sxs-lookup"><span data-stu-id="86597-198">The signature of the *View()* helper method we are using above looks like below:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image7.png)

<span data-ttu-id="86597-199">最初のパラメーター、 *View()*ヘルパー メソッドが HTML 応答を表示するために使用するビューのテンプレート ファイルの名前。</span><span class="sxs-lookup"><span data-stu-id="86597-199">The first parameter to the *View()* helper method is the name of the view template file we want to use to render the HTML response.</span></span> <span data-ttu-id="86597-200">2 番目のパラメーターは、テンプレートの表示が HTML 応答を表示するために必要なデータを含むモデル オブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="86597-200">The second parameter is a model object that contains the data that the view template needs in order to render the HTML response.</span></span>

<span data-ttu-id="86597-201">呼び出す、Index() アクション メソッド内で、 *View()*ヘルパー メソッドのディナー"Index"ビュー テンプレートを使用して、HTML の一覧を表示することを指定しています。</span><span class="sxs-lookup"><span data-stu-id="86597-201">Within our Index() action method we are calling the *View()* helper method and indicating that we want to render an HTML listing of dinners using an "Index" view template.</span></span> <span data-ttu-id="86597-202">ビュー テンプレートから一覧を生成する Dinner オブジェクトのシーケンスを渡しています。</span><span class="sxs-lookup"><span data-stu-id="86597-202">We are passing the view template a sequence of Dinner objects to generate the list from:</span></span>

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample5.cs)]

<span data-ttu-id="86597-203">当社 Details() アクション メソッド内で、URL 内で指定された id を使用して、夕食オブジェクトを取得ましょう。</span><span class="sxs-lookup"><span data-stu-id="86597-203">Within our Details() action method we attempt to retrieve a Dinner object using the id provided within the URL.</span></span> <span data-ttu-id="86597-204">呼び出して有効な夕食が見つかった場合、 *View()*ヘルパー メソッドを呼び出した Dinner オブジェクトを表示するために、[詳細] ビューのテンプレートを使用することを示すです。</span><span class="sxs-lookup"><span data-stu-id="86597-204">If a valid Dinner is found we call the *View()* helper method, indicating we want to use a "Details" view template to render the retrieved Dinner object.</span></span> <span data-ttu-id="86597-205">"NotFound"ビューのテンプレートを使用して、夕食が存在しないことを示す便利なエラー メッセージをレンダリングは無効な夕食が要求されている場合 (とオーバー ロードされたバージョンの*View()*のみテンプレート名を取得するヘルパー メソッド):</span><span class="sxs-lookup"><span data-stu-id="86597-205">If an invalid dinner is requested, we render a helpful error message that indicates that the Dinner doesn't exist using a "NotFound" view template (and an overloaded version of the *View()* helper method that just takes the template name):</span></span>

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample6.cs)]

<span data-ttu-id="86597-206">"NotFound"、「詳細」、および"Index"ビューのテンプレートを実装してみましょう。</span><span class="sxs-lookup"><span data-stu-id="86597-206">Let's now implement the "NotFound", "Details", and "Index" view templates.</span></span>

### <a name="implementing-the-notfound-view-template"></a><span data-ttu-id="86597-207">"NotFound"のビュー テンプレートの実装</span><span class="sxs-lookup"><span data-stu-id="86597-207">Implementing the "NotFound" View Template</span></span>

<span data-ttu-id="86597-208">まず、– 要求された dinner が見つからないことを示すフレンドリ エラー メッセージが表示されます、"NotFound"ビュー テンプレートを実装します。</span><span class="sxs-lookup"><span data-stu-id="86597-208">We'll begin by implementing the "NotFound" view template – which displays a friendly error message indicating that the requested dinner can't be found.</span></span>

<span data-ttu-id="86597-209">コント ローラー アクション メソッド内で、テキストのカーソルを配置し、新しいビュー テンプレートを作成しを右クリックし、(してもコマンドを実行できるこの Ctrl-M, ctrl + V」と入力して)「ビューの追加」メニュー コマンドを選択します。</span><span class="sxs-lookup"><span data-stu-id="86597-209">We'll create a new view template by positioning our text cursor within a controller action method, and then right click and choose the "Add View" menu command (we can also execute this command by typing Ctrl-M, Ctrl-V):</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image8.png)

<span data-ttu-id="86597-210">これは、次のような「ビューの追加」ダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="86597-210">This will bring up an "Add View" dialog like below.</span></span> <span data-ttu-id="86597-211">既定では、ダイアログ ボックスが作成するビューの名前を事前に設定アクション メソッドの名前と一致するカーソル時のダイアログが (この場合は「詳細」) で起動されました。</span><span class="sxs-lookup"><span data-stu-id="86597-211">By default the dialog will pre-populate the name of the view to create to match the name of the action method the cursor was in when the dialog was launched (in this case "Details").</span></span> <span data-ttu-id="86597-212">、まず、"NotFound"テンプレートを実装する必要があるため、このビューの名前をオーバーライドし、"NotFound"を代わりになるように設定おします。</span><span class="sxs-lookup"><span data-stu-id="86597-212">Because we want to first implement the "NotFound" template, we'll override this view name and set it to instead be "NotFound":</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image9.png)

<span data-ttu-id="86597-213">「追加」ボタンをクリックすると Visual Studio は新しい"NotFound.aspx"ビュー テンプレートを (これは、ディレクトリが既に存在しない場合にも作成)"\Views\Dinners"ディレクトリ内の作成します。</span><span class="sxs-lookup"><span data-stu-id="86597-213">When we click the "Add" button, Visual Studio will create a new "NotFound.aspx" view template for us within the "\Views\Dinners" directory (which it will also create if the directory doesn't already exist):</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image10.png)

<span data-ttu-id="86597-214">コード エディター内で、新しい"NotFound.aspx"ビュー テンプレートも開きます。</span><span class="sxs-lookup"><span data-stu-id="86597-214">It will also open up our new "NotFound.aspx" view template within the code-editor:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image11.png)

<span data-ttu-id="86597-215">既定ではテンプレートの表示がある 2 つ「コンテンツ領域」コンテンツおよびコードを追加しました。</span><span class="sxs-lookup"><span data-stu-id="86597-215">View templates by default have two "content regions" where we can add content and code.</span></span> <span data-ttu-id="86597-216">1 つ目では、「タイトル」HTML ページの返信をカスタマイズすることができます。</span><span class="sxs-lookup"><span data-stu-id="86597-216">The first allows us to customize the "title" of the HTML page sent back.</span></span> <span data-ttu-id="86597-217">2 つ目では、コンテンツをカスタマイズする"メイン"HTML ページの返信されることができます。</span><span class="sxs-lookup"><span data-stu-id="86597-217">The second allows us to customize the "main content" of the HTML page sent back.</span></span>

<span data-ttu-id="86597-218">"NotFound"ビューのテンプレートを実装するには、いくつかの基本的なコンテンツを追加おします。</span><span class="sxs-lookup"><span data-stu-id="86597-218">To implement our "NotFound" view template we'll add some basic content:</span></span>

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample7.aspx)]

<span data-ttu-id="86597-219">お試すことができますし、ブラウザー内で。</span><span class="sxs-lookup"><span data-stu-id="86597-219">We can then try it out within the browser.</span></span> <span data-ttu-id="86597-220">これを行うみましょう要求、 *「/9999/ディナー/詳細」* URL。</span><span class="sxs-lookup"><span data-stu-id="86597-220">To do this let's request the *"/Dinners/Details/9999"* URL.</span></span> <span data-ttu-id="86597-221">これは、現在、データベースに存在しないし、"NotFound"ビューのテンプレートを表示するために、DinnersController.Details() アクション メソッドが発生する dinner を参照してください。</span><span class="sxs-lookup"><span data-stu-id="86597-221">This will refer to a dinner that doesn't currently exist in the database, and will cause our DinnersController.Details() action method to render our "NotFound" view template:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image12.png)

<span data-ttu-id="86597-222">1 つに気付くこと上スクリーン ショットでは、基本的なビュー テンプレートには、多数の HTML 画面上のメイン コンテンツを囲むが継承されてます。</span><span class="sxs-lookup"><span data-stu-id="86597-222">One thing you'll notice in the screen-shot above is that our basic view template has inherited a bunch of HTML that surrounds the main content on the screen.</span></span> <span data-ttu-id="86597-223">これは、ビュー テンプレートが、サイト上のすべてのビューで一貫したレイアウトを適用することにより、「マスター ページ」テンプレートを使用しているためです。</span><span class="sxs-lookup"><span data-stu-id="86597-223">This is because our view-template is using a "master page" template that enables us to apply a consistent layout across all views on the site.</span></span> <span data-ttu-id="86597-224">マスター ページの詳細については、このチュートリアルの後半のしくみについて説明します。</span><span class="sxs-lookup"><span data-stu-id="86597-224">We'll discuss how master pages work more in a later part of this tutorial.</span></span>

### <a name="implementing-the-details-view-template"></a><span data-ttu-id="86597-225">「詳細」ビュー テンプレートの実装</span><span class="sxs-lookup"><span data-stu-id="86597-225">Implementing the "Details" View Template</span></span>

<span data-ttu-id="86597-226">– 単一 Dinner モデルの HTML を生成する"Details"ビュー テンプレートを実装してみましょう。</span><span class="sxs-lookup"><span data-stu-id="86597-226">Let's now implement the "Details" view template – which will generate HTML for a single Dinner model.</span></span>

<span data-ttu-id="86597-227">おを Details アクション メソッド内で、テキストのカーソルを配置しを右クリックし「ビューの追加」メニュー コマンドを選択 (または Ctrl-M, ctrl + V キーを押します)。</span><span class="sxs-lookup"><span data-stu-id="86597-227">We'll do this by positioning our text cursor within the Details action method, and then right click and choose the "Add View" menu command (or press Ctrl-M, Ctrl-V):</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image13.png)

<span data-ttu-id="86597-228">「ビューの追加」ダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="86597-228">This will bring up the "Add View" dialog.</span></span> <span data-ttu-id="86597-229">既定のビュー名 (「詳細」) ではこのままです。</span><span class="sxs-lookup"><span data-stu-id="86597-229">We'll keep the default view name ("Details").</span></span> <span data-ttu-id="86597-230">おありますも「厳密に型指定されたビューの作成」のチェック ボックス ダイアログ ボックスを選択 (コンボ ボックスのドロップダウン リストを使用) に渡しているコント ローラーからビューにモデルの種類の名前します。</span><span class="sxs-lookup"><span data-stu-id="86597-230">We'll also select the "Create a strongly-typed View" checkbox in the dialog and select (using the combobox dropdown) the name of the model type we are passing from the Controller to the View.</span></span> <span data-ttu-id="86597-231">このビューの Dinner オブジェクトを渡している (この型の完全修飾名:"NerdDinner.Models.Dinner")。</span><span class="sxs-lookup"><span data-stu-id="86597-231">For this view we are passing a Dinner object (the fully qualified name for this type is: "NerdDinner.Models.Dinner"):</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image14.png)

<span data-ttu-id="86597-232">「空のビュー」を作成する選びましたここでは、以前のテンプレートとは異なりこの時刻に自動的に選択します「スキャフォールディング」「詳細」テンプレートを使用して表示します。</span><span class="sxs-lookup"><span data-stu-id="86597-232">Unlike the previous template, where we chose to create an "Empty View", this time we will choose to automatically "scaffold" the view using a "Details" template.</span></span> <span data-ttu-id="86597-233">このダイアログ ボックスで「コンテンツの表示」のドロップダウン リストを変更することで、これを表示おことができます。</span><span class="sxs-lookup"><span data-stu-id="86597-233">We can indicate this by changing the "View content" drop-down in the dialog above.</span></span>

<span data-ttu-id="86597-234">「スキャフォールディング」を渡している Dinner オブジェクトに基づいて、詳細ビュー テンプレートの最初の実装が生成されます。</span><span class="sxs-lookup"><span data-stu-id="86597-234">"Scaffolding" will generate an initial implementation of our details view template based on the Dinner object we are passing to it.</span></span> <span data-ttu-id="86597-235">これは、ビュー テンプレートの実装に簡単に開始することは簡単な方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="86597-235">This provides an easy way for us to quickly get started on our view template implementation.</span></span>

<span data-ttu-id="86597-236">「追加」ボタンをクリックすると Visual Studio は新しい"Details.aspx"ビュー テンプレート ファイルを"\Views\Dinners"ディレクトリ内でご利用の米国作成します。</span><span class="sxs-lookup"><span data-stu-id="86597-236">When we click the "Add" button, Visual Studio will create a new "Details.aspx" view template file for us within our "\Views\Dinners" directory:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image15.png)

<span data-ttu-id="86597-237">コード エディター内で、新しい"Details.aspx"ビュー テンプレートが開放されるもします。</span><span class="sxs-lookup"><span data-stu-id="86597-237">It will also open up our new "Details.aspx" view template within the code-editor.</span></span> <span data-ttu-id="86597-238">Dinner モデルに基づいた詳細ビューの初期 scaffold 実装が含まれます。</span><span class="sxs-lookup"><span data-stu-id="86597-238">It will contain an initial scaffold implementation of a details view based on a Dinner model.</span></span> <span data-ttu-id="86597-239">スキャフォールディング エンジンは、.NET リフレクションを使用して、渡されたクラスで公開されているパブリック プロパティを確認して、見つかった各型に基づく適切なコンテンツを追加します。</span><span class="sxs-lookup"><span data-stu-id="86597-239">The scaffolding engine uses .NET reflection to look at the public properties exposed on the class passed it, and will add appropriate content based on each type it finds:</span></span>

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample8.aspx)]

<span data-ttu-id="86597-240">*「/1/ディナー/詳細」*の URL をブラウザー内でこの「詳細」scaffold 実装がどのようにを参照してください。</span><span class="sxs-lookup"><span data-stu-id="86597-240">We can request the *"/Dinners/Details/1"* URL to see what this "details" scaffold implementation looks like in the browser.</span></span> <span data-ttu-id="86597-241">この URL を使用して手動で追加したデータベースに最初に作成した際ディナーの 1 つ表示されます。</span><span class="sxs-lookup"><span data-stu-id="86597-241">Using this URL will display one of the dinners we manually added to our database when we first created it:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image16.png)

<span data-ttu-id="86597-242">これは us 立ち上がっており実行中をすばやく取得、および、Details.aspx ビューの最初の実装を提供します。</span><span class="sxs-lookup"><span data-stu-id="86597-242">This gets us up and running quickly, and provides us with an initial implementation of our Details.aspx view.</span></span> <span data-ttu-id="86597-243">お移動し、満足度に UI をカスタマイズすることを調整します。</span><span class="sxs-lookup"><span data-stu-id="86597-243">We can then go and tweak it to customize the UI to our satisfaction.</span></span>

<span data-ttu-id="86597-244">Details.aspx テンプレートより密接を見るときにわかること、静的な HTML を含むだけでなくレンダリング コードが埋め込まれています。</span><span class="sxs-lookup"><span data-stu-id="86597-244">When we look at the Details.aspx template more closely, we'll find that it contains static HTML as well as embedded rendering code.</span></span> <span data-ttu-id="86597-245">&lt;%%&gt;ビュー テンプレートをレンダリングするときに、コード ナゲットがコードを実行し、 &lt;% = %&gt;コード ナゲットがそれぞれに含まれるコードを実行し、テンプレートの出力ストリームに結果をレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="86597-245">&lt;% %&gt; code nuggets execute code when the view template renders, and &lt;%= %&gt; code nuggets execute the code contained within them and then render the result to the output stream of the template.</span></span>

<span data-ttu-id="86597-246">厳密に型指定された「モデル」プロパティを使用して、コント ローラーから渡された"Dinner"モデル オブジェクトにアクセスする、ビュー内でコードを記述できます。</span><span class="sxs-lookup"><span data-stu-id="86597-246">We can write code within our View that accesses the "Dinner" model object that was passed from our controller using a strongly-typed "Model" property.</span></span> <span data-ttu-id="86597-247">Visual Studio により、完全コード intellisense を備えた、エディター内で「モデル」このプロパティにアクセスするとき。</span><span class="sxs-lookup"><span data-stu-id="86597-247">Visual Studio provides us with full code-intellisense when accessing this "Model" property within the editor:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image17.png)

<span data-ttu-id="86597-248">みましょうように扱いました最終的な詳細ビュー テンプレートのソースは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="86597-248">Let's make some tweaks so that the source for our final Details view template looks like below:</span></span>

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample9.aspx)]

<span data-ttu-id="86597-249">アクセスして、 *「/1/ディナー/詳細」* URL 再度はこれで、以下のようなレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="86597-249">When we access the *"/Dinners/Details/1"* URL again it will now render like below:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image18.png)

### <a name="implementing-the-index-view-template"></a><span data-ttu-id="86597-250">"Index"ビュー テンプレートの実装</span><span class="sxs-lookup"><span data-stu-id="86597-250">Implementing the "Index" View Template</span></span>

<span data-ttu-id="86597-251">– 今後ディナーの一覧を生成する"Index"ビュー テンプレートを実装してみましょう。</span><span class="sxs-lookup"><span data-stu-id="86597-251">Let's now implement the "Index" view template – which will generate a listing of upcoming Dinners.</span></span> <span data-ttu-id="86597-252">To do おインデックス アクション メソッド内で、テキストのカーソルの位置し、し、右をこの をクリックし、コマンドを選択「ビューの追加」メニュー (または Ctrl-M, ctrl + V キーを押します)。</span><span class="sxs-lookup"><span data-stu-id="86597-252">To-do this we'll position our text cursor within the Index action method, and then right click and choose the "Add View" menu command (or press Ctrl-M, Ctrl-V).</span></span>

<span data-ttu-id="86597-253">「ビューの追加」ダイアログ ボックスでを"Index"という名前のビュー テンプレートを保持し、「厳密に型指定されたビューの作成」チェック ボックスをオンします。</span><span class="sxs-lookup"><span data-stu-id="86597-253">Within the "Add View" dialog we'll keep the view template named "Index" and select the "Create a strongly-typed view" checkbox.</span></span> <span data-ttu-id="86597-254">今度は自動的に"List"ビュー テンプレートを生成し、"NerdDinner.Models.Dinner"モデルの種類として、ビューに渡されるを選択してを選択します (これ scaffold が発生することが想定するビューの追加 ダイアログ「リスト」を作成することによって示されたおためシーケンスを渡して Dinner オブジェクトのコント ローラーからビューに)。</span><span class="sxs-lookup"><span data-stu-id="86597-254">This time we will choose to automatically generate a "List" view template, and select "NerdDinner.Models.Dinner" as the model type passed to the view (which because we have indicated we are creating a "List" scaffold will cause the Add View dialog to assume we are passing a sequence of Dinner objects from our Controller to the View):</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image19.png)

<span data-ttu-id="86597-255">「追加」ボタンをクリックすると Visual Studio は新しい"Index.aspx"ビュー テンプレート ファイルを"\Views\Dinners"ディレクトリ内でご利用の米国作成します。</span><span class="sxs-lookup"><span data-stu-id="86597-255">When we click the "Add" button, Visual Studio will create a new "Index.aspx" view template file for us within our "\Views\Dinners" directory.</span></span> <span data-ttu-id="86597-256">これは「スキャフォールディング」、初期の実装内のビューに渡すおディナー、HTML テーブルの一覧を提供します。</span><span class="sxs-lookup"><span data-stu-id="86597-256">It will "scaffold" an initial implementation within it that provides an HTML table listing of the Dinners we pass to the view.</span></span>

<span data-ttu-id="86597-257">アプリケーションとアクセスを実行するとき、 *「ディナー/」* URL ディナーの一覧を表示する次のようにします。</span><span class="sxs-lookup"><span data-stu-id="86597-257">When we run the application and access the *"/Dinners/"* URL it will render our list of dinners like so:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image20.png)

<span data-ttu-id="86597-258">上記のテーブル ソリューションでは、これは非常に必要な夕食一覧が直面している、コンシューマーでは、Dinner データのグリッドのようなレイアウトによりします。</span><span class="sxs-lookup"><span data-stu-id="86597-258">The table solution above gives us a grid-like layout of our Dinner data – which isn't quite what we want for our consumer facing Dinner listing.</span></span> <span data-ttu-id="86597-259">Index.aspx ビュー テンプレートを更新して、データの少ない列を一覧表示して使用するように変更を&lt;ul&gt;レンダリングして、次のコードを使用してテーブルではなく要素。</span><span class="sxs-lookup"><span data-stu-id="86597-259">We can update the Index.aspx view template and modify it to list fewer columns of data, and use a &lt;ul&gt; element to render them instead of a table using the code below:</span></span>

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample10.aspx)]

<span data-ttu-id="86597-260">各 dinner 経由でこのモデルでループ処理、上記の foreach ステートメント内で"var"キーワードを使用します。</span><span class="sxs-lookup"><span data-stu-id="86597-260">We are using the "var" keyword within the above foreach statement as we loop over each dinner in our Model.</span></span> <span data-ttu-id="86597-261">"Var"を使用することを意味 dinner オブジェクトは、遅延バインディングを c# 3.0 に慣れていないものと考える可能性があります。</span><span class="sxs-lookup"><span data-stu-id="86597-261">Those unfamiliar with C# 3.0 might think that using "var" means that the dinner object is late-bound.</span></span> <span data-ttu-id="86597-262">代わりにすること、コンパイラが厳密に型指定の「モデル」プロパティに対して型推論を使用している (種類がある"IEnumerable&lt;Dinner&gt;") およびおがいっぱいになることを意味 – Dinner の種類としてローカル"dinner"変数のコンパイルintellisense およびコンパイル時のコード ブロック内でこれを確認します。</span><span class="sxs-lookup"><span data-stu-id="86597-262">It instead means that the compiler is using type-inference against the strongly typed "Model" property (which is of type "IEnumerable&lt;Dinner&gt;") and compiling the local "dinner" variable as a Dinner type – which means we get full intellisense and compile-time checking for it within code blocks:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image21.png)

<span data-ttu-id="86597-263">更新をヒットしました、 */Dinners*以下、更新されたビュー今すぐ検索のようなブラウザーで URL:</span><span class="sxs-lookup"><span data-stu-id="86597-263">When we hit refresh on the */Dinners* URL in our browser our updated view now looks like below:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image22.png)

<span data-ttu-id="86597-264">これは優れた – はまだ完全があります。</span><span class="sxs-lookup"><span data-stu-id="86597-264">This is looking better – but isn't entirely there yet.</span></span> <span data-ttu-id="86597-265">最後に をクリックして、リスト内の個々 のディナーに関する詳細情報を参照してください。 エンドユーザーを有効にします。</span><span class="sxs-lookup"><span data-stu-id="86597-265">Our last step is to enable end-users to click individual Dinners in the list and see details about them.</span></span> <span data-ttu-id="86597-266">当社 DinnersController に Details アクション メソッドにリンクする HTML ハイパーリンク要素を表示して、これを実装します。</span><span class="sxs-lookup"><span data-stu-id="86597-266">We'll implement this by rendering HTML hyperlink elements that link to the Details action method on our DinnersController.</span></span>

<span data-ttu-id="86597-267">2 つの方法のいずれかで、インデックス ビュー内でこれらのハイパーリンクを生成できます。</span><span class="sxs-lookup"><span data-stu-id="86597-267">We can generate these hyperlinks within our Index view in one of two ways.</span></span> <span data-ttu-id="86597-268">最初に HTML を手動で作成、 &lt;、&gt;などの要素の下、私たちの埋め込み&lt;%%&gt;内でブロック、 &lt;、&gt; HTML 要素。</span><span class="sxs-lookup"><span data-stu-id="86597-268">The first is to manually create HTML &lt;a&gt; elements like below, where we embed &lt;% %&gt; blocks within the &lt;a&gt; HTML element:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image23.png)

<span data-ttu-id="86597-269">使用して、別の方法は、HTML をプログラムで作成をサポートする ASP.NET MVC では、組み込みの"Html.ActionLink()"ヘルパー メソッドを利用する&lt;、&gt;で別のアクション メソッドにリンク要素をコント ローラー:</span><span class="sxs-lookup"><span data-stu-id="86597-269">An alternative approach we can use is to take advantage of the built-in "Html.ActionLink()" helper method within ASP.NET MVC that supports programmatically creating an HTML &lt;a&gt; element that links to another action method on a Controller:</span></span>

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample11.aspx)]

<span data-ttu-id="86597-270">Html.ActionLink() ヘルパー メソッドの最初のパラメーターを表示するリンク テキストは、(ここでは、タイトル、夕食の)、2 番目のパラメーター (この場合、詳細メソッド) へのリンクを生成するコント ローラーのアクション名は、3 番目のパラメーター(プロパティの名前/値を持つ匿名型として実装)、アクションに送信するパラメーターのセット。</span><span class="sxs-lookup"><span data-stu-id="86597-270">The first parameter to the Html.ActionLink() helper method is the link-text to display (in this case the title of the dinner), the second parameter is the Controller action name we want to generate the link to (in this case the Details method), and the third parameter is a set of parameters to send to the action (implemented as an anonymous type with property name/values).</span></span> <span data-ttu-id="86597-271">ここでは dinner おにリンクして、ASP.NET MVC の既定の URL ルーティング ルールのための"id"パラメーターを指定することは"{controller}/{controller}/{id}"Html.ActionLink() ヘルパー メソッドは、次の出力を生成します。</span><span class="sxs-lookup"><span data-stu-id="86597-271">In this case we are specifying the "id" parameter of the dinner we want to link to, and because the default URL routing rule in ASP.NET MVC is "{Controller}/{Action}/{id}" the Html.ActionLink() helper method will generate the following output:</span></span>

[!code-html[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample12.html)]

<span data-ttu-id="86597-272">当社 Index.aspx ビューは、Html.ActionLink() ヘルパー メソッドの手法を使用し、リスト、適切な詳細情報の URL にリンクに各 dinner があります。</span><span class="sxs-lookup"><span data-stu-id="86597-272">For our Index.aspx view we'll use the Html.ActionLink() helper method approach and have each dinner in the list link to the appropriate details URL:</span></span>

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample13.aspx)]

<span data-ttu-id="86597-273">これで ヒット時と、 */Dinners* dinner 一覧は、次のように下の URL:</span><span class="sxs-lookup"><span data-stu-id="86597-273">And now when we hit the */Dinners* URL our dinner list looks like below:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image24.png)

<span data-ttu-id="86597-274">一覧でディナーのいずれかをクリックしたときは、その詳細を参照してくださいに移動したします。</span><span class="sxs-lookup"><span data-stu-id="86597-274">When we click any of the Dinners in the list we'll navigate to see details about it:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image25.png)

### <a name="convention-based-naming-and-the-views-directory-structure"></a><span data-ttu-id="86597-275">名前付け規則に基づくおよび \Views ディレクトリ構造</span><span class="sxs-lookup"><span data-stu-id="86597-275">Convention-based naming and the \Views directory structure</span></span>

<span data-ttu-id="86597-276">既定では ASP.NET MVC アプリケーションでは、テンプレートの表示を解決するときに構造体の名前付け規則に基づくディレクトリを使用します。</span><span class="sxs-lookup"><span data-stu-id="86597-276">ASP.NET MVC applications by default use a convention-based directory naming structure when resolving view templates.</span></span> <span data-ttu-id="86597-277">これにより、開発者は完全修飾の場所のパスから、コント ローラー クラス内のビューを参照するときにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="86597-277">This allows developers to avoid having to fully-qualify a location path when referencing views from within a Controller class.</span></span> <span data-ttu-id="86597-278">既定では ASP.NET MVC はファイルを検索、表示テンプレート内で、* \Views\[ControllerName]\*アプリケーションの下にあるディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="86597-278">By default ASP.NET MVC will look for the view template file within the *\Views\[ControllerName]\* directory underneath the application.</span></span>

<span data-ttu-id="86597-279">たとえば、きております – 3 つのテンプレートの表示を明示的に参照する DinnersController クラスに:"Index"、「詳細」および"NotFound"です。</span><span class="sxs-lookup"><span data-stu-id="86597-279">For example, we've been working on the DinnersController class – which explicitly references three view templates: "Index", "Details" and "NotFound".</span></span> <span data-ttu-id="86597-280">ASP.NET MVC は既定で探します内でこれらのビュー、 *\Views\Dinners*アプリケーションのルート ディレクトリ下にあるディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="86597-280">ASP.NET MVC will by default look for these views within the *\Views\Dinners* directory underneath our application root directory:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image26.png)

<span data-ttu-id="86597-281">上方法がありますが現在、プロジェクト内の 3 つのコント ローラー クラスに注意してください (DinnersController、HomeController および AccountController – 最後の 2 つが既定で追加されたプロジェクトを作成したとき)、3 つのサブ ディレクトリ (1 つずつ、コント ローラー) \Views ディレクトリ内。</span><span class="sxs-lookup"><span data-stu-id="86597-281">Notice above how there are currently three controller classes within the project (DinnersController, HomeController and AccountController – the last two were added by default when we created the project), and there are three sub-directories (one for each controller) within the \Views directory.</span></span>

<span data-ttu-id="86597-282">ホーム ネットワークおよびアカウント コント ローラーから参照されるビューは、それぞれからそのビュー テンプレートを自動的に解決*\Views\Home*と*\Views\Account*ディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="86597-282">Views referenced from the Home and Accounts controllers will automatically resolve their view templates from the respective *\Views\Home* and *\Views\Account* directories.</span></span> <span data-ttu-id="86597-283">*\Views\Shared*サブ directory は、アプリケーション内で複数のコント ローラー全体で再利用できるテンプレートの表示を格納する手段を提供します。</span><span class="sxs-lookup"><span data-stu-id="86597-283">The *\Views\Shared* sub-directory provides a way to store view templates that are re-used across multiple controllers within the application.</span></span> <span data-ttu-id="86597-284">ASP.NET MVC は、ビュー テンプレートの解決を試みると、これが最初内で確認する、 *\Views\[コント ローラー]*特定のディレクトリ ビュー テンプレートを見つけられない場合がありますなります内で、 *\Views\共有*ディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="86597-284">When ASP.NET MVC attempts to resolve a view template, it will first check within the *\Views\[Controller]* specific directory, and if it can't find the view template there it will look within the *\Views\Shared* directory.</span></span>

<span data-ttu-id="86597-285">個々 のビューのテンプレートを命名する際に推奨されるガイダンス ビュー テンプレートを表示するためにその原因となったアクション メソッドと同じ名前の共有を開始します。</span><span class="sxs-lookup"><span data-stu-id="86597-285">When it comes to naming individual view templates, the recommended guidance is to have the view template share the same name as the action method that caused it to render.</span></span> <span data-ttu-id="86597-286">たとえば、「インデックス」上のアクション メソッドがビューを使用して、"Index"ビューの結果を表示するためにし、"Details"アクション メソッドは、"Details"ビューを使用してその結果を表示するためには。</span><span class="sxs-lookup"><span data-stu-id="86597-286">For example, above our "Index" action method is using the "Index" view to render the view result, and the "Details" action method is using the "Details" view to render its results.</span></span> <span data-ttu-id="86597-287">これにより、簡単にすばやく参照するテンプレートが各アクションに関連付けられています。</span><span class="sxs-lookup"><span data-stu-id="86597-287">This makes it easy to quickly see which template is associated with each action.</span></span>

<span data-ttu-id="86597-288">開発者は、ビュー テンプレートがコント ローラーで呼び出されるアクション メソッドとして同じ名前を持つ場合、ビュー テンプレート名を明示的に指定する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="86597-288">Developers do not need to explicitly specify the view template name when the view template has the same name as the action method being invoked on the controller.</span></span> <span data-ttu-id="86597-289">おできます代わりにだけ、モデル オブジェクトに渡します"View()"ヘルパー メソッド (ビューの名前を指定)、なしと ASP.NET MVC を使用することが自動的に推論、 *\Views\[ControllerName]\[ActionName]*レンダリングへのディスク上のビュー テンプレート。</span><span class="sxs-lookup"><span data-stu-id="86597-289">We can instead just pass the model object to the "View()" helper method (without specifying the view name), and ASP.NET MVC will automatically infer that we want to use the *\Views\[ControllerName]\[ActionName]* view template on disk to render it.</span></span>

<span data-ttu-id="86597-290">これにより、コント ローラーのコードを少し、クリーンアップをコードで 2 回名前と重複を回避するため。</span><span class="sxs-lookup"><span data-stu-id="86597-290">This allows us to clean up our controller code a little, and avoid duplicating the name twice in our code:</span></span>

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample14.cs)]

<span data-ttu-id="86597-291">上記のコードは、サイトのエクスペリエンスを nice Dinner 一覧と詳細を実装するために必要なすべてです。</span><span class="sxs-lookup"><span data-stu-id="86597-291">The above code is all that is needed to implement a nice Dinner listing/details experience for the site.</span></span>

#### <a name="next-step"></a><span data-ttu-id="86597-292">次の手順</span><span class="sxs-lookup"><span data-stu-id="86597-292">Next Step</span></span>

<span data-ttu-id="86597-293">構築されたエクスペリエンスをブラウズ nice Dinner があるようになりました。</span><span class="sxs-lookup"><span data-stu-id="86597-293">We now have a nice Dinner browsing experience built.</span></span>

<span data-ttu-id="86597-294">みましょう CRUD (Create、Read、Update、Delete) データ形式のサポートを編集できるようになりました。</span><span class="sxs-lookup"><span data-stu-id="86597-294">Let's now enable CRUD (Create, Read, Update, Delete) data form editing support.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="86597-295">[前へ](build-a-model-with-business-rule-validations.md)
> [次へ](provide-crud-create-read-update-delete-data-form-entry-support.md)</span><span class="sxs-lookup"><span data-stu-id="86597-295">[Previous](build-a-model-with-business-rule-validations.md)
[Next](provide-crud-create-read-update-delete-data-form-entry-support.md)</span></span>
