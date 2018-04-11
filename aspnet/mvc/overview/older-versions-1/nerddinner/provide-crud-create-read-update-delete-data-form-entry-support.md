---
uid: mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
title: 提供 CRUD (作成、読み取り、更新、削除) データ フォーム エントリ サポート |Microsoft ドキュメント
author: microsoft
description: 手順 5 では、編集、作成、および関連付けディナーを削除すると、同様のサポートを有効にし、さらに、DinnersController クラスを得る方法を示します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: bbb976e5-6150-4283-a374-c22fbafe29f5
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
msc.type: authoredcontent
ms.openlocfilehash: bd906282db5c620476966ffbe09cecb5ade66ee4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="provide-crud-create-read-update-delete-data-form-entry-support"></a><span data-ttu-id="9894d-103">提供 CRUD (作成、読み取り、更新、削除) データ フォームのエントリのサポート</span><span class="sxs-lookup"><span data-stu-id="9894d-103">Provide CRUD (Create, Read, Update, Delete) Data Form Entry Support</span></span>
====================
<span data-ttu-id="9894d-104">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="9894d-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="9894d-105">PDF をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="9894d-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="9894d-106">これは、無料の手順 5 ["NerdDinner"アプリケーションのチュートリアル](introducing-the-nerddinner-tutorial.md)をウォーク-にする方法を小規模の構築が完了すると、ASP.NET MVC 1 を使用して web アプリケーションです。</span><span class="sxs-lookup"><span data-stu-id="9894d-106">This is step 5 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="9894d-107">手順 5 では、編集、作成、および関連付けディナーを削除すると、同様のサポートを有効にし、さらに、DinnersController クラスを得る方法を示します。</span><span class="sxs-lookup"><span data-stu-id="9894d-107">Step 5 shows how to take our DinnersController class further by enable support for editing, creating and deleting Dinners with it as well.</span></span>
> 
> <span data-ttu-id="9894d-108">ASP.NET MVC 3 を使用している場合ことをお勧めする、 [MVC 3 の開始と取得](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)または[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)チュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="9894d-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-5-create-update-delete-form-scenarios"></a><span data-ttu-id="9894d-109">NerdDinner 手順 5: 作成、更新、フォームのシナリオの削除</span><span class="sxs-lookup"><span data-stu-id="9894d-109">NerdDinner Step 5: Create, Update, Delete Form Scenarios</span></span>

<span data-ttu-id="9894d-110">導入されたコント ローラーとビュー、およびそれらを使用してサイトにディナーのカタログ/登録エクスペリエンスを実装する方法を説明しました。</span><span class="sxs-lookup"><span data-stu-id="9894d-110">We've introduced controllers and views, and covered how to use them to implement a listing/details experience for Dinners on site.</span></span> <span data-ttu-id="9894d-111">次の手順は、DinnersController クラスはさらに、編集、作成、および関連付けディナーを削除すると、同様のサポートを有効になります。</span><span class="sxs-lookup"><span data-stu-id="9894d-111">Our next step will be to take our DinnersController class further and enable support for editing, creating and deleting Dinners with it as well.</span></span>

### <a name="urls-handled-by-dinnerscontroller"></a><span data-ttu-id="9894d-112">DinnersController によって処理される Url</span><span class="sxs-lookup"><span data-stu-id="9894d-112">URLs handled by DinnersController</span></span>

<span data-ttu-id="9894d-113">2 つの Url のサポートを実装する DinnersController を以前にアクション メソッドを追加しました: */Dinners*と*/Dinners 詳細/[id]*です。</span><span class="sxs-lookup"><span data-stu-id="9894d-113">We previously added action methods to DinnersController that implemented support for two URLs: */Dinners* and */Dinners/Details/[id]*.</span></span>

| <span data-ttu-id="9894d-114">**URL**</span><span class="sxs-lookup"><span data-stu-id="9894d-114">**URL**</span></span> | <span data-ttu-id="9894d-115">**VERB**</span><span class="sxs-lookup"><span data-stu-id="9894d-115">**VERB**</span></span> | <span data-ttu-id="9894d-116">**目的**</span><span class="sxs-lookup"><span data-stu-id="9894d-116">**Purpose**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="9894d-117">*/Dinners/*</span><span class="sxs-lookup"><span data-stu-id="9894d-117">*/Dinners/*</span></span> | <span data-ttu-id="9894d-118">GET</span><span class="sxs-lookup"><span data-stu-id="9894d-118">GET</span></span> | <span data-ttu-id="9894d-119">今後ディナーの HTML の一覧を表示します。</span><span class="sxs-lookup"><span data-stu-id="9894d-119">Display an HTML list of upcoming dinners.</span></span> |
| <span data-ttu-id="9894d-120">*/Dinners/Details/[id]*</span><span class="sxs-lookup"><span data-stu-id="9894d-120">*/Dinners/Details/[id]*</span></span> | <span data-ttu-id="9894d-121">GET</span><span class="sxs-lookup"><span data-stu-id="9894d-121">GET</span></span> | <span data-ttu-id="9894d-122">特定の dinner に関する詳細を表示します。</span><span class="sxs-lookup"><span data-stu-id="9894d-122">Display details about a specific dinner.</span></span> |

<span data-ttu-id="9894d-123">今すぐに次の 3 つの追加の Url を実装するアクション メソッドを追加します: <em>/Dinners/編集/[id]、ディナー/作成、</em>と<em>/Dinners/削除/[id]</em>です。</span><span class="sxs-lookup"><span data-stu-id="9894d-123">We will now add action methods to implement three additional URLs: <em>/Dinners/Edit/[id], /Dinners/Create,</em>and<em>/Dinners/Delete/[id]</em>.</span></span> <span data-ttu-id="9894d-124">これらの Url は、新しいディナーを作成およびディナーを削除する編集の既存ディナーのサポートを有効になります。</span><span class="sxs-lookup"><span data-stu-id="9894d-124">These URLs will enable support for editing existing Dinners, creating new Dinners, and deleting Dinners.</span></span>

<span data-ttu-id="9894d-125">これらの新しい Url に HTTP GET および HTTP POST 動詞相互作用がサポートされます。</span><span class="sxs-lookup"><span data-stu-id="9894d-125">We will support both HTTP GET and HTTP POST verb interactions with these new URLs.</span></span> <span data-ttu-id="9894d-126">これらの Url に HTTP GET 要求は、(フォームの場合は"edit"Dinner データが設定される、「作成」の場合は空白のフォームおよび「削除」の場合、削除の確認画面) のデータの初期の HTML ビューに表示されます。</span><span class="sxs-lookup"><span data-stu-id="9894d-126">HTTP GET requests to these URLs will display the initial HTML view of the data (a form populated with the Dinner data in the case of "edit", a blank form in the case of "create", and a delete confirmation screen in the case of "delete").</span></span> <span data-ttu-id="9894d-127">これらの Url に HTTP POST 要求は、Dinner データ、DinnerRepository (および、データベースにそこから) を保存/更新/削除になります。</span><span class="sxs-lookup"><span data-stu-id="9894d-127">HTTP POST requests to these URLs will save/update/delete the Dinner data in our DinnerRepository (and from there to the database).</span></span>

| <span data-ttu-id="9894d-128">**URL**</span><span class="sxs-lookup"><span data-stu-id="9894d-128">**URL**</span></span> | <span data-ttu-id="9894d-129">**VERB**</span><span class="sxs-lookup"><span data-stu-id="9894d-129">**VERB**</span></span> | <span data-ttu-id="9894d-130">**目的**</span><span class="sxs-lookup"><span data-stu-id="9894d-130">**Purpose**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="9894d-131">*/Dinners/Edit/[id]*</span><span class="sxs-lookup"><span data-stu-id="9894d-131">*/Dinners/Edit/[id]*</span></span> | <span data-ttu-id="9894d-132">GET</span><span class="sxs-lookup"><span data-stu-id="9894d-132">GET</span></span> | <span data-ttu-id="9894d-133">Dinner データが設定される編集可能な HTML フォームを表示します。</span><span class="sxs-lookup"><span data-stu-id="9894d-133">Display an editable HTML form populated with Dinner data.</span></span> |
| <span data-ttu-id="9894d-134">POST</span><span class="sxs-lookup"><span data-stu-id="9894d-134">POST</span></span> | <span data-ttu-id="9894d-135">データベースに特定 Dinner のフォームの変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="9894d-135">Save the form changes for a particular Dinner to the database.</span></span> |
| <span data-ttu-id="9894d-136">*/Dinners/Create*</span><span class="sxs-lookup"><span data-stu-id="9894d-136">*/Dinners/Create*</span></span> | <span data-ttu-id="9894d-137">GET</span><span class="sxs-lookup"><span data-stu-id="9894d-137">GET</span></span> | <span data-ttu-id="9894d-138">により、ユーザーが新しいディナーを定義する空の HTML フォームを表示します。</span><span class="sxs-lookup"><span data-stu-id="9894d-138">Display an empty HTML form that allows users to define new Dinners.</span></span> |
| <span data-ttu-id="9894d-139">POST</span><span class="sxs-lookup"><span data-stu-id="9894d-139">POST</span></span> | <span data-ttu-id="9894d-140">新しい Dinner を作成し、データベースに保存します。</span><span class="sxs-lookup"><span data-stu-id="9894d-140">Create a new Dinner and save it in the database.</span></span> |
| <span data-ttu-id="9894d-141">*/Dinners/Delete/[id]*</span><span class="sxs-lookup"><span data-stu-id="9894d-141">*/Dinners/Delete/[id]*</span></span> | <span data-ttu-id="9894d-142">GET</span><span class="sxs-lookup"><span data-stu-id="9894d-142">GET</span></span> | <span data-ttu-id="9894d-143">削除の確認画面を表示します。</span><span class="sxs-lookup"><span data-stu-id="9894d-143">Display delete confirmation screen.</span></span> |
| <span data-ttu-id="9894d-144">POST</span><span class="sxs-lookup"><span data-stu-id="9894d-144">POST</span></span> | <span data-ttu-id="9894d-145">指定された dinner データベースから削除します。</span><span class="sxs-lookup"><span data-stu-id="9894d-145">Deletes the specified dinner from the database.</span></span> |

### <a name="edit-support"></a><span data-ttu-id="9894d-146">サポートを編集します。</span><span class="sxs-lookup"><span data-stu-id="9894d-146">Edit Support</span></span>

<span data-ttu-id="9894d-147">"Edit"シナリオを実装することで見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="9894d-147">Let's begin by implementing the "edit" scenario.</span></span>

#### <a name="the-http-get-edit-action-method"></a><span data-ttu-id="9894d-148">HTTP GET 編集アクション メソッド</span><span class="sxs-lookup"><span data-stu-id="9894d-148">The HTTP-GET Edit Action Method</span></span>

<span data-ttu-id="9894d-149">まず、HTTP"GET"メソッドの動作の編集操作を実装します。</span><span class="sxs-lookup"><span data-stu-id="9894d-149">We'll start by implementing the HTTP "GET" behavior of our edit action method.</span></span> <span data-ttu-id="9894d-150">このメソッドがあるときに呼び出さ、 */Dinners/編集/[id]* URL を要求します。</span><span class="sxs-lookup"><span data-stu-id="9894d-150">This method will be invoked when the */Dinners/Edit/[id]* URL is requested.</span></span> <span data-ttu-id="9894d-151">この実装は、ようになります。</span><span class="sxs-lookup"><span data-stu-id="9894d-151">Our implementation will look like:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample1.cs)]

