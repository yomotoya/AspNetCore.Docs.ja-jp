---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
title: 'パート 2: コント ローラー |Microsoft Docs'
author: jongalloway
description: このチュートリアル シリーズでは、すべての ASP.NET MVC のミュージック ストア サンプル アプリケーションをビルドする手順について説明します。 第 2 部では、コント ローラーについて説明します。
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: 998ce4e1-9d72-435b-8f1c-399a10ae4360
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
msc.type: authoredcontent
ms.openlocfilehash: 8824d5d2f5670aee2df6dc6e74767e4a851dd4ae
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841096"
---
<a name="part-2-controllers"></a><span data-ttu-id="0fd69-104">パート 2: コント ローラー</span><span class="sxs-lookup"><span data-stu-id="0fd69-104">Part 2: Controllers</span></span>
====================
<span data-ttu-id="0fd69-105">によって[Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="0fd69-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="0fd69-106">MVC のミュージック ストアは、チュートリアル アプリケーションを紹介し、web 開発用の ASP.NET MVC と Visual Studio を使用する方法をステップ バイ ステップについて説明します。</span><span class="sxs-lookup"><span data-stu-id="0fd69-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="0fd69-107">MVC のミュージック ストアは、オンラインで音楽のアルバムを販売し、基本的なサイトの管理、ユーザー サインインし、買い物カゴの機能を実装する軽量サンプル ストア実装です。</span><span class="sxs-lookup"><span data-stu-id="0fd69-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="0fd69-108">このチュートリアル シリーズでは、すべての ASP.NET MVC のミュージック ストア サンプル アプリケーションをビルドする手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="0fd69-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="0fd69-109">第 2 部では、コント ローラーについて説明します。</span><span class="sxs-lookup"><span data-stu-id="0fd69-109">Part 2 covers Controllers.</span></span>


<span data-ttu-id="0fd69-110">従来の web フレームワークでは、受信した Url は通常ディスク上のファイルにマップされます。</span><span class="sxs-lookup"><span data-stu-id="0fd69-110">With traditional web frameworks, incoming URLs are typically mapped to files on disk.</span></span> <span data-ttu-id="0fd69-111">例: のような要求 URL の"/ディスク上のファイル"または"/されず"、「繰り返し」または「されず」ファイルで処理される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="0fd69-111">For example: a request for a URL like "/Products.aspx" or "/Products.php" might be processed by a "Products.aspx" or "Products.php" file.</span></span>

<span data-ttu-id="0fd69-112">Web ベースの MVC フレームワークは、少し異なる方法で Url をサーバー コードにマップします。</span><span class="sxs-lookup"><span data-stu-id="0fd69-112">Web-based MVC frameworks map URLs to server code in a slightly different way.</span></span> <span data-ttu-id="0fd69-113">受信した Url をファイルにマップするには、代わりには、代わりに、クラスのメソッドに Url をマップします。</span><span class="sxs-lookup"><span data-stu-id="0fd69-113">Instead of mapping incoming URLs to files, they instead map URLs to methods on classes.</span></span> <span data-ttu-id="0fd69-114">これらのクラスは「コント ローラー」と呼ばれますとユーザー入力を処理する受信 HTTP 要求を処理する責任が、クライアント バックアップを取得して、データの保存と送信する応答を決定する (HTML を表示、ダウンロード ファイルを別にリダイレクトURL など)。</span><span class="sxs-lookup"><span data-stu-id="0fd69-114">These classes are called "Controllers" and they are responsible for processing incoming HTTP requests, handling user input, retrieving and saving data, and determining the response to send back to the client (display HTML, download a file, redirect to a different URL, etc.).</span></span>

## <a name="adding-a-homecontroller"></a><span data-ttu-id="0fd69-115">HomeController の追加</span><span class="sxs-lookup"><span data-stu-id="0fd69-115">Adding a HomeController</span></span>

<span data-ttu-id="0fd69-116">サイトのホーム ページに Url を処理するコント ローラー クラスを追加することで、MVC Music Store アプリケーションをまず。</span><span class="sxs-lookup"><span data-stu-id="0fd69-116">We'll begin our MVC Music Store application by adding a Controller class that will handle URLs to the Home page of our site.</span></span> <span data-ttu-id="0fd69-117">ASP.NET MVC の既定の名前付け規則に従うがされ HomeController を付けます。</span><span class="sxs-lookup"><span data-stu-id="0fd69-117">We'll follow the default naming conventions of ASP.NET MVC and call it HomeController.</span></span>

<span data-ttu-id="0fd69-118">ソリューション エクスプ ローラー内でコント ローラー」フォルダーを右クリックし、"Add"し、[コント ローラー] コマンドを選択します。</span><span class="sxs-lookup"><span data-stu-id="0fd69-118">Right-click the "Controllers" folder within the Solution Explorer and select "Add", and then the "Controller…" command:</span></span>

![](mvc-music-store-part-2/_static/image1.jpg)

<span data-ttu-id="0fd69-119">「コント ローラーの追加」ダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="0fd69-119">This will bring up the "Add Controller" dialog.</span></span> <span data-ttu-id="0fd69-120">「HomeController」コント ローラーを指定し、、追加ボタンを押します。</span><span class="sxs-lookup"><span data-stu-id="0fd69-120">Name the controller "HomeController" and press the Add button.</span></span>

![](mvc-music-store-part-2/_static/image1.png)

<span data-ttu-id="0fd69-121">これにより、次のコードで、HomeController.cs で、新しいファイルが作成されます。</span><span class="sxs-lookup"><span data-stu-id="0fd69-121">This will create a new file, HomeController.cs, with the following code:</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample1.cs)]

