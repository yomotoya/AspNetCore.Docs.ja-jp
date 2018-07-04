---
uid: single-page-application/overview/templates/breezeangular-template
title: Breeze/angular テンプレート |Microsoft Docs
author: madskristensen
description: Breeze/angular シングル ページ アプリケーション テンプレート
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/08/2013
ms.topic: article
ms.assetid: db31e909-563a-4516-aadd-62aa210ac7e4
ms.technology: ''
msc.legacyurl: /single-page-application/overview/templates/breezeangular-template
msc.type: authoredcontent
ms.openlocfilehash: 541d1a71b58a0d55651d823dc7425b7d1ce73cdb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378061"
---
<a name="breezeangular-template"></a><span data-ttu-id="26b59-103">Breeze/angular テンプレート</span><span class="sxs-lookup"><span data-stu-id="26b59-103">Breeze/Angular template</span></span>
====================
<span data-ttu-id="26b59-104">によって[Mads Kristensen](https://github.com/madskristensen)</span><span class="sxs-lookup"><span data-stu-id="26b59-104">by [Mads Kristensen](https://github.com/madskristensen)</span></span>

> <span data-ttu-id="26b59-105">Ward Bell によって Breeze/angular の MVC テンプレートが作成されました</span><span class="sxs-lookup"><span data-stu-id="26b59-105">The Breeze/Angular MVC Template was written by Ward Bell</span></span>
> 
> [<span data-ttu-id="26b59-106">Breeze/Angular の MVC テンプレートをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="26b59-106">Download the Breeze/Angular MVC Template</span></span>](https://go.microsoft.com/fwlink/?LinkId=286437)


<span data-ttu-id="26b59-107">[AngularJS](http://angularjs.org) Google からオープン ソース ライブラリをシングル ページ アプリケーション (Spa) を構築するためです。</span><span class="sxs-lookup"><span data-stu-id="26b59-107">[AngularJS](http://angularjs.org) is an open source library from Google for building Single Page Applications (SPAs).</span></span> <span data-ttu-id="26b59-108">データ バインド、依存関係の挿入、および画面の管理を提供します。</span><span class="sxs-lookup"><span data-stu-id="26b59-108">It offers data binding, dependency injection, and screen management.</span></span> <span data-ttu-id="26b59-109">組み合わせる[BreezeJS](http://www.breezejs.com/?utm_source=ms-spa)、データ モデリングとデータ管理、他のオープン ソース ライブラリがある優れた Html/javascript クライアント アプリの基本的な構成要素。</span><span class="sxs-lookup"><span data-stu-id="26b59-109">Combine it with [BreezeJS](http://www.breezejs.com/?utm_source=ms-spa), another open source library for data modeling and data management, and you have the essential ingredients for a great HTML/JavaScript client app.</span></span>

<span data-ttu-id="26b59-110">Breeze/angular SPA テンプレートはバリエーションの 1 つ、 [KnockoutJS SPA テンプレート](../introduction/knockoutjs-template.md)ASP.NET および Web Tools 2012.2 Update に含まれています。</span><span class="sxs-lookup"><span data-stu-id="26b59-110">The Breeze/Angular SPA template is a variation on the [KnockoutJS SPA template](../introduction/knockoutjs-template.md) included in the ASP.NET and Web Tools 2012.2 Update.</span></span> <span data-ttu-id="26b59-111">ある場合は Visual Studio が得られたら、SPA の例を稼働して 60 秒以内にします。</span><span class="sxs-lookup"><span data-stu-id="26b59-111">If you've got Visual Studio, you'll have an example SPA up and running in less than 60 seconds.</span></span>

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningTodoPage.png)

<span data-ttu-id="26b59-112">表面上、アプリケーションは、非常によう KnockoutJS SPA テンプレートになります。</span><span class="sxs-lookup"><span data-stu-id="26b59-112">Outwardly, the application looks the very similar to the KnockoutJS SPA template.</span></span> <span data-ttu-id="26b59-113">内部的にはまったく異なります。</span><span class="sxs-lookup"><span data-stu-id="26b59-113">But it's quite different under the hood.</span></span> <span data-ttu-id="26b59-114">KnockoutJS テンプレートは、データ バインディングとデータ アクセス用の生の AJAX Knockout を使用します。</span><span class="sxs-lookup"><span data-stu-id="26b59-114">The KnockoutJS template uses Knockout for data binding and raw AJAX for data access.</span></span> <span data-ttu-id="26b59-115">Breeze/angular テンプレートは、データ アクセスのデータ バインディングと Breeze Angular を使用します。</span><span class="sxs-lookup"><span data-stu-id="26b59-115">The Breeze/Angular template uses Angular for data binding and Breeze for data access.</span></span> <span data-ttu-id="26b59-116">これらのライブラリは、ページ ナビゲーションと履歴を含む、追加の機能を有効にします。</span><span class="sxs-lookup"><span data-stu-id="26b59-116">These libaries enable additional capabilities, including page navigation and history.</span></span>

<span data-ttu-id="26b59-117">アプリケーションのに関するページを次に示します。</span><span class="sxs-lookup"><span data-stu-id="26b59-117">Here is the application's About page:</span></span>

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningAboutPage.png)

<span data-ttu-id="26b59-118">このページは、現在のユーザー セッション中にイベントの実行ログを表示を含みます。</span><span class="sxs-lookup"><span data-stu-id="26b59-118">This page displays a running log of events during the current user session, including:</span></span>

- <span data-ttu-id="26b59-119">ページングします。</span><span class="sxs-lookup"><span data-stu-id="26b59-119">Paging.</span></span> <span data-ttu-id="26b59-120">#2 および 7 で Todo コント ローラーの作成に注意してください。</span><span class="sxs-lookup"><span data-stu-id="26b59-120">Note the Todo controller creation at #2 and #7.</span></span>
- <span data-ttu-id="26b59-121">リモート クエリ (3) とローカル キャッシュのクエリ (7)。</span><span class="sxs-lookup"><span data-stu-id="26b59-121">Remote queries (#3) and local cache queries (#7).</span></span>
- <span data-ttu-id="26b59-122">新しく保存する (5, 6) と (4) エンティティを変更します。</span><span class="sxs-lookup"><span data-stu-id="26b59-122">Saving new (#5, #6) and modified (#4) entities.</span></span>
- <span data-ttu-id="26b59-123">データベースに変更をコミットする前に、ユーザーが誤りを修正できるようにを (#9)、クライアントで検証を変更します。</span><span class="sxs-lookup"><span data-stu-id="26b59-123">Changes validated on the client (#9), so the user can correct mistakes before committing changes to the database.</span></span>

<span data-ttu-id="26b59-124">さらに、このテンプレートでの検索含みます。</span><span class="sxs-lookup"><span data-stu-id="26b59-124">There's more to explore in this template, including:</span></span>

- <span data-ttu-id="26b59-125">HTML ビュー テンプレートの動的読み込み。</span><span class="sxs-lookup"><span data-stu-id="26b59-125">Dynamic loading of HTML view templates.</span></span>
- <span data-ttu-id="26b59-126">Angular「ディレクティブ」を使用してカスタム データ バインド</span><span class="sxs-lookup"><span data-stu-id="26b59-126">Custom data binding through Angular "directives."</span></span>
- <span data-ttu-id="26b59-127">モジュール性と依存関係の注入します。</span><span class="sxs-lookup"><span data-stu-id="26b59-127">Modularity and dependency injection.</span></span>
- <span data-ttu-id="26b59-128">クエリ フィルター、並べ替え、ページング、予測、および関連エンティティを含めること。</span><span class="sxs-lookup"><span data-stu-id="26b59-128">Query filters, sorts, paging, projections, and inclusion of related entities.</span></span>
- <span data-ttu-id="26b59-129">複数の画面でのデータを共有します。</span><span class="sxs-lookup"><span data-stu-id="26b59-129">Sharing data across multiple screens.</span></span>
- <span data-ttu-id="26b59-130">単一のトランザクションとしては、複数の変更を保存しています。</span><span class="sxs-lookup"><span data-stu-id="26b59-130">Saving multiple changes as a single transaction.</span></span>
- <span data-ttu-id="26b59-131">JavaScript クライアントに、検証規則が、サーバーからは自動的に反映します。</span><span class="sxs-lookup"><span data-stu-id="26b59-131">Validation rules propagated automatically from the server to the JavaScript client.</span></span>

<span data-ttu-id="26b59-132">では、始めましょう。</span><span class="sxs-lookup"><span data-stu-id="26b59-132">Let's get started.</span></span>

## <a name="create-a-breezeangular-template-project"></a><span data-ttu-id="26b59-133">Breeze/Angular プロジェクト テンプレートを作成します。</span><span class="sxs-lookup"><span data-stu-id="26b59-133">Create a Breeze/Angular Template Project</span></span>

<span data-ttu-id="26b59-134">ダウンロードし、上記の [ダウンロード] ボタンをクリックして、テンプレートをインストールします。</span><span class="sxs-lookup"><span data-stu-id="26b59-134">Download and install the template by clicking the Download button above.</span></span> <span data-ttu-id="26b59-135">テンプレートは、Visual Studio Extension (VSIX) ファイルとしてパッケージ化されます。</span><span class="sxs-lookup"><span data-stu-id="26b59-135">The template is packaged as a Visual Studio Extension (VSIX) file.</span></span> <span data-ttu-id="26b59-136">Visual Studio を再起動する必要があります。</span><span class="sxs-lookup"><span data-stu-id="26b59-136">You might need to restart Visual Studio.</span></span>

<span data-ttu-id="26b59-137">**テンプレート**ペインで、**インストールされたテンプレート**を展開し、 **Visual c#** ノード。</span><span class="sxs-lookup"><span data-stu-id="26b59-137">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="26b59-138">**Visual c#**、 **Web**します。</span><span class="sxs-lookup"><span data-stu-id="26b59-138">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="26b59-139">プロジェクト テンプレートの一覧で選択**ASP.NET MVC 4 Web アプリケーション**します。</span><span class="sxs-lookup"><span data-stu-id="26b59-139">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="26b59-140">プロジェクトの名前し、クリックして**OK**します。</span><span class="sxs-lookup"><span data-stu-id="26b59-140">Name the project and click **OK**.</span></span>

<span data-ttu-id="26b59-141">**新しいプロジェクト**ウィザードで、 **Breeze Angular SPA**します。</span><span class="sxs-lookup"><span data-stu-id="26b59-141">In the **New Project** wizard, select **Breeze Angular SPA**.</span></span>

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeNgSpaTemplate.png)

<span data-ttu-id="26b59-142">ビルド、デバッグを行わずにアプリケーションを実行して ctrl + f5 を押すか、f5 キーを押してデバッグを実行します。</span><span class="sxs-lookup"><span data-stu-id="26b59-142">Press Ctrl-F5 to build and run the application without debugging, or press F5 to run with debugging.</span></span>

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrLogin.png)

<span data-ttu-id="26b59-143">まず、アプリケーションを実行すると、ログイン画面が表示されます。</span><span class="sxs-lookup"><span data-stu-id="26b59-143">When the application first runs, it displays a login screen.</span></span> <span data-ttu-id="26b59-144">「サインアップ」リンクをクリックし、新しいページ滑空をビューにユーザー名とパスワードを入力できます。</span><span class="sxs-lookup"><span data-stu-id="26b59-144">Click the "Sign up" link and a new page glides into view, where you can enter a username and password.</span></span> <span data-ttu-id="26b59-145">(ログインと登録ページは構築される ASP.NET MVC を使用します。)登録フォームを送信するときに、サーバーには、アカウントの 2 つの項目を含む、TodoList が生成されます。</span><span class="sxs-lookup"><span data-stu-id="26b59-145">(The login and registration pages are built using ASP.NET MVC.) When you submit the registration form, the server generates a TodoList with two items for your account.</span></span> <span data-ttu-id="26b59-146">それらに表示を黄色に注意してください。</span><span class="sxs-lookup"><span data-stu-id="26b59-146">Then it presents them to you on a yellow note.</span></span>

![](http://www.breezejs.com/sites/all/images/spa-template/TodoList.png)

<span data-ttu-id="26b59-147">SPA の land するようになりました。</span><span class="sxs-lookup"><span data-stu-id="26b59-147">Now you are in the land of SPA.</span></span> <span data-ttu-id="26b59-148">すべてのものが表示され、todo 項目の操作の表示し、Knockout と簡単に利用してクライアントで管理中に発生します。</span><span class="sxs-lookup"><span data-stu-id="26b59-148">Everything you see and experience while manipulating Todos is rendered and managed on the client with the help of Knockout and Breeze.</span></span> <span data-ttu-id="26b59-149">ユーザーとしてアプリの詳細.</span><span class="sxs-lookup"><span data-stu-id="26b59-149">Explore the app as a user …</span></span> <span data-ttu-id="26b59-150">開発者の目にします。</span><span class="sxs-lookup"><span data-stu-id="26b59-150">but with a developer's eye.</span></span> <span data-ttu-id="26b59-151">お使いのブラウザーで開発者ツールを使用して、ネットワーク トラフィックをキャプチャします。</span><span class="sxs-lookup"><span data-stu-id="26b59-151">Use the developer tools in your browser to capture the network traffic.</span></span> <span data-ttu-id="26b59-152">(Internet explorer: f12 キーを押して選択、**ネットワーク**タブをクリックし、をクリックして**のキャプチャを開始**)。これで、次の点をお試しください。</span><span class="sxs-lookup"><span data-stu-id="26b59-152">(In Internet Explorer: Press F12, select the **Network** tab, and click **Start capturing**.) Now try the following:</span></span>

- <span data-ttu-id="26b59-153">新しい Todo 項目を追加します。</span><span class="sxs-lookup"><span data-stu-id="26b59-153">Add a new Todo item.</span></span>
- <span data-ttu-id="26b59-154">ラベルをクリックし、Todo 項目のタイトルの編集</span><span class="sxs-lookup"><span data-stu-id="26b59-154">Click the label and edit the Todo item title</span></span>
- <span data-ttu-id="26b59-155">項目の完了をマークする チェック ボックスを確認します。</span><span class="sxs-lookup"><span data-stu-id="26b59-155">Check a checkbox to mark the item done.</span></span> <span data-ttu-id="26b59-156">タイトルが編集可能なテキスト ボックスが無効になっていることに注目してください。</span><span class="sxs-lookup"><span data-stu-id="26b59-156">Notice that the textbox is disabled, so the title is no longer editable.</span></span>
- <span data-ttu-id="26b59-157">ラベルの右側に 'x' をクリックします。</span><span class="sxs-lookup"><span data-stu-id="26b59-157">Click the ‘x' to the right of the label.</span></span> <span data-ttu-id="26b59-158">項目が表示されなくなり、データベースから削除されます。</span><span class="sxs-lookup"><span data-stu-id="26b59-158">The item disappears and is deleted from the database.</span></span>
- <span data-ttu-id="26b59-159">別の項目を選択し、そのタイトルをオフにします。</span><span class="sxs-lookup"><span data-stu-id="26b59-159">Pick another item and clear its title.</span></span> <span data-ttu-id="26b59-160">タイトルが必要である検証エラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="26b59-160">You'll get a validation error that the title is required.</span></span> <span data-ttu-id="26b59-161">少し間を置いて、以前のタイトルが復元されます。</span><span class="sxs-lookup"><span data-stu-id="26b59-161">After a brief pause, the previous title is restored.</span></span>
- <span data-ttu-id="26b59-162">驚くほど長いタイトルを入力します。</span><span class="sxs-lookup"><span data-stu-id="26b59-162">Type in a ridiculously long title.</span></span> <span data-ttu-id="26b59-163">タイトルが長すぎるという異なる検証エラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="26b59-163">You'll get a different validation error that the title is too long.</span></span>
- <span data-ttu-id="26b59-164">Todo List"追加"ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="26b59-164">Click the "Add Todo List" button.</span></span> <span data-ttu-id="26b59-165">上記の一覧の左側に新しいリストが表示されます。</span><span class="sxs-lookup"><span data-stu-id="26b59-165">A new list appears to the left of the previous list.</span></span>
- <span data-ttu-id="26b59-166">TodoList のタイトル、トリガーの要求に応じて、および長さの検証を再生します。</span><span class="sxs-lookup"><span data-stu-id="26b59-166">Play with the TodoList title, triggering its required and length validations.</span></span>
- <span data-ttu-id="26b59-167">エラー メッセージを消去するタイトル テキスト ボックスをクリックします。</span><span class="sxs-lookup"><span data-stu-id="26b59-167">Click in the title textbox to clear the error message.</span></span>
- <span data-ttu-id="26b59-168">TodoList と、todo 項目を削除する右上隅にある円の中で [x] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="26b59-168">Click the "x" in the circle in the upper right corner to delete the TodoList and its todos.</span></span>
- <span data-ttu-id="26b59-169">これらのアクティビティのログを表示する右上にある"About"リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="26b59-169">Click the "About" link in the upper right to see a log of these activities.</span></span>

<span data-ttu-id="26b59-170">検証ロジックは、簡単で実行されるクライアント側です。</span><span class="sxs-lookup"><span data-stu-id="26b59-170">The validation logic is performed client-side by Breeze.</span></span> <span data-ttu-id="26b59-171">サーバー モデルのクラスに検証属性がクライアントに伝達し、クライアントは、サーバーに接続する前に自動的に実行します。</span><span class="sxs-lookup"><span data-stu-id="26b59-171">Validation attributes on the server model classes are propagated to the client and executed automatically before the client contacts the server.</span></span>

<span data-ttu-id="26b59-172">ネットワーク トラフィックを確認します。</span><span class="sxs-lookup"><span data-stu-id="26b59-172">Review the network traffic.</span></span> <span data-ttu-id="26b59-173">存在しないこと、サーバー呼び出しを簡単にエラーが検出されたときに注意してください。</span><span class="sxs-lookup"><span data-stu-id="26b59-173">Notice that there were no calls to the server when Breeze detected an error.</span></span> <span data-ttu-id="26b59-174">POST 要求に「/api/Todo/SaveChanges」に有効な各変更が発生しました。</span><span class="sxs-lookup"><span data-stu-id="26b59-174">Each valid change resulted in a POST request to "/api/Todo/SaveChanges".</span></span> <span data-ttu-id="26b59-175">簡単、変更をバンドルし、送信まとめて 1 つの要求として、Web API コント ローラーに`SaveChanges`メソッド。</span><span class="sxs-lookup"><span data-stu-id="26b59-175">Breeze bundles the changes and sends them together as a single request to the Web API controller's `SaveChanges` method.</span></span> <span data-ttu-id="26b59-176">KockoutJS SPA テンプレートは、PUT、POST、および削除の各項目の要求を個別には、異なるです。</span><span class="sxs-lookup"><span data-stu-id="26b59-176">That's different from KockoutJS SPA template, which makes PUT, POST, and DELETE requests for each item individually.</span></span>

<span data-ttu-id="26b59-177">また、およびページについて、TodoList 間を切り替えると、ネットワーク トラフィックはありません。</span><span class="sxs-lookup"><span data-stu-id="26b59-177">Also, notice there is no network traffic when you switch between the TodoList and About pages.</span></span> <span data-ttu-id="26b59-178">Breeze のローカル キャッシュにクエリが制約されているためにです。</span><span class="sxs-lookup"><span data-stu-id="26b59-178">That's because the query has been constrained to the local Breeze cache.</span></span>

## <a name="peek-inside"></a><span data-ttu-id="26b59-179">内部を見る</span><span class="sxs-lookup"><span data-stu-id="26b59-179">Peek inside</span></span>

<span data-ttu-id="26b59-180">このアプリケーションは、クライアント側とサーバー側を持ちます。</span><span class="sxs-lookup"><span data-stu-id="26b59-180">This application has a client side and a server side.</span></span> <span data-ttu-id="26b59-181">クライアント側のスタックは、少しの HTML およびアプリケーションの JavaScript モジュール ("app"フォルダー) 内の組み合わせプラス ("Scripts"フォルダーにあります) でサード パーティ製の JavaScript ライブラリで構成されます。</span><span class="sxs-lookup"><span data-stu-id="26b59-181">The client-side stack consists of a little HTML and a combination of application JavaScript modules (in the "app" folder) plus third-party JavaScript libraries (in the "Scripts" folder).</span></span>

![](http://www.breezejs.com/sites/all/images/spa-template/NgClientArchitecture2.png)

<span data-ttu-id="26b59-182">UI のアーキテクチャでは、コント ローラーでのサポートのプレゼンテーション コードとビューの HTML ウィジェットを分離します。</span><span class="sxs-lookup"><span data-stu-id="26b59-182">The UI architecture separates the HTML widgets of the views from the supporting presentation code in the controllers.</span></span> <span data-ttu-id="26b59-183">Angular のデータ バインド システムは、それぞれ他の詳しい知識がない、そのジョブを実行できるように、ビューとコント ローラーを調整します。</span><span class="sxs-lookup"><span data-stu-id="26b59-183">The Angular data-binding system coordinates views and controllers so that each can do its job without intimate knowledge of the other.</span></span>

<span data-ttu-id="26b59-184">コント ローラーは、取得、およびモデルのエンティティを保存するデータ コンテキストを確認します。</span><span class="sxs-lookup"><span data-stu-id="26b59-184">The controller asks the data context to acquire and save the model entities.</span></span> <span data-ttu-id="26b59-185">データ コンテキストは、ほとんどの簡単で、JSON クエリの結果から、自己追跡のモデル オブジェクトを構築する作業を委任します。</span><span class="sxs-lookup"><span data-stu-id="26b59-185">The data context delegates most of the work to Breeze, which constructs self-tracking model objects from JSON query results.</span></span>

<span data-ttu-id="26b59-186">サーバー側スタックは、開発者コードと 3 つの原則の .NET ライブラリで構成されます Web API、Entity Framework、および Breeze.NET:。</span><span class="sxs-lookup"><span data-stu-id="26b59-186">The server-side stack consists of some developer code and three principle .NET libraries: Web API, Entity Framework, and Breeze.NET:</span></span>

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

<span data-ttu-id="26b59-187">基本的なアーキテクチャでは、KockoutJS SPA テンプレートと同じです。</span><span class="sxs-lookup"><span data-stu-id="26b59-187">The basic architecture is the same as the KockoutJS SPA template.</span></span> <span data-ttu-id="26b59-188">ただし、実装ははるかに簡単です。、Dto が削除された、およびエンティティ フレームワークの詳細のほとんどを Breeze.NET に委任されています。</span><span class="sxs-lookup"><span data-stu-id="26b59-188">However, the implementation is much simpler: The DTOs were deleted, and most of the Entity Framework details have been delegated to Breeze.NET.</span></span>

## <a name="next-steps"></a><span data-ttu-id="26b59-189">次の手順</span><span class="sxs-lookup"><span data-stu-id="26b59-189">Next Steps</span></span>

<span data-ttu-id="26b59-190">によって実行されるコードを検証することをお勧め、[広範なディスカッション](http://www.breezejs.com/ng-spa-template?utm_source=ms-spa)クライアントと Breeze web サイトでサーバー スタックの両方の。</span><span class="sxs-lookup"><span data-stu-id="26b59-190">We suggest that you explore the code, guided by the [extensive discussion](http://www.breezejs.com/ng-spa-template?utm_source=ms-spa) of both the client and the server stacks on the Breeze website.</span></span>

<span data-ttu-id="26b59-191">Breeze クライアント側のクエリでの再生を試すことがあります。いくつかのフィルターと並べ替えを追加します。</span><span class="sxs-lookup"><span data-stu-id="26b59-191">You might try playing with Breeze client-side query; add some filters and sorts.</span></span> <span data-ttu-id="26b59-192">モデルのプロパティとのエンド ツー エンドの SPA の開発を理解する複数のエンティティを追加する場合があります。</span><span class="sxs-lookup"><span data-stu-id="26b59-192">You might add more model properties and more entities to get a better feel for end-to-end SPA development.</span></span> <span data-ttu-id="26b59-193">設計の確信できる場合は、Todo の機能を破棄し、独自で置き換えることすることができます。</span><span class="sxs-lookup"><span data-stu-id="26b59-193">When you are confident of the design, you can tear out the Todo features and replace them with your own.</span></span>

<span data-ttu-id="26b59-194">コーディングを楽しんでください。</span><span class="sxs-lookup"><span data-stu-id="26b59-194">Happy coding!</span></span>