<span data-ttu-id="9894d-152">上記のコードは、Dinner オブジェクトを取得するのに、DinnerRepository を使用します。</span><span class="sxs-lookup"><span data-stu-id="9894d-152">The code above uses the DinnerRepository to retrieve a Dinner object.</span></span> <span data-ttu-id="9894d-153">ビューを Dinner オブジェクトを使用してテンプレートをレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="9894d-153">It then renders a View template using the Dinner object.</span></span> <span data-ttu-id="9894d-154">テンプレート名を明示的に渡されたしていないため、 *View()*ヘルパー メソッドに使用する規約に基づく既定のパス テンプレートの表示を解決するには:/Views/Dinners/Edit.aspx です。</span><span class="sxs-lookup"><span data-stu-id="9894d-154">Because we haven't explicitly passed a template name to the *View()* helper method, it will use the convention based default path to resolve the view template: /Views/Dinners/Edit.aspx.</span></span>

<span data-ttu-id="9894d-155">このテンプレートの表示を今すぐ作成してみましょう。</span><span class="sxs-lookup"><span data-stu-id="9894d-155">Let's now create this view template.</span></span> <span data-ttu-id="9894d-156">このが作業を行う編集メソッド内で右クリックし、「ビューの追加」のコンテキスト メニュー コマンドを選択します。</span><span class="sxs-lookup"><span data-stu-id="9894d-156">We will do this by right-clicking within the Edit method and selecting the "Add View" context menu command:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image1.png)

<span data-ttu-id="9894d-157">「ビューの追加」ダイアログ ボックスでおすることを示すためお Dinner オブジェクトとしてのモデル、ビュー テンプレートに渡す自動 scaffold を「編集」テンプレートを選択します。</span><span class="sxs-lookup"><span data-stu-id="9894d-157">Within the "Add View" dialog we'll indicate that we are passing a Dinner object to our view template as its model, and choose to auto-scaffold an "Edit" template:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image2.png)

<span data-ttu-id="9894d-158">[追加] ボタンをクリックするおと Visual Studio は新しい"Edit.aspx"ビュー テンプレート ファイルを"\Views\Dinners"ディレクトリ内でご利用の米国追加します。</span><span class="sxs-lookup"><span data-stu-id="9894d-158">When we click the "Add" button, Visual Studio will add a new "Edit.aspx" view template file for us within the "\Views\Dinners" directory.</span></span> <span data-ttu-id="9894d-159">開くまで、新しい"Edit.aspx"ビュー内のテンプレート、コード エディター: 初期"Edit"scaffold 実装すると、以下に設定されます。</span><span class="sxs-lookup"><span data-stu-id="9894d-159">It will also open up the new "Edit.aspx" view template within the code-editor – populated with an initial "Edit" scaffold implementation like below:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image3.png)

<span data-ttu-id="9894d-160">生成されると、既定値"edit"scaffold にいくつかの変更を加えるし、(を公開したくありませんプロパティの一部を削除) コンテンツが編集ビュー テンプレートを更新してみましょう。</span><span class="sxs-lookup"><span data-stu-id="9894d-160">Let's make a few changes to the default "edit" scaffold generated, and update the edit view template to have the content below (which removes a few of the properties we don't want to expose):</span></span>

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample2.aspx)]

<span data-ttu-id="9894d-161">アプリケーションと要求を実行するとき、 *「/ディナー/編集/1」* URL は、次のページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="9894d-161">When we run the application and request the *"/Dinners/Edit/1"* URL we will see the following page:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image4.png)

<span data-ttu-id="9894d-162">このビューによって生成される HTML マークアップは、以下のようになります。</span><span class="sxs-lookup"><span data-stu-id="9894d-162">The HTML markup generated by our view looks like below.</span></span> <span data-ttu-id="9894d-163">です。 標準 HTML –、&lt;フォーム&gt;要素への HTTP POST を実行する、 */Dinners/Edit/1* URL と [保存]&lt;入力の種類 ="送信"/&gt;ボタンが押されました。</span><span class="sxs-lookup"><span data-stu-id="9894d-163">It is standard HTML – with a &lt;form&gt; element that performs an HTTP POST to the */Dinners/Edit/1* URL when the "Save" &lt;input type="submit"/&gt; button is pushed.</span></span> <span data-ttu-id="9894d-164">Html ドキュメント&lt;入力の種類 ="text"/&gt;要素が編集可能なプロパティごとに出力されました。</span><span class="sxs-lookup"><span data-stu-id="9894d-164">A HTML &lt;input type="text"/&gt; element has been output for each editable property:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image5.png)

#### <a name="htmlbeginform-and-htmltextbox-html-helper-methods"></a><span data-ttu-id="9894d-165">Html.BeginForm() と Html.TextBox() Html ヘルパー メソッド</span><span class="sxs-lookup"><span data-stu-id="9894d-165">Html.BeginForm() and Html.TextBox() Html Helper Methods</span></span>

<span data-ttu-id="9894d-166">この"Edit.aspx"ビューのテンプレートがいくつかの方法「Html ヘルパー」: Html.ValidationSummary()、Html.BeginForm()、Html.TextBox()、および Html.ValidationMessage() です。</span><span class="sxs-lookup"><span data-stu-id="9894d-166">Our "Edit.aspx" view template is using several "Html Helper" methods: Html.ValidationSummary(), Html.BeginForm(), Html.TextBox(), and Html.ValidationMessage().</span></span> <span data-ttu-id="9894d-167">組み込みのエラー処理と検証の HTML マークアップを生成する、に加えてこれらのヘルパー メソッドを提供をサポートします。</span><span class="sxs-lookup"><span data-stu-id="9894d-167">In addition to generating HTML markup for us, these helper methods provide built-in error handling and validation support.</span></span>

##### <a name="htmlbeginform-helper-method"></a><span data-ttu-id="9894d-168">Html.BeginForm() ヘルパー メソッド</span><span class="sxs-lookup"><span data-stu-id="9894d-168">Html.BeginForm() helper method</span></span>

<span data-ttu-id="9894d-169">Html.BeginForm() ヘルパー メソッドは HTML 出力&lt;フォーム&gt;マークアップ内の要素。</span><span class="sxs-lookup"><span data-stu-id="9894d-169">The Html.BeginForm() helper method is what output the HTML &lt;form&gt; element in our markup.</span></span> <span data-ttu-id="9894d-170">当社 Edit.aspx ビュー テンプレートに適用する c# の"using"ステートメントこのメソッドを使用する場合は明らかです。</span><span class="sxs-lookup"><span data-stu-id="9894d-170">In our Edit.aspx view template you'll notice that we are applying a C# "using" statement when using this method.</span></span> <span data-ttu-id="9894d-171">中かっこの先頭を示す、&lt;フォーム&gt;コンテンツ、および終了の中かっこがの末尾を示す内容、 &lt;/form&gt;要素。</span><span class="sxs-lookup"><span data-stu-id="9894d-171">The open curly brace indicates the beginning of the &lt;form&gt; content, and the closing curly brace is what indicates the end of the &lt;/form&gt; element:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample3.cs)]

<span data-ttu-id="9894d-172">また、"using"ステートメントを検索する場合は、次のようなシナリオの不自然なアプローチ、Html.BeginForm() と Html.EndForm() の組み合わせが同じもの) を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="9894d-172">Alternatively, if you find the "using" statement approach unnatural for a scenario like this, you can use a Html.BeginForm() and Html.EndForm() combination (which does the same thing):</span></span>

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample4.aspx)]

<span data-ttu-id="9894d-173">パラメーターを使用せず Html.BeginForm() を呼び出すことを現在の要求の URL に HTTP POST を行うフォーム要素を出力することになります。</span><span class="sxs-lookup"><span data-stu-id="9894d-173">Calling Html.BeginForm() without any parameters will cause it to output a form element that does an HTTP-POST to the current request's URL.</span></span> <span data-ttu-id="9894d-174">つまり、編集ビューを生成理由、 *&lt;フォーム アクション =「/ディナー/編集/1」のメソッド"post"=&gt;* 要素。</span><span class="sxs-lookup"><span data-stu-id="9894d-174">That is why our Edit view generates a *&lt;form action="/Dinners/Edit/1" method="post"&gt;* element.</span></span> <span data-ttu-id="9894d-175">私たちがまたはに渡すことが明示的なパラメーター Html.BeginForm() 別の URL に投稿する場合。</span><span class="sxs-lookup"><span data-stu-id="9894d-175">We could have alternatively passed explicit parameters to Html.BeginForm() if we wanted to post to a different URL.</span></span>

##### <a name="htmltextbox-helper-method"></a><span data-ttu-id="9894d-176">Html.TextBox() ヘルパー メソッド</span><span class="sxs-lookup"><span data-stu-id="9894d-176">Html.TextBox() helper method</span></span>

<span data-ttu-id="9894d-177">当社 Edit.aspx ビュー Html.TextBox() ヘルパー メソッドを使用して出力する&lt;入力の種類 ="text"/&gt;要素。</span><span class="sxs-lookup"><span data-stu-id="9894d-177">Our Edit.aspx view uses the Html.TextBox() helper method to output &lt;input type="text"/&gt; elements:</span></span>

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample5.aspx)]

<span data-ttu-id="9894d-178">上記の Html.TextBox() メソッドには 1 つのパラメーター – の id または名前属性を指定するために使用される、&lt;入力の種類 ="text"/&gt;要素からテキスト ボックス値の設定に、モデル プロパティと同様に、出力をします。</span><span class="sxs-lookup"><span data-stu-id="9894d-178">The Html.TextBox() method above takes a single parameter – which is being used to specify both the id/name attributes of the &lt;input type="text"/&gt; element to output, as well as the model property to populate the textbox value from.</span></span> <span data-ttu-id="9894d-179">たとえば、編集ビューに渡されたお Dinner オブジェクト「.NET フューチャ」の"Title"プロパティの値があり、Html.TextBox("Title") メソッドが出力を呼び出すため: *&lt;入力 id ="Title"名前「タイトル」の種類 ="text"の値を = =「.NET フューチャ」/&gt;*.</span><span class="sxs-lookup"><span data-stu-id="9894d-179">For example, the Dinner object we passed to the Edit view had a "Title" property value of ".NET Futures", and so our Html.TextBox("Title") method call output: *&lt;input id="Title" name="Title" type="text" value=".NET Futures" /&gt;*.</span></span>

<span data-ttu-id="9894d-180">または、Html.TextBox() の最初のパラメーターを使用して、要素の id または名前を指定し、2 番目のパラメーターとして使用する値で明示的に渡すことができますお。</span><span class="sxs-lookup"><span data-stu-id="9894d-180">Alternatively, we can use the first Html.TextBox() parameter to specify the id/name of the element, and then explicitly pass in the value to use as a second parameter:</span></span>

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample6.aspx)]

