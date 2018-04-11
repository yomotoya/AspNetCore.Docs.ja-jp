---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
title: チェック アウトおよび PayPal の支払い |Microsoft ドキュメント
author: Erikre
description: このチュートリアルの系列では、お用 ASP.NET 4.5 と Microsoft Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 664ec95e-b0c9-4f43-a39f-798d0f2a7e08
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
msc.type: authoredcontent
ms.openlocfilehash: 0dba613594686a28b82bc6d7701cda6e24b82e2e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/10/2018
---
<a name="checkout-and-payment-with-paypal"></a><span data-ttu-id="ef6df-103">チェック アウトおよび PayPal の支払い</span><span class="sxs-lookup"><span data-stu-id="ef6df-103">Checkout and Payment with PayPal</span></span>
====================
<span data-ttu-id="ef6df-104">によって[Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="ef6df-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="ef6df-105">[Wingtip Toys のサンプル プロジェクト (c#) をダウンロード](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)または[電子書籍 (PDF) のダウンロード](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="ef6df-105">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

> <span data-ttu-id="ef6df-106">このチュートリアルの系列では、Web 用 ASP.NET 4.5 と Microsoft Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-106">This tutorial series will teach you the basics of building an ASP.NET Web Forms application using ASP.NET 4.5 and Microsoft Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="ef6df-107">Visual Studio 2013[プロジェクトと c# ソース コード](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)はこのチュートリアルのシリーズに付随する使用できます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-107">A Visual Studio 2013 [project with C# source code](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) is available to accompany this tutorial series.</span></span>


<span data-ttu-id="ef6df-108">このチュートリアルでは、Wingtip Toys サンプル アプリケーションは、ユーザーの承認、登録、PayPal を使用して支払を変更する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-108">This tutorial describes how to modify the Wingtip Toys sample application to include user authorization, registration, and payment using PayPal.</span></span> <span data-ttu-id="ef6df-109">ログオンしているユーザーだけでは、製品を購入するための承認があります。</span><span class="sxs-lookup"><span data-stu-id="ef6df-109">Only users who are logged in will have authorization to purchase products.</span></span> <span data-ttu-id="ef6df-110">既に ASP.NET 4.5 Web フォーム プロジェクト テンプレートの組み込みのユーザーの登録の機能にはでは必要なものの多くが含まれます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-110">The ASP.NET 4.5 Web Forms project template's built-in user registration functionality already includes much of what you need.</span></span> <span data-ttu-id="ef6df-111">PayPal Express のチェック アウト機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-111">You will add PayPal Express Checkout functionality.</span></span> <span data-ttu-id="ef6df-112">このチュートリアルでは、テスト環境では、実際の資金は転送されません PayPal developer を使用します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-112">In this tutorial you be using the PayPal developer testing environment, so no actual funds will be transferred.</span></span> <span data-ttu-id="ef6df-113">チュートリアルの最後に、ショッピング カート、チェック アウト] ボタンをクリックすると、PayPal のテストの web サイトにデータを転送するに追加する製品を選択して、アプリケーションをテストします。</span><span class="sxs-lookup"><span data-stu-id="ef6df-113">At the end of the tutorial, you will test the application by selecting products to add to the shopping cart, clicking the checkout button, and transferring data to the PayPal testing web site.</span></span> <span data-ttu-id="ef6df-114">PayPal テストの web サイトの配布と支払い情報を確認して、ローカルの Wingtip Toys サンプル アプリケーションを確認し、購入を完了に戻ります。</span><span class="sxs-lookup"><span data-stu-id="ef6df-114">On the PayPal testing web site, you will confirm your shipping and payment information and then return to the local Wingtip Toys sample application to confirm and complete the purchase.</span></span>

<span data-ttu-id="ef6df-115">そのアドレスのスケーラビリティとセキュリティは、オンライン ショッピングに特化したいくつかのサード パーティの支払いを経験豊富なプロセッサがあります。</span><span class="sxs-lookup"><span data-stu-id="ef6df-115">There are several experienced third-party payment processors that specialize in online shopping that address scalability and security.</span></span> <span data-ttu-id="ef6df-116">ASP.NET 開発者は、ショッピングを実装して、ソリューションを購入する前に、サード パーティの支払いソリューションを利用する利点を検討してください。</span><span class="sxs-lookup"><span data-stu-id="ef6df-116">ASP.NET developers should consider the advantages of utilizing a third party payment solution before implementing a shopping and purchasing solution.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="ef6df-117">Wingtip Toys のサンプル アプリケーションは、ASP.NET web 開発者に特定の ASP.NET 概念と使用可能な機能を示すために設計されました。</span><span class="sxs-lookup"><span data-stu-id="ef6df-117">The Wingtip Toys sample application was designed to shown specific ASP.NET concepts and features available to ASP.NET web developers.</span></span> <span data-ttu-id="ef6df-118">このサンプル アプリケーションは、スケーラビリティ、およびセキュリティに関してどのような状況が最適化されません。</span><span class="sxs-lookup"><span data-stu-id="ef6df-118">This sample application was not optimized for all possible circumstances in regard to scalability and security.</span></span>


## <a name="what-youll-learn"></a><span data-ttu-id="ef6df-119">学習する内容。</span><span class="sxs-lookup"><span data-stu-id="ef6df-119">What you'll learn:</span></span>

- <span data-ttu-id="ef6df-120">フォルダー内の特定のページへのアクセスを制限する方法。</span><span class="sxs-lookup"><span data-stu-id="ef6df-120">How to restrict access to specific pages in a folder.</span></span>
- <span data-ttu-id="ef6df-121">匿名のショッピング カートから既知のショッピング カートを作成する方法です。</span><span class="sxs-lookup"><span data-stu-id="ef6df-121">How to create a known shopping cart from an anonymous shopping cart.</span></span>
- <span data-ttu-id="ef6df-122">プロジェクトの SSL を有効にする方法です。</span><span class="sxs-lookup"><span data-stu-id="ef6df-122">How to enable SSL for the project.</span></span>
- <span data-ttu-id="ef6df-123">OAuth プロバイダーをプロジェクトに追加する方法。</span><span class="sxs-lookup"><span data-stu-id="ef6df-123">How to add an OAuth provider to the project.</span></span>
- <span data-ttu-id="ef6df-124">PayPal を使用して、PayPal のテスト環境を使用して製品を購入する方法です。</span><span class="sxs-lookup"><span data-stu-id="ef6df-124">How to use PayPal to purchase products using the PayPal testing environment.</span></span>
- <span data-ttu-id="ef6df-125">PayPal から詳細を表示する方法、 **DetailsView**コントロール。</span><span class="sxs-lookup"><span data-stu-id="ef6df-125">How to display details from PayPal in a **DetailsView** control.</span></span>
- <span data-ttu-id="ef6df-126">PayPal から取得の詳細で、Wingtip Toys アプリケーションのデータベースを更新する方法。</span><span class="sxs-lookup"><span data-stu-id="ef6df-126">How to update the database of the Wingtip Toys application with details obtained from PayPal.</span></span>

## <a name="adding-order-tracking"></a><span data-ttu-id="ef6df-127">注文の追跡を追加します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-127">Adding Order Tracking</span></span>

<span data-ttu-id="ef6df-128">このチュートリアルでは、ユーザーが作成される、注文からデータを追跡するために 2 つの新しいクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-128">In this tutorial, you'll create two new classes to track data from the order a user has created.</span></span> <span data-ttu-id="ef6df-129">クラスは、出荷情報、注文書合計、および支払いの確認に関するデータを追跡します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-129">The classes will track data regarding shipping information, purchase total, and payment confirmation.</span></span>

### <a name="add-the-order-and-orderdetail-model-classes"></a><span data-ttu-id="ef6df-130">順序と OrderDetail モデル クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-130">Add the Order and OrderDetail Model Classes</span></span>

<span data-ttu-id="ef6df-131">このチュートリアル シリーズの前半のカテゴリ、製品、スキーマを定義し、ショッピング カートのアイテムを作成することで、 `Category`、 `Product`、および`CartItem`内のクラス、*モデル*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="ef6df-131">Earlier in this tutorial series, you defined the schema for categories, products, and shopping cart items by creating the `Category`, `Product`, and `CartItem` classes in the *Models* folder.</span></span> <span data-ttu-id="ef6df-132">製品の注文と注文の詳細のスキーマを定義する 2 つの新しいクラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-132">Now you will add two new classes to define the schema for the product order and the details of the order.</span></span>

1. <span data-ttu-id="ef6df-133">**モデル**フォルダー、という名前の新しいクラスを追加*Order.cs*です。</span><span class="sxs-lookup"><span data-stu-id="ef6df-133">In the **Models** folder, add a new class named *Order.cs*.</span></span>   
   <span data-ttu-id="ef6df-134">新しいクラス ファイルがエディターで表示されます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-134">The new class file is displayed in the editor.</span></span>
2. <span data-ttu-id="ef6df-135">次のように、既定のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-135">Replace the default code with the following:</span></span>   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample1.cs)]
3. <span data-ttu-id="ef6df-136">追加、 *OrderDetail.cs*クラスを*モデル*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="ef6df-136">Add an *OrderDetail.cs* class to the *Models* folder.</span></span>
4. <span data-ttu-id="ef6df-137">既定のコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-137">Replace the default code with the following code:</span></span>   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample2.cs)]

<span data-ttu-id="ef6df-138">`Order`と`OrderDetail`クラスには、購入および配布に使用される順序情報を定義するスキーマが含まれています。</span><span class="sxs-lookup"><span data-stu-id="ef6df-138">The `Order` and `OrderDetail` classes contain the schema to define the order information used for purchasing and shipping.</span></span>

<span data-ttu-id="ef6df-139">さらに、エンティティ クラスを管理して、データベースへのデータ アクセスを提供するデータベース コンテキスト クラスを更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ef6df-139">In addition, you will need to update the database context class that manages the entity classes and that provides data access to the database.</span></span> <span data-ttu-id="ef6df-140">新しく作成された順序を追加すると`OrderDetail`モデル クラスを`ProductContext`クラスです。</span><span class="sxs-lookup"><span data-stu-id="ef6df-140">To do this, you will add the newly created Order and `OrderDetail` model classes to `ProductContext` class.</span></span>

1. <span data-ttu-id="ef6df-141">**ソリューション エクスプ ローラー**を検索して開く、 *ProductContext.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="ef6df-141">In **Solution Explorer**, find and open the *ProductContext.cs* file.</span></span>
2. <span data-ttu-id="ef6df-142">強調表示されたコードを追加、 *ProductContext.cs*ファイルの次のようにします。</span><span class="sxs-lookup"><span data-stu-id="ef6df-142">Add the highlighted code to the *ProductContext.cs* file as shown below:</span></span>   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample3.cs?highlight=14-15)]

<span data-ttu-id="ef6df-143">このチュートリアル シリーズのコードで説明したように、 *ProductContext.cs*ファイルを追加、`System.Data.Entity`名前空間、Entity Framework のすべてのコア機能にアクセスできるようにします。</span><span class="sxs-lookup"><span data-stu-id="ef6df-143">As mentioned previously in this tutorial series, the code in the *ProductContext.cs* file adds the `System.Data.Entity` namespace so that you have access to all the core functionality of the Entity Framework.</span></span> <span data-ttu-id="ef6df-144">この機能には、クエリ、挿入、更新、および厳密に型指定されたオブジェクトを使用してデータを削除する機能が含まれています。</span><span class="sxs-lookup"><span data-stu-id="ef6df-144">This functionality includes the capability to query, insert, update, and delete data by working with strongly typed objects.</span></span> <span data-ttu-id="ef6df-145">上記のコードで、`ProductContext`クラスでは、Entity Framework へのアクセス権を追加、新たに追加する`Order`と`OrderDetail`クラスです。</span><span class="sxs-lookup"><span data-stu-id="ef6df-145">The above code in the `ProductContext` class adds Entity Framework access to the newly added `Order` and `OrderDetail` classes.</span></span>

## <a name="adding-checkout-access"></a><span data-ttu-id="ef6df-146">チェック アウトのアクセスを追加します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-146">Adding Checkout Access</span></span>

<span data-ttu-id="ef6df-147">Wingtip Toys のサンプル アプリケーションは、匿名ユーザーを確認し、製品をショッピング カートに追加を許可します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-147">The Wingtip Toys sample application allows anonymous users to review and add products to a shopping cart.</span></span> <span data-ttu-id="ef6df-148">ただし、匿名ユーザーがショッピング カートに追加する製品を購入する場合、する必要がありますログオン サイトにします。</span><span class="sxs-lookup"><span data-stu-id="ef6df-148">However, when anonymous users choose to purchase the products they added to the shopping cart, they must log on to the site.</span></span> <span data-ttu-id="ef6df-149">ログオンしている、されたら、制限されたページ、Web アプリケーションのチェック アウトを処理し、購入のプロセスにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-149">Once they have logged on, they can access the restricted pages of the Web application that handle the checkout and purchase process.</span></span> <span data-ttu-id="ef6df-150">これらの制限されたページに含まれる、*チェック アウト*アプリケーションのフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="ef6df-150">These restricted pages are contained in the *Checkout* folder of the application.</span></span>

