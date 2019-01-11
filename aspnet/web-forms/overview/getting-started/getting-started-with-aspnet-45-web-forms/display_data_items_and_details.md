---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: 項目し、詳細のデータの表示 |Microsoft Docs
author: Erikre
description: このチュートリアル シリーズが Web 用の ASP.NET 4.7 および Microsoft Visual Studio Community 2017 に ASP.NET Web フォーム アプリケーションの構築の基礎を講義します。
ms.author: riande
ms.date: 1/09/2019
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: 73ae1660f5d6e3e28c1c155e745a62936e3502df
ms.sourcegitcommit: cec77d5ad8a0cedb1ecbec32834111492afd0cd2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/10/2019
ms.locfileid: "54207435"
---
<a name="display-data-items-and-details"></a><span data-ttu-id="6e083-103">データ アイテムの表示と詳細</span><span class="sxs-lookup"><span data-stu-id="6e083-103">Display data items and details</span></span>
====================
<span data-ttu-id="6e083-104">によって[Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="6e083-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

> <span data-ttu-id="6e083-105">このチュートリアル シリーズでは、Web 用の ASP.NET 4.7 および Microsoft Visual Studio Community 2017 に ASP.NET Web フォーム アプリケーションの構築の基礎を説明します。</span><span class="sxs-lookup"><span data-stu-id="6e083-105">This tutorial series teaches you the basics of building an ASP.NET Web Forms application with ASP.NET 4.7 and Microsoft Visual Studio Community 2017 for the Web.</span></span>

<span data-ttu-id="6e083-106">このチュートリアルでは、データ項目と ASP.NET Web フォームと Entity Framework Code First でのデータ項目の詳細を表示する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="6e083-106">In this tutorial, you learn how to display data items and data item details with ASP.NET Web Forms and Entity Framework Code First.</span></span> <span data-ttu-id="6e083-107">このチュートリアルでは、Wingtip 玩具店のチュートリアル シリーズの一部として、前の「UI とナビゲーション」チュートリアルに基づいています。</span><span class="sxs-lookup"><span data-stu-id="6e083-107">This tutorial builds on the previous "UI and Navigation" tutorial as part of the Wingtip Toy Store tutorial series.</span></span> <span data-ttu-id="6e083-108">完了したチュートリアルでは、上の製品で、 *ProductsList.aspx*ページと、製品の詳細について、 *ProductDetails.aspx*ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="6e083-108">In the completed tutorial, products on the *ProductsList.aspx* page and a product's details on the *ProductDetails.aspx* page are displayed.</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="6e083-109">学習内容</span><span class="sxs-lookup"><span data-stu-id="6e083-109">What you learn</span></span>

- <span data-ttu-id="6e083-110">データベース製品を表示するデータ コントロールを追加します。</span><span class="sxs-lookup"><span data-stu-id="6e083-110">Add a data control to display database products.</span></span>
- <span data-ttu-id="6e083-111">データ コントロールを選択したデータに接続します。</span><span class="sxs-lookup"><span data-stu-id="6e083-111">Connect a data control to selected data.</span></span>
- <span data-ttu-id="6e083-112">製品の詳細を表示するデータ コントロールを追加します。</span><span class="sxs-lookup"><span data-stu-id="6e083-112">Add a data control to display product details.</span></span>
- <span data-ttu-id="6e083-113">クエリ文字列の値を解析し、取得したデータベースのデータをフィルター処理に使用します。</span><span class="sxs-lookup"><span data-stu-id="6e083-113">Parse a query string value and use it to filter retrieved database data.</span></span>

<span data-ttu-id="6e083-114">このチュートリアルで導入された機能には、モデル バインドと値のプロバイダーが含まれます。</span><span class="sxs-lookup"><span data-stu-id="6e083-114">Features introduced in this tutorial include model binding and value providers.</span></span>

## <a name="add-a-data-control-to-display-products"></a><span data-ttu-id="6e083-115">製品を表示するデータ コントロールを追加します。</span><span class="sxs-lookup"><span data-stu-id="6e083-115">Add a data control to display products</span></span>
 
<span data-ttu-id="6e083-116">サーバー コントロールにデータをバインドするいくつかのオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="6e083-116">You have a few options to bind data to a server control.</span></span> <span data-ttu-id="6e083-117">最も一般的なのとおりです。</span><span class="sxs-lookup"><span data-stu-id="6e083-117">The most common include:</span></span>

 * <span data-ttu-id="6e083-118">データ ソース コントロールの追加</span><span class="sxs-lookup"><span data-stu-id="6e083-118">Adding a data source control</span></span>
 * <span data-ttu-id="6e083-119">コードを手動で追加します。</span><span class="sxs-lookup"><span data-stu-id="6e083-119">Adding code by hand</span></span>
 * <span data-ttu-id="6e083-120">モデル バインドを実装します。</span><span class="sxs-lookup"><span data-stu-id="6e083-120">Implementing model binding</span></span>

### <a name="use-a-data-source-control-to-bind-data"></a><span data-ttu-id="6e083-121">データ ソース コントロールを使用してデータをバインドするには</span><span class="sxs-lookup"><span data-stu-id="6e083-121">Use a data source control to bind data</span></span>

<span data-ttu-id="6e083-122">データ ソース コントロールの追加データを表示するコントロールにデータ ソース コントロールをリンクします。</span><span class="sxs-lookup"><span data-stu-id="6e083-122">Adding a data source control links the data source control to the control that displays the data.</span></span> <span data-ttu-id="6e083-123">この方法により、ここでは、代わりにできますプログラムでは、サーバー側コントロールをデータ ソースに接続します。</span><span class="sxs-lookup"><span data-stu-id="6e083-123">With this approach, you can declaratively, rather than programmatically, connect server-side controls to data sources.</span></span>

### <a name="code-by-hand-to-bind-data"></a><span data-ttu-id="6e083-124">データをバインドするには、手動でコード</span><span class="sxs-lookup"><span data-stu-id="6e083-124">Code by hand to bind data</span></span>

<span data-ttu-id="6e083-125">手動でコーディングが含まれます。</span><span class="sxs-lookup"><span data-stu-id="6e083-125">Coding by hand involves:</span></span>

1. <span data-ttu-id="6e083-126">値の読み取り</span><span class="sxs-lookup"><span data-stu-id="6e083-126">Reading a value</span></span>
2. <span data-ttu-id="6e083-127">かどうかは null をチェックします。</span><span class="sxs-lookup"><span data-stu-id="6e083-127">Checking if it's null</span></span>
3. <span data-ttu-id="6e083-128">適切な型に変換します。</span><span class="sxs-lookup"><span data-stu-id="6e083-128">Converting it to an appropriate type</span></span>
4. <span data-ttu-id="6e083-129">変換成功の確認</span><span class="sxs-lookup"><span data-stu-id="6e083-129">Checking conversion success</span></span>
5. <span data-ttu-id="6e083-130">変換後の値を持つクエリを作成します。</span><span class="sxs-lookup"><span data-stu-id="6e083-130">Making a query with the converted value</span></span> 

<span data-ttu-id="6e083-131">この方法で、データ アクセス ロジックを完全に制御があります。</span><span class="sxs-lookup"><span data-stu-id="6e083-131">With this approach, you have full control over your data-access logic.</span></span>

### <a name="use-model-binding-to-bind-data"></a><span data-ttu-id="6e083-132">モデル バインドを使用してデータをバインドするには</span><span class="sxs-lookup"><span data-stu-id="6e083-132">Use model binding to bind data</span></span>

<span data-ttu-id="6e083-133">モデル バインディングではるかに少ないコードで結果を連結して、使用すると、アプリケーション全体で機能を再利用できます。</span><span class="sxs-lookup"><span data-stu-id="6e083-133">With model binding, you bind results with far less code and it gives you the ability to reuse the functionality throughout your application.</span></span> <span data-ttu-id="6e083-134">豊富なデータ バインディング フレームワークを提供しながらコード中心のデータ アクセス ロジックと作業が簡略化します。</span><span class="sxs-lookup"><span data-stu-id="6e083-134">It simplifies working with code-focused data-access logic while still providing a rich, data-binding framework.</span></span>

## <a name="display-products"></a><span data-ttu-id="6e083-135">製品を表示します。</span><span class="sxs-lookup"><span data-stu-id="6e083-135">Display products</span></span>

<span data-ttu-id="6e083-136">このチュートリアルでは、モデル バインドを使用してデータをバインドします。</span><span class="sxs-lookup"><span data-stu-id="6e083-136">In this tutorial, you use model binding to bind data.</span></span> <span data-ttu-id="6e083-137">モデル バインドを使用してデータを選択するデータ コントロールを構成するには、コントロールを設定する`SelectMethod`プロパティ ページのコード内のメソッドにします。</span><span class="sxs-lookup"><span data-stu-id="6e083-137">To configure a data control to use model binding to select data, you set the control's `SelectMethod` property to a method in the page's code.</span></span> <span data-ttu-id="6e083-138">データ コントロールでは、ページのライフ サイクルの適切なタイミングでメソッドを呼び出すし、自動的に返されるデータをバインドします。</span><span class="sxs-lookup"><span data-stu-id="6e083-138">The data control calls the method at the appropriate time in the page life cycle and automatically binds the returned data.</span></span> <span data-ttu-id="6e083-139">明示的に呼び出す必要はありません、`DataBind`メソッド。</span><span class="sxs-lookup"><span data-stu-id="6e083-139">There's no need to explicitly call the `DataBind` method.</span></span>

<span data-ttu-id="6e083-140">変更する、次の手順を通して*ProductList.aspx*製品を表示するマークアップ。</span><span class="sxs-lookup"><span data-stu-id="6e083-140">Working through the following steps, you modify *ProductList.aspx* markup to display products.</span></span>

1. <span data-ttu-id="6e083-141">**ソリューション エクスプ ローラー**オープン*ProductList.aspx*します。</span><span class="sxs-lookup"><span data-stu-id="6e083-141">In **Solution Explorer**, open *ProductList.aspx*.</span></span>

2. <span data-ttu-id="6e083-142">既存のマークアップを次のマークアップに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="6e083-142">Replace the existing markup with the following markup:</span></span> 

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample1.aspx)]

