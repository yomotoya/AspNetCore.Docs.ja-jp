---
uid: mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
title: 提供の CRUD (作成、読み取り、更新、削除) データ フォーム エントリ サポート |Microsoft Docs
author: microsoft
description: 手順 5 では、編集、作成、およびられて Dinners を削除すると、同様のサポートを有効にして、DinnersController クラスをさらにする方法を示します。
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: bbb976e5-6150-4283-a374-c22fbafe29f5
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
msc.type: authoredcontent
ms.openlocfilehash: bfb8446ec8b39ad6fc88a0d5b747f0cec33bbd25
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817637"
---
<a name="provide-crud-create-read-update-delete-data-form-entry-support"></a><span data-ttu-id="158a1-103">提供の CRUD (作成、読み取り、更新、削除) データ フォーム エントリ サポート</span><span class="sxs-lookup"><span data-stu-id="158a1-103">Provide CRUD (Create, Read, Update, Delete) Data Form Entry Support</span></span>
====================
<span data-ttu-id="158a1-104">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="158a1-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="158a1-105">PDF のダウンロード</span><span class="sxs-lookup"><span data-stu-id="158a1-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="158a1-106">これは、無料の手順 5 ["NerdDinner"アプリケーションのチュートリアル](introducing-the-nerddinner-tutorial.md)をウォーク スルーの小さなをビルドしても、ASP.NET MVC 1 を使用して web アプリケーションを実行する方法。</span><span class="sxs-lookup"><span data-stu-id="158a1-106">This is step 5 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="158a1-107">手順 5 では、編集、作成、およびられて Dinners を削除すると、同様のサポートを有効にして、DinnersController クラスをさらにする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="158a1-107">Step 5 shows how to take our DinnersController class further by enable support for editing, creating and deleting Dinners with it as well.</span></span>
> 
> <span data-ttu-id="158a1-108">次のことをお勧め ASP.NET MVC 3 を使用している場合、 [MVC 3 の開始と取得](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)または[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)チュートリアル。</span><span class="sxs-lookup"><span data-stu-id="158a1-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-5-create-update-delete-form-scenarios"></a><span data-ttu-id="158a1-109">NerdDinner 手順 5: 作成、更新、削除のフォームのシナリオ</span><span class="sxs-lookup"><span data-stu-id="158a1-109">NerdDinner Step 5: Create, Update, Delete Form Scenarios</span></span>

<span data-ttu-id="158a1-110">コント ローラーとビューを導入し、それらを使用してサイトの Dinners のリスティング/詳細エクスペリエンスを実装する方法について説明しました。</span><span class="sxs-lookup"><span data-stu-id="158a1-110">We've introduced controllers and views, and covered how to use them to implement a listing/details experience for Dinners on site.</span></span> <span data-ttu-id="158a1-111">次の手順をさらに、DinnersController クラスを取得して編集、作成、およびられて Dinners を削除すると、同様のサポートを有効になります。</span><span class="sxs-lookup"><span data-stu-id="158a1-111">Our next step will be to take our DinnersController class further and enable support for editing, creating and deleting Dinners with it as well.</span></span>

### <a name="urls-handled-by-dinnerscontroller"></a><span data-ttu-id="158a1-112">DinnersController によって処理される Url</span><span class="sxs-lookup"><span data-stu-id="158a1-112">URLs handled by DinnersController</span></span>

<span data-ttu-id="158a1-113">以前に 2 つの Url のサポートを実装した DinnersController アクション メソッドを追加しました: */Dinners*と */Dinners 詳細/[id]* します。</span><span class="sxs-lookup"><span data-stu-id="158a1-113">We previously added action methods to DinnersController that implemented support for two URLs: */Dinners* and */Dinners/Details/[id]*.</span></span>

| <span data-ttu-id="158a1-114">**URL**</span><span class="sxs-lookup"><span data-stu-id="158a1-114">**URL**</span></span> | <span data-ttu-id="158a1-115">**動詞**</span><span class="sxs-lookup"><span data-stu-id="158a1-115">**VERB**</span></span> | <span data-ttu-id="158a1-116">**目的**</span><span class="sxs-lookup"><span data-stu-id="158a1-116">**Purpose**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="158a1-117">*/Dinners/*</span><span class="sxs-lookup"><span data-stu-id="158a1-117">*/Dinners/*</span></span> | <span data-ttu-id="158a1-118">GET</span><span class="sxs-lookup"><span data-stu-id="158a1-118">GET</span></span> | <span data-ttu-id="158a1-119">今後の dinners の HTML リストを表示します。</span><span class="sxs-lookup"><span data-stu-id="158a1-119">Display an HTML list of upcoming dinners.</span></span> |
| <span data-ttu-id="158a1-120">*/Dinners 詳細/[id]*</span><span class="sxs-lookup"><span data-stu-id="158a1-120">*/Dinners/Details/[id]*</span></span> | <span data-ttu-id="158a1-121">GET</span><span class="sxs-lookup"><span data-stu-id="158a1-121">GET</span></span> | <span data-ttu-id="158a1-122">特定の dinner に関する詳細を表示します。</span><span class="sxs-lookup"><span data-stu-id="158a1-122">Display details about a specific dinner.</span></span> |

<span data-ttu-id="158a1-123">これで 3 つの追加の Url を実装するアクション メソッドを追加します: <em>/Dinners/編集/[id]、/Dinners/作成、</em>と<em>/Dinners/削除/[id]</em>します。</span><span class="sxs-lookup"><span data-stu-id="158a1-123">We will now add action methods to implement three additional URLs: <em>/Dinners/Edit/[id], /Dinners/Create,</em>and<em>/Dinners/Delete/[id]</em>.</span></span> <span data-ttu-id="158a1-124">これらの Url は、新しい Dinners を作成および Dinners を削除する編集の既存 Dinners のサポートを有効になります。</span><span class="sxs-lookup"><span data-stu-id="158a1-124">These URLs will enable support for editing existing Dinners, creating new Dinners, and deleting Dinners.</span></span>

<span data-ttu-id="158a1-125">私たちは、これらの新しい Url に HTTP GET と HTTP POST の両方の動詞相互作用をサポートします。</span><span class="sxs-lookup"><span data-stu-id="158a1-125">We will support both HTTP GET and HTTP POST verb interactions with these new URLs.</span></span> <span data-ttu-id="158a1-126">これらの Url に HTTP GET 要求 ("edit"の場合、Dinner データ設定フォーム、「作成」の場合、空のフォームおよび「削除」の場合、削除の確認画面) のデータの初期の HTML ビューが表示されます。</span><span class="sxs-lookup"><span data-stu-id="158a1-126">HTTP GET requests to these URLs will display the initial HTML view of the data (a form populated with the Dinner data in the case of "edit", a blank form in the case of "create", and a delete confirmation screen in the case of "delete").</span></span> <span data-ttu-id="158a1-127">これらの Url に HTTP POST 要求は、Dinner のデータのときは必ず DinnerRepository (および、データベースにそこから) を保存/更新/削除になります。</span><span class="sxs-lookup"><span data-stu-id="158a1-127">HTTP POST requests to these URLs will save/update/delete the Dinner data in our DinnerRepository (and from there to the database).</span></span>

| <span data-ttu-id="158a1-128">**URL**</span><span class="sxs-lookup"><span data-stu-id="158a1-128">**URL**</span></span> | <span data-ttu-id="158a1-129">**動詞**</span><span class="sxs-lookup"><span data-stu-id="158a1-129">**VERB**</span></span> | <span data-ttu-id="158a1-130">**目的**</span><span class="sxs-lookup"><span data-stu-id="158a1-130">**Purpose**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="158a1-131">*/Dinners/Edit/[id]*</span><span class="sxs-lookup"><span data-stu-id="158a1-131">*/Dinners/Edit/[id]*</span></span> | <span data-ttu-id="158a1-132">GET</span><span class="sxs-lookup"><span data-stu-id="158a1-132">GET</span></span> | <span data-ttu-id="158a1-133">Dinner データが設定の編集可能な HTML フォームを表示します。</span><span class="sxs-lookup"><span data-stu-id="158a1-133">Display an editable HTML form populated with Dinner data.</span></span> |
| <span data-ttu-id="158a1-134">POST</span><span class="sxs-lookup"><span data-stu-id="158a1-134">POST</span></span> | <span data-ttu-id="158a1-135">特定の夕食をデータベースには、フォームの変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="158a1-135">Save the form changes for a particular Dinner to the database.</span></span> |
| <span data-ttu-id="158a1-136">*/Dinners/Create*</span><span class="sxs-lookup"><span data-stu-id="158a1-136">*/Dinners/Create*</span></span> | <span data-ttu-id="158a1-137">GET</span><span class="sxs-lookup"><span data-stu-id="158a1-137">GET</span></span> | <span data-ttu-id="158a1-138">ユーザーが新しい Dinners を定義できる空の HTML フォームを表示します。</span><span class="sxs-lookup"><span data-stu-id="158a1-138">Display an empty HTML form that allows users to define new Dinners.</span></span> |
| <span data-ttu-id="158a1-139">POST</span><span class="sxs-lookup"><span data-stu-id="158a1-139">POST</span></span> | <span data-ttu-id="158a1-140">新しい夕食を作成し、データベースに保存します。</span><span class="sxs-lookup"><span data-stu-id="158a1-140">Create a new Dinner and save it in the database.</span></span> |
| <span data-ttu-id="158a1-141">*/Dinners/Delete/[id]*</span><span class="sxs-lookup"><span data-stu-id="158a1-141">*/Dinners/Delete/[id]*</span></span> | <span data-ttu-id="158a1-142">GET</span><span class="sxs-lookup"><span data-stu-id="158a1-142">GET</span></span> | <span data-ttu-id="158a1-143">削除の確認画面を表示します。</span><span class="sxs-lookup"><span data-stu-id="158a1-143">Display delete confirmation screen.</span></span> |
| <span data-ttu-id="158a1-144">POST</span><span class="sxs-lookup"><span data-stu-id="158a1-144">POST</span></span> | <span data-ttu-id="158a1-145">データベースから指定の夕食を削除します。</span><span class="sxs-lookup"><span data-stu-id="158a1-145">Deletes the specified dinner from the database.</span></span> |

### <a name="edit-support"></a><span data-ttu-id="158a1-146">編集のサポート</span><span class="sxs-lookup"><span data-stu-id="158a1-146">Edit Support</span></span>

<span data-ttu-id="158a1-147">"Edit"シナリオの実装を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="158a1-147">Let's begin by implementing the "edit" scenario.</span></span>

#### <a name="the-http-get-edit-action-method"></a><span data-ttu-id="158a1-148">HTTP GET の Edit アクション メソッド</span><span class="sxs-lookup"><span data-stu-id="158a1-148">The HTTP-GET Edit Action Method</span></span>

<span data-ttu-id="158a1-149">Edit アクション メソッドの HTTP"GET"動作を実装することから始めます。</span><span class="sxs-lookup"><span data-stu-id="158a1-149">We'll start by implementing the HTTP "GET" behavior of our edit action method.</span></span> <span data-ttu-id="158a1-150">このメソッドになりますされると呼び出されます、 */Dinners/編集/[id]* URL を要求します。</span><span class="sxs-lookup"><span data-stu-id="158a1-150">This method will be invoked when the */Dinners/Edit/[id]* URL is requested.</span></span> <span data-ttu-id="158a1-151">今回の実装は、ようになります。</span><span class="sxs-lookup"><span data-stu-id="158a1-151">Our implementation will look like:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample1.cs)]