### <a name="add-a-checkout-folder-and-pages"></a><span data-ttu-id="ef6df-151">チェック アウト フォルダーとページを追加します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-151">Add a Checkout Folder and Pages</span></span>

<span data-ttu-id="ef6df-152">作成、*チェック アウト*フォルダーとにチェック アウト プロセス中に、顧客に表示されるページです。</span><span class="sxs-lookup"><span data-stu-id="ef6df-152">You will now create the *Checkout* folder and the pages in it that the customer will see during the checkout process.</span></span> <span data-ttu-id="ef6df-153">このチュートリアルで後でこれらのページが更新されます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-153">You will update these pages later in this tutorial.</span></span>

1. <span data-ttu-id="ef6df-154">プロジェクト名を右クリックして (**Wingtip Toys**) で**ソリューション エクスプ ローラー**選択**新しいフォルダーを追加**です。</span><span class="sxs-lookup"><span data-stu-id="ef6df-154">Right-click the project name (**Wingtip Toys**) in **Solution Explorer** and select **Add a New Folder**.</span></span> 

    ![チェック アウトおよび PayPal - 新しいフォルダーに支払い](checkout-and-payment-with-paypal/_static/image1.png)
2. <span data-ttu-id="ef6df-156">新しいフォルダーの名前を付けます*チェック アウト*です。</span><span class="sxs-lookup"><span data-stu-id="ef6df-156">Name the new folder *Checkout*.</span></span>
3. <span data-ttu-id="ef6df-157">右クリックし、*チェック アウト*クリックしてフォルダー**追加**-&gt;**新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-157">Right-click the *Checkout* folder and then select **Add**-&gt;**New Item**.</span></span> 

    ![チェック アウトおよび PayPal の新しい項目の追加の支払い](checkout-and-payment-with-paypal/_static/image2.png)
4. <span data-ttu-id="ef6df-159">**[新しい項目の追加]** ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-159">The **Add New Item** dialog box is displayed.</span></span>
5. <span data-ttu-id="ef6df-160">選択、 **Visual c#**  - &gt; **Web**左側のテンプレートのグループです。</span><span class="sxs-lookup"><span data-stu-id="ef6df-160">Select the **Visual C#** -&gt; **Web** templates group on the left.</span></span> <span data-ttu-id="ef6df-161">次に、中央のペインでは、次のように選択します。**マスター ページを含む Web フォーム**し名前を付けます*CheckoutStart.aspx*です。</span><span class="sxs-lookup"><span data-stu-id="ef6df-161">Then, from the middle pane, select **Web Form with Master Page**and name it *CheckoutStart.aspx*.</span></span> 

    ![チェック アウトおよび - PayPal の支払いは、新しい項目] ダイアログ ボックスを追加します。](checkout-and-payment-with-paypal/_static/image3.png)
6. <span data-ttu-id="ef6df-163">以前と同様、選択、 *Site.Master*マスター ページとしてファイル。</span><span class="sxs-lookup"><span data-stu-id="ef6df-163">As before, select the *Site.Master* file as the master page.</span></span>
7. <span data-ttu-id="ef6df-164">ページを追加、次に、*チェック アウト*前と同じ手順を使用してフォルダー。</span><span class="sxs-lookup"><span data-stu-id="ef6df-164">Add the following additional pages to the *Checkout* folder using the same steps above:</span></span>   

    - <span data-ttu-id="ef6df-165">CheckoutReview.aspx</span><span class="sxs-lookup"><span data-stu-id="ef6df-165">CheckoutReview.aspx</span></span>
    - <span data-ttu-id="ef6df-166">CheckoutComplete.aspx</span><span class="sxs-lookup"><span data-stu-id="ef6df-166">CheckoutComplete.aspx</span></span>
    - <span data-ttu-id="ef6df-167">CheckoutCancel.aspx</span><span class="sxs-lookup"><span data-stu-id="ef6df-167">CheckoutCancel.aspx</span></span>
    - <span data-ttu-id="ef6df-168">CheckoutError.aspx</span><span class="sxs-lookup"><span data-stu-id="ef6df-168">CheckoutError.aspx</span></span>

### <a name="add-a-webconfig-file"></a><span data-ttu-id="ef6df-169">Web.config ファイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-169">Add a Web.config File</span></span>

<span data-ttu-id="ef6df-170">新しいを追加して*Web.config*ファイルの名前を*チェック アウト*フォルダー、ことができます、フォルダーに含まれるすべてのページにアクセスを制限します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-170">By adding a new *Web.config* file to the *Checkout* folder, you will be able to restrict access to all the pages contained in the folder.</span></span>

1. <span data-ttu-id="ef6df-171">右クリックし、*チェック アウト*フォルダーと選択**追加** - &gt; **新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-171">Right-click the *Checkout* folder and select **Add** -&gt; **New Item**.</span></span>  
   <span data-ttu-id="ef6df-172">**[新しい項目の追加]** ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-172">The **Add New Item** dialog box is displayed.</span></span>
2. <span data-ttu-id="ef6df-173">選択、 **Visual c#**  - &gt; **Web**左側のテンプレートのグループです。</span><span class="sxs-lookup"><span data-stu-id="ef6df-173">Select the **Visual C#** -&gt; **Web** templates group on the left.</span></span> <span data-ttu-id="ef6df-174">次に、中央のペインでは、次のように選択します。 **Web 構成ファイル**、の既定の名前を受け入れる*Web.config*、し、[**追加**です。</span><span class="sxs-lookup"><span data-stu-id="ef6df-174">Then, from the middle pane, select **Web Configuration File**, accept the default name of *Web.config*, and then select **Add**.</span></span>
3. <span data-ttu-id="ef6df-175">既存の XML の内容を置き換える、 *Web.config*を次のファイル。</span><span class="sxs-lookup"><span data-stu-id="ef6df-175">Replace the existing XML content in the *Web.config* file with the following:</span></span>  

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample4.xml)]
4. <span data-ttu-id="ef6df-176">保存、 *Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="ef6df-176">Save the *Web.config* file.</span></span>

<span data-ttu-id="ef6df-177">*Web.config*ファイルは、Web アプリケーションのすべての不明なユーザーにする必要がありますに含まれているページへのアクセスが拒否されることを指定します、*チェック アウト*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="ef6df-177">The *Web.config* file specifies that all unknown users of the Web application must be denied access to the pages contained in the *Checkout* folder.</span></span> <span data-ttu-id="ef6df-178">ただし、ユーザー アカウントは登録、ログオンしている場合、既知のユーザーであるし、内のページにはアクセス、*チェック アウト*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="ef6df-178">However, if the user has registered an account and is logged on, they will be a known user and will have access to the pages in the *Checkout* folder.</span></span>

<span data-ttu-id="ef6df-179">ASP.NET の構成が、階層構造に従うことを確認することが重要で各*Web.config*ファイルが含まれているフォルダーとすべての子ディレクトリ下にある構成設定が適用されます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-179">It's important to note that ASP.NET configuration follows a hierarchy, where each *Web.config* file applies configuration settings to the folder that it is in and to all of the child directories below it.</span></span>

<a id="SSLWebForms"></a>
## <a name="enable-ssl-for-the-project"></a><span data-ttu-id="ef6df-180">プロジェクトの SSL を有効にします。</span><span class="sxs-lookup"><span data-stu-id="ef6df-180">Enable SSL for the Project</span></span>

 <span data-ttu-id="ef6df-181">安全な Sockets Layer (SSL) は、Web サーバーと Web クライアントの暗号化を使用するより安全に通信するために使用できるように定義されているプロトコルです。</span><span class="sxs-lookup"><span data-stu-id="ef6df-181">Secure Sockets Layer (SSL) is a protocol defined to allow Web servers and Web clients to communicate more securely through the use of encryption.</span></span> <span data-ttu-id="ef6df-182">SSL を使用しない場合、クライアントとサーバー間で送信されるデータには、ネットワークに物理的にアクセスできる任意のユーザーを見つけ出すパケットできます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-182">When SSL is not used, data sent between the client and server is open to packet sniffing by anyone with physical access to the network.</span></span> <span data-ttu-id="ef6df-183">さらに、いくつかの一般的な認証スキーム安全ではありませんプレーンな HTTP 経由でします。</span><span class="sxs-lookup"><span data-stu-id="ef6df-183">Additionally, several common authentication schemes are not secure over plain HTTP.</span></span> <span data-ttu-id="ef6df-184">具体的には、基本認証とフォーム認証は、暗号化されていない資格情報を送信します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-184">In particular, Basic authentication and forms authentication send unencrypted credentials.</span></span> <span data-ttu-id="ef6df-185">セキュリティで保護するには、これらの認証スキームは、SSL を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ef6df-185">To be secure, these authentication schemes must use SSL.</span></span> 

