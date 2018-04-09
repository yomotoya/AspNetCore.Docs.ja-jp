---
uid: mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
title: マスター ページとパーシャルを使用して UI を再利用 |Microsoft ドキュメント
author: microsoft
description: 手順 7 では、部分ビュー テンプレートおよびマスター ページを使用して、コードの重複を除去するには、当社ビュー テンプレート内で 'ドライ原則' を適用した方法で検索します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: d4243a4a-e91c-4116-9ae0-5c08e5285677
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
msc.type: authoredcontent
ms.openlocfilehash: ade655f3a4a429360b678d02fb564ac9cf255d42
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="re-use-ui-using-master-pages-and-partials"></a><span data-ttu-id="5c8c2-103">マスター ページとパーシャルを使用して UI を再利用します。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-103">Re-use UI Using Master Pages and Partials</span></span>
====================
<span data-ttu-id="5c8c2-104">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="5c8c2-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="5c8c2-105">PDF をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="5c8c2-106">これは、無料の手順 7 ["NerdDinner"アプリケーションのチュートリアル](introducing-the-nerddinner-tutorial.md)をウォーク-にする方法を小規模の構築が完了すると、ASP.NET MVC 1 を使用して web アプリケーションです。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-106">This is step 7 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="5c8c2-107">手順 7 では、部分ビュー テンプレートおよびマスター ページを使用して、コードの重複を除去するには、当社ビュー テンプレート内で「ドライ原則」を適用した方法で検索します。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-107">Step 7 looks at ways we can apply the "DRY Principle" within our view templates to eliminate code duplication, using partial view templates and master pages.</span></span>
> 
> <span data-ttu-id="5c8c2-108">ASP.NET MVC 3 を使用している場合ことをお勧めする、 [MVC 3 の開始と取得](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)または[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)チュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-7-partials-and-master-pages"></a><span data-ttu-id="5c8c2-109">NerdDinner 手順 7: パーシャルとマスター ページ</span><span class="sxs-lookup"><span data-stu-id="5c8c2-109">NerdDinner Step 7: Partials and Master Pages</span></span>

<span data-ttu-id="5c8c2-110">ASP.NET MVC 持ち込デザインの理念の 1 つは、「はいない繰り返さない」原則 (「ドライ」とも呼ばれます) です。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-110">One of the design philosophies ASP.NET MVC embraces is the "Do Not Repeat Yourself" principle (commonly referred to as "DRY").</span></span> <span data-ttu-id="5c8c2-111">ドライ デザインは、コードおよびアプリケーションを作成する迅速かつ容易に管理するため、最終的にはロジックの重複を解消します。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-111">A DRY design helps eliminate the duplication of code and logic, which ultimately makes applications faster to build and easier to maintain.</span></span>

<span data-ttu-id="5c8c2-112">NerdDinner シナリオのいくつか適用性、既に説明しました。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-112">We've already seen the DRY principle applied in several of our NerdDinner scenarios.</span></span> <span data-ttu-id="5c8c2-113">いくつかの例: 両方の編集に適用され、コント ローラーでシナリオを作成できるように、モデル レイヤー内で、検証ロジックを実装再を使用する、"NotFound"のビュー テンプレート メソッド間で、編集、詳細、および Delete アクションです。View() ヘルパー メソッドを呼び出すと、名前を明示的に指定する必要があるビュー テンプレートを取得するには、規則の名前付けパターンを使用します。両方の編集用 DinnerFormViewModel クラスを再利用し、アクションのシナリオを作成します。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-113">A few examples: our validation logic is implemented within our model layer, which enables it to be enforced across both edit and create scenarios in our controller; we are re-using the "NotFound" view template across the Edit, Details and Delete action methods; we are using a convention- naming pattern with our view templates, which eliminates the need to explicitly specify the name when we call the View() helper method; and we are re-using the DinnerFormViewModel class for both Edit and Create action scenarios.</span></span>

<span data-ttu-id="5c8c2-114">今すぐ方法を見てみましょう「ドライ原則」を適用したことができますもあるコードの重複を排除するには、当社ビュー テンプレート内で。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-114">Let's now look at ways we can apply the "DRY Principle" within our view templates to eliminate code duplication there as well.</span></span>

### <a name="re-visiting-our-edit-and-create-view-templates"></a><span data-ttu-id="5c8c2-115">再度、編集を訪問し、テンプレートの表示を作成します。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-115">Re-visiting our Edit and Create View Templates</span></span>

