---
title: 最初の Razor Components アプリを構築する
author: guardrex
description: Razor Components アプリを段階的に構築し、Razor Components に関する基本的な概念について学習します。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/11/2019
uid: tutorials/first-razor-components-app
ms.openlocfilehash: 0c3dd2366581d73bad44e2911602e13c6c0daf9a
ms.sourcegitcommit: af8a6eb5375ef547a52ffae22465e265837aa82b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/12/2019
ms.locfileid: "56159344"
---
# <a name="build-your-first-razor-components-app"></a><span data-ttu-id="2117d-103">最初の Razor Components アプリを構築する</span><span class="sxs-lookup"><span data-stu-id="2117d-103">Build your first Razor Components app</span></span>

<span data-ttu-id="2117d-104">作成者: [Daniel Roth](https://github.com/danroth27)、[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2117d-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="2117d-105">このチュートリアルでは、Razor Components を使ってアプリを構築する方法を説明し、Razor Components に関する基本的な概念を示します。</span><span class="sxs-lookup"><span data-stu-id="2117d-105">This tutorial shows you how to build an app with Razor Components and demonstrates basic Razor Components concepts.</span></span> <span data-ttu-id="2117d-106">Razor Components ベースのプロジェクト (.NET Core 3.0 以降でサポート)、Blazor ベースのプロジェクト (.NET Core の今後のリリースでサポート) のいずれかを使ってこのチュートリアルに取り組むことができます。</span><span class="sxs-lookup"><span data-stu-id="2117d-106">You can enjoy this tutorial using either a Razor Components-based project (supported in .NET Core 3.0 or later) or using a Blazor-based project (supported in a future release of .NET Core).</span></span>

<span data-ttu-id="2117d-107">ASP.NET Core Razor Components を使ったエクスペリエンスの場合 ("*推奨*"):</span><span class="sxs-lookup"><span data-stu-id="2117d-107">For an experience using ASP.NET Core Razor Components (*recommended*):</span></span>

* <span data-ttu-id="2117d-108"><xref:razor-components/get-started> のガイダンスに従って Razor Components ベースのプロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="2117d-108">Follow the guidance in <xref:razor-components/get-started> to create a Razor Components-based project.</span></span>
* <span data-ttu-id="2117d-109">プロジェクトに `RazorComponents` という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="2117d-109">Name the project `RazorComponents`.</span></span>
* <span data-ttu-id="2117d-110">Razor Components のテンプレートから複数プロジェクトのソリューションが作成されます。</span><span class="sxs-lookup"><span data-stu-id="2117d-110">A multi-project solution is created from the Razor Components template.</span></span> <span data-ttu-id="2117d-111">Razor Components プロジェクトは *RazorComponents.App* として生成されます。</span><span class="sxs-lookup"><span data-stu-id="2117d-111">The Razor Components project is generated as *RazorComponents.App*.</span></span>

<span data-ttu-id="2117d-112">Blazor を使ったエクスペリエンスの場合:</span><span class="sxs-lookup"><span data-stu-id="2117d-112">For an experience using Blazor:</span></span>

* <span data-ttu-id="2117d-113"><xref:spa/blazor/get-started> のガイダンスに従って Blazor ベースのプロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="2117d-113">Follow the guidance in <xref:spa/blazor/get-started> to create a Blazor-based project.</span></span>
* <span data-ttu-id="2117d-114">プロジェクトに `Blazor` という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="2117d-114">Name the project `Blazor`.</span></span>
* <span data-ttu-id="2117d-115">Blazor のテンプレートから単一プロジェクトのソリューションが作成されます。</span><span class="sxs-lookup"><span data-stu-id="2117d-115">A single-project solution is created from the Blazor template.</span></span>

<span data-ttu-id="2117d-116">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-razor-components-app/samples/)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="2117d-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-razor-components-app/samples/) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="2117d-117">前提条件については、次のトピックをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="2117d-117">See the following topics for prerequisites:</span></span>

## <a name="build-components"></a><span data-ttu-id="2117d-118">コンポーネントを構築する</span><span class="sxs-lookup"><span data-stu-id="2117d-118">Build components</span></span>

