---
uid: visual-studio/overview/2013/using-browser-link
title: Visual Studio 2013 でブラウザー リンクの使用 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 10/04/2013
ms.assetid: 46cbfe20-b4dc-449b-9016-80657dd44fbe
msc.legacyurl: /visual-studio/overview/2013/using-browser-link
msc.type: authoredcontent
ms.openlocfilehash: 9da93c279bfa2af614733e3234ba62429abf688a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37822196"
---
<a name="using-browser-link-in-visual-studio-2013"></a><span data-ttu-id="33571-102">Visual Studio 2013 でブラウザー リンクの使用</span><span class="sxs-lookup"><span data-stu-id="33571-102">Using Browser Link in Visual Studio 2013</span></span>
====================
<span data-ttu-id="33571-103">作成者[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="33571-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="33571-104">ブラウザー リンクとは、開発環境と 1 つまたは複数の web ブラウザーの間の通信チャネルを作成する Visual Studio 2013 の新機能です。</span><span class="sxs-lookup"><span data-stu-id="33571-104">Browser Link is a new feature in Visual Studio 2013 that creates a communication channel between the development environment and one or more web browsers.</span></span> <span data-ttu-id="33571-105">クロス ブラウザー テスト用に便利です、いくつかのブラウザーで web のアプリケーションは、一度にを更新するブラウザー リンクを使用できます。</span><span class="sxs-lookup"><span data-stu-id="33571-105">You can use Browser Link to refresh your web application in several browsers at once, which is useful for cross-browser testing.</span></span>

- [<span data-ttu-id="33571-106">ブラウザーの更新</span><span class="sxs-lookup"><span data-stu-id="33571-106">Browser Refresh</span></span>](#browser-refresh)
- [<span data-ttu-id="33571-107">ブラウザー リンク ダッシュ ボードを表示します。</span><span class="sxs-lookup"><span data-stu-id="33571-107">Viewing the Browser Link Dashboard</span></span>](#dashboard)
- [<span data-ttu-id="33571-108">静的な HTML ファイルのブラウザー リンクの有効化</span><span class="sxs-lookup"><span data-stu-id="33571-108">Enabling Browser Link for Static HTML Files</span></span>](#static-html)
- [<span data-ttu-id="33571-109">ブラウザー リンクを無効にします。</span><span class="sxs-lookup"><span data-stu-id="33571-109">Disabling Browser Link</span></span>](#disabling)
- [<span data-ttu-id="33571-110">作業のでしょうか。</span><span class="sxs-lookup"><span data-stu-id="33571-110">How Does It Work?</span></span>](#how-it-works)

<a id="browser-refresh"></a>
## <a name="browser-refresh"></a><span data-ttu-id="33571-111">ブラウザーの更新</span><span class="sxs-lookup"><span data-stu-id="33571-111">Browser Refresh</span></span>

<span data-ttu-id="33571-112">ブラウザーを更新するには、ブラウザー リンクを介して Visual Studio に接続されている複数のブラウザーを更新できます。</span><span class="sxs-lookup"><span data-stu-id="33571-112">With Browser Refresh, you can refresh multiple browsers that are connected to Visual Studio through Browser Link.</span></span>

<span data-ttu-id="33571-113">ブラウザーの更新を使用するには、最初のプロジェクト テンプレートのいずれかを使用して、ASP.NET アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="33571-113">To use Browser Refresh, first create an ASP.NET application, using any of the project templates.</span></span> <span data-ttu-id="33571-114">F5 キーを押すか、ツールバーの矢印アイコンをクリックしてアプリケーションをデバッグします。</span><span class="sxs-lookup"><span data-stu-id="33571-114">Debug the application by pressing F5 or clicking the arrow icon in the toolbar:</span></span>

![](using-browser-link/_static/image1.png)

<span data-ttu-id="33571-115">デバッグ用の特定のブラウザーを選択するのにドロップダウン リストを使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="33571-115">You can also use the dropdown to select a specific browser for debugging.</span></span>

![](using-browser-link/_static/image2.png)

<span data-ttu-id="33571-116">複数のブラウザーをデバッグするには、選択**ブラウザー**します。</span><span class="sxs-lookup"><span data-stu-id="33571-116">To debug with multiple browsers, select **Browse With**.</span></span> <span data-ttu-id="33571-117">**ブラウザー**ダイアログ ボックスで、キーを押し、CTRL キーを 1 つ以上のブラウザーを選択します。</span><span class="sxs-lookup"><span data-stu-id="33571-117">In the **Browse With** dialog, hold down the CTRL key to select more than one browser.</span></span> <span data-ttu-id="33571-118">クリックして**参照**で選択されているブラウザーをデバッグします。</span><span class="sxs-lookup"><span data-stu-id="33571-118">Click **Browse** to debug with the selected browsers.</span></span> <span data-ttu-id="33571-119">ブラウザー リンクは、Visual Studio の外部からブラウザーを起動し、アプリケーションの URL に移動する場合にも機能します。</span><span class="sxs-lookup"><span data-stu-id="33571-119">Browser Link also works if you launch a browser from outside Visual Studio and navigate to the application URL.</span></span>

![](using-browser-link/_static/image3.png)

<span data-ttu-id="33571-120">ブラウザー リンク コントロールは、円形の矢印アイコンを含むドロップダウンに配置されます。</span><span class="sxs-lookup"><span data-stu-id="33571-120">The Browser Link controls are located in the dropdown with the circular arrow icon.</span></span> <span data-ttu-id="33571-121">矢印アイコンが、**更新**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="33571-121">The arrow icon is the **Refresh** button.</span></span>

![](using-browser-link/_static/image4.png)

<span data-ttu-id="33571-122">どのブラウザーが接続されているを表示するには、マウス ポインターを置き、**更新**デバッグ中にボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="33571-122">To see which browsers are connected, hover the mouse over the **Refresh** button while debugging.</span></span> <span data-ttu-id="33571-123">ツールヒント ウィンドウは、接続されているブラウザーに表示されます。</span><span class="sxs-lookup"><span data-stu-id="33571-123">The connected browsers are shown in a ToolTip window.</span></span>

![](using-browser-link/_static/image5.png)

<span data-ttu-id="33571-124">接続されているブラウザーを更新する をクリックして、**更新**ボタンまたは CTRL + ALT + ENTER キーを押します。</span><span class="sxs-lookup"><span data-stu-id="33571-124">To refresh the connected browsers, click the **Refresh** button or press CTRL+ALT+ENTER.</span></span> <span data-ttu-id="33571-125">たとえば、次のスクリーン ショットでは、MVC 5 プロジェクト テンプレートを使用して作成した ASP.NET プロジェクトを示しています。</span><span class="sxs-lookup"><span data-stu-id="33571-125">For example, the following screenshot shows an ASP.NET project, which I created using the MVC 5 project template.</span></span> <span data-ttu-id="33571-126">上部にある 2 つのブラウザーで実行されているアプリケーションを確認できます。</span><span class="sxs-lookup"><span data-stu-id="33571-126">You can see the application running in two browsers at the top.</span></span> <span data-ttu-id="33571-127">下部で、プロジェクトは Visual Studio で開いています。</span><span class="sxs-lookup"><span data-stu-id="33571-127">At the bottom, the project is open in Visual Studio.</span></span>

![](using-browser-link/_static/image6.png)

<span data-ttu-id="33571-128">Visual Studio で、変更、 &lt;h1&gt;のホーム ページの見出し。</span><span class="sxs-lookup"><span data-stu-id="33571-128">In Visual Studio, I changed the &lt;h1&gt; heading for the home page:</span></span>

![](using-browser-link/_static/image7.png)

<span data-ttu-id="33571-129">クリックしたときに、**更新** ボタン、両方のブラウザー ウィンドウに表示されていた変更。</span><span class="sxs-lookup"><span data-stu-id="33571-129">When I clicked the **Refresh** button, the change appeared in both browser windows:</span></span>

![](using-browser-link/_static/image8.png)

<span data-ttu-id="33571-130">**ノート**</span><span class="sxs-lookup"><span data-stu-id="33571-130">**Notes**</span></span>

- <span data-ttu-id="33571-131">Browser Link を有効にするには設定`debug=true`で、 [&lt;コンパイル&gt;](https://msdn.microsoft.com/library/s10awwz0(v=vs.85).aspx)プロジェクトの Web.config ファイル内の要素。</span><span class="sxs-lookup"><span data-stu-id="33571-131">To enable Browser Link, set `debug=true` in the [&lt;compilation&gt;](https://msdn.microsoft.com/library/s10awwz0(v=vs.85).aspx) element in the project's Web.config file.</span></span>
- <span data-ttu-id="33571-132">アプリケーションは、localhost 上で実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="33571-132">The application must be running on localhost.</span></span>
- <span data-ttu-id="33571-133">アプリケーションでは、.NET 4.0 以降をターゲットする必要があります。</span><span class="sxs-lookup"><span data-stu-id="33571-133">The application must target .NET 4.0 or later.</span></span>

<a id="dashboard"></a>
## <a name="viewing-the-browser-link-dashboard"></a><span data-ttu-id="33571-134">ブラウザー リンク ダッシュ ボードを表示します。</span><span class="sxs-lookup"><span data-stu-id="33571-134">Viewing the Browser Link Dashboard</span></span>

<span data-ttu-id="33571-135">ブラウザー リンク ダッシュ ボードには、ブラウザー リンク接続に関する情報が表示されます。</span><span class="sxs-lookup"><span data-stu-id="33571-135">The Browser Link dashboard shows information about the Browser Link connections.</span></span> <span data-ttu-id="33571-136">ダッシュ ボードを表示するには、ブラウザー リンクのドロップダウン メニューを選択します。 (横にある小さい矢印、**更新**ボタン)。</span><span class="sxs-lookup"><span data-stu-id="33571-136">To view the dashboard, select the Browser Link dropdown menu (the small arrow next to the **Refresh** button).</span></span> <span data-ttu-id="33571-137">クリックして**ブラウザー リンク ダッシュ ボード**します。</span><span class="sxs-lookup"><span data-stu-id="33571-137">Then click **Browser Link Dashboard**.</span></span>

![](using-browser-link/_static/image9.png)

<span data-ttu-id="33571-138">ダッシュ ボードには、接続されているブラウザーと各ブラウザーが移動先 URL が一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="33571-138">The dashboard lists the connected Browsers and the URL to which each browser has navigated.</span></span>

![](using-browser-link/_static/image10.png)

<span data-ttu-id="33571-139">**の前提条件**でそのプロジェクトに対してブラウザー リンクを有効にするために必要なすべての手順を説明します。</span><span class="sxs-lookup"><span data-stu-id="33571-139">The **Prerequisites** section shows any steps needed to enable Browser Link for that project.</span></span> <span data-ttu-id="33571-140">たとえば、次のスクリーン ショット、プロジェクトで Web.config ファイルで false に設定が"debug"。</span><span class="sxs-lookup"><span data-stu-id="33571-140">For example, the following screenshot shows a project where "debug" is set to false in the Web.config file.</span></span>

![](using-browser-link/_static/image11.png)

<a id="static-html"></a>
## <a name="enabling-browser-link-for-static-html-files"></a><span data-ttu-id="33571-141">静的な HTML ファイルのブラウザー リンクの有効化</span><span class="sxs-lookup"><span data-stu-id="33571-141">Enabling Browser Link for Static HTML Files</span></span>

<span data-ttu-id="33571-142">静的な HTML ファイルで Browser Link を有効にするには、Web.config ファイルに、次を追加します。</span><span class="sxs-lookup"><span data-stu-id="33571-142">To enable Browser Link for static HTML files, add the following to your Web.config file.</span></span>

[!code-xml[Main](using-browser-link/samples/sample1.xml)]

<span data-ttu-id="33571-143">パフォーマンス向上のためには、プロジェクトを発行するときに、この設定を削除します。</span><span class="sxs-lookup"><span data-stu-id="33571-143">For performance reasons, remove this setting when you publish your project.</span></span>

<a id="disabling"></a>
## <a name="disabling-browser-link"></a><span data-ttu-id="33571-144">ブラウザー リンクを無効にします。</span><span class="sxs-lookup"><span data-stu-id="33571-144">Disabling Browser Link</span></span>

<span data-ttu-id="33571-145">ブラウザー リンクは、既定で有効です。</span><span class="sxs-lookup"><span data-stu-id="33571-145">Browser Link is enabled by default.</span></span> <span data-ttu-id="33571-146">無効にするいくつかの方法はあります。</span><span class="sxs-lookup"><span data-stu-id="33571-146">There are several ways to disable it:</span></span>

- <span data-ttu-id="33571-147">ブラウザー リンクのドロップダウン メニューで、オフに**Browser Link を有効にする**します。</span><span class="sxs-lookup"><span data-stu-id="33571-147">In the Browser Link dropdown menu, uncheck **Enable Browser Link**.</span></span> 

    ![](using-browser-link/_static/image12.png)
- <span data-ttu-id="33571-148">Web.config ファイルでは、"vs: EnableBrowserLink"値"false"に appSettings セクションでという名前のキーを追加します。</span><span class="sxs-lookup"><span data-stu-id="33571-148">In the Web.config file, add a key named "vs:EnableBrowserLink" with the value "false" in the appSettings section.</span></span> 

    [!code-xml[Main](using-browser-link/samples/sample2.xml)]
- <span data-ttu-id="33571-149">Web.config ファイルでデバッグを false に設定します。</span><span class="sxs-lookup"><span data-stu-id="33571-149">In the Web.config file, set debug to false.</span></span> 

    [!code-xml[Main](using-browser-link/samples/sample3.xml)]

<a id="how-it-works"></a>
## <a name="how-does-it-work"></a><span data-ttu-id="33571-150">作業のでしょうか。</span><span class="sxs-lookup"><span data-stu-id="33571-150">How Does It Work?</span></span>

<span data-ttu-id="33571-151">ブラウザー リンクを使用して[SignalR](../../../signalr/index.md) Visual Studio とブラウザー間の通信チャネルを作成します。</span><span class="sxs-lookup"><span data-stu-id="33571-151">Browser Link uses [SignalR](../../../signalr/index.md) to create a communication channel between Visual Studio and the browser.</span></span> <span data-ttu-id="33571-152">Browser Link を有効にすると、Visual Studio は、複数のクライアント (ブラウザー) が接続できる SignalR サーバーとして機能します。</span><span class="sxs-lookup"><span data-stu-id="33571-152">When Browser Link is enabled, Visual Studio acts as a SignalR server that multiple clients (browsers) can connect to.</span></span> <span data-ttu-id="33571-153">また、ブラウザー リンクは、ASP.NET を使用した HTTP モジュールを登録します。</span><span class="sxs-lookup"><span data-stu-id="33571-153">Browser Link also registers an HTTP module with ASP.NET.</span></span> <span data-ttu-id="33571-154">このモジュールは特殊な挿入&lt;スクリプト&gt;ページ要求ごとに、サーバーからの参照。</span><span class="sxs-lookup"><span data-stu-id="33571-154">This module injects special &lt;script&gt; references into every page request from the server.</span></span> <span data-ttu-id="33571-155">ブラウザーでソースの表示 を選択して、スクリプト参照を確認できます。</span><span class="sxs-lookup"><span data-stu-id="33571-155">You can see the script references by selecting "View source" in the browser.</span></span>

![](using-browser-link/_static/image13.png)

<span data-ttu-id="33571-156">ソース ファイルは変更されません。</span><span class="sxs-lookup"><span data-stu-id="33571-156">Your source files are not modified.</span></span> <span data-ttu-id="33571-157">HTTP モジュールは、スクリプト参照を動的に挿入します。</span><span class="sxs-lookup"><span data-stu-id="33571-157">The HTTP module injects the script references dynamically.</span></span>

<span data-ttu-id="33571-158">ブラウザー側のコードは、すべての JavaScript であるために、すべてのブラウザーで動作する[SignalR をサポートしています](../../../signalr/overview/getting-started/supported-platforms.md)、任意のブラウザー プラグインを必要とせずします。</span><span class="sxs-lookup"><span data-stu-id="33571-158">Because the browser-side code is all JavaScript, it works on all browsers that [SignalR supports](../../../signalr/overview/getting-started/supported-platforms.md), without requiring any browser plug-in.</span></span>
