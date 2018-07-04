---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
title: 'パート 8: ショッピング カートと Ajax 更新 |Microsoft Docs'
author: jongalloway
description: このチュートリアル シリーズでは、すべての ASP.NET MVC のミュージック ストア サンプル アプリケーションをビルドする手順について説明します。 パート 8 では、ショッピング カートと Ajax 更新について説明します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 26b2f55e-ed42-4277-89b0-c941eb754145
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
msc.type: authoredcontent
ms.openlocfilehash: 327b7ee4e302188323c229c231ae750cbed709a1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369797"
---
<a name="part-8-shopping-cart-with-ajax-updates"></a><span data-ttu-id="9f49e-104">パート 8: ショッピング カートと Ajax 更新</span><span class="sxs-lookup"><span data-stu-id="9f49e-104">Part 8: Shopping Cart with Ajax Updates</span></span>
====================
<span data-ttu-id="9f49e-105">によって[Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="9f49e-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="9f49e-106">MVC のミュージック ストアは、チュートリアル アプリケーションを紹介し、web 開発用の ASP.NET MVC と Visual Studio を使用する方法をステップ バイ ステップについて説明します。</span><span class="sxs-lookup"><span data-stu-id="9f49e-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="9f49e-107">MVC のミュージック ストアは、オンラインで音楽のアルバムを販売し、基本的なサイトの管理、ユーザー サインインし、買い物カゴの機能を実装する軽量サンプル ストア実装です。</span><span class="sxs-lookup"><span data-stu-id="9f49e-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="9f49e-108">このチュートリアル シリーズでは、すべての ASP.NET MVC のミュージック ストア サンプル アプリケーションをビルドする手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="9f49e-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="9f49e-109">パート 8 では、ショッピング カートと Ajax 更新について説明します。</span><span class="sxs-lookup"><span data-stu-id="9f49e-109">Part 8 covers Shopping Cart with Ajax Updates.</span></span>


<span data-ttu-id="9f49e-110">ユーザー登録を行わない、カートにアルバムを配置する を許可しますが、完全なチェック アウトをゲストとして登録する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9f49e-110">We'll allow users to place albums in their cart without registering, but they'll need to register as guests to complete checkout.</span></span> <span data-ttu-id="9f49e-111">買い物とチェック アウト プロセスが 2 つのコント ローラーに区切られる: 匿名で、カートにアイテムを追加できるように、ShoppingCart コント ローラーとチェック アウト プロセスを処理するチェック アウト コント ローラー。</span><span class="sxs-lookup"><span data-stu-id="9f49e-111">The shopping and checkout process will be separated into two controllers: a ShoppingCart Controller which allows anonymously adding items to a cart, and a Checkout Controller which handles the checkout process.</span></span> <span data-ttu-id="9f49e-112">このセクションでショッピング カートの開始をし、次のセクションで、清算処理をビルドします。</span><span class="sxs-lookup"><span data-stu-id="9f49e-112">We'll start with the Shopping Cart in this section, then build the Checkout process in the following section.</span></span>

## <a name="adding-the-cart-order-and-orderdetail-model-classes"></a><span data-ttu-id="9f49e-113">カート、順序、および OrderDetail モデル クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="9f49e-113">Adding the Cart, Order, and OrderDetail model classes</span></span>

<span data-ttu-id="9f49e-114">ショッピング カートとチェック アウト プロセスにより、いくつかの新しいクラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="9f49e-114">Our Shopping Cart and Checkout processes will make use of some new classes.</span></span> <span data-ttu-id="9f49e-115">Models フォルダーを右クリックし、カート クラス (Cart.cs) を次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="9f49e-115">Right-click the Models folder and add a Cart class (Cart.cs) with the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample1.cs)]