1. <span data-ttu-id="2117d-119">アプリの 3 つのページにそれぞれ移動します。ホーム、カウンター、データのフェッチです。</span><span class="sxs-lookup"><span data-stu-id="2117d-119">Browse to each of the app's three pages: Home, Counter, and Fetch data.</span></span> <span data-ttu-id="2117d-120">これらのページは、*Pages* フォルダーにある Razor ファイルによって実装されています。*Index.cshtml*、*Counter.cshtml*、*FetchData.cshtml* です。</span><span class="sxs-lookup"><span data-stu-id="2117d-120">These pages are implemented by Razor files in the *Pages* folder: *Index.cshtml*, *Counter.cshtml*, and *FetchData.cshtml*.</span></span>

1. <span data-ttu-id="2117d-121">Counter ページ上で **[クリックしてください]** ボタンを選択し、ページを更新することなくカウンターをインクリメントします。</span><span class="sxs-lookup"><span data-stu-id="2117d-121">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="2117d-122">Web ページでカウンターをインクリメントする場合、通常は JavaScript を記述することが必要ですが、Razor Components には C# を使ったより優れた方法が用意されています。</span><span class="sxs-lookup"><span data-stu-id="2117d-122">Incrementing a counter in a webpage normally requires writing JavaScript, but Razor Components provides a better approach using C#.</span></span>

1. <span data-ttu-id="2117d-123">Counter コンポーネントの実装を、*Counter.cshtml* ファイルで調べます。</span><span class="sxs-lookup"><span data-stu-id="2117d-123">Examine the implementation of the Counter component in the *Counter.cshtml* file.</span></span>

   <span data-ttu-id="2117d-124">*Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="2117d-124">*Pages/Counter.cshtml*:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Counter1.cshtml)]

   <span data-ttu-id="2117d-125">Counter コンポーネントの UI は、HTML を使って定義されています。</span><span class="sxs-lookup"><span data-stu-id="2117d-125">The UI of the Counter component is defined using HTML.</span></span> <span data-ttu-id="2117d-126">動的なレンダリング ロジック (たとえばループ、条件、式) が、[Razor](xref:mvc/views/razor) と呼ばれる埋め込みの C# 構文を使って追加されています。</span><span class="sxs-lookup"><span data-stu-id="2117d-126">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="2117d-127">HTML マークアップと C# のレンダリング ロジックは、ビルド時にコンポーネント クラスに変換されます。</span><span class="sxs-lookup"><span data-stu-id="2117d-127">The HTML markup and C# rendering logic are converted into a component class at build time.</span></span> <span data-ttu-id="2117d-128">生成される .NET クラスの名前はファイル名と同じです。</span><span class="sxs-lookup"><span data-stu-id="2117d-128">The name of the generated .NET class matches the file name.</span></span>

   <span data-ttu-id="2117d-129">コンポーネント クラスのメンバーは、`@functions` ブロック内で定義されています。</span><span class="sxs-lookup"><span data-stu-id="2117d-129">Members of the component class are defined in a `@functions` block.</span></span> <span data-ttu-id="2117d-130">`@functions` ブロック内では、イベント処理や他のコンポーネント ロジックの定義のために、コンポーネントの状態 (プロパティ、フィールド) とメソッドを指定します。</span><span class="sxs-lookup"><span data-stu-id="2117d-130">In the `@functions` block, component state (properties, fields) and methods are specified for event handling or for defining other component logic.</span></span> <span data-ttu-id="2117d-131">これらのメンバーは、コンポーネントのレンダリング ロジックの一部として、またイベントを処理するために使われます。</span><span class="sxs-lookup"><span data-stu-id="2117d-131">These members are then used as part of the component's rendering logic and for handling events.</span></span>

   <span data-ttu-id="2117d-132">**[クリックしてください]** ボタンを選択すると:</span><span class="sxs-lookup"><span data-stu-id="2117d-132">When the **Click me** button is selected:</span></span>

   * <span data-ttu-id="2117d-133">Counter コンポーネントの登録済みの `onclick` ハンドラーが呼び出されます (`IncrementCount` メソッドです)。</span><span class="sxs-lookup"><span data-stu-id="2117d-133">The Counter component's registered `onclick` handler is called (the `IncrementCount` method).</span></span>
   * <span data-ttu-id="2117d-134">Counter コンポーネントによりそのレンダリング ツリーが再生成されます。</span><span class="sxs-lookup"><span data-stu-id="2117d-134">The Counter component regenerates its render tree.</span></span>
   * <span data-ttu-id="2117d-135">新しいレンダリング ツリーが以前のものと比較されます。</span><span class="sxs-lookup"><span data-stu-id="2117d-135">The new render tree is compared to the previous one.</span></span>
   * <span data-ttu-id="2117d-136">ドキュメント オブジェクト モデル (DOM) に対する変更のみが適用されます。</span><span class="sxs-lookup"><span data-stu-id="2117d-136">Only modifications to the Document Object Model (DOM) are applied.</span></span> <span data-ttu-id="2117d-137">表示されている数が更新されます。</span><span class="sxs-lookup"><span data-stu-id="2117d-137">The displayed count is updated.</span></span>

