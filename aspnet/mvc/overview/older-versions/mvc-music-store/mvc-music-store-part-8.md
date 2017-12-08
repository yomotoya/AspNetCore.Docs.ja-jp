---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
title: "手順 8: ショッピング カート Ajax の更新と |Microsoft ドキュメント"
author: jongalloway
description: "このチュートリアルの系列では、すべての ASP.NET MVC Music Store サンプル アプリケーションをビルドする手順について説明します。 8 の一部では、Ajax の更新でショッピング カートについて説明します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 26b2f55e-ed42-4277-89b0-c941eb754145
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
msc.type: authoredcontent
ms.openlocfilehash: 75e1dff96f8b56d74c28ff9d522f4766fbad669f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="part-8-shopping-cart-with-ajax-updates"></a><span data-ttu-id="bf885-104">手順 8: ショッピング カートの Ajax の更新</span><span class="sxs-lookup"><span data-stu-id="bf885-104">Part 8: Shopping Cart with Ajax Updates</span></span>
====================
<span data-ttu-id="bf885-105">によって[Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="bf885-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="bf885-106">MVC Music Store は、チュートリアル アプリケーションを紹介し、ASP.NET MVC と Visual Studio web 開発を使用する方法の詳細な手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="bf885-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="bf885-107">MVC 音楽ストアは、オンラインにして、音楽のアルバムを販売し、基本的なサイトの管理、ユーザー サインインおよび買い物カゴの機能を実装する軽量なサンプル ストアの実装です。</span><span class="sxs-lookup"><span data-stu-id="bf885-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="bf885-108">このチュートリアルの系列では、すべての ASP.NET MVC Music Store サンプル アプリケーションをビルドする手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="bf885-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="bf885-109">8 の一部では、Ajax の更新でショッピング カートについて説明します。</span><span class="sxs-lookup"><span data-stu-id="bf885-109">Part 8 covers Shopping Cart with Ajax Updates.</span></span>


<span data-ttu-id="bf885-110">登録、せずカゴにアルバムを配置するユーザーを使用するをおは完全なチェック アウトをゲストとして登録する必要があります。</span><span class="sxs-lookup"><span data-stu-id="bf885-110">We'll allow users to place albums in their cart without registering, but they'll need to register as guests to complete checkout.</span></span> <span data-ttu-id="bf885-111">2 つのコント ローラーに、ショッピングおよびチェック アウト プロセスが区切られる: 匿名で、カートにアイテムを追加できるようにする ShoppingCart コント ローラーとチェック アウト プロセスを処理するチェック アウト コント ローラー。</span><span class="sxs-lookup"><span data-stu-id="bf885-111">The shopping and checkout process will be separated into two controllers: a ShoppingCart Controller which allows anonymously adding items to a cart, and a Checkout Controller which handles the checkout process.</span></span> <span data-ttu-id="bf885-112">このセクションのショッピング カートで始まるその後、次のセクションで、チェック アウト処理をビルドします。</span><span class="sxs-lookup"><span data-stu-id="bf885-112">We'll start with the Shopping Cart in this section, then build the Checkout process in the following section.</span></span>

## <a name="adding-the-cart-order-and-orderdetail-model-classes"></a><span data-ttu-id="bf885-113">カート、順序、および OrderDetail モデル クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="bf885-113">Adding the Cart, Order, and OrderDetail model classes</span></span>

<span data-ttu-id="bf885-114">ショッピング カートおよび支払いプロセスにより、いくつかの新しいクラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="bf885-114">Our Shopping Cart and Checkout processes will make use of some new classes.</span></span> <span data-ttu-id="bf885-115">モデル フォルダーを右クリックし、次のコードを持つカート クラス (Cart.cs) を追加します。</span><span class="sxs-lookup"><span data-stu-id="bf885-115">Right-click the Models folder and add a Cart class (Cart.cs) with the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample1.cs)]

