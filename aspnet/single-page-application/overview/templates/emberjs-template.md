---
uid: single-page-application/overview/templates/emberjs-template
title: EmberJS テンプレート |Microsoft Docs
author: xqiu
description: EmberJS テンプレート
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/30/2013
ms.topic: article
ms.assetid: 04d5f142-5f62-494a-b5ea-4f3d068d34cb
ms.technology: ''
msc.legacyurl: /single-page-application/overview/templates/emberjs-template
msc.type: authoredcontent
ms.openlocfilehash: 1f5b005180fed15c51b36417cdcd6acc98123c9a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374167"
---
<a name="emberjs-template"></a><span data-ttu-id="fb46f-103">EmberJS テンプレート</span><span class="sxs-lookup"><span data-stu-id="fb46f-103">EmberJS template</span></span>
====================
<span data-ttu-id="fb46f-104">によって[Xinyang Qiu](https://github.com/xqiu)</span><span class="sxs-lookup"><span data-stu-id="fb46f-104">by [Xinyang Qiu](https://github.com/xqiu)</span></span>

> <span data-ttu-id="fb46f-105">EmberJS の MVC テンプレートは、Nathan Totten、Thiago Santos、および Xinyang Qiu によって書き込まれます。</span><span class="sxs-lookup"><span data-stu-id="fb46f-105">The EmberJS MVC Template is written by Nathan Totten, Thiago Santos, and Xinyang Qiu.</span></span>
> 
> [<span data-ttu-id="fb46f-106">EmberJS MVC テンプレートをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="fb46f-106">Download the EmberJS MVC Template</span></span>](https://go.microsoft.com/fwlink/?LinkId=282647)


<span data-ttu-id="fb46f-107">EmberJS SPA テンプレートは EmberJS を使用して対話型のクライアント側 web アプリをすばやく構築を開始するために設計されています。</span><span class="sxs-lookup"><span data-stu-id="fb46f-107">The EmberJS SPA template is designed to get you started quickly building interactive client-side web apps using EmberJS.</span></span>

<span data-ttu-id="fb46f-108">「シングル ページ アプリケーション」(SPA) は、1 つの HTML ページが読み込まれ、新しいページの読み込みではなく、ページを動的に更新する web アプリケーションの一般的な用語です。</span><span class="sxs-lookup"><span data-stu-id="fb46f-108">"Single-page application" (SPA) is the general term for a web application that loads a single HTML page and then updates the page dynamically, instead of loading new pages.</span></span> <span data-ttu-id="fb46f-109">最初のページの読み込み後に、SPA は、AJAX 要求を介してサーバーとで説明します。</span><span class="sxs-lookup"><span data-stu-id="fb46f-109">After the initial page load, the SPA talks with the server through AJAX requests.</span></span>

![](emberjs-template/_static/image1.png)

<span data-ttu-id="fb46f-110">AJAX は目新しいものでは、現在 JavaScript フレームワークを構築し、大規模な高度な SPA アプリケーションを管理するが容易では。</span><span class="sxs-lookup"><span data-stu-id="fb46f-110">AJAX is nothing new, but today there are JavaScript frameworks that make it easier to build and maintain a large sophisticated SPA application.</span></span> <span data-ttu-id="fb46f-111">また、HTML 5、CSS3 が簡単に豊富な Ui を作成します。</span><span class="sxs-lookup"><span data-stu-id="fb46f-111">Also, HTML 5 and CSS3 are making it easier to create rich UIs.</span></span>

<span data-ttu-id="fb46f-112">EmberJS SPA テンプレートを使用して、 [Ember](http://emberjs.com/) AJAX 要求からのページ更新を処理する JavaScript ライブラリです。</span><span class="sxs-lookup"><span data-stu-id="fb46f-112">The EmberJS SPA Template uses the [Ember](http://emberjs.com/) JavaScript library to handle page updates from AJAX requests.</span></span> <span data-ttu-id="fb46f-113">Ember.js では、データ バインディングを使用して、ページを最新のデータと同期します。</span><span class="sxs-lookup"><span data-stu-id="fb46f-113">Ember.js uses data binding to synchronize the page with the latest data.</span></span> <span data-ttu-id="fb46f-114">これにより、任意の JSON データについて説明し、DOM を更新するコードを記述する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="fb46f-114">That way, you don't have to write any of the code that walks through the JSON data and updates the DOM.</span></span> <span data-ttu-id="fb46f-115">代わりに、Ember.js、データを表示する方法を指示する HTML で宣言型属性を配置します。</span><span class="sxs-lookup"><span data-stu-id="fb46f-115">Instead, you put declarative attributes in the HTML that tell Ember.js how to present the data.</span></span>

<span data-ttu-id="fb46f-116">EmberJS テンプレートのとほぼ同じですが、サーバー側で、 [KnockoutJS SPA テンプレート](../introduction/knockoutjs-template.md)します。</span><span class="sxs-lookup"><span data-stu-id="fb46f-116">On the server side, the EmberJS template is almost identical to the [KnockoutJS SPA template](../introduction/knockoutjs-template.md).</span></span> <span data-ttu-id="fb46f-117">HTML ドキュメント、およびクライアントからの AJAX 要求を処理するために ASP.NET Web API を処理するために、ASP.NET MVC を使用します。</span><span class="sxs-lookup"><span data-stu-id="fb46f-117">It uses ASP.NET MVC to serve HTML documents, and ASP.NET Web API to handle AJAX requests from the client.</span></span> <span data-ttu-id="fb46f-118">テンプレートの機能についての詳細についてを参照してください、 [KnockoutJS テンプレート](../introduction/knockoutjs-template.md)ドキュメント。</span><span class="sxs-lookup"><span data-stu-id="fb46f-118">For more information about those aspects of the template, refer to the [KnockoutJS template](../introduction/knockoutjs-template.md) documentation.</span></span> <span data-ttu-id="fb46f-119">このトピックでは、Knockout テンプレートと EmberJS テンプレートの違いについて説明します。</span><span class="sxs-lookup"><span data-stu-id="fb46f-119">This topic focuses on the differences between the Knockout template and the EmberJS template.</span></span>

## <a name="create-an-emberjs-spa-template-project"></a><span data-ttu-id="fb46f-120">EmberJS SPA テンプレート プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="fb46f-120">Create an EmberJS SPA Template Project</span></span>

<span data-ttu-id="fb46f-121">ダウンロードし、上記の [ダウンロード] ボタンをクリックして、テンプレートをインストールします。</span><span class="sxs-lookup"><span data-stu-id="fb46f-121">Download and install the template by clicking the Download button above.</span></span> <span data-ttu-id="fb46f-122">Visual Studio を再起動する必要があります。</span><span class="sxs-lookup"><span data-stu-id="fb46f-122">You might need to restart Visual Studio.</span></span>

<span data-ttu-id="fb46f-123">**テンプレート**ペインで、**インストールされたテンプレート**を展開し、 **Visual c#** ノード。</span><span class="sxs-lookup"><span data-stu-id="fb46f-123">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="fb46f-124">**Visual c#**、 **Web**します。</span><span class="sxs-lookup"><span data-stu-id="fb46f-124">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="fb46f-125">プロジェクト テンプレートの一覧で選択**ASP.NET MVC 4 Web アプリケーション**します。</span><span class="sxs-lookup"><span data-stu-id="fb46f-125">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="fb46f-126">プロジェクトの名前し、クリックして**OK**します。</span><span class="sxs-lookup"><span data-stu-id="fb46f-126">Name the project and click **OK**.</span></span>

![](emberjs-template/_static/image2.png)

<span data-ttu-id="fb46f-127">**新しいプロジェクト**ウィザードで、 **Ember.js SPA プロジェクト**します。</span><span class="sxs-lookup"><span data-stu-id="fb46f-127">In the **New Project** wizard, select **Ember.js SPA Project**.</span></span>

![](emberjs-template/_static/image4.png)

## <a name="emberjs-spa-template-overview"></a><span data-ttu-id="fb46f-128">EmberJS SPA テンプレートの概要</span><span class="sxs-lookup"><span data-stu-id="fb46f-128">EmberJS SPA Template Overview</span></span>

<span data-ttu-id="fb46f-129">EmberJS テンプレートは、jQuery、Ember.js、smooth、対話型の UI を作成する Handlebars.js の組み合わせを使用します。</span><span class="sxs-lookup"><span data-stu-id="fb46f-129">The EmberJS template uses a combination of jQuery, Ember.js, Handlebars.js to create a smooth, interactive UI.</span></span>

<span data-ttu-id="fb46f-130">Ember.js は、クライアント側の MVC パターンを使用して JavaScript ライブラリです。</span><span class="sxs-lookup"><span data-stu-id="fb46f-130">Ember.js is a JavaScript library that uses a client-side MVC pattern.</span></span>

- <span data-ttu-id="fb46f-131">A*テンプレート*、Handlebars テンプレート作成言語で記述されたアプリケーションのユーザー インターフェイスについて説明します。</span><span class="sxs-lookup"><span data-stu-id="fb46f-131">A *template*, written in the Handlebars templating language, describes the application user interface.</span></span> <span data-ttu-id="fb46f-132">リリース モードで、 [Handlebars コンパイラ](https://github.com/Myslik/csharp-ember-handlebars)バンドルし、handlebars テンプレートのコンパイルに使用されます。</span><span class="sxs-lookup"><span data-stu-id="fb46f-132">In release mode, the [Handlebars compiler](https://github.com/Myslik/csharp-ember-handlebars) is used to bundle and compile the handlebars template.</span></span>
- <span data-ttu-id="fb46f-133">A*モデル*(ToDo リストと ToDo 項目)、サーバーから取得したアプリケーション データを格納します。</span><span class="sxs-lookup"><span data-stu-id="fb46f-133">A *model* stores the application data that it gets from the server (ToDo lists and ToDo items).</span></span>
- <span data-ttu-id="fb46f-134">A*コント ローラー*アプリケーションの状態を格納します。</span><span class="sxs-lookup"><span data-stu-id="fb46f-134">A *controller* stores application state.</span></span> <span data-ttu-id="fb46f-135">コント ローラーは、多くの場合、対応するテンプレート モデル データを提示します。</span><span class="sxs-lookup"><span data-stu-id="fb46f-135">Controllers often present model data to the corresponding templates.</span></span>
- <span data-ttu-id="fb46f-136">A*ビュー*アプリケーションのプリミティブのイベントを変換し、コント ローラーに渡されます。</span><span class="sxs-lookup"><span data-stu-id="fb46f-136">A *view* translates primitive events from the application and passes these to the controller.</span></span>
- <span data-ttu-id="fb46f-137">A*ルーター* Url とテンプレートの同期を保つ、アプリケーションの状態を管理します。</span><span class="sxs-lookup"><span data-stu-id="fb46f-137">A *router* manages application state, keeping URLs and templates in sync.</span></span>

<span data-ttu-id="fb46f-138">さらに、Ember データ ライブラリを使用して、(RESTful API を使用してサーバーから取得された) JSON オブジェクトとクライアントのモデルを同期することができます。</span><span class="sxs-lookup"><span data-stu-id="fb46f-138">In addition, the Ember Data library can be used to synchronize JSON objects (obtained from the server through a RESTful API) and the client models.</span></span>

<span data-ttu-id="fb46f-139">EmberJS SPA テンプレートには、8 つの層に、スクリプトが編成されています。</span><span class="sxs-lookup"><span data-stu-id="fb46f-139">The EmberJS SPA template organizes the scripts into eight layers:</span></span>

- <span data-ttu-id="fb46f-140">webapi\_adapter.js、webapi\_serializer.js: ASP.NET Web API を使用する Ember データ ライブラリを拡張します。</span><span class="sxs-lookup"><span data-stu-id="fb46f-140">webapi\_adapter.js, webapi\_serializer.js: Extends the Ember Data library to work with ASP.NET Web API.</span></span>
- <span data-ttu-id="fb46f-141">Scripts/helpers.js: は、新しい Ember Handlebars ヘルパーを定義します。</span><span class="sxs-lookup"><span data-stu-id="fb46f-141">Scripts/helpers.js: Defines new Ember Handlebars helpers.</span></span>
- <span data-ttu-id="fb46f-142">Scripts/app.js: は、アプリを作成し、アダプターとシリアライザーを構成します。</span><span class="sxs-lookup"><span data-stu-id="fb46f-142">Scripts/app.js: Creates the app and configures the adapter and serializer.</span></span>
- <span data-ttu-id="fb46f-143">スクリプト/アプリ/モデル/\*.js: モデルを定義します。</span><span class="sxs-lookup"><span data-stu-id="fb46f-143">Scripts/app/models/\*.js: Defines the models.</span></span>
- <span data-ttu-id="fb46f-144">スクリプト/アプリ/ビュー/\*.js: ビューを定義します。</span><span class="sxs-lookup"><span data-stu-id="fb46f-144">Scripts/app/views/\*.js: Defines the views.</span></span>
- <span data-ttu-id="fb46f-145">コント ローラー アプリケーション/スクリプト//\*.js: コント ローラーを定義します。</span><span class="sxs-lookup"><span data-stu-id="fb46f-145">Scripts/app/controllers/\*.js: Defines the controllers.</span></span>
- <span data-ttu-id="fb46f-146">スクリプト/アプリ/ルート、Scripts/app/router.js: ルートを定義します。</span><span class="sxs-lookup"><span data-stu-id="fb46f-146">Scripts/app/routes, Scripts/app/router.js: Defines the routes.</span></span>
- <span data-ttu-id="fb46f-147">テンプレート/\*.hbs: handlebars テンプレートを定義します。</span><span class="sxs-lookup"><span data-stu-id="fb46f-147">Templates/\*.hbs: Defines the handlebars templates.</span></span>

<span data-ttu-id="fb46f-148">詳細でこれらのスクリプトのいくつかを見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="fb46f-148">Let's look at some of these scripts in more detail.</span></span>

## <a name="models"></a><span data-ttu-id="fb46f-149">モデル</span><span class="sxs-lookup"><span data-stu-id="fb46f-149">Models</span></span>

<span data-ttu-id="fb46f-150">モデルは、スクリプト/アプリ/models フォルダーで定義されます。</span><span class="sxs-lookup"><span data-stu-id="fb46f-150">The models are defined in the Scripts/app/models folder.</span></span> <span data-ttu-id="fb46f-151">2 つのモデル ファイルがある: todoItem.js と todoList.js します。</span><span class="sxs-lookup"><span data-stu-id="fb46f-151">There are two model files: todoItem.js and todoList.js.</span></span>

<span data-ttu-id="fb46f-152">**todo.model.js** to do リストのクライアント側 (ブラウザー) モデルを定義します。</span><span class="sxs-lookup"><span data-stu-id="fb46f-152">**todo.model.js** defines the client-side (browser) models for the to-do lists.</span></span> <span data-ttu-id="fb46f-153">2 つのモデル クラスがあります: todoItem todoList とします。</span><span class="sxs-lookup"><span data-stu-id="fb46f-153">There are two model classes: todoItem and todoList.</span></span> <span data-ttu-id="fb46f-154">Ember、モデルとは、DS のサブクラスです。モデル。</span><span class="sxs-lookup"><span data-stu-id="fb46f-154">In Ember, models are subclasses of DS.Model.</span></span> <span data-ttu-id="fb46f-155">モデル プロパティの属性を持つことができます。</span><span class="sxs-lookup"><span data-stu-id="fb46f-155">A model can have properties with attributes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample1.js)]

<span data-ttu-id="fb46f-156">モデルは、その他のモデルのリレーションシップを定義できます。</span><span class="sxs-lookup"><span data-stu-id="fb46f-156">Models can define relationships to other models:</span></span>

[!code-css[Main](emberjs-template/samples/sample2.css)]

<span data-ttu-id="fb46f-157">モデルには、その他のプロパティにバインドするプロパティが計算ことができます。</span><span class="sxs-lookup"><span data-stu-id="fb46f-157">Models can have computed properties that bind to other properties:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample3.js)]

