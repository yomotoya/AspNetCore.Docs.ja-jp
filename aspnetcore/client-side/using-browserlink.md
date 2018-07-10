---
title: ASP.NET Core でのブラウザー リンク
author: ncarandini
description: ブラウザー リンクの 1 つまたは複数の web ブラウザーでの開発環境をリンクしている Visual Studio の機能の方法について説明します。
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 09/22/2017
uid: client-side/using-browserlink
ms.openlocfilehash: 5ab15c841c472e6c9d47bad70fcf5e0c6dc3010f
ms.sourcegitcommit: ea7ec8d47f94cfb8e008d771f647f86bbb4baa44
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/06/2018
ms.locfileid: "37894180"
---
# <a name="browser-link-in-aspnet-core"></a><span data-ttu-id="19fdd-103">ASP.NET Core でのブラウザー リンク</span><span class="sxs-lookup"><span data-stu-id="19fdd-103">Browser Link in ASP.NET Core</span></span>

<span data-ttu-id="19fdd-104">によって[Nicolò Carandini](https://github.com/ncarandini)、 [Mike Wasson](https://github.com/MikeWasson)、および[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="19fdd-104">By [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="19fdd-105">ブラウザー リンクは、Visual studio 開発環境と 1 つまたは複数の web ブラウザーの間の通信チャネルを作成する機能です。</span><span class="sxs-lookup"><span data-stu-id="19fdd-105">Browser Link is a feature in Visual Studio that creates a communication channel between the development environment and one or more web browsers.</span></span> <span data-ttu-id="19fdd-106">クロス ブラウザー テスト用に便利です、いくつかのブラウザーで web のアプリケーションは、一度にを更新するブラウザー リンクを使用できます。</span><span class="sxs-lookup"><span data-stu-id="19fdd-106">You can use Browser Link to refresh your web application in several browsers at once, which is useful for cross-browser testing.</span></span>

## <a name="browser-link-setup"></a><span data-ttu-id="19fdd-107">ブラウザー リンクのセットアップ</span><span class="sxs-lookup"><span data-stu-id="19fdd-107">Browser Link setup</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="19fdd-108">ASP.NET Core 2.1 に移行して、ASP.NET Core 2.0 プロジェクトを変換するときに、 [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)、インストール、 [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/)のパッケージ化します。BrowserLink 機能します。</span><span class="sxs-lookup"><span data-stu-id="19fdd-108">When converting an ASP.NET Core 2.0 project to ASP.NET Core 2.1 and transitioning to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), install the [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) package for BrowserLink functionality.</span></span> <span data-ttu-id="19fdd-109">ASP.NET Core 2.1 のプロジェクト テンプレートを使用して、`Microsoft.AspNetCore.App`既定メタパッケージ。</span><span class="sxs-lookup"><span data-stu-id="19fdd-109">The ASP.NET Core 2.1 project templates use the `Microsoft.AspNetCore.App` metapackage by default.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="19fdd-110">ASP.NET Core 2.0 **Web アプリケーション**、**空**、および**Web API**プロジェクト テンプレートの使用、 [Microsoft.AspNetCore.All メタパッケージ](xref:fundamentals/metapackage)、のパッケージ参照が含まれています[Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/)します。</span><span class="sxs-lookup"><span data-stu-id="19fdd-110">The ASP.NET Core 2.0 **Web Application**, **Empty**, and **Web API** project templates use the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), which contains a package reference for [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/).</span></span> <span data-ttu-id="19fdd-111">したがってを使用して、`Microsoft.AspNetCore.All`メタパッケージにはブラウザー リンクを使用できるようにするさらに操作は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="19fdd-111">Therefore, using the `Microsoft.AspNetCore.All` metapackage requires no further action to make Browser Link available for use.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="19fdd-112">ASP.NET Core 1.x **Web アプリケーション**プロジェクト テンプレートのパッケージ参照がある、 [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/)パッケージ。</span><span class="sxs-lookup"><span data-stu-id="19fdd-112">The ASP.NET Core 1.x **Web Application** project template has a package reference for the [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) package.</span></span> <span data-ttu-id="19fdd-113">**空**または**Web API**プロジェクト テンプレートでは、パッケージ参照を追加する必要があります`Microsoft.VisualStudio.Web.BrowserLink`します。</span><span class="sxs-lookup"><span data-stu-id="19fdd-113">The **Empty** or **Web API** template projects require you to add a package reference to `Microsoft.VisualStudio.Web.BrowserLink`.</span></span>

<span data-ttu-id="19fdd-114">これは、Visual Studio の機能では、最も簡単な方法にパッケージを追加するため、**空**または**Web API**テンプレート プロジェクトを開くことです、**パッケージ マネージャー コンソール**(**ビュー** > **その他の Windows** > **パッケージ マネージャー コンソール**) し、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="19fdd-114">Since this is a Visual Studio feature, the easiest way to add the package to an **Empty** or **Web API** template project is to open the **Package Manager Console** (**View** > **Other Windows** > **Package Manager Console**) and run the following command:</span></span>

```console
install-package Microsoft.VisualStudio.Web.BrowserLink
```

<span data-ttu-id="19fdd-115">また、使用することができます**NuGet パッケージ マネージャー**します。</span><span class="sxs-lookup"><span data-stu-id="19fdd-115">Alternatively, you can use **NuGet Package Manager**.</span></span> <span data-ttu-id="19fdd-116">プロジェクト名を右クリックして**ソリューション エクスプ ローラー**選択**NuGet パッケージの管理**:</span><span class="sxs-lookup"><span data-stu-id="19fdd-116">Right-click the project name in **Solution Explorer** and choose **Manage NuGet Packages**:</span></span>

![開いている NuGet パッケージ マネージャー](using-browserlink/_static/open-nuget-package-manager.png)

<span data-ttu-id="19fdd-118">検索して、パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="19fdd-118">Find and install the package:</span></span>

![パッケージで NuGet パッケージ マネージャーを追加します。](using-browserlink/_static/add-package-with-nuget-package-manager.png)

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="19fdd-120">構成</span><span class="sxs-lookup"><span data-stu-id="19fdd-120">Configuration</span></span>

<span data-ttu-id="19fdd-121">`Startup.Configure` メソッドの場合:</span><span class="sxs-lookup"><span data-stu-id="19fdd-121">In the `Startup.Configure` method:</span></span>

```csharp
app.UseBrowserLink();
```

<span data-ttu-id="19fdd-122">内でコードは、通常、`if`ブロックしかここで示すように、開発環境で Browser Link を有効にします。</span><span class="sxs-lookup"><span data-stu-id="19fdd-122">Usually the code is inside an `if` block that only enables Browser Link in the Development environment, as shown here:</span></span>

```csharp
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
    app.UseBrowserLink();
}
```

<span data-ttu-id="19fdd-123">詳細については、「[Use multiple environments](xref:fundamentals/environments)」(複数の環境の使用) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="19fdd-123">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

## <a name="how-to-use-browser-link"></a><span data-ttu-id="19fdd-124">ブラウザー リンクを使用する方法</span><span class="sxs-lookup"><span data-stu-id="19fdd-124">How to use Browser Link</span></span>

<span data-ttu-id="19fdd-125">Visual Studio がブラウザー リンクのツール バー コントロールを横に表示されますがある場合、ASP.NET Core プロジェクトを開いて、**デバッグ ターゲット**ツール バー コントロール。</span><span class="sxs-lookup"><span data-stu-id="19fdd-125">When you have an ASP.NET Core project open, Visual Studio shows the Browser Link toolbar control next to the **Debug Target** toolbar control:</span></span>

![ブラウザー リンクのドロップダウン メニュー](using-browserlink/_static/browserLink-dropdown-menu.png)

<span data-ttu-id="19fdd-127">ブラウザー リンク ツール バー コントロールでは、次の操作を実行できます。</span><span class="sxs-lookup"><span data-stu-id="19fdd-127">From the Browser Link toolbar control, you can:</span></span>

* <span data-ttu-id="19fdd-128">複数のブラウザーで web アプリケーションを一度にを更新します。</span><span class="sxs-lookup"><span data-stu-id="19fdd-128">Refresh the web application in several browsers at once.</span></span>
* <span data-ttu-id="19fdd-129">開く、**ブラウザー リンク ダッシュ ボード**します。</span><span class="sxs-lookup"><span data-stu-id="19fdd-129">Open the **Browser Link Dashboard**.</span></span>
* <span data-ttu-id="19fdd-130">有効または無効に**Browser Link**します。</span><span class="sxs-lookup"><span data-stu-id="19fdd-130">Enable or disable **Browser Link**.</span></span> <span data-ttu-id="19fdd-131">注: ブラウザー リンクは Visual Studio 2017 (15.3) で既定で無効になります。</span><span class="sxs-lookup"><span data-stu-id="19fdd-131">Note: Browser Link is disabled by default in Visual Studio 2017 (15.3).</span></span>
* <span data-ttu-id="19fdd-132">有効または無効に[CSS 自動同期](#enable-or-disable-css-auto-sync)します。</span><span class="sxs-lookup"><span data-stu-id="19fdd-132">Enable or disable [CSS Auto-Sync](#enable-or-disable-css-auto-sync).</span></span>

> [!NOTE]
> <span data-ttu-id="19fdd-133">Visual Studio プラグインによって、最も顕著な*Web 拡張機能パック 2015*と*Web 拡張機能パック 2017*、ブラウザー リンク拡張機能を提供して、ASP でいくつかの追加機能は動作しません。NET Core プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="19fdd-133">Some Visual Studio plug-ins, most notably *Web Extension Pack 2015* and *Web Extension Pack 2017*, offer extended functionality for Browser Link, but some of the additional features don't work with ASP.NET Core projects.</span></span>

## <a name="refresh-the-web-app-in-several-browsers-at-once"></a><span data-ttu-id="19fdd-134">一度に複数のブラウザーで web アプリを更新します。</span><span class="sxs-lookup"><span data-stu-id="19fdd-134">Refresh the web app in several browsers at once</span></span>

<span data-ttu-id="19fdd-135">プロジェクトを開始するときに起動する 1 つの web ブラウザーを選択するドロップダウン メニューを使用して、**デバッグ ターゲット**ツール バー コントロール。</span><span class="sxs-lookup"><span data-stu-id="19fdd-135">To choose a single web browser to launch when starting the project, use the drop-down menu in the **Debug Target** toolbar control:</span></span>

![F5 キーを押してドロップダウン メニュー](using-browserlink/_static/debug-target-dropdown-menu.png)

<span data-ttu-id="19fdd-137">一度に複数のブラウザーを開く、次のように選択します**を参照しています.。** 同じドロップダウン リストから。</span><span class="sxs-lookup"><span data-stu-id="19fdd-137">To open multiple browsers at once, choose **Browse with...** from the same drop-down.</span></span> <span data-ttu-id="19fdd-138">ブラウザーを選択して CTRL キーを押しながらクリックして**参照**:</span><span class="sxs-lookup"><span data-stu-id="19fdd-138">Hold down the CTRL key to select the browsers you want, and then click **Browse**:</span></span>

![一度に多くのブラウザーを開く](using-browserlink/_static/open-many-browsers-at-once.png)

<span data-ttu-id="19fdd-140">Index ビューで開いている Visual Studio を示すスクリーン ショットと 2 つの開いているブラウザーを次に示します。</span><span class="sxs-lookup"><span data-stu-id="19fdd-140">Here's a screenshot showing Visual Studio with the Index view open and two open browsers:</span></span>

![2 つのブラウザーの例との同期します。](using-browserlink/_static/sync-with-two-browsers-example.png)

<span data-ttu-id="19fdd-142">ブラウザー リンクのツール バー コントロールをプロジェクトに接続されているブラウザーを表示するポインターを合わせます。</span><span class="sxs-lookup"><span data-stu-id="19fdd-142">Hover over the Browser Link toolbar control to see the browsers that are connected to the project:</span></span>

![ポイントしたときのヒント](using-browserlink/_static/hoover-tip.png)

<span data-ttu-id="19fdd-144">インデックス ビューを変更し、ブラウザー リンクの更新 ボタンをクリックすると、接続されているすべてのブラウザーは更新されます。</span><span class="sxs-lookup"><span data-stu-id="19fdd-144">Change the Index view, and all connected browsers are updated when you click the Browser Link refresh button:</span></span>

![browsers-sync-to-changes](using-browserlink/_static/browsers-sync-to-changes.png)

<span data-ttu-id="19fdd-146">ブラウザー リンクは、アプリケーションの URL に移動して Visual Studio の外部からを起動しているブラウザーでも機能します。</span><span class="sxs-lookup"><span data-stu-id="19fdd-146">Browser Link also works with browsers that you launch from outside Visual Studio and navigate to the application URL.</span></span>

### <a name="the-browser-link-dashboard"></a><span data-ttu-id="19fdd-147">ブラウザー リンク ダッシュ ボード</span><span class="sxs-lookup"><span data-stu-id="19fdd-147">The Browser Link Dashboard</span></span>

<span data-ttu-id="19fdd-148">ブラウザー リンクのドロップダウン メニューを開いているブラウザーとの接続を管理するのには、ブラウザー リンク ダッシュ ボードを開きます。</span><span class="sxs-lookup"><span data-stu-id="19fdd-148">Open the Browser Link Dashboard from the Browser Link drop down menu to manage the connection with open browsers:</span></span>

![オープン browserslink ダッシュ ボード](using-browserlink/_static/open-browserlink-dashboard.png)

<span data-ttu-id="19fdd-150">ブラウザーが接続されていない場合を選択して非デバッグ セッションを開始することができます、*ブラウザーで表示*リンク。</span><span class="sxs-lookup"><span data-stu-id="19fdd-150">If no browser is connected, you can start a non-debugging session by selecting the *View in Browser* link:</span></span>

![browserlink-dashboard-no-connections](using-browserlink/_static/browserlink-dashboard-no-connections.png)

<span data-ttu-id="19fdd-152">それ以外の場合、接続されたブラウザーは、各ブラウザーで表示されているページへのパスと表示されます。</span><span class="sxs-lookup"><span data-stu-id="19fdd-152">Otherwise, the connected browsers are shown with the path to the page that each browser is showing:</span></span>

![browserlink-dashboard-two-connections](using-browserlink/_static/browserlink-dashboard-two-connections.png)

<span data-ttu-id="19fdd-154">必要に応じて、その 1 つのブラウザーを更新する表示されているブラウザー名をクリックすることができます。</span><span class="sxs-lookup"><span data-stu-id="19fdd-154">If you like, you can click on a listed browser name to refresh that single browser.</span></span>

### <a name="enable-or-disable-browser-link"></a><span data-ttu-id="19fdd-155">有効にするか、ブラウザー リンクを無効にします。</span><span class="sxs-lookup"><span data-stu-id="19fdd-155">Enable or disable Browser Link</span></span>

<span data-ttu-id="19fdd-156">無効にすると、ブラウザー リンクを再度有効には、それらを再接続するようにブラウザーを更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="19fdd-156">When you re-enable Browser Link after disabling it, you must refresh the browsers to reconnect them.</span></span>

### <a name="enable-or-disable-css-auto-sync"></a><span data-ttu-id="19fdd-157">有効にするか、CSS 自動同期を無効にします。</span><span class="sxs-lookup"><span data-stu-id="19fdd-157">Enable or disable CSS Auto-Sync</span></span>

<span data-ttu-id="19fdd-158">CSS 自動同期を有効にすると、CSS ファイルを変更するときに接続されているブラウザーが自動的に更新します。</span><span class="sxs-lookup"><span data-stu-id="19fdd-158">When CSS Auto-Sync is enabled, connected browsers are automatically refreshed when you make any change to CSS files.</span></span>

## <a name="how-it-works"></a><span data-ttu-id="19fdd-159">しくみ</span><span class="sxs-lookup"><span data-stu-id="19fdd-159">How it works</span></span>

<span data-ttu-id="19fdd-160">ブラウザー リンクでは、SignalR を使用して、Visual Studio とブラウザー間の通信チャネルを作成します。</span><span class="sxs-lookup"><span data-stu-id="19fdd-160">Browser Link uses SignalR to create a communication channel between Visual Studio and the browser.</span></span> <span data-ttu-id="19fdd-161">Browser Link を有効にすると、Visual Studio は、複数のクライアント (ブラウザー) が接続できる SignalR サーバーとして機能します。</span><span class="sxs-lookup"><span data-stu-id="19fdd-161">When Browser Link is enabled, Visual Studio acts as a SignalR server that multiple clients (browsers) can connect to.</span></span> <span data-ttu-id="19fdd-162">また、ブラウザー リンクは、ASP.NET 要求パイプラインでミドルウェア コンポーネントを登録します。</span><span class="sxs-lookup"><span data-stu-id="19fdd-162">Browser Link also registers a middleware component in the ASP.NET request pipeline.</span></span> <span data-ttu-id="19fdd-163">このコンポーネントは特殊な挿入`<script>`ページ要求ごとに、サーバーからの参照。</span><span class="sxs-lookup"><span data-stu-id="19fdd-163">This component injects special `<script>` references into every page request from the server.</span></span> <span data-ttu-id="19fdd-164">選択して、スクリプト参照を確認できます**ソースの表示**の末尾にスクロールし、ブラウザーで、`<body>`コンテンツをタグします。</span><span class="sxs-lookup"><span data-stu-id="19fdd-164">You can see the script references by selecting **View source** in the browser and scrolling to the end of the `<body>` tag content:</span></span>

```html
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

<span data-ttu-id="19fdd-165">ソース ファイルは変更されません。</span><span class="sxs-lookup"><span data-stu-id="19fdd-165">Your source files aren't modified.</span></span> <span data-ttu-id="19fdd-166">ミドルウェア コンポーネントは、スクリプト参照を動的に挿入します。</span><span class="sxs-lookup"><span data-stu-id="19fdd-166">The middleware component injects the script references dynamically.</span></span>

<span data-ttu-id="19fdd-167">ブラウザー側のコードは、すべての JavaScript であるために、ブラウザーのプラグインを必要とせず、SignalR がサポートされているすべてのブラウザーで動作します。</span><span class="sxs-lookup"><span data-stu-id="19fdd-167">Because the browser-side code is all JavaScript, it works on all browsers that SignalR supports without requiring a browser plug-in.</span></span>
