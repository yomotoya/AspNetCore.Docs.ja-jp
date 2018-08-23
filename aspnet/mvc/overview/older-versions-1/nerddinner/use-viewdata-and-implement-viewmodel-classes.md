---
uid: mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
title: ViewData を使用し、ViewModel クラスの実装 |Microsoft Docs
author: microsoft
description: 手順 6 の表示には、シナリオの編集より豊富なフォームのサポートが有効にする方法もコント ローラーからビューにデータを渡すために使用できる 2 つの方法を説明しています:.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 5755ec4c-60f1-4057-9ec0-3a5de3a20e23
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
msc.type: authoredcontent
ms.openlocfilehash: 8df1ca30f8c0415b68d29eeaa0f7d83a606989ff
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832368"
---
<a name="use-viewdata-and-implement-viewmodel-classes"></a><span data-ttu-id="a9e12-103">ViewData を使用し、ViewModel クラスを実装</span><span class="sxs-lookup"><span data-stu-id="a9e12-103">Use ViewData and Implement ViewModel Classes</span></span>
====================
<span data-ttu-id="a9e12-104">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="a9e12-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="a9e12-105">PDF のダウンロード</span><span class="sxs-lookup"><span data-stu-id="a9e12-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="a9e12-106">これは、無料の手順 6 ["NerdDinner"アプリケーションのチュートリアル](introducing-the-nerddinner-tutorial.md)をウォーク スルーの小さなをビルドしても、ASP.NET MVC 1 を使用して web アプリケーションを実行する方法。</span><span class="sxs-lookup"><span data-stu-id="a9e12-106">This is step 6 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="a9e12-107">手順 6 の表示には、シナリオの編集より豊富なフォームのサポートが有効にする方法もコント ローラーからビューにデータを渡すために使用できる 2 つの方法を説明しています: ViewData とビューモデルです。</span><span class="sxs-lookup"><span data-stu-id="a9e12-107">Step 6 shows how enable support for richer form editing scenarios, and also discusses two approaches that can be used to pass data from controllers to views: ViewData and ViewModel.</span></span>
> 
> <span data-ttu-id="a9e12-108">次のことをお勧め ASP.NET MVC 3 を使用している場合、 [MVC 3 の開始と取得](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)または[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)チュートリアル。</span><span class="sxs-lookup"><span data-stu-id="a9e12-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-6-viewdata-and-viewmodel"></a><span data-ttu-id="a9e12-109">NerdDinner 手順 6: ViewData とビューモデル</span><span class="sxs-lookup"><span data-stu-id="a9e12-109">NerdDinner Step 6: ViewData and ViewModel</span></span>

<span data-ttu-id="a9e12-110">多くのフォーム post のシナリオを対象として実装する方法について説明しました作成、更新、削除 (CRUD) のサポート。</span><span class="sxs-lookup"><span data-stu-id="a9e12-110">We've covered a number of form post scenarios, and discussed how to implement create, update and delete (CRUD) support.</span></span> <span data-ttu-id="a9e12-111">DinnersController の実装をさらにかかるようになりましたされ高度なフォームのシナリオの編集のサポートを有効にします。</span><span class="sxs-lookup"><span data-stu-id="a9e12-111">We'll now take our DinnersController implementation further and enable support for richer form editing scenarios.</span></span> <span data-ttu-id="a9e12-112">これを行う際にコント ローラーからビューにデータを渡すために使用できる 2 つの方法について説明します。 ViewData とビューモデルです。</span><span class="sxs-lookup"><span data-stu-id="a9e12-112">While doing this we'll discuss two approaches that can be used to pass data from controllers to views: ViewData and ViewModel.</span></span>

### <a name="passing-data-from-controllers-to-view-templates"></a><span data-ttu-id="a9e12-113">コント ローラーからビュー テンプレートにデータを渡す</span><span class="sxs-lookup"><span data-stu-id="a9e12-113">Passing Data from Controllers to View-Templates</span></span>