<span data-ttu-id="5c8c2-116">現在は 2 つの異なるビュー テンプレート –"Edit.aspx"と"Create.aspx"– 使用 Dinner フォーム UI を表示します。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-116">Currently we are using two different view templates – "Edit.aspx" and "Create.aspx" – to display our Dinner form UI.</span></span> <span data-ttu-id="5c8c2-117">これらのビジュアル簡単に比較するは、類似性が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-117">A quick visual comparison of them highlights how similar they are.</span></span> <span data-ttu-id="5c8c2-118">作成するフォームの外観を次に示します。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-118">Below is what the create form looks like:</span></span>

![](re-use-ui-using-master-pages-and-partials/_static/image1.png)

<span data-ttu-id="5c8c2-119">"Edit"フォームは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-119">And here is what our "Edit" form looks like:</span></span>

![](re-use-ui-using-master-pages-and-partials/_static/image2.png)

<span data-ttu-id="5c8c2-120">異なる量はありますか。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-120">Not much of a difference is there?</span></span> <span data-ttu-id="5c8c2-121">タイトルとヘッダーの文字列以外は、フォームのレイアウトおよび入力コントロールが同じです。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-121">Other than the title and header text, the form layout and input controls are identical.</span></span>

<span data-ttu-id="5c8c2-122">ビューのテンプレートを検索します"Edit.aspx"と"Create.aspx"を開く場合と同じフォームのレイアウトおよび入力コントロールのコードが含まれます。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-122">If we open up the "Edit.aspx" and "Create.aspx" view templates we'll find that they contain identical form layout and input control code.</span></span> <span data-ttu-id="5c8c2-123">この重複は最終的におを導入したり、適切ではない - 新しい Dinner プロパティを変更するたびに 2 回変更することを意味します。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-123">This duplication means we end up having to make changes twice anytime we introduce or change a new Dinner property - which is not good.</span></span>

### <a name="using-partial-view-templates"></a><span data-ttu-id="5c8c2-124">部分ビューのテンプレートを使用します。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-124">Using Partial View Templates</span></span>

<span data-ttu-id="5c8c2-125">ASP.NET MVC には、ページのサブ部分ビューのレンダリング ロジックをカプセル化に使用できる「部分的なビュー」テンプレートを定義する機能がサポートされています。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-125">ASP.NET MVC supports the ability to define "partial view" templates that can be used to encapsulate view rendering logic for a sub-portion of a page.</span></span> <span data-ttu-id="5c8c2-126">「パーシャル」1 回、ビューのレンダリング ロジックを定義する便利な方法を提供し、再で使用して複数の場所、アプリケーション全体でします。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-126">"Partials" provide a useful way to define view rendering logic once, and then re-use it in multiple places across an application.</span></span>

<span data-ttu-id="5c8c2-127">「ドライ アップ」、Edit.aspx および Create.aspx ビュー テンプレートの複製のため、フォームのレイアウトと両方に共通の入力要素をカプセル化"DinnerForm.ascx"をという名前の部分ビュー テンプレートを作成できます。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-127">To help "DRY-up" our Edit.aspx and Create.aspx view template duplication, we can create a partial view template named "DinnerForm.ascx" that encapsulates the form layout and input elements common to both.</span></span> <span data-ttu-id="5c8c2-128">それでは、当社/ビュー/ディナー ディレクトリを右クリックして を選択して、"追加-&gt;ビュー"メニュー コマンド。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-128">We'll do this by right-clicking on our /Views/Dinners directory and choosing the "Add-&gt;View" menu command:</span></span>

![](re-use-ui-using-master-pages-and-partials/_static/image3.png)

<span data-ttu-id="5c8c2-129">これにより、「ビューの追加」ダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-129">This will display the "Add View" dialog.</span></span> <span data-ttu-id="5c8c2-130">新しいビューお"DinnerForm"を作成、ダイアログ ボックスに「部分ビューの作成」チェック ボックスをオンにしてを渡していること、DinnerFormViewModel クラスを示すという名前を。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-130">We'll name the new view we want to create "DinnerForm", select the "Create a partial view" checkbox within the dialog, and indicate that we will pass it a DinnerFormViewModel class:</span></span>

![](re-use-ui-using-master-pages-and-partials/_static/image4.png)

