---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
title: 精算と PayPal による支払い |Microsoft Docs
author: Erikre
description: このチュートリアル シリーズでは、私たちの ASP.NET 4.5 と Microsoft Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 664ec95e-b0c9-4f43-a39f-798d0f2a7e08
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
msc.type: authoredcontent
ms.openlocfilehash: e516bcccdecce72d4fa6c705b0227ce873429e3c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386670"
---
<a name="checkout-and-payment-with-paypal"></a><span data-ttu-id="e6ade-103">精算と PayPal による支払い</span><span class="sxs-lookup"><span data-stu-id="e6ade-103">Checkout and Payment with PayPal</span></span>
====================
<span data-ttu-id="e6ade-104">によって[Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="e6ade-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="e6ade-105">[Wingtip Toys のサンプル プロジェクト (c#) をダウンロード](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)または[電子書籍 (PDF) をダウンロード](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="e6ade-105">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

> <span data-ttu-id="e6ade-106">このチュートリアル シリーズでは、Web 用 ASP.NET 4.5 と Microsoft Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-106">This tutorial series will teach you the basics of building an ASP.NET Web Forms application using ASP.NET 4.5 and Microsoft Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="e6ade-107">Visual Studio 2013[プロジェクトと c# ソース コード](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)このチュートリアル シリーズをと共に使用できます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-107">A Visual Studio 2013 [project with C# source code](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) is available to accompany this tutorial series.</span></span>


<span data-ttu-id="e6ade-108">このチュートリアルでは、Wingtip Toys のサンプル アプリケーションは、ユーザーの承認、登録、および PayPal を使用して支払いを変更する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-108">This tutorial describes how to modify the Wingtip Toys sample application to include user authorization, registration, and payment using PayPal.</span></span> <span data-ttu-id="e6ade-109">ログオンしているユーザーのみが製品を購入するための承認があります。</span><span class="sxs-lookup"><span data-stu-id="e6ade-109">Only users who are logged in will have authorization to purchase products.</span></span> <span data-ttu-id="e6ade-110">既に ASP.NET 4.5 Web フォーム プロジェクト テンプレートの組み込みのユーザーの登録機能にはでは必要なものの多くが含まれます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-110">The ASP.NET 4.5 Web Forms project template's built-in user registration functionality already includes much of what you need.</span></span> <span data-ttu-id="e6ade-111">PayPal Express のチェック アウト機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-111">You will add PayPal Express Checkout functionality.</span></span> <span data-ttu-id="e6ade-112">このチュートリアルで使用する、PayPal 開発者がテスト環境では、実際の資金は転送されません。</span><span class="sxs-lookup"><span data-stu-id="e6ade-112">In this tutorial you be using the PayPal developer testing environment, so no actual funds will be transferred.</span></span> <span data-ttu-id="e6ade-113">チュートリアルの最後に、ショッピング カート、チェック アウト ボタンをクリックして転送するデータ、PayPal テスト web サイトに追加する製品を選択して、アプリケーションをテストします。</span><span class="sxs-lookup"><span data-stu-id="e6ade-113">At the end of the tutorial, you will test the application by selecting products to add to the shopping cart, clicking the checkout button, and transferring data to the PayPal testing web site.</span></span> <span data-ttu-id="e6ade-114">PayPal テスト web サイトでは、配布と支払い情報を確認しを確認し、購入を完了しますローカル Wingtip Toys のサンプル アプリケーションに戻りますされます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-114">On the PayPal testing web site, you will confirm your shipping and payment information and then return to the local Wingtip Toys sample application to confirm and complete the purchase.</span></span>

<span data-ttu-id="e6ade-115">そのアドレスのスケーラビリティとセキュリティ、オンライン ショッピングに特化したいくつかのサード パーティの支払いを経験豊富なプロセッサがあります。</span><span class="sxs-lookup"><span data-stu-id="e6ade-115">There are several experienced third-party payment processors that specialize in online shopping that address scalability and security.</span></span> <span data-ttu-id="e6ade-116">ASP.NET 開発者は、ショッピングを実装して、ソリューションを購入する前に、サード パーティの支払いソリューションを利用するメリットを検討してください。</span><span class="sxs-lookup"><span data-stu-id="e6ade-116">ASP.NET developers should consider the advantages of utilizing a third party payment solution before implementing a shopping and purchasing solution.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="e6ade-117">Wingtip Toys のサンプル アプリケーションは、ASP.NET web 開発者に特定の ASP.NET の概念と使用可能な機能を示すために設計されました。</span><span class="sxs-lookup"><span data-stu-id="e6ade-117">The Wingtip Toys sample application was designed to shown specific ASP.NET concepts and features available to ASP.NET web developers.</span></span> <span data-ttu-id="e6ade-118">このサンプル アプリケーションは、スケーラビリティとセキュリティに関してどのような状況は最適化されていませんでした。</span><span class="sxs-lookup"><span data-stu-id="e6ade-118">This sample application was not optimized for all possible circumstances in regard to scalability and security.</span></span>


## <a name="what-youll-learn"></a><span data-ttu-id="e6ade-119">学習内容。</span><span class="sxs-lookup"><span data-stu-id="e6ade-119">What you'll learn:</span></span>

- <span data-ttu-id="e6ade-120">フォルダー内の特定のページへのアクセスを制限する方法。</span><span class="sxs-lookup"><span data-stu-id="e6ade-120">How to restrict access to specific pages in a folder.</span></span>
- <span data-ttu-id="e6ade-121">匿名のショッピング カートから既知のショッピング カートを作成する方法。</span><span class="sxs-lookup"><span data-stu-id="e6ade-121">How to create a known shopping cart from an anonymous shopping cart.</span></span>
- <span data-ttu-id="e6ade-122">プロジェクトの SSL を有効にする方法。</span><span class="sxs-lookup"><span data-stu-id="e6ade-122">How to enable SSL for the project.</span></span>
- <span data-ttu-id="e6ade-123">プロジェクトに、OAuth プロバイダーを追加する方法。</span><span class="sxs-lookup"><span data-stu-id="e6ade-123">How to add an OAuth provider to the project.</span></span>
- <span data-ttu-id="e6ade-124">PayPal を使用して、PayPal のテスト環境を使用して製品を購入する方法。</span><span class="sxs-lookup"><span data-stu-id="e6ade-124">How to use PayPal to purchase products using the PayPal testing environment.</span></span>
- <span data-ttu-id="e6ade-125">あれば PayPal から詳細を表示する方法、 **DetailsView**コントロール。</span><span class="sxs-lookup"><span data-stu-id="e6ade-125">How to display details from PayPal in a **DetailsView** control.</span></span>
- <span data-ttu-id="e6ade-126">PayPal から取得の詳細と、Wingtip Toys アプリケーションのデータベースを更新する方法。</span><span class="sxs-lookup"><span data-stu-id="e6ade-126">How to update the database of the Wingtip Toys application with details obtained from PayPal.</span></span>

## <a name="adding-order-tracking"></a><span data-ttu-id="e6ade-127">注文の追跡を追加します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-127">Adding Order Tracking</span></span>

<span data-ttu-id="e6ade-128">このチュートリアルでは、ユーザーが作成された注文からデータを追跡するために 2 つの新しいクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-128">In this tutorial, you'll create two new classes to track data from the order a user has created.</span></span> <span data-ttu-id="e6ade-129">クラスでは、発送情報、発注の合計、および支払いの確認に関するデータを追跡します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-129">The classes will track data regarding shipping information, purchase total, and payment confirmation.</span></span>

### <a name="add-the-order-and-orderdetail-model-classes"></a><span data-ttu-id="e6ade-130">順序と OrderDetail モデル クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-130">Add the Order and OrderDetail Model Classes</span></span>

<span data-ttu-id="e6ade-131">このチュートリアルのシリーズの前半では、カテゴリ、製品のスキーマを定義し、ショッピング カートのアイテムを作成して、 `Category`、 `Product`、および`CartItem`クラス、*モデル*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="e6ade-131">Earlier in this tutorial series, you defined the schema for categories, products, and shopping cart items by creating the `Category`, `Product`, and `CartItem` classes in the *Models* folder.</span></span> <span data-ttu-id="e6ade-132">製品の注文や注文の詳細については、スキーマを定義する 2 つの新しいクラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-132">Now you will add two new classes to define the schema for the product order and the details of the order.</span></span>

1. <span data-ttu-id="e6ade-133">**モデル**フォルダー、という名前の新しいクラスを追加*Order.cs*します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-133">In the **Models** folder, add a new class named *Order.cs*.</span></span>   
   <span data-ttu-id="e6ade-134">新しいクラス ファイルがエディターで表示されます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-134">The new class file is displayed in the editor.</span></span>
2. <span data-ttu-id="e6ade-135">次のように、既定のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-135">Replace the default code with the following:</span></span>   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample1.cs)]
3. <span data-ttu-id="e6ade-136">追加、 *OrderDetail.cs*クラスを*モデル*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="e6ade-136">Add an *OrderDetail.cs* class to the *Models* folder.</span></span>
4. <span data-ttu-id="e6ade-137">既定のコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-137">Replace the default code with the following code:</span></span>   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample2.cs)]

<span data-ttu-id="e6ade-138">`Order`と`OrderDetail`クラスは、購入および出荷するため、注文情報を定義するスキーマを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-138">The `Order` and `OrderDetail` classes contain the schema to define the order information used for purchasing and shipping.</span></span>

<span data-ttu-id="e6ade-139">さらに、エンティティ クラスは、管理し、データベースへのデータ アクセスを提供する、データベース コンテキスト クラスを更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e6ade-139">In addition, you will need to update the database context class that manages the entity classes and that provides data access to the database.</span></span> <span data-ttu-id="e6ade-140">これを行うには、新しく作成された注文を追加して`OrderDetail`モデル クラスを`ProductContext`クラス。</span><span class="sxs-lookup"><span data-stu-id="e6ade-140">To do this, you will add the newly created Order and `OrderDetail` model classes to `ProductContext` class.</span></span>

1. <span data-ttu-id="e6ade-141">**ソリューション エクスプ ローラー**を検索して開く、 *ProductContext.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="e6ade-141">In **Solution Explorer**, find and open the *ProductContext.cs* file.</span></span>
2. <span data-ttu-id="e6ade-142">強調表示されたコードを追加、 *ProductContext.cs*次に示すようにファイルします。</span><span class="sxs-lookup"><span data-stu-id="e6ade-142">Add the highlighted code to the *ProductContext.cs* file as shown below:</span></span>   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample3.cs?highlight=14-15)]

<span data-ttu-id="e6ade-143">コードで、このチュートリアル シリーズで説明したよう、 *ProductContext.cs*ファイルを追加、`System.Data.Entity`名前空間、Entity Framework のすべてのコア機能にアクセスするようにします。</span><span class="sxs-lookup"><span data-stu-id="e6ade-143">As mentioned previously in this tutorial series, the code in the *ProductContext.cs* file adds the `System.Data.Entity` namespace so that you have access to all the core functionality of the Entity Framework.</span></span> <span data-ttu-id="e6ade-144">この機能には、クエリ、挿入、更新、および厳密に型指定されたオブジェクトを使用してデータを削除する機能が含まれています。</span><span class="sxs-lookup"><span data-stu-id="e6ade-144">This functionality includes the capability to query, insert, update, and delete data by working with strongly typed objects.</span></span> <span data-ttu-id="e6ade-145">上記のコードで、`ProductContext`クラスを新しく追加した Entity Framework のアクセス権を追加します`Order`と`OrderDetail`クラス。</span><span class="sxs-lookup"><span data-stu-id="e6ade-145">The above code in the `ProductContext` class adds Entity Framework access to the newly added `Order` and `OrderDetail` classes.</span></span>

## <a name="adding-checkout-access"></a><span data-ttu-id="e6ade-146">チェック アウトのアクセスを追加します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-146">Adding Checkout Access</span></span>

<span data-ttu-id="e6ade-147">Wingtip Toys のサンプル アプリケーションは、匿名ユーザーをレビューして、ショッピング カートに製品を追加できます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-147">The Wingtip Toys sample application allows anonymous users to review and add products to a shopping cart.</span></span> <span data-ttu-id="e6ade-148">ただし、匿名ユーザーが選択したショッピング カートに追加された、製品を購入するときに、必要がありますログオン サイトにします。</span><span class="sxs-lookup"><span data-stu-id="e6ade-148">However, when anonymous users choose to purchase the products they added to the shopping cart, they must log on to the site.</span></span> <span data-ttu-id="e6ade-149">ログオンするがチェック アウトを処理し、購入のプロセスは、Web アプリケーションの制限されたページにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-149">Once they have logged on, they can access the restricted pages of the Web application that handle the checkout and purchase process.</span></span> <span data-ttu-id="e6ade-150">これらの制限付きのページに含まれる、*チェック アウト*アプリケーションのフォルダー。</span><span class="sxs-lookup"><span data-stu-id="e6ade-150">These restricted pages are contained in the *Checkout* folder of the application.</span></span>