<span data-ttu-id="9894d-181">多くの場合、出力される値のカスタム書式設定を実行することをおします。</span><span class="sxs-lookup"><span data-stu-id="9894d-181">Often we'll want to perform custom formatting on the value that is output.</span></span> <span data-ttu-id="9894d-182">指定する静的メソッドに組み込ま .NET は、これらのシナリオに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="9894d-182">The String.Format() static method built-into .NET is useful for these scenarios.</span></span> <span data-ttu-id="9894d-183">当社 Edit.aspx ビュー テンプレートを使用してこの、EventDate 値 (DateTime 型の) を書式設定、時間の秒数を表示しないように。</span><span class="sxs-lookup"><span data-stu-id="9894d-183">Our Edit.aspx view template is using this to format the EventDate value (which is of type DateTime) so that it doesn't show seconds for the time:</span></span>

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample7.aspx)]

<span data-ttu-id="9894d-184">Html.TextBox() を 3 番目のパラメーターは、追加の HTML 属性を出力するオプションで使用できます。</span><span class="sxs-lookup"><span data-stu-id="9894d-184">A third parameter to Html.TextBox() can optionally be used to output additional HTML attributes.</span></span> <span data-ttu-id="9894d-185">次のコードがその他のサイズを表示する方法を示します =「30」属性とクラス"mycssclass"属性を = on、&lt;入力の種類 ="text"/&gt;要素。</span><span class="sxs-lookup"><span data-stu-id="9894d-185">The code-snippet below demonstrates how to render an additional size="30" attribute and a class="mycssclass" attribute on the &lt;input type="text"/&gt; element.</span></span> <span data-ttu-id="9894d-186">クラス属性を使用して、名前をエスケープお方法に注意してください、"@" character because "クラス"を予約済みキーワード (C#)。</span><span class="sxs-lookup"><span data-stu-id="9894d-186">Note how we are escaping the name of the class attribute using a "@" character because "class" is a reserved keyword in C#:</span></span>

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample8.aspx)]

#### <a name="implementing-the-http-post-edit-action-method"></a><span data-ttu-id="9894d-187">HTTP POST 編集アクション メソッドの実装</span><span class="sxs-lookup"><span data-stu-id="9894d-187">Implementing the HTTP-POST Edit Action Method</span></span>

<span data-ttu-id="9894d-188">おに、編集アクション メソッドが実装されて、HTTP GET バージョンがインストールされているようになりました。</span><span class="sxs-lookup"><span data-stu-id="9894d-188">We now have the HTTP-GET version of our Edit action method implemented.</span></span> <span data-ttu-id="9894d-189">ユーザーが要求したとき、 */Dinners/Edit/1*次のような HTML ページが表示されるこれらの URL:</span><span class="sxs-lookup"><span data-stu-id="9894d-189">When a user requests the */Dinners/Edit/1* URL they receive an HTML page like the following:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image6.png)

<span data-ttu-id="9894d-190">フォームの post を上書き保存 ボタンを押すと、 */Dinners/Edit/1* URL、HTML を送信して&lt;入力&gt;HTTP POST 動詞を使用して値を形成します。</span><span class="sxs-lookup"><span data-stu-id="9894d-190">Pressing the "Save" button causes a form post to the */Dinners/Edit/1* URL, and submits the HTML &lt;input&gt; form values using the HTTP POST verb.</span></span> <span data-ttu-id="9894d-191">– Dinner の保存を処理する、編集のアクション メソッドの HTTP POST の動作を実装してみましょう。</span><span class="sxs-lookup"><span data-stu-id="9894d-191">Let's now implement the HTTP POST behavior of our edit action method – which will handle saving the Dinner.</span></span>

<span data-ttu-id="9894d-192">まず、DinnersController 上に"AcceptVerbs"属性があることを示す HTTP POST のシナリオを処理するオーバー ロードされた"Edit"アクション メソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="9894d-192">We'll begin by adding an overloaded "Edit" action method to our DinnersController that has an "AcceptVerbs" attribute on it that indicates it handles HTTP POST scenarios:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample9.cs)]

<span data-ttu-id="9894d-193">[AcceptVerbs] 属性を適用するオーバー ロードされたアクション メソッドに ASP.NET MVC は自動的に入力方向の HTTP 動詞に応じて適切なアクション メソッドにディスパッチ要求を処理します。</span><span class="sxs-lookup"><span data-stu-id="9894d-193">When the [AcceptVerbs] attribute is applied to overloaded action methods, ASP.NET MVC automatically handles dispatching requests to the appropriate action method depending on the incoming HTTP verb.</span></span> <span data-ttu-id="9894d-194">HTTP POST 要求を<em>/Dinners/編集/[id]</em>を他のすべての HTTP 動詞の要求中に、上記の編集方法に移動する Url <em>/Dinners/編集/[id]</em>Url に移動する (これが最初の編集メソッドを実装しましたいません [AcceptVerbs] 属性を持つ)。</span><span class="sxs-lookup"><span data-stu-id="9894d-194">HTTP POST requests to <em>/Dinners/Edit/[id]</em> URLs will go to the above Edit method, while all other HTTP verb requests to <em>/Dinners/Edit/[id]</em>URLs will go to the first Edit method we implemented (which did not have an [AcceptVerbs] attribute).</span></span>

| <span data-ttu-id="9894d-195">**側トピックの内容は、理由、HTTP 動詞を使用して区別しますか。**</span><span class="sxs-lookup"><span data-stu-id="9894d-195">**Side Topic: Why differentiate via HTTP verbs?**</span></span> |
| --- |
| <span data-ttu-id="9894d-196">思うかもしれません – はなぜ 1 つの URL を使用して、HTTP 動詞を使用してその動作を区別しますか。</span><span class="sxs-lookup"><span data-stu-id="9894d-196">You might ask – why are we using a single URL and differentiating its behavior via the HTTP verb?</span></span> <span data-ttu-id="9894d-197">読み込みと編集の変更の保存を処理する 2 つの独立した Url があるだけしないのはなぜですか。</span><span class="sxs-lookup"><span data-stu-id="9894d-197">Why not just have two separate URLs to handle loading and saving edit changes?</span></span> <span data-ttu-id="9894d-198">例:/Dinners/編集/[id] と表示するの初期フォーム/Dinners 保存/[id] を保存するためのフォーム post を処理しますか?</span><span class="sxs-lookup"><span data-stu-id="9894d-198">For example: /Dinners/Edit/[id] to display the initial form and /Dinners/Save/[id] to handle the form post to save it?</span></span> <span data-ttu-id="9894d-199">発行の 2 つの独立した Url での短所は、/Dinners/Save/2 を投稿し、入力エラーのため、HTML フォームを再表示する必要がありますがの場合、エンドユーザーが終了することを (でしたので、ブラウザーのアドレス バーにディナー/保存/2 の URL を持つURL にポストされたフォーム) です。</span><span class="sxs-lookup"><span data-stu-id="9894d-199">The downside with publishing two separate URLs is that in cases where we post to /Dinners/Save/2, and then need to redisplay the HTML form because of an input error, the end-user will end up having the /Dinners/Save/2 URL in their browser's address bar (since that was the URL the form posted to).</span></span> <span data-ttu-id="9894d-200">場合、エンドユーザーのブラウザーのお気に入りリストに redisplayed このページをブックマークまたは URL のコピー/貼り付けます。 電子メールを友人に、これらは (その URL は、post 値によって異なります) してから後で動作しない URL の保存を終了します。</span><span class="sxs-lookup"><span data-stu-id="9894d-200">If the end-user bookmarks this redisplayed page to their browser favorites list, or copy/pastes the URL and emails it to a friend, they will end up saving a URL that won't work in the future (since that URL depends on post values).</span></span> <span data-ttu-id="9894d-201">1 つの URL を公開することで (のような:/Dinners/Edit/[id]) および HTTP 動詞でその処理を区別するの編集ページをブックマークや他のユーザーに、URL を送信するエンドユーザーのも安全です。</span><span class="sxs-lookup"><span data-stu-id="9894d-201">By exposing a single URL (like: /Dinners/Edit/[id]) and differentiating the processing of it by HTTP verb, it is safe for end-users to bookmark the edit page and/or send the URL to others.</span></span> |

#### <a name="retrieving-form-post-values"></a><span data-ttu-id="9894d-202">フォーム ポスト値を取得します。</span><span class="sxs-lookup"><span data-stu-id="9894d-202">Retrieving Form Post Values</span></span>

<span data-ttu-id="9894d-203">これは、さまざまな HTTP POST、"Edit"メソッド内でのフォーム パラメーターを投稿にアクセスする方法です。</span><span class="sxs-lookup"><span data-stu-id="9894d-203">There are a variety of ways we can access posted form parameters within our HTTP POST "Edit" method.</span></span> <span data-ttu-id="9894d-204">簡単な方法の 1 つは、だけフォーム コレクションへのアクセスおよび、ポストされた値を直接取得するコント ローラーの基本クラスの要求プロパティを使用します。</span><span class="sxs-lookup"><span data-stu-id="9894d-204">One simple approach is to just use the Request property on the Controller base class to access the form collection and retrieve the posted values directly:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample10.cs)]

<span data-ttu-id="9894d-205">上記の方法が少し verbose、ただし、特にエラー処理ロジックを追加します。</span><span class="sxs-lookup"><span data-stu-id="9894d-205">The above approach is a little verbose, though, especially once we add error handling logic.</span></span>

<span data-ttu-id="9894d-206">このシナリオでは、組み込みの利用のより優れた方法*UpdateModel()*コント ローラーの基本クラスのヘルパー メソッドです。</span><span class="sxs-lookup"><span data-stu-id="9894d-206">A better approach for this scenario is to leverage the built-in *UpdateModel()* helper method on the Controller base class.</span></span> <span data-ttu-id="9894d-207">受信フォーム パラメーターを使用して渡すオブジェクトのプロパティの更新をサポートします。</span><span class="sxs-lookup"><span data-stu-id="9894d-207">It supports updating the properties of an object we pass it using the incoming form parameters.</span></span> <span data-ttu-id="9894d-208">リフレクションを使用して、オブジェクトのプロパティの名前を決定し自動的に変換し、クライアントから送信された入力値に基づいて値を割り当てます。</span><span class="sxs-lookup"><span data-stu-id="9894d-208">It uses reflection to determine the property names on the object, and then automatically converts and assigns values to them based on the input values submitted by the client.</span></span>

<span data-ttu-id="9894d-209">UpdateModel() メソッドを使用して、HTTP POST の編集を使用してアクションこのコードを簡略化する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="9894d-209">We could use the UpdateModel() method to simplify our HTTP-POST Edit Action using this code:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample11.cs)]

<span data-ttu-id="9894d-210">私たちがアクセスできるようになりました、 */Dinners/Edit/1* URL、および、夕食のタイトルを変更します。</span><span class="sxs-lookup"><span data-stu-id="9894d-210">We can now visit the */Dinners/Edit/1* URL, and change the title of our Dinner:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image7.png)

<span data-ttu-id="9894d-211">[保存] ボタンをクリックしてと、編集の操作にポストされたフォームを実行、更新された値は、データベースに保存されます。</span><span class="sxs-lookup"><span data-stu-id="9894d-211">When we click the "Save" button, we'll perform a form post to our Edit action, and the updated values will be persisted in the database.</span></span> <span data-ttu-id="9894d-212">おは、Dinner (を新たに保存した値が表示されます) の詳細情報の URL にリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="9894d-212">We will then be redirected to the Details URL for the Dinner (which will display the newly saved values):</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image8.png)

