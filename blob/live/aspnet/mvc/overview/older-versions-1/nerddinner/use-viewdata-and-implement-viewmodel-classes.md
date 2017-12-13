---
uid: mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
title: "使用して ViewData と実装 ViewModel クラス |Microsoft ドキュメント"
author: microsoft
description: "手順 6 に示す方法、シナリオの編集より豊富なフォームのサポートを有効にして、コント ローラーからビューにデータを渡すために使用する 2 つの方法についても説明: しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 5755ec4c-60f1-4057-9ec0-3a5de3a20e23
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
msc.type: authoredcontent
ms.openlocfilehash: 36b9e87cc24f74f7f2cc592afb5102709b598f74
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="use-viewdata-and-implement-viewmodel-classes"></a><span data-ttu-id="a8f6c-103">使用して ViewData と実装 ViewModel クラス</span><span class="sxs-lookup"><span data-stu-id="a8f6c-103">Use ViewData and Implement ViewModel Classes</span></span>
====================
<span data-ttu-id="a8f6c-104">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="a8f6c-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="a8f6c-105">PDF をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="a8f6c-106">これは、無料の手順 6. ["NerdDinner"アプリケーションのチュートリアル](introducing-the-nerddinner-tutorial.md)をウォーク-にする方法を小規模の構築が完了すると、ASP.NET MVC 1 を使用して web アプリケーションです。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-106">This is step 6 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="a8f6c-107">手順 6 に示しますがどの高度なフォームが、シナリオの編集のサポートを有効にし、コント ローラーからビューにデータを渡すために使用する 2 つの方法についても説明: ViewData と ViewModel です。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-107">Step 6 shows how enable support for richer form editing scenarios, and also discusses two approaches that can be used to pass data from controllers to views: ViewData and ViewModel.</span></span>
> 
> <span data-ttu-id="a8f6c-108">ASP.NET MVC 3 を使用している場合ことをお勧めする、 [MVC 3 の開始と取得](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)または[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)チュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-6-viewdata-and-viewmodel"></a><span data-ttu-id="a8f6c-109">NerdDinner 手順 6: ViewData と ViewModel</span><span class="sxs-lookup"><span data-stu-id="a8f6c-109">NerdDinner Step 6: ViewData and ViewModel</span></span>

<span data-ttu-id="a8f6c-110">フォーム ポスト シナリオの数を説明および実装する方法を説明した作成、更新および削除 (CRUD) のサポート。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-110">We've covered a number of form post scenarios, and discussed how to implement create, update and delete (CRUD) support.</span></span> <span data-ttu-id="a8f6c-111">うまく DinnersController 実装がさらにかかるようになりましたしシナリオの編集より豊富なフォームのサポートを有効にします。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-111">We'll now take our DinnersController implementation further and enable support for richer form editing scenarios.</span></span> <span data-ttu-id="a8f6c-112">これを行う際に紹介するコント ローラーからビューにデータを渡すために使用する 2 つの方法: ViewData と ViewModel です。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-112">While doing this we'll discuss two approaches that can be used to pass data from controllers to views: ViewData and ViewModel.</span></span>

### <a name="passing-data-from-controllers-to-view-templates"></a><span data-ttu-id="a8f6c-113">コント ローラーからテンプレートを表示するデータの受け渡し</span><span class="sxs-lookup"><span data-stu-id="a8f6c-113">Passing Data from Controllers to View-Templates</span></span>

<span data-ttu-id="a8f6c-114">MVC パターンの定義特性の 1 つは、厳密な"関心の分離"アプリケーションのさまざまなコンポーネント間で適用すると役に立ちます。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-114">One of the defining characteristics of the MVC pattern is the strict "separation of concerns" it helps enforce between the different components of an application.</span></span> <span data-ttu-id="a8f6c-115">モデル、コント ローラーとビューの各適切に定義の役割と責任を定義し、適切に定義された方法で互いの間で通信します。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-115">Models, Controllers and Views each have well defined roles and responsibilities, and they communicate amongst each other in well defined ways.</span></span> <span data-ttu-id="a8f6c-116">これにより、テストの容易性を昇格させるし、コードの再利用できます。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-116">This helps promote testability and code reuse.</span></span>

