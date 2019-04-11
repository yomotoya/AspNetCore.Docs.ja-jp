---
title: 最初の Razor Components アプリを構築する
author: guardrex
description: Razor Components アプリを段階的に構築し、Razor Components に関する基本的な概念について学習します。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/06/2019
uid: tutorials/first-razor-components-app
ms.openlocfilehash: 697c4659bcc9952ffe9868fe9b3c0d28019bc369
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/10/2019
ms.locfileid: "59468777"
---
# <a name="build-your-first-razor-components-app"></a><span data-ttu-id="82c25-103">最初の Razor Components アプリを構築する</span><span class="sxs-lookup"><span data-stu-id="82c25-103">Build your first Razor Components app</span></span>

<span data-ttu-id="82c25-104">作成者: [Daniel Roth](https://github.com/danroth27)、[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="82c25-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="82c25-105">このチュートリアルでは、Razor Components を使ってアプリを構築する方法を説明し、Razor Components に関する基本的な概念を示します。</span><span class="sxs-lookup"><span data-stu-id="82c25-105">This tutorial shows you how to build an app with Razor Components and demonstrates basic Razor Components concepts.</span></span> <span data-ttu-id="82c25-106">Razor Components ベースのプロジェクト (.NET Core 3.0 以降でサポート)、Blazor ベースのプロジェクト (.NET Core の今後のリリースでサポート) のいずれかを使ってこのチュートリアルに取り組むことができます。</span><span class="sxs-lookup"><span data-stu-id="82c25-106">You can enjoy this tutorial using either a Razor Components-based project (supported in .NET Core 3.0 or later) or using a Blazor-based project (supported in a future release of .NET Core).</span></span>

<span data-ttu-id="82c25-107">ASP.NET Core Razor Components を使ったエクスペリエンスの場合 ("*推奨*"):</span><span class="sxs-lookup"><span data-stu-id="82c25-107">For an experience using ASP.NET Core Razor Components (*recommended*):</span></span>

* <span data-ttu-id="82c25-108"><xref:razor-components/get-started> のガイダンスに従って Razor Components ベースのプロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="82c25-108">Follow the guidance in <xref:razor-components/get-started> to create a Razor Components-based project.</span></span>
* <span data-ttu-id="82c25-109">プロジェクトに `RazorComponents` という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="82c25-109">Name the project `RazorComponents`.</span></span>

<span data-ttu-id="82c25-110">Blazor を使ったエクスペリエンスの場合:</span><span class="sxs-lookup"><span data-stu-id="82c25-110">For an experience using Blazor:</span></span>

* <span data-ttu-id="82c25-111"><xref:spa/blazor/get-started> のガイダンスに従って Blazor ベースのプロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="82c25-111">Follow the guidance in <xref:spa/blazor/get-started> to create a Blazor-based project.</span></span>
* <span data-ttu-id="82c25-112">プロジェクトに `Blazor` という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="82c25-112">Name the project `Blazor`.</span></span>

<span data-ttu-id="82c25-113">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-razor-components-app/samples/)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="82c25-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-razor-components-app/samples/) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="82c25-114">前提条件については、次のトピックをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="82c25-114">See the following topics for prerequisites:</span></span>

## <a name="build-components"></a><span data-ttu-id="82c25-115">コンポーネントを構築する</span><span class="sxs-lookup"><span data-stu-id="82c25-115">Build components</span></span>

1. <span data-ttu-id="82c25-116">*Components/Pages* フォルダー (Blazor では *Pages* ) 内のアプリの 3 つのページをそれぞれ閲覧します。ホーム、カウンター、データのフェッチです。</span><span class="sxs-lookup"><span data-stu-id="82c25-116">Browse to each of the app's three pages in the *Components/Pages* folder (*Pages* in Blazor): Home, Counter, and Fetch data.</span></span> <span data-ttu-id="82c25-117">これらのページは、次の Razor Component ファイル(*Index.razor*、*Counter.razor*、*FetchData.razor*) によって実装されています。</span><span class="sxs-lookup"><span data-stu-id="82c25-117">These pages are implemented by Razor Component files: *Index.razor*, *Counter.razor*, and *FetchData.razor*.</span></span> <span data-ttu-id="82c25-118">(Blazor は引き続き *.cshtml* ファイル拡張子(*Index.cshtml*、*Counter.cshtml*、*FetchData.cshtml*) を使用します。)</span><span class="sxs-lookup"><span data-stu-id="82c25-118">(Blazor continues to use the *.cshtml* file extension: *Index.cshtml*, *Counter.cshtml*, and *FetchData.cshtml*.)</span></span>

