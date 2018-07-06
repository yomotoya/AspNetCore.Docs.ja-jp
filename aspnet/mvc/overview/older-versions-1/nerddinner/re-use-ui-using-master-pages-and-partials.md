---
uid: mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
title: マスター ページとパーシャルを使用して UI を再利用 |Microsoft Docs
author: microsoft
description: 手順 7 では、部分ビュー テンプレートおよびマスター ページを使用して、コードの重複を除去する、ビュー テンプレート内で 'DRY 原則' を適用できる方法で検索します。
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: d4243a4a-e91c-4116-9ae0-5c08e5285677
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
msc.type: authoredcontent
ms.openlocfilehash: 961378d6d710c678a0cd223a5c31544f014ace7f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839595"
---
<a name="re-use-ui-using-master-pages-and-partials"></a><span data-ttu-id="e70d3-103">マスター ページとパーシャルを使用して UI を再利用します。</span><span class="sxs-lookup"><span data-stu-id="e70d3-103">Re-use UI Using Master Pages and Partials</span></span>
====================
<span data-ttu-id="e70d3-104">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="e70d3-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="e70d3-105">PDF のダウンロード</span><span class="sxs-lookup"><span data-stu-id="e70d3-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="e70d3-106">これは、無料の手順 7 ["NerdDinner"アプリケーションのチュートリアル](introducing-the-nerddinner-tutorial.md)をウォーク スルーの小さなをビルドしても、ASP.NET MVC 1 を使用して web アプリケーションを実行する方法。</span><span class="sxs-lookup"><span data-stu-id="e70d3-106">This is step 7 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="e70d3-107">手順 7 では、部分ビュー テンプレートおよびマスター ページを使用して、コードの重複を除去する、ビュー テンプレート内で「DRY 原則」適用可能な方法で検索します。</span><span class="sxs-lookup"><span data-stu-id="e70d3-107">Step 7 looks at ways we can apply the "DRY Principle" within our view templates to eliminate code duplication, using partial view templates and master pages.</span></span>
> 
> <span data-ttu-id="e70d3-108">次のことをお勧め ASP.NET MVC 3 を使用している場合、 [MVC 3 の開始と取得](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)または[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)チュートリアル。</span><span class="sxs-lookup"><span data-stu-id="e70d3-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-7-partials-and-master-pages"></a><span data-ttu-id="e70d3-109">NerdDinner 手順 7: パーシャルとマスター ページ</span><span class="sxs-lookup"><span data-stu-id="e70d3-109">NerdDinner Step 7: Partials and Master Pages</span></span>

<span data-ttu-id="e70d3-110">ASP.NET MVC を採用する設計哲学の 1 つは"実行しない Repeat Yourself"原則です (「ドライ」とも呼ばれます)。</span><span class="sxs-lookup"><span data-stu-id="e70d3-110">One of the design philosophies ASP.NET MVC embraces is the "Do Not Repeat Yourself" principle (commonly referred to as "DRY").</span></span> <span data-ttu-id="e70d3-111">コードと、最終的には、アプリケーションを構築する迅速かつ容易に管理するロジックの重複の排除ドライ設計に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="e70d3-111">A DRY design helps eliminate the duplication of code and logic, which ultimately makes applications faster to build and easier to maintain.</span></span>

<span data-ttu-id="e70d3-112">NerdDinner シナリオのいくつか適用 DRY 原則を既に説明しました。</span><span class="sxs-lookup"><span data-stu-id="e70d3-112">We've already seen the DRY principle applied in several of our NerdDinner scenarios.</span></span> <span data-ttu-id="e70d3-113">いくつかの例: 両方の編集に適用され、コント ローラーでシナリオを作成できるように、モデル レイヤー内で、検証ロジックが実装されています。編集、詳細、削除のアクション メソッド間で、"NotFound"のビュー テンプレートを再利用する、View() ヘルパー メソッドを呼び出すと、名前を明示的に指定する必要がなくなりますビュー テンプレートを取得するには、使用規則の名前付けパターンを使用していること両方の編集用 DinnerFormViewModel クラスを再利用はおアクション シナリオを作成します。</span><span class="sxs-lookup"><span data-stu-id="e70d3-113">A few examples: our validation logic is implemented within our model layer, which enables it to be enforced across both edit and create scenarios in our controller; we are re-using the "NotFound" view template across the Edit, Details and Delete action methods; we are using a convention- naming pattern with our view templates, which eliminates the need to explicitly specify the name when we call the View() helper method; and we are re-using the DinnerFormViewModel class for both Edit and Create action scenarios.</span></span>

