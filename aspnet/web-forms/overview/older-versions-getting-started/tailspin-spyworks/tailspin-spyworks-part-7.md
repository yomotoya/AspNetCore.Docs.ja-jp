---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
title: '手順 7: 機能の追加 |Microsoft ドキュメント'
author: JoeStagner
description: このチュートリアルの系列では、すべて Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。 第 7 部では、アカウント revie などの追加機能を追加しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 50223ee9-11b9-4cf3-bca2-e2f10bf471f3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
msc.type: authoredcontent
ms.openlocfilehash: 17f068155f6726047901e2f7d580d375a4e07c87
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30888394"
---
<a name="part-7-adding-features"></a><span data-ttu-id="79d09-104">手順 7: 追加の機能</span><span class="sxs-lookup"><span data-stu-id="79d09-104">Part 7: Adding Features</span></span>
====================
<span data-ttu-id="79d09-105">によって[行える](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="79d09-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="79d09-106">Tailspin Spyworks では、.NET プラットフォーム用の強力な拡張性の高いアプリケーションを作成するはどのように非常に単純なを示しています。</span><span class="sxs-lookup"><span data-stu-id="79d09-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="79d09-107">ASP.NET 4 の優れた新機能を使用して、ショッピング、チェック アウト、および管理を含む、オンライン ストアをビルドする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="79d09-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="79d09-108">このチュートリアルの系列では、すべて Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="79d09-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="79d09-109">第 7 部では、アカウントの確認、製品レビュー、および「人気のある項目」および「も購入済み」のユーザー コントロールなどの追加機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="79d09-109">Part 7 adds additional features, such as account review, product reviews, and "popular items" and "also purchased" user controls.</span></span>


## <a id="_Toc260221673"></a>  <span data-ttu-id="79d09-110">機能の追加</span><span class="sxs-lookup"><span data-stu-id="79d09-110">Adding Features</span></span>

<span data-ttu-id="79d09-111">ユーザーは、弊社のカタログを参照できる、ショッピング カートにアイテムを配置とチェック アウト プロセスを完了、数をサポートする機能のサイトを向上させることが含まれているがあります。</span><span class="sxs-lookup"><span data-stu-id="79d09-111">Though users can browse our catalog, place items in their shopping cart, and complete the checkout process, there are a number of supporting features that we will include to improve our site.</span></span>

1. <span data-ttu-id="79d09-112">アカウントの確認 (一覧からの注文が配置され、詳細を表示します。)</span><span class="sxs-lookup"><span data-stu-id="79d09-112">Account Review (List orders placed and view details.)</span></span>
2. <span data-ttu-id="79d09-113">最初のページがいくつかのコンテキストの特定のコンテンツを追加します。</span><span class="sxs-lookup"><span data-stu-id="79d09-113">Add some context specific content to the front page.</span></span>
3. <span data-ttu-id="79d09-114">カタログにユーザーがレビューできるように、機能、製品を追加します。</span><span class="sxs-lookup"><span data-stu-id="79d09-114">Add a feature to let users Review the products in the catalog.</span></span>
4. <span data-ttu-id="79d09-115">表示する人気のある項目の場所を制御する最初のページのユーザー コントロールを作成します。</span><span class="sxs-lookup"><span data-stu-id="79d09-115">Create a User Control to display Popular Items and Place that control on the front page.</span></span>
5. <span data-ttu-id="79d09-116">「購入も」ユーザー コントロールを作成し、製品の詳細ページに追加します。</span><span class="sxs-lookup"><span data-stu-id="79d09-116">Create an "Also Purchased" user control and add it to the product details page.</span></span>
6. <span data-ttu-id="79d09-117">連絡先を追加するページ。</span><span class="sxs-lookup"><span data-stu-id="79d09-117">Add a Contact Page.</span></span>
7. <span data-ttu-id="79d09-118">追加、ページに関するします。</span><span class="sxs-lookup"><span data-stu-id="79d09-118">Add an About Page.</span></span>
8. <span data-ttu-id="79d09-119">グローバル エラー</span><span class="sxs-lookup"><span data-stu-id="79d09-119">Global Error</span></span>

## <a id="_Toc260221674"></a>  <span data-ttu-id="79d09-120">アカウントの確認</span><span class="sxs-lookup"><span data-stu-id="79d09-120">Account Review</span></span>

<span data-ttu-id="79d09-121">「アカウント」フォルダーに 2 つの .aspx ページ 1 つの名前付き OrderList.aspx およびその他の名前付き OrderDetails.aspx を作成します</span><span class="sxs-lookup"><span data-stu-id="79d09-121">In the "Account" folder create two .aspx pages one named OrderList.aspx and the other named OrderDetails.aspx</span></span>

<span data-ttu-id="79d09-122">ここがある以前と同様、OrderList.aspx は、GridView と EntityDataSoure コントロールを活用します。</span><span class="sxs-lookup"><span data-stu-id="79d09-122">OrderList.aspx will leverage the GridView and EntityDataSoure controls much as we have previously.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample1.aspx)]

