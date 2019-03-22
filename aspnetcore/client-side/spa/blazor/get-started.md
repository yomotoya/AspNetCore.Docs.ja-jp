---
title: Blazor を概要します。
author: guardrex
description: 作成と Blazor プロジェクトを変更して Blazor を開始する方法について説明します。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2019
uid: spa/blazor/get-started
ms.openlocfilehash: f46bd9af0f0762e794349d4e98de5c086a690d72
ms.sourcegitcommit: a1c43150ed46aa01572399e8aede50d4668745ca
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/21/2019
ms.locfileid: "58327230"
---
# <a name="get-started-with-blazor"></a><span data-ttu-id="60c86-103">Blazor を概要します。</span><span class="sxs-lookup"><span data-stu-id="60c86-103">Get started with Blazor</span></span>

<span data-ttu-id="60c86-104">作成者: [Daniel Roth](https://github.com/danroth27)、[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="60c86-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="60c86-105">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="60c86-105">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="60c86-106">必要条件:</span><span class="sxs-lookup"><span data-stu-id="60c86-106">Prerequisites:</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

<span data-ttu-id="60c86-107">Visual Studio で、最初の Blazor プロジェクトを作成するには</span><span class="sxs-lookup"><span data-stu-id="60c86-107">To create your first Blazor project in Visual Studio:</span></span>

1. <span data-ttu-id="60c86-108">最新のインストール[Blazor 拡張子](https://go.microsoft.com/fwlink/?linkid=870389)Visual Studio Marketplace から。</span><span class="sxs-lookup"><span data-stu-id="60c86-108">Install the latest [Blazor extension](https://go.microsoft.com/fwlink/?linkid=870389) from the Visual Studio Marketplace.</span></span> <span data-ttu-id="60c86-109">この手順では、Visual Studio を使用可能な Blazor テンプレートをによりします。</span><span class="sxs-lookup"><span data-stu-id="60c86-109">This step makes Blazor templates available to Visual Studio.</span></span>
1. <span data-ttu-id="60c86-110">コマンド シェルで次のコマンドを実行して Blazor テンプレートを .NET Core CLI で使用できるようにするには。</span><span class="sxs-lookup"><span data-stu-id="60c86-110">Make the Blazor templates available for use with the .NET Core CLI by running the following command in a command shell:</span></span>

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::0.9.0-preview3-19154-02
   ```

1. <span data-ttu-id="60c86-111">選択**ファイル** > **新しいプロジェクト** > **Web** > **ASP.NET Core Web アプリケーション**します。</span><span class="sxs-lookup"><span data-stu-id="60c86-111">Select **File** > **New Project** > **Web** > **ASP.NET Core Web Application**.</span></span>
1. <span data-ttu-id="60c86-112">確認します **.NET Core**と**ASP.NET Core 3.0**上部にある選択されます。</span><span class="sxs-lookup"><span data-stu-id="60c86-112">Make sure **.NET Core** and **ASP.NET Core 3.0** are selected at the top.</span></span>
1. <span data-ttu-id="60c86-113">**[Blazor]** テンプレートを選択して **[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="60c86-113">Choose the **Blazor** template and select **OK**.</span></span>
1. <span data-ttu-id="60c86-114">**F5 キー**を押してアプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="60c86-114">Press **F5** to run the app.</span></span>

<span data-ttu-id="60c86-115">おめでとうございます!</span><span class="sxs-lookup"><span data-stu-id="60c86-115">Congratulations!</span></span> <span data-ttu-id="60c86-116">最初の Blazor アプリを実行しました。</span><span class="sxs-lookup"><span data-stu-id="60c86-116">You just ran your first Blazor app!</span></span>

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

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="60c86-117">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="60c86-117">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="60c86-118">必要条件:</span><span class="sxs-lookup"><span data-stu-id="60c86-118">Prerequisites:</span></span>

* [<span data-ttu-id="60c86-119">.NET core SDK 3.0 プレビュー</span><span class="sxs-lookup"><span data-stu-id="60c86-119">.NET Core SDK 3.0 Preview</span></span>](https://dotnet.microsoft.com/download/dotnet-core/3.0)

1. <span data-ttu-id="60c86-120">コマンド シェルで次のコマンドを実行して Blazor テンプレートを追加します。</span><span class="sxs-lookup"><span data-stu-id="60c86-120">Add the Blazor templates by running the following command in a command shell:</span></span>

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::0.9.0-preview3-19154-02
   ```

1. <span data-ttu-id="60c86-121">コマンド シェルで、最初の Blazor プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="60c86-121">Create your first Blazor project in a command shell:</span></span>

   ```console
   dotnet new blazor -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

1. <span data-ttu-id="60c86-122">ブラウザーで、`https://localhost:5001` に移動します。</span><span class="sxs-lookup"><span data-stu-id="60c86-122">In a browser, navigate to `https://localhost:5001`.</span></span>

<span data-ttu-id="60c86-123">おめでとうございます!</span><span class="sxs-lookup"><span data-stu-id="60c86-123">Congratulations!</span></span> <span data-ttu-id="60c86-124">最初の Blazor アプリを実行しました。</span><span class="sxs-lookup"><span data-stu-id="60c86-124">You just ran your first Blazor app!</span></span>

---

## <a name="blazor-project"></a><span data-ttu-id="60c86-125">Blazor プロジェクト</span><span class="sxs-lookup"><span data-stu-id="60c86-125">Blazor project</span></span>

<span data-ttu-id="60c86-126">アプリを実行すると、複数のページは、サイドバー内のタブから利用できます。</span><span class="sxs-lookup"><span data-stu-id="60c86-126">When the app is run, multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="60c86-127">Home</span><span class="sxs-lookup"><span data-stu-id="60c86-127">Home</span></span>
* <span data-ttu-id="60c86-128">カウンター</span><span class="sxs-lookup"><span data-stu-id="60c86-128">Counter</span></span>
* <span data-ttu-id="60c86-129">データをフェッチします。</span><span class="sxs-lookup"><span data-stu-id="60c86-129">Fetch data</span></span>

<span data-ttu-id="60c86-130">Counter ページ上で **[クリックしてください]** ボタンを選択し、ページを更新することなくカウンターをインクリメントします。</span><span class="sxs-lookup"><span data-stu-id="60c86-130">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="60c86-131">通常、web ページにカウンターをインクリメントする場合は、JavaScript を記述する必要がありますが、Blazor は、使用してより優れたアプローチC#します。</span><span class="sxs-lookup"><span data-stu-id="60c86-131">Incrementing a counter in a webpage normally requires writing JavaScript, but Blazor provides a better approach using C#.</span></span>

<span data-ttu-id="60c86-132">*Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="60c86-132">*Pages/Counter.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.cshtml)]