<span data-ttu-id="5c8c2-131">「追加」ボタンをクリックすると Visual Studio は新しい"DinnerForm.ascx"ビュー テンプレートを"\Views\Dinners"ディレクトリ内でご利用の米国作成します。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-131">When we click the "Add" button, Visual Studio will create a new "DinnerForm.ascx" view template for us within the "\Views\Dinners" directory.</span></span>

<span data-ttu-id="5c8c2-132">おできますし、コピー/貼り付け、重複するフォームのレイアウトを新しい"DinnerForm.ascx"部分ビュー テンプレートに、Edit.aspx/Create.aspx ビュー テンプレートからのコントロールのコードを入力/。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-132">We can then copy/paste the duplicate form layout / input control code from our Edit.aspx/ Create.aspx view templates into our new "DinnerForm.ascx" partial view template:</span></span>

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample1.aspx)]

<span data-ttu-id="5c8c2-133">編集および作成するテンプレートの表示、DinnerForm 部分的なテンプレートを呼び出すし、フォームの重複を排除するし、更新することができます。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-133">We can then update our Edit and Create view templates to call the DinnerForm partial template and eliminate the form duplication.</span></span> <span data-ttu-id="5c8c2-134">We can do this by calling Html.RenderPartial("DinnerForm") within our view templates:</span><span class="sxs-lookup"><span data-stu-id="5c8c2-134">We can do this by calling Html.RenderPartial("DinnerForm") within our view templates:</span></span>

##### <a name="createaspx"></a><span data-ttu-id="5c8c2-135">Create.aspx</span><span class="sxs-lookup"><span data-stu-id="5c8c2-135">Create.aspx</span></span>

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample2.aspx)]

##### <a name="editaspx"></a><span data-ttu-id="5c8c2-136">Edit.aspx</span><span class="sxs-lookup"><span data-stu-id="5c8c2-136">Edit.aspx</span></span>

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample3.aspx)]