<span data-ttu-id="9f49e-116">このクラスは、レコード Id プロパティの [キー] 属性を除く、これまでに使用した他のユーザーに非常に似ています。</span><span class="sxs-lookup"><span data-stu-id="9f49e-116">This class is pretty similar to others we've used so far, with the exception of the [Key] attribute for the RecordId property.</span></span> <span data-ttu-id="9f49e-117">カートのアイテムは、匿名ショッピングを許可する CartID という名前の文字列識別子がテーブルにレコード Id を名前付き整数型のプライマリ キーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="9f49e-117">Our Cart items will have a string identifier named CartID to allow anonymous shopping, but the table includes an integer primary key named RecordId.</span></span> <span data-ttu-id="9f49e-118">慣例により、Entity Framework Code First には、ことカートをという名前のテーブルの主キーは CartId または ID のいずれかになりますが簡単にオーバーライドできますを注釈またはコードを使用して必要な場合が想定しています。</span><span class="sxs-lookup"><span data-stu-id="9f49e-118">By convention, Entity Framework Code-First expects that the primary key for a table named Cart will be either CartId or ID, but we can easily override that via annotations or code if we want.</span></span> <span data-ttu-id="9f49e-119">使用する方法、シンプルな規約で Entity Framework Code First に合ったときの例は、これが私たちを受けなくが不要になったとき。</span><span class="sxs-lookup"><span data-stu-id="9f49e-119">This is an example of how we can use the simple conventions in Entity Framework Code-First when they suit us, but we're not constrained by them when they don't.</span></span>

<span data-ttu-id="9f49e-120">次に、次のコードで Order クラス (Order.cs) を追加します。</span><span class="sxs-lookup"><span data-stu-id="9f49e-120">Next, add an Order class (Order.cs) with the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample2.cs)]

<span data-ttu-id="9f49e-121">このクラスは、注文の概要と配信の情報を追跡します。</span><span class="sxs-lookup"><span data-stu-id="9f49e-121">This class tracks summary and delivery information for an order.</span></span> <span data-ttu-id="9f49e-122">**まだコンパイルされません**まだ作成していないクラスに依存する OrderDetails ナビゲーション プロパティを持つため、します。</span><span class="sxs-lookup"><span data-stu-id="9f49e-122">**It won't compile yet**, because it has an OrderDetails navigation property which depends on a class we haven't created yet.</span></span> <span data-ttu-id="9f49e-123">今すぐ追加することでクラスという名前の OrderDetail.cs、次のコードを追加することを修正しましょう。</span><span class="sxs-lookup"><span data-stu-id="9f49e-123">Let's fix that now by adding a class named OrderDetail.cs, adding the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample3.cs)]

<span data-ttu-id="9f49e-124">これら新しいモデルなどのクラスも、DbSet を公開する Dbset を含める MusicStoreEntities クラスに 1 つの最後の更新を行います&lt;アーティスト&gt;します。</span><span class="sxs-lookup"><span data-stu-id="9f49e-124">We'll make one last update to our MusicStoreEntities class to include DbSets which expose those new Model classes, also including a DbSet&lt;Artist&gt;.</span></span> <span data-ttu-id="9f49e-125">更新された MusicStoreEntities クラスとして表示されます以下です。</span><span class="sxs-lookup"><span data-stu-id="9f49e-125">The updated MusicStoreEntities class appears as below.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample4.cs)]

## <a name="managing-the-shopping-cart-business-logic"></a><span data-ttu-id="9f49e-126">ショッピング カートのビジネス ロジックを管理します。</span><span class="sxs-lookup"><span data-stu-id="9f49e-126">Managing the Shopping Cart business logic</span></span>

<span data-ttu-id="9f49e-127">次に、Models フォルダーに ShoppingCart クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="9f49e-127">Next, we'll create the ShoppingCart class in the Models folder.</span></span> <span data-ttu-id="9f49e-128">ShoppingCart モデルでは、カート テーブルにデータ アクセスを処理します。</span><span class="sxs-lookup"><span data-stu-id="9f49e-128">The ShoppingCart model handles data access to the Cart table.</span></span> <span data-ttu-id="9f49e-129">さらに、追加すると、ショッピング カートから項目を削除するビジネス ロジックを処理します。</span><span class="sxs-lookup"><span data-stu-id="9f49e-129">Additionally, it will handle the business logic to for adding and removing items from the shopping cart.</span></span>