### <a name="add-a-checkout-folder-and-pages"></a><span data-ttu-id="e6ade-151">チェック アウト フォルダーとページを追加します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-151">Add a Checkout Folder and Pages</span></span>

<span data-ttu-id="e6ade-152">作成、*チェック アウト*フォルダーとがチェック アウト プロセス中に、顧客に表示されるページ。</span><span class="sxs-lookup"><span data-stu-id="e6ade-152">You will now create the *Checkout* folder and the pages in it that the customer will see during the checkout process.</span></span> <span data-ttu-id="e6ade-153">このチュートリアルの後半でこれらのページが更新されます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-153">You will update these pages later in this tutorial.</span></span>

1. <span data-ttu-id="e6ade-154">プロジェクト名を右クリックして (**Wingtip Toys**) で**ソリューション エクスプ ローラー**選択**新しいフォルダーを追加**します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-154">Right-click the project name (**Wingtip Toys**) in **Solution Explorer** and select **Add a New Folder**.</span></span> 

    ![精算と PayPal の新しいフォルダーによる支払い](checkout-and-payment-with-paypal/_static/image1.png)
2. <span data-ttu-id="e6ade-156">新しいフォルダーの名前*チェック アウト*します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-156">Name the new folder *Checkout*.</span></span>
3. <span data-ttu-id="e6ade-157">右クリックし、*チェック アウト*クリックしてフォルダー**追加**-&gt;**新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-157">Right-click the *Checkout* folder and then select **Add**-&gt;**New Item**.</span></span> 

    ![精算と PayPal - 新しい項目による支払い](checkout-and-payment-with-paypal/_static/image2.png)
4. <span data-ttu-id="e6ade-159">**[新しい項目の追加]** ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-159">The **Add New Item** dialog box is displayed.</span></span>
5. <span data-ttu-id="e6ade-160">選択、 **Visual c#**  - &gt; **Web**左側のテンプレート グループ。</span><span class="sxs-lookup"><span data-stu-id="e6ade-160">Select the **Visual C#** -&gt; **Web** templates group on the left.</span></span> <span data-ttu-id="e6ade-161">次に、中央のウィンドウから次のように選択します。**マスター ページを使用した Web フォーム**名前を付けます*CheckoutStart.aspx*します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-161">Then, from the middle pane, select **Web Form with Master Page**and name it *CheckoutStart.aspx*.</span></span> 

    ![精算と PayPal - による支払いが新しい項目 ダイアログ ボックスを追加します。](checkout-and-payment-with-paypal/_static/image3.png)
6. <span data-ttu-id="e6ade-163">同様に、選択、 *Site.Master*のマスター ページとしてファイル。</span><span class="sxs-lookup"><span data-stu-id="e6ade-163">As before, select the *Site.Master* file as the master page.</span></span>
7. <span data-ttu-id="e6ade-164">次の追加ページを追加、*チェック アウト*前述の手順を使用してフォルダー。</span><span class="sxs-lookup"><span data-stu-id="e6ade-164">Add the following additional pages to the *Checkout* folder using the same steps above:</span></span>   

    - <span data-ttu-id="e6ade-165">CheckoutReview.aspx</span><span class="sxs-lookup"><span data-stu-id="e6ade-165">CheckoutReview.aspx</span></span>
    - <span data-ttu-id="e6ade-166">CheckoutComplete.aspx</span><span class="sxs-lookup"><span data-stu-id="e6ade-166">CheckoutComplete.aspx</span></span>
    - <span data-ttu-id="e6ade-167">CheckoutCancel.aspx</span><span class="sxs-lookup"><span data-stu-id="e6ade-167">CheckoutCancel.aspx</span></span>
    - <span data-ttu-id="e6ade-168">CheckoutError.aspx</span><span class="sxs-lookup"><span data-stu-id="e6ade-168">CheckoutError.aspx</span></span>

### <a name="add-a-webconfig-file"></a><span data-ttu-id="e6ade-169">Web.config ファイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-169">Add a Web.config File</span></span>

<span data-ttu-id="e6ade-170">新しいを追加して*Web.config*ファイルを*チェック アウト*フォルダー、ことができます、フォルダーに含まれるすべてのページにアクセスを制限します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-170">By adding a new *Web.config* file to the *Checkout* folder, you will be able to restrict access to all the pages contained in the folder.</span></span>

1. <span data-ttu-id="e6ade-171">右クリックし、*チェック アウト*フォルダーと選択**追加** - &gt; **新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-171">Right-click the *Checkout* folder and select **Add** -&gt; **New Item**.</span></span>  
   <span data-ttu-id="e6ade-172">**[新しい項目の追加]** ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-172">The **Add New Item** dialog box is displayed.</span></span>
2. <span data-ttu-id="e6ade-173">選択、 **Visual c#**  - &gt; **Web**左側のテンプレート グループ。</span><span class="sxs-lookup"><span data-stu-id="e6ade-173">Select the **Visual C#** -&gt; **Web** templates group on the left.</span></span> <span data-ttu-id="e6ade-174">次に、中央のウィンドウから次のように選択します。 **Web 構成ファイル**、の既定の名前をそのまま使用*Web.config*、し、**追加**します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-174">Then, from the middle pane, select **Web Configuration File**, accept the default name of *Web.config*, and then select **Add**.</span></span>
3. <span data-ttu-id="e6ade-175">既存の XML コンテンツを置き換える、 *Web.config*を次のファイル。</span><span class="sxs-lookup"><span data-stu-id="e6ade-175">Replace the existing XML content in the *Web.config* file with the following:</span></span>  

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample4.xml)]
4. <span data-ttu-id="e6ade-176">保存、 *Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="e6ade-176">Save the *Web.config* file.</span></span>

<span data-ttu-id="e6ade-177">*Web.config*ファイルでは、Web アプリケーションのすべての不明なユーザーにする必要がありますに含まれるページへのアクセスが拒否されることを指定します、*チェック アウト*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="e6ade-177">The *Web.config* file specifies that all unknown users of the Web application must be denied access to the pages contained in the *Checkout* folder.</span></span> <span data-ttu-id="e6ade-178">ただし場合は、ユーザーは、アカウントが登録され、ログオンしている、既知のユーザーになります。 また、があります内のページへのアクセス、*チェック アウト*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="e6ade-178">However, if the user has registered an account and is logged on, they will be a known user and will have access to the pages in the *Checkout* folder.</span></span>

<span data-ttu-id="e6ade-179">ASP.NET の構成が、階層構造に従うことを確認することが重要で各*Web.config*ファイル内にあるフォルダーとその下の子ディレクトリのすべての構成設定が適用されます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-179">It's important to note that ASP.NET configuration follows a hierarchy, where each *Web.config* file applies configuration settings to the folder that it is in and to all of the child directories below it.</span></span>

<a id="SSLWebForms"></a>
## <a name="enable-ssl-for-the-project"></a><span data-ttu-id="e6ade-180">プロジェクトの SSL を有効にします。</span><span class="sxs-lookup"><span data-stu-id="e6ade-180">Enable SSL for the Project</span></span>

 <span data-ttu-id="e6ade-181">セキュリティで保護された Sockets Layer (SSL) は、Web サーバーと Web クライアントが暗号化を使用してより安全に通信できるように定義されているプロトコルです。</span><span class="sxs-lookup"><span data-stu-id="e6ade-181">Secure Sockets Layer (SSL) is a protocol defined to allow Web servers and Web clients to communicate more securely through the use of encryption.</span></span> <span data-ttu-id="e6ade-182">SSL を使用しない場合、クライアントとサーバー間で送信されるデータは、ネットワークに物理的にアクセスできるすべてのユーザーによるパケット スニッフィングを開いています。</span><span class="sxs-lookup"><span data-stu-id="e6ade-182">When SSL is not used, data sent between the client and server is open to packet sniffing by anyone with physical access to the network.</span></span> <span data-ttu-id="e6ade-183">また、いくつかの一般的な認証方式、プレーンな HTTP 経由でセキュリティで保護されたもなりました。</span><span class="sxs-lookup"><span data-stu-id="e6ade-183">Additionally, several common authentication schemes are not secure over plain HTTP.</span></span> <span data-ttu-id="e6ade-184">具体的には、基本認証とフォーム認証、暗号化されていない資格情報を送信します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-184">In particular, Basic authentication and forms authentication send unencrypted credentials.</span></span> <span data-ttu-id="e6ade-185">セキュリティで保護するには、これらの認証方式は、SSL を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e6ade-185">To be secure, these authentication schemes must use SSL.</span></span> 

