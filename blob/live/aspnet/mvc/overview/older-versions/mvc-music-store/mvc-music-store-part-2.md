---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
title: "パート 2: コント ローラー |Microsoft ドキュメント"
author: jongalloway
description: "このチュートリアルの系列では、すべての ASP.NET MVC Music Store サンプル アプリケーションをビルドする手順について説明します。 第 2 部では、コント ローラーについて説明します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 998ce4e1-9d72-435b-8f1c-399a10ae4360
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
msc.type: authoredcontent
ms.openlocfilehash: bdafd751e996e759d516d0fa25b09eff21241ed7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="part-2-controllers"></a><span data-ttu-id="5073e-104">パート 2: コント ローラー</span><span class="sxs-lookup"><span data-stu-id="5073e-104">Part 2: Controllers</span></span>
====================
<span data-ttu-id="5073e-105">によって[Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="5073e-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="5073e-106">MVC Music Store は、チュートリアル アプリケーションを紹介し、ASP.NET MVC と Visual Studio web 開発を使用する方法の詳細な手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="5073e-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="5073e-107">MVC 音楽ストアは、オンラインにして、音楽のアルバムを販売し、基本的なサイトの管理、ユーザー サインインおよび買い物カゴの機能を実装する軽量なサンプル ストアの実装です。</span><span class="sxs-lookup"><span data-stu-id="5073e-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="5073e-108">このチュートリアルの系列では、すべての ASP.NET MVC Music Store サンプル アプリケーションをビルドする手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="5073e-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="5073e-109">第 2 部では、コント ローラーについて説明します。</span><span class="sxs-lookup"><span data-stu-id="5073e-109">Part 2 covers Controllers.</span></span>


<span data-ttu-id="5073e-110">従来の web フレームワークで受信した Url は通常、ディスク上のファイルにマップします。</span><span class="sxs-lookup"><span data-stu-id="5073e-110">With traditional web frameworks, incoming URLs are typically mapped to files on disk.</span></span> <span data-ttu-id="5073e-111">例: などの URL に対する要求"/繰り返し"または"/Products.php"、「繰り返し」または"Products.php"ファイルによって処理される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="5073e-111">For example: a request for a URL like "/Products.aspx" or "/Products.php" might be processed by a "Products.aspx" or "Products.php" file.</span></span>

<span data-ttu-id="5073e-112">MVC フレームワークの web ベースでは、少し異なる方法で、Url がサーバー コードをマップします。</span><span class="sxs-lookup"><span data-stu-id="5073e-112">Web-based MVC frameworks map URLs to server code in a slightly different way.</span></span> <span data-ttu-id="5073e-113">受信 Url をファイルにマップするには、代わりには、代わりに、クラスのメソッドに Url をマップします。</span><span class="sxs-lookup"><span data-stu-id="5073e-113">Instead of mapping incoming URLs to files, they instead map URLs to methods on classes.</span></span> <span data-ttu-id="5073e-114">これらのクラスは「コント ローラー」と呼ばれますとユーザー入力の処理、入力方向の HTTP 要求の処理を担当している、クライアント バックアップを取得して、データを保存および送信する応答を決定する (HTML を表示、ファイルをダウンロードする、別のサーバーに対するリダイレクトURL など)。</span><span class="sxs-lookup"><span data-stu-id="5073e-114">These classes are called "Controllers" and they are responsible for processing incoming HTTP requests, handling user input, retrieving and saving data, and determining the response to send back to the client (display HTML, download a file, redirect to a different URL, etc.).</span></span>

## <a name="adding-a-homecontroller"></a><span data-ttu-id="5073e-115">HomeController の追加</span><span class="sxs-lookup"><span data-stu-id="5073e-115">Adding a HomeController</span></span>

<span data-ttu-id="5073e-116">マイクロソフトのサイトのホーム ページに Url を処理するコント ローラー クラスを追加することによって、MVC 音楽ストア アプリケーションをまずします。</span><span class="sxs-lookup"><span data-stu-id="5073e-116">We'll begin our MVC Music Store application by adding a Controller class that will handle URLs to the Home page of our site.</span></span> <span data-ttu-id="5073e-117">ASP.NET MVC の既定の名前付け規則に従うし、HomeController を呼び出すおします。</span><span class="sxs-lookup"><span data-stu-id="5073e-117">We'll follow the default naming conventions of ASP.NET MVC and call it HomeController.</span></span>

<span data-ttu-id="5073e-118">ソリューション エクスプ ローラー内でコント ローラー」フォルダーを右クリックし、"Add"および「コント ローラー…」選択</span><span class="sxs-lookup"><span data-stu-id="5073e-118">Right-click the "Controllers" folder within the Solution Explorer and select "Add", and then the "Controller…"</span></span> <span data-ttu-id="5073e-119">コマンド:</span><span class="sxs-lookup"><span data-stu-id="5073e-119">command:</span></span>

![](mvc-music-store-part-2/_static/image1.jpg)

<span data-ttu-id="5073e-120">これは、「コント ローラーの追加」ダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="5073e-120">This will bring up the "Add Controller" dialog.</span></span> <span data-ttu-id="5073e-121">コント ローラー"HomeController"という名前を追加 ボタンを押します。</span><span class="sxs-lookup"><span data-stu-id="5073e-121">Name the controller "HomeController" and press the Add button.</span></span>

![](mvc-music-store-part-2/_static/image1.png)

<span data-ttu-id="5073e-122">これにより、次のコードで HomeController.cs、新しいファイルが作成されます。</span><span class="sxs-lookup"><span data-stu-id="5073e-122">This will create a new file, HomeController.cs, with the following code:</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample1.cs)]

