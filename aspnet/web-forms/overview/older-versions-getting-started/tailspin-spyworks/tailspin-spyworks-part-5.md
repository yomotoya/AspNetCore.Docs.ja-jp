---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
title: '手順 5: ビジネス ロジック |Microsoft ドキュメント'
author: JoeStagner
description: このチュートリアルの系列では、すべて Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。 第 5 部では、いくつかのビジネス ロジックを追加します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: eaef475a-ca91-47ea-a4a7-d074005ed80c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
msc.type: authoredcontent
ms.openlocfilehash: e4342e634ef8c4bcf4e0085650a28f414ab23736
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30885082"
---
<a name="part-5-business-logic"></a><span data-ttu-id="09f9d-104">手順 5: ビジネス ロジック</span><span class="sxs-lookup"><span data-stu-id="09f9d-104">Part 5: Business Logic</span></span>
====================
<span data-ttu-id="09f9d-105">によって[行える](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="09f9d-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="09f9d-106">Tailspin Spyworks では、.NET プラットフォーム用の強力な拡張性の高いアプリケーションを作成するはどのように非常に単純なを示しています。</span><span class="sxs-lookup"><span data-stu-id="09f9d-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="09f9d-107">ASP.NET 4 の優れた新機能を使用して、ショッピング、チェック アウト、および管理を含む、オンライン ストアをビルドする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="09f9d-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="09f9d-108">このチュートリアルの系列では、すべて Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="09f9d-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="09f9d-109">第 5 部では、いくつかのビジネス ロジックを追加します。</span><span class="sxs-lookup"><span data-stu-id="09f9d-109">Part 5 adds some business logic.</span></span>


## <a id="_Toc260221671"></a>  <span data-ttu-id="09f9d-110">一部のビジネス ロジックを追加します。</span><span class="sxs-lookup"><span data-stu-id="09f9d-110">Adding Some Business Logic</span></span>

<span data-ttu-id="09f9d-111">当社の web サイトにアクセスするたびに使用できる、ショッピングします。</span><span class="sxs-lookup"><span data-stu-id="09f9d-111">We want our shopping experience to be available whenever someone visits our web site.</span></span> <span data-ttu-id="09f9d-112">閲覧者を参照して、登録またはログインしていない場合でも、ショッピング カートにアイテムを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="09f9d-112">Visitors will be able to browse and add items to the shopping cart even if they are not registered or logged in.</span></span> <span data-ttu-id="09f9d-113">チェック アウトする準備ができたらが与えられます認証のオプションをしない場合はまだメンバーができるアカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="09f9d-113">When they are ready to check out they will be given the option to authenticate and if they are not yet members they will be able to create an account.</span></span>

<span data-ttu-id="09f9d-114">これは、匿名の状態から「ユーザーの登録済み」状態に、ショッピング カートを変換するロジックを実装する必要がありますが、ことを意味します。</span><span class="sxs-lookup"><span data-stu-id="09f9d-114">This means that we will need to implement the logic to convert the shopping cart from an anonymous state to a "Registered User" state.</span></span>

<span data-ttu-id="09f9d-115">みましょう「クラス」という名前のディレクトリを作成、フォルダーを右クリックし、MyShoppingCart.cs をという名前の新しい"Class"ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="09f9d-115">Let's create a directory named "Classes" then Right-Click on the folder and create a new "Class" file named MyShoppingCart.cs</span></span>

![](tailspin-spyworks-part-5/_static/image1.jpg)

![](tailspin-spyworks-part-5/_static/image1.png)

<span data-ttu-id="09f9d-116">既に触れましたが MyShoppingCart.aspx ページを実装するクラスを拡張して、私たちはこれを使用して実行します。NET の強力な「部分的なクラス」を構築します。</span><span class="sxs-lookup"><span data-stu-id="09f9d-116">As previously mentioned we will be extending the class that implements the MyShoppingCart.aspx page and we will do this using .NET's powerful "Partial Class" construct.</span></span>

<span data-ttu-id="09f9d-117">次のような MyShoppingCart.aspx.cf ファイルの生成された呼び出しが表示されます。</span><span class="sxs-lookup"><span data-stu-id="09f9d-117">The generated call for our MyShoppingCart.aspx.cf file looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample1.cs)]