<span data-ttu-id="158a1-152">上記のコードでは、Dinner オブジェクトを取得するのにときは、必ず DinnerRepository を使用します。</span><span class="sxs-lookup"><span data-stu-id="158a1-152">The code above uses the DinnerRepository to retrieve a Dinner object.</span></span> <span data-ttu-id="158a1-153">Dinner オブジェクトを使用してビュー テンプレートをレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="158a1-153">It then renders a View template using the Dinner object.</span></span> <span data-ttu-id="158a1-154">テンプレート名を明示的に渡されたしていないため、 *View()* ヘルパー メソッドをテンプレートの表示を解決するのには規約ベースの既定パスを使用には:/Views/Dinners/Edit.aspx します。</span><span class="sxs-lookup"><span data-stu-id="158a1-154">Because we haven't explicitly passed a template name to the *View()* helper method, it will use the convention based default path to resolve the view template: /Views/Dinners/Edit.aspx.</span></span>

<span data-ttu-id="158a1-155">このテンプレートの表示を今すぐ作成しましょう。</span><span class="sxs-lookup"><span data-stu-id="158a1-155">Let's now create this view template.</span></span> <span data-ttu-id="158a1-156">Edit メソッド内で右クリックし、「ビューの追加」コンテキスト メニュー コマンドを選択して行います。</span><span class="sxs-lookup"><span data-stu-id="158a1-156">We will do this by right-clicking within the Edit method and selecting the "Add View" context menu command:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image1.png)

<span data-ttu-id="158a1-157">"ビューの追加 ダイアログ ボックスで指定する Dinner オブジェクトとしてのモデル、ビュー テンプレートに渡すし、自動スキャフォールディングを「編集」テンプレートを選択します。</span><span class="sxs-lookup"><span data-stu-id="158a1-157">Within the "Add View" dialog we'll indicate that we are passing a Dinner object to our view template as its model, and choose to auto-scaffold an "Edit" template:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image2.png)

<span data-ttu-id="158a1-158">[追加] ボタンをクリックすると、ときに Visual Studio は新しい"Edit.aspx"ビュー テンプレート ファイルを"\Views\Dinners"ディレクトリ内を追加します。</span><span class="sxs-lookup"><span data-stu-id="158a1-158">When we click the "Add" button, Visual Studio will add a new "Edit.aspx" view template file for us within the "\Views\Dinners" directory.</span></span> <span data-ttu-id="158a1-159">初期「編集」スキャフォールディング実装すると、以下を含むコードのエディター – 内に、新しい"Edit.aspx"ビュー テンプレートも開きます。</span><span class="sxs-lookup"><span data-stu-id="158a1-159">It will also open up the new "Edit.aspx" view template within the code-editor – populated with an initial "Edit" scaffold implementation like below:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image3.png)

<span data-ttu-id="158a1-160">生成されると、既定値"edit"スキャフォールディングにいくつかの変更を加えるし、(いくつかのプロパティを公開する必要はありませんが削除されます) をコンテンツの編集ビュー テンプレートを更新しましょう。</span><span class="sxs-lookup"><span data-stu-id="158a1-160">Let's make a few changes to the default "edit" scaffold generated, and update the edit view template to have the content below (which removes a few of the properties we don't want to expose):</span></span>

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample2.aspx)]

<span data-ttu-id="158a1-161">アプリケーションと要求を実行したときに、 *「/1/Dinners/編集」* URL は、次のページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="158a1-161">When we run the application and request the *"/Dinners/Edit/1"* URL we will see the following page:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image4.png)

<span data-ttu-id="158a1-162">このビューによって生成される HTML マークアップは、以下のようになります。</span><span class="sxs-lookup"><span data-stu-id="158a1-162">The HTML markup generated by our view looks like below.</span></span> <span data-ttu-id="158a1-163">– 標準の HTML では、&lt;フォーム&gt;要素への HTTP POST を実行する、 */Dinners/Edit/1* URL と [保存]&lt;入力の種類 =「送信」/&gt;ボタンが押されました。</span><span class="sxs-lookup"><span data-stu-id="158a1-163">It is standard HTML – with a &lt;form&gt; element that performs an HTTP POST to the */Dinners/Edit/1* URL when the "Save" &lt;input type="submit"/&gt; button is pushed.</span></span> <span data-ttu-id="158a1-164">HTML&lt;入力の種類 ="text"/&gt;要素には、編集可能な各プロパティの出力がされています。</span><span class="sxs-lookup"><span data-stu-id="158a1-164">A HTML &lt;input type="text"/&gt; element has been output for each editable property:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image5.png)

#### <a name="htmlbeginform-and-htmltextbox-html-helper-methods"></a><span data-ttu-id="158a1-165">Html.BeginForm() と Html.TextBox() Html ヘルパー メソッド</span><span class="sxs-lookup"><span data-stu-id="158a1-165">Html.BeginForm() and Html.TextBox() Html Helper Methods</span></span>

<span data-ttu-id="158a1-166">"Edit.aspx"ビュー テンプレートがいくつかの「Html ヘルパー」メソッドを使用して: Html.ValidationSummary()、Html.BeginForm()、Html.TextBox()、および Html.ValidationMessage() します。</span><span class="sxs-lookup"><span data-stu-id="158a1-166">Our "Edit.aspx" view template is using several "Html Helper" methods: Html.ValidationSummary(), Html.BeginForm(), Html.TextBox(), and Html.ValidationMessage().</span></span> <span data-ttu-id="158a1-167">組み込みのエラー処理と検証の HTML マークアップを生成する、に加えてこれらのヘルパー メソッドを提供をサポートします。</span><span class="sxs-lookup"><span data-stu-id="158a1-167">In addition to generating HTML markup for us, these helper methods provide built-in error handling and validation support.</span></span>

##### <a name="htmlbeginform-helper-method"></a><span data-ttu-id="158a1-168">Html.BeginForm() ヘルパー メソッド</span><span class="sxs-lookup"><span data-stu-id="158a1-168">Html.BeginForm() helper method</span></span>

<span data-ttu-id="158a1-169">Html.BeginForm() のヘルパー メソッドは、どのような出力 HTML&lt;フォーム&gt;マークアップ要素。</span><span class="sxs-lookup"><span data-stu-id="158a1-169">The Html.BeginForm() helper method is what output the HTML &lt;form&gt; element in our markup.</span></span> <span data-ttu-id="158a1-170">Edit.aspx ビュー テンプレートに適用する"using"ステートメントと、このメソッドを使用して C# でわかります。</span><span class="sxs-lookup"><span data-stu-id="158a1-170">In our Edit.aspx view template you'll notice that we are applying a C# "using" statement when using this method.</span></span> <span data-ttu-id="158a1-171">中かっこの先頭を示します、&lt;フォーム&gt;コンテンツ、および右中かっこがの末尾を示す内容、 &lt;/form&gt;要素。</span><span class="sxs-lookup"><span data-stu-id="158a1-171">The open curly brace indicates the beginning of the &lt;form&gt; content, and the closing curly brace is what indicates the end of the &lt;/form&gt; element:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample3.cs)]

<span data-ttu-id="158a1-172">また、"using"ステートメントを検索する場合は、このようなシナリオの不自然なアプローチ、Html.BeginForm() と Html.EndForm() を組み合わせて (これは、同じ動作が) を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="158a1-172">Alternatively, if you find the "using" statement approach unnatural for a scenario like this, you can use a Html.BeginForm() and Html.EndForm() combination (which does the same thing):</span></span>

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample4.aspx)]

<span data-ttu-id="158a1-173">パラメーターを指定せず Html.BeginForm() を呼び出すことは現在の要求の URL への HTTP POST をフォーム要素を出力することになります。</span><span class="sxs-lookup"><span data-stu-id="158a1-173">Calling Html.BeginForm() without any parameters will cause it to output a form element that does an HTTP-POST to the current request's URL.</span></span> <span data-ttu-id="158a1-174">つまり、編集ビューを生成理由、 *&lt;フォーム アクション =「/1/Dinners/編集」メソッド ="post"&gt;* 要素。</span><span class="sxs-lookup"><span data-stu-id="158a1-174">That is why our Edit view generates a *&lt;form action="/Dinners/Edit/1" method="post"&gt;* element.</span></span> <span data-ttu-id="158a1-175">でしたがまたは明示的なパラメーターに渡した Html.BeginForm() 別の URL に投稿する場合。</span><span class="sxs-lookup"><span data-stu-id="158a1-175">We could have alternatively passed explicit parameters to Html.BeginForm() if we wanted to post to a different URL.</span></span>

##### <a name="htmltextbox-helper-method"></a><span data-ttu-id="158a1-176">Html.TextBox() ヘルパー メソッド</span><span class="sxs-lookup"><span data-stu-id="158a1-176">Html.TextBox() helper method</span></span>

<span data-ttu-id="158a1-177">この Edit.aspx ビュー Html.TextBox() のヘルパー メソッドを使用して出力する&lt;入力の種類 ="text"/&gt;要素。</span><span class="sxs-lookup"><span data-stu-id="158a1-177">Our Edit.aspx view uses the Html.TextBox() helper method to output &lt;input type="text"/&gt; elements:</span></span>

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample5.aspx)]

<span data-ttu-id="158a1-178">上記 Html.TextBox() メソッドには、1 つのパラメーター – の id/名前属性を指定するために使用される、&lt;入力の種類 ="text"/&gt;からテキスト ボックスに値を設定するモデルのプロパティと同様に、出力する要素。</span><span class="sxs-lookup"><span data-stu-id="158a1-178">The Html.TextBox() method above takes a single parameter – which is being used to specify both the id/name attributes of the &lt;input type="text"/&gt; element to output, as well as the model property to populate the textbox value from.</span></span> <span data-ttu-id="158a1-179">たとえば、編集ビューに渡された Dinner オブジェクトが".NET Future"の"Title"プロパティの値と、Html.TextBox("Title") メソッドが出力を呼び出すため: *&lt;入力 id ="Title"名前 =「タイトル」の種類 ="text"の値 =".NET Future"/&gt;*.</span><span class="sxs-lookup"><span data-stu-id="158a1-179">For example, the Dinner object we passed to the Edit view had a "Title" property value of ".NET Futures", and so our Html.TextBox("Title") method call output: *&lt;input id="Title" name="Title" type="text" value=".NET Futures" /&gt;*.</span></span>

<span data-ttu-id="158a1-180">または、要素の id/名前を指定し、2 番目のパラメーターとして使用する値で明示的に渡す Html.TextBox() の最初のパラメーターを使用できます。</span><span class="sxs-lookup"><span data-stu-id="158a1-180">Alternatively, we can use the first Html.TextBox() parameter to specify the id/name of the element, and then explicitly pass in the value to use as a second parameter:</span></span>

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample6.aspx)]