1. <span data-ttu-id="2117d-138">Counter コンポーネントの C# のロジックを変更して、カウントが 1 ではなく 2 ずつインクリメントされるようにします。</span><span class="sxs-lookup"><span data-stu-id="2117d-138">Modify the C# logic of the Counter component to make the count increment by two instead of one.</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Counter2.cshtml?highlight=14)]

1. <span data-ttu-id="2117d-139">アプリをリビルドして実行し、変更を確認します。</span><span class="sxs-lookup"><span data-stu-id="2117d-139">Rebuild and run the app to see the changes.</span></span> <span data-ttu-id="2117d-140">**[クリックしてください]** ボタンを選択すると、カウンターは 2 ずつインクリメントされます。</span><span class="sxs-lookup"><span data-stu-id="2117d-140">Select the **Click me** button, and the counter increments by two.</span></span>

## <a name="use-components"></a><span data-ttu-id="2117d-141">コンポーネントを使う</span><span class="sxs-lookup"><span data-stu-id="2117d-141">Use components</span></span>

<span data-ttu-id="2117d-142">HTML のような構文を使用して、別のコンポーネントにコンポーネントを含めます。</span><span class="sxs-lookup"><span data-stu-id="2117d-142">Include a component into another component using an HTML-like syntax.</span></span>

1. <span data-ttu-id="2117d-143">Index コンポーネントに `<Counter />` 要素を追加することで、アプリの Index (ホーム ページ) コンポーネントにカウンター コンポーネントを追加します。</span><span class="sxs-lookup"><span data-stu-id="2117d-143">Add the Counter component to the app's Index (home page) component by adding a `<Counter />` element to the Index component.</span></span>

   <span data-ttu-id="2117d-144">このエクスペリエンスのために Blazor を使っている場合は、Survey Prompt コンポーネント (`<SurveyPrompt>` 要素) が Index コンポーネントに含まれています。</span><span class="sxs-lookup"><span data-stu-id="2117d-144">If you're using Blazor for this experience, a Survey Prompt component (`<SurveyPrompt>` element) is in the Index component.</span></span> <span data-ttu-id="2117d-145">`<SurveyPrompt>` 要素を `<Counter>` 要素に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="2117d-145">Replace the `<SurveyPrompt>` element with the `<Counter>` element.</span></span>

   <span data-ttu-id="2117d-146">*Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="2117d-146">*Pages/Index.cshtml*:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Index.cshtml?highlight=7)]

1. <span data-ttu-id="2117d-147">アプリケーションをリビルドして実行します。</span><span class="sxs-lookup"><span data-stu-id="2117d-147">Rebuild and run the app.</span></span> <span data-ttu-id="2117d-148">ホーム ページに、独自のカウンターがあります。</span><span class="sxs-lookup"><span data-stu-id="2117d-148">The home page has its own counter.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="2117d-149">コンポーネントのパラメーター</span><span class="sxs-lookup"><span data-stu-id="2117d-149">Component parameters</span></span>