1. <span data-ttu-id="e6ade-186">**ソリューション エクスプ ローラー**、 をクリックして、 **WingtipToys**プロジェクト、キーを押して**F4**を表示する、**プロパティ**ウィンドウ。</span><span class="sxs-lookup"><span data-stu-id="e6ade-186">In **Solution Explorer**, click the **WingtipToys** project, then press **F4** to display the **Properties** window.</span></span>
2. <span data-ttu-id="e6ade-187">変更**SSL が有効になっている**に`true`します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-187">Change **SSL Enabled** to `true`.</span></span>
3. <span data-ttu-id="e6ade-188">コピー、 **SSL URL**後で使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="e6ade-188">Copy the **SSL URL** so you can use it later.</span></span>   
 <span data-ttu-id="e6ade-189">SSL url は、 `https://localhost:44300/` (下図参照) と SSL Web サイトを以前作成した場合を除き、します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-189">The SSL URL will be `https://localhost:44300/` unless you've previously created SSL Web Sites (as shown below).</span></span>   
    <span data-ttu-id="e6ade-190">![プロジェクト プロパティ](checkout-and-payment-with-paypal/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="e6ade-190">![Project Properties](checkout-and-payment-with-paypal/_static/image4.png)</span></span>
4. <span data-ttu-id="e6ade-191">**ソリューション エクスプ ローラー**を右クリックして、 **WingtipToys**プロジェクトし、クリックして**プロパティ**。</span><span class="sxs-lookup"><span data-stu-id="e6ade-191">In **Solution Explorer**, right click the **WingtipToys** project and click **Properties**.</span></span>
5. <span data-ttu-id="e6ade-192">左側のタブで次のようにクリックします。 **Web**します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-192">In the left tab, click **Web**.</span></span>
6. <span data-ttu-id="e6ade-193">変更、**プロジェクト Url**を使用する、 **SSL URL**以前に保存します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-193">Change the **Project Url** to use the **SSL URL** that you saved earlier.</span></span>   
    <span data-ttu-id="e6ade-194">![プロジェクトの Web プロパティ](checkout-and-payment-with-paypal/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="e6ade-194">![Project Web Properties](checkout-and-payment-with-paypal/_static/image5.png)</span></span>
7. <span data-ttu-id="e6ade-195">キーを押してページを保存**CTRL + S**します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-195">Save the page by pressing **CTRL+S**.</span></span>
8. <span data-ttu-id="e6ade-196">**Ctrl キーを押しながら F5 キーを押して**アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-196">Press **Ctrl+F5** to run the application.</span></span> <span data-ttu-id="e6ade-197">Visual Studio の SSL の警告を回避できるようにするオプションが表示されます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-197">Visual Studio will display an option to allow you to avoid SSL warnings.</span></span>
9. <span data-ttu-id="e6ade-198">をクリックして**はい**IIS Express SSL 証明書を信頼して続行します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-198">Click **Yes** to trust the IIS Express SSL certificate and continue.</span></span>   
    <span data-ttu-id="e6ade-199">![IIS Express SSL 証明書の詳細](checkout-and-payment-with-paypal/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="e6ade-199">![IIS Express SSL certificate details](checkout-and-payment-with-paypal/_static/image6.png)</span></span>  
 <span data-ttu-id="e6ade-200">セキュリティの警告が表示されます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-200">A Security Warning is displayed.</span></span>
10. <span data-ttu-id="e6ade-201">クリックして**はい**localhost 証明書をインストールします。</span><span class="sxs-lookup"><span data-stu-id="e6ade-201">Click **Yes** to install the certificate to your localhost.</span></span>   
    <span data-ttu-id="e6ade-202">![セキュリティ警告 ダイアログ ボックス](checkout-and-payment-with-paypal/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="e6ade-202">![Security Warning dialog box](checkout-and-payment-with-paypal/_static/image7.png)</span></span>  
 <span data-ttu-id="e6ade-203">ブラウザー ウィンドウが表示されます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-203">The browser window will be displayed.</span></span>

<span data-ttu-id="e6ade-204">これで SSL を使用してローカルで Web アプリケーションを簡単にテストできます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-204">You can now easily test your Web application locally using SSL.</span></span>

<a id="OAuthWebForms"></a>
## <a name="add-an-oauth-20-provider"></a><span data-ttu-id="e6ade-205">OAuth 2.0 プロバイダーを追加します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-205">Add an OAuth 2.0 Provider</span></span>

<span data-ttu-id="e6ade-206">ASP.NET Web フォームでは、メンバーシップと認証のオプションの強化を提供します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-206">ASP.NET Web Forms provides enhanced options for membership and authentication.</span></span> <span data-ttu-id="e6ade-207">これらの拡張機能には、OAuth が含まれます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-207">These enhancements include OAuth.</span></span> <span data-ttu-id="e6ade-208">OAuth は、web、モバイル、およびデスクトップ アプリケーションからシンプルで標準的な方法で安全に認証を許可するオープン プロトコルです。</span><span class="sxs-lookup"><span data-stu-id="e6ade-208">OAuth is an open protocol that allows secure authorization in a simple and standard method from web, mobile, and desktop applications.</span></span> <span data-ttu-id="e6ade-209">ASP.NET Web フォーム テンプレートは OAuth を使用して、認証プロバイダーとして Facebook、Twitter、Google、Microsoft を公開します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-209">The ASP.NET Web Forms template uses OAuth to expose Facebook, Twitter, Google and Microsoft as authentication providers.</span></span> <span data-ttu-id="e6ade-210">このチュートリアルでは、認証プロバイダーとして Google のみを使用して、任意のプロバイダーを使用するコードを簡単に変更できます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-210">Although this tutorial uses only Google as the authentication provider, you can easily modify the code to use any of the providers.</span></span> <span data-ttu-id="e6ade-211">その他のプロバイダーを実装する手順は、このチュートリアルに表示される手順とよく似ています。</span><span class="sxs-lookup"><span data-stu-id="e6ade-211">The steps to implement other providers are very similar to the steps you will see in this tutorial.</span></span>

<span data-ttu-id="e6ade-212">認証に加えて、このチュートリアルはロールを使用して承認を実装します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-212">In addition to authentication, the tutorial will also use roles to implement authorization.</span></span> <span data-ttu-id="e6ade-213">追加したユーザーのみ、`canEdit`ロールは、データを変更することになります (作成、編集、または連絡先を削除) します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-213">Only those users you add to the `canEdit` role will be able to change data (create, edit, or delete contacts).</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="e6ade-214">Windows Live アプリケーションでは、ログインをテストするため、ローカルの web サイトの URL を使用することはできませんので作業用の web サイトでライブ URL のみ受け入れます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-214">Windows Live applications only accept a live URL for a working website, so you cannot use a local website URL for testing logins.</span></span>


<span data-ttu-id="e6ade-215">次の手順では Google の認証プロバイダーを追加することを許可します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-215">The following steps will allow you to add a Google authentication provider.</span></span>

1. <span data-ttu-id="e6ade-216">開く、*アプリ\_Start\Startup.Auth.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="e6ade-216">Open the *App\_Start\Startup.Auth.cs* file.</span></span>
2. <span data-ttu-id="e6ade-217">コメント文字を削除、`app.UseGoogleAuthentication()`メソッドとして、メソッドが表示されるように従います。</span><span class="sxs-lookup"><span data-stu-id="e6ade-217">Remove the comment characters from the `app.UseGoogleAuthentication()` method so that the method appears as follows:</span></span> 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample5.cs)]
3. <span data-ttu-id="e6ade-218">移動し、 [Google Developers Console](https://console.developers.google.com/)します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-218">Navigate to the [Google Developers Console](https://console.developers.google.com/).</span></span> <span data-ttu-id="e6ade-219">また、Google デベロッパーの電子メール アカウント (gmail.com) でサインインする必要があります。</span><span class="sxs-lookup"><span data-stu-id="e6ade-219">You will also need to sign-in with your Google developer email account (gmail.com).</span></span> <span data-ttu-id="e6ade-220">Google アカウントがいない場合は、選択、**アカウントを作成する**リンク。</span><span class="sxs-lookup"><span data-stu-id="e6ade-220">If you do not have a Google account, select the **Create an account** link.</span></span>   
   <span data-ttu-id="e6ade-221">次に、表示されます、 **Google Developers Console**します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-221">Next, you'll see the **Google Developers Console**.</span></span>   
    <span data-ttu-id="e6ade-222">![Google Developers Console](checkout-and-payment-with-paypal/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="e6ade-222">![Google Developers Console](checkout-and-payment-with-paypal/_static/image8.png)</span></span>
4. <span data-ttu-id="e6ade-223">をクリックして、**プロジェクトの作成**ボタンをクリックし、プロジェクトの名前と ID (既定値を使用することができます) を入力します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-223">Click the **Create Project** button and enter a project name and ID (you can use the default values).</span></span> <span data-ttu-id="e6ade-224">をクリックし、**契約のチェック ボックス**と**作成**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="e6ade-224">Then, click the **agreement checkbox** and the **Create** button.</span></span>  

    ![Google - 新しいプロジェクト](checkout-and-payment-with-paypal/_static/image9.png)

   <span data-ttu-id="e6ade-226">数秒で新しいプロジェクトを作成して、新しいプロジェクトのページがブラウザーに表示されます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-226">In a few seconds the new project will be created and your browser will display the new projects page.</span></span>
5. <span data-ttu-id="e6ade-227">左側のタブで次のようにクリックします。 **Api &amp; auth**、 をクリックし、**資格情報**します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-227">In the left tab, click **APIs &amp; auth**, and then click **Credentials**.</span></span>
6. <span data-ttu-id="e6ade-228">をクリックして、**新しいクライアント ID の作成** **OAuth**します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-228">Click the **Create New Client ID** under **OAuth**.</span></span>   
   <span data-ttu-id="e6ade-229">**Create Client ID**ダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-229">The **Create Client ID** dialog will be displayed.</span></span>   
    <span data-ttu-id="e6ade-230">![Google - クライアント ID の作成](checkout-and-payment-with-paypal/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="e6ade-230">![Google - Create Client ID](checkout-and-payment-with-paypal/_static/image10.png)</span></span>
7. <span data-ttu-id="e6ade-231">**Create Client ID**ダイアログ ボックスで、既定値を保持**Web アプリケーション**アプリケーションの種類。</span><span class="sxs-lookup"><span data-stu-id="e6ade-231">In the **Create Client ID** dialog, keep the default **Web application** for the application type.</span></span>
8. <span data-ttu-id="e6ade-232">設定、**承認済みの JavaScript 生成元**このチュートリアルで先ほど使用した SSL URL を (`https://localhost:44300/`した SSL の他のプロジェクトを作成していない場合)。</span><span class="sxs-lookup"><span data-stu-id="e6ade-232">Set the **Authorized JavaScript Origins** to the SSL URL you used earlier in this tutorial (`https://localhost:44300/` unless you've created other SSL projects).</span></span>   
   <span data-ttu-id="e6ade-233">この URL は、アプリケーションの原点です。</span><span class="sxs-lookup"><span data-stu-id="e6ade-233">This URL is the origin for your application.</span></span> <span data-ttu-id="e6ade-234">このサンプルでは、localhost の URL のテストをのみ入力されます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-234">For this sample, you will only enter the localhost test URL.</span></span> <span data-ttu-id="e6ade-235">ただし、localhost と運用環境に対応する複数の Url を入力することができます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-235">However, you can enter multiple URLs to account for localhost and production.</span></span>
9. <span data-ttu-id="e6ade-236">設定、 **Authorized Redirect URI**次。</span><span class="sxs-lookup"><span data-stu-id="e6ade-236">Set the **Authorized Redirect URI** to the following:</span></span> 

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample6.html)]

   <span data-ttu-id="e6ade-237">この値は URI その ASP.NET OAuth ユーザーが google OAuth サーバーと通信します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-237">This value is the URI that ASP.NET OAuth users to communicate with the google OAuth server.</span></span> <span data-ttu-id="e6ade-238">上記で使用する SSL URL に注意してください (`https://localhost:44300/`した SSL の他のプロジェクトを作成していない場合)。</span><span class="sxs-lookup"><span data-stu-id="e6ade-238">Remember the SSL URL you used above (    `https://localhost:44300/` unless you've created other SSL projects).</span></span>
10. <span data-ttu-id="e6ade-239">をクリックして、 **Create Client ID**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="e6ade-239">Click the **Create Client ID** button.</span></span>
11. <span data-ttu-id="e6ade-240">Google 開発者コンソールの左側のメニューでをクリックして、**同意画面**メニュー項目は、電子メール アドレスと製品名を設定します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-240">On the left menu of the Google Developers Console, click the **Consent screen** menu item, then set your email address and product name.</span></span> <span data-ttu-id="e6ade-241">フォームを完了すると、クリックして**保存**します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-241">When you have completed the form, click **Save**.</span></span>
12. <span data-ttu-id="e6ade-242">をクリックして、 **Api**メニュー項目、下にスクロールし、**オフ**横に**Google + API**します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-242">Click the **APIs** menu item, scroll down and click the **off** button next to **Google+ API**.</span></span>   
    <span data-ttu-id="e6ade-243">このオプションを受け入れると、Google + API が有効になります。</span><span class="sxs-lookup"><span data-stu-id="e6ade-243">Accepting this option will enable the Google+ API.</span></span>
13. <span data-ttu-id="e6ade-244">更新することも必要があります、 **Microsoft.Owin**バージョン 3.0.0 への NuGet パッケージ。</span><span class="sxs-lookup"><span data-stu-id="e6ade-244">You must also update the **Microsoft.Owin** NuGet package to version 3.0.0.</span></span>   
    <span data-ttu-id="e6ade-245">**ツール**メニューの  **NuGet パッケージ マネージャー**選び**ソリューションの NuGet パッケージの管理**します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-245">From the **Tools** menu, select **NuGet Package Manager** and then select **Manage NuGet Packages for Solution**.</span></span>  
    <span data-ttu-id="e6ade-246">**NuGet パッケージの管理**ウィンドウで検索し、更新、 **Microsoft.Owin**バージョン 3.0.0 へのパッケージ。</span><span class="sxs-lookup"><span data-stu-id="e6ade-246">From the **Manage NuGet Packages** window, find and update the **Microsoft.Owin** package to version 3.0.0.</span></span>
14. <span data-ttu-id="e6ade-247">Visual Studio で、更新、`UseGoogleAuthentication`のメソッド、 *Startup.Auth.cs*コピー アンド ペーストでページ、**クライアント ID**と**クライアント シークレット**メソッドにします。</span><span class="sxs-lookup"><span data-stu-id="e6ade-247">In Visual Studio, update the `UseGoogleAuthentication` method of the *Startup.Auth.cs* page by copying and pasting the **Client ID** and **Client Secret** into the method.</span></span> <span data-ttu-id="e6ade-248">**クライアント ID**と**クライアント シークレット**の下に表示される値のサンプルし、は機能しません。</span><span class="sxs-lookup"><span data-stu-id="e6ade-248">The **Client ID** and **Client Secret** values shown below are samples and will not work.</span></span> 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample7.cs?highlight=64-65)]
15. <span data-ttu-id="e6ade-249">キーを押して**CTRL + F5**をビルドして、アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-249">Press **CTRL+F5** to build and run the application.</span></span> <span data-ttu-id="e6ade-250">をクリックして、**ログイン**リンク。</span><span class="sxs-lookup"><span data-stu-id="e6ade-250">Click the **Log in** link.</span></span>
16. <span data-ttu-id="e6ade-251">[**別のサービスを使用してログイン**、] をクリックして**Google**します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-251">Under **Use another service to log in**, click **Google**.</span></span>  
    <span data-ttu-id="e6ade-252">![ログイン](checkout-and-payment-with-paypal/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="e6ade-252">![Log in](checkout-and-payment-with-paypal/_static/image11.png)</span></span>
17. <span data-ttu-id="e6ade-253">資格情報を入力する必要がある場合、資格情報を入力する google サイトにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-253">If you need to enter your credentials, you will be redirected to the google site where you will enter your credentials.</span></span>  
    ![Google - サインイン](checkout-and-payment-with-paypal/_static/image12.png)
18. <span data-ttu-id="e6ade-255">資格情報を入力した後は、作成した web アプリケーションへのアクセス許可を与える促されます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-255">After you enter your credentials, you will be prompted to give permissions to the web application you just created.</span></span>  
    ![プロジェクトの既定のサービス アカウント](checkout-and-payment-with-paypal/_static/image13.png)
19. <span data-ttu-id="e6ade-257">クリックして**受け入れる**します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-257">Click **Accept**.</span></span> <span data-ttu-id="e6ade-258">今すぐにリダイレクトされます、**登録**のページ、 **WingtipToys**アプリケーションが Google アカウントを登録することができます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-258">You will now be redirected back to the **Register** page of the **WingtipToys** application where you can register your Google account.</span></span>  
    <span data-ttu-id="e6ade-259">![Google アカウントの登録します。](checkout-and-payment-with-paypal/_static/image14.png)</span><span class="sxs-lookup"><span data-stu-id="e6ade-259">![Register with your Google Account](checkout-and-payment-with-paypal/_static/image14.png)</span></span>
20. <span data-ttu-id="e6ade-260">Gmail アカウントに使用されるローカルの電子メール登録名を変更するオプションがありますが、通常、既定の電子メール エイリアス (認証に使用するもの) を保持したいです。</span><span class="sxs-lookup"><span data-stu-id="e6ade-260">You have the option of changing the local email registration name used for your Gmail account, but you generally want to keep the default email alias (that is, the one you used for authentication).</span></span> <span data-ttu-id="e6ade-261">クリックして**ログイン**上記のようです。</span><span class="sxs-lookup"><span data-stu-id="e6ade-261">Click **Log in** as shown above.</span></span>

### <a name="modifying-login-functionality"></a><span data-ttu-id="e6ade-262">ログイン機能の変更</span><span class="sxs-lookup"><span data-stu-id="e6ade-262">Modifying Login Functionality</span></span>

<span data-ttu-id="e6ade-263">このチュートリアル シリーズで説明したとおりは以前、ユーザーの登録機能の多くが含まれています、ASP.NET Web フォーム テンプレートで既定では。</span><span class="sxs-lookup"><span data-stu-id="e6ade-263">As previously mentioned in this tutorial series, much of the user registration functionality has been included in the ASP.NET Web Forms template by default.</span></span> <span data-ttu-id="e6ade-264">これは、既定値を変更*Login.aspx*と*Register.aspx*を呼び出すページ、`MigrateCart`メソッド。</span><span class="sxs-lookup"><span data-stu-id="e6ade-264">Now you will modify the default *Login.aspx* and *Register.aspx* pages to call the `MigrateCart` method.</span></span> <span data-ttu-id="e6ade-265">`MigrateCart`メソッドは、匿名のショッピング カートに新しくログインしているユーザーを関連付けます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-265">The `MigrateCart` method associates a newly logged in user with an anonymous shopping cart.</span></span> <span data-ttu-id="e6ade-266">ユーザーを関連付けると、ショッピング カート、Wingtip Toys のサンプル アプリケーションは訪問者の間でユーザーのショッピング カートを維持することになります。</span><span class="sxs-lookup"><span data-stu-id="e6ade-266">By associating the user and shopping cart, the Wingtip Toys sample application will be able to maintain the shopping cart of the user between visits.</span></span>

1. <span data-ttu-id="e6ade-267">**ソリューション エクスプ ローラー**を検索して開く、*アカウント*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="e6ade-267">In **Solution Explorer**, find and open the *Account* folder.</span></span>
2. <span data-ttu-id="e6ade-268">という名前の分離コード ページを変更*Login.aspx.cs*を次のように表示されるように、黄色で強調表示されているコードを含めます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-268">Modify the code-behind page named *Login.aspx.cs* to include the code highlighted in yellow, so that it appears as follows:</span></span>   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample8.cs?highlight=41-43)]
3. <span data-ttu-id="e6ade-269">保存、 *Login.aspx.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="e6ade-269">Save the *Login.aspx.cs* file.</span></span>

<span data-ttu-id="e6ade-270">ここでは、警告の定義が存在しないことを無視することができます、`MigrateCart`メソッド。</span><span class="sxs-lookup"><span data-stu-id="e6ade-270">For now, you can ignore the warning that there is no definition for the `MigrateCart` method.</span></span> <span data-ttu-id="e6ade-271">このチュートリアルでは、少し後でそれを追加する予定です。</span><span class="sxs-lookup"><span data-stu-id="e6ade-271">You will be adding it a bit later in this tutorial.</span></span>

<span data-ttu-id="e6ade-272">*Login.aspx.cs*分離コード ファイルには、LogIn メソッドがサポートしています。</span><span class="sxs-lookup"><span data-stu-id="e6ade-272">The *Login.aspx.cs* code-behind file supports a LogIn method.</span></span> <span data-ttu-id="e6ade-273">Login.aspx ページを調べることでこのページに ログイン ボタンが含まれているを確認しますトリガー をクリックすると、`LogIn`分離コードでハンドラー。</span><span class="sxs-lookup"><span data-stu-id="e6ade-273">By inspecting the Login.aspx page, you'll see that this page includes a "Log in" button that when click triggers the `LogIn` handler on the code-behind.</span></span>

<span data-ttu-id="e6ade-274">ときに、`Login`メソッドを*Login.aspx.cs*を呼び出すと、ショッピング カートという名前の新しいインスタンス`usersShoppingCart`が作成されます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-274">When the `Login` method on the *Login.aspx.cs* is called, a new instance of the shopping cart named `usersShoppingCart` is created.</span></span> <span data-ttu-id="e6ade-275">ショッピング カート (GUID) の ID が取得され、設定、`cartId`変数。</span><span class="sxs-lookup"><span data-stu-id="e6ade-275">The ID of the shopping cart (a GUID) is retrieved and set to the `cartId` variable.</span></span> <span data-ttu-id="e6ade-276">次に、`MigrateCart`メソッドが呼び出され、どちらも、`cartId`と、この方法では、ログインのユーザーの名前。</span><span class="sxs-lookup"><span data-stu-id="e6ade-276">Then, the `MigrateCart` method is called, passing both the `cartId` and the name of the logged-in user to this method.</span></span> <span data-ttu-id="e6ade-277">ショッピング カートが移行されると、匿名ショッピング カートを識別するために使用される GUID は、ユーザー名に置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-277">When the shopping cart is migrated, the GUID used to identify the anonymous shopping cart is replaced with the user name.</span></span>

<span data-ttu-id="e6ade-278">変更するだけでなく、 *Login.aspx.cs* 、ユーザーがログインすると、ショッピング カートを移行する分離コード ファイルも変更する必要があります、 *Register.aspx.cs 分離コード ファイル*ショッピング カートを移行するにはときに、ユーザーは新しいアカウントを作成しログに記録します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-278">In addition to modifying the *Login.aspx.cs* code-behind file to migrate the shopping cart when the user logs in, you must also modify the *Register.aspx.cs code-behind file* to migrate the shopping cart when the user creates a new account and logs in.</span></span>

1. <span data-ttu-id="e6ade-279">*アカウント*フォルダーで、という名前の分離コード ファイルを開く*Register.aspx.cs*します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-279">In the *Account* folder, open the code-behind file named *Register.aspx.cs*.</span></span>
2. <span data-ttu-id="e6ade-280">次のように表示されるように、黄色でコードを含めることによって分離コード ファイルを変更します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-280">Modify the code-behind file by including the code in yellow, so that it appears as follows:</span></span>   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample9.cs?highlight=28-32)]
3. <span data-ttu-id="e6ade-281">保存、 *Register.aspx.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="e6ade-281">Save the *Register.aspx.cs* file.</span></span> <span data-ttu-id="e6ade-282">もう一度、に関する警告は無視、`MigrateCart`メソッド。</span><span class="sxs-lookup"><span data-stu-id="e6ade-282">Once again, ignore the warning about the `MigrateCart` method.</span></span>

<span data-ttu-id="e6ade-283">使用したコードに注目してください、`CreateUser_Click`イベント ハンドラーで使用したコードとよく似ていますが、`LogIn`メソッド。</span><span class="sxs-lookup"><span data-stu-id="e6ade-283">Notice that the code you used in the `CreateUser_Click` event handler is very similar to the code you used in the `LogIn` method.</span></span> <span data-ttu-id="e6ade-284">ユーザーが登録またはへの呼び出し、サイトにログイン、`MigrateCart`メソッドになります。</span><span class="sxs-lookup"><span data-stu-id="e6ade-284">When the user registers or logs in to the site, a call to the `MigrateCart` method will be made.</span></span>

## <a name="migrating-the-shopping-cart"></a><span data-ttu-id="e6ade-285">ショッピング カートを移行します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-285">Migrating the Shopping Cart</span></span>

<span data-ttu-id="e6ade-286">使用してショッピング カートを移行するコードを追加するには、ログインと登録プロセスを更新したら、`MigrateCart`メソッド。</span><span class="sxs-lookup"><span data-stu-id="e6ade-286">Now that you have the log-in and registration process updated, you can add the code to migrate the shopping cart using the `MigrateCart` method.</span></span>

1. <span data-ttu-id="e6ade-287">**ソリューション エクスプ ローラー**、検索、*ロジック*フォルダーとオープン、 *ShoppingCartActions.cs*クラス ファイル。</span><span class="sxs-lookup"><span data-stu-id="e6ade-287">In **Solution Explorer**, find the *Logic* folder and open the *ShoppingCartActions.cs* class file.</span></span>
2. <span data-ttu-id="e6ade-288">既存のコードに黄色で強調表示されているコードを追加、 *ShoppingCartActions.cs*ファイル、ようにコードでは、 *ShoppingCartActions.cs*ファイルが次のように表示されます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-288">Add the code highlighted in yellow to the existing code in the *ShoppingCartActions.cs* file, so that the code in the *ShoppingCartActions.cs* file appears as follows:</span></span>   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample10.cs?highlight=215-224)]

<span data-ttu-id="e6ade-289">`MigrateCart`メソッドでは、既存の cartId を使用して、ユーザーのショッピング カートを検索します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-289">The `MigrateCart` method uses the existing cartId to find the shopping cart of the user.</span></span> <span data-ttu-id="e6ade-290">コードが次に、ショッピング カートのアイテムをループ処理し、置換、`CartId`プロパティ (で指定された、`CartItem`スキーマ) では、ログイン ユーザー名。</span><span class="sxs-lookup"><span data-stu-id="e6ade-290">Next, the code loops through all the shopping cart items and replaces the `CartId` property (as specified by the `CartItem` schema) with the logged-in user name.</span></span>

### <a name="updating-the-database-connection"></a><span data-ttu-id="e6ade-291">データベース接続を更新しています</span><span class="sxs-lookup"><span data-stu-id="e6ade-291">Updating the Database Connection</span></span>

<span data-ttu-id="e6ade-292">このチュートリアルを使用している場合、**事前構築済み**Wingtip Toys のサンプル アプリケーションでは、既定のメンバーシップ データベースを再作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e6ade-292">If you are following this tutorial using the **prebuilt** Wingtip Toys sample application, you must recreate the default membership database.</span></span> <span data-ttu-id="e6ade-293">既定の接続文字列を変更すると、メンバーシップ データベースが作成されます、次回、アプリケーションが実行されます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-293">By modifying the default connection string, the membership database will be created the next time the application runs.</span></span>

1. <span data-ttu-id="e6ade-294">開く、 *Web.config*プロジェクトのルートにあるファイル。</span><span class="sxs-lookup"><span data-stu-id="e6ade-294">Open the *Web.config* file at the root of the project.</span></span>
2. <span data-ttu-id="e6ade-295">次のように表示されるように、既定の接続文字列を更新します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-295">Update the default connection string so that it appears as follows:</span></span>   

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample11.xml)]

<a id="PayPalWebForms"></a>
## <a name="integrating-paypal"></a><span data-ttu-id="e6ade-296">PayPal の統合</span><span class="sxs-lookup"><span data-stu-id="e6ade-296">Integrating PayPal</span></span>

<span data-ttu-id="e6ade-297">PayPal は、オンライン ショップで支払いを受け付ける web ベースの課金プラットフォームです。</span><span class="sxs-lookup"><span data-stu-id="e6ade-297">PayPal is a web-based billing platform that accepts payments by online merchants.</span></span> <span data-ttu-id="e6ade-298">このチュートリアルは、次に、PayPal の Express のチェック アウト機能をアプリケーションに統合する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-298">This tutorial next explains how to integrate PayPal's Express Checkout functionality into your application.</span></span> <span data-ttu-id="e6ade-299">Express のチェック アウトにより、顧客が買い物カゴに追加した項目の料金を支払う PayPal を使用します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-299">Express Checkout allows your customers to use PayPal to pay for the items they have added to their shopping cart.</span></span>

### <a name="create-paylpal-test-accounts"></a><span data-ttu-id="e6ade-300">PaylPal テスト アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-300">Create PaylPal Test Accounts</span></span>

<span data-ttu-id="e6ade-301">PayPal のテスト環境を使用する必要があるには、作成し、開発者のテスト アカウントを確認します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-301">To use the PayPal testing environment, you must create and verify a developer test account.</span></span> <span data-ttu-id="e6ade-302">開発者のテスト アカウントを使用して、テスト アカウントと販売者のテスト アカウントに、購入者を作成します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-302">You will use the developer test account to create a buyer test account and a seller test account.</span></span> <span data-ttu-id="e6ade-303">開発者、テスト アカウントの資格情報も PayPal のテスト環境にアクセスする Wingtip Toys のサンプル アプリケーションが、できます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-303">The developer test account credentials also will allow the Wingtip Toys sample application to access the PayPal testing environment.</span></span>

1. <span data-ttu-id="e6ade-304">ブラウザーでは、サイトのテスト、PayPal 開発者に移動します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-304">In a browser, navigate to the PayPal developer testing site:</span></span>   
    [https://developer.paypal.com](https://developer.paypal.com/)
2. <span data-ttu-id="e6ade-305">PayPal の開発者アカウントを持っていない場合は、クリックして新しいアカウントを作成**サインアップ**サインアップ手順に従います。</span><span class="sxs-lookup"><span data-stu-id="e6ade-305">If you don't have a PayPal developer account, create a new account by clicking **Sign Up**and following the sign up steps.</span></span> <span data-ttu-id="e6ade-306">既存の PayPal の開発者アカウントがあればがクリックしてサインイン**ログで**します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-306">If you have an existing PayPal developer account, sign in by clicking **Log In**.</span></span> <span data-ttu-id="e6ade-307">PayPal 開発者アカウントをこのチュートリアルの後半で、Wingtip Toys のサンプル アプリケーションをテストする必要があります。</span><span class="sxs-lookup"><span data-stu-id="e6ade-307">You will need your PayPal developer account to test the Wingtip Toys sample application later in this tutorial.</span></span>
3. <span data-ttu-id="e6ade-308">場合は、PayPal の開発者アカウントにサインアップしただけでは、PayPal、PayPal 開発者アカウントを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e6ade-308">If you have just signed up for your PayPal developer account, you may need to verify your PayPal developer account with PayPal.</span></span> <span data-ttu-id="e6ade-309">電子メール アカウントに送信される PayPal の手順に従って、アカウントを確認できます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-309">You can verify your account by following the steps that PayPal sent to your email account.</span></span> <span data-ttu-id="e6ade-310">PayPal 開発者アカウントを確認した後は、サイトのテスト、PayPal 開発者に再度ログインします。</span><span class="sxs-lookup"><span data-stu-id="e6ade-310">Once you have verified your PayPal developer account, log back into the PayPal developer testing site.</span></span>
4. <span data-ttu-id="e6ade-311">後で、PayPal 開発者アカウントがまだない場合は、PayPal buyer テスト アカウントを作成する必要がある、PayPal の開発者向けサイトにログインしているいずれかであります。</span><span class="sxs-lookup"><span data-stu-id="e6ade-311">After you are logged in to the PayPal developer site with your PayPal developer account you need to create a PayPal buyer test account if you don't already have one.</span></span> <span data-ttu-id="e6ade-312">PayPal の サイト をクリックで、buyer テスト アカウントを作成する、**アプリケーション** タブをクリックして**サンド ボックス アカウント**します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-312">To create a buyer test account, on the PayPal site click the **Applications** tab and then click **Sandbox accounts**.</span></span>   
 <span data-ttu-id="e6ade-313">**サンド ボックス テスト アカウント**ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-313">The **Sandbox test accounts** page is shown.</span></span>   

    > [!NOTE] 
    > 
    > <span data-ttu-id="e6ade-314">PayPal の開発者向けサイトには、テストのマーチャント アカウントが既に用意されています。</span><span class="sxs-lookup"><span data-stu-id="e6ade-314">The PayPal Developer site already provides a merchant test account.</span></span>

    ![精算と PayPal - サンド ボックス テスト アカウントによる支払い](checkout-and-payment-with-paypal/_static/image15.png)
5. <span data-ttu-id="e6ade-316">サンド ボックス テスト アカウント ページで、次のようにクリックします。**アカウントの作成**です。</span><span class="sxs-lookup"><span data-stu-id="e6ade-316">On the Sandbox test accounts page, click **Create Account**.</span></span>
6. <span data-ttu-id="e6ade-317">**テスト アカウントの作成**ページは、テスト アカウントの電子メール アドレスと、任意のパスワードに、購入者を選択します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-317">On the **Create test account** page choose a buyer test account email and password of your choice.</span></span>   

    > [!NOTE] 
    > 
    > <span data-ttu-id="e6ade-318">購入者の電子メール アドレスとパスワードをこのチュートリアルの最後に、Wingtip Toys のサンプル アプリケーションをテストする必要があります。</span><span class="sxs-lookup"><span data-stu-id="e6ade-318">You will need the buyer email addresses and password to test the Wingtip Toys sample application at the end of this tutorial.</span></span>

    ![精算と PayPal - サンド ボックス テスト アカウントによる支払い](checkout-and-payment-with-paypal/_static/image16.png)
7. <span data-ttu-id="e6ade-320">クリックして、購入者のテスト アカウントを作成、**アカウントの作成**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="e6ade-320">Create the buyer test account by clicking the **Create Account** button.</span></span>  
 <span data-ttu-id="e6ade-321">**サンド ボックス テスト アカウント**ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-321">The **Sandbox Test accounts** page is displayed.</span></span> 

    ![精算と PayPal - PaylPal アカウントによる支払い](checkout-and-payment-with-paypal/_static/image17.png)
8. <span data-ttu-id="e6ade-323">**サンド ボックス テスト アカウント** ページで、をクリックして、**進行**電子メール アカウント。</span><span class="sxs-lookup"><span data-stu-id="e6ade-323">On the **Sandbox test accounts** page, click the **facilitator** email account.</span></span>  
    <span data-ttu-id="e6ade-324">**プロファイル**と**通知**オプションが表示されます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-324">**Profile** and **Notification** options appear.</span></span>
9. <span data-ttu-id="e6ade-325">選択、**プロファイル** をクリックし、 **API 資格情報**マーチャント テスト アカウントの資格情報、API を表示します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-325">Select the **Profile** option, then click **API credentials** to view your API credentials for the merchant test account.</span></span>
10. <span data-ttu-id="e6ade-326">API のテストの資格情報をメモ帳にコピーします。</span><span class="sxs-lookup"><span data-stu-id="e6ade-326">Copy the TEST API credentials to notepad.</span></span>

<span data-ttu-id="e6ade-327">表示されているクラシック API のテスト資格情報 (ユーザー名、パスワード、および署名)、Wingtip Toys のサンプル アプリケーションの API 呼び出しを行うには、PayPal の環境をテストする必要があります。</span><span class="sxs-lookup"><span data-stu-id="e6ade-327">You will need your displayed Classic TEST API credentials (Username, Password, and Signature) to make API calls from the Wingtip Toys sample application to the PayPal testing environment.</span></span> <span data-ttu-id="e6ade-328">次の手順では、資格情報を追加します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-328">You will add the credentials in the next step.</span></span>

### <a name="add-paypal-class-and-api-credentials"></a><span data-ttu-id="e6ade-329">PayPal クラスと API の資格情報を追加します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-329">Add PayPal Class and API Credentials</span></span>

<span data-ttu-id="e6ade-330">1 つのクラスに PayPal コードの大部分が配置されます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-330">You will place the majority of the PayPal code into a single class.</span></span> <span data-ttu-id="e6ade-331">このクラスには、PayPal との通信に使用するメソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="e6ade-331">This class contains the methods used to communicate with PayPal.</span></span> <span data-ttu-id="e6ade-332">また、このクラスには、PayPal の資格情報を追加します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-332">Also, you will add your PayPal credentials to this class.</span></span>

1. <span data-ttu-id="e6ade-333">Visual Studio 内で Wingtip Toys のサンプル アプリケーションを右クリックし、**ロジック**フォルダーと、選択**追加** - &gt; **新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-333">In the Wingtip Toys sample application within Visual Studio, right-click the **Logic** folder and then select **Add** -&gt; **New Item**.</span></span>   
   <span data-ttu-id="e6ade-334">**[新しい項目の追加]** ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-334">The **Add New Item** dialog box is displayed.</span></span>
2. <span data-ttu-id="e6ade-335">**Visual c#** から、**インストール済み**、左側のペイン**コード**します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-335">Under **Visual C#** from the **Installed** pane on the left, select **Code**.</span></span>
3. <span data-ttu-id="e6ade-336">中央のウィンドウから次のように選択します。**クラス**します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-336">From the middle pane, select **Class**.</span></span> <span data-ttu-id="e6ade-337">この新しいクラスの名前を**PayPalFunctions.cs**します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-337">Name this new class **PayPalFunctions.cs**.</span></span>
4. <span data-ttu-id="e6ade-338">**[追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e6ade-338">Click **Add**.</span></span>  
   <span data-ttu-id="e6ade-339">新しいクラス ファイルがエディターで表示されます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-339">The new class file is displayed in the editor.</span></span>
5. <span data-ttu-id="e6ade-340">既定のコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-340">Replace the default code with the following code:</span></span>  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample12.cs)]
6. <span data-ttu-id="e6ade-341">マーチャント API 資格情報 (ユーザー名、パスワード、および署名) できるように、PayPal のテスト環境への関数呼び出しを行うことができます、このチュートリアルで先ほど表示したを追加します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-341">Add the Merchant API credentials (Username, Password, and Signature) that you displayed earlier in this tutorial so that you can make function calls to the PayPal testing environment.</span></span>  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample13.cs)]

> [!NOTE] 
> 
> <span data-ttu-id="e6ade-342">このサンプル アプリケーションで c# ファイル (.cs) に資格情報を追加するだけです。</span><span class="sxs-lookup"><span data-stu-id="e6ade-342">In this sample application you are simply adding credentials to a C# file (.cs).</span></span> <span data-ttu-id="e6ade-343">ただし、実装済みのソリューションで、構成ファイルで、資格情報の暗号化を検討してください。</span><span class="sxs-lookup"><span data-stu-id="e6ade-343">However, in a implemented solution, you should consider encrypting your credentials in a configuration file.</span></span>


<span data-ttu-id="e6ade-344">NVPAPICaller クラスには、PayPal の機能の大半が含まれています。</span><span class="sxs-lookup"><span data-stu-id="e6ade-344">The NVPAPICaller class contains the majority of the PayPal functionality.</span></span> <span data-ttu-id="e6ade-345">クラスのコードでは、購入、PayPal のテスト環境からテストの作成に必要なメソッドを提供します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-345">The code in the class provides the methods needed to make a test purchase from the PayPal testing environment.</span></span> <span data-ttu-id="e6ade-346">次の 3 つの PayPal 関数は、購入のために使用されます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-346">The following three PayPal functions are used to make purchases:</span></span>

- <span data-ttu-id="e6ade-347">`SetExpressCheckout` 関数</span><span class="sxs-lookup"><span data-stu-id="e6ade-347">`SetExpressCheckout` function</span></span>
- <span data-ttu-id="e6ade-348">`GetExpressCheckoutDetails` 関数</span><span class="sxs-lookup"><span data-stu-id="e6ade-348">`GetExpressCheckoutDetails` function</span></span>
- <span data-ttu-id="e6ade-349">`DoExpressCheckoutPayment` 関数</span><span class="sxs-lookup"><span data-stu-id="e6ade-349">`DoExpressCheckoutPayment` function</span></span>

<span data-ttu-id="e6ade-350">`ShortcutExpressCheckout`メソッドは、ショッピング カートと呼び出しからテストの購入情報と製品の詳細を収集、 `SetExpressCheckout` PayPal 関数。</span><span class="sxs-lookup"><span data-stu-id="e6ade-350">The `ShortcutExpressCheckout` method collects the test purchase information and product details from the shopping cart and calls the `SetExpressCheckout` PayPal function.</span></span> <span data-ttu-id="e6ade-351">`GetCheckoutDetails`メソッドは、購入の詳細と呼び出しを確認します。、`GetExpressCheckoutDetails`テスト購入を行う前に、PayPal 関数。</span><span class="sxs-lookup"><span data-stu-id="e6ade-351">The `GetCheckoutDetails` method confirms purchase details and calls the `GetExpressCheckoutDetails` PayPal function before making the test purchase.</span></span> <span data-ttu-id="e6ade-352">`DoCheckoutPayment`メソッドが呼び出すことにより、テスト環境からテストの購入を完了、 `DoExpressCheckoutPayment` PayPal 関数。</span><span class="sxs-lookup"><span data-stu-id="e6ade-352">The `DoCheckoutPayment` method completes the test purchase from the testing environment by calling the `DoExpressCheckoutPayment` PayPal function.</span></span> <span data-ttu-id="e6ade-353">残りのコードでは、PayPal メソッドと文字列をエンコード、文字列をデコード、配列、処理、および資格情報を決定するなどのプロセスをサポートします。</span><span class="sxs-lookup"><span data-stu-id="e6ade-353">The remaining code supports the PayPal methods and process, such as encoding strings, decoding strings, processing arrays, and determining credentials.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="e6ade-354">PayPal を使用すると、に基づいて省略可能な購入の詳細を含む[PayPal の API 仕様](https://cms.paypal.com/us/cgi-bin/?cmd=_render-content&amp;content_ID=developer/e_howto_api_nvp_r_SetExpressCheckout)します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-354">PayPal allows you to include optional purchase details based on [PayPal's API specification](https://cms.paypal.com/us/cgi-bin/?cmd=_render-content&amp;content_ID=developer/e_howto_api_nvp_r_SetExpressCheckout).</span></span> <span data-ttu-id="e6ade-355">Wingtip Toys のサンプル アプリケーションでコードを拡張するには、ローカライズの詳細、製品の説明、税金、顧客サービスの数、だけでなく他の多くの省略可能なフィールドを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-355">By extending the code in the Wingtip Toys sample application, you can include localization details, product descriptions, tax, a customer service number, as well as many other optional fields.</span></span>


<span data-ttu-id="e6ade-356">注意して、戻り値と [キャンセル] Url で指定されている、 **ShortcutExpressCheckout**メソッドは、ポート番号を使用します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-356">Notice that the return and cancel URLs that are specified in the **ShortcutExpressCheckout** method use a port number.</span></span>

[!code-html[Main](checkout-and-payment-with-paypal/samples/sample14.html)]

<span data-ttu-id="e6ade-357">Visual Web Developer は、SSL を使用して web プロジェクトを実行するとよくポート 44300 が web サーバーの使用されます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-357">When Visual Web Developer runs a web project using SSL, commonly the port 44300 is used for the web server.</span></span> <span data-ttu-id="e6ade-358">上記のように、ポート番号は 44300 が。</span><span class="sxs-lookup"><span data-stu-id="e6ade-358">As shown above, the port number is 44300.</span></span> <span data-ttu-id="e6ade-359">アプリケーションを実行するときに、別のポート番号を表示できます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-359">When you run the application, you could see a different port number.</span></span> <span data-ttu-id="e6ade-360">ポート番号のニーズを正しく設定するコードを行えるように成功は、このチュートリアルの最後に、Wingtip Toys のサンプル アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-360">Your port number needs to be correctly set in the code so that you can successful run the Wingtip Toys sample application at the end of this tutorial.</span></span> <span data-ttu-id="e6ade-361">このチュートリアルの次のセクションでは、ローカル ホストのポート番号を取得して、PayPal クラスを更新する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-361">The next section of this tutorial explains how to retrieve the local host port number and update the PayPal class.</span></span>

### <a name="update-the-localhost-port-number-in-the-paypal-class"></a><span data-ttu-id="e6ade-362">PayPal クラスでは、LocalHost のポート番号を更新します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-362">Update the LocalHost Port Number in the PayPal Class</span></span>

<span data-ttu-id="e6ade-363">Wingtip Toys のサンプル アプリケーションは、PayPal テスト サイトに移動し、Wingtip Toys のサンプル アプリケーションのローカル インスタンスを返すことで、製品を購入します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-363">The Wingtip Toys sample application purchases products by navigating to the PayPal testing site and returning to your local instance of the Wingtip Toys sample application.</span></span> <span data-ttu-id="e6ade-364">PayPal の正しい URL を返すためには、ローカルで実行中のポート番号を指定する必要があります。 上記で説明した PayPal コードでのアプリケーションのサンプルです。</span><span class="sxs-lookup"><span data-stu-id="e6ade-364">In order to have PayPal return to the correct URL, you need to specify the port number of the locally running sample application in the PayPal code mentioned above.</span></span>

1. <span data-ttu-id="e6ade-365">プロジェクト名を右クリックして (**WingtipToys**) で**ソリューション エクスプ ローラー**選択**プロパティ**します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-365">Right-click the project name (**WingtipToys**) in **Solution Explorer** and select **Properties**.</span></span>
2. <span data-ttu-id="e6ade-366">左の列を選択、 **Web**タブ。</span><span class="sxs-lookup"><span data-stu-id="e6ade-366">In the left column, select the **Web** tab.</span></span>
3. <span data-ttu-id="e6ade-367">ポート番号を取得、**プロジェクト Url**ボックス。</span><span class="sxs-lookup"><span data-stu-id="e6ade-367">Retrieve the port number from the **Project Url** box.</span></span>
4. <span data-ttu-id="e6ade-368">必要な場合、更新、`returnURL`と`cancelURL`PayPal クラスで (`NVPAPICaller`) で、 *PayPalFunctions.cs* web アプリケーションのポート番号を使用するファイル。</span><span class="sxs-lookup"><span data-stu-id="e6ade-368">If needed, update the `returnURL` and `cancelURL` in the PayPal class (`NVPAPICaller`) in the *PayPalFunctions.cs* file to use the port number of your web application:</span></span>   

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample15.html?highlight=1-2)]

<span data-ttu-id="e6ade-369">ここで追加したコードでは、ローカル Web アプリケーションの必要なポートが一致します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-369">Now the code that you added will match the expected port for your local Web application.</span></span> <span data-ttu-id="e6ade-370">PayPal はローカル コンピューターに、正しい URL に返すことになります。</span><span class="sxs-lookup"><span data-stu-id="e6ade-370">PayPal will be able to return to the correct URL on your local machine.</span></span>

### <a name="add-the-paypal-checkout-button"></a><span data-ttu-id="e6ade-371">PayPal のチェック アウト ボタンを追加します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-371">Add the PayPal Checkout Button</span></span>

<span data-ttu-id="e6ade-372">プライマリの PayPal 関数は、サンプル アプリケーションに追加されましたが、これで、マークアップとコードをこれらの関数を呼び出すために必要な追加を開始できます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-372">Now that the primary PayPal functions have been added to the sample application, you can begin adding the markup and code needed to call these functions.</span></span> <span data-ttu-id="e6ade-373">最初に、ショッピング カート ページで、ユーザーに表示されるチェック アウト ボタンを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e6ade-373">First, you must add the checkout button that the user will see on the shopping cart page.</span></span>

1. <span data-ttu-id="e6ade-374">開く、*後*ファイル。</span><span class="sxs-lookup"><span data-stu-id="e6ade-374">Open the *ShoppingCart.aspx* file.</span></span>
2. <span data-ttu-id="e6ade-375">ファイルの一番下までスクロールし、検索、`<!--Checkout Placeholder -->`コメント。</span><span class="sxs-lookup"><span data-stu-id="e6ade-375">Scroll to the bottom of the file and find the `<!--Checkout Placeholder -->` comment.</span></span>
3. <span data-ttu-id="e6ade-376">コメントを置き換える、`ImageButton`コントロール マークアップは次のように置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-376">Replace the comment with an `ImageButton` control so that the mark up is replaced as follows:</span></span>  

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample16.aspx)]
4. <span data-ttu-id="e6ade-377">*ShoppingCart.aspx.cs*した後、ファイル、 `UpdateBtn_Click` 、ファイルの末尾近くのイベント ハンドラーを追加、`CheckOutBtn_Click`イベント ハンドラー。</span><span class="sxs-lookup"><span data-stu-id="e6ade-377">In the *ShoppingCart.aspx.cs* file, after the `UpdateBtn_Click` event handler near the end of the file, add the `CheckOutBtn_Click` event handler:</span></span>  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample17.cs)]
5. <span data-ttu-id="e6ade-378">さらに、 *ShoppingCart.aspx.cs*ファイルへの参照を追加、`CheckoutBtn`新しいイメージ ボタンは次のように参照されているため。</span><span class="sxs-lookup"><span data-stu-id="e6ade-378">Also in the *ShoppingCart.aspx.cs* file, add a reference to the `CheckoutBtn`, so that the new image button is referenced as follows:</span></span>  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample18.cs?highlight=18)]
6. <span data-ttu-id="e6ade-379">両方に変更を保存、*後*ファイルと*ShoppingCart.aspx.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="e6ade-379">Save your changes to both the *ShoppingCart.aspx* file and the *ShoppingCart.aspx.cs* file.</span></span>
7. <span data-ttu-id="e6ade-380">メニューから、次のように選択します。**デバッグ**-&gt;**ビルド WingtipToys**します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-380">From the menu, select **Debug**-&gt;**Build WingtipToys**.</span></span>  
   <span data-ttu-id="e6ade-381">プロジェクトが再構築で新しく追加された**ImageButton**コントロール。</span><span class="sxs-lookup"><span data-stu-id="e6ade-381">The project will be rebuilt with the newly added **ImageButton** control.</span></span>