<span data-ttu-id="6e083-143">上記のマークアップを使用して、 **ListView**という名前のコントロール`productList`商品を表示します。</span><span class="sxs-lookup"><span data-stu-id="6e083-143">The preceding markup uses a **ListView** control named `productList` to display products.</span></span>

[!code-aspx-csharp[Main](display_data_items_and_details/samples/sample2.aspx)]

<span data-ttu-id="6e083-144">テンプレートとスタイルを使用して定義する方法、 **ListView**コントロールにデータが表示されます。</span><span class="sxs-lookup"><span data-stu-id="6e083-144">With templates and styles, you define how the **ListView** control displays data.</span></span> <span data-ttu-id="6e083-145">任意の繰り返し構造内のデータと便利です。</span><span class="sxs-lookup"><span data-stu-id="6e083-145">It's useful for data in any repeating structure.</span></span> <span data-ttu-id="6e083-146">ただしこの**ListView**の例には、データベースのデータだけが表示されます、編集、挿入、およびデータを削除して、並べ替えやデータをページにユーザーを有効にするコードがないこともできます。</span><span class="sxs-lookup"><span data-stu-id="6e083-146">Though this **ListView** example simply displays database data, you can also, without code, enable users to edit, insert, and delete data, and to sort and page data.</span></span>

