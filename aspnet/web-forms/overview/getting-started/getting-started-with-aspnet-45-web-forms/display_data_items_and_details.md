---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: 項目し、詳細のデータの表示 |Microsoft Docs
author: Erikre
description: このチュートリアル シリーズが ASP.NET 4.7 および Microsoft Visual Studio 2017 に ASP.NET Web フォーム アプリケーションの構築の基礎を表示します。
ms.author: riande
ms.date: 1/04/2019
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: acc2f8e78375ef0455d467e2af750ecbee623224
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396226"
---
<a name="display-data-items-and-details"></a><span data-ttu-id="10920-103">データ アイテムの表示と詳細</span><span class="sxs-lookup"><span data-stu-id="10920-103">Display data items and details</span></span>
====================
<span data-ttu-id="10920-104">によって[Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="10920-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

> <span data-ttu-id="10920-105">このチュートリアル シリーズでは、ASP.NET 4.7 および Microsoft Visual Studio 2017 に ASP.NET Web フォーム アプリケーションの構築の基礎を説明します。</span><span class="sxs-lookup"><span data-stu-id="10920-105">This tutorial series teaches you the basics of building an ASP.NET Web Forms application with ASP.NET 4.7 and Microsoft Visual Studio 2017.</span></span>

<span data-ttu-id="10920-106">このチュートリアルでは、データ項目と ASP.NET Web フォームと Entity Framework Code First でのデータ項目の詳細を表示する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="10920-106">In this tutorial, you'll learn how to display data items and data item details with ASP.NET Web Forms and Entity Framework Code First.</span></span> <span data-ttu-id="10920-107">このチュートリアルでは、Wingtip 玩具店のチュートリアル シリーズの一部として、前の「UI とナビゲーション」チュートリアルに基づいています。</span><span class="sxs-lookup"><span data-stu-id="10920-107">This tutorial builds on the previous "UI and Navigation" tutorial as part of the Wingtip Toy Store tutorial series.</span></span> <span data-ttu-id="10920-108">このチュートリアルを完了した後に製品が表示されます、 *ProductsList.aspx*ページと、製品の詳細について、 *ProductDetails.aspx*ページ。</span><span class="sxs-lookup"><span data-stu-id="10920-108">After completing this tutorial, you'll see products on the *ProductsList.aspx* page and a product's details on the *ProductDetails.aspx* page.</span></span>

## <a name="youll-learn-how-to"></a><span data-ttu-id="10920-109">以下の方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="10920-109">You'll learn how to:</span></span>

- <span data-ttu-id="10920-110">データベースから製品を表示するデータ コントロールを追加します。</span><span class="sxs-lookup"><span data-stu-id="10920-110">Add a data control to display products from the database</span></span>
- <span data-ttu-id="10920-111">データ コントロールを選択したデータに接続します。</span><span class="sxs-lookup"><span data-stu-id="10920-111">Connect a data control to the selected data</span></span>
- <span data-ttu-id="10920-112">データベースから製品の詳細を表示するデータ コントロールを追加します。</span><span class="sxs-lookup"><span data-stu-id="10920-112">Add a data control to display product details from the database</span></span>
- <span data-ttu-id="10920-113">クエリ文字列から値を取得し、その値を使用して、データベースから取得したデータを制限するには</span><span class="sxs-lookup"><span data-stu-id="10920-113">Retrieve a value from the query string and use that value to limit the data that's retrieved from the database</span></span>

### <a name="features-introduced-in-this-tutorial"></a><span data-ttu-id="10920-114">このチュートリアルで導入された機能:</span><span class="sxs-lookup"><span data-stu-id="10920-114">Features introduced in this tutorial:</span></span>

- <span data-ttu-id="10920-115">モデル バインド</span><span class="sxs-lookup"><span data-stu-id="10920-115">Model binding</span></span>
- <span data-ttu-id="10920-116">値プロバイダー</span><span class="sxs-lookup"><span data-stu-id="10920-116">Value providers</span></span>

## <a name="add-a-data-control"></a><span data-ttu-id="10920-117">データ コントロールを追加します。</span><span class="sxs-lookup"><span data-stu-id="10920-117">Add a data control</span></span>

<span data-ttu-id="10920-118">いくつかの異なるオプションを使用すると、サーバー コントロールにデータをバインドします。</span><span class="sxs-lookup"><span data-stu-id="10920-118">You can use a few different options to bind data to a server control.</span></span> <span data-ttu-id="10920-119">最も一般的なのとおりです。</span><span class="sxs-lookup"><span data-stu-id="10920-119">The most common include:</span></span>

 * <span data-ttu-id="10920-120">データ ソース コントロールの追加</span><span class="sxs-lookup"><span data-stu-id="10920-120">Adding a data source control</span></span>
 * <span data-ttu-id="10920-121">コードを手動で追加します。</span><span class="sxs-lookup"><span data-stu-id="10920-121">Adding code by hand</span></span>
 * <span data-ttu-id="10920-122">モデル バインドを使用します。</span><span class="sxs-lookup"><span data-stu-id="10920-122">Using model binding</span></span>

### <a name="use-a-data-source-control-to-bind-data"></a><span data-ttu-id="10920-123">データ ソース コントロールを使用してデータをバインドするには</span><span class="sxs-lookup"><span data-stu-id="10920-123">Use a data source control to bind data</span></span>

<span data-ttu-id="10920-124">データ ソース コントロールの追加データを表示するコントロールにデータ ソース コントロールをリンクすることができます。</span><span class="sxs-lookup"><span data-stu-id="10920-124">Adding a data source control allows you to link the data source control to the control that displays the data.</span></span> <span data-ttu-id="10920-125">この方法により、ここでは、代わりにできますプログラムでは、サーバー側コントロールをデータ ソースに接続します。</span><span class="sxs-lookup"><span data-stu-id="10920-125">With this approach, you can declaratively,  rather than programmatically, connect server-side controls to data sources.</span></span>

### <a name="code-by-hand-to-bind-data"></a><span data-ttu-id="10920-126">データをバインドするには、手動でコード</span><span class="sxs-lookup"><span data-stu-id="10920-126">Code by hand to bind data</span></span>

<span data-ttu-id="10920-127">手動でコーディングが含まれます。</span><span class="sxs-lookup"><span data-stu-id="10920-127">Coding by hand involves:</span></span>

1. <span data-ttu-id="10920-128">値の読み取り</span><span class="sxs-lookup"><span data-stu-id="10920-128">Reading a value</span></span>
2. <span data-ttu-id="10920-129">かどうかは null をチェックします。</span><span class="sxs-lookup"><span data-stu-id="10920-129">Checking if it's null</span></span>
3. <span data-ttu-id="10920-130">適切な型に変換します。</span><span class="sxs-lookup"><span data-stu-id="10920-130">Converting it to an appropriate type</span></span>
4. <span data-ttu-id="10920-131">変換成功の確認</span><span class="sxs-lookup"><span data-stu-id="10920-131">Checking conversion success</span></span>
5. <span data-ttu-id="10920-132">クエリで値を使用します。</span><span class="sxs-lookup"><span data-stu-id="10920-132">Using the value in the query</span></span> 

<span data-ttu-id="10920-133">このアプローチにより、データ アクセス ロジックを完全に制御できます。</span><span class="sxs-lookup"><span data-stu-id="10920-133">This approach lets you have full control over your data-access logic.</span></span>

### <a name="use-model-binding-to-bind-data"></a><span data-ttu-id="10920-134">モデル バインドを使用してデータをバインドするには</span><span class="sxs-lookup"><span data-stu-id="10920-134">Use model binding to bind data</span></span>

<span data-ttu-id="10920-135">モデル バインドでは、はるかに少ないコードで結果をバインドすることができ、使用すると、アプリケーション全体で機能を再利用できます。</span><span class="sxs-lookup"><span data-stu-id="10920-135">Model binding lets you bind results with far less code and gives you the ability to reuse the functionality throughout your application.</span></span> <span data-ttu-id="10920-136">豊富なデータ バインディング フレームワークを提供しながらコード中心のデータ アクセス ロジックと作業が簡略化します。</span><span class="sxs-lookup"><span data-stu-id="10920-136">It simplifies working with code-focused data-access logic while still providing a rich, data-binding framework.</span></span>

## <a name="display-products"></a><span data-ttu-id="10920-137">製品を表示します。</span><span class="sxs-lookup"><span data-stu-id="10920-137">Display products</span></span>

<span data-ttu-id="10920-138">このチュートリアルでは、データをバインドするのにモデル バインドを使用します。</span><span class="sxs-lookup"><span data-stu-id="10920-138">In this tutorial, you'll use model binding to bind data.</span></span> <span data-ttu-id="10920-139">モデル バインドを使用してデータを選択するデータ コントロールを構成するには、コントロールを設定する`SelectMethod`プロパティ ページのコードでメソッドの名前にします。</span><span class="sxs-lookup"><span data-stu-id="10920-139">To configure a data control to use model binding to select data, you set the control's `SelectMethod` property to a method name in the page's code.</span></span> <span data-ttu-id="10920-140">データ コントロールでは、ページのライフ サイクルの適切なタイミングでメソッドを呼び出すし、自動的に返されるデータをバインドします。</span><span class="sxs-lookup"><span data-stu-id="10920-140">The data control calls the method at the appropriate time in the page life cycle and automatically binds the returned data.</span></span> <span data-ttu-id="10920-141">明示的に呼び出す必要はありません、`DataBind`メソッド。</span><span class="sxs-lookup"><span data-stu-id="10920-141">There's no need to explicitly call the `DataBind` method.</span></span>

1. <span data-ttu-id="10920-142">**ソリューション エクスプ ローラー**オープン*ProductList.aspx*します。</span><span class="sxs-lookup"><span data-stu-id="10920-142">In **Solution Explorer**, open *ProductList.aspx*.</span></span>
2. <span data-ttu-id="10920-143">このマークアップで既存のマークアップに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="10920-143">Replace the existing markup with this markup:</span></span>   

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample1.aspx)]