<span data-ttu-id="bf885-116">このクラスは、RecordId プロパティの [キー] 属性を除くをこれまで使用して他のユーザーに非常に似ています。</span><span class="sxs-lookup"><span data-stu-id="bf885-116">This class is pretty similar to others we've used so far, with the exception of the [Key] attribute for the RecordId property.</span></span> <span data-ttu-id="bf885-117">買い物カゴの品目が匿名買い物を許可する CartID をという名前の文字列識別子がテーブルには、RecordId をという名前の整数主キーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="bf885-117">Our Cart items will have a string identifier named CartID to allow anonymous shopping, but the table includes an integer primary key named RecordId.</span></span> <span data-ttu-id="bf885-118">慣例により、Entity Framework コードで最初には、ことカートをという名前のテーブルの主キーは CartId または ID のいずれかになりますが簡単にオーバーライドできますを注釈またはコードを使用してたい場合が期待しています。</span><span class="sxs-lookup"><span data-stu-id="bf885-118">By convention, Entity Framework Code-First expects that the primary key for a table named Cart will be either CartId or ID, but we can easily override that via annotations or code if we want.</span></span> <span data-ttu-id="bf885-119">これを使用する方法、単純な規則で Entity Framework コード優先 us, に合わせて、場合の例ですがおいるによって制約されないことがない場合にあります。</span><span class="sxs-lookup"><span data-stu-id="bf885-119">This is an example of how we can use the simple conventions in Entity Framework Code-First when they suit us, but we're not constrained by them when they don't.</span></span>

<span data-ttu-id="bf885-120">次に、次のコードで Order クラス (Order.cs) を追加します。</span><span class="sxs-lookup"><span data-stu-id="bf885-120">Next, add an Order class (Order.cs) with the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample2.cs)]

<span data-ttu-id="bf885-121">このクラスは、注文の概要および配信の情報を追跡します。</span><span class="sxs-lookup"><span data-stu-id="bf885-121">This class tracks summary and delivery information for an order.</span></span> <span data-ttu-id="bf885-122">**これはまだコンパイルされません**おをまだ作成していないクラスに依存する OrderDetails ナビゲーション プロパティがあるため、します。</span><span class="sxs-lookup"><span data-stu-id="bf885-122">**It won't compile yet**, because it has an OrderDetails navigation property which depends on a class we haven't created yet.</span></span> <span data-ttu-id="bf885-123">今すぐ追加することで、クラスという名前の OrderDetail.cs、次のコードを追加することを修正してみましょう。</span><span class="sxs-lookup"><span data-stu-id="bf885-123">Let's fix that now by adding a class named OrderDetail.cs, adding the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample3.cs)]

<span data-ttu-id="bf885-124">最後の 1 つの更新に DbSet も含む、新しいそれらモデル クラスを公開する DbSets を含める MusicStoreEntities クラスを作成します&lt;アーティスト&gt;です。</span><span class="sxs-lookup"><span data-stu-id="bf885-124">We'll make one last update to our MusicStoreEntities class to include DbSets which expose those new Model classes, also including a DbSet&lt;Artist&gt;.</span></span> <span data-ttu-id="bf885-125">更新された MusicStoreEntities クラスとして表示されます以下です。</span><span class="sxs-lookup"><span data-stu-id="bf885-125">The updated MusicStoreEntities class appears as below.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample4.cs)]

## <a name="managing-the-shopping-cart-business-logic"></a><span data-ttu-id="bf885-126">ショッピング カートのビジネス ロジックを管理します。</span><span class="sxs-lookup"><span data-stu-id="bf885-126">Managing the Shopping Cart business logic</span></span>

<span data-ttu-id="bf885-127">次に、Models フォルダー ShoppingCart クラスが作成されます。</span><span class="sxs-lookup"><span data-stu-id="bf885-127">Next, we'll create the ShoppingCart class in the Models folder.</span></span> <span data-ttu-id="bf885-128">ShoppingCart モデルでは、買い物カゴ テーブルへのデータ アクセスを処理します。</span><span class="sxs-lookup"><span data-stu-id="bf885-128">The ShoppingCart model handles data access to the Cart table.</span></span> <span data-ttu-id="bf885-129">さらに、追加すると、ショッピング カートから項目を削除するビジネス ロジックを処理します。</span><span class="sxs-lookup"><span data-stu-id="bf885-129">Additionally, it will handle the business logic to for adding and removing items from the shopping cart.</span></span>