<span data-ttu-id="6e083-147">設定すると、`ItemType`プロパティ、 **ListView**制御、データ バインディング式`Item`が使用可能なコントロールが厳密に型指定されました。</span><span class="sxs-lookup"><span data-stu-id="6e083-147">When you set the `ItemType` property in the **ListView** control, the data-binding expression `Item` is available and the control becomes strongly typed.</span></span> <span data-ttu-id="6e083-148">指定するなど、IntelliSense 項目オブジェクトの詳細を選択するには前のチュートリアルで前述の`ProductName`:</span><span class="sxs-lookup"><span data-stu-id="6e083-148">As mentioned in the previous tutorial, you can select Item object details with IntelliSense, such as specifying the `ProductName`:</span></span>

![データを表示する項目と IntelliSense の詳細](display_data_items_and_details/_static/image1.png)

<span data-ttu-id="6e083-150">モデル バインディングで指定する、`SelectMethod`値 (`GetProducts`)。</span><span class="sxs-lookup"><span data-stu-id="6e083-150">With model binding, you're specifying a `SelectMethod` value (`GetProducts`).</span></span> <span data-ttu-id="6e083-151">これは、コードに追加する方法を次の手順で製品を表示する遅れています。</span><span class="sxs-lookup"><span data-stu-id="6e083-151">This is the method you add to the code behind to display products in the next step.</span></span>

### <a name="add-code-to-display-products"></a><span data-ttu-id="6e083-152">製品を表示するコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="6e083-152">Add code to display products</span></span>

<span data-ttu-id="6e083-153">この手順では、データを読み込むコードを追加、 **ListView**データベース製品のデータを制御します。</span><span class="sxs-lookup"><span data-stu-id="6e083-153">In this step, you add code to populate the **ListView** control with database product data.</span></span> <span data-ttu-id="6e083-154">コードでは、すべての製品と個々 のカテゴリの製品を表示をサポートします。</span><span class="sxs-lookup"><span data-stu-id="6e083-154">The code supports showing all products and individual category products.</span></span>

