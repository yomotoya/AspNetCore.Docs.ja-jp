---
uid: single-page-application/overview/templates/breezeangular-template
title: 簡単/角テンプレート |Microsoft ドキュメント
author: madskristensen
description: 簡単/角の単一ページ アプリケーション テンプレート
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/08/2013
ms.topic: article
ms.assetid: db31e909-563a-4516-aadd-62aa210ac7e4
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/breezeangular-template
msc.type: authoredcontent
ms.openlocfilehash: faf28a510a83b7fa07585904344176601c2e1f34
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "26506691"
---
<a name="breezeangular-template"></a><span data-ttu-id="e129b-103">簡単/角テンプレート</span><span class="sxs-lookup"><span data-stu-id="e129b-103">Breeze/Angular template</span></span>
====================
<span data-ttu-id="e129b-104">によって[Mads Kristensen](https://github.com/madskristensen)</span><span class="sxs-lookup"><span data-stu-id="e129b-104">by [Mads Kristensen](https://github.com/madskristensen)</span></span>

> <span data-ttu-id="e129b-105">ワード ベルによって簡単/角の MVC テンプレートが書き込まれました</span><span class="sxs-lookup"><span data-stu-id="e129b-105">The Breeze/Angular MVC Template was written by Ward Bell</span></span>
> 
> [<span data-ttu-id="e129b-106">簡単/角運動の MVC テンプレートをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="e129b-106">Download the Breeze/Angular MVC Template</span></span>](https://go.microsoft.com/fwlink/?LinkId=286437)


<span data-ttu-id="e129b-107">[AngularJS](http://angularjs.org)単一ページ アプリケーション (SPAs) の構築には、Google からオープン ソース ライブラリです。</span><span class="sxs-lookup"><span data-stu-id="e129b-107">[AngularJS](http://angularjs.org) is an open source library from Google for building Single Page Applications (SPAs).</span></span> <span data-ttu-id="e129b-108">データ バインディング、依存関係の挿入、および画面の管理を提供します。</span><span class="sxs-lookup"><span data-stu-id="e129b-108">It offers data binding, dependency injection, and screen management.</span></span> <span data-ttu-id="e129b-109">組み合わせて[BreezeJS](http://www.breezejs.com/?utm_source=ms-spa)、データ モデリングとデータ管理用の別のオープン ソース ライブラリがある、優れた HTML または JavaScript クライアント アプリケーションの基本的な構成要素。</span><span class="sxs-lookup"><span data-stu-id="e129b-109">Combine it with [BreezeJS](http://www.breezejs.com/?utm_source=ms-spa), another open source library for data modeling and data management, and you have the essential ingredients for a great HTML/JavaScript client app.</span></span>

<span data-ttu-id="e129b-110">簡単/角 SPA テンプレートは、 [KnockoutJS SPA テンプレート](../introduction/knockoutjs-template.md)ASP.NET および Web ツール 2012.2 更新プログラムに含まれます。</span><span class="sxs-lookup"><span data-stu-id="e129b-110">The Breeze/Angular SPA template is a variation on the [KnockoutJS SPA template](../introduction/knockoutjs-template.md) included in the ASP.NET and Web Tools 2012.2 Update.</span></span> <span data-ttu-id="e129b-111">Visual Studio を取得したらがあります例 SPA 立ち上がっており実行中 60 秒未満にします。</span><span class="sxs-lookup"><span data-stu-id="e129b-111">If you've got Visual Studio, you'll have an example SPA up and running in less than 60 seconds.</span></span>

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningTodoPage.png)

<span data-ttu-id="e129b-112">外見上、アプリケーション、よく似て KnockoutJS SPA テンプレート。</span><span class="sxs-lookup"><span data-stu-id="e129b-112">Outwardly, the application looks the very similar to the KnockoutJS SPA template.</span></span> <span data-ttu-id="e129b-113">ですが、内部で大きく異なります。</span><span class="sxs-lookup"><span data-stu-id="e129b-113">But it's quite different under the hood.</span></span> <span data-ttu-id="e129b-114">KnockoutJS テンプレートは、データ バインディング、およびデータ アクセス用の生の AJAX の Knockout を使用します。</span><span class="sxs-lookup"><span data-stu-id="e129b-114">The KnockoutJS template uses Knockout for data binding and raw AJAX for data access.</span></span> <span data-ttu-id="e129b-115">簡単/角テンプレートは、データ アクセス用のデータのバインドともとても簡単角を使用します。</span><span class="sxs-lookup"><span data-stu-id="e129b-115">The Breeze/Angular template uses Angular for data binding and Breeze for data access.</span></span> <span data-ttu-id="e129b-116">これらのライブラリには、ページ ナビゲーションと履歴を含む、追加の機能が有効にします。</span><span class="sxs-lookup"><span data-stu-id="e129b-116">These libaries enable additional capabilities, including page navigation and history.</span></span>

<span data-ttu-id="e129b-117">アプリケーションのに関するページを次に示します。</span><span class="sxs-lookup"><span data-stu-id="e129b-117">Here is the application's About page:</span></span>

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningAboutPage.png)

<span data-ttu-id="e129b-118">このページでは、現在のユーザー セッション中にイベントの実行ログを表示を含みます。</span><span class="sxs-lookup"><span data-stu-id="e129b-118">This page displays a running log of events during the current user session, including:</span></span>

- <span data-ttu-id="e129b-119">ページングします。</span><span class="sxs-lookup"><span data-stu-id="e129b-119">Paging.</span></span> <span data-ttu-id="e129b-120">#2 と 7 に、Todo コント ローラーの作成に注意してください。</span><span class="sxs-lookup"><span data-stu-id="e129b-120">Note the Todo controller creation at #2 and #7.</span></span>
- <span data-ttu-id="e129b-121">リモート クエリ (3) とローカル キャッシュ クエリ (7)。</span><span class="sxs-lookup"><span data-stu-id="e129b-121">Remote queries (#3) and local cache queries (#7).</span></span>
- <span data-ttu-id="e129b-122">新規の保存 (5, 6)、(4) エンティティを変更します。</span><span class="sxs-lookup"><span data-stu-id="e129b-122">Saving new (#5, #6) and modified (#4) entities.</span></span>
- <span data-ttu-id="e129b-123">上で検証クライアント (9)、ユーザーが変更をコミットする前に誤りを修正できるように、データベースに変更します。</span><span class="sxs-lookup"><span data-stu-id="e129b-123">Changes validated on the client (#9), so the user can correct mistakes before committing changes to the database.</span></span>

<span data-ttu-id="e129b-124">さらに、このテンプレートで探索を含みます。</span><span class="sxs-lookup"><span data-stu-id="e129b-124">There's more to explore in this template, including:</span></span>

- <span data-ttu-id="e129b-125">HTML ビュー テンプレートの動的な読み込みします。</span><span class="sxs-lookup"><span data-stu-id="e129b-125">Dynamic loading of HTML view templates.</span></span>
- <span data-ttu-id="e129b-126">角「ディレクティブ」を使用してカスタムのデータ バインド</span><span class="sxs-lookup"><span data-stu-id="e129b-126">Custom data binding through Angular "directives."</span></span>
- <span data-ttu-id="e129b-127">モジュール性と依存関係の挿入します。</span><span class="sxs-lookup"><span data-stu-id="e129b-127">Modularity and dependency injection.</span></span>
- <span data-ttu-id="e129b-128">クエリ フィルター、並べ替え、ページング、射影、および関連エンティティを追加します。</span><span class="sxs-lookup"><span data-stu-id="e129b-128">Query filters, sorts, paging, projections, and inclusion of related entities.</span></span>
- <span data-ttu-id="e129b-129">複数の画面でデータを共有します。</span><span class="sxs-lookup"><span data-stu-id="e129b-129">Sharing data across multiple screens.</span></span>
- <span data-ttu-id="e129b-130">単一のトランザクションとして複数の変更を保存しています。</span><span class="sxs-lookup"><span data-stu-id="e129b-130">Saving multiple changes as a single transaction.</span></span>
- <span data-ttu-id="e129b-131">JavaScript クライアントに、検証規則が、サーバーからは自動的に反映します。</span><span class="sxs-lookup"><span data-stu-id="e129b-131">Validation rules propagated automatically from the server to the JavaScript client.</span></span>

<span data-ttu-id="e129b-132">では、始めましょう。</span><span class="sxs-lookup"><span data-stu-id="e129b-132">Let's get started.</span></span>

## <a name="create-a-breezeangular-template-project"></a><span data-ttu-id="e129b-133">簡単/Angular テンプレート プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="e129b-133">Create a Breeze/Angular Template Project</span></span>

<span data-ttu-id="e129b-134">ダウンロードし、上記の [ダウンロード] ボタンをクリックして、テンプレートをインストールします。</span><span class="sxs-lookup"><span data-stu-id="e129b-134">Download and install the template by clicking the Download button above.</span></span> <span data-ttu-id="e129b-135">テンプレートは、Visual Studio Extension (VSIX) ファイルとしてパッケージされています。</span><span class="sxs-lookup"><span data-stu-id="e129b-135">The template is packaged as a Visual Studio Extension (VSIX) file.</span></span> <span data-ttu-id="e129b-136">Visual Studio を再起動する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e129b-136">You might need to restart Visual Studio.</span></span>

<span data-ttu-id="e129b-137">**テンプレート**ペインで、**インストールされたテンプレート**を展開し、 **Visual c#** ノード。</span><span class="sxs-lookup"><span data-stu-id="e129b-137">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="e129b-138">**Visual c#** **Web**です。</span><span class="sxs-lookup"><span data-stu-id="e129b-138">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="e129b-139">プロジェクト テンプレートの一覧で選択**ASP.NET MVC 4 Web Application**です。</span><span class="sxs-lookup"><span data-stu-id="e129b-139">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="e129b-140">プロジェクトの名前を指定し、をクリックして**OK**です。</span><span class="sxs-lookup"><span data-stu-id="e129b-140">Name the project and click **OK**.</span></span>

<span data-ttu-id="e129b-141">**新しいプロジェクト**ウィザードで、**簡単 Angular SPA**です。</span><span class="sxs-lookup"><span data-stu-id="e129b-141">In the **New Project** wizard, select **Breeze Angular SPA**.</span></span>

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeNgSpaTemplate.png)

<span data-ttu-id="e129b-142">Ctrl キーを押し、f5 キーを押してビルドおよびデバッグせずにアプリケーションを実行するか、f5 キーを押してデバッグを実行します。</span><span class="sxs-lookup"><span data-stu-id="e129b-142">Press Ctrl-F5 to build and run the application without debugging, or press F5 to run with debugging.</span></span>

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrLogin.png)

<span data-ttu-id="e129b-143">アプリケーションを最初に実行すると、ログイン画面が表示されます。</span><span class="sxs-lookup"><span data-stu-id="e129b-143">When the application first runs, it displays a login screen.</span></span> <span data-ttu-id="e129b-144">「登録」リンクをクリックし、新しいページ滑空をビューには、ユーザー名とパスワードを入力できます。</span><span class="sxs-lookup"><span data-stu-id="e129b-144">Click the "Sign up" link and a new page glides into view, where you can enter a username and password.</span></span> <span data-ttu-id="e129b-145">(ログインと登録ページは、ビルド ASP.NET MVC を使用して)。登録フォームを送信するときに、サーバーは、アカウントの 2 つの項目を含む TodoList を生成します。</span><span class="sxs-lookup"><span data-stu-id="e129b-145">(The login and registration pages are built using ASP.NET MVC.) When you submit the registration form, the server generates a TodoList with two items for your account.</span></span> <span data-ttu-id="e129b-146">それらに表示する黄色に注意してください。</span><span class="sxs-lookup"><span data-stu-id="e129b-146">Then it presents them to you on a yellow note.</span></span>

![](http://www.breezejs.com/sites/all/images/spa-template/TodoList.png)

<span data-ttu-id="e129b-147">土地の SPA するようになりました。</span><span class="sxs-lookup"><span data-stu-id="e129b-147">Now you are in the land of SPA.</span></span> <span data-ttu-id="e129b-148">すべてのものが表示され、todo などの操作は表示され、Knockout と簡単を利用して、クライアントで管理中に発生します。</span><span class="sxs-lookup"><span data-stu-id="e129b-148">Everything you see and experience while manipulating Todos is rendered and managed on the client with the help of Knockout and Breeze.</span></span> <span data-ttu-id="e129b-149">ユーザーとして、アプリを調査してください.</span><span class="sxs-lookup"><span data-stu-id="e129b-149">Explore the app as a user …</span></span> <span data-ttu-id="e129b-150">開発者の目にします。</span><span class="sxs-lookup"><span data-stu-id="e129b-150">but with a developer's eye.</span></span> <span data-ttu-id="e129b-151">お使いのブラウザーで developer tools を使用して、ネットワーク トラフィックをキャプチャします。</span><span class="sxs-lookup"><span data-stu-id="e129b-151">Use the developer tools in your browser to capture the network traffic.</span></span> <span data-ttu-id="e129b-152">(Internet Explorer で: キーを押して F12、select、**ネットワーク**タブをクリックし、をクリックして**のキャプチャを開始**)。これで、次を再試行してください。</span><span class="sxs-lookup"><span data-stu-id="e129b-152">(In Internet Explorer: Press F12, select the **Network** tab, and click **Start capturing**.) Now try the following:</span></span>

- <span data-ttu-id="e129b-153">新しい Todo 項目を追加します。</span><span class="sxs-lookup"><span data-stu-id="e129b-153">Add a new Todo item.</span></span>
- <span data-ttu-id="e129b-154">ラベルをクリックし、Todo 項目のタイトルの編集</span><span class="sxs-lookup"><span data-stu-id="e129b-154">Click the label and edit the Todo item title</span></span>
- <span data-ttu-id="e129b-155">項目の終了をマークする チェック ボックスを確認します。</span><span class="sxs-lookup"><span data-stu-id="e129b-155">Check a checkbox to mark the item done.</span></span> <span data-ttu-id="e129b-156">タイトルが編集可能なテキスト ボックスが無効になっていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e129b-156">Notice that the textbox is disabled, so the title is no longer editable.</span></span>
- <span data-ttu-id="e129b-157">ラベルの右側にある 'x' をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e129b-157">Click the ‘x' to the right of the label.</span></span> <span data-ttu-id="e129b-158">アイテムは表示されなくなりますされ、データベースから削除されます。</span><span class="sxs-lookup"><span data-stu-id="e129b-158">The item disappears and is deleted from the database.</span></span>
- <span data-ttu-id="e129b-159">別の項目を選択し、そのタイトルをオフにします。</span><span class="sxs-lookup"><span data-stu-id="e129b-159">Pick another item and clear its title.</span></span> <span data-ttu-id="e129b-160">タイトルが必要である検証エラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="e129b-160">You'll get a validation error that the title is required.</span></span> <span data-ttu-id="e129b-161">少し間を置いて、以前のタイトルが復元されます。</span><span class="sxs-lookup"><span data-stu-id="e129b-161">After a brief pause, the previous title is restored.</span></span>
- <span data-ttu-id="e129b-162">長い途方もなく、タイトルを入力します。</span><span class="sxs-lookup"><span data-stu-id="e129b-162">Type in a ridiculously long title.</span></span> <span data-ttu-id="e129b-163">タイトルが長すぎるという別の検証エラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="e129b-163">You'll get a different validation error that the title is too long.</span></span>
- <span data-ttu-id="e129b-164">「Todo リストの追加」ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="e129b-164">Click the "Add Todo List" button.</span></span> <span data-ttu-id="e129b-165">上記の一覧の左側に、新しいリストが表示されます。</span><span class="sxs-lookup"><span data-stu-id="e129b-165">A new list appears to the left of the previous list.</span></span>
- <span data-ttu-id="e129b-166">TodoList タイトル、その必要なトリガーと長さ検証を再生します。</span><span class="sxs-lookup"><span data-stu-id="e129b-166">Play with the TodoList title, triggering its required and length validations.</span></span>
- <span data-ttu-id="e129b-167">エラー メッセージを消去するタイトル テキスト ボックス内をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e129b-167">Click in the title textbox to clear the error message.</span></span>
- <span data-ttu-id="e129b-168">TodoList およびその todo などを削除する右上隅の円の"x"をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e129b-168">Click the "x" in the circle in the upper right corner to delete the TodoList and its todos.</span></span>
- <span data-ttu-id="e129b-169">これらのアクティビティ ログを表示する右上に、"About"リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="e129b-169">Click the "About" link in the upper right to see a log of these activities.</span></span>

<span data-ttu-id="e129b-170">検証ロジックは、とても簡単で実行されるクライアント側です。</span><span class="sxs-lookup"><span data-stu-id="e129b-170">The validation logic is performed client-side by Breeze.</span></span> <span data-ttu-id="e129b-171">サーバー モデルのクラスに検証属性は、クライアントに伝達し、クライアントがサーバーに接続する前に自動的に実行します。</span><span class="sxs-lookup"><span data-stu-id="e129b-171">Validation attributes on the server model classes are propagated to the client and executed automatically before the client contacts the server.</span></span>

<span data-ttu-id="e129b-172">ネットワーク トラフィックを確認します。</span><span class="sxs-lookup"><span data-stu-id="e129b-172">Review the network traffic.</span></span> <span data-ttu-id="e129b-173">存在しないサーバーへの呼び出しもとても簡単にエラーが検出されたときに注意してください。</span><span class="sxs-lookup"><span data-stu-id="e129b-173">Notice that there were no calls to the server when Breeze detected an error.</span></span> <span data-ttu-id="e129b-174">「/Api/Todo/SaveChanges」には、POST 要求に有効な変更が発生しました。</span><span class="sxs-lookup"><span data-stu-id="e129b-174">Each valid change resulted in a POST request to "/api/Todo/SaveChanges".</span></span> <span data-ttu-id="e129b-175">とても簡単です、変更内容をバンドルし、送信一緒に 1 つの要求として、Web API コント ローラーに`SaveChanges`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="e129b-175">Breeze bundles the changes and sends them together as a single request to the Web API controller's `SaveChanges` method.</span></span> <span data-ttu-id="e129b-176">KockoutJS SPA テンプレートは、PUT、POST、および DELETE の各項目に対して要求を個別には、異なっています。</span><span class="sxs-lookup"><span data-stu-id="e129b-176">That's different from KockoutJS SPA template, which makes PUT, POST, and DELETE requests for each item individually.</span></span>

<span data-ttu-id="e129b-177">また、TodoList 間およびページの概要を切り替えたとき、ネットワーク トラフィックがないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="e129b-177">Also, notice there is no network traffic when you switch between the TodoList and About pages.</span></span> <span data-ttu-id="e129b-178">クエリがとても簡単のローカル キャッシュに制約されているためにです。</span><span class="sxs-lookup"><span data-stu-id="e129b-178">That's because the query has been constrained to the local Breeze cache.</span></span>

## <a name="peek-inside"></a><span data-ttu-id="e129b-179">内部のピークします。</span><span class="sxs-lookup"><span data-stu-id="e129b-179">Peek inside</span></span>

<span data-ttu-id="e129b-180">このアプリケーションは、クライアント側およびサーバー側を持ちます。</span><span class="sxs-lookup"><span data-stu-id="e129b-180">This application has a client side and a server side.</span></span> <span data-ttu-id="e129b-181">クライアント側のスタックは、少しの HTML およびアプリケーションの JavaScript モジュール (フォルダー内の"app") の組み合わせ ("Scripts"フォルダー) にサード パーティ製の JavaScript ライブラリとで構成されます。</span><span class="sxs-lookup"><span data-stu-id="e129b-181">The client-side stack consists of a little HTML and a combination of application JavaScript modules (in the "app" folder) plus third-party JavaScript libraries (in the "Scripts" folder).</span></span>

![](http://www.breezejs.com/sites/all/images/spa-template/NgClientArchitecture2.png)

<span data-ttu-id="e129b-182">UI のアーキテクチャは、コント ローラーのサポートのプレゼンテーション コードとビューの HTML ウィジェットを分離します。</span><span class="sxs-lookup"><span data-stu-id="e129b-182">The UI architecture separates the HTML widgets of the views from the supporting presentation code in the controllers.</span></span> <span data-ttu-id="e129b-183">角度のデータ バインド システムでは、各熟知し、その他のジョブの実行できるようにビューとコント ローラーを調整します。</span><span class="sxs-lookup"><span data-stu-id="e129b-183">The Angular data-binding system coordinates views and controllers so that each can do its job without intimate knowledge of the other.</span></span>

<span data-ttu-id="e129b-184">コント ローラーでは、データ コンテキストを取得し、モデルのエンティティの保存を求められます。</span><span class="sxs-lookup"><span data-stu-id="e129b-184">The controller asks the data context to acquire and save the model entities.</span></span> <span data-ttu-id="e129b-185">データ コンテキストは、ほとんどの JSON クエリ結果から自律追跡のモデル オブジェクトの構築もとても簡単に作業を委任します。</span><span class="sxs-lookup"><span data-stu-id="e129b-185">The data context delegates most of the work to Breeze, which constructs self-tracking model objects from JSON query results.</span></span>

<span data-ttu-id="e129b-186">一部の開発者のコードと 3 つの原則 .NET ライブラリのサーバー側のスタックで構成されます Web API、Entity Framework および Breeze.NET:。</span><span class="sxs-lookup"><span data-stu-id="e129b-186">The server-side stack consists of some developer code and three principle .NET libraries: Web API, Entity Framework, and Breeze.NET:</span></span>

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

<span data-ttu-id="e129b-187">基本的なアーキテクチャでは、KockoutJS SPA テンプレートと同じです。</span><span class="sxs-lookup"><span data-stu-id="e129b-187">The basic architecture is the same as the KockoutJS SPA template.</span></span> <span data-ttu-id="e129b-188">ただし、実装はずっと簡単:「dtos の使用が削除され、エンティティ フレームワークの詳細のほとんどが Breeze.NET を委任されています。</span><span class="sxs-lookup"><span data-stu-id="e129b-188">However, the implementation is much simpler: The DTOs were deleted, and most of the Entity Framework details have been delegated to Breeze.NET.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e129b-189">次の手順</span><span class="sxs-lookup"><span data-stu-id="e129b-189">Next Steps</span></span>

<span data-ttu-id="e129b-190">によって実行されるコードを検証することをお勧め、[広範なディスカッション](http://www.breezejs.com/ng-spa-template?utm_source=ms-spa)クライアントとサーバー スタック、とても簡単 web サイトの両方をします。</span><span class="sxs-lookup"><span data-stu-id="e129b-190">We suggest that you explore the code, guided by the [extensive discussion](http://www.breezejs.com/ng-spa-template?utm_source=ms-spa) of both the client and the server stacks on the Breeze website.</span></span>

<span data-ttu-id="e129b-191">クライアント側のクエリをとても簡単です; での再生しようとする可能性があります。一部のフィルターや並べ替えを追加します。</span><span class="sxs-lookup"><span data-stu-id="e129b-191">You might try playing with Breeze client-side query; add some filters and sorts.</span></span> <span data-ttu-id="e129b-192">複数のモデルのプロパティと、エンド ツー エンド SPA 開発の理解を取得する複数のエンティティを追加する場合があります。</span><span class="sxs-lookup"><span data-stu-id="e129b-192">You might add more model properties and more entities to get a better feel for end-to-end SPA development.</span></span> <span data-ttu-id="e129b-193">設計の確信できる場合は、Todo 機能を破棄し、自分と置き換えています。</span><span class="sxs-lookup"><span data-stu-id="e129b-193">When you are confident of the design, you can tear out the Todo features and replace them with your own.</span></span>

<span data-ttu-id="e129b-194">コーディングを楽しんでください。</span><span class="sxs-lookup"><span data-stu-id="e129b-194">Happy coding!</span></span>