<span data-ttu-id="158a1-181">多くの場合、出力される値のカスタム書式設定を実行するします。</span><span class="sxs-lookup"><span data-stu-id="158a1-181">Often we'll want to perform custom formatting on the value that is output.</span></span> <span data-ttu-id="158a1-182">.NET に組み込まれて指定する静的メソッドは、これらのシナリオに適しています。</span><span class="sxs-lookup"><span data-stu-id="158a1-182">The String.Format() static method built-into .NET is useful for these scenarios.</span></span> <span data-ttu-id="158a1-183">Edit.aspx ビュー テンプレートを使用してこの書式、EventDate 値 (DateTime 型の) 時間の秒を表示しないように。</span><span class="sxs-lookup"><span data-stu-id="158a1-183">Our Edit.aspx view template is using this to format the EventDate value (which is of type DateTime) so that it doesn't show seconds for the time:</span></span>

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample7.aspx)]

<span data-ttu-id="158a1-184">Html.TextBox() の 3 番目のパラメーターは、追加の HTML 属性を出力するオプションで使用できます。</span><span class="sxs-lookup"><span data-stu-id="158a1-184">A third parameter to Html.TextBox() can optionally be used to output additional HTML attributes.</span></span> <span data-ttu-id="158a1-185">次のコードのスニペットは、その他のサイズを表示する方法を示します「30」属性とクラスを = = on"mycssclass"属性、&lt;入力の種類 ="text"/&gt;要素。</span><span class="sxs-lookup"><span data-stu-id="158a1-185">The code-snippet below demonstrates how to render an additional size="30" attribute and a class="mycssclass" attribute on the &lt;input type="text"/&gt; element.</span></span> <span data-ttu-id="158a1-186">使用して、クラス属性の名前をエスケープする方法に注意してください、"@" character because "クラス"予約済みキーワード (C#)。</span><span class="sxs-lookup"><span data-stu-id="158a1-186">Note how we are escaping the name of the class attribute using a "@" character because "class" is a reserved keyword in C#:</span></span>

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample8.aspx)]

#### <a name="implementing-the-http-post-edit-action-method"></a><span data-ttu-id="158a1-187">HTTP POST の Edit アクション メソッドを実装します。</span><span class="sxs-lookup"><span data-stu-id="158a1-187">Implementing the HTTP-POST Edit Action Method</span></span>

<span data-ttu-id="158a1-188">HTTP GET バージョン、Edit アクション メソッドの実装のようになりましたがあります。</span><span class="sxs-lookup"><span data-stu-id="158a1-188">We now have the HTTP-GET version of our Edit action method implemented.</span></span> <span data-ttu-id="158a1-189">ユーザーが要求したとき、 */Dinners/Edit/1*次のような HTML ページを受信した URL:</span><span class="sxs-lookup"><span data-stu-id="158a1-189">When a user requests the */Dinners/Edit/1* URL they receive an HTML page like the following:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image6.png)

<span data-ttu-id="158a1-190">フォーム post を「保存」ボタンを押すと、 */Dinners/Edit/1* URL、HTML を送信して&lt;入力&gt;HTTP POST 動詞を使用して値を形成します。</span><span class="sxs-lookup"><span data-stu-id="158a1-190">Pressing the "Save" button causes a form post to the */Dinners/Edit/1* URL, and submits the HTML &lt;input&gt; form values using the HTTP POST verb.</span></span> <span data-ttu-id="158a1-191">今すぐ – Dinner の保存を処理する、edit アクション メソッドの HTTP POST の動作を実装してみましょう。</span><span class="sxs-lookup"><span data-stu-id="158a1-191">Let's now implement the HTTP POST behavior of our edit action method – which will handle saving the Dinner.</span></span>

<span data-ttu-id="158a1-192">まず、DinnersController を"AcceptVerbs"属性を持つ HTTP POST のシナリオを扱うことを示すを「編集」アクション オーバー ロードされたメソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="158a1-192">We'll begin by adding an overloaded "Edit" action method to our DinnersController that has an "AcceptVerbs" attribute on it that indicates it handles HTTP POST scenarios:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample9.cs)]

<span data-ttu-id="158a1-193">[AcceptVerbs] 属性は、オーバー ロードされたアクション メソッドに適用するときに ASP.NET MVC は自動的に入力方向の HTTP 動詞に応じて適切なアクション メソッドにディスパッチ要求を処理します。</span><span class="sxs-lookup"><span data-stu-id="158a1-193">When the [AcceptVerbs] attribute is applied to overloaded action methods, ASP.NET MVC automatically handles dispatching requests to the appropriate action method depending on the incoming HTTP verb.</span></span> <span data-ttu-id="158a1-194">HTTP POST 要求を<em>/Dinners/編集/[id]</em> Url への他のすべての HTTP 動詞要求中に、上記の Edit メソッドに移動します<em>/Dinners/編集/[id]</em>Url が変わります (これが最初の編集メソッドを実装しましたいない属性を持つ [AcceptVerbs])。</span><span class="sxs-lookup"><span data-stu-id="158a1-194">HTTP POST requests to <em>/Dinners/Edit/[id]</em> URLs will go to the above Edit method, while all other HTTP verb requests to <em>/Dinners/Edit/[id]</em>URLs will go to the first Edit method we implemented (which did not have an [AcceptVerbs] attribute).</span></span>

| <span data-ttu-id="158a1-195">**側のトピック: は、HTTP 動詞を使用して区別理由でしょうか。**</span><span class="sxs-lookup"><span data-stu-id="158a1-195">**Side Topic: Why differentiate via HTTP verbs?**</span></span> |
| --- |
| <span data-ttu-id="158a1-196">思うかもしれません – はなぜ 1 つの URL を使用して、HTTP 動詞を使用してその動作を区別しますか。</span><span class="sxs-lookup"><span data-stu-id="158a1-196">You might ask – why are we using a single URL and differentiating its behavior via the HTTP verb?</span></span> <span data-ttu-id="158a1-197">なぜの読み込みと編集の変更の保存を処理するために 2 つの個別の Url があるだけですか。</span><span class="sxs-lookup"><span data-stu-id="158a1-197">Why not just have two separate URLs to handle loading and saving edit changes?</span></span> <span data-ttu-id="158a1-198">例:/Dinners/編集/[id] の初期フォームと/Dinners/保存/[id] を保存してフォーム ポストを処理するために表示するか。</span><span class="sxs-lookup"><span data-stu-id="158a1-198">For example: /Dinners/Edit/[id] to display the initial form and /Dinners/Save/[id] to handle the form post to save it?</span></span> <span data-ttu-id="158a1-199">発行の 2 つの独立した Url に欠点最終的には、/Dinners/Save/2 に投稿し、入力エラーのための HTML フォームを再表示する必要があります場所の場合、エンドユーザーが (だったために、ブラウザーのアドレス バーに Dinners/保存/2 の URL を持つURL にポストされたフォーム)。</span><span class="sxs-lookup"><span data-stu-id="158a1-199">The downside with publishing two separate URLs is that in cases where we post to /Dinners/Save/2, and then need to redisplay the HTML form because of an input error, the end-user will end up having the /Dinners/Save/2 URL in their browser's address bar (since that was the URL the form posted to).</span></span> <span data-ttu-id="158a1-200">場合は、エンド ユーザー ブラウザーのお気に入りの一覧にこの redisplayed ページをブックマークまたは URL のコピー/貼り付け、友人にメールで送信 (ため、その URL は、投稿の値によって異なります) は、後で動作しない URL を保存になります。</span><span class="sxs-lookup"><span data-stu-id="158a1-200">If the end-user bookmarks this redisplayed page to their browser favorites list, or copy/pastes the URL and emails it to a friend, they will end up saving a URL that won't work in the future (since that URL depends on post values).</span></span> <span data-ttu-id="158a1-201">1 つの URL を公開することで (のような:/Dinners/Edit/[id]) および HTTP 動詞でその処理を区別するの編集ページをブックマークや他のユーザーに、URL を送信するエンドユーザーのも安全です。</span><span class="sxs-lookup"><span data-stu-id="158a1-201">By exposing a single URL (like: /Dinners/Edit/[id]) and differentiating the processing of it by HTTP verb, it is safe for end-users to bookmark the edit page and/or send the URL to others.</span></span> |

#### <a name="retrieving-form-post-values"></a><span data-ttu-id="158a1-202">フォーム ポスト値を取得します。</span><span class="sxs-lookup"><span data-stu-id="158a1-202">Retrieving Form Post Values</span></span>

<span data-ttu-id="158a1-203">さまざまなフォーム パラメーター、HTTP POST の「編集」メソッド内での投稿にアクセスする方法があります。</span><span class="sxs-lookup"><span data-stu-id="158a1-203">There are a variety of ways we can access posted form parameters within our HTTP POST "Edit" method.</span></span> <span data-ttu-id="158a1-204">単純なアプローチの 1 つだけコント ローラーの基本クラスで要求のプロパティを使用して、フォームのコレクションへのアクセスを直接ポストされた値を取得することです。</span><span class="sxs-lookup"><span data-stu-id="158a1-204">One simple approach is to just use the Request property on the Controller base class to access the form collection and retrieve the posted values directly:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample10.cs)]

<span data-ttu-id="158a1-205">上記の方法は特に、エラー処理ロジックを追加した、少し冗長です。</span><span class="sxs-lookup"><span data-stu-id="158a1-205">The above approach is a little verbose, though, especially once we add error handling logic.</span></span>

<span data-ttu-id="158a1-206">このシナリオでは、組み込みの活用のより優れた方法*UpdateModel()* コント ローラーの基本クラスのヘルパー メソッド。</span><span class="sxs-lookup"><span data-stu-id="158a1-206">A better approach for this scenario is to leverage the built-in *UpdateModel()* helper method on the Controller base class.</span></span> <span data-ttu-id="158a1-207">渡します受信フォーム パラメーターを使用してオブジェクトのプロパティの更新をサポートします。</span><span class="sxs-lookup"><span data-stu-id="158a1-207">It supports updating the properties of an object we pass it using the incoming form parameters.</span></span> <span data-ttu-id="158a1-208">リフレクションを使用して、オブジェクトのプロパティ名を決定して自動的に変換し、クライアントから送信された入力値に基づいて値を割り当てます。</span><span class="sxs-lookup"><span data-stu-id="158a1-208">It uses reflection to determine the property names on the object, and then automatically converts and assigns values to them based on the input values submitted by the client.</span></span>

<span data-ttu-id="158a1-209">UpdateModel() メソッドを使用して、HTTP POST アクションの編集このコードを使用して簡素化でした。</span><span class="sxs-lookup"><span data-stu-id="158a1-209">We could use the UpdateModel() method to simplify our HTTP-POST Edit Action using this code:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample11.cs)]

<span data-ttu-id="158a1-210">アクセスできるようになりました、 */Dinners/Edit/1* URL、および、Dinner のタイトルを変更します。</span><span class="sxs-lookup"><span data-stu-id="158a1-210">We can now visit the */Dinners/Edit/1* URL, and change the title of our Dinner:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image7.png)

<span data-ttu-id="158a1-211">[保存] ボタンをクリックしてとポストされたフォームの編集の操作を実行します、更新された値は、データベースに保存されます。</span><span class="sxs-lookup"><span data-stu-id="158a1-211">When we click the "Save" button, we'll perform a form post to our Edit action, and the updated values will be persisted in the database.</span></span> <span data-ttu-id="158a1-212">私たちは、夕食 (これは、新しく保存された値が表示されます) の詳細 URL にリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="158a1-212">We will then be redirected to the Details URL for the Dinner (which will display the newly saved values):</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image8.png)

