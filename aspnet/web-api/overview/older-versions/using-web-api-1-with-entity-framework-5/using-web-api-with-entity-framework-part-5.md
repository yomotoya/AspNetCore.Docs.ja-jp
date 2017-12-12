---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
title: "手順 5: Knockout.js にダイナミック UI を作成する |Microsoft ドキュメント"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 9d9cb3b0-f4a7-434e-a508-9fc0ad0eb813
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
msc.type: authoredcontent
ms.openlocfilehash: 20ebdb1b8ba710e0fbc6040f7cd4064b44658c53
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="part-5-creating-a-dynamic-ui-with-knockoutjs"></a><span data-ttu-id="dced8-102">手順 5: Knockout.js にダイナミック UI を作成します。</span><span class="sxs-lookup"><span data-stu-id="dced8-102">Part 5: Creating a Dynamic UI with Knockout.js</span></span>
====================
<span data-ttu-id="dced8-103">によって[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="dced8-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="dced8-104">完成したプロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="dced8-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-a-dynamic-ui-with-knockoutjs"></a><span data-ttu-id="dced8-105">Knockout.js にダイナミック UI を作成します。</span><span class="sxs-lookup"><span data-stu-id="dced8-105">Creating a Dynamic UI with Knockout.js</span></span>

<span data-ttu-id="dced8-106">このセクションでは、管理ビューの機能を追加するのに Knockout.js 使用されます。</span><span class="sxs-lookup"><span data-stu-id="dced8-106">In this section, we'll use Knockout.js to add functionality to the Admin view.</span></span>

<span data-ttu-id="dced8-107">[Knockout.js](http://knockoutjs.com/) Javascript ライブラリの HTML コントロールをデータにバインドするが容易です。</span><span class="sxs-lookup"><span data-stu-id="dced8-107">[Knockout.js](http://knockoutjs.com/) is a Javascript library that makes it easy to bind HTML controls to data.</span></span> <span data-ttu-id="dced8-108">Knockout.js は、モデル View-viewmodel (MVVM) パターンを使用します。</span><span class="sxs-lookup"><span data-stu-id="dced8-108">Knockout.js uses the Model-View-ViewModel (MVVM) pattern.</span></span>

- <span data-ttu-id="dced8-109">*モデル*の場合、製品および orders) の「ビジネス ドメイン内のデータのサーバー側表現です。</span><span class="sxs-lookup"><span data-stu-id="dced8-109">The *model* is the server-side representation of the data in the business domain (in our case, products and orders).</span></span>
- <span data-ttu-id="dced8-110">*ビュー*は、プレゼンテーション層 (HTML)。</span><span class="sxs-lookup"><span data-stu-id="dced8-110">The *view* is the presentation layer (HTML).</span></span>
- <span data-ttu-id="dced8-111">*ビュー モデル*Javascript オブジェクト モデルのデータを保持します。</span><span class="sxs-lookup"><span data-stu-id="dced8-111">The *view-model* is a Javascript object that holds the model data.</span></span> <span data-ttu-id="dced8-112">ビュー モデルは、UI のコードの抽象化です。</span><span class="sxs-lookup"><span data-stu-id="dced8-112">The view-model is a code abstraction of the UI.</span></span> <span data-ttu-id="dced8-113">HTML 形式の知識がないです。</span><span class="sxs-lookup"><span data-stu-id="dced8-113">It has no knowledge of the HTML representation.</span></span> <span data-ttu-id="dced8-114">代わりに、「アイテムの一覧」など、ビューの抽象機能を表します。</span><span class="sxs-lookup"><span data-stu-id="dced8-114">Instead, it represents abstract features of the view, such as "a list of items".</span></span>

<span data-ttu-id="dced8-115">ビュー データにバインドされてビュー モデルです。</span><span class="sxs-lookup"><span data-stu-id="dced8-115">The view is data-bound to the view-model.</span></span> <span data-ttu-id="dced8-116">ビュー モデルへの更新は、ビューに自動的に反映されます。</span><span class="sxs-lookup"><span data-stu-id="dced8-116">Updates to the view-model are automatically reflected in the view.</span></span> <span data-ttu-id="dced8-117">ビュー モデルは、ボタンのクリックなど、ビューからイベントを取得し、注文を作成するなど、モデルでの操作を実行します。</span><span class="sxs-lookup"><span data-stu-id="dced8-117">The view-model also gets events from the view, such as button clicks, and performs operations on the model, such as creating an order.</span></span>

![](using-web-api-with-entity-framework-part-5/_static/image1.png)

<span data-ttu-id="dced8-118">最初のビュー モデルを定義します。</span><span class="sxs-lookup"><span data-stu-id="dced8-118">First we'll define the view-model.</span></span> <span data-ttu-id="dced8-119">その後は、ビュー モデルに、HTML マークアップをバインドします。</span><span class="sxs-lookup"><span data-stu-id="dced8-119">After that, we will bind the HTML markup to the view-model.</span></span>

<span data-ttu-id="dced8-120">Admin.cshtml に Razor の次のセクションを追加します。</span><span class="sxs-lookup"><span data-stu-id="dced8-120">Add the following Razor section to Admin.cshtml:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample1.cshtml)]