1. <span data-ttu-id="ef6df-186">**ソリューション エクスプ ローラー**をクリックして、 **WingtipToys** [プロジェクト] を押します**F4**を表示する、**プロパティ**ウィンドウです。</span><span class="sxs-lookup"><span data-stu-id="ef6df-186">In **Solution Explorer**, click the **WingtipToys** project, then press **F4** to display the **Properties** window.</span></span>
2. <span data-ttu-id="ef6df-187">変更**SSL を有効に**に`true`です。</span><span class="sxs-lookup"><span data-stu-id="ef6df-187">Change **SSL Enabled** to `true`.</span></span>
3. <span data-ttu-id="ef6df-188">コピー、 **SSL URL**後で使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="ef6df-188">Copy the **SSL URL** so you can use it later.</span></span>   
 <span data-ttu-id="ef6df-189">SSL url は、 `https://localhost:44300/` (下図のように) 以前 SSL Web サイトに作成した場合を除き、します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-189">The SSL URL will be `https://localhost:44300/` unless you've previously created SSL Web Sites (as shown below).</span></span>   
    <span data-ttu-id="ef6df-190">![プロジェクト プロパティ](checkout-and-payment-with-paypal/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="ef6df-190">![Project Properties](checkout-and-payment-with-paypal/_static/image4.png)</span></span>
4. <span data-ttu-id="ef6df-191">**ソリューション エクスプ ローラー**を右クリックして、 **WingtipToys**プロジェクトし、クリックして**プロパティ**です。</span><span class="sxs-lookup"><span data-stu-id="ef6df-191">In **Solution Explorer**, right click the **WingtipToys** project and click **Properties**.</span></span>
5. <span data-ttu-id="ef6df-192">左側のタブをクリックして**Web**です。</span><span class="sxs-lookup"><span data-stu-id="ef6df-192">In the left tab, click **Web**.</span></span>
6. <span data-ttu-id="ef6df-193">変更、**プロジェクト Url**を使用する、 **SSL URL**以前に保存します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-193">Change the **Project Url** to use the **SSL URL** that you saved earlier.</span></span>   
    <span data-ttu-id="ef6df-194">![プロジェクトの Web プロパティ](checkout-and-payment-with-paypal/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="ef6df-194">![Project Web Properties](checkout-and-payment-with-paypal/_static/image5.png)</span></span>
7. <span data-ttu-id="ef6df-195">キーを押してページを保存**CTRL + S**です。</span><span class="sxs-lookup"><span data-stu-id="ef6df-195">Save the page by pressing **CTRL+S**.</span></span>
8. <span data-ttu-id="ef6df-196">**Ctrl キーを押しながら F5 キーを押して**アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-196">Press **Ctrl+F5** to run the application.</span></span> <span data-ttu-id="ef6df-197">Visual Studio の SSL の警告を回避できるようにするオプションが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-197">Visual Studio will display an option to allow you to avoid SSL warnings.</span></span>
9. <span data-ttu-id="ef6df-198">をクリックして**はい**IIS Express の SSL 証明書を信頼して続行します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-198">Click **Yes** to trust the IIS Express SSL certificate and continue.</span></span>   
    <span data-ttu-id="ef6df-199">![IIS Express の SSL 証明書の詳細](checkout-and-payment-with-paypal/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="ef6df-199">![IIS Express SSL certificate details](checkout-and-payment-with-paypal/_static/image6.png)</span></span>  
 <span data-ttu-id="ef6df-200">セキュリティの警告が表示されます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-200">A Security Warning is displayed.</span></span>
10. <span data-ttu-id="ef6df-201">をクリックして**はい**localhost 証明書をインストールします。</span><span class="sxs-lookup"><span data-stu-id="ef6df-201">Click **Yes** to install the certificate to your localhost.</span></span>   
    <span data-ttu-id="ef6df-202">![セキュリティ警告] ダイアログ ボックス](checkout-and-payment-with-paypal/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="ef6df-202">![Security Warning dialog box](checkout-and-payment-with-paypal/_static/image7.png)</span></span>  
 <span data-ttu-id="ef6df-203">ブラウザー ウィンドウが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-203">The browser window will be displayed.</span></span>

<span data-ttu-id="ef6df-204">これで SSL を使用してローカルで、Web アプリケーションを簡単にテストできます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-204">You can now easily test your Web application locally using SSL.</span></span>

<a id="OAuthWebForms"></a>
## <a name="add-an-oauth-20-provider"></a><span data-ttu-id="ef6df-205">OAuth 2.0 プロバイダーを追加します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-205">Add an OAuth 2.0 Provider</span></span>

<span data-ttu-id="ef6df-206">ASP.NET Web フォームでは、メンバーシップと認証の拡張オプションを提供します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-206">ASP.NET Web Forms provides enhanced options for membership and authentication.</span></span> <span data-ttu-id="ef6df-207">これらの拡張機能には、OAuth が含まれます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-207">These enhancements include OAuth.</span></span> <span data-ttu-id="ef6df-208">OAuth は、web、モバイル、およびデスクトップ アプリケーションからの単純で標準的な方法で安全な承認を許可するオープン プロトコルです。</span><span class="sxs-lookup"><span data-stu-id="ef6df-208">OAuth is an open protocol that allows secure authorization in a simple and standard method from web, mobile, and desktop applications.</span></span> <span data-ttu-id="ef6df-209">ASP.NET Web フォーム テンプレートは、認証プロバイダーとして Facebook、Twitter、Google、Microsoft を公開するのに OAuth を使用します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-209">The ASP.NET Web Forms template uses OAuth to expose Facebook, Twitter, Google and Microsoft as authentication providers.</span></span> <span data-ttu-id="ef6df-210">このチュートリアルでは、認証プロバイダーとして Google のみを使用して、任意のプロバイダーを使用するコードを簡単に変更できます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-210">Although this tutorial uses only Google as the authentication provider, you can easily modify the code to use any of the providers.</span></span> <span data-ttu-id="ef6df-211">その他のプロバイダーを実装する手順は、このチュートリアルに表示される手順に非常に似ています。</span><span class="sxs-lookup"><span data-stu-id="ef6df-211">The steps to implement other providers are very similar to the steps you will see in this tutorial.</span></span>

<span data-ttu-id="ef6df-212">認証に加えて、チュートリアルもロールを使用して承認を実装します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-212">In addition to authentication, the tutorial will also use roles to implement authorization.</span></span> <span data-ttu-id="ef6df-213">追加するユーザーのみ、`canEdit`ロールはデータを変更するようになります (作成、編集、または連絡先を削除) します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-213">Only those users you add to the `canEdit` role will be able to change data (create, edit, or delete contacts).</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="ef6df-214">アプリケーションの Windows Live では、作業用の web サイトの URL をライブのみ承認し、ログインをテストするため、ローカルの web サイトの URL を使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="ef6df-214">Windows Live applications only accept a live URL for a working website, so you cannot use a local website URL for testing logins.</span></span>


<span data-ttu-id="ef6df-215">次の手順を使用すると、Google の認証プロバイダーを追加できます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-215">The following steps will allow you to add a Google authentication provider.</span></span>

1. <span data-ttu-id="ef6df-216">開く、*アプリ\_Start\Startup.Auth.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="ef6df-216">Open the *App\_Start\Startup.Auth.cs* file.</span></span>
2. <span data-ttu-id="ef6df-217">コメント文字を削除、`app.UseGoogleAuthentication()`メソッドとしてメソッドが表示されるように依存します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-217">Remove the comment characters from the `app.UseGoogleAuthentication()` method so that the method appears as follows:</span></span> 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample5.cs)]
3. <span data-ttu-id="ef6df-218">移動し、 [Google Developers Console](https://console.developers.google.com/)です。</span><span class="sxs-lookup"><span data-stu-id="ef6df-218">Navigate to the [Google Developers Console](https://console.developers.google.com/).</span></span> <span data-ttu-id="ef6df-219">Google developer 電子メール アカウント (gmail.com など) を使用してサインインする必要があります。</span><span class="sxs-lookup"><span data-stu-id="ef6df-219">You will also need to sign-in with your Google developer email account (gmail.com).</span></span> <span data-ttu-id="ef6df-220">Google アカウントがあるない場合は、選択、**アカウントを作成する**リンクします。</span><span class="sxs-lookup"><span data-stu-id="ef6df-220">If you do not have a Google account, select the **Create an account** link.</span></span>   
   <span data-ttu-id="ef6df-221">次に、表示、 **Google Developers Console**です。</span><span class="sxs-lookup"><span data-stu-id="ef6df-221">Next, you'll see the **Google Developers Console**.</span></span>   
    <span data-ttu-id="ef6df-222">![Google Developers Console](checkout-and-payment-with-paypal/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="ef6df-222">![Google Developers Console](checkout-and-payment-with-paypal/_static/image8.png)</span></span>
4. <span data-ttu-id="ef6df-223">クリックして、**プロジェクトの作成**] ボタンをクリックし、プロジェクト名と (既定値を使用することができます) ID を入力します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-223">Click the **Create Project** button and enter a project name and ID (you can use the default values).</span></span> <span data-ttu-id="ef6df-224">[] をクリックして、**契約のチェック ボックス**と**作成**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="ef6df-224">Then, click the **agreement checkbox** and the **Create** button.</span></span>  

    ![Google - 新しいプロジェクト](checkout-and-payment-with-paypal/_static/image9.png)

   <span data-ttu-id="ef6df-226">数秒で新しいプロジェクトを作成して、ブラウザーは、新しいプロジェクト] ページを表示します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-226">In a few seconds the new project will be created and your browser will display the new projects page.</span></span>
5. <span data-ttu-id="ef6df-227">左側のタブをクリックして**Api &amp; auth**、順にクリック**資格情報**です。</span><span class="sxs-lookup"><span data-stu-id="ef6df-227">In the left tab, click **APIs &amp; auth**, and then click **Credentials**.</span></span>
6. <span data-ttu-id="ef6df-228">クリックして、**新しいクライアント ID を作成する**[ **OAuth**です。</span><span class="sxs-lookup"><span data-stu-id="ef6df-228">Click the **Create New Client ID** under **OAuth**.</span></span>   
   <span data-ttu-id="ef6df-229">**クライアント ID を作成**ダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-229">The **Create Client ID** dialog will be displayed.</span></span>   
    <span data-ttu-id="ef6df-230">![Google のクライアント ID を作成します。](checkout-and-payment-with-paypal/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="ef6df-230">![Google - Create Client ID](checkout-and-payment-with-paypal/_static/image10.png)</span></span>
7. <span data-ttu-id="ef6df-231">**クライアント ID を作成**ダイアログ ボックスで、既定値を保持**Web アプリケーション**アプリケーションの種類。</span><span class="sxs-lookup"><span data-stu-id="ef6df-231">In the **Create Client ID** dialog, keep the default **Web application** for the application type.</span></span>
8. <span data-ttu-id="ef6df-232">設定、**承認済みの JavaScript 生成元**このチュートリアルで以前に使用する SSL URL を (`https://localhost:44300/` SSL の他のプロジェクトを作成した場合を除く)。</span><span class="sxs-lookup"><span data-stu-id="ef6df-232">Set the **Authorized JavaScript Origins** to the SSL URL you used earlier in this tutorial (`https://localhost:44300/` unless you've created other SSL projects).</span></span>   
   <span data-ttu-id="ef6df-233">この URL は、アプリケーションのための原点です。</span><span class="sxs-lookup"><span data-stu-id="ef6df-233">This URL is the origin for your application.</span></span> <span data-ttu-id="ef6df-234">このサンプルでは、のみ入力する localhost の URL のテスト。</span><span class="sxs-lookup"><span data-stu-id="ef6df-234">For this sample, you will only enter the localhost test URL.</span></span> <span data-ttu-id="ef6df-235">ただし、localhost および運用のために複数の Url を入力することができます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-235">However, you can enter multiple URLs to account for localhost and production.</span></span>
9. <span data-ttu-id="ef6df-236">設定、**リダイレクト URI の承認**以下。</span><span class="sxs-lookup"><span data-stu-id="ef6df-236">Set the **Authorized Redirect URI** to the following:</span></span> 

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample6.html)]

   <span data-ttu-id="ef6df-237">この値は、その ASP.NET OAuth の URI をユーザーが google OAuth サーバーと通信します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-237">This value is the URI that ASP.NET OAuth users to communicate with the google OAuth server.</span></span> <span data-ttu-id="ef6df-238">上記で使用する SSL URL に注意してください ( `https://localhost:44300/` SSL の他のプロジェクトを作成した場合を除く)。</span><span class="sxs-lookup"><span data-stu-id="ef6df-238">Remember the SSL URL you used above (    `https://localhost:44300/` unless you've created other SSL projects).</span></span>
10. <span data-ttu-id="ef6df-239">クリックして、**クライアント ID を作成**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="ef6df-239">Click the **Create Client ID** button.</span></span>
11. <span data-ttu-id="ef6df-240">Google 開発者コンソールの左側のメニューをクリックして、**同意画面**メニュー項目は、電子メール アドレスと製品名を設定します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-240">On the left menu of the Google Developers Console, click the **Consent screen** menu item, then set your email address and product name.</span></span> <span data-ttu-id="ef6df-241">フォームが完了したら、をクリックして**保存**です。</span><span class="sxs-lookup"><span data-stu-id="ef6df-241">When you have completed the form, click **Save**.</span></span>
12. <span data-ttu-id="ef6df-242">クリックして、 **Api**メニュー項目、スクロール ダウン] をクリック、**オフ**横に**Google + API**です。</span><span class="sxs-lookup"><span data-stu-id="ef6df-242">Click the **APIs** menu item, scroll down and click the **off** button next to **Google+ API**.</span></span>   
    <span data-ttu-id="ef6df-243">このオプションを受け入れると、Google + API が有効になります。</span><span class="sxs-lookup"><span data-stu-id="ef6df-243">Accepting this option will enable the Google+ API.</span></span>
13. <span data-ttu-id="ef6df-244">更新する必要があります、 **Microsoft.Owin** 3.0.0 NuGet パッケージです。</span><span class="sxs-lookup"><span data-stu-id="ef6df-244">You must also update the **Microsoft.Owin** NuGet package to version 3.0.0.</span></span>   
    <span data-ttu-id="ef6df-245">**ツール**メニューの [ **NuGet Package Manager**し、[ **Manage NuGet Packages for Solution**です。</span><span class="sxs-lookup"><span data-stu-id="ef6df-245">From the **Tools** menu, select **NuGet Package Manager** and then select **Manage NuGet Packages for Solution**.</span></span>  
    <span data-ttu-id="ef6df-246">**NuGet パッケージの管理**ウィンドウ、検索、更新、 **Microsoft.Owin** 3.0.0 パッケージです。</span><span class="sxs-lookup"><span data-stu-id="ef6df-246">From the **Manage NuGet Packages** window, find and update the **Microsoft.Owin** package to version 3.0.0.</span></span>
14. <span data-ttu-id="ef6df-247">Visual Studio で、更新、`UseGoogleAuthentication`のメソッド、 *Startup.Auth.cs*コピー アンド ペーストでページ、**クライアント ID**と**クライアント シークレット**メソッドにします。</span><span class="sxs-lookup"><span data-stu-id="ef6df-247">In Visual Studio, update the `UseGoogleAuthentication` method of the *Startup.Auth.cs* page by copying and pasting the **Client ID** and **Client Secret** into the method.</span></span> <span data-ttu-id="ef6df-248">**クライアント ID**と**クライアント シークレット**の下に表示される値のサンプルし、は機能しません。</span><span class="sxs-lookup"><span data-stu-id="ef6df-248">The **Client ID** and **Client Secret** values shown below are samples and will not work.</span></span> 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample7.cs?highlight=64-65)]
15. <span data-ttu-id="ef6df-249">キーを押して**ctrl キーを押しながら f5 キーを押して**アプリケーションをビルドして実行します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-249">Press **CTRL+F5** to build and run the application.</span></span> <span data-ttu-id="ef6df-250">クリックして、**ログイン**リンクします。</span><span class="sxs-lookup"><span data-stu-id="ef6df-250">Click the **Log in** link.</span></span>
16. <span data-ttu-id="ef6df-251">下にある**のログインに別のサービスを使用して**をクリックして**Google**です。</span><span class="sxs-lookup"><span data-stu-id="ef6df-251">Under **Use another service to log in**, click **Google**.</span></span>  
    <span data-ttu-id="ef6df-252">![ログイン](checkout-and-payment-with-paypal/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="ef6df-252">![Log in](checkout-and-payment-with-paypal/_static/image11.png)</span></span>
17. <span data-ttu-id="ef6df-253">資格情報を入力する必要がある場合、資格情報を入力する google サイトにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-253">If you need to enter your credentials, you will be redirected to the google site where you will enter your credentials.</span></span>  
    ![Google のサインイン](checkout-and-payment-with-paypal/_static/image12.png)
18. <span data-ttu-id="ef6df-255">資格情報を入力した後は、作成した web アプリケーションへのアクセス許可を与えるが求められます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-255">After you enter your credentials, you will be prompted to give permissions to the web application you just created.</span></span>  
    ![プロジェクトの既定のサービス アカウント](checkout-and-payment-with-paypal/_static/image13.png)
19. <span data-ttu-id="ef6df-257">をクリックして**受け入れる**です。</span><span class="sxs-lookup"><span data-stu-id="ef6df-257">Click **Accept**.</span></span> <span data-ttu-id="ef6df-258">今すぐにリダイレクトされます、**登録**のページ、 **WingtipToys**アプリケーションが Google アカウントを登録することができます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-258">You will now be redirected back to the **Register** page of the **WingtipToys** application where you can register your Google account.</span></span>  
    <span data-ttu-id="ef6df-259">![Google アカウントに登録します。](checkout-and-payment-with-paypal/_static/image14.png)</span><span class="sxs-lookup"><span data-stu-id="ef6df-259">![Register with your Google Account](checkout-and-payment-with-paypal/_static/image14.png)</span></span>
20. <span data-ttu-id="ef6df-260">Gmail アカウントで使用するローカル電子メール登録名を変更することがあるが、通常、既定の電子メール エイリアス (つまり、認証に使用するもの) を保持します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-260">You have the option of changing the local email registration name used for your Gmail account, but you generally want to keep the default email alias (that is, the one you used for authentication).</span></span> <span data-ttu-id="ef6df-261">をクリックして**ログイン**上記のようにします。</span><span class="sxs-lookup"><span data-stu-id="ef6df-261">Click **Log in** as shown above.</span></span>

### <a name="modifying-login-functionality"></a><span data-ttu-id="ef6df-262">ログインの機能を変更します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-262">Modifying Login Functionality</span></span>

<span data-ttu-id="ef6df-263">既に説明したようこのチュートリアルのシリーズで、ユーザー登録機能の多くに含まれている ASP.NET Web フォーム テンプレート既定です。</span><span class="sxs-lookup"><span data-stu-id="ef6df-263">As previously mentioned in this tutorial series, much of the user registration functionality has been included in the ASP.NET Web Forms template by default.</span></span> <span data-ttu-id="ef6df-264">これで、既定値を変更するは*Login.aspx*と*Register.aspx*を呼び出してページ、`MigrateCart`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="ef6df-264">Now you will modify the default *Login.aspx* and *Register.aspx* pages to call the `MigrateCart` method.</span></span> <span data-ttu-id="ef6df-265">`MigrateCart`メソッドは、匿名のショッピング カートに新しくログインしたユーザーを関連付けます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-265">The `MigrateCart` method associates a newly logged in user with an anonymous shopping cart.</span></span> <span data-ttu-id="ef6df-266">ユーザーを関連付けると、ショッピング カート、Wingtip Toys のサンプル アプリケーションはビジットの間でユーザーのショッピング カートを保持することになります。</span><span class="sxs-lookup"><span data-stu-id="ef6df-266">By associating the user and shopping cart, the Wingtip Toys sample application will be able to maintain the shopping cart of the user between visits.</span></span>

1. <span data-ttu-id="ef6df-267">**ソリューション エクスプ ローラー**を検索して開く、*アカウント*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="ef6df-267">In **Solution Explorer**, find and open the *Account* folder.</span></span>
2. <span data-ttu-id="ef6df-268">という名前の分離コード ページを変更して*Login.aspx.cs*を次のように表示されるように、黄色で強調表示されているコードを含めます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-268">Modify the code-behind page named *Login.aspx.cs* to include the code highlighted in yellow, so that it appears as follows:</span></span>   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample8.cs?highlight=41-43)]
3. <span data-ttu-id="ef6df-269">保存、 *Login.aspx.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="ef6df-269">Save the *Login.aspx.cs* file.</span></span>

<span data-ttu-id="ef6df-270">ここでは、警告の定義が存在しないことを無視することができます、`MigrateCart`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="ef6df-270">For now, you can ignore the warning that there is no definition for the `MigrateCart` method.</span></span> <span data-ttu-id="ef6df-271">このチュートリアルでは、少し後でそれを追加するは。</span><span class="sxs-lookup"><span data-stu-id="ef6df-271">You will be adding it a bit later in this tutorial.</span></span>

<span data-ttu-id="ef6df-272">*Login.aspx.cs*分離コード ファイルには、ログイン方法がサポートしています。</span><span class="sxs-lookup"><span data-stu-id="ef6df-272">The *Login.aspx.cs* code-behind file supports a LogIn method.</span></span> <span data-ttu-id="ef6df-273">Login.aspx ページを調べることによってわかりますこのページに「ログイン」のボタンが含まれている時に [トリガー] をクリックして、`LogIn`分離コードでハンドラー。</span><span class="sxs-lookup"><span data-stu-id="ef6df-273">By inspecting the Login.aspx page, you'll see that this page includes a "Log in" button that when click triggers the `LogIn` handler on the code-behind.</span></span>

<span data-ttu-id="ef6df-274">ときに、`Login`メソッドを*Login.aspx.cs*が呼び出されたが、ショッピング カートという名前の新しいインスタンス`usersShoppingCart`を作成します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-274">When the `Login` method on the *Login.aspx.cs* is called, a new instance of the shopping cart named `usersShoppingCart` is created.</span></span> <span data-ttu-id="ef6df-275">ショッピング カート (GUID) の ID が取得され、設定、`cartId`変数。</span><span class="sxs-lookup"><span data-stu-id="ef6df-275">The ID of the shopping cart (a GUID) is retrieved and set to the `cartId` variable.</span></span> <span data-ttu-id="ef6df-276">次に、`MigrateCart`両方を渡して、メソッドが呼び出されます、`cartId`と、このメソッドへのログイン ユーザーの名前。</span><span class="sxs-lookup"><span data-stu-id="ef6df-276">Then, the `MigrateCart` method is called, passing both the `cartId` and the name of the logged-in user to this method.</span></span> <span data-ttu-id="ef6df-277">移行時に、ショッピング カートは、匿名のショッピング カートの識別に使用される GUID は、ユーザー名に置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-277">When the shopping cart is migrated, the GUID used to identify the anonymous shopping cart is replaced with the user name.</span></span>

<span data-ttu-id="ef6df-278">変更だけでなく、 *Login.aspx.cs* 、ユーザーがログインするときに、ショッピング カートを移行する分離コード ファイルも変更する必要があります、 *Register.aspx.cs 分離コード ファイル*ショッピング カートを移行するにはときに、ユーザーは新しいアカウントを作成しログに記録します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-278">In addition to modifying the *Login.aspx.cs* code-behind file to migrate the shopping cart when the user logs in, you must also modify the *Register.aspx.cs code-behind file* to migrate the shopping cart when the user creates a new account and logs in.</span></span>

1. <span data-ttu-id="ef6df-279">*アカウント*フォルダーを開き、分離コード ファイルの名前は*Register.aspx.cs*です。</span><span class="sxs-lookup"><span data-stu-id="ef6df-279">In the *Account* folder, open the code-behind file named *Register.aspx.cs*.</span></span>
2. <span data-ttu-id="ef6df-280">次のように表示されるように、黄色でコードを含めることによって分離コード ファイルを変更します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-280">Modify the code-behind file by including the code in yellow, so that it appears as follows:</span></span>   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample9.cs?highlight=28-32)]
3. <span data-ttu-id="ef6df-281">保存、 *Register.aspx.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="ef6df-281">Save the *Register.aspx.cs* file.</span></span> <span data-ttu-id="ef6df-282">に関する警告を無視してもう一度、`MigrateCart`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="ef6df-282">Once again, ignore the warning about the `MigrateCart` method.</span></span>

<span data-ttu-id="ef6df-283">使用したコードに注目してください、`CreateUser_Click`イベント ハンドラーにで使用するコードによく似ていますが、`LogIn`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="ef6df-283">Notice that the code you used in the `CreateUser_Click` event handler is very similar to the code you used in the `LogIn` method.</span></span> <span data-ttu-id="ef6df-284">ユーザーが登録またはサイトへの呼び出しへのログイン、`MigrateCart`メソッドになります。</span><span class="sxs-lookup"><span data-stu-id="ef6df-284">When the user registers or logs in to the site, a call to the `MigrateCart` method will be made.</span></span>

## <a name="migrating-the-shopping-cart"></a><span data-ttu-id="ef6df-285">ショッピング カートを移行します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-285">Migrating the Shopping Cart</span></span>

<span data-ttu-id="ef6df-286">ショッピング カートを使用して、移行するコードを追加するには、ログで、登録プロセスの更新がある場合は、これで、`MigrateCart`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="ef6df-286">Now that you have the log-in and registration process updated, you can add the code to migrate the shopping cart using the `MigrateCart` method.</span></span>

1. <span data-ttu-id="ef6df-287">**ソリューション エクスプ ローラー**、検索、*ロジック*フォルダーと open、 *ShoppingCartActions.cs*クラス ファイルです。</span><span class="sxs-lookup"><span data-stu-id="ef6df-287">In **Solution Explorer**, find the *Logic* folder and open the *ShoppingCartActions.cs* class file.</span></span>
2. <span data-ttu-id="ef6df-288">既存のコードに黄色で強調表示のコードを追加、 *ShoppingCartActions.cs*ファイル、ように内のコード、 *ShoppingCartActions.cs*ファイルが次のように表示されます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-288">Add the code highlighted in yellow to the existing code in the *ShoppingCartActions.cs* file, so that the code in the *ShoppingCartActions.cs* file appears as follows:</span></span>   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample10.cs?highlight=215-224)]

<span data-ttu-id="ef6df-289">`MigrateCart`メソッドでは、既存の cartId を使用して、ユーザーのショッピング カートを検索します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-289">The `MigrateCart` method uses the existing cartId to find the shopping cart of the user.</span></span> <span data-ttu-id="ef6df-290">コードが次に、ショッピング カートのすべての項目をループし、置換、`CartId`プロパティ (で指定されたとおり、`CartItem`スキーマ) のログイン ユーザー名。</span><span class="sxs-lookup"><span data-stu-id="ef6df-290">Next, the code loops through all the shopping cart items and replaces the `CartId` property (as specified by the `CartItem` schema) with the logged-in user name.</span></span>

### <a name="updating-the-database-connection"></a><span data-ttu-id="ef6df-291">データベース接続の更新</span><span class="sxs-lookup"><span data-stu-id="ef6df-291">Updating the Database Connection</span></span>

<span data-ttu-id="ef6df-292">このチュートリアルを使用する場合、**構築済み**Wingtip Toys サンプル アプリケーション、既定のメンバーシップ データベースを再作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ef6df-292">If you are following this tutorial using the **prebuilt** Wingtip Toys sample application, you must recreate the default membership database.</span></span> <span data-ttu-id="ef6df-293">既定の接続文字列を変更すると、メンバーシップ データベースは次回アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-293">By modifying the default connection string, the membership database will be created the next time the application runs.</span></span>

1. <span data-ttu-id="ef6df-294">開く、 *Web.config*プロジェクトのルートにあるファイルです。</span><span class="sxs-lookup"><span data-stu-id="ef6df-294">Open the *Web.config* file at the root of the project.</span></span>
2. <span data-ttu-id="ef6df-295">次のように表示されるように、既定の接続文字列を更新します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-295">Update the default connection string so that it appears as follows:</span></span>   

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample11.xml)]

<a id="PayPalWebForms"></a>
## <a name="integrating-paypal"></a><span data-ttu-id="ef6df-296">PayPal の統合</span><span class="sxs-lookup"><span data-stu-id="ef6df-296">Integrating PayPal</span></span>

<span data-ttu-id="ef6df-297">PayPal は、オンライン ショップで支払いを受け付ける web ベースの課金プラットフォームです。</span><span class="sxs-lookup"><span data-stu-id="ef6df-297">PayPal is a web-based billing platform that accepts payments by online merchants.</span></span> <span data-ttu-id="ef6df-298">このチュートリアルでは、PayPal のチェック アウトの Express の機能をアプリケーションに統合する方法について次に説明します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-298">This tutorial next explains how to integrate PayPal's Express Checkout functionality into your application.</span></span> <span data-ttu-id="ef6df-299">Express のチェック アウトにより、PayPal をショッピング カートに追加したアイテムの支払いに使用できます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-299">Express Checkout allows your customers to use PayPal to pay for the items they have added to their shopping cart.</span></span>

### <a name="create-paylpal-test-accounts"></a><span data-ttu-id="ef6df-300">PaylPal テスト アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-300">Create PaylPal Test Accounts</span></span>

<span data-ttu-id="ef6df-301">PayPal のテスト環境を使用するのには、作成し、開発者のテスト アカウントを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ef6df-301">To use the PayPal testing environment, you must create and verify a developer test account.</span></span> <span data-ttu-id="ef6df-302">開発者テスト アカウントを使用して、購入者のテスト アカウントとアカウントの作成、seller テストされます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-302">You will use the developer test account to create a buyer test account and a seller test account.</span></span> <span data-ttu-id="ef6df-303">開発者のテスト アカウントの資格情報もにより、Wingtip Toys サンプル アプリケーション PayPal テスト環境にアクセスします。</span><span class="sxs-lookup"><span data-stu-id="ef6df-303">The developer test account credentials also will allow the Wingtip Toys sample application to access the PayPal testing environment.</span></span>

1. <span data-ttu-id="ef6df-304">ブラウザーでは、サイトのテスト、PayPal 開発者に移動します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-304">In a browser, navigate to the PayPal developer testing site:</span></span>   
    [https://developer.paypal.com](https://developer.paypal.com/)
2. <span data-ttu-id="ef6df-305">PayPal の開発者アカウントを持っていない場合は、新しいアカウントを作成] をクリックして**サインアップ**サインアップの手順に従います。</span><span class="sxs-lookup"><span data-stu-id="ef6df-305">If you don't have a PayPal developer account, create a new account by clicking **Sign Up**and following the sign up steps.</span></span> <span data-ttu-id="ef6df-306">既存の PayPal の開発者アカウントがある場合は、サインイン] をクリックして**ログで**です。</span><span class="sxs-lookup"><span data-stu-id="ef6df-306">If you have an existing PayPal developer account, sign in by clicking **Log In**.</span></span> <span data-ttu-id="ef6df-307">PayPal 開発者アカウントをこのチュートリアルで後ほど Wingtip Toys のサンプル アプリケーションをテストする必要があります。</span><span class="sxs-lookup"><span data-stu-id="ef6df-307">You will need your PayPal developer account to test the Wingtip Toys sample application later in this tutorial.</span></span>
3. <span data-ttu-id="ef6df-308">だけ、PayPal 開発者アカウントのサインアップしている場合は、PayPal、PayPal 開発者アカウントを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ef6df-308">If you have just signed up for your PayPal developer account, you may need to verify your PayPal developer account with PayPal.</span></span> <span data-ttu-id="ef6df-309">電子メール アカウントに送信される PayPal の手順に従ってアカウントを確認できます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-309">You can verify your account by following the steps that PayPal sent to your email account.</span></span> <span data-ttu-id="ef6df-310">PayPal 開発者アカウントを確認した後は、サイトのテスト、PayPal 開発者に再度ログインします。</span><span class="sxs-lookup"><span data-stu-id="ef6df-310">Once you have verified your PayPal developer account, log back into the PayPal developer testing site.</span></span>
4. <span data-ttu-id="ef6df-311">後で、PayPal の開発者アカウントを既にしていない場合は、PayPal buyer テスト アカウントを作成する必要があります、PayPal developer サイトにログインしているいずれかであります。</span><span class="sxs-lookup"><span data-stu-id="ef6df-311">After you are logged in to the PayPal developer site with your PayPal developer account you need to create a PayPal buyer test account if you don't already have one.</span></span> <span data-ttu-id="ef6df-312">PayPal の [サイト] をクリックに購入者のテスト アカウントを作成する、**アプリケーション**] タブをクリックして**サンド ボックス アカウント**です。</span><span class="sxs-lookup"><span data-stu-id="ef6df-312">To create a buyer test account, on the PayPal site click the **Applications** tab and then click **Sandbox accounts**.</span></span>   
 <span data-ttu-id="ef6df-313">**サンド ボックスのテスト アカウント**ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-313">The **Sandbox test accounts** page is shown.</span></span>   

    > [!NOTE] 
    > 
    > <span data-ttu-id="ef6df-314">PayPal の開発者向けサイトには、販売者のテスト用のアカウントが既に用意されています。</span><span class="sxs-lookup"><span data-stu-id="ef6df-314">The PayPal Developer site already provides a merchant test account.</span></span>

    ![チェック アウトおよび PayPal のサンド ボックスのテスト アカウントでの支払い](checkout-and-payment-with-paypal/_static/image15.png)
5. <span data-ttu-id="ef6df-316">サンド ボックス テスト アカウント] ページで、**アカウントの作成**です。</span><span class="sxs-lookup"><span data-stu-id="ef6df-316">On the Sandbox test accounts page, click **Create Account**.</span></span>
6. <span data-ttu-id="ef6df-317">**テスト アカウントの作成**] ページでは、テスト アカウントの電子メール アドレスと、任意のパスワードに、購入者を選択します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-317">On the **Create test account** page choose a buyer test account email and password of your choice.</span></span>   

    > [!NOTE] 
    > 
    > <span data-ttu-id="ef6df-318">購入者の電子メール アドレスと、このチュートリアルの最後の Wingtip Toys サンプル アプリケーションをテストするためのパスワードが必要です。</span><span class="sxs-lookup"><span data-stu-id="ef6df-318">You will need the buyer email addresses and password to test the Wingtip Toys sample application at the end of this tutorial.</span></span>

    ![チェック アウトおよび PayPal のサンド ボックスのテスト アカウントでの支払い](checkout-and-payment-with-paypal/_static/image16.png)
