---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
title: 'パート 5: ビジネス ロジック |Microsoft Docs'
author: JoeStagner
description: このチュートリアル シリーズでは、すべての Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。 パート 5 では、いくつかのビジネス ロジックを追加します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: eaef475a-ca91-47ea-a4a7-d074005ed80c
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
msc.type: authoredcontent
ms.openlocfilehash: 719d56e0764e2f66b8813c9487119bbc700d738c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377987"
---
<a name="part-5-business-logic"></a><span data-ttu-id="0e5e9-104">パート 5: ビジネス ロジック</span><span class="sxs-lookup"><span data-stu-id="0e5e9-104">Part 5: Business Logic</span></span>
====================
<span data-ttu-id="0e5e9-105">によって[Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="0e5e9-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="0e5e9-106">Tailspin Spyworks では、.NET プラットフォーム用の強力でスケーラブルなアプリケーションを作成するはどの非常に単純なを示します。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="0e5e9-107">ASP.NET 4 の優れた新機能を使用して、ショッピング、チェック アウト、および管理を含む、オンライン ストアを構築する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="0e5e9-108">このチュートリアル シリーズでは、すべての Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="0e5e9-109">パート 5 では、いくつかのビジネス ロジックを追加します。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-109">Part 5 adds some business logic.</span></span>


## <a id="_Toc260221671"></a>  <span data-ttu-id="0e5e9-110">一部のビジネス ロジックを追加します。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-110">Adding Some Business Logic</span></span>

<span data-ttu-id="0e5e9-111">経験ショッピング web サイトにアクセスするたびに使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-111">We want our shopping experience to be available whenever someone visits our web site.</span></span> <span data-ttu-id="0e5e9-112">訪問者を参照して、登録またはログインしていない場合でも、ショッピング カートにアイテムを追加することになります。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-112">Visitors will be able to browse and add items to the shopping cart even if they are not registered or logged in.</span></span> <span data-ttu-id="0e5e9-113">認証するオプションを指定するをチェック アウトの準備ができたら、それ以外の場合まだメンバーができるアカウントを作成する.</span><span class="sxs-lookup"><span data-stu-id="0e5e9-113">When they are ready to check out they will be given the option to authenticate and if they are not yet members they will be able to create an account.</span></span>

<span data-ttu-id="0e5e9-114">これは、ショッピング カートを匿名の状態から「ユーザーの登録済み」状態に変換するロジックを実装する必要ことを意味します。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-114">This means that we will need to implement the logic to convert the shopping cart from an anonymous state to a "Registered User" state.</span></span>

<span data-ttu-id="0e5e9-115">みましょう「クラス」という名前のディレクトリを作成し、フォルダーを右クリックし、MyShoppingCart.cs をという名前の新しい「クラス」ファイルの作成</span><span class="sxs-lookup"><span data-stu-id="0e5e9-115">Let's create a directory named "Classes" then Right-Click on the folder and create a new "Class" file named MyShoppingCart.cs</span></span>

![](tailspin-spyworks-part-5/_static/image1.jpg)

![](tailspin-spyworks-part-5/_static/image1.png)

<span data-ttu-id="0e5e9-116">前述したよう MyShoppingCart.aspx ページを実装するクラスを拡張しましたと私たちはこれを使用します。NET の強力な「部分クラス」のコンス トラクター。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-116">As previously mentioned we will be extending the class that implements the MyShoppingCart.aspx page and we will do this using .NET's powerful "Partial Class" construct.</span></span>

<span data-ttu-id="0e5e9-117">MyShoppingCart.aspx.cf ファイルの生成された呼び出しは次のようです。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-117">The generated call for our MyShoppingCart.aspx.cf file looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample1.cs)]

<span data-ttu-id="0e5e9-118">"Partial"キーワードの使用に注意してください。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-118">Note the use of the "partial" keyword.</span></span>

<span data-ttu-id="0e5e9-119">生成したばかりのクラス ファイルは、次のように検索します。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-119">The class file that we just generated looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample2.cs)]

<span data-ttu-id="0e5e9-120">実装も、このファイルに部分的なキーワードを追加することでマージされます。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-120">We will merge our implementations by adding the partial keyword to this file as well.</span></span>

<span data-ttu-id="0e5e9-121">新しいクラス ファイルは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-121">Our new class file now looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample3.cs)]

<span data-ttu-id="0e5e9-122">クラスに追加する最初のメソッドは、"AddItem"方法です。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-122">The first method that we will add to our class is the "AddItem" method.</span></span> <span data-ttu-id="0e5e9-123">これは、ユーザーが、製品リストと製品の詳細ページに「アートを追加」リンクをクリックすると最終的に呼び出されるメソッド。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-123">This is the method that will ultimately be called when the user clicks on the "Add to Art" links on the Product List and Product Details pages.</span></span>

<span data-ttu-id="0e5e9-124">使用して、次を追加、ページの上部にあるステートメント。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-124">Append the following to the using statements at the top of the page.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample4.cs)]

<span data-ttu-id="0e5e9-125">MyShoppingCart クラスにこのメソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-125">And add this method to the MyShoppingCart class.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample5.cs)]

<span data-ttu-id="0e5e9-126">カートにアイテムが既にかどうかは LINQ to Entities を使用しています。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-126">We are using LINQ to Entities to see if the item is already in the cart.</span></span> <span data-ttu-id="0e5e9-127">それ以外の場合、選択した項目の新しいエントリを作る場合、そのため、アイテムの注文数量を更新、</span><span class="sxs-lookup"><span data-stu-id="0e5e9-127">If so, we update the order quantity of the item, otherwise we create a new entry for the selected item</span></span>

<span data-ttu-id="0e5e9-128">このメソッドを呼び出すためには、このメソッドをクラスだけでなく、現在の項目が追加された後に = カートをショッピングを表示する AddToCart.aspx ページを実装します。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-128">In order to call this method we will implement an AddToCart.aspx page that not only class this method but then displayed the current shopping a=cart after the item has been added.</span></span>

<span data-ttu-id="0e5e9-129">ソリューション エクスプ ローラーでソリューション名を右クリックし、追加し、新しいページが以前に行ったように、AddToCart.aspx をという名前です。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-129">Right-Click on the solution name in the solution explorer and add and new page named AddToCart.aspx as we have done previously.</span></span>

<span data-ttu-id="0e5e9-130">実装に在庫不足の問題などのように中間結果を表示このページを使用できますが、ページが実際には、レンダリングしますが、ではなく、"Add"ロジックを呼び出すし、リダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-130">While we could use this page to display interim results like low stock issues, etc, in our implementation, the page will not actually render, but rather call the "Add" logic and redirect.</span></span>

<span data-ttu-id="0e5e9-131">これを実現する、ページに、次のコードを追加します\_Load イベント。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-131">To accomplish this we'll add the following code to the Page\_Load event.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample6.cs)]

<span data-ttu-id="0e5e9-132">クエリ文字列パラメーターと、クラスの AddItem メソッドを呼び出してから、ショッピング カートに追加する製品取得することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-132">Note that we are retrieving the product to add to the shopping cart from a QueryString parameter and calling the AddItem method of our class.</span></span>

<span data-ttu-id="0e5e9-133">発生したエラーは想定されません後ページを次に実装は完全に制御が渡されます。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-133">Assuming no errors are encountered control is passed to the SHoppingCart.aspx page which we will fully implement next.</span></span> <span data-ttu-id="0e5e9-134">エラーがある必要がある場合は、例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-134">If there should be an error we throw an exception.</span></span>

<span data-ttu-id="0e5e9-135">現在まだ実装していないグローバル エラー ハンドラーのため、この例外は、アプリケーションでハンドルされないがまもなく修正は。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-135">Currently we have not yet implemented a global error handler so this exception would go unhandled by our application but we will remedy this shortly.</span></span>

<span data-ttu-id="0e5e9-136">Debug.Fail() (を介して使用できるステートメントを使用しても注意してください。 `using System.Diagnostics;)`</span><span class="sxs-lookup"><span data-stu-id="0e5e9-136">Note also the use of the statement Debug.Fail() (available via `using System.Diagnostics;)`</span></span>

<span data-ttu-id="0e5e9-137">アプリケーションがデバッガー内部で実行されている、このメソッドを指定するエラー メッセージと共にアプリケーションの状態に関する情報を含む詳細なダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-137">Is the application is running inside the debugger, this method will display a detailed dialog with information about the applications state along with the error message that we specify.</span></span>

<span data-ttu-id="0e5e9-138">Debug.Fail() ステートメントを運用環境で実行されている場合は無視されます。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-138">When running in production the Debug.Fail() statement is ignored.</span></span>

<span data-ttu-id="0e5e9-139">上で、ショッピング カート クラス名"GetShoppingCartId"メソッドの呼び出しのコードことがわかります。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-139">You will note in the code above a call to a method in our shopping cart class names "GetShoppingCartId".</span></span>

<span data-ttu-id="0e5e9-140">次のように、メソッドを実装するコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-140">Add the code to implement the method as follows.</span></span>

<span data-ttu-id="0e5e9-141">更新プログラムとチェック アウトのボタンと「合計」カートの内容を表示しますラベルも追加したことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-141">Note that we've also added update and checkout buttons and a label where we can display the cart "total".</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample7.cs)]

<span data-ttu-id="0e5e9-142">このショッピング カートにアイテムを追加することができますようになりましたが、製品を追加した後、カートを表示するためのロジックを実装していません。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-142">We can now add items to our shopping cart but we have not implemented the logic to display the cart after a product has been added.</span></span>

<span data-ttu-id="0e5e9-143">、MyShoppingCart.aspx ページでこれを追加、EntityDataSource コントロールと GridVire コントロールには、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-143">So, in the MyShoppingCart.aspx page we'll add an EntityDataSource control and a GridVire control as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample8.aspx)]

<span data-ttu-id="0e5e9-144">カートの更新 ボタンをダブルクリックして、マークアップで宣言で指定されているクリック イベント ハンドラーを生成できるように、デザイナーでフォームを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-144">Call up the form in the designer so that you can double click on the Update Cart button and generate the click event handler that is specified in the declaration in the markup.</span></span>

<span data-ttu-id="0e5e9-145">後で詳細を実装しますが、お知らせビルドおよび実行のエラーのないアプリケーションではこれを行います。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-145">We'll implement the details later but doing this will let us build and run our application without errors.</span></span>

<span data-ttu-id="0e5e9-146">アプリケーションを実行し、ショッピング カートにアイテムを追加するときにこれが表示されます。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-146">When you run the application and add an item to the shopping cart you will see this.</span></span>

![](tailspin-spyworks-part-5/_static/image2.jpg)

<span data-ttu-id="0e5e9-147">3 つのカスタム列を実装することで、「既定」のグリッド表示から逸脱ことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-147">Note that we have deviated from the "default" grid display by implementing three custom columns.</span></span>

<span data-ttu-id="0e5e9-148">1 つは、編集可能な数量の「バインド」フィールドには。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-148">The first is an Editable, "Bound" field for the Quantity:</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample9.aspx)]

<span data-ttu-id="0e5e9-149">(注文する数量回アイテム コスト) 行アイテムの合計を表示する「計算」の列を次に示します。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-149">The next is a "calculated" column that displays the line item total (the item cost times the quantity to be ordered):</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample10.aspx)]

<span data-ttu-id="0e5e9-150">最後に、ユーザーがショッピングのグラフから、アイテムを削除することを示すために使用する チェック ボックス コントロールを含むカスタムの列があります。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-150">Lastly we have a custom column that contains a CheckBox control that the user will use to indicate that the item should be removed from the shopping chart.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample11.aspx)]

![](tailspin-spyworks-part-5/_static/image3.jpg)

<span data-ttu-id="0e5e9-151">ご覧のように、合計行がでは空の順序は注文合計を計算するいくつかのロジックを追加します。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-151">As you can see, the Order Total line is empty so let's add some logic to calculate the Order Total.</span></span>

<span data-ttu-id="0e5e9-152">まず"GetTotal"メソッド MyShoppingCart クラスに実装します。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-152">We'll first implement a "GetTotal" method to our MyShoppingCart Class.</span></span>

<span data-ttu-id="0e5e9-153">MyShoppingCart.cs ファイルでは、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-153">In the MyShoppingCart.cs file add the following code.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample12.cs)]

<span data-ttu-id="0e5e9-154">次のページに\_Load イベント ハンドラーが、GetTotal メソッドを呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-154">Then in the Page\_Load event handler we'll can call our GetTotal method.</span></span> <span data-ttu-id="0e5e9-155">同時に、ショッピング カートが空かどうかである場合、表示を調整してテストを追加します。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-155">At the same time we'll add a test to see if the shopping cart is empty and adjust the display accordingly if it is.</span></span>

<span data-ttu-id="0e5e9-156">これで、ショッピング カートの内容が空の場合これが表示されます。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-156">Now if the shopping cart is empty we get this:</span></span>

![](tailspin-spyworks-part-5/_static/image4.jpg)

<span data-ttu-id="0e5e9-157">そうでない場合、合計がわかります。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-157">And if not, we see our total.</span></span>

![](tailspin-spyworks-part-5/_static/image5.jpg)

<span data-ttu-id="0e5e9-158">ただし、このページはまだ完了しません。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-158">However, this page is not yet complete.</span></span>

<span data-ttu-id="0e5e9-159">追加のロジックを削除対象としてマークされた項目を削除して、一部が変更されたグリッドで、ユーザーが、新しい数量の値を決定することにより、ショッピング カートを再計算する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-159">We will need additional logic to recalculate the shopping cart by removing items marked for removal and by determining new quantity values as some may have been changed in the grid by the user.</span></span>

<span data-ttu-id="0e5e9-160">ユーザーがアイテムの削除をマークするときに、ケースを処理する MyShoppingCart.cs でショッピング カート クラスに"RemoveItem"メソッドを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-160">Lets add a "RemoveItem" method to our shopping cart class in MyShoppingCart.cs to handle the case when a user marks an item for removal.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample13.cs)]

<span data-ttu-id="0e5e9-161">今すぐみましょう ad ユーザーは、GridView で注文する品質を単に変更されたときに、状況を処理するメソッド。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-161">Now let's ad a method to handle the circumstance when a user simply changes the quality to be ordered in the GridView.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample14.cs)]

<span data-ttu-id="0e5e9-162">場所の削除と更新プログラムの基本的な機能実際には、データベース内のショッピング カートを更新するロジックを実装できます。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-162">With the basic Remove and Update features in place we can implement the logic that actually updates the shopping cart in the database.</span></span> <span data-ttu-id="0e5e9-163">(で MyShoppingCart.cs)</span><span class="sxs-lookup"><span data-stu-id="0e5e9-163">(In MyShoppingCart.cs)</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample15.cs)]

<span data-ttu-id="0e5e9-164">このメソッドが 2 つのパラメーターを期待していることがわかります。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-164">You'll note that this method expects two parameters.</span></span> <span data-ttu-id="0e5e9-165">ショッピング カート Id 1 つは、他のユーザー定義型のオブジェクトの配列。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-165">One is the shopping cart Id and the other is an array of objects of user defined type.</span></span>

<span data-ttu-id="0e5e9-166">ユーザー インターフェイスの仕様に、ロジックの依存関係を最小限にとどめる GridView コントロールに直接アクセスする必要がある方法では、せず、コードをショッピング カートのアイテムを渡す使用できるデータ構造を定義しました。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-166">So as to minimize the dependency of our logic on user interface specifics, we've defined a data structure that we can use to pass the shopping cart items to our code without our method needing to directly access the GridView control.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample16.cs)]

<span data-ttu-id="0e5e9-167">MyShoppingCart.aspx.cs ファイルで使用できますこの構造体の Update ボタンの Click イベント ハンドラーでとおり。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-167">In our MyShoppingCart.aspx.cs file we can use this structure in our Update Button Click Event handler as follows.</span></span> <span data-ttu-id="0e5e9-168">に加えて、カートの更新を更新してカート合計も注意してください。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-168">Note that in addition to updating the cart we will update the cart total as well.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample17.cs)]

<span data-ttu-id="0e5e9-169">次のコード行で特に重要な注意してください。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-169">Note with particular interest this line of code:</span></span>

[!code-javascript[Main](tailspin-spyworks-part-5/samples/sample18.js)]

<span data-ttu-id="0e5e9-170">GetValues() は、次のように MyShoppingCart.aspx.cs で私たちが実装する特殊なヘルパー関数です。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-170">GetValues() is a special helper function that we will implement in MyShoppingCart.aspx.cs as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample19.cs)]

<span data-ttu-id="0e5e9-171">これは、クリーン、GridView コントロールのバインド要素の値にアクセスする方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-171">This provides a clean way to access the values of the bound elements in our GridView control.</span></span> <span data-ttu-id="0e5e9-172">ユーザー「アイテムの削除」のチェック ボックス コントロールがバインドされていないために、FindControl() メソッドを使用してアクセスします。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-172">Since our "Remove Item" CheckBox Control is not bound we'll access it via the FindControl() method.</span></span>

<span data-ttu-id="0e5e9-173">この段階で、プロジェクトの開発、清算処理を実装する準備ができていますされています。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-173">At this stage in your project's development we are getting ready to implement the checkout process.</span></span>

<span data-ttu-id="0e5e9-174">そってしましょう前に、Visual Studio を使用して、メンバーシップ データベースを生成し、ユーザーのメンバーシップのリポジトリを追加します。</span><span class="sxs-lookup"><span data-stu-id="0e5e9-174">Before doing so let's use Visual Studio to generate the membership database and add a user to the membership repository.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0e5e9-175">[前へ](tailspin-spyworks-part-4.md)
> [次へ](tailspin-spyworks-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="0e5e9-175">[Previous](tailspin-spyworks-part-4.md)
[Next](tailspin-spyworks-part-6.md)</span></span>