<span data-ttu-id="a8f6c-117">クライアントへの HTML 応答を表示するために、コント ローラー クラスが決定したら、それは、応答を表示するために必要なすべてのデータ ビューのテンプレートを明示的に渡すこと担当します。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-117">When a Controller class decides to render an HTML response back to a client, it is responsible for explicitly passing to the view template all of the data needed to render the response.</span></span> <span data-ttu-id="a8f6c-118">表示テンプレートは、データの取得やアプリケーション ロジック – を実行する必要がありますしないおよび自体を渡し、コント ローラーのモデル/データ駆動するレンダリング コードのみを制限する必要があります代わりにします。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-118">View templates should never perform any data retrieval or application logic – and should instead limit themselves to only have rendering code that is driven off of the model/data passed to it by the controller.</span></span>

<span data-ttu-id="a8f6c-119">今すぐモデル データ、DinnersController によって渡されるテンプレートを表示するクラスは単純であり簡単 – の場合は、Index() Dinner オブジェクトの一覧、および単一 Dinner オブジェクト Details()、Edit()、Create() および Delete() 場合。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-119">Right now the model data being passed by our DinnersController class to our view templates is simple and straight-forward – a list of Dinner objects in the case of Index(), and a single Dinner object in the case of Details(), Edit(), Create() and Delete().</span></span> <span data-ttu-id="a8f6c-120">アプリケーションに UI 機能を追加して、多くの場合、しようとして、ビュー テンプレート内での HTML 応答を表示するためにこのデータだけを通過する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-120">As we add more UI capabilities to our application, we are often going to need to pass more than just this data to render HTML responses within our view templates.</span></span> <span data-ttu-id="a8f6c-121">たとえば、お可能性がある、編集内の"Country"フィールドを変更 dropdownlist、HTML テキスト ボックスの中からビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-121">For example, we might want to change the "Country" field within our Edit and Create views from being an HTML textbox to a dropdownlist.</span></span> <span data-ttu-id="a8f6c-122">ハード コード ビュー テンプレートの国名のドロップダウン リストではなく動的設定はサポートされている国の一覧から生成たい場合があります。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-122">Rather than hard-code the dropdown list of country names in the view template, we might want to generate it from a list of supported countries that we populate dynamically.</span></span> <span data-ttu-id="a8f6c-123">Dinner オブジェクトを渡す方法には必要が*と*テンプレートを表示する、コント ローラーからサポートされている国の一覧です。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-123">We will need a way to pass both the Dinner object *and* the list of supported countries from our controller to our view templates.</span></span>

<span data-ttu-id="a8f6c-124">このためにはできる 2 つの方法を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-124">Let's look at two ways we can accomplish this.</span></span>

### <a name="using-the-viewdata-dictionary"></a><span data-ttu-id="a8f6c-125">ViewData ディクショナリを使用してください。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-125">Using the ViewData Dictionary</span></span>

<span data-ttu-id="a8f6c-126">コント ローラーの基本クラスは、コント ローラーからビューに渡される追加のデータ項目に使用できる"ViewData"辞書プロパティを公開します。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-126">The Controller base class exposes a "ViewData" dictionary property that can be used to pass additional data items from Controllers to Views.</span></span>

<span data-ttu-id="a8f6c-127">たとえば、お dropdownlist、HTML テキスト ボックスの中から、編集ビュー内で、「国」テキスト ボックスを変更するシナリオをサポートするために更新することがオブジェクトを渡します (Dinner オブジェクト) だけでなく SelectList m として使用できる、Edit() アクション メソッド国の dropdownlist の odel します。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-127">For example, to support the scenario where we want to change the "Country" textbox within our Edit view from being an HTML textbox to a dropdownlist, we can update our Edit() action method to pass (in addition to a Dinner object) a SelectList object that can be used as the model of a countries dropdownlist.</span></span>

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample1.cs)]

<span data-ttu-id="a8f6c-128">上記の SelectList のコンス トラクターでは、郡、ドロップ ダウン リストを設定するだけでなく、現在選択されている値の一覧が受け入れされます。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-128">The constructor of the SelectList above is accepting a list of counties to populate the drop-downlist with, as well as the currently selected value.</span></span>

