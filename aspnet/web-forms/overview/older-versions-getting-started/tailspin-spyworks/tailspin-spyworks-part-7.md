---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
title: 'パート 7: 機能の追加 |Microsoft Docs'
author: JoeStagner
description: このチュートリアル シリーズでは、すべての Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。 第 7 部では、アカウントの確認などの追加機能を追加しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 50223ee9-11b9-4cf3-bca2-e2f10bf471f3
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
msc.type: authoredcontent
ms.openlocfilehash: 8cdde10981835877e5ac2f65860010920a68d0a2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389177"
---
<a name="part-7-adding-features"></a><span data-ttu-id="0f61c-104">パート 7: 機能の追加</span><span class="sxs-lookup"><span data-stu-id="0f61c-104">Part 7: Adding Features</span></span>
====================
<span data-ttu-id="0f61c-105">によって[Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="0f61c-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="0f61c-106">Tailspin Spyworks では、.NET プラットフォーム用の強力でスケーラブルなアプリケーションを作成するはどの非常に単純なを示します。</span><span class="sxs-lookup"><span data-stu-id="0f61c-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="0f61c-107">ASP.NET 4 の優れた新機能を使用して、ショッピング、チェック アウト、および管理を含む、オンライン ストアを構築する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="0f61c-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="0f61c-108">このチュートリアル シリーズでは、すべての Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="0f61c-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="0f61c-109">第 7 部では、アカウントの確認、製品レビュー、および「人気のある項目」および「も購入済み」のユーザー コントロールなどの追加機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="0f61c-109">Part 7 adds additional features, such as account review, product reviews, and "popular items" and "also purchased" user controls.</span></span>


## <a id="_Toc260221673"></a>  <span data-ttu-id="0f61c-110">機能の追加</span><span class="sxs-lookup"><span data-stu-id="0f61c-110">Adding Features</span></span>

<span data-ttu-id="0f61c-111">ユーザーが、カタログを参照できる、買い物かごにアイテムを配置、清算処理を完了ししましたが、サイトを向上させるために含まれるをサポートしている多数の機能があります。</span><span class="sxs-lookup"><span data-stu-id="0f61c-111">Though users can browse our catalog, place items in their shopping cart, and complete the checkout process, there are a number of supporting features that we will include to improve our site.</span></span>

1. <span data-ttu-id="0f61c-112">アカウントの確認 (リストから注文が配置され、詳細を表示します。)</span><span class="sxs-lookup"><span data-stu-id="0f61c-112">Account Review (List orders placed and view details.)</span></span>
2. <span data-ttu-id="0f61c-113">最初のページには、いくつかのコンテキストの特定のコンテンツを追加します。</span><span class="sxs-lookup"><span data-stu-id="0f61c-113">Add some context specific content to the front page.</span></span>
3. <span data-ttu-id="0f61c-114">カタログ内のユーザーがレビューできるようにするための機能、製品を追加します。</span><span class="sxs-lookup"><span data-stu-id="0f61c-114">Add a feature to let users Review the products in the catalog.</span></span>
4. <span data-ttu-id="0f61c-115">表紙を制御する人気のある項目と場所を表示するユーザー コントロールを作成します。</span><span class="sxs-lookup"><span data-stu-id="0f61c-115">Create a User Control to display Popular Items and Place that control on the front page.</span></span>
5. <span data-ttu-id="0f61c-116">「も購入した」ユーザー コントロールを作成し、製品の詳細ページに追加します。</span><span class="sxs-lookup"><span data-stu-id="0f61c-116">Create an "Also Purchased" user control and add it to the product details page.</span></span>
6. <span data-ttu-id="0f61c-117">連絡先を追加するページ。</span><span class="sxs-lookup"><span data-stu-id="0f61c-117">Add a Contact Page.</span></span>
7. <span data-ttu-id="0f61c-118">追加、ページについて。</span><span class="sxs-lookup"><span data-stu-id="0f61c-118">Add an About Page.</span></span>
8. <span data-ttu-id="0f61c-119">グローバル エラー</span><span class="sxs-lookup"><span data-stu-id="0f61c-119">Global Error</span></span>

## <a id="_Toc260221674"></a>  <span data-ttu-id="0f61c-120">アカウントの確認</span><span class="sxs-lookup"><span data-stu-id="0f61c-120">Account Review</span></span>

<span data-ttu-id="0f61c-121">「アカウント」フォルダーで 1 つの名前付き OrderList.aspx とその他の名前付きの OrderDetails.aspx の 2 つの .aspx ページを作成します</span><span class="sxs-lookup"><span data-stu-id="0f61c-121">In the "Account" folder create two .aspx pages one named OrderList.aspx and the other named OrderDetails.aspx</span></span>

<span data-ttu-id="0f61c-122">私たちがある以前と同様、OrderList.aspx は GridView と EntityDataSoure コントロールを活用します。</span><span class="sxs-lookup"><span data-stu-id="0f61c-122">OrderList.aspx will leverage the GridView and EntityDataSoure controls much as we have previously.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample1.aspx)]