1. <span data-ttu-id="82c25-119">Counter ページ上で **[クリックしてください]** ボタンを選択し、ページを更新することなくカウンターをインクリメントします。</span><span class="sxs-lookup"><span data-stu-id="82c25-119">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="82c25-120">Web ページでカウンターをインクリメントする場合、通常は JavaScript を記述することが必要ですが、Razor Components には C# を使ったより優れた方法が用意されています。</span><span class="sxs-lookup"><span data-stu-id="82c25-120">Incrementing a counter in a webpage normally requires writing JavaScript, but Razor Components provides a better approach using C#.</span></span>

1. <span data-ttu-id="82c25-121">Counter コンポーネントの実装を、*Counter.razor* ファイルで調べます。</span><span class="sxs-lookup"><span data-stu-id="82c25-121">Examine the implementation of the Counter component in the *Counter.razor* file.</span></span>

   <span data-ttu-id="82c25-122">*Components/Pages/Counter.razor* (Blazor では *Pages/Counter.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="82c25-122">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Counter1.razor)]

   <span data-ttu-id="82c25-123">Counter コンポーネントの UI は、HTML を使って定義されています。</span><span class="sxs-lookup"><span data-stu-id="82c25-123">The UI of the Counter component is defined using HTML.</span></span> <span data-ttu-id="82c25-124">動的なレンダリング ロジック (たとえばループ、条件、式) が、[Razor](xref:mvc/views/razor) と呼ばれる埋め込みの C# 構文を使って追加されています。</span><span class="sxs-lookup"><span data-stu-id="82c25-124">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="82c25-125">HTML マークアップと C# のレンダリング ロジックは、ビルド時にコンポーネント クラスに変換されます。</span><span class="sxs-lookup"><span data-stu-id="82c25-125">The HTML markup and C# rendering logic are converted into a component class at build time.</span></span> <span data-ttu-id="82c25-126">生成される .NET クラスの名前はファイル名と同じです。</span><span class="sxs-lookup"><span data-stu-id="82c25-126">The name of the generated .NET class matches the file name.</span></span>

   <span data-ttu-id="82c25-127">コンポーネント クラスのメンバーは、`@functions` ブロック内で定義されています。</span><span class="sxs-lookup"><span data-stu-id="82c25-127">Members of the component class are defined in a `@functions` block.</span></span> <span data-ttu-id="82c25-128">`@functions` ブロック内では、イベント処理や他のコンポーネント ロジックの定義のために、コンポーネントの状態 (プロパティ、フィールド) とメソッドを指定します。</span><span class="sxs-lookup"><span data-stu-id="82c25-128">In the `@functions` block, component state (properties, fields) and methods are specified for event handling or for defining other component logic.</span></span> <span data-ttu-id="82c25-129">これらのメンバーは、コンポーネントのレンダリング ロジックの一部として、またイベントを処理するために使われます。</span><span class="sxs-lookup"><span data-stu-id="82c25-129">These members are then used as part of the component's rendering logic and for handling events.</span></span>

   <span data-ttu-id="82c25-130">**[クリックしてください]** ボタンを選択すると:</span><span class="sxs-lookup"><span data-stu-id="82c25-130">When the **Click me** button is selected:</span></span>

   * <span data-ttu-id="82c25-131">Counter コンポーネントの登録済みの `onclick` ハンドラーが呼び出されます (`IncrementCount` メソッドです)。</span><span class="sxs-lookup"><span data-stu-id="82c25-131">The Counter component's registered `onclick` handler is called (the `IncrementCount` method).</span></span>
   * <span data-ttu-id="82c25-132">Counter コンポーネントによりそのレンダリング ツリーが再生成されます。</span><span class="sxs-lookup"><span data-stu-id="82c25-132">The Counter component regenerates its render tree.</span></span>
   * <span data-ttu-id="82c25-133">新しいレンダリング ツリーが以前のものと比較されます。</span><span class="sxs-lookup"><span data-stu-id="82c25-133">The new render tree is compared to the previous one.</span></span>
   * <span data-ttu-id="82c25-134">ドキュメント オブジェクト モデル (DOM) に対する変更のみが適用されます。</span><span class="sxs-lookup"><span data-stu-id="82c25-134">Only modifications to the Document Object Model (DOM) are applied.</span></span> <span data-ttu-id="82c25-135">表示されている数が更新されます。</span><span class="sxs-lookup"><span data-stu-id="82c25-135">The displayed count is updated.</span></span>