<span data-ttu-id="0fd69-122">限りシンプルにしてみましょう文字列を返すだけ簡単な方法で Index メソッドを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="0fd69-122">To start as simply as possible, let's replace the Index method with a simple method that just returns a string.</span></span> <span data-ttu-id="0fd69-123">私たちは 2 つの変更を行います。</span><span class="sxs-lookup"><span data-stu-id="0fd69-123">We'll make two changes:</span></span>

- <span data-ttu-id="0fd69-124">ActionResult ではなく文字列を返す方法を変更します。</span><span class="sxs-lookup"><span data-stu-id="0fd69-124">Change the method to return a string instead of an ActionResult</span></span>
- <span data-ttu-id="0fd69-125">"こんにちはから Home"を返す return ステートメントを変更します。</span><span class="sxs-lookup"><span data-stu-id="0fd69-125">Change the return statement to return "Hello from Home"</span></span>

<span data-ttu-id="0fd69-126">メソッドは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="0fd69-126">The method should now look like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample2.cs)]

## <a name="running-the-application"></a><span data-ttu-id="0fd69-127">アプリケーションの実行</span><span class="sxs-lookup"><span data-stu-id="0fd69-127">Running the Application</span></span>

<span data-ttu-id="0fd69-128">今すぐサイトを実行してみましょう。</span><span class="sxs-lookup"><span data-stu-id="0fd69-128">Now let's run the site.</span></span> <span data-ttu-id="0fd69-129">当社の web サーバーを起動し、次のいずれかを使用してサイトを試す:。</span><span class="sxs-lookup"><span data-stu-id="0fd69-129">We can start our web-server and try out the site using any of the following::</span></span>

- <span data-ttu-id="0fd69-130">デバッグ ⇨ デバッグの開始 メニュー項目を選択します。</span><span class="sxs-lookup"><span data-stu-id="0fd69-130">Choose the Debug ⇨ Start Debugging menu item</span></span>
- <span data-ttu-id="0fd69-131">ツールバーの緑色の矢印ボタンをクリックします。 ![](mvc-music-store-part-2/_static/image2.jpg)</span><span class="sxs-lookup"><span data-stu-id="0fd69-131">Click the Green arrow button in the toolbar ![](mvc-music-store-part-2/_static/image2.jpg)</span></span>
- <span data-ttu-id="0fd69-132">キーボード ショートカット、f5 キーを使用します。</span><span class="sxs-lookup"><span data-stu-id="0fd69-132">Use the keyboard shortcut, F5.</span></span>