#### <a name="handling-edit-errors"></a><span data-ttu-id="158a1-213">編集エラーの処理</span><span class="sxs-lookup"><span data-stu-id="158a1-213">Handling Edit Errors</span></span>

<span data-ttu-id="158a1-214">場合を除き、現在の HTTP POST 実装の動作を細かく – エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="158a1-214">Our current HTTP-POST implementation works fine – except when there are errors.</span></span>

<span data-ttu-id="158a1-215">ユーザーが間違いを犯したフォームを編集すると、フォームが問題の修正をガイドするわかりやすいエラー メッセージと共に再表示するかどうかを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="158a1-215">When a user makes a mistake editing a form, we need to make sure that the form is redisplayed with an informative error message that guides them to fix it.</span></span> <span data-ttu-id="158a1-216">エンドユーザーが正しくない入力を投稿するケースが含まれます (例: 形式が正しくない日付の文字列)、あるケースも入力形式は有効ですが、ビジネス ルール違反があります。</span><span class="sxs-lookup"><span data-stu-id="158a1-216">This includes cases where an end-user posts incorrect input (for example: a malformed date string), as well as cases where the input format is valid but there is a business rule violation.</span></span> <span data-ttu-id="158a1-217">エラーが発生した場合、フォームにその変更を手動で再読み込みするがあるないように最初に入力したユーザーの入力データを保持する必要があります。</span><span class="sxs-lookup"><span data-stu-id="158a1-217">When errors occur the form should preserve the input data the user originally entered so that they don't have to refill their changes manually.</span></span> <span data-ttu-id="158a1-218">フォームが正常に完了するまで、このプロセスが必要な回数だけ繰り返されます。</span><span class="sxs-lookup"><span data-stu-id="158a1-218">This process should repeat as many times as necessary until the form successfully completes.</span></span>

<span data-ttu-id="158a1-219">ASP.NET MVC には、エラー処理とフォームの再表示を簡単に構成するいくつかの便利な組み込み機能が含まれています。</span><span class="sxs-lookup"><span data-stu-id="158a1-219">ASP.NET MVC includes some nice built-in features that make error handling and form redisplay easy.</span></span> <span data-ttu-id="158a1-220">みましょうアクションでこれらの機能を表示する次のコードで、Edit アクション メソッドを更新します。</span><span class="sxs-lookup"><span data-stu-id="158a1-220">To see these features in action let's update our Edit action method with the following code:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample12.cs)]

<span data-ttu-id="158a1-221">上記のコードは、前の実装に似ていますが、作業を try/catch エラー処理のブロックをラップするようになりました。</span><span class="sxs-lookup"><span data-stu-id="158a1-221">The above code is similar to our previous implementation – except that we are now wrapping a try/catch error handling block around our work.</span></span> <span data-ttu-id="158a1-222">UpdateModel() を呼び出すときに、またはしようとして、ときは必ず DinnerRepository (これが、私たちのモデル内のルール違反のため、Dinner オブジェクトを保存しようとしましたが無効な場合、例外が発生)、保存時に、ブロックの catch エラー処理、例外が発生した場合は実行します。</span><span class="sxs-lookup"><span data-stu-id="158a1-222">If an exception occurs either when calling UpdateModel(), or when we try and save the DinnerRepository (which will raise an exception if the Dinner object we are trying to save is invalid because of a rule violation within our model), our catch error handling block will execute.</span></span> <span data-ttu-id="158a1-223">内にループに Dinner オブジェクト内に存在する任意の規則違反の上を (すぐ説明します) する ModelState オブジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="158a1-223">Within it we loop over any rule violations that exist in the Dinner object and add them to a ModelState object (which we'll discuss shortly).</span></span> <span data-ttu-id="158a1-224">ここには、ビュー、再表示します。</span><span class="sxs-lookup"><span data-stu-id="158a1-224">We then redisplay the view.</span></span>

<span data-ttu-id="158a1-225">早速アプリケーションを再実行動作しているこのかの夕食を編集し、EventDate"BOGUS"の空のタイトルがあり、米国の国の値で英国の電話番号を使用するように変更します。</span><span class="sxs-lookup"><span data-stu-id="158a1-225">To see this working let's re-run the application, edit a Dinner, and change it to have an empty Title, an EventDate of "BOGUS", and use a UK phone number with a country value of USA.</span></span> <span data-ttu-id="158a1-226">キーを押すと [保存] ボタンと HTTP POST の編集方法では、ことはできません (エラーがある) ため、Dinner を保存して、フォームを再表示されます。</span><span class="sxs-lookup"><span data-stu-id="158a1-226">When we press the "Save" button our HTTP POST Edit method will not be able to save the Dinner (because there are errors) and will redisplay the form:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image9.png)

<span data-ttu-id="158a1-227">このアプリケーションでは、適切なエラー エクスペリエンスがあります。</span><span class="sxs-lookup"><span data-stu-id="158a1-227">Our application has a decent error experience.</span></span> <span data-ttu-id="158a1-228">入力に無効なテキスト要素が赤で強調表示し、それらについてエンドユーザーに検証エラー メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="158a1-228">The text elements with the invalid input are highlighted in red, and validation error messages are displayed to the end user about them.</span></span> <span data-ttu-id="158a1-229">フォームも保持しているユーザーの元の入力: 入力データを何も再設定があるないようにします。</span><span class="sxs-lookup"><span data-stu-id="158a1-229">The form is also preserving the input data the user originally entered – so that they don't have to refill anything.</span></span>

<span data-ttu-id="158a1-230">方法については、疑問に思うかもしれませんがこれが発生するでしょうか。</span><span class="sxs-lookup"><span data-stu-id="158a1-230">How, you might ask, did this occur?</span></span> <span data-ttu-id="158a1-231">方法でした、タイトル、EventDate、および ContactPhone のテキスト ボックス自体を赤で強調表示し、最初に入力したユーザーの値を出力するでしょうか。</span><span class="sxs-lookup"><span data-stu-id="158a1-231">How did the Title, EventDate, and ContactPhone textboxes highlight themselves in red and know to output the originally entered user values?</span></span> <span data-ttu-id="158a1-232">方法がエラー メッセージを取得一覧に表示、上部にあるでしょうか。</span><span class="sxs-lookup"><span data-stu-id="158a1-232">And how did error messages get displayed in the list at the top?</span></span> <span data-ttu-id="158a1-233">これで、ViewData とビューモデル クラスを使用できます、フォームでより豊富な UI を有効にする方法を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="158a1-233">The good news is that this didn't occur by magic - rather it was because we used some of the built-in ASP.NET MVC features that make input validation and error handling scenarios easy.</span></span>

#### <a name="understanding-modelstate-and-the-validation-html-helper-methods"></a><span data-ttu-id="158a1-234">ModelState の理解と検証の HTML ヘルパー メソッド</span><span class="sxs-lookup"><span data-stu-id="158a1-234">Understanding ModelState and the Validation HTML Helper Methods</span></span>

<span data-ttu-id="158a1-235">コント ローラー クラスでは、エラーがビューに渡されるモデル オブジェクトに存在することを示す方法を提供する"ModelState"プロパティ コレクションがあります。</span><span class="sxs-lookup"><span data-stu-id="158a1-235">Controller classes have a "ModelState" property collection which provides a way to indicate that errors exist with a model object being passed to a View.</span></span> <span data-ttu-id="158a1-236">ModelState コレクション内のエラーのエントリでは、モデル プロパティの名前を識別して問題 (例:"Title"、"EventDate"または"ContactPhone")、人間にとってわかりやすいエラー メッセージを指定することができ (例:「タイトルが必要です」)。</span><span class="sxs-lookup"><span data-stu-id="158a1-236">Error entries within the ModelState collection identify the name of the model property with the issue (for example: "Title", "EventDate", or "ContactPhone"), and allow a human-friendly error message to be specified (for example: "Title is required").</span></span>

<span data-ttu-id="158a1-237">*UpdateModel()* モデル オブジェクトのプロパティにフォームの値を代入しようとしています。 中にエラーが発生したときにヘルパー メソッドが ModelState コレクションに自動的に設定します。</span><span class="sxs-lookup"><span data-stu-id="158a1-237">The *UpdateModel()* helper method automatically populates the ModelState collection when it encounters errors while trying to assign form values to properties on the model object.</span></span> <span data-ttu-id="158a1-238">たとえば、Dinner オブジェクトの EventDate プロパティは DateTime 型です。</span><span class="sxs-lookup"><span data-stu-id="158a1-238">For example, our Dinner object's EventDate property is of type DateTime.</span></span> <span data-ttu-id="158a1-239">UpdateModel() メソッドは、上記のシナリオで"BOGUS"の文字列値を割り当てることができませんが、そのプロパティに割り当てエラーを示す ModelState コレクションにエントリが発生しました UpdateModel() メソッドが追加されます。</span><span class="sxs-lookup"><span data-stu-id="158a1-239">When the UpdateModel() method was unable to assign the string value "BOGUS" to it in the scenario above, the UpdateModel() method added an entry to the ModelState collection indicating an assignment error had occurred with that property.</span></span>

<span data-ttu-id="158a1-240">開発者は、以下、"catch"エラーの処理ブロック内でエントリで、アクティブなルール違反に基づく ModelState コレクションが読み込みを実行しているように明示的に ModelState コレクションへのエラー エントリを追加するコードを記述できますも、Dinner オブジェクト:</span><span class="sxs-lookup"><span data-stu-id="158a1-240">Developers can also write code to explicitly add error entries into the ModelState collection like we are doing below within our "catch" error handling block, which is populating the ModelState collection with entries based on the active Rule Violations in the Dinner object:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample13.cs)]

#### <a name="html-helper-integration-with-modelstate"></a><span data-ttu-id="158a1-241">ModelState と html ヘルパーの統合</span><span class="sxs-lookup"><span data-stu-id="158a1-241">Html Helper Integration with ModelState</span></span>

<span data-ttu-id="158a1-242">Html.TextBox() - などの HTML ヘルパー メソッドは、出力を表示するときに、ModelState のコレクションを確認します。</span><span class="sxs-lookup"><span data-stu-id="158a1-242">HTML helper methods - like Html.TextBox() - check the ModelState collection when rendering output.</span></span> <span data-ttu-id="158a1-243">項目のエラーが存在する場合、ユーザーが入力した値と CSS エラー クラス レンダリングします。</span><span class="sxs-lookup"><span data-stu-id="158a1-243">If an error for the item exists, they render the user-entered value and a CSS error class.</span></span>

<span data-ttu-id="158a1-244">たとえばで、[編集] ビューは使用 Html.TextBox() ヘルパー メソッドは、Dinner オブジェクトの EventDate を表示するために。</span><span class="sxs-lookup"><span data-stu-id="158a1-244">For example, in our "Edit" view we are using the Html.TextBox() helper method to render the EventDate of our Dinner object:</span></span>

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample14.aspx)]