7. <span data-ttu-id="ef6df-320">クリックして、購入者のテスト アカウントを作成、**アカウントの作成**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="ef6df-320">Create the buyer test account by clicking the **Create Account** button.</span></span>  
 <span data-ttu-id="ef6df-321">**サンド ボックスのテスト アカウント**ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-321">The **Sandbox Test accounts** page is displayed.</span></span> 

    ![チェック アウトおよび PayPal - PaylPal アカウントに支払い](checkout-and-payment-with-paypal/_static/image17.png)
8. <span data-ttu-id="ef6df-323">**サンド ボックスのテスト アカウント**] ページで、をクリックして、**読み上げて**電子メール アカウント。</span><span class="sxs-lookup"><span data-stu-id="ef6df-323">On the **Sandbox test accounts** page, click the **facilitator** email account.</span></span>  
    <span data-ttu-id="ef6df-324">**プロファイル**と**通知**オプションが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-324">**Profile** and **Notification** options appear.</span></span>
9. <span data-ttu-id="ef6df-325">選択、**プロファイル**オプション、をクリックして**API の資格情報**商業テスト アカウント用の API 資格情報を表示します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-325">Select the **Profile** option, then click **API credentials** to view your API credentials for the merchant test account.</span></span>
10. <span data-ttu-id="ef6df-326">テスト API 資格情報をメモ帳にコピーします。</span><span class="sxs-lookup"><span data-stu-id="ef6df-326">Copy the TEST API credentials to notepad.</span></span>