<span data-ttu-id="0fd69-133">上記の手順のいずれかを使用して、このプロジェクトでのコンパイルし、組み込ま Visual Web Developer を開始するには、ASP.NET 開発サーバーと、します。</span><span class="sxs-lookup"><span data-stu-id="0fd69-133">Using any of the above steps will compile our project, and then cause the ASP.NET Development Server that is built-into Visual Web Developer to start.</span></span> <span data-ttu-id="0fd69-134">通知を ASP.NET 開発サーバーが開始されたことを示すために、画面の下隅にある表示され、下で実行されているポート番号が表示されます。</span><span class="sxs-lookup"><span data-stu-id="0fd69-134">A notification will appear in the bottom corner of the screen to indicate that the ASP.NET Development Server has started up, and will show the port number that it is running under.</span></span>

![](mvc-music-store-part-2/_static/image2.png)

<span data-ttu-id="0fd69-135">Visual Web Developer が、web サーバーを指す URL のブラウザー ウィンドウを自動的に開きます。</span><span class="sxs-lookup"><span data-stu-id="0fd69-135">Visual Web Developer will then automatically open a browser window whose URL points to our web-server.</span></span> <span data-ttu-id="0fd69-136">これにより、web アプリケーションを簡単に試すことが。</span><span class="sxs-lookup"><span data-stu-id="0fd69-136">This will allow us to quickly try out our web application:</span></span>

![](mvc-music-store-part-2/_static/image3.png)

<span data-ttu-id="0fd69-137">では、非常に速かった – 新しい web サイトを作成しましたが、関数の場合、次の 3 つの行を追加して、ブラウザーでテキストがあります。</span><span class="sxs-lookup"><span data-stu-id="0fd69-137">Okay, that was pretty quick – we created a new website, added a three line function, and we've got text in a browser.</span></span> <span data-ttu-id="0fd69-138">いないきわめてが、開始します。</span><span class="sxs-lookup"><span data-stu-id="0fd69-138">Not rocket science, but it's a start.</span></span>

<span data-ttu-id="0fd69-139">*注: Visual Web Developer には、無料の「ポート」ランダムな番号で、web サイトを実行する ASP.NET 開発サーバーが含まれています。上記のスクリーン ショットでは、サイトが実行されている`http://localhost:26641/`26641 ポートを使用しているため、します。実際のポート番号は別になります。このチュートリアルでは URL の like/Store/Browse について説明と、は、ポート番号の後が変わります。26641 のポート番号と仮定すると、/ストア/参照への参照を参照する`http://localhost:26641/Store/Browse`します。*</span><span class="sxs-lookup"><span data-stu-id="0fd69-139">*Note: Visual Web Developer includes the ASP.NET Development Server, which will run your website on a random free "port" number. In the screenshot above, the site is running at `http://localhost:26641/`, so it's using port 26641. Your port number will be different. When we talk about URL's like /Store/Browse in this tutorial, that will go after the port number. Assuming a port number of 26641, browsing to /Store/Browse will mean browsing to `http://localhost:26641/Store/Browse`.*</span></span>

## <a name="adding-a-storecontroller"></a><span data-ttu-id="0fd69-140">StoreController を追加します。</span><span class="sxs-lookup"><span data-stu-id="0fd69-140">Adding a StoreController</span></span>

<span data-ttu-id="0fd69-141">サイトのホーム ページを実装する単純な HomeController を追加しました。</span><span class="sxs-lookup"><span data-stu-id="0fd69-141">We added a simple HomeController that implements the Home Page of our site.</span></span> <span data-ttu-id="0fd69-142">閲覧、ミュージック ストアの機能を実装するために使用する別のコント ローラーを追加してみましょう。</span><span class="sxs-lookup"><span data-stu-id="0fd69-142">Let's now add another controller that we'll use to implement the browsing functionality of our music store.</span></span> <span data-ttu-id="0fd69-143">ストア コント ローラーは、3 つのシナリオをサポートします。</span><span class="sxs-lookup"><span data-stu-id="0fd69-143">Our store controller will support three scenarios:</span></span>