<span data-ttu-id="a9e12-114">厳密な"関心の分離"MVC パターンの定義特性の 1 つは、アプリケーションの異なるコンポーネント間を適用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="a9e12-114">One of the defining characteristics of the MVC pattern is the strict "separation of concerns" it helps enforce between the different components of an application.</span></span> <span data-ttu-id="a9e12-115">モデル、コント ローラーとビューの各適切に定義のロールと職務と適切に定義された方法で互いの間で通信します。</span><span class="sxs-lookup"><span data-stu-id="a9e12-115">Models, Controllers and Views each have well defined roles and responsibilities, and they communicate amongst each other in well defined ways.</span></span> <span data-ttu-id="a9e12-116">これにより、テストの容易性を昇格させるし、コードの再利用できます。</span><span class="sxs-lookup"><span data-stu-id="a9e12-116">This helps promote testability and code reuse.</span></span>

<span data-ttu-id="a9e12-117">クライアントに、HTML 応答を表示するために、コント ローラー クラスが決定したらは、応答を表示するために必要なすべてのデータのビュー テンプレートを明示的に渡す責任を負います。</span><span class="sxs-lookup"><span data-stu-id="a9e12-117">When a Controller class decides to render an HTML response back to a client, it is responsible for explicitly passing to the view template all of the data needed to render the response.</span></span> <span data-ttu-id="a9e12-118">ビュー テンプレートでは、– データの取得やアプリケーション ロジックを実行することはありませんし、代わりに、自体は、コント ローラーによって渡されたモデル/データ駆動するレンダリング コードのみを制限する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a9e12-118">View templates should never perform any data retrieval or application logic – and should instead limit themselves to only have rendering code that is driven off of the model/data passed to it by the controller.</span></span>

<span data-ttu-id="a9e12-119">今すぐモデル データ、DinnersController で渡されたテンプレートを表示するクラスは単純であり簡単 – の場合は、Index() Dinner オブジェクトの一覧、および 1 つの Dinner オブジェクト Details()、Edit()、Create() および Delete() の場合。</span><span class="sxs-lookup"><span data-stu-id="a9e12-119">Right now the model data being passed by our DinnersController class to our view templates is simple and straight-forward – a list of Dinner objects in the case of Index(), and a single Dinner object in the case of Details(), Edit(), Create() and Delete().</span></span> <span data-ttu-id="a9e12-120">アプリケーションに UI 機能を追加すると、多くの場合、ここ、ビュー テンプレート内での HTML 応答を表示するためにこのデータだけを渡す必要があります。</span><span class="sxs-lookup"><span data-stu-id="a9e12-120">As we add more UI capabilities to our application, we are often going to need to pass more than just this data to render HTML responses within our view templates.</span></span> <span data-ttu-id="a9e12-121">たとえば、dropdownlist に HTML をテキスト ボックスの中からビューを作成、編集内の"Country"フィールドを変更する必要な可能性があります。</span><span class="sxs-lookup"><span data-stu-id="a9e12-121">For example, we might want to change the "Country" field within our Edit and Create views from being an HTML textbox to a dropdownlist.</span></span> <span data-ttu-id="a9e12-122">ハード コード国名ビュー テンプレートでのドロップダウン リストではなく動的に設定しますが、サポートされている国の一覧からそれを生成する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="a9e12-122">Rather than hard-code the dropdown list of country names in the view template, we might want to generate it from a list of supported countries that we populate dynamically.</span></span> <span data-ttu-id="a9e12-123">Dinner オブジェクトを渡す方法は必要があります*と*ビュー テンプレートに、コント ローラーからサポートされている国の一覧。</span><span class="sxs-lookup"><span data-stu-id="a9e12-123">We will need a way to pass both the Dinner object *and* the list of supported countries from our controller to our view templates.</span></span>

<span data-ttu-id="a9e12-124">そのためにできる 2 つの方法を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="a9e12-124">Let's look at two ways we can accomplish this.</span></span>

### <a name="using-the-viewdata-dictionary"></a><span data-ttu-id="a9e12-125">ViewData 辞書を使用します。</span><span class="sxs-lookup"><span data-stu-id="a9e12-125">Using the ViewData Dictionary</span></span>