1. <span data-ttu-id="6e083-155">**ソリューション エクスプ ローラー**を右クリックして*ProductList.aspx*選び**コードの表示**します。</span><span class="sxs-lookup"><span data-stu-id="6e083-155">In **Solution Explorer**, right-click *ProductList.aspx* and then select **View Code**.</span></span>
2. <span data-ttu-id="6e083-156">既存のコードを置き換える、 *ProductList.aspx.cs*このファイル。</span><span class="sxs-lookup"><span data-stu-id="6e083-156">Replace the existing code in the *ProductList.aspx.cs* file with this:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

<span data-ttu-id="6e083-157">このコードを示しています、`GetProducts`メソッドを**ListView**コントロールの`ItemType`内のプロパティ参照*ProductList.aspx*します。</span><span class="sxs-lookup"><span data-stu-id="6e083-157">This code shows the `GetProducts` method that the **ListView** control's `ItemType` property references in *ProductList.aspx*.</span></span> <span data-ttu-id="6e083-158">コードの設定を特定のデータベース カテゴリに結果を制限するため、`categoryId`に渡されるクエリ文字列の値を*ProductList.aspx*します。</span><span class="sxs-lookup"><span data-stu-id="6e083-158">To limit the results to a specific database category, the code sets the `categoryId` value from the query string passed to *ProductList.aspx*.</span></span> <span data-ttu-id="6e083-159">`QueryStringAttribute`クラス、`System.Web.ModelBinding`名前空間を使用して、クエリ文字列変数を取得`id`の値します。</span><span class="sxs-lookup"><span data-stu-id="6e083-159">The `QueryStringAttribute` class in the `System.Web.ModelBinding` namespace is used to retrieve the query string variable `id`'s value.</span></span> <span data-ttu-id="6e083-160">これにより、実行時に、クエリ文字列値をバインドするモデル バインド、`categoryId`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="6e083-160">This instructs model binding to, at run time, bind a query string value to the `categoryId` parameter.</span></span>

<span data-ttu-id="6e083-161">ときに有効なカテゴリ (`categoryId`) が渡されると、結果はそのカテゴリのデータベース製品に制限されています。</span><span class="sxs-lookup"><span data-stu-id="6e083-161">When a valid category (`categoryId`) is passed, the results are limited to that category's database products.</span></span> <span data-ttu-id="6e083-162">たとえば場合、 *ProductsList.aspx*はページの URL:</span><span class="sxs-lookup"><span data-stu-id="6e083-162">For instance, if the *ProductsList.aspx* page URL is this:</span></span>