<span data-ttu-id="ef6df-327">テスト環境 PayPal に表示されているテスト API をクラシック資格情報 (ユーザー名、パスワード、および署名)、Wingtip Toys サンプル アプリケーションの API 呼び出しを行う必要になります。</span><span class="sxs-lookup"><span data-stu-id="ef6df-327">You will need your displayed Classic TEST API credentials (Username, Password, and Signature) to make API calls from the Wingtip Toys sample application to the PayPal testing environment.</span></span> <span data-ttu-id="ef6df-328">次の手順では、資格情報を追加します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-328">You will add the credentials in the next step.</span></span>

### <a name="add-paypal-class-and-api-credentials"></a><span data-ttu-id="ef6df-329">PayPal クラスと API の資格情報を追加します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-329">Add PayPal Class and API Credentials</span></span>

<span data-ttu-id="ef6df-330">1 つのクラスに PayPal のコードの大半を配置します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-330">You will place the majority of the PayPal code into a single class.</span></span> <span data-ttu-id="ef6df-331">このクラスには、PayPal との通信に使用するメソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="ef6df-331">This class contains the methods used to communicate with PayPal.</span></span> <span data-ttu-id="ef6df-332">またには、PayPal の資格情報は、このクラスに追加します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-332">Also, you will add your PayPal credentials to this class.</span></span>

1. <span data-ttu-id="ef6df-333">Wingtip Toys サンプル アプリケーションの Visual Studio 内で右クリックし、**ロジック**クリックしてフォルダー**追加** - &gt; **新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-333">In the Wingtip Toys sample application within Visual Studio, right-click the **Logic** folder and then select **Add** -&gt; **New Item**.</span></span>   
   <span data-ttu-id="ef6df-334">**[新しい項目の追加]** ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-334">The **Add New Item** dialog box is displayed.</span></span>
2. <span data-ttu-id="ef6df-335">**Visual c#**から、**インストール**左側のウィンドウ、[**コード**です。</span><span class="sxs-lookup"><span data-stu-id="ef6df-335">Under **Visual C#** from the **Installed** pane on the left, select **Code**.</span></span>
3. <span data-ttu-id="ef6df-336">中央のペインでは、次のように選択します。**クラス**です。</span><span class="sxs-lookup"><span data-stu-id="ef6df-336">From the middle pane, select **Class**.</span></span> <span data-ttu-id="ef6df-337">この新しいクラスの名前を**PayPalFunctions.cs**です。</span><span class="sxs-lookup"><span data-stu-id="ef6df-337">Name this new class **PayPalFunctions.cs**.</span></span>
4. <span data-ttu-id="ef6df-338">**[追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="ef6df-338">Click **Add**.</span></span>  
   <span data-ttu-id="ef6df-339">新しいクラス ファイルがエディターで表示されます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-339">The new class file is displayed in the editor.</span></span>
5. <span data-ttu-id="ef6df-340">既定のコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-340">Replace the default code with the following code:</span></span>  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample12.cs)]
6. <span data-ttu-id="ef6df-341">ショップ API 資格情報 (ユーザー名、パスワード、および署名)、PayPal のテスト環境に関数呼び出しを行うことができるように、このチュートリアルで先ほどの表示を追加します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-341">Add the Merchant API credentials (Username, Password, and Signature) that you displayed earlier in this tutorial so that you can make function calls to the PayPal testing environment.</span></span>  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample13.cs)]

> [!NOTE] 
> 
> <span data-ttu-id="ef6df-342">このサンプル アプリケーションでは、c# ファイル (\*.cs) に単に資格情報を追加します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-342">In this sample application you are simply adding credentials to a C# file (.cs).</span></span> <span data-ttu-id="ef6df-343">ただし、実装済みのソリューションで、構成ファイルで、資格情報の暗号化を検討してください。</span><span class="sxs-lookup"><span data-stu-id="ef6df-343">However, in a implemented solution, you should consider encrypting your credentials in a configuration file.</span></span>


<span data-ttu-id="ef6df-344">NVPAPICaller クラスには、PayPal 機能の大部分が含まれています。</span><span class="sxs-lookup"><span data-stu-id="ef6df-344">The NVPAPICaller class contains the majority of the PayPal functionality.</span></span> <span data-ttu-id="ef6df-345">クラスのコードでは、購入、PayPal のテスト環境からテストの作成に必要なメソッドを提供します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-345">The code in the class provides the methods needed to make a test purchase from the PayPal testing environment.</span></span> <span data-ttu-id="ef6df-346">次の 3 つの PayPal 関数は、購入に使用されます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-346">The following three PayPal functions are used to make purchases:</span></span>

- <span data-ttu-id="ef6df-347">`SetExpressCheckout` 関数</span><span class="sxs-lookup"><span data-stu-id="ef6df-347">`SetExpressCheckout` function</span></span>
- <span data-ttu-id="ef6df-348">`GetExpressCheckoutDetails` 関数</span><span class="sxs-lookup"><span data-stu-id="ef6df-348">`GetExpressCheckoutDetails` function</span></span>
- <span data-ttu-id="ef6df-349">`DoExpressCheckoutPayment` 関数</span><span class="sxs-lookup"><span data-stu-id="ef6df-349">`DoExpressCheckoutPayment` function</span></span>

<span data-ttu-id="ef6df-350">`ShortcutExpressCheckout`メソッドは、ショッピング カートの呼び出しからテストの購入情報と製品の詳細を収集、 `SetExpressCheckout` PayPal 関数。</span><span class="sxs-lookup"><span data-stu-id="ef6df-350">The `ShortcutExpressCheckout` method collects the test purchase information and product details from the shopping cart and calls the `SetExpressCheckout` PayPal function.</span></span> <span data-ttu-id="ef6df-351">`GetCheckoutDetails`メソッドは、購入の詳細と呼び出しを確認、`GetExpressCheckoutDetails`テスト購買を行う前に、PayPal 関数。</span><span class="sxs-lookup"><span data-stu-id="ef6df-351">The `GetCheckoutDetails` method confirms purchase details and calls the `GetExpressCheckoutDetails` PayPal function before making the test purchase.</span></span> <span data-ttu-id="ef6df-352">`DoCheckoutPayment`メソッドを呼び出して、テスト環境からテスト購入を完了すると、 `DoExpressCheckoutPayment` PayPal 関数。</span><span class="sxs-lookup"><span data-stu-id="ef6df-352">The `DoCheckoutPayment` method completes the test purchase from the testing environment by calling the `DoExpressCheckoutPayment` PayPal function.</span></span> <span data-ttu-id="ef6df-353">残りのコードは、PayPal メソッドおよび文字列をエンコード、文字列をデコード、配列、処理、および資格情報を決定するなどのプロセスをサポートします。</span><span class="sxs-lookup"><span data-stu-id="ef6df-353">The remaining code supports the PayPal methods and process, such as encoding strings, decoding strings, processing arrays, and determining credentials.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="ef6df-354">PayPal を使用すると、基に省略可能な購入の詳細を含める[PayPal の API 仕様](https://cms.paypal.com/us/cgi-bin/?cmd=_render-content&amp;content_ID=developer/e_howto_api_nvp_r_SetExpressCheckout)です。</span><span class="sxs-lookup"><span data-stu-id="ef6df-354">PayPal allows you to include optional purchase details based on [PayPal's API specification](https://cms.paypal.com/us/cgi-bin/?cmd=_render-content&amp;content_ID=developer/e_howto_api_nvp_r_SetExpressCheckout).</span></span> <span data-ttu-id="ef6df-355">Wingtip Toys サンプル アプリケーションでコードを拡張するには、ローカライズの詳細、製品の説明、税、カスタマー サービスの数、だけでなく他の多くの省略可能なフィールドを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-355">By extending the code in the Wingtip Toys sample application, you can include localization details, product descriptions, tax, a customer service number, as well as many other optional fields.</span></span>


<span data-ttu-id="ef6df-356">注意して、戻り値と [キャンセル] Url で指定されている、 **ShortcutExpressCheckout**メソッドは、ポート番号を使用します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-356">Notice that the return and cancel URLs that are specified in the **ShortcutExpressCheckout** method use a port number.</span></span>

[!code-html[Main](checkout-and-payment-with-paypal/samples/sample14.html)]

<span data-ttu-id="ef6df-357">Visual Web Developer は、SSL を使用して、web プロジェクトを実行するときに一般的 44300 ポートは、web サーバーの使用します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-357">When Visual Web Developer runs a web project using SSL, commonly the port 44300 is used for the web server.</span></span> <span data-ttu-id="ef6df-358">上記のように、ポート番号は 44300 がします。</span><span class="sxs-lookup"><span data-stu-id="ef6df-358">As shown above, the port number is 44300.</span></span> <span data-ttu-id="ef6df-359">アプリケーションを実行するときに、別のポート番号を表示できます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-359">When you run the application, you could see a different port number.</span></span> <span data-ttu-id="ef6df-360">ポート番号のニーズを正しく設定するコードでことができます成功したは、このチュートリアルの最後に、Wingtip Toys のサンプル アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-360">Your port number needs to be correctly set in the code so that you can successful run the Wingtip Toys sample application at the end of this tutorial.</span></span> <span data-ttu-id="ef6df-361">このチュートリアルの次のセクションでは、ローカル ホストのポート番号を取得し、PayPal クラスを更新する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-361">The next section of this tutorial explains how to retrieve the local host port number and update the PayPal class.</span></span>

### <a name="update-the-localhost-port-number-in-the-paypal-class"></a><span data-ttu-id="ef6df-362">更新プログラム、LocalHost にポート番号、PayPal クラス</span><span class="sxs-lookup"><span data-stu-id="ef6df-362">Update the LocalHost Port Number in the PayPal Class</span></span>

<span data-ttu-id="ef6df-363">Wingtip Toys のサンプル アプリケーションは、PayPal テスト サイトに移動し、Wingtip Toys のサンプル アプリケーションのローカル インスタンスを返すことで、製品を購入します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-363">The Wingtip Toys sample application purchases products by navigating to the PayPal testing site and returning to your local instance of the Wingtip Toys sample application.</span></span> <span data-ttu-id="ef6df-364">正しい URL を返す PayPal するために、ローカルで実行中のポート番号を指定する必要があります。 アプリケーション、PayPal のコードに上記のサンプルです。</span><span class="sxs-lookup"><span data-stu-id="ef6df-364">In order to have PayPal return to the correct URL, you need to specify the port number of the locally running sample application in the PayPal code mentioned above.</span></span>

1. <span data-ttu-id="ef6df-365">プロジェクト名を右クリックして (**WingtipToys**) で**ソリューション エクスプ ローラー**選択**プロパティ**です。</span><span class="sxs-lookup"><span data-stu-id="ef6df-365">Right-click the project name (**WingtipToys**) in **Solution Explorer** and select **Properties**.</span></span>
2. <span data-ttu-id="ef6df-366">左側の列を選択、 **Web**タブです。</span><span class="sxs-lookup"><span data-stu-id="ef6df-366">In the left column, select the **Web** tab.</span></span>
3. <span data-ttu-id="ef6df-367">ポート番号を取得、**プロジェクト Url**ボックス。</span><span class="sxs-lookup"><span data-stu-id="ef6df-367">Retrieve the port number from the **Project Url** box.</span></span>
4. <span data-ttu-id="ef6df-368">必要な場合、更新、`returnURL`と`cancelURL`PayPal クラスで (`NVPAPICaller`) で、 *PayPalFunctions.cs* web アプリケーションのポート番号を使用するファイル。</span><span class="sxs-lookup"><span data-stu-id="ef6df-368">If needed, update the `returnURL` and `cancelURL` in the PayPal class (`NVPAPICaller`) in the *PayPalFunctions.cs* file to use the port number of your web application:</span></span>   

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample15.html?highlight=1-2)]