<span data-ttu-id="0f61c-123">EntityDataSoure、ユーザー名でフィルター処理、Orders テーブルからレコードを選択します (WhereParameter を参照してください) をユーザー ログオンのときにセッション変数に設定します。</span><span class="sxs-lookup"><span data-stu-id="0f61c-123">The EntityDataSoure selects records from the Orders table filtered on the UserName (see the WhereParameter) which we set in a session variable when the user log's in.</span></span>

<span data-ttu-id="0f61c-124">GridView の内でこれらのパラメーターにも注意してください。</span><span class="sxs-lookup"><span data-stu-id="0f61c-124">Note also these parameters in the HyperlinkField of the GridView:</span></span>

[!code-xml[Main](tailspin-spyworks-part-7/samples/sample2.xml)]

<span data-ttu-id="0f61c-125">これらの OrderDetails.aspx ページへのクエリ文字列パラメーターとして、OrderID フィールドを指定する各製品の注文の詳細ビューへのリンクを指定します。</span><span class="sxs-lookup"><span data-stu-id="0f61c-125">These specify the link to the Order details view for each product specifying the OrderID field as a QueryString parameter to the OrderDetails.aspx page.</span></span>

## <a id="_Toc260221675"></a>  <span data-ttu-id="0f61c-126">OrderDetails.aspx</span><span class="sxs-lookup"><span data-stu-id="0f61c-126">OrderDetails.aspx</span></span>

<span data-ttu-id="0f61c-127">EntityDataSource コントロールを注文と FormView、注文データと別の EntityDataSource による GridView を表示するのにすべての注文の品目を表示するのにへのアクセスに使用します。</span><span class="sxs-lookup"><span data-stu-id="0f61c-127">We will use an EntityDataSource control to access the Orders and a FormView to display the Order data and another EntityDataSource with a GridView to display all the Order's line items.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample3.aspx)]

<span data-ttu-id="0f61c-128">分離コード ファイル (OrderDetails.aspx.cs) では、ハウスキーピングの 2 つの小さなビットがあります。</span><span class="sxs-lookup"><span data-stu-id="0f61c-128">In the Code Behind file (OrderDetails.aspx.cs) we have two little bits of housekeeping.</span></span>

<span data-ttu-id="0f61c-129">まず OrderDetails が常に、OrderId を取得するかどうかを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0f61c-129">First we need to make sure that OrderDetails always gets an OrderId.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample4.cs)]

<span data-ttu-id="0f61c-130">また、計算され、注文の品目の合計を表示する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0f61c-130">We also need to calculate and display the order total from the line items.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample5.cs)]

## <a id="_Toc260221676"></a>  <span data-ttu-id="0f61c-131">ホーム ページ</span><span class="sxs-lookup"><span data-stu-id="0f61c-131">The Home Page</span></span>

<span data-ttu-id="0f61c-132">Default.aspx ページには、いくつかの静的コンテンツを追加してみましょう。</span><span class="sxs-lookup"><span data-stu-id="0f61c-132">Let's add some static content to the Default.aspx page.</span></span>

