---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
title: 'パート 10: ナビゲーションとサイト デザイン、結論の最終的な更新プログラム |Microsoft Docs'
author: jongalloway
description: このチュートリアル シリーズでは、すべての ASP.NET MVC のミュージック ストア サンプル アプリケーションをビルドする手順について説明します。 パート 10 では、ナビゲーションと S. の最終的な更新プログラムについて説明します.
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: 0c6e4c2f-fcdb-4978-9656-1990c6f15727
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
msc.type: authoredcontent
ms.openlocfilehash: 40ed7c337e097675199ab66229095bd3315d8c8d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836811"
---
<a name="part-10-final-updates-to-navigation-and-site-design-conclusion"></a><span data-ttu-id="fd485-104">パート 10: ナビゲーションとサイト デザイン、結論の最終的な更新プログラム</span><span class="sxs-lookup"><span data-stu-id="fd485-104">Part 10: Final Updates to Navigation and Site Design, Conclusion</span></span>
====================
<span data-ttu-id="fd485-105">によって[Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="fd485-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="fd485-106">MVC のミュージック ストアは、チュートリアル アプリケーションを紹介し、web 開発用の ASP.NET MVC と Visual Studio を使用する方法をステップ バイ ステップについて説明します。</span><span class="sxs-lookup"><span data-stu-id="fd485-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="fd485-107">MVC のミュージック ストアは、オンラインで音楽のアルバムを販売し、基本的なサイトの管理、ユーザー サインインし、買い物カゴの機能を実装する軽量サンプル ストア実装です。</span><span class="sxs-lookup"><span data-stu-id="fd485-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="fd485-108">このチュートリアル シリーズでは、すべての ASP.NET MVC のミュージック ストア サンプル アプリケーションをビルドする手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="fd485-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="fd485-109">パート 10 では、ナビゲーションとサイト デザイン、結論を最終的な更新プログラムについて説明します。</span><span class="sxs-lookup"><span data-stu-id="fd485-109">Part 10 covers Final Updates to Navigation and Site Design, Conclusion.</span></span>


<span data-ttu-id="fd485-110">私たちのサイトのすべての主要な機能が終了しましたが、サイト ナビゲーション、ホーム ページで、およびストアの参照ページに追加するいくつかの機能、まだいます。</span><span class="sxs-lookup"><span data-stu-id="fd485-110">We've completed all the major functionality for our site, but we still have some features to add to the site navigation, the home page, and the Store Browse page.</span></span>

## <a name="creating-the-shopping-cart-summary-partial-view"></a><span data-ttu-id="fd485-111">ショッピング カート概要部分ビューの作成</span><span class="sxs-lookup"><span data-stu-id="fd485-111">Creating the Shopping Cart Summary Partial View</span></span>

<span data-ttu-id="fd485-112">サイト全体にわたってユーザーのショッピング カート内の項目の数を公開します。</span><span class="sxs-lookup"><span data-stu-id="fd485-112">We want to expose the number of items in the user's shopping cart across the entire site.</span></span>

![](mvc-music-store-part-10/_static/image1.png)

<span data-ttu-id="fd485-113">Site.master に追加される部分ビューを作成して簡単に実装できます。</span><span class="sxs-lookup"><span data-stu-id="fd485-113">We can easily implement this by creating a partial view which is added to our Site.master.</span></span>

<span data-ttu-id="fd485-114">前述のように、ShoppingCart コント ローラーには、部分ビューを返す CartSummary アクション メソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="fd485-114">As shown previously, the ShoppingCart controller includes a CartSummary action method which returns a partial view:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample1.cs)]

<span data-ttu-id="fd485-115">CartSummary 部分ビューを作成するには、ビュー/ShoppingCart フォルダーを右クリックし、ビューの追加を選択します。</span><span class="sxs-lookup"><span data-stu-id="fd485-115">To create the CartSummary partial view, right-click on the Views/ShoppingCart folder and select Add View.</span></span> <span data-ttu-id="fd485-116">CartSummary ビューの名前を指定し、次に示すように"部分ビューを作成する チェック ボックスをオンします。</span><span class="sxs-lookup"><span data-stu-id="fd485-116">Name the view CartSummary and check the "Create a partial view" checkbox as shown below.</span></span>

![](mvc-music-store-part-10/_static/image2.png)