- <span data-ttu-id="0fd69-144">ミュージック ストアで音楽のジャンルのリスト ページ</span><span class="sxs-lookup"><span data-stu-id="0fd69-144">A listing page of the music genres in our music store</span></span>
- <span data-ttu-id="0fd69-145">すべての音楽の album には、特定のジャンルを一覧表示する [参照] ページ</span><span class="sxs-lookup"><span data-stu-id="0fd69-145">A browse page that lists all of the music albums in a particular genre</span></span>
- <span data-ttu-id="0fd69-146">特定の音楽のアルバムに関する情報を表示するための詳細ページ</span><span class="sxs-lookup"><span data-stu-id="0fd69-146">A details page that shows information about a specific music album</span></span>

<span data-ttu-id="0fd69-147">新しい StoreController クラスを追加してを起動します.</span><span class="sxs-lookup"><span data-stu-id="0fd69-147">We'll start by adding a new StoreController class..</span></span> <span data-ttu-id="0fd69-148">まだインストールしていない場合、ブラウザーを閉じるか、デバッグ ⇨ デバッグの停止 メニュー項目を選択して、アプリケーションの実行を停止します。</span><span class="sxs-lookup"><span data-stu-id="0fd69-148">If you haven't already, stop running the application either by closing the browser or selecting the Debug ⇨ Stop Debugging menu item.</span></span>

<span data-ttu-id="0fd69-149">新しい StoreController を追加します。</span><span class="sxs-lookup"><span data-stu-id="0fd69-149">Now add a new StoreController.</span></span> <span data-ttu-id="0fd69-150">HomeController で行ったように、同様これは、ソリューション エクスプ ローラー内でコント ローラー」フォルダーを右クリックし、追加の選択によって&gt;コント ローラーのメニュー項目</span><span class="sxs-lookup"><span data-stu-id="0fd69-150">Just like we did with HomeController, we'll do this by right-clicking on the "Controllers" folder within the Solution Explorer and choosing the Add-&gt;Controller menu item</span></span>

![](mvc-music-store-part-2/_static/image4.png)

<span data-ttu-id="0fd69-151">この新しい StoreController は既に"Index"メソッドを使用しています。</span><span class="sxs-lookup"><span data-stu-id="0fd69-151">Our new StoreController already has an "Index" method.</span></span> <span data-ttu-id="0fd69-152">この"Index"メソッドを使用して、ミュージック ストア内のすべてのジャンルを一覧表示された、リスト ページを実装します。</span><span class="sxs-lookup"><span data-stu-id="0fd69-152">We'll use this "Index" method to implement our listing page that lists all genres in our music store.</span></span> <span data-ttu-id="0fd69-153">シナリオを実装する、2 つの他の処理するために、StoreController する 2 つの追加メソッドも追加します。 参照と詳細。</span><span class="sxs-lookup"><span data-stu-id="0fd69-153">We'll also add two additional methods to implement the two other scenarios we want our StoreController to handle: Browse and Details.</span></span>

<span data-ttu-id="0fd69-154">コント ローラー内のこれらのメソッド (インデックス、参照、および詳細情報) を「コント ローラー アクション」と呼びます。 HomeController.Index () のアクション メソッドで既に説明した、仕事が URL 要求に応答し、(一般) どのようなコンテンツを決定するにはブラウザーまたは URL を呼び出すユーザーに送信する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0fd69-154">These methods (Index, Browse and Details) within our Controller are called "Controller Actions", and as you've already seen with the HomeController.Index()action method, their job is to respond to URL requests and (generally speaking) determine what content should be sent back to the browser or user that invoked the URL.</span></span>

<span data-ttu-id="0fd69-155">文字列"こんにちはから Store.Index()"を返す theIndex() メソッドを変更することで、StoreController 実装をまずし、Browse() と Details() の同様のメソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="0fd69-155">We'll start our StoreController implementation by changing theIndex() method to return the string "Hello from Store.Index()" and we'll add similar methods for Browse() and Details():</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample3.cs)]