<span data-ttu-id="bf885-130">ショッピング カートにアイテムを追加するだけアカウントにサインアップするユーザーに要求するのでない、割り当てることで、ユーザー、一時的な一意の識別子 (GUID、またはグローバルに一意の識別子を使用)、ショッピング カートをアクセスしたときにします。</span><span class="sxs-lookup"><span data-stu-id="bf885-130">Since we don't want to require users to sign up for an account just to add items to their shopping cart, we will assign users a temporary unique identifier (using a GUID, or globally unique identifier) when they access the shopping cart.</span></span> <span data-ttu-id="bf885-131">ASP.NET セッション クラスを使用してこの ID を格納します。</span><span class="sxs-lookup"><span data-stu-id="bf885-131">We'll store this ID using the ASP.NET Session class.</span></span>

<span data-ttu-id="bf885-132">*メモ: ASP.NET セッションは、サイトに移動して、有効期限が切れるユーザー固有の情報を格納する便利な場所です。セッション状態の誤用はパフォーマンスに影響を持つ大規模なサイトで、ライト使用はデモンストレーションのためにも機能します。*</span><span class="sxs-lookup"><span data-stu-id="bf885-132">*Note: The ASP.NET Session is a convenient place to store user-specific information which will expire after they leave the site. While misuse of session state can have performance implications on larger sites, our light use will work well for demonstration purposes.*</span></span>

<span data-ttu-id="bf885-133">ShoppingCart クラスは、次のメソッドを公開します。</span><span class="sxs-lookup"><span data-stu-id="bf885-133">The ShoppingCart class exposes the following methods:</span></span>

<span data-ttu-id="bf885-134">**AddToCart**アルバムをパラメーターとしては、ユーザーのカートに追加します。</span><span class="sxs-lookup"><span data-stu-id="bf885-134">**AddToCart** takes an Album as a parameter and adds it to the user's cart.</span></span> <span data-ttu-id="bf885-135">カート テーブルは、各アルバムの数量を追跡しているために、必要な場合は、新しい行を作成またはだけ場合は、ユーザーが既に 1 つはアルバムのコピーを注文数量をインクリメントするためのロジックが含まれます。</span><span class="sxs-lookup"><span data-stu-id="bf885-135">Since the Cart table tracks quantity for each album, it includes logic to create a new row if needed or just increment the quantity if the user has already ordered one copy of the album.</span></span>

<span data-ttu-id="bf885-136">**RemoveFromCart**アルバムの ID を受け取り、ユーザーのカートからは削除されます。</span><span class="sxs-lookup"><span data-stu-id="bf885-136">**RemoveFromCart** takes an Album ID and removes it from the user's cart.</span></span> <span data-ttu-id="bf885-137">ユーザーには、カートのアルバムのコピーを 1 つしかありませんでしたがある、行が削除されます。</span><span class="sxs-lookup"><span data-stu-id="bf885-137">If the user only had one copy of the album in their cart, the row is removed.</span></span>

<span data-ttu-id="bf885-138">**EmptyCart**ユーザーのショッピング カートからすべての項目を削除します。</span><span class="sxs-lookup"><span data-stu-id="bf885-138">**EmptyCart** removes all items from a user's shopping cart.</span></span>

<span data-ttu-id="bf885-139">**GetCartItems** CartItems の表示や処理用の一覧を取得します。</span><span class="sxs-lookup"><span data-stu-id="bf885-139">**GetCartItems** retrieves a list of CartItems for display or processing.</span></span>

<span data-ttu-id="bf885-140">**GetCount**を取得するユーザーがショッピング カートに入っているアルバムの合計数。</span><span class="sxs-lookup"><span data-stu-id="bf885-140">**GetCount** retrieves a the total number of albums a user has in their shopping cart.</span></span>

<span data-ttu-id="bf885-141">**GetTotal**カート内のすべての項目の総コストを計算します。</span><span class="sxs-lookup"><span data-stu-id="bf885-141">**GetTotal** calculates the total cost of all items in the cart.</span></span>