<span data-ttu-id="09f9d-118">"Partial"キーワードの使用に注意してください。</span><span class="sxs-lookup"><span data-stu-id="09f9d-118">Note the use of the "partial" keyword.</span></span>

<span data-ttu-id="09f9d-119">生成したクラス ファイルは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="09f9d-119">The class file that we just generated looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample2.cs)]

<span data-ttu-id="09f9d-120">このファイルにも、部分的なキーワードを追加することで実装がマージされます。</span><span class="sxs-lookup"><span data-stu-id="09f9d-120">We will merge our implementations by adding the partial keyword to this file as well.</span></span>

<span data-ttu-id="09f9d-121">新しいクラス ファイルは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="09f9d-121">Our new class file now looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample3.cs)]

<span data-ttu-id="09f9d-122">このクラスに追加する最初のメソッドは、"AddItem"メソッドです。</span><span class="sxs-lookup"><span data-stu-id="09f9d-122">The first method that we will add to our class is the "AddItem" method.</span></span> <span data-ttu-id="09f9d-123">これは、ユーザーが、製品の一覧と製品の詳細ページに「アートを追加」へのリンクをクリックしたときに最終的に呼び出されるメソッドです。</span><span class="sxs-lookup"><span data-stu-id="09f9d-123">This is the method that will ultimately be called when the user clicks on the "Add to Art" links on the Product List and Product Details pages.</span></span>

<span data-ttu-id="09f9d-124">使用して、、次を追加、ページの上部にあるステートメントです。</span><span class="sxs-lookup"><span data-stu-id="09f9d-124">Append the following to the using statements at the top of the page.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample4.cs)]

<span data-ttu-id="09f9d-125">MyShoppingCart クラスにこのメソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="09f9d-125">And add this method to the MyShoppingCart class.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample5.cs)]

<span data-ttu-id="09f9d-126">かどうか、項目は既に、カートを表示するのに LINQ to Entities を使用します。</span><span class="sxs-lookup"><span data-stu-id="09f9d-126">We are using LINQ to Entities to see if the item is already in the cart.</span></span> <span data-ttu-id="09f9d-127">場合は、アイテムの注文数量を更新、それ以外の場合新しいエントリを作成する、選んだ項目の</span><span class="sxs-lookup"><span data-stu-id="09f9d-127">If so, we update the order quantity of the item, otherwise we create a new entry for the selected item</span></span>

<span data-ttu-id="09f9d-128">このメソッドを呼び出すためには、このメソッドをクラスだけでなく、現在の項目が追加された後に = カートを買い物を表示するための AddToCart.aspx ページを実装します。</span><span class="sxs-lookup"><span data-stu-id="09f9d-128">In order to call this method we will implement an AddToCart.aspx page that not only class this method but then displayed the current shopping a=cart after the item has been added.</span></span>

<span data-ttu-id="09f9d-129">ソリューション エクスプ ローラーでソリューション名を右クリックし、追加し、新しいページを既に行って AddToCart.aspx という名前です。</span><span class="sxs-lookup"><span data-stu-id="09f9d-129">Right-Click on the solution name in the solution explorer and add and new page named AddToCart.aspx as we have done previously.</span></span>

<span data-ttu-id="09f9d-130">実装と低のストック問題などの中間結果を表示おこのページを使用できますが、ページが実際には、レンダリングしますが、ではなく"Add"ロジックを呼び出すし、リダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="09f9d-130">While we could use this page to display interim results like low stock issues, etc, in our implementation, the page will not actually render, but rather call the "Add" logic and redirect.</span></span>

<span data-ttu-id="09f9d-131">これを実現する、ページに、次のコードを追加しますお\_Load イベント。</span><span class="sxs-lookup"><span data-stu-id="09f9d-131">To accomplish this we'll add the following code to the Page\_Load event.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample6.cs)]

<span data-ttu-id="09f9d-132">QueryString パラメーターと、クラスの AddItem メソッドを呼び出してから、ショッピング カートに追加する製品を取得することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="09f9d-132">Note that we are retrieving the product to add to the shopping cart from a QueryString parameter and calling the AddItem method of our class.</span></span>

<span data-ttu-id="09f9d-133">発生したエラーがないと想定し、完全を実装します。 次に後ページに制御が渡されます。</span><span class="sxs-lookup"><span data-stu-id="09f9d-133">Assuming no errors are encountered control is passed to the SHoppingCart.aspx page which we will fully implement next.</span></span> <span data-ttu-id="09f9d-134">必要がありますエラーが発生する場合、例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="09f9d-134">If there should be an error we throw an exception.</span></span>