<span data-ttu-id="0fd69-156">プロジェクトをもう一度実行して、次の Url を参照してください。</span><span class="sxs-lookup"><span data-stu-id="0fd69-156">Run the project again and browse the following URLs:</span></span>

- <span data-ttu-id="0fd69-157">/ストア</span><span class="sxs-lookup"><span data-stu-id="0fd69-157">/Store</span></span>
- <span data-ttu-id="0fd69-158">/ストア/参照</span><span class="sxs-lookup"><span data-stu-id="0fd69-158">/Store/Browse</span></span>
- <span data-ttu-id="0fd69-159">/保存/詳細</span><span class="sxs-lookup"><span data-stu-id="0fd69-159">/Store/Details</span></span>

<span data-ttu-id="0fd69-160">これらの Url にアクセスする、コント ローラー内のアクション メソッドを呼び出すし、文字列の応答を返します。</span><span class="sxs-lookup"><span data-stu-id="0fd69-160">Accessing these URLs will invoke the action methods within our Controller and return string responses:</span></span>

![](mvc-music-store-part-2/_static/image5.png)

<span data-ttu-id="0fd69-161">ですしますが、これらは、単に定数文字列。</span><span class="sxs-lookup"><span data-stu-id="0fd69-161">That's great, but these are just constant strings.</span></span> <span data-ttu-id="0fd69-162">しましょうに動的で、URL から情報を取得し、出力ページに表示します。</span><span class="sxs-lookup"><span data-stu-id="0fd69-162">Let's make them dynamic, so they take information from the URL and display it in the page output.</span></span>

<span data-ttu-id="0fd69-163">まず、URL からクエリ文字列値を取得する参照アクション メソッドを変更します。</span><span class="sxs-lookup"><span data-stu-id="0fd69-163">First we'll change the Browse action method to retrieve a querystring value from the URL.</span></span> <span data-ttu-id="0fd69-164">このメソッドは、アクション メソッドに"genre"パラメーターを追加することで実行できます。</span><span class="sxs-lookup"><span data-stu-id="0fd69-164">We can do this by adding a "genre" parameter to our action method.</span></span> <span data-ttu-id="0fd69-165">この作業を行う ASP.NET MVC は自動的に、アクション メソッドには、"genre"という名前のメソッドが呼び出されたときにクエリ文字列またはフォーム post パラメーターを渡します。</span><span class="sxs-lookup"><span data-stu-id="0fd69-165">When we do this ASP.NET MVC will automatically pass any querystring or form post parameters named "genre" to our action method when it is invoked.</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample4.cs)]

<span data-ttu-id="0fd69-166">*注: ユーザー入力をサニタイズするのに HttpUtility.HtmlEncode ユーティリティ メソッドを使用しています。これにより、ユーザーが/Store/Browse などのリンクを使用して、ビューに Javascript を挿入することからできないようにしますか。ジャンル =&lt;スクリプト&gt;window.location='http://hackersite.com'&lt;/script&gt;します。*</span><span class="sxs-lookup"><span data-stu-id="0fd69-166">*Note: We're using the HttpUtility.HtmlEncode utility method to sanitize the user input. This prevents users from injecting Javascript into our View with a link like /Store/Browse?Genre=&lt;script&gt;window.location='http://hackersite.com'&lt;/script&gt;.*</span></span>

<span data-ttu-id="0fd69-167">今すぐ/ストア/参照をブラウズしますか?ジャンル Disco を =</span><span class="sxs-lookup"><span data-stu-id="0fd69-167">Now let's browse to /Store/Browse?Genre=Disco</span></span>

![](mvc-music-store-part-2/_static/image6.png)

