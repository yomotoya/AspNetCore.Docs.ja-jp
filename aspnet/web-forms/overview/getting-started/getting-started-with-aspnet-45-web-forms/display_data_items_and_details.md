---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: 項目し、詳細のデータの表示 |Microsoft Docs
author: Erikre
description: このチュートリアル シリーズでは、私たちの ASP.NET 4.5 と Microsoft Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: 08efafc1ae076bcf481c5c7d8aea2bbaa704af17
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37379739"
---
<a name="display-data-items-and-details"></a><span data-ttu-id="a2f98-103">項目し、詳細のデータを表示</span><span class="sxs-lookup"><span data-stu-id="a2f98-103">Display Data Items and Details</span></span>
====================
<span data-ttu-id="a2f98-104">によって[Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="a2f98-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="a2f98-105">[Wingtip Toys のサンプル プロジェクト (c#) をダウンロード](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)または[電子書籍 (PDF) をダウンロード](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="a2f98-105">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

> <span data-ttu-id="a2f98-106">このチュートリアル シリーズでは、Web 用 ASP.NET 4.5 と Microsoft Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明します。</span><span class="sxs-lookup"><span data-stu-id="a2f98-106">This tutorial series will teach you the basics of building an ASP.NET Web Forms application using ASP.NET 4.5 and Microsoft Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="a2f98-107">Visual Studio 2013[プロジェクトと c# ソース コード](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)このチュートリアル シリーズをと共に使用できます。</span><span class="sxs-lookup"><span data-stu-id="a2f98-107">A Visual Studio 2013 [project with C# source code](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) is available to accompany this tutorial series.</span></span>


<span data-ttu-id="a2f98-108">このチュートリアルでは、データ項目と ASP.NET Web フォームと Entity Framework Code First を使用してデータ項目の詳細を表示する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="a2f98-108">This tutorial describes how to display data items and data item details using ASP.NET Web Forms and Entity Framework Code First.</span></span> <span data-ttu-id="a2f98-109">このチュートリアルでは、前のチュートリアルでは、「UI とナビゲーション」で作成し、、Wingtip 玩具店のチュートリアル シリーズの一部です。</span><span class="sxs-lookup"><span data-stu-id="a2f98-109">This tutorial builds on the previous tutorial "UI and Navigation" and is part of the Wingtip Toy Store tutorial series.</span></span> <span data-ttu-id="a2f98-110">このチュートリアルを完了すると、ときにの製品を表示するができます、 *ProductsList.aspx*ページとに個別の製品に関する情報を*ProductDetails.aspx*ページ。</span><span class="sxs-lookup"><span data-stu-id="a2f98-110">When you've completed this tutorial, you'll be able to see products on the *ProductsList.aspx* page and details about an individual product on the *ProductDetails.aspx* page.</span></span>

## <a name="what-youll-learn"></a><span data-ttu-id="a2f98-111">学習内容。</span><span class="sxs-lookup"><span data-stu-id="a2f98-111">What you'll learn:</span></span>

- <span data-ttu-id="a2f98-112">データベースから製品を表示するデータ コントロールを追加する方法。</span><span class="sxs-lookup"><span data-stu-id="a2f98-112">How to add a data control to display products from the database.</span></span>
- <span data-ttu-id="a2f98-113">データ コントロールを選択したデータに接続する方法。</span><span class="sxs-lookup"><span data-stu-id="a2f98-113">How to connect a data control to the selected data.</span></span>
- <span data-ttu-id="a2f98-114">データベースから製品の詳細を表示するデータ コントロールを追加する方法。</span><span class="sxs-lookup"><span data-stu-id="a2f98-114">How to add a data control to display product details from the database.</span></span>
- <span data-ttu-id="a2f98-115">クエリ文字列から値を取得し、その値を使用して、データベースから取得したデータを制限する方法。</span><span class="sxs-lookup"><span data-stu-id="a2f98-115">How to retrieve a value from the query string and use that value to limit the data that's retrieved from the database.</span></span>

### <a name="these-are-the-features-introduced-in-the-tutorial"></a><span data-ttu-id="a2f98-116">このチュートリアルで導入された機能を次に示します。</span><span class="sxs-lookup"><span data-stu-id="a2f98-116">These are the features introduced in the tutorial:</span></span>

- <span data-ttu-id="a2f98-117">モデル バインド</span><span class="sxs-lookup"><span data-stu-id="a2f98-117">Model Binding</span></span>
- <span data-ttu-id="a2f98-118">値プロバイダー</span><span class="sxs-lookup"><span data-stu-id="a2f98-118">Value providers</span></span>

## <a name="adding-a-data-control-to-display-products"></a><span data-ttu-id="a2f98-119">製品を表示するデータ コントロールを追加します。</span><span class="sxs-lookup"><span data-stu-id="a2f98-119">Adding a Data Control to Display Products</span></span>

<span data-ttu-id="a2f98-120">サーバー コントロールにデータをバインドするときに使用できるいくつかの異なるオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="a2f98-120">When binding data to a server control, there are a few different options you can use.</span></span> <span data-ttu-id="a2f98-121">データ ソース コントロールの追加、コードを手動で追加またはモデルのバインディングを使用して、最も一般的なオプションが含まれます。</span><span class="sxs-lookup"><span data-stu-id="a2f98-121">The most common options include adding a data source control, adding code by hand, or using model binding.</span></span>

### <a name="using-a-data-source-control-to-bind-data"></a><span data-ttu-id="a2f98-122">データ ソース コントロールを使用してデータをバインドするには</span><span class="sxs-lookup"><span data-stu-id="a2f98-122">Using a Data Source Control to Bind Data</span></span>

<span data-ttu-id="a2f98-123">データ ソース コントロールの追加データを表示するコントロールにデータ ソース コントロールをリンクすることができます。</span><span class="sxs-lookup"><span data-stu-id="a2f98-123">Adding a data source control allows you to link the data source control to the control that displays the data.</span></span> <span data-ttu-id="a2f98-124">このアプローチでは、宣言によってサーバー側コントロールをプログラム的なアプローチを使用するのではなく、データ ソースに直接接続できます。</span><span class="sxs-lookup"><span data-stu-id="a2f98-124">This approach allows you to declaratively connect server-side controls directly to data sources, rather than using a programmatic approach.</span></span>

### <a name="coding-by-hand-to-bind-data"></a><span data-ttu-id="a2f98-125">データ バインドを手動でコーディング</span><span class="sxs-lookup"><span data-stu-id="a2f98-125">Coding By Hand to Bind Data</span></span>

<span data-ttu-id="a2f98-126">追加を手作業でコードが含まれて値を読み取るの null 値の確認、適切な型に変換しようとして、変換が成功したかどうかをチェックおよびクエリの最後に、値を使用しています。</span><span class="sxs-lookup"><span data-stu-id="a2f98-126">Adding code by hand involves reading the value, checking for a null value, attempting to convert it to the appropriate type, checking whether the conversion was successful, and finally, using the value in the query.</span></span> <span data-ttu-id="a2f98-127">データ アクセス ロジックを完全に制御を保持する必要がある場合は、このアプローチを使用します。</span><span class="sxs-lookup"><span data-stu-id="a2f98-127">You would use this approach when you need to retain full control over your data-access logic.</span></span>

### <a name="using-model-binding-to-bind-data"></a><span data-ttu-id="a2f98-128">モデルのデータをバインドするバインドを使用します。</span><span class="sxs-lookup"><span data-stu-id="a2f98-128">Using Model Binding to Bind Data</span></span>

<span data-ttu-id="a2f98-129">モデル バインドを使用して、はるかに少ないコードを使用して結果をバインドすることができます、使用すると、アプリケーション全体で機能を再利用できます。</span><span class="sxs-lookup"><span data-stu-id="a2f98-129">Using model binding allows you to bind results using far less code and gives you the ability to reuse the functionality throughout your application.</span></span> <span data-ttu-id="a2f98-130">モデル バインドの豊富なデータ バインディング フレームワークの利点を維持しながらコードを重視したデータ アクセス ロジックの使用を簡略化する目的です。</span><span class="sxs-lookup"><span data-stu-id="a2f98-130">Model binding aims to simplify working with code-focused data-access logic while still retaining the benefits of a rich, data-binding framework.</span></span>

## <a name="displaying-products"></a><span data-ttu-id="a2f98-131">製品を表示します。</span><span class="sxs-lookup"><span data-stu-id="a2f98-131">Displaying Products</span></span>

<span data-ttu-id="a2f98-132">このチュートリアルでは、データをバインドするのにモデル バインドを使用します。</span><span class="sxs-lookup"><span data-stu-id="a2f98-132">In this tutorial, you'll use model binding to bind data.</span></span> <span data-ttu-id="a2f98-133">モデル バインドを使用してデータを選択するデータ コントロールを構成するには、コントロールを設定する`SelectMethod`プロパティ ページのコード内のメソッドの名前にします。</span><span class="sxs-lookup"><span data-stu-id="a2f98-133">To configure a data control to use model binding to select data, you set the control's `SelectMethod` property to the name of a method in the page's code.</span></span> <span data-ttu-id="a2f98-134">データ コントロールでは、ページのライフ サイクルの適切なタイミングでメソッドを呼び出すし、自動的に返されるデータをバインドします。</span><span class="sxs-lookup"><span data-stu-id="a2f98-134">The data control calls the method at the appropriate time in the page life cycle and automatically binds the returned data.</span></span> <span data-ttu-id="a2f98-135">明示的に呼び出す必要はありません、`DataBind`メソッド。</span><span class="sxs-lookup"><span data-stu-id="a2f98-135">There's no need to explicitly call the `DataBind` method.</span></span>

<span data-ttu-id="a2f98-136">次の手順を使用して、内のマークアップを変更します、 *ProductList.aspx*ページ、ページは、製品を表示できるようにします。</span><span class="sxs-lookup"><span data-stu-id="a2f98-136">Using the steps below, you'll modify the markup in the *ProductList.aspx* page so that the page can display products.</span></span>

1. <span data-ttu-id="a2f98-137">**ソリューション エクスプ ローラー**、オープン、 *ProductList.aspx*ページ。</span><span class="sxs-lookup"><span data-stu-id="a2f98-137">In **Solution Explorer**, open the *ProductList.aspx* page.</span></span>
2. <span data-ttu-id="a2f98-138">既存のマークアップを次のマークアップに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="a2f98-138">Replace the existing markup with the following markup:</span></span>   

    [!code-aspx[Main](display_data_items_and_details/samples/sample1.aspx)]

<span data-ttu-id="a2f98-139">このコードを使用して、 **ListView**製品を表示する「[productlist]」という名前のコントロール。</span><span class="sxs-lookup"><span data-stu-id="a2f98-139">This code uses a **ListView** control named "productList" to display the products.</span></span>

[!code-aspx[Main](display_data_items_and_details/samples/sample2.aspx)]

<span data-ttu-id="a2f98-140">**ListView**コントロール テンプレートおよびスタイルを使用して定義した形式でデータを表示します。</span><span class="sxs-lookup"><span data-stu-id="a2f98-140">The **ListView** control displays data in a format that you define by using templates and styles.</span></span> <span data-ttu-id="a2f98-141">任意の繰り返し構造内のデータと便利です。</span><span class="sxs-lookup"><span data-stu-id="a2f98-141">It is useful for data in any repeating structure.</span></span> <span data-ttu-id="a2f98-142">これは、 **ListView**例には、データベース、ユーザーを編集、挿入、およびデータを削除し、並べ替えを有効にしたただしおよびコードなしですべてのページのデータからデータを単には示しています。</span><span class="sxs-lookup"><span data-stu-id="a2f98-142">This **ListView** example simply shows data from the database, however you can enable users to edit, insert, and delete data, and to sort and page data, all without code.</span></span>

<span data-ttu-id="a2f98-143">設定して、`ItemType`プロパティ、 **ListView**制御、データ バインディング式`Item`が使用可能なコントロールが厳密に型指定されました。</span><span class="sxs-lookup"><span data-stu-id="a2f98-143">By setting the `ItemType` property in the **ListView** control, the data-binding expression `Item` is available and the control becomes strongly typed.</span></span> <span data-ttu-id="a2f98-144">前のチュートリアルで説明したように、詳細を指定するなど、IntelliSense を使用してアイテム オブジェクトの選択もできます、 `ProductName`:</span><span class="sxs-lookup"><span data-stu-id="a2f98-144">As mentioned in the previous tutorial, you can select details of the Item object using IntelliSense, such as specifying the `ProductName`:</span></span>

![データを表示する項目と IntelliSense の詳細](display_data_items_and_details/_static/image1.png)

<span data-ttu-id="a2f98-146">さらに、モデル バインドを使用して指定するが、`SelectMethod`値。</span><span class="sxs-lookup"><span data-stu-id="a2f98-146">In addition, you are using model binding to specify a `SelectMethod` value.</span></span> <span data-ttu-id="a2f98-147">この値 (`GetProducts`) は、次の手順に製品を表示する分離コードを追加するメソッドに対応します。</span><span class="sxs-lookup"><span data-stu-id="a2f98-147">This value (`GetProducts`) will correspond to the method that you will add to the code behind to display products in the next step.</span></span>

### <a name="adding-code-to-display-products"></a><span data-ttu-id="a2f98-148">製品を表示するコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="a2f98-148">Adding Code to Display Products</span></span>

<span data-ttu-id="a2f98-149">この手順では、データを読み込むコードを追加します、 **ListView**コントロールに、データベースから製品データ。</span><span class="sxs-lookup"><span data-stu-id="a2f98-149">In this step, you'll add code to populate the **ListView** control with product data from the database.</span></span> <span data-ttu-id="a2f98-150">コードでは、すべての製品を示すほか、個々 のカテゴリで表示されている製品をサポートします。</span><span class="sxs-lookup"><span data-stu-id="a2f98-150">The code will support showing products by individual category, as well as showing all products.</span></span>

1. <span data-ttu-id="a2f98-151">**ソリューション エクスプ ローラー**、右クリック*ProductList.aspx*  をクリックし、**コードの表示**します。</span><span class="sxs-lookup"><span data-stu-id="a2f98-151">In **Solution Explorer**, right-click *ProductList.aspx* and then click **View Code**.</span></span>
2. <span data-ttu-id="a2f98-152">既存のコードを置き換える、 *ProductList.aspx.cs*を次のコード ファイル。</span><span class="sxs-lookup"><span data-stu-id="a2f98-152">Replace the existing code in the *ProductList.aspx.cs* file with the following code:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

<span data-ttu-id="a2f98-153">このコードを示しています、`GetProducts`によって参照されるメソッド、`ItemType`のプロパティ、 **ListView**を制御、 *ProductList.aspx*ページ。</span><span class="sxs-lookup"><span data-stu-id="a2f98-153">This code shows the `GetProducts` method that's referenced by the `ItemType` property of the **ListView** control in the *ProductList.aspx* page.</span></span> <span data-ttu-id="a2f98-154">コードの設定、データベース内の特定のカテゴリに結果を制限するため、`categoryId`に渡されるクエリ文字列値から、 *ProductList.aspx*ときにページ、 *ProductList.aspx*ページは、移動します。</span><span class="sxs-lookup"><span data-stu-id="a2f98-154">To limit the results to a specific category in the database, the code sets the `categoryId` value from the query string value passed to the *ProductList.aspx* page when the *ProductList.aspx* page is navigated to.</span></span> <span data-ttu-id="a2f98-155">`QueryStringAttribute`クラス、`System.Web.ModelBinding`名前空間を使用して、クエリ文字列変数 id の値を取得します。これに指示するクエリ文字列から値をバインドしようとするモデル バインド、`categoryId`実行時にパラメーター。</span><span class="sxs-lookup"><span data-stu-id="a2f98-155">The `QueryStringAttribute` class in the `System.Web.ModelBinding` namespace is used to retrieve the value of the query string variable id. This instructs model binding to try to bind a value from the query string to the `categoryId` parameter at run time.</span></span>

<span data-ttu-id="a2f98-156">クエリの結果に一致するデータベース内のこれらの製品に制限されています ページにクエリ文字列として有効なカテゴリが渡される、`categoryId`値。</span><span class="sxs-lookup"><span data-stu-id="a2f98-156">When a valid category is passed as a query string to the page, the results of the query are limited to those products in the database that match the `categoryId` value.</span></span> <span data-ttu-id="a2f98-157">たとえば場合の URL、 *ProductsList.aspx*ページは、次。</span><span class="sxs-lookup"><span data-stu-id="a2f98-157">For instance, if the URL to the *ProductsList.aspx* page is the following:</span></span>

[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

<span data-ttu-id="a2f98-158">ページには、製品のみが表示されます。 場所、 `category` equals`1`します。</span><span class="sxs-lookup"><span data-stu-id="a2f98-158">The page displays only the products where the `category` equals `1`.</span></span>

<span data-ttu-id="a2f98-159">クエリ文字列が含まれていないに移動すると場合、 *ProductList.aspx*  ページで、すべての製品が表示されます。</span><span class="sxs-lookup"><span data-stu-id="a2f98-159">If no query string is included when navigating to the *ProductList.aspx* page, all products will be displayed.</span></span>

<span data-ttu-id="a2f98-160">これらのメソッドの値のソースとして参照されます*値プロバイダー* (など*QueryString*)、使用する値プロバイダーを示すパラメーターの属性値として参照されますプロバイダーの属性 (など"`id`")。</span><span class="sxs-lookup"><span data-stu-id="a2f98-160">The sources of values for these methods are referred to as *value providers* (such as *QueryString*), and the parameter attributes that indicate which value provider to use are referred to as value provider attributes (such as "`id`").</span></span> <span data-ttu-id="a2f98-161">ASP.NET には、クエリ文字列、cookie、フォーム値、コントロール、ビュー ステート、セッション状態、およびプロファイルのプロパティなどの Web フォーム アプリケーションでの値プロバイダーとすべてのユーザー入力の一般的なソースの対応する属性が含まれています。</span><span class="sxs-lookup"><span data-stu-id="a2f98-161">ASP.NET includes value providers and corresponding attributes for all of the typical sources of user input in a Web Forms application, such as the query string, cookies, form values, controls, view state, session state, and profile properties.</span></span> <span data-ttu-id="a2f98-162">カスタム値プロバイダーを記述することもできます。</span><span class="sxs-lookup"><span data-stu-id="a2f98-162">You can also write custom value providers.</span></span>

### <a name="running-the-application"></a><span data-ttu-id="a2f98-163">アプリケーションの実行</span><span class="sxs-lookup"><span data-stu-id="a2f98-163">Running the Application</span></span>

<span data-ttu-id="a2f98-164">すべての製品または製品カテゴリによって制限のセットだけを表示する方法を表示するアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="a2f98-164">Run the application now to see how you can view all of the products or just a set of products limited by category.</span></span>

1. <span data-ttu-id="a2f98-165">**ソリューション エクスプ ローラー**を右クリックし、 *Default.aspx*  ページ**ブラウザーで表示**します。</span><span class="sxs-lookup"><span data-stu-id="a2f98-165">In the **Solution Explorer**, right-click the *Default.aspx* page and select **View in Browser**.</span></span>  
 <span data-ttu-id="a2f98-166">ブラウザーが開き、表示、 *Default.aspx*ページ。</span><span class="sxs-lookup"><span data-stu-id="a2f98-166">The browser will open and show the *Default.aspx* page.</span></span>
2. <span data-ttu-id="a2f98-167">選択**自動車**製品カテゴリのナビゲーション メニュー。</span><span class="sxs-lookup"><span data-stu-id="a2f98-167">Select **Cars** from the product category navigation menu.</span></span>  
 <span data-ttu-id="a2f98-168">*ProductList.aspx* "Cars"カテゴリに含まれる製品のみを示すページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="a2f98-168">The *ProductList.aspx* page is displayed showing only products included in the "Cars" category.</span></span> <span data-ttu-id="a2f98-169">このチュートリアルで後で製品の詳細が表示されます。</span><span class="sxs-lookup"><span data-stu-id="a2f98-169">Later in this tutorial, you will display product details.</span></span>  

    ![データを表示する項目と詳細 - 自動車](display_data_items_and_details/_static/image2.png)
3. <span data-ttu-id="a2f98-171">選択**製品**上部にあるナビゲーション メニュー。</span><span class="sxs-lookup"><span data-stu-id="a2f98-171">Select **Products** from the navigation menu at the top.</span></span>  
 <span data-ttu-id="a2f98-172">もう一度、 *ProductList.aspx*ページが表示されますが、今度は製品の全リストが表示されます。</span><span class="sxs-lookup"><span data-stu-id="a2f98-172">Again, the *ProductList.aspx* page is displayed, however this time it shows the entire list of products.</span></span>   

    ![データを表示する項目と製品の詳細](display_data_items_and_details/_static/image3.png)
4. <span data-ttu-id="a2f98-174">ブラウザーを閉じて、Visual Studio に戻ります。</span><span class="sxs-lookup"><span data-stu-id="a2f98-174">Close the browser and return to Visual Studio.</span></span>

### <a name="adding-a-data-control-to-display-product-details"></a><span data-ttu-id="a2f98-175">製品の詳細を表示するデータ コントロールを追加します。</span><span class="sxs-lookup"><span data-stu-id="a2f98-175">Adding a Data Control to Display Product Details</span></span>

<span data-ttu-id="a2f98-176">内のマークアップを次に、変更を*ProductDetails.aspx*ページは、個々 の製品についての情報を表示できるように、前のチュートリアルで追加したページです。</span><span class="sxs-lookup"><span data-stu-id="a2f98-176">Next, you'll modify the markup in the *ProductDetails.aspx* page that you added in the previous tutorial so that the page can display information about an individual product.</span></span>

1. <span data-ttu-id="a2f98-177">**ソリューション エクスプ ローラー**、オープン、 *ProductDetails.aspx*ページ。</span><span class="sxs-lookup"><span data-stu-id="a2f98-177">In **Solution Explorer**, open the *ProductDetails.aspx* page.</span></span>
2. <span data-ttu-id="a2f98-178">既存のマークアップを次のマークアップに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="a2f98-178">Replace the existing markup with the following markup:</span></span>   

    [!code-aspx[Main](display_data_items_and_details/samples/sample5.aspx)]

<span data-ttu-id="a2f98-179">このコードを使用して、 **FormView**個別の製品についての詳細を表示するコントロール。</span><span class="sxs-lookup"><span data-stu-id="a2f98-179">This code uses a **FormView** control to display details about an individual product.</span></span> <span data-ttu-id="a2f98-180">このマークアップは、データを表示するために使用するようなメソッドを使用して、 *ProductList.aspx*ページ。</span><span class="sxs-lookup"><span data-stu-id="a2f98-180">This markup uses methods like those that are used to display data in the *ProductList.aspx* page.</span></span> <span data-ttu-id="a2f98-181">**FormView**コントロールを使用すると、データ ソースから一度に 1 つのレコードを表示します。</span><span class="sxs-lookup"><span data-stu-id="a2f98-181">The **FormView** control is used to display a single record at a time from a data source.</span></span> <span data-ttu-id="a2f98-182">使用すると、 **FormView**コントロール、データ バインドされた値を表示および編集テンプレートを作成します。</span><span class="sxs-lookup"><span data-stu-id="a2f98-182">When you use the **FormView** control, you create templates to display and edit data-bound values.</span></span> <span data-ttu-id="a2f98-183">テンプレートには、コントロールに、バインド式と書式設定をフォームの機能と外観を定義が含まれています。</span><span class="sxs-lookup"><span data-stu-id="a2f98-183">The templates contain controls, binding expressions, and formatting that define the look and functionality of the form.</span></span>

<span data-ttu-id="a2f98-184">上記のマークアップをデータベースに接続するにコードを追加する必要があります、 *ProductDetails.aspx*コード。</span><span class="sxs-lookup"><span data-stu-id="a2f98-184">To connect the above markup to the database, you must add additional code to the *ProductDetails.aspx* code.</span></span>

1. <span data-ttu-id="a2f98-185">**ソリューション エクスプ ローラー**、右クリック*ProductDetails.aspx*  をクリックし、**コードの表示**します。</span><span class="sxs-lookup"><span data-stu-id="a2f98-185">In **Solution Explorer**, right-click *ProductDetails.aspx* and then click **View Code**.</span></span>  
   <span data-ttu-id="a2f98-186">*ProductDetails.aspx.cs*ファイルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="a2f98-186">The *ProductDetails.aspx.cs* file will be displayed.</span></span>
2. <span data-ttu-id="a2f98-187">既存のコードを次のコードで置換します。</span><span class="sxs-lookup"><span data-stu-id="a2f98-187">Replace the existing code with the following code:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

<span data-ttu-id="a2f98-188">このコードをチェックする"`productID`"クエリ文字列値。</span><span class="sxs-lookup"><span data-stu-id="a2f98-188">This code checks for a "`productID`" query-string value.</span></span> <span data-ttu-id="a2f98-189">有効なクエリ文字列値が見つかった場合は、一致する製品が表示されます。</span><span class="sxs-lookup"><span data-stu-id="a2f98-189">If a valid query-string value is found, the matching product is displayed.</span></span> <span data-ttu-id="a2f98-190">製品が表示されない場合、クエリ文字列が見つからないか、クエリ文字列値が無効です、 *ProductDetails.aspx*ページ。</span><span class="sxs-lookup"><span data-stu-id="a2f98-190">If no query-string is found, or the query-string value is not valid, no product is displayed on the *ProductDetails.aspx* page.</span></span>

### <a name="running-the-application"></a><span data-ttu-id="a2f98-191">アプリケーションの実行</span><span class="sxs-lookup"><span data-stu-id="a2f98-191">Running the Application</span></span>

<span data-ttu-id="a2f98-192">これでアプリケーションが表示される個別の製品を実行する、製品の id に基づいています。</span><span class="sxs-lookup"><span data-stu-id="a2f98-192">Now you can run the application to see an individual product displayed based on the id of the product.</span></span>

1. <span data-ttu-id="a2f98-193">キーを押して**F5** Visual Studio でアプリケーションを実行するときにします。</span><span class="sxs-lookup"><span data-stu-id="a2f98-193">Press **F5** while in Visual Studio to run the application.</span></span>  
 <span data-ttu-id="a2f98-194">ブラウザーが開き、表示、 *Default.aspx*ページ。</span><span class="sxs-lookup"><span data-stu-id="a2f98-194">The browser will open and show the *Default.aspx* page.</span></span>
2. <span data-ttu-id="a2f98-195">カテゴリのナビゲーション メニューから「ボート」を選択します。</span><span class="sxs-lookup"><span data-stu-id="a2f98-195">Select "Boats" from the category navigation menu.</span></span>  
 <span data-ttu-id="a2f98-196">*ProductList.aspx*ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="a2f98-196">The *ProductList.aspx* page is displayed.</span></span>
3. <span data-ttu-id="a2f98-197">製品の一覧から「用紙ボート」製品を選択します。</span><span class="sxs-lookup"><span data-stu-id="a2f98-197">Select the "Paper Boat" product from the product list.</span></span>  
 <span data-ttu-id="a2f98-198">*ProductDetails.aspx*ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="a2f98-198">The *ProductDetails.aspx* page is displayed.</span></span>   

    ![データを表示する項目と製品の詳細](display_data_items_and_details/_static/image4.png)
4. <span data-ttu-id="a2f98-200">ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="a2f98-200">Close the browser.</span></span>

## <a name="summary"></a><span data-ttu-id="a2f98-201">まとめ</span><span class="sxs-lookup"><span data-stu-id="a2f98-201">Summary</span></span>

<span data-ttu-id="a2f98-202">シリーズのこのチュートリアルでが、マークアップと製品一覧を表示して、製品の詳細を表示するコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="a2f98-202">In this tutorial of the series you have add markup and code to display a product list and to display product details.</span></span> <span data-ttu-id="a2f98-203">このプロセス中には、厳密に型指定されたデータ コントロール、モデル バインド、および値プロバイダーについて説明しました。</span><span class="sxs-lookup"><span data-stu-id="a2f98-203">During this process you have learned about strongly typed data controls, model binding, and value providers.</span></span> <span data-ttu-id="a2f98-204">次のチュートリアルでは、Wingtip Toys のサンプル アプリケーションをショッピング カートを追加します。</span><span class="sxs-lookup"><span data-stu-id="a2f98-204">In the next tutorial, you'll add a shopping cart to the Wingtip Toys sample application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a2f98-205">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="a2f98-205">Additional Resources</span></span>

[<span data-ttu-id="a2f98-206">取得して、モデル バインディングと web フォームでデータの表示</span><span class="sxs-lookup"><span data-stu-id="a2f98-206">Retrieving and displaying data with model binding and web forms</span></span>](../../presenting-and-managing-data/model-binding/retrieving-data.md)

> [!div class="step-by-step"]
> <span data-ttu-id="a2f98-207">[前へ](ui_and_navigation.md)
> [次へ](shopping-cart.md)</span><span class="sxs-lookup"><span data-stu-id="a2f98-207">[Previous](ui_and_navigation.md)
[Next](shopping-cart.md)</span></span>