<span data-ttu-id="0f61c-133">最初に「コンテンツ」フォルダーを作成しますその中で、Images フォルダー (およびホーム ページで使用されるイメージに含めます)。</span><span class="sxs-lookup"><span data-stu-id="0f61c-133">First I'll create a "Content" folder and within it an Images folder (and I'll include an image to be used on the home page.)</span></span>

<span data-ttu-id="0f61c-134">Default.aspx ページの下部にあるプレース ホルダーには、次のマークアップを追加します。</span><span class="sxs-lookup"><span data-stu-id="0f61c-134">Into the bottom placeholder of the Default.aspx page, add the following markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample6.aspx)]

## <a id="_Toc260221677"></a>  <span data-ttu-id="0f61c-135">製品のレビュー</span><span class="sxs-lookup"><span data-stu-id="0f61c-135">Product Reviews</span></span>

<span data-ttu-id="0f61c-136">最初に製品のレビューを入力するために使用できるフォームのリンク付きのボタンを追加します。</span><span class="sxs-lookup"><span data-stu-id="0f61c-136">First we'll add a button with a link to a form that we can use to enter a product review.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample7.aspx)]

![](tailspin-spyworks-part-7/_static/image1.jpg)

<span data-ttu-id="0f61c-137">クエリ文字列で、ProductID を渡していることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="0f61c-137">Note that we are passing the ProductID in the query string</span></span>

<span data-ttu-id="0f61c-138">[次へ] ReviewAdd.aspx という名前のページを追加してみましょう</span><span class="sxs-lookup"><span data-stu-id="0f61c-138">Next let's add page named ReviewAdd.aspx</span></span>

<span data-ttu-id="0f61c-139">このページでは、ASP.NET AJAX Control Toolkit を使用します。</span><span class="sxs-lookup"><span data-stu-id="0f61c-139">This page will use the ASP.NET AJAX Control Toolkit.</span></span> <span data-ttu-id="0f61c-140">かどうかを行っていないためからダウンロードできます[DevExpress](http://devexpress.com/act)ここで Visual Studio で使用するためのツールキットの設定に関するガイダンスがあると[ https://www.asp.net/learn/ajax-videos/video-76.aspx](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md)します。</span><span class="sxs-lookup"><span data-stu-id="0f61c-140">If you have not already done so you can download it from [DevExpress](http://devexpress.com/act) and there is guidance on setting up the toolkit for use with Visual Studio here [https://www.asp.net/learn/ajax-videos/video-76.aspx](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).</span></span>

<span data-ttu-id="0f61c-141">デザイン モードでコントロールと検証コントロールをツールボックスからドラッグし、次のようにフォームを作成します。</span><span class="sxs-lookup"><span data-stu-id="0f61c-141">In design mode, drag controls and validators from the toolbox and build a form like the one below.</span></span>

![](tailspin-spyworks-part-7/_static/image2.jpg)

<span data-ttu-id="0f61c-142">マークアップは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="0f61c-142">The markup will look something like this.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample8.aspx)]

<span data-ttu-id="0f61c-143">レビューを入力しましたが、これで、製品ページにこれらのレビューを表示することができます。</span><span class="sxs-lookup"><span data-stu-id="0f61c-143">Now that we can enter reviews, lets display those reviews on the product page.</span></span>

<span data-ttu-id="0f61c-144">ProductDetails.aspx ページには、このマークアップを追加します。</span><span class="sxs-lookup"><span data-stu-id="0f61c-144">Add this markup to the ProductDetails.aspx page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample9.aspx)]

<span data-ttu-id="0f61c-145">これで、アプリケーションを実行している製品に移動して、顧客レビューなどの製品情報を示します。</span><span class="sxs-lookup"><span data-stu-id="0f61c-145">Running our application now and navigating to a product shows the product information including customer reviews.</span></span>

![](tailspin-spyworks-part-7/_static/image3.jpg)