<span data-ttu-id="60c86-133">要求を`/counter`で指定したとおり、ブラウザーで、`@page`上部にあるディレクティブがそのコンテンツを表示するために、カウンターのコンポーネント。</span><span class="sxs-lookup"><span data-stu-id="60c86-133">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the Counter component to render its content.</span></span> <span data-ttu-id="60c86-134">コンポーネントは、柔軟で効率的な方法で UI を更新するために使用するレンダリング ツリーのメモリ内表現にレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="60c86-134">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="60c86-135">毎回、 **Click me**ボタンが選択されています。</span><span class="sxs-lookup"><span data-stu-id="60c86-135">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="60c86-136">`onclick`イベントが発生します。</span><span class="sxs-lookup"><span data-stu-id="60c86-136">The `onclick` event is fired.</span></span>
* <span data-ttu-id="60c86-137">`IncrementCount` メソッドが呼び出された場合。</span><span class="sxs-lookup"><span data-stu-id="60c86-137">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="60c86-138">`currentCount`がインクリメントされます。</span><span class="sxs-lookup"><span data-stu-id="60c86-138">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="60c86-139">コンポーネントは、もう一度表示されます。</span><span class="sxs-lookup"><span data-stu-id="60c86-139">The component is rendered again.</span></span>

<span data-ttu-id="60c86-140">ランタイムは、前のコンテンツを新しいコンテンツを比較し、ドキュメント オブジェクト モデル (DOM) に変更されたコンテンツにのみ適用されます。</span><span class="sxs-lookup"><span data-stu-id="60c86-140">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="60c86-141">HTML のような構文を使用して別のコンポーネントにコンポーネントを追加します。</span><span class="sxs-lookup"><span data-stu-id="60c86-141">Add a component to another component using an HTML-like syntax.</span></span> <span data-ttu-id="60c86-142">コンポーネントのパラメーターは、属性または子コンテンツを使用して指定されます。</span><span class="sxs-lookup"><span data-stu-id="60c86-142">Component parameters are specified using attributes or child content.</span></span> <span data-ttu-id="60c86-143">追加することでカウンター コンポーネントをアプリのホーム ページに追加するなど、`<Counter />`インデックス コンポーネント要素。</span><span class="sxs-lookup"><span data-stu-id="60c86-143">For example, a Counter component can be added to the app's homepage by adding a `<Counter />` element to the Index component.</span></span>

<span data-ttu-id="60c86-144">*Pages/Index.cshtml*アンケートのプロンプトのコンポーネントをカウンター コンポーネントに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="60c86-144">In *Pages/Index.cshtml*, replace the Survey Prompt component with a Counter component:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.cshtml?highlight=7)]

<span data-ttu-id="60c86-145">アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="60c86-145">Run the app.</span></span> <span data-ttu-id="60c86-146">ホームページには、独自のカウンターを指定します。</span><span class="sxs-lookup"><span data-stu-id="60c86-146">The homepage has its own counter.</span></span>

<span data-ttu-id="60c86-147">カウンターのコンポーネントにパラメーターを追加する更新コンポーネントの`@functions`ブロック。</span><span class="sxs-lookup"><span data-stu-id="60c86-147">To add a parameter to the Counter component, update the component's `@functions` block:</span></span>

* <span data-ttu-id="60c86-148">プロパティを追加`IncrementAmount`で修飾された、`[Parameter]`属性。</span><span class="sxs-lookup"><span data-stu-id="60c86-148">Add a property for `IncrementAmount` decorated with the `[Parameter]` attribute.</span></span>
* <span data-ttu-id="60c86-149">`currentCount` の値を増やすときに `IncrementAmount` を使うように `IncrementCount` メソッドを変更します。</span><span class="sxs-lookup"><span data-stu-id="60c86-149">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="60c86-150">*Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="60c86-150">*Pages/Counter.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.cshtml?highlight=4,8)]

<span data-ttu-id="60c86-151">属性を使って、Home コンポーネントの `<Counter>` 要素に `IncrementAmount` パラメーターを指定します。</span><span class="sxs-lookup"><span data-stu-id="60c86-151">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="60c86-152">*Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="60c86-152">*Pages/Index.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.cshtml)]

<span data-ttu-id="60c86-153">アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="60c86-153">Run the app.</span></span> <span data-ttu-id="60c86-154">ホームページには、10 たびにインクリメントする独自のカウンター、 **Click me**ボタンが選択されています。</span><span class="sxs-lookup"><span data-stu-id="60c86-154">The homepage has its own counter that increments by ten each time the **Click me** button is selected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="60c86-155">次の手順</span><span class="sxs-lookup"><span data-stu-id="60c86-155">Next steps</span></span>

<xref:tutorials/first-razor-components-app>