<span data-ttu-id="2117d-150">コンポーネントにパラメーターを持たせることもできます。</span><span class="sxs-lookup"><span data-stu-id="2117d-150">Components can also have parameters.</span></span> <span data-ttu-id="2117d-151">コンポーネントのパラメーターは、`[Parameter]` で修飾されたコンポーネント クラス上の、パブリックでないプロパティを使って定義します。</span><span class="sxs-lookup"><span data-stu-id="2117d-151">Component parameters are defined using non-public properties on the component class decorated with `[Parameter]`.</span></span> <span data-ttu-id="2117d-152">マークアップ内でコンポーネントの引数を指定するには、属性を使います。</span><span class="sxs-lookup"><span data-stu-id="2117d-152">Use attributes to specify arguments for a component in markup.</span></span>

1. <span data-ttu-id="2117d-153">コンポーネントの `@functions` に関する C# コードを更新します。</span><span class="sxs-lookup"><span data-stu-id="2117d-153">Update the component's `@functions` C# code:</span></span>

   * <span data-ttu-id="2117d-154">`[Parameter]` 属性で修飾された `IncrementAmount` プロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="2117d-154">Add a `IncrementAmount` property decorated with the `[Parameter]` attribute.</span></span>
   * <span data-ttu-id="2117d-155">`currentCount` の値を増やすときに `IncrementAmount` を使うように `IncrementCount` メソッドを変更します。</span><span class="sxs-lookup"><span data-stu-id="2117d-155">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

   <span data-ttu-id="2117d-156">*Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="2117d-156">*Pages/Counter.cshtml*:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/RazorComponents.App/Pages/Counter.cshtml?highlight=12,16)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. <span data-ttu-id="2117d-157">属性を使って、Home コンポーネントの `<Counter>` 要素に `IncrementAmount` パラメーターを指定します。</span><span class="sxs-lookup"><span data-stu-id="2117d-157">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span> <span data-ttu-id="2117d-158">カウンターを 10 ずつインクリメントするように値を設定します。</span><span class="sxs-lookup"><span data-stu-id="2117d-158">Set the value to increment the counter by ten.</span></span>

   <span data-ttu-id="2117d-159">*Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="2117d-159">*Pages/Index.cshtml*:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/RazorComponents.App/Pages/Index.cshtml?highlight=7)]

1. <span data-ttu-id="2117d-160">ページを再度読み込みます。</span><span class="sxs-lookup"><span data-stu-id="2117d-160">Reload the page.</span></span> <span data-ttu-id="2117d-161">ホーム ページのカウンターは、**[クリックしてください]** ボタンを選択するたびに 10 ずつインクリメントされます。</span><span class="sxs-lookup"><span data-stu-id="2117d-161">The home page counter increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="2117d-162">*Counter* ページ上のカウンターは 1 ずつインクリメントされます。</span><span class="sxs-lookup"><span data-stu-id="2117d-162">The counter on the *Counter* page increments by one.</span></span>

## <a name="route-to-components"></a><span data-ttu-id="2117d-163">コンポーネントにルーティングする</span><span class="sxs-lookup"><span data-stu-id="2117d-163">Route to components</span></span>

<span data-ttu-id="2117d-164">*Counter.cshtml* ファイルの上部にある `@page` ディレクティブは、このコンポーネントがルーティング エンドポイントであることを指定しています。</span><span class="sxs-lookup"><span data-stu-id="2117d-164">The `@page` directive at the top of the *Counter.cshtml* file specifies that this component is a routing endpoint.</span></span> <span data-ttu-id="2117d-165">Counter コンポーネントによって `/Counter` に送信された要求が処理されます。</span><span class="sxs-lookup"><span data-stu-id="2117d-165">The Counter component handles requests sent to `/Counter`.</span></span> <span data-ttu-id="2117d-166">`@page` ディレクティブがない場合、ルーティングされた要求はコンポーネントで処理されませんが、このコンポーネントは引き続き他のコンポーネントで使えます。</span><span class="sxs-lookup"><span data-stu-id="2117d-166">Without the `@page` directive, the component doesn't handle routed requests, but the component can still be used by other components.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="2117d-167">依存関係の挿入</span><span class="sxs-lookup"><span data-stu-id="2117d-167">Dependency injection</span></span>