<span data-ttu-id="fd485-117">CartSummary 部分ビューは非常に単純 - カートにアイテムの数を示す ShoppingCart インデックス ビューへのリンクだけです。</span><span class="sxs-lookup"><span data-stu-id="fd485-117">The CartSummary partial view is really simple - it's just a link to the ShoppingCart Index view which shows the number of items in the cart.</span></span> <span data-ttu-id="fd485-118">CartSummary.cshtml の完全なコードは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="fd485-118">The complete code for CartSummary.cshtml is as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample2.cshtml)]

<span data-ttu-id="fd485-119">Html.RenderAction メソッドを使用して、サイトのマスターを含め、サイトの任意のページで部分ビューを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="fd485-119">We can include a partial view in any page in the site, including the Site master, by using the Html.RenderAction method.</span></span> <span data-ttu-id="fd485-120">RenderAction では、アクションの名前 ("CartSummary") と以下としてコント ローラー名 ("ShoppingCart") を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="fd485-120">RenderAction requires us to specify the Action Name ("CartSummary") and the Controller Name ("ShoppingCart") as below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample3.cshtml)]

<span data-ttu-id="fd485-121">これをサイトのレイアウトに追加する前にも作成、[ジャンル] メニューの Site.master 更新をすべて同時に確認できるようにします。</span><span class="sxs-lookup"><span data-stu-id="fd485-121">Before adding this to the site Layout, we will also create the Genre Menu so we can make all of our Site.master updates at one time.</span></span>

## <a name="creating-the-genre-menu-partial-view"></a><span data-ttu-id="fd485-122">ジャンル メニュー部分ビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="fd485-122">Creating the Genre Menu Partial View</span></span>

<span data-ttu-id="fd485-123">できることができます、ストアで使用可能なすべてのジャンルの一覧を表示するジャンル メニューを追加することで、ストア内を移動するユーザーをずっと簡単。</span><span class="sxs-lookup"><span data-stu-id="fd485-123">We can make it a lot easier for our users to navigate through the store by adding a Genre Menu which lists all the Genres available in our store.</span></span>

![](mvc-music-store-part-10/_static/image3.png)

<span data-ttu-id="fd485-124">同じに従って手順も、GenreMenu 部分ビューを作成し、両方をサイトのマスターに追加します。</span><span class="sxs-lookup"><span data-stu-id="fd485-124">We will follow the same steps also create a GenreMenu partial view, and then we can add them both to the Site master.</span></span> <span data-ttu-id="fd485-125">最初に、次の GenreMenu コント ローラー アクションを StoreController に追加します。</span><span class="sxs-lookup"><span data-stu-id="fd485-125">First, add the following GenreMenu controller action to the StoreController:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample4.cs)]

<span data-ttu-id="fd485-126">このアクションは、次で作成されますが、部分ビューによって表示されるジャンルの一覧を返します。</span><span class="sxs-lookup"><span data-stu-id="fd485-126">This action returns a list of Genres which will be displayed by the partial view, which we will create next.</span></span>

<span data-ttu-id="fd485-127">*注: [ChildActionOnly] 属性から、部分ビューを使用するには、この操作のみが必要であることを示します。 このコント ローラー アクションに追加されました。この属性により、コント ローラー アクションから/Store/GenreMenu を参照して実行されています。部分ビューは、必須ではありませんが、意図したように、コント ローラー アクションが使用されるかどうかを確認するために、ことをお勧めです。ビュー エンジンがその他のビュー内に含まれる際に、このビューのレイアウトを使用しないでください、知っておくことができるビューではなく PartialView を返すもいます。*</span><span class="sxs-lookup"><span data-stu-id="fd485-127">*Note: We have added the [ChildActionOnly] attribute to this controller action, which indicates that we only want this action to be used from a Partial View. This attribute will prevent the controller action from being executed by browsing to /Store/GenreMenu. This isn't required for partial views, but it is a good practice, since we want to make sure our controller actions are used as we intend. We are also returning PartialView rather than View, which lets the view engine know that it shouldn't use the Layout for this view, as it is being included in other views.*</span></span>

<span data-ttu-id="fd485-128">GenreMenu コント ローラーのアクションを右クリックし、次に示すように、ジャンルのビュー データ クラスを使用して厳密に型指定された GenreMenu をという名前の部分的なビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="fd485-128">Right-click on the GenreMenu controller action and create a partial view named GenreMenu which is strongly typed using the Genre view data class as shown below.</span></span>

![](mvc-music-store-part-10/_static/image4.png)

<span data-ttu-id="fd485-129">次のように、順序なしリストを使用して項目を表示する GenreMenu 部分ビューのコードの表示を更新します。</span><span class="sxs-lookup"><span data-stu-id="fd485-129">Update the view code for the GenreMenu partial view to display the items using an unordered list as follows.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample5.cshtml)]

