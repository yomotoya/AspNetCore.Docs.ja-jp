---
title: Razor のコンポーネントを概要します。
author: guardrex
description: 作成と Razor コンポーネント プロジェクトを変更して Razor コンポーネントを開始する方法について説明します。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: razor-components/get-started
ms.openlocfilehash: 151e58497b0f22fa7c5a9bde1f665eeb73fd5dc3
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/08/2019
ms.locfileid: "59515651"
---
# <a name="get-started-with-razor-components"></a><span data-ttu-id="51616-103">Razor のコンポーネントを概要します。</span><span class="sxs-lookup"><span data-stu-id="51616-103">Get started with Razor Components</span></span>

<span data-ttu-id="51616-104">作成者: [Daniel Roth](https://github.com/danroth27)、[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="51616-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

# [<a name="visual-studio"></a><span data-ttu-id="51616-105">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="51616-105">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="51616-106">必要条件:</span><span class="sxs-lookup"><span data-stu-id="51616-106">Prerequisites:</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

<span data-ttu-id="51616-107">Visual Studio で、最初の Razor コンポーネント プロジェクトを作成するには</span><span class="sxs-lookup"><span data-stu-id="51616-107">To create your first Razor Components project in Visual Studio:</span></span>

1. <span data-ttu-id="51616-108">最新のインストール[.NET Core 3.0 プレビュー SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0)リリースします。</span><span class="sxs-lookup"><span data-stu-id="51616-108">Install the latest [.NET Core 3.0 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.</span></span>
1. <span data-ttu-id="51616-109">Visual Studio プレビュー Sdk の使用を有効にします。</span><span class="sxs-lookup"><span data-stu-id="51616-109">Enable Visual Studio to use preview SDKs:</span></span>
   1. <span data-ttu-id="51616-110">開いている**ツール** > **オプション**メニュー バーにします。</span><span class="sxs-lookup"><span data-stu-id="51616-110">Open **Tools** > **Options** in the menu bar.</span></span>
   1. <span data-ttu-id="51616-111">開く、**プロジェクトおよびソリューション**ノード。</span><span class="sxs-lookup"><span data-stu-id="51616-111">Open the **Projects and Solutions** node.</span></span> <span data-ttu-id="51616-112">開く、 **.NET Core**タブ。</span><span class="sxs-lookup"><span data-stu-id="51616-112">Open the **.NET Core** tab.</span></span>
   1. <span data-ttu-id="51616-113">チェック ボックスをオン **、.NET Core SDK のプレビューを使用して、** します。</span><span class="sxs-lookup"><span data-stu-id="51616-113">Check the box for **Use previews of the .NET Core SDK**.</span></span> <span data-ttu-id="51616-114">**[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="51616-114">Select **OK**.</span></span>
1. <span data-ttu-id="51616-115">新しいプロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="51616-115">Create a new project.</span></span>
1. <span data-ttu-id="51616-116">**[ASP.NET Core Web アプリケーション]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="51616-116">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="51616-117">**[次へ]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="51616-117">Select **Next**.</span></span>
1. <span data-ttu-id="51616-118">名前を入力、**プロジェクト名**フィールド。</span><span class="sxs-lookup"><span data-stu-id="51616-118">Provide a name in the **Project name** field.</span></span> <span data-ttu-id="51616-119">確認、**場所**エントリが正しいか、プロジェクトの場所を指定します。</span><span class="sxs-lookup"><span data-stu-id="51616-119">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="51616-120">**[作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="51616-120">Select **Create**.</span></span>
1. <span data-ttu-id="51616-121">確認します **.NET Core**と**ASP.NET Core 3.0**上部にある選択されます。</span><span class="sxs-lookup"><span data-stu-id="51616-121">Make sure **.NET Core** and **ASP.NET Core 3.0** are selected at the top.</span></span>
1. <span data-ttu-id="51616-122">選択、 **Razor コンポーネント**テンプレートと選択**作成**です。</span><span class="sxs-lookup"><span data-stu-id="51616-122">Choose the **Razor Components** template and select **Create**.</span></span>
1. <span data-ttu-id="51616-123">**F5 キー**を押してアプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="51616-123">Press **F5** to run the app.</span></span>

<span data-ttu-id="51616-124">おめでとうございます!</span><span class="sxs-lookup"><span data-stu-id="51616-124">Congratulations!</span></span> <span data-ttu-id="51616-125">最初の Razor コンポーネント アプリを実行しました。</span><span class="sxs-lookup"><span data-stu-id="51616-125">You just ran your first Razor Components app!</span></span>

<!--

# [Visual Studio Code](#tab/visual-studio-code)

Prerequisites:

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

To create your first Razor Components project in Visual Studio Code:

1. Execute the following command from a command shell:

   ```console
   dotnet new razorcomponents -o WebApplication1
   ```

1. Open the *WebApplication1* folder in Visual Studio Code.

1. Add a *.vscode* folder.

1. Add a *tasks.json* file to the *.vscode* folder with the following content:

   [!code-json[](get-started/samples_snapshot/3.x/tasks.json)]

1. Add a *launch.json* file to the *.vscode* folder with the following content:

   [!code-json[](get-started/samples_snapshot/3.x/launch.json)]

1. Execute the app using the Visual Studio Code debugger.

1. In a browser, navigate to `https://localhost:5001`.

Congratulations! You just ran your first Razor Components app!

# [Visual Studio for Mac](#tab/visual-studio-mac)

.NET Core 3.0 will be supported with Visual Studio for Mac version 8.0 or later. Visual Studio for Mac version 8.0 Preview isn't available at this time.

Use the [.NET Core CLI version of this topic](xref:razor-components/get-started?tabs=netcore-cli) on macOS.

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

To create your first project Razor Components project in Visual Studio for Mac:

1. Select **File** > **New Solution** or **New Project**.
1. In the sidebar, select **.NET Core** > **App**.
1. Select **ASP.NET Core Razor Components** and select **Next**.
1. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.
1. In the **Project Name** field, enter `WebApplication1`. Select **Create**.
1. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

Congratulations! You just ran your first Razor Components app!
-->

# [<a name="net-core-cli"></a><span data-ttu-id="51616-126">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="51616-126">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="51616-127">必要条件:</span><span class="sxs-lookup"><span data-stu-id="51616-127">Prerequisites:</span></span>

* [<span data-ttu-id="51616-128">.NET core SDK 3.0 プレビュー</span><span class="sxs-lookup"><span data-stu-id="51616-128">.NET Core SDK 3.0 Preview</span></span>](https://dotnet.microsoft.com/download/dotnet-core/3.0)

1. <span data-ttu-id="51616-129">コマンド シェルから、最初の Razor コンポーネント プロジェクトを作成するには</span><span class="sxs-lookup"><span data-stu-id="51616-129">To create your first Razor Components project from a command shell:</span></span>

   ```console
   dotnet new razorcomponents -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

1. <span data-ttu-id="51616-130">ブラウザーで、`https://localhost:5001` に移動します。</span><span class="sxs-lookup"><span data-stu-id="51616-130">In a browser, navigate to `https://localhost:5001`.</span></span>

<span data-ttu-id="51616-131">おめでとうございます!</span><span class="sxs-lookup"><span data-stu-id="51616-131">Congratulations!</span></span> <span data-ttu-id="51616-132">最初の Razor コンポーネント アプリを実行しました。</span><span class="sxs-lookup"><span data-stu-id="51616-132">You just ran your first Razor Components app!</span></span>

---

## <a name="razor-components-project"></a><span data-ttu-id="51616-133">Razor のコンポーネントのプロジェクト</span><span class="sxs-lookup"><span data-stu-id="51616-133">Razor Components project</span></span>

<span data-ttu-id="51616-134">Razor のコンポーネントは、Razor 構文を使用して作成されますが、Razor ページと MVC のビューとは異なるコンパイルされます。</span><span class="sxs-lookup"><span data-stu-id="51616-134">Razor Components are authored using Razor syntax but are compiled differently than Razor Pages and MVC views.</span></span> <span data-ttu-id="51616-135">*.Razor* Razor コンポーネントを指定するファイル拡張子を使用します。</span><span class="sxs-lookup"><span data-stu-id="51616-135">The *.razor* file extension is used to specify a Razor Component.</span></span> <span data-ttu-id="51616-136">Razor ページと MVC ビューを引き続き使用する、 *.cshtml*ファイル拡張子。</span><span class="sxs-lookup"><span data-stu-id="51616-136">Razor Pages and MVC views continue to use the *.cshtml* file extension.</span></span>

> [!NOTE]
> <span data-ttu-id="51616-137">使用して razor コンポーネントを作成することができます、 *.cshtml* Razor コンポーネントのファイルを使用してそれらのファイルが検出された場合に限り、ファイル拡張子、 `_RazorComponentInclude` MSBuild プロパティです。</span><span class="sxs-lookup"><span data-stu-id="51616-137">Razor Components can be authored using the *.cshtml* file extension as long as those files are identified as Razor Component files using the `_RazorComponentInclude` MSBuild property.</span></span> <span data-ttu-id="51616-138">などのコンポーネントの Razor テンプレートを使用して作成されたアプリを指定するすべて *.cshtml*下でファイル、*コンポーネント*Razor コンポーネントとしてフォルダーを処理する必要があります。</span><span class="sxs-lookup"><span data-stu-id="51616-138">For example, an app created using the Razor Component template specifies that all *.cshtml* files under the *Components* folder should be treated as Razor Components:</span></span>
>
> ```xml
> <_RazorComponentInclude>Components\**\*.cshtml</_RazorComponentInclude>
> ```

<span data-ttu-id="51616-139">アプリを実行すると、複数のページは、サイドバー内のタブから利用できます。</span><span class="sxs-lookup"><span data-stu-id="51616-139">When the app is run, multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="51616-140">Home</span><span class="sxs-lookup"><span data-stu-id="51616-140">Home</span></span>
* <span data-ttu-id="51616-141">カウンター</span><span class="sxs-lookup"><span data-stu-id="51616-141">Counter</span></span>
* <span data-ttu-id="51616-142">データをフェッチします。</span><span class="sxs-lookup"><span data-stu-id="51616-142">Fetch data</span></span>

<span data-ttu-id="51616-143">Counter ページ上で **[クリックしてください]** ボタンを選択し、ページを更新することなくカウンターをインクリメントします。</span><span class="sxs-lookup"><span data-stu-id="51616-143">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="51616-144">Web ページでカウンターをインクリメントする場合、通常は JavaScript を記述することが必要ですが、Razor Components には C# を使ったより優れた方法が用意されています。</span><span class="sxs-lookup"><span data-stu-id="51616-144">Incrementing a counter in a webpage normally requires writing JavaScript, but Razor Components provides a better approach using C#.</span></span>

<span data-ttu-id="51616-145">*WebApplication1/Components/Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="51616-145">*WebApplication1/Components/Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor)]

<span data-ttu-id="51616-146">要求を`/counter`で指定したとおり、ブラウザーで、`@page`上部にあるディレクティブがそのコンテンツを表示するために、カウンターのコンポーネント。</span><span class="sxs-lookup"><span data-stu-id="51616-146">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the Counter component to render its content.</span></span> <span data-ttu-id="51616-147">コンポーネントは、柔軟で効率的な方法で UI を更新するために使用するレンダリング ツリーのメモリ内表現にレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="51616-147">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="51616-148">毎回、 **Click me**ボタンが選択されています。</span><span class="sxs-lookup"><span data-stu-id="51616-148">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="51616-149">`onclick`イベントが発生します。</span><span class="sxs-lookup"><span data-stu-id="51616-149">The `onclick` event is fired.</span></span>
* <span data-ttu-id="51616-150">`IncrementCount` メソッドが呼び出された場合。</span><span class="sxs-lookup"><span data-stu-id="51616-150">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="51616-151">`currentCount`がインクリメントされます。</span><span class="sxs-lookup"><span data-stu-id="51616-151">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="51616-152">コンポーネントは、もう一度表示されます。</span><span class="sxs-lookup"><span data-stu-id="51616-152">The component is rendered again.</span></span>

<span data-ttu-id="51616-153">ランタイムは、前のコンテンツを新しいコンテンツを比較し、ドキュメント オブジェクト モデル (DOM) に変更されたコンテンツにのみ適用されます。</span><span class="sxs-lookup"><span data-stu-id="51616-153">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="51616-154">HTML のような構文を使用して別のコンポーネントにコンポーネントを追加します。</span><span class="sxs-lookup"><span data-stu-id="51616-154">Add a component to another component using an HTML-like syntax.</span></span> <span data-ttu-id="51616-155">コンポーネントのパラメーターは、属性または子コンテンツを使用して指定されます。</span><span class="sxs-lookup"><span data-stu-id="51616-155">Component parameters are specified using attributes or child content.</span></span> <span data-ttu-id="51616-156">追加することでカウンター コンポーネントをアプリのホーム ページに追加するなど、`<Counter />`インデックス コンポーネント要素。</span><span class="sxs-lookup"><span data-stu-id="51616-156">For example, a Counter component can be added to the app's homepage by adding a `<Counter />` element to the Index component.</span></span>

<span data-ttu-id="51616-157">*WebApplication1/Components/Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="51616-157">*WebApplication1/Components/Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="51616-158">アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="51616-158">Run the app.</span></span> <span data-ttu-id="51616-159">ホームページには、独自のカウンターを指定します。</span><span class="sxs-lookup"><span data-stu-id="51616-159">The homepage has its own counter.</span></span>

<span data-ttu-id="51616-160">カウンターのコンポーネントにパラメーターを追加する更新コンポーネントの`@functions`ブロック。</span><span class="sxs-lookup"><span data-stu-id="51616-160">To add a parameter to the Counter component, update the component's `@functions` block:</span></span>

* <span data-ttu-id="51616-161">プロパティを追加`IncrementAmount`で修飾された、`[Parameter]`属性。</span><span class="sxs-lookup"><span data-stu-id="51616-161">Add a property for `IncrementAmount` decorated with the `[Parameter]` attribute.</span></span>
* <span data-ttu-id="51616-162">`currentCount` の値を増やすときに `IncrementAmount` を使うように `IncrementCount` メソッドを変更します。</span><span class="sxs-lookup"><span data-stu-id="51616-162">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="51616-163">*WebApplication1/Components/Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="51616-163">*WebApplication1/Components/Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=4,8)]

<span data-ttu-id="51616-164">属性を使って、Home コンポーネントの `<Counter>` 要素に `IncrementAmount` パラメーターを指定します。</span><span class="sxs-lookup"><span data-stu-id="51616-164">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="51616-165">*WebApplication1/Components/Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="51616-165">*WebApplication1/Components/Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor)]

<span data-ttu-id="51616-166">アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="51616-166">Run the app.</span></span> <span data-ttu-id="51616-167">ホームページには、10 たびにインクリメントする独自のカウンター、 **Click me**ボタンが選択されています。</span><span class="sxs-lookup"><span data-stu-id="51616-167">The homepage has its own counter that increments by ten each time the **Click me** button is selected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="51616-168">次の手順</span><span class="sxs-lookup"><span data-stu-id="51616-168">Next steps</span></span>

<xref:tutorials/first-razor-components-app>