<span data-ttu-id="09f9d-135">現在を実装していないグローバル エラー ハンドラーのため、この例外は、アプリケーションで処理されないが、私たちはこの問題を解決直後。</span><span class="sxs-lookup"><span data-stu-id="09f9d-135">Currently we have not yet implemented a global error handler so this exception would go unhandled by our application but we will remedy this shortly.</span></span>

<span data-ttu-id="09f9d-136">Debug.Fail() (を介して使用できるステートメントを使用しても注意してください。 `using System.Diagnostics;)`</span><span class="sxs-lookup"><span data-stu-id="09f9d-136">Note also the use of the statement Debug.Fail() (available via `using System.Diagnostics;)`</span></span>

<span data-ttu-id="09f9d-137">デバッガー内のアプリケーションが実行されている、このメソッドを指定するエラー メッセージと共にアプリケーションの状態に関する情報を含む詳細なダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="09f9d-137">Is the application is running inside the debugger, this method will display a detailed dialog with information about the applications state along with the error message that we specify.</span></span>

<span data-ttu-id="09f9d-138">Debug.Fail() ステートメントを実稼働環境で実行されている場合は無視されます。</span><span class="sxs-lookup"><span data-stu-id="09f9d-138">When running in production the Debug.Fail() statement is ignored.</span></span>

<span data-ttu-id="09f9d-139">ショッピング カート クラス名"GetShoppingCartId"のメソッドを呼び出す前のコードで注意してください。</span><span class="sxs-lookup"><span data-stu-id="09f9d-139">You will note in the code above a call to a method in our shopping cart class names "GetShoppingCartId".</span></span>

<span data-ttu-id="09f9d-140">次のように、メソッドを実装するコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="09f9d-140">Add the code to implement the method as follows.</span></span>

<span data-ttu-id="09f9d-141">更新プログラムとチェック アウトのボタンと、カート「合計」を表示おラベルも追加されたことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="09f9d-141">Note that we've also added update and checkout buttons and a label where we can display the cart "total".</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample7.cs)]

<span data-ttu-id="09f9d-142">このショッピング カートにアイテムを追加できますようになりましたが、製品を追加した後、カートを表示するためのロジックを実装していないこと。</span><span class="sxs-lookup"><span data-stu-id="09f9d-142">We can now add items to our shopping cart but we have not implemented the logic to display the cart after a product has been added.</span></span>

<span data-ttu-id="09f9d-143">そのため、MyShoppingCart.aspx ページで追加 EntityDataSource コントロールと GridVire コントロールとおりです。</span><span class="sxs-lookup"><span data-stu-id="09f9d-143">So, in the MyShoppingCart.aspx page we'll add an EntityDataSource control and a GridVire control as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample8.aspx)]

<span data-ttu-id="09f9d-144">カートの更新ボタンをダブルクリックしてしたり、マークアップ内の宣言で指定されている click イベント ハンドラーを生成できるように、デザイナーでフォームを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="09f9d-144">Call up the form in the designer so that you can double click on the Update Cart button and generate the click event handler that is specified in the declaration in the markup.</span></span>

<span data-ttu-id="09f9d-145">後で詳細を実装しますが、これはお知らせアプリケーション ビルドし実行、エラーが発生しません。</span><span class="sxs-lookup"><span data-stu-id="09f9d-145">We'll implement the details later but doing this will let us build and run our application without errors.</span></span>

<span data-ttu-id="09f9d-146">アプリケーションを実行し、ショッピング カートにアイテムを追加するときにこれが表示されます。</span><span class="sxs-lookup"><span data-stu-id="09f9d-146">When you run the application and add an item to the shopping cart you will see this.</span></span>

![](tailspin-spyworks-part-5/_static/image2.jpg)

<span data-ttu-id="09f9d-147">次の 3 つのカスタム列を実装することで、"default"のグリッド表示から傾いたりおに注意してください。</span><span class="sxs-lookup"><span data-stu-id="09f9d-147">Note that we have deviated from the "default" grid display by implementing three custom columns.</span></span>

<span data-ttu-id="09f9d-148">1 つは、編集可能、数量の「バインド」フィールドには。</span><span class="sxs-lookup"><span data-stu-id="09f9d-148">The first is an Editable, "Bound" field for the Quantity:</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample9.aspx)]