1. <span data-ttu-id="82c25-136">Counter コンポーネントの C# のロジックを変更して、カウントが 1 ではなく 2 ずつインクリメントされるようにします。</span><span class="sxs-lookup"><span data-stu-id="82c25-136">Modify the C# logic of the Counter component to make the count increment by two instead of one.</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. <span data-ttu-id="82c25-137">アプリをリビルドして実行し、変更を確認します。</span><span class="sxs-lookup"><span data-stu-id="82c25-137">Rebuild and run the app to see the changes.</span></span> <span data-ttu-id="82c25-138">**[クリックしてください]** ボタンを選択すると、カウンターは 2 ずつインクリメントされます。</span><span class="sxs-lookup"><span data-stu-id="82c25-138">Select the **Click me** button, and the counter increments by two.</span></span>

## <a name="use-components"></a><span data-ttu-id="82c25-139">コンポーネントを使う</span><span class="sxs-lookup"><span data-stu-id="82c25-139">Use components</span></span>

<span data-ttu-id="82c25-140">HTML のような構文を使用して、別のコンポーネントにコンポーネントを含めます。</span><span class="sxs-lookup"><span data-stu-id="82c25-140">Include a component into another component using an HTML-like syntax.</span></span>

1. <span data-ttu-id="82c25-141">Index コンポーネントに `<Counter />` 要素を追加することで、アプリの Index (ホーム) コンポーネントにカウンター コンポーネントを追加します。</span><span class="sxs-lookup"><span data-stu-id="82c25-141">Add the Counter component to the app's Index (Home) component by adding a `<Counter />` element to the Index component.</span></span>

   <span data-ttu-id="82c25-142">このエクスペリエンスのために Blazor を使っている場合は、Survey Prompt コンポーネント (`<SurveyPrompt>` 要素) が Index コンポーネントに含まれています。</span><span class="sxs-lookup"><span data-stu-id="82c25-142">If you're using Blazor for this experience, a Survey Prompt component (`<SurveyPrompt>` element) is in the Index component.</span></span> <span data-ttu-id="82c25-143">`<SurveyPrompt>` 要素を `<Counter>` 要素に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="82c25-143">Replace the `<SurveyPrompt>` element with the `<Counter>` element.</span></span>

   <span data-ttu-id="82c25-144">*Components/Pages/Index.razor* (Blazor では *Pages/Index.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="82c25-144">*Components/Pages/Index.razor* (*Pages/Index.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Index.razor?highlight=7)]

1. <span data-ttu-id="82c25-145">アプリケーションをリビルドして実行します。</span><span class="sxs-lookup"><span data-stu-id="82c25-145">Rebuild and run the app.</span></span> <span data-ttu-id="82c25-146">ホーム ページに、独自のカウンターがあります。</span><span class="sxs-lookup"><span data-stu-id="82c25-146">The Home page has its own counter.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="82c25-147">コンポーネントのパラメーター</span><span class="sxs-lookup"><span data-stu-id="82c25-147">Component parameters</span></span>

<span data-ttu-id="82c25-148">コンポーネントにパラメーターを持たせることもできます。</span><span class="sxs-lookup"><span data-stu-id="82c25-148">Components can also have parameters.</span></span> <span data-ttu-id="82c25-149">コンポーネントのパラメーターは、`[Parameter]` で修飾されたコンポーネント クラス上の、パブリックでないプロパティを使って定義します。</span><span class="sxs-lookup"><span data-stu-id="82c25-149">Component parameters are defined using non-public properties on the component class decorated with `[Parameter]`.</span></span> <span data-ttu-id="82c25-150">マークアップ内でコンポーネントの引数を指定するには、属性を使います。</span><span class="sxs-lookup"><span data-stu-id="82c25-150">Use attributes to specify arguments for a component in markup.</span></span>