<span data-ttu-id="a8f6c-129">以前を使用した Html.TextBox() ヘルパー メソッドではなく Html.DropDownList() ヘルパー メソッドを使用して、テンプレートの Edit.aspx 表示し、更新できます。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-129">We can then update our Edit.aspx view template to use the Html.DropDownList() helper method instead of the Html.TextBox() helper method we used previously:</span></span>

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample2.aspx)]

<span data-ttu-id="a8f6c-130">上記の Html.DropDownList() ヘルパー メソッドは、次の 2 つのパラメーターを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-130">The Html.DropDownList() helper method above takes two parameters.</span></span> <span data-ttu-id="a8f6c-131">1 つ目は、出力を HTML フォーム要素の名前です。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-131">The first is the name of the HTML form element to output.</span></span> <span data-ttu-id="a8f6c-132">2 つ目は、ViewData ディクショナリを使用して渡された"SelectList"モデルです。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-132">The second is the "SelectList" model we passed via the ViewData dictionary.</span></span> <span data-ttu-id="a8f6c-133">おは c# を使用して、"as"キーワード、SelectList としてディクショナリ内の型にキャストします。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-133">We are using the C# "as" keyword to cast the type within the dictionary as a SelectList.</span></span>

<span data-ttu-id="a8f6c-134">今すぐ実行するとき、マイクロソフトのアプリケーションとアクセスと、 */Dinners/Edit/1* URL、ブラウザー内で後ほどお見せをテキスト ボックスではなく国の dropdownlist の表示、編集の UI が更新されたこと。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-134">And now when we run our application and access the */Dinners/Edit/1* URL within our browser we'll see that our edit UI has been updated to display a dropdownlist of countries instead of a textbox:</span></span>

![](use-viewdata-and-implement-viewmodel-classes/_static/image1.png)

<span data-ttu-id="a8f6c-135">また、HTTP POST メソッドの編集 (でエラーが発生したときに使用する場合) からテンプレートを編集ビューをレンダリングおために、私たちもビュー テンプレートは、エラーのシナリオでレンダリングするときに、SelectList を ViewData に追加するには、このメソッドが更新することを確認してくださいお必要があります。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-135">Because we also render the Edit view template from the HTTP-POST Edit method (in scenarios when errors occur), we'll want to make sure that we also update this method to add the SelectList to ViewData when the view template is rendered in error scenarios:</span></span>

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample3.cs)]

<span data-ttu-id="a8f6c-136">今すぐ DinnersController 編集に紹介したシナリオが DropDownList をサポートしています。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-136">And now our DinnersController edit scenario supports a DropDownList.</span></span>

### <a name="using-a-viewmodel-pattern"></a><span data-ttu-id="a8f6c-137">ViewModel パターンの使用</span><span class="sxs-lookup"><span data-stu-id="a8f6c-137">Using a ViewModel Pattern</span></span>

<span data-ttu-id="a8f6c-138">ViewData ディクショナリ方法には、非常に高速で簡単に実装されているの利点があります。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-138">The ViewData dictionary approach has the benefit of being fairly fast and easy to implement.</span></span> <span data-ttu-id="a8f6c-139">一部の開発者に満足できない文字列ベースのディクショナリを使用して、ので、入力ミスがコンパイル時ではキャッチされないエラーにつながることができます。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-139">Some developers don't like using string-based dictionaries, though, since typos can lead to errors that will not be caught at compile-time.</span></span> <span data-ttu-id="a8f6c-140">型指定されていない ViewData ディクショナリは、「と」演算子を使用してまたはキャスト ビュー テンプレート c# のような厳密に型指定された言語を使用する場合にも必要です。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-140">The un-typed ViewData dictionary also requires using the "as" operator or casting when using a strongly-typed language like C# in a view template.</span></span>

<span data-ttu-id="a8f6c-141">使用できるその他の方法では、"ViewModel"パターンと呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-141">An alternative approach that we could use is one often referred to as the "ViewModel" pattern.</span></span> <span data-ttu-id="a8f6c-142">このパターンを使用する場合、特定のビューのシナリオ用に最適化されていると、テンプレートの表示に必要な動的な値/コンテンツのプロパティを公開する厳密に型指定されたクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-142">When using this pattern we create strongly-typed classes that are optimized for our specific view scenarios, and which expose properties for the dynamic values/content needed by our view templates.</span></span> <span data-ttu-id="a8f6c-143">コント ローラー クラスでは設定し、ビュー用に最適化されたこれらのクラスを使用するには、このビュー テンプレートに渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-143">Our controller classes can then populate and pass these view-optimized classes to our view template to use.</span></span> <span data-ttu-id="a8f6c-144">これにより、タイプ セーフ、コンパイル時のチェック、およびエディターの intellisense ビュー テンプレート内で。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-144">This enables type-safety, compile-time checking, and editor intellisense within view templates.</span></span>

