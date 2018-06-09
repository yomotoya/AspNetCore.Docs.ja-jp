---
uid: single-page-application/overview/templates/backbonejs-template
title: Backbone テンプレート |Microsoft ドキュメント
author: madskristensen
description: Backbone.js SPA テンプレート
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/04/2013
ms.topic: article
ms.assetid: 00aca413-f067-4108-9bd1-cf21e64a2646
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/backbonejs-template
msc.type: authoredcontent
ms.openlocfilehash: 3b8eabd3cefcb96dc40bbf6cc6e3ee81accb0d7c
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "26506681"
---
<a name="backbone-template"></a><span data-ttu-id="81980-103">Backbone テンプレート</span><span class="sxs-lookup"><span data-stu-id="81980-103">Backbone Template</span></span>
====================
<span data-ttu-id="81980-104">によって[Mads Kristensen](https://github.com/madskristensen)</span><span class="sxs-lookup"><span data-stu-id="81980-104">by [Mads Kristensen](https://github.com/madskristensen)</span></span>

> <span data-ttu-id="81980-105">Kazi Manzur Rashid によってバックボーン SPA テンプレートが書き込まれました</span><span class="sxs-lookup"><span data-stu-id="81980-105">The Backbone SPA Template was written by Kazi Manzur Rashid</span></span>
> 
> [<span data-ttu-id="81980-106">テンプレートをダウンロードして Backbone.js SPA</span><span class="sxs-lookup"><span data-stu-id="81980-106">Download the Backbone.js SPA Template</span></span>](https://go.microsoft.com/fwlink/?LinkId=293631)


<span data-ttu-id="81980-107">Backbone.js SPA テンプレートを使用して対話型のクライアント側 web アプリの構築をすばやく開始するように設計された[Backbone.js です。](http://backbonejs.org/)</span><span class="sxs-lookup"><span data-stu-id="81980-107">The Backbone.js SPA template is designed to get you started quickly building interactive client-side web apps using [Backbone.js.](http://backbonejs.org/)</span></span>

<span data-ttu-id="81980-108">テンプレートは、ASP.NET MVC で Backbone.js アプリケーションの開発の初期のスケルトンを提供します。</span><span class="sxs-lookup"><span data-stu-id="81980-108">The template provides an initial skeleton for developing a Backbone.js application in ASP.NET MVC.</span></span> <span data-ttu-id="81980-109">すぐに、ユーザーのサインアップ、サインイン、パスワードのリセットと基本的な電子メール テンプレートを使用してユーザーの確認など、基本的なユーザー ログインの機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="81980-109">Out of the box it provides basic user login functionality, including user sign-up, sign-in, password reset, and user confirmation with basic email templates.</span></span>

<span data-ttu-id="81980-110">要件:</span><span class="sxs-lookup"><span data-stu-id="81980-110">Requirements:</span></span>

- [<span data-ttu-id="81980-111">ASP.NET および Web ツール 2012.2 の更新</span><span class="sxs-lookup"><span data-stu-id="81980-111">ASP.NET and Web Tools 2012.2 update</span></span>](https://go.microsoft.com/fwlink/?LinkId=282650)

## <a name="create-a-backbone-template-project"></a><span data-ttu-id="81980-112">Backbone テンプレート プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="81980-112">Create a Backbone Template Project</span></span>

<span data-ttu-id="81980-113">ダウンロードし、上記の [ダウンロード] ボタンをクリックして、テンプレートをインストールします。</span><span class="sxs-lookup"><span data-stu-id="81980-113">Download and install the template by clicking the Download button above.</span></span> <span data-ttu-id="81980-114">テンプレートは、Visual Studio Extension (VSIX) ファイルとしてパッケージされています。</span><span class="sxs-lookup"><span data-stu-id="81980-114">The template is packaged as a Visual Studio Extension (VSIX) file.</span></span> <span data-ttu-id="81980-115">Visual Studio を再起動する必要があります。</span><span class="sxs-lookup"><span data-stu-id="81980-115">You might need to restart Visual Studio.</span></span>

<span data-ttu-id="81980-116">**テンプレート**ペインで、**インストールされたテンプレート**を展開し、 **Visual c#** ノード。</span><span class="sxs-lookup"><span data-stu-id="81980-116">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="81980-117">**Visual c#** **Web**です。</span><span class="sxs-lookup"><span data-stu-id="81980-117">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="81980-118">プロジェクト テンプレートの一覧で選択**ASP.NET MVC 4 Web Application**です。</span><span class="sxs-lookup"><span data-stu-id="81980-118">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="81980-119">プロジェクトの名前を指定し、をクリックして**OK**です。</span><span class="sxs-lookup"><span data-stu-id="81980-119">Name the project and click **OK**.</span></span>

![](backbonejs-template/_static/image1.png)

<span data-ttu-id="81980-120">**新しいプロジェクト**Backbone.js SPA プロジェクトのウィザードを選択します。</span><span class="sxs-lookup"><span data-stu-id="81980-120">In the **New Project** wizard, select Backbone.js SPA Project.</span></span>

![](backbonejs-template/_static/image2.png)

<span data-ttu-id="81980-121">Ctrl キーを押し、f5 キーを押してビルドおよびデバッグせずにアプリケーションを実行するか、f5 キーを押してデバッグを実行します。</span><span class="sxs-lookup"><span data-stu-id="81980-121">Press Ctrl-F5 to build and run the application without debugging, or press F5 to run with debugging.</span></span>

![](backbonejs-template/_static/image3.png)

<span data-ttu-id="81980-122">「マイ アカウント」をクリックするとは、ログイン ページを表示します。</span><span class="sxs-lookup"><span data-stu-id="81980-122">Clicking "My Account" brings up the login page:</span></span>

![](backbonejs-template/_static/image4.png)

## <a name="walkthrough-client-code"></a><span data-ttu-id="81980-123">チュートリアル: クライアント コード</span><span class="sxs-lookup"><span data-stu-id="81980-123">Walkthrough: Client Code</span></span>

<span data-ttu-id="81980-124">クライアント側で始まてみましょう。</span><span class="sxs-lookup"><span data-stu-id="81980-124">Let's starts with the client side.</span></span> <span data-ttu-id="81980-125">クライアント アプリケーションのスクリプトは ~/Scripts/application フォルダーにあります。</span><span class="sxs-lookup"><span data-stu-id="81980-125">The client application scripts are located in the ~/Scripts/application folder.</span></span> <span data-ttu-id="81980-126">アプリケーションが記述された[TypeScript](http://www.typescriptlang.org/) (.ts ファイル) は、JavaScript (.js ファイル) にコンパイルします。</span><span class="sxs-lookup"><span data-stu-id="81980-126">The application is written in [TypeScript](http://www.typescriptlang.org/) (.ts files) which are compiled into JavaScript (.js files).</span></span>

<span data-ttu-id="81980-127">**アプリケーション**</span><span class="sxs-lookup"><span data-stu-id="81980-127">**Application**</span></span>

<span data-ttu-id="81980-128">`Application` application.ts で定義されます。</span><span class="sxs-lookup"><span data-stu-id="81980-128">`Application` is defined in application.ts.</span></span> <span data-ttu-id="81980-129">このオブジェクトは、アプリケーションを初期化し、ルート名前空間として機能します。</span><span class="sxs-lookup"><span data-stu-id="81980-129">This object initializes the application and acts as the root namespace.</span></span> <span data-ttu-id="81980-130">ユーザーがサインインしているかどうかなど、アプリケーション全体で共有されている構成と状態の情報を保持します。</span><span class="sxs-lookup"><span data-stu-id="81980-130">It maintains configuration and state information that is shared across the application, such as whether the user is signed in.</span></span>

<span data-ttu-id="81980-131">`application.start`メソッドは、モーダル ビューを作成し、ユーザー サインインなど、アプリケーション レベルのイベントのイベント ハンドラーをアタッチします。</span><span class="sxs-lookup"><span data-stu-id="81980-131">The `application.start` method creates the modal views and attaches event handlers for application-level events, such as user sign-in.</span></span> <span data-ttu-id="81980-132">次に、既定のルーターを作成し、任意のクライアント側の URL が指定されているかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="81980-132">Next, it creates the default router and checks whether any client-side URL is specified.</span></span> <span data-ttu-id="81980-133">既定の url にリダイレクトされていない場合 (#!/)。</span><span class="sxs-lookup"><span data-stu-id="81980-133">If not, it redirects to the default url (#!/).</span></span>

<span data-ttu-id="81980-134">**イベント**</span><span class="sxs-lookup"><span data-stu-id="81980-134">**Events**</span></span>

<span data-ttu-id="81980-135">イベントは、コンポーネント開発疎結合されたときに常に重要です。</span><span class="sxs-lookup"><span data-stu-id="81980-135">Events are always important when developing loosely coupled components.</span></span> <span data-ttu-id="81980-136">アプリケーションは、多くの場合、ユーザーの操作への応答で複数の操作を実行します。</span><span class="sxs-lookup"><span data-stu-id="81980-136">Applications often perform multiple operations in response to a user action.</span></span> <span data-ttu-id="81980-137">Backbone は、モデル、コレクション、およびビューなどのコンポーネントでの組み込みのイベントを提供します。</span><span class="sxs-lookup"><span data-stu-id="81980-137">Backbone provides built-in events with components such as Model, Collection, and View.</span></span> <span data-ttu-id="81980-138">これらのコンポーネント間の間の依存関係を作成するには、代わりに、テンプレートは「パブリッシュ/サブスクライブ」モデルを使用: `events` events.ts で定義されている、オブジェクトの公開とアプリケーションのイベントにサブスクライブするため、イベント ハブとして機能します。</span><span class="sxs-lookup"><span data-stu-id="81980-138">Instead of creating inter-dependencies among these components, the template uses a "pub/sub" model: The `events` object, defined in events.ts, acts as an event hub for publishing and subscribing to application events.</span></span> <span data-ttu-id="81980-139">`events`オブジェクトがシングルトンです。</span><span class="sxs-lookup"><span data-stu-id="81980-139">The `events` object is a singleton.</span></span> <span data-ttu-id="81980-140">次のコードでは、イベントにサブスクライブし、イベントをトリガーする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="81980-140">The following code shows how to subscribe to an event and then trigger the event:</span></span>

[!code-csharp[Main](backbonejs-template/samples/sample1.cs)]

<span data-ttu-id="81980-141">**ルーター**</span><span class="sxs-lookup"><span data-stu-id="81980-141">**Router**</span></span>

<span data-ttu-id="81980-142">Backbone.js では、ルーターは、クライアント側のページのルーティングとアクションとイベントへの接続のためのメソッドを提供します。</span><span class="sxs-lookup"><span data-stu-id="81980-142">In Backbone.js, a router provides methods for routing client-side pages and connecting them to actions and events.</span></span> <span data-ttu-id="81980-143">テンプレートは、router.ts で 1 台のルーターを定義します。</span><span class="sxs-lookup"><span data-stu-id="81980-143">The template defines a single router, in router.ts.</span></span> <span data-ttu-id="81980-144">ルーターは、activable ビューを作成し、ビューを切り替えるときに状態を維持します。</span><span class="sxs-lookup"><span data-stu-id="81980-144">The router creates the activable views and maintains the state when switching views.</span></span> <span data-ttu-id="81980-145">(Activable ビューは 次のセクションで説明します)。最初に、プロジェクトでは 2 つのダミー ビュー、およびホームです。</span><span class="sxs-lookup"><span data-stu-id="81980-145">(Activable views are described in the next section.) Initially, the project has two dummy views, Home and About.</span></span> <span data-ttu-id="81980-146">ルートが不明の場合に表示される NotFound ビューもあります。</span><span class="sxs-lookup"><span data-stu-id="81980-146">It also has a NotFound view, which is displayed if the route is not known.</span></span>

<span data-ttu-id="81980-147">**ビュー**</span><span class="sxs-lookup"><span data-stu-id="81980-147">**Views**</span></span>

<span data-ttu-id="81980-148">ビューは、~/Scripts/application/ビューで定義されます。</span><span class="sxs-lookup"><span data-stu-id="81980-148">The views are defined in ~/Scripts/application/views.</span></span> <span data-ttu-id="81980-149">ビュー、activable ビューおよびモーダル ダイアログ ビューの 2 種類があります。</span><span class="sxs-lookup"><span data-stu-id="81980-149">There are two kinds of views, activable views and modal dialog views.</span></span> <span data-ttu-id="81980-150">Activable ビューは、ルーターによって呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="81980-150">Activable views are invoked by the router.</span></span> <span data-ttu-id="81980-151">Activable ビューが表示されるとその他のすべての activable ビューは非アクティブになります。</span><span class="sxs-lookup"><span data-stu-id="81980-151">When an activable view is shown, all other activable views become inactive.</span></span> <span data-ttu-id="81980-152">Activable ビューを作成するビューを拡張、`Activable`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="81980-152">To create an activable view, extend the view with the `Activable` object:</span></span>

[!code-javascript[Main](backbonejs-template/samples/sample2.js)]

<span data-ttu-id="81980-153">による拡張`Activable` ビューに 2 つの新しいメソッドを追加`activate`と`deactivate`です。</span><span class="sxs-lookup"><span data-stu-id="81980-153">Extending with `Activable` adds two new methods to the view, `activate` and `deactivate`.</span></span> <span data-ttu-id="81980-154">ルーターは、これらのメソッドをアクティブにして、ビューを非アクティブ化を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="81980-154">The router calls these methods to activate and deactive the view.</span></span>

<span data-ttu-id="81980-155">モーダル ビューとして実装されて[Twitter のブートス トラップ](http://twitter.github.com/bootstrap/)モーダル ダイアログ ボックス。</span><span class="sxs-lookup"><span data-stu-id="81980-155">Modal views are implemented as [Twitter Bootstrap](http://twitter.github.com/bootstrap/) modal dialogs.</span></span> <span data-ttu-id="81980-156">`Membership`と`Profile`ビューは、モーダル ビュー。</span><span class="sxs-lookup"><span data-stu-id="81980-156">The `Membership` and `Profile` views are modal views.</span></span> <span data-ttu-id="81980-157">モデル ビューは、アプリケーション イベントによって呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="81980-157">Model views can be invoked by any application events.</span></span> <span data-ttu-id="81980-158">たとえば、 `Navigation` 「マイ アカウント」のリンクをクリックして、ビューを表示するか、`Membership`ビューまたは`Profile`ユーザーがログインしているかどうかに応じて、表示します。</span><span class="sxs-lookup"><span data-stu-id="81980-158">For example, in the `Navigation` view, clicking the "My Account" link shows either the `Membership` view or the `Profile` view, depending on whether the user is logged in.</span></span> <span data-ttu-id="81980-159">`Navigation`アタッチ クリックしてイベント ハンドラーを持つすべての子要素、`data-command`属性。</span><span class="sxs-lookup"><span data-stu-id="81980-159">The `Navigation` attaches click event handlers to any child elements that have the `data-command` attribute.</span></span> <span data-ttu-id="81980-160">HTML マークアップを次に示します。</span><span class="sxs-lookup"><span data-stu-id="81980-160">Here is the HTML markup:</span></span>

[!code-html[Main](backbonejs-template/samples/sample3.html)]

<span data-ttu-id="81980-161">次に、イベントをフック navigation.ts でコードを示します。</span><span class="sxs-lookup"><span data-stu-id="81980-161">Here is the code in navigation.ts to hook up the events:</span></span>

[!code-csharp[Main](backbonejs-template/samples/sample4.cs)]

<span data-ttu-id="81980-162">**モデル**</span><span class="sxs-lookup"><span data-stu-id="81980-162">**Models**</span></span>

<span data-ttu-id="81980-163">モデルは、~/Scripts/application/モデルで定義されます。</span><span class="sxs-lookup"><span data-stu-id="81980-163">The models are defined in ~/Scripts/application/models.</span></span> <span data-ttu-id="81980-164">すべてのモデルは、次の 3 つの基本的な項目をある: 既定の属性、検証規則、およびサーバー側のエンド ポイント。</span><span class="sxs-lookup"><span data-stu-id="81980-164">The models all have three basic things: default attributes, validation rules, and a server-side end point.</span></span> <span data-ttu-id="81980-165">一般的な使用例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="81980-165">Here is a typical example:</span></span>

[!code-javascript[Main](backbonejs-template/samples/sample5.js)]

<span data-ttu-id="81980-166">**プラグイン**</span><span class="sxs-lookup"><span data-stu-id="81980-166">**Plug-ins**</span></span>

<span data-ttu-id="81980-167">~/Scripts/application/lib フォルダーには、いくつかの便利な jQuery プラグインが含まれています。Form.ts ファイルは、フォームのデータを操作するためのプラグインを定義します。</span><span class="sxs-lookup"><span data-stu-id="81980-167">The ~/Scripts/application/lib folder contains a few handy jQuery plug-ins. The form.ts file defines a plug-in for working with form data.</span></span> <span data-ttu-id="81980-168">多くの場合は、シリアル化またはフォームのデータを逆シリアル化し、モデルの検証エラーを表示する必要があります。</span><span class="sxs-lookup"><span data-stu-id="81980-168">Often you need to serialize or deserialize form data and show any model validation errors.</span></span> <span data-ttu-id="81980-169">プラグインの form.ts などのメソッドがあります`serializeFields`、 `deserializeFields`、および`showFieldErrors`です。</span><span class="sxs-lookup"><span data-stu-id="81980-169">The form.ts plug-in has methods such as `serializeFields`, `deserializeFields`, and `showFieldErrors`.</span></span> <span data-ttu-id="81980-170">次の例では、モデルにフォームをシリアル化する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="81980-170">The following example shows how to serialize a form to a model.</span></span>

[!code-javascript[Main](backbonejs-template/samples/sample6.js)]

<span data-ttu-id="81980-171">Flashbar.ts プラグインでは、さまざまな種類のフィードバック メッセージをユーザーに与えられます。</span><span class="sxs-lookup"><span data-stu-id="81980-171">The flashbar.ts plug-in gives various kinds of feedback messages to the user.</span></span> <span data-ttu-id="81980-172">メソッドは、 `$.showSuccessbar`、`$.showErrorbar`と`$.showInfobar`です。</span><span class="sxs-lookup"><span data-stu-id="81980-172">The methods are `$.showSuccessbar`, `$.showErrorbar` and `$.showInfobar`.</span></span> <span data-ttu-id="81980-173">背後では、適切にアニメーション化されたメッセージを表示するのにブートス トラップの Twitter のアラートを使用します。</span><span class="sxs-lookup"><span data-stu-id="81980-173">Behind the scenes, it uses Twitter Bootstrap alerts to show nicely animated messages.</span></span>

<span data-ttu-id="81980-174">プラグインの confirm.ts 置換ブラウザーの API では若干異なりますが、ダイアログ ボックスを確認します。</span><span class="sxs-lookup"><span data-stu-id="81980-174">The confirm.ts plug-in replaces the browser's confirm dialog, although the API is somewhat different:</span></span>

[!code-javascript[Main](backbonejs-template/samples/sample7.js)]

## <a name="walkthrough-server-code"></a><span data-ttu-id="81980-175">チュートリアル: サーバー コード</span><span class="sxs-lookup"><span data-stu-id="81980-175">Walkthrough: Server Code</span></span>

<span data-ttu-id="81980-176">これで、サーバー側を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="81980-176">Now let's look at the server side.</span></span>

<span data-ttu-id="81980-177">**コントローラー**</span><span class="sxs-lookup"><span data-stu-id="81980-177">**Controllers**</span></span>

<span data-ttu-id="81980-178">単一ページ アプリケーションでは、サーバーは、ユーザー インターフェイス内の小さなロールのみを再生します。</span><span class="sxs-lookup"><span data-stu-id="81980-178">In a single page application, the server plays only a small role in the user interface.</span></span> <span data-ttu-id="81980-179">通常、サーバーの初期ページを表示および送信し、JSON データを受信します。</span><span class="sxs-lookup"><span data-stu-id="81980-179">Typically, the server renders the initial page and then sends and receives JSON data.</span></span>

<span data-ttu-id="81980-180">テンプレートが 2 つの MVC コント ローラー:`HomeController`最初のページを表示および`SupportsController`を新しいユーザー アカウントを確認し、パスワードをリセットするために使用します。</span><span class="sxs-lookup"><span data-stu-id="81980-180">The template has two MVC controllers: `HomeController` renders the initial page, and `SupportsController` is used to confirm new user accounts and reset passwords.</span></span> <span data-ttu-id="81980-181">テンプレートの他のすべてのコント ローラーは、JSON データを送受信する ASP.NET Web API コント ローラーです。</span><span class="sxs-lookup"><span data-stu-id="81980-181">All other controllers in the template are ASP.NET Web API controllers, which send and receive JSON data.</span></span> <span data-ttu-id="81980-182">既定では、コント ローラーを使用して、新しい`WebSecurity`をユーザーに関連するタスクを実行するクラス。</span><span class="sxs-lookup"><span data-stu-id="81980-182">By default, the controllers use the new `WebSecurity` class to perform user-related tasks.</span></span> <span data-ttu-id="81980-183">ただし、それらも省略可能なコンス トラクターを持つこれらのタスクのデリゲートに渡すことが許可します。</span><span class="sxs-lookup"><span data-stu-id="81980-183">However, they also have optional constructors that let you pass in delegates for these tasks.</span></span> <span data-ttu-id="81980-184">これにより、テストを容易になり、置き換えることができます`WebSecurity`IoC コンテナーを使用して、他のものとします。</span><span class="sxs-lookup"><span data-stu-id="81980-184">This makes testing easier, and lets you replace `WebSecurity` with something else, by using an IoC Container.</span></span> <span data-ttu-id="81980-185">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="81980-185">Here is an example:</span></span>

[!code-csharp[Main](backbonejs-template/samples/sample8.cs)]

## <a name="views"></a><span data-ttu-id="81980-186">Views</span><span class="sxs-lookup"><span data-stu-id="81980-186">Views</span></span>

<span data-ttu-id="81980-187">ビューはモジュール形式であるように設計されています。 ページの各セクションでは、独自の専用のビューです。</span><span class="sxs-lookup"><span data-stu-id="81980-187">The views are designed to be modular: Each section of a page has its own dedicated view.</span></span> <span data-ttu-id="81980-188">単一ページ アプリケーションに共通する任意の対応するコント ローラーを持たないビューを含めるは。</span><span class="sxs-lookup"><span data-stu-id="81980-188">In a single page application, it is common to include views that do not have any corresponding controller.</span></span> <span data-ttu-id="81980-189">呼び出して、ビューを含めることができます`@Html.Partial('myView')`、面倒でこれを取得します。</span><span class="sxs-lookup"><span data-stu-id="81980-189">You can include a view by calling `@Html.Partial('myView')`, but this gets tedious.</span></span> <span data-ttu-id="81980-190">テンプレートが、ヘルパー メソッドを定義しやすくにするには、 `IncludeClientViews`、すべての指定したフォルダーにビューを表示します。</span><span class="sxs-lookup"><span data-stu-id="81980-190">To make this easier, the template defines a helper method, `IncludeClientViews`, that renders all of the views in a specified folder:</span></span>

[!code-cshtml[Main](backbonejs-template/samples/sample9.cshtml)]

<span data-ttu-id="81980-191">フォルダー名が指定されていない場合、既定のフォルダー名は"ClientViews"がします。</span><span class="sxs-lookup"><span data-stu-id="81980-191">If the folder name is not specified, the default folder name is "ClientViews".</span></span> <span data-ttu-id="81980-192">クライアントの表示は、部分ビューを使用しても、アンダー スコア文字の部分ビューの名前を付けます (たとえば、 `_SignUp`)。</span><span class="sxs-lookup"><span data-stu-id="81980-192">If your client view also uses partial views, name the partial view with an underscore character (for example, `_SignUp`).</span></span> <span data-ttu-id="81980-193">`IncludeClientViews`メソッドは、アンダー スコアで始まる名前を持つすべてのビューを除外します。</span><span class="sxs-lookup"><span data-stu-id="81980-193">The `IncludeClientViews` method excludes any views whose name starts with an underscore.</span></span> <span data-ttu-id="81980-194">ビューに含める部分ビュー、クライアント、呼び出す`Html.ClientView('SignUp')`の代わりに`Html.Partial('_SignUp')`です。</span><span class="sxs-lookup"><span data-stu-id="81980-194">To include a partial view in the client view, call `Html.ClientView('SignUp')` instead of `Html.Partial('_SignUp')`.</span></span>

<span data-ttu-id="81980-195">**電子メールを送信します。**</span><span class="sxs-lookup"><span data-stu-id="81980-195">**Sending Email**</span></span>

<span data-ttu-id="81980-196">テンプレートを使用して電子メールを送信するには、[郵便](http://aboutcode.net/postal)です。</span><span class="sxs-lookup"><span data-stu-id="81980-196">To send email, the template uses [Postal](http://aboutcode.net/postal).</span></span> <span data-ttu-id="81980-197">ただし、郵便を使用してコードの残りの部分から切り離されます、`IMailer`インターフェイスのため、別の実装と簡単に置き換えることができます。</span><span class="sxs-lookup"><span data-stu-id="81980-197">However, Postal is abstracted from the rest of the code with the `IMailer` interface, so you can easily replace it with another implementation.</span></span> <span data-ttu-id="81980-198">電子メール テンプレートは、ビューや電子メール フォルダーにあります。</span><span class="sxs-lookup"><span data-stu-id="81980-198">The email templates are located in the Views/Emails folder.</span></span> <span data-ttu-id="81980-199">送信者の電子メール アドレスが web.config ファイル内で指定された、`sender.email`のキー、 **appSettings**セクションです。</span><span class="sxs-lookup"><span data-stu-id="81980-199">The sender's email address is specified in the web.config file, in the `sender.email` key of the **appSettings** section.</span></span> <span data-ttu-id="81980-200">また、 `debug="true"` 、web.config ファイル、アプリケーションを必要としない開発を高速化するのには、ユーザーは電子メール確認します。</span><span class="sxs-lookup"><span data-stu-id="81980-200">Also, when `debug="true"` in web.config, the application does not require user email confirmation, to speed up development.</span></span>

## <a name="github"></a><span data-ttu-id="81980-201">GitHub</span><span class="sxs-lookup"><span data-stu-id="81980-201">GitHub</span></span>

<span data-ttu-id="81980-202">Backbone.js SPA テンプレートを検索することも[GitHub](https://github.com/kazimanzurrashid/AspNetMvcBackboneJsSpa)です。</span><span class="sxs-lookup"><span data-stu-id="81980-202">You can also find the Backbone.js SPA template on [GitHub](https://github.com/kazimanzurrashid/AspNetMvcBackboneJsSpa).</span></span>
