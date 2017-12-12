---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
title: "パート 3: レイアウトとカテゴリ メニュー |Microsoft ドキュメント"
author: JoeStagner
description: "このチュートリアルの系列では、すべて Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。 パート 3 は、追加のレイアウトとカテゴリのメニューについて説明します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 94ea1a70-a9bc-4241-8f36-08366d64bab9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
msc.type: authoredcontent
ms.openlocfilehash: 57c0342efb67b94a0d8c8b06dc13a727e7184db8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="part-3-layout-and-category-menu"></a><span data-ttu-id="86dba-104">パート 3: レイアウトとカテゴリのメニュー</span><span class="sxs-lookup"><span data-stu-id="86dba-104">Part 3: Layout and Category Menu</span></span>
====================
<span data-ttu-id="86dba-105">によって[行える](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="86dba-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="86dba-106">Tailspin Spyworks では、.NET プラットフォーム用の強力な拡張性の高いアプリケーションを作成するはどのように非常に単純なを示しています。</span><span class="sxs-lookup"><span data-stu-id="86dba-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="86dba-107">ASP.NET 4 の優れた新機能を使用して、ショッピング、チェック アウト、および管理を含む、オンライン ストアをビルドする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="86dba-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="86dba-108">このチュートリアルの系列では、すべて Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="86dba-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="86dba-109">パート 3 は、追加のレイアウトとカテゴリのメニューについて説明します。</span><span class="sxs-lookup"><span data-stu-id="86dba-109">Part 3 covers adding layout and a category menu.</span></span>


## <a id="_Toc260221669"></a><span data-ttu-id="86dba-110">一部のレイアウトとカテゴリのメニューを追加します。</span><span class="sxs-lookup"><span data-stu-id="86dba-110">Adding Some Layout and a Category Menu</span></span>

<span data-ttu-id="86dba-111">このサイトのマスター ページで、左側にあるを含む列に、製品カテゴリ メニューの div を追加します。</span><span class="sxs-lookup"><span data-stu-id="86dba-111">In our site master page we'll add a div for the left side column that will contain our product category menu.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample1.aspx)]

<span data-ttu-id="86dba-112">目的の整列やその他の書式設定を Style.css ファイルに追加した CSS クラスによって提供されることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="86dba-112">Note that the desired aligning and other formatting will be provided by the CSS class that we added to our Style.css file.</span></span>

[!code-css[Main](tailspin-spyworks-part-3/samples/sample2.css)]

<span data-ttu-id="86dba-113">製品のカテゴリのメニューは、実行時で動的に既存の製品カテゴリとメニュー項目を作成、および対応するリンクの Commerce データベースのクエリを実行して作成されます。</span><span class="sxs-lookup"><span data-stu-id="86dba-113">The product category menu will be dynamically created at runtime by querying the Commerce database for existing product categories and creating the menu items and corresponding links.</span></span>

<span data-ttu-id="86dba-114">これを実現するには、2 つの ASP を使用します。NET の強力なデータを制御します。</span><span class="sxs-lookup"><span data-stu-id="86dba-114">To accomplish this we will use two of ASP.NET's powerful data controls.</span></span> <span data-ttu-id="86dba-115">[エンティティ データ ソース] コントロールは、"ListView"コントロール。</span><span class="sxs-lookup"><span data-stu-id="86dba-115">The "Entity Data Source" control and the "ListView" control.</span></span>

![](tailspin-spyworks-part-3/_static/image1.jpg)

<span data-ttu-id="86dba-116">「デザイン ビュー」にし、コントロールを設定するヘルパーを使用してみましょう。</span><span class="sxs-lookup"><span data-stu-id="86dba-116">Let's switch to "Design View" and use the helpers to configure our controls.</span></span>

![](tailspin-spyworks-part-3/_static/image2.jpg)

<span data-ttu-id="86dba-117">Eds EntityDataSource ID プロパティを設定してみましょう\_カテゴリ\_メニューと「データ ソースの構成」をクリックします。</span><span class="sxs-lookup"><span data-stu-id="86dba-117">Let's set the EntityDataSource ID property to EDS\_Category\_Menu and click on "Configure Data Source".</span></span>

![](tailspin-spyworks-part-3/_static/image3.jpg)

<span data-ttu-id="86dba-118">ご利用の米国 Commerce データベースのエンティティのデータ ソースのモデルを作成したときに作成された CommerceEntities 接続を選択し、[次へ] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="86dba-118">Select the CommerceEntities Connection that was created for us when we created the Entity Data Source Model for our Commerce Database and click "Next".</span></span>

![](tailspin-spyworks-part-3/_static/image4.jpg)

<span data-ttu-id="86dba-119">"Categories"エンティティ セットの名前を選択し、既定値としての残りのオプションのままにします。</span><span class="sxs-lookup"><span data-stu-id="86dba-119">Select the "Categories" Entity set name and leave the rest of the options as default.</span></span> <span data-ttu-id="86dba-120">[完了] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="86dba-120">Click "Finish".</span></span>

<span data-ttu-id="86dba-121">ここで、ページを ListView に配置しているお ListView コントロールのインスタンスの ID プロパティを設定してみましょう\_ProductsMenu とそのヘルパーをアクティブ化します。</span><span class="sxs-lookup"><span data-stu-id="86dba-121">Now let's set the ID property of the ListView control instance that we placed on our page to ListView\_ProductsMenu and activate its helper.</span></span>

![](tailspin-spyworks-part-3/_static/image5.jpg)

<span data-ttu-id="86dba-122">ただし管理のオプションを使用してデータ項目の表示の書式を設定して、書式設定、メニューの作成のみが必要になります単純なマークアップのため、コード、ソース ビューに入力されます。</span><span class="sxs-lookup"><span data-stu-id="86dba-122">Though we could use control options to format the data item display and formatting, our menu creation will only require simple markup so we will enter the code in the source view.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample3.aspx)]