<span data-ttu-id="79d09-123">EntityDataSoure ユーザー名でフィルター処理、Orders テーブルからレコードを選択する (、WhereParameter を参照してください) のユーザーのログオン時にセッション変数に設定します。</span><span class="sxs-lookup"><span data-stu-id="79d09-123">The EntityDataSoure selects records from the Orders table filtered on the UserName (see the WhereParameter) which we set in a session variable when the user log's in.</span></span>

<span data-ttu-id="79d09-124">GridView の内でこれらのパラメーターにも注意してください。</span><span class="sxs-lookup"><span data-stu-id="79d09-124">Note also these parameters in the HyperlinkField of the GridView:</span></span>

[!code-xml[Main](tailspin-spyworks-part-7/samples/sample2.xml)]

<span data-ttu-id="79d09-125">これらの OrderDetails.aspx ページへのクエリ文字列パラメーターとして、OrderID フィールドを指定する各製品の注文の詳細ビューへのリンクを指定します。</span><span class="sxs-lookup"><span data-stu-id="79d09-125">These specify the link to the Order details view for each product specifying the OrderID field as a QueryString parameter to the OrderDetails.aspx page.</span></span>

## <a id="_Toc260221675"></a>  <span data-ttu-id="79d09-126">OrderDetails.aspx</span><span class="sxs-lookup"><span data-stu-id="79d09-126">OrderDetails.aspx</span></span>

<span data-ttu-id="79d09-127">EntityDataSource コントロールへのアクセス、注文と、FormView 注文データと、GridView と別の EntityDataSource を表示するのにすべての注文の品目を表示するのに使用します。</span><span class="sxs-lookup"><span data-stu-id="79d09-127">We will use an EntityDataSource control to access the Orders and a FormView to display the Order data and another EntityDataSource with a GridView to display all the Order's line items.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample3.aspx)]

<span data-ttu-id="79d09-128">分離コード ファイル (OrderDetails.aspx.cs) では、ハウスキーピングのわずか 2 つのビットがあります。</span><span class="sxs-lookup"><span data-stu-id="79d09-128">In the Code Behind file (OrderDetails.aspx.cs) we have two little bits of housekeeping.</span></span>

<span data-ttu-id="79d09-129">まず OrderDetails が常に、OrderId を取得するかどうかを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="79d09-129">First we need to make sure that OrderDetails always gets an OrderId.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample4.cs)]

<span data-ttu-id="79d09-130">また、計算され、注文の品目の合計を表示する必要があります。</span><span class="sxs-lookup"><span data-stu-id="79d09-130">We also need to calculate and display the order total from the line items.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample5.cs)]

## <a id="_Toc260221676"></a>  <span data-ttu-id="79d09-131">ホーム ページ</span><span class="sxs-lookup"><span data-stu-id="79d09-131">The Home Page</span></span>

