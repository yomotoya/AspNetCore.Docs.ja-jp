---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
title: '手順 10: 最終的な更新プログラムのナビゲーションおよびサイト設計、結論 |Microsoft ドキュメント'
author: jongalloway
description: このチュートリアルの系列では、すべての ASP.NET MVC Music Store サンプル アプリケーションをビルドする手順について説明します。 10 の一部では、ナビゲーションと S を最終的な更新プログラムについて説明.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 0c6e4c2f-fcdb-4978-9656-1990c6f15727
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
msc.type: authoredcontent
ms.openlocfilehash: b40d194c4d08f3564da59bacde4b5d3d7663373a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878595"
---
<a name="part-10-final-updates-to-navigation-and-site-design-conclusion"></a><span data-ttu-id="d3447-104">手順 10: 最終的な更新プログラムのナビゲーションおよびサイト設計、結論</span><span class="sxs-lookup"><span data-stu-id="d3447-104">Part 10: Final Updates to Navigation and Site Design, Conclusion</span></span>
====================
<span data-ttu-id="d3447-105">によって[Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="d3447-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="d3447-106">MVC Music Store は、チュートリアル アプリケーションを紹介し、ASP.NET MVC と Visual Studio web 開発を使用する方法の詳細な手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="d3447-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="d3447-107">MVC 音楽ストアは、オンラインにして、音楽のアルバムを販売し、基本的なサイトの管理、ユーザー サインインおよび買い物カゴの機能を実装する軽量なサンプル ストアの実装です。</span><span class="sxs-lookup"><span data-stu-id="d3447-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="d3447-108">このチュートリアルの系列では、すべての ASP.NET MVC Music Store サンプル アプリケーションをビルドする手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="d3447-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="d3447-109">10 の一部では、ナビゲーションとサイトの設計、結論を最終的な更新プログラムについて説明します。</span><span class="sxs-lookup"><span data-stu-id="d3447-109">Part 10 covers Final Updates to Navigation and Site Design, Conclusion.</span></span>


<span data-ttu-id="d3447-110">サイトのすべての主要な機能が終了しましたが、お、まだ一部の機能をサイトのナビゲーション、ホーム ページで、ストアの参照 ページに追加します。</span><span class="sxs-lookup"><span data-stu-id="d3447-110">We've completed all the major functionality for our site, but we still have some features to add to the site navigation, the home page, and the Store Browse page.</span></span>

## <a name="creating-the-shopping-cart-summary-partial-view"></a><span data-ttu-id="d3447-111">ショッピング カート概要部分ビューの作成</span><span class="sxs-lookup"><span data-stu-id="d3447-111">Creating the Shopping Cart Summary Partial View</span></span>

<span data-ttu-id="d3447-112">サイト全体にわたって、ユーザーのショッピング カート内の項目数を公開することができます。</span><span class="sxs-lookup"><span data-stu-id="d3447-112">We want to expose the number of items in the user's shopping cart across the entire site.</span></span>

![](mvc-music-store-part-10/_static/image1.png)

<span data-ttu-id="d3447-113">当社 Site.master に追加されている部分ビューを作成することで簡単に実装できます。</span><span class="sxs-lookup"><span data-stu-id="d3447-113">We can easily implement this by creating a partial view which is added to our Site.master.</span></span>

<span data-ttu-id="d3447-114">上記のように、ShoppingCart コント ローラーには、部分ビューを返す、CartSummary アクション メソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="d3447-114">As shown previously, the ShoppingCart controller includes a CartSummary action method which returns a partial view:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample1.cs)]

<span data-ttu-id="d3447-115">CartSummary 部分ビューを作成するには、ビュー/ShoppingCart フォルダーを右クリックし、ビューの追加を選択します。</span><span class="sxs-lookup"><span data-stu-id="d3447-115">To create the CartSummary partial view, right-click on the Views/ShoppingCart folder and select Add View.</span></span> <span data-ttu-id="d3447-116">CartSummary ビューを指定し、次に示すように、「部分ビューの作成」のチェック ボックスを確認します。</span><span class="sxs-lookup"><span data-stu-id="d3447-116">Name the view CartSummary and check the "Create a partial view" checkbox as shown below.</span></span>

![](mvc-music-store-part-10/_static/image2.png)

<span data-ttu-id="d3447-117">CartSummary 部分ビューは実に簡単 - カートにアイテムの数を示す ShoppingCart インデックス ビューへのリンクだけです。</span><span class="sxs-lookup"><span data-stu-id="d3447-117">The CartSummary partial view is really simple - it's just a link to the ShoppingCart Index view which shows the number of items in the cart.</span></span> <span data-ttu-id="d3447-118">CartSummary.cshtml の完全なコードは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="d3447-118">The complete code for CartSummary.cshtml is as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample2.cshtml)]