<span data-ttu-id="5c8c2-137">Html.RenderPartial を呼び出すときに使用する部分的なテンプレートのパスを明示的に修飾することができます (例: ~ Views/Dinners/DinnerForm.ascx") です。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-137">You can explicitly qualify the path of the partial template you want when calling Html.RenderPartial (for example: ~Views/Dinners/DinnerForm.ascx").</span></span> <span data-ttu-id="5c8c2-138">ただし、コードでは、上、ASP.NET MVC 内の規則に基づく名前付けパターンの利用と表示するために一部の名前として"DinnerForm"を指定するだけがおです。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-138">In our code above, though, we are taking advantage of the convention-based naming pattern within ASP.NET MVC, and just specifying "DinnerForm" as the name of the partial to render.</span></span> <span data-ttu-id="5c8c2-139">この作業を行うときに ASP.NET MVC ビューの規約に基づくディレクトリに最初 (を検索 DinnersController/ビュー/ディナーとなります)。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-139">When we do this ASP.NET MVC will look first in the convention-based views directory (for DinnersController this would be /Views/Dinners).</span></span> <span data-ttu-id="5c8c2-140">テンプレートの部分的なが見つからない場合がありますは探します、/Views/Shared ディレクトリにします。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-140">If it doesn't find the partial template there it will then look for it in the /Views/Shared directory.</span></span>

<span data-ttu-id="5c8c2-141">Html.RenderPartial() は部分ビューの名前だけで呼び出されると、ASP.NET MVC に渡されます部分ビュー、同じモデルと ViewData 辞書オブジェクト、呼び出し元のビュー テンプレートで使用されます。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-141">When Html.RenderPartial() is called with just the name of the partial view, ASP.NET MVC will pass to the partial view the same Model and ViewData dictionary objects used by the calling view template.</span></span> <span data-ttu-id="5c8c2-142">代わりに、オーバー ロードされたバージョンがの Html.RenderPartial()、別のモデル オブジェクトや、部分ビューを使用するための ViewData ディクショナリを渡すことができるようにします。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-142">Alternatively, there are overloaded versions of Html.RenderPartial() that enable you to pass an alternate Model object and/or ViewData dictionary for the partial view to use.</span></span> <span data-ttu-id="5c8c2-143">これは、のみしようとする場合にフル モデル/ViewModel のサブセットを渡すに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-143">This is useful for scenarios where you only want to pass a subset of the full Model/ViewModel.</span></span>

| <span data-ttu-id="5c8c2-144">**側トピックは、なぜ&lt;%%&gt;の代わりに&lt;% = %&gt;しますか?**</span><span class="sxs-lookup"><span data-stu-id="5c8c2-144">**Side Topic: Why &lt;% %&gt; instead of &lt;%= %&gt;?**</span></span> |
| --- |
| <span data-ttu-id="5c8c2-145">使用されていることに気付きます上記のコードで軽度のいずれかが、 &lt;%%&gt;ブロックの代わりに、 &lt;% = %&gt; Html.RenderPartial() を呼び出す場合は、ブロックします。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-145">One of the subtle things you might have noticed with the code above is that we are using a &lt;% %&gt; block instead of a &lt;%= %&gt; block when calling Html.RenderPartial().</span></span> <span data-ttu-id="5c8c2-146">&lt;% = %&gt;開発者が、指定した値を表示するためには、ASP.NET でのブロックを示す (例: &lt;% =「こんにちは」%&gt; 「こんにちは」と表示)。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-146">&lt;%= %&gt; blocks in ASP.NET indicate that a developer wants to render a specified value (for example: &lt;%= "Hello" %&gt; would render "Hello").</span></span> <span data-ttu-id="5c8c2-147">&lt;%%&gt;ブロックを示す代わりに、開発者がコードを実行する場合、そのいずれかによって、それらに含まれる出力が表示されることし、明示的に行う必要があります (例: &lt;%response.write("hello")&gt;です。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-147">&lt;% %&gt; blocks instead indicate that the developer wants to execute code, and that any rendered output within them must be done explicitly (for example: &lt;% Response.Write("Hello") %&gt;.</span></span> <span data-ttu-id="5c8c2-148">使用する理由、 &lt;%%&gt;上記 Html.RenderPartial コード ブロックでは Html.RenderPartial() メソッドは、文字列を返さないし、代わりに、呼び出し元のビュー テンプレートに直接コンテンツのストリームの出力を出力します。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-148">The reason we are using a &lt;% %&gt; block with our Html.RenderPartial code above is because the Html.RenderPartial() method doesn't return a string, and instead outputs the content directly to the calling view template's output stream.</span></span> <span data-ttu-id="5c8c2-149">これは、パフォーマンス効率向上のため、により、一時的な (場合によっては非常に大きい可能性があります) の文字列オブジェクトを作成する必要性を回避できます。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-149">It does this for performance efficiency reasons, and by doing so it avoids the need to create a (potentially very large) temporary string object.</span></span> <span data-ttu-id="5c8c2-150">これにより、メモリ使用率を削減し、アプリケーション全体のスループットを向上させます。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-150">This reduces memory usage and improves overall application throughput.</span></span> <span data-ttu-id="5c8c2-151">1 つの一般的なミス内にある場合、呼び出しの最後に、セミコロンを追加することを忘れがちを Html.RenderPartial() を使用する場合、 &lt;%%&gt;ブロックします。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-151">One common mistake when using Html.RenderPartial() is to forget to add a semi-colon at the end of the call when it is within a &lt;% %&gt; block.</span></span> <span data-ttu-id="5c8c2-152">たとえば、このコードでは、コンパイラ エラーが発生: &lt;%html.renderpartial("dinnerform")&gt;代わりに記述する必要があります: &lt;Html.RenderPartial("DinnerForm"); %library;&gt;これは、ため&lt;%%&gt;ブロックは、自己完結型のコード ステートメント (C#) ステートメントをセミコロンで終了する必要のあるコードを使用する場合とします。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-152">For example, this code will cause a compiler error: &lt;% Html.RenderPartial("DinnerForm") %&gt; You instead need to write: &lt;% Html.RenderPartial("DinnerForm"); %&gt; This is because &lt;% %&gt; blocks are self-contained code statements, and when using C# code statements need to be terminated with a semi-colon.</span></span> |

### <a name="using-partial-view-templates-to-clarify-code"></a><span data-ttu-id="5c8c2-153">部分ビューのテンプレートを使用したコードを明確にします。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-153">Using Partial View Templates to Clarify Code</span></span>

<span data-ttu-id="5c8c2-154">複数の場所でビューのレンダリング ロジックと重複しないように、"DinnerForm"部分ビューのテンプレートを作成しました。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-154">We created the "DinnerForm" partial view template to avoid duplicating view rendering logic in multiple places.</span></span> <span data-ttu-id="5c8c2-155">これは、部分ビューのテンプレートを作成する最も一般的な理由です。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-155">This is the most common reason to create partial view templates.</span></span>

<span data-ttu-id="5c8c2-156">場合によってまだ理にかなってを 1 か所でのみ呼び出すされている場合でも、部分ビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-156">Sometimes it still makes sense to create partial views even when they are only being called in a single place.</span></span> <span data-ttu-id="5c8c2-157">テンプレートの表示が非常に複雑な多くの場合がの場合、ビューのレンダリング ロジック抽出し 1 つにパーティション分割またはよりも名前付き部分のテンプレートを読みやすくなります。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-157">Very complicated view templates can often become much easier to read when their view rendering logic is extracted and partitioned into one or more well named partial templates.</span></span>

<span data-ttu-id="5c8c2-158">たとえば、(ここでする検索は間もなく) をプロジェクトに、Site.master ファイルからのコード スニペットの下。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-158">For example, consider the below code-snippet from the Site.master file in our project (which we will be looking at shortly).</span></span> <span data-ttu-id="5c8c2-159">コードが比較的簡単を読み取る – 一部のログインとログアウトを表示するためのロジックが上部にあるリンクは、画面の右側は"LogOnUserControl"部分内にカプセル化します。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-159">The code is relatively straight-forward to read – partly because the logic to display a login/logout link at the top right of the screen is encapsulated within the "LogOnUserControl" partial:</span></span>

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample4.aspx)]

<span data-ttu-id="5c8c2-160">混乱気づいたらされるたびにしようとしてビュー テンプレート内の html/コード マークアップを理解を検討してくださいかどうかしなければならない場合は抽出され、適切な名前の部分ビューへのリファクタリングの一部がより明確です。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-160">Whenever you find yourself getting confused trying to understand the html/code markup within a view-template, consider whether it wouldn't be clearer if some of it was extracted and refactored into well-named partial views.</span></span>

### <a name="master-pages"></a><span data-ttu-id="5c8c2-161">マスター ページ</span><span class="sxs-lookup"><span data-stu-id="5c8c2-161">Master Pages</span></span>

<span data-ttu-id="5c8c2-162">ASP.NET MVC には、部分ビューをサポートしているだけでなく、サイトの最上位の html および一般的なレイアウトを定義するために使用する「マスター ページ」テンプレートを作成する機能もサポートしています。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-162">In addition to supporting partial views, ASP.NET MVC also supports the ability to create "master page" templates that can be used to define the common layout and top-level html of a site.</span></span> <span data-ttu-id="5c8c2-163">コンテンツの上書きまたはビューによって「設定」できる置き換え可能な領域を識別するマスター ページにコントロールを追加することができますし、プレース ホルダーです。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-163">Content placeholder controls can then be added to the master page to identify replaceable regions that can be overridden or "filled in" by views.</span></span> <span data-ttu-id="5c8c2-164">これにより、非常に効果的な (およびドライ) をアプリケーション全体で共通のレイアウトを適用します。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-164">This provides a very effective (and DRY) way to apply a common layout across an application.</span></span>

<span data-ttu-id="5c8c2-165">新しい ASP.NET MVC プロジェクトでは既定では、それらに自動的に追加のマスター ページ テンプレートが存在します。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-165">By default, new ASP.NET MVC projects have a master page template automatically added to them.</span></span> <span data-ttu-id="5c8c2-166">このマスター ページは、"Site.master"および \Views\Shared\ フォルダー内での生活にという名前は。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-166">This master page is named "Site.master" and lives within the \Views\Shared\ folder:</span></span>

![](re-use-ui-using-master-pages-and-partials/_static/image5.png)

<span data-ttu-id="5c8c2-167">既定の Site.master ファイルは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-167">The default Site.master file looks like below.</span></span> <span data-ttu-id="5c8c2-168">上部のナビゲーション メニューと共に、サイトの外部の html を定義します。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-168">It defines the outer html of the site, along with a menu for navigation at the top.</span></span> <span data-ttu-id="5c8c2-169">2 つの置き換え可能コンテンツ プレース ホルダー コントロール – タイトル、およびその他のページの主要なコンテンツを置き換える必要がありますのいずれかが含まれています。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-169">It contains two replaceable content placeholder controls – one for the title, and the other for where the primary content of a page should be replaced:</span></span>

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample5.aspx)]