## <a id="_Toc260221678"></a>  <span data-ttu-id="0f61c-146">人気のある項目のコントロール (ユーザー コントロールを作成する)</span><span class="sxs-lookup"><span data-stu-id="0f61c-146">Popular Items Control (Creating User Controls)</span></span>

<span data-ttu-id="0f61c-147">Web サイトの売上を強化するためには、「推奨の販売」人気のあるまたは関連する製品に、いくつかの機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="0f61c-147">In order to increase sales on your web site we will add a couple of features to "suggestive sell" popular or related products.</span></span>

<span data-ttu-id="0f61c-148">これらの機能の 1 つ目は、製品カタログで人気の製品の一覧になります。</span><span class="sxs-lookup"><span data-stu-id="0f61c-148">The first of these features will be a list of the more popular product in our product catalog.</span></span>

<span data-ttu-id="0f61c-149">最も売れているアプリケーションのホーム ページ上の項目を表示する「ユーザー コントロール」を作成します。</span><span class="sxs-lookup"><span data-stu-id="0f61c-149">We will create a "User Control" to display the top selling items on the home page of our application.</span></span> <span data-ttu-id="0f61c-150">これは、コントロールであるが、ためには、ドラッグ アンドを気に入っている任意のページに、Visual Studio のデザイナーでコントロールを削除するだけで任意のページで使用できます。</span><span class="sxs-lookup"><span data-stu-id="0f61c-150">Since this will be a control, we can use it on any page by simply dragging and dropping the control in Visual Studio's designer onto any page that we like.</span></span>

<span data-ttu-id="0f61c-151">Visual Studio のソリューション エクスプ ローラーでソリューション名を右クリックし、「コントロール」をという名前の新しいディレクトリを作成します。</span><span class="sxs-lookup"><span data-stu-id="0f61c-151">In Visual Studio's solutions explorer, right-click on the solution name and create a new directory named "Controls".</span></span> <span data-ttu-id="0f61c-152">これを行うには必要ありませんが、「コントロール」のディレクトリで、すべてのユーザー コントロールを作成して整理された、プロジェクトの継続的されます。</span><span class="sxs-lookup"><span data-stu-id="0f61c-152">While it is not necessary to do so, we will help keep our project organized by creating all our user controls in the "Controls" directory.</span></span>

<span data-ttu-id="0f61c-153">コントロールのフォルダーを右クリックし、"新しい項目 を選択します。</span><span class="sxs-lookup"><span data-stu-id="0f61c-153">Right-click on the controls folder and choose "New Item" :</span></span>

![](tailspin-spyworks-part-7/_static/image4.jpg)

<span data-ttu-id="0f61c-154">"PopularItems"のユーザーのコントロールの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="0f61c-154">Specify a name for our control of "PopularItems".</span></span> <span data-ttu-id="0f61c-155">ユーザー コントロールのファイル拡張子が .ascx .aspx できませんに注意してください。</span><span class="sxs-lookup"><span data-stu-id="0f61c-155">Note that the file extension for user controls is .ascx not .aspx.</span></span>

<span data-ttu-id="0f61c-156">人気のある項目のユーザー コントロールは次のように定義されます。</span><span class="sxs-lookup"><span data-stu-id="0f61c-156">Our Popular Items User control will be defined as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample10.aspx)]

<span data-ttu-id="0f61c-157">ここでこのアプリケーションでまだ使用がないメソッドを使用しています。</span><span class="sxs-lookup"><span data-stu-id="0f61c-157">Here we're using a method we have not used yet in this application.</span></span> <span data-ttu-id="0f61c-158">Repeater コントロールを使用していることとデータ ソース コントロールを使用する代わりに、LINQ to Entities クエリの結果を Repeater コントロールをバインドしているいます。</span><span class="sxs-lookup"><span data-stu-id="0f61c-158">We're using the repeater control and instead of using a data source control we're binding the Repeater Control to the results of a LINQ to Entities query.</span></span>

<span data-ttu-id="0f61c-159">コントロールの分離コードのようなこととしては、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="0f61c-159">In the code behind of our control we do that as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample11.cs)]

