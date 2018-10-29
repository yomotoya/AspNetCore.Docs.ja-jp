---
title: NSwag と ASP.NET Core の概要
author: zuckerthoben
description: NSwag を使用し、ASP.NET Core Web API のドキュメント ページとヘルプ ページを生成する方法について説明します。
ms.author: scaddie
ms.custom: mvc
ms.date: 09/20/2018
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: 6c7d76e2202bf47c8d3e5d296e64e9e8c820e2a1
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207850"
---
# <a name="get-started-with-nswag-and-aspnet-core"></a><span data-ttu-id="14ee9-103">NSwag と ASP.NET Core の概要</span><span class="sxs-lookup"><span data-stu-id="14ee9-103">Get started with NSwag and ASP.NET Core</span></span>

<span data-ttu-id="14ee9-104">作成者: [Christoph Nienaber](https://twitter.com/zuckerthoben) および [Rico Suter](https://rsuter.com)</span><span class="sxs-lookup"><span data-stu-id="14ee9-104">By [Christoph Nienaber](https://twitter.com/zuckerthoben) and [Rico Suter](https://rsuter.com)</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="14ee9-105">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="14ee9-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="14ee9-106">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="14ee9-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker-end

<span data-ttu-id="14ee9-107">NSwag ミドルウェアの登録でできること:</span><span class="sxs-lookup"><span data-stu-id="14ee9-107">Register the NSwag middlewares to:</span></span>

* <span data-ttu-id="14ee9-108">実装済みの Web API に対して Swagger 仕様を生成します。</span><span class="sxs-lookup"><span data-stu-id="14ee9-108">Generate the Swagger specification for the implemented web API.</span></span>
* <span data-ttu-id="14ee9-109">Web API を参照し、テストするサービスを Swagger UI に提供します。</span><span class="sxs-lookup"><span data-stu-id="14ee9-109">Serve the Swagger UI to browse and test the web API.</span></span>

<span data-ttu-id="14ee9-110">[NSwag](https://github.com/RSuter/NSwag) ASP.NET Core ミドルウェアを使用するには、[NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="14ee9-110">To use the [NSwag](https://github.com/RSuter/NSwag) ASP.NET Core middlewares, install the [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet package.</span></span> <span data-ttu-id="14ee9-111">このパッケージには、Swagger 仕様、Swagger UI (v2 と v3)、[ReDoc UI](https://github.com/Rebilly/ReDoc) を生成し、それらにサービスを提供するミドルウェアが含まれています。</span><span class="sxs-lookup"><span data-stu-id="14ee9-111">This package contains the middlewares to generate and serve the Swagger specification, Swagger UI (v2 and v3), and [ReDoc UI](https://github.com/Rebilly/ReDoc).</span></span>

<span data-ttu-id="14ee9-112">また、NSwag のコード生成機能を利用することを強くお勧めします。</span><span class="sxs-lookup"><span data-stu-id="14ee9-112">Additionally, it's highly recommended to make use of NSwag's code generation capabilities.</span></span> <span data-ttu-id="14ee9-113">次のオプションのいずれかを選択し、コード生成機能を使用します。</span><span class="sxs-lookup"><span data-stu-id="14ee9-113">Choose one of the following options to use the code generation capabilities:</span></span>

* <span data-ttu-id="14ee9-114">[NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio) を使用します。これは API のクライアント コードを C# と TypeScript で生成するための Windows デスクトップ アプリです。</span><span class="sxs-lookup"><span data-stu-id="14ee9-114">Use [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), a Windows desktop app for generating client code in C# and TypeScript for your API.</span></span>
* <span data-ttu-id="14ee9-115">[NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) または [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet パッケージを使用し、プロジェクト内でコードを生成します。</span><span class="sxs-lookup"><span data-stu-id="14ee9-115">Use the [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) or [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet packages to do code generation inside your project.</span></span>
* <span data-ttu-id="14ee9-116">[コマンド ライン](https://github.com/NSwag/NSwag/wiki/CommandLine)から NSwag を使用します。</span><span class="sxs-lookup"><span data-stu-id="14ee9-116">Use NSwag from the [command line](https://github.com/NSwag/NSwag/wiki/CommandLine).</span></span>
* <span data-ttu-id="14ee9-117">[NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) NuGet パッケージを使用します。</span><span class="sxs-lookup"><span data-stu-id="14ee9-117">Use the [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) NuGet package.</span></span>

## <a name="features"></a><span data-ttu-id="14ee9-118">フィーチャー</span><span class="sxs-lookup"><span data-stu-id="14ee9-118">Features</span></span>

<span data-ttu-id="14ee9-119">NSwag を使用する主な理由は、Swagger UI と Swagger ジェネレーターを導入できることのみならず、柔軟なコード生成機能を利用できることにもあります。</span><span class="sxs-lookup"><span data-stu-id="14ee9-119">The main reason to use NSwag is the ability to not only introduce the Swagger UI and Swagger generator, but to also make use of the flexible code generation capabilities.</span></span> <span data-ttu-id="14ee9-120">既存の API は必要ありません&mdash;Swagger が組み込まれているサードパーティ製の API を利用し、NSwag にクライアント実装を生成させることができます。</span><span class="sxs-lookup"><span data-stu-id="14ee9-120">You don't need an existing API&mdash;you can use third-party APIs that incorporate Swagger and let NSwag generate a client implementation.</span></span> <span data-ttu-id="14ee9-121">いずれの方法でも、開発周期が早まり、API 変更に合わせた調整が簡単になります。</span><span class="sxs-lookup"><span data-stu-id="14ee9-121">Either way, the development cycle is expedited and you can more easily adapt to API changes.</span></span>

## <a name="package-installation"></a><span data-ttu-id="14ee9-122">パッケージ インストール</span><span class="sxs-lookup"><span data-stu-id="14ee9-122">Package installation</span></span>

<span data-ttu-id="14ee9-123">NSwag NuGet パッケージは、次の方法で追加できます。</span><span class="sxs-lookup"><span data-stu-id="14ee9-123">The NSwag NuGet package can be added with the following approaches:</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="14ee9-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="14ee9-124">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="14ee9-125">**[パッケージ マネージャー コンソール]** ウィンドウから:</span><span class="sxs-lookup"><span data-stu-id="14ee9-125">From the **Package Manager Console** window:</span></span>
  * <span data-ttu-id="14ee9-126">**[ビュー]** > **[Other Windows]** \(その他の Windows\) > **[パッケージ マネージャー コンソール]** に移動します。</span><span class="sxs-lookup"><span data-stu-id="14ee9-126">Go to **View** > **Other Windows** > **Package Manager Console**</span></span>
  * <span data-ttu-id="14ee9-127">*TodoApi.csproj* ファイルが存在するディレクトリに移動します。</span><span class="sxs-lookup"><span data-stu-id="14ee9-127">Navigate to the directory in which the *TodoApi.csproj* file exists</span></span>
  * <span data-ttu-id="14ee9-128">次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="14ee9-128">Execute the following command:</span></span>

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* <span data-ttu-id="14ee9-129">**[NuGet パッケージの管理]** ダイアログ ボックスから:</span><span class="sxs-lookup"><span data-stu-id="14ee9-129">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="14ee9-130">**[ソリューション エクスプローラー]** > **[NuGet パッケージの管理]** でプロジェクトを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="14ee9-130">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="14ee9-131">**パッケージ ソース**を "nuget.org" に設定します。</span><span class="sxs-lookup"><span data-stu-id="14ee9-131">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="14ee9-132">検索ボックスに「NSwag.AspNetCore」と入力します。</span><span class="sxs-lookup"><span data-stu-id="14ee9-132">Enter "NSwag.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="14ee9-133">**[参照]** タブから "NSwag.AspNetCore" パッケージを選択して、**[インストール]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="14ee9-133">Select the "NSwag.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="14ee9-134">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="14ee9-134">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="14ee9-135">**[Solution Pad]** > **[パッケージを追加]** で [*パッケージ*] フォルダーを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="14ee9-135">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="14ee9-136">**[パッケージを追加]** ウィンドウの **[ソース]** ドロップダウンを "nuget.org" に設定します。</span><span class="sxs-lookup"><span data-stu-id="14ee9-136">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="14ee9-137">検索ボックスに「NSwag.AspNetCore」と入力します。</span><span class="sxs-lookup"><span data-stu-id="14ee9-137">Enter "NSwag.AspNetCore" in the search box</span></span>
* <span data-ttu-id="14ee9-138">結果ウィンドウから NSwag.AspNetCore パッケージを選択し、**[パッケージを追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="14ee9-138">Select the "NSwag.AspNetCore" package from the results pane and click **Add Package**</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="14ee9-139">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="14ee9-139">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="14ee9-140">**統合端末**からから次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="14ee9-140">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="14ee9-141">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="14ee9-141">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="14ee9-142">次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="14ee9-142">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="14ee9-143">Swagger ミドルウェアを追加して構成する</span><span class="sxs-lookup"><span data-stu-id="14ee9-143">Add and configure Swagger middleware</span></span>

<span data-ttu-id="14ee9-144">`Startup` クラスで次の名前空間をインポートします。</span><span class="sxs-lookup"><span data-stu-id="14ee9-144">Import the following namespaces in the `Startup` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_StartupConfigureImports)]

<span data-ttu-id="14ee9-145">`Startup.ConfigureServices` メソッドで、必須の Swagger サービスを登録します。</span><span class="sxs-lookup"><span data-stu-id="14ee9-145">In the `Startup.ConfigureServices` method, register the required Swagger services:</span></span> 

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_ConfigureServices&highlight=8)]

<span data-ttu-id="14ee9-146">`Startup.Configure` メソッドで、生成された Swagger 仕様と SwaggerUI v3 対応のミドルウェアを有効にします。</span><span class="sxs-lookup"><span data-stu-id="14ee9-146">In the `Startup.Configure` method, enable the middleware for serving the generated Swagger specification and the Swagger UI v3:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=6-10)]