<span data-ttu-id="bf885-142">**CreateOrder**順にチェック アウト フェーズ中に、ショッピング カートを変換します。</span><span class="sxs-lookup"><span data-stu-id="bf885-142">**CreateOrder** converts the shopping cart to an order during the checkout phase.</span></span>

<span data-ttu-id="bf885-143">**GetCart**カート オブジェクトを取得する、コント ローラーを使用する静的メソッドです。</span><span class="sxs-lookup"><span data-stu-id="bf885-143">**GetCart** is a static method which allows our controllers to obtain a cart object.</span></span> <span data-ttu-id="bf885-144">使用して、 **GetCartId**ユーザーのセッションから、CartId の読み取りを処理するメソッド。</span><span class="sxs-lookup"><span data-stu-id="bf885-144">It uses the **GetCartId** method to handle reading the CartId from the user's session.</span></span> <span data-ttu-id="bf885-145">GetCartId メソッドでは、ユーザーのセッションからユーザーの CartId が読み取ることができるように、HttpContextBase が必要です。</span><span class="sxs-lookup"><span data-stu-id="bf885-145">The GetCartId method requires the HttpContextBase so that it can read the user's CartId from user's session.</span></span>

<span data-ttu-id="bf885-146">ここでは、完全な**ShoppingCart クラス**:</span><span class="sxs-lookup"><span data-stu-id="bf885-146">Here's the complete **ShoppingCart class**:</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample5.cs)]

## <a name="viewmodels"></a><span data-ttu-id="bf885-147">ViewModels</span><span class="sxs-lookup"><span data-stu-id="bf885-147">ViewModels</span></span>

<span data-ttu-id="bf885-148">買い物カ ゴ コント ローラーは、いくつか複雑な情報をそのビューに、モデル オブジェクトに完全に対応していないを通信する必要があります。</span><span class="sxs-lookup"><span data-stu-id="bf885-148">Our Shopping Cart Controller will need to communicate some complex information to its views which doesn't map cleanly to our Model objects.</span></span> <span data-ttu-id="bf885-149">このビューに合わせて、モデルを変更したくありません。モデルのクラスには、ユーザー インターフェイスではなく、ドメインを表す必要があります。</span><span class="sxs-lookup"><span data-stu-id="bf885-149">We don't want to modify our Models to suit our views; Model classes should represent our domain, not the user interface.</span></span> <span data-ttu-id="bf885-150">ストア マネージャー ドロップダウンについてでは、使用しましたが、管理が困難で取得 ViewBag 経由で大量の情報を渡す ViewBag クラスを使用して、ビューに、情報を渡す 1 つのソリューションになります。</span><span class="sxs-lookup"><span data-stu-id="bf885-150">One solution would be to pass the information to our Views using the ViewBag class, as we did with the Store Manager dropdown information, but passing a lot of information via ViewBag gets hard to manage.</span></span>

<span data-ttu-id="bf885-151">この解決方法は、使用する、 *ViewModel*パターン。</span><span class="sxs-lookup"><span data-stu-id="bf885-151">A solution to this is to use the *ViewModel* pattern.</span></span> <span data-ttu-id="bf885-152">このパターンを使用する場合、特定のビューのシナリオ用に最適化されていると、テンプレートの表示に必要な動的な値/コンテンツのプロパティを公開する厳密に型指定されたクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="bf885-152">When using this pattern we create strongly-typed classes that are optimized for our specific view scenarios, and which expose properties for the dynamic values/content needed by our view templates.</span></span> <span data-ttu-id="bf885-153">コント ローラー クラスでは設定し、ビュー用に最適化されたこれらのクラスを使用するには、このビュー テンプレートに渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="bf885-153">Our controller classes can then populate and pass these view-optimized classes to our view template to use.</span></span> <span data-ttu-id="bf885-154">これにより、タイプ セーフ、コンパイル時のチェック、および IntelliSense ビュー テンプレート内のエディターです。</span><span class="sxs-lookup"><span data-stu-id="bf885-154">This enables type-safety, compile-time checking, and editor IntelliSense within view templates.</span></span>

