---
title: "ASP.NET Core のブラウザー リンク"
author: ncarandini
description: "Browser Link は、Visual Studio の機能に 1 つまたは複数の web ブラウザーで、開発環境をリンクする方法について説明します。"
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 09/22/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/using-browserlink
ms.openlocfilehash: 3e62bdd180bb1f5e2ce0645a8cf13c9ffe76197e
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="browser-link-in-aspnet-core"></a><span data-ttu-id="f90de-103">ASP.NET Core のブラウザー リンク</span><span class="sxs-lookup"><span data-stu-id="f90de-103">Browser Link in ASP.NET Core</span></span> 

<span data-ttu-id="f90de-104">によって[Nicolò Carandini](https://github.com/ncarandini)、 [Mike Wasson](https://github.com/MikeWasson)、および[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="f90de-104">By [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="f90de-105">ブラウザー リンクは、開発環境と 1 つまたは複数の web ブラウザーの間の通信チャネルを作成する Visual Studio の機能です。</span><span class="sxs-lookup"><span data-stu-id="f90de-105">Browser Link is a feature in Visual Studio that creates a communication channel between the development environment and one or more web browsers.</span></span> <span data-ttu-id="f90de-106">これはクロス ブラウザー テスト用に便利にいくつかのブラウザーで web のアプリケーションは、一度に更新 Browser Link を使用できます。</span><span class="sxs-lookup"><span data-stu-id="f90de-106">You can use Browser Link to refresh your web application in several browsers at once, which is useful for cross-browser testing.</span></span>

## <a name="browser-link-setup"></a><span data-ttu-id="f90de-107">ブラウザー リンクのセットアップ</span><span class="sxs-lookup"><span data-stu-id="f90de-107">Browser Link setup</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f90de-108">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f90de-108">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="f90de-109">ASP.NET Core 2.x **Web アプリケーション**、**空**、および**Web API**テンプレート プロジェクトを使用して、 [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/)パッケージ リファレンスを含むメタパッケージ[Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/)です。</span><span class="sxs-lookup"><span data-stu-id="f90de-109">The ASP.NET Core 2.x **Web Application**, **Empty**, and **Web API** template projects use the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) meta-package, which contains a package reference for [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/).</span></span> <span data-ttu-id="f90de-110">したがってを使用して、`Microsoft.AspNetCore.All`メタパッケージ Browser Link を使用できるようにする追加の操作は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="f90de-110">Therefore, using the `Microsoft.AspNetCore.All` meta-package requires no further action to make Browser Link available for use.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f90de-111">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f90de-111">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="f90de-112">ASP.NET Core 1.x **Web アプリケーション**プロジェクト テンプレートは、のパッケージの参照を持つ、 [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/)パッケージです。</span><span class="sxs-lookup"><span data-stu-id="f90de-112">The ASP.NET Core 1.x **Web Application** project template has a package reference for the [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) package.</span></span> <span data-ttu-id="f90de-113">**空**または**Web API**テンプレート プロジェクトでは、パッケージの参照を追加する必要があります`Microsoft.VisualStudio.Web.BrowserLink`です。</span><span class="sxs-lookup"><span data-stu-id="f90de-113">The **Empty** or **Web API** template projects require you to add a package reference to `Microsoft.VisualStudio.Web.BrowserLink`.</span></span>

<span data-ttu-id="f90de-114">これは、Visual Studio の機能では、最も簡単な方法にパッケージを追加するため、**空**または**Web API**プロジェクト テンプレートを開くには、 **Package Manager Console** (**ビュー** > **その他のウィンドウ** > **Package Manager Console**) し、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="f90de-114">Since this is a Visual Studio feature, the easiest way to add the package to an **Empty** or **Web API** template project is to open the **Package Manager Console** (**View** > **Other Windows** > **Package Manager Console**) and run the following command:</span></span>

```console
install-package Microsoft.VisualStudio.Web.BrowserLink
```

<span data-ttu-id="f90de-115">また、使用することができます**NuGet Package Manager**です。</span><span class="sxs-lookup"><span data-stu-id="f90de-115">Alternatively, you can use **NuGet Package Manager**.</span></span> <span data-ttu-id="f90de-116">プロジェクト名を右クリックして**ソリューション エクスプ ローラー**選択**NuGet パッケージの管理**:</span><span class="sxs-lookup"><span data-stu-id="f90de-116">Right-click the project name in **Solution Explorer** and choose **Manage NuGet Packages**:</span></span>

![開いている NuGet パッケージ マネージャー](using-browserlink/_static/open-nuget-package-manager.png)

<span data-ttu-id="f90de-118">検索して、パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="f90de-118">Find and install the package:</span></span>

![パッケージで NuGet パッケージ マネージャーを追加します。](using-browserlink/_static/add-package-with-nuget-package-manager.png)

---

### <a name="configuration"></a><span data-ttu-id="f90de-120">構成</span><span class="sxs-lookup"><span data-stu-id="f90de-120">Configuration</span></span>

<span data-ttu-id="f90de-121">`Configure`のメソッド、 *Startup.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="f90de-121">In the `Configure` method of the *Startup.cs* file:</span></span>

```csharp
app.UseBrowserLink();
```

<span data-ttu-id="f90de-122">内のコードは、通常、`if`ブロックを次に示すように、開発環境で Browser Link をのみ有効にします。</span><span class="sxs-lookup"><span data-stu-id="f90de-122">Usually the code is inside an `if` block that only enables Browser Link in the Development environment, as shown here:</span></span>

```csharp
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
    app.UseBrowserLink();
}
```

<span data-ttu-id="f90de-123">詳細については、「[Working with multiple environments](xref:fundamentals/environments)」 (複数の環境の使用) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="f90de-123">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

## <a name="how-to-use-browser-link"></a><span data-ttu-id="f90de-124">Browser Link を使用する方法</span><span class="sxs-lookup"><span data-stu-id="f90de-124">How to use Browser Link</span></span>

<span data-ttu-id="f90de-125">Visual Studio の横にブラウザー リンク ツール バー コントロールの表示を開く ASP.NET Core プロジェクトがある場合は、ときに、**デバッグ ターゲット**ツール バー コントロール。</span><span class="sxs-lookup"><span data-stu-id="f90de-125">When you have an ASP.NET Core project open, Visual Studio shows the Browser Link toolbar control next to the **Debug Target** toolbar control:</span></span>

![ブラウザー リンク ドロップダウン メニュー](using-browserlink/_static/browserLink-dropdown-menu.png)

<span data-ttu-id="f90de-127">ブラウザー リンク ツール バー コントロールで、次の操作を実行できます。</span><span class="sxs-lookup"><span data-stu-id="f90de-127">From the Browser Link toolbar control, you can:</span></span>

* <span data-ttu-id="f90de-128">複数のブラウザーで web アプリケーションを一度にを更新します。</span><span class="sxs-lookup"><span data-stu-id="f90de-128">Refresh the web application in several browsers at once.</span></span>
* <span data-ttu-id="f90de-129">開く、**ブラウザー リンク ダッシュ ボード**です。</span><span class="sxs-lookup"><span data-stu-id="f90de-129">Open the **Browser Link Dashboard**.</span></span>
* <span data-ttu-id="f90de-130">有効または無効に**Browser Link**です。</span><span class="sxs-lookup"><span data-stu-id="f90de-130">Enable or disable **Browser Link**.</span></span> <span data-ttu-id="f90de-131">注: ブラウザー リンクは、Visual Studio 2017 (15.3) で既定では無効です。</span><span class="sxs-lookup"><span data-stu-id="f90de-131">Note: Browser Link is disabled by default in Visual Studio 2017 (15.3).</span></span>
* <span data-ttu-id="f90de-132">有効または無効に[CSS 自動同期](#enable-or-disable-css-auto-sync)です。</span><span class="sxs-lookup"><span data-stu-id="f90de-132">Enable or disable [CSS Auto-Sync](#enable-or-disable-css-auto-sync).</span></span>

> [!NOTE]
> <span data-ttu-id="f90de-133">Visual Studio プラグインによって、最も顕著な*Web 拡張機能パック 2015*と*Web 拡張機能パック 2017*ブラウザー リンク拡張機能を提供、ASP でいくつかの追加の機能は機能しません。NET Core プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="f90de-133">Some Visual Studio plug-ins, most notably *Web Extension Pack 2015* and *Web Extension Pack 2017*, offer extended functionality for Browser Link, but some of the additional features don't work with ASP.NET Core projects.</span></span>

## <a name="refresh-the-web-application-in-several-browsers-at-once"></a><span data-ttu-id="f90de-134">複数のブラウザーで web アプリケーションを一度に更新します。</span><span class="sxs-lookup"><span data-stu-id="f90de-134">Refresh the web application in several browsers at once</span></span>

<span data-ttu-id="f90de-135">プロジェクトを開始するときに起動する 1 つの web ブラウザーを選択するには、ドロップダウン メニューを使用して、**デバッグ ターゲット**ツール バー コントロール。</span><span class="sxs-lookup"><span data-stu-id="f90de-135">To choose a single web browser to launch when starting the project, use the drop-down menu in the **Debug Target** toolbar control:</span></span>

![F5 キーを押してドロップダウン メニュー](using-browserlink/_static/debug-target-dropdown-menu.png)

<span data-ttu-id="f90de-137">一度に複数のブラウザーを開き、選択**を参照しています.**同一のドロップダウン リストからです。</span><span class="sxs-lookup"><span data-stu-id="f90de-137">To open multiple browsers at once, choose **Browse with...** from the same drop-down.</span></span> <span data-ttu-id="f90de-138">ブラウザーを選択して CTRL キーを押しながらクリックして**参照**:</span><span class="sxs-lookup"><span data-stu-id="f90de-138">Hold down the CTRL key to select the browsers you want, and then click **Browse**:</span></span>

![一度に多くのブラウザーを開く](using-browserlink/_static/open-many-browsers-at-once.png)

<span data-ttu-id="f90de-140">Visual Studio を開いて、インデックスが表示された表示スクリーン ショットと 2 つの開いているブラウザーを次に示します。</span><span class="sxs-lookup"><span data-stu-id="f90de-140">Here's a screenshot showing Visual Studio with the Index view open and two open browsers:</span></span>

![例を次の 2 つのブラウザーとの同期します。](using-browserlink/_static/sync-with-two-browsers-example.png)

<span data-ttu-id="f90de-142">ブラウザー リンク ツール バー コントロールをプロジェクトに接続されているブラウザーを参照してくださいにポインターを合わせます。</span><span class="sxs-lookup"><span data-stu-id="f90de-142">Hover over the Browser Link toolbar control to see the browsers that are connected to the project:</span></span>

![ホバー時のヒント](using-browserlink/_static/hoover-tip.png)

<span data-ttu-id="f90de-144">インデックス ビューを変更し、ブラウザー リンクは、更新ボタンをクリックすると、接続されているすべてのブラウザーは更新されます。</span><span class="sxs-lookup"><span data-stu-id="f90de-144">Change the Index view, and all connected browsers are updated when you click the Browser Link refresh button:</span></span>

![browsers-sync-to-changes](using-browserlink/_static/browsers-sync-to-changes.png)

<span data-ttu-id="f90de-146">ブラウザー リンクは、Visual Studio の外部からを起動して、アプリケーションの URL に移動したブラウザーでも動作します。</span><span class="sxs-lookup"><span data-stu-id="f90de-146">Browser Link also works with browsers that you launch from outside Visual Studio and navigate to the application URL.</span></span>

### <a name="the-browser-link-dashboard"></a><span data-ttu-id="f90de-147">ブラウザー リンク ダッシュ ボード</span><span class="sxs-lookup"><span data-stu-id="f90de-147">The Browser Link Dashboard</span></span>

<span data-ttu-id="f90de-148">Browser Link のドロップダウン メニューを開いているブラウザーとの接続を管理からブラウザー リンク ダッシュ ボードを開きます。</span><span class="sxs-lookup"><span data-stu-id="f90de-148">Open the Browser Link Dashboard from the Browser Link drop down menu to manage the connection with open browsers:</span></span>

![open-browserslink-dashboard](using-browserlink/_static/open-browserlink-dashboard.png)

<span data-ttu-id="f90de-150">ブラウザーが接続されていない場合を選択して非デバッグ セッションを開始することができます、*ブラウザーで表示*リンク。</span><span class="sxs-lookup"><span data-stu-id="f90de-150">If no browser is connected, you can start a non-debugging session by selecting the *View in Browser* link:</span></span>

![browserlink-dashboard-no-connections](using-browserlink/_static/browserlink-dashboard-no-connections.png)

<span data-ttu-id="f90de-152">それ以外の場合、接続されているブラウザーは、各ブラウザーで表示されているページへのパスで示されています。</span><span class="sxs-lookup"><span data-stu-id="f90de-152">Otherwise, the connected browsers are shown with the path to the page that each browser is showing:</span></span>

![browserlink-dashboard-two-connections](using-browserlink/_static/browserlink-dashboard-two-connections.png)

<span data-ttu-id="f90de-154">必要に応じて、その 1 つのブラウザーを更新するリストされているブラウザーの名前をクリックすることができます。</span><span class="sxs-lookup"><span data-stu-id="f90de-154">If you like, you can click on a listed browser name to refresh that single browser.</span></span>

### <a name="enable-or-disable-browser-link"></a><span data-ttu-id="f90de-155">有効にするにまたは Browser Link を無効にします。</span><span class="sxs-lookup"><span data-stu-id="f90de-155">Enable or disable Browser Link</span></span>

<span data-ttu-id="f90de-156">これを無効にした後 Browser Link を再度有効に再接続するようにブラウザーを更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f90de-156">When you re-enable Browser Link after disabling it, you must refresh the browsers to reconnect them.</span></span>

### <a name="enable-or-disable-css-auto-sync"></a><span data-ttu-id="f90de-157">有効にするにまたは、CSS 自動同期を無効にします。</span><span class="sxs-lookup"><span data-stu-id="f90de-157">Enable or disable CSS Auto-Sync</span></span>

<span data-ttu-id="f90de-158">CSS 自動同期を有効にすると、CSS ファイルにすべての変更を加えるときに接続されているブラウザーが自動的に更新します。</span><span class="sxs-lookup"><span data-stu-id="f90de-158">When CSS Auto-Sync is enabled, connected browsers are automatically refreshed when you make any change to CSS files.</span></span>

## <a name="how-does-it-work"></a><span data-ttu-id="f90de-159">Application Insights のしくみ</span><span class="sxs-lookup"><span data-stu-id="f90de-159">How does it work?</span></span>

<span data-ttu-id="f90de-160">Browser Link では、SignalR を使用して、Visual Studio とブラウザー間の通信チャネルを作成します。</span><span class="sxs-lookup"><span data-stu-id="f90de-160">Browser Link uses SignalR to create a communication channel between Visual Studio and the browser.</span></span> <span data-ttu-id="f90de-161">Browser Link を有効にすると、Visual Studio は、複数のクライアント (ブラウザー) に接続できる SignalR サーバーとして機能します。</span><span class="sxs-lookup"><span data-stu-id="f90de-161">When Browser Link is enabled, Visual Studio acts as a SignalR server that multiple clients (browsers) can connect to.</span></span> <span data-ttu-id="f90de-162">Browser Link では、ASP.NET の要求パイプラインにミドルウェア コンポーネントも登録します。</span><span class="sxs-lookup"><span data-stu-id="f90de-162">Browser Link also registers a middleware component in the ASP.NET request pipeline.</span></span> <span data-ttu-id="f90de-163">このコンポーネントは特殊な挿入`<script>`ページ要求ごとに、サーバーからの参照。</span><span class="sxs-lookup"><span data-stu-id="f90de-163">This component injects special `<script>` references into every page request from the server.</span></span> <span data-ttu-id="f90de-164">選択すると、スクリプト参照を表示する**ソースの表示**の最後までスクロールし、ブラウザーで、`<body>`タグの内容。</span><span class="sxs-lookup"><span data-stu-id="f90de-164">You can see the script references by selecting **View source** in the browser and scrolling to the end of the `<body>` tag content:</span></span>

```javascript
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

<span data-ttu-id="f90de-165">ソース ファイルは変更されません。</span><span class="sxs-lookup"><span data-stu-id="f90de-165">Your source files aren't modified.</span></span> <span data-ttu-id="f90de-166">ミドルウェア コンポーネントは、スクリプト参照を動的に挿入します。</span><span class="sxs-lookup"><span data-stu-id="f90de-166">The middleware component injects the script references dynamically.</span></span> 

<span data-ttu-id="f90de-167">ブラウザー側のコードは、すべての JavaScript であるために、ブラウザーのプラグインを必要とせず、SignalR がサポートされているすべてのブラウザーで機能します。</span><span class="sxs-lookup"><span data-stu-id="f90de-167">Because the browser-side code is all JavaScript, it works on all browsers that SignalR supports without requiring a browser plug-in.</span></span>