[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

<span data-ttu-id="6e083-163">ページには、製品のみが表示されます。 場所、 `categoryId` equals`1`します。</span><span class="sxs-lookup"><span data-stu-id="6e083-163">The page displays only the products where the `categoryId` equals `1`.</span></span>

<span data-ttu-id="6e083-164">クエリ文字列が渡されない場合は、すべての製品が表示されます。</span><span class="sxs-lookup"><span data-stu-id="6e083-164">All products are displayed if no query string is passed.</span></span>

<span data-ttu-id="6e083-165">これらのメソッドの値のソースと呼びます*値プロバイダー* (など`QueryString`)、および使用する値プロバイダーを示すパラメーターの属性と呼びます*プロバイダー属性の値*(など`id`)。</span><span class="sxs-lookup"><span data-stu-id="6e083-165">The value sources for these methods are called *value providers* (such as `QueryString`), and the parameter attributes that indicate which value provider to use are called *value provider attributes* (such as `id`).</span></span> <span data-ttu-id="6e083-166">ASP.NET には、値プロバイダーとのすべての一般的な Web フォーム アプリケーションのユーザー入力ソースの属性が含まれています。</span><span class="sxs-lookup"><span data-stu-id="6e083-166">ASP.NET includes value providers and attributes for all typical Web Forms application user input sources.</span></span> <span data-ttu-id="6e083-167">クエリ文字列、cookie、フォーム値、コントロール、ビュー ステート、セッション状態、およびプロファイルのプロパティが含まれます。</span><span class="sxs-lookup"><span data-stu-id="6e083-167">These include the query string, cookies, form values, controls, view state, session state, and profile properties.</span></span> <span data-ttu-id="6e083-168">カスタム値プロバイダーを記述することもできます。</span><span class="sxs-lookup"><span data-stu-id="6e083-168">You can also write custom value providers.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="6e083-169">アプリケーションの実行</span><span class="sxs-lookup"><span data-stu-id="6e083-169">Run the application</span></span>

<span data-ttu-id="6e083-170">すべての製品またはカテゴリの製品を表示するアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="6e083-170">Run the application now to view all products or a category's products.</span></span>

1. <span data-ttu-id="6e083-171">Visual Studio で、キーを押して**F5**アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="6e083-171">In Visual Studio, press **F5** to run the application.</span></span>
 <span data-ttu-id="6e083-172">ブラウザーが開きを示しています、 *Default.aspx*ページ。</span><span class="sxs-lookup"><span data-stu-id="6e083-172">The browser opens and shows the *Default.aspx* page.</span></span>

2. <span data-ttu-id="6e083-173">製品カテゴリ メニューの**自動車**します。</span><span class="sxs-lookup"><span data-stu-id="6e083-173">From the product category menu, select **Cars**.</span></span>

   <span data-ttu-id="6e083-174">*ProductList.aspx*から製品のみが表示されたページが表示されます、**自動車**カテゴリ。</span><span class="sxs-lookup"><span data-stu-id="6e083-174">The *ProductList.aspx* page appears, showing only products from the **Cars** category.</span></span> <span data-ttu-id="6e083-175">このチュートリアルの後半では、製品の詳細を表示します。</span><span class="sxs-lookup"><span data-stu-id="6e083-175">Later in this tutorial, you display product details.</span></span>

    ![データを表示する項目と詳細 - 自動車](display_data_items_and_details/_static/image2.png)

3. <span data-ttu-id="6e083-177">選択**製品**上部のメニューから。</span><span class="sxs-lookup"><span data-stu-id="6e083-177">Select **Products** from the top menu.</span></span>
 <span data-ttu-id="6e083-178">*ProductList.aspx*ページには、すべての製品が表示されます。</span><span class="sxs-lookup"><span data-stu-id="6e083-178">The *ProductList.aspx* page now displays all products.</span></span> 

    ![データを表示する項目と製品の詳細](display_data_items_and_details/_static/image3.png)

4. <span data-ttu-id="6e083-180">ブラウザーを閉じて、Visual Studio に戻ります。</span><span class="sxs-lookup"><span data-stu-id="6e083-180">Close the browser and return to Visual Studio.</span></span>

### <a name="add-a-data-control-to-display-product-details"></a><span data-ttu-id="6e083-181">製品の詳細を表示するデータ コントロールを追加します。</span><span class="sxs-lookup"><span data-stu-id="6e083-181">Add a Data Control to display product details</span></span>

<span data-ttu-id="6e083-182">変更、 *ProductDetails.aspx*特定の製品情報を表示する前のチュートリアルで追加したマークアップ。</span><span class="sxs-lookup"><span data-stu-id="6e083-182">Modify the *ProductDetails.aspx* markup that you added in the previous tutorial to display specific product information:</span></span>

1. <span data-ttu-id="6e083-183">**ソリューション エクスプ ローラー**オープン*ProductDetails.aspx*します。</span><span class="sxs-lookup"><span data-stu-id="6e083-183">In **Solution Explorer**, open *ProductDetails.aspx*.</span></span>

2. <span data-ttu-id="6e083-184">このマークアップで既存のマークアップに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="6e083-184">Replace the existing markup with this markup:</span></span>

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample5.aspx)] 

<span data-ttu-id="6e083-185">このマークアップを使用して、 **FormView**特定の製品の詳細を表示するコントロール。</span><span class="sxs-lookup"><span data-stu-id="6e083-185">This markup uses a **FormView** control to display specific product details.</span></span> <span data-ttu-id="6e083-186">データを表示するのに使用されるものなどのメソッドを使用して*ProductList.aspx*します。</span><span class="sxs-lookup"><span data-stu-id="6e083-186">It uses methods like those used to display data in *ProductList.aspx*.</span></span> <span data-ttu-id="6e083-187">**FormView**コントロールを使用すると、データ ソースから一度に 1 つのレコードを表示します。</span><span class="sxs-lookup"><span data-stu-id="6e083-187">The **FormView** control is used to display a single record at a time from a data source.</span></span> <span data-ttu-id="6e083-188">使用すると、 **FormView**コントロール、データ バインドされた値を表示および編集テンプレートを作成します。</span><span class="sxs-lookup"><span data-stu-id="6e083-188">When you use the **FormView** control, you create templates to display and edit data-bound values.</span></span> <span data-ttu-id="6e083-189">これらのテンプレートは、バインド式のコントロールを含めるし、フォームの外観と機能を定義する書式設定します。</span><span class="sxs-lookup"><span data-stu-id="6e083-189">These templates contain controls, binding expressions, and formatting that define the form's look and functionality.</span></span>