<span data-ttu-id="10920-144">このコードを使用して、 **ListView**という名前のコントロール`productList`商品を表示します。</span><span class="sxs-lookup"><span data-stu-id="10920-144">This code uses a **ListView** control named `productList` to display products.</span></span>

[!code-aspx-csharp[Main](display_data_items_and_details/samples/sample2.aspx)]

<span data-ttu-id="10920-145">テンプレートとスタイルを使用して定義する方法、 **ListView**コントロールにデータが表示されます。</span><span class="sxs-lookup"><span data-stu-id="10920-145">With templates and styles, you define how the **ListView** control displays data.</span></span> <span data-ttu-id="10920-146">任意の繰り返し構造内のデータと便利です。</span><span class="sxs-lookup"><span data-stu-id="10920-146">It's useful for data in any repeating structure.</span></span> <span data-ttu-id="10920-147">ただしこの**ListView**の例には、データベースのデータだけが表示されます、編集、挿入、およびデータを削除して、並べ替えやデータをページにユーザーを有効にするコードがないこともできます。</span><span class="sxs-lookup"><span data-stu-id="10920-147">Though this **ListView** example simply displays database data, you can also, without code, enable users to edit, insert, and delete data, and to sort and page data.</span></span>

<span data-ttu-id="10920-148">設定して、`ItemType`プロパティ、 **ListView**制御、データ バインディング式`Item`が使用可能なコントロールが厳密に型指定されました。</span><span class="sxs-lookup"><span data-stu-id="10920-148">By setting the `ItemType` property in the **ListView** control, the data-binding expression `Item` is available and the control becomes strongly typed.</span></span> <span data-ttu-id="10920-149">指定するなど、IntelliSense 項目オブジェクトの詳細を選択するには前のチュートリアルで前述の`ProductName`:</span><span class="sxs-lookup"><span data-stu-id="10920-149">As mentioned in the previous tutorial, you can select Item object details with IntelliSense, such as specifying the `ProductName`:</span></span>