#### <a name="handling-edit-errors"></a><span data-ttu-id="9894d-213">編集エラーの処理</span><span class="sxs-lookup"><span data-stu-id="9894d-213">Handling Edit Errors</span></span>

<span data-ttu-id="9894d-214">場合を除き、現在の HTTP POST 実装の動作微調整 – エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="9894d-214">Our current HTTP-POST implementation works fine – except when there are errors.</span></span>

<span data-ttu-id="9894d-215">ユーザーがフォームを編集誤りと、その修正方法が説明するエラーに関する情報メッセージを使用、フォームが再表示するかどうかを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9894d-215">When a user makes a mistake editing a form, we need to make sure that the form is redisplayed with an informative error message that guides them to fix it.</span></span> <span data-ttu-id="9894d-216">場合には、エンドユーザーが正しくない入力を投稿が含まれます (例: 形式が正しくない日付文字列) 入力形式が無効であるケースもが、ビジネス ルールの違反があるように、します。</span><span class="sxs-lookup"><span data-stu-id="9894d-216">This includes cases where an end-user posts incorrect input (for example: a malformed date string), as well as cases where the input format is valid but there is a business rule violation.</span></span> <span data-ttu-id="9894d-217">エラーが発生するフォームが最初にその変更を手動で再読み込みする必要があるない、ようにに入力したユーザーの入力データを保持する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9894d-217">When errors occur the form should preserve the input data the user originally entered so that they don't have to refill their changes manually.</span></span> <span data-ttu-id="9894d-218">フォームが正常に完了するまで、このプロセスが必要な回数だけ繰り返されます。</span><span class="sxs-lookup"><span data-stu-id="9894d-218">This process should repeat as many times as necessary until the form successfully completes.</span></span>

<span data-ttu-id="9894d-219">ASP.NET MVC には、エラー処理とフォームの再表示を簡単にできるようにする便利な組み込みの機能が含まれています。</span><span class="sxs-lookup"><span data-stu-id="9894d-219">ASP.NET MVC includes some nice built-in features that make error handling and form redisplay easy.</span></span> <span data-ttu-id="9894d-220">みましょうアクションでこれらの機能を表示する次のコードで、編集のアクション メソッドを更新します。</span><span class="sxs-lookup"><span data-stu-id="9894d-220">To see these features in action let's update our Edit action method with the following code:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample12.cs)]

<span data-ttu-id="9894d-221">作業の周囲 try/catch エラー処理のブロックをラップするようになりましたおする点を除いて、上記のコードは前の実装では – に似ています。</span><span class="sxs-lookup"><span data-stu-id="9894d-221">The above code is similar to our previous implementation – except that we are now wrapping a try/catch error handling block around our work.</span></span> <span data-ttu-id="9894d-222">UpdateModel() を呼び出すとき、またはしようとして (これは、例外が、モデル内のルール違反のため、Dinner オブジェクトを保存しようとしましたが無効な場合) DinnerRepository、保存したときに、catch のエラー処理のブロックが、例外が発生した場合実行します。</span><span class="sxs-lookup"><span data-stu-id="9894d-222">If an exception occurs either when calling UpdateModel(), or when we try and save the DinnerRepository (which will raise an exception if the Dinner object we are trying to save is invalid because of a rule violation within our model), our catch error handling block will execute.</span></span> <span data-ttu-id="9894d-223">内にはお Dinner オブジェクト内に存在する任意の規則違反をループし、ModelState オブジェクト (を間もなくについて説明します) に追加します。</span><span class="sxs-lookup"><span data-stu-id="9894d-223">Within it we loop over any rule violations that exist in the Dinner object and add them to a ModelState object (which we'll discuss shortly).</span></span> <span data-ttu-id="9894d-224">ここには、ビューからもう一度です。</span><span class="sxs-lookup"><span data-stu-id="9894d-224">We then redisplay the view.</span></span>

<span data-ttu-id="9894d-225">操作してみましょうアプリケーションを再実行このを表示するには、夕食を編集し、EventDate"BOGUS"の空のタイトルを持ち、米国の国の値でイギリスの電話番号を使用するように変更します。</span><span class="sxs-lookup"><span data-stu-id="9894d-225">To see this working let's re-run the application, edit a Dinner, and change it to have an empty Title, an EventDate of "BOGUS", and use a UK phone number with a country value of USA.</span></span> <span data-ttu-id="9894d-226">[保存] ボタン キーを押すと、HTTP POST の編集メソッドことはできません (エラーがある) ため、Dinner を保存して、フォームを再表示されます。</span><span class="sxs-lookup"><span data-stu-id="9894d-226">When we press the "Save" button our HTTP POST Edit method will not be able to save the Dinner (because there are errors) and will redisplay the form:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image9.png)

<span data-ttu-id="9894d-227">アプリケーションには、適切なエラー経験があります。</span><span class="sxs-lookup"><span data-stu-id="9894d-227">Our application has a decent error experience.</span></span> <span data-ttu-id="9894d-228">テキスト要素を無効な入力が赤で強調表示し、それらについてエンドユーザーに検証エラー メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="9894d-228">The text elements with the invalid input are highlighted in red, and validation error messages are displayed to the end user about them.</span></span> <span data-ttu-id="9894d-229">フォームも保持している最初のユーザーに入力した – 入力データを何もを補充するがあるないようにします。</span><span class="sxs-lookup"><span data-stu-id="9894d-229">The form is also preserving the input data the user originally entered – so that they don't have to refill anything.</span></span>

<span data-ttu-id="9894d-230">方法についてと思うかもしれませんでしたこれが発生しますか。</span><span class="sxs-lookup"><span data-stu-id="9894d-230">How, you might ask, did this occur?</span></span> <span data-ttu-id="9894d-231">でしたタイトル、EventDate、および ContactPhone テキスト ボックス自体を赤で強調表示してに最初に入力したユーザーの値を出力する方法</span><span class="sxs-lookup"><span data-stu-id="9894d-231">How did the Title, EventDate, and ContactPhone textboxes highlight themselves in red and know to output the originally entered user values?</span></span> <span data-ttu-id="9894d-232">でしたエラー メッセージの取得の表示方法、上部の一覧にしますか。</span><span class="sxs-lookup"><span data-stu-id="9894d-232">And how did error messages get displayed in the list at the top?</span></span> <span data-ttu-id="9894d-233">良いニュースは、ことマジックによるこれが発生しなかったがいくつかの組み込みの ASP.NET MVC 機能を使用する入力の検証とエラー処理シナリオを使用しているではなくです。</span><span class="sxs-lookup"><span data-stu-id="9894d-233">The good news is that this didn't occur by magic - rather it was because we used some of the built-in ASP.NET MVC features that make input validation and error handling scenarios easy.</span></span>

#### <a name="understanding-modelstate-and-the-validation-html-helper-methods"></a><span data-ttu-id="9894d-234">Understanding ModelState と検証の HTML ヘルパー メソッド</span><span class="sxs-lookup"><span data-stu-id="9894d-234">Understanding ModelState and the Validation HTML Helper Methods</span></span>

<span data-ttu-id="9894d-235">コント ローラー クラスには、エラーがビューに渡されるモデル オブジェクトに存在することを指定する手段を提供する"ModelState"プロパティ コレクションがあります。</span><span class="sxs-lookup"><span data-stu-id="9894d-235">Controller classes have a "ModelState" property collection which provides a way to indicate that errors exist with a model object being passed to a View.</span></span> <span data-ttu-id="9894d-236">ModelState コレクション内のエラーのエントリでは、モデル プロパティの名前を特定の問題に (例:「タイトル」、"EventDate"または"ContactPhone")、わかりやすいエラー メッセージを指定することができ (例:「タイトルが必要」) です。</span><span class="sxs-lookup"><span data-stu-id="9894d-236">Error entries within the ModelState collection identify the name of the model property with the issue (for example: "Title", "EventDate", or "ContactPhone"), and allow a human-friendly error message to be specified (for example: "Title is required").</span></span>

<span data-ttu-id="9894d-237">*UpdateModel()*ヘルパー メソッドは、モデル オブジェクトのプロパティをフォームの値を割り当てようとするとエラーが発生したときに自動的に ModelState コレクションを設定します。</span><span class="sxs-lookup"><span data-stu-id="9894d-237">The *UpdateModel()* helper method automatically populates the ModelState collection when it encounters errors while trying to assign form values to properties on the model object.</span></span> <span data-ttu-id="9894d-238">たとえば、Dinner オブジェクトの EventDate プロパティは DateTime 型です。</span><span class="sxs-lookup"><span data-stu-id="9894d-238">For example, our Dinner object's EventDate property is of type DateTime.</span></span> <span data-ttu-id="9894d-239">UpdateModel() メソッドは、上記のシナリオで"BOGUS"の文字列値を割り当てることができませんが、そのプロパティを使用して割り当てエラーを示す ModelState コレクションにエントリが発生しました、UpdateModel() メソッドが追加されます。</span><span class="sxs-lookup"><span data-stu-id="9894d-239">When the UpdateModel() method was unable to assign the string value "BOGUS" to it in the scenario above, the UpdateModel() method added an entry to the ModelState collection indicating an assignment error had occurred with that property.</span></span>

<span data-ttu-id="9894d-240">開発者が行っている下ブロック内で、"catch"エラー処理のアクティブな規則違反に基づいてエントリ ModelState コレクションの読み込みはこれと同じように明示的に ModelState コレクション内のエラー エントリを追加するコードを記述できますも、Dinner オブジェクト:</span><span class="sxs-lookup"><span data-stu-id="9894d-240">Developers can also write code to explicitly add error entries into the ModelState collection like we are doing below within our "catch" error handling block, which is populating the ModelState collection with entries based on the active Rule Violations in the Dinner object:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample13.cs)]

#### <a name="html-helper-integration-with-modelstate"></a><span data-ttu-id="9894d-241">ModelState と html ヘルパーの統合</span><span class="sxs-lookup"><span data-stu-id="9894d-241">Html Helper Integration with ModelState</span></span>

<span data-ttu-id="9894d-242">Html.TextBox() - などの HTML ヘルパー メソッドは、出力を表示するときに、ModelState コレクションを確認します。</span><span class="sxs-lookup"><span data-stu-id="9894d-242">HTML helper methods - like Html.TextBox() - check the ModelState collection when rendering output.</span></span> <span data-ttu-id="9894d-243">項目のエラーが存在する場合、ユーザーが入力した値と、CSS エラー クラス レンダリングします。</span><span class="sxs-lookup"><span data-stu-id="9894d-243">If an error for the item exists, they render the user-entered value and a CSS error class.</span></span>

<span data-ttu-id="9894d-244">たとえばで、[編集] ビューは使用 Html.TextBox() ヘルパー メソッド Dinner オブジェクトの EventDate を表示するために。</span><span class="sxs-lookup"><span data-stu-id="9894d-244">For example, in our "Edit" view we are using the Html.TextBox() helper method to render the EventDate of our Dinner object:</span></span>

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample14.aspx)]