<span data-ttu-id="e70d3-114">これで方法を見てみましょう「DRY 原則」を適用できますもあるコードの重複を除去する、ビュー テンプレート内で。</span><span class="sxs-lookup"><span data-stu-id="e70d3-114">Let's now look at ways we can apply the "DRY Principle" within our view templates to eliminate code duplication there as well.</span></span>

### <a name="re-visiting-our-edit-and-create-view-templates"></a><span data-ttu-id="e70d3-115">再度、編集にアクセスして、ビュー テンプレートを作成</span><span class="sxs-lookup"><span data-stu-id="e70d3-115">Re-visiting our Edit and Create View Templates</span></span>

<span data-ttu-id="e70d3-116">現在は 2 つの異なるビュー テンプレート –"Edit.aspx"と"Create.aspx"– 使用、Dinner フォーム UI を表示します。</span><span class="sxs-lookup"><span data-stu-id="e70d3-116">Currently we are using two different view templates – "Edit.aspx" and "Create.aspx" – to display our Dinner form UI.</span></span> <span data-ttu-id="e70d3-117">うち visual 簡単に比較するには、どのように類似しているが強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="e70d3-117">A quick visual comparison of them highlights how similar they are.</span></span> <span data-ttu-id="e70d3-118">作成フォームの外観を次に示します。</span><span class="sxs-lookup"><span data-stu-id="e70d3-118">Below is what the create form looks like:</span></span>

![](re-use-ui-using-master-pages-and-partials/_static/image1.png)

<span data-ttu-id="e70d3-119">ここでは、[編集] フォームがどのように.</span><span class="sxs-lookup"><span data-stu-id="e70d3-119">And here is what our "Edit" form looks like:</span></span>

![](re-use-ui-using-master-pages-and-partials/_static/image2.png)

<span data-ttu-id="e70d3-120">ほどの違いはありますか。</span><span class="sxs-lookup"><span data-stu-id="e70d3-120">Not much of a difference is there?</span></span> <span data-ttu-id="e70d3-121">タイトルとヘッダーの文字列以外のフォーム レイアウトと入力コントロールは同じです。</span><span class="sxs-lookup"><span data-stu-id="e70d3-121">Other than the title and header text, the form layout and input controls are identical.</span></span>

<span data-ttu-id="e70d3-122">テンプレートの表示を検索します"Edit.aspx"と"Create.aspx"を開く場合と同じフォームのレイアウトと入力コントロールのコードが含まれます。</span><span class="sxs-lookup"><span data-stu-id="e70d3-122">If we open up the "Edit.aspx" and "Create.aspx" view templates we'll find that they contain identical form layout and input control code.</span></span> <span data-ttu-id="e70d3-123">この重複回避の意味を導入した場合です。 新しい Dinner プロパティの変更またはたびに 2 回変更を行うことができ上がります。</span><span class="sxs-lookup"><span data-stu-id="e70d3-123">This duplication means we end up having to make changes twice anytime we introduce or change a new Dinner property - which is not good.</span></span>

### <a name="using-partial-view-templates"></a><span data-ttu-id="e70d3-124">部分ビュー テンプレートの使用</span><span class="sxs-lookup"><span data-stu-id="e70d3-124">Using Partial View Templates</span></span>

<span data-ttu-id="e70d3-125">ASP.NET MVC では、ページのサブ部分ビューのレンダリング ロジックをカプセル化に使用できる「部分的なビュー」テンプレートを定義する機能をサポートします。</span><span class="sxs-lookup"><span data-stu-id="e70d3-125">ASP.NET MVC supports the ability to define "partial view" templates that can be used to encapsulate view rendering logic for a sub-portion of a page.</span></span> <span data-ttu-id="e70d3-126">「パーシャル」ビューのレンダリング ロジックを 1 回、定義する便利な方法を提供し、再に複数の場所で、アプリケーション全体でします。</span><span class="sxs-lookup"><span data-stu-id="e70d3-126">"Partials" provide a useful way to define view rendering logic once, and then re-use it in multiple places across an application.</span></span>