<span data-ttu-id="0fd69-168">読み取り、という名前の id。 入力パラメーターを表示、Details アクションを次に変更してみましょう</span><span class="sxs-lookup"><span data-stu-id="0fd69-168">Let's next change the Details action to read and display an input parameter named ID.</span></span> <span data-ttu-id="0fd69-169">前のメソッドとは異なりしませんに埋め込む ID 値をクエリ文字列パラメーターとして。</span><span class="sxs-lookup"><span data-stu-id="0fd69-169">Unlike our previous method, we won't be embedding the ID value as a querystring parameter.</span></span> <span data-ttu-id="0fd69-170">代わりに URL 自体内で直接埋め込むことをしました。</span><span class="sxs-lookup"><span data-stu-id="0fd69-170">Instead we'll embed it directly within the URL itself.</span></span> <span data-ttu-id="0fd69-171">例:/Store/Details/5 します。</span><span class="sxs-lookup"><span data-stu-id="0fd69-171">For example: /Store/Details/5.</span></span>

<span data-ttu-id="0fd69-172">ASP.NET MVC では、何も構成しなくても簡単に実行できます。</span><span class="sxs-lookup"><span data-stu-id="0fd69-172">ASP.NET MVC lets us easily do this without having to configure anything.</span></span> <span data-ttu-id="0fd69-173">ASP.NET MVC の既定のルーティング規則では、"ID"という名前のパラメーターとしてアクション メソッド名の後、URL のセグメントを処理します。</span><span class="sxs-lookup"><span data-stu-id="0fd69-173">ASP.NET MVC's default routing convention is to treat the segment of a URL after the action method name as a parameter named "ID".</span></span> <span data-ttu-id="0fd69-174">アクション メソッドは、ID が付けられたパラメーターを持つ場合、ASP.NET MVC は自動的に URL セグメントにパラメーターとして渡します。</span><span class="sxs-lookup"><span data-stu-id="0fd69-174">If your action method has a parameter named ID then ASP.NET MVC will automatically pass the URL segment to you as a parameter.</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample5.cs)]

<span data-ttu-id="0fd69-175">アプリケーションを実行して、/Store/Details/5 を参照してください。</span><span class="sxs-lookup"><span data-stu-id="0fd69-175">Run the application and browse to /Store/Details/5:</span></span>

![](mvc-music-store-part-2/_static/image7.png)

<span data-ttu-id="0fd69-176">これまでに実行した内容を確認しましょう。</span><span class="sxs-lookup"><span data-stu-id="0fd69-176">Let's recap what we've done so far:</span></span>

- <span data-ttu-id="0fd69-177">Visual Web Developer で新しい ASP.NET MVC プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="0fd69-177">We've created a new ASP.NET MVC project in Visual Web Developer</span></span>
- <span data-ttu-id="0fd69-178">ASP.NET MVC アプリケーションの基本的なフォルダー構造を説明しました。</span><span class="sxs-lookup"><span data-stu-id="0fd69-178">We've discussed the basic folder structure of an ASP.NET MVC application</span></span>
- <span data-ttu-id="0fd69-179">ASP.NET 開発サーバーを使用して、web サイトを実行する方法を学習しました</span><span class="sxs-lookup"><span data-stu-id="0fd69-179">We've learned how to run our website using the ASP.NET Development Server</span></span>
- <span data-ttu-id="0fd69-180">2 つのコント ローラー クラスを作成しました: テンプレートを使用して、StoreController</span><span class="sxs-lookup"><span data-stu-id="0fd69-180">We've created two Controller classes: a HomeController and a StoreController</span></span>
- <span data-ttu-id="0fd69-181">URL 要求に応答し、ブラウザーに返されるテキストが、コント ローラーにアクション メソッドを追加しました</span><span class="sxs-lookup"><span data-stu-id="0fd69-181">We've added Action Methods to our controllers which respond to URL requests and return text to the browser</span></span>


> [!div class="step-by-step"]
> <span data-ttu-id="0fd69-182">[前へ](mvc-music-store-part-1.md)
> [次へ](mvc-music-store-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="0fd69-182">[Previous](mvc-music-store-part-1.md)
[Next](mvc-music-store-part-3.md)</span></span>