### <a name="send-purchase-details-to-paypal"></a><span data-ttu-id="e6ade-382">PayPal を購入の詳細情報を送信します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-382">Send Purchase Details to PayPal</span></span>

<span data-ttu-id="e6ade-383">ユーザーがクリックすると、**チェック アウト**ショッピング カート ページのボタン (*後*)、発注プロセスを始めます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-383">When the user clicks the **Checkout** button on the shopping cart page (*ShoppingCart.aspx*), they'll begin the purchase process.</span></span> <span data-ttu-id="e6ade-384">次のコードでは、製品を購入するために必要な最初の PayPal 関数を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-384">The following code calls the first PayPal function needed to purchase products.</span></span>

1. <span data-ttu-id="e6ade-385">*チェック アウト*フォルダーで、という名前の分離コード ファイルを開く*CheckoutStart.aspx.cs*します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-385">From the *Checkout* folder, open the code-behind file named *CheckoutStart.aspx.cs*.</span></span>   
   <span data-ttu-id="e6ade-386">必ず、分離コード ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-386">Be sure to open the code-behind file.</span></span>
2. <span data-ttu-id="e6ade-387">既存のコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-387">Replace the existing code with the following:</span></span>   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample19.cs)]

<span data-ttu-id="e6ade-388">アプリケーションのユーザーがクリックすると、**チェック アウト**ショッピング カート ページで、ブラウザーのボタンに移動、 *CheckoutStart.aspx*ページ。</span><span class="sxs-lookup"><span data-stu-id="e6ade-388">When the user of the application clicks the **Checkout** button on the shopping cart page, the browser will navigate to the *CheckoutStart.aspx* page.</span></span> <span data-ttu-id="e6ade-389">ときに、 *CheckoutStart.aspx*ページの読み込み、`ShortcutExpressCheckout`メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-389">When the *CheckoutStart.aspx* page loads, the `ShortcutExpressCheckout` method is called.</span></span> <span data-ttu-id="e6ade-390">この時点では、ユーザーは、PayPal テスト web サイトに転送されます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-390">At this point, the user is transferred to the PayPal testing web site.</span></span> <span data-ttu-id="e6ade-391">PayPal のサイトでユーザー PayPal 資格情報を入力、購入の詳細を確認、PayPal 契約を受け取り、Wingtip Toys のサンプル アプリケーションに返します場所、`ShortcutExpressCheckout`メソッドが完了するとします。</span><span class="sxs-lookup"><span data-stu-id="e6ade-391">On the PayPal site, the user enters their PayPal credentials, reviews the purchase details, accepts the PayPal agreement and returns to the Wingtip Toys sample application where the `ShortcutExpressCheckout` method completes.</span></span> <span data-ttu-id="e6ade-392">ときに、`ShortcutExpressCheckout`メソッドが完了したら、ユーザーがリダイレクトされる、 *CheckoutReview.aspx*で指定されたページ、`ShortcutExpressCheckout`メソッド。</span><span class="sxs-lookup"><span data-stu-id="e6ade-392">When the `ShortcutExpressCheckout` method is complete, it will redirect the user to the *CheckoutReview.aspx* page specified in the `ShortcutExpressCheckout` method.</span></span> <span data-ttu-id="e6ade-393">これにより、ユーザーは、Wingtip Toys のサンプル アプリケーション内で注文の詳細を確認できます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-393">This allows the user to review the order details from within the Wingtip Toys sample application.</span></span>

