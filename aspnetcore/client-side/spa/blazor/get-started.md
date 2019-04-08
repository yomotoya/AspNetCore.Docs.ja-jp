---
title: Blazor を概要します。
author: guardrex
description: 作成と Blazor プロジェクトを変更して Blazor を開始する方法について説明します。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: spa/blazor/get-started
ms.openlocfilehash: b3928c2812be6f34cdf2f17295a1251106f651e5
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068236"
---
# <a name="get-started-with-blazor"></a><span data-ttu-id="5a1d3-103">Blazor を概要します。</span><span class="sxs-lookup"><span data-stu-id="5a1d3-103">Get started with Blazor</span></span>

<span data-ttu-id="5a1d3-104">作成者: [Daniel Roth](https://github.com/danroth27)、[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="5a1d3-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

# [<a name="visual-studio"></a><span data-ttu-id="5a1d3-105">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5a1d3-105">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="5a1d3-106">必要条件:</span><span class="sxs-lookup"><span data-stu-id="5a1d3-106">Prerequisites:</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

<span data-ttu-id="5a1d3-107">Visual Studio で、最初の Blazor プロジェクトを作成するには</span><span class="sxs-lookup"><span data-stu-id="5a1d3-107">To create your first Blazor project in Visual Studio:</span></span>

1. <span data-ttu-id="5a1d3-108">最新のインストール[.NET Core 3.0 プレビュー SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0)リリースします。</span><span class="sxs-lookup"><span data-stu-id="5a1d3-108">Install the latest [.NET Core 3.0 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.</span></span>
1. <span data-ttu-id="5a1d3-109">Visual Studio プレビュー Sdk の使用を有効にします。</span><span class="sxs-lookup"><span data-stu-id="5a1d3-109">Enable Visual Studio to use preview SDKs:</span></span>
   1. <span data-ttu-id="5a1d3-110">開いている**ツール** > **オプション**メニュー バーにします。</span><span class="sxs-lookup"><span data-stu-id="5a1d3-110">Open **Tools** > **Options** in the menu bar.</span></span>
   1. <span data-ttu-id="5a1d3-111">開く、**プロジェクトおよびソリューション**ノード。</span><span class="sxs-lookup"><span data-stu-id="5a1d3-111">Open the **Projects and Solutions** node.</span></span> <span data-ttu-id="5a1d3-112">開く、 **.NET Core**タブ。</span><span class="sxs-lookup"><span data-stu-id="5a1d3-112">Open the **.NET Core** tab.</span></span>
   1. <span data-ttu-id="5a1d3-113">チェック ボックスをオン **、.NET Core SDK のプレビューを使用して、** します。</span><span class="sxs-lookup"><span data-stu-id="5a1d3-113">Check the box for **Use previews of the .NET Core SDK**.</span></span> <span data-ttu-id="5a1d3-114">**[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="5a1d3-114">Select **OK**.</span></span>
1. <span data-ttu-id="5a1d3-115">最新のインストール[Blazor 拡張子](https://go.microsoft.com/fwlink/?linkid=870389)Visual Studio Marketplace から。</span><span class="sxs-lookup"><span data-stu-id="5a1d3-115">Install the latest [Blazor extension](https://go.microsoft.com/fwlink/?linkid=870389) from the Visual Studio Marketplace.</span></span> <span data-ttu-id="5a1d3-116">この手順では、Visual Studio を使用可能な Blazor テンプレートをによりします。</span><span class="sxs-lookup"><span data-stu-id="5a1d3-116">This step makes Blazor templates available to Visual Studio.</span></span>
1. <span data-ttu-id="5a1d3-117">コマンド シェルで次のコマンドを実行して Blazor テンプレートを .NET Core CLI で使用できるようにするには。</span><span class="sxs-lookup"><span data-stu-id="5a1d3-117">Make the Blazor templates available for use with the .NET Core CLI by running the following command in a command shell:</span></span>

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::0.9.0-preview3-19154-02
   ```
1. <span data-ttu-id="5a1d3-118">新しいプロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="5a1d3-118">Create a new project.</span></span>
1. <span data-ttu-id="5a1d3-119">**[ASP.NET Core Web アプリケーション]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="5a1d3-119">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="5a1d3-120">**[次へ]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="5a1d3-120">Select **Next**.</span></span>
1. <span data-ttu-id="5a1d3-121">名前を入力、**プロジェクト名**フィールド。</span><span class="sxs-lookup"><span data-stu-id="5a1d3-121">Provide a name in the **Project name** field.</span></span> <span data-ttu-id="5a1d3-122">確認、**場所**エントリが正しいか、プロジェクトの場所を指定します。</span><span class="sxs-lookup"><span data-stu-id="5a1d3-122">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="5a1d3-123">**[作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="5a1d3-123">Select **Create**.</span></span>
1. <span data-ttu-id="5a1d3-124">確認します **.NET Core**と**ASP.NET Core 3.0**上部にある選択されます。</span><span class="sxs-lookup"><span data-stu-id="5a1d3-124">Make sure **.NET Core** and **ASP.NET Core 3.0** are selected at the top.</span></span>
1. <span data-ttu-id="5a1d3-125">選択、 **Blazor**テンプレートと選択**作成**です。</span><span class="sxs-lookup"><span data-stu-id="5a1d3-125">Select the **Blazor** template and select **Create**.</span></span>
1. <span data-ttu-id="5a1d3-126">**F5 キー**を押してアプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="5a1d3-126">Press **F5** to run the app.</span></span>

<span data-ttu-id="5a1d3-127">おめでとうございます!</span><span class="sxs-lookup"><span data-stu-id="5a1d3-127">Congratulations!</span></span> <span data-ttu-id="5a1d3-128">最初の Blazor アプリを実行しました。</span><span class="sxs-lookup"><span data-stu-id="5a1d3-128">You just ran your first Blazor app!</span></span>

<!--

# [Visual Studio Code](#tab/visual-studio-code)

Prerequisites:

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

To create your first Blazor project in Visual Studio Code:

1. Execute the following command in a command shell:

   ```console
   dotnet new blazor -o WebApplication1
   ```

1. Open the *WebApplication1* folder in Visual Studio Code.

1. Visual Studio code offers to create assets to build and debug the app, which includes the *tasks.json* and *launch.json* files. Select **Yes** to add the assets.

1. Execute the app using the Visual Studio Code debugger.

1. In a browser, navigate to `https://localhost:5001`.

Congratulations! You just ran your first Blazor app!

# [Visual Studio for Mac](#tab/visual-studio-mac)

.NET Core 3.0 will be supported with Visual Studio for Mac version 8.0 or later. Visual Studio for Mac version 8.0 Preview isn't available at this time.

Use the [.NET Core CLI version of this topic](xref:razor-components/get-started?tabs=netcore-cli) on macOS.

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

To create your first project Blazor project in Visual Studio for Mac:

1. Select **File** > **New Solution** or **New Project**.
1. In the sidebar, select **.NET Core** > **App**.
1. Select **Blazor** and select **Next**.
1. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.
1. In the **Project Name** field, enter `WebApplication1`. Select **Create**.
1. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

Congratulations! You just ran your first Blazor app!
-->

# [<a name="net-core-cli"></a><span data-ttu-id="5a1d3-129">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="5a1d3-129">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="5a1d3-130">必要条件:</span><span class="sxs-lookup"><span data-stu-id="5a1d3-130">Prerequisites:</span></span>

* [<span data-ttu-id="5a1d3-131">.NET core SDK 3.0 プレビュー</span><span class="sxs-lookup"><span data-stu-id="5a1d3-131">.NET Core SDK 3.0 Preview</span></span>](https://dotnet.microsoft.com/download/dotnet-core/3.0)

1. <span data-ttu-id="5a1d3-132">コマンド シェルで次のコマンドを実行して Blazor テンプレートを追加します。</span><span class="sxs-lookup"><span data-stu-id="5a1d3-132">Add the Blazor templates by running the following command in a command shell:</span></span>

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::0.9.0-preview3-19154-02
   ```

1. <span data-ttu-id="5a1d3-133">コマンド シェルで、最初の Blazor プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="5a1d3-133">Create your first Blazor project in a command shell:</span></span>

   ```console
   dotnet new blazor -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

1. <span data-ttu-id="5a1d3-134">ブラウザーで、`https://localhost:5001` に移動します。</span><span class="sxs-lookup"><span data-stu-id="5a1d3-134">In a browser, navigate to `https://localhost:5001`.</span></span>

<span data-ttu-id="5a1d3-135">おめでとうございます!</span><span class="sxs-lookup"><span data-stu-id="5a1d3-135">Congratulations!</span></span> <span data-ttu-id="5a1d3-136">最初の Blazor アプリを実行しました。</span><span class="sxs-lookup"><span data-stu-id="5a1d3-136">You just ran your first Blazor app!</span></span>

---

## <a name="blazor-project"></a><span data-ttu-id="5a1d3-137">Blazor プロジェクト</span><span class="sxs-lookup"><span data-stu-id="5a1d3-137">Blazor project</span></span>

<span data-ttu-id="5a1d3-138">アプリを実行すると、複数のページは、サイドバー内のタブから利用できます。</span><span class="sxs-lookup"><span data-stu-id="5a1d3-138">When the app is run, multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="5a1d3-139">Home</span><span class="sxs-lookup"><span data-stu-id="5a1d3-139">Home</span></span>
* <span data-ttu-id="5a1d3-140">カウンター</span><span class="sxs-lookup"><span data-stu-id="5a1d3-140">Counter</span></span>
* <span data-ttu-id="5a1d3-141">データをフェッチします。</span><span class="sxs-lookup"><span data-stu-id="5a1d3-141">Fetch data</span></span>

<span data-ttu-id="5a1d3-142">Counter ページ上で **[クリックしてください]** ボタンを選択し、ページを更新することなくカウンターをインクリメントします。</span><span class="sxs-lookup"><span data-stu-id="5a1d3-142">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="5a1d3-143">通常、web ページにカウンターをインクリメントする場合は、JavaScript を記述する必要がありますが、Blazor は、使用してより優れたアプローチC#します。</span><span class="sxs-lookup"><span data-stu-id="5a1d3-143">Incrementing a counter in a webpage normally requires writing JavaScript, but Blazor provides a better approach using C#.</span></span>

<span data-ttu-id="5a1d3-144">*Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="5a1d3-144">*Pages/Counter.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.cshtml)]

<span data-ttu-id="5a1d3-145">要求を`/counter`で指定したとおり、ブラウザーで、`@page`上部にあるディレクティブがそのコンテンツを表示するために、カウンターのコンポーネント。</span><span class="sxs-lookup"><span data-stu-id="5a1d3-145">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the Counter component to render its content.</span></span> <span data-ttu-id="5a1d3-146">コンポーネントは、柔軟で効率的な方法で UI を更新するために使用するレンダリング ツリーのメモリ内表現にレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="5a1d3-146">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="5a1d3-147">毎回、 **Click me**ボタンが選択されています。</span><span class="sxs-lookup"><span data-stu-id="5a1d3-147">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="5a1d3-148">`onclick`イベントが発生します。</span><span class="sxs-lookup"><span data-stu-id="5a1d3-148">The `onclick` event is fired.</span></span>
* <span data-ttu-id="5a1d3-149">`IncrementCount` メソッドが呼び出された場合。</span><span class="sxs-lookup"><span data-stu-id="5a1d3-149">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="5a1d3-150">`currentCount`がインクリメントされます。</span><span class="sxs-lookup"><span data-stu-id="5a1d3-150">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="5a1d3-151">コンポーネントは、もう一度表示されます。</span><span class="sxs-lookup"><span data-stu-id="5a1d3-151">The component is rendered again.</span></span>

<span data-ttu-id="5a1d3-152">ランタイムは、前のコンテンツを新しいコンテンツを比較し、ドキュメント オブジェクト モデル (DOM) に変更されたコンテンツにのみ適用されます。</span><span class="sxs-lookup"><span data-stu-id="5a1d3-152">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="5a1d3-153">HTML のような構文を使用して別のコンポーネントにコンポーネントを追加します。</span><span class="sxs-lookup"><span data-stu-id="5a1d3-153">Add a component to another component using an HTML-like syntax.</span></span> <span data-ttu-id="5a1d3-154">コンポーネントのパラメーターは、属性または子コンテンツを使用して指定されます。</span><span class="sxs-lookup"><span data-stu-id="5a1d3-154">Component parameters are specified using attributes or child content.</span></span> <span data-ttu-id="5a1d3-155">追加することでカウンター コンポーネントをアプリのホーム ページに追加するなど、`<Counter />`インデックス コンポーネント要素。</span><span class="sxs-lookup"><span data-stu-id="5a1d3-155">For example, a Counter component can be added to the app's homepage by adding a `<Counter />` element to the Index component.</span></span>

<span data-ttu-id="5a1d3-156">*Pages/Index.cshtml*アンケートのプロンプトのコンポーネントをカウンター コンポーネントに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="5a1d3-156">In *Pages/Index.cshtml*, replace the Survey Prompt component with a Counter component:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.cshtml?highlight=7)]

<span data-ttu-id="5a1d3-157">アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="5a1d3-157">Run the app.</span></span> <span data-ttu-id="5a1d3-158">ホームページには、独自のカウンターを指定します。</span><span class="sxs-lookup"><span data-stu-id="5a1d3-158">The homepage has its own counter.</span></span>

<span data-ttu-id="5a1d3-159">カウンターのコンポーネントにパラメーターを追加する更新コンポーネントの`@functions`ブロック。</span><span class="sxs-lookup"><span data-stu-id="5a1d3-159">To add a parameter to the Counter component, update the component's `@functions` block:</span></span>

* <span data-ttu-id="5a1d3-160">プロパティを追加`IncrementAmount`で修飾された、`[Parameter]`属性。</span><span class="sxs-lookup"><span data-stu-id="5a1d3-160">Add a property for `IncrementAmount` decorated with the `[Parameter]` attribute.</span></span>
* <span data-ttu-id="5a1d3-161">`currentCount` の値を増やすときに `IncrementAmount` を使うように `IncrementCount` メソッドを変更します。</span><span class="sxs-lookup"><span data-stu-id="5a1d3-161">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="5a1d3-162">*Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="5a1d3-162">*Pages/Counter.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.cshtml?highlight=4,8)]

<span data-ttu-id="5a1d3-163">属性を使って、Home コンポーネントの `<Counter>` 要素に `IncrementAmount` パラメーターを指定します。</span><span class="sxs-lookup"><span data-stu-id="5a1d3-163">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="5a1d3-164">*Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="5a1d3-164">*Pages/Index.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.cshtml)]

<span data-ttu-id="5a1d3-165">アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="5a1d3-165">Run the app.</span></span> <span data-ttu-id="5a1d3-166">ホームページには、10 たびにインクリメントする独自のカウンター、 **Click me**ボタンが選択されています。</span><span class="sxs-lookup"><span data-stu-id="5a1d3-166">The homepage has its own counter that increments by ten each time the **Click me** button is selected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5a1d3-167">次の手順</span><span class="sxs-lookup"><span data-stu-id="5a1d3-167">Next steps</span></span>

<xref:tutorials/first-razor-components-app>