<span data-ttu-id="bf885-155">2 つのビュー モデルを使用して、ショッピング カートのコント ローラーに作成します。 ShoppingCartViewModel がユーザーのショッピング カートの内容を保持して、ユーザーが何かを削除すると、確認情報を表示に使用される、ShoppingCartRemoveViewModelカートです。</span><span class="sxs-lookup"><span data-stu-id="bf885-155">We'll create two View Models for use in our Shopping Cart controller: the ShoppingCartViewModel will hold the contents of the user's shopping cart, and the ShoppingCartRemoveViewModel will be used to display confirmation information when a user removes something from their cart.</span></span>

<span data-ttu-id="bf885-156">わかりやすくするために、プロジェクトのルートには、新しい ViewModels フォルダーを作成してみましょう。</span><span class="sxs-lookup"><span data-stu-id="bf885-156">Let's create a new ViewModels folder in the root of our project to keep things organized.</span></span> <span data-ttu-id="bf885-157">プロジェクトを右クリックし、追加 を選択/新規フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="bf885-157">Right-click the project, select Add / New Folder.</span></span>

![](mvc-music-store-part-8/_static/image1.jpg)

<span data-ttu-id="bf885-158">ViewModels フォルダーの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="bf885-158">Name the folder ViewModels.</span></span>

![](mvc-music-store-part-8/_static/image1.png)

<span data-ttu-id="bf885-159">次に、ViewModels フォルダーに ShoppingCartViewModel クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="bf885-159">Next, add the ShoppingCartViewModel class in the ViewModels folder.</span></span> <span data-ttu-id="bf885-160">2 つのプロパティがある: カート項目、およびすべてのアイテムの合計金額をカートに保持するために 10 進値の一覧です。</span><span class="sxs-lookup"><span data-stu-id="bf885-160">It has two properties: a list of Cart items, and a decimal value to hold the total price for all items in the cart.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample6.cs)]

<span data-ttu-id="bf885-161">次の 4 つのプロパティで、ViewModels フォルダーに、ShoppingCartRemoveViewModel を追加します。</span><span class="sxs-lookup"><span data-stu-id="bf885-161">Now add the ShoppingCartRemoveViewModel to the ViewModels folder, with the following four properties.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample7.cs)]

## <a name="the-shopping-cart-controller"></a><span data-ttu-id="bf885-162">買い物カ ゴ コント ローラー</span><span class="sxs-lookup"><span data-stu-id="bf885-162">The Shopping Cart Controller</span></span>

<span data-ttu-id="bf885-163">ショッピング カート コント ローラーが次の 3 つの主な目的: カートから項目を削除する、カートにアイテムを表示するアイテムをカートに追加します。</span><span class="sxs-lookup"><span data-stu-id="bf885-163">The Shopping Cart controller has three main purposes: adding items to a cart, removing items from the cart, and viewing items in the cart.</span></span> <span data-ttu-id="bf885-164">これは、3 つのクラスを使用してため先ほど作成した: ShoppingCartViewModel、ShoppingCartRemoveViewModel、および ShoppingCart です。</span><span class="sxs-lookup"><span data-stu-id="bf885-164">It will make use of the three classes we just created: ShoppingCartViewModel, ShoppingCartRemoveViewModel, and ShoppingCart.</span></span> <span data-ttu-id="bf885-165">StoreController、StoreManagerController、MusicStoreEntities のインスタンスを保持するためのフィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="bf885-165">As in the StoreController and StoreManagerController, we'll add a field to hold an instance of MusicStoreEntities.</span></span>

<span data-ttu-id="bf885-166">空のコント ローラーのテンプレートを使用してプロジェクトに新しい Shopping Cart コント ローラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="bf885-166">Add a new Shopping Cart controller to the project using the Empty controller template.</span></span>

![](mvc-music-store-part-8/_static/image2.png)