<span data-ttu-id="d3447-119">マスターを含む、サイト、Html.RenderAction メソッドを使用して、サイト内の任意のページには、部分ビューを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="d3447-119">We can include a partial view in any page in the site, including the Site master, by using the Html.RenderAction method.</span></span> <span data-ttu-id="d3447-120">RenderAction では、アクション名 ("CartSummary") と、コント ローラー名 ("ShoppingCart") として以下を指定することが必要です。</span><span class="sxs-lookup"><span data-stu-id="d3447-120">RenderAction requires us to specify the Action Name ("CartSummary") and the Controller Name ("ShoppingCart") as below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample3.cshtml)]

<span data-ttu-id="d3447-121">これをサイトのレイアウトに追加する前にも作成、[ジャンル] メニューを行えるように、Site.master 更新をすべて一度に 1 つです。</span><span class="sxs-lookup"><span data-stu-id="d3447-121">Before adding this to the site Layout, we will also create the Genre Menu so we can make all of our Site.master updates at one time.</span></span>

## <a name="creating-the-genre-menu-partial-view"></a><span data-ttu-id="d3447-122">ジャンル メニュー部分ビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="d3447-122">Creating the Genre Menu Partial View</span></span>

<span data-ttu-id="d3447-123">おやすく多くをストアで利用可能なすべてのジャンルの一覧を表示するジャンル メニューを追加することで、ストア内を移動するユーザー向けです。</span><span class="sxs-lookup"><span data-stu-id="d3447-123">We can make it a lot easier for our users to navigate through the store by adding a Genre Menu which lists all the Genres available in our store.</span></span>

![](mvc-music-store-part-10/_static/image3.png)

<span data-ttu-id="d3447-124">同じに従って手順も GenreMenu、部分ビューを作成し、その両方をサイトに、マスター追加おし、します。</span><span class="sxs-lookup"><span data-stu-id="d3447-124">We will follow the same steps also create a GenreMenu partial view, and then we can add them both to the Site master.</span></span> <span data-ttu-id="d3447-125">最初に、次の GenreMenu コント ローラーのアクションを StoreController に追加します。</span><span class="sxs-lookup"><span data-stu-id="d3447-125">First, add the following GenreMenu controller action to the StoreController:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample4.cs)]

<span data-ttu-id="d3447-126">この操作は、部分のビューは、次を作成して表示されるジャンルの一覧を返します。</span><span class="sxs-lookup"><span data-stu-id="d3447-126">This action returns a list of Genres which will be displayed by the partial view, which we will create next.</span></span>

<span data-ttu-id="d3447-127">*注: [ChildActionOnly] 属性のみ、部分ビューから使用するには、この操作が必要ことを示すこのコント ローラー アクションに追加されました。この属性は、/Store/GenreMenu を参照して実行されるをコント ローラーのアクションをできなくなります。部分ビューは、必須ではありませんが、ことをお勧めは、意図したとおりに使用する、コント ローラーのアクションを確認するためです。ビュー エンジンが他のビューの対象は、このビューのレイアウトを使用しないでくださいことを確認できるように、ビューではなく PartialView おも返します。*</span><span class="sxs-lookup"><span data-stu-id="d3447-127">*Note: We have added the [ChildActionOnly] attribute to this controller action, which indicates that we only want this action to be used from a Partial View. This attribute will prevent the controller action from being executed by browsing to /Store/GenreMenu. This isn't required for partial views, but it is a good practice, since we want to make sure our controller actions are used as we intend. We are also returning PartialView rather than View, which lets the view engine know that it shouldn't use the Layout for this view, as it is being included in other views.*</span></span>

<span data-ttu-id="d3447-128">GenreMenu コント ローラーのアクションを右クリックし、次に示すように、ジャンル ビューのデータ クラスを使用して型指定された厳密 GenreMenu をという名前の部分ビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="d3447-128">Right-click on the GenreMenu controller action and create a partial view named GenreMenu which is strongly typed using the Genre view data class as shown below.</span></span>

![](mvc-music-store-part-10/_static/image4.png)

<span data-ttu-id="d3447-129">次のように、順序なしリストを使用して項目を表示する GenreMenu 部分ビューのコードの表示を更新します。</span><span class="sxs-lookup"><span data-stu-id="d3447-129">Update the view code for the GenreMenu partial view to display the items using an unordered list as follows.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample5.cshtml)]