<span data-ttu-id="79d09-132">Default.aspx ページには、いくつかの静的なコンテンツを追加してみましょう。</span><span class="sxs-lookup"><span data-stu-id="79d09-132">Let's add some static content to the Default.aspx page.</span></span>

<span data-ttu-id="79d09-133">まず「コンテンツ」フォルダーを作成し、その中 Images フォルダー (およびホーム ページで使用する画像を追加します)。</span><span class="sxs-lookup"><span data-stu-id="79d09-133">First I'll create a "Content" folder and within it an Images folder (and I'll include an image to be used on the home page.)</span></span>

<span data-ttu-id="79d09-134">Default.aspx ページの下部のプレース ホルダーには、次のマークアップを追加します。</span><span class="sxs-lookup"><span data-stu-id="79d09-134">Into the bottom placeholder of the Default.aspx page, add the following markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample6.aspx)]

## <a id="_Toc260221677"></a>  <span data-ttu-id="79d09-135">製品のレビュー</span><span class="sxs-lookup"><span data-stu-id="79d09-135">Product Reviews</span></span>

<span data-ttu-id="79d09-136">まず、リンク ボタン追加、製品のレビューの入力に使用できる形式にします。</span><span class="sxs-lookup"><span data-stu-id="79d09-136">First we'll add a button with a link to a form that we can use to enter a product review.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample7.aspx)]

![](tailspin-spyworks-part-7/_static/image1.jpg)

<span data-ttu-id="79d09-137">クエリ文字列に、ProductID を渡していることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="79d09-137">Note that we are passing the ProductID in the query string</span></span>

<span data-ttu-id="79d09-138">[次へ] ReviewAdd.aspx をという名前のページを追加してみましょう。</span><span class="sxs-lookup"><span data-stu-id="79d09-138">Next let's add page named ReviewAdd.aspx</span></span>

<span data-ttu-id="79d09-139">このページでは、ASP.NET AJAX コントロール ツールキットを使用します。</span><span class="sxs-lookup"><span data-stu-id="79d09-139">This page will use the ASP.NET AJAX Control Toolkit.</span></span> <span data-ttu-id="79d09-140">かどうかをまだ行っていないからダウンロードすることができますので[DevExpress](http://devexpress.com/act) 、toolkit for Visual Studio での使用は、ここを設定する方法のガイダンスと[ https://www.asp.net/learn/ajax-videos/video-76.aspx](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md)です。</span><span class="sxs-lookup"><span data-stu-id="79d09-140">If you have not already done so you can download it from [DevExpress](http://devexpress.com/act) and there is guidance on setting up the toolkit for use with Visual Studio here [https://www.asp.net/learn/ajax-videos/video-76.aspx](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).</span></span>

<span data-ttu-id="79d09-141">デザイン モードでは、コントロールと検証コントロールをツールボックスからドラッグし、以下のようなフォームを作成します。</span><span class="sxs-lookup"><span data-stu-id="79d09-141">In design mode, drag controls and validators from the toolbox and build a form like the one below.</span></span>

![](tailspin-spyworks-part-7/_static/image2.jpg)

<span data-ttu-id="79d09-142">マークアップは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="79d09-142">The markup will look something like this.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample8.aspx)]

<span data-ttu-id="79d09-143">レビューを入力するには、これでは、製品 ページでこれらのレビューを表示することができます。</span><span class="sxs-lookup"><span data-stu-id="79d09-143">Now that we can enter reviews, lets display those reviews on the product page.</span></span>

<span data-ttu-id="79d09-144">このマークアップを ProductDetails.aspx ページに追加します。</span><span class="sxs-lookup"><span data-stu-id="79d09-144">Add this markup to the ProductDetails.aspx page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample9.aspx)]

<span data-ttu-id="79d09-145">これで、アプリケーションを実行していると、製品への移動は、顧客レビューなどの製品情報を示します。</span><span class="sxs-lookup"><span data-stu-id="79d09-145">Running our application now and navigating to a product shows the product information including customer reviews.</span></span>

![](tailspin-spyworks-part-7/_static/image3.jpg)