<span data-ttu-id="9894d-245">エラーのシナリオでは、ビュー、表示されて、Html.TextBox() メソッド ModelState コレクションを参照し、夕食オブジェクトの"EventDate"プロパティに関連付けられているすべてのエラーが発生したがチェックされます。</span><span class="sxs-lookup"><span data-stu-id="9894d-245">When the view was rendered in the error scenario, the Html.TextBox() method checked the ModelState collection to see if there were any errors associated with the "EventDate" property of our Dinner object.</span></span> <span data-ttu-id="9894d-246">値として送信されたユーザー入力 ("BOGUS") を表示し、css エラー クラスを追加エラーがあったことが確認された場合、&lt;入力の種類 ="textbox"/&gt;マークアップを生成します。</span><span class="sxs-lookup"><span data-stu-id="9894d-246">When it determined that there was an error it rendered the submitted user input ("BOGUS") as the value, and added a css error class to the &lt;input type="textbox"/&gt; markup it generated:</span></span>

[!code-html[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample15.html)]

<span data-ttu-id="9894d-247">ただし場合を検索する css エラー クラスの外観をカスタマイズできます。</span><span class="sxs-lookup"><span data-stu-id="9894d-247">You can customize the appearance of the css error class to look however you want.</span></span> <span data-ttu-id="9894d-248">既定の CSS エラー クラス –「の入力の検証エラー」– がで定義されている、 *\content\site.css*下などのスタイル シートとは。</span><span class="sxs-lookup"><span data-stu-id="9894d-248">The default CSS error class – "input-validation-error" – is defined in the *\content\site.css* stylesheet and looks like below:</span></span>

[!code-css[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample16.css)]

<span data-ttu-id="9894d-249">このような CSS 規則は、無効な入力要素を次のように強調表示の原因を示します。</span><span class="sxs-lookup"><span data-stu-id="9894d-249">This CSS rule is what caused our invalid input elements to be highlighted like below:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image10.png)

##### <a name="htmlvalidationmessage-helper-method"></a><span data-ttu-id="9894d-250">Html.ValidationMessage() ヘルパー メソッド</span><span class="sxs-lookup"><span data-stu-id="9894d-250">Html.ValidationMessage() Helper Method</span></span>

<span data-ttu-id="9894d-251">特定のモデル プロパティに関連付けられた ModelState エラー メッセージを出力する Html.ValidationMessage() ヘルパー メソッドを使用できます。</span><span class="sxs-lookup"><span data-stu-id="9894d-251">The Html.ValidationMessage() helper method can be used to output the ModelState error message associated with a particular model property:</span></span>

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample17.aspx)]

<span data-ttu-id="9894d-252">上記のコードを出力: *&lt;クラスにまたがる =「フィールドの検証エラー」&gt; 'BOGUS' の値が無効&lt;/span&gt;*</span><span class="sxs-lookup"><span data-stu-id="9894d-252">The above code outputs: *&lt;span class="field-validation-error"&gt; The value ‘BOGUS' is invalid&lt;/span&gt;*</span></span>

<span data-ttu-id="9894d-253">Html.ValidationMessage() ヘルパー メソッドには、表示されるエラー テキスト メッセージを上書きする開発者は、2 番目のパラメーターもサポートしています。</span><span class="sxs-lookup"><span data-stu-id="9894d-253">The Html.ValidationMessage() helper method also supports a second parameter that allows developers to override the error text message that is displayed:</span></span>

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample18.aspx)]

<span data-ttu-id="9894d-254">上記のコードを出力: <em>&lt;クラスにまたがる =「フィールドの検証エラー」&gt;\*&lt;/span&gt;</em>エラーの場合、既定のエラー テキストではなく、EventDate プロパティです。</span><span class="sxs-lookup"><span data-stu-id="9894d-254">The above code outputs: <em>&lt;span class="field-validation-error"&gt;\*&lt;/span&gt;</em>instead of the default error text when an error is present for the EventDate property.</span></span>

##### <a name="htmlvalidationsummary-helper-method"></a><span data-ttu-id="9894d-255">Html.ValidationSummary() ヘルパー メソッド</span><span class="sxs-lookup"><span data-stu-id="9894d-255">Html.ValidationSummary() Helper Method</span></span>

<span data-ttu-id="9894d-256">Html.ValidationSummary() ヘルパー メソッドは、簡単なエラー メッセージを伴うを表示するために使用できる、 &lt;ul&gt;&lt;li/&gt;&lt;/ul&gt;メッセージはすべて詳細なエラーの一覧で、ModelState コレクション:</span><span class="sxs-lookup"><span data-stu-id="9894d-256">The Html.ValidationSummary() helper method can be used to render a summary error message, accompanied by a &lt;ul&gt;&lt;li/&gt;&lt;/ul&gt; list of all detailed error messages in the ModelState collection:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image11.png)

<span data-ttu-id="9894d-257">Html.ValidationSummary() のヘルパー メソッドは、詳細なエラーの一覧の上に表示する概要エラー メッセージを定義する、省略可能な文字列パラメーターを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="9894d-257">The Html.ValidationSummary() helper method takes an optional string parameter – which defines a summary error message to display above the list of detailed errors:</span></span>

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample19.aspx)]

<span data-ttu-id="9894d-258">エラー一覧の外観をオーバーライドするのに CSS を使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="9894d-258">You can optionally use CSS to override what the error list looks like.</span></span>

#### <a name="using-a-addruleviolations-helper-method"></a><span data-ttu-id="9894d-259">AddRuleViolations ヘルパー メソッドを使用してください。</span><span class="sxs-lookup"><span data-stu-id="9894d-259">Using a AddRuleViolations Helper Method</span></span>

<span data-ttu-id="9894d-260">初期の HTTP POST を編集実装では、Dinner オブジェクトの規則の違反をループし、コント ローラーの ModelState コレクションに追加の catch ブロック内で、foreach ステートメントを使用しました。</span><span class="sxs-lookup"><span data-stu-id="9894d-260">Our initial HTTP-POST Edit implementation used a foreach statement within its catch block to loop over the Dinner object's Rule Violations and add them to the controller's ModelState collection:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample20.cs)]

<span data-ttu-id="9894d-261">このコード"ControllerHelpers"を追加することによってほとんどクリーナー NerdDinner プロジェクトにクラスし、メソッドを実装して、"AddRuleViolations"拡張機能内に、ASP.NET MVC ModelStateDictionary クラスにヘルパー メソッドを追加するようにできます。</span><span class="sxs-lookup"><span data-stu-id="9894d-261">We can make this code a little cleaner by adding a "ControllerHelpers" class to the NerdDinner project, and implement an "AddRuleViolations" extension method within it that adds a helper method to the ASP.NET MVC ModelStateDictionary class.</span></span> <span data-ttu-id="9894d-262">この拡張メソッドは、ModelStateDictionary RuleViolation エラーの一覧の設定に必要なロジックをカプセル化できます。</span><span class="sxs-lookup"><span data-stu-id="9894d-262">This extension method can encapsulate the logic necessary to populate the ModelStateDictionary with a list of RuleViolation errors:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample21.cs)]

<span data-ttu-id="9894d-263">HTTP POST 操作メソッドの編集メソッドを使用してこの拡張機能、夕食規則違反を ModelState コレクションを設定し、更新することができます。</span><span class="sxs-lookup"><span data-stu-id="9894d-263">We can then update our HTTP-POST Edit action method to use this extension method to populate the ModelState collection with our Dinner Rule Violations.</span></span>

#### <a name="complete-edit-action-method-implementations"></a><span data-ttu-id="9894d-264">アクション メソッドの実装は編集を完了します。</span><span class="sxs-lookup"><span data-stu-id="9894d-264">Complete Edit Action Method Implementations</span></span>

<span data-ttu-id="9894d-265">次のコードでは、すべての編集に紹介したシナリオに必要なコント ローラー ロジックを実装します。</span><span class="sxs-lookup"><span data-stu-id="9894d-265">The code below implements all of the controller logic necessary for our Edit scenario:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample22.cs)]

<span data-ttu-id="9894d-266">編集実装優れている点は、コント ローラー クラスでも、ビュー テンプレート固有の検証や、夕食モデルに設定されているビジネス ルールについて知っています。</span><span class="sxs-lookup"><span data-stu-id="9894d-266">The nice thing about our Edit implementation is that neither our Controller class nor our View template has to know anything about the specific validation or business rules being enforced by our Dinner model.</span></span> <span data-ttu-id="9894d-267">おと追加できます追加の規則に、モデル、将来、コント ローラーまたはビューでサポートされるためにそれらの順番にコード変更を加える必要はありません。</span><span class="sxs-lookup"><span data-stu-id="9894d-267">We can add additional rules to our model in the future and do not have to make any code changes to our controller or view in order for them to be supported.</span></span> <span data-ttu-id="9894d-268">これにより、アプリケーション要件は、後で、最小限に抑えてコードの変更内容を簡単に進化する柔軟性が付加されます。</span><span class="sxs-lookup"><span data-stu-id="9894d-268">This provides us with the flexibility to easily evolve our application requirements in the future with a minimum of code changes.</span></span>

### <a name="create-support"></a><span data-ttu-id="9894d-269">サポートを作成します。</span><span class="sxs-lookup"><span data-stu-id="9894d-269">Create Support</span></span>

<span data-ttu-id="9894d-270">DinnersController クラスの"Edit"動作を実装することが完了しました。</span><span class="sxs-lookup"><span data-stu-id="9894d-270">We've finished implementing the "Edit" behavior of our DinnersController class.</span></span> <span data-ttu-id="9894d-271">今すぐに進みましょう、– 新しいディナーを追加するユーザーが、これを「作成」のサポートを実装します。</span><span class="sxs-lookup"><span data-stu-id="9894d-271">Let's now move on to implement the "Create" support on it – which will enable users to add new Dinners.</span></span>

#### <a name="the-http-get-create-action-method"></a><span data-ttu-id="9894d-272">HTTP GET がアクション メソッドを作成します。</span><span class="sxs-lookup"><span data-stu-id="9894d-272">The HTTP-GET Create Action Method</span></span>

<span data-ttu-id="9894d-273">最初の HTTP"GET"動作を実装することによって、アクション メソッドを作成します。</span><span class="sxs-lookup"><span data-stu-id="9894d-273">We'll begin by implementing the HTTP "GET" behavior of our create action method.</span></span> <span data-ttu-id="9894d-274">アクセスと、このメソッドが呼び出されます、*ディナー/作成*URL。</span><span class="sxs-lookup"><span data-stu-id="9894d-274">This method will be called when someone visits the */Dinners/Create* URL.</span></span> <span data-ttu-id="9894d-275">この実装は、ようになります。</span><span class="sxs-lookup"><span data-stu-id="9894d-275">Our implementation looks like:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample23.cs)]

<span data-ttu-id="9894d-276">上記のコードは、新しい Dinner オブジェクトを作成し、今後 1 週間である場合は、その EventDate プロパティを割り当てます。</span><span class="sxs-lookup"><span data-stu-id="9894d-276">The code above creates a new Dinner object, and assigns its EventDate property to be one week in the future.</span></span> <span data-ttu-id="9894d-277">新しい Dinner オブジェクトに基づいているビューをレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="9894d-277">It then renders a View that is based on the new Dinner object.</span></span> <span data-ttu-id="9894d-278">名前を明示的に渡されたしていないため、 *View()*ヘルパー メソッドに使用する規約に基づく既定のパス テンプレートの表示を解決するには:/Views/Dinners/Create.aspx です。</span><span class="sxs-lookup"><span data-stu-id="9894d-278">Because we haven't explicitly passed a name to the *View()* helper method, it will use the convention based default path to resolve the view template: /Views/Dinners/Create.aspx.</span></span>