<span data-ttu-id="dced8-121">このセクションでは、ファイルの任意の場所追加できます。</span><span class="sxs-lookup"><span data-stu-id="dced8-121">You can add this section anywhere in the file.</span></span> <span data-ttu-id="dced8-122">終了する前に、ビューが表示されると、セクションは、HTML ページの下部に表示されます。 右&lt;/body&gt;タグ。</span><span class="sxs-lookup"><span data-stu-id="dced8-122">When the view is rendered, the section appears at the bottom of the HTML page, right before the closing &lt;/body&gt; tag.</span></span>

<span data-ttu-id="dced8-123">このページのスクリプトのすべてがコメントで示されているスクリプト タグの内部参照してください。</span><span class="sxs-lookup"><span data-stu-id="dced8-123">All of the script for this page will go inside the script tag indicated by the comment:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample2.html)]

<span data-ttu-id="dced8-124">最初に、ビュー モデル クラスを定義します。</span><span class="sxs-lookup"><span data-stu-id="dced8-124">First, define a view-model class:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample3.js)]

<span data-ttu-id="dced8-125">**ko.observableArray** Knockout と呼ばれる内のオブジェクトの特殊なは、 *observable*です。</span><span class="sxs-lookup"><span data-stu-id="dced8-125">**ko.observableArray** is a special kind of object in Knockout, called an *observable*.</span></span> <span data-ttu-id="dced8-126">[Knockout.js ドキュメント](http://knockoutjs.com/documentation/observables.html): 監視対象は、「を JavaScript オブジェクトの変更はサブスクライバーに通知できます」。</span><span class="sxs-lookup"><span data-stu-id="dced8-126">From the [Knockout.js documentation](http://knockoutjs.com/documentation/observables.html): An observable is a "JavaScript object that can notify subscribers about changes."</span></span> <span data-ttu-id="dced8-127">監視対象の内容を変更するときに一致するように、ビューが自動的に更新します。</span><span class="sxs-lookup"><span data-stu-id="dced8-127">When the contents of an observable change, the view is automatically updated to match.</span></span>

<span data-ttu-id="dced8-128">設定する、`products`配列、web API への AJAX 要求を作成します。</span><span class="sxs-lookup"><span data-stu-id="dced8-128">To populate the `products` array, make an AJAX request to the web API.</span></span> <span data-ttu-id="dced8-129">ビュー バッグ内の API のベース URI を格納おことに注意してください (を参照してください[パート 4](using-web-api-with-entity-framework-part-4.md)チュートリアルの)。</span><span class="sxs-lookup"><span data-stu-id="dced8-129">Recall that we stored the base URI for the API in the view bag (see [Part 4](using-web-api-with-entity-framework-part-4.md) of the tutorial).</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample4.js?highlight=5)]

<span data-ttu-id="dced8-130">次に、関数を作成、更新、および製品を削除するビュー モデルに追加します。</span><span class="sxs-lookup"><span data-stu-id="dced8-130">Next, add functions to the view-model to create, update, and delete products.</span></span> <span data-ttu-id="dced8-131">これらの関数は、web API への AJAX 呼び出しを送信し、結果を使用して、ビュー モデルを更新します。</span><span class="sxs-lookup"><span data-stu-id="dced8-131">These functions submit AJAX calls to the web API and use the results to update the view-model.</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample5.js?highlight=7)]

<span data-ttu-id="dced8-132">最も重要な部分ようになりました: と DOM が fulled ロードを呼び出し、 **ko.applyBindings**機能しの新しいインスタンスを渡す、 `ProductsViewModel`:</span><span class="sxs-lookup"><span data-stu-id="dced8-132">Now the most important part: When the DOM is fulled loaded, call the **ko.applyBindings** function and pass in a new instance of the `ProductsViewModel`:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample6.js)]

<span data-ttu-id="dced8-133">**Ko.applyBindings**メソッド Knockout をアクティブにし、ビューをビュー モデルに接続します。</span><span class="sxs-lookup"><span data-stu-id="dced8-133">The **ko.applyBindings** method activates Knockout and wires up the view-model to the view.</span></span>

<span data-ttu-id="dced8-134">ビュー モデルがある、バインディングを作成できます。</span><span class="sxs-lookup"><span data-stu-id="dced8-134">Now that we have a view-model, we can create the bindings.</span></span> <span data-ttu-id="dced8-135">Knockout.js でこれを行う追加することによって`data-bind`HTML 要素に属性します。</span><span class="sxs-lookup"><span data-stu-id="dced8-135">In Knockout.js, you do this by adding `data-bind` attributes to HTML elements.</span></span> <span data-ttu-id="dced8-136">たとえば、HTML の一覧を配列にバインドするに使用して、`foreach`バインド。</span><span class="sxs-lookup"><span data-stu-id="dced8-136">For example, to bind an HTML list to an array, use the `foreach` binding:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample7.html?highlight=1)]