<span data-ttu-id="fb46f-158">モデルには、オブザーバー関数は、監視対象のプロパティが変更されたときに呼び出されることがあります。</span><span class="sxs-lookup"><span data-stu-id="fb46f-158">Models can have observer functions, which are invoked when an observed property changes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample4.js)]

## <a name="views"></a><span data-ttu-id="fb46f-159">Views</span><span class="sxs-lookup"><span data-stu-id="fb46f-159">Views</span></span>

<span data-ttu-id="fb46f-160">ビューは、スクリプト/アプリ/ビュー フォルダーで定義されます。</span><span class="sxs-lookup"><span data-stu-id="fb46f-160">The views are defined in the Scripts/app/views folder.</span></span> <span data-ttu-id="fb46f-161">ビューは、アプリケーションの UI からのイベントを変換します。</span><span class="sxs-lookup"><span data-stu-id="fb46f-161">A view translates events from the application UI.</span></span> <span data-ttu-id="fb46f-162">イベント ハンドラーは、コント ローラーの関数にコールバックまたはデータ コンテキストを直接呼び出します。</span><span class="sxs-lookup"><span data-stu-id="fb46f-162">An event handler can call back to controller functions, or simply call the data context directly.</span></span>

<span data-ttu-id="fb46f-163">たとえば、次のコードは views/TodoItemEditView.js します。</span><span class="sxs-lookup"><span data-stu-id="fb46f-163">For example, the following code is from views/TodoItemEditView.js.</span></span> <span data-ttu-id="fb46f-164">イベント処理、テキスト入力フィールドを定義します。</span><span class="sxs-lookup"><span data-stu-id="fb46f-164">It defines the event handling for an input text field.</span></span>