<span data-ttu-id="5073e-123">開始するには限りシンプルにしてみましょう文字列を返すだけ単純なメソッドを使用してインデックス メソッドに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="5073e-123">To start as simply as possible, let's replace the Index method with a simple method that just returns a string.</span></span> <span data-ttu-id="5073e-124">2 つの変更を確認しましょう。</span><span class="sxs-lookup"><span data-stu-id="5073e-124">We'll make two changes:</span></span>

- <span data-ttu-id="5073e-125">ActionResult ではなく文字列を返す方法を変更します。</span><span class="sxs-lookup"><span data-stu-id="5073e-125">Change the method to return a string instead of an ActionResult</span></span>
- <span data-ttu-id="5073e-126">「こんにちはからホーム」を返す return ステートメントを変更します。</span><span class="sxs-lookup"><span data-stu-id="5073e-126">Change the return statement to return "Hello from Home"</span></span>

<span data-ttu-id="5073e-127">メソッドは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="5073e-127">The method should now look like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample2.cs)]

## <a name="running-the-application"></a><span data-ttu-id="5073e-128">アプリケーションの実行</span><span class="sxs-lookup"><span data-stu-id="5073e-128">Running the Application</span></span>

<span data-ttu-id="5073e-129">今すぐサイトを実行してみましょう。</span><span class="sxs-lookup"><span data-stu-id="5073e-129">Now let's run the site.</span></span> <span data-ttu-id="5073e-130">当社の web サーバーを起動し、次のいずれかを使用してサイトを実行してください。</span><span class="sxs-lookup"><span data-stu-id="5073e-130">We can start our web-server and try out the site using any of the following::</span></span>

- <span data-ttu-id="5073e-131">デバッグ ⇨ デバッグの開始 メニュー項目を選択します。</span><span class="sxs-lookup"><span data-stu-id="5073e-131">Choose the Debug ⇨ Start Debugging menu item</span></span>
- <span data-ttu-id="5073e-132">ツールバーの緑色の矢印ボタンをクリックします。![](mvc-music-store-part-2/_static/image2.jpg)</span><span class="sxs-lookup"><span data-stu-id="5073e-132">Click the Green arrow button in the toolbar ![](mvc-music-store-part-2/_static/image2.jpg)</span></span>
- <span data-ttu-id="5073e-133">キーボード ショートカット、f5 キーを使用します。</span><span class="sxs-lookup"><span data-stu-id="5073e-133">Use the keyboard shortcut, F5.</span></span>