<span data-ttu-id="e70d3-127">「ドライ アップ」、Edit.aspx と Create.aspx ビュー テンプレートの複製のため、フォームのレイアウトと両方に共通する入力要素をカプセル化する"DinnerForm.ascx"をという名前の部分ビュー テンプレートを作成できます。</span><span class="sxs-lookup"><span data-stu-id="e70d3-127">To help "DRY-up" our Edit.aspx and Create.aspx view template duplication, we can create a partial view template named "DinnerForm.ascx" that encapsulates the form layout and input elements common to both.</span></span> <span data-ttu-id="e70d3-128">これを/ビュー/Dinners ディレクトリを右クリックし、選択は、"追加-&gt;ビュー"メニュー コマンド。</span><span class="sxs-lookup"><span data-stu-id="e70d3-128">We'll do this by right-clicking on our /Views/Dinners directory and choosing the "Add-&gt;View" menu command:</span></span>

![](re-use-ui-using-master-pages-and-partials/_static/image3.png)

<span data-ttu-id="e70d3-129">これにより、「ビューの追加」ダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="e70d3-129">This will display the "Add View" dialog.</span></span> <span data-ttu-id="e70d3-130">という名前を"DinnerForm"を作成するには、ダイアログ ボックスで"部分ビューを作成する チェック ボックスをオンし、私たちが合格する、DinnerFormViewModel クラスを示すために、新しいビュー。</span><span class="sxs-lookup"><span data-stu-id="e70d3-130">We'll name the new view we want to create "DinnerForm", select the "Create a partial view" checkbox within the dialog, and indicate that we will pass it a DinnerFormViewModel class:</span></span>

![](re-use-ui-using-master-pages-and-partials/_static/image4.png)

<span data-ttu-id="e70d3-131">[追加] ボタンをクリックすると、ときに Visual Studio は新しい"DinnerForm.ascx"ビュー テンプレートを"\Views\Dinners"ディレクトリ内を作成します。</span><span class="sxs-lookup"><span data-stu-id="e70d3-131">When we click the "Add" button, Visual Studio will create a new "DinnerForm.ascx" view template for us within the "\Views\Dinners" directory.</span></span>

<span data-ttu-id="e70d3-132">私たちできますし、コピー/貼り付け、重複するフォームのレイアウト/新しい"DinnerForm.ascx"部分ビュー テンプレートに、Edit.aspx/Create.aspx ビュー テンプレートからのコントロールのコードを入力します。</span><span class="sxs-lookup"><span data-stu-id="e70d3-132">We can then copy/paste the duplicate form layout / input control code from our Edit.aspx/ Create.aspx view templates into our new "DinnerForm.ascx" partial view template:</span></span>

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample1.aspx)]

<span data-ttu-id="e70d3-133">私たち DinnerForm 部分的なテンプレートを呼び出すし、フォームの重複を排除して、編集および作成のビュー テンプレートを更新できます。</span><span class="sxs-lookup"><span data-stu-id="e70d3-133">We can then update our Edit and Create view templates to call the DinnerForm partial template and eliminate the form duplication.</span></span> <span data-ttu-id="e70d3-134">Html.RenderPartial("DinnerForm") 呼び出し元によって、ビュー テンプレート内ではことができます。</span><span class="sxs-lookup"><span data-stu-id="e70d3-134">We can do this by calling Html.RenderPartial("DinnerForm") within our view templates:</span></span>

##### <a name="createaspx"></a><span data-ttu-id="e70d3-135">Create.aspx</span><span class="sxs-lookup"><span data-stu-id="e70d3-135">Create.aspx</span></span>

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample2.aspx)]

##### <a name="editaspx"></a><span data-ttu-id="e70d3-136">Edit.aspx</span><span class="sxs-lookup"><span data-stu-id="e70d3-136">Edit.aspx</span></span>

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample3.aspx)]