[!code-javascript[Main](emberjs-template/samples/sample5.js)]

## <a name="controller"></a><span data-ttu-id="fb46f-165">コントローラー</span><span class="sxs-lookup"><span data-stu-id="fb46f-165">Controller</span></span>

<span data-ttu-id="fb46f-166">コント ローラーは、スクリプト/アプリ/controllers フォルダーで定義されます。</span><span class="sxs-lookup"><span data-stu-id="fb46f-166">The controllers are defined in the Scripts/app/controllers folder.</span></span> <span data-ttu-id="fb46f-167">1 つのモデルを表現する拡張`Ember.ObjectController`:</span><span class="sxs-lookup"><span data-stu-id="fb46f-167">To represent a single model, extend `Ember.ObjectController`:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample6.js)]

<span data-ttu-id="fb46f-168">コント ローラーを拡張することによってモデルのコレクションを表すことができますも`Ember.ArrayController`します。</span><span class="sxs-lookup"><span data-stu-id="fb46f-168">A controller can also represent a collection of models by extending `Ember.ArrayController`.</span></span> <span data-ttu-id="fb46f-169">たとえば、TodoListController がの配列を表します`todoList`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="fb46f-169">For example, the TodoListController represents an array of `todoList` objects.</span></span> <span data-ttu-id="fb46f-170">コント ローラーを todoList id、降順で並べ替えます。</span><span class="sxs-lookup"><span data-stu-id="fb46f-170">The controller sorts by todoList ID, in descending order:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample7.js)]