<span data-ttu-id="158a1-245">エラー シナリオでは、ビューがレンダリングされた、Html.TextBox() メソッドで ModelState コレクションは、Dinner オブジェクトの"EventDate"プロパティに関連付けられたすべてのエラーがないかどうがチェックされます。</span><span class="sxs-lookup"><span data-stu-id="158a1-245">When the view was rendered in the error scenario, the Html.TextBox() method checked the ModelState collection to see if there were any errors associated with the "EventDate" property of our Dinner object.</span></span> <span data-ttu-id="158a1-246">値として送信されたユーザー入力 ("BOGUS") を表示し、css エラー クラスを追加エラーがあったことが確認された場合、&lt;入力の種類 ="textbox"/&gt;が生成されたマークアップ。</span><span class="sxs-lookup"><span data-stu-id="158a1-246">When it determined that there was an error it rendered the submitted user input ("BOGUS") as the value, and added a css error class to the &lt;input type="textbox"/&gt; markup it generated:</span></span>

[!code-html[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample15.html)]

<span data-ttu-id="158a1-247">ただしすることを確認する css エラー クラスの外観をカスタマイズすることができます。</span><span class="sxs-lookup"><span data-stu-id="158a1-247">You can customize the appearance of the css error class to look however you want.</span></span> <span data-ttu-id="158a1-248">既定の CSS エラー クラス –「入力の検証エラー」-がで定義されている、 *\content\site.css*スタイル シートとは次のような。</span><span class="sxs-lookup"><span data-stu-id="158a1-248">The default CSS error class – "input-validation-error" – is defined in the *\content\site.css* stylesheet and looks like below:</span></span>

[!code-css[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample16.css)]

<span data-ttu-id="158a1-249">この CSS ルールでは、以下のように強調表示する、無効な入力要素の原因を示します。</span><span class="sxs-lookup"><span data-stu-id="158a1-249">This CSS rule is what caused our invalid input elements to be highlighted like below:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image10.png)

##### <a name="htmlvalidationmessage-helper-method"></a><span data-ttu-id="158a1-250">Html.ValidationMessage() ヘルパー メソッド</span><span class="sxs-lookup"><span data-stu-id="158a1-250">Html.ValidationMessage() Helper Method</span></span>

<span data-ttu-id="158a1-251">特定のモデル プロパティに関連付けられた ModelState エラー メッセージを出力する Html.ValidationMessage() ヘルパー メソッドを使用できます。</span><span class="sxs-lookup"><span data-stu-id="158a1-251">The Html.ValidationMessage() helper method can be used to output the ModelState error message associated with a particular model property:</span></span>

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample17.aspx)]

<span data-ttu-id="158a1-252">上記のコードを出力: *&lt;クラスにまたがる =「フィールドの検証エラー」&gt; 'BOGUS' 値が有効でない&lt;/span&gt;*</span><span class="sxs-lookup"><span data-stu-id="158a1-252">The above code outputs: *&lt;span class="field-validation-error"&gt; The value ‘BOGUS' is invalid&lt;/span&gt;*</span></span>

<span data-ttu-id="158a1-253">Html.ValidationMessage() のヘルパー メソッドには、表示されるエラー テキスト メッセージを上書きできるようにする 2 番目のパラメーターもサポートしています。</span><span class="sxs-lookup"><span data-stu-id="158a1-253">The Html.ValidationMessage() helper method also supports a second parameter that allows developers to override the error text message that is displayed:</span></span>

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample18.aspx)]

<span data-ttu-id="158a1-254">上記のコードを出力: <em>&lt;クラスにまたがる =「フィールドの検証エラー」&gt;\*&lt;/span&gt;</em>エラーの場合、既定のエラー テキストではなく、EventDate のプロパティです。</span><span class="sxs-lookup"><span data-stu-id="158a1-254">The above code outputs: <em>&lt;span class="field-validation-error"&gt;\*&lt;/span&gt;</em>instead of the default error text when an error is present for the EventDate property.</span></span>

##### <a name="htmlvalidationsummary-helper-method"></a><span data-ttu-id="158a1-255">Html.ValidationSummary() ヘルパー メソッド</span><span class="sxs-lookup"><span data-stu-id="158a1-255">Html.ValidationSummary() Helper Method</span></span>

<span data-ttu-id="158a1-256">Html.ValidationSummary() のヘルパー メソッドを伴う概要エラー メッセージを表示するために使用できます、 &lt;ul&gt;&lt;li/&gt;&lt;/ul&gt;でメッセージをすべて詳細なエラーの一覧、ModelState コレクション:</span><span class="sxs-lookup"><span data-stu-id="158a1-256">The Html.ValidationSummary() helper method can be used to render a summary error message, accompanied by a &lt;ul&gt;&lt;li/&gt;&lt;/ul&gt; list of all detailed error messages in the ModelState collection:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image11.png)

<span data-ttu-id="158a1-257">Html.ValidationSummary() のヘルパー メソッドでは、詳細なエラーの一覧の上に表示される概要エラー メッセージを定義する、省略可能な文字列パラメーターを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="158a1-257">The Html.ValidationSummary() helper method takes an optional string parameter – which defines a summary error message to display above the list of detailed errors:</span></span>

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample19.aspx)]

<span data-ttu-id="158a1-258">必要に応じて CSS を使用して、エラー一覧のようにオーバーライドすることができます。</span><span class="sxs-lookup"><span data-stu-id="158a1-258">You can optionally use CSS to override what the error list looks like.</span></span>

#### <a name="using-a-addruleviolations-helper-method"></a><span data-ttu-id="158a1-259">AddRuleViolations ヘルパー メソッドを使用してください。</span><span class="sxs-lookup"><span data-stu-id="158a1-259">Using a AddRuleViolations Helper Method</span></span>

<span data-ttu-id="158a1-260">最初の HTTP POST 編集実装では、Dinner オブジェクトの規則違反をループし、コント ローラーの ModelState のコレクションに追加するその catch ブロック内での foreach ステートメントを使用しました。</span><span class="sxs-lookup"><span data-stu-id="158a1-260">Our initial HTTP-POST Edit implementation used a foreach statement within its catch block to loop over the Dinner object's Rule Violations and add them to the controller's ModelState collection:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample20.cs)]

<span data-ttu-id="158a1-261">"ControllerHelpers"を追加することによってほとんど明確になり NerdDinner プロジェクトにクラスし、メソッドを実装して"AddRuleViolations"拡張機能内に ASP.NET MVC ModelStateDictionary クラスにヘルパー メソッドを追加する、このコードを実行できます。</span><span class="sxs-lookup"><span data-stu-id="158a1-261">We can make this code a little cleaner by adding a "ControllerHelpers" class to the NerdDinner project, and implement an "AddRuleViolations" extension method within it that adds a helper method to the ASP.NET MVC ModelStateDictionary class.</span></span> <span data-ttu-id="158a1-262">この拡張メソッドは、ModelStateDictionary RuleViolation エラーの一覧を設定するために必要なロジックをカプセル化できます。</span><span class="sxs-lookup"><span data-stu-id="158a1-262">This extension method can encapsulate the logic necessary to populate the ModelStateDictionary with a list of RuleViolation errors:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample21.cs)]

<span data-ttu-id="158a1-263">HTTP POST アクション メソッドの編集、Dinner 規則違反で ModelState のコレクションを設定するこの拡張メソッドを使用することを更新できます。</span><span class="sxs-lookup"><span data-stu-id="158a1-263">We can then update our HTTP-POST Edit action method to use this extension method to populate the ModelState collection with our Dinner Rule Violations.</span></span>

#### <a name="complete-edit-action-method-implementations"></a><span data-ttu-id="158a1-264">Edit アクション メソッドの実装を完了します。</span><span class="sxs-lookup"><span data-stu-id="158a1-264">Complete Edit Action Method Implementations</span></span>

<span data-ttu-id="158a1-265">次のコードは、編集シナリオに必要なコント ローラー ロジックのすべてを実装します。</span><span class="sxs-lookup"><span data-stu-id="158a1-265">The code below implements all of the controller logic necessary for our Edit scenario:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample22.cs)]

<span data-ttu-id="158a1-266">編集実装の便利な点は、コント ローラー クラスでも、テンプレートの表示が、特定の検証や、Dinner モデルに設定されているビジネス ルールについての知識が。</span><span class="sxs-lookup"><span data-stu-id="158a1-266">The nice thing about our Edit implementation is that neither our Controller class nor our View template has to know anything about the specific validation or business rules being enforced by our Dinner model.</span></span> <span data-ttu-id="158a1-267">追加ルールを追加できます、モデル、今後おに、コント ローラーまたはビューをサポートするためにコード変更を加える必要はありません。</span><span class="sxs-lookup"><span data-stu-id="158a1-267">We can add additional rules to our model in the future and do not have to make any code changes to our controller or view in order for them to be supported.</span></span> <span data-ttu-id="158a1-268">これにより、アプリケーションの要件、将来を最小限のコードの変更を簡単に進化する柔軟性を活用できます。</span><span class="sxs-lookup"><span data-stu-id="158a1-268">This provides us with the flexibility to easily evolve our application requirements in the future with a minimum of code changes.</span></span>

### <a name="create-support"></a><span data-ttu-id="158a1-269">サポートを作成します。</span><span class="sxs-lookup"><span data-stu-id="158a1-269">Create Support</span></span>

<span data-ttu-id="158a1-270">DinnersController クラスの"Edit"の動作を実装するは終了しました。</span><span class="sxs-lookup"><span data-stu-id="158a1-270">We've finished implementing the "Edit" behavior of our DinnersController class.</span></span> <span data-ttu-id="158a1-271">今すぐに進みましょうが – 新しい Dinners を追加するユーザーを有効にする「作成」のサポートを実装します。</span><span class="sxs-lookup"><span data-stu-id="158a1-271">Let's now move on to implement the "Create" support on it – which will enable users to add new Dinners.</span></span>

#### <a name="the-http-get-create-action-method"></a><span data-ttu-id="158a1-272">HTTP GET アクション メソッドを作成します。</span><span class="sxs-lookup"><span data-stu-id="158a1-272">The HTTP-GET Create Action Method</span></span>

<span data-ttu-id="158a1-273">まずの HTTP"GET"動作を実装することによって、アクション メソッドを作成します。</span><span class="sxs-lookup"><span data-stu-id="158a1-273">We'll begin by implementing the HTTP "GET" behavior of our create action method.</span></span> <span data-ttu-id="158a1-274">アクセスと、このメソッドが呼び出される、 */Dinners/作成*URL。</span><span class="sxs-lookup"><span data-stu-id="158a1-274">This method will be called when someone visits the */Dinners/Create* URL.</span></span> <span data-ttu-id="158a1-275">今回の実装は、ようになります。</span><span class="sxs-lookup"><span data-stu-id="158a1-275">Our implementation looks like:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample23.cs)]

<span data-ttu-id="158a1-276">上記のコードでは、新しい Dinner オブジェクトを作成し、今後 1 週間に、EventDate のプロパティを割り当てます。</span><span class="sxs-lookup"><span data-stu-id="158a1-276">The code above creates a new Dinner object, and assigns its EventDate property to be one week in the future.</span></span> <span data-ttu-id="158a1-277">新しい Dinner オブジェクトに基づいているビューをレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="158a1-277">It then renders a View that is based on the new Dinner object.</span></span> <span data-ttu-id="158a1-278">名を明示的に渡されたしていないため、 *View()* ヘルパー メソッドをテンプレートの表示を解決するのには規約ベースの既定パスを使用には:/Views/Dinners/Create.aspx します。</span><span class="sxs-lookup"><span data-stu-id="158a1-278">Because we haven't explicitly passed a name to the *View()* helper method, it will use the convention based default path to resolve the view template: /Views/Dinners/Create.aspx.</span></span>