## <a name="updating-site-layout-to-display-our-partial-views"></a><span data-ttu-id="fd485-130">部分ビューを表示するサイトのレイアウトを更新しています</span><span class="sxs-lookup"><span data-stu-id="fd485-130">Updating Site Layout to display our Partial Views</span></span>

<span data-ttu-id="fd485-131">サイトのレイアウトに、部分ビューを追加できます (共有/ビュー/\_Layout.cshtml) Html.RenderAction() を呼び出すことによって。</span><span class="sxs-lookup"><span data-stu-id="fd485-131">We can add our partial views to the Site Layout (/Views/Shared/\_Layout.cshtml) by calling Html.RenderAction().</span></span> <span data-ttu-id="fd485-132">次に示すように両方、だけでなく、それらを表示するマークアップを追加しますします。</span><span class="sxs-lookup"><span data-stu-id="fd485-132">We'll add them both in, as well as some additional markup to display them, as shown below:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample6.cshtml)]

<span data-ttu-id="fd485-133">これでアプリケーションを実行すると、左側のナビゲーション領域のジャンルと上部にあるカートの概要が表示されます。</span><span class="sxs-lookup"><span data-stu-id="fd485-133">Now when we run the application, we will see the Genre in the left navigation area and the Cart Summary at the top.</span></span>

## <a name="update-to-the-store-browse-page"></a><span data-ttu-id="fd485-134">ストアの参照ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="fd485-134">Update to the Store Browse page</span></span>

<span data-ttu-id="fd485-135">ストアの参照 ページは機能しますが、非常に良いが正しく表示されません。</span><span class="sxs-lookup"><span data-stu-id="fd485-135">The Store Browse page is functional, but doesn't look very good.</span></span> <span data-ttu-id="fd485-136">レイアウトをわかりやすく、コードの表示 (/Views/Store/Browse.cshtml で確認) 次のように更新することで、アルバムを表示するページを更新できます。</span><span class="sxs-lookup"><span data-stu-id="fd485-136">We can update the page to show the albums in a better layout by updating the view code (found in /Views/Store/Browse.cshtml) as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample7.cshtml)]

<span data-ttu-id="fd485-137">ここで行っています Html.ActionLink ではなく Url.Action を使用してアルバム アートを含むへのリンクを特殊な書式設定を適用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="fd485-137">Here we are making use of Url.Action rather than Html.ActionLink so that we can apply special formatting to the link to include the album artwork.</span></span>

<span data-ttu-id="fd485-138">*注: これらのアルバムの汎用のアルバム カバーが表示されます。この情報は、データベースに格納されはストア マネージャーを使用して編集できます。独自のアートワークを追加する、できます。*</span><span class="sxs-lookup"><span data-stu-id="fd485-138">*Note: We are displaying a generic album cover for these albums. This information is stored in the database and is editable via the Store Manager. You are welcome to add your own artwork.*</span></span>

<span data-ttu-id="fd485-139">今すぐを参照、ジャンル、ときにアルバム アートを持つグリッドに表示されるアルバムが表示されます。</span><span class="sxs-lookup"><span data-stu-id="fd485-139">Now when we browse to a Genre, we will see the albums shown in a grid with the album artwork.</span></span>

![](mvc-music-store-part-10/_static/image5.png)

## <a name="updating-the-home-page-to-show-top-selling-albums"></a><span data-ttu-id="fd485-140">上部の販売アルバムを表示するホーム ページを更新しています</span><span class="sxs-lookup"><span data-stu-id="fd485-140">Updating the Home Page to show Top Selling Albums</span></span>

<span data-ttu-id="fd485-141">販売の売上が増加するホーム ページ上のアルバム、一番上に機能します。</span><span class="sxs-lookup"><span data-stu-id="fd485-141">We want to feature our top selling albums on the home page to increase sales.</span></span> <span data-ttu-id="fd485-142">処理を追加するのにもいくつか追加のグラフィックス、HomeController に一部の更新プログラムをしましょう。</span><span class="sxs-lookup"><span data-stu-id="fd485-142">We'll make some updates to our HomeController to handle that, and add in some additional graphics as well.</span></span>

<span data-ttu-id="fd485-143">まず、EntityFramework が関連付けられていることを認識できるように、アルバム クラスにナビゲーション プロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="fd485-143">First, we'll add a navigation property to our Album class so that EntityFramework knows that they're associated.</span></span> <span data-ttu-id="fd485-144">最後の数行、**アルバム**クラスが次のようになります。</span><span class="sxs-lookup"><span data-stu-id="fd485-144">The last few lines of our **Album** class should now look like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample8.cs)]