<span data-ttu-id="a8f6c-145">例については、厳密に型指定された 2 つのプロパティを公開する dinner フォーム"DinnerFormViewModel"を作成できるよう編集のシナリオは以下のようにクラスを有効にする: Dinner オブジェクト、および国の dropdownlist の作成に必要な SelectList モデル。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-145">For example, to enable dinner form editing scenarios we can create a "DinnerFormViewModel" class like below that exposes two strongly-typed properties: a Dinner object, and the SelectList model needed to populate the countries dropdownlist:</span></span>

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample4.cs)]

<span data-ttu-id="a8f6c-146">このリポジトリから取得するまでお Dinner オブジェクトを使用して DinnerFormViewModel を作成する、Edit() アクション メソッドを更新したり、ビュー テンプレートに渡すことできます。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-146">We can then update our Edit() action method to create the DinnerFormViewModel using the Dinner object we retrieve from our repository, and then pass it to our view template:</span></span>

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample5.cs)]

<span data-ttu-id="a8f6c-147">Edit.aspx ページの上部にある"inherits"属性を変更することによってオブジェクトのため、その it"Dinner"ではなく"DinnerFormViewModel"が必要ですが、ビュー テンプレート更新次のように、します。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-147">We'll then update our view template so that it expects a "DinnerFormViewModel" instead of a "Dinner" object by changing the "inherits" attribute at the top of the edit.aspx page like so:</span></span>

[!code-cshtml[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample6.cshtml)]

<span data-ttu-id="a8f6c-148">この作業を行う、「モデル」プロパティの表示テンプレート内での intellisense を渡している DinnerFormViewModel 型のオブジェクト モデルを反映するように更新されます。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-148">Once we do this, the intellisense of the "Model" property within our view template will be updated to reflect the object model of the DinnerFormViewModel type we are passing it:</span></span>

![](use-viewdata-and-implement-viewmodel-classes/_static/image2.png)

![](use-viewdata-and-implement-viewmodel-classes/_static/image3.png)

<span data-ttu-id="a8f6c-149">これを使用する、コードの表示更新できます。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-149">We can then update our view code to work off of it.</span></span> <span data-ttu-id="a8f6c-150">作成する方法は、入力要素の名前を変更しない下通知 (フォーム要素があるという名前のまま"Title"、「国」) – DinnerFormViewModel クラスを使用して値を取得する HTML ヘルパー メソッドを更新しましたが、します。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-150">Notice below how we are not changing the names of the input elements we are creating (the form elements will still be named "Title", "Country") – but we are updating the HTML Helper methods to retrieve the values using the DinnerFormViewModel class:</span></span>

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample7.aspx)]

<span data-ttu-id="a8f6c-151">エラーを表示するときに、DinnerFormViewModel クラスを使用して、編集の post メソッドも更新されます。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-151">We'll also update our Edit post method to use the DinnerFormViewModel class when rendering errors:</span></span>

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample8.cs)]

<span data-ttu-id="a8f6c-152">更新することが、Create() アクション メソッドを正確な再使用する同じ*DinnerFormViewModel*国 DropDownList 内部のも有効にするクラス。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-152">We can also update our Create() action methods to re-use the exact same *DinnerFormViewModel* class to enable the countries DropDownList within those as well.</span></span> <span data-ttu-id="a8f6c-153">HTTP GET の実装を次に示します。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-153">Below is the HTTP-GET implementation:</span></span>

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample9.cs)]

<span data-ttu-id="a8f6c-154">HTTP POST 作成メソッドの実装を次に示します。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-154">Below is the implementation of the HTTP-POST Create method:</span></span>

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample10.cs)]

<span data-ttu-id="a8f6c-155">国を選択するため、両方の編集および作成する画面がドロップダウン リストをサポートするようになりました。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-155">And now both our Edit and Create screens support drop-downlists for picking the country.</span></span>