## <a id="_Toc260221678"></a>  <span data-ttu-id="79d09-146">人気のある項目のコントロール (ユーザー コントロールの作成)</span><span class="sxs-lookup"><span data-stu-id="79d09-146">Popular Items Control (Creating User Controls)</span></span>

<span data-ttu-id="79d09-147">Web サイトの売上を増やすためには、「挑発的な販売」人気のある、または関連する製品に、いくつかの機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="79d09-147">In order to increase sales on your web site we will add a couple of features to "suggestive sell" popular or related products.</span></span>

<span data-ttu-id="79d09-148">これらの機能の最初の製品カタログでよく使用される製品の一覧表示されます。</span><span class="sxs-lookup"><span data-stu-id="79d09-148">The first of these features will be a list of the more popular product in our product catalog.</span></span>

<span data-ttu-id="79d09-149">好調、アプリケーションのホーム ページ上の項目を表示する「ユーザー コントロール」を作成します。</span><span class="sxs-lookup"><span data-stu-id="79d09-149">We will create a "User Control" to display the top selling items on the home page of our application.</span></span> <span data-ttu-id="79d09-150">これはなるため、コントロールは、単にドラッグすると同様に任意のページ上に Visual Studio のデザイナーでコントロールを削除する任意のページに使用したことができます。</span><span class="sxs-lookup"><span data-stu-id="79d09-150">Since this will be a control, we can use it on any page by simply dragging and dropping the control in Visual Studio's designer onto any page that we like.</span></span>

<span data-ttu-id="79d09-151">Visual Studio のソリューション エクスプ ローラーで、ソリューション名を右クリックし、「コントロール」という名前の新しいディレクトリを作成します。</span><span class="sxs-lookup"><span data-stu-id="79d09-151">In Visual Studio's solutions explorer, right-click on the solution name and create a new directory named "Controls".</span></span> <span data-ttu-id="79d09-152">そのためには必要ありませんが、お手伝い「コントロール」ディレクトリすべてのユーザー コントロールを作成して整理されたプロジェクトを保持します。</span><span class="sxs-lookup"><span data-stu-id="79d09-152">While it is not necessary to do so, we will help keep our project organized by creating all our user controls in the "Controls" directory.</span></span>

<span data-ttu-id="79d09-153">コントロールのフォルダーを右クリックし、「新しい項目の」を選択します。</span><span class="sxs-lookup"><span data-stu-id="79d09-153">Right-click on the controls folder and choose "New Item" :</span></span>

![](tailspin-spyworks-part-7/_static/image4.jpg)

<span data-ttu-id="79d09-154">"PopularItems"のユーザーのコントロールの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="79d09-154">Specify a name for our control of "PopularItems".</span></span> <span data-ttu-id="79d09-155">ユーザー コントロールのファイル拡張子が .ascx .aspx できませんに注意してください。</span><span class="sxs-lookup"><span data-stu-id="79d09-155">Note that the file extension for user controls is .ascx not .aspx.</span></span>

<span data-ttu-id="79d09-156">ユーザーの一般的なアイテムのユーザー コントロールは、次のように定義されます。</span><span class="sxs-lookup"><span data-stu-id="79d09-156">Our Popular Items User control will be defined as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample10.aspx)]

<span data-ttu-id="79d09-157">ここでこのアプリケーションでまだお使用いないメソッドを使用しているおです。</span><span class="sxs-lookup"><span data-stu-id="79d09-157">Here we're using a method we have not used yet in this application.</span></span> <span data-ttu-id="79d09-158">リピータ コントロールを使用して、データ ソース コントロールを使用する代わりに、LINQ to Entities クエリの結果を Repeater コントロールをバインドしているおです。</span><span class="sxs-lookup"><span data-stu-id="79d09-158">We're using the repeater control and instead of using a data source control we're binding the Repeater Control to the results of a LINQ to Entities query.</span></span>

<span data-ttu-id="79d09-159">ユーザーのコントロールの分離コードでそうには、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="79d09-159">In the code behind of our control we do that as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample11.cs)]