<span data-ttu-id="fd485-145">*注: これは追加する必要を使用して、System.Collections.Generic 名前空間でのステートメント。*</span><span class="sxs-lookup"><span data-stu-id="fd485-145">*Note: This will require adding a using statement to bring in the System.Collections.Generic namespace.*</span></span>

<span data-ttu-id="fd485-146">まず、storeDB フィールドと、他のコント ローラーのように、ステートメントを使用して MvcMusicStore.Models を追加します。</span><span class="sxs-lookup"><span data-stu-id="fd485-146">First, we'll add a storeDB field and the MvcMusicStore.Models using statements, as in our other controllers.</span></span> <span data-ttu-id="fd485-147">次に、HomeController OrderDetails に従って販売上のアルバムを検索するデータベースを照会する次のメソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="fd485-147">Next, we'll add the following method to the HomeController which queries our database to find top selling albums according to OrderDetails.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample9.cs)]

<span data-ttu-id="fd485-148">これは、コント ローラーのアクションとして使用できるようにする望ましくないため、プライベート メソッドです。</span><span class="sxs-lookup"><span data-stu-id="fd485-148">This is a private method, since we don't want to make it available as a controller action.</span></span> <span data-ttu-id="fd485-149">わかりやすくするために、HomeController に含めることが、必要に応じて別のサービス クラスに、ビジネス ロジックを移動することが推奨されます。</span><span class="sxs-lookup"><span data-stu-id="fd485-149">We are including it in the HomeController for simplicity, but you are encouraged to move your business logic into separate service classes as appropriate.</span></span>

<span data-ttu-id="fd485-150">配置して、アルバムを販売して上位 5 つのクエリをビューに戻すインデックス コント ローラー アクション更新できます。</span><span class="sxs-lookup"><span data-stu-id="fd485-150">With that in place, we can update the Index controller action to query the top 5 selling albums and return them to the view.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample10.cs)]

<span data-ttu-id="fd485-151">更新された HomeController の完全なコードは、次に示すようです。</span><span class="sxs-lookup"><span data-stu-id="fd485-151">The complete code for the updated HomeController is as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample11.cs)]

<span data-ttu-id="fd485-152">最後に、モデルの種類を更新して、アルバム一覧を下に追加のアルバムの一覧を表示できるように、ホーム インデックス ビューを更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="fd485-152">Finally, we'll need to update our Home Index view so that it can display a list of albums by updating the Model type and adding the album list to the bottom.</span></span> <span data-ttu-id="fd485-153">私たちもページに見出しセクションがあり、昇格を追加するには、この機会になります。</span><span class="sxs-lookup"><span data-stu-id="fd485-153">We will take this opportunity to also add a heading and a promotion section to the page.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample12.cshtml)]

<span data-ttu-id="fd485-154">今すぐアプリケーションを実行すると販売の最上位のアルバムと、キャンペーンのメッセージを使用、更新のホーム ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="fd485-154">Now when we run the application, we'll see our updated home page with top selling albums and our promotional message.</span></span>

![](mvc-music-store-part-10/_static/image1.jpg)

## <a name="conclusion"></a><span data-ttu-id="fd485-155">まとめ</span><span class="sxs-lookup"><span data-stu-id="fd485-155">Conclusion</span></span>

<span data-ttu-id="fd485-156">ASP.NET MVC 簡単見たなどのデータベース アクセス、メンバーシップ、AJAX、高度な web サイトを作成します。</span><span class="sxs-lookup"><span data-stu-id="fd485-156">We've seen that ASP.NET MVC makes it easy to create a sophisticated website with database access, membership, AJAX, etc.</span></span> <span data-ttu-id="fd485-157">非常に短時間です。</span><span class="sxs-lookup"><span data-stu-id="fd485-157">pretty quickly.</span></span> <span data-ttu-id="fd485-158">このチュートリアルが願って、独自の ASP.NET MVC アプリケーションの構築を開始する必要があるツールです。</span><span class="sxs-lookup"><span data-stu-id="fd485-158">Hopefully this tutorial has given you the tools you need to get started building your own ASP.NET MVC applications!</span></span>


> [!div class="step-by-step"]
> [<span data-ttu-id="fd485-159">前へ</span><span class="sxs-lookup"><span data-stu-id="fd485-159">Previous</span></span>](mvc-music-store-part-9.md)