<span data-ttu-id="9894d-279">このテンプレートの表示を今すぐ作成してみましょう。</span><span class="sxs-lookup"><span data-stu-id="9894d-279">Let's now create this view template.</span></span> <span data-ttu-id="9894d-280">作成アクション メソッド内で右クリックし、「ビューの追加」のコンテキスト メニュー コマンドを選択してこの作業を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="9894d-280">We can do this by right-clicking within the Create action method and selecting the "Add View" context menu command.</span></span> <span data-ttu-id="9894d-281">「ビューの追加」ダイアログ ボックスでをビュー テンプレートに Dinner オブジェクトを渡すことはおこと、および自動 scaffold の「作成」テンプレートを選択を示します。</span><span class="sxs-lookup"><span data-stu-id="9894d-281">Within the "Add View" dialog we'll indicate that we are passing a Dinner object to the view template, and choose to auto-scaffold a "Create" template:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image12.png)

<span data-ttu-id="9894d-282">「追加」ボタンをクリックして、Visual Studio は新しい scaffold ベース"Create.aspx"ビュー"\Views\Dinners"ディレクトリに保存し、ide 開きます。</span><span class="sxs-lookup"><span data-stu-id="9894d-282">When we click the "Add" button, Visual Studio will save a new scaffold-based "Create.aspx" view to the "\Views\Dinners" directory, and open it up within the IDE:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image13.png)

<span data-ttu-id="9894d-283">みましょういくつかの変更を「作成」scaffold される既定のファイルが生成された、行い、以下のようにアップ変更します。</span><span class="sxs-lookup"><span data-stu-id="9894d-283">Let's make a few changes to the default "create" scaffold file that was generated for us, and modify it up to look like below:</span></span>

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample24.aspx)]

<span data-ttu-id="9894d-284">今すぐ実行するとき、マイクロソフトのアプリケーションとアクセスと、 *「ディナー/作成」*今回作成アクションの実装から次のような UI を表示する、ブラウザー内の URL:</span><span class="sxs-lookup"><span data-stu-id="9894d-284">And now when we run our application and access the *"/Dinners/Create"* URL within the browser it will render UI like below from our Create action implementation:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image14.png)

#### <a name="implementing-the-http-post-create-action-method"></a><span data-ttu-id="9894d-285">アクション メソッドを作成する HTTP POST を実装します。</span><span class="sxs-lookup"><span data-stu-id="9894d-285">Implementing the HTTP-POST Create Action Method</span></span>

<span data-ttu-id="9894d-286">おには、実装、作成するアクション メソッドの HTTP GET バージョンがインストールされています。</span><span class="sxs-lookup"><span data-stu-id="9894d-286">We have the HTTP-GET version of our Create action method implemented.</span></span> <span data-ttu-id="9894d-287">フォーム ポスト実行ユーザーが [保存] ボタンをクリックしたときに、*ディナー/作成*URL、HTML を送信して&lt;入力&gt;HTTP POST 動詞を使用して値を形成します。</span><span class="sxs-lookup"><span data-stu-id="9894d-287">When a user clicks the "Save" button it performs a form post to the */Dinners/Create* URL, and submits the HTML &lt;input&gt; form values using the HTTP POST verb.</span></span>

<span data-ttu-id="9894d-288">実装してみましょうの HTTP POST の動作、アクション メソッドを作成します。</span><span class="sxs-lookup"><span data-stu-id="9894d-288">Let's now implement the HTTP POST behavior of our create action method.</span></span> <span data-ttu-id="9894d-289">まず、DinnersController 上に"AcceptVerbs"属性があることを示す HTTP POST のシナリオを処理するオーバー ロードされた「作成」アクション メソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="9894d-289">We'll begin by adding an overloaded "Create" action method to our DinnersController that has an "AcceptVerbs" attribute on it that indicates it handles HTTP POST scenarios:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample25.cs)]

<span data-ttu-id="9894d-290">これは、さまざまな方法は、HTTP POST が有効になっている「作成」のメソッド内で、ポストされたフォーム パラメーターにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="9894d-290">There are a variety of ways we can access the posted form parameters within our HTTP-POST enabled "Create" method.</span></span>

<span data-ttu-id="9894d-291">新しい Dinner オブジェクトを作成し、使用する方法が、 *UpdateModel()*ヘルパー メソッド (と編集の操作)、ポストされたフォーム値を設定します。</span><span class="sxs-lookup"><span data-stu-id="9894d-291">One approach is to create a new Dinner object and then use the *UpdateModel()* helper method (like we did with the Edit action) to populate it with the posted form values.</span></span> <span data-ttu-id="9894d-292">お、DinnerRepository に追加する、データベースに保持し、次のコードを使用して新しく作成された Dinner を表示するには、この詳細アクションにユーザーをリダイレクトします。、</span><span class="sxs-lookup"><span data-stu-id="9894d-292">We can then add it to our DinnerRepository, persist it to the database, and redirect the user to our Details action to show the newly created Dinner using the code below:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample26.cs)]

<span data-ttu-id="9894d-293">または、使用できますアプローチ Dinner オブジェクトのメソッド パラメーターとして取得、Create() アクション メソッドがあります。</span><span class="sxs-lookup"><span data-stu-id="9894d-293">Alternatively, we can use an approach where we have our Create() action method take a Dinner object as a method parameter.</span></span> <span data-ttu-id="9894d-294">ASP.NET MVC は自動的にうえで、新しい Dinner オブジェクトをインスタンス化、フォームの入力を使用してそのプロパティを設定し、アクション メソッドに渡すこと。</span><span class="sxs-lookup"><span data-stu-id="9894d-294">ASP.NET MVC will then automatically instantiate a new Dinner object for us, populate its properties using the form inputs, and pass it to our action method:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample27.cs)]

<span data-ttu-id="9894d-295">上記のアクション メソッドは、こと Dinner オブジェクトが正常に作成されたフォーム ポスト値を持つ ModelState.IsValid プロパティをチェックして、確認します。</span><span class="sxs-lookup"><span data-stu-id="9894d-295">Our action method above verifies that the Dinner object has been successfully populated with the form post values by checking the ModelState.IsValid property.</span></span> <span data-ttu-id="9894d-296">変換の問題の入力がある場合は false が返されます (例: EventDate プロパティの"BOGUS"の文字列)、および、アクション メソッドが、フォームを再問題がある場合。</span><span class="sxs-lookup"><span data-stu-id="9894d-296">This will return false if there are input conversion issues (for example: a string of "BOGUS" for the EventDate property), and if there are any issues our action method redisplays the form.</span></span>

<span data-ttu-id="9894d-297">入力値が有効な場合、アクション メソッドは、追加し、DinnerRepository に新しい Dinner を保存を試みます。</span><span class="sxs-lookup"><span data-stu-id="9894d-297">If the input values are valid, then the action method attempts to add and save the new Dinner to the DinnerRepository.</span></span> <span data-ttu-id="9894d-298">Try ブロックと catch ブロック内でこの作業をラップし、(その結果、dinnerRepository.Save() メソッドで例外が発生する)、ビジネス ルール違反がある場合は、フォームを再表示します。</span><span class="sxs-lookup"><span data-stu-id="9894d-298">It wraps this work within a try/catch block and redisplays the form if there are any business rule violations (which would cause the dinnerRepository.Save() method to raise an exception).</span></span>

<span data-ttu-id="9894d-299">このエラーの処理の動作の動作を表示するを要求できる、*ディナー/作成*URL と新しい Dinner に関する詳細を記入します。</span><span class="sxs-lookup"><span data-stu-id="9894d-299">To see this error handling behavior in action, we can request the */Dinners/Create* URL and fill out details about a new Dinner.</span></span> <span data-ttu-id="9894d-300">不適切な入力または値を次のように強調表示されているエラーが再表示するフォームの作成になります。</span><span class="sxs-lookup"><span data-stu-id="9894d-300">Incorrect input or values will cause the create form to be redisplayed with the errors highlighted like below:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image15.png)

<span data-ttu-id="9894d-301">フォームを作成する方法は、編集フォームと正確な同じ検証とビジネス規則を記念するために注意してください。</span><span class="sxs-lookup"><span data-stu-id="9894d-301">Notice how our Create form is honoring the exact same validation and business rules as our Edit form.</span></span> <span data-ttu-id="9894d-302">これは、この検証規則とビジネス規則が、モデルで定義されたが、UI またはアプリケーションのコント ローラーには埋め込まれていないためです。</span><span class="sxs-lookup"><span data-stu-id="9894d-302">This is because our validation and business rules were defined in the model, and were not embedded within the UI or controller of the application.</span></span> <span data-ttu-id="9894d-303">これは後で変更/に応じて変化できる、検証、または 1 つのビジネス ルールを配置して、アプリケーション全体に適用することを意味します。</span><span class="sxs-lookup"><span data-stu-id="9894d-303">This means we can later change/evolve our validation or business rules in a single place and have them apply throughout our application.</span></span> <span data-ttu-id="9894d-304">内で任意のコードを変更するには、編集、または、新しいルールまたは既存のテーブルへの変更が自動的に付与するアクション メソッドを作成することはありません。</span><span class="sxs-lookup"><span data-stu-id="9894d-304">We will not have to change any code within either our Edit or Create action methods to automatically honor any new rules or modifications to existing ones.</span></span>

<span data-ttu-id="9894d-305">入力値を修正し、[保存] ボタンをクリックして、もう一度、DinnerRepository への追記を弊社は成功しますと新しい Dinner をデータベースに追加されます。</span><span class="sxs-lookup"><span data-stu-id="9894d-305">When we fix the input values and click the "Save" button again, our addition to the DinnerRepository will succeed, and a new Dinner will be added to the database.</span></span> <span data-ttu-id="9894d-306">ここにリダイレクトされます、 */Dinners 詳細/[id]* URL: 新しく作成された Dinner に関する詳細情報をその場所お表示されます。</span><span class="sxs-lookup"><span data-stu-id="9894d-306">We will then be redirected to the */Dinners/Details/[id]* URL – where we will be presented with details about the newly created Dinner:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image16.png)

### <a name="delete-support"></a><span data-ttu-id="9894d-307">サポートを削除します。</span><span class="sxs-lookup"><span data-stu-id="9894d-307">Delete Support</span></span>

<span data-ttu-id="9894d-308">当社 DinnersController に"Delete"サポートを今すぐ追加してみましょう。</span><span class="sxs-lookup"><span data-stu-id="9894d-308">Let's now add "Delete" support to our DinnersController.</span></span>

#### <a name="the-http-get-delete-action-method"></a><span data-ttu-id="9894d-309">Delete アクション メソッドを HTTP GET</span><span class="sxs-lookup"><span data-stu-id="9894d-309">The HTTP-GET Delete Action Method</span></span>

<span data-ttu-id="9894d-310">まず、HTTP GET、delete アクション メソッドの動作を実装します。</span><span class="sxs-lookup"><span data-stu-id="9894d-310">We'll begin by implementing the HTTP GET behavior of our delete action method.</span></span> <span data-ttu-id="9894d-311">このメソッドへのアクセス時に呼び出される、 */Dinners/削除/[id]* URL。</span><span class="sxs-lookup"><span data-stu-id="9894d-311">This method will get called when someone visits the */Dinners/Delete/[id]* URL.</span></span> <span data-ttu-id="9894d-312">実装では、次に示します。</span><span class="sxs-lookup"><span data-stu-id="9894d-312">Below is the implementation:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample28.cs)]