1. <span data-ttu-id="82c25-151">コンポーネントの `@functions` に関する C# コードを更新します。</span><span class="sxs-lookup"><span data-stu-id="82c25-151">Update the component's `@functions` C# code:</span></span>

   * <span data-ttu-id="82c25-152">`[Parameter]` 属性で修飾された `IncrementAmount` プロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="82c25-152">Add a `IncrementAmount` property decorated with the `[Parameter]` attribute.</span></span>
   * <span data-ttu-id="82c25-153">`currentCount` の値を増やすときに `IncrementAmount` を使うように `IncrementCount` メソッドを変更します。</span><span class="sxs-lookup"><span data-stu-id="82c25-153">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

   <span data-ttu-id="82c25-154">*Components/Pages/Counter.razor* (Blazor では *Pages/Counter.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="82c25-154">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/Components/Pages/Counter.razor?highlight=12,16)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. <span data-ttu-id="82c25-155">属性を使って、Home コンポーネントの `<Counter>` 要素に `IncrementAmount` パラメーターを指定します。</span><span class="sxs-lookup"><span data-stu-id="82c25-155">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span> <span data-ttu-id="82c25-156">カウンターを 10 ずつインクリメントするように値を設定します。</span><span class="sxs-lookup"><span data-stu-id="82c25-156">Set the value to increment the counter by ten.</span></span>

   <span data-ttu-id="82c25-157">*Components/Pages/Index.razor* (Blazor では *Pages/Index.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="82c25-157">*Components/Pages/Index.razor* (*Pages/Index.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/Components/Pages/Index.razor?highlight=7)]

1. <span data-ttu-id="82c25-158">ホーム ページを再度読み込みます。</span><span class="sxs-lookup"><span data-stu-id="82c25-158">Reload the Home page.</span></span> <span data-ttu-id="82c25-159">カウンターは、**[クリックしてください]** ボタンを選択するたびに 10 ずつインクリメントされます。</span><span class="sxs-lookup"><span data-stu-id="82c25-159">The counter increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="82c25-160">Counter ページ上のカウンターは 1 ずつインクリメントされます。</span><span class="sxs-lookup"><span data-stu-id="82c25-160">The counter on the Counter page increments by one.</span></span>

## <a name="route-to-components"></a><span data-ttu-id="82c25-161">コンポーネントにルーティングする</span><span class="sxs-lookup"><span data-stu-id="82c25-161">Route to components</span></span>

<span data-ttu-id="82c25-162">*Counter.razor* ファイルの上部にある `@page` ディレクティブは、このコンポーネントがルーティング エンドポイントであることを指定しています。</span><span class="sxs-lookup"><span data-stu-id="82c25-162">The `@page` directive at the top of the *Counter.razor* file specifies that this component is a routing endpoint.</span></span> <span data-ttu-id="82c25-163">Counter コンポーネントによって `/Counter` に送信された要求が処理されます。</span><span class="sxs-lookup"><span data-stu-id="82c25-163">The Counter component handles requests sent to `/Counter`.</span></span> <span data-ttu-id="82c25-164">`@page` ディレクティブがない場合、ルーティングされた要求はコンポーネントで処理されませんが、このコンポーネントは引き続き他のコンポーネントで使えます。</span><span class="sxs-lookup"><span data-stu-id="82c25-164">Without the `@page` directive, the component doesn't handle routed requests, but the component can still be used by other components.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="82c25-165">依存関係の挿入</span><span class="sxs-lookup"><span data-stu-id="82c25-165">Dependency injection</span></span>

<span data-ttu-id="82c25-166">アプリのサービス コンテナーに登録されたサービスは、[依存関係の挿入 (DI)](xref:fundamentals/dependency-injection) を介してコンポーネントから使用できます。</span><span class="sxs-lookup"><span data-stu-id="82c25-166">Services registered in the app's service container are available to components via [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="82c25-167">`@inject` ディレクティブを使ってサービスをコンポーネントに挿入します。</span><span class="sxs-lookup"><span data-stu-id="82c25-167">Inject services into a component using the `@inject` directive.</span></span>

<span data-ttu-id="82c25-168">サンプル アプリで FetchData コンポーネントのディレクティブを調べます。</span><span class="sxs-lookup"><span data-stu-id="82c25-168">Examine the directives of the FetchData component in the sample app.</span></span>