<span data-ttu-id="5073e-134">上記の手順のいずれかの方法は、プロジェクトをコンパイルしに組み込ま Visual Web Developer を開始である ASP.NET 開発サーバーです。</span><span class="sxs-lookup"><span data-stu-id="5073e-134">Using any of the above steps will compile our project, and then cause the ASP.NET Development Server that is built-into Visual Web Developer to start.</span></span> <span data-ttu-id="5073e-135">通知は、下隅にある ASP.NET 開発サーバーが、開始されたことを示すために、画面の表示され、下で実行されているポート番号が表示されます。</span><span class="sxs-lookup"><span data-stu-id="5073e-135">A notification will appear in the bottom corner of the screen to indicate that the ASP.NET Development Server has started up, and will show the port number that it is running under.</span></span>

![](mvc-music-store-part-2/_static/image2.png)

<span data-ttu-id="5073e-136">Visual Web Developer は自動的に開き、web サーバーを指す URL をブラウザー ウィンドウです。</span><span class="sxs-lookup"><span data-stu-id="5073e-136">Visual Web Developer will then automatically open a browser window whose URL points to our web-server.</span></span> <span data-ttu-id="5073e-137">これにより、web アプリケーションを迅速に試すことがします。</span><span class="sxs-lookup"><span data-stu-id="5073e-137">This will allow us to quickly try out our web application:</span></span>

![](mvc-music-store-part-2/_static/image3.png)

<span data-ttu-id="5073e-138">さて、かなりクイック – 新しい web サイトを作成しましたが 3 つのインライン関数を追加し、ブラウザーでテキストをしました。</span><span class="sxs-lookup"><span data-stu-id="5073e-138">Okay, that was pretty quick – we created a new website, added a three line function, and we've got text in a browser.</span></span> <span data-ttu-id="5073e-139">いないきわめてが、開始します。</span><span class="sxs-lookup"><span data-stu-id="5073e-139">Not rocket science, but it's a start.</span></span>

<span data-ttu-id="5073e-140">*注意: Visual Web Developer には、ランダム空き"port"番号で、web サイトを実行する ASP.NET 開発サーバーが含まれます。上記のスクリーン ショットでは、サイトが実行されているで`http://localhost:26641/`ポート 26641 使用されているため、します。使用するポート番号は別になります。このチュートリアルでは、URL の like/Store/Browse 方法について説明、ポート番号の後は変わります。26641 のポート番号を想定すると、ストア/参照する参照は意味を参照して`http://localhost:26641/Store/Browse`です。*</span><span class="sxs-lookup"><span data-stu-id="5073e-140">*Note: Visual Web Developer includes the ASP.NET Development Server, which will run your website on a random free "port" number. In the screenshot above, the site is running at `http://localhost:26641/`, so it's using port 26641. Your port number will be different. When we talk about URL's like /Store/Browse in this tutorial, that will go after the port number. Assuming a port number of 26641, browsing to /Store/Browse will mean browsing to `http://localhost:26641/Store/Browse`.*</span></span>

## <a name="adding-a-storecontroller"></a><span data-ttu-id="5073e-141">StoreController を追加します。</span><span class="sxs-lookup"><span data-stu-id="5073e-141">Adding a StoreController</span></span>

<span data-ttu-id="5073e-142">マイクロソフトのサイトのホーム ページを実装する単純な HomeController が追加されました。</span><span class="sxs-lookup"><span data-stu-id="5073e-142">We added a simple HomeController that implements the Home Page of our site.</span></span> <span data-ttu-id="5073e-143">使用して、参照、音楽ストアの機能を実装する別のコント ローラーを追加してみましょう。</span><span class="sxs-lookup"><span data-stu-id="5073e-143">Let's now add another controller that we'll use to implement the browsing functionality of our music store.</span></span> <span data-ttu-id="5073e-144">このストアのコント ローラーは、3 つのシナリオをサポートします。</span><span class="sxs-lookup"><span data-stu-id="5073e-144">Our store controller will support three scenarios:</span></span>