<span data-ttu-id="14ee9-147">アプリを起動します。</span><span class="sxs-lookup"><span data-stu-id="14ee9-147">Launch the app.</span></span> <span data-ttu-id="14ee9-148">`http://localhost:<port>/swagger` に移動し、Swagger UI を表示します。</span><span class="sxs-lookup"><span data-stu-id="14ee9-148">Navigate to `http://localhost:<port>/swagger` to view the Swagger UI.</span></span> <span data-ttu-id="14ee9-149">`http://localhost:<port>/swagger/v1/swagger.json` に移動し、Swagger 仕様を表示します。</span><span class="sxs-lookup"><span data-stu-id="14ee9-149">Navigate to `http://localhost:<port>/swagger/v1/swagger.json` to view the Swagger specification.</span></span>

## <a name="code-generation"></a><span data-ttu-id="14ee9-150">コード生成</span><span class="sxs-lookup"><span data-stu-id="14ee9-150">Code generation</span></span>

### <a name="via-nswagstudio"></a><span data-ttu-id="14ee9-151">NSwagStudio から</span><span class="sxs-lookup"><span data-stu-id="14ee9-151">Via NSwagStudio</span></span>

* <span data-ttu-id="14ee9-152">公式 [GitHub repository](https://github.com/RSuter/NSwag/wiki/NSwagStudio) リポジトリから NSwagStudio をインストールします。</span><span class="sxs-lookup"><span data-stu-id="14ee9-152">Install NSwagStudio from the official [GitHub repository](https://github.com/RSuter/NSwag/wiki/NSwagStudio).</span></span>
* <span data-ttu-id="14ee9-153">NSwagStudio を起動します。</span><span class="sxs-lookup"><span data-stu-id="14ee9-153">Launch NSwagStudio.</span></span> <span data-ttu-id="14ee9-154">**[Swagger Specification URL]** \(Swagger 仕様 URL\) テキストボックスに *swagger.json* ファイルの URL を入力し、**[Create local Copy]** \(ローカル コピーの作成\) ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="14ee9-154">Enter the *swagger.json* file URL in the **Swagger Specification URL** textbox, and click the **Create local Copy** button.</span></span>
* <span data-ttu-id="14ee9-155">**[CSharp Client]** \(CSharp クライアント\) クライアント出力の種類を選択します。</span><span class="sxs-lookup"><span data-stu-id="14ee9-155">Select the **CSharp Client** client output type.</span></span> <span data-ttu-id="14ee9-156">他の選択肢には、**[TypeScript Client]\(TypeScript クライアント\)**、**[CSharp Web API Controller]\(CSharp Web API コントローラー\)** があります。</span><span class="sxs-lookup"><span data-stu-id="14ee9-156">Other options include **TypeScript Client** and **CSharp Web API Controller**.</span></span> <span data-ttu-id="14ee9-157">Web API コントローラーは基本的に逆生成です。</span><span class="sxs-lookup"><span data-stu-id="14ee9-157">Using a Web API Controller is basically a reverse generation.</span></span> <span data-ttu-id="14ee9-158">サービスの仕様を利用してサービスを再構築します。</span><span class="sxs-lookup"><span data-stu-id="14ee9-158">It uses a specification of a service to rebuild the service.</span></span>
* <span data-ttu-id="14ee9-159">**[Generate Outputs]** \(出力の生成\) ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="14ee9-159">Click the **Generate Outputs** button.</span></span> <span data-ttu-id="14ee9-160">*TodoApi.NSwag* プロジェクトの完全な C# クライアントの実装が作成されます。</span><span class="sxs-lookup"><span data-stu-id="14ee9-160">A complete C# client implementation of the *TodoApi.NSwag* project is produced.</span></span> <span data-ttu-id="14ee9-161">**[出力]** セクションの **[CSharp Client]** \(CSharp クライアント\) タブをクリックし、生成されたクライアント コードを表示します。</span><span class="sxs-lookup"><span data-stu-id="14ee9-161">Click the **CSharp Client** tab of the **Outputs** section to see the generated client code:</span></span>

```csharp
//----------------------
// <auto-generated>
//     Generated using the NSwag toolchain v11.17.3.0 (NJsonSchema v9.10.46.0 (Newtonsoft.Json v9.0.0.0)) (http://NSwag.org)
// </auto-generated>
//----------------------

namespace MyNamespace
{
    #pragma warning disable // Disable all warnings

    [System.CodeDom.Compiler.GeneratedCode("NSwag",
        "11.17.3.0 (NJsonSchema v9.10.46.0 (Newtonsoft.Json v9.0.0.0))")]
    public partial class TodoClient
    {
        private string _baseUrl = "http://localhost:50499";
        private System.Lazy<Newtonsoft.Json.JsonSerializerSettings> _settings;

        public TodoClient()
        {
            _settings = new System.Lazy
                <Newtonsoft.Json.JsonSerializerSettings>(() =>
            {
                var settings = new Newtonsoft.Json.JsonSerializerSettings();
                UpdateJsonSerializerSettings(settings);
                return settings;
            });
        }

        public string BaseUrl
        {
            get { return _baseUrl; }
            set { _baseUrl = value; }
        }

        // code omitted for brevity
```

> [!TIP]
> <span data-ttu-id="14ee9-162">C# クライアント コードは、**[CSharp Client]** \(CSharp クライアント\) タブの **[設定]** タブに定義された設定に基づき生成されます。この設定を変更して、既定の名前空間の名前変更や同期メソッドの生成などのタスクを実行します。</span><span class="sxs-lookup"><span data-stu-id="14ee9-162">The C# client code is generated based on settings defined in the **Settings** tab of the **CSharp Client** tab. Modify the settings to perform tasks such as default namespace renaming and synchronous method generation.</span></span>

* <span data-ttu-id="14ee9-163">生成された C# コードは、クライアント プロジェクトのファイルに入れます (例: [Xamarin.Forms](/xamarin/xamarin-forms/) アプリ)。</span><span class="sxs-lookup"><span data-stu-id="14ee9-163">Copy the generated C# code into a file in a client project (for example, a [Xamarin.Forms](/xamarin/xamarin-forms/) app).</span></span>
* <span data-ttu-id="14ee9-164">Web API の使用を開始します。</span><span class="sxs-lookup"><span data-stu-id="14ee9-164">Start consuming the web API:</span></span>

```csharp
var todoClient = new TodoClient();

// Gets all to-dos from the API
var allTodos = await todoClient.GetAllAsync();

// Create a new TodoItem, and save it in the API
var createdTodo = await todoClient.CreateAsync(new TodoItem());

// Get a single to-do by ID
var foundTodo = await todoClient.GetByIdAsync(1);
```

> [!NOTE]
> <span data-ttu-id="14ee9-165">ベース URL または HTTP クライアントを API クライアントに挿入できます。</span><span class="sxs-lookup"><span data-stu-id="14ee9-165">You can inject a base URL and/or a HTTP client into the API client.</span></span> <span data-ttu-id="14ee9-166">ベスト プラクティスは常に [HttpClient を再利用すること](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/)です。</span><span class="sxs-lookup"><span data-stu-id="14ee9-166">The best practice is to always [reuse the HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).</span></span>

### <a name="other-ways-to-generate-client-code"></a><span data-ttu-id="14ee9-167">クライアント コードを生成するその他の方法</span><span class="sxs-lookup"><span data-stu-id="14ee9-167">Other ways to generate client code</span></span>

<span data-ttu-id="14ee9-168">自分のワークフローにより適した、他の方法でクライアント コードを生成できます。</span><span class="sxs-lookup"><span data-stu-id="14ee9-168">You can generate the client code in other ways, more suited to your workflow:</span></span>

* [<span data-ttu-id="14ee9-169">MSBuild</span><span class="sxs-lookup"><span data-stu-id="14ee9-169">MSBuild</span></span>](https://www.nuget.org/packages/NSwag.MSBuild/)

* [<span data-ttu-id="14ee9-170">コードで</span><span class="sxs-lookup"><span data-stu-id="14ee9-170">In code</span></span>](https://github.com/NSwag/NSwag/wiki/SwaggerToCSharpClientGenerator)

* [<span data-ttu-id="14ee9-171">T4 テンプレート</span><span class="sxs-lookup"><span data-stu-id="14ee9-171">T4 templates</span></span>](https://github.com/NSwag/NSwag/wiki/T4)

## <a name="customize"></a><span data-ttu-id="14ee9-172">カスタマイズ</span><span class="sxs-lookup"><span data-stu-id="14ee9-172">Customize</span></span>

<span data-ttu-id="14ee9-173">Swagger には、Web API の消費を容易にする、オブジェクト モデルをドキュメント化するオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="14ee9-173">Swagger provides options for documenting the object model to ease consumption of the web API.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="14ee9-174">API 情報と説明</span><span class="sxs-lookup"><span data-stu-id="14ee9-174">API info and description</span></span>

<span data-ttu-id="14ee9-175">`Startup.Configure`メソッドでは、`UseSwagger` メソッドに渡された構成アクションによって、作成者、ライセンス、説明などの情報が追加されます。</span><span class="sxs-lookup"><span data-stu-id="14ee9-175">In the `Startup.Configure` method, a configuration action passed to the `UseSwagger` method adds information such as the author, license, and description:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup2.cs?name=snippet_UseSwagger)]

<span data-ttu-id="14ee9-176">Swagger UI には、バージョンの情報が表示されます。</span><span class="sxs-lookup"><span data-stu-id="14ee9-176">The Swagger UI displays the version's information:</span></span>

![バージョン情報を含む Swagger UI](web-api-help-pages-using-swagger/_static/custom-info-nswag.png)

### <a name="xml-comments"></a><span data-ttu-id="14ee9-178">XML コメント</span><span class="sxs-lookup"><span data-stu-id="14ee9-178">XML comments</span></span>

<span data-ttu-id="14ee9-179">XML コメントは次の方法で有効になります。</span><span class="sxs-lookup"><span data-stu-id="14ee9-179">XML comments are enabled with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio-xml"></a>[<span data-ttu-id="14ee9-180">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="14ee9-180">Visual Studio</span></span>](#tab/visual-studio-xml/)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="14ee9-181">**ソリューション エクスプローラー**でプロジェクトを右クリックし、**[<project_name>.csproj の編集]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="14ee9-181">Right-click the project in **Solution Explorer** and select **Edit <project_name>.csproj**.</span></span>
* <span data-ttu-id="14ee9-182">強調表示された行を手動で *.csproj* ファイルに追加します。</span><span class="sxs-lookup"><span data-stu-id="14ee9-182">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="14ee9-183">**ソリューション エクスプローラー**で、プロジェクトを右クリックし、**[プロパティ]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="14ee9-183">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="14ee9-184">**[ビルド]** タブの **[出力]** セクションの下にある **[XML ドキュメント ファイル]** チェック ボックスをオンにします。</span><span class="sxs-lookup"><span data-stu-id="14ee9-184">Check the **XML documentation file** box under the **Output** section of the **Build** tab</span></span>

::: moniker-end

# <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[<span data-ttu-id="14ee9-185">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="14ee9-185">Visual Studio for Mac</span></span>](#tab/visual-studio-mac-xml/)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="14ee9-186">*Solution Pad* から **Ctrl** キーを押しながらプロジェクト名をクリックします。</span><span class="sxs-lookup"><span data-stu-id="14ee9-186">From the *Solution Pad*, press **control** and click the project name.</span></span> <span data-ttu-id="14ee9-187">**[ツール]** > **[ファイルの編集]** に移動します。</span><span class="sxs-lookup"><span data-stu-id="14ee9-187">Navigate to **Tools** > **Edit File**.</span></span>
* <span data-ttu-id="14ee9-188">強調表示された行を手動で *.csproj* ファイルに追加します。</span><span class="sxs-lookup"><span data-stu-id="14ee9-188">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="14ee9-189">**[プロジェクト オプション]** ダイアログ > **[ビルド]**>**[コンパイラ]** を開きます。</span><span class="sxs-lookup"><span data-stu-id="14ee9-189">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="14ee9-190">**[全般オプション]** セクションの下にある **[XML ドキュメントを生成する]** チェック ボックスをオンにします。</span><span class="sxs-lookup"><span data-stu-id="14ee9-190">Check the **Generate xml documentation** box under the **General Options** section</span></span>

::: moniker-end

# <a name="visual-studio-codetabvisual-studio-code-xml"></a>[<span data-ttu-id="14ee9-191">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="14ee9-191">Visual Studio Code</span></span>](#tab/visual-studio-code-xml/)

<span data-ttu-id="14ee9-192">強調表示された行を手動で *.csproj* ファイルに追加します。</span><span class="sxs-lookup"><span data-stu-id="14ee9-192">Manually add the highlighted lines to the *.csproj* file:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

---

### <a name="data-annotations"></a><span data-ttu-id="14ee9-193">データの注釈</span><span class="sxs-lookup"><span data-stu-id="14ee9-193">Data annotations</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="14ee9-194">NSwag では、[Reflection](/dotnet/csharp/programming-guide/concepts/reflection) が使用されます。Web API アクションに推奨される戻り値の型は [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) です。</span><span class="sxs-lookup"><span data-stu-id="14ee9-194">NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span></span> <span data-ttu-id="14ee9-195">結果的に、NSwag では、アクションの内容とそれで返されるものを推論できません。</span><span class="sxs-lookup"><span data-stu-id="14ee9-195">Consequently, NSwag can't infer what your action is doing and what it returns.</span></span> <span data-ttu-id="14ee9-196">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="14ee9-196">Consider the following example:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

<span data-ttu-id="14ee9-197">先行するアクションによって `IActionResult` が返されますが、このアクションの内部では [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) または [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest) を返しています。</span><span class="sxs-lookup"><span data-stu-id="14ee9-197">The preceding action returns `IActionResult`, but inside the action it's returning either [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) or [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest).</span></span> <span data-ttu-id="14ee9-198">データ注釈は、このアクションによって返される既知の HTTP 応答コードをクライアントに通知します。</span><span class="sxs-lookup"><span data-stu-id="14ee9-198">Data annotations are used to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="14ee9-199">アクションに次の属性を追加します。</span><span class="sxs-lookup"><span data-stu-id="14ee9-199">Decorate the action with the following attributes:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="14ee9-200">NSwag では、[Reflection](/dotnet/csharp/programming-guide/concepts/reflection) が使用されます。Web API アクションに推奨される戻り値の型は [ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1) です。</span><span class="sxs-lookup"><span data-stu-id="14ee9-200">NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1).</span></span> <span data-ttu-id="14ee9-201">その結果、NSwag では `T` によって定義された戻り値のみ推論できます。</span><span class="sxs-lookup"><span data-stu-id="14ee9-201">Consequently, NSwag can only infer the return type defined by `T`.</span></span> <span data-ttu-id="14ee9-202">アクションのその他の可能な戻り値は推論できません。</span><span class="sxs-lookup"><span data-stu-id="14ee9-202">Other possible return types in the action cannot be inferred.</span></span> <span data-ttu-id="14ee9-203">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="14ee9-203">Consider the following example:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

<span data-ttu-id="14ee9-204">先行するアクションによって `ActionResult<T>` が返されますが、このアクションの内部では [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) を返しています。</span><span class="sxs-lookup"><span data-stu-id="14ee9-204">The preceding action returns `ActionResult<T>`, but inside the action it's returning either [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute).</span></span> <span data-ttu-id="14ee9-205">コントローラーは、[[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) 属性で修飾されるため、応答として [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest) も可能です。</span><span class="sxs-lookup"><span data-stu-id="14ee9-205">Since the controller is decorated with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute, a [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest) response is possible too.</span></span> <span data-ttu-id="14ee9-206">詳細については、「[自動的な HTTP 400 応答](xref:web-api/index#automatic-http-400-responses)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="14ee9-206">See [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses) for more info.</span></span> <span data-ttu-id="14ee9-207">データ注釈は、このアクションによって返される既知の HTTP 応答コードをクライアントに通知します。</span><span class="sxs-lookup"><span data-stu-id="14ee9-207">Data annotations are used to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="14ee9-208">アクションに次の属性を追加します。</span><span class="sxs-lookup"><span data-stu-id="14ee9-208">Decorate the action with the following attributes:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]

::: moniker-end

<span data-ttu-id="14ee9-209">Swagger ジェネレーターでは、このアクションを正確に表現できるようになりました。生成されたクライアントでは、エンドポイントの呼び出し時に受け取るものが認識されます。</span><span class="sxs-lookup"><span data-stu-id="14ee9-209">The Swagger generator can now accurately describe this action, and generated clients know what they receive when calling the endpoint.</span></span> <span data-ttu-id="14ee9-210">すべてのアクションにこれらの属性を追加することを強くお勧めします。</span><span class="sxs-lookup"><span data-stu-id="14ee9-210">Decorating all actions with these attributes is highly recommended.</span></span> <span data-ttu-id="14ee9-211">API アクションで返される HTTP 応答のガイドラインについては、[RFC 7231 仕様](https://tools.ietf.org/html/rfc7231#section-4.3)をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="14ee9-211">For guidelines on what HTTP responses your API actions should return, see the [RFC 7231 specification](https://tools.ietf.org/html/rfc7231#section-4.3).</span></span>