<span data-ttu-id="9f49e-130">一時的な一意の識別子 (GUID、またはグローバルに一意の識別子を使用して) ユーザーを割り当てることが買い物かごにアイテムを追加するためだけのアカウントにサインアップするユーザーを必要とするのでない、ショッピング カートにアクセスするとき。</span><span class="sxs-lookup"><span data-stu-id="9f49e-130">Since we don't want to require users to sign up for an account just to add items to their shopping cart, we will assign users a temporary unique identifier (using a GUID, or globally unique identifier) when they access the shopping cart.</span></span> <span data-ttu-id="9f49e-131">ASP.NET セッション クラスを使用して、この ID を格納します。</span><span class="sxs-lookup"><span data-stu-id="9f49e-131">We'll store this ID using the ASP.NET Session class.</span></span>

<span data-ttu-id="9f49e-132">*注: ASP.NET セッションは、サイトを離れた後の有効期限をユーザーに固有の情報を格納する便利な場所です。セッション状態の不正使用では、大きいサイトのパフォーマンスに影響を持つことができます、明るい使用はデモンストレーションのためにも機能します。*</span><span class="sxs-lookup"><span data-stu-id="9f49e-132">*Note: The ASP.NET Session is a convenient place to store user-specific information which will expire after they leave the site. While misuse of session state can have performance implications on larger sites, our light use will work well for demonstration purposes.*</span></span>

<span data-ttu-id="9f49e-133">ShoppingCart クラスは、次のメソッドを公開します。</span><span class="sxs-lookup"><span data-stu-id="9f49e-133">The ShoppingCart class exposes the following methods:</span></span>

<span data-ttu-id="9f49e-134">**AddToCart**アルバムをパラメーターとして受け取り、ユーザーのカートに追加します。</span><span class="sxs-lookup"><span data-stu-id="9f49e-134">**AddToCart** takes an Album as a parameter and adds it to the user's cart.</span></span> <span data-ttu-id="9f49e-135">カートの表では、各アルバムの数量を追跡しているために、必要な場合は、新しい行を作成またはだけ場合は、ユーザーが既に 1 つのコピーのアルバムを注文数量をインクリメントするためのロジックが含まれます。</span><span class="sxs-lookup"><span data-stu-id="9f49e-135">Since the Cart table tracks quantity for each album, it includes logic to create a new row if needed or just increment the quantity if the user has already ordered one copy of the album.</span></span>

<span data-ttu-id="9f49e-136">**RemoveFromCart**アルバムの ID を受け取り、ユーザーのカートから削除されます。</span><span class="sxs-lookup"><span data-stu-id="9f49e-136">**RemoveFromCart** takes an Album ID and removes it from the user's cart.</span></span> <span data-ttu-id="9f49e-137">ユーザーは、カゴにアルバムのコピーを 1 つのみがある、行が削除されます。</span><span class="sxs-lookup"><span data-stu-id="9f49e-137">If the user only had one copy of the album in their cart, the row is removed.</span></span>

<span data-ttu-id="9f49e-138">**EmptyCart**ユーザーのショッピング カートからすべての項目を削除します。</span><span class="sxs-lookup"><span data-stu-id="9f49e-138">**EmptyCart** removes all items from a user's shopping cart.</span></span>

<span data-ttu-id="9f49e-139">**GetCartItems** CartItems の表示または処理するための一覧を取得します。</span><span class="sxs-lookup"><span data-stu-id="9f49e-139">**GetCartItems** retrieves a list of CartItems for display or processing.</span></span>

<span data-ttu-id="9f49e-140">**GetCount**を取得するユーザーがショッピング カートに入っているアルバムの合計数。</span><span class="sxs-lookup"><span data-stu-id="9f49e-140">**GetCount** retrieves a the total number of albums a user has in their shopping cart.</span></span>

<span data-ttu-id="9f49e-141">**GetTotal**カート内のすべての項目の合計コストを計算します。</span><span class="sxs-lookup"><span data-stu-id="9f49e-141">**GetTotal** calculates the total cost of all items in the cart.</span></span>

<span data-ttu-id="9f49e-142">**CreateOrder**ショッピング カートをチェック アウト フェーズ中に、注文に変換します。</span><span class="sxs-lookup"><span data-stu-id="9f49e-142">**CreateOrder** converts the shopping cart to an order during the checkout phase.</span></span>