- <span data-ttu-id="5073e-145">音楽のジャンル、音楽ストア内のリスト ページ</span><span class="sxs-lookup"><span data-stu-id="5073e-145">A listing page of the music genres in our music store</span></span>
- <span data-ttu-id="5073e-146">すべての音楽アルバムには、特定のジャンルの一覧を表示する [参照] ページ</span><span class="sxs-lookup"><span data-stu-id="5073e-146">A browse page that lists all of the music albums in a particular genre</span></span>
- <span data-ttu-id="5073e-147">特定の音楽のアルバムに関する情報が表示される詳細ページ</span><span class="sxs-lookup"><span data-stu-id="5073e-147">A details page that shows information about a specific music album</span></span>

<span data-ttu-id="5073e-148">まず、新しい StoreController クラスの追加.</span><span class="sxs-lookup"><span data-stu-id="5073e-148">We'll start by adding a new StoreController class..</span></span> <span data-ttu-id="5073e-149">まだの場合は、ブラウザーを閉じる、またはデバッグ ⇨ デバッグの停止 メニュー項目を選択して、アプリケーションの実行を停止します。</span><span class="sxs-lookup"><span data-stu-id="5073e-149">If you haven't already, stop running the application either by closing the browser or selecting the Debug ⇨ Stop Debugging menu item.</span></span>

<span data-ttu-id="5073e-150">新しい StoreController を追加します。</span><span class="sxs-lookup"><span data-stu-id="5073e-150">Now add a new StoreController.</span></span> <span data-ttu-id="5073e-151">HomeController で行ったのと同じようになりますこの作業を行う、ソリューション エクスプ ローラー内の「コント ローラー」フォルダーを右クリックして、追加の を選択して&gt;コント ローラーのメニュー項目</span><span class="sxs-lookup"><span data-stu-id="5073e-151">Just like we did with HomeController, we'll do this by right-clicking on the "Controllers" folder within the Solution Explorer and choosing the Add-&gt;Controller menu item</span></span>

![](mvc-music-store-part-2/_static/image4.png)

<span data-ttu-id="5073e-152">この新しい StoreController は既に"Index"メソッドを使用しています。</span><span class="sxs-lookup"><span data-stu-id="5073e-152">Our new StoreController already has an "Index" method.</span></span> <span data-ttu-id="5073e-153">この"Index"メソッドを使用して、音楽ストア内のすべてのジャンルの一覧を示す、リスト ページを実装します。</span><span class="sxs-lookup"><span data-stu-id="5073e-153">We'll use this "Index" method to implement our listing page that lists all genres in our music store.</span></span> <span data-ttu-id="5073e-154">シナリオを実装する 2 つの他のマイクロソフト StoreController を処理する 2 つの追加メソッドも追加されます: 参照および詳細。</span><span class="sxs-lookup"><span data-stu-id="5073e-154">We'll also add two additional methods to implement the two other scenarios we want our StoreController to handle: Browse and Details.</span></span>

<span data-ttu-id="5073e-155">当社のコント ローラー内でのこれらのメソッド (インデックス、参照および詳細) は「コント ローラー アクション」と呼ばれます、HomeController.Index () のアクション メソッドで既に説明したように、ジョブを URL 要求に応答し、どのような内容の決定、(一般に)ブラウザーや URL を呼び出したユーザーに送信する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5073e-155">These methods (Index, Browse and Details) within our Controller are called "Controller Actions", and as you've already seen with the HomeController.Index()action method, their job is to respond to URL requests and (generally speaking) determine what content should be sent back to the browser or user that invoked the URL.</span></span>