<span data-ttu-id="79d09-160">ユーザーのコントロールのマークアップの上部にある重要な行を注意してください。</span><span class="sxs-lookup"><span data-stu-id="79d09-160">Note also this important line at the top of our control's markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample12.aspx)]

<span data-ttu-id="79d09-161">分を分単位で最も人気のある項目を変更しないため、アプリケーションのパフォーマンスを向上させるために出るディレクティブを追加できます。</span><span class="sxs-lookup"><span data-stu-id="79d09-161">Since the most popular items won't be changing on a minute to minute basis we can add a aching directive to improve the performance of our application.</span></span> <span data-ttu-id="79d09-162">このディレクティブは、コントロールのキャッシュされた出力の有効期限が切れるときにのみ実行するコントロールのコードになります。</span><span class="sxs-lookup"><span data-stu-id="79d09-162">This directive will cause the controls code to only be executed when the cached output of the control expires.</span></span> <span data-ttu-id="79d09-163">それ以外の場合は、コントロールの出力のキャッシュされたバージョンが使用されます。</span><span class="sxs-lookup"><span data-stu-id="79d09-163">Otherwise, the cached version of the control's output will be used.</span></span>

<span data-ttu-id="79d09-164">やることを行うには、Default.aspc ページで、新しいコントロールが含まれます。</span><span class="sxs-lookup"><span data-stu-id="79d09-164">Now all we have to do is include our new control in our Default.aspc page.</span></span>

<span data-ttu-id="79d09-165">ドラッグ アンド ドロップで、既定のフォームの開いている列に、コントロールのインスタンスを配置します。</span><span class="sxs-lookup"><span data-stu-id="79d09-165">Use drag and drop to place an instance of the control in the open column of our Default form.</span></span>

![](tailspin-spyworks-part-7/_static/image5.jpg)

<span data-ttu-id="79d09-166">今すぐ実行するとき、アプリケーションのホーム ページには、最も人気のある項目が表示されます。</span><span class="sxs-lookup"><span data-stu-id="79d09-166">Now when we run our application the home page displays the most popular items.</span></span>

![](tailspin-spyworks-part-7/_static/image6.jpg)

## <a id="_Toc260221679"></a>  <span data-ttu-id="79d09-167">(パラメーターを使用してユーザー コントロール) を管理「も購入」</span><span class="sxs-lookup"><span data-stu-id="79d09-167">"Also Purchased" Control (User Controls with Parameters)</span></span>

<span data-ttu-id="79d09-168">コンテキストの特異性を追加することで、次のレベルを販売して推奨を作成した 2 つ目のユーザー コントロールになります。</span><span class="sxs-lookup"><span data-stu-id="79d09-168">The second User Control that we'll create will take suggestive selling to the next level by adding context specificity.</span></span>

<span data-ttu-id="79d09-169">上位の「購入も」アイテムを計算するためのロジックは、非単純です。</span><span class="sxs-lookup"><span data-stu-id="79d09-169">The logic to calculate the top "Also Purchased" items is non-trivial.</span></span>

<span data-ttu-id="79d09-170">「購入も」コントロールは、現在選択されている ProductID の (以前の購入) OrderDetails レコードを選択しが見つかった一意の注文ごと order Id を取得します。</span><span class="sxs-lookup"><span data-stu-id="79d09-170">Our "Also Purchased" control will select the OrderDetails records (previously purchased) for the currently selected ProductID and grab the OrderIDs for each unique order that is found.</span></span>

<span data-ttu-id="79d09-171">おは al から製品を選択これらすべての注文と合計数量を購入します。</span><span class="sxs-lookup"><span data-stu-id="79d09-171">Then we will select al the products from all those Orders and sum the quantities purchased.</span></span> <span data-ttu-id="79d09-172">製品を数量合計に並べ替えるし、上位 5 つのアイテムを表示おします。</span><span class="sxs-lookup"><span data-stu-id="79d09-172">We'll sort the products by that quantity sum and display the top five items.</span></span>