### <a name="review-order-details"></a><span data-ttu-id="e6ade-394">注文の詳細を確認してください。</span><span class="sxs-lookup"><span data-stu-id="e6ade-394">Review Order Details</span></span>

<span data-ttu-id="e6ade-395">PayPal から返された後、 *CheckoutReview.aspx* Wingtip Toys のサンプル アプリケーションのページには、注文の詳細が表示されます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-395">After returning from PayPal, the *CheckoutReview.aspx* page of the Wingtip Toys sample application displays the order details.</span></span> <span data-ttu-id="e6ade-396">このページは、製品を購入する前に、注文の詳細を確認できます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-396">This page allows the user to review the order details before purchasing the products.</span></span> <span data-ttu-id="e6ade-397">*CheckoutReview.aspx*ページを次のように作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e6ade-397">The *CheckoutReview.aspx* page must be created as follows:</span></span>

1. <span data-ttu-id="e6ade-398">*チェック アウト*フォルダーで、という名前のページを開く*CheckoutReview.aspx*します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-398">In the *Checkout* folder, open the page named *CheckoutReview.aspx*.</span></span>
2. <span data-ttu-id="e6ade-399">次のように既存のマークアップに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-399">Replace the existing markup with the following:</span></span>   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample20.aspx)]
3. <span data-ttu-id="e6ade-400">という名前の分離コード ページを開く*CheckoutReview.aspx.cs*次の既存のコードを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-400">Open the code-behind page named *CheckoutReview.aspx.cs* and replace the existing code with the following:</span></span>   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample21.cs)]