<span data-ttu-id="e70d3-137">Html.RenderPartial を呼び出すときに使用する一部のテンプレートのパスを明示的に修飾することができます (例: ~ Views/Dinners/DinnerForm.ascx")。</span><span class="sxs-lookup"><span data-stu-id="e70d3-137">You can explicitly qualify the path of the partial template you want when calling Html.RenderPartial (for example: ~Views/Dinners/DinnerForm.ascx").</span></span> <span data-ttu-id="e70d3-138">ただし、上記コードで ASP.NET MVC、内の規則に基づく名前付けパターンを活用とをレンダリングする部分の名前として"DinnerForm"を指定するだけはいます。</span><span class="sxs-lookup"><span data-stu-id="e70d3-138">In our code above, though, we are taking advantage of the convention-based naming pattern within ASP.NET MVC, and just specifying "DinnerForm" as the name of the partial to render.</span></span> <span data-ttu-id="e70d3-139">この作業を行う ASP.NET MVC は検索規則に基づく views ディレクトリに最初 (DinnersController Dinners/ビュー/になります)。</span><span class="sxs-lookup"><span data-stu-id="e70d3-139">When we do this ASP.NET MVC will look first in the convention-based views directory (for DinnersController this would be /Views/Dinners).</span></span> <span data-ttu-id="e70d3-140">部分的なテンプレートが見つからない場合がありますを検索して/Views/Shared ディレクトリにします。</span><span class="sxs-lookup"><span data-stu-id="e70d3-140">If it doesn't find the partial template there it will then look for it in the /Views/Shared directory.</span></span>

<span data-ttu-id="e70d3-141">Html.RenderPartial() は部分ビューの名前だけで呼び出されると、ASP.NET MVC はビューに渡す部分的な呼び出し元のビュー テンプレートで使用される同じのモデルと ViewData 辞書のオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="e70d3-141">When Html.RenderPartial() is called with just the name of the partial view, ASP.NET MVC will pass to the partial view the same Model and ViewData dictionary objects used by the calling view template.</span></span> <span data-ttu-id="e70d3-142">またはに、別のモデル オブジェクトや使用する部分ビューの ViewData 辞書を渡すことができるようにする Html.RenderPartial() のオーバー ロードされたバージョンがあります。</span><span class="sxs-lookup"><span data-stu-id="e70d3-142">Alternatively, there are overloaded versions of Html.RenderPartial() that enable you to pass an alternate Model object and/or ViewData dictionary for the partial view to use.</span></span> <span data-ttu-id="e70d3-143">これは、場所のみをする完全なモデルとビューモデルのサブセットを渡す場合に便利です。</span><span class="sxs-lookup"><span data-stu-id="e70d3-143">This is useful for scenarios where you only want to pass a subset of the full Model/ViewModel.</span></span>