<span data-ttu-id="a9e12-126">コント ローラーの基本クラスは、コント ローラーからビューを他のデータ項目を渡すために使用できる"ViewData"ディクショナリ プロパティを公開します。</span><span class="sxs-lookup"><span data-stu-id="a9e12-126">The Controller base class exposes a "ViewData" dictionary property that can be used to pass additional data items from Controllers to Views.</span></span>

<span data-ttu-id="a9e12-127">たとえば、dropdownlist に HTML をテキスト ボックスから編集ビューで、「国」テキスト ボックスを変更しシナリオをサポートするために更新できます (Dinner オブジェクト) だけでなく、m として使用できる SelectList オブジェクトを渡す、Edit() アクション メソッド国の dropdownlist のデルします。</span><span class="sxs-lookup"><span data-stu-id="a9e12-127">For example, to support the scenario where we want to change the "Country" textbox within our Edit view from being an HTML textbox to a dropdownlist, we can update our Edit() action method to pass (in addition to a Dinner object) a SelectList object that can be used as the model of a countries dropdownlist.</span></span>

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample1.cs)]

<span data-ttu-id="a9e12-128">上記の SelectList のコンス トラクターでは、郡を使用すると、ドロップ ダウン リストを設定すると、現在選択されている値の一覧を受け付けてください。</span><span class="sxs-lookup"><span data-stu-id="a9e12-128">The constructor of the SelectList above is accepting a list of counties to populate the drop-downlist with, as well as the currently selected value.</span></span>

<span data-ttu-id="a9e12-129">Html.DropDownList() ヘルパー メソッドを使用して、以前に使いました Html.TextBox() ヘルパー メソッドではなく、テンプレートの Edit.aspx 表示し、更新できます。</span><span class="sxs-lookup"><span data-stu-id="a9e12-129">We can then update our Edit.aspx view template to use the Html.DropDownList() helper method instead of the Html.TextBox() helper method we used previously:</span></span>

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample2.aspx)]

<span data-ttu-id="a9e12-130">上記の Html.DropDownList() ヘルパー メソッドは、2 つのパラメーターを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="a9e12-130">The Html.DropDownList() helper method above takes two parameters.</span></span> <span data-ttu-id="a9e12-131">最初は、出力を HTML フォーム要素の名前です。</span><span class="sxs-lookup"><span data-stu-id="a9e12-131">The first is the name of the HTML form element to output.</span></span> <span data-ttu-id="a9e12-132">2 つ目は、それは ViewData 辞書を使用して渡された"SelectList"モデルです。</span><span class="sxs-lookup"><span data-stu-id="a9e12-132">The second is the "SelectList" model we passed via the ViewData dictionary.</span></span> <span data-ttu-id="a9e12-133">使用、c#"キーワード as"を SelectList としてディクショナリ内で型をキャストします。</span><span class="sxs-lookup"><span data-stu-id="a9e12-133">We are using the C# "as" keyword to cast the type within the dictionary as a SelectList.</span></span>

<span data-ttu-id="a9e12-134">今すぐ実行するとき、アプリケーションとアクセスと、 */Dinners/Edit/1* URL、ブラウザー内で見て、テキスト ボックスの代わりに国の dropdownlist を表示、編集 UI が更新されたこと。</span><span class="sxs-lookup"><span data-stu-id="a9e12-134">And now when we run our application and access the */Dinners/Edit/1* URL within our browser we'll see that our edit UI has been updated to display a dropdownlist of countries instead of a textbox:</span></span>

![](use-viewdata-and-implement-viewmodel-classes/_static/image1.png)

<span data-ttu-id="a9e12-135">また、HTTP POST メソッドの編集 (でエラーが発生したときに使用する場合) から、テンプレートの編集ビューをレンダリングしますので、エラーのシナリオで、ビュー テンプレートが表示されるときに、SelectList を ViewData に追加するには、このメソッドも更新するかどうかを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a9e12-135">Because we also render the Edit view template from the HTTP-POST Edit method (in scenarios when errors occur), we'll want to make sure that we also update this method to add the SelectList to ViewData when the view template is rendered in error scenarios:</span></span>

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample3.cs)]