<span data-ttu-id="158a1-279">このテンプレートの表示を今すぐ作成しましょう。</span><span class="sxs-lookup"><span data-stu-id="158a1-279">Let's now create this view template.</span></span> <span data-ttu-id="158a1-280">これは、Create アクション メソッド内で右クリックし、「ビューの追加」コンテキスト メニュー コマンドを選択して実行できます。</span><span class="sxs-lookup"><span data-stu-id="158a1-280">We can do this by right-clicking within the Create action method and selecting the "Add View" context menu command.</span></span> <span data-ttu-id="158a1-281">"ビューの追加 ダイアログ ボックスで指定する Dinner オブジェクト、ビュー テンプレートに渡すし、自動スキャフォールディングの「作成」テンプレートを選択します。</span><span class="sxs-lookup"><span data-stu-id="158a1-281">Within the "Add View" dialog we'll indicate that we are passing a Dinner object to the view template, and choose to auto-scaffold a "Create" template:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image12.png)

<span data-ttu-id="158a1-282">[追加] ボタンをクリックすると、ときに、Visual Studio は新しいスキャフォールディング ベース"Create.aspx"ビューを"\Views\Dinners"ディレクトリに保存し、ide の起動。</span><span class="sxs-lookup"><span data-stu-id="158a1-282">When we click the "Add" button, Visual Studio will save a new scaffold-based "Create.aspx" view to the "\Views\Dinners" directory, and open it up within the IDE:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image13.png)

<span data-ttu-id="158a1-283">既定値「作成」スキャフォールディングの生成されたファイルに、いくつかの変更を加えるし、以下のように変更をしましょう。</span><span class="sxs-lookup"><span data-stu-id="158a1-283">Let's make a few changes to the default "create" scaffold file that was generated for us, and modify it up to look like below:</span></span>

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample24.aspx)]

<span data-ttu-id="158a1-284">今すぐ実行するとき、アプリケーションとアクセスと、 *「Dinners/作成」* 作成アクションの実装から次のような UI を表示するブラウザー内の URL:</span><span class="sxs-lookup"><span data-stu-id="158a1-284">And now when we run our application and access the *"/Dinners/Create"* URL within the browser it will render UI like below from our Create action implementation:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image14.png)

#### <a name="implementing-the-http-post-create-action-method"></a><span data-ttu-id="158a1-285">アクション メソッドを作成する HTTP POST を実装します。</span><span class="sxs-lookup"><span data-stu-id="158a1-285">Implementing the HTTP-POST Create Action Method</span></span>

<span data-ttu-id="158a1-286">私たちには、実装、Create アクション メソッドの HTTP GET バージョンがインストールされています。</span><span class="sxs-lookup"><span data-stu-id="158a1-286">We have the HTTP-GET version of our Create action method implemented.</span></span> <span data-ttu-id="158a1-287">フォーム post を実行、ユーザーが [保存] ボタンをクリックすると、 */Dinners/作成*URL、HTML を送信して&lt;入力&gt;HTTP POST 動詞を使用して値を形成します。</span><span class="sxs-lookup"><span data-stu-id="158a1-287">When a user clicks the "Save" button it performs a form post to the */Dinners/Create* URL, and submits the HTML &lt;input&gt; form values using the HTTP POST verb.</span></span>

<span data-ttu-id="158a1-288">ここでの HTTP POST の動作を実装してみましょう、アクション メソッドを作成します。</span><span class="sxs-lookup"><span data-stu-id="158a1-288">Let's now implement the HTTP POST behavior of our create action method.</span></span> <span data-ttu-id="158a1-289">まず、DinnersController を"AcceptVerbs"属性を持つ HTTP POST のシナリオを扱うことを示すをオーバー ロードされたの"Create"のアクション メソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="158a1-289">We'll begin by adding an overloaded "Create" action method to our DinnersController that has an "AcceptVerbs" attribute on it that indicates it handles HTTP POST scenarios:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample25.cs)]

<span data-ttu-id="158a1-290">これは、さまざまな方法は、HTTP POST が有効になっている"Create"メソッド内で、ポストされたフォーム パラメーターにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="158a1-290">There are a variety of ways we can access the posted form parameters within our HTTP-POST enabled "Create" method.</span></span>

<span data-ttu-id="158a1-291">1 つのアプローチは、新しい Dinner オブジェクトを作成し、使用する、 *UpdateModel()* (編集の操作を使用したとき) などのヘルパー メソッドにポストされたフォーム値を設定します。</span><span class="sxs-lookup"><span data-stu-id="158a1-291">One approach is to create a new Dinner object and then use the *UpdateModel()* helper method (like we did with the Edit action) to populate it with the posted form values.</span></span> <span data-ttu-id="158a1-292">私たちのときは必ず DinnerRepository に追加、データベースに保存し、次のコードを使用して、新しく作成された Dinner を表示するには、当社の詳細アクションにユーザーをリダイレクトします。、</span><span class="sxs-lookup"><span data-stu-id="158a1-292">We can then add it to our DinnerRepository, persist it to the database, and redirect the user to our Details action to show the newly created Dinner using the code below:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample26.cs)]

<span data-ttu-id="158a1-293">または、アプローチを使用しました、メソッド パラメーターとして Dinner オブジェクト Create() アクション メソッドは、あります。</span><span class="sxs-lookup"><span data-stu-id="158a1-293">Alternatively, we can use an approach where we have our Create() action method take a Dinner object as a method parameter.</span></span> <span data-ttu-id="158a1-294">ASP.NET MVC は自動的に私たちにとって新しい Dinner オブジェクトをインスタンス化、フォームの入力を使用してそのプロパティを設定し、アクション メソッドに渡します。</span><span class="sxs-lookup"><span data-stu-id="158a1-294">ASP.NET MVC will then automatically instantiate a new Dinner object for us, populate its properties using the form inputs, and pass it to our action method:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample27.cs)]

<span data-ttu-id="158a1-295">ある Dinner オブジェクトが正常に値が設定されて、フォーム post ModelState.IsValid プロパティをチェックして、上記のアクション メソッドを確認します。</span><span class="sxs-lookup"><span data-stu-id="158a1-295">Our action method above verifies that the Dinner object has been successfully populated with the form post values by checking the ModelState.IsValid property.</span></span> <span data-ttu-id="158a1-296">変換の問題の入力がある場合は false が返されます (例: EventDate のプロパティの"BOGUS"の文字列)、し、問題がある場合、アクション メソッドはフォームを再表示します。</span><span class="sxs-lookup"><span data-stu-id="158a1-296">This will return false if there are input conversion issues (for example: a string of "BOGUS" for the EventDate property), and if there are any issues our action method redisplays the form.</span></span>

<span data-ttu-id="158a1-297">入力値が有効な場合、アクション メソッドは、追加し、新しい夕食をときは必ず DinnerRepository に保存を試みます。</span><span class="sxs-lookup"><span data-stu-id="158a1-297">If the input values are valid, then the action method attempts to add and save the new Dinner to the DinnerRepository.</span></span> <span data-ttu-id="158a1-298">Try/catch ブロック内でこの作業をラップし、(例外を発生させる dinnerRepository.Save() メソッドになる) ため、ビジネス ルール違反がある場合は、フォームを再表示します。</span><span class="sxs-lookup"><span data-stu-id="158a1-298">It wraps this work within a try/catch block and redisplays the form if there are any business rule violations (which would cause the dinnerRepository.Save() method to raise an exception).</span></span>

<span data-ttu-id="158a1-299">このエラーの処理の動作の動作を確認することを要求できます、 */Dinners/作成*URL と新しい夕食について詳細に記入します。</span><span class="sxs-lookup"><span data-stu-id="158a1-299">To see this error handling behavior in action, we can request the */Dinners/Create* URL and fill out details about a new Dinner.</span></span> <span data-ttu-id="158a1-300">不適切な入力または値には、次のように強調表示されたエラーと共に再表示するフォームを作成するが発生します。</span><span class="sxs-lookup"><span data-stu-id="158a1-300">Incorrect input or values will cause the create form to be redisplayed with the errors highlighted like below:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image15.png)

<span data-ttu-id="158a1-301">フォームを作成するによって、編集フォームとして正確な同じ検証規則とビジネス規則が受け入れることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="158a1-301">Notice how our Create form is honoring the exact same validation and business rules as our Edit form.</span></span> <span data-ttu-id="158a1-302">これは、検証とビジネス ルールがモデルでは、定義された、UI またはアプリケーションのコント ローラー内で埋め込まれていないためです。</span><span class="sxs-lookup"><span data-stu-id="158a1-302">This is because our validation and business rules were defined in the model, and were not embedded within the UI or controller of the application.</span></span> <span data-ttu-id="158a1-303">これは後で変更/進化させる、検証、または 1 つのビジネス ルールに置き、アプリケーション全体で適用することがあることを意味します。</span><span class="sxs-lookup"><span data-stu-id="158a1-303">This means we can later change/evolve our validation or business rules in a single place and have them apply throughout our application.</span></span> <span data-ttu-id="158a1-304">私たちは、内いずれかのコードは、編集を変更したり、新しいルールまたは既存の変更を自動的に優先するアクション メソッドを作成する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="158a1-304">We will not have to change any code within either our Edit or Create action methods to automatically honor any new rules or modifications to existing ones.</span></span>

<span data-ttu-id="158a1-305">入力の値を修正し、[保存] ボタンをクリックします。 もう一度、追加するときは、必ず DinnerRepository は成功しますと新しい夕食をデータベースに追加されます。</span><span class="sxs-lookup"><span data-stu-id="158a1-305">When we fix the input values and click the "Save" button again, our addition to the DinnerRepository will succeed, and a new Dinner will be added to the database.</span></span> <span data-ttu-id="158a1-306">ここにリダイレクトされますが、 */Dinners 詳細/[id]* URL – の詳細については、新しく作成された Dinner が、その場所表示されます。</span><span class="sxs-lookup"><span data-stu-id="158a1-306">We will then be redirected to the */Dinners/Details/[id]* URL – where we will be presented with details about the newly created Dinner:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image16.png)

### <a name="delete-support"></a><span data-ttu-id="158a1-307">Delete のサポート</span><span class="sxs-lookup"><span data-stu-id="158a1-307">Delete Support</span></span>

<span data-ttu-id="158a1-308">DinnersController を「削除」のサポートを今すぐ追加してみましょう。</span><span class="sxs-lookup"><span data-stu-id="158a1-308">Let's now add "Delete" support to our DinnersController.</span></span>

#### <a name="the-http-get-delete-action-method"></a><span data-ttu-id="158a1-309">Delete アクション メソッドを HTTP GET</span><span class="sxs-lookup"><span data-stu-id="158a1-309">The HTTP-GET Delete Action Method</span></span>