<span data-ttu-id="dced8-137">`foreach`バインディングが、配列を反復処理し、配列の各オブジェクトの要素の子を作成します。</span><span class="sxs-lookup"><span data-stu-id="dced8-137">The `foreach` binding iterates through the array and creates child elements for each object in the array.</span></span> <span data-ttu-id="dced8-138">子要素のバインディングは、配列のオブジェクトのプロパティを参照できます。</span><span class="sxs-lookup"><span data-stu-id="dced8-138">Bindings on the child elements can refer to properties on the array objects.</span></span>

<span data-ttu-id="dced8-139">「製品更新プログラム」一覧に、以下のバインドを追加します。</span><span class="sxs-lookup"><span data-stu-id="dced8-139">Add the following bindings to the "update-products" list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample8.html)]

<span data-ttu-id="dced8-140">`<li>`のスコープ内の要素が発生した、 **foreach**バインドします。</span><span class="sxs-lookup"><span data-stu-id="dced8-140">The `<li>` element occurs within the scope of the **foreach** binding.</span></span> <span data-ttu-id="dced8-141">手段 Knockout されますが、要素の各製品の 1 回をレンダリング、`products`配列。</span><span class="sxs-lookup"><span data-stu-id="dced8-141">That means Knockout will render the element once for each product in the `products` array.</span></span> <span data-ttu-id="dced8-142">すべてのバインド内で、`<li>`要素は、その製品のインスタンスを参照してください。</span><span class="sxs-lookup"><span data-stu-id="dced8-142">All of the bindings within the `<li>` element refer to that product instance.</span></span> <span data-ttu-id="dced8-143">たとえば、`$data.Name`を指す、`Name`製品のプロパティです。</span><span class="sxs-lookup"><span data-stu-id="dced8-143">For example, `$data.Name` refers to the `Name` property on the product.</span></span>

<span data-ttu-id="dced8-144">テキスト入力の値を設定するには、使用、`value`バインドします。</span><span class="sxs-lookup"><span data-stu-id="dced8-144">To set the values of the text inputs, use the `value` binding.</span></span> <span data-ttu-id="dced8-145">ボタンは、モデル ビューで関数にバインドされるを使用して、`click`バインドします。</span><span class="sxs-lookup"><span data-stu-id="dced8-145">The buttons are bound to functions on the model-view, using the `click` binding.</span></span> <span data-ttu-id="dced8-146">製品のインスタンスは、各関数にパラメーターとして渡されます。</span><span class="sxs-lookup"><span data-stu-id="dced8-146">The product instance is passed as a parameter to each function.</span></span> <span data-ttu-id="dced8-147">詳細については、 [Knockout.js ドキュメント](http://knockoutjs.com/documentation/observables.html)が各種のバインディングの適切な説明。</span><span class="sxs-lookup"><span data-stu-id="dced8-147">For more information, the [Knockout.js documentation](http://knockoutjs.com/documentation/observables.html) has good descriptions of the various bindings.</span></span>

<span data-ttu-id="dced8-148">次に、追加のバインディングを**送信**製品の追加フォームでイベント。</span><span class="sxs-lookup"><span data-stu-id="dced8-148">Next, add a binding for the **submit** event on the Add Product form:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample9.html)]

<span data-ttu-id="dced8-149">このバインディングを呼び出す、`create`関数をビュー モデルに新しい製品を作成します。</span><span class="sxs-lookup"><span data-stu-id="dced8-149">This binding calls the `create` function on the view-model to create a new product.</span></span>

<span data-ttu-id="dced8-150">管理ビューの完全なコードを次に示します。</span><span class="sxs-lookup"><span data-stu-id="dced8-150">Here is the complete code for the Admin view:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample10.cshtml)]

<span data-ttu-id="dced8-151">アプリケーションを実行し、管理者アカウントでログイン"Admin"リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="dced8-151">Run the application, log in with the Administrator account, and click the "Admin" link.</span></span> <span data-ttu-id="dced8-152">、製品の一覧を表示し、作成、更新、または製品を削除することができる必要があります。</span><span class="sxs-lookup"><span data-stu-id="dced8-152">You should see the list of products, and be able to create, update, or delete products.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="dced8-153">[前へ](using-web-api-with-entity-framework-part-4.md)
[次へ](using-web-api-with-entity-framework-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="dced8-153">[Previous](using-web-api-with-entity-framework-part-4.md)
[Next](using-web-api-with-entity-framework-part-6.md)</span></span>