| <span data-ttu-id="e70d3-144">**側のトピックは、なぜ&lt;%%&gt;の代わりに&lt;% = %&gt;でしょうか。**</span><span class="sxs-lookup"><span data-stu-id="e70d3-144">**Side Topic: Why &lt;% %&gt; instead of &lt;%= %&gt;?**</span></span> |
| --- |
| <span data-ttu-id="e70d3-145">使用されているは、上記のコードでお気付き微妙な点の 1 つ、 &lt;%%&gt;ブロックの代わりに、 &lt;% = %&gt; Html.RenderPartial() を呼び出すときにブロックします。</span><span class="sxs-lookup"><span data-stu-id="e70d3-145">One of the subtle things you might have noticed with the code above is that we are using a &lt;% %&gt; block instead of a &lt;%= %&gt; block when calling Html.RenderPartial().</span></span> <span data-ttu-id="e70d3-146">&lt;% = %&gt; ASP.NET 内のブロックは、開発者が指定された値を表示することを示します (例: &lt;% =「こんにちは」%&gt; 「こんにちは」となる)。</span><span class="sxs-lookup"><span data-stu-id="e70d3-146">&lt;%= %&gt; blocks in ASP.NET indicate that a developer wants to render a specified value (for example: &lt;%= "Hello" %&gt; would render "Hello").</span></span> <span data-ttu-id="e70d3-147">&lt;%%&gt;ブロックを示す代わりに、開発者がコードを実行する必要があること、およびそれらに含まれる出力が表示されたし、明示的に実行する必要があります (例: &lt;%response.write("hello")&gt;します。</span><span class="sxs-lookup"><span data-stu-id="e70d3-147">&lt;% %&gt; blocks instead indicate that the developer wants to execute code, and that any rendered output within them must be done explicitly (for example: &lt;% Response.Write("Hello") %&gt;.</span></span> <span data-ttu-id="e70d3-148">使用している理由を&lt;%%&gt;上記 Html.RenderPartial コードを持つブロックが Html.RenderPartial() メソッドが、文字列を返さないため、代わりに、呼び出し元のビュー テンプレートに直接コンテンツのストリームの出力を出力します。</span><span class="sxs-lookup"><span data-stu-id="e70d3-148">The reason we are using a &lt;% %&gt; block with our Html.RenderPartial code above is because the Html.RenderPartial() method doesn't return a string, and instead outputs the content directly to the calling view template's output stream.</span></span> <span data-ttu-id="e70d3-149">これは、パフォーマンスの効率向上のため、により、一時的な (場合によっては非常に大きな可能性があります) の文字列オブジェクトを作成する必要を回避できます。</span><span class="sxs-lookup"><span data-stu-id="e70d3-149">It does this for performance efficiency reasons, and by doing so it avoids the need to create a (potentially very large) temporary string object.</span></span> <span data-ttu-id="e70d3-150">これにより、メモリ使用量を削減し、アプリケーション全体のスループットを改善します。</span><span class="sxs-lookup"><span data-stu-id="e70d3-150">This reduces memory usage and improves overall application throughput.</span></span> <span data-ttu-id="e70d3-151">1 つのよくある間違い内にある場合、呼び出しの最後にセミコロンを追加することを忘れ Html.RenderPartial() を使用する場合、 &lt;%%&gt;ブロックします。</span><span class="sxs-lookup"><span data-stu-id="e70d3-151">One common mistake when using Html.RenderPartial() is to forget to add a semi-colon at the end of the call when it is within a &lt;% %&gt; block.</span></span> <span data-ttu-id="e70d3-152">たとえば、このコードでは、コンパイラ エラーが発生: &lt;%html.renderpartial("dinnerform")&gt;代わりに書き込む必要があります: &lt;Html.RenderPartial("DinnerForm"); %&gt;ため、これは&lt;%%&gt;ブロックは、自己完結型のコード ステートメント (C#) ステートメントをセミコロンで終了する必要があるコードを使用する場合。</span><span class="sxs-lookup"><span data-stu-id="e70d3-152">For example, this code will cause a compiler error: &lt;% Html.RenderPartial("DinnerForm") %&gt; You instead need to write: &lt;% Html.RenderPartial("DinnerForm"); %&gt; This is because &lt;% %&gt; blocks are self-contained code statements, and when using C# code statements need to be terminated with a semi-colon.</span></span> |

### <a name="using-partial-view-templates-to-clarify-code"></a><span data-ttu-id="e70d3-153">部分ビュー テンプレートを使ってコードを明確にするには</span><span class="sxs-lookup"><span data-stu-id="e70d3-153">Using Partial View Templates to Clarify Code</span></span>

<span data-ttu-id="e70d3-154">複数の場所でビューのレンダリング ロジックを重複しないようにする"DinnerForm"部分ビュー テンプレートを作成しました。</span><span class="sxs-lookup"><span data-stu-id="e70d3-154">We created the "DinnerForm" partial view template to avoid duplicating view rendering logic in multiple places.</span></span> <span data-ttu-id="e70d3-155">これは、部分ビュー テンプレートを作成する最も一般的な理由です。</span><span class="sxs-lookup"><span data-stu-id="e70d3-155">This is the most common reason to create partial view templates.</span></span>

<span data-ttu-id="e70d3-156">場合によってまだ理にかなってを 1 か所でのみ呼び出すされている場合でも、部分ビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="e70d3-156">Sometimes it still makes sense to create partial views even when they are only being called in a single place.</span></span> <span data-ttu-id="e70d3-157">テンプレートの表示が非常に複雑な多くの場合、ずっとときと、ビューのレンダリング ロジックの抽出し 1 つにパーティション分割またはよりも名前付き部分のテンプレートを読みやすくなります。</span><span class="sxs-lookup"><span data-stu-id="e70d3-157">Very complicated view templates can often become much easier to read when their view rendering logic is extracted and partitioned into one or more well named partial templates.</span></span>

<span data-ttu-id="e70d3-158">たとえば、(これは、私たちは見てすぐ) プロジェクトで Site.master ファイルからのコード スニペットの下。</span><span class="sxs-lookup"><span data-stu-id="e70d3-158">For example, consider the below code-snippet from the Site.master file in our project (which we will be looking at shortly).</span></span> <span data-ttu-id="e70d3-159">コードは比較的簡単読む – ログイン/ログアウトを表示するためのロジックが上部にあるリンクのため、部分的に画面の右側は"LogOnUserControl"部分内にカプセル化します。</span><span class="sxs-lookup"><span data-stu-id="e70d3-159">The code is relatively straight-forward to read – partly because the logic to display a login/logout link at the top right of the screen is encapsulated within the "LogOnUserControl" partial:</span></span>

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample4.aspx)]

<span data-ttu-id="e70d3-160">自分で混乱を見つける場合にしようとしてビュー テンプレート内の html コードとマークアップを理解、明確抽出され、適切な名前の部分ビューへのリファクタリングがその一部、かどうかを検討してください。</span><span class="sxs-lookup"><span data-stu-id="e70d3-160">Whenever you find yourself getting confused trying to understand the html/code markup within a view-template, consider whether it wouldn't be clearer if some of it was extracted and refactored into well-named partial views.</span></span>

### <a name="master-pages"></a><span data-ttu-id="e70d3-161">マスター ページ</span><span class="sxs-lookup"><span data-stu-id="e70d3-161">Master Pages</span></span>

<span data-ttu-id="e70d3-162">ASP.NET MVC には、部分ビューをサポートしているだけでなく、一般的なレイアウトと最上位レベルの html サイトの定義に使用できる「マスター ページ」テンプレートを作成する機能もサポートしています。</span><span class="sxs-lookup"><span data-stu-id="e70d3-162">In addition to supporting partial views, ASP.NET MVC also supports the ability to create "master page" templates that can be used to define the common layout and top-level html of a site.</span></span> <span data-ttu-id="e70d3-163">コンテンツのプレース ホルダーの後に上書きまたはビューによって「入力」できる置き換え可能な領域を識別するためにマスター ページのコントロールを追加できます。</span><span class="sxs-lookup"><span data-stu-id="e70d3-163">Content placeholder controls can then be added to the master page to identify replaceable regions that can be overridden or "filled in" by views.</span></span> <span data-ttu-id="e70d3-164">これは、アプリケーション全体で共通のレイアウトを適用する非常に効果的な (とドライ) 方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="e70d3-164">This provides a very effective (and DRY) way to apply a common layout across an application.</span></span>

<span data-ttu-id="e70d3-165">新しい ASP.NET MVC プロジェクトでは既定では、マスター ページ テンプレートが自動的に追加することがあります。</span><span class="sxs-lookup"><span data-stu-id="e70d3-165">By default, new ASP.NET MVC projects have a master page template automatically added to them.</span></span> <span data-ttu-id="e70d3-166">このマスター ページは、"Site.master"と \Views\Shared\ フォルダー内に存在する名前は。</span><span class="sxs-lookup"><span data-stu-id="e70d3-166">This master page is named "Site.master" and lives within the \Views\Shared\ folder:</span></span>

![](re-use-ui-using-master-pages-and-partials/_static/image5.png)

<span data-ttu-id="e70d3-167">既定の Site.master ファイルは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="e70d3-167">The default Site.master file looks like below.</span></span> <span data-ttu-id="e70d3-168">上部のナビゲーションのためのメニューと、サイトの外部の html を定義します。</span><span class="sxs-lookup"><span data-stu-id="e70d3-168">It defines the outer html of the site, along with a menu for navigation at the top.</span></span> <span data-ttu-id="e70d3-169">1 つは、タイトルをその他のページの主要なコンテンツを置き換える必要があります – 2 つの置き換え可能コンテンツ プレース ホルダー コントロールが含まれています。</span><span class="sxs-lookup"><span data-stu-id="e70d3-169">It contains two replaceable content placeholder controls – one for the title, and the other for where the primary content of a page should be replaced:</span></span>

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample5.aspx)]