<span data-ttu-id="0f61c-160">また、コントロールのマークアップの上部にある重要な行に注意してください。</span><span class="sxs-lookup"><span data-stu-id="0f61c-160">Note also this important line at the top of our control's markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample12.aspx)]

<span data-ttu-id="0f61c-161">分単位で最も人気のある項目を変更しないため、アプリケーションのパフォーマンスを向上させるために出るディレクティブを追加できます。</span><span class="sxs-lookup"><span data-stu-id="0f61c-161">Since the most popular items won't be changing on a minute to minute basis we can add a aching directive to improve the performance of our application.</span></span> <span data-ttu-id="0f61c-162">このディレクティブは、コントロールのキャッシュされた出力の有効期限が切れる場合に、実行されるのみコントロール コードになります。</span><span class="sxs-lookup"><span data-stu-id="0f61c-162">This directive will cause the controls code to only be executed when the cached output of the control expires.</span></span> <span data-ttu-id="0f61c-163">それ以外の場合は、コントロールの出力のキャッシュされたバージョンが使用されます。</span><span class="sxs-lookup"><span data-stu-id="0f61c-163">Otherwise, the cached version of the control's output will be used.</span></span>

<span data-ttu-id="0f61c-164">次に行う必要があるは、Default.aspc ページで、新しいコントロールを含めます。</span><span class="sxs-lookup"><span data-stu-id="0f61c-164">Now all we have to do is include our new control in our Default.aspc page.</span></span>

<span data-ttu-id="0f61c-165">ドラッグ アンド ドロップで、既定のフォームのオープンの列に、コントロールのインスタンスを配置します。</span><span class="sxs-lookup"><span data-stu-id="0f61c-165">Use drag and drop to place an instance of the control in the open column of our Default form.</span></span>

![](tailspin-spyworks-part-7/_static/image5.jpg)

<span data-ttu-id="0f61c-166">これで、アプリケーションの実行ときに、ホーム ページには、最も人気のある項目が表示されます。</span><span class="sxs-lookup"><span data-stu-id="0f61c-166">Now when we run our application the home page displays the most popular items.</span></span>

![](tailspin-spyworks-part-7/_static/image6.jpg)

## <a id="_Toc260221679"></a>  <span data-ttu-id="0f61c-167">(パラメーターを持つユーザー コントロール) を制御する「も購入」</span><span class="sxs-lookup"><span data-stu-id="0f61c-167">"Also Purchased" Control (User Controls with Parameters)</span></span>

<span data-ttu-id="0f61c-168">コンテキストの特異性を追加することで、次のレベルへの販売挑発的な方で作成する 2 つ目のユーザー コントロールになります。</span><span class="sxs-lookup"><span data-stu-id="0f61c-168">The second User Control that we'll create will take suggestive selling to the next level by adding context specificity.</span></span>

<span data-ttu-id="0f61c-169">最初の「も購入」の項目を計算するロジックが自明でないです。</span><span class="sxs-lookup"><span data-stu-id="0f61c-169">The logic to calculate the top "Also Purchased" items is non-trivial.</span></span>

<span data-ttu-id="0f61c-170">「も購入」コントロールは、現在選択されている ProductID に対して (以前に購入された) OrderDetails レコードを選択しが見つかった各一意の注文の Orderid を取得します。</span><span class="sxs-lookup"><span data-stu-id="0f61c-170">Our "Also Purchased" control will select the OrderDetails records (previously purchased) for the currently selected ProductID and grab the OrderIDs for each unique order that is found.</span></span>

<span data-ttu-id="0f61c-171">私たちは製品を選択 al からこれらすべての注文と合計数量を購入しました。</span><span class="sxs-lookup"><span data-stu-id="0f61c-171">Then we will select al the products from all those Orders and sum the quantities purchased.</span></span> <span data-ttu-id="0f61c-172">製品を並べ替えるその量の合計がされ、上位 5 つの項目を表示します。</span><span class="sxs-lookup"><span data-stu-id="0f61c-172">We'll sort the products by that quantity sum and display the top five items.</span></span>