<span data-ttu-id="86dba-123">"Eval"ステートメントに注意してください: &lt;%# Eval("CategoryName") %&gt;</span><span class="sxs-lookup"><span data-stu-id="86dba-123">Please note the "Eval" statement : &lt;%# Eval("CategoryName") %&gt;</span></span>

<span data-ttu-id="86dba-124">ASP.NET の構文&lt;%# %&gt;内に含まれるものと、"行"の結果を出力を実行するランタイムに指示するためのショートハンド規約です。</span><span class="sxs-lookup"><span data-stu-id="86dba-124">The ASP.NET syntax &lt;%# %&gt; is a shorthand convention that instructs the runtime to execute whatever is contained within and output the results "in Line".</span></span>

<span data-ttu-id="86dba-125">エンティティ モデル アイテム名の値を"CatagoryName"にフェッチ、Eval("CategoryName") ステートメントをデータ項目のバインドされたコレクションの現在のエントリのです。</span><span class="sxs-lookup"><span data-stu-id="86dba-125">The statement Eval("CategoryName") instructs that, for the current entry in the bound collection of data items, fetch the value of the Entity Model item names "CatagoryName".</span></span> <span data-ttu-id="86dba-126">これは、非常に強力な機能の簡潔な構文です。</span><span class="sxs-lookup"><span data-stu-id="86dba-126">This is concise syntax for a very powerful feature.</span></span>

<span data-ttu-id="86dba-127">今すぐアプリケーションを実行することができます。</span><span class="sxs-lookup"><span data-stu-id="86dba-127">Lets run the application now.</span></span>

![](tailspin-spyworks-part-3/_static/image6.jpg)

<span data-ttu-id="86dba-128">当社製品カテゴリ メニューに表示されていますしおよび ProductsList.aspx ときにマウス ポインターを移動 メニュー項目のリンク先を実装するには、まだあるページに表示カテゴリ メニュー項目の 1 つという名前を作成した、動的なクエリ文字列引数を含む、 カテゴリ id。</span><span class="sxs-lookup"><span data-stu-id="86dba-128">Note that our product category menu is now displayed and when we hover over one of the category menu items we can see the menu item link points to a page we have yet to implement named ProductsList.aspx and that we have built a dynamic query string argument that contains the category id.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="86dba-129">[前へ](tailspin-spyworks-part-2.md)
[次へ](tailspin-spyworks-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="86dba-129">[Previous](tailspin-spyworks-part-2.md)
[Next](tailspin-spyworks-part-4.md)</span></span>