<span data-ttu-id="09f9d-149">"次へ計算された"列、行アイテムの合計 (を順序付ける数量回アイテム コスト) を表示します。</span><span class="sxs-lookup"><span data-stu-id="09f9d-149">The next is a "calculated" column that displays the line item total (the item cost times the quantity to be ordered):</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample10.aspx)]

<span data-ttu-id="09f9d-150">最後に、ユーザーがショッピング グラフから項目を削除する必要があることを示すために使用する チェック ボックス コントロールを含むカスタムの列があります。</span><span class="sxs-lookup"><span data-stu-id="09f9d-150">Lastly we have a custom column that contains a CheckBox control that the user will use to indicate that the item should be removed from the shopping chart.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample11.aspx)]

![](tailspin-spyworks-part-5/_static/image3.jpg)

<span data-ttu-id="09f9d-151">ご覧のように、合計行がでは空の順序は注文合計を計算するいくつかのロジックを追加します。</span><span class="sxs-lookup"><span data-stu-id="09f9d-151">As you can see, the Order Total line is empty so let's add some logic to calculate the Order Total.</span></span>

<span data-ttu-id="09f9d-152">"GetTotal"メソッド、MyShoppingCart クラスにまずを実装します。</span><span class="sxs-lookup"><span data-stu-id="09f9d-152">We'll first implement a "GetTotal" method to our MyShoppingCart Class.</span></span>

<span data-ttu-id="09f9d-153">MyShoppingCart.cs ファイルでは、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="09f9d-153">In the MyShoppingCart.cs file add the following code.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample12.cs)]

<span data-ttu-id="09f9d-154">その後、ページ \_Load イベント ハンドラーが、GetTotal メソッドを呼び出してことがあります。</span><span class="sxs-lookup"><span data-stu-id="09f9d-154">Then in the Page\_Load event handler we'll can call our GetTotal method.</span></span> <span data-ttu-id="09f9d-155">同時に、ショッピング カートが空かどうかである場合は、必要に応じて表示を調整してテストを追加します。</span><span class="sxs-lookup"><span data-stu-id="09f9d-155">At the same time we'll add a test to see if the shopping cart is empty and adjust the display accordingly if it is.</span></span>

<span data-ttu-id="09f9d-156">これで、ショッピング カートの内容が空の場合これが表示されます。</span><span class="sxs-lookup"><span data-stu-id="09f9d-156">Now if the shopping cart is empty we get this:</span></span>

![](tailspin-spyworks-part-5/_static/image4.jpg)

<span data-ttu-id="09f9d-157">合計が表示されていない場合。</span><span class="sxs-lookup"><span data-stu-id="09f9d-157">And if not, we see our total.</span></span>

![](tailspin-spyworks-part-5/_static/image5.jpg)

<span data-ttu-id="09f9d-158">ただし、このページはまだ完了しません。</span><span class="sxs-lookup"><span data-stu-id="09f9d-158">However, this page is not yet complete.</span></span>

<span data-ttu-id="09f9d-159">削除のマークされた項目を削除して、一部が変更された可能性がグリッドに、ユーザーが、新しい数量の値を決定することにより、ショッピング カートを再計算する追加のロジックが必要ですが。</span><span class="sxs-lookup"><span data-stu-id="09f9d-159">We will need additional logic to recalculate the shopping cart by removing items marked for removal and by determining new quantity values as some may have been changed in the grid by the user.</span></span>

<span data-ttu-id="09f9d-160">ユーザーがアイテムの削除をマークするときに、ケースを処理する MyShoppingCart.cs に、ショッピング カート クラスに"RemoveItem"メソッドを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="09f9d-160">Lets add a "RemoveItem" method to our shopping cart class in MyShoppingCart.cs to handle the case when a user marks an item for removal.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample13.cs)]

<span data-ttu-id="09f9d-161">今すぐみましょう ad ユーザーが GridView で注文する品質を単に変更するときに、状況を処理するメソッド。</span><span class="sxs-lookup"><span data-stu-id="09f9d-161">Now let's ad a method to handle the circumstance when a user simply changes the quality to be ordered in the GridView.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample14.cs)]