<span data-ttu-id="0f61c-173">このロジックの複雑さを考えると、ストアド プロシージャとして、このアルゴリズムを実装しましたします。</span><span class="sxs-lookup"><span data-stu-id="0f61c-173">Given the complexity of this logic, we will implement this algorithm as a stored procedure.</span></span>

<span data-ttu-id="0f61c-174">ストアド プロシージャの T-SQL では次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="0f61c-174">The T-SQL for the stored procedure is as follows.</span></span>

[!code-sql[Main](tailspin-spyworks-part-7/samples/sample13.sql)]

<span data-ttu-id="0f61c-175">アプリケーションでは、テーブルと、必要な Entity Data Model ビューだけでなく、指定したエンティティ データ モデルを生成したときに含めて ときにデータベースでこのストアド プロシージャ (SelectPurchasedWithProducts) が存在したことに注意してください。このストアド プロシージャを含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="0f61c-175">Note that this stored procedure (SelectPurchasedWithProducts) existed in the database when we included it in our application and when we generated the Entity Data Model we specified that, in addition to the Tables and Views that we needed, the Entity Data Model should include this stored procedure.</span></span>

<span data-ttu-id="0f61c-176">Entity Data Model からストアド プロシージャにアクセスするには、関数をインポートする必要があります。</span><span class="sxs-lookup"><span data-stu-id="0f61c-176">To access the stored procedure from the Entity Data Model we need to import the function.</span></span>

<span data-ttu-id="0f61c-177">ダブルクリックしてデザイナーで開き、モデル ブラウザーを開き、ソリューション エクスプ ローラーで、Entity Data Model デザイナー内を右クリックし、「関数インポートの追加」を選択します。</span><span class="sxs-lookup"><span data-stu-id="0f61c-177">Double Click on the Entity Data Model in the Solutions Explorer to open it in the designer and open the Model Browser, then right-click in the designer and select "Add Function Import".</span></span>

![](tailspin-spyworks-part-7/_static/image1.png)

<span data-ttu-id="0f61c-178">そうと、このダイアログ ボックスが開きます。</span><span class="sxs-lookup"><span data-stu-id="0f61c-178">Doing so will open this dialog.</span></span>

![](tailspin-spyworks-part-7/_static/image2.png)

<span data-ttu-id="0f61c-179">"SelectPurchasedWithProducts"を選択すると、上記に参照するようにしてフィールドに入力し、インポートされた関数の名前のプロシージャ名を使用します。</span><span class="sxs-lookup"><span data-stu-id="0f61c-179">Fill out the fields as you see above, selecting the "SelectPurchasedWithProducts" and use the procedure name for the name of our imported function.</span></span>

<span data-ttu-id="0f61c-180">[Ok] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="0f61c-180">Click "Ok".</span></span>

<span data-ttu-id="0f61c-181">かもしれませんモデルの他のアイテムとしてストアド プロシージャに対してプログラミングできる単にこれを行うこと。</span><span class="sxs-lookup"><span data-stu-id="0f61c-181">Having done this we can simply program against the stored procedure as we might any other item in the model.</span></span>

<span data-ttu-id="0f61c-182">そのため、「コントロール」フォルダーでは、AlsoPurchased.ascx という名前の新しいユーザー コントロールを作成します。</span><span class="sxs-lookup"><span data-stu-id="0f61c-182">So, in our "Controls" folder create a new user control named AlsoPurchased.ascx.</span></span>

<span data-ttu-id="0f61c-183">このコントロールのマークアップは非常に使いやすく PopularItems コントロールになります。</span><span class="sxs-lookup"><span data-stu-id="0f61c-183">The markup for this control will look very familiar to the PopularItems control.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample14.aspx)]

<span data-ttu-id="0f61c-184">重要な違いは、出力キャッシュしないすると、項目の表示には製品によって異なるためです。</span><span class="sxs-lookup"><span data-stu-id="0f61c-184">The notable difference is that are not caching the output since the item's to be rendered will differ by product.</span></span>

