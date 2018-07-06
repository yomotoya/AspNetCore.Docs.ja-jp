---
title: NSwag と ASP.NET Core の概要
author: zuckerthoben
description: NSwag を使用し、ASP.NET Core Web API のドキュメント ページとヘルプ ページを生成する方法について説明します。
ms.author: scaddie
ms.custom: mvc
ms.date: 06/29/2018
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: c0811593609b7d1e3529d5253e8b053f180281f3
ms.sourcegitcommit: 2941e24d7f3fd3d5e88d27e5f852aaedd564deda
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/29/2018
ms.locfileid: "37126275"
---
# <a name="get-started-with-nswag-and-aspnet-core"></a><span data-ttu-id="846a6-103">NSwag と ASP.NET Core の概要</span><span class="sxs-lookup"><span data-stu-id="846a6-103">Get started with NSwag and ASP.NET Core</span></span>

<span data-ttu-id="846a6-104">作成者: [Christoph Nienaber](https://twitter.com/zuckerthoben) および [Rico Suter](https://rsuter.com)</span><span class="sxs-lookup"><span data-stu-id="846a6-104">By [Christoph Nienaber](https://twitter.com/zuckerthoben) and [Rico Suter](https://rsuter.com)</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="846a6-105">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="846a6-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="846a6-106">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="846a6-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker-end

<span data-ttu-id="846a6-107">[NSwag](https://github.com/RSuter/NSwag) と共に ASP.NET Core ミドルウェアを使用するには、[NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet パッケージが必要です。</span><span class="sxs-lookup"><span data-stu-id="846a6-107">Using [NSwag](https://github.com/RSuter/NSwag) with ASP.NET Core middleware requires the [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet package.</span></span> <span data-ttu-id="846a6-108">このパッケージは、Swagger ジェネレーター、Swagger UI (v2 と v3)、[ReDoc UI](https://github.com/Rebilly/ReDoc) で構成されています。</span><span class="sxs-lookup"><span data-stu-id="846a6-108">The package consists of a Swagger generator, Swagger UI (v2 and v3), and [ReDoc UI](https://github.com/Rebilly/ReDoc).</span></span>

<span data-ttu-id="846a6-109">NSwag のコード生成機能を利用することを強くお勧めします。</span><span class="sxs-lookup"><span data-stu-id="846a6-109">It's highly recommended to make use of NSwag's code generation capabilities.</span></span> <span data-ttu-id="846a6-110">コード生成に次のいずれかのオプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="846a6-110">Choose one of the following options for code generation:</span></span>

* <span data-ttu-id="846a6-111">[NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio) を使用します。これは API のクライアント コードを C# と TypeScript で生成するための Windows デスクトップ アプリです。</span><span class="sxs-lookup"><span data-stu-id="846a6-111">Use [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), a Windows desktop app for generating client code in C# and TypeScript for your API.</span></span>
* <span data-ttu-id="846a6-112">[NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) または [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet パッケージを使用し、プロジェクト内でコードを生成します。</span><span class="sxs-lookup"><span data-stu-id="846a6-112">Use the [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) or [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet packages to do code generation inside your project.</span></span>
* <span data-ttu-id="846a6-113">[コマンド ライン](https://github.com/NSwag/NSwag/wiki/CommandLine)から NSwag を使用します。</span><span class="sxs-lookup"><span data-stu-id="846a6-113">Use NSwag from the [command line](https://github.com/NSwag/NSwag/wiki/CommandLine).</span></span>
* <span data-ttu-id="846a6-114">[NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) NuGet パッケージを使用します。</span><span class="sxs-lookup"><span data-stu-id="846a6-114">Use the [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) NuGet package.</span></span>

## <a name="features"></a><span data-ttu-id="846a6-115">フィーチャー</span><span class="sxs-lookup"><span data-stu-id="846a6-115">Features</span></span>

<span data-ttu-id="846a6-116">NSwag を使用する主な理由は、Swagger UI と Swagger ジェネレーターを導入できることのみならず、柔軟なコード生成機能を利用できることにあります。</span><span class="sxs-lookup"><span data-stu-id="846a6-116">The main reason to use NSwag is the ability to not only introduce the Swagger UI and Swagger generator, but to make use of the flexible code generation capabilities.</span></span> <span data-ttu-id="846a6-117">既存の API は必要ありません&mdash;Swagger が組み込まれているサードパーティ製の API を利用し、NSwag にクライアント実装を生成させることができます。</span><span class="sxs-lookup"><span data-stu-id="846a6-117">You don't need an existing API&mdash;you can use third-party APIs that incorporate Swagger and let NSwag generate a client implementation.</span></span> <span data-ttu-id="846a6-118">いずれの方法でも、開発周期が早まり、API 変更に合わせた調整が簡単になります。</span><span class="sxs-lookup"><span data-stu-id="846a6-118">Either way, the development cycle is expedited and you can more easily adapt to API changes.</span></span>

## <a name="package-installation"></a><span data-ttu-id="846a6-119">パッケージ インストール</span><span class="sxs-lookup"><span data-stu-id="846a6-119">Package installation</span></span>

<span data-ttu-id="846a6-120">NSwag NuGet パッケージは、次の方法で追加できます。</span><span class="sxs-lookup"><span data-stu-id="846a6-120">The NSwag NuGet package can be added with the following approaches:</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="846a6-121">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="846a6-121">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="846a6-122">**[パッケージ マネージャー コンソール]** ウィンドウから:</span><span class="sxs-lookup"><span data-stu-id="846a6-122">From the **Package Manager Console** window:</span></span>
  * <span data-ttu-id="846a6-123">**[ビュー]** > **[Other Windows]** \(その他の Windows\) > **[パッケージ マネージャー コンソール]** に移動します。</span><span class="sxs-lookup"><span data-stu-id="846a6-123">Go to **View** > **Other Windows** > **Package Manager Console**</span></span>
  * <span data-ttu-id="846a6-124">*TodoApi.csproj* ファイルが存在するディレクトリに移動します。</span><span class="sxs-lookup"><span data-stu-id="846a6-124">Navigate to the directory in which the *TodoApi.csproj* file exists</span></span>
  * <span data-ttu-id="846a6-125">次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="846a6-125">Execute the following command:</span></span>

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* <span data-ttu-id="846a6-126">**[NuGet パッケージの管理]** ダイアログ ボックスから:</span><span class="sxs-lookup"><span data-stu-id="846a6-126">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="846a6-127">**[ソリューション エクスプローラー]** > **[NuGet パッケージの管理]** でプロジェクトを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="846a6-127">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="846a6-128">**パッケージ ソース**を "nuget.org" に設定します。</span><span class="sxs-lookup"><span data-stu-id="846a6-128">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="846a6-129">検索ボックスに「NSwag.AspNetCore」と入力します。</span><span class="sxs-lookup"><span data-stu-id="846a6-129">Enter "NSwag.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="846a6-130">**[参照]** タブから "NSwag.AspNetCore" パッケージを選択して、**[インストール]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="846a6-130">Select the "NSwag.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="846a6-131">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="846a6-131">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="846a6-132">**[Solution Pad]** > **[パッケージを追加]** で [*パッケージ*] フォルダーを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="846a6-132">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="846a6-133">**[パッケージを追加]** ウィンドウの **[ソース]** ドロップダウンを "nuget.org" に設定します。</span><span class="sxs-lookup"><span data-stu-id="846a6-133">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="846a6-134">検索ボックスに「NSwag.AspNetCore」と入力します。</span><span class="sxs-lookup"><span data-stu-id="846a6-134">Enter "NSwag.AspNetCore" in the search box</span></span>
* <span data-ttu-id="846a6-135">結果ウィンドウから NSwag.AspNetCore パッケージを選択し、**[パッケージを追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="846a6-135">Select the "NSwag.AspNetCore" package from the results pane and click **Add Package**</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="846a6-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="846a6-136">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="846a6-137">**統合端末**からから次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="846a6-137">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="846a6-138">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="846a6-138">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="846a6-139">次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="846a6-139">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="846a6-140">Swagger ミドルウェアを追加して構成する</span><span class="sxs-lookup"><span data-stu-id="846a6-140">Add and configure Swagger middleware</span></span>

<span data-ttu-id="846a6-141">`Startup` クラスで次の名前空間をインポートします。</span><span class="sxs-lookup"><span data-stu-id="846a6-141">Import the following namespaces in the `Startup` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_StartupConfigureImports)]

<span data-ttu-id="846a6-142">`Startup.Configure` メソッドで、生成された Swagger 仕様と SwaggerUI 対応のミドルウェアを有効にします。</span><span class="sxs-lookup"><span data-stu-id="846a6-142">In the `Startup.Configure` method, enable the middleware for serving the generated Swagger specification and the Swagger UI:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=6-10)]

<span data-ttu-id="846a6-143">アプリを起動します。</span><span class="sxs-lookup"><span data-stu-id="846a6-143">Launch the app.</span></span> <span data-ttu-id="846a6-144">`http://localhost:<port>/swagger` に移動し、Swagger UI を表示します。</span><span class="sxs-lookup"><span data-stu-id="846a6-144">Navigate to `http://localhost:<port>/swagger` to view the Swagger UI.</span></span> <span data-ttu-id="846a6-145">`http://localhost:<port>/swagger/v1/swagger.json` に移動し、Swagger 仕様を表示します。</span><span class="sxs-lookup"><span data-stu-id="846a6-145">Navigate to `http://localhost:<port>/swagger/v1/swagger.json` to view the Swagger specification.</span></span>

## <a name="code-generation"></a><span data-ttu-id="846a6-146">コード生成</span><span class="sxs-lookup"><span data-stu-id="846a6-146">Code generation</span></span>

### <a name="via-nswagstudio"></a><span data-ttu-id="846a6-147">NSwagStudio から</span><span class="sxs-lookup"><span data-stu-id="846a6-147">Via NSwagStudio</span></span>

* <span data-ttu-id="846a6-148">公式 [GitHub repository](https://github.com/RSuter/NSwag/wiki/NSwagStudio) リポジトリから NSwagStudio をインストールします。</span><span class="sxs-lookup"><span data-stu-id="846a6-148">Install NSwagStudio from the official [GitHub repository](https://github.com/RSuter/NSwag/wiki/NSwagStudio).</span></span>
* <span data-ttu-id="846a6-149">NSwagStudio を起動します。</span><span class="sxs-lookup"><span data-stu-id="846a6-149">Launch NSwagStudio.</span></span> <span data-ttu-id="846a6-150">**[Swagger Specification URL]** \(Swagger 仕様 URL\) テキストボックスに *swagger.json* ファイルの URL を入力し、**[Create local Copy]** \(ローカル コピーの作成\) ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="846a6-150">Enter the *swagger.json* file URL in the **Swagger Specification URL** textbox, and click the **Create local Copy** button.</span></span>
* <span data-ttu-id="846a6-151">**[CSharp Client]** \(CSharp クライアント\) クライアント出力の種類を選択します。</span><span class="sxs-lookup"><span data-stu-id="846a6-151">Select the **CSharp Client** client output type.</span></span> <span data-ttu-id="846a6-152">他の選択肢には、**[TypeScript Client]\(TypeScript クライアント\)**、**[CSharp Web API Controller]\(CSharp Web API コントローラー\)** があります。</span><span class="sxs-lookup"><span data-stu-id="846a6-152">Other options include **TypeScript Client** and **CSharp Web API Controller**.</span></span> <span data-ttu-id="846a6-153">Web API コントローラーは基本的に逆生成です。</span><span class="sxs-lookup"><span data-stu-id="846a6-153">Using a Web API Controller is basically a reverse generation.</span></span> <span data-ttu-id="846a6-154">サービスの仕様を利用してサービスを再構築します。</span><span class="sxs-lookup"><span data-stu-id="846a6-154">It uses a specification of a service to rebuild the service.</span></span>
* <span data-ttu-id="846a6-155">**[Generate Outputs]** \(出力の生成\) ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="846a6-155">Click the **Generate Outputs** button.</span></span> <span data-ttu-id="846a6-156">*TodoApi.NSwag* プロジェクトの完全な C# クライアントの実装が作成されます。</span><span class="sxs-lookup"><span data-stu-id="846a6-156">A complete C# client implementation of the *TodoApi.NSwag* project is produced.</span></span> <span data-ttu-id="846a6-157">**[出力]** セクションの **[CSharp Client]** \(CSharp クライアント\) タブをクリックし、生成されたクライアント コードを表示します。</span><span class="sxs-lookup"><span data-stu-id="846a6-157">Click the **CSharp Client** tab of the **Outputs** section to see the generated client code:</span></span>

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
> <span data-ttu-id="846a6-158">C# クライアント コードは、**[CSharp Client]** \(CSharp クライアント\) タブの **[設定]** タブに定義された設定に基づき生成されます。この設定を変更して、既定の名前空間の名前変更や同期メソッドの生成などのタスクを実行します。</span><span class="sxs-lookup"><span data-stu-id="846a6-158">The C# client code is generated based on settings defined in the **Settings** tab of the **CSharp Client** tab. Modify the settings to perform tasks such as default namespace renaming and synchronous method generation.</span></span>

* <span data-ttu-id="846a6-159">生成された C# コードは、クライアント プロジェクトのファイルに入れます (例: [Xamarin.Forms](/xamarin/xamarin-forms/) アプリ)。</span><span class="sxs-lookup"><span data-stu-id="846a6-159">Copy the generated C# code into a file in a client project (for example, a [Xamarin.Forms](/xamarin/xamarin-forms/) app).</span></span>
* <span data-ttu-id="846a6-160">Web API の使用を開始します。</span><span class="sxs-lookup"><span data-stu-id="846a6-160">Start consuming the web API:</span></span>

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
> <span data-ttu-id="846a6-161">ベース URL または HTTP クライアントを API クライアントに挿入できます。</span><span class="sxs-lookup"><span data-stu-id="846a6-161">You can inject a base URL and/or a HTTP client into the API client.</span></span> <span data-ttu-id="846a6-162">ベスト プラクティスは常に [HttpClient を再利用すること](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/)です。</span><span class="sxs-lookup"><span data-stu-id="846a6-162">The best practice is to always [reuse the HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).</span></span>

### <a name="other-ways-to-generate-client-code"></a><span data-ttu-id="846a6-163">クライアント コードを生成するその他の方法</span><span class="sxs-lookup"><span data-stu-id="846a6-163">Other ways to generate client code</span></span>

<span data-ttu-id="846a6-164">自分のワークフローにより適した、他の方法でクライアント コードを生成できます。</span><span class="sxs-lookup"><span data-stu-id="846a6-164">You can generate the client code in other ways, more suited to your workflow:</span></span>

* [<span data-ttu-id="846a6-165">MSBuild</span><span class="sxs-lookup"><span data-stu-id="846a6-165">MSBuild</span></span>](https://www.nuget.org/packages/NSwag.MSBuild/)

* [<span data-ttu-id="846a6-166">コードで</span><span class="sxs-lookup"><span data-stu-id="846a6-166">In code</span></span>](https://github.com/NSwag/NSwag/wiki/SwaggerToCSharpClientGenerator)

* [<span data-ttu-id="846a6-167">T4 テンプレート</span><span class="sxs-lookup"><span data-stu-id="846a6-167">T4 templates</span></span>](https://github.com/NSwag/NSwag/wiki/T4)

## <a name="customize"></a><span data-ttu-id="846a6-168">カスタマイズ</span><span class="sxs-lookup"><span data-stu-id="846a6-168">Customize</span></span>

<span data-ttu-id="846a6-169">Swagger には、Web API の消費を容易にする、オブジェクト モデルをドキュメント化するオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="846a6-169">Swagger provides options for documenting the object model to ease consumption of the web API.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="846a6-170">API 情報と説明</span><span class="sxs-lookup"><span data-stu-id="846a6-170">API info and description</span></span>

<span data-ttu-id="846a6-171">`Startup.Configure`メソッドでは、`UseSwagger` メソッドに渡された構成アクションによって、作成者、ライセンス、説明などの情報が追加されます。</span><span class="sxs-lookup"><span data-stu-id="846a6-171">In the `Startup.Configure` method, a configuration action passed to the `UseSwagger` method adds information such as the author, license, and description:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup2.cs?name=snippet_UseSwagger)]

<span data-ttu-id="846a6-172">Swagger UI には、バージョンの情報が表示されます。</span><span class="sxs-lookup"><span data-stu-id="846a6-172">The Swagger UI displays the version's information:</span></span>

![バージョン情報を含む Swagger UI](web-api-help-pages-using-swagger/_static/custom-info-nswag.png)

### <a name="xml-comments"></a><span data-ttu-id="846a6-174">XML コメント</span><span class="sxs-lookup"><span data-stu-id="846a6-174">XML comments</span></span>

<span data-ttu-id="846a6-175">XML コメントは次の方法で有効になります。</span><span class="sxs-lookup"><span data-stu-id="846a6-175">XML comments are enabled with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio-xml"></a>[<span data-ttu-id="846a6-176">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="846a6-176">Visual Studio</span></span>](#tab/visual-studio-xml/)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="846a6-177">**ソリューション エクスプローラー**でプロジェクトを右クリックし、**[<project_name>.csproj の編集]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="846a6-177">Right-click the project in **Solution Explorer** and select **Edit <project_name>.csproj**.</span></span>
* <span data-ttu-id="846a6-178">強調表示された行を手動で *.csproj* ファイルに追加します。</span><span class="sxs-lookup"><span data-stu-id="846a6-178">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="846a6-179">**ソリューション エクスプローラー**で、プロジェクトを右クリックし、**[プロパティ]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="846a6-179">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="846a6-180">**[ビルド]** タブの **[出力]** セクションの下にある **[XML ドキュメント ファイル]** チェック ボックスをオンにします。</span><span class="sxs-lookup"><span data-stu-id="846a6-180">Check the **XML documentation file** box under the **Output** section of the **Build** tab</span></span>

::: moniker-end

# <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[<span data-ttu-id="846a6-181">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="846a6-181">Visual Studio for Mac</span></span>](#tab/visual-studio-mac-xml/)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="846a6-182">*Solution Pad* から **Ctrl** キーを押しながらプロジェクト名をクリックします。</span><span class="sxs-lookup"><span data-stu-id="846a6-182">From the *Solution Pad*, press **control** and click the project name.</span></span> <span data-ttu-id="846a6-183">**[ツール]** > **[ファイルの編集]** に移動します。</span><span class="sxs-lookup"><span data-stu-id="846a6-183">Navigate to **Tools** > **Edit File**.</span></span>
* <span data-ttu-id="846a6-184">強調表示された行を手動で *.csproj* ファイルに追加します。</span><span class="sxs-lookup"><span data-stu-id="846a6-184">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="846a6-185">**[プロジェクト オプション]** ダイアログ > **[ビルド]**>**[コンパイラ]** を開きます。</span><span class="sxs-lookup"><span data-stu-id="846a6-185">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="846a6-186">**[全般オプション]** セクションの下にある **[XML ドキュメントを生成する]** チェック ボックスをオンにします。</span><span class="sxs-lookup"><span data-stu-id="846a6-186">Check the **Generate xml documentation** box under the **General Options** section</span></span>

::: moniker-end

# <a name="visual-studio-codetabvisual-studio-code-xml"></a>[<span data-ttu-id="846a6-187">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="846a6-187">Visual Studio Code</span></span>](#tab/visual-studio-code-xml/)

<span data-ttu-id="846a6-188">強調表示された行を手動で *.csproj* ファイルに追加します。</span><span class="sxs-lookup"><span data-stu-id="846a6-188">Manually add the highlighted lines to the *.csproj* file:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

---

### <a name="data-annotations"></a><span data-ttu-id="846a6-189">データの注釈</span><span class="sxs-lookup"><span data-stu-id="846a6-189">Data annotations</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="846a6-190">NSwag では、[Reflection](/dotnet/csharp/programming-guide/concepts/reflection) が使用されます。Web API アクションに推奨される戻り値の型は [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) です。</span><span class="sxs-lookup"><span data-stu-id="846a6-190">NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span></span> <span data-ttu-id="846a6-191">結果的に、NSwag では、アクションの内容とそれで返されるものを推論できません。</span><span class="sxs-lookup"><span data-stu-id="846a6-191">Consequently, NSwag can't infer what your action is doing and what it returns.</span></span> <span data-ttu-id="846a6-192">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="846a6-192">Consider the following example:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

<span data-ttu-id="846a6-193">先行するアクションによって `IActionResult` が返されますが、このアクションの内部では [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) または [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest) を返しています。</span><span class="sxs-lookup"><span data-stu-id="846a6-193">The preceding action returns `IActionResult`, but inside the action it's returning either [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) or [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest).</span></span> <span data-ttu-id="846a6-194">データ注釈は、このアクションによって返される既知の HTTP 応答コードをクライアントに通知します。</span><span class="sxs-lookup"><span data-stu-id="846a6-194">Data annotations are used to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="846a6-195">アクションに次の属性を追加します。</span><span class="sxs-lookup"><span data-stu-id="846a6-195">Decorate the action with the following attributes:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="846a6-196">NSwag では、[Reflection](/dotnet/csharp/programming-guide/concepts/reflection) が使用されます。Web API アクションに推奨される戻り値の型は [ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1) です。</span><span class="sxs-lookup"><span data-stu-id="846a6-196">NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1).</span></span> <span data-ttu-id="846a6-197">その結果、NSwag では `T` によって定義された戻り値のみ推論できます。</span><span class="sxs-lookup"><span data-stu-id="846a6-197">Consequently, NSwag can only infer the return type defined by `T`.</span></span> <span data-ttu-id="846a6-198">アクションのその他の可能な戻り値は推論できません。</span><span class="sxs-lookup"><span data-stu-id="846a6-198">Other possible return types in the action cannot be inferred.</span></span> <span data-ttu-id="846a6-199">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="846a6-199">Consider the following example:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

<span data-ttu-id="846a6-200">先行するアクションによって `ActionResult<T>` が返されますが、このアクションの内部では [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) を返しています。</span><span class="sxs-lookup"><span data-stu-id="846a6-200">The preceding action returns `ActionResult<T>`, but inside the action it's returning either [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute).</span></span> <span data-ttu-id="846a6-201">コントローラーは、[[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) 属性で修飾されるため、応答として [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest) も可能です。</span><span class="sxs-lookup"><span data-stu-id="846a6-201">Since the controller is decorated with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute, a [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest) response is possible too.</span></span> <span data-ttu-id="846a6-202">詳細については、「[自動的な HTTP 400 応答](xref:web-api/index#automatic-http-400-responses)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="846a6-202">See [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses) for more info.</span></span> <span data-ttu-id="846a6-203">データ注釈は、このアクションによって返される既知の HTTP 応答コードをクライアントに通知します。</span><span class="sxs-lookup"><span data-stu-id="846a6-203">Data annotations are used to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="846a6-204">アクションに次の属性を追加します。</span><span class="sxs-lookup"><span data-stu-id="846a6-204">Decorate the action with the following attributes:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]
::: moniker-end

<span data-ttu-id="846a6-205">Swagger ジェネレーターでは、このアクションを正確に表現できるようになりました。生成されたクライアントでは、エンドポイントの呼び出し時に受け取るものが認識されます。</span><span class="sxs-lookup"><span data-stu-id="846a6-205">The Swagger generator can now accurately describe this action, and generated clients know what they receive when calling the endpoint.</span></span> <span data-ttu-id="846a6-206">すべてのアクションにこれらの属性を追加することを強くお勧めします。</span><span class="sxs-lookup"><span data-stu-id="846a6-206">Decorating all actions with these attributes is highly recommended.</span></span> <span data-ttu-id="846a6-207">API アクションで返される HTTP 応答のガイドラインについては、[RFC 7231 仕様](https://tools.ietf.org/html/rfc7231#section-4.3)をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="846a6-207">For guidelines on what HTTP responses your API actions should return, see the [RFC 7231 specification](https://tools.ietf.org/html/rfc7231#section-4.3).</span></span>