![データを表示する項目と IntelliSense の詳細](display_data_items_and_details/_static/image1.png)

<span data-ttu-id="10920-151">モデル バインドを使用して指定することもいる、`SelectMethod`値。</span><span class="sxs-lookup"><span data-stu-id="10920-151">You're also using model binding to specify a `SelectMethod` value.</span></span> <span data-ttu-id="10920-152">この値 (`GetProducts`) に対応するメソッドのコードを追加します次の手順で製品を表示するには、遅れています。</span><span class="sxs-lookup"><span data-stu-id="10920-152">This value (`GetProducts`) corresponds to the method you'll add to the code behind to display products in the next step.</span></span>

### <a name="add-code-to-display-products"></a><span data-ttu-id="10920-153">製品を表示するコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="10920-153">Add code to display products</span></span>

<span data-ttu-id="10920-154">この手順では、データを読み込むコードを追加します、 **ListView**コントロールに、データベースから製品データ。</span><span class="sxs-lookup"><span data-stu-id="10920-154">In this step, you'll add code to populate the **ListView** control with product data from the database.</span></span> <span data-ttu-id="10920-155">コードでは、すべての製品と個々 のカテゴリの製品を表示をサポートします。</span><span class="sxs-lookup"><span data-stu-id="10920-155">The code supports showing all products and  individual category products.</span></span>