<span data-ttu-id="0f61c-185">ProductId をコントロールに"property"となります。</span><span class="sxs-lookup"><span data-stu-id="0f61c-185">The ProductId will be a "property" to the control.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample15.cs)]

<span data-ttu-id="0f61c-186">コントロールの PreRender イベント ハンドラーで次の 3 つの作業を行う eed します。</span><span class="sxs-lookup"><span data-stu-id="0f61c-186">In the control's PreRender event handler we eed to do three things.</span></span>

1. <span data-ttu-id="0f61c-187">ProductID が設定されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="0f61c-187">Make sure the ProductID is set.</span></span>
2. <span data-ttu-id="0f61c-188">現在の 1 つで購入された製品があるかどうかを参照してください。</span><span class="sxs-lookup"><span data-stu-id="0f61c-188">See if there are any products that have been purchased with the current one.</span></span>
3. <span data-ttu-id="0f61c-189">手順 2 で決定されるいくつかの項目を出力します。</span><span class="sxs-lookup"><span data-stu-id="0f61c-189">Output some items as determined in #2.</span></span>

<span data-ttu-id="0f61c-190">モデルを使ってストアド プロシージャを呼び出すがいかに簡単かに注意してください。</span><span class="sxs-lookup"><span data-stu-id="0f61c-190">Note how easy it is to call the stored procedure through the model.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample16.cs)]

<span data-ttu-id="0f61c-191">ですが、「も購入」を決定した後だけです、repeater、クエリによって返される結果にバインドできます。</span><span class="sxs-lookup"><span data-stu-id="0f61c-191">After determining that there ARE "also purchased" we can simply bind the repeater to the results returned by the query.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample17.cs)]

<span data-ttu-id="0f61c-192">「も購入」項目がない場合は、弊社のカタログから他の人気のある項目を表示しますがだけです。</span><span class="sxs-lookup"><span data-stu-id="0f61c-192">If there were not any "also purchased" items we'll simply display other popular items from our catalog.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample18.cs)]

<span data-ttu-id="0f61c-193">「購入も」アイテムを表示するには、ProductDetails.aspx ページを開き、マークアップでは、この位置に表示されるように、ソリューション エクスプ ローラーから AlsoPurchased コントロールをドラッグします。</span><span class="sxs-lookup"><span data-stu-id="0f61c-193">To view the "Also Purchased" items, open the ProductDetails.aspx page and drag the AlsoPurchased control from the Solutions Explorer so that it appears in this position in the markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample19.aspx)]

<span data-ttu-id="0f61c-194">そうと、ProductDetails ページの上部にあるコントロールへの参照が作成されます。</span><span class="sxs-lookup"><span data-stu-id="0f61c-194">Doing so will create a reference to the control at the top of the ProductDetails page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample20.aspx)]

<span data-ttu-id="0f61c-195">AlsoPurchased ユーザー コントロールには、ページの現在のデータ モデルのアイテムに対しては Eval ステートメントを使用して、コントロールの ProductID プロパティを設定しますが、ProductId 番号が必要です。</span><span class="sxs-lookup"><span data-stu-id="0f61c-195">Since the AlsoPurchased user control requires a ProductId number we will set the ProductID property of our control by using an Eval statement against the current data model item of the page.</span></span>

![](tailspin-spyworks-part-7/_static/image3.png)

<span data-ttu-id="0f61c-196">開発し今すぐ実行および成果物への参照時に、「購入も」アイテムがわかります。</span><span class="sxs-lookup"><span data-stu-id="0f61c-196">When we build and run now and browse to a product we see the "Also Purchased" items.</span></span>

![](tailspin-spyworks-part-7/_static/image7.jpg)

> [!div class="step-by-step"]
> <span data-ttu-id="0f61c-197">[前へ](tailspin-spyworks-part-6.md)
> [次へ](tailspin-spyworks-part-8.md)</span><span class="sxs-lookup"><span data-stu-id="0f61c-197">[Previous](tailspin-spyworks-part-6.md)
[Next](tailspin-spyworks-part-8.md)</span></span>