<span data-ttu-id="158a1-310">まず、HTTP GET、delete アクション メソッドの動作を実装します。</span><span class="sxs-lookup"><span data-stu-id="158a1-310">We'll begin by implementing the HTTP GET behavior of our delete action method.</span></span> <span data-ttu-id="158a1-311">このメソッドへのアクセス時に呼び出される、 */Dinners/削除/[id]* URL。</span><span class="sxs-lookup"><span data-stu-id="158a1-311">This method will get called when someone visits the */Dinners/Delete/[id]* URL.</span></span> <span data-ttu-id="158a1-312">実装を次に示します。</span><span class="sxs-lookup"><span data-stu-id="158a1-312">Below is the implementation:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample28.cs)]

<span data-ttu-id="158a1-313">アクション メソッドは、削除する Dinner の取得を試みます。</span><span class="sxs-lookup"><span data-stu-id="158a1-313">The action method attempts to retrieve the Dinner to be deleted.</span></span> <span data-ttu-id="158a1-314">Dinner が存在する場合、レンダリング Dinner オブジェクトにビューを基づいています。</span><span class="sxs-lookup"><span data-stu-id="158a1-314">If the Dinner exists it renders a View based on the Dinner object.</span></span> <span data-ttu-id="158a1-315">オブジェクトが存在しない (または既に削除されている) 場合、"NotFound"をレンダリングするビューを返しますが、"Details"アクション メソッドの前に作成したテンプレートを表示します。</span><span class="sxs-lookup"><span data-stu-id="158a1-315">If the object doesn't exist (or has already been deleted) it returns a View that renders the "NotFound" view template we created earlier for our "Details" action method.</span></span>

<span data-ttu-id="158a1-316">Delete アクション メソッド内で右クリックし、「ビューの追加」コンテキスト メニュー コマンドを選択して、「削除」ビュー テンプレートを作成できます。</span><span class="sxs-lookup"><span data-stu-id="158a1-316">We can create the "Delete" view template by right-clicking within the Delete action method and selecting the "Add View" context menu command.</span></span> <span data-ttu-id="158a1-317">"ビューの追加 ダイアログ ボックスで Dinner オブジェクトとしてのモデル、ビュー テンプレートに渡すと、空のテンプレートを作成することを指定します。</span><span class="sxs-lookup"><span data-stu-id="158a1-317">Within the "Add View" dialog we'll indicate that we are passing a Dinner object to our view template as its model, and choose to create an empty template:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image17.png)

<span data-ttu-id="158a1-318">[追加] ボタンをクリックするとと、Visual Studio は新しい"Delete.aspx"ビュー テンプレート ファイルを"\Views\Dinners"ディレクトリ内の追加します。</span><span class="sxs-lookup"><span data-stu-id="158a1-318">When we click the "Add" button, Visual Studio will add a new "Delete.aspx" view template file for us within our "\Views\Dinners" directory.</span></span> <span data-ttu-id="158a1-319">削除の確認 画面を実装するためにテンプレートをいくつかの HTML とコードを追加します次のような。</span><span class="sxs-lookup"><span data-stu-id="158a1-319">We'll add some HTML and code to the template to implement a delete confirmation screen like below:</span></span>

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample29.aspx)]

<span data-ttu-id="158a1-320">上記のコードは、削除するには、Dinner と出力のタイトルを表示、&lt;フォーム&gt;要素を/Dinners/削除/[id] URL に投稿を行う場合は、エンドユーザーがその中の [削除] ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="158a1-320">The code above displays the title of the Dinner to be deleted, and outputs a &lt;form&gt; element that does a POST to the /Dinners/Delete/[id] URL if the end-user clicks the "Delete" button within it.</span></span>

<span data-ttu-id="158a1-321">当社のアプリケーションとアクセスを実行したときに、 *"/Dinners/削除/[id]"* オブジェクトの有効な夕食 URL 次のような UI をレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="158a1-321">When we run our application and access the *"/Dinners/Delete/[id]"* URL for a valid Dinner object it renders UI like below:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image18.png)

| <span data-ttu-id="158a1-322">**側のトピックは、理由をしている投稿ですか。**</span><span class="sxs-lookup"><span data-stu-id="158a1-322">**Side Topic: Why are we doing a POST?**</span></span> |
| --- |
| <span data-ttu-id="158a1-323">作成する作業を通じて移動でした理由: 依頼可能性があります、&lt;フォーム&gt;削除の確認画面内か?</span><span class="sxs-lookup"><span data-stu-id="158a1-323">You might ask – why did we go through the effort of creating a &lt;form&gt; within our Delete confirmation screen?</span></span> <span data-ttu-id="158a1-324">なぜ実際の削除操作を実行するアクション メソッドにリンクするだけの標準のハイパーリンクを使用しますか。</span><span class="sxs-lookup"><span data-stu-id="158a1-324">Why not just use a standard hyperlink to link to an action method that does the actual delete operation?</span></span> <span data-ttu-id="158a1-325">Web クローラーからの保護し、検索エンジン、Url を探索し、リンクに従っている場合に削除されるデータを誤って原因に注意する必要があるためにです。</span><span class="sxs-lookup"><span data-stu-id="158a1-325">The reason is because we want to be careful to guard against web-crawlers and search engines discovering our URLs and inadvertently causing data to be deleted when they follow the links.</span></span> <span data-ttu-id="158a1-326">HTTP GET ベースの Url アクセス/クロールにするには「安全」と見なされに、HTTP POST ものに従っていません。</span><span class="sxs-lookup"><span data-stu-id="158a1-326">HTTP-GET based URLs are considered "safe" for them to access/crawl, and they are supposed to not follow HTTP-POST ones.</span></span> <span data-ttu-id="158a1-327">HTTP POST 要求の背後にあるデータ変更操作または破壊を常に配置することを確認することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="158a1-327">A good rule is to make sure you always put destructive or data modifying operations behind HTTP-POST requests.</span></span> |

#### <a name="implementing-the-http-post-delete-action-method"></a><span data-ttu-id="158a1-328">Delete アクション メソッドを HTTP POST を実装します。</span><span class="sxs-lookup"><span data-stu-id="158a1-328">Implementing the HTTP-POST Delete Action Method</span></span>

<span data-ttu-id="158a1-329">HTTP GET、Delete アクション メソッドの実装のバージョンが削除の確認 画面を表示しますがあるようになりました。</span><span class="sxs-lookup"><span data-stu-id="158a1-329">We now have the HTTP-GET version of our Delete action method implemented which displays a delete confirmation screen.</span></span> <span data-ttu-id="158a1-330">フォーム post が実行されます、エンドユーザーが [削除] ボタンをクリックすると、 */Dinners Dinner/[id]* URL。</span><span class="sxs-lookup"><span data-stu-id="158a1-330">When an end user clicks the "Delete" button it will perform a form post to the */Dinners/Dinner/[id]* URL.</span></span>

<span data-ttu-id="158a1-331">みましょう delete アクション メソッドを次のコードを使用して HTTP"POST"動作を実装するようになりました。</span><span class="sxs-lookup"><span data-stu-id="158a1-331">Let's now implement the HTTP "POST" behavior of the delete action method using the code below:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample30.cs)]

<span data-ttu-id="158a1-332">Delete アクション メソッドを当社の HTTP POST のバージョンは、削除する dinner オブジェクトを取得しようとします。</span><span class="sxs-lookup"><span data-stu-id="158a1-332">The HTTP-POST version of our Delete action method attempts to retrieve the dinner object to delete.</span></span> <span data-ttu-id="158a1-333">(既に削除されている) ために検出できない場合、"NotFound"テンプレートを表示します。</span><span class="sxs-lookup"><span data-stu-id="158a1-333">If it can't find it (because it has already been deleted) it renders our "NotFound" template.</span></span> <span data-ttu-id="158a1-334">Dinner を検出した場合、ときは必ず DinnerRepository から削除します。</span><span class="sxs-lookup"><span data-stu-id="158a1-334">If it finds the Dinner, it deletes it from the DinnerRepository.</span></span> <span data-ttu-id="158a1-335">「削除済み」テンプレートをレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="158a1-335">It then renders a "Deleted" template.</span></span>

<span data-ttu-id="158a1-336">「削除済み」テンプレートを実装するには、アクション メソッドで右クリックし、「ビューの追加」コンテキスト メニューを選択してします。</span><span class="sxs-lookup"><span data-stu-id="158a1-336">To implement the "Deleted" template we'll right-click in the action method and choose the "Add View" context menu.</span></span> <span data-ttu-id="158a1-337">このビューの「削除済み」という名前がされ空のテンプレート (および厳密に型指定されたモデル オブジェクトにならない) ことがあります。</span><span class="sxs-lookup"><span data-stu-id="158a1-337">We'll name our view "Deleted" and have it be an empty template (and not take a strongly-typed model object).</span></span> <span data-ttu-id="158a1-338">いくつかの HTML コンテンツを追加します。</span><span class="sxs-lookup"><span data-stu-id="158a1-338">We'll then add some HTML content to it:</span></span>

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample31.aspx)]

<span data-ttu-id="158a1-339">今すぐ実行するとき、アプリケーションとアクセスと、 *"/Dinners/削除/[id]"* 夕食を表示するオブジェクトの削除の確認、有効な Dinner の URL は以下のような画面します。</span><span class="sxs-lookup"><span data-stu-id="158a1-339">And now when we run our application and access the *"/Dinners/Delete/[id]"* URL for a valid Dinner object it will render our Dinner delete confirmation screen like below:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image19.png)

<span data-ttu-id="158a1-340">[削除] ボタンをクリックしたときに HTTP POST が実行されます、 */Dinners/削除/[id]* Dinner をデータベースから削除され、表示、「削除済み」のビュー テンプレートの URL:</span><span class="sxs-lookup"><span data-stu-id="158a1-340">When we click the "Delete" button it will perform an HTTP-POST to the */Dinners/Delete/[id]* URL, which will delete the Dinner from our database, and display our "Deleted" view template:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image20.png)

### <a name="model-binding-security"></a><span data-ttu-id="158a1-341">モデル バインディングのセキュリティ</span><span class="sxs-lookup"><span data-stu-id="158a1-341">Model Binding Security</span></span>

<span data-ttu-id="158a1-342">ASP.NET MVC の組み込みのモデル バインド機能を使用する 2 つの方法を説明しました。</span><span class="sxs-lookup"><span data-stu-id="158a1-342">We've discussed two different ways to use the built-in model-binding features of ASP.NET MVC.</span></span> <span data-ttu-id="158a1-343">最初 UpdateModel() メソッドを使用して、既存のモデル オブジェクトのプロパティを更新して、2 つ目を使用してアクション メソッドのパラメーターとしてのモデル オブジェクトを渡すための ASP.NET MVC のサポート。</span><span class="sxs-lookup"><span data-stu-id="158a1-343">The first using the UpdateModel() method to update properties on an existing model object, and the second using ASP.NET MVC's support for passing model objects in as action method parameters.</span></span> <span data-ttu-id="158a1-344">これらのテクニックは、非常に強力で非常に便利です。</span><span class="sxs-lookup"><span data-stu-id="158a1-344">Both of these techniques are very powerful and extremely useful.</span></span>