<span data-ttu-id="5c8c2-170">NerdDinner アプリケーション ("List"、「詳細」、"Edit"、「作成」、"NotFound"など) を作成しましたビュー テンプレートのすべては、この Site.master テンプレートに基づくされています。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-170">All of the view templates we've created for our NerdDinner application ("List", "Details", "Edit", "Create", "NotFound", etc) have been based on this Site.master template.</span></span> <span data-ttu-id="5c8c2-171">これは、最上位に既定で追加された"MasterPageFile"属性を使用して表示&lt;% ページ % @&gt;ディレクティブ「ビューの追加」ダイアログを使用して、ビューを作成したとき。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-171">This is indicated via the "MasterPageFile" attribute that was added by default to the top &lt;% @ Page %&gt; directive when we created our views using the "Add View" dialog:</span></span>

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample6.aspx)]

<span data-ttu-id="5c8c2-172">これが意味であり、Site.master コンテンツを変更することができますが、変更に自動的に適用し、これらのビューのテンプレートのいずれかのレンダリング時に使用します。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-172">What this means is that we can change the Site.master content, and have the changes automatically be applied and used when we render any of our view templates.</span></span>

<span data-ttu-id="5c8c2-173">アプリケーションのヘッダーが"My MVC Application"ではなく"NerdDinner"ようにしましょう、Site.master のヘッダー セクションを更新します。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-173">Let's update our Site.master's header section so that the header of our application is "NerdDinner" instead of "My MVC Application".</span></span> <span data-ttu-id="5c8c2-174">みましょうも更新して、ナビゲーション メニューように最初のタブは、「を検索、夕食」(HomeController の Index() アクション メソッドによって処理されます)、および"ホストを Dinner"(DinnersController の Create() アクション メソッドによって処理されます) と呼ばれる新しいタブを追加してみましょう。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-174">Let's also update our navigation menu so that the first tab is "Find a Dinner" (handled by the HomeController's Index() action method), and let's add a new tab called "Host a Dinner" (handled by the DinnersController's Create() action method):</span></span>

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample7.aspx)]