<span data-ttu-id="9894d-313">アクション メソッドは、削除する Dinner の取得を試みます。</span><span class="sxs-lookup"><span data-stu-id="9894d-313">The action method attempts to retrieve the Dinner to be deleted.</span></span> <span data-ttu-id="9894d-314">Dinner が存在する場合、レンダリング、表示は、Dinner オブジェクトに基づいています。</span><span class="sxs-lookup"><span data-stu-id="9894d-314">If the Dinner exists it renders a View based on the Dinner object.</span></span> <span data-ttu-id="9894d-315">オブジェクトが存在しない (または既に削除されている) 場合、"NotFound"をレンダリングするビューを返しますが、"Details"アクション メソッドの前に作成したテンプレートを表示します。</span><span class="sxs-lookup"><span data-stu-id="9894d-315">If the object doesn't exist (or has already been deleted) it returns a View that renders the "NotFound" view template we created earlier for our "Details" action method.</span></span>

<span data-ttu-id="9894d-316">Delete アクション メソッド内で右クリックし、「ビューの追加」のコンテキスト メニュー コマンドを選択して、"Delete"ビューのテンプレートを作成できます。</span><span class="sxs-lookup"><span data-stu-id="9894d-316">We can create the "Delete" view template by right-clicking within the Delete action method and selecting the "Add View" context menu command.</span></span> <span data-ttu-id="9894d-317">「ビューの追加」ダイアログ ボックスでをそのモデルと、ビュー テンプレートに Dinner オブジェクトを渡すことはお、および空のテンプレートを作成することを示します。</span><span class="sxs-lookup"><span data-stu-id="9894d-317">Within the "Add View" dialog we'll indicate that we are passing a Dinner object to our view template as its model, and choose to create an empty template:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image17.png)

<span data-ttu-id="9894d-318">[追加] ボタンをクリックするおと Visual Studio は新しい"Delete.aspx"ビュー テンプレート ファイルを"\Views\Dinners"ディレクトリ内でご利用の米国追加します。</span><span class="sxs-lookup"><span data-stu-id="9894d-318">When we click the "Add" button, Visual Studio will add a new "Delete.aspx" view template file for us within our "\Views\Dinners" directory.</span></span> <span data-ttu-id="9894d-319">いくつかの HTML とコードを追加、削除の確認画面を実装するテンプレートを以下のような。</span><span class="sxs-lookup"><span data-stu-id="9894d-319">We'll add some HTML and code to the template to implement a delete confirmation screen like below:</span></span>

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample29.aspx)]

<span data-ttu-id="9894d-320">上記のコードは、削除する Dinner と出力のタイトルを表示、&lt;フォーム&gt;をエンドユーザーが内で、[削除] ボタンをクリックした場合、/Dinners/削除/[id] URL に POST を行う要素。</span><span class="sxs-lookup"><span data-stu-id="9894d-320">The code above displays the title of the Dinner to be deleted, and outputs a &lt;form&gt; element that does a POST to the /Dinners/Delete/[id] URL if the end-user clicks the "Delete" button within it.</span></span>

<span data-ttu-id="9894d-321">このアプリケーションとアクセスを実行するとき、 *"/ディナー/削除/[id]"*オブジェクトの有効な夕食の URL を以下のような UI が表示されます。</span><span class="sxs-lookup"><span data-stu-id="9894d-321">When we run our application and access the *"/Dinners/Delete/[id]"* URL for a valid Dinner object it renders UI like below:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image18.png)

| <span data-ttu-id="9894d-322">**側トピックの内容: 理由が行って投稿しますか。**</span><span class="sxs-lookup"><span data-stu-id="9894d-322">**Side Topic: Why are we doing a POST?**</span></span> |
| --- |
| <span data-ttu-id="9894d-323">思うかもしれません – 理由でしたを進めていくを作成する作業量、&lt;フォーム&gt;削除の確認画面内ですか?</span><span class="sxs-lookup"><span data-stu-id="9894d-323">You might ask – why did we go through the effort of creating a &lt;form&gt; within our Delete confirmation screen?</span></span> <span data-ttu-id="9894d-324">標準のハイパーリンクを使用だけが実際の削除操作を実行するアクション メソッドへのリンクしないのはなぜですか。</span><span class="sxs-lookup"><span data-stu-id="9894d-324">Why not just use a standard hyperlink to link to an action method that does the actual delete operation?</span></span> <span data-ttu-id="9894d-325">理由は、検索エンジン、Url を検出して、誤ってリンクに従っている場合に削除されるデータの原因と web クローラーから保護するように注意する必要があるためにです。</span><span class="sxs-lookup"><span data-stu-id="9894d-325">The reason is because we want to be careful to guard against web-crawlers and search engines discovering our URLs and inadvertently causing data to be deleted when they follow the links.</span></span> <span data-ttu-id="9894d-326">HTTP GET ベース Url はアクセス/クロール、それらの「安全な」と見なされ、想定どおりに HTTP POST ものに従っていません。</span><span class="sxs-lookup"><span data-stu-id="9894d-326">HTTP-GET based URLs are considered "safe" for them to access/crawl, and they are supposed to not follow HTTP-POST ones.</span></span> <span data-ttu-id="9894d-327">HTTP POST 要求の背後にあるデータの変更操作または破壊を常に配置することを確認することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="9894d-327">A good rule is to make sure you always put destructive or data modifying operations behind HTTP-POST requests.</span></span> |

#### <a name="implementing-the-http-post-delete-action-method"></a><span data-ttu-id="9894d-328">HTTP POST Delete アクション メソッドの実装</span><span class="sxs-lookup"><span data-stu-id="9894d-328">Implementing the HTTP-POST Delete Action Method</span></span>

<span data-ttu-id="9894d-329">お今すぐバージョンである、HTTP GET、Delete アクション メソッドを実装が削除の確認画面が表示されます。</span><span class="sxs-lookup"><span data-stu-id="9894d-329">We now have the HTTP-GET version of our Delete action method implemented which displays a delete confirmation screen.</span></span> <span data-ttu-id="9894d-330">エンドユーザーが [削除] ボタンをクリックしたときにポストされたフォームが実行されます、 */Dinners Dinner/[id]* URL。</span><span class="sxs-lookup"><span data-stu-id="9894d-330">When an end user clicks the "Delete" button it will perform a form post to the */Dinners/Dinner/[id]* URL.</span></span>

<span data-ttu-id="9894d-331">次のコードを使用して delete アクション メソッドの HTTP"POST"動作を実装してみましょう。</span><span class="sxs-lookup"><span data-stu-id="9894d-331">Let's now implement the HTTP "POST" behavior of the delete action method using the code below:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample30.cs)]

<span data-ttu-id="9894d-332">Delete アクション メソッドの HTTP POST バージョンは、削除する dinner オブジェクトの取得を試みます。</span><span class="sxs-lookup"><span data-stu-id="9894d-332">The HTTP-POST version of our Delete action method attempts to retrieve the dinner object to delete.</span></span> <span data-ttu-id="9894d-333">(これが既に削除されているため) ことを見つけられない場合、"NotFound"テンプレートをレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="9894d-333">If it can't find it (because it has already been deleted) it renders our "NotFound" template.</span></span> <span data-ttu-id="9894d-334">Dinner が見つかった場合、DinnerRepository から削除します。</span><span class="sxs-lookup"><span data-stu-id="9894d-334">If it finds the Dinner, it deletes it from the DinnerRepository.</span></span> <span data-ttu-id="9894d-335">「削除済み」テンプレートをレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="9894d-335">It then renders a "Deleted" template.</span></span>

<span data-ttu-id="9894d-336">「削除済み」テンプレートを実装するには、アクション メソッドを右クリックし、「ビューの追加」のコンテキスト メニューを選択しておします。</span><span class="sxs-lookup"><span data-stu-id="9894d-336">To implement the "Deleted" template we'll right-click in the action method and choose the "Add View" context menu.</span></span> <span data-ttu-id="9894d-337">おをビューの「削除済み」という名前をしてそれを空のテンプレート (と厳密に型指定されたモデル オブジェクトになりません)。</span><span class="sxs-lookup"><span data-stu-id="9894d-337">We'll name our view "Deleted" and have it be an empty template (and not take a strongly-typed model object).</span></span> <span data-ttu-id="9894d-338">いくつかの HTML コンテンツを追加。</span><span class="sxs-lookup"><span data-stu-id="9894d-338">We'll then add some HTML content to it:</span></span>

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample31.aspx)]

<span data-ttu-id="9894d-339">今すぐ実行するとき、マイクロソフトのアプリケーションとアクセスと、 *"/ディナー/削除/[id]"*夕食をレンダリングするオブジェクトの削除の確認の有効な夕食の URL が以下のような画面します。</span><span class="sxs-lookup"><span data-stu-id="9894d-339">And now when we run our application and access the *"/Dinners/Delete/[id]"* URL for a valid Dinner object it will render our Dinner delete confirmation screen like below:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image19.png)

<span data-ttu-id="9894d-340">[削除] ボタンをクリックしたときに HTTP POST が実行されます、 */Dinners/削除/[id]*の URL をデータベースから Dinner を削除し、「削除済み」ビューのテンプレートを表示します。</span><span class="sxs-lookup"><span data-stu-id="9894d-340">When we click the "Delete" button it will perform an HTTP-POST to the */Dinners/Delete/[id]* URL, which will delete the Dinner from our database, and display our "Deleted" view template:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image20.png)

### <a name="model-binding-security"></a><span data-ttu-id="9894d-341">モデル バインディングのセキュリティ</span><span class="sxs-lookup"><span data-stu-id="9894d-341">Model Binding Security</span></span>

<span data-ttu-id="9894d-342">ASP.NET MVC の組み込みモデル バインド機能を使用する 2 つの方法について説明しました。</span><span class="sxs-lookup"><span data-stu-id="9894d-342">We've discussed two different ways to use the built-in model-binding features of ASP.NET MVC.</span></span> <span data-ttu-id="9894d-343">最初 UpdateModel() メソッドを使用して、既存のモデル オブジェクトのプロパティを更新して、2 つ目を使用して、アクション メソッドのパラメーターとしてのモデル オブジェクトを渡すための ASP.NET MVC のサポート。</span><span class="sxs-lookup"><span data-stu-id="9894d-343">The first using the UpdateModel() method to update properties on an existing model object, and the second using ASP.NET MVC's support for passing model objects in as action method parameters.</span></span> <span data-ttu-id="9894d-344">これらのテクニックは、非常に強力な非常に便利です。</span><span class="sxs-lookup"><span data-stu-id="9894d-344">Both of these techniques are very powerful and extremely useful.</span></span>