<span data-ttu-id="79d09-173">このロジックの複雑さを考えると、ストアド プロシージャとして、このアルゴリズムを実装します。</span><span class="sxs-lookup"><span data-stu-id="79d09-173">Given the complexity of this logic, we will implement this algorithm as a stored procedure.</span></span>

<span data-ttu-id="79d09-174">ストアド プロシージャの T-SQL では次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="79d09-174">The T-SQL for the stored procedure is as follows.</span></span>

[!code-sql[Main](tailspin-spyworks-part-7/samples/sample13.sql)]

<span data-ttu-id="79d09-175">このストアド プロシージャ (SelectPurchasedWithProducts) 存在していたこと、データベースには含まれている場合、アプリケーションを指定したテーブルと、必要な Entity Data Model ビューだけでなく、Entity Data Model を生成したときに注意してください。このストアド プロシージャを含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="79d09-175">Note that this stored procedure (SelectPurchasedWithProducts) existed in the database when we included it in our application and when we generated the Entity Data Model we specified that, in addition to the Tables and Views that we needed, the Entity Data Model should include this stored procedure.</span></span>

<span data-ttu-id="79d09-176">エンティティ データ モデルからストアド プロシージャにアクセスするには、関数をインポートする必要があります。</span><span class="sxs-lookup"><span data-stu-id="79d09-176">To access the stored procedure from the Entity Data Model we need to import the function.</span></span>

<span data-ttu-id="79d09-177">Entity Data Model デザイナーで開くし、モデル ブラウザーを開き、ソリューション エクスプ ローラーでをダブルクリックし、デザイナー内を右クリックして「関数インポートの追加」を選択します。</span><span class="sxs-lookup"><span data-stu-id="79d09-177">Double Click on the Entity Data Model in the Solutions Explorer to open it in the designer and open the Model Browser, then right-click in the designer and select "Add Function Import".</span></span>

![](tailspin-spyworks-part-7/_static/image1.png)

<span data-ttu-id="79d09-178">そうと、このダイアログ ボックスが開きます。</span><span class="sxs-lookup"><span data-stu-id="79d09-178">Doing so will open this dialog.</span></span>

![](tailspin-spyworks-part-7/_static/image2.png)

<span data-ttu-id="79d09-179">"SelectPurchasedWithProducts"を選択すると、表示されるフィールドに入力し、インポートされた関数の名前のプロシージャ名を使用します。</span><span class="sxs-lookup"><span data-stu-id="79d09-179">Fill out the fields as you see above, selecting the "SelectPurchasedWithProducts" and use the procedure name for the name of our imported function.</span></span>

<span data-ttu-id="79d09-180">"Ok"をクリックします。</span><span class="sxs-lookup"><span data-stu-id="79d09-180">Click "Ok".</span></span>

<span data-ttu-id="79d09-181">おが単に、プログラム、ストアド プロシージャかもしれません、モデル内の他のアイテムとしてこの手順を完了します。</span><span class="sxs-lookup"><span data-stu-id="79d09-181">Having done this we can simply program against the stored procedure as we might any other item in the model.</span></span>

<span data-ttu-id="79d09-182">そのため、「コントロール」フォルダーでは、AlsoPurchased.ascx をという名前の新しいユーザー コントロールを作成します。</span><span class="sxs-lookup"><span data-stu-id="79d09-182">So, in our "Controls" folder create a new user control named AlsoPurchased.ascx.</span></span>

<span data-ttu-id="79d09-183">このコントロールのマークアップは PopularItems コントロールに非常に使いやすくなります。</span><span class="sxs-lookup"><span data-stu-id="79d09-183">The markup for this control will look very familiar to the PopularItems control.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample14.aspx)]

<span data-ttu-id="79d09-184">重要な違いは、出力キャッシュしないことと、項目の表示するのには製品でによって異なるためです。</span><span class="sxs-lookup"><span data-stu-id="79d09-184">The notable difference is that are not caching the output since the item's to be rendered will differ by product.</span></span>