<span data-ttu-id="a9e12-136">これで、DinnersController 編集シナリオが DropDownList をサポートしています。</span><span class="sxs-lookup"><span data-stu-id="a9e12-136">And now our DinnersController edit scenario supports a DropDownList.</span></span>

### <a name="using-a-viewmodel-pattern"></a><span data-ttu-id="a9e12-137">ViewModel パターンを使用します。</span><span class="sxs-lookup"><span data-stu-id="a9e12-137">Using a ViewModel Pattern</span></span>

<span data-ttu-id="a9e12-138">ViewData 辞書のアプローチでは、非常に高速で簡単に実装されているのメリットがあります。</span><span class="sxs-lookup"><span data-stu-id="a9e12-138">The ViewData dictionary approach has the benefit of being fairly fast and easy to implement.</span></span> <span data-ttu-id="a9e12-139">一部の開発者が気文字列ベースのディクショナリを使用して、入力ミスがコンパイル時にキャッチされず、エラー発生する可能性があるためです。</span><span class="sxs-lookup"><span data-stu-id="a9e12-139">Some developers don't like using string-based dictionaries, though, since typos can lead to errors that will not be caught at compile-time.</span></span> <span data-ttu-id="a9e12-140">型指定されていない ViewData 辞書は、"as"演算子を使用して、またはキャスト ビュー テンプレートで c# のような厳密に型指定された言語を使用する場合にも必要です。</span><span class="sxs-lookup"><span data-stu-id="a9e12-140">The un-typed ViewData dictionary also requires using the "as" operator or casting when using a strongly-typed language like C# in a view template.</span></span>

<span data-ttu-id="a9e12-141">使用して別のアプローチでは、"ViewModel"パターンと呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="a9e12-141">An alternative approach that we could use is one often referred to as the "ViewModel" pattern.</span></span> <span data-ttu-id="a9e12-142">このパターンを使用する場合、特定のビューのシナリオ用に最適化されたと、ビュー テンプレートで必要な動的値/コンテンツ プロパティを公開する厳密に型指定されたクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="a9e12-142">When using this pattern we create strongly-typed classes that are optimized for our specific view scenarios, and which expose properties for the dynamic values/content needed by our view templates.</span></span> <span data-ttu-id="a9e12-143">コント ローラー クラスを設定し、使用するには、このビュー テンプレートにこれらのビューに最適化されたクラスを渡します。</span><span class="sxs-lookup"><span data-stu-id="a9e12-143">Our controller classes can then populate and pass these view-optimized classes to our view template to use.</span></span> <span data-ttu-id="a9e12-144">これにより、タイプ セーフ、コンパイル時のチェック、およびビュー テンプレート内のエディターの intellisense。</span><span class="sxs-lookup"><span data-stu-id="a9e12-144">This enables type-safety, compile-time checking, and editor intellisense within view templates.</span></span>

<span data-ttu-id="a9e12-145">たとえば、厳密に型指定された 2 つのプロパティを公開する dinner フォーム"DinnerFormViewModel"を作成編集シナリオは次のようなクラスを有効にする: Dinner オブジェクト、および国の dropdownlist を設定するために必要な SelectList モデル。</span><span class="sxs-lookup"><span data-stu-id="a9e12-145">For example, to enable dinner form editing scenarios we can create a "DinnerFormViewModel" class like below that exposes two strongly-typed properties: a Dinner object, and the SelectList model needed to populate the countries dropdownlist:</span></span>

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample4.cs)]

<span data-ttu-id="a9e12-146">リポジトリから取得する Dinner オブジェクトを使用して DinnerFormViewModel を作成する、Edit() アクション メソッドを更新して、ビュー テンプレートに渡します。</span><span class="sxs-lookup"><span data-stu-id="a9e12-146">We can then update our Edit() action method to create the DinnerFormViewModel using the Dinner object we retrieve from our repository, and then pass it to our view template:</span></span>

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample5.cs)]