<span data-ttu-id="9894d-345">この電源がそれに点も担当します。</span><span class="sxs-lookup"><span data-stu-id="9894d-345">This power also brings with it responsibility.</span></span> <span data-ttu-id="9894d-346">すべてのユーザー入力を受け入れる場合するセキュリティについてパラノイド常にすることが重要これとはも true フォーム入力にオブジェクトをバインドするとき。</span><span class="sxs-lookup"><span data-stu-id="9894d-346">It is important to always be paranoid about security when accepting any user input, and this is also true when binding objects to form input.</span></span> <span data-ttu-id="9894d-347">ように注意する必要があります HTML が常に、HTML および JavaScript インジェクション攻撃を回避し、SQL インジェクション攻撃の注意が必要、ユーザーが入力した値をエンコード (注: アプリケーションでは、これらを防ぐためにパラメーターを自動的にエンコードされるの LINQ to SQL を使用します。種類の攻撃)。</span><span class="sxs-lookup"><span data-stu-id="9894d-347">You should be careful to always HTML encode any user-entered values to avoid HTML and JavaScript injection attacks, and be careful of SQL injection attacks (note: we are using LINQ to SQL for our application, which automatically encodes parameters to prevent these types of attacks).</span></span> <span data-ttu-id="9894d-348">ありません、単独で、クライアント側の検証に依存して、常に間違った値を送信しようとしています。 ハッカーから保護するためにサーバー側の検証を使用してください。</span><span class="sxs-lookup"><span data-stu-id="9894d-348">You should never rely on client-side validation alone, and always employ server-side validation to guard against hackers attempting to send you bogus values.</span></span>

<span data-ttu-id="9894d-349">ASP.NET MVC のバインド機能を使用するときに考慮するかどうかを確認する追加のセキュリティ項目を 1 つは、バインドするオブジェクトのスコープです。</span><span class="sxs-lookup"><span data-stu-id="9894d-349">One additional security item to make sure you think about when using the binding features of ASP.NET MVC is the scope of the objects you are binding.</span></span> <span data-ttu-id="9894d-350">具体的には、バインドして、実際には更新するエンドユーザーが更新可能なこれらのプロパティのみを許可するかどうかを確認するを許可するプロパティのセキュリティへの影響を理解することを確認します。</span><span class="sxs-lookup"><span data-stu-id="9894d-350">Specifically, you want to make sure you understand the security implications of the properties you are allowing to be bound, and make sure you only allow those properties that really should be updatable by an end-user to be updated.</span></span>

<span data-ttu-id="9894d-351">既定では、UpdateModel() メソッドが受信フォーム パラメーターの値に一致するモデル オブジェクトのすべてのプロパティを更新しようとするされます。</span><span class="sxs-lookup"><span data-stu-id="9894d-351">By default, the UpdateModel() method will attempt to update all properties on the model object that match incoming form parameter values.</span></span> <span data-ttu-id="9894d-352">同様に、または既定では、アクション メソッドのパラメーターとして渡されたオブジェクトのすべてのプロパティ フォーム パラメーターを使用して設定のことができます。</span><span class="sxs-lookup"><span data-stu-id="9894d-352">Likewise, objects passed as action method parameters also by default can have all of their properties set via form parameters.</span></span>

#### <a name="locking-down-binding-on-a-per-usage-basis"></a><span data-ttu-id="9894d-353">ロックダウン状況ベースごとにバインドします。</span><span class="sxs-lookup"><span data-stu-id="9894d-353">Locking down binding on a per-usage basis</span></span>

<span data-ttu-id="9894d-354">あたり使用量ベースのバインディング ポリシーを提供して、明示的な"include リスト"更新可能なプロパティのしてロックすることができます。</span><span class="sxs-lookup"><span data-stu-id="9894d-354">You can lock down the binding policy on a per-usage basis by providing an explicit "include list" of properties that can be updated.</span></span> <span data-ttu-id="9894d-355">これは、次のような UpdateModel() メソッドに追加の文字列の配列パラメーターを渡すことによって実行できます。</span><span class="sxs-lookup"><span data-stu-id="9894d-355">This can be done by passing an extra string array parameter to the UpdateModel() method like below:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample32.cs)]

<span data-ttu-id="9894d-356">また、アクション メソッドのパラメーターとして渡されたオブジェクトは、「インクルード リスト」の下のように指定するプロパティを使用できるようにする [バインド] 属性をサポートします。</span><span class="sxs-lookup"><span data-stu-id="9894d-356">Objects passed as action method parameters also support a [Bind] attribute that enables an "include list" of allowed properties to be specified like below:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample33.cs)]

#### <a name="locking-down-binding-on-a-type-basis"></a><span data-ttu-id="9894d-357">ロックダウン種類ごとにバインドします。</span><span class="sxs-lookup"><span data-stu-id="9894d-357">Locking down binding on a type basis</span></span>

<span data-ttu-id="9894d-358">型ごとにバインディング規則をロックダウンすることもできます。</span><span class="sxs-lookup"><span data-stu-id="9894d-358">You can also lock down the binding rules on a per-type basis.</span></span> <span data-ttu-id="9894d-359">これにより、1 回、バインディング規則を指定し、それらすべてのコント ローラーとアクション メソッド間で (UpdateModel とアクションの両方のメソッド パラメーターのシナリオを含む) すべてのシナリオに適用することができます。</span><span class="sxs-lookup"><span data-stu-id="9894d-359">This allows you to specify the binding rules once, and then have them apply in all scenarios (including both UpdateModel and action method parameter scenarios) across all controllers and action methods.</span></span>

<span data-ttu-id="9894d-360">種類のバインディング規則は、型の場合に、[バインド] 属性を追加するか (の型を所有していないシナリオに便利です) のアプリケーションの Global.asax ファイル内で登録することによってカスタマイズできます。</span><span class="sxs-lookup"><span data-stu-id="9894d-360">You can customize the per-type binding rules by adding a [Bind] attribute onto a type, or by registering it within the Global.asax file of the application (useful for scenarios where you don't own the type).</span></span> <span data-ttu-id="9894d-361">バインド属性の挿入を行うこともできますし、および制御するプロパティを除外するプロパティは、特定のクラスまたはインターフェイスのバインド可能です。</span><span class="sxs-lookup"><span data-stu-id="9894d-361">You can then use the Bind attribute's Include and Exclude properties to control which properties are bindable for the particular class or interface.</span></span>

<span data-ttu-id="9894d-362">Dinner クラス NerdDinner アプリケーションでは、この手法を使用し、[バインド] 属性を追加するには、次のバインド可能なプロパティの一覧を制限することおします。</span><span class="sxs-lookup"><span data-stu-id="9894d-362">We'll use this technique for the Dinner class in our NerdDinner application, and add a [Bind] attribute to it that restricts the list of bindable properties to the following:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample34.cs)]

<span data-ttu-id="9894d-363">、バインディングによって操作される RSVPs コレクションが許可されていないことに注意してください。 また DinnerID または HostedBy プロパティのバインディングによって設定を許可することはできません。</span><span class="sxs-lookup"><span data-stu-id="9894d-363">Notice we are not allowing the RSVPs collection to be manipulated via binding, nor are we allowing the DinnerID or HostedBy properties to be set via binding.</span></span> <span data-ttu-id="9894d-364">セキュリティ上の理由からおを代わりにのみ操作、アクション メソッド内で明示的なコードを使用してこれらの特定のプロパティ。</span><span class="sxs-lookup"><span data-stu-id="9894d-364">For security reasons we'll instead only manipulate these particular properties using explicit code within our action methods.</span></span>

### <a name="crud-wrap-up"></a><span data-ttu-id="9894d-365">CRUD まとめ</span><span class="sxs-lookup"><span data-stu-id="9894d-365">CRUD Wrap-Up</span></span>

<span data-ttu-id="9894d-366">ASP.NET MVC には、さまざまなシナリオを投稿するフォームの実装に役立つ組み込みの機能が含まれています。</span><span class="sxs-lookup"><span data-stu-id="9894d-366">ASP.NET MVC includes a number of built-in features that help with implementing form posting scenarios.</span></span> <span data-ttu-id="9894d-367">これらの機能のさまざまなを使用して、DinnerRepository 上に CRUD UI サポートを提供しました。</span><span class="sxs-lookup"><span data-stu-id="9894d-367">We used a variety of these features to provide CRUD UI support on top of our DinnerRepository.</span></span>

<span data-ttu-id="9894d-368">このアプリケーションを実装するのにモデル中心のアプローチを使用します。</span><span class="sxs-lookup"><span data-stu-id="9894d-368">We are using a model-focused approach to implement our application.</span></span> <span data-ttu-id="9894d-369">つまり、すべての検証とビジネス ルール ロジック、および、コント ローラーまたはビュー内ではなく、モデル レイヤー内で定義されます。</span><span class="sxs-lookup"><span data-stu-id="9894d-369">This means that all our validation and business rule logic is defined within our model layer – and not within our controllers or views.</span></span> <span data-ttu-id="9894d-370">コント ローラー クラスでも、テンプレートの表示に関する知識がまったく Dinner モデル クラスに設定されている特定のビジネス ルール。</span><span class="sxs-lookup"><span data-stu-id="9894d-370">Neither our Controller class nor our View templates know anything about the specific business rules being enforced by our Dinner model class.</span></span>

<span data-ttu-id="9894d-371">これは、マイクロソフト アプリケーション アーキテクチャ クリーンし、テストを容易にできるようにします。</span><span class="sxs-lookup"><span data-stu-id="9894d-371">This will keep our application architecture clean and make it easier to test.</span></span> <span data-ttu-id="9894d-372">追加できるその他のビジネス ルール、モデル レイヤー今後と*を任意のコード変更を加える必要*をサポートするのには、コント ローラーまたはそれらの順序で表示します。</span><span class="sxs-lookup"><span data-stu-id="9894d-372">We can add additional business rules to our model layer in the future and *not have to make any code changes* to our Controller or View in order for them to be supported.</span></span> <span data-ttu-id="9894d-373">これは、進化し、アプリケーションでは、将来の変更に機敏性を大幅に向上にご提供する中です。</span><span class="sxs-lookup"><span data-stu-id="9894d-373">This is going to provide us with a great deal of agility to evolve and change our application in the future.</span></span>

<span data-ttu-id="9894d-374">当社 DinnersController ようになりましたにより、Dinner 番組表/詳細については、だけでなくを作成、編集および削除のサポート。</span><span class="sxs-lookup"><span data-stu-id="9894d-374">Our DinnersController now enables Dinner listings/details, as well as create, edit and delete support.</span></span> <span data-ttu-id="9894d-375">以下は、クラスの完全なコードを確認できます。</span><span class="sxs-lookup"><span data-stu-id="9894d-375">The complete code for the class can be found below:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample35.cs)]

### <a name="next-step"></a><span data-ttu-id="9894d-376">次の手順</span><span class="sxs-lookup"><span data-stu-id="9894d-376">Next Step</span></span>

<span data-ttu-id="9894d-377">DinnersController クラス内での実装の基本的な CRUD (作成、読み取り、更新、削除) サポートがあるようになりました。</span><span class="sxs-lookup"><span data-stu-id="9894d-377">We now have basic CRUD (Create, Read, Update and Delete) support implement within our DinnersController class.</span></span>

<span data-ttu-id="9894d-378">これで、フォームでより豊富な UI を有効にする ViewData および ViewModel クラスを使用できる方法を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="9894d-378">Let's now look at how we can use ViewData and ViewModel classes to enable even richer UI on our forms.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9894d-379">[前へ](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
> [次へ](use-viewdata-and-implement-viewmodel-classes.md)</span><span class="sxs-lookup"><span data-stu-id="9894d-379">[Previous](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
[Next](use-viewdata-and-implement-viewmodel-classes.md)</span></span>