<span data-ttu-id="79d09-185">ProductId をコントロールに"property"となります。</span><span class="sxs-lookup"><span data-stu-id="79d09-185">The ProductId will be a "property" to the control.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample15.cs)]

<span data-ttu-id="79d09-186">コントロールの PreRender イベント ハンドラーでは 3 つの作業を行う eed です。</span><span class="sxs-lookup"><span data-stu-id="79d09-186">In the control's PreRender event handler we eed to do three things.</span></span>

1. <span data-ttu-id="79d09-187">ProductID が設定されていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="79d09-187">Make sure the ProductID is set.</span></span>
2. <span data-ttu-id="79d09-188">すべての製品購入された現在のものがあるかどうかに参照してください。</span><span class="sxs-lookup"><span data-stu-id="79d09-188">See if there are any products that have been purchased with the current one.</span></span>
3. <span data-ttu-id="79d09-189">いくつかのアイテム 2 で決まるを出力します。</span><span class="sxs-lookup"><span data-stu-id="79d09-189">Output some items as determined in #2.</span></span>

<span data-ttu-id="79d09-190">簡単に方法をモデルからストアド プロシージャを呼び出すことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="79d09-190">Note how easy it is to call the stored procedure through the model.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample16.cs)]

<span data-ttu-id="79d09-191">あります「も購入される」を決定した後だけでリピータ クエリによって返される結果にバインドできます。</span><span class="sxs-lookup"><span data-stu-id="79d09-191">After determining that there ARE "also purchased" we can simply bind the repeater to the results returned by the query.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample17.cs)]

<span data-ttu-id="79d09-192">「購入も」項目がない場合だけ表示人気のあるその他の項目、カタログから。</span><span class="sxs-lookup"><span data-stu-id="79d09-192">If there were not any "also purchased" items we'll simply display other popular items from our catalog.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample18.cs)]

<span data-ttu-id="79d09-193">「購入も」アイテムを表示するには、ProductDetails.aspx ページを開き、マークアップ内のこの位置に表示されるように、ソリューション エクスプ ローラーから AlsoPurchased コントロールをドラッグします。</span><span class="sxs-lookup"><span data-stu-id="79d09-193">To view the "Also Purchased" items, open the ProductDetails.aspx page and drag the AlsoPurchased control from the Solutions Explorer so that it appears in this position in the markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample19.aspx)]

<span data-ttu-id="79d09-194">これにより、ProductDetails ページの上部にあるコントロールへの参照が作成されます。</span><span class="sxs-lookup"><span data-stu-id="79d09-194">Doing so will create a reference to the control at the top of the ProductDetails page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample20.aspx)]

<span data-ttu-id="79d09-195">AlsoPurchased ユーザー コントロールには、ProductId 番号が必要です。 ため ProductID プロパティ、コントロールのページの現在のデータ モデルのアイテムに対して Eval のステートメントを使用して設定されます。</span><span class="sxs-lookup"><span data-stu-id="79d09-195">Since the AlsoPurchased user control requires a ProductId number we will set the ProductID property of our control by using an Eval statement against the current data model item of the page.</span></span>

![](tailspin-spyworks-part-7/_static/image3.png)

<span data-ttu-id="79d09-196">今すぐ実行し、製品を参照してビルドときに、「購入も」アイテムを確認します。</span><span class="sxs-lookup"><span data-stu-id="79d09-196">When we build and run now and browse to a product we see the "Also Purchased" items.</span></span>

![](tailspin-spyworks-part-7/_static/image7.jpg)

> [!div class="step-by-step"]
> <span data-ttu-id="79d09-197">[前へ](tailspin-spyworks-part-6.md)
> [次へ](tailspin-spyworks-part-8.md)</span><span class="sxs-lookup"><span data-stu-id="79d09-197">[Previous](tailspin-spyworks-part-6.md)
[Next](tailspin-spyworks-part-8.md)</span></span>
