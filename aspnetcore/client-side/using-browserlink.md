---
title: "ASP.NET Core のブラウザー リンク"
author: ncarandini
description: "1 つまたは複数の web ブラウザーを備えた開発環境をリンクしている Visual Studio の機能"
keywords: "ASP.NET Core、browser link を CSS 同期"
ms.author: riande
manager: wpickett
ms.date: 12/28/2016
ms.topic: article
ms.assetid: 11813d4c-3f8a-445a-b23b-e4a57d001abc
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/using-browserlink
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b2ff38288cee3e9ca42a07c219521bb79a00a359
ms.sourcegitcommit: 4e84d8bf5f404bb77f3d41665cf7e7374fc39142
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/05/2017
---
# <a name="introduction-to-browser-link-in-aspnet-core"></a><span data-ttu-id="d3f25-104">ASP.NET Core で Browser Link の概要</span><span class="sxs-lookup"><span data-stu-id="d3f25-104">Introduction to Browser Link in ASP.NET Core</span></span> 

<span data-ttu-id="d3f25-105">によって[Nicolò Carandini](https://github.com/ncarandini)、 [Mike Wasson](https://github.com/MikeWasson)、および[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="d3f25-105">By [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="d3f25-106">ブラウザー リンクは、開発環境と 1 つまたは複数の web ブラウザーの間の通信チャネルを作成する Visual Studio の機能です。</span><span class="sxs-lookup"><span data-stu-id="d3f25-106">Browser Link is a feature in Visual Studio that creates a communication channel between the development environment and one or more web browsers.</span></span> <span data-ttu-id="d3f25-107">これはクロス ブラウザー テスト用に便利にいくつかのブラウザーで web のアプリケーションは、一度に更新 Browser Link を使用できます。</span><span class="sxs-lookup"><span data-stu-id="d3f25-107">You can use Browser Link to refresh your web application in several browsers at once, which is useful for cross-browser testing.</span></span>

## <a name="browser-link-setup"></a><span data-ttu-id="d3f25-108">ブラウザー リンクのセットアップ</span><span class="sxs-lookup"><span data-stu-id="d3f25-108">Browser Link setup</span></span>

<span data-ttu-id="d3f25-109">ASP.NET Core **Web アプリケーション**Visual Studio 2015 のプロジェクト テンプレートと、以降のブラウザー リンクに必要なすべてのものが含まれています。</span><span class="sxs-lookup"><span data-stu-id="d3f25-109">The ASP.NET Core **Web Application** project templates in Visual Studio 2015 and later include everything needed for Browser Link.</span></span>

<span data-ttu-id="d3f25-110">Browser Link を ASP.NET Core を使用して作成したプロジェクトに追加する**空**または**Web API**テンプレート、これらの手順に従います。</span><span class="sxs-lookup"><span data-stu-id="d3f25-110">To add Browser Link to a project that you created by using the ASP.NET Core **Empty** or **Web API** template, follow these steps:</span></span>

1. <span data-ttu-id="d3f25-111">追加、 *Microsoft.VisualStudio.Web.BrowserLink.Loader*パッケージ</span><span class="sxs-lookup"><span data-stu-id="d3f25-111">Add the *Microsoft.VisualStudio.Web.BrowserLink.Loader* package</span></span> 
2. <span data-ttu-id="d3f25-112">構成コードを追加、 *Startup.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="d3f25-112">Add configuration code in the *Startup.cs* file.</span></span>

### <a name="add-the-package"></a><span data-ttu-id="d3f25-113">パッケージを追加します。</span><span class="sxs-lookup"><span data-stu-id="d3f25-113">Add the package</span></span>

<span data-ttu-id="d3f25-114">開くには、パッケージを追加する最も簡単な方法は、これは、Visual Studio の機能であるため、**パッケージ マネージャー コンソール**(**ビュー > その他のウィンドウ > パッケージ マネージャー コンソール**) し、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="d3f25-114">Since this is a Visual Studio feature, the easiest way to add the package is to open the **Package Manager Console** (**View > Other Windows > Package Manager Console**) and run the following command:</span></span>

```console
install-package Microsoft.VisualStudio.Web.BrowserLink.Loader
```

<span data-ttu-id="d3f25-115">また、使用することができます**NuGet Package Manager**です。</span><span class="sxs-lookup"><span data-stu-id="d3f25-115">Alternatively, you can use **NuGet Package Manager**.</span></span>  <span data-ttu-id="d3f25-116">プロジェクト名を右クリックして**ソリューション エクスプ ローラー**を選択して**NuGet パッケージの管理**です。</span><span class="sxs-lookup"><span data-stu-id="d3f25-116">Right-click the project name in **Solution Explorer**, and choose **Manage NuGet Packages**.</span></span> 

![開いている NuGet パッケージ マネージャー](using-browserlink/_static/open-nuget-package-manager.png)

<span data-ttu-id="d3f25-118">検索し、パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="d3f25-118">Then find and install the package.</span></span>

![パッケージで NuGet パッケージ マネージャーを追加します。](using-browserlink/_static/add-package-with-nuget-package-manager.png)

### <a name="add-configuration-code"></a><span data-ttu-id="d3f25-120">構成コードを追加します。</span><span class="sxs-lookup"><span data-stu-id="d3f25-120">Add configuration code</span></span>

<span data-ttu-id="d3f25-121">開く、 *Startup.cs*ファイル、し、、`Configure`メソッドは、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="d3f25-121">Open the *Startup.cs* file, and in the `Configure` method add the following code:</span></span>

```csharp
app.UseBrowserLink();
```

<span data-ttu-id="d3f25-122">内のコードは通常、`if`ブロックを次に示すように、開発環境でのみ Browser Link を有効にします。</span><span class="sxs-lookup"><span data-stu-id="d3f25-122">Usually that code is inside an `if` block that enables Browser Link only in the Development environment, as shown here:</span></span>

<span data-ttu-id="d3f25-123">[!code-csharp[Main](./using-browserlink/sample/BrowserLinkSample/src/BrowserLinkSample/Startup.cs?highlight=1,4&range=40-44)]</span><span class="sxs-lookup"><span data-stu-id="d3f25-123">[!code-csharp[Main](./using-browserlink/sample/BrowserLinkSample/src/BrowserLinkSample/Startup.cs?highlight=1,4&range=40-44)]</span></span>

<span data-ttu-id="d3f25-124">詳細については、次を参照してください。[複数の環境で作業](../fundamentals/environments.md)です。</span><span class="sxs-lookup"><span data-stu-id="d3f25-124">For more information, see [Working with Multiple Environments](../fundamentals/environments.md).</span></span>

## <a name="how-to-use-browser-link"></a><span data-ttu-id="d3f25-125">Browser Link を使用する方法</span><span class="sxs-lookup"><span data-stu-id="d3f25-125">How to use Browser Link</span></span>

<span data-ttu-id="d3f25-126">Visual Studio の横にブラウザー リンク ツール バー コントロールの表示を開く ASP.NET Core プロジェクトがある場合は、ときに、**デバッグ ターゲット**ツール バー コントロール。</span><span class="sxs-lookup"><span data-stu-id="d3f25-126">When you have an ASP.NET Core project open, Visual Studio shows the Browser Link toolbar control next to the **Debug Target** toolbar control:</span></span>

![ブラウザー リンク ドロップダウン メニュー](using-browserlink/_static/browserLink-dropdown-menu.png)

<span data-ttu-id="d3f25-128">ブラウザー リンク ツール バー コントロールで、次の操作を実行できます。</span><span class="sxs-lookup"><span data-stu-id="d3f25-128">From the Browser Link toolbar control, you can:</span></span>

- <span data-ttu-id="d3f25-129">複数のブラウザーで web アプリケーションを一度に更新します。</span><span class="sxs-lookup"><span data-stu-id="d3f25-129">Refresh the web application in several browsers at once</span></span>
- <span data-ttu-id="d3f25-130">開く、**ブラウザー リンク ダッシュ ボード**</span><span class="sxs-lookup"><span data-stu-id="d3f25-130">Open the **Browser Link Dashboard**</span></span>
- <span data-ttu-id="d3f25-131">有効または無効に**ブラウザー リンク**</span><span class="sxs-lookup"><span data-stu-id="d3f25-131">Enable or disable **Browser Link**</span></span>
- <span data-ttu-id="d3f25-132">有効にするにまたは、CSS 自動同期を無効にします。</span><span class="sxs-lookup"><span data-stu-id="d3f25-132">Enable or disable CSS Auto-Sync</span></span>

> [!NOTE]
> <span data-ttu-id="d3f25-133">Visual Studio プラグインによって、最も顕著な*Web 拡張機能パック 2015*と*Web 拡張機能パック 2017*ブラウザー リンク拡張機能を提供、ASP でいくつかの追加の機能は機能しません。NET Core プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="d3f25-133">Some Visual Studio plug-ins, most notably *Web Extension Pack 2015* and *Web Extension Pack 2017*, offer extended functionality for Browser Link, but some of the additional features don't work with ASP.NET Core projects.</span></span>

## <a name="refresh-the-web-application-in-several-browsers-at-once"></a><span data-ttu-id="d3f25-134">複数のブラウザーで web アプリケーションを一度に更新します。</span><span class="sxs-lookup"><span data-stu-id="d3f25-134">Refresh the web application in several browsers at once</span></span>

<span data-ttu-id="d3f25-135">プロジェクトを開始するときに起動する 1 つの web ブラウザーを選択するには、ドロップダウン メニューを使用して、**デバッグ ターゲット**ツール バー コントロール。</span><span class="sxs-lookup"><span data-stu-id="d3f25-135">To choose a single web browser to launch when starting the project, use the drop-down menu in the **Debug Target** toolbar control:</span></span>

![F5 キーを押してドロップダウン メニュー](using-browserlink/_static/debug-target-dropdown-menu.png)

<span data-ttu-id="d3f25-137">一度に複数のブラウザーを開き、選択**を参照しています.**同一のドロップダウン リストからです。</span><span class="sxs-lookup"><span data-stu-id="d3f25-137">To open multiple browsers at once, choose **Browse with...** from the same drop-down.</span></span>  <span data-ttu-id="d3f25-138">ブラウザーを選択して CTRL キーを押しながらクリックして**参照**:</span><span class="sxs-lookup"><span data-stu-id="d3f25-138">Hold down the CTRL key to select the browsers you want, and then click **Browse**:</span></span>

![一度に多くのブラウザーを開く](using-browserlink/_static/open-many-browsers-at-once.png)

<span data-ttu-id="d3f25-140">Visual Studio を開いて、インデックスが表示された表示サンプル スクリーン ショットと 2 つの開いているブラウザーを次に示します。</span><span class="sxs-lookup"><span data-stu-id="d3f25-140">Here's a sample screenshot showing Visual Studio with the Index view open and two open browsers:</span></span>

![例を次の 2 つのブラウザーとの同期します。](using-browserlink/_static/sync-with-two-browsers-example.png)

<span data-ttu-id="d3f25-142">ブラウザー リンク ツール バー コントロールをプロジェクトに接続されているブラウザーを参照してくださいにポインターを合わせます。</span><span class="sxs-lookup"><span data-stu-id="d3f25-142">Hover over the Browser Link toolbar control to see the browsers that are connected to the project:</span></span>

![ホバー時のヒント](using-browserlink/_static/hoover-tip.png)

<span data-ttu-id="d3f25-144">インデックス ビューを変更し、ブラウザー リンクは、更新ボタンをクリックすると、接続されているすべてのブラウザーは更新されます。</span><span class="sxs-lookup"><span data-stu-id="d3f25-144">Change the Index view, and all connected browsers are updated when you click the Browser Link refresh button:</span></span>

![ブラウザー変更の同期](using-browserlink/_static/browsers-sync-to-changes.png)

<span data-ttu-id="d3f25-146">ブラウザー リンクは、Visual Studio の外部からを起動して、アプリケーションの URL に移動したブラウザーでも動作します。</span><span class="sxs-lookup"><span data-stu-id="d3f25-146">Browser Link also works with browsers that you launch from outside Visual Studio and navigate to the application URL.</span></span>

### <a name="the-browser-link-dashboard"></a><span data-ttu-id="d3f25-147">ブラウザー リンク ダッシュ ボード</span><span class="sxs-lookup"><span data-stu-id="d3f25-147">The Browser Link Dashboard</span></span>

<span data-ttu-id="d3f25-148">Browser Link のドロップダウン メニューを開いているブラウザーとの接続を管理からブラウザー リンク ダッシュ ボードを開きます。</span><span class="sxs-lookup"><span data-stu-id="d3f25-148">Open the Browser Link Dashboard from the Browser Link drop down menu to manage the connection with open browsers:</span></span>

![ダッシュ ボード browserslink 開く](using-browserlink/_static/open-browserlink-dashboard.png)

<span data-ttu-id="d3f25-150">クリックして非デバッグ セッションを開始するにはブラウザーが接続されていない場合、_ブラウザーで表示_リンク。</span><span class="sxs-lookup"><span data-stu-id="d3f25-150">If no browser is connected, you can start a non debugging session clicking the _View in Browser_ link:</span></span>

![browserlink ダッシュ ボードいいえ接続](using-browserlink/_static/browserlink-dashboard-no-connections.png)

<span data-ttu-id="d3f25-152">それ以外の場合、各ブラウザーで表示されているページへのパスと、接続されているブラウザーのとおりです。</span><span class="sxs-lookup"><span data-stu-id="d3f25-152">Otherwise, the connected browsers are shown, with the path to the page that each browser is showing:</span></span>

![browserlink-ダッシュ ボードの 2 つの接続](using-browserlink/_static/browserlink-dashboard-two-connections.png)

<span data-ttu-id="d3f25-154">必要に応じて、その 1 つのブラウザーを更新するリストされているブラウザーの名前をクリックすることができます。</span><span class="sxs-lookup"><span data-stu-id="d3f25-154">If you like, you can click on a listed browser name to refresh that single browser.</span></span>

### <a name="enable-or-disable-browser-link"></a><span data-ttu-id="d3f25-155">有効にするにまたは Browser Link を無効にします。</span><span class="sxs-lookup"><span data-stu-id="d3f25-155">Enable or disable Browser Link</span></span>

<span data-ttu-id="d3f25-156">再度有効にするブラウザー リンクが無効にした後ときに、それらを再接続するようにブラウザーを更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d3f25-156">When you re-enable Browser Link after disabling it, you have to refresh the browsers to reconnect them.</span></span>

### <a name="enable-or-disable-css-auto-sync"></a><span data-ttu-id="d3f25-157">有効にするにまたは、CSS 自動同期を無効にします。</span><span class="sxs-lookup"><span data-stu-id="d3f25-157">Enable or disable CSS Auto-Sync</span></span>

<span data-ttu-id="d3f25-158">CSS 自動同期を有効にすると、CSS ファイルにすべての変更を加えるときに接続されているブラウザーが自動的に更新します。</span><span class="sxs-lookup"><span data-stu-id="d3f25-158">When CSS Auto-Sync is enabled, connected browsers are automatically refreshed when you make any change to CSS files.</span></span>

## <a name="how-does-it-work"></a><span data-ttu-id="d3f25-159">Application Insights のしくみ</span><span class="sxs-lookup"><span data-stu-id="d3f25-159">How does it work?</span></span>

<span data-ttu-id="d3f25-160">Browser Link では、SignalR を使用して、Visual Studio とブラウザー間の通信チャネルを作成します。</span><span class="sxs-lookup"><span data-stu-id="d3f25-160">Browser Link uses SignalR to create a communication channel between Visual Studio and the browser.</span></span> <span data-ttu-id="d3f25-161">Browser Link を有効にすると、Visual Studio は、複数のクライアント (ブラウザー) に接続できる SignalR サーバーとして機能します。</span><span class="sxs-lookup"><span data-stu-id="d3f25-161">When Browser Link is enabled, Visual Studio acts as a SignalR server that multiple clients (browsers) can connect to.</span></span> <span data-ttu-id="d3f25-162">Browser Link では、ASP.NET の要求パイプラインにミドルウェア コンポーネントも登録します。</span><span class="sxs-lookup"><span data-stu-id="d3f25-162">Browser Link also registers a middleware component in the ASP.NET request pipeline.</span></span> <span data-ttu-id="d3f25-163">このコンポーネントは特殊な挿入`<script>`ページ要求ごとに、サーバーからの参照。</span><span class="sxs-lookup"><span data-stu-id="d3f25-163">This component injects special `<script>` references into every page request from the server.</span></span> <span data-ttu-id="d3f25-164">選択すると、スクリプト参照を表示する**ソースの表示**の最後までスクロールし、ブラウザーで、`<body>`タグの内容。</span><span class="sxs-lookup"><span data-stu-id="d3f25-164">You can see the script references by selecting **View source** in the browser and scrolling to the end of the `<body>` tag content:</span></span>

```javascript
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

<span data-ttu-id="d3f25-165">ソース ファイルは変更されません。</span><span class="sxs-lookup"><span data-stu-id="d3f25-165">Your source files are not modified.</span></span> <span data-ttu-id="d3f25-166">ミドルウェア コンポーネントは、スクリプト参照を動的に挿入します。</span><span class="sxs-lookup"><span data-stu-id="d3f25-166">The middleware component injects the script references dynamically.</span></span> 

<span data-ttu-id="d3f25-167">ブラウザー側のコードは、すべての JavaScript であるために、任意のブラウザー プラグインを必要とせず、SignalR をサポートするすべてのブラウザーで機能します。</span><span class="sxs-lookup"><span data-stu-id="d3f25-167">Because the browser-side code is all JavaScript, it works on all browsers that SignalR supports, without requiring any browser plug-in.</span></span>