<span data-ttu-id="ef6df-369">ここで追加したコードでは、ローカル Web アプリケーションの適切なポートは一致します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-369">Now the code that you added will match the expected port for your local Web application.</span></span> <span data-ttu-id="ef6df-370">PayPal は、ローカル コンピューターに正しい URL に戻るにことができます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-370">PayPal will be able to return to the correct URL on your local machine.</span></span>

### <a name="add-the-paypal-checkout-button"></a><span data-ttu-id="ef6df-371">PayPal のチェック アウト] ボタンを追加します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-371">Add the PayPal Checkout Button</span></span>

<span data-ttu-id="ef6df-372">プライマリの PayPal 関数は、サンプル アプリケーションに追加されましたが、これで、マークアップとこれらの関数の呼び出しに必要なコードを追加することを開始できます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-372">Now that the primary PayPal functions have been added to the sample application, you can begin adding the markup and code needed to call these functions.</span></span> <span data-ttu-id="ef6df-373">最初に、ユーザーが買い物カゴのページに表示されるチェック アウト] ボタンを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ef6df-373">First, you must add the checkout button that the user will see on the shopping cart page.</span></span>

1. <span data-ttu-id="ef6df-374">開く、*後*ファイル。</span><span class="sxs-lookup"><span data-stu-id="ef6df-374">Open the *ShoppingCart.aspx* file.</span></span>
2. <span data-ttu-id="ef6df-375">ファイルの一番下までスクロールし、検索、`<!--Checkout Placeholder -->`コメントです。</span><span class="sxs-lookup"><span data-stu-id="ef6df-375">Scroll to the bottom of the file and find the `<!--Checkout Placeholder -->` comment.</span></span>
3. <span data-ttu-id="ef6df-376">コメントを置き換える、`ImageButton`コントロールを印は次のように置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-376">Replace the comment with an `ImageButton` control so that the mark up is replaced as follows:</span></span>  

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample16.aspx)]
4. <span data-ttu-id="ef6df-377">*ShoppingCart.aspx.cs*後、ファイル、 `UpdateBtn_Click` 、ファイルの末尾付近のイベント ハンドラーを追加、`CheckOutBtn_Click`イベントのハンドラー。</span><span class="sxs-lookup"><span data-stu-id="ef6df-377">In the *ShoppingCart.aspx.cs* file, after the `UpdateBtn_Click` event handler near the end of the file, add the `CheckOutBtn_Click` event handler:</span></span>  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample17.cs)]
5. <span data-ttu-id="ef6df-378">また、 *ShoppingCart.aspx.cs*ファイルの参照を追加する、`CheckoutBtn`新しいイメージ ボタンは次のように参照されているため。</span><span class="sxs-lookup"><span data-stu-id="ef6df-378">Also in the *ShoppingCart.aspx.cs* file, add a reference to the `CheckoutBtn`, so that the new image button is referenced as follows:</span></span>  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample18.cs?highlight=18)]
6. <span data-ttu-id="ef6df-379">両方に変更を保存、*後*ファイルおよび*ShoppingCart.aspx.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="ef6df-379">Save your changes to both the *ShoppingCart.aspx* file and the *ShoppingCart.aspx.cs* file.</span></span>
7. <span data-ttu-id="ef6df-380">メニューから、次のように選択します。**デバッグ**-&gt;**ビルド WingtipToys**です。</span><span class="sxs-lookup"><span data-stu-id="ef6df-380">From the menu, select **Debug**-&gt;**Build WingtipToys**.</span></span>  
   <span data-ttu-id="ef6df-381">プロジェクトが再構築で新しく追加された**ImageButton**コントロール。</span><span class="sxs-lookup"><span data-stu-id="ef6df-381">The project will be rebuilt with the newly added **ImageButton** control.</span></span>

### <a name="send-purchase-details-to-paypal"></a><span data-ttu-id="ef6df-382">注文書の詳細を PayPal に送信します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-382">Send Purchase Details to PayPal</span></span>

<span data-ttu-id="ef6df-383">ユーザーがクリックしたとき、**チェック アウト**ショッピング カート ページ上のボタン (*後*)、発注プロセスを始めます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-383">When the user clicks the **Checkout** button on the shopping cart page (*ShoppingCart.aspx*), they'll begin the purchase process.</span></span> <span data-ttu-id="ef6df-384">次のコードでは、製品を購入するために必要な最初の PayPal 関数を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-384">The following code calls the first PayPal function needed to purchase products.</span></span>

1. <span data-ttu-id="ef6df-385">*チェック アウト*フォルダーを開き、分離コード ファイルの名前は*CheckoutStart.aspx.cs*です。</span><span class="sxs-lookup"><span data-stu-id="ef6df-385">From the *Checkout* folder, open the code-behind file named *CheckoutStart.aspx.cs*.</span></span>   
   <span data-ttu-id="ef6df-386">分離コード ファイルを開くことを確認します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-386">Be sure to open the code-behind file.</span></span>
2. <span data-ttu-id="ef6df-387">既存のコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-387">Replace the existing code with the following:</span></span>   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample19.cs)]

<span data-ttu-id="ef6df-388">アプリケーションのユーザーがクリックしたとき、**チェック アウト**ショッピング カート] ページで、ブラウザーのボタンに移動、 *CheckoutStart.aspx*ページ。</span><span class="sxs-lookup"><span data-stu-id="ef6df-388">When the user of the application clicks the **Checkout** button on the shopping cart page, the browser will navigate to the *CheckoutStart.aspx* page.</span></span> <span data-ttu-id="ef6df-389">ときに、 *CheckoutStart.aspx*ロード、ページ、`ShortcutExpressCheckout`メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-389">When the *CheckoutStart.aspx* page loads, the `ShortcutExpressCheckout` method is called.</span></span> <span data-ttu-id="ef6df-390">この時点では、ユーザーは、PayPal のテストの web サイトに転送されます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-390">At this point, the user is transferred to the PayPal testing web site.</span></span> <span data-ttu-id="ef6df-391">PayPal サイトで、ユーザー、PayPal の資格情報を入力、購入の詳細を確認、PayPal 契約を受け入れるおよび Wingtip Toys サンプル アプリケーションに返します場所、`ShortcutExpressCheckout`メソッドが完了しました。</span><span class="sxs-lookup"><span data-stu-id="ef6df-391">On the PayPal site, the user enters their PayPal credentials, reviews the purchase details, accepts the PayPal agreement and returns to the Wingtip Toys sample application where the `ShortcutExpressCheckout` method completes.</span></span> <span data-ttu-id="ef6df-392">ときに、`ShortcutExpressCheckout`メソッドが完了したら、それがユーザーをリダイレクトして、 *CheckoutReview.aspx* ] ページで指定された、`ShortcutExpressCheckout`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="ef6df-392">When the `ShortcutExpressCheckout` method is complete, it will redirect the user to the *CheckoutReview.aspx* page specified in the `ShortcutExpressCheckout` method.</span></span> <span data-ttu-id="ef6df-393">これにより、ユーザーは、Wingtip Toys サンプル アプリケーション内で注文の詳細を確認できます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-393">This allows the user to review the order details from within the Wingtip Toys sample application.</span></span>

### <a name="review-order-details"></a><span data-ttu-id="ef6df-394">注文の詳細の確認</span><span class="sxs-lookup"><span data-stu-id="ef6df-394">Review Order Details</span></span>

<span data-ttu-id="ef6df-395">PayPal から返された後、 *CheckoutReview.aspx* Wingtip Toys のサンプル アプリケーションのページには、注文の詳細が表示されます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-395">After returning from PayPal, the *CheckoutReview.aspx* page of the Wingtip Toys sample application displays the order details.</span></span> <span data-ttu-id="ef6df-396">このページでは、ユーザーが製品を購入する前に、注文の詳細を確認します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-396">This page allows the user to review the order details before purchasing the products.</span></span> <span data-ttu-id="ef6df-397">*CheckoutReview.aspx*ページを次のように作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ef6df-397">The *CheckoutReview.aspx* page must be created as follows:</span></span>

1. <span data-ttu-id="ef6df-398">*チェック アウト*フォルダーを開き、という名前のページ*CheckoutReview.aspx*です。</span><span class="sxs-lookup"><span data-stu-id="ef6df-398">In the *Checkout* folder, open the page named *CheckoutReview.aspx*.</span></span>
2. <span data-ttu-id="ef6df-399">既存のマークアップを次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-399">Replace the existing markup with the following:</span></span>   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample20.aspx)]
3. <span data-ttu-id="ef6df-400">という名前の分離コード ページを開く*CheckoutReview.aspx.cs*し、既存のコードを次に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-400">Open the code-behind page named *CheckoutReview.aspx.cs* and replace the existing code with the following:</span></span>   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample21.cs)]