<span data-ttu-id="e70d3-170">すべての ("List"、"Details"、「編集」、「作成」、"NotFound"など) は、NerdDinner アプリケーション用に作成したビュー テンプレートは、この Site.master テンプレートに基づくされてがあります。</span><span class="sxs-lookup"><span data-stu-id="e70d3-170">All of the view templates we've created for our NerdDinner application ("List", "Details", "Edit", "Create", "NotFound", etc) have been based on this Site.master template.</span></span> <span data-ttu-id="e70d3-171">これは既定では、上部に追加された"MasterPageFile"属性によって示されます&lt;ページ % @ %&gt;ディレクティブ「ビューの追加」ダイアログを使用してビューを作成したときに。</span><span class="sxs-lookup"><span data-stu-id="e70d3-171">This is indicated via the "MasterPageFile" attribute that was added by default to the top &lt;% @ Page %&gt; directive when we created our views using the "Add View" dialog:</span></span>

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample6.aspx)]

<span data-ttu-id="e70d3-172">この意味であり、Site.master のコンテンツを変更することができますが、変更に自動的に適用し、ビュー テンプレートのいずれかのレンダリング時に使用します。</span><span class="sxs-lookup"><span data-stu-id="e70d3-172">What this means is that we can change the Site.master content, and have the changes automatically be applied and used when we render any of our view templates.</span></span>