<span data-ttu-id="82c25-169">Razor コンポーネントのサンプル アプリで、`WeatherForecastService` サービスは[シングルトン](xref:fundamentals/dependency-injection#service-lifetimes)として登録されているため、このサービスの 1 つのインスタンスをアプリ全体で使用できます。</span><span class="sxs-lookup"><span data-stu-id="82c25-169">In the Razor Components sample app, the `WeatherForecastService` service is registered as a [singleton](xref:fundamentals/dependency-injection#service-lifetimes), so one instance of the service is available throughout the app.</span></span> <span data-ttu-id="82c25-170">コンポーネントに `WeatherForecastService` サービスのインスタンスを挿入するために、`@inject` ディレクティブが使われています。</span><span class="sxs-lookup"><span data-stu-id="82c25-170">The `@inject` directive is used to inject the instance of the `WeatherForecastService` service into the component.</span></span>

<span data-ttu-id="82c25-171">*Components/Pages/FetchData.razor*: </span><span class="sxs-lookup"><span data-stu-id="82c25-171">*Components/Pages/FetchData.razor*:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

<span data-ttu-id="82c25-172">FetchData コンポーネントでは、`ForecastService` のような挿入されたサービスを使って、`WeatherForecast` オブジェクトの配列を取得します。</span><span class="sxs-lookup"><span data-stu-id="82c25-172">The FetchData component uses the injected service, as `ForecastService`, to retrieve an array of `WeatherForecast` objects:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

<span data-ttu-id="82c25-173">Blazor バージョンのサンプル アプリでは、*wwwroot/sample-data* フォルダー内の *weather.json* から天気予報データを取得するために、`HttpClient` が挿入されています。</span><span class="sxs-lookup"><span data-stu-id="82c25-173">In the Blazor version of the sample app, `HttpClient` is injected to obtain weather forecast data from the *weather.json* file in the *wwwroot/sample-data* folder:</span></span>

<span data-ttu-id="82c25-174">*Pages/FetchData.cshtml*: </span><span class="sxs-lookup"><span data-stu-id="82c25-174">*Pages/FetchData.cshtml*:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData1.cshtml?highlight=7)]

<span data-ttu-id="82c25-175">両方のサンプル アプリで、各 forecast 変数を気象データのテーブルの行としてレンダリングするために、[@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) ループが使われています。</span><span class="sxs-lookup"><span data-stu-id="82c25-175">In both sample apps, a [@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) loop is used to render each forecast instance as a row in the table of weather data:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]

## <a name="build-a-todo-list"></a><span data-ttu-id="82c25-176">Todo リストを構築する</span><span class="sxs-lookup"><span data-stu-id="82c25-176">Build a todo list</span></span>

<span data-ttu-id="82c25-177">シンプルな ToDo リストを実装したアプリに新しいコンポーネントを追加します。</span><span class="sxs-lookup"><span data-stu-id="82c25-177">Add a new component to the app that implements a simple todo list.</span></span>

1. <span data-ttu-id="82c25-178">サンプル アプリに空のファイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="82c25-178">Add an empty file to the sample app:</span></span>

   * <span data-ttu-id="82c25-179">Razor コンポーネントのエクスペリエンスのために、*Todo.razor* ファイルを *Components/Pages* フォルダーに追加します。</span><span class="sxs-lookup"><span data-stu-id="82c25-179">For the Razor Components experience, add a *Todo.razor* file to the *Components/Pages* folder.</span></span>
   * <span data-ttu-id="82c25-180">Blazor のエクスペリエンスのために、*Todo.cshtml* ファイルを *Pages* フォルダーに追加します。</span><span class="sxs-lookup"><span data-stu-id="82c25-180">For the Blazor experience, add a *Todo.cshtml* file to the *Pages* folder.</span></span>