<span data-ttu-id="bf885-167">完全な ShoppingCart コント ローラーを次に示します。</span><span class="sxs-lookup"><span data-stu-id="bf885-167">Here's the complete ShoppingCart Controller.</span></span> <span data-ttu-id="bf885-168">インデックスとコント ローラーの追加のアクションは非常に使いやすくなります。</span><span class="sxs-lookup"><span data-stu-id="bf885-168">The Index and Add Controller actions should look very familiar.</span></span> <span data-ttu-id="bf885-169">削除と CartSummary コント ローラーのアクションは、次のセクションで取り上げる 2 つの特殊なケースを処理します。</span><span class="sxs-lookup"><span data-stu-id="bf885-169">The Remove and CartSummary controller actions handle two special cases, which we'll discuss in the following section.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample8.cs)]

## <a name="ajax-updates-with-jquery"></a><span data-ttu-id="bf885-170">JQuery での Ajax の更新</span><span class="sxs-lookup"><span data-stu-id="bf885-170">Ajax Updates with jQuery</span></span>

<span data-ttu-id="bf885-171">次に、ShoppingCartViewModel を厳密に型指定、リスト ビューを前に、と同じ方法を使用してテンプレートを使用して、ショッピング カートのインデックス ページを作成します。</span><span class="sxs-lookup"><span data-stu-id="bf885-171">We'll next create a Shopping Cart Index page that is strongly typed to the ShoppingCartViewModel and uses the List View template using the same method as before.</span></span>

![](mvc-music-store-part-8/_static/image3.png)

<span data-ttu-id="bf885-172">ただし、カートから項目を削除するのに、Html.ActionLink を使用する代わりに使用されます jQuery RemoveLink HTML クラスである必要が、このビュー内のすべてのリンクのクリック イベントを「接続」。</span><span class="sxs-lookup"><span data-stu-id="bf885-172">However, instead of using an Html.ActionLink to remove items from the cart, we'll use jQuery to "wire up" the click event for all links in this view which have the HTML class RemoveLink.</span></span> <span data-ttu-id="bf885-173">フォームの送信ではなくこの click イベント ハンドラーだけと、AJAX コールバック、RemoveFromCart コント ローラーのアクションになります。</span><span class="sxs-lookup"><span data-stu-id="bf885-173">Rather than posting the form, this click event handler will just make an AJAX callback to our RemoveFromCart controller action.</span></span> <span data-ttu-id="bf885-174">RemoveFromCart 返します JSON シリアル化の結果、jQuery、コールバックは、次を解析し、jQuery を使用して、ページに次の 4 つのクイック更新を実行します。</span><span class="sxs-lookup"><span data-stu-id="bf885-174">The RemoveFromCart returns a JSON serialized result, which our jQuery callback then parses and performs four quick updates to the page using jQuery:</span></span>

- 1. <span data-ttu-id="bf885-175">一覧から削除したアルバムを削除します。</span><span class="sxs-lookup"><span data-stu-id="bf885-175">Removes the deleted album from the list</span></span>
- 2. <span data-ttu-id="bf885-176">ヘッダーのカート数を更新します。</span><span class="sxs-lookup"><span data-stu-id="bf885-176">Updates the cart count in the header</span></span>
- 3. <span data-ttu-id="bf885-177">更新メッセージをユーザーに表示します。</span><span class="sxs-lookup"><span data-stu-id="bf885-177">Displays an update message to the user</span></span>
- 4. <span data-ttu-id="bf885-178">カート合計価格を更新します。</span><span class="sxs-lookup"><span data-stu-id="bf885-178">Updates the cart total price</span></span>

<span data-ttu-id="bf885-179">削除シナリオは、インデックス ビュー内で、Ajax コールバックによって処理されているが、以降 RemoveFromCart アクションの追加のビューは不要です。</span><span class="sxs-lookup"><span data-stu-id="bf885-179">Since the remove scenario is being handled by an Ajax callback within the Index view, we don't need an additional view for RemoveFromCart action.</span></span> <span data-ttu-id="bf885-180">/ShoppingCart/Index ビューの完全なコードを次に示します。</span><span class="sxs-lookup"><span data-stu-id="bf885-180">Here is the complete code for the /ShoppingCart/Index view:</span></span>

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample9.cshtml)]