<span data-ttu-id="5073e-156">文字列"こんにちはから Store.Index()"を返す theIndex() メソッドを変更することによって、StoreController 実装をまずし、Browse() および Details() 同様のメソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="5073e-156">We'll start our StoreController implementation by changing theIndex() method to return the string "Hello from Store.Index()" and we'll add similar methods for Browse() and Details():</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample3.cs)]

<span data-ttu-id="5073e-157">プロジェクトをもう一度実行し、次の Url を参照します。</span><span class="sxs-lookup"><span data-stu-id="5073e-157">Run the project again and browse the following URLs:</span></span>

- <span data-ttu-id="5073e-158">/ストア</span><span class="sxs-lookup"><span data-stu-id="5073e-158">/Store</span></span>
- <span data-ttu-id="5073e-159">/ストア/[参照]</span><span class="sxs-lookup"><span data-stu-id="5073e-159">/Store/Browse</span></span>
- <span data-ttu-id="5073e-160">/ストア/詳細</span><span class="sxs-lookup"><span data-stu-id="5073e-160">/Store/Details</span></span>

<span data-ttu-id="5073e-161">これらの Url にアクセスする、当社のコント ローラー内でアクション メソッドを呼び出すし、文字列の応答を返します。</span><span class="sxs-lookup"><span data-stu-id="5073e-161">Accessing these URLs will invoke the action methods within our Controller and return string responses:</span></span>

![](mvc-music-store-part-2/_static/image5.png)

<span data-ttu-id="5073e-162">ですしますが、これらは、単に定数文字列。</span><span class="sxs-lookup"><span data-stu-id="5073e-162">That's great, but these are just constant strings.</span></span> <span data-ttu-id="5073e-163">みましょう動的で、URL から情報を取得し、ページの出力に表示します。</span><span class="sxs-lookup"><span data-stu-id="5073e-163">Let's make them dynamic, so they take information from the URL and display it in the page output.</span></span>

<span data-ttu-id="5073e-164">まず URL からクエリ文字列値を取得する参照アクション メソッドを変更します。</span><span class="sxs-lookup"><span data-stu-id="5073e-164">First we'll change the Browse action method to retrieve a querystring value from the URL.</span></span> <span data-ttu-id="5073e-165">「ジャンル」パラメーターをアクション メソッドに追加することによってこの作業を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="5073e-165">We can do this by adding a "genre" parameter to our action method.</span></span> <span data-ttu-id="5073e-166">この作業を行うときに ASP.NET MVC では、クエリ文字列パラメーターまたはフォーム ポスト パラメーターが呼び出されると、アクション メソッドに「ジャンル」をという名前を自動的に渡されます。</span><span class="sxs-lookup"><span data-stu-id="5073e-166">When we do this ASP.NET MVC will automatically pass any querystring or form post parameters named "genre" to our action method when it is invoked.</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample4.cs)]

<span data-ttu-id="5073e-167">*注: おしているメソッドを使用 HttpUtility.HtmlEncode ユーティリティ、ユーザー入力を不要部分を削除します。これにより、ユーザーが/Store/Browse のようなリンクを使用して、ビューに Javascript を挿入できないようにしますか。ジャンル =&lt;スクリプト&gt;window.location= 'http://hackersite.com'&lt;/script&gt;です。*</span><span class="sxs-lookup"><span data-stu-id="5073e-167">*Note: We're using the HttpUtility.HtmlEncode utility method to sanitize the user input. This prevents users from injecting Javascript into our View with a link like /Store/Browse?Genre=&lt;script&gt;window.location='http://hackersite.com'&lt;/script&gt;.*</span></span>

<span data-ttu-id="5073e-168">ここでストア/参照をブラウズしますか?ジャンル Disco を =</span><span class="sxs-lookup"><span data-stu-id="5073e-168">Now let's browse to /Store/Browse?Genre=Disco</span></span>

![](mvc-music-store-part-2/_static/image6.png)