1. <span data-ttu-id="82c25-181">コンポーネントに最初のマークアップを指定します。</span><span class="sxs-lookup"><span data-stu-id="82c25-181">Provide the initial markup for the component:</span></span>

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. <span data-ttu-id="82c25-182">ナビゲーション バーに Todo コンポーネントを追加します。</span><span class="sxs-lookup"><span data-stu-id="82c25-182">Add the Todo component to the navigation bar.</span></span>

   <span data-ttu-id="82c25-183">NavMenu コンポーネント (*Components/Shared/NavMenu.razor* または Blazor では *Shared/NavMenu.cshtml*) は、アプリのレイアウトで使用されます。</span><span class="sxs-lookup"><span data-stu-id="82c25-183">The NavMenu component (*Components/Shared/NavMenu.razor* or *Shared/NavMenu.cshtml* in Blazor) is used in the app's layout.</span></span> <span data-ttu-id="82c25-184">レイアウトは、アプリ内でのコンテンツの重複を回避するために使うコンポーネントです。</span><span class="sxs-lookup"><span data-stu-id="82c25-184">Layouts are components that allow you to avoid duplication of content in the app.</span></span> <span data-ttu-id="82c25-185">詳細については、「<xref:razor-components/layouts>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="82c25-185">For more information, see <xref:razor-components/layouts>.</span></span>

   <span data-ttu-id="82c25-186">*Components/Shared/NavMenu.razor* (Blazor では *Shared/NavMenu.cshtml*) ファイルで、以下のリスト アイテムのマークアップを既存のリスト アイテムの下に追加して、Todo コンポーネント用の `<NavLink>` を追加します。</span><span class="sxs-lookup"><span data-stu-id="82c25-186">Add a `<NavLink>` for the Todo component by adding the following list item markup below the existing list items in the *Components/Shared/NavMenu.razor* (*Shared/NavMenu.cshtml* in Blazor) file:</span></span>

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. <span data-ttu-id="82c25-187">アプリケーションをリビルドして実行します。</span><span class="sxs-lookup"><span data-stu-id="82c25-187">Rebuild and run the app.</span></span> <span data-ttu-id="82c25-188">新しい Todo ページに移動して、Todo コンポーネントへのリンクが機能することを確認します。</span><span class="sxs-lookup"><span data-stu-id="82c25-188">Visit the new Todo page to confirm that the link to the Todo component works.</span></span>

1. <span data-ttu-id="82c25-189">Todo アイテムを表すクラスを保持するために、プロジェクトのルートに *TodoItem.cs* ファイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="82c25-189">Add a *TodoItem.cs* file to the root of the project to hold a class that represents a todo item.</span></span> <span data-ttu-id="82c25-190">`TodoItem` クラス用に次の C# コードを使います。</span><span class="sxs-lookup"><span data-stu-id="82c25-190">Use the following C# code for the `TodoItem` class:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/TodoItem.cs)]

1. <span data-ttu-id="82c25-191">Todo コンポーネント (*Components/Pages/Todo.razor* または Blazor では *Pages/Todo.cshtml*) に戻ります。</span><span class="sxs-lookup"><span data-stu-id="82c25-191">Return to the Todo component (*Components/Pages/Todo.razor* or *Pages/Todo.cshtml* in Blazor):</span></span>

   * <span data-ttu-id="82c25-192">Todo 用のフィールドを `@functions` ブロックに追加します。</span><span class="sxs-lookup"><span data-stu-id="82c25-192">Add a field for the todos in an `@functions` block.</span></span> <span data-ttu-id="82c25-193">Todo コンポーネントでは、このフィールドを使って Todo リストの状態を維持します。</span><span class="sxs-lookup"><span data-stu-id="82c25-193">The Todo component uses this field to maintain the state of the todo list.</span></span>
   * <span data-ttu-id="82c25-194">各 ToDo アイテムをリスト アイテムとしてレンダリングするために、順序のないリストのマークアップと `foreach` ループを追加します。</span><span class="sxs-lookup"><span data-stu-id="82c25-194">Add unordered list markup and a `foreach` loop to render each todo item as a list item.</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. <span data-ttu-id="82c25-195">アプリには、リストに ToDo を追加するための UI 要素が必要です。</span><span class="sxs-lookup"><span data-stu-id="82c25-195">The app requires UI elements for adding todos to the list.</span></span> <span data-ttu-id="82c25-196">リストの下に、テキスト入力とボタンを追加します。</span><span class="sxs-lookup"><span data-stu-id="82c25-196">Add a text input and a button below the list:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. <span data-ttu-id="82c25-197">アプリケーションをリビルドして実行します。</span><span class="sxs-lookup"><span data-stu-id="82c25-197">Rebuild and run the app.</span></span> <span data-ttu-id="82c25-198">**[Add todo]\(ToDo の追加\)** ボタンを選択しても何も起こりません。ボタンにイベント ハンドラーが関連付けられていないためです。</span><span class="sxs-lookup"><span data-stu-id="82c25-198">When the **Add todo** button is selected, nothing happens because an event handler isn't wired up to the button.</span></span>