<span data-ttu-id="e70d3-173">アプリケーションのヘッダーが"My MVC Application"ではなく"NerdDinner"なるように、Site.master のヘッダー セクションを更新しましょう。</span><span class="sxs-lookup"><span data-stu-id="e70d3-173">Let's update our Site.master's header section so that the header of our application is "NerdDinner" instead of "My MVC Application".</span></span> <span data-ttu-id="e70d3-174">最初のタブが"を検索、Dinner"(HomeController の Index() アクション メソッドによって処理されます)、および"ホストを Dinner"(DinnersController の Create() アクション メソッドによって処理されます) と呼ばれる新しいタブを追加してみましょうように、ナビゲーション メニューも更新してみましょう。</span><span class="sxs-lookup"><span data-stu-id="e70d3-174">Let's also update our navigation menu so that the first tab is "Find a Dinner" (handled by the HomeController's Index() action method), and let's add a new tab called "Host a Dinner" (handled by the DinnersController's Create() action method):</span></span>

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample7.aspx)]

<span data-ttu-id="e70d3-175">Site.master ファイルと更新を保存して、ヘッダーを見て、ブラウザーは、アプリケーション内のすべてのビューでまで表示を変更します。</span><span class="sxs-lookup"><span data-stu-id="e70d3-175">When we save the Site.master file and refresh our browser we'll see our header changes show up across all views within our application.</span></span> <span data-ttu-id="e70d3-176">例えば:</span><span class="sxs-lookup"><span data-stu-id="e70d3-176">For example:</span></span>

![](re-use-ui-using-master-pages-and-partials/_static/image6.png)

<span data-ttu-id="e70d3-177">使用して、 */Dinners/編集/[id]* URL:</span><span class="sxs-lookup"><span data-stu-id="e70d3-177">And with the */Dinners/Edit/[id]* URL:</span></span>

![](re-use-ui-using-master-pages-and-partials/_static/image7.png)

### <a name="next-step"></a><span data-ttu-id="e70d3-178">次の手順</span><span class="sxs-lookup"><span data-stu-id="e70d3-178">Next Step</span></span>

<span data-ttu-id="e70d3-179">パーシャルとマスター ページは、クリーンにビューを整理するための非常に柔軟なオプションを提供します。</span><span class="sxs-lookup"><span data-stu-id="e70d3-179">Partials and master pages provide very flexible options that enable you to cleanly organize views.</span></span> <span data-ttu-id="e70d3-180">ビューを重複しないようにするのに役立ちますコンテンツ/コードし、テンプレートの表示を簡単に読み取りおよびメンテナンスが見つかります。</span><span class="sxs-lookup"><span data-stu-id="e70d3-180">You'll find that they help you avoid duplicating view content/ code, and make your view templates easier to read and maintain.</span></span>

<span data-ttu-id="e70d3-181">みましょう今すぐ前に作成した一覧のシナリオの見直しし、スケーラブルなページングのサポートを有効にします。</span><span class="sxs-lookup"><span data-stu-id="e70d3-181">Let's now revisit the listing scenario we built earlier and enable scalable paging support.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e70d3-182">[前へ](use-viewdata-and-implement-viewmodel-classes.md)
> [次へ](implement-efficient-data-paging.md)</span><span class="sxs-lookup"><span data-stu-id="e70d3-182">[Previous](use-viewdata-and-implement-viewmodel-classes.md)
[Next](implement-efficient-data-paging.md)</span></span>
