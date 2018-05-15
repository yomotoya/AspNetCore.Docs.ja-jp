---
title: Swashbuckle と ASP.NET Core の概要
author: zuckerthoben
description: Swashbuckle を ASP.NET Core プロジェクトに追加し、Swagger UI を統合する方法について説明します。
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 03/26/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/get-started-with-swashbuckle
ms.openlocfilehash: e90339f2884dd9b20cf135f879c9cab6110efecf
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-swashbuckle-and-aspnet-core"></a><span data-ttu-id="4ee1e-103">Swashbuckle と ASP.NET Core の概要</span><span class="sxs-lookup"><span data-stu-id="4ee1e-103">Get started with Swashbuckle and ASP.NET Core</span></span>

<span data-ttu-id="4ee1e-104">[Shayne Boyer](https://twitter.com/spboyer) および [Scott Addie](https://twitter.com/Scott_Addie) 著</span><span class="sxs-lookup"><span data-stu-id="4ee1e-104">By [Shayne Boyer](https://twitter.com/spboyer) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="4ee1e-105">Swashbuckle には 3 つの主要なコンポーネントがあります。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-105">There are three main components to Swashbuckle:</span></span>

* <span data-ttu-id="4ee1e-106">[Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/): `SwaggerDocument` オブジェクトを JSON エンドポイントとして公開するための Swagger オブジェクト モデルとミドルウェア。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-106">[Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/): a Swagger object model and middleware to expose `SwaggerDocument` objects as JSON endpoints.</span></span>

* <span data-ttu-id="4ee1e-107">[Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/): ルート、コントローラー、モデルから直接 `SwaggerDocument` オブジェクトを構築する Swagger ジェネレーター。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-107">[Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/): a Swagger generator that builds `SwaggerDocument` objects directly from your routes, controllers, and models.</span></span> <span data-ttu-id="4ee1e-108">通常、Swagger エンドポイント ミドルウェアと組み合わせて Swagger JSON を自動的に公開します。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-108">It's typically combined with the Swagger endpoint middleware to automatically expose Swagger JSON.</span></span>

* <span data-ttu-id="4ee1e-109">[Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): Swagger UI ツールの埋め込みバージョン。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-109">[Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): an embedded version of the Swagger UI tool.</span></span> <span data-ttu-id="4ee1e-110">Swagger JSON を解釈して、Web API の機能を説明するカスタマイズ可能な機能豊富なエクスペリエンスを構築します。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-110">It interprets Swagger JSON to build a rich, customizable experience for describing the Web API functionality.</span></span> <span data-ttu-id="4ee1e-111">これには、パブリック メソッド用の組み込みのテスト ハーネスが含まれます。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-111">It includes built-in test harnesses for the public methods.</span></span>

## <a name="package-installation"></a><span data-ttu-id="4ee1e-112">パッケージ インストール</span><span class="sxs-lookup"><span data-stu-id="4ee1e-112">Package installation</span></span>

<span data-ttu-id="4ee1e-113">Swashbuckle は、次の方法で追加できます。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-113">Swashbuckle can be added with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4ee1e-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4ee1e-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="4ee1e-115">**[パッケージ マネージャー コンソール]** ウィンドウから:</span><span class="sxs-lookup"><span data-stu-id="4ee1e-115">From the **Package Manager Console** window:</span></span>

    ```powershell
    Install-Package Swashbuckle.AspNetCore
    ```

* <span data-ttu-id="4ee1e-116">**[NuGet パッケージの管理]** ダイアログ ボックスから:</span><span class="sxs-lookup"><span data-stu-id="4ee1e-116">From the **Manage NuGet Packages** dialog:</span></span>

  * <span data-ttu-id="4ee1e-117">**[ソリューション エクスプローラー]** > **[NuGet パッケージの管理]** でプロジェクトを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-117">Right-click your project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="4ee1e-118">**パッケージ ソース**を "nuget.org" に設定します。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-118">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="4ee1e-119">検索ボックスに「Swashbuckle.AspNetCore」と入力します。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-119">Enter "Swashbuckle.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="4ee1e-120">**[参照]** タブから "Swashbuckle.AspNetCore"パッケージを選択して、**[インストール]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-120">Select the "Swashbuckle.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="4ee1e-121">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="4ee1e-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="4ee1e-122">**[Solution Pad]** > **[パッケージを追加]** で [*パッケージ*] フォルダーを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-122">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="4ee1e-123">**[パッケージを追加]** ウィンドウの **[ソース]** ドロップダウンを "nuget.org" に設定します。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-123">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="4ee1e-124">検索ボックスに「Swashbuckle.AspNetCore」と入力します。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-124">Enter Swashbuckle.AspNetCore in the search box</span></span>
* <span data-ttu-id="4ee1e-125">結果ウィンドウから Swashbuckle.AspNetCore パッケージを選択し、**[パッケージを追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-125">Select the Swashbuckle.AspNetCore package from the results pane and click **Add Package**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4ee1e-126">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4ee1e-126">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="4ee1e-127">**統合端末**からから次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-127">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="4ee1e-128">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="4ee1e-128">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="4ee1e-129">次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-129">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="4ee1e-130">Swagger ミドルウェアを追加して構成する</span><span class="sxs-lookup"><span data-stu-id="4ee1e-130">Add and configure Swagger middleware</span></span>

<span data-ttu-id="4ee1e-131">`Startup.ConfigureServices` メソッドで Swagger ジェネレーターをサービス コレクションに追加します。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-131">Add the Swagger generator to the services collection in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=7-10)]

<span data-ttu-id="4ee1e-132">次の名前空間をインポートし、`Info` クラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-132">Import the following namespace to use the `Info` class:</span></span>

```csharp
using Swashbuckle.AspNetCore.Swagger;
```

<span data-ttu-id="4ee1e-133">`Startup.Configure` メソッドで、生成された JSON ドキュメントと Swagger UI 対応のミドルウェアを有効にします。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-133">In the `Startup.Configure` method, enable the middleware for serving the generated JSON document and the Swagger UI:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Startup2.cs?name=snippet_Configure&highlight=4,7-10)]

<span data-ttu-id="4ee1e-134">アプリを起動し、`http://localhost:<random_port>/swagger/v1/swagger.json` に移動します。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-134">Launch the app, and navigate to `http://localhost:<random_port>/swagger/v1/swagger.json`.</span></span> <span data-ttu-id="4ee1e-135">[Swagger 仕様 (swagger.json)](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson) に基づき、エンドポイントについて説明するドキュメントが生成され、表示されます。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-135">The generated document describing the endpoints appears as shown in [Swagger specification (swagger.json)](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson).</span></span>

<span data-ttu-id="4ee1e-136">Swagger UI は `http://localhost:<random_port>/swagger` にあります。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-136">The Swagger UI can be found at `http://localhost:<random_port>/swagger`.</span></span> <span data-ttu-id="4ee1e-137">Swagger UI から API を探し、他のプログラムに組み込みます。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-137">Explore the API via Swagger UI and incorporate it in other programs.</span></span>

> [!TIP]
> <span data-ttu-id="4ee1e-138">アプリのルート (`http://localhost:<random_port>/`) で Swagger UI にサービスを提供するには、`RoutePrefix` プロパティを空の文字列に設定します。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-138">To serve the Swagger UI at the app's root (`http://localhost:<random_port>/`), set the `RoutePrefix` property to an empty string:</span></span>
> 
> [!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Startup3.cs?name=snippet_UseSwaggerUI&highlight=4)]

## <a name="customize--extend"></a><span data-ttu-id="4ee1e-139">カスタマイズと拡張</span><span class="sxs-lookup"><span data-stu-id="4ee1e-139">Customize & extend</span></span>

<span data-ttu-id="4ee1e-140">Swagger は、テーマに合わせてオブジェクト モデルを文書化して、UI をカスタマイズするオプションを提供します。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-140">Swagger provides options for documenting the object model and customizing the UI to match your theme.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="4ee1e-141">API 情報と説明</span><span class="sxs-lookup"><span data-stu-id="4ee1e-141">API info and description</span></span>

<span data-ttu-id="4ee1e-142">`AddSwaggerGen` メソッドに渡された構成アクションによって、作成者、ライセンス、説明などの情報が追加されます。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-142">The configuration action passed to the `AddSwaggerGen` method adds information such as the author, license, and description:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Startup.cs?range=21-40,46)]

<span data-ttu-id="4ee1e-143">Swagger UI には、バージョンの情報が表示されます。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-143">The Swagger UI displays the version's information:</span></span>

![バージョン情報を含む Swagger UI: 説明、作成者、およびその他のリンクの表示](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a><span data-ttu-id="4ee1e-145">XML コメント</span><span class="sxs-lookup"><span data-stu-id="4ee1e-145">XML comments</span></span>

<span data-ttu-id="4ee1e-146">XML コメントは、次の方法で有効にすることができます。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-146">XML comments can be enabled with the following approaches:</span></span>

#### <a name="visual-studiotabvisual-studio-xml"></a>[<span data-ttu-id="4ee1e-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4ee1e-147">Visual Studio</span></span>](#tab/visual-studio-xml/)
* <span data-ttu-id="4ee1e-148">**ソリューション エクスプローラー**で、プロジェクトを右クリックし、**[プロパティ]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-148">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="4ee1e-149">**[ビルド]** タブの **[出力]** セクションの下にある **[XML ドキュメント ファイル]** チェック ボックスをオンにします。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-149">Check the **XML documentation file** box under the **Output** section of the **Build** tab:</span></span>

![プロジェクトのプロパティの [ビルド] タブ](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

#### <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[<span data-ttu-id="4ee1e-151">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="4ee1e-151">Visual Studio for Mac</span></span>](#tab/visual-studio-mac-xml/)
* <span data-ttu-id="4ee1e-152">**[プロジェクト オプション]** ダイアログ > **[ビルド]** > **[コンパイラ]** を開きます。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-152">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="4ee1e-153">**[全般オプション]** セクションの下にある **[XML ドキュメントを生成する]** チェック ボックスをオンにします。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-153">Check the **Generate xml documentation** box under the **General Options** section:</span></span>

![プロジェクトのオプションの [全般オプション] セクション](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

#### <a name="visual-studio-codetabvisual-studio-code-xml"></a>[<span data-ttu-id="4ee1e-155">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4ee1e-155">Visual Studio Code</span></span>](#tab/visual-studio-code-xml/)
<span data-ttu-id="4ee1e-156">次のスニペットを *.csproj* ファイルに手動で追加します。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-156">Manually add the following snippet to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=2)]

* * *
<span data-ttu-id="4ee1e-157">XML コメントを有効にすると、ドキュメントに未記載のパブリック型とメンバーのデバッグ情報を提供することができます。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-157">Enabling XML comments provides debug information for undocumented public types and members.</span></span> <span data-ttu-id="4ee1e-158">文書化されない型とメンバーは警告メッセージで示されます。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-158">Undocumented types and members are indicated by the warning message.</span></span> <span data-ttu-id="4ee1e-159">たとえば、次のメッセージは警告コード 1591 の違反を示します。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-159">For example, the following message indicates a violation of warning code 1591:</span></span>

```text
warning CS1591: Missing XML comment for publicly visible type or member 'TodoController.GetAll()'
```

<span data-ttu-id="4ee1e-160">警告を非表示にするには、*.csproj* ファイルで、無視する警告コードの一覧をセミコロン区切りで定義します。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-160">Suppress warnings by defining a semicolon-delimited list of warning codes to ignore in the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=3)]

<span data-ttu-id="4ee1e-161">生成された XML ファイルを使用するように Swagger を構成します。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-161">Configure Swagger to use the generated XML file.</span></span> <span data-ttu-id="4ee1e-162">Linux または Windows 以外のオペレーティング システムでは、ファイル名やパスで大文字小文字が区別されます。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-162">For Linux or non-Windows operating systems, file names and paths can be case-sensitive.</span></span> <span data-ttu-id="4ee1e-163">たとえば、*TodoApi.XML* ファイルは Windows では有効ですが、CentOS では無効です。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-163">For example, a *TodoApi.XML* file is valid on Windows but not CentOS.</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=29-31)]

<span data-ttu-id="4ee1e-164">前述のコードでは、[Reflection](/dotnet/csharp/programming-guide/concepts/reflection) を使用し、Web API プロジェクトの名前に一致する XML ファイル名が構築されます。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-164">In the preceding code, [Reflection](/dotnet/csharp/programming-guide/concepts/reflection) is used to build an XML file name matching that of the Web API project.</span></span> <span data-ttu-id="4ee1e-165">この手法により、生成された XML ファイル名がプロジェクト名に一致します。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-165">This approach ensures that the generated XML file name matches the project name.</span></span> <span data-ttu-id="4ee1e-166">[AppContext.BaseDirectory](/dotnet/api/system.appcontext.basedirectory#System_AppContext_BaseDirectory) プロパティは、XML ファイルのパス構築に使用されます。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-166">The [AppContext.BaseDirectory](/dotnet/api/system.appcontext.basedirectory#System_AppContext_BaseDirectory) property is used to construct a path to the XML file.</span></span>

<span data-ttu-id="4ee1e-167">アクションにトリプル スラッシュのコメントを追加すると、セクション ヘッダーに説明が追加され、Swagger UI が向上します。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-167">Adding triple-slash comments to an action enhances the Swagger UI by adding the description to the section header.</span></span> <span data-ttu-id="4ee1e-168">`Delete` アクションの上に [\<summary>](/dotnet/csharp/programming-guide/xmldoc/summary) 要素を追加します。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-168">Add a [\<summary>](/dotnet/csharp/programming-guide/xmldoc/summary) element above the `Delete` action:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Delete&highlight=1-3)]

<span data-ttu-id="4ee1e-169">Swagger UI には、前述のコードの `<summary>` 要素の内部テキストが表示されます。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-169">The Swagger UI displays the inner text of the preceding code's `<summary>` element:</span></span>

![DELETE メソッドの XML コメント 'Deletes a specific TodoItem.' を](web-api-help-pages-using-swagger/_static/triple-slash-comments.png)

<span data-ttu-id="4ee1e-172">UI は、生成された JSON スキーマによって決まります。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-172">The UI is driven by the generated JSON schema:</span></span>

```json
"delete": {
    "tags": [
        "Todo"
    ],
    "summary": "Deletes a specific TodoItem.",
    "operationId": "ApiTodoByIdDelete",
    "consumes": [],
    "produces": [],
    "parameters": [
        {
            "name": "id",
            "in": "path",
            "description": "",
            "required": true,
            "type": "integer",
            "format": "int64"
        }
    ],
    "responses": {
        "200": {
            "description": "Success"
        }
    }
}
```

<span data-ttu-id="4ee1e-173">[\<remarks>](/dotnet/csharp/programming-guide/xmldoc/remarks) 要素を `Create` アクション メソッドのドキュメントに追加します。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-173">Add a [\<remarks>](/dotnet/csharp/programming-guide/xmldoc/remarks) element to the `Create` action method documentation.</span></span> <span data-ttu-id="4ee1e-174">これは、`<summary>` 要素で指定された情報を補足し、Swagger UI をより堅牢にします。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-174">It supplements information specified in the `<summary>` element and provides a more robust Swagger UI.</span></span> <span data-ttu-id="4ee1e-175">`<remarks>` 要素の内容は、テキスト、JSON、または XML で構成されます。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-175">The `<remarks>` element content can consist of text, JSON, or XML.</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

<span data-ttu-id="4ee1e-176">追加コメントで UI が向上している点にご注目ください。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-176">Notice the UI enhancements with these additional comments:</span></span>

![追加のコメントが表示された Swagger UI](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a><span data-ttu-id="4ee1e-178">データの注釈</span><span class="sxs-lookup"><span data-stu-id="4ee1e-178">Data annotations</span></span>

<span data-ttu-id="4ee1e-179">[System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) 名前空間にある属性をモデルに追加し、Swagger UI コンポーネントの向上に役立てます。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-179">Decorate the model with attributes, found in the [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) namespace, to help drive the Swagger UI components.</span></span>

<span data-ttu-id="4ee1e-180">`[Required]` 属性を `TodoItem` クラスの `Name` プロパティに追加します。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-180">Add the `[Required]` attribute to the `Name` property of the `TodoItem` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Models/TodoItem.cs?highlight=10)]

<span data-ttu-id="4ee1e-181">この属性の有無は、UI 動作を変更し、基になる JSON スキーマを変更します。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-181">The presence of this attribute changes the UI behavior and alters the underlying JSON schema:</span></span>

```json
"definitions": {
    "TodoItem": {
        "required": [
            "name"
        ],
        "type": "object",
        "properties": {
            "id": {
                "format": "int64",
                "type": "integer"
            },
            "name": {
                "type": "string"
            },
            "isComplete": {
                "default": false,
                "type": "boolean"
            }
        }
    }
},
```

<span data-ttu-id="4ee1e-182">API コントローラーに `[Produces("application/json")]` 属性を追加します。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-182">Add the `[Produces("application/json")]` attribute to the API controller.</span></span> <span data-ttu-id="4ee1e-183">その目的は、コントローラーのアクションが *application/json* の応答コンテンツ タイプをサポートすることを宣言することです。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-183">Its purpose is to declare that the controller's actions support a response content type of *application/json*:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=3)]

<span data-ttu-id="4ee1e-184">**[応答コンテンツ タイプ]** ドロップダウンがコント ローラーの GET 操作の既定値としてこのコンテンツ タイプを選択します。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-184">The **Response Content Type** drop-down selects this content type as the default for the controller's GET actions:</span></span>

![既定の応答のコンテンツ タイプを持つ Swagger UI](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

<span data-ttu-id="4ee1e-186">Web API のデータの注釈の使用量が多いほど、UI と API のヘルプ ページはわかりやすく便利になります。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-186">As the usage of data annotations in the Web API increases, the UI and API help pages become more descriptive and useful.</span></span>

### <a name="describe-response-types"></a><span data-ttu-id="4ee1e-187">応答の種類の説明</span><span class="sxs-lookup"><span data-stu-id="4ee1e-187">Describe response types</span></span>

<span data-ttu-id="4ee1e-188">開発者は、何が返されるかを、具体的には、応答の種類とエラー コード (標準以外の場合) を最も考慮します。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-188">Consuming developers are most concerned with what's returned&mdash;specifically response types and error codes (if not standard).</span></span> <span data-ttu-id="4ee1e-189">応答の種類とエラー コードは、XML コメントとデータ注釈に示されます。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-189">The response types and error codes are denoted in the XML comments and data annotations.</span></span>

<span data-ttu-id="4ee1e-190">`Create` アクションでは、成功すると HTTP 201 状態コードが返されます。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-190">The `Create` action returns an HTTP 201 status code on success.</span></span> <span data-ttu-id="4ee1e-191">HTTP 400 状態コードは、投稿された要求本文が null のときに返されます。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-191">An HTTP 400 status code is returned when the posted request body is null.</span></span> <span data-ttu-id="4ee1e-192">Swagger UI に適切なドキュメントがないと、利用者にはこれらの予期される結果の知識がありません。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-192">Without proper documentation in the Swagger UI, the consumer lacks knowledge of these expected outcomes.</span></span> <span data-ttu-id="4ee1e-193">その問題を解決するために、次の例では強調表示された行を追加しています。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-193">Fix that problem by adding the highlighted lines in the following example:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

<span data-ttu-id="4ee1e-194">Swagger UI は、予期される HTTP 応答コードを明確に記述するようになりました。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-194">The Swagger UI now clearly documents the expected HTTP response codes:</span></span>

![ステータスコードの POST 応答クラスの説明 'Returns the newly created Todo item' and '400 - If the item is null' および応答メッセージの下に理由を示す Swagger UI](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

### <a name="customize-the-ui"></a><span data-ttu-id="4ee1e-196">UI をカスタマイズする</span><span class="sxs-lookup"><span data-stu-id="4ee1e-196">Customize the UI</span></span>

<span data-ttu-id="4ee1e-197">ストック UI は機能も見栄えも優れています。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-197">The stock UI is both functional and presentable.</span></span> <span data-ttu-id="4ee1e-198">しかしながら、API ドキュメント ページでブランドやテーマを表す必要があります。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-198">However, API documentation pages should represent your brand or theme.</span></span> <span data-ttu-id="4ee1e-199">Swashbuckle コンポーネントをブランド化するには、静的ファイルにサービスを提供するリソースを追加し、それらのファイルをホストするフォルダー構造を構築する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-199">Branding the Swashbuckle components requires adding the resources to serve static files and building the folder structure to host those files.</span></span>

<span data-ttu-id="4ee1e-200">.NET Framework または .NET Core 1.x を対象としている場合、プロジェクトに [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) NuGet パッケージを追加します。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-200">If targeting .NET Framework or .NET Core 1.x, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) NuGet package to the project:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

<span data-ttu-id="4ee1e-201">.NET Core 2.x を対象とし、[メタパッケージ](xref:fundamentals/metapackage)を使用している場合、前述の NuGet パッケージが既にインストールされています。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-201">The preceding NuGet package is already installed if targeting .NET Core 2.x and using the [metapackage](xref:fundamentals/metapackage).</span></span>

<span data-ttu-id="4ee1e-202">静的ファイル ミドルウェアを有効にします。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-202">Enable the static files middleware:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Startup.cs?name=snippet_Configure&highlight=3)]

<span data-ttu-id="4ee1e-203">[Swagger UI GitHub リポジトリ](https://github.com/swagger-api/swagger-ui/tree/master/dist)から *dist* フォルダーの内容を取得します。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-203">Acquire the contents of the *dist* folder from the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui/tree/master/dist).</span></span> <span data-ttu-id="4ee1e-204">このフォルダーには、Swagger UI ページに必要なアセットが含まれています。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-204">This folder contains the necessary assets for the Swagger UI page.</span></span>

<span data-ttu-id="4ee1e-205">*wwwroot/swagger/ui* フォルダーを作成し、それに *dist* フォルダーのコンテンツをコピーします。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-205">Create a *wwwroot/swagger/ui* folder, and copy into it the contents of the *dist* folder.</span></span>

<span data-ttu-id="4ee1e-206">*wwwroot/swagger/ui* で *custom.css* ファイルを作成し、次の CSS でページ ヘッダーをカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-206">Create a *custom.css* file, in *wwwroot/swagger/ui*, with the following CSS to customize the page header:</span></span>

[!code-css[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/wwwroot/swagger/ui/custom.css)]

<span data-ttu-id="4ee1e-207">他に CSS ファイルがあればその後で *index.html* ファイルの *custom.css* を参照します。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-207">Reference *custom.css* in the *index.html* file, after any other CSS files:</span></span>

[!code-html[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/wwwroot/swagger/ui/index.html?name=snippet_SwaggerUiCss&highlight=3)]

<span data-ttu-id="4ee1e-208">`http://localhost:<random_port>/swagger/ui/index.html` で *index.html* ページを参照します。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-208">Browse to the *index.html* page at `http://localhost:<random_port>/swagger/ui/index.html`.</span></span> <span data-ttu-id="4ee1e-209">ヘッダーのテキスト ボックスに「`http://localhost:<random_port>/swagger/v1/swagger.json`」を入力し、**[探索]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-209">Enter `http://localhost:<random_port>/swagger/v1/swagger.json` in the header's textbox, and click the **Explore** button.</span></span> <span data-ttu-id="4ee1e-210">結果のページは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-210">The resulting page looks as follows:</span></span>

![カスタム ヘッダーのタイトルを含む Swagger UI](web-api-help-pages-using-swagger/_static/custom-header.png)

<span data-ttu-id="4ee1e-212">このページでできることはたくさんあります。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-212">There's much more you can do with the page.</span></span> <span data-ttu-id="4ee1e-213">[Swagger UI GitHub リポジトリ](https://github.com/swagger-api/swagger-ui)で UI リソースの完全な機能を参照してください。</span><span class="sxs-lookup"><span data-stu-id="4ee1e-213">See the full capabilities for the UI resources at the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui).</span></span>