<span data-ttu-id="6e083-190">前のマークアップをデータベースに接続するには、コードを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="6e083-190">Connecting the previous markup to the database requires additional code.</span></span>

1. <span data-ttu-id="6e083-191">**ソリューション エクスプ ローラー**を右クリックして*ProductDetails.aspx*選び**コードの表示**します。</span><span class="sxs-lookup"><span data-stu-id="6e083-191">In **Solution Explorer**, right-click *ProductDetails.aspx* and then select **View Code**.</span></span>  
   <span data-ttu-id="6e083-192">*ProductDetails.aspx.cs*ファイルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="6e083-192">The *ProductDetails.aspx.cs* file is displayed.</span></span>

2. <span data-ttu-id="6e083-193">これで、既存のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="6e083-193">Replace the existing code with this:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

<span data-ttu-id="6e083-194">このコードをチェックする"`productID`"クエリ文字列の値。</span><span class="sxs-lookup"><span data-stu-id="6e083-194">This code checks for a "`productID`" query string value.</span></span> <span data-ttu-id="6e083-195">有効な値が見つかった場合は、一致する製品が表示されます。</span><span class="sxs-lookup"><span data-stu-id="6e083-195">If a valid value is found, the matching product is displayed.</span></span> <span data-ttu-id="6e083-196">クエリ文字列が見つからない、またはその値が無効です、製品は表示されません。</span><span class="sxs-lookup"><span data-stu-id="6e083-196">If the query string isn't found, or its value isn't valid, no product is displayed.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="6e083-197">アプリケーションの実行</span><span class="sxs-lookup"><span data-stu-id="6e083-197">Run the application</span></span>

<span data-ttu-id="6e083-198">製品 ID に基づいて特定の製品の詳細を表示するアプリケーションを実行するようになりました</span><span class="sxs-lookup"><span data-stu-id="6e083-198">Now you can run the application to see specific product details based on product ID.</span></span>

1. <span data-ttu-id="6e083-199">Visual Studio で、キーを押して**F5**アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="6e083-199">In Visual Studio, press **F5** to run the application.</span></span>  
 <span data-ttu-id="6e083-200">ブラウザーが開きます*Default.aspx*します。</span><span class="sxs-lookup"><span data-stu-id="6e083-200">The browser opens to *Default.aspx*.</span></span>

2. <span data-ttu-id="6e083-201">[カテゴリ] メニューから選択**ボート**します。</span><span class="sxs-lookup"><span data-stu-id="6e083-201">From the category menu, select **Boats**.</span></span>  
 <span data-ttu-id="6e083-202">*ProductList.aspx*ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="6e083-202">The *ProductList.aspx* page is displayed.</span></span>

3. <span data-ttu-id="6e083-203">選択**ボートを紙**します。</span><span class="sxs-lookup"><span data-stu-id="6e083-203">Select **Paper Boat**.</span></span>  
 <span data-ttu-id="6e083-204">*ProductDetails.aspx*ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="6e083-204">The *ProductDetails.aspx* page is displayed.</span></span>   

    ![データを表示する項目と製品の詳細](display_data_items_and_details/_static/image4.png)

<span data-ttu-id="6e083-206">次のチュートリアルでは、ショッピング カートを Wingtip Toys アプリケーションに追加します。</span><span class="sxs-lookup"><span data-stu-id="6e083-206">In the next tutorial, you add a shopping cart to the Wingtip Toys application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6e083-207">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="6e083-207">Additional resources</span></span>

[<span data-ttu-id="6e083-208">取得して、モデル バインディングと web フォームでデータの表示</span><span class="sxs-lookup"><span data-stu-id="6e083-208">Retrieving and displaying data with model binding and web forms</span></span>](../../presenting-and-managing-data/model-binding/retrieving-data.md)

> [!div class="step-by-step"]
> <span data-ttu-id="6e083-209">[前へ](ui_and_navigation.md)
> [次へ](shopping-cart.md)</span><span class="sxs-lookup"><span data-stu-id="6e083-209">[Previous](ui_and_navigation.md)
[Next](shopping-cart.md)</span></span>