<span data-ttu-id="158a1-345">この power も伴います。 この責任です。</span><span class="sxs-lookup"><span data-stu-id="158a1-345">This power also brings with it responsibility.</span></span> <span data-ttu-id="158a1-346">すべてのユーザー入力を受け入れるときにセキュリティに関する妄想常にすることが重要とフォームの入力にオブジェクトをバインドする場合は true。 これはもします。</span><span class="sxs-lookup"><span data-stu-id="158a1-346">It is important to always be paranoid about security when accepting any user input, and this is also true when binding objects to form input.</span></span> <span data-ttu-id="158a1-347">ように注意する必要があります常に、HTML エンコードを HTML および JavaScript インジェクション攻撃を回避し、SQL インジェクション攻撃注意が必要なユーザーが入力した値 (注: このアプリケーションでは、これらを防ぐためにパラメーターを自動的にエンコード LINQ to SQL を使用しています種類の攻撃)。</span><span class="sxs-lookup"><span data-stu-id="158a1-347">You should be careful to always HTML encode any user-entered values to avoid HTML and JavaScript injection attacks, and be careful of SQL injection attacks (note: we are using LINQ to SQL for our application, which automatically encodes parameters to prevent these types of attacks).</span></span> <span data-ttu-id="158a1-348">偽の値を送信しようとするハッカーから保護するためのサーバー側の検証を常に採用できるにクライアント側の検証だけで、依存する必要があることはありません。</span><span class="sxs-lookup"><span data-stu-id="158a1-348">You should never rely on client-side validation alone, and always employ server-side validation to guard against hackers attempting to send you bogus values.</span></span>

<span data-ttu-id="158a1-349">ASP.NET MVC のバインド機能を使用する場合について検討するかどうかを確認する 1 つの追加のセキュリティ項目は、バインドしているオブジェクトのスコープです。</span><span class="sxs-lookup"><span data-stu-id="158a1-349">One additional security item to make sure you think about when using the binding features of ASP.NET MVC is the scope of the objects you are binding.</span></span> <span data-ttu-id="158a1-350">具体的には、バインド、および更新するには、エンドユーザーが更新可能な実際にする必要があるこれらのプロパティのみを許可するかどうかを確認することが許可されているプロパティのセキュリティの影響を理解することを確認します。</span><span class="sxs-lookup"><span data-stu-id="158a1-350">Specifically, you want to make sure you understand the security implications of the properties you are allowing to be bound, and make sure you only allow those properties that really should be updatable by an end-user to be updated.</span></span>

<span data-ttu-id="158a1-351">既定では、UpdateModel() メソッドは受信フォーム パラメーターの値に一致するモデル オブジェクトのすべてのプロパティの更新を試みます。</span><span class="sxs-lookup"><span data-stu-id="158a1-351">By default, the UpdateModel() method will attempt to update all properties on the model object that match incoming form parameter values.</span></span> <span data-ttu-id="158a1-352">同様に、または既定では、アクション メソッドのパラメーターとして渡されたオブジェクトすべてのプロパティはフォーム パラメーターを使用して設定のことができます。</span><span class="sxs-lookup"><span data-stu-id="158a1-352">Likewise, objects passed as action method parameters also by default can have all of their properties set via form parameters.</span></span>

#### <a name="locking-down-binding-on-a-per-usage-basis"></a><span data-ttu-id="158a1-353">あたり使用量ベースのバインディングのロックダウン</span><span class="sxs-lookup"><span data-stu-id="158a1-353">Locking down binding on a per-usage basis</span></span>

<span data-ttu-id="158a1-354">提供する、明示的な「インクルード リスト」を更新できるプロパティのあたり使用量ベースのバインディング ポリシーをロックダウンすることができます。</span><span class="sxs-lookup"><span data-stu-id="158a1-354">You can lock down the binding policy on a per-usage basis by providing an explicit "include list" of properties that can be updated.</span></span> <span data-ttu-id="158a1-355">これは、次のような UpdateModel() メソッドに余分な文字列の配列パラメーターを渡すことによって実行できます。</span><span class="sxs-lookup"><span data-stu-id="158a1-355">This can be done by passing an extra string array parameter to the UpdateModel() method like below:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample32.cs)]

<span data-ttu-id="158a1-356">「インクルード リスト」のプロパティを次のように指定できるを許可できるようにする [バインド] 属性をサポートしても、アクション メソッドのパラメーターとして渡されるオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="158a1-356">Objects passed as action method parameters also support a [Bind] attribute that enables an "include list" of allowed properties to be specified like below:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample33.cs)]

#### <a name="locking-down-binding-on-a-type-basis"></a><span data-ttu-id="158a1-357">バインドの種類ごとにロックダウン</span><span class="sxs-lookup"><span data-stu-id="158a1-357">Locking down binding on a type basis</span></span>

<span data-ttu-id="158a1-358">型ごとにバインディング規則をロックダウンすることもできます。</span><span class="sxs-lookup"><span data-stu-id="158a1-358">You can also lock down the binding rules on a per-type basis.</span></span> <span data-ttu-id="158a1-359">これにより、1 回、バインディング規則を指定し、それらすべてのコント ローラーとアクション メソッド間で (UpdateModel とアクションの両方のメソッド パラメーターのシナリオを含む) すべてのシナリオで適用できます。</span><span class="sxs-lookup"><span data-stu-id="158a1-359">This allows you to specify the binding rules once, and then have them apply in all scenarios (including both UpdateModel and action method parameter scenarios) across all controllers and action methods.</span></span>

<span data-ttu-id="158a1-360">型の場合に、[バインド] 属性を追加することで、または (種類を所有していない場合に便利です)、アプリケーションの Global.asax ファイル内で登録することによって、型のバインディング規則をカスタマイズできます。</span><span class="sxs-lookup"><span data-stu-id="158a1-360">You can customize the per-type binding rules by adding a [Bind] attribute onto a type, or by registering it within the Global.asax file of the application (useful for scenarios where you don't own the type).</span></span> <span data-ttu-id="158a1-361">プロパティを制御するプロパティを除外するは、特定のクラスまたはインターフェイスのバインド可能なとをバインド属性のインクルードを使用できます。</span><span class="sxs-lookup"><span data-stu-id="158a1-361">You can then use the Bind attribute's Include and Exclude properties to control which properties are bindable for the particular class or interface.</span></span>

<span data-ttu-id="158a1-362">NerdDinner アプリケーションでは、Dinner クラスのこの手法を使用して、され、次のバインド可能なプロパティの一覧を制限することを [Bind] 属性を追加します。</span><span class="sxs-lookup"><span data-stu-id="158a1-362">We'll use this technique for the Dinner class in our NerdDinner application, and add a [Bind] attribute to it that restricts the list of bindable properties to the following:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample34.cs)]

<span data-ttu-id="158a1-363">バインディング、によって操作される予約したコレクションが許可されていないことも、DinnerID または HostedBy プロパティのバインドを使用して設定が許可されていることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="158a1-363">Notice we are not allowing the RSVPs collection to be manipulated via binding, nor are we allowing the DinnerID or HostedBy properties to be set via binding.</span></span> <span data-ttu-id="158a1-364">セキュリティ上の理由からを代わりにのみ操作、アクション メソッド内で明示的なコードを使用してこれらの特定のプロパティ。</span><span class="sxs-lookup"><span data-stu-id="158a1-364">For security reasons we'll instead only manipulate these particular properties using explicit code within our action methods.</span></span>

### <a name="crud-wrap-up"></a><span data-ttu-id="158a1-365">CRUD のまとめ</span><span class="sxs-lookup"><span data-stu-id="158a1-365">CRUD Wrap-Up</span></span>

<span data-ttu-id="158a1-366">ASP.NET MVC には、さまざまなフォーム ポスト シナリオの実装に役立つ組み込みの機能が含まれています。</span><span class="sxs-lookup"><span data-stu-id="158a1-366">ASP.NET MVC includes a number of built-in features that help with implementing form posting scenarios.</span></span> <span data-ttu-id="158a1-367">このときは必ず DinnerRepository 上に CRUD UI サポートを提供するさまざまなこれらの機能を使いました。</span><span class="sxs-lookup"><span data-stu-id="158a1-367">We used a variety of these features to provide CRUD UI support on top of our DinnerRepository.</span></span>

<span data-ttu-id="158a1-368">モデルに重点を置いたアプローチは、アプリケーションを実装するために使用します。</span><span class="sxs-lookup"><span data-stu-id="158a1-368">We are using a model-focused approach to implement our application.</span></span> <span data-ttu-id="158a1-369">つまり、すべての検証とビジネス ルールのロジック、および、コント ローラーまたはビュー内ではなく、モデル レイヤー内で定義されます。</span><span class="sxs-lookup"><span data-stu-id="158a1-369">This means that all our validation and business rule logic is defined within our model layer – and not within our controllers or views.</span></span> <span data-ttu-id="158a1-370">コント ローラー クラスでも、テンプレートの表示、Dinner モデル クラスに設定されている特定のビジネス ルールに関する知識します。</span><span class="sxs-lookup"><span data-stu-id="158a1-370">Neither our Controller class nor our View templates know anything about the specific business rules being enforced by our Dinner model class.</span></span>

<span data-ttu-id="158a1-371">アプリケーション アーキテクチャをクリーンに維持を容易にテストします。</span><span class="sxs-lookup"><span data-stu-id="158a1-371">This will keep our application architecture clean and make it easier to test.</span></span> <span data-ttu-id="158a1-372">他のビジネス ルールを将来、モデル レイヤーに追加しましたできますと*コード変更を加える必要がなく*をサポートするのには、コント ローラーまたはビューでそれらの順番にします。</span><span class="sxs-lookup"><span data-stu-id="158a1-372">We can add additional business rules to our model layer in the future and *not have to make any code changes* to our Controller or View in order for them to be supported.</span></span> <span data-ttu-id="158a1-373">これで、大きな進化し、アプリケーションに将来の変更に機敏性を提供します。</span><span class="sxs-lookup"><span data-stu-id="158a1-373">This is going to provide us with a great deal of agility to evolve and change our application in the future.</span></span>

<span data-ttu-id="158a1-374">DinnersController ようになりましたにより、Dinner 番組/詳細、だけでなくを作成、編集および削除のサポート。</span><span class="sxs-lookup"><span data-stu-id="158a1-374">Our DinnersController now enables Dinner listings/details, as well as create, edit and delete support.</span></span> <span data-ttu-id="158a1-375">クラスの完全なコードは、以下をあります。</span><span class="sxs-lookup"><span data-stu-id="158a1-375">The complete code for the class can be found below:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample35.cs)]

### <a name="next-step"></a><span data-ttu-id="158a1-376">次の手順</span><span class="sxs-lookup"><span data-stu-id="158a1-376">Next Step</span></span>

<span data-ttu-id="158a1-377">基本的な CRUD (作成、読み取り、更新、削除) サポートが、DinnersController クラス内で実装があるようになりました。</span><span class="sxs-lookup"><span data-stu-id="158a1-377">We now have basic CRUD (Create, Read, Update and Delete) support implement within our DinnersController class.</span></span>

<span data-ttu-id="158a1-378">これで、ViewData とビューモデル クラスを使用できます、フォームでより豊富な UI を有効にする方法を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="158a1-378">Let's now look at how we can use ViewData and ViewModel classes to enable even richer UI on our forms.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="158a1-379">[前へ](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
> [次へ](use-viewdata-and-implement-viewmodel-classes.md)</span><span class="sxs-lookup"><span data-stu-id="158a1-379">[Previous](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
[Next](use-viewdata-and-implement-viewmodel-classes.md)</span></span>