<span data-ttu-id="5073e-169">読み取りおよび id。 をという名前の入力パラメーターを表示する、Details アクションを次に変更してみましょう</span><span class="sxs-lookup"><span data-stu-id="5073e-169">Let's next change the Details action to read and display an input parameter named ID.</span></span> <span data-ttu-id="5073e-170">前のメソッドとは異なりはクエリ文字列パラメーターとして ID 値お埋め込みではありません。</span><span class="sxs-lookup"><span data-stu-id="5073e-170">Unlike our previous method, we won't be embedding the ID value as a querystring parameter.</span></span> <span data-ttu-id="5073e-171">代わりに、URL 自体内で直接埋め込むおします。</span><span class="sxs-lookup"><span data-stu-id="5073e-171">Instead we'll embed it directly within the URL itself.</span></span> <span data-ttu-id="5073e-172">例:/Store/Details/5 です。</span><span class="sxs-lookup"><span data-stu-id="5073e-172">For example: /Store/Details/5.</span></span>

<span data-ttu-id="5073e-173">ASP.NET MVC により、何も構成することがなく簡単に実行します。</span><span class="sxs-lookup"><span data-stu-id="5073e-173">ASP.NET MVC lets us easily do this without having to configure anything.</span></span> <span data-ttu-id="5073e-174">ASP.NET MVC の既定のルーティング規則では、"ID"という名前のパラメーターとしてアクション メソッド名の後に URL のセグメントを処理します。</span><span class="sxs-lookup"><span data-stu-id="5073e-174">ASP.NET MVC's default routing convention is to treat the segment of a URL after the action method name as a parameter named "ID".</span></span> <span data-ttu-id="5073e-175">アクション メソッドに ID をという名前のパラメーターがある場合、ASP.NET MVC は自動的に URL セグメントをパラメーターとして渡します。</span><span class="sxs-lookup"><span data-stu-id="5073e-175">If your action method has a parameter named ID then ASP.NET MVC will automatically pass the URL segment to you as a parameter.</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample5.cs)]

<span data-ttu-id="5073e-176">アプリケーションを実行し、/Store/Details/5 を参照します。</span><span class="sxs-lookup"><span data-stu-id="5073e-176">Run the application and browse to /Store/Details/5:</span></span>

![](mvc-music-store-part-2/_static/image7.png)

<span data-ttu-id="5073e-177">これまで実行したの要約します。</span><span class="sxs-lookup"><span data-stu-id="5073e-177">Let's recap what we've done so far:</span></span>

- <span data-ttu-id="5073e-178">Visual Web Developer で、新しい ASP.NET MVC プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="5073e-178">We've created a new ASP.NET MVC project in Visual Web Developer</span></span>
- <span data-ttu-id="5073e-179">ASP.NET MVC アプリケーションの基本的なフォルダー構造について説明しました</span><span class="sxs-lookup"><span data-stu-id="5073e-179">We've discussed the basic folder structure of an ASP.NET MVC application</span></span>
- <span data-ttu-id="5073e-180">ASP.NET 開発サーバーを使用して、web サイトを実行する方法を学習しました</span><span class="sxs-lookup"><span data-stu-id="5073e-180">We've learned how to run our website using the ASP.NET Development Server</span></span>
- <span data-ttu-id="5073e-181">2 つのコント ローラー クラスを作成しました: テンプレートを使用して、StoreController</span><span class="sxs-lookup"><span data-stu-id="5073e-181">We've created two Controller classes: a HomeController and a StoreController</span></span>
- <span data-ttu-id="5073e-182">URL 要求に応答し、ブラウザーにテキストを返しますが、コント ローラーにアクション メソッドが追加されました</span><span class="sxs-lookup"><span data-stu-id="5073e-182">We've added Action Methods to our controllers which respond to URL requests and return text to the browser</span></span>


>[!div class="step-by-step"]
<span data-ttu-id="5073e-183">[前へ](mvc-music-store-part-1.md)
[次へ](mvc-music-store-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="5073e-183">[Previous](mvc-music-store-part-1.md)
[Next](mvc-music-store-part-3.md)</span></span>