<span data-ttu-id="9f49e-143">**GetCart**カート オブジェクトを取得するコント ローラーを使用する静的メソッドです。</span><span class="sxs-lookup"><span data-stu-id="9f49e-143">**GetCart** is a static method which allows our controllers to obtain a cart object.</span></span> <span data-ttu-id="9f49e-144">使用して、 **GetCartId**ユーザーのセッションから、CartId の読み取りを処理するメソッド。</span><span class="sxs-lookup"><span data-stu-id="9f49e-144">It uses the **GetCartId** method to handle reading the CartId from the user's session.</span></span> <span data-ttu-id="9f49e-145">GetCartId メソッドでは、ユーザーの CartId ユーザーのセッションから読み取ることができるように、HttpContextBase が必要です。</span><span class="sxs-lookup"><span data-stu-id="9f49e-145">The GetCartId method requires the HttpContextBase so that it can read the user's CartId from user's session.</span></span>

<span data-ttu-id="9f49e-146">ここでは、完全な**ShoppingCart クラス**:</span><span class="sxs-lookup"><span data-stu-id="9f49e-146">Here's the complete **ShoppingCart class**:</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample5.cs)]

## <a name="viewmodels"></a><span data-ttu-id="9f49e-147">ViewModels</span><span class="sxs-lookup"><span data-stu-id="9f49e-147">ViewModels</span></span>

<span data-ttu-id="9f49e-148">ショッピング カート コント ローラーは、複雑な情報をビュー モデル オブジェクトに正しくマップされないと通信する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9f49e-148">Our Shopping Cart Controller will need to communicate some complex information to its views which doesn't map cleanly to our Model objects.</span></span> <span data-ttu-id="9f49e-149">ありません。 ビューに合わせて、モデルを変更します。モデル クラスには、ユーザー インターフェイスではなく、ドメインを表す必要があります。</span><span class="sxs-lookup"><span data-stu-id="9f49e-149">We don't want to modify our Models to suit our views; Model classes should represent our domain, not the user interface.</span></span> <span data-ttu-id="9f49e-150">ストア マネージャー、ボックスの一覧についてで行ったように、管理が困難で取得 ViewBag を使用して、多くの情報を渡しますが、ViewBag クラスを使用して、ビューに、情報を渡す 1 つのソリューションになります。</span><span class="sxs-lookup"><span data-stu-id="9f49e-150">One solution would be to pass the information to our Views using the ViewBag class, as we did with the Store Manager dropdown information, but passing a lot of information via ViewBag gets hard to manage.</span></span>

<span data-ttu-id="9f49e-151">これに対する解決策は、使用する、 *ViewModel*パターン。</span><span class="sxs-lookup"><span data-stu-id="9f49e-151">A solution to this is to use the *ViewModel* pattern.</span></span> <span data-ttu-id="9f49e-152">このパターンを使用する場合、特定のビューのシナリオ用に最適化されたと、ビュー テンプレートで必要な動的値/コンテンツ プロパティを公開する厳密に型指定されたクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="9f49e-152">When using this pattern we create strongly-typed classes that are optimized for our specific view scenarios, and which expose properties for the dynamic values/content needed by our view templates.</span></span> <span data-ttu-id="9f49e-153">コント ローラー クラスを設定し、使用するには、このビュー テンプレートにこれらのビューに最適化されたクラスを渡します。</span><span class="sxs-lookup"><span data-stu-id="9f49e-153">Our controller classes can then populate and pass these view-optimized classes to our view template to use.</span></span> <span data-ttu-id="9f49e-154">これにより、タイプ セーフ、コンパイル時のチェック、およびエディター IntelliSense 内でのテンプレートを表示できます。</span><span class="sxs-lookup"><span data-stu-id="9f49e-154">This enables type-safety, compile-time checking, and editor IntelliSense within view templates.</span></span>