## <a name="updating-site-layout-to-display-our-partial-views"></a><span data-ttu-id="d3447-130">部分ビューを表示するサイトのレイアウトの更新</span><span class="sxs-lookup"><span data-stu-id="d3447-130">Updating Site Layout to display our Partial Views</span></span>

<span data-ttu-id="d3447-131">サイトのレイアウトに、部分ビューを追加できます (/ビュー/共有\_Layout.cshtml) Html.RenderAction() を呼び出すことによってです。</span><span class="sxs-lookup"><span data-stu-id="d3447-131">We can add our partial views to the Site Layout (/Views/Shared/\_Layout.cshtml) by calling Html.RenderAction().</span></span> <span data-ttu-id="d3447-132">次に示すよう、これらの両方をだけでなく、それらを表示するマークアップを追加するいくつかを追加おします。</span><span class="sxs-lookup"><span data-stu-id="d3447-132">We'll add them both in, as well as some additional markup to display them, as shown below:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample6.cshtml)]

<span data-ttu-id="d3447-133">今すぐアプリケーションを実行したときに、左側のナビゲーション領域のジャンル、上部にあるカート概要が表示されます。</span><span class="sxs-lookup"><span data-stu-id="d3447-133">Now when we run the application, we will see the Genre in the left navigation area and the Cart Summary at the top.</span></span>

## <a name="update-to-the-store-browse-page"></a><span data-ttu-id="d3447-134">ストアの参照 ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="d3447-134">Update to the Store Browse page</span></span>

<span data-ttu-id="d3447-135">ストアを参照ページでは、機能は、非常に良いが正しく表示されません。</span><span class="sxs-lookup"><span data-stu-id="d3447-135">The Store Browse page is functional, but doesn't look very good.</span></span> <span data-ttu-id="d3447-136">優れたレイアウトで、コードの表示 (/Views/Store/Browse.cshtml にあります) 次のように更新することで、アルバムを表示するページを更新することをがします。</span><span class="sxs-lookup"><span data-stu-id="d3447-136">We can update the page to show the albums in a better layout by updating the view code (found in /Views/Store/Browse.cshtml) as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample7.cshtml)]

<span data-ttu-id="d3447-137">ここで行っているお Html.ActionLink ではなく Url.Action の使用して、アルバム アートを含むへのリンクに特殊な書式設定を適用したようにします。</span><span class="sxs-lookup"><span data-stu-id="d3447-137">Here we are making use of Url.Action rather than Html.ActionLink so that we can apply special formatting to the link to include the album artwork.</span></span>

<span data-ttu-id="d3447-138">*注: これらのアルバムの汎用のアルバム カバーが表示されます。この情報は、データベースに格納され、ストア マネージャーを使用して編集可能な。独自のアートワークを追加するへようこそ があります。*</span><span class="sxs-lookup"><span data-stu-id="d3447-138">*Note: We are displaying a generic album cover for these albums. This information is stored in the database and is editable via the Store Manager. You are welcome to add your own artwork.*</span></span>

<span data-ttu-id="d3447-139">今すぐジャンルをブラウズ、ときにアルバム アートを持つグリッドに表示されるアルバムが表示されます。</span><span class="sxs-lookup"><span data-stu-id="d3447-139">Now when we browse to a Genre, we will see the albums shown in a grid with the album artwork.</span></span>

![](mvc-music-store-part-10/_static/image5.png)

## <a name="updating-the-home-page-to-show-top-selling-albums"></a><span data-ttu-id="d3447-140">上部の販売アルバムを表示するホーム ページの更新</span><span class="sxs-lookup"><span data-stu-id="d3447-140">Updating the Home Page to show Top Selling Albums</span></span>

<span data-ttu-id="d3447-141">売上増大をホーム ページ上のアルバムを販売している、上位の機能をさせていただきます。</span><span class="sxs-lookup"><span data-stu-id="d3447-141">We want to feature our top selling albums on the home page to increase sales.</span></span> <span data-ttu-id="d3447-142">ハンドルを追加するいくつか追加のグラフィック同様に、テンプレートを使用するには一部の更新プログラムにしましょう。</span><span class="sxs-lookup"><span data-stu-id="d3447-142">We'll make some updates to our HomeController to handle that, and add in some additional graphics as well.</span></span>

<span data-ttu-id="d3447-143">最初に、EntityFramework が関連付けられていることを認識できるように、アルバム クラスにナビゲーション プロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="d3447-143">First, we'll add a navigation property to our Album class so that EntityFramework knows that they're associated.</span></span> <span data-ttu-id="d3447-144">最後の数行、**アルバム**クラスは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="d3447-144">The last few lines of our **Album** class should now look like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample8.cs)]