1. <span data-ttu-id="10920-156">**ソリューション エクスプ ローラー**を右クリックして*ProductList.aspx*選び**コードの表示**します。</span><span class="sxs-lookup"><span data-stu-id="10920-156">In **Solution Explorer**, right-click *ProductList.aspx* and then select **View Code**.</span></span>
2. <span data-ttu-id="10920-157">既存のコードを置き換える、 *ProductList.aspx.cs*このファイル。</span><span class="sxs-lookup"><span data-stu-id="10920-157">Replace the existing code in the *ProductList.aspx.cs* file with this:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

<span data-ttu-id="10920-158">このコードを示しています、`GetProducts`メソッドを**ListView**コントロールの`ItemType`内のプロパティ参照、 *ProductList.aspx*ページ。</span><span class="sxs-lookup"><span data-stu-id="10920-158">This code shows the `GetProducts` method that the **ListView** control's `ItemType` property references in the *ProductList.aspx* page.</span></span> <span data-ttu-id="10920-159">コードの設定を特定のデータベース カテゴリに結果を制限するため、`categoryId`に渡されるクエリ文字列値から、 *ProductList.aspx*ときにページ、 *ProductList.aspx*ページは、移動します。</span><span class="sxs-lookup"><span data-stu-id="10920-159">To limit the results to a specific database category, the code sets the `categoryId` value from the query string value passed to the *ProductList.aspx* page when the *ProductList.aspx* page is navigated to.</span></span> <span data-ttu-id="10920-160">`QueryStringAttribute`クラス、`System.Web.ModelBinding`名前空間を使用して、クエリ文字列変数の値を取得`id`します。</span><span class="sxs-lookup"><span data-stu-id="10920-160">The `QueryStringAttribute` class in the `System.Web.ModelBinding` namespace is used to retrieve the value of the query string variable `id`.</span></span> <span data-ttu-id="10920-161">これに指示するクエリ文字列から値をバインドしようとするモデル バインド、`categoryId`実行時にパラメーター。</span><span class="sxs-lookup"><span data-stu-id="10920-161">This instructs model binding to try to bind a value from the query string to the `categoryId` parameter at run time.</span></span>

<span data-ttu-id="10920-162">クエリの結果に一致するデータベース内のこれらの製品に制限されています ページにクエリ文字列として有効なカテゴリが渡される、`categoryId`値。</span><span class="sxs-lookup"><span data-stu-id="10920-162">When a valid category is passed as a query string to the page, the results of the query are limited to those products in the database that match the `categoryId` value.</span></span> <span data-ttu-id="10920-163">たとえば場合、 *ProductsList.aspx*はページの URL:</span><span class="sxs-lookup"><span data-stu-id="10920-163">For instance, if the *ProductsList.aspx* page URL is this:</span></span>


