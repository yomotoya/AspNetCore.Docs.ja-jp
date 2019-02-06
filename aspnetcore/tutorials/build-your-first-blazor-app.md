---
title: 最初の Blazor アプリを構築する
author: guardrex
description: Blazor アプリを段階的に構築して、Razor Components フレームワークの基本的な機能について簡単に学習します。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: tutorials/first-blazor-app
ms.openlocfilehash: 99396e200eb121524abccc20a7d461062c94ecc7
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/02/2019
ms.locfileid: "55668032"
---
# <a name="build-your-first-blazor-app"></a><span data-ttu-id="6d1a1-103">最初の Blazor アプリを構築する</span><span class="sxs-lookup"><span data-stu-id="6d1a1-103">Build your first Blazor app</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="6d1a1-104">このチュートリアルでは、Blazor アプリを段階的に構築して、Razor Components フレームワークの基本的な機能について簡単に学習します。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-104">In this tutorial, you build a Blazor app step-by-step and quickly learn the basic features of the Razor Components framework.</span></span>

<span data-ttu-id="6d1a1-105">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-blazor-app/samples/)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-blazor-app/samples/) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="6d1a1-106">前提条件については、[作業開始](xref:razor-components/get-started)に関するトピックをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-106">See the [Get started](xref:razor-components/get-started) topic for prerequisites.</span></span>

<span data-ttu-id="6d1a1-107">Visual Studio でプロジェクトを作成するには:</span><span class="sxs-lookup"><span data-stu-id="6d1a1-107">To create the project in Visual Studio:</span></span>

1. <span data-ttu-id="6d1a1-108">**[ファイル]** > **[新規作成]** > **[プロジェクト]** を順に選択します。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-108">Select **File** > **New** > **Project**.</span></span> <span data-ttu-id="6d1a1-109">**[Web]** > **[ASP.NET Core Web アプリケーション]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-109">Select **Web** > **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="6d1a1-110">**[名前]** フィールドでプロジェクトに "BlazorApp1" という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-110">Name the project "BlazorApp1" in the **Name** field.</span></span> <span data-ttu-id="6d1a1-111">**[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-111">Select **OK**.</span></span>

    ![新しい ASP.NET Core プロジェクト](build-your-first-blazor-app/_static/new-aspnet-core-project.png)

1. <span data-ttu-id="6d1a1-113">**[新しい ASP.NET Core Web アプリケーション]** ダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-113">The **New ASP.NET Core Web Application** dialog appears.</span></span> <span data-ttu-id="6d1a1-114">上部で **[.NET Core]** が選択されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-114">Make sure **.NET Core** is selected at the top.</span></span> <span data-ttu-id="6d1a1-115">**[ASP.NET Core 2.1]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-115">Select **ASP.NET Core 2.1**.</span></span> <span data-ttu-id="6d1a1-116">**[Blazor]** テンプレートを選択して **[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-116">Choose the **Blazor** template and select **OK**.</span></span>

    ![新しい Blazor アプリのダイアログ](build-your-first-blazor-app/_static/new-blazor-app-dialog.png)

1. <span data-ttu-id="6d1a1-118">プロジェクトが作成されたら、**Ctrl キーを押しながら F5 キー**を押して、"*デバッガーなしで*" アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-118">Once the project is created, press **Ctrl-F5** to run the app *without the debugger*.</span></span> <span data-ttu-id="6d1a1-119">デバッガーを使った実行 (**F5 キー**) は、現時点ではでサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-119">Running with the debugger (**F5**) isn't supported at this time.</span></span>

> [!NOTE]
> <span data-ttu-id="6d1a1-120">Visual Studio を使わない場合は、Windows、macOS、または Linux 上のコマンド プロンプトで Blazor アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-120">If not using Visual Studio, create the Blazor app at a command prompt on Windows, macOS, or Linux:</span></span>
>
> ```console
> dotnet new --install "Microsoft.AspNetCore.Blazor.Templates"
> dotnet new blazor -o BlazorApp1
> cd BlazorApp1
> dotnet run
> ```
>
> <span data-ttu-id="6d1a1-121">`dotnet run` の実行後にコンソール ウィンドウの出力に表示される localhost アドレスとポートを使って、アプリに移動します。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-121">Navigate to the app using the localhost address and port provided in the console window output after `dotnet run` is executed.</span></span> <span data-ttu-id="6d1a1-122">アプリをシャット ダウンするには、コンソール ウィンドウで **Ctrl キーを押しながら C キー**を押します。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-122">Use **Ctrl-C** in the console window to shutdown the app.</span></span>