<span data-ttu-id="ef6df-401">**DetailsView** PayPal から返された注文の詳細を表示するコントロールを使用します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-401">The **DetailsView** control is used to display the order details that have been returned from PayPal.</span></span> <span data-ttu-id="ef6df-402">また、上記のコード、注文の詳細、Wingtip Toys をデータベースに保存として、`OrderDetail`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="ef6df-402">Also, the above code saves the order details to the Wingtip Toys database as an `OrderDetail` object.</span></span> <span data-ttu-id="ef6df-403">ユーザーがクリックしたときに、**注文完了**ボタンにリダイレクトされます、 *CheckoutComplete.aspx*ページ。</span><span class="sxs-lookup"><span data-stu-id="ef6df-403">When the user clicks on the **Complete Order** button, they are redirected to the *CheckoutComplete.aspx* page.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="ef6df-404">**ヒント。**</span><span class="sxs-lookup"><span data-stu-id="ef6df-404">**Tip**</span></span>
> 
> <span data-ttu-id="ef6df-405">マークアップで、 *CheckoutReview.aspx* ] ページで、ことに注意して、`<ItemStyle>`タグが使用中の項目のスタイルを変更する、 **DetailsView**ページの下部にあるコントロールです。</span><span class="sxs-lookup"><span data-stu-id="ef6df-405">In the markup of the *CheckoutReview.aspx* page, notice that the `<ItemStyle>` tag is used to change the style of the items within the **DetailsView** control near the bottom of the page.</span></span> <span data-ttu-id="ef6df-406">内のページを表示して**デザイン ビュー** (を選択して**デザイン**Visual Studio の左下隅にある) を選択し、 **DetailsView**制御、および、を選択します。**スマート タグ**(上部にある矢印アイコン コントロールの右) を表示することができます、 **DetailsView タスク**です。</span><span class="sxs-lookup"><span data-stu-id="ef6df-406">By viewing the page in **Design View** (by selecting **Design** at the lower left corner of Visual Studio), then selecting the **DetailsView** control, and selecting the **Smart Tag** (the arrow icon at the top right of the control), you will be able to see the **DetailsView Tasks**.</span></span>
> 
> ![チェック アウトと PayPal の支払フィールドを編集します。](checkout-and-payment-with-paypal/_static/image18.png)
> 
> <span data-ttu-id="ef6df-408">選択して**フィールドの編集**、**フィールド**] ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-408">By selecting **Edit Fields**, the **Fields** dialog box will appear.</span></span> <span data-ttu-id="ef6df-409">このダイアログ ボックスで簡単に制御できます visual のプロパティなど**後**の**DetailsView**コントロール。</span><span class="sxs-lookup"><span data-stu-id="ef6df-409">In this dialog box you can easily control the visual properties, such as **ItemStyle**, of the **DetailsView** control.</span></span>
> 
> ![チェック アウトおよび PayPal - フィールド] ダイアログ ボックスでの支払い](checkout-and-payment-with-paypal/_static/image19.png)


### <a name="complete-purchase"></a><span data-ttu-id="ef6df-411">購入を完了します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-411">Complete Purchase</span></span>

<span data-ttu-id="ef6df-412">*CheckoutComplete.aspx* PayPal のページでは、購入します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-412">*CheckoutComplete.aspx* page makes the purchase from PayPal.</span></span> <span data-ttu-id="ef6df-413">前述のように、ユーザーをクリックして、**注文完了**ボタンをアプリケーションが移動する前に、 *CheckoutComplete.aspx*ページ。</span><span class="sxs-lookup"><span data-stu-id="ef6df-413">As mentioned above, the user must click on the **Complete Order** button before the application will navigate to the *CheckoutComplete.aspx* page.</span></span>

1. <span data-ttu-id="ef6df-414">*チェック アウト*フォルダーを開き、という名前のページ*CheckoutComplete.aspx*です。</span><span class="sxs-lookup"><span data-stu-id="ef6df-414">In the *Checkout* folder, open the page named *CheckoutComplete.aspx*.</span></span>
2. <span data-ttu-id="ef6df-415">既存のマークアップを次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-415">Replace the existing markup with the following:</span></span>   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample22.aspx)]
3. <span data-ttu-id="ef6df-416">という名前の分離コード ページを開く*CheckoutComplete.aspx.cs*し、既存のコードを次に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-416">Open the code-behind page named *CheckoutComplete.aspx.cs* and replace the existing code with the following:</span></span>   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample23.cs)]

<span data-ttu-id="ef6df-417">ときに、 *CheckoutComplete.aspx*ページが読み込まれると、`DoCheckoutPayment`メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-417">When the *CheckoutComplete.aspx* page is loaded, the `DoCheckoutPayment` method is called.</span></span> <span data-ttu-id="ef6df-418">以前に説明したように、`DoCheckoutPayment`メソッドが、PayPal のテスト環境から購入を完了します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-418">As mentioned earlier, the `DoCheckoutPayment` method completes the purchase from the PayPal testing environment.</span></span> <span data-ttu-id="ef6df-419">PayPal の注文の購入が完了した後、 *CheckoutComplete.aspx*ページには、支払トランザクションが表示されます。`ID`購入者にします。</span><span class="sxs-lookup"><span data-stu-id="ef6df-419">Once PayPal has completed the purchase of the order, the *CheckoutComplete.aspx* page displays a payment transaction `ID` to the purchaser.</span></span>

### <a name="handle-cancel-purchase"></a><span data-ttu-id="ef6df-420">キャンセルの注文書を処理します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-420">Handle Cancel Purchase</span></span>

<span data-ttu-id="ef6df-421">ユーザーは、購入を取り消しますが、それらに送られます、 *CheckoutCancel.aspx*ページの場所が表示の順序が取り消されました。</span><span class="sxs-lookup"><span data-stu-id="ef6df-421">If the user decides to cancel the purchase, they will be directed to the *CheckoutCancel.aspx* page where they will see that their order has been cancelled.</span></span>

1. <span data-ttu-id="ef6df-422">という名前のページを開く*CheckoutCancel.aspx*で、*チェック アウト*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="ef6df-422">Open the page named *CheckoutCancel.aspx* in the *Checkout* folder.</span></span>
2. <span data-ttu-id="ef6df-423">既存のマークアップを次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-423">Replace the existing markup with the following:</span></span>   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample24.aspx)]

### <a name="handle-purchase-errors"></a><span data-ttu-id="ef6df-424">注文書のエラーを処理します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-424">Handle Purchase Errors</span></span>

<span data-ttu-id="ef6df-425">発注プロセス中にエラーを処理する、 *CheckoutError.aspx*ページ。</span><span class="sxs-lookup"><span data-stu-id="ef6df-425">Errors during the purchase process will be handled by the *CheckoutError.aspx* page.</span></span> <span data-ttu-id="ef6df-426">分離コード、 *CheckoutStart.aspx* ] ページで、 *CheckoutReview.aspx* ] ページで、および*CheckoutComplete.aspx*ページそれぞれをリダイレクトする、 *CheckoutError.aspx* ] ページで、エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-426">The code-behind of the *CheckoutStart.aspx* page, the *CheckoutReview.aspx* page, and the *CheckoutComplete.aspx* page will each redirect to the *CheckoutError.aspx* page if an error occurs.</span></span>

1. <span data-ttu-id="ef6df-427">という名前のページを開く*CheckoutError.aspx*で、*チェック アウト*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="ef6df-427">Open the page named *CheckoutError.aspx* in the *Checkout* folder.</span></span>
2. <span data-ttu-id="ef6df-428">既存のマークアップを次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-428">Replace the existing markup with the following:</span></span>   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample25.aspx)]

<span data-ttu-id="ef6df-429">*CheckoutError.aspx*チェック アウト プロセス中にエラーが発生したときにエラーの詳細にページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-429">The *CheckoutError.aspx* page is displayed with the error details when an error occurs during the checkout process.</span></span>

## <a name="running-the-application"></a><span data-ttu-id="ef6df-430">アプリケーションの実行</span><span class="sxs-lookup"><span data-stu-id="ef6df-430">Running the Application</span></span>

<span data-ttu-id="ef6df-431">製品を購入する方法を説明するアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-431">Run the application to see how to purchase products.</span></span> <span data-ttu-id="ef6df-432">実行する、PayPal のテスト環境に注意してください。</span><span class="sxs-lookup"><span data-stu-id="ef6df-432">Note that you will be running in the PayPal testing environment.</span></span> <span data-ttu-id="ef6df-433">実際のコストは交換されているされません。</span><span class="sxs-lookup"><span data-stu-id="ef6df-433">No actual money is being exchanged.</span></span>