[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

<span data-ttu-id="10920-164">ページには、製品のみが表示されます。 場所、 `categoryId` equals`1`します。</span><span class="sxs-lookup"><span data-stu-id="10920-164">The page displays only the products where the `categoryId` equals `1`.</span></span>

<span data-ttu-id="10920-165">クエリ文字列が含まれていないときに場合、すべての製品が表示されます、 *ProductList.aspx*ページが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="10920-165">All products are displayed if no query string is included when the *ProductList.aspx* page is called.</span></span>

<span data-ttu-id="10920-166">これらのメソッドの値のソースとして参照されます*値プロバイダー* (など*QueryString*)、パラメーターの属性を使用する値プロバイダーを示すとして参照されます*値プロバイダー属性*(など`id`)。</span><span class="sxs-lookup"><span data-stu-id="10920-166">The sources of values for these methods are referred to as *value providers* (such as *QueryString*), and the parameter attributes that indicate which value provider to use are referred to as *value provider attributes* (such as `id`).</span></span> <span data-ttu-id="10920-167">ASP.NET には、クエリ文字列、cookie、フォーム値、コントロール、ビュー ステート、セッション状態、およびプロファイルのプロパティなどの Web フォーム アプリケーションでの値プロバイダーとすべてのユーザー入力の一般的なソースの対応する属性が含まれています。</span><span class="sxs-lookup"><span data-stu-id="10920-167">ASP.NET includes value providers and corresponding attributes for all of the typical sources of user input in a Web Forms application such as the query string, cookies, form values, controls, view state, session state, and profile properties.</span></span> <span data-ttu-id="10920-168">カスタム値プロバイダーを記述することもできます。</span><span class="sxs-lookup"><span data-stu-id="10920-168">You can also write custom value providers.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="10920-169">アプリケーションの実行</span><span class="sxs-lookup"><span data-stu-id="10920-169">Run the application</span></span>

<span data-ttu-id="10920-170">すべての製品またはカテゴリの製品を表示するアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="10920-170">Run the application now to view all products or a category's products.</span></span>

1. <span data-ttu-id="10920-171">キーを押して**F5** Visual Studio でアプリケーションを実行するときにします。</span><span class="sxs-lookup"><span data-stu-id="10920-171">Press **F5** while in Visual Studio to run the application.</span></span>  
   <span data-ttu-id="10920-172">ブラウザーが開きを示しています、 *Default.aspx*ページ。</span><span class="sxs-lookup"><span data-stu-id="10920-172">The browser opens and shows the *Default.aspx* page.</span></span>

2. <span data-ttu-id="10920-173">選択**自動車**製品カテゴリのナビゲーション メニュー。</span><span class="sxs-lookup"><span data-stu-id="10920-173">Select **Cars** from the product category navigation menu.</span></span>  
   <span data-ttu-id="10920-174">*ProductList.aspx*ページのみ表示**自動車**カテゴリの製品です。</span><span class="sxs-lookup"><span data-stu-id="10920-174">The *ProductList.aspx* page displays showing only **Cars** category products.</span></span> <span data-ttu-id="10920-175">このチュートリアルの後半では、製品の詳細を表示します。</span><span class="sxs-lookup"><span data-stu-id="10920-175">Later in this tutorial, you'll display product details.</span></span>  

    ![データを表示する項目と詳細 - 自動車](display_data_items_and_details/_static/image2.png)

3. <span data-ttu-id="10920-177">選択**製品**上部にあるナビゲーション メニュー。</span><span class="sxs-lookup"><span data-stu-id="10920-177">Select **Products** from the navigation menu at the top.</span></span>  
   <span data-ttu-id="10920-178">もう一度、 *ProductList.aspx*ページが表示されますが、今度は製品の全リストが表示されます。</span><span class="sxs-lookup"><span data-stu-id="10920-178">Again, the *ProductList.aspx* page is displayed, however this time it shows the entire list of products.</span></span>   

    ![データを表示する項目と製品の詳細](display_data_items_and_details/_static/image3.png)

4. <span data-ttu-id="10920-180">ブラウザーを閉じて、Visual Studio に戻ります。</span><span class="sxs-lookup"><span data-stu-id="10920-180">Close the browser and return to Visual Studio.</span></span>

### <a name="add-a-data-control-to-display-product-details"></a><span data-ttu-id="10920-181">製品の詳細を表示するデータ コントロールを追加します。</span><span class="sxs-lookup"><span data-stu-id="10920-181">Add a data control to display product details</span></span>

<span data-ttu-id="10920-182">内のマークアップを次に、変更を*ProductDetails.aspx*ページが特定の製品情報を表示する前のチュートリアルで追加しました。</span><span class="sxs-lookup"><span data-stu-id="10920-182">Next, you'll modify the markup in the *ProductDetails.aspx* page that you added in the previous tutorial to display specific product information.</span></span>

1. <span data-ttu-id="10920-183">**ソリューション エクスプ ローラー**オープン*ProductDetails.aspx*します。</span><span class="sxs-lookup"><span data-stu-id="10920-183">In **Solution Explorer**, open *ProductDetails.aspx*.</span></span>

2. <span data-ttu-id="10920-184">このマークアップで既存のマークアップに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="10920-184">Replace the existing markup with this markup:</span></span>

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample5.aspx)] 

    <span data-ttu-id="10920-185">このコードを使用して、 **FormView**特定の製品の詳細を表示するコントロール。</span><span class="sxs-lookup"><span data-stu-id="10920-185">This code uses a **FormView** control to display specific product details.</span></span> <span data-ttu-id="10920-186">このマークアップは、データを表示するために使用するメソッドと同様のメソッドを使用して、 *ProductList.aspx*ページ。</span><span class="sxs-lookup"><span data-stu-id="10920-186">This markup uses methods like the methods used to display data in the *ProductList.aspx* page.</span></span> <span data-ttu-id="10920-187">**FormView**コントロールを使用すると、データ ソースから一度に 1 つのレコードを表示します。</span><span class="sxs-lookup"><span data-stu-id="10920-187">The **FormView** control is used to display a single record at a time from a data source.</span></span> <span data-ttu-id="10920-188">使用すると、 **FormView**コントロール、データ バインドされた値を表示および編集テンプレートを作成します。</span><span class="sxs-lookup"><span data-stu-id="10920-188">When you use the **FormView** control, you create templates to display and edit data-bound values.</span></span> <span data-ttu-id="10920-189">これらのテンプレートは、バインド式のコントロールを含めるし、フォームの外観と機能を定義する書式設定します。</span><span class="sxs-lookup"><span data-stu-id="10920-189">These templates contain controls, binding expressions, and formatting that define the form's look and functionality.</span></span>