<span data-ttu-id="e6ade-401">**DetailsView** PayPal から返された注文の詳細を表示するコントロールを使用します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-401">The **DetailsView** control is used to display the order details that have been returned from PayPal.</span></span> <span data-ttu-id="e6ade-402">上記のコードが、注文の詳細として Wingtip Toys データベースに保存することも、`OrderDetail`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="e6ade-402">Also, the above code saves the order details to the Wingtip Toys database as an `OrderDetail` object.</span></span> <span data-ttu-id="e6ade-403">ユーザーがクリックしたときに、**注文完了**ボタンにリダイレクトされます、 *CheckoutComplete.aspx*ページ。</span><span class="sxs-lookup"><span data-stu-id="e6ade-403">When the user clicks on the **Complete Order** button, they are redirected to the *CheckoutComplete.aspx* page.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="e6ade-404">**ヒント。**</span><span class="sxs-lookup"><span data-stu-id="e6ade-404">**Tip**</span></span>
> 
> <span data-ttu-id="e6ade-405">マークアップで、 *CheckoutReview.aspx*  ページで、注意、`<ItemStyle>`タグを使用中の項目のスタイルを変更して、 **DetailsView**ページの下部にあるコントロール。</span><span class="sxs-lookup"><span data-stu-id="e6ade-405">In the markup of the *CheckoutReview.aspx* page, notice that the `<ItemStyle>` tag is used to change the style of the items within the **DetailsView** control near the bottom of the page.</span></span> <span data-ttu-id="e6ade-406">内のページを表示して**デザイン ビュー** (を選択して**デザイン**Visual Studio の左上隅にある)、選択し、 **DetailsView**を制御して、を選択します。**スマート タグ**(上部にある矢印アイコン コントロールの右) を表示することができます、 **DetailsView タスク**します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-406">By viewing the page in **Design View** (by selecting **Design** at the lower left corner of Visual Studio), then selecting the **DetailsView** control, and selecting the **Smart Tag** (the arrow icon at the top right of the control), you will be able to see the **DetailsView Tasks**.</span></span>
> 
> ![精算と PayPal - による支払いのフィールドを編集します。](checkout-and-payment-with-paypal/_static/image18.png)
> 
> <span data-ttu-id="e6ade-408">選択して**フィールドの編集**、**フィールド** ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-408">By selecting **Edit Fields**, the **Fields** dialog box will appear.</span></span> <span data-ttu-id="e6ade-409">このダイアログ ボックスで簡単に制御できますビジュアルのプロパティなど**後**の**DetailsView**コントロール。</span><span class="sxs-lookup"><span data-stu-id="e6ade-409">In this dialog box you can easily control the visual properties, such as **ItemStyle**, of the **DetailsView** control.</span></span>
> 
> ![精算と PayPal - フィールド ダイアログ ボックスでの支払い](checkout-and-payment-with-paypal/_static/image19.png)