<span data-ttu-id="2117d-168">アプリのサービス コンテナーに登録されたサービスは、[依存関係の挿入 (DI)](xref:fundamentals/dependency-injection) を介してコンポーネントから使用できます。</span><span class="sxs-lookup"><span data-stu-id="2117d-168">Services registered in the app's service container are available to components via [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="2117d-169">`@inject` ディレクティブを使ってサービスをコンポーネントに挿入します。</span><span class="sxs-lookup"><span data-stu-id="2117d-169">Inject services into a component using the `@inject` directive.</span></span>

<span data-ttu-id="2117d-170">FetchData コンポーネントのディレクティブを調べます (*Pages/FetchData.cshtml*)。</span><span class="sxs-lookup"><span data-stu-id="2117d-170">Examine the directives of the FetchData component (*Pages/FetchData.cshtml*).</span></span> <span data-ttu-id="2117d-171">コンポーネントに `WeatherForecastService` サービスのインスタンスを挿入するために、`@inject` ディレクティブが使われています。</span><span class="sxs-lookup"><span data-stu-id="2117d-171">The `@inject` directive is used to inject the instance of the `WeatherForecastService` service into the component:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData1.cshtml?highlight=3)]

<span data-ttu-id="2117d-172">`WeatherForecastService` サービスは[シングルトン](xref:fundamentals/dependency-injection#service-lifetimes)として登録されているため、このサービスの 1 つのインスタンスをアプリ全体で使用できます。</span><span class="sxs-lookup"><span data-stu-id="2117d-172">The `WeatherForecastService` service is registered as a [singleton](xref:fundamentals/dependency-injection#service-lifetimes), so one instance of the service is available throughout the app.</span></span>

<span data-ttu-id="2117d-173">FetchData コンポーネントでは、`ForecastService` のような挿入されたサービスを使って、`WeatherForecast` オブジェクトの配列を取得します。</span><span class="sxs-lookup"><span data-stu-id="2117d-173">The FetchData component uses the injected service, as `ForecastService`, to retrieve an array of `WeatherForecast` objects:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData2.cshtml?highlight=6)]

<span data-ttu-id="2117d-174">各 forecast 変数を気象データのテーブルの行としてレンダリングするために、[@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) ループが使われています。</span><span class="sxs-lookup"><span data-stu-id="2117d-174">A [@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) loop is used to render each forecast instance as a row in the table of weather data:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData3.cshtml?highlight=11-19)]

## <a name="build-a-todo-list"></a><span data-ttu-id="2117d-175">Todo リストを構築する</span><span class="sxs-lookup"><span data-stu-id="2117d-175">Build a todo list</span></span>

<span data-ttu-id="2117d-176">シンプルな ToDo リストを実装した新しいページをアプリに追加します。</span><span class="sxs-lookup"><span data-stu-id="2117d-176">Add a new page to the app that implements a simple todo list.</span></span>

1. <span data-ttu-id="2117d-177">*Pages* フォルダーに、*Todo.cshtml* という名前の空のファイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="2117d-177">Add an empty file to the *Pages* folder named *Todo.cshtml*.</span></span>