<span data-ttu-id="bf885-181">アウトこれをテストするためには、このショッピング カートにアイテムを追加することである必要があります。</span><span class="sxs-lookup"><span data-stu-id="bf885-181">In order to test this out, we need to be able to add items to our shopping cart.</span></span> <span data-ttu-id="bf885-182">更新されます、**ストアの詳細**ビューに含め、"カートに追加 ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="bf885-182">We'll update our **Store Details** view to include an "Add to cart" button.</span></span> <span data-ttu-id="bf885-183">おが、アルバムの追加情報が追加されました。 これの一部を含めることで思いますが、このビューを最後に更新しましたので: ジャンル、アーティスト、価格、およびアルバム アート。</span><span class="sxs-lookup"><span data-stu-id="bf885-183">While we're at it, we can include some of the Album additional information which we've added since we last updated this view: Genre, Artist, Price, and Album Art.</span></span> <span data-ttu-id="bf885-184">更新されたコードのストアの詳細表示では、次のように表示されます。</span><span class="sxs-lookup"><span data-stu-id="bf885-184">The updated Store Details view code appears as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample10.cshtml)]

<span data-ttu-id="bf885-185">今すぐストアをクリックし、テストを追加して、このショッピング カートからアルバムを削除するおできます。</span><span class="sxs-lookup"><span data-stu-id="bf885-185">Now we can click through the store and test adding and removing Albums to and from our shopping cart.</span></span> <span data-ttu-id="bf885-186">アプリケーションを実行し、ストア インデックスを参照します。</span><span class="sxs-lookup"><span data-stu-id="bf885-186">Run the application and browse to the Store Index.</span></span>

![](mvc-music-store-part-8/_static/image4.png)

<span data-ttu-id="bf885-187">次に、アルバムの一覧を表示するジャンルをクリックします。</span><span class="sxs-lookup"><span data-stu-id="bf885-187">Next, click on a Genre to view a list of albums.</span></span>

![](mvc-music-store-part-8/_static/image5.png)

<span data-ttu-id="bf885-188">これで、アルバム タイトルをクリックすると、アルバムの詳細表示を更新"カートに追加 ボタンを含むを示します。</span><span class="sxs-lookup"><span data-stu-id="bf885-188">Clicking on an Album title now shows our updated Album Details view, including the "Add to cart" button.</span></span>

![](mvc-music-store-part-8/_static/image6.png)

<span data-ttu-id="bf885-189">ショッピング カート概要リスト、ショッピング カート インデックス ビューを示して"カートに追加 ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="bf885-189">Clicking the "Add to cart" button shows our Shopping Cart Index view with the shopping cart summary list.</span></span>

![](mvc-music-store-part-8/_static/image7.png)

<span data-ttu-id="bf885-190">ショッピング カートを読み込むには、ショッピング カートに Ajax の更新を表示する、カートのリンクから削除 をクリックすることができます。</span><span class="sxs-lookup"><span data-stu-id="bf885-190">After loading up your shopping cart, you can click on the Remove from cart link to see the Ajax update to your shopping cart.</span></span>

![](mvc-music-store-part-8/_static/image8.png)

<span data-ttu-id="bf885-191">ショッピング カートをカートにアイテムを追加する登録されていないユーザーが作業を開発しています。</span><span class="sxs-lookup"><span data-stu-id="bf885-191">We've built out a working shopping cart which allows unregistered users to add items to their cart.</span></span> <span data-ttu-id="bf885-192">次のセクションに登録をチェック アウト プロセスを完了することができますをします。</span><span class="sxs-lookup"><span data-stu-id="bf885-192">In the following section, we'll allow them to register and complete the checkout process.</span></span>


>[!div class="step-by-step"]
<span data-ttu-id="bf885-193">[前へ](mvc-music-store-part-7.md)
[次へ](mvc-music-store-part-9.md)</span><span class="sxs-lookup"><span data-stu-id="bf885-193">[Previous](mvc-music-store-part-7.md)
[Next](mvc-music-store-part-9.md)</span></span>