<span data-ttu-id="6d1a1-123">ブラウザーで Blazor アプリが実行されます。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-123">The Blazor app runs in the browser:</span></span>

![Blazor アプリのホーム ページ](https://user-images.githubusercontent.com/1874516/39509497-5515c3ea-4d9b-11e8-887f-019ea4fdb3ee.png)

## <a name="build-components"></a><span data-ttu-id="6d1a1-125">コンポーネントを構築する</span><span class="sxs-lookup"><span data-stu-id="6d1a1-125">Build components</span></span>

1. <span data-ttu-id="6d1a1-126">アプリの 3 つのページにそれぞれ移動します。ホーム、カウンター、データのフェッチです。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-126">Browse to each of the app's three pages: Home, Counter, and Fetch data.</span></span>

    <span data-ttu-id="6d1a1-127">これらの 3 つのページは、*Pages* フォルダーにある 3 つの Razor ファイルによって実装されています。*Index.cshtml*、*Counter.cshtml*、*FetchData.cshtml* です。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-127">These three pages are implemented by the three Razor files in the *Pages* folder: *Index.cshtml*, *Counter.cshtml*, and *FetchData.cshtml*.</span></span> <span data-ttu-id="6d1a1-128">これらの 3 つのファイルのそれぞれが、ブラウザーでクライアント側でコンパイルされて実行されるコンポーネントを実装しています。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-128">Each of these files implements a component that's compiled and executed client-side in the browser.</span></span>

1. <span data-ttu-id="6d1a1-129">カウンター ページ上のボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-129">Select the button on the Counter page.</span></span>

    ![Blazor アプリのホーム ページ](https://user-images.githubusercontent.com/1874516/39509525-6e367c66-4d9b-11e8-9978-e52a9750c34b.png)

    <span data-ttu-id="6d1a1-131">ボタンを選択するたびに、ページを更新することなくカウンターがインクリメントされます。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-131">Each time the button is selected, the counter is incremented without a page refresh.</span></span> <span data-ttu-id="6d1a1-132">通常、この種のクライアント側の動作は JavaScript で処理されますが、ここではそれが `Counter` コンポーネントによって C# と .NET で実装されています。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-132">Normally, this kind of client-side behavior is handled in JavaScript; but here, it's implemented in C# and .NET by the `Counter` component.</span></span>

1. <span data-ttu-id="6d1a1-133">`Counter` コンポーネントの実装を、*Counter.cshtml* ファイルで見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-133">Take a look at the implementation of the `Counter` component in the *Counter.cshtml* file:</span></span>

    ```cshtml
    @page "/counter"

    <h1>Counter</h1>

    <p>Current count: @currentCount</p>

    <button class="btn btn-primary" onclick="@IncrementCount">Click me</button>

    @functions {
        int currentCount = 0;

        void IncrementCount()
        {
            currentCount++;
        }
    }
    ```

    <span data-ttu-id="6d1a1-134">`Counter` コンポーネントの UI は通常の HTML を使って定義されています。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-134">The UI for the `Counter` component is defined using normal HTML.</span></span> <span data-ttu-id="6d1a1-135">動的なレンダリング ロジック (たとえばループ、条件、式) が、[Razor](https://docs.microsoft.com/aspnet/core/mvc/views/razor) と呼ばれる埋め込みの C# 構文を使って追加されています。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-135">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](https://docs.microsoft.com/aspnet/core/mvc/views/razor).</span></span> <span data-ttu-id="6d1a1-136">HTML マークアップと C# のレンダリング ロジックは、ビルド時にコンポーネント クラスに変換されます。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-136">The HTML markup and C# rendering logic are converted into a component class at build time.</span></span> <span data-ttu-id="6d1a1-137">生成される .NET クラスの名前はファイル名と同じです。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-137">The name of the generated .NET class matches the name of the file.</span></span>

    <span data-ttu-id="6d1a1-138">コンポーネント クラスのメンバーは、`@functions` ブロック内で定義されています。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-138">Members of the component class are defined in a `@functions` block.</span></span> <span data-ttu-id="6d1a1-139">`@functions` ブロック内では、イベント処理や他のコンポーネント ロジックの定義のために、コンポーネントの状態 (プロパティ、フィールド) とメソッドを指定できます。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-139">In the `@functions` block, component state (properties, fields) and methods can be specified for event handling or for defining other component logic.</span></span> <span data-ttu-id="6d1a1-140">これらのメンバーは、コンポーネントのレンダリング ロジックの一部として、またイベントを処理するために使えます。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-140">These members can then be used as part of the component's rendering logic and for handling events.</span></span>

    <span data-ttu-id="6d1a1-141">ボタンが選択されると、`Counter` コンポーネントの登録済みの `onclick` ハンドラー (`IncrementCount` メソッド) が呼び出され、`Counter` コンポーネンによりそのレンダリング ツリーが再生成されます。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-141">When the button is selected, the `Counter` component's registered `onclick` handler is called (the `IncrementCount` method) and the `Counter` component regenerates its render tree.</span></span> <span data-ttu-id="6d1a1-142">Blazor によって新旧のレンダリング ツリーが比較され、ブラウザーのドキュメント オブジェクト モデル (DOM) に変更が適用されます。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-142">Blazor compares the new render tree against the previous one and applies any modifications to the browser Document Object Model (DOM).</span></span> <span data-ttu-id="6d1a1-143">表示されている数が更新されます。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-143">The displayed count is updated.</span></span>

1. <span data-ttu-id="6d1a1-144">`Counter` コンポーネントのマークアップを更新して、最上位のヘッダーをより "*魅力的に*" します。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-144">Update the markup for the `Counter` component to make the top-level header more *exciting*.</span></span>

    ```cshtml
    @page "/counter"
    <h1><em>Counter!!</em></h1>
    ```

1. <span data-ttu-id="6d1a1-145">また、`Counter` コンポーネントの C# のロジックを変更して、カウントが 1 ではなく 2 ずつインクリメントされるようにします。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-145">Also modify the C# logic of the `Counter` component to make the count increment by two instead of one.</span></span>

    ```cshtml
    @functions {
        int currentCount = 0;

        void IncrementCount()
        {
            currentCount += 2;
        }
    }
    ```

1. <span data-ttu-id="6d1a1-146">ブラウザーでカウンター ページを更新して、変更内容を確認します。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-146">Refresh the counter page in the browser to see the changes.</span></span>

    ![魅力的なカウンター](https://user-images.githubusercontent.com/1874516/39509668-e8949a92-4d9b-11e8-91e9-b6a494695d92.png)

## <a name="use-components"></a><span data-ttu-id="6d1a1-148">コンポーネントを使う</span><span class="sxs-lookup"><span data-stu-id="6d1a1-148">Use components</span></span>

<span data-ttu-id="6d1a1-149">コンポーネントを定義したら、そのコンポーネントを使って他のコンポーネントを実装できます。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-149">After a component is defined, the component can be used to implement other components.</span></span> <span data-ttu-id="6d1a1-150">コンポーネントを使うためのマークアップは、そのコンポーネントの種類をタグ名とする HTML タグのようになります。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-150">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

1. <span data-ttu-id="6d1a1-151">アプリのホーム ページ (*Index.cshtml*) に `Counter` コンポーネントを追加します。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-151">Add a `Counter` component to the Home page of the app (*Index.cshtml*).</span></span>

    ```cshtml
    @page "/"

    <h1>Hello, world!</h1>

    Welcome to your new app.

    <SurveyPrompt Title="How is Blazor working for you?" />

    <Counter />
    ```

1. <span data-ttu-id="6d1a1-152">ブラウザーでホーム ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-152">Refresh the home page in the browser.</span></span> <span data-ttu-id="6d1a1-153">ホーム ページ上にある `Counter` コンポーネントの別のインスタンスに注意してください。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-153">Note the separate instance of the `Counter` component on the Home page.</span></span>

    ![カウンター付き Blazor ホーム ページ](https://user-images.githubusercontent.com/1874516/39509718-224483f6-4d9c-11e8-9030-b4c7228d669d.png)

## <a name="component-parameters"></a><span data-ttu-id="6d1a1-155">コンポーネントのパラメーター</span><span class="sxs-lookup"><span data-stu-id="6d1a1-155">Component parameters</span></span>

<span data-ttu-id="6d1a1-156">コンポーネントにパラメーターを持たせることもできます。これは、`[Parameter]` で修飾されたコンポーネント クラスのプライベート プロパティを使って定義します。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-156">Components can also have parameters, which are defined using private properties on the component class decorated with `[Parameter]`.</span></span> <span data-ttu-id="6d1a1-157">マークアップ内でコンポーネントの引数を指定するには、属性を使います。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-157">Use attributes to specify arguments for a component in markup.</span></span> 

1. <span data-ttu-id="6d1a1-158">既定値が 1 の `IncrementAmount` パラメーターを持つように `Counter` コンポーネントを更新します。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-158">Update the `Counter` component to have an `IncrementAmount` parameter that defaults to 1.</span></span>

    ```cshtml
    @functions {
        int currentCount = 0;

        [Parameter]
        private int IncrementAmount { get; set; } = 1;

        void IncrementCount()
        {
            currentCount += IncrementAmount;
        }
    }
    ```

    > [!NOTE]
    > <span data-ttu-id="6d1a1-159">Visual Studio から、`para` スニペットを使ってコンポーネントのパラメーターを簡単に追加できます。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-159">From Visual Studio, you can quickly add a component parameter by using the `para` snippet.</span></span> <span data-ttu-id="6d1a1-160">「`para`」と入力した後、`Tab` キーを 2 回押します。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-160">Type `para` and then press the `Tab` key twice.</span></span>

1. <span data-ttu-id="6d1a1-161">ホーム ページ (*Index.cshtml*) 上で、コンポーネントのプロパティ `IncrementCount` の名前と一致する属性を設定して、`Counter` の増分量を 10 に変更します。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-161">On the Home page (*Index.cshtml*), change the increment amount for the `Counter` to 10 by setting an attribute that matches the name of the component's property for `IncrementCount`.</span></span>

    ```cshtml
    <Counter IncrementAmount="10" />
    ```

1. <span data-ttu-id="6d1a1-162">ページを再度読み込みます。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-162">Reload the page.</span></span>

    <span data-ttu-id="6d1a1-163">これで、ホーム ページ上のカウンターは 10 ずつインクリメントされるようになりましたが、カウンター ページ上のカウンターは 1 ずつインクリメントされるままです。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-163">The counter on the Home page now increments by 10, while the counter on the Counter page still increments by 1.</span></span>

    ![Blazor 10 ずつのカウント](https://user-images.githubusercontent.com/1874516/39509798-618f0720-4d9c-11e8-9125-3d4c634dff46.png)

## <a name="route-to-components"></a><span data-ttu-id="6d1a1-165">コンポーネントにルーティングする</span><span class="sxs-lookup"><span data-stu-id="6d1a1-165">Route to components</span></span>

<span data-ttu-id="6d1a1-166">*Counter.cshtml* ファイルの上部にある `@page` ディレクティブは、このコンポーネントが要求をルーティングできるページであることを指定しています。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-166">The `@page` directive at the top of the *Counter.cshtml* file specifies that this component is a page to which requests can be routed.</span></span> <span data-ttu-id="6d1a1-167">具体的には、`Counter` コンポーネントによって `/Counter` に送信された要求が処理されます。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-167">Specifically, the `Counter` component handles requests sent to `/Counter`.</span></span> <span data-ttu-id="6d1a1-168">`@page` ディレクティブがない場合、ルーティングされた要求はコンポーネントで処理されませんが、このコンポーネントは引き続き他のコンポーネントで使えます。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-168">Without the `@page` directive, the component wouldn't handle routed requests, but the component could still be used by other components.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="6d1a1-169">依存関係の挿入</span><span class="sxs-lookup"><span data-stu-id="6d1a1-169">Dependency injection</span></span>

<span data-ttu-id="6d1a1-170">アプリのサービス プロバイダーに登録されたサービスは、[依存関係の挿入 (DI)](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) を介してコンポーネントから使用できます。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-170">Services registered with the app's service provider are available to components via [dependency injection (DI)](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection).</span></span> <span data-ttu-id="6d1a1-171">サービスは、`@inject` ディレクティブを使ってコンポーネントに挿入できます。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-171">Services can be injected into a component using the `@inject` directive.</span></span>

<span data-ttu-id="6d1a1-172">`FetchData` コンポーネントの実装を、*FetchData.cshtml* ファイルで見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-172">Take a look at the implementation of the `FetchData` component in *FetchData.cshtml*.</span></span> <span data-ttu-id="6d1a1-173">コンポーネントに [HttpClient](https://docs.microsoft.com/dotnet/api/system.net.http.httpclient) インスタンスを挿入するために、`@inject` ディレクティブが使われています。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-173">The `@inject` directive is used to inject an [HttpClient](https://docs.microsoft.com/dotnet/api/system.net.http.httpclient) instance into the component.</span></span>

```cshtml
@page "/fetchdata"
@inject HttpClient Http
```

<span data-ttu-id="6d1a1-174">`FetchData` コンポーネントでは、挿入された `HttpClient` を使って、コンポーネントの初期化時にサーバーから JSON データが取得されます。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-174">The `FetchData` component uses the injected `HttpClient` to retrieve JSON data from the server when the component is initialized.</span></span> <span data-ttu-id="6d1a1-175">内部の処理では、Blazor のランタイムが提供する `HttpClient` は JavaScript 相互運用を使って実装されていて、要求を送信するために基になるブラウザーの Fetch API を呼び出しています (C# からは、あらゆる JavaScript ライブラリやブラウザー API を呼び出すことが可能です)。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-175">Under the covers, the `HttpClient` provided by the Blazor runtime is implemented using JavaScript interop to call the underlying browser's Fetch API to send the request (from C#, it's possible to call any JavaScript library or browser API).</span></span> <span data-ttu-id="6d1a1-176">取得したデータは、`WeatherForecast` の配列である C# 変数 `forecasts` に逆シリアル化されます。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-176">The retrieved data is deserialized into the `forecasts` C# variable as an array of `WeatherForecast` objects.</span></span>

```cshtml
@functions {
    WeatherForecast[] forecasts;

    protected override async Task OnInitAsync()
    {
        forecasts = await Http.GetJsonAsync<WeatherForecast[]>("/sample-data/weather.json");
    }

    class WeatherForecast
    {
        public DateTime Date { get; set; }
        public int TemperatureC { get; set; }
        public int TemperatureF { get; set; }
        public string Summary { get; set; }
    }
}
```

<span data-ttu-id="6d1a1-177">各 forecast 変数を気象テーブルの行としてレンダリングするために、`@foreach` ループが使われています。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-177">A `@foreach` loop is used to render each forecast instance as a row in the weather table.</span></span>

```cshtml
<table class="table">
    <thead>
        <tr>
            <th>Date</th>
            <th>Temp. (C)</th>
            <th>Temp. (F)</th>
            <th>Summary</th>
        </tr>
    </thead>
    <tbody>
        @foreach (var forecast in forecasts)
        {
            <tr>
                <td>@forecast.Date.ToShortDateString()</td>
                <td>@forecast.TemperatureC</td>
                <td>@forecast.TemperatureF</td>
                <td>@forecast.Summary</td>
            </tr>
        }
    </tbody>
</table>
```

## <a name="build-a-todo-list"></a><span data-ttu-id="6d1a1-178">Todo リストを構築する</span><span class="sxs-lookup"><span data-stu-id="6d1a1-178">Build a todo list</span></span>

<span data-ttu-id="6d1a1-179">シンプルな ToDo リストを実装した新しいページをアプリに追加します。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-179">Add a new page to the app that implements a simple todo list.</span></span>

1. <span data-ttu-id="6d1a1-180">*Pages* フォルダーに、*Todo.cshtml* という名前の空のテキスト ファイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-180">Add an empty text file to the *Pages* folder named *Todo.cshtml*.</span></span>

1. <span data-ttu-id="6d1a1-181">ページの最初のマークアップを指定します。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-181">Provide the initial markup for the page.</span></span>

    ```cshtml
    @page "/todo"

    <h1>Todo</h1>
    ```

1. <span data-ttu-id="6d1a1-182">*Shared/NavMenu.cshtml* を更新して、ナビゲーション バーに ToDo ページを追加します。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-182">Add the Todo page to the navigation bar by updating *Shared/NavMenu.cshtml*.</span></span> <span data-ttu-id="6d1a1-183">以下のリスト アイテムのマークアップを既存のリスト アイテムの下に追加して、ToDo ページ用の `NavLink` を追加します。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-183">Add a `NavLink` for the Todo page by adding the following list item markup below the existing list items.</span></span>

    ```cshtml
    <li class="nav-item px-3">
        <NavLink class="nav-link" href="todo">
            <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
        </NavLink>
    </li>
    ```

1. <span data-ttu-id="6d1a1-184">ブラウザーでアプリを更新します。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-184">Refresh the app in the browser.</span></span> <span data-ttu-id="6d1a1-185">新しい ToDo ページを確認します。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-185">See the new Todo page.</span></span>

    ![Blazor ToDo の開始](https://user-images.githubusercontent.com/1874516/39509907-bb27e77a-4d9c-11e8-91e7-ea1e7c01729e.png)

1. <span data-ttu-id="6d1a1-187">ToDo アイテムを表すクラスを保持するために、プロジェクトのルートに *TodoItem.cs* ファイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-187">Add a *TodoItem.cs* file to the root of the project to hold a class to represent the todo items.</span></span>

1. <span data-ttu-id="6d1a1-188">`TodoItem` クラス用に次の C# コードを使います。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-188">Use the following C# code for the `TodoItem` class.</span></span>

    ```csharp
    public class TodoItem
    {
        public string Title { get; set; }
        public bool IsDone { get; set; }
    }
    ```

1. <span data-ttu-id="6d1a1-189">*Todo.cshtml* 内の `Todo` コンポーネントに戻って、ToDo 用のフィールドを `@functions` ブロックに追加します。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-189">Go back to the `Todo` component in *Todo.cshtml* and add a field for the todos in a `@functions` block.</span></span> <span data-ttu-id="6d1a1-190">`Todo` コンポーネントでは、このフィールドを使って ToDo リストの状態を維持します。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-190">The `Todo` component uses this field to maintain the state of the todo list.</span></span>

    ```cshtml
    @functions {
        IList<TodoItem> todos = new List<TodoItem>();
    }
    ```

1. <span data-ttu-id="6d1a1-191">各 ToDo アイテムをリスト アイテムとしてレンダリングするために、順序のないリストのマークアップと `foreach` ループを追加します。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-191">Add unordered list markup and a `foreach` loop to render each todo item as a list item.</span></span>

    ```cshtml
    @page "/todo"

    <h1>Todo</h1>

    <ul>
        @foreach (var todo in todos)
        {
            <li>@todo.Title</li>
        }
    </ul>
    ```

1. <span data-ttu-id="6d1a1-192">アプリには、リストに ToDo を追加するための UI 要素が必要です。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-192">The app requires UI elements for adding todos to the list.</span></span> <span data-ttu-id="6d1a1-193">リストの下に、テキスト入力とボタンを追加します。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-193">Add a text input and a button below the list.</span></span>

    ```cshtml
    @page "/todo"

    <h1>Todo</h1>

    <ul>
        @foreach (var todo in todos)
        {
            <li>@todo.Title</li>
        }
    </ul>

    <input placeholder="Something todo" />
    <button>Add todo</button>

    @functions {
        IList<TodoItem> todos = new List<TodoItem>();
    }
    ```

1. <span data-ttu-id="6d1a1-194">ブラウザーを更新します。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-194">Refresh the browser.</span></span>

    ![[Add todo]\(ToDo の追加\) ボタン](https://user-images.githubusercontent.com/1874516/39512402-bc88ab46-4da5-11e8-9e3f-87b875b56383.png)

    <span data-ttu-id="6d1a1-196">**[Add todo]\(ToDo の追加\)** ボタンを選択しても何も起こりません。ボタンにイベント ハンドラーが関連付けられていないためです。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-196">Nothing happens when the **Add todo** button is selected because no event handler is wired up to the button.</span></span>

1. <span data-ttu-id="6d1a1-197">`Todo` コンポーネントに `AddTodo` メソッドを追加し、`onclick` 属性を使ってボタンのクリック用にこれを登録します。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-197">Add an `AddTodo` method to the `Todo` component and register it for button clicks using the `onclick` attribute.</span></span>

    ```cshtml
    <input placeholder="Something todo" />
    <button onclick="@AddTodo">Add todo</button>

    @functions {
        IList<TodoItem> todos = new List<TodoItem>();

        void AddTodo()
        {
            // Todo: Add the todo
        }
    }
    ```

    <span data-ttu-id="6d1a1-198">ボタンが選択されるたびに、C# のメソッド `AddTodo` が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-198">The `AddTodo` C# method is called every time the button is selected.</span></span>

1. <span data-ttu-id="6d1a1-199">新しい ToDo アイテムのタイトルを取得するために、文字列フィールド `newTodo` を追加し、`bind` 属性を使ってこれをテキスト入力の値とバインドします。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-199">To get the title of the new todo item, add a `newTodo` string field and bind it to the value of the text input using the `bind` attribute.</span></span>

    ```csharp
    IList<TodoItem> todos = new List<TodoItem>();
    string newTodo;
    ```

    ```cshtml
    <input placeholder="Something todo" bind="@newTodo" />
    ```

1. <span data-ttu-id="6d1a1-200">指定したタイトルを備えた `TodoItem` をリストに追加するように、`AddTodo` メソッドを更新します。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-200">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="6d1a1-201">`newTodo` を空の文字列に設定して、テキスト入力の値をクリアすることを忘れないでください。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-201">Don't forget to clear the value of the text input by setting `newTodo` to an empty string.</span></span>

    ```cshtml
    @page "/todo"

    <h1>Todo</h1>

    <ul>
        @foreach (var todo in todos)
        {
            <li>@todo.Title</li>
        }
    </ul>

    <input placeholder="Something todo" bind="@newTodo" />
    <button onclick="@AddTodo">Add todo</button>

    @functions {
        IList<TodoItem> todos = new List<TodoItem>();
        string newTodo;

        void AddTodo()
        {
            if (!string.IsNullOrWhiteSpace(newTodo))
            {
                todos.Add(new TodoItem { Title = newTodo });
                newTodo = string.Empty;
            }
        }
    }
    ```

1. <span data-ttu-id="6d1a1-202">ブラウザーを更新します。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-202">Refresh the browser.</span></span> <span data-ttu-id="6d1a1-203">ToDo リストに ToDo をいくつか追加します。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-203">Add some todos to the todo list.</span></span>

    ![ToDo を追加する](https://user-images.githubusercontent.com/1874516/39512531-2d2ff62e-4da6-11e8-8b83-291b0efc821b.png)

1. <span data-ttu-id="6d1a1-205">最後に、ToDo リストにチェック ボックスを追加しましょう。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-205">Lastly, what's a todo list without check boxes?</span></span> <span data-ttu-id="6d1a1-206">各 ToDo アイテムのタイトル テキストも、編集可能にすることができます。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-206">The title text for each todo item can be made editable as well.</span></span> <span data-ttu-id="6d1a1-207">各 ToDo アイテムにチェック ボックス入力とテキスト入力を追加し、それらの値をそれぞれ `Title` および `IsDone` プロパティにバインドします。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-207">Add a check box input and text input for each todo item and bind their values to the `Title` and `IsDone` properties, respectively.</span></span>

    ```cshtml
    <ul>
        @foreach (var todo in todos)
        {
            <li>
                <input type="checkbox" bind="@todo.IsDone" />
                <input bind="@todo.Title" />
            </li>
        }
    </ul>
    ```

1. <span data-ttu-id="6d1a1-208">それらの値がバインドされていることを確認するために、`h1` ヘッダーを更新して、まだ完了していない (`IsDone` が `false` の) ToDo アイテムの数のカウントを表示するようにします。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-208">To verify that these values are bound, update the `h1` header to show a count of the number of todo items that are not yet done (`IsDone` is `false`).</span></span>

    ```cshtml
    <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
    ```

1. <span data-ttu-id="6d1a1-209">完成した `Todo` コンポーネントは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-209">The completed `Todo` component should look like this:</span></span>

    ```cshtml
    @page "/todo"

    <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>

    <ul>
        @foreach (var todo in todos)
        {
            <li>
                <input type="checkbox" bind="@todo.IsDone" />
                <input bind="@todo.Title" />
            </li>
        }
    </ul>

    <input placeholder="Something todo" bind="@newTodo" />
    <button onclick="@AddTodo">Add todo</button>

    @functions {
        IList<TodoItem> todos = new List<TodoItem>();
        string newTodo;

        void AddTodo()
        {
            if (!string.IsNullOrWhiteSpace(newTodo))
            {
                todos.Add(new TodoItem { Title = newTodo });
                newTodo = string.Empty;
            }
        }
    }
    ```

<span data-ttu-id="6d1a1-210">ブラウザーでアプリを更新します。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-210">Refresh the app in the browser.</span></span> <span data-ttu-id="6d1a1-211">ToDo アイテムをいくつか追加してみてください。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-211">Try adding some todo items.</span></span>

![完成した Blazor ToDo リスト](https://user-images.githubusercontent.com/1874516/39512621-83406508-4da6-11e8-8244-5bae2ac6b22d.png)

## <a name="publish-and-deploy"></a><span data-ttu-id="6d1a1-213">発行と配置</span><span class="sxs-lookup"><span data-stu-id="6d1a1-213">Publish and deploy</span></span>

<span data-ttu-id="6d1a1-214">Visual Studio を使う場合、ToDo Blazor アプリを Azure に発行するには、次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-214">When using Visual Studio, perform the following steps to publish the Todo Blazor app to Azure:</span></span>

1. <span data-ttu-id="6d1a1-215">**[ソリューション エクスプローラー]** でプロジェクトを右クリックし、**[発行]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-215">Right-click on the project in **Solution Explorer** and select **Publish**.</span></span>

1. <span data-ttu-id="6d1a1-216">**[発行先を選択]** ダイアログで、**[App Service]** と **[新規作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-216">In the **Pick a publish target** dialog, select **App Service** and **Create New**.</span></span> <span data-ttu-id="6d1a1-217">**[発行]** を選びます。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-217">Select **Publish**.</span></span>

    ![発行先を選択](build-your-first-blazor-app/_static/blazor-publish-pick-target.png)

1. <span data-ttu-id="6d1a1-219">**[App Service の作成]** ダイアログで、アプリの名前を選択し、サブスクリプション、リソース グループ、およびホスティング プランを選択します。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-219">In the **Create App Service** dialog, choose a name for the app and select the subscription, resource group, and hosting plan.</span></span> <span data-ttu-id="6d1a1-220">**[作成]** を選択して App Service を作成し、アプリを発行します。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-220">Select **Create** to create the app service and publish the app.</span></span>

    ![App Service の作成](build-your-first-blazor-app/_static/blazor-publish-create-appservice2.png)

<span data-ttu-id="6d1a1-222">アプリが配置されるまで 1 分ほど待機します。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-222">Wait a minute or so for the app to be deployed.</span></span>

<span data-ttu-id="6d1a1-223">これで、アプリが Azure で稼働されました。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-223">The app should now be running in Azure.</span></span> <span data-ttu-id="6d1a1-224">最初の Blazor アプリを構築するという ToDo アイテムを、"*完了*" にマークしましょう。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-224">Mark the todo item to build your first Blazor app as *done*.</span></span> <span data-ttu-id="6d1a1-225">お疲れさまでした。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-225">Nice job!</span></span>

![Azure 上の Blazor](https://user-images.githubusercontent.com/1874516/39512621-83406508-4da6-11e8-8244-5bae2ac6b22d.png)

> [!NOTE]
> <span data-ttu-id="6d1a1-227">Visual Studio を使わない場合は、Windows、macOS、または Linux 上のコマンド プロンプトで Blazor アプリを発行します。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-227">If not using Visual Studio, publish the Blazor app at a command prompt on Windows, macOS, or Linux:</span></span>
>
> ```console
> dotnet publish -c Release
> ```
>
> <span data-ttu-id="6d1a1-228">配置は、*/bin/Release/\<ターゲット フレームワーク>/publish* フォルダーに作成されます。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-228">The deployment is created in the */bin/Release/\<target-framework>/publish* folder.</span></span> <span data-ttu-id="6d1a1-229">*publish* フォルダーのコンテンツを、サーバーまたはホスティング サービスに移動します。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-229">Move the contents of the *publish* folder to the server or hosting service.</span></span>
>
> <span data-ttu-id="6d1a1-230">詳細については、[ホストと配置](xref:host-and-deploy/razor-components/index#publish-the-app)に関するトピックをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-230">For more information, see the [Host and deploy](xref:host-and-deploy/razor-components/index#publish-the-app) topic.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6d1a1-231">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="6d1a1-231">Additional resources</span></span>

<span data-ttu-id="6d1a1-232">より複雑な Blazor のサンプル アプリについては、GitHub 上の [Flight Finder サンプル](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="6d1a1-232">For a more involved Blazor sample app, check out the [Flight Finder sample](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor) on GitHub.</span></span>

![Blazor Flight Finder](https://msdnshared.blob.core.windows.net/media/2018/03/blazor-flight-finder.png)