1. <span data-ttu-id="82c25-199">Todo コンポーネントに `AddTodo` メソッドを追加し、`onclick` 属性を使ってボタンのクリック用にこれを登録します。</span><span class="sxs-lookup"><span data-stu-id="82c25-199">Add an `AddTodo` method to the Todo component and register it for button clicks using the `onclick` attribute:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

   <span data-ttu-id="82c25-200">ボタンを選択すると C# のメソッド `AddTodo` が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="82c25-200">The `AddTodo` C# method is called when the button is selected.</span></span>

1. <span data-ttu-id="82c25-201">新しい Todo アイテムのタイトルを取得するために、文字列フィールド `newTodo` を追加し、`bind` 属性を使ってこれをテキスト入力の値とバインドします。</span><span class="sxs-lookup"><span data-stu-id="82c25-201">To get the title of the new todo item, add a `newTodo` string field and bind it to the value of the text input using the `bind` attribute:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" bind="@newTodo">
   ```

1. <span data-ttu-id="82c25-202">指定したタイトルを備えた `TodoItem` をリストに追加するように、`AddTodo` メソッドを更新します。</span><span class="sxs-lookup"><span data-stu-id="82c25-202">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="82c25-203">`newTodo` を空の文字列に設定して、テキスト入力の値をクリアします。</span><span class="sxs-lookup"><span data-stu-id="82c25-203">Clear the value of the text input by setting `newTodo` to an empty string:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. <span data-ttu-id="82c25-204">アプリケーションをリビルドして実行します。</span><span class="sxs-lookup"><span data-stu-id="82c25-204">Rebuild and run the app.</span></span> <span data-ttu-id="82c25-205">Todo リストに Todo をいくつか追加して、新しいコードをテストします。</span><span class="sxs-lookup"><span data-stu-id="82c25-205">Add some todos to the todo list to test the new code.</span></span>

1. <span data-ttu-id="82c25-206">各 Todo アイテムのタイトルのテキストは編集可能にすることができます。また、チェック ボックスはユーザーが完了したアイテムを追跡するのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="82c25-206">The title text for each todo item can be made editable and a check box can help the user keep track of completed items.</span></span> <span data-ttu-id="82c25-207">各 Todo アイテムにチェック ボックス入力を追加し、その値を `IsDone` プロパティにバインドします。</span><span class="sxs-lookup"><span data-stu-id="82c25-207">Add a check box input for each todo item and bind its value to the `IsDone` property.</span></span> <span data-ttu-id="82c25-208">`@todo.Title` を、`@todo.Title` にバインドされた `<input>` 要素に変更します。</span><span class="sxs-lookup"><span data-stu-id="82c25-208">Change `@todo.Title` to an `<input>` element bound to `@todo.Title`:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. <span data-ttu-id="82c25-209">それらの値がバインドされていることを確認するために、`<h1>` ヘッダーを更新して、完了していない (`IsDone` が `false` の) Todo アイテムの数のカウントを表示するようにします。</span><span class="sxs-lookup"><span data-stu-id="82c25-209">To verify that these values are bound, update the `<h1>` header to show a count of the number of todo items that aren't complete (`IsDone` is `false`).</span></span>

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. <span data-ttu-id="82c25-210">完了した Todo コンポーネント (*Components/Pages/Todo.razor* または Blazor では *Pages/Todo.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="82c25-210">The completed Todo component (*Components/Pages/Todo.razor* or *Pages/Todo.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/Components/Pages/Todo.razor)]

1. <span data-ttu-id="82c25-211">アプリケーションをリビルドして実行します。</span><span class="sxs-lookup"><span data-stu-id="82c25-211">Rebuild and run the app.</span></span> <span data-ttu-id="82c25-212">Todo アイテムを追加して新しいコードをテストします。</span><span class="sxs-lookup"><span data-stu-id="82c25-212">Add todo items to test the new code.</span></span>

## <a name="publish-and-deploy-the-app"></a><span data-ttu-id="82c25-213">アプリを発行および配置する</span><span class="sxs-lookup"><span data-stu-id="82c25-213">Publish and deploy the app</span></span>

<span data-ttu-id="82c25-214">アプリの発行方法については、<xref:host-and-deploy/razor-components-blazor/index> をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="82c25-214">To publish the app, see <xref:host-and-deploy/razor-components-blazor/index>.</span></span>