### <a name="complete-purchase"></a><span data-ttu-id="e6ade-411">購入を完了します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-411">Complete Purchase</span></span>

<span data-ttu-id="e6ade-412">*CheckoutComplete.aspx*ページでは、PayPal から購入します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-412">*CheckoutComplete.aspx* page makes the purchase from PayPal.</span></span> <span data-ttu-id="e6ade-413">前述のように、ユーザーをクリックする必要があります、**注文完了**ボタン、アプリケーションが移動する前に、 *CheckoutComplete.aspx*ページ。</span><span class="sxs-lookup"><span data-stu-id="e6ade-413">As mentioned above, the user must click on the **Complete Order** button before the application will navigate to the *CheckoutComplete.aspx* page.</span></span>

1. <span data-ttu-id="e6ade-414">*チェック アウト*フォルダーで、という名前のページを開く*CheckoutComplete.aspx*します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-414">In the *Checkout* folder, open the page named *CheckoutComplete.aspx*.</span></span>
2. <span data-ttu-id="e6ade-415">次のように既存のマークアップに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-415">Replace the existing markup with the following:</span></span>   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample22.aspx)]
3. <span data-ttu-id="e6ade-416">という名前の分離コード ページを開く*CheckoutComplete.aspx.cs*次の既存のコードを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-416">Open the code-behind page named *CheckoutComplete.aspx.cs* and replace the existing code with the following:</span></span>   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample23.cs)]

<span data-ttu-id="e6ade-417">ときに、 *CheckoutComplete.aspx*ページが読み込まれる、`DoCheckoutPayment`メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-417">When the *CheckoutComplete.aspx* page is loaded, the `DoCheckoutPayment` method is called.</span></span> <span data-ttu-id="e6ade-418">前に説明したように、`DoCheckoutPayment`メソッドは、PayPal のテスト環境からの購入を完了します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-418">As mentioned earlier, the `DoCheckoutPayment` method completes the purchase from the PayPal testing environment.</span></span> <span data-ttu-id="e6ade-419">PayPal の注文の購入が完了すると、 *CheckoutComplete.aspx*ページには、支払取引が表示されます。`ID`購入者にします。</span><span class="sxs-lookup"><span data-stu-id="e6ade-419">Once PayPal has completed the purchase of the order, the *CheckoutComplete.aspx* page displays a payment transaction `ID` to the purchaser.</span></span>

### <a name="handle-cancel-purchase"></a><span data-ttu-id="e6ade-420">購入をキャンセルを処理します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-420">Handle Cancel Purchase</span></span>

<span data-ttu-id="e6ade-421">ユーザー、購入をキャンセルする場合、それらが表示されます、 *CheckoutCancel.aspx*ページの場所の順序が取り消されましたが表示されます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-421">If the user decides to cancel the purchase, they will be directed to the *CheckoutCancel.aspx* page where they will see that their order has been cancelled.</span></span>

1. <span data-ttu-id="e6ade-422">という名前のページを開く*CheckoutCancel.aspx*で、*チェック アウト*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="e6ade-422">Open the page named *CheckoutCancel.aspx* in the *Checkout* folder.</span></span>
2. <span data-ttu-id="e6ade-423">次のように既存のマークアップに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-423">Replace the existing markup with the following:</span></span>   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample24.aspx)]

### <a name="handle-purchase-errors"></a><span data-ttu-id="e6ade-424">購入のエラーの処理</span><span class="sxs-lookup"><span data-stu-id="e6ade-424">Handle Purchase Errors</span></span>

<span data-ttu-id="e6ade-425">によって、購入プロセス中にエラーが処理される、 *CheckoutError.aspx*ページ。</span><span class="sxs-lookup"><span data-stu-id="e6ade-425">Errors during the purchase process will be handled by the *CheckoutError.aspx* page.</span></span> <span data-ttu-id="e6ade-426">分離コード、 *CheckoutStart.aspx*  ページで、 *CheckoutReview.aspx*  ページで、および*CheckoutComplete.aspx*ページごとにリダイレクトされます、 *CheckoutError.aspx*ページ エラーが発生した場合。</span><span class="sxs-lookup"><span data-stu-id="e6ade-426">The code-behind of the *CheckoutStart.aspx* page, the *CheckoutReview.aspx* page, and the *CheckoutComplete.aspx* page will each redirect to the *CheckoutError.aspx* page if an error occurs.</span></span>

1. <span data-ttu-id="e6ade-427">という名前のページを開く*CheckoutError.aspx*で、*チェック アウト*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="e6ade-427">Open the page named *CheckoutError.aspx* in the *Checkout* folder.</span></span>
2. <span data-ttu-id="e6ade-428">次のように既存のマークアップに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-428">Replace the existing markup with the following:</span></span>   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample25.aspx)]

<span data-ttu-id="e6ade-429">*CheckoutError.aspx*チェック アウト プロセス中にエラーが発生するとエラーの詳細ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-429">The *CheckoutError.aspx* page is displayed with the error details when an error occurs during the checkout process.</span></span>

## <a name="running-the-application"></a><span data-ttu-id="e6ade-430">アプリケーションの実行</span><span class="sxs-lookup"><span data-stu-id="e6ade-430">Running the Application</span></span>

<span data-ttu-id="e6ade-431">製品を購入する方法について、アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-431">Run the application to see how to purchase products.</span></span> <span data-ttu-id="e6ade-432">実行する、PayPal のテスト環境に注意してください。</span><span class="sxs-lookup"><span data-stu-id="e6ade-432">Note that you will be running in the PayPal testing environment.</span></span> <span data-ttu-id="e6ade-433">交換される実際のコストはありません。</span><span class="sxs-lookup"><span data-stu-id="e6ade-433">No actual money is being exchanged.</span></span>