1. <span data-ttu-id="2117d-178">ページの最初のマークアップを指定します。</span><span class="sxs-lookup"><span data-stu-id="2117d-178">Provide the initial markup for the page:</span></span>

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. <span data-ttu-id="2117d-179">ナビゲーション バーに Todo ページを追加します。</span><span class="sxs-lookup"><span data-stu-id="2117d-179">Add the Todo page to the navigation bar.</span></span>

   <span data-ttu-id="2117d-180">NavMenu コンポーネント (*Shared/NavMenu.csthml*) は、アプリのレイアウトで使用します。</span><span class="sxs-lookup"><span data-stu-id="2117d-180">The NavMenu component (*Shared/NavMenu.csthml*) is used in the app's layout.</span></span> <span data-ttu-id="2117d-181">レイアウトは、アプリ内でのコンテンツの重複を回避するために使うコンポーネントです。</span><span class="sxs-lookup"><span data-stu-id="2117d-181">Layouts are components that allow you to avoid duplication of content in the app.</span></span> <span data-ttu-id="2117d-182">詳細については、「<xref:razor-components/layouts>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="2117d-182">For more information, see <xref:razor-components/layouts>.</span></span>

   <span data-ttu-id="2117d-183">*Shared/NavMenu.csthml* ファイルで、以下のリスト アイテムのマークアップを既存のリスト アイテムの下に追加して、Todo ページ用の `<NavLink>` を追加します。</span><span class="sxs-lookup"><span data-stu-id="2117d-183">Add a `<NavLink>` for the Todo page by adding the following list item markup below the existing list items in the *Shared/NavMenu.csthml* file:</span></span>

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. <span data-ttu-id="2117d-184">アプリケーションをリビルドして実行します。</span><span class="sxs-lookup"><span data-stu-id="2117d-184">Rebuild and run the app.</span></span> <span data-ttu-id="2117d-185">新しい Todo ページに移動して、Todo ページへのリンクが機能することを確認します。</span><span class="sxs-lookup"><span data-stu-id="2117d-185">Visit the new Todo page to confirm that the link to the Todo page works.</span></span>

1. <span data-ttu-id="2117d-186">Todo アイテムを表すクラスを保持するために、プロジェクトのルートに *TodoItem.cs* ファイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="2117d-186">Add a *TodoItem.cs* file to the root of the project to hold a class that represents a todo item.</span></span> <span data-ttu-id="2117d-187">`TodoItem` クラス用に次の C# コードを使います。</span><span class="sxs-lookup"><span data-stu-id="2117d-187">Use the following C# code for the `TodoItem` class:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/RazorComponents.App/TodoItem.cs)]

1. <span data-ttu-id="2117d-188">Todo コンポーネントに戻ります (*Todo.cshtml*)。</span><span class="sxs-lookup"><span data-stu-id="2117d-188">Return to the Todo component (*Todo.cshtml*):</span></span>

   * <span data-ttu-id="2117d-189">Todo 用のフィールドを `@functions` ブロックに追加します。</span><span class="sxs-lookup"><span data-stu-id="2117d-189">Add a field for the todos in an `@functions` block.</span></span> <span data-ttu-id="2117d-190">Todo コンポーネントでは、このフィールドを使って Todo リストの状態を維持します。</span><span class="sxs-lookup"><span data-stu-id="2117d-190">The Todo component uses this field to maintain the state of the todo list.</span></span>
   * <span data-ttu-id="2117d-191">各 ToDo アイテムをリスト アイテムとしてレンダリングするために、順序のないリストのマークアップと `foreach` ループを追加します。</span><span class="sxs-lookup"><span data-stu-id="2117d-191">Add unordered list markup and a `foreach` loop to render each todo item as a list item.</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData4.cshtml?highlight=5-10,12-14)]

1. <span data-ttu-id="2117d-192">アプリには、リストに ToDo を追加するための UI 要素が必要です。</span><span class="sxs-lookup"><span data-stu-id="2117d-192">The app requires UI elements for adding todos to the list.</span></span> <span data-ttu-id="2117d-193">リストの下に、テキスト入力とボタンを追加します。</span><span class="sxs-lookup"><span data-stu-id="2117d-193">Add a text input and a button below the list:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData5.cshtml?highlight=12-13)]

1. <span data-ttu-id="2117d-194">アプリケーションをリビルドして実行します。</span><span class="sxs-lookup"><span data-stu-id="2117d-194">Rebuild and run the app.</span></span> <span data-ttu-id="2117d-195">**[Add todo]\(ToDo の追加\)** ボタンを選択しても何も起こりません。ボタンにイベント ハンドラーが関連付けられていないためです。</span><span class="sxs-lookup"><span data-stu-id="2117d-195">When the **Add todo** button is selected, nothing happens because an event handler isn't wired up to the button.</span></span>

1. <span data-ttu-id="2117d-196">Todo コンポーネントに `AddTodo` メソッドを追加し、`onclick` 属性を使ってボタンのクリック用にこれを登録します。</span><span class="sxs-lookup"><span data-stu-id="2117d-196">Add an `AddTodo` method to the Todo component and register it for button clicks using the `onclick` attribute:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData6.cshtml?highlight=2,7-10)]

   <span data-ttu-id="2117d-197">ボタンを選択すると C# のメソッド `AddTodo` が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="2117d-197">The `AddTodo` C# method is called when the button is selected.</span></span>