### <a name="custom-shaped-viewmodel-classes"></a><span data-ttu-id="a8f6c-156">ViewModel クラスのカスタムの形</span><span class="sxs-lookup"><span data-stu-id="a8f6c-156">Custom-shaped ViewModel classes</span></span>

<span data-ttu-id="a8f6c-157">上記のシナリオに DinnerFormViewModel クラスは直接サポート SelectList モデル プロパティと、プロパティとして Dinner モデル オブジェクトを公開します。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-157">In the scenario above, our DinnerFormViewModel class directly exposes the Dinner model object as a property, along with a supporting SelectList model property.</span></span> <span data-ttu-id="a8f6c-158">この方法は問題なく動作のシナリオが、ビュー テンプレート内で作成する HTML UI に対応する比較的密接にドメイン モデル オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-158">This approach works fine for scenarios where the HTML UI we want to create within our view template corresponds relatively closely to our domain model objects.</span></span>

<span data-ttu-id="a8f6c-159">シナリオではありません、使用できる 1 つのオプションは、– ビューによって消費のオブジェクト モデルがより最適化されており、これが検索する、基になるドメイン モデル オブジェクトとはまったく異なるにカスタムの形 ViewModel クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-159">For scenarios where this isn't the case, one option that you can use is to create a custom-shaped ViewModel class whose object model is more optimized for consumption by the view – and which might look completely different from the underlying domain model object.</span></span> <span data-ttu-id="a8f6c-160">たとえば、可能性のある公開される可能性が別のプロパティの名前や複数のモデル オブジェクトから収集した集計のプロパティです。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-160">For example, it could potentially expose different property names and/or aggregate properties collected from multiple model objects.</span></span>

<span data-ttu-id="a8f6c-161">ViewModel クラスのカスタムの形は、両方を表示するだけでなく、コント ローラーのアクション メソッドにポストバックされるフォーム データの処理に役立つビューに、コント ローラーからデータを渡すために使用します。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-161">Custom-shaped ViewModel classes can be used both to pass data from controllers to views to render, as well as to help handle form data posted back to a controller's action method.</span></span> <span data-ttu-id="a8f6c-162">後でこのシナリオでは、ViewModel オブジェクトをフォーム ポストされたデータで更新し、ViewModel インスタンスを使用して、マップまたは実際のドメイン モデル オブジェクトを取得、アクション メソッドがあります。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-162">For this later scenario, you might have the action method update a ViewModel object with the form-posted data, and then use the ViewModel instance to map or retrieve an actual domain model object.</span></span>

<span data-ttu-id="a8f6c-163">ViewModel クラスのカスタムの形では、柔軟性、大幅に向上を提供し、複雑すぎて取得を開始して、アクション メソッドの中のテンプレートの表示またはフォーム ポスト コード内でレンダリング コードを検索する任意の時間を調査するものでは。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-163">Custom-shaped ViewModel classes can provide a great deal of flexibility, and are something to investigate any time you find the rendering code within your view templates or the form-posting code inside your action methods starting to get too complicated.</span></span> <span data-ttu-id="a8f6c-164">これは、多くの場合、ドメイン モデルが生成するには、UI に対応していないクリーンにおよび、カスタムの形の中間の ViewModel クラスを支援できる記号です。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-164">This is often a sign that your domain models don't cleanly correspond to the UI you are generating, and that an intermediate custom-shaped ViewModel class can help.</span></span>

### <a name="next-step"></a><span data-ttu-id="a8f6c-165">次の手順</span><span class="sxs-lookup"><span data-stu-id="a8f6c-165">Next Step</span></span>

<span data-ttu-id="a8f6c-166">これで、パーシャルおよびマスター ページを使用する再利用し、アプリケーション間で UI を共有お方法を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="a8f6c-166">Let's now look at how we can use partials and master-pages to re-use and share UI across our application.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="a8f6c-167">[前へ](provide-crud-create-read-update-delete-data-form-entry-support.md)
[次へ](re-use-ui-using-master-pages-and-partials.md)</span><span class="sxs-lookup"><span data-stu-id="a8f6c-167">[Previous](provide-crud-create-read-update-delete-data-form-entry-support.md)
[Next](re-use-ui-using-master-pages-and-partials.md)</span></span>