<span data-ttu-id="9f49e-155">ショッピング カート コント ローラーで使用するための 2 つのビュー モデルを作成します。 ShoppingCartViewModel はユーザーのショッピング カートの内容を保持し、ユーザーが何かを削除すると確認情報の表示に使用される、ShoppingCartRemoveViewModelカート。</span><span class="sxs-lookup"><span data-stu-id="9f49e-155">We'll create two View Models for use in our Shopping Cart controller: the ShoppingCartViewModel will hold the contents of the user's shopping cart, and the ShoppingCartRemoveViewModel will be used to display confirmation information when a user removes something from their cart.</span></span>

<span data-ttu-id="9f49e-156">わかりやすくするために、プロジェクトのルートでは、新しいビューモデル フォルダーを作成しましょう。</span><span class="sxs-lookup"><span data-stu-id="9f49e-156">Let's create a new ViewModels folder in the root of our project to keep things organized.</span></span> <span data-ttu-id="9f49e-157">プロジェクトを右クリックし、追加の選択/新規フォルダー。</span><span class="sxs-lookup"><span data-stu-id="9f49e-157">Right-click the project, select Add / New Folder.</span></span>

![](mvc-music-store-part-8/_static/image1.jpg)

<span data-ttu-id="9f49e-158">ビューモデル フォルダーの名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="9f49e-158">Name the folder ViewModels.</span></span>

![](mvc-music-store-part-8/_static/image1.png)

<span data-ttu-id="9f49e-159">次に、ビューモデル フォルダーで ShoppingCartViewModel クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="9f49e-159">Next, add the ShoppingCartViewModel class in the ViewModels folder.</span></span> <span data-ttu-id="9f49e-160">2 つのプロパティがあります: カートのアイテムと、カート内のすべての項目の合計価格を保持するために 10 進値の一覧。</span><span class="sxs-lookup"><span data-stu-id="9f49e-160">It has two properties: a list of Cart items, and a decimal value to hold the total price for all items in the cart.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample6.cs)]

<span data-ttu-id="9f49e-161">これで、ShoppingCartRemoveViewModel をビューモデル フォルダーに次の 4 つのプロパティに追加します。</span><span class="sxs-lookup"><span data-stu-id="9f49e-161">Now add the ShoppingCartRemoveViewModel to the ViewModels folder, with the following four properties.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample7.cs)]

## <a name="the-shopping-cart-controller"></a><span data-ttu-id="9f49e-162">買い物カ ゴ コント ローラー</span><span class="sxs-lookup"><span data-stu-id="9f49e-162">The Shopping Cart Controller</span></span>

<span data-ttu-id="9f49e-163">ショッピング カートのコント ローラーは、次の 3 つの主な目的を持つ: カートから項目を削除する、カートにアイテムを表示する、カートにアイテムを追加します。</span><span class="sxs-lookup"><span data-stu-id="9f49e-163">The Shopping Cart controller has three main purposes: adding items to a cart, removing items from the cart, and viewing items in the cart.</span></span> <span data-ttu-id="9f49e-164">行う 3 つのクラスの使用先ほど作成した: ShoppingCartViewModel、ShoppingCartRemoveViewModel、および ShoppingCart します。</span><span class="sxs-lookup"><span data-stu-id="9f49e-164">It will make use of the three classes we just created: ShoppingCartViewModel, ShoppingCartRemoveViewModel, and ShoppingCart.</span></span> <span data-ttu-id="9f49e-165">ように、StoreController と StoreManagerController MusicStoreEntities のインスタンスを保持するフィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="9f49e-165">As in the StoreController and StoreManagerController, we'll add a field to hold an instance of MusicStoreEntities.</span></span>

<span data-ttu-id="9f49e-166">空のコント ローラー テンプレートを使用してプロジェクトに新しいショッピング カートのコント ローラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="9f49e-166">Add a new Shopping Cart controller to the project using the Empty controller template.</span></span>

![](mvc-music-store-part-8/_static/image2.png)

