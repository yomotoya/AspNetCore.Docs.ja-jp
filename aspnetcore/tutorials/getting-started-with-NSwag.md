---
title: NSwag と ASP.NET Core の概要
author: zuckerthoben
description: NSwag を使用し、ASP.NET Core Web API アプリのドキュメント ページとヘルプ ページを生成する方法について説明します。
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 03/15/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: 80e6a9e1702d8f68d139d2ff9c3a01a27c40cecb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-nswag-and-aspnet-core"></a><span data-ttu-id="aaf7c-103">NSwag と ASP.NET Core の概要</span><span class="sxs-lookup"><span data-stu-id="aaf7c-103">Get started with NSwag and ASP.NET Core</span></span>

<span data-ttu-id="aaf7c-104">作成者: [Christoph Nienaber](https://twitter.com/zuckerthoben) / [Rico Suter](https://rsuter.com)</span><span class="sxs-lookup"><span data-stu-id="aaf7c-104">By [Christoph Nienaber](https://twitter.com/zuckerthoben) and [Rico Suter](https://rsuter.com)</span></span>

<span data-ttu-id="aaf7c-105">[NSwag](https://github.com/RSuter/NSwag) と共に ASP.NET Core ミドルウェアを使用するには、[NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet パッケージが必要です。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-105">Using [NSwag](https://github.com/RSuter/NSwag) with ASP.NET Core middleware requires the [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet package.</span></span> <span data-ttu-id="aaf7c-106">このパッケージは、Swagger ジェネレーター、Swagger UI (v2 と v3)、[ReDoc UI](https://github.com/Rebilly/ReDoc) で構成されています。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-106">The package consists of a Swagger generator, Swagger UI (v2 and v3), and [ReDoc UI](https://github.com/Rebilly/ReDoc).</span></span>

<span data-ttu-id="aaf7c-107">NSwag のコード生成機能を利用することを強くお勧めします。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-107">It's highly recommended to make use of NSwag's code generation capabilities.</span></span> <span data-ttu-id="aaf7c-108">コード生成に次のいずれかのオプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-108">Choose one of the following options for code generation:</span></span>

* <span data-ttu-id="aaf7c-109">[NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio) を使用します。これは API のクライアント コードを C# と TypeScript で生成するための Windows デスクトップ アプリです。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-109">Use [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), a Windows desktop app for generating client code in C# and TypeScript for your API</span></span>
* <span data-ttu-id="aaf7c-110">[NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) または [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet パッケージを使用し、プロジェクト内でコードを生成します。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-110">Use the [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) or [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet packages to do code generation inside your project</span></span>
* <span data-ttu-id="aaf7c-111">[コマンド ライン](https://github.com/NSwag/NSwag/wiki/CommandLine)から NSwag を使用します。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-111">Use NSwag from the [command line](https://github.com/NSwag/NSwag/wiki/CommandLine)</span></span>
* <span data-ttu-id="aaf7c-112">[NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) NuGet パッケージを使用します。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-112">Use the [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) NuGet package</span></span>

## <a name="features"></a><span data-ttu-id="aaf7c-113">フィーチャー</span><span class="sxs-lookup"><span data-stu-id="aaf7c-113">Features</span></span>

<span data-ttu-id="aaf7c-114">NSwag を使用する主な理由は、Swagger UI と Swagger ジェネレーターを導入できることのみならず、柔軟なコード生成機能を利用できることにあります。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-114">The main reason to use NSwag is the ability to not only introduce the Swagger UI and Swagger generator, but to make use of the flexible code generation capabilities.</span></span> <span data-ttu-id="aaf7c-115">既存の API は必要ありません&mdash;Swagger が組み込まれているサードパーティ製の API を利用し、NSwag にクライアント実装を生成させることができます。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-115">You don't need an existing API&mdash;you can use third-party APIs that incorporate Swagger and let NSwag generate a client implementation.</span></span> <span data-ttu-id="aaf7c-116">いずれの方法でも、開発周期が早まり、API 変更に合わせた調整が簡単になります。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-116">Either way, the development cycle is expedited and you can more easily adapt to API changes.</span></span>

## <a name="package-installation"></a><span data-ttu-id="aaf7c-117">パッケージ インストール</span><span class="sxs-lookup"><span data-stu-id="aaf7c-117">Package installation</span></span>

<span data-ttu-id="aaf7c-118">NSwag NuGet パッケージは、次の方法で追加できます。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-118">The NSwag NuGet package can be added with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="aaf7c-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="aaf7c-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="aaf7c-120">**[パッケージ マネージャー コンソール]** ウィンドウから:</span><span class="sxs-lookup"><span data-stu-id="aaf7c-120">From the **Package Manager Console** window:</span></span>

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* <span data-ttu-id="aaf7c-121">**[NuGet パッケージの管理]** ダイアログ ボックスから:</span><span class="sxs-lookup"><span data-stu-id="aaf7c-121">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="aaf7c-122">**[ソリューション エクスプローラー]** > **[NuGet パッケージの管理]** でプロジェクトを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-122">Right-click your project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="aaf7c-123">**パッケージ ソース**を "nuget.org" に設定します。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-123">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="aaf7c-124">検索ボックスに「NSwag.AspNetCore」と入力します。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-124">Enter "NSwag.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="aaf7c-125">**[参照]** タブから "NSwag.AspNetCore" パッケージを選択して、**[インストール]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-125">Select the "NSwag.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="aaf7c-126">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="aaf7c-126">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="aaf7c-127">**[Solution Pad]** > **[パッケージを追加]** で [*パッケージ*] フォルダーを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-127">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="aaf7c-128">**[パッケージを追加]** ウィンドウの **[ソース]** ドロップダウンを "nuget.org" に設定します。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-128">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="aaf7c-129">検索ボックスに「NSwag.AspNetCore」と入力します。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-129">Enter NSwag.AspNetCore in the search box</span></span>
* <span data-ttu-id="aaf7c-130">結果ウィンドウから NSwag.AspNetCore パッケージを選択し、**[パッケージを追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-130">Select the NSwag.AspNetCore package from the results pane and click **Add Package**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="aaf7c-131">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="aaf7c-131">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="aaf7c-132">**統合端末**からから次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-132">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.NSwag.csproj package NSwag.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="aaf7c-133">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="aaf7c-133">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="aaf7c-134">次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-134">Run the following command:</span></span>

```console
dotnet add TodoApi.NSwag.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="aaf7c-135">Swagger ミドルウェアを追加して構成する</span><span class="sxs-lookup"><span data-stu-id="aaf7c-135">Add and configure Swagger middleware</span></span>

<span data-ttu-id="aaf7c-136">`Info` クラスで次の名前空間をインポートします。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-136">Import the following namespaces in the `Info` class:</span></span>

```csharp
using NSwag.AspNetCore;
using System.Reflection;
using NJsonSchema;
```

<span data-ttu-id="aaf7c-137">`Startup.Configure` メソッドで、生成された Swagger 仕様と SwaggerUI 対応のミドルウェアを有効にします。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-137">In the `Startup.Configure` method, enable the middleware for serving the generated Swagger specification and the Swagger UI:</span></span>

[!code-cs[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=4,7-10)]

<span data-ttu-id="aaf7c-138">アプリを起動します。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-138">Launch the app.</span></span> <span data-ttu-id="aaf7c-139">`/swagger` に移動し、Swagger UI を表示します。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-139">Navigate to `/swagger` to view the Swagger UI.</span></span> <span data-ttu-id="aaf7c-140">`/swagger/v1/swagger.json` に移動し、Swagger 仕様を表示します。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-140">Navigate to `/swagger/v1/swagger.json` to view the Swagger specification.</span></span>

## <a name="code-generation"></a><span data-ttu-id="aaf7c-141">コード生成</span><span class="sxs-lookup"><span data-stu-id="aaf7c-141">Code generation</span></span>

### <a name="via-nswagstudio"></a><span data-ttu-id="aaf7c-142">NSwagStudio から</span><span class="sxs-lookup"><span data-stu-id="aaf7c-142">Via NSwagStudio</span></span>

* <span data-ttu-id="aaf7c-143">公式 [GitHub リポジトリ](https://github.com/RSuter/NSwag/wiki/NSwagStudio)から `NSwagStudio` をインストールします。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-143">Install `NSwagStudio` from the official [GitHub repository](https://github.com/RSuter/NSwag/wiki/NSwagStudio).</span></span>

* <span data-ttu-id="aaf7c-144">NSwagStudio を起動します。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-144">Launch NSwagStudio.</span></span> <span data-ttu-id="aaf7c-145">*swagger.json* の場所を入力するか、直接コピーします。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-145">Enter the location of your *swagger.json* or copy it directly:</span></span>

![NSwagStudio](web-api-help-pages-using-swagger/_static/NSwagStudio.png)

* <span data-ttu-id="aaf7c-147">クライアントの出力の種類を指定します。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-147">Indicate the desired client output type.</span></span> <span data-ttu-id="aaf7c-148">選択肢には、**[TypeScript Client]\(TypeScript クライアント\)**、**[CSharp Client]\(CSharp クライアント\)**、**[CSharp Web API Controller]\(CSharp Web API コントローラー\)** があります。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-148">Options include **TypeScript Client**, **CSharp Client**, or **CSharp Web API Controller**.</span></span> <span data-ttu-id="aaf7c-149">Web API コントローラーは基本的に逆生成です。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-149">Using a Web API Controller is basically a reverse generation.</span></span> <span data-ttu-id="aaf7c-150">サービスの仕様を利用してサービスを再構築します。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-150">It uses a specification of a service to rebuild the service.</span></span>

* <span data-ttu-id="aaf7c-151">**[Generate Outputs]\(出力の生成\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-151">Click **Generate Outputs**.</span></span>

* <span data-ttu-id="aaf7c-152">C# の TodoApi.NSwag クライアント実装の完全なサンプルは*こちらで*ご覧いただけます。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-152">Here you see a complete client implementation of the *TodoApi.NSwag* sample in C#:</span></span>

![NSwagStudio - 出力](web-api-help-pages-using-swagger/_static/NSwagStudio-Output.png)

* <span data-ttu-id="aaf7c-154">ファイルをクライアント プロジェクトに入れます (例: [Xamarin.Forms](/xamarin/xamarin-forms/) アプリ)。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-154">Put the file into a client project (for example, a [Xamarin.Forms](/xamarin/xamarin-forms/) app).</span></span> <span data-ttu-id="aaf7c-155">API の使用を開始します。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-155">Start consuming the API:</span></span>

```csharp
var todoClient = new TodoClient();

// Gets all Todos from the Api
var allTodos = await todoClient.GetAllAsync();

// Create a new TodoItem and save it in the Api
var createdTodo = await todoClient.CreateAsync(new TodoItem);

// Get a single Todo by Id
var foundTodo = await todoClient.GetByIdAsync(1);
```

> [!NOTE]
> <span data-ttu-id="aaf7c-156">ベース URL または HTTP クライアントを API クライアントに挿入できます。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-156">You can inject a base URL and/or a HTTP client into the API client.</span></span> <span data-ttu-id="aaf7c-157">ベスト プラクティスは常に [HttpClient を再利用すること](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/)です。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-157">The best practice is to always [reuse the HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).</span></span>

<span data-ttu-id="aaf7c-158">これで API をクライアント プロジェクトに簡単に実装できます。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-158">You can now start implementing your API into client projects easily.</span></span>

### <a name="other-ways-to-generate-client-code"></a><span data-ttu-id="aaf7c-159">クライアント コードを生成するその他の方法</span><span class="sxs-lookup"><span data-stu-id="aaf7c-159">Other ways to generate client code</span></span>

<span data-ttu-id="aaf7c-160">自分のワークフローにより適した、他の方法でコードを生成できます。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-160">You can generate the code in other ways, more suited to your workflow:</span></span>

* [<span data-ttu-id="aaf7c-161">MSBuild</span><span class="sxs-lookup"><span data-stu-id="aaf7c-161">MSBuild</span></span>](https://www.nuget.org/packages/NSwag.MSBuild/)

* [<span data-ttu-id="aaf7c-162">コードで</span><span class="sxs-lookup"><span data-stu-id="aaf7c-162">In code</span></span>](https://github.com/NSwag/NSwag/wiki/SwaggerToCSharpClientGenerator)

* [<span data-ttu-id="aaf7c-163">T4 テンプレート</span><span class="sxs-lookup"><span data-stu-id="aaf7c-163">T4 templates</span></span>](https://github.com/NSwag/NSwag/wiki/T4)

## <a name="customization"></a><span data-ttu-id="aaf7c-164">カスタマイズ</span><span class="sxs-lookup"><span data-stu-id="aaf7c-164">Customization</span></span>

### <a name="xml-comments"></a><span data-ttu-id="aaf7c-165">XML コメント</span><span class="sxs-lookup"><span data-stu-id="aaf7c-165">XML comments</span></span>

<span data-ttu-id="aaf7c-166">XML コメントは次の方法で有効になります。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-166">XML comments are enabled with the following approaches:</span></span>

#### <a name="visual-studiotabvisual-studio-xml"></a>[<span data-ttu-id="aaf7c-167">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="aaf7c-167">Visual Studio</span></span>](#tab/visual-studio-xml/)
* <span data-ttu-id="aaf7c-168">**ソリューション エクスプローラー**で、プロジェクトを右クリックし、**[プロパティ]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-168">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="aaf7c-169">**[ビルド]** タブの **[出力]** セクションの下にある **[XML ドキュメント ファイル]** チェック ボックスをオンにします。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-169">Check the **XML documentation file** box under the **Output** section of the **Build** tab:</span></span>

![プロジェクトのプロパティの [ビルド] タブ](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

#### <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[<span data-ttu-id="aaf7c-171">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="aaf7c-171">Visual Studio for Mac</span></span>](#tab/visual-studio-mac-xml/)
* <span data-ttu-id="aaf7c-172">**[プロジェクト オプション]** ダイアログ > **[ビルド]** > **[コンパイラ]** を開きます。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-172">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="aaf7c-173">**[全般オプション]** セクションの下にある **[XML ドキュメントを生成する]** チェック ボックスをオンにします。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-173">Check the **Generate xml documentation** box under the **General Options** section:</span></span>

![プロジェクトのオプションの [全般オプション] セクション](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

#### <a name="visual-studio-codetabvisual-studio-code-xml"></a>[<span data-ttu-id="aaf7c-175">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="aaf7c-175">Visual Studio Code</span></span>](#tab/visual-studio-code-xml/)
<span data-ttu-id="aaf7c-176">次のスニペットを *.csproj* ファイルに手動で追加します。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-176">Manually add the following snippet to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.NSwag/TodoApiNSwag.csproj?range=7-9)]

* * *
## <a name="data-annotations"></a><span data-ttu-id="aaf7c-177">データの注釈</span><span class="sxs-lookup"><span data-stu-id="aaf7c-177">Data annotations</span></span>

<span data-ttu-id="aaf7c-178">NSwag では、[Reflection](/dotnet/csharp/programming-guide/concepts/reflection) が使用されます。Web API アクションのベスト プラクティスは [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) を返すことです。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-178">NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the best practice for Web API actions is to return [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span></span> <span data-ttu-id="aaf7c-179">結果的に、NSwag では、アクションの内容とそれで返されるものを推論できません。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-179">Consequently, NSwag can't infer what your action is doing and what it returns.</span></span> <span data-ttu-id="aaf7c-180">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-180">Consider the following example:</span></span>

```csharp
public IActionResult Create([FromBody] TodoItem item)
{
    if (item == null)
    {
        return BadRequest();
    }

    _context.TodoItems.Add(item);
    _context.SaveChanges();

     return CreatedAtRoute("GetTodo", new { id = item.Id }, item);
}
```

<span data-ttu-id="aaf7c-181">先行するアクションによって `IActionResult` が返されますが、このアクションの内部では [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) または [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest) を返しています。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-181">The preceding action returns `IActionResult`, but inside the action it's returning either [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) or [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest).</span></span> <span data-ttu-id="aaf7c-182">データ注釈を利用し、このアクションによって返される HTTP 応答をクライアントに通知します。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-182">Data annotations are used to tell clients which HTTP response this action is returning.</span></span> <span data-ttu-id="aaf7c-183">アクションに次の属性を追加します。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-183">Decorate the action with the following attributes:</span></span>

```csharp
[HttpPost]
[ProducesResponseType(typeof(TodoItem), 201)] // Created
[ProducesResponseType(typeof(TodoItem), 400)] // BadRequest
public IActionResult Create([FromBody] TodoItem item)
```

<span data-ttu-id="aaf7c-184">Swagger ジェネレーターでは、このアクションを正確に表現できるようになりました。生成されたクライアントでは、エンドポイントの呼び出し時に受け取るものが認識されます。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-184">The Swagger generator can now accurately describe this action, and generated clients know what they receive when calling the endpoint.</span></span> <span data-ttu-id="aaf7c-185">すべてのアクションにこれらの属性を追加することを強くお勧めします。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-185">Decorating all actions with these attributes is highly recommended.</span></span> <span data-ttu-id="aaf7c-186">API アクションで返される HTTP 応答のガイドラインについては、[RFC 7231 仕様](https://tools.ietf.org/html/rfc7231#section-4.3)をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="aaf7c-186">For guidelines on what HTTP responses your API actions should return, see the [RFC 7231 specification](https://tools.ietf.org/html/rfc7231#section-4.3).</span></span>