<span data-ttu-id="09f9d-162">インプレースの削除と更新プログラムの基本的な機能では、実際には、データベースに、ショッピング カートを更新するロジックを実装おことができます。</span><span class="sxs-lookup"><span data-stu-id="09f9d-162">With the basic Remove and Update features in place we can implement the logic that actually updates the shopping cart in the database.</span></span> <span data-ttu-id="09f9d-163">(で MyShoppingCart.cs)</span><span class="sxs-lookup"><span data-stu-id="09f9d-163">(In MyShoppingCart.cs)</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample15.cs)]

<span data-ttu-id="09f9d-164">このメソッドが 2 つのパラメーターを期待していることに注意します。</span><span class="sxs-lookup"><span data-stu-id="09f9d-164">You'll note that this method expects two parameters.</span></span> <span data-ttu-id="09f9d-165">1 は、ショッピング カート Id で、もう 1 つはユーザー定義型のオブジェクトの配列です。</span><span class="sxs-lookup"><span data-stu-id="09f9d-165">One is the shopping cart Id and the other is an array of objects of user defined type.</span></span>

<span data-ttu-id="09f9d-166">ユーザー インターフェイスについて、ロジックの依存関係を最小限にとどめる GridView コントロールに直接アクセスする必要がある、メソッドを使用しないで、ショッピング カートのアイテムをコードに渡すに使用できるデータ構造を定義したのでです。</span><span class="sxs-lookup"><span data-stu-id="09f9d-166">So as to minimize the dependency of our logic on user interface specifics, we've defined a data structure that we can use to pass the shopping cart items to our code without our method needing to directly access the GridView control.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample16.cs)]

<span data-ttu-id="09f9d-167">MyShoppingCart.aspx.cs ファイルで使用できますこの構造体、更新ボタンの Click イベント ハンドラーでとおり。</span><span class="sxs-lookup"><span data-stu-id="09f9d-167">In our MyShoppingCart.aspx.cs file we can use this structure in our Update Button Click Event handler as follows.</span></span> <span data-ttu-id="09f9d-168">に加えて、買い物カゴの更新を更新してカートの合計金額にも注意してください。</span><span class="sxs-lookup"><span data-stu-id="09f9d-168">Note that in addition to updating the cart we will update the cart total as well.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample17.cs)]

<span data-ttu-id="09f9d-169">次のコード行の特定の目的に注意してください。</span><span class="sxs-lookup"><span data-stu-id="09f9d-169">Note with particular interest this line of code:</span></span>

[!code-javascript[Main](tailspin-spyworks-part-5/samples/sample18.js)]

<span data-ttu-id="09f9d-170">GetValues() は MyShoppingCart.aspx.cs で次のように実装が特殊なヘルパー関数です。</span><span class="sxs-lookup"><span data-stu-id="09f9d-170">GetValues() is a special helper function that we will implement in MyShoppingCart.aspx.cs as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample19.cs)]

<span data-ttu-id="09f9d-171">これにより、クリーン、GridView コントロールのバインド要素の値にアクセスします。</span><span class="sxs-lookup"><span data-stu-id="09f9d-171">This provides a clean way to access the values of the bound elements in our GridView control.</span></span> <span data-ttu-id="09f9d-172">ユーザーの「アイテムの削除」チェック ボックス コントロールがバインドされていないために、FindControl() メソッドを使用してアクセスしてみましょう。</span><span class="sxs-lookup"><span data-stu-id="09f9d-172">Since our "Remove Item" CheckBox Control is not bound we'll access it via the FindControl() method.</span></span>

<span data-ttu-id="09f9d-173">プロジェクトの開発では、この段階では、お支払い処理を実装する準備がされています。</span><span class="sxs-lookup"><span data-stu-id="09f9d-173">At this stage in your project's development we are getting ready to implement the checkout process.</span></span>

<span data-ttu-id="09f9d-174">みましょうそう前に、Visual Studio を使用して、メンバーシップ データベースを生成し、ユーザーのメンバーシップのリポジトリを追加します。</span><span class="sxs-lookup"><span data-stu-id="09f9d-174">Before doing so let's use Visual Studio to generate the membership database and add a user to the membership repository.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="09f9d-175">[前へ](tailspin-spyworks-part-4.md)
> [次へ](tailspin-spyworks-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="09f9d-175">[Previous](tailspin-spyworks-part-4.md)
[Next](tailspin-spyworks-part-6.md)</span></span>