<span data-ttu-id="d3447-145">*注: これを追加する必要を使用して、ステートメントを System.Collections.Generic 名前空間にします。*</span><span class="sxs-lookup"><span data-stu-id="d3447-145">*Note: This will require adding a using statement to bring in the System.Collections.Generic namespace.*</span></span>

<span data-ttu-id="d3447-146">まず、storeDB フィールドと、他のコント ローラーと同様に、ステートメントを使用して MvcMusicStore.Models 追加します。</span><span class="sxs-lookup"><span data-stu-id="d3447-146">First, we'll add a storeDB field and the MvcMusicStore.Models using statements, as in our other controllers.</span></span> <span data-ttu-id="d3447-147">次に、次のメソッドを OrderDetails に従ってトップ販売アルバムを検索するデータベースを照会する HomeController お追加します。</span><span class="sxs-lookup"><span data-stu-id="d3447-147">Next, we'll add the following method to the HomeController which queries our database to find top selling albums according to OrderDetails.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample9.cs)]

<span data-ttu-id="d3447-148">これは、コント ローラーのアクションとして使用できるようにしたくありませんので、プライベート メソッドです。</span><span class="sxs-lookup"><span data-stu-id="d3447-148">This is a private method, since we don't want to make it available as a controller action.</span></span> <span data-ttu-id="d3447-149">わかりやすくするため、テンプレートを使用するに含めることが、必要に応じて別のサービス クラスに、ビジネス ロジックを移動することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="d3447-149">We are including it in the HomeController for simplicity, but you are encouraged to move your business logic into separate service classes as appropriate.</span></span>

<span data-ttu-id="d3447-150">インプレースで、Index コント ローラー アクションは、上位 5 販売アルバムをクエリおよびビューに戻すに更新することができます。</span><span class="sxs-lookup"><span data-stu-id="d3447-150">With that in place, we can update the Index controller action to query the top 5 selling albums and return them to the view.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample10.cs)]

<span data-ttu-id="d3447-151">更新された HomeController の完全なコードは、次に示すようはします。</span><span class="sxs-lookup"><span data-stu-id="d3447-151">The complete code for the updated HomeController is as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample11.cs)]

<span data-ttu-id="d3447-152">最後に、モデルの種類を更新して、最下位にアルバム リストを追加することによってアルバムの一覧を表示できるように、ホーム インデックス ビューを更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d3447-152">Finally, we'll need to update our Home Index view so that it can display a list of albums by updating the Model type and adding the album list to the bottom.</span></span> <span data-ttu-id="d3447-153">私たちもページに見出しセクションがあり、プロモーションを追加するには、この機会になります。</span><span class="sxs-lookup"><span data-stu-id="d3447-153">We will take this opportunity to also add a heading and a promotion section to the page.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample12.cshtml)]

<span data-ttu-id="d3447-154">今すぐアプリケーションを実行したときにお最上位の販売アルバムやプロモーション、メッセージ、更新のホーム ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="d3447-154">Now when we run the application, we'll see our updated home page with top selling albums and our promotional message.</span></span>

![](mvc-music-store-part-10/_static/image1.jpg)

## <a name="conclusion"></a><span data-ttu-id="d3447-155">まとめ</span><span class="sxs-lookup"><span data-stu-id="d3447-155">Conclusion</span></span>

<span data-ttu-id="d3447-156">ある ASP.NET MVC 簡単これまで見てきたなどのデータベース アクセス、メンバーシップ、AJAX、高度な web サイトを作成します。</span><span class="sxs-lookup"><span data-stu-id="d3447-156">We've seen that ASP.NET MVC makes it easy to create a sophisticated website with database access, membership, AJAX, etc.</span></span> <span data-ttu-id="d3447-157">非常に短時間です。</span><span class="sxs-lookup"><span data-stu-id="d3447-157">pretty quickly.</span></span> <span data-ttu-id="d3447-158">おそらくこのチュートリアルが提供されているツールを独自の ASP.NET MVC アプリケーションの構築を開始する必要があります!</span><span class="sxs-lookup"><span data-stu-id="d3447-158">Hopefully this tutorial has given you the tools you need to get started building your own ASP.NET MVC applications!</span></span>


> [!div class="step-by-step"]
> [<span data-ttu-id="d3447-159">前へ</span><span class="sxs-lookup"><span data-stu-id="d3447-159">Previous</span></span>](mvc-music-store-part-9.md)