<span data-ttu-id="5c8c2-175">Site.master ファイルと更新を保存して、ブラウザー、ヘッダーを見てみましょうは、アプリケーション内のすべてのビューで表示を変更します。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-175">When we save the Site.master file and refresh our browser we'll see our header changes show up across all views within our application.</span></span> <span data-ttu-id="5c8c2-176">例えば:</span><span class="sxs-lookup"><span data-stu-id="5c8c2-176">For example:</span></span>

![](re-use-ui-using-master-pages-and-partials/_static/image6.png)

<span data-ttu-id="5c8c2-177">使用して、 */Dinners/編集/[id]* URL:</span><span class="sxs-lookup"><span data-stu-id="5c8c2-177">And with the */Dinners/Edit/[id]* URL:</span></span>

![](re-use-ui-using-master-pages-and-partials/_static/image7.png)

### <a name="next-step"></a><span data-ttu-id="5c8c2-178">次の手順</span><span class="sxs-lookup"><span data-stu-id="5c8c2-178">Next Step</span></span>

<span data-ttu-id="5c8c2-179">パーシャルおよびマスター ページは、クリーンにビューを整理する非常に柔軟なオプションを提供します。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-179">Partials and master pages provide very flexible options that enable you to cleanly organize views.</span></span> <span data-ttu-id="5c8c2-180">ビューと重複しないようにするのに役立ちますコンテンツ/コードし、テンプレートの表示を容易に読み取りおよびメンテナンスがあります。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-180">You'll find that they help you avoid duplicating view content/ code, and make your view templates easier to read and maintain.</span></span>

<span data-ttu-id="5c8c2-181">みましょう今すぐ前に作成した一覧のシナリオを再確認し、スケーラブルなページングのサポートを有効にします。</span><span class="sxs-lookup"><span data-stu-id="5c8c2-181">Let's now revisit the listing scenario we built earlier and enable scalable paging support.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5c8c2-182">[前へ](use-viewdata-and-implement-viewmodel-classes.md)
> [次へ](implement-efficient-data-paging.md)</span><span class="sxs-lookup"><span data-stu-id="5c8c2-182">[Previous](use-viewdata-and-implement-viewmodel-classes.md)
[Next](implement-efficient-data-paging.md)</span></span>