<span data-ttu-id="9f49e-167">完全な ShoppingCart コント ローラーを次に示します。</span><span class="sxs-lookup"><span data-stu-id="9f49e-167">Here's the complete ShoppingCart Controller.</span></span> <span data-ttu-id="9f49e-168">インデックスとコント ローラーの追加のアクションの使用率は見覚えがあります。</span><span class="sxs-lookup"><span data-stu-id="9f49e-168">The Index and Add Controller actions should look very familiar.</span></span> <span data-ttu-id="9f49e-169">削除と CartSummary コント ローラー アクションは、次のセクションで後ほど説明しますが、2 つの特殊なケースを処理します。</span><span class="sxs-lookup"><span data-stu-id="9f49e-169">The Remove and CartSummary controller actions handle two special cases, which we'll discuss in the following section.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample8.cs)]

## <a name="ajax-updates-with-jquery"></a><span data-ttu-id="9f49e-170">JQuery で Ajax の更新</span><span class="sxs-lookup"><span data-stu-id="9f49e-170">Ajax Updates with jQuery</span></span>

<span data-ttu-id="9f49e-171">次に、ShoppingCartViewModel を厳密に型指定し、同じ方法を使用して、リスト ビュー テンプレートを使用するショッピング カートのインデックス ページを作成します。</span><span class="sxs-lookup"><span data-stu-id="9f49e-171">We'll next create a Shopping Cart Index page that is strongly typed to the ShoppingCartViewModel and uses the List View template using the same method as before.</span></span>

![](mvc-music-store-part-8/_static/image3.png)

<span data-ttu-id="9f49e-172">ただし、カートから項目を削除するのに、Html.ActionLink を使用する代わりには、HTML クラス RemoveLink がこのビュー内のすべてのリンクのクリック イベントを「結び付ける」jQuery を使用します。</span><span class="sxs-lookup"><span data-stu-id="9f49e-172">However, instead of using an Html.ActionLink to remove items from the cart, we'll use jQuery to "wire up" the click event for all links in this view which have the HTML class RemoveLink.</span></span> <span data-ttu-id="9f49e-173">フォームの送信ではなくこの click イベント ハンドラーだけ、AJAX コールバックを RemoveFromCart コント ローラー アクションになります。</span><span class="sxs-lookup"><span data-stu-id="9f49e-173">Rather than posting the form, this click event handler will just make an AJAX callback to our RemoveFromCart controller action.</span></span> <span data-ttu-id="9f49e-174">JSON のシリアル化された結果を返す、RemoveFromCart、jQuery のコールバックは、次を解析し、jQuery を使用してページに 4 つの簡単な更新プログラムを実行します。</span><span class="sxs-lookup"><span data-stu-id="9f49e-174">The RemoveFromCart returns a JSON serialized result, which our jQuery callback then parses and performs four quick updates to the page using jQuery:</span></span>

- 1. <span data-ttu-id="9f49e-175">一覧から削除したアルバムを削除します。</span><span class="sxs-lookup"><span data-stu-id="9f49e-175">Removes the deleted album from the list</span></span>
- 2. <span data-ttu-id="9f49e-176">ヘッダーのカート数を更新します。</span><span class="sxs-lookup"><span data-stu-id="9f49e-176">Updates the cart count in the header</span></span>
- 3. <span data-ttu-id="9f49e-177">更新メッセージをユーザーに表示します。</span><span class="sxs-lookup"><span data-stu-id="9f49e-177">Displays an update message to the user</span></span>
- 4. <span data-ttu-id="9f49e-178">カートの総額を更新します。</span><span class="sxs-lookup"><span data-stu-id="9f49e-178">Updates the cart total price</span></span>

<span data-ttu-id="9f49e-179">削除のシナリオは、インデックス ビュー内で、Ajax コールバックが処理される、ため RemoveFromCart アクションの追加のビューは不要です。</span><span class="sxs-lookup"><span data-stu-id="9f49e-179">Since the remove scenario is being handled by an Ajax callback within the Index view, we don't need an additional view for RemoveFromCart action.</span></span> <span data-ttu-id="9f49e-180">/ShoppingCart/Index ビューの完全なコードを次に示します。</span><span class="sxs-lookup"><span data-stu-id="9f49e-180">Here is the complete code for the /ShoppingCart/Index view:</span></span>

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample9.cshtml)]