<span data-ttu-id="10920-190">前のマークアップをデータベースに接続するには、コードを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="10920-190">Connecting the previous markup to the database requires additional code.</span></span>

1. <span data-ttu-id="10920-191">**ソリューション エクスプ ローラー**、右クリック*ProductDetails.aspx*  をクリックし、**コードの表示**します。</span><span class="sxs-lookup"><span data-stu-id="10920-191">In **Solution Explorer**, right-click *ProductDetails.aspx* and then click **View Code**.</span></span>  
   <span data-ttu-id="10920-192">*ProductDetails.aspx.cs*ファイルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="10920-192">The *ProductDetails.aspx.cs* file is displayed.</span></span>

2. <span data-ttu-id="10920-193">このコードでは、既存のコードを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="10920-193">Replace the existing code with this code:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

<span data-ttu-id="10920-194">このコードをチェックする"`productID`"クエリ文字列値。</span><span class="sxs-lookup"><span data-stu-id="10920-194">This code checks for a "`productID`" query-string value.</span></span> <span data-ttu-id="10920-195">有効なクエリ文字列値が見つかった場合は、一致する製品が表示されます。</span><span class="sxs-lookup"><span data-stu-id="10920-195">If a valid query-string value is found, the matching product is displayed.</span></span> <span data-ttu-id="10920-196">クエリ文字列が見つからない、またはその値が無効です、製品は表示されません。</span><span class="sxs-lookup"><span data-stu-id="10920-196">If the query-string isn't found, or its value isn't valid, no product is displayed.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="10920-197">アプリケーションの実行</span><span class="sxs-lookup"><span data-stu-id="10920-197">Run the application</span></span>