<span data-ttu-id="fb46f-171">コント ローラーという名前の関数を定義する`addTodoList`、新しい todoList を作成し、配列に追加します。</span><span class="sxs-lookup"><span data-stu-id="fb46f-171">The controller defines a function named `addTodoList`, which creates a new todoList and adds it to the array.</span></span> <span data-ttu-id="fb46f-172">この関数が呼び出される方法を表示するには、テンプレート フォルダー内の todoListTemplate.html をという名前のテンプレート ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="fb46f-172">To see how this function gets called, open the template file named todoListTemplate.html, in the Templates folder.</span></span> <span data-ttu-id="fb46f-173">次のテンプレート コードにバインドするためのボタン、`addTodoList`関数。</span><span class="sxs-lookup"><span data-stu-id="fb46f-173">The following template code binds a button to the `addTodoList` function:</span></span>

[!code-html[Main](emberjs-template/samples/sample8.html)]

<span data-ttu-id="fb46f-174">コント ローラーにも含まれています、`error`プロパティで、エラー メッセージを保持します。</span><span class="sxs-lookup"><span data-stu-id="fb46f-174">The controller also contains an `error` property, which holds an error message.</span></span> <span data-ttu-id="fb46f-175">(TodoListTemplate.html) でもエラー メッセージを表示するテンプレート コードを次に示します。</span><span class="sxs-lookup"><span data-stu-id="fb46f-175">Here is the template code to display the error message (also in todoListTemplate.html):</span></span>