1. <span data-ttu-id="e6ade-434">次のすべての Visual Studio で、ファイルが保存を確認します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-434">Make sure all your files are saved in Visual Studio.</span></span>
2. <span data-ttu-id="e6ade-435">Web ブラウザーを開きに移動します[ https://developer.paypal.com](https://developer.paypal.com/)します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-435">Open a Web browser and navigate to [https://developer.paypal.com](https://developer.paypal.com/).</span></span>
3. <span data-ttu-id="e6ade-436">このチュートリアルで先ほど作成した、PayPal 開発者アカウントでログインします。</span><span class="sxs-lookup"><span data-stu-id="e6ade-436">Login with your PayPal developer account that you created earlier in this tutorial.</span></span>  
   <span data-ttu-id="e6ade-437">PayPal の開発者のサンド ボックスでログに記録する必要があります。 [ https://developer.paypal.com ](https://developer.paypal.com/) express のチェック アウトをテストします。</span><span class="sxs-lookup"><span data-stu-id="e6ade-437">For PayPal's developer sandbox, you need to be logged in at [https://developer.paypal.com](https://developer.paypal.com/) to test express checkout.</span></span> <span data-ttu-id="e6ade-438">これは、テスト、PayPal の実際の環境が PayPal のサンド ボックスにのみ適用されます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-438">This only applies to PayPal's sandbox testing, not to PayPal's live environment.</span></span>
4. <span data-ttu-id="e6ade-439">Visual Studio で、キーを押して**F5** Wingtip Toys のサンプル アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-439">In Visual Studio, press **F5** to run the Wingtip Toys sample application.</span></span>  
   <span data-ttu-id="e6ade-440">データベースが再構築した後、ブラウザーが開き、表示、 *Default.aspx*ページ。</span><span class="sxs-lookup"><span data-stu-id="e6ade-440">After the database rebuilds, the browser will open and show the *Default.aspx* page.</span></span>
5. <span data-ttu-id="e6ade-441">クリックして、"Cars"など、製品カテゴリを選択すると、ショッピング カートに 3 つの異なる製品を追加**カートに追加**各製品の横にあります。</span><span class="sxs-lookup"><span data-stu-id="e6ade-441">Add three different products to the shopping cart by selecting the product category, such as "Cars" and then clicking **Add to Cart** next to each product.</span></span>  
   <span data-ttu-id="e6ade-442">選択した製品は、ショッピング カートが表示されます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-442">The shopping cart will display the product you have selected.</span></span>
6. <span data-ttu-id="e6ade-443">をクリックして、 **PayPal**チェック アウトするボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="e6ade-443">Click the **PayPal** button to checkout.</span></span> 

    ![精算と PayPal - カートによる支払い](checkout-and-payment-with-paypal/_static/image20.png)

   <span data-ttu-id="e6ade-445">チェック アウトすると、Wingtip Toys のサンプル アプリケーションのユーザー アカウントを持っている必要があります。</span><span class="sxs-lookup"><span data-stu-id="e6ade-445">Checking out will require that you have a user account for the Wingtip Toys sample application.</span></span>
7. <span data-ttu-id="e6ade-446">をクリックして、 **Google**既存 gmail.com 電子メール アカウントでログインするページの右上のリンク。</span><span class="sxs-lookup"><span data-stu-id="e6ade-446">Click the **Google** link on the right of the page to log in with an existing gmail.com email account.</span></span>  
   <span data-ttu-id="e6ade-447">Gmail.com アカウントがいない場合、テストの目的で 1 つを作成できます[www.gmail.com](https://www.gmail.com/)します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-447">If you do not have a gmail.com account, you can create one for testing purposes at [www.gmail.com](https://www.gmail.com/).</span></span> <span data-ttu-id="e6ade-448">登録する をクリックして、標準のローカル アカウントを使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-448">You can also use a standard local account by clicking "Register".</span></span> 

    ![チェック アウトと - PayPal による支払いログインします。](checkout-and-payment-with-paypal/_static/image21.png)
8. <span data-ttu-id="e6ade-450">Gmail アカウントとパスワードでサインインします。</span><span class="sxs-lookup"><span data-stu-id="e6ade-450">Sign in with your gmail account and password.</span></span> 

    ![精算と PayPal - Gmail サインインによる支払い](checkout-and-payment-with-paypal/_static/image22.png)
9. <span data-ttu-id="e6ade-452">をクリックして、**ログイン**を Wingtip Toys のサンプル アプリケーションのユーザー名、gmail アカウントを登録するボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="e6ade-452">Click the **Log in** button to register your gmail account with your Wingtip Toys sample application user name.</span></span> 

    ![精算と PayPal のアカウントの登録による支払い](checkout-and-payment-with-paypal/_static/image23.png)
10. <span data-ttu-id="e6ade-454">PayPal テスト サイトで追加、 **buyer**電子メール アドレスと、このチュートリアルで先ほど作成したパスワードをクリック、**ログで**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="e6ade-454">On the PayPal test site, add your **buyer** email address and password that you created earlier in this tutorial, then click the **Log In** button.</span></span> 

    ![精算と PayPal - サインイン PayPal による支払い](checkout-and-payment-with-paypal/_static/image24.png)
11. <span data-ttu-id="e6ade-456">PayPal ポリシーに同意し、をクリックして、**同意コンティニュ**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="e6ade-456">Agree to the PayPal policy and click the **Agree and Continue** button.</span></span>  
    <span data-ttu-id="e6ade-457">このページはのみに注意してくださいでは、この PayPal アカウントを使用して最初に表示されます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-457">Note that this page is only displayed the first time you use this PayPal account.</span></span> <span data-ttu-id="e6ade-458">これは、テスト アカウントを実際のコストは交換されませんを再度確認します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-458">Again note that this is a test account, no real money is exchanged.</span></span> 

    ![精算と PayPal - ポリシーの PayPal による支払い](checkout-and-payment-with-paypal/_static/image25.png)
12. <span data-ttu-id="e6ade-460">テスト環境の確認 ページとクリック PayPal で注文情報を確認**続行**します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-460">Review the order information on the PayPal testing environment review page and click **Continue**.</span></span> 

    ![精算と PayPal - 情報の確認による支払い](checkout-and-payment-with-paypal/_static/image26.png)
13. <span data-ttu-id="e6ade-462">*CheckoutReview.aspx*ページで、注文の合計を確認し、生成された出荷先住所を表示します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-462">On the *CheckoutReview.aspx* page, verify the order amount and view the generated shipping address.</span></span> <span data-ttu-id="e6ade-463">をクリックし、**注文完了**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="e6ade-463">Then, click the **Complete Order** button.</span></span> 

    ![精算と PayPal の注文の確認による支払い](checkout-and-payment-with-paypal/_static/image27.png)
14. <span data-ttu-id="e6ade-465">**CheckoutComplete.aspx**支払いトランザクションの ID を持つページが表示されます</span><span class="sxs-lookup"><span data-stu-id="e6ade-465">The **CheckoutComplete.aspx** page is displayed with a payment transaction ID.</span></span> 

    ![精算と PayPal のチェック アウトの完了による支払い](checkout-and-payment-with-paypal/_static/image28.png)

<a id="ReviewDBWebForms"></a>
## <a name="reviewing-the-database"></a><span data-ttu-id="e6ade-467">データベースを確認します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-467">Reviewing the Database</span></span>

<span data-ttu-id="e6ade-468">Wingtip Toys のサンプル アプリケーションのデータベース内のデータの更新を確認すると、アプリケーションを実行した後、アプリケーションが正常に製品の購入を記録することがわかります。</span><span class="sxs-lookup"><span data-stu-id="e6ade-468">By reviewing the updated data in the Wingtip Toys sample application database after running the application, you can see that the application successfully recorded the purchase of the products.</span></span>

<span data-ttu-id="e6ade-469">含まれるデータを検査することができます、 *Wingtiptoys.mdf*データベース ファイルを使用して、**データベース エクスプ ローラー**ウィンドウ (**サーバー エクスプ ローラー** Visual Studio のウィンドウ) と同様このチュートリアルのシリーズで前。</span><span class="sxs-lookup"><span data-stu-id="e6ade-469">You can inspect the data contained in the *Wingtiptoys.mdf* database file by using the **Database Explorer** window (**Server Explorer** window in Visual Studio) as you did earlier in this tutorial series.</span></span>

1. <span data-ttu-id="e6ade-470">まだ開いている場合は、ブラウザー ウィンドウを閉じます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-470">Close the browser window if it is still open.</span></span>
2. <span data-ttu-id="e6ade-471">Visual Studio で、選択、 **すべてのファイル**の上部にあるアイコン**ソリューション エクスプ ローラー**展開できるように、**アプリ\_データ**フォルダー。</span><span class="sxs-lookup"><span data-stu-id="e6ade-471">In Visual Studio, select the **Show All Files** icon at the top of **Solution Explorer** to allow you to expand the **App\_Data** folder.</span></span>
3. <span data-ttu-id="e6ade-472">展開、**アプリ\_データ**フォルダー。</span><span class="sxs-lookup"><span data-stu-id="e6ade-472">Expand the **App\_Data** folder.</span></span>  
 <span data-ttu-id="e6ade-473">選択する必要があります、 **すべてのファイル**フォルダーのアイコン。</span><span class="sxs-lookup"><span data-stu-id="e6ade-473">You may need to select the **Show All Files** icon for the folder.</span></span>
4. <span data-ttu-id="e6ade-474">右クリックし、 *Wingtiptoys.mdf*データベース ファイルと選択**オープン**します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-474">Right-click the *Wingtiptoys.mdf* database file and select **Open**.</span></span>  
    <span data-ttu-id="e6ade-475">**サーバー エクスプ ローラー**が表示されます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-475">**Server Explorer** is displayed.</span></span>
5. <span data-ttu-id="e6ade-476">展開、**テーブル**フォルダー。</span><span class="sxs-lookup"><span data-stu-id="e6ade-476">Expand the **Tables** folder.</span></span>
6. <span data-ttu-id="e6ade-477">右クリックし、**注文**テーブルを選択**テーブル データの表示**します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-477">Right-click the **Orders**table and select **Show Table Data**.</span></span>  
 <span data-ttu-id="e6ade-478">**注文**テーブルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="e6ade-478">The **Orders** table is displayed.</span></span>
7. <span data-ttu-id="e6ade-479">レビュー、 **PaymentTransactionID**列を成功したトランザクションを確認します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-479">Review the **PaymentTransactionID** column to confirm successful transactions.</span></span> 

    ![精算と PayPal - レビュー データベースによる支払い](checkout-and-payment-with-paypal/_static/image29.png)
8. <span data-ttu-id="e6ade-481">閉じる、**注文**テーブル ウィンドウ。</span><span class="sxs-lookup"><span data-stu-id="e6ade-481">Close the **Orders** table window.</span></span>
9. <span data-ttu-id="e6ade-482">サーバー エクスプ ローラーで右クリックし、 **OrderDetails**テーブルを選択**テーブル データの表示**します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-482">In the Server Explorer, right-click the **OrderDetails** table and select **Show Table Data**.</span></span>
10. <span data-ttu-id="e6ade-483">レビュー、`OrderId`と`Username`値、 **OrderDetails**テーブル。</span><span class="sxs-lookup"><span data-stu-id="e6ade-483">Review the `OrderId` and `Username` values in the **OrderDetails** table.</span></span> <span data-ttu-id="e6ade-484">これらの値に一致するメモ、`OrderId`と`Username`に含まれる値、**注文**テーブル。</span><span class="sxs-lookup"><span data-stu-id="e6ade-484">Note that these values match the `OrderId` and `Username` values included in the **Orders** table.</span></span>
11. <span data-ttu-id="e6ade-485">閉じる、 **OrderDetails**テーブル ウィンドウ。</span><span class="sxs-lookup"><span data-stu-id="e6ade-485">Close the **OrderDetails** table window.</span></span>
12. <span data-ttu-id="e6ade-486">Wingtip Toys のデータベース ファイルを右クリックして (*Wingtiptoys.mdf*) を選択および**接続を閉じる**します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-486">Right-click the Wingtip Toys database file (*Wingtiptoys.mdf*) and select **Close Connection**.</span></span>
13. <span data-ttu-id="e6ade-487">表示されない場合、**ソリューション エクスプ ローラー**ウィンドウで、をクリックして**ソリューション エクスプ ローラー**の下部にある、**サーバー エクスプ ローラー**ウィンドウに表示、**ソリューション エクスプ ローラー**もう一度です。</span><span class="sxs-lookup"><span data-stu-id="e6ade-487">If you do not see the **Solution Explorer** window, click **Solution Explorer** at the bottom of the **Server Explorer** window to show the **Solution Explorer** again.</span></span>

## <a name="summary"></a><span data-ttu-id="e6ade-488">まとめ</span><span class="sxs-lookup"><span data-stu-id="e6ade-488">Summary</span></span>

<span data-ttu-id="e6ade-489">このチュートリアルでは、製品の購入を追跡するためには、注文と注文詳細スキーマを追加します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-489">In this tutorial you added order and order detail schemas to track the purchase of products.</span></span> <span data-ttu-id="e6ade-490">また、PayPal の機能は、Wingtip Toys のサンプル アプリケーションに統合。</span><span class="sxs-lookup"><span data-stu-id="e6ade-490">You also integrated PayPal functionality into the Wingtip Toys sample application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e6ade-491">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="e6ade-491">Additional Resources</span></span>

<span data-ttu-id="e6ade-492">[ASP.NET の構成の概要](https://msdn.microsoft.com/library/ms178683(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="e6ade-492">[ASP.NET Configuration Overview](https://msdn.microsoft.com/library/ms178683(v=vs.100).aspx)</span></span>  
[<span data-ttu-id="e6ade-493">Azure App Service へのメンバーシップ、OAuth、SQL Database を使用したセキュリティで保護された ASP.NET Web フォーム アプリをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="e6ade-493">Deploy a Secure ASP.NET Web Forms App with Membership, OAuth, and SQL Database to Azure App Service</span></span>](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[<span data-ttu-id="e6ade-494">Microsoft Azure の無料試用版</span><span class="sxs-lookup"><span data-stu-id="e6ade-494">Microsoft Azure - Free Trial</span></span>](https://azure.microsoft.com/pricing/free-trial/)

## <a name="disclaimer"></a><span data-ttu-id="e6ade-495">免責情報</span><span class="sxs-lookup"><span data-stu-id="e6ade-495">Disclaimer</span></span>

<span data-ttu-id="e6ade-496">このチュートリアルには、サンプル コードが含まれています。</span><span class="sxs-lookup"><span data-stu-id="e6ade-496">This tutorial contains sample code.</span></span> <span data-ttu-id="e6ade-497">このようなサンプル コードでは、いかなる保証も伴わず「現状有姿を提供されます。 されます</span><span class="sxs-lookup"><span data-stu-id="e6ade-497">Such sample code is provided "as is" without warranty of any kind.</span></span> <span data-ttu-id="e6ade-498">したがって、Microsoft は正確性、整合性、または、サンプル コードの品質保証されません。</span><span class="sxs-lookup"><span data-stu-id="e6ade-498">Accordingly, Microsoft does not guarantee the accuracy, integrity, or quality of the sample code.</span></span> <span data-ttu-id="e6ade-499">ご自身の責任でサンプル コードを使用するものとします。</span><span class="sxs-lookup"><span data-stu-id="e6ade-499">You agree to use the sample code at your own risk.</span></span> <span data-ttu-id="e6ade-500">状況でも Microsoft 責任を負いかねますに何らかの方法でコンテンツを含む、すべてのサンプル コード、コンテンツ、または損失またはすべてのサンプル コードの使用の結果として発生したあらゆる種類の破損でのエラーまたはに限定されませんが、サンプル コード用。</span><span class="sxs-lookup"><span data-stu-id="e6ade-500">Under no circumstances will Microsoft be liable to you in any way for any sample code, content, including but not limited to, any errors or omissions in any sample code, content, or any loss or damage of any kind incurred as a result of the use of any sample code.</span></span> <span data-ttu-id="e6ade-501">改変通知が表示され補償、保存、および免責 Microsoft から、すべての損失、けがの損失や種類含めて、これらに限定して occasioned または投稿するマテリアルに起因の破損の要求に同意しないでください。送信で使用するかに限定されませんが、その中の見解を含むに依存します。</span><span class="sxs-lookup"><span data-stu-id="e6ade-501">You are hereby notified and do hereby agree to indemnify, save and hold Microsoft harmless from and against any and all loss, claims of loss, injury or damage of any kind including, without limitation, those occasioned by or arising from material that you post, transmit, use or rely on including, but not limited to, the views expressed therein.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e6ade-502">[前へ](shopping-cart.md)
> [次へ](membership-and-administration.md)</span><span class="sxs-lookup"><span data-stu-id="e6ade-502">[Previous](shopping-cart.md)
[Next](membership-and-administration.md)</span></span>