<span data-ttu-id="10920-198">プロダクト ID に基づいて、表示される個別の製品を表示するアプリケーションを実行するようになりました</span><span class="sxs-lookup"><span data-stu-id="10920-198">Now you can run the application to see an individual product displayed based on product ID.</span></span>

1. <span data-ttu-id="10920-199">キーを押して**F5** Visual Studio でアプリケーションを実行するときにします。</span><span class="sxs-lookup"><span data-stu-id="10920-199">Press **F5** while in Visual Studio to run the application.</span></span>  
   <span data-ttu-id="10920-200">ブラウザーが開きを示しています、 *Default.aspx*ページ。</span><span class="sxs-lookup"><span data-stu-id="10920-200">The browser opens and shows the *Default.aspx* page.</span></span>

2. <span data-ttu-id="10920-201">選択**ボート**カテゴリのナビゲーション メニュー。</span><span class="sxs-lookup"><span data-stu-id="10920-201">Select **Boats** from the category navigation menu.</span></span>  
   <span data-ttu-id="10920-202">*ProductList.aspx*ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="10920-202">The *ProductList.aspx* page is displayed.</span></span>

3. <span data-ttu-id="10920-203">選択**用紙ボート**製品一覧から。</span><span class="sxs-lookup"><span data-stu-id="10920-203">Select **Paper Boat** from the product list.</span></span>
   <span data-ttu-id="10920-204">*ProductDetails.aspx*ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="10920-204">The *ProductDetails.aspx* page is displayed.</span></span>

    ![データを表示する項目と製品の詳細](display_data_items_and_details/_static/image4.png)
    
4. <span data-ttu-id="10920-206">ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="10920-206">Close the browser.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="10920-207">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="10920-207">Additional resources</span></span>

[<span data-ttu-id="10920-208">取得して、モデル バインディングと web フォームでデータの表示</span><span class="sxs-lookup"><span data-stu-id="10920-208">Retrieving and displaying data with model binding and web forms</span></span>](../../presenting-and-managing-data/model-binding/retrieving-data.md)

## <a name="next-steps"></a><span data-ttu-id="10920-209">次の手順</span><span class="sxs-lookup"><span data-stu-id="10920-209">Next steps</span></span>

<span data-ttu-id="10920-210">このチュートリアルでは、マークアップと製品と製品の詳細を表示するコードを追加しました。</span><span class="sxs-lookup"><span data-stu-id="10920-210">In this tutorial, you added markup and code to display products and product details.</span></span> <span data-ttu-id="10920-211">厳密に型指定されたデータ コントロール、モデル バインド、および値プロバイダーについて説明しました。</span><span class="sxs-lookup"><span data-stu-id="10920-211">You learned about strongly typed data controls, model binding, and value providers.</span></span> <span data-ttu-id="10920-212">次のチュートリアルでは、Wingtip Toys のサンプル アプリケーションをショッピング カートを追加します。</span><span class="sxs-lookup"><span data-stu-id="10920-212">In the next tutorial, you'll add a shopping cart to the Wingtip Toys sample application.</span></span> 

> [!div class="step-by-step"]
> <span data-ttu-id="10920-213">[前へ](ui_and_navigation.md)
> [次へ](shopping-cart.md)</span><span class="sxs-lookup"><span data-stu-id="10920-213">[Previous](ui_and_navigation.md)
[Next](shopping-cart.md)</span></span>