<span data-ttu-id="9f49e-181">このアプリケーションをテストするためには、このショッピング カートにアイテムを追加できる必要があります。</span><span class="sxs-lookup"><span data-stu-id="9f49e-181">In order to test this out, we need to be able to add items to our shopping cart.</span></span> <span data-ttu-id="9f49e-182">更新、**ストアの詳細**をビューに含め、"カートに追加 ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="9f49e-182">We'll update our **Store Details** view to include an "Add to cart" button.</span></span> <span data-ttu-id="9f49e-183">いくつかのアルバムの追加情報が追加されましたが含めることができますに思いますが、このビューを最後に更新しましたので: ジャンル、アーティスト、価格、およびアルバム アート。</span><span class="sxs-lookup"><span data-stu-id="9f49e-183">While we're at it, we can include some of the Album additional information which we've added since we last updated this view: Genre, Artist, Price, and Album Art.</span></span> <span data-ttu-id="9f49e-184">更新されたストアの詳細ビューのコードは、次に示すように表示されます。</span><span class="sxs-lookup"><span data-stu-id="9f49e-184">The updated Store Details view code appears as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample10.cshtml)]

<span data-ttu-id="9f49e-185">今すぐクリックして、ストアを追加して、アルバムを削除して、ショッピング カートからテストできます。</span><span class="sxs-lookup"><span data-stu-id="9f49e-185">Now we can click through the store and test adding and removing Albums to and from our shopping cart.</span></span> <span data-ttu-id="9f49e-186">アプリケーションを実行し、ストアのインデックスを参照します。</span><span class="sxs-lookup"><span data-stu-id="9f49e-186">Run the application and browse to the Store Index.</span></span>

![](mvc-music-store-part-8/_static/image4.png)

<span data-ttu-id="9f49e-187">次に、アルバムの一覧を表示するジャンルをクリックします。</span><span class="sxs-lookup"><span data-stu-id="9f49e-187">Next, click on a Genre to view a list of albums.</span></span>

![](mvc-music-store-part-8/_static/image5.png)

<span data-ttu-id="9f49e-188">アルバム タイトルに今すぐクリックしてには、"カートに追加 ボタンを含む、更新されたアルバムの詳細ビューが表示されます。</span><span class="sxs-lookup"><span data-stu-id="9f49e-188">Clicking on an Album title now shows our updated Album Details view, including the "Add to cart" button.</span></span>

![](mvc-music-store-part-8/_static/image6.png)

<span data-ttu-id="9f49e-189">カートに追加"ボタンをクリックすると、ショッピング カートの一覧で、ショッピング カートのインデックス ビューが表示されます。</span><span class="sxs-lookup"><span data-stu-id="9f49e-189">Clicking the "Add to cart" button shows our Shopping Cart Index view with the shopping cart summary list.</span></span>

![](mvc-music-store-part-8/_static/image7.png)

<span data-ttu-id="9f49e-190">ショッピング カートを読み込んだら、Ajax の更新、ショッピング カートを表示する、カートのリンクから削除 をクリックできます。</span><span class="sxs-lookup"><span data-stu-id="9f49e-190">After loading up your shopping cart, you can click on the Remove from cart link to see the Ajax update to your shopping cart.</span></span>

![](mvc-music-store-part-8/_static/image8.png)

<span data-ttu-id="9f49e-191">ショッピング カートのアイテムをカートに追加する登録されていないユーザーが作業を構築しました。</span><span class="sxs-lookup"><span data-stu-id="9f49e-191">We've built out a working shopping cart which allows unregistered users to add items to their cart.</span></span> <span data-ttu-id="9f49e-192">次のセクションで登録し、チェック アウト プロセスを完了することができます。</span><span class="sxs-lookup"><span data-stu-id="9f49e-192">In the following section, we'll allow them to register and complete the checkout process.</span></span>


> [!div class="step-by-step"]
> <span data-ttu-id="9f49e-193">[前へ](mvc-music-store-part-7.md)
> [次へ](mvc-music-store-part-9.md)</span><span class="sxs-lookup"><span data-stu-id="9f49e-193">[Previous](mvc-music-store-part-7.md)
[Next](mvc-music-store-part-9.md)</span></span>