<span data-ttu-id="a9e12-147">よう edit.aspx ページの上部にある"inherits"属性を変更することでオブジェクトのため、その it"Dinner"ではなく"DinnerFormViewModel"が必要ですが、ビュー テンプレートを更新し、作成します。</span><span class="sxs-lookup"><span data-stu-id="a9e12-147">We'll then update our view template so that it expects a "DinnerFormViewModel" instead of a "Dinner" object by changing the "inherits" attribute at the top of the edit.aspx page like so:</span></span>

[!code-cshtml[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample6.cshtml)]

<span data-ttu-id="a9e12-148">このビュー テンプレート内の"Model"プロパティの intellisense を渡している DinnerFormViewModel 型のオブジェクト モデルを反映するように更新されます。</span><span class="sxs-lookup"><span data-stu-id="a9e12-148">Once we do this, the intellisense of the "Model" property within our view template will be updated to reflect the object model of the DinnerFormViewModel type we are passing it:</span></span>

![](use-viewdata-and-implement-viewmodel-classes/_static/image2.png)

![](use-viewdata-and-implement-viewmodel-classes/_static/image3.png)

<span data-ttu-id="a9e12-149">これを使用するコードの表示、私たちを更新できます。</span><span class="sxs-lookup"><span data-stu-id="a9e12-149">We can then update our view code to work off of it.</span></span> <span data-ttu-id="a9e12-150">ここでは作成方法入力要素の名前を変更しない次の通知 (フォーム要素がある名前は"Title"、「国」) – DinnerFormViewModel クラスを使用して値を取得する HTML ヘルパー メソッドを更新していますが。</span><span class="sxs-lookup"><span data-stu-id="a9e12-150">Notice below how we are not changing the names of the input elements we are creating (the form elements will still be named "Title", "Country") – but we are updating the HTML Helper methods to retrieve the values using the DinnerFormViewModel class:</span></span>

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample7.aspx)]

<span data-ttu-id="a9e12-151">エラーを表示するときに、DinnerFormViewModel クラスを使用して、編集の post メソッドも更新します。</span><span class="sxs-lookup"><span data-stu-id="a9e12-151">We'll also update our Edit post method to use the DinnerFormViewModel class when rendering errors:</span></span>

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample8.cs)]

<span data-ttu-id="a9e12-152">今後も更新できる、Create() のアクション メソッドを再利用、正確な同じ*DinnerFormViewModel* DropDownList 内も国を有効にするクラス。</span><span class="sxs-lookup"><span data-stu-id="a9e12-152">We can also update our Create() action methods to re-use the exact same *DinnerFormViewModel* class to enable the countries DropDownList within those as well.</span></span> <span data-ttu-id="a9e12-153">HTTP GET の実装を次に示します。</span><span class="sxs-lookup"><span data-stu-id="a9e12-153">Below is the HTTP-GET implementation:</span></span>

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample9.cs)]

<span data-ttu-id="a9e12-154">作成する HTTP POST メソッドの実装を次に示します。</span><span class="sxs-lookup"><span data-stu-id="a9e12-154">Below is the implementation of the HTTP-POST Create method:</span></span>

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample10.cs)]

<span data-ttu-id="a9e12-155">国を選択するため、両方の編集および作成画面がドロップダウン リストをサポートするようになりました。</span><span class="sxs-lookup"><span data-stu-id="a9e12-155">And now both our Edit and Create screens support drop-downlists for picking the country.</span></span>

### <a name="custom-shaped-viewmodel-classes"></a><span data-ttu-id="a9e12-156">カスタム型の ViewModel クラス</span><span class="sxs-lookup"><span data-stu-id="a9e12-156">Custom-shaped ViewModel classes</span></span>