1. <span data-ttu-id="ef6df-434">次のすべての Visual Studio で、ファイルが保存を確認します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-434">Make sure all your files are saved in Visual Studio.</span></span>
2. <span data-ttu-id="ef6df-435">Web ブラウザーを開きに移動[ https://developer.paypal.com](https://developer.paypal.com/)です。</span><span class="sxs-lookup"><span data-stu-id="ef6df-435">Open a Web browser and navigate to [https://developer.paypal.com](https://developer.paypal.com/).</span></span>
3. <span data-ttu-id="ef6df-436">このチュートリアルで先ほど作成した、PayPal 開発者アカウントでログインします。</span><span class="sxs-lookup"><span data-stu-id="ef6df-436">Login with your PayPal developer account that you created earlier in this tutorial.</span></span>  
   <span data-ttu-id="ef6df-437">PayPal の開発者のサンド ボックスにログインする必要があります。 [ https://developer.paypal.com ](https://developer.paypal.com/) express のチェック アウトをテストします。</span><span class="sxs-lookup"><span data-stu-id="ef6df-437">For PayPal's developer sandbox, you need to be logged in at [https://developer.paypal.com](https://developer.paypal.com/) to test express checkout.</span></span> <span data-ttu-id="ef6df-438">これは、テスト、PayPal の実稼働環境にない PayPal のサンド ボックスにのみ適用されます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-438">This only applies to PayPal's sandbox testing, not to PayPal's live environment.</span></span>
4. <span data-ttu-id="ef6df-439">Visual Studio で、キーを押して**f5 キーを押して**Wingtip Toys サンプル アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-439">In Visual Studio, press **F5** to run the Wingtip Toys sample application.</span></span>  
   <span data-ttu-id="ef6df-440">データベースが再構築後に、ブラウザーが開き、表示、 *Default.aspx*ページ。</span><span class="sxs-lookup"><span data-stu-id="ef6df-440">After the database rebuilds, the browser will open and show the *Default.aspx* page.</span></span>
5. <span data-ttu-id="ef6df-441">「自動車」など、製品カテゴリを選択し、をクリックして 3 つの異なる製品をショッピング カートに追加**をカートに追加**各製品の横にあります。</span><span class="sxs-lookup"><span data-stu-id="ef6df-441">Add three different products to the shopping cart by selecting the product category, such as "Cars" and then clicking **Add to Cart** next to each product.</span></span>  
   <span data-ttu-id="ef6df-442">ショッピング カートでは、選択した製品を表示します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-442">The shopping cart will display the product you have selected.</span></span>
6. <span data-ttu-id="ef6df-443">クリックして、 **PayPal**チェック アウトするボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="ef6df-443">Click the **PayPal** button to checkout.</span></span> 

    ![チェック アウトおよび PayPal のカートに支払い](checkout-and-payment-with-paypal/_static/image20.png)

   <span data-ttu-id="ef6df-445">チェック アウトするには、Wingtip Toys サンプル アプリケーションのユーザー アカウントを持っている必要があります。</span><span class="sxs-lookup"><span data-stu-id="ef6df-445">Checking out will require that you have a user account for the Wingtip Toys sample application.</span></span>
7. <span data-ttu-id="ef6df-446">クリックして、 **Google**既存 gmail.com などの電子メール アカウントでログインするページの右上のリンク。</span><span class="sxs-lookup"><span data-stu-id="ef6df-446">Click the **Google** link on the right of the page to log in with an existing gmail.com email account.</span></span>  
   <span data-ttu-id="ef6df-447">Gmail.com などのアカウントがあるない場合、テスト目的で 1 つを作成できます[www.gmail.com](https://www.gmail.com/)です。"Register"をクリックして、標準のローカル アカウントを使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-447">If you do not have a gmail.com account, you can create one for testing purposes at [www.gmail.com](https://www.gmail.com/). You can also use a standard local account by clicking "Register".</span></span> 

    ![チェック アウトおよび - PayPal の支払いにログインします。](checkout-and-payment-with-paypal/_static/image21.png)
8. <span data-ttu-id="ef6df-449">Gmail アカウントとパスワードでサインインします。</span><span class="sxs-lookup"><span data-stu-id="ef6df-449">Sign in with your gmail account and password.</span></span> 

    ![チェック アウトと支払 PayPal - gmail などのサインイン](checkout-and-payment-with-paypal/_static/image22.png)
9. <span data-ttu-id="ef6df-451">クリックして、**ログイン**Wingtip Toys サンプル アプリケーションのユーザー名、gmail アカウントを登録するにはボタン。</span><span class="sxs-lookup"><span data-stu-id="ef6df-451">Click the **Log in** button to register your gmail account with your Wingtip Toys sample application user name.</span></span> 

    ![チェック アウトおよび PayPal のレジスタのアカウントに支払い](checkout-and-payment-with-paypal/_static/image23.png)
10. <span data-ttu-id="ef6df-453">PayPal テスト サイトの追加、 **buyer**電子メール アドレス、このチュートリアルで先ほど作成したパスワードをクリックして、**ログで**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="ef6df-453">On the PayPal test site, add your **buyer** email address and password that you created earlier in this tutorial, then click the **Log In** button.</span></span> 

    ![チェック アウトおよび PayPal のサインインの PayPal の支払い](checkout-and-payment-with-paypal/_static/image24.png)
11. <span data-ttu-id="ef6df-455">PayPal ポリシーに同意し、をクリックして、**同意コンティニュ**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="ef6df-455">Agree to the PayPal policy and click the **Agree and Continue** button.</span></span>  
    <span data-ttu-id="ef6df-456">このページはのみ注には、この PayPal アカウントを使用する最初の時刻が表示されます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-456">Note that this page is only displayed the first time you use this PayPal account.</span></span> <span data-ttu-id="ef6df-457">もう一度テスト用のアカウントは、このことは、実際のコストは交換されませんに注意してください。</span><span class="sxs-lookup"><span data-stu-id="ef6df-457">Again note that this is a test account, no real money is exchanged.</span></span> 

    ![チェック アウトおよび PayPal の PayPal のポリシーでの支払い](checkout-and-payment-with-paypal/_static/image25.png)
12. <span data-ttu-id="ef6df-459">テスト環境の確認] ページをクリック PayPal の注文情報を確認**続行**です。</span><span class="sxs-lookup"><span data-stu-id="ef6df-459">Review the order information on the PayPal testing environment review page and click **Continue**.</span></span> 

    ![チェック アウトと支払 PayPal - 情報の確認](checkout-and-payment-with-paypal/_static/image26.png)
13. <span data-ttu-id="ef6df-461">*CheckoutReview.aspx* ] ページで注文量を確認して、生成された出荷先住所を表示します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-461">On the *CheckoutReview.aspx* page, verify the order amount and view the generated shipping address.</span></span> <span data-ttu-id="ef6df-462">次に、をクリックして、**注文完了**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="ef6df-462">Then, click the **Complete Order** button.</span></span> 

    ![チェック アウトと支払 PayPal の注文の確認](checkout-and-payment-with-paypal/_static/image27.png)
14. <span data-ttu-id="ef6df-464">**CheckoutComplete.aspx**支払トランザクション ID を持つページが表示されます</span><span class="sxs-lookup"><span data-stu-id="ef6df-464">The **CheckoutComplete.aspx** page is displayed with a payment transaction ID.</span></span> 

    ![チェック アウトおよび PayPal の完全なチェック アウトと支払い](checkout-and-payment-with-paypal/_static/image28.png)

<a id="ReviewDBWebForms"></a>
## <a name="reviewing-the-database"></a><span data-ttu-id="ef6df-466">データベースを確認します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-466">Reviewing the Database</span></span>

<span data-ttu-id="ef6df-467">Wingtip Toys サンプル アプリケーションのデータベース内のデータ更新を確認すると、アプリケーションを実行した後、アプリケーションは、製品の注文書を正常に録画がわかります。</span><span class="sxs-lookup"><span data-stu-id="ef6df-467">By reviewing the updated data in the Wingtip Toys sample application database after running the application, you can see that the application successfully recorded the purchase of the products.</span></span>

<span data-ttu-id="ef6df-468">含まれるデータを検査することができます、 *Wingtiptoys.mdf*データベース ファイルを使用して、**データベース エクスプ ローラー**ウィンドウ (**サーバー エクスプ ローラー** Visual Studio のウィンドウで) 場合と同様このチュートリアルのシリーズ前半。</span><span class="sxs-lookup"><span data-stu-id="ef6df-468">You can inspect the data contained in the *Wingtiptoys.mdf* database file by using the **Database Explorer** window (**Server Explorer** window in Visual Studio) as you did earlier in this tutorial series.</span></span>

1. <span data-ttu-id="ef6df-469">まだ開いている場合は、ブラウザー ウィンドウを閉じます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-469">Close the browser window if it is still open.</span></span>
2. <span data-ttu-id="ef6df-470">Visual Studio で、選択、 **[すべてのファイル**の上部にあるアイコン**ソリューション エクスプ ローラー**を展開できるようにする、**アプリ\_データ**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="ef6df-470">In Visual Studio, select the **Show All Files** icon at the top of **Solution Explorer** to allow you to expand the **App\_Data** folder.</span></span>
3. <span data-ttu-id="ef6df-471">展開して、**アプリ\_データ**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="ef6df-471">Expand the **App\_Data** folder.</span></span>  
 <span data-ttu-id="ef6df-472">選択する必要があります、 **[すべてのファイル**フォルダーのアイコン。</span><span class="sxs-lookup"><span data-stu-id="ef6df-472">You may need to select the **Show All Files** icon for the folder.</span></span>
4. <span data-ttu-id="ef6df-473">右クリックし、 *Wingtiptoys.mdf*データベース ファイルと選択**開く**です。</span><span class="sxs-lookup"><span data-stu-id="ef6df-473">Right-click the *Wingtiptoys.mdf* database file and select **Open**.</span></span>  
    <span data-ttu-id="ef6df-474">**サーバー エクスプ ローラー**が表示されます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-474">**Server Explorer** is displayed.</span></span>
5. <span data-ttu-id="ef6df-475">展開して、**テーブル**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="ef6df-475">Expand the **Tables** folder.</span></span>
6. <span data-ttu-id="ef6df-476">右クリックし、 **Orders**テーブルを選択して**テーブル データの表示**です。</span><span class="sxs-lookup"><span data-stu-id="ef6df-476">Right-click the **Orders**table and select **Show Table Data**.</span></span>  
 <span data-ttu-id="ef6df-477">**Orders**テーブルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-477">The **Orders** table is displayed.</span></span>
7. <span data-ttu-id="ef6df-478">確認、 **PaymentTransactionID**成功したトランザクションを確認する列。</span><span class="sxs-lookup"><span data-stu-id="ef6df-478">Review the **PaymentTransactionID** column to confirm successful transactions.</span></span> 

    ![チェック アウトおよび PayPal のレビュー データベースでの支払い](checkout-and-payment-with-paypal/_static/image29.png)
8. <span data-ttu-id="ef6df-480">閉じる、 **Orders**テーブル ウィンドウです。</span><span class="sxs-lookup"><span data-stu-id="ef6df-480">Close the **Orders** table window.</span></span>
9. <span data-ttu-id="ef6df-481">サーバー エクスプ ローラーで右クリックし、 **OrderDetails**テーブルを選択して**テーブル データの表示**です。</span><span class="sxs-lookup"><span data-stu-id="ef6df-481">In the Server Explorer, right-click the **OrderDetails** table and select **Show Table Data**.</span></span>
10. <span data-ttu-id="ef6df-482">確認、`OrderId`と`Username`の値が、 **OrderDetails**テーブル。</span><span class="sxs-lookup"><span data-stu-id="ef6df-482">Review the `OrderId` and `Username` values in the **OrderDetails** table.</span></span> <span data-ttu-id="ef6df-483">これらの値に一致するメモ、`OrderId`と`Username`に含まれる値、 **Orders**テーブル。</span><span class="sxs-lookup"><span data-stu-id="ef6df-483">Note that these values match the `OrderId` and `Username` values included in the **Orders** table.</span></span>
11. <span data-ttu-id="ef6df-484">閉じる、 **OrderDetails**テーブル ウィンドウです。</span><span class="sxs-lookup"><span data-stu-id="ef6df-484">Close the **OrderDetails** table window.</span></span>
12. <span data-ttu-id="ef6df-485">Wingtip Toys のデータベース ファイルを右クリックして (*Wingtiptoys.mdf*) を選択して**接続を閉じる**です。</span><span class="sxs-lookup"><span data-stu-id="ef6df-485">Right-click the Wingtip Toys database file (*Wingtiptoys.mdf*) and select **Close Connection**.</span></span>
13. <span data-ttu-id="ef6df-486">表示されない場合、**ソリューション エクスプ ローラー**ウィンドウで、をクリックして**ソリューション エクスプ ローラー**の下部にある、**サーバー エクスプ ローラー**を表示するウィンドウ、**ソリューション エクスプ ローラー**もう一度です。</span><span class="sxs-lookup"><span data-stu-id="ef6df-486">If you do not see the **Solution Explorer** window, click **Solution Explorer** at the bottom of the **Server Explorer** window to show the **Solution Explorer** again.</span></span>

## <a name="summary"></a><span data-ttu-id="ef6df-487">まとめ</span><span class="sxs-lookup"><span data-stu-id="ef6df-487">Summary</span></span>

<span data-ttu-id="ef6df-488">このチュートリアルでは、製品の注文書を追跡するためには、注文と注文詳細スキーマを追加します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-488">In this tutorial you added order and order detail schemas to track the purchase of products.</span></span> <span data-ttu-id="ef6df-489">また、PayPal 機能は、Wingtip Toys のサンプル アプリケーションに統合されます。</span><span class="sxs-lookup"><span data-stu-id="ef6df-489">You also integrated PayPal functionality into the Wingtip Toys sample application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ef6df-490">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="ef6df-490">Additional Resources</span></span>

<span data-ttu-id="ef6df-491">[ASP.NET の構成の概要](https://msdn.microsoft.com/library/ms178683(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="ef6df-491">[ASP.NET Configuration Overview](https://msdn.microsoft.com/library/ms178683(v=vs.100).aspx)</span></span>  
[<span data-ttu-id="ef6df-492">Azure App Service のメンバーシップ、OAuth、SQL データベースでのセキュリティで保護された ASP.NET Web フォーム アプリケーションを展開します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-492">Deploy a Secure ASP.NET Web Forms App with Membership, OAuth, and SQL Database to Azure App Service</span></span>](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[<span data-ttu-id="ef6df-493">Microsoft Azure の無料試用版</span><span class="sxs-lookup"><span data-stu-id="ef6df-493">Microsoft Azure - Free Trial</span></span>](https://azure.microsoft.com/pricing/free-trial/)

## <a name="disclaimer"></a><span data-ttu-id="ef6df-494">免責情報</span><span class="sxs-lookup"><span data-stu-id="ef6df-494">Disclaimer</span></span>

<span data-ttu-id="ef6df-495">このチュートリアルには、サンプル コードが含まれています。</span><span class="sxs-lookup"><span data-stu-id="ef6df-495">This tutorial contains sample code.</span></span> <span data-ttu-id="ef6df-496">このようなサンプル コードには、どのような種類の保証も伴わず「現状有姿を示します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-496">Such sample code is provided "as is" without warranty of any kind.</span></span> <span data-ttu-id="ef6df-497">同様に、Microsoft は、正確性、整合性、または、サンプル コードの品質保証されません。</span><span class="sxs-lookup"><span data-stu-id="ef6df-497">Accordingly, Microsoft does not guarantee the accuracy, integrity, or quality of the sample code.</span></span> <span data-ttu-id="ef6df-498">うえで、サンプル コードを使用するものとします。</span><span class="sxs-lookup"><span data-stu-id="ef6df-498">You agree to use the sample code at your own risk.</span></span> <span data-ttu-id="ef6df-499">状況においても Microsoft 責任を負いかねますに任意の方法で、サンプル コードは、コンテンツなどですが、これらに、エラーまたは不作為サンプル コード、コンテンツ、または任意の損失や任意の種類のすべてのサンプル コードを使用した結果として発生する破損に限定されません。</span><span class="sxs-lookup"><span data-stu-id="ef6df-499">Under no circumstances will Microsoft be liable to you in any way for any sample code, content, including but not limited to, any errors or omissions in any sample code, content, or any loss or damage of any kind incurred as a result of the use of any sample code.</span></span> <span data-ttu-id="ef6df-500">無償通知が表示され、保存および免責 Microsoft から、すべてが失われる、信頼性情報の損失、負傷事故やの kind 含め、これらに限定して occasioned またはマテリアルを投稿することにより発生する破損の影響を与えません同意しないでください。送信使用するかに限らず見解をそこに依存します。</span><span class="sxs-lookup"><span data-stu-id="ef6df-500">You are hereby notified and do hereby agree to indemnify, save and hold Microsoft harmless from and against any and all loss, claims of loss, injury or damage of any kind including, without limitation, those occasioned by or arising from material that you post, transmit, use or rely on including, but not limited to, the views expressed therein.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ef6df-501">[前へ](shopping-cart.md)
> [次へ](membership-and-administration.md)</span><span class="sxs-lookup"><span data-stu-id="ef6df-501">[Previous](shopping-cart.md)
[Next](membership-and-administration.md)</span></span>