1. <span data-ttu-id="2117d-198">新しい Todo アイテムのタイトルを取得するために、文字列フィールド `newTodo` を追加し、`bind` 属性を使ってこれをテキスト入力の値とバインドします。</span><span class="sxs-lookup"><span data-stu-id="2117d-198">To get the title of the new todo item, add a `newTodo` string field and bind it to the value of the text input using the `bind` attribute:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData7.cshtml?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" bind="@newTodo" />
   ```

1. <span data-ttu-id="2117d-199">指定したタイトルを備えた `TodoItem` をリストに追加するように、`AddTodo` メソッドを更新します。</span><span class="sxs-lookup"><span data-stu-id="2117d-199">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="2117d-200">`newTodo` を空の文字列に設定して、テキスト入力の値をクリアします。</span><span class="sxs-lookup"><span data-stu-id="2117d-200">Clear the value of the text input by setting `newTodo` to an empty string:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData8.cshtml?highlight=19-26)]

1. <span data-ttu-id="2117d-201">アプリケーションをリビルドして実行します。</span><span class="sxs-lookup"><span data-stu-id="2117d-201">Rebuild and run the app.</span></span> <span data-ttu-id="2117d-202">Todo リストに Todo をいくつか追加して、新しいコードをテストします。</span><span class="sxs-lookup"><span data-stu-id="2117d-202">Add some todos to the todo list to test the new code.</span></span>

1. <span data-ttu-id="2117d-203">各 Todo アイテムのタイトルのテキストは編集可能にすることができます。また、チェック ボックスはユーザーが完了したアイテムを追跡するのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="2117d-203">The title text for each todo item can be made editable and a check box can help the user keep track of completed items.</span></span> <span data-ttu-id="2117d-204">各 Todo アイテムにチェック ボックス入力を追加し、その値を `IsDone` プロパティにバインドします。</span><span class="sxs-lookup"><span data-stu-id="2117d-204">Add a check box input for each todo item and bind its value to the `IsDone` property.</span></span> <span data-ttu-id="2117d-205">`@todo.Title` を、`@todo.Title` にバインドされた `<input>` 要素に変更します。</span><span class="sxs-lookup"><span data-stu-id="2117d-205">Change `@todo.Title` to an `<input>` element bound to `@todo.Title`:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData9.cshtml?highlight=5-6)]

1. <span data-ttu-id="2117d-206">それらの値がバインドされていることを確認するために、`<h1>` ヘッダーを更新して、完了していない (`IsDone` が `false` の) Todo アイテムの数のカウントを表示するようにします。</span><span class="sxs-lookup"><span data-stu-id="2117d-206">To verify that these values are bound, update the `<h1>` header to show a count of the number of todo items that aren't complete (`IsDone` is `false`).</span></span>

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. <span data-ttu-id="2117d-207">完成した Todo コンポーネント (*Todo.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="2117d-207">The completed Todo component (*Todo.cshtml*):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/RazorComponents.App/Pages/Todo.cshtml)]

1. <span data-ttu-id="2117d-208">アプリケーションをリビルドして実行します。</span><span class="sxs-lookup"><span data-stu-id="2117d-208">Rebuild and run the app.</span></span> <span data-ttu-id="2117d-209">Todo アイテムを追加して新しいコードをテストします。</span><span class="sxs-lookup"><span data-stu-id="2117d-209">Add todo items to test the new code.</span></span>

## <a name="publish-and-deploy-the-app"></a><span data-ttu-id="2117d-210">アプリを発行および配置する</span><span class="sxs-lookup"><span data-stu-id="2117d-210">Publish and deploy the app</span></span>

<span data-ttu-id="2117d-211">アプリの発行方法については、<xref:host-and-deploy/razor-components/index#publish-the-app> をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="2117d-211">To publish the app, see <xref:host-and-deploy/razor-components/index#publish-the-app>.</span></span>