<span data-ttu-id="a9e12-157">上記のシナリオでは、DinnerFormViewModel クラスは SelectList モデル プロパティのサポートと共に、プロパティとして Dinner のモデル オブジェクトを直接公開します。</span><span class="sxs-lookup"><span data-stu-id="a9e12-157">In the scenario above, our DinnerFormViewModel class directly exposes the Dinner model object as a property, along with a supporting SelectList model property.</span></span> <span data-ttu-id="a9e12-158">この方法で正しく動作のシナリオで、ビュー テンプレートで作成する HTML UI に対応して比較的密接にドメイン モデル オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="a9e12-158">This approach works fine for scenarios where the HTML UI we want to create within our view template corresponds relatively closely to our domain model objects.</span></span>

<span data-ttu-id="a9e12-159">シナリオではない場合は、使用できる 1 つのオプションではビュー – によって消費のオブジェクト モデルがより最適化されており、基になるドメイン モデル オブジェクトとはまったく異なるになりますにカスタムの形 ViewModel クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="a9e12-159">For scenarios where this isn't the case, one option that you can use is to create a custom-shaped ViewModel class whose object model is more optimized for consumption by the view – and which might look completely different from the underlying domain model object.</span></span> <span data-ttu-id="a9e12-160">たとえば、さまざまなプロパティ名や複数のモデル オブジェクトから収集した集計のプロパティを公開、でした可能性があります。</span><span class="sxs-lookup"><span data-stu-id="a9e12-160">For example, it could potentially expose different property names and/or aggregate properties collected from multiple model objects.</span></span>

<span data-ttu-id="a9e12-161">カスタム型の ViewModel クラスは、両方をコント ローラーからビューをレンダリングするだけでなく、コント ローラーのアクション メソッドにポストバックされたフォーム データの処理に役立つデータを渡すために使用します。</span><span class="sxs-lookup"><span data-stu-id="a9e12-161">Custom-shaped ViewModel classes can be used both to pass data from controllers to views to render, as well as to help handle form data posted back to a controller's action method.</span></span> <span data-ttu-id="a9e12-162">この後のシナリオでは、アクション メソッドで、フォーム ポストされたデータを ViewModel オブジェクトを更新し、ViewModel インスタンスを使用して、マップまたは実際のドメイン モデル オブジェクトを取得するがあります。</span><span class="sxs-lookup"><span data-stu-id="a9e12-162">For this later scenario, you might have the action method update a ViewModel object with the form-posted data, and then use the ViewModel instance to map or retrieve an actual domain model object.</span></span>

<span data-ttu-id="a9e12-163">ViewModel クラスのカスタムの形では、大量の柔軟性を指定できるほかは複雑すぎる取得を開始しています、アクション メソッド内で、テンプレートの表示またはフォーム ポスト コード内でレンダリング コードを検索する任意の時点を調査するものです。</span><span class="sxs-lookup"><span data-stu-id="a9e12-163">Custom-shaped ViewModel classes can provide a great deal of flexibility, and are something to investigate any time you find the rendering code within your view templates or the form-posting code inside your action methods starting to get too complicated.</span></span> <span data-ttu-id="a9e12-164">これは、多くの場合、記号、ドメイン モデルが生成すると、UI に対応していないクリーンにして、カスタムの形の中間の ViewModel クラスが役立つのです。</span><span class="sxs-lookup"><span data-stu-id="a9e12-164">This is often a sign that your domain models don't cleanly correspond to the UI you are generating, and that an intermediate custom-shaped ViewModel class can help.</span></span>

### <a name="next-step"></a><span data-ttu-id="a9e12-165">次の手順</span><span class="sxs-lookup"><span data-stu-id="a9e12-165">Next Step</span></span>

<span data-ttu-id="a9e12-166">これで、パーシャルとマスター ページを使用すると、再利用し、アプリケーション全体で UI を共有した方法を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="a9e12-166">Let's now look at how we can use partials and master-pages to re-use and share UI across our application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a9e12-167">[前へ](provide-crud-create-read-update-delete-data-form-entry-support.md)
> [次へ](re-use-ui-using-master-pages-and-partials.md)</span><span class="sxs-lookup"><span data-stu-id="a9e12-167">[Previous](provide-crud-create-read-update-delete-data-form-entry-support.md)
[Next](re-use-ui-using-master-pages-and-partials.md)</span></span>