[!code-html[Main](emberjs-template/samples/sample9.html)]

## <a name="routes"></a><span data-ttu-id="fb46f-176">ルート</span><span class="sxs-lookup"><span data-stu-id="fb46f-176">Routes</span></span>

<span data-ttu-id="fb46f-177">Router.js では、ルートとアプリケーションの状態のセットを表示します。 既定のテンプレートを定義し、ルート Url と一致します。</span><span class="sxs-lookup"><span data-stu-id="fb46f-177">Router.js defines the routes and the default template to display, sets up application state, and matches URLs to routes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample10.js)]

<span data-ttu-id="fb46f-178">TodoListRoute.js は、setupController 関数をオーバーライドすることで、TodoListRoute のデータを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="fb46f-178">TodoListRoute.js loads data for the TodoListRoute by overriding the setupController function:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample11.js)]

<span data-ttu-id="fb46f-179">Ember は、Url、ルート名、コント ローラー、およびテンプレートに一致するように、名前付け規則を使用します。</span><span class="sxs-lookup"><span data-stu-id="fb46f-179">Ember uses naming conventions to match URLs, route names, controllers, and templates.</span></span> <span data-ttu-id="fb46f-180">詳細については、次を参照してください。 [ http://emberjs.com/guides/routing/defining-your-routes/ ](http://emberjs.com/guides/routing/defining-your-routes/) EmberJS ドキュメント。</span><span class="sxs-lookup"><span data-stu-id="fb46f-180">For more information, see [http://emberjs.com/guides/routing/defining-your-routes/](http://emberjs.com/guides/routing/defining-your-routes/) at the EmberJS documentation.</span></span>

## <a name="templates"></a><span data-ttu-id="fb46f-181">テンプレート</span><span class="sxs-lookup"><span data-stu-id="fb46f-181">Templates</span></span>

<span data-ttu-id="fb46f-182">テンプレート フォルダーには、4 つのテンプレートが含まれています。</span><span class="sxs-lookup"><span data-stu-id="fb46f-182">The Templates folder contains four templates:</span></span>

- <span data-ttu-id="fb46f-183">application.hbs: アプリケーションの起動時に表示される既定のテンプレート。</span><span class="sxs-lookup"><span data-stu-id="fb46f-183">application.hbs: The default template that is rendered when the application is started.</span></span>
- <span data-ttu-id="fb46f-184">about.hbs:「/約」ルート テンプレート。</span><span class="sxs-lookup"><span data-stu-id="fb46f-184">about.hbs: The template for the "/about" route.</span></span>
- <span data-ttu-id="fb46f-185">index.hbs: ルートのテンプレート「/」のルート。</span><span class="sxs-lookup"><span data-stu-id="fb46f-185">index.hbs: The template for the root "/" route.</span></span>
- <span data-ttu-id="fb46f-186">todoList.hbs: テンプレートを"/todo"ルート。</span><span class="sxs-lookup"><span data-stu-id="fb46f-186">todoList.hbs: The template for the "/todo" route.</span></span>
- <span data-ttu-id="fb46f-187">\_navbar.hbs: テンプレートは、ナビゲーション メニューを定義します。</span><span class="sxs-lookup"><span data-stu-id="fb46f-187">\_navbar.hbs: The template defines the navigation menu.</span></span>

<span data-ttu-id="fb46f-188">アプリケーション テンプレートは、マスター ページと同様に機能します。</span><span class="sxs-lookup"><span data-stu-id="fb46f-188">The application template acts like a master page.</span></span> <span data-ttu-id="fb46f-189">ヘッダー、フッター、および「{{アウトレット}}」をルートによってでは、その他のテンプレートを挿入するが含まれています。</span><span class="sxs-lookup"><span data-stu-id="fb46f-189">It contains a header, a footer, and an "{{outlet}}" to insert other templates in depending on the route.</span></span> <span data-ttu-id="fb46f-190">Ember のアプリケーション テンプレートの詳細については、次を参照してください。 [ http://guides.emberjs.com/v1.10.0/templates/the-application-template//](http://guides.emberjs.com/v1.10.0/templates/the-application-template/)します。</span><span class="sxs-lookup"><span data-stu-id="fb46f-190">For more information about application templates in Ember, see [http://guides.emberjs.com/v1.10.0/templates/the-application-template//](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).</span></span>

<span data-ttu-id="fb46f-191">"/TodoList"テンプレートには、2 つのループ式が含まれています。</span><span class="sxs-lookup"><span data-stu-id="fb46f-191">The "/todoList" template contains two loop expressions.</span></span> <span data-ttu-id="fb46f-192">外側のループが`{{#each controller}}`、および内部ループは`{{#each todos}}`します。</span><span class="sxs-lookup"><span data-stu-id="fb46f-192">The outside loop is `{{#each controller}}`, and the inside loop is `{{#each todos}}`.</span></span> <span data-ttu-id="fb46f-193">次のコードは、組み込み`Ember.Checkbox`表示、カスタマイズされた`App.TodoItemEditView`とのリンクを`deleteTodo`アクション。</span><span class="sxs-lookup"><span data-stu-id="fb46f-193">The following code shows a built-in `Ember.Checkbox` view, a customized `App.TodoItemEditView`, and a link with a `deleteTodo` action.</span></span>

[!code-html[Main](emberjs-template/samples/sample12.html)]

<span data-ttu-id="fb46f-194">`HtmlHelperExtensions` Controllers/HtmlHelperExensions.cs で定義されているクラス定義のヘルパー関数をキャッシュし、テンプレートの挿入時にファイル**デバッグ**に設定されている**true** Web.config ファイルで。</span><span class="sxs-lookup"><span data-stu-id="fb46f-194">The `HtmlHelperExtensions` class, defined in Controllers/HtmlHelperExensions.cs, defines a helper function to cache and insert template files when **debug** is set to **true** in the Web.config file.</span></span> <span data-ttu-id="fb46f-195">この関数は Views/Home/App.cshtml で定義されている ASP.NET MVC ビュー ファイルから呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="fb46f-195">This function is called from the ASP.NET MVC view file defined in Views/Home/App.cshtml:</span></span>

[!code-cshtml[Main](emberjs-template/samples/sample13.cshtml)]

<span data-ttu-id="fb46f-196">関数を引数なしで呼び出されると、すべてのテンプレート ファイル テンプレート フォルダーをレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="fb46f-196">Called with no arguments, the function renders all of the template files in the Templates folder.</span></span> <span data-ttu-id="fb46f-197">サブフォルダーまたは特定のテンプレート ファイルを指定することもできます。</span><span class="sxs-lookup"><span data-stu-id="fb46f-197">You can also specify a subfolder or a specific template file.</span></span>

<span data-ttu-id="fb46f-198">ときに**デバッグ**は**false** 、web.config ファイルで、アプリケーションにはバンドルの項目"~/bundles/templates"が含まれています。</span><span class="sxs-lookup"><span data-stu-id="fb46f-198">When **debug** is **false** in Web.config, the application includes the bundle item "~/bundles/templates".</span></span> <span data-ttu-id="fb46f-199">BundleConfig.cs、Handlebars コンパイラのライブラリを使用するのには、バンドルは、この項目が追加されます。</span><span class="sxs-lookup"><span data-stu-id="fb46f-199">This bundle item is added in BundleConfig.cs, using the Handlebars compiler library:</span></span>

[!code-csharp[Main](emberjs-template/samples/sample14.cs)]
