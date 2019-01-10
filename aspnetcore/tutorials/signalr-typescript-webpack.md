---
title: TypeScript と Webpack で ASP.NET Core SignalR を使用する
author: ssougnez
description: このチュートリアルでは、クライアントが TypeScript で記述された ASP.NET Core SignalR Web アプリをバンドルおよびビルドするために Webpack を構成します。
ms.author: scaddie
ms.custom: mvc
ms.date: 11/30/2018
uid: tutorials/signalr-typescript-webpack
ms.openlocfilehash: 92709beb7a99289b8639135aab9d821937825103
ms.sourcegitcommit: a16352c1c88a71770ab3922200a8cd148fb278a6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/13/2018
ms.locfileid: "53335287"
---
# <a name="use-aspnet-core-signalr-with-typescript-and-webpack"></a><span data-ttu-id="d7d91-103">TypeScript と Webpack で ASP.NET Core SignalR を使用する</span><span class="sxs-lookup"><span data-stu-id="d7d91-103">Use ASP.NET Core SignalR with TypeScript and Webpack</span></span>

<span data-ttu-id="d7d91-104">作成者: [Sébastien Sougnez](https://twitter.com/ssougnez)、[Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="d7d91-104">By [Sébastien Sougnez](https://twitter.com/ssougnez) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="d7d91-105">[Webpack](https://webpack.js.org/) を使用すると、開発者は Web アプリのクライアント側のリソースをバンドルおよびビルドすることができます。</span><span class="sxs-lookup"><span data-stu-id="d7d91-105">[Webpack](https://webpack.js.org/) enables developers to bundle and build the client-side resources of a web app.</span></span> <span data-ttu-id="d7d91-106">このチュートリアルでは、クライアントが [TypeScript](https://www.typescriptlang.org/) で記述された ASP.NET Core SignalR Web アプリでの Webpack の使用法を示します。</span><span class="sxs-lookup"><span data-stu-id="d7d91-106">This tutorial demonstrates using Webpack in an ASP.NET Core SignalR web app whose client is written in [TypeScript](https://www.typescriptlang.org/).</span></span>

<span data-ttu-id="d7d91-107">このチュートリアルでは、次の作業を行う方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="d7d91-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d7d91-108">スターター ASP.NET Core SignalR アプリをスキャフォールディングする</span><span class="sxs-lookup"><span data-stu-id="d7d91-108">Scaffold a starter ASP.NET Core SignalR app</span></span>
> * <span data-ttu-id="d7d91-109">SignalR TypeScript クライアントを構成する</span><span class="sxs-lookup"><span data-stu-id="d7d91-109">Configure the SignalR TypeScript client</span></span>
> * <span data-ttu-id="d7d91-110">Webpack を使用してビルド パイプラインを構成する</span><span class="sxs-lookup"><span data-stu-id="d7d91-110">Configure a build pipeline using Webpack</span></span>
> * <span data-ttu-id="d7d91-111">SignalR サーバーを構成する</span><span class="sxs-lookup"><span data-stu-id="d7d91-111">Configure the SignalR server</span></span>
> * <span data-ttu-id="d7d91-112">クライアントとサーバー間の通信を有効にする</span><span class="sxs-lookup"><span data-stu-id="d7d91-112">Enable communication between client and server</span></span>

<span data-ttu-id="d7d91-113">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr-typescript-webpack/sample)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="d7d91-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr-typescript-webpack/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

[!INCLUDE [Prerequisites](~/includes/net-core-prereqs-vs-vsc-2.2.md)]

## <a name="create-the-aspnet-core-web-app"></a><span data-ttu-id="d7d91-114">ASP.NET Core Web アプリを作成する</span><span class="sxs-lookup"><span data-stu-id="d7d91-114">Create the ASP.NET Core web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d7d91-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d7d91-115">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="d7d91-116">*PATH* 環境変数で npm を検索するように Visual Studio を構成します。</span><span class="sxs-lookup"><span data-stu-id="d7d91-116">Configure Visual Studio to look for npm in the *PATH* environment variable.</span></span> <span data-ttu-id="d7d91-117">既定では、Visual Studio は、そのインストール ディレクトリ内で見つかった npm のバージョンを使用します。</span><span class="sxs-lookup"><span data-stu-id="d7d91-117">By default, Visual Studio uses the version of npm found in its installation directory.</span></span> <span data-ttu-id="d7d91-118">Visual Studio のこれらの説明に従ってください。</span><span class="sxs-lookup"><span data-stu-id="d7d91-118">Follow these instructions in Visual Studio:</span></span>

1. <span data-ttu-id="d7d91-119">**[ツール]** > **[オプション]** > **[プロジェクトおよびソリューション]** > **[Web パッケージ管理]** > **[外部 Web ツール]** の順に移動します。</span><span class="sxs-lookup"><span data-stu-id="d7d91-119">Navigate to **Tools** > **Options** > **Projects and Solutions** > **Web Package Management** > **External Web Tools**.</span></span>
1. <span data-ttu-id="d7d91-120">リストから *[$(PATH)]* エントリを選択します。</span><span class="sxs-lookup"><span data-stu-id="d7d91-120">Select the *$(PATH)* entry from the list.</span></span> <span data-ttu-id="d7d91-121">上向き矢印をクリックして、エントリをリストの 2 番目の位置に移動します。</span><span class="sxs-lookup"><span data-stu-id="d7d91-121">Click the up arrow to move the entry to the second position in the list.</span></span>

    ![Visual Studio の構成](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

<span data-ttu-id="d7d91-123">Visual Studio の構成が完了しました。</span><span class="sxs-lookup"><span data-stu-id="d7d91-123">Visual Studio configuration is completed.</span></span> <span data-ttu-id="d7d91-124">次はプロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="d7d91-124">It's time to create the project.</span></span>

1. <span data-ttu-id="d7d91-125">**[ファイル]** > **[新規]** > **[プロジェクト]** メニュー オプション、**[ASP.NET Core Web アプリケーション]** テンプレートの順に選択します。</span><span class="sxs-lookup"><span data-stu-id="d7d91-125">Use the **File** > **New** > **Project** menu option and choose the **ASP.NET Core Web Application** template.</span></span>
1. <span data-ttu-id="d7d91-126">プロジェクトに *SignalRWebPack* という名前を付け、**[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="d7d91-126">Name the project *SignalRWebPack*, and select **OK**.</span></span>
1. <span data-ttu-id="d7d91-127">ターゲット フレームワークのドロップダウンから、*[.NET Core]* を選択し、フレームワーク セレクターのドロップダウンから *[ASP.NET Core 2.2]* を選択します。</span><span class="sxs-lookup"><span data-stu-id="d7d91-127">Select *.NET Core* from the target framework drop-down, and select *ASP.NET Core 2.2* from the framework selector drop-down.</span></span> <span data-ttu-id="d7d91-128">**空**のテンプレートを選択して、**[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="d7d91-128">Select the **Empty** template, and select **OK**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d7d91-129">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d7d91-129">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="d7d91-130">**統合端末**で次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="d7d91-130">Run the following command in the **Integrated Terminal**:</span></span>

```console
dotnet new web -o SignalRWebPack
```

<span data-ttu-id="d7d91-131">.NET Core をターゲットとする、空の ASP.NET Core Web アプリが *SignalRWebPack* ディレクトリ内に作成されます。</span><span class="sxs-lookup"><span data-stu-id="d7d91-131">An empty ASP.NET Core web app, targeting .NET Core, is created in a *SignalRWebPack* directory.</span></span>

---

## <a name="configure-webpack-and-typescript"></a><span data-ttu-id="d7d91-132">WebPack および TypeScript の構成</span><span class="sxs-lookup"><span data-stu-id="d7d91-132">Configure Webpack and TypeScript</span></span>

<span data-ttu-id="d7d91-133">次の手順では、TypeScript の JavaScript への変換と、クライアント側のリソースのバンドルを構成します。</span><span class="sxs-lookup"><span data-stu-id="d7d91-133">The following steps configure the conversion of TypeScript to JavaScript and the bundling of client-side resources.</span></span>

1. <span data-ttu-id="d7d91-134">プロジェクト ルートで次のコマンドを実行して、*package.json* ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="d7d91-134">Execute the following command in the project root to create a *package.json* file:</span></span>

    ```console
    npm init -y
    ```

1. <span data-ttu-id="d7d91-135">強調表示されているプロパティを *package.json* ファイルに追加します。</span><span class="sxs-lookup"><span data-stu-id="d7d91-135">Add the highlighted property to the *package.json* file:</span></span>

    [!code-json[package.json](signalr-typescript-webpack/sample/snippets/package1.json?highlight=4)]

    <span data-ttu-id="d7d91-136">`private` プロパティを `true` に設定して、次の手順でパッケージのインストールの警告が表示されないようにします。</span><span class="sxs-lookup"><span data-stu-id="d7d91-136">Setting the `private` property to `true` prevents package installation warnings in the next step.</span></span>

1. <span data-ttu-id="d7d91-137">必要な npm パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="d7d91-137">Install the required npm packages.</span></span> <span data-ttu-id="d7d91-138">プロジェクト ルートで、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="d7d91-138">Execute the following command from the project root:</span></span>

    ```console
    npm install -D -E clean-webpack-plugin@0.1.19 css-loader@0.28.11 html-webpack-plugin@3.2.0 mini-css-extract-plugin@0.4.0 ts-loader@4.4.1 typescript@2.9.2 webpack@4.12.0 webpack-cli@3.0.6
    ```

    <span data-ttu-id="d7d91-139">注目するべきコマンドの詳細:</span><span class="sxs-lookup"><span data-stu-id="d7d91-139">Some command details to note:</span></span>

    * <span data-ttu-id="d7d91-140">バージョン番号は、各パッケージ名の `@` 記号の後に続きます。</span><span class="sxs-lookup"><span data-stu-id="d7d91-140">A version number follows the `@` sign for each package name.</span></span> <span data-ttu-id="d7d91-141">npm によって、これらの特定のパッケージ バージョンがインストールされます。</span><span class="sxs-lookup"><span data-stu-id="d7d91-141">npm installs those specific package versions.</span></span>
    * <span data-ttu-id="d7d91-142">`-E` オプションは、[セマンティック バージョニング](https://semver.org/)範囲演算子を *package.json* に書き込む npm の既定の動作を無効にします。</span><span class="sxs-lookup"><span data-stu-id="d7d91-142">The `-E` option disables npm's default behavior of writing [semantic versioning](https://semver.org/) range operators to *package.json*.</span></span> <span data-ttu-id="d7d91-143">たとえば、`"webpack": "4.12.0"` が `"webpack": "^4.12.0"` の代わりに使用されています。</span><span class="sxs-lookup"><span data-stu-id="d7d91-143">For example, `"webpack": "4.12.0"` is used instead of `"webpack": "^4.12.0"`.</span></span> <span data-ttu-id="d7d91-144">このオプションにより、新しいパッケージ バージョンへの予期しないアップグレードが防止されます。</span><span class="sxs-lookup"><span data-stu-id="d7d91-144">This option prevents unintended upgrades to newer package versions.</span></span>

    <span data-ttu-id="d7d91-145">詳細については、[npm-install](https://docs.npmjs.com/cli/install) の公式ドキュメントを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d7d91-145">See the official [npm-install](https://docs.npmjs.com/cli/install) docs for more detail.</span></span>

1. <span data-ttu-id="d7d91-146">*package.json* ファイルの `scripts` プロパティを次のスニペットで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="d7d91-146">Replace the `scripts` property of the *package.json* file with the following snippet:</span></span>

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    <span data-ttu-id="d7d91-147">スクリプトの説明 (一部):</span><span class="sxs-lookup"><span data-stu-id="d7d91-147">Some explanation of the scripts:</span></span>

    * <span data-ttu-id="d7d91-148">`build`: 開発モードでクライアント側のリソースをバンドルし、ファイルの変更を監視します。</span><span class="sxs-lookup"><span data-stu-id="d7d91-148">`build`: Bundles your client-side resources in development mode and watches for file changes.</span></span> <span data-ttu-id="d7d91-149">ファイル監視により、プロジェクト ファイルを変更するたびにバンドルが再生成されます。</span><span class="sxs-lookup"><span data-stu-id="d7d91-149">The file watcher causes the bundle to regenerate each time a project file changes.</span></span> <span data-ttu-id="d7d91-150">`mode` オプションは、ツリー シェイキングや縮小などの運用環境の最適化を無効にします。</span><span class="sxs-lookup"><span data-stu-id="d7d91-150">The `mode` option disables production optimizations, such as tree shaking and minification.</span></span> <span data-ttu-id="d7d91-151">`build` は開発でのみ使用します。</span><span class="sxs-lookup"><span data-stu-id="d7d91-151">Only use `build` in development.</span></span>
    * <span data-ttu-id="d7d91-152">`release`: 運用モードでクライアント側のリソースをバンドルします。</span><span class="sxs-lookup"><span data-stu-id="d7d91-152">`release`: Bundles your client-side resources in production mode.</span></span>
    * <span data-ttu-id="d7d91-153">`publish`: `release` スクリプトを実行して、運用モードでクライアント側のリソースをバンドルします。</span><span class="sxs-lookup"><span data-stu-id="d7d91-153">`publish`: Runs the `release` script to bundle the client-side resources in production mode.</span></span> <span data-ttu-id="d7d91-154">.NET Core CLI の [publish](/dotnet/core/tools/dotnet-publish) コマンドを呼び出してアプリを公開します。</span><span class="sxs-lookup"><span data-stu-id="d7d91-154">It calls the .NET Core CLI's [publish](/dotnet/core/tools/dotnet-publish) command to publish the app.</span></span>

1. <span data-ttu-id="d7d91-155">プロジェクト ルートに、次の内容を持つ *webpack.config.js* という名前のファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="d7d91-155">Create a file named *webpack.config.js*, in the project root, with the following content:</span></span>

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/webpack.config.js)]

    <span data-ttu-id="d7d91-156">上記のファイルは、Webpack コンパイルを構成します。</span><span class="sxs-lookup"><span data-stu-id="d7d91-156">The preceding file configures the Webpack compilation.</span></span> <span data-ttu-id="d7d91-157">注目するべき構成の詳細 (一部):</span><span class="sxs-lookup"><span data-stu-id="d7d91-157">Some configuration details to note:</span></span>

    * <span data-ttu-id="d7d91-158">`output` プロパティにより、*dist* の既定値がオーバーライドされます。</span><span class="sxs-lookup"><span data-stu-id="d7d91-158">The `output` property overrides the default value of *dist*.</span></span> <span data-ttu-id="d7d91-159">代わりにバンドルが *wwwroot* ディレクトリ内に生成されます。</span><span class="sxs-lookup"><span data-stu-id="d7d91-159">The bundle is instead emitted in the *wwwroot* directory.</span></span>
    * <span data-ttu-id="d7d91-160">`resolve.extensions` 配列には、SignalR クライアント JavaScript をインポートするための *.js* が含まれています。</span><span class="sxs-lookup"><span data-stu-id="d7d91-160">The `resolve.extensions` array includes *.js* to import the SignalR client JavaScript.</span></span>

1. <span data-ttu-id="d7d91-161">プロジェクト ルートに新しい *src* プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="d7d91-161">Create a new *src* directory in the project root.</span></span> <span data-ttu-id="d7d91-162">その目的は、プロジェクトのクライアント側アセットを格納することです。</span><span class="sxs-lookup"><span data-stu-id="d7d91-162">Its purpose is to store the project's client-side assets.</span></span>

1. <span data-ttu-id="d7d91-163">次のコンテンツを含む *src/index.html* を作成します。</span><span class="sxs-lookup"><span data-stu-id="d7d91-163">Create *src/index.html* with the following content.</span></span>

    [!code-html[index.html](signalr-typescript-webpack/sample/src/index.html)]

    <span data-ttu-id="d7d91-164">上記の HTML では、ホームページの定型マークアップが定義されます。</span><span class="sxs-lookup"><span data-stu-id="d7d91-164">The preceding HTML defines the homepage's boilerplate markup.</span></span>

1. <span data-ttu-id="d7d91-165">新しい *src/css* ディレクトリを作成します。</span><span class="sxs-lookup"><span data-stu-id="d7d91-165">Create a new *src/css* directory.</span></span> <span data-ttu-id="d7d91-166">その目的は、プロジェクトの *.css* ファイルを格納することです。</span><span class="sxs-lookup"><span data-stu-id="d7d91-166">Its purpose is to store the project's *.css* files.</span></span>

1. <span data-ttu-id="d7d91-167">次のコンテンツを含む *src/css/main.css* を作成します。</span><span class="sxs-lookup"><span data-stu-id="d7d91-167">Create *src/css/main.css* with the following content:</span></span>

    [!code-css[main.css](signalr-typescript-webpack/sample/src/css/main.css)]

    <span data-ttu-id="d7d91-168">上記の *main.css* ファイルは、アプリをスタイル設定します。</span><span class="sxs-lookup"><span data-stu-id="d7d91-168">The preceding *main.css* file styles the app.</span></span>

1. <span data-ttu-id="d7d91-169">次のコンテンツを含む *src/tsconfig.json* を作成します。</span><span class="sxs-lookup"><span data-stu-id="d7d91-169">Create *src/tsconfig.json* with the following content:</span></span>

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/src/tsconfig.json)]

    <span data-ttu-id="d7d91-170">上記のコードは、[ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5 対応の JavaScript を生成するように TypeScript コンパイラを構成します。</span><span class="sxs-lookup"><span data-stu-id="d7d91-170">The preceding code configures the TypeScript compiler to produce [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5-compatible JavaScript.</span></span>

1. <span data-ttu-id="d7d91-171">次のコンテンツを含む *src/index.ts* を作成します。</span><span class="sxs-lookup"><span data-stu-id="d7d91-171">Create *src/index.ts* with the following content:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    <span data-ttu-id="d7d91-172">上記の TypeScript は、DOM 要素への参照を取得し、次の 2 つのイベント ハンドラーをアタッチします。</span><span class="sxs-lookup"><span data-stu-id="d7d91-172">The preceding TypeScript retrieves references to DOM elements and attaches two event handlers:</span></span>

    * <span data-ttu-id="d7d91-173">`keyup`: ユーザーがテキスト ボックスに `tbMessage` として識別されるものを入力すると、このイベントが発生します。</span><span class="sxs-lookup"><span data-stu-id="d7d91-173">`keyup`: This event fires when the user types something in the textbox identified as `tbMessage`.</span></span> <span data-ttu-id="d7d91-174">ユーザーが **Enter** キーを押すと、`send` 関数が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="d7d91-174">The `send` function is called when the user presses the **Enter** key.</span></span>
    * <span data-ttu-id="d7d91-175">`click`: ユーザーが **[送信]** ボタンをクリックすると、このイベントが発生します。</span><span class="sxs-lookup"><span data-stu-id="d7d91-175">`click`: This event fires when the user clicks the **Send** button.</span></span> <span data-ttu-id="d7d91-176">`send` 関数が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="d7d91-176">The `send` function is called.</span></span>

## <a name="configure-the-aspnet-core-app"></a><span data-ttu-id="d7d91-177">ASP.NET Core アプリを構成する</span><span class="sxs-lookup"><span data-stu-id="d7d91-177">Configure the ASP.NET Core app</span></span>

1. <span data-ttu-id="d7d91-178">`Startup.Configure` メソッドで提供されたコードにより、*Hello World!* が表示されます。</span><span class="sxs-lookup"><span data-stu-id="d7d91-178">The code provided in the `Startup.Configure` method displays *Hello World!*.</span></span> <span data-ttu-id="d7d91-179">`app.Run` メソッド呼び出しを、[UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) と [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) の呼び出しで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="d7d91-179">Replace the `app.Run` method call with calls to [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_UseStaticDefaultFiles)]

    <span data-ttu-id="d7d91-180">上記のコードにより、サーバーが *index.html* ファイルを見つけて提供することができます。ユーザーがファイルの完全な URL または Web アプリのルート URL を入力するかどうかは関係ありません。</span><span class="sxs-lookup"><span data-stu-id="d7d91-180">The preceding code allows the server to locate and serve the *index.html* file, whether the user enters its full URL or the root URL of the web app.</span></span>

1. <span data-ttu-id="d7d91-181">`Startup.ConfigureServices` メソッドで [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="d7d91-181">Call [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="d7d91-182">これにより SignalR サービスがプロジェクトに追加されます。</span><span class="sxs-lookup"><span data-stu-id="d7d91-182">It adds the SignalR services to your project.</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_AddSignalR)]

1. <span data-ttu-id="d7d91-183">*/hub* ルートを `ChatHub` ハブにマップします。</span><span class="sxs-lookup"><span data-stu-id="d7d91-183">Map a */hub* route to the `ChatHub` hub.</span></span> <span data-ttu-id="d7d91-184">`Startup.Configure` メソッドの末尾に、次の行を追加します。</span><span class="sxs-lookup"><span data-stu-id="d7d91-184">Add the following lines at the end of the `Startup.Configure` method:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_UseSignalR)]

1. <span data-ttu-id="d7d91-185">プロジェクト ルートに *Hubs* という新しいディレクトリを作成します。</span><span class="sxs-lookup"><span data-stu-id="d7d91-185">Create a new directory, called *Hubs*, in the project root.</span></span> <span data-ttu-id="d7d91-186">その目的は、次の手順で作成される SignalR ハブを格納することです。</span><span class="sxs-lookup"><span data-stu-id="d7d91-186">Its purpose is to store the SignalR hub, which is created in the next step.</span></span>

1. <span data-ttu-id="d7d91-187">次のコードを使用して、*Hubs/ChatHub.cs* を作成します。</span><span class="sxs-lookup"><span data-stu-id="d7d91-187">Create hub *Hubs/ChatHub.cs* with the following code:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. <span data-ttu-id="d7d91-188">`ChatHub` 参照を解決するには、次のコードを *Startup.cs* ファイルの先頭に追加します。</span><span class="sxs-lookup"><span data-stu-id="d7d91-188">Add the following code at the top of the *Startup.cs* file to resolve the `ChatHub` reference:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a><span data-ttu-id="d7d91-189">クライアントとサーバーの通信を有効にする</span><span class="sxs-lookup"><span data-stu-id="d7d91-189">Enable client and server communication</span></span>

<span data-ttu-id="d7d91-190">アプリには現在、メッセージを送信するための単純なフォームが表示されています。</span><span class="sxs-lookup"><span data-stu-id="d7d91-190">The app currently displays a simple form to send messages.</span></span> <span data-ttu-id="d7d91-191">送信しようとしても、何も起こりません。</span><span class="sxs-lookup"><span data-stu-id="d7d91-191">Nothing happens when you try to do so.</span></span> <span data-ttu-id="d7d91-192">サーバーは特定のルートをリッスンしていますが、メッセージの送信については何も行いません。</span><span class="sxs-lookup"><span data-stu-id="d7d91-192">The server is listening to a specific route but does nothing with sent messages.</span></span>

1. <span data-ttu-id="d7d91-193">プロジェクト ルートで、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="d7d91-193">Execute the following command at the project root:</span></span>

    ```console
    npm install @aspnet/signalr
    ```

    <span data-ttu-id="d7d91-194">上記のコマンドにより [SignalR TypeScript クライアント](https://www.npmjs.com/package/@aspnet/signalr) がインストールされ、クライアントがサーバーにメッセージを送信できるようになります。</span><span class="sxs-lookup"><span data-stu-id="d7d91-194">The preceding command installs the [SignalR TypeScript client](https://www.npmjs.com/package/@aspnet/signalr), which allows the client to send messages to the server.</span></span>

1. <span data-ttu-id="d7d91-195">強調表示されたコードを *src/index.ts* ファイルに追加します。</span><span class="sxs-lookup"><span data-stu-id="d7d91-195">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    <span data-ttu-id="d7d91-196">上記のコードは、サーバーからのメッセージの受信をサポートします。</span><span class="sxs-lookup"><span data-stu-id="d7d91-196">The preceding code supports receiving messages from the server.</span></span> <span data-ttu-id="d7d91-197">`HubConnectionBuilder` クラスは、サーバー接続を構成するための新しいビルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="d7d91-197">The `HubConnectionBuilder` class creates a new builder for configuring the server connection.</span></span> <span data-ttu-id="d7d91-198">`withUrl` 関数は、ハブ URL を構成します。</span><span class="sxs-lookup"><span data-stu-id="d7d91-198">The `withUrl` function configures the hub URL.</span></span>

    <span data-ttu-id="d7d91-199">SignalR により、クライアントとサーバー間でのメッセージのやり取りが可能になります。</span><span class="sxs-lookup"><span data-stu-id="d7d91-199">SignalR enables the exchange of messages between a client and a server.</span></span> <span data-ttu-id="d7d91-200">各メッセージには特定の名前があります。</span><span class="sxs-lookup"><span data-stu-id="d7d91-200">Each message has a specific name.</span></span> <span data-ttu-id="d7d91-201">たとえば、メッセージ ゾーンに新しいメッセージを表示するためのロジックを実行する `messageReceived` という名前を持つメッセージを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="d7d91-201">For example, you can have messages with the name `messageReceived` that execute the logic responsible for displaying the new message in the messages zone.</span></span> <span data-ttu-id="d7d91-202">特定のメッセージをリッスンするには、`on` 関数を使用します。</span><span class="sxs-lookup"><span data-stu-id="d7d91-202">Listening to a specific message can be done via the `on` function.</span></span> <span data-ttu-id="d7d91-203">任意の数のメッセージ名をリッスンできます。</span><span class="sxs-lookup"><span data-stu-id="d7d91-203">You can listen to any number of message names.</span></span> <span data-ttu-id="d7d91-204">作成者の名前や受信したメッセージの内容など、パラメーターをメッセージに渡すこともできます。</span><span class="sxs-lookup"><span data-stu-id="d7d91-204">It's also possible to pass parameters to the message, such as the author's name and the content of the message received.</span></span> <span data-ttu-id="d7d91-205">クライアントがメッセージを受信すると、`innerHTML` 属性に作成者の名前とメッセージ コンテンツを持つ新しい `div` 要素が作成されます。</span><span class="sxs-lookup"><span data-stu-id="d7d91-205">Once the client receives a message, a new `div` element is created with the author's name and the message content in its `innerHTML` attribute.</span></span> <span data-ttu-id="d7d91-206">これはメッセージを表示する主要な `div` 要素に追加されます。</span><span class="sxs-lookup"><span data-stu-id="d7d91-206">It's added to the main `div` element displaying the messages.</span></span>

1. <span data-ttu-id="d7d91-207">これでクライアントがメッセージを受信できるようになったので、メッセージを送信するように構成します。</span><span class="sxs-lookup"><span data-stu-id="d7d91-207">Now that the client can receive a message, configure it to send messages.</span></span> <span data-ttu-id="d7d91-208">強調表示されたコードを *src/index.ts* ファイルに追加します。</span><span class="sxs-lookup"><span data-stu-id="d7d91-208">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/src/index.ts?highlight=34-35)]

    <span data-ttu-id="d7d91-209">WebSocket 接続を介してメッセージを送信するには、`send` メソッドを呼び出す必要があります。</span><span class="sxs-lookup"><span data-stu-id="d7d91-209">Sending a message through the WebSockets connection requires calling the `send` method.</span></span> <span data-ttu-id="d7d91-210">メソッドの最初のパラメーターは、メッセージ名です。</span><span class="sxs-lookup"><span data-stu-id="d7d91-210">The method's first parameter is the message name.</span></span> <span data-ttu-id="d7d91-211">メッセージ データは、他のパラメーターに存在しています。</span><span class="sxs-lookup"><span data-stu-id="d7d91-211">The message data inhabits the other parameters.</span></span> <span data-ttu-id="d7d91-212">この例では、`newMessage` として識別されたメッセージがサーバーに送信されます。</span><span class="sxs-lookup"><span data-stu-id="d7d91-212">In this example, a message identified as `newMessage` is sent to the server.</span></span> <span data-ttu-id="d7d91-213">メッセージは、ユーザー名と、テキスト ボックスへのユーザー入力で構成されます。</span><span class="sxs-lookup"><span data-stu-id="d7d91-213">The message consists of the username and the user input from a text box.</span></span> <span data-ttu-id="d7d91-214">送信が機能していると、テキスト ボックスの値はクリアされます。</span><span class="sxs-lookup"><span data-stu-id="d7d91-214">If the send works, the text box value is cleared.</span></span>

1. <span data-ttu-id="d7d91-215">強調表示されているメソッドを `ChatHub` クラスに追加します。</span><span class="sxs-lookup"><span data-stu-id="d7d91-215">Add the highlighted method to the `ChatHub` class:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/Hubs/ChatHub.cs?highlight=8-11)]

    <span data-ttu-id="d7d91-216">上記のコードは、サーバーがメッセージを受信すると、受信したメッセージをすべての接続されているユーザーにブロードキャストします。</span><span class="sxs-lookup"><span data-stu-id="d7d91-216">The preceding code broadcasts received messages to all connected users once the server receives them.</span></span> <span data-ttu-id="d7d91-217">すべてのメッセージを受信するのに、ジェネリック `on` メソッドは必要ありません。</span><span class="sxs-lookup"><span data-stu-id="d7d91-217">It's unnecessary to have a generic `on` method to receive all the messages.</span></span> <span data-ttu-id="d7d91-218">メソッドはメッセージ名のサフィックスに基づいて名前が付けられています。</span><span class="sxs-lookup"><span data-stu-id="d7d91-218">A method named after the message name suffices.</span></span>

    <span data-ttu-id="d7d91-219">この例では、TypeScript クライアントが `newMessage` として識別されるメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="d7d91-219">In this example, the TypeScript client sends a message identified as `newMessage`.</span></span> <span data-ttu-id="d7d91-220">C# `NewMessage` メソッドは、データがクライアントから送信されることを想定しています。</span><span class="sxs-lookup"><span data-stu-id="d7d91-220">The C# `NewMessage` method expects the data sent by the client.</span></span> <span data-ttu-id="d7d91-221">[Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all) で [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="d7d91-221">A call is made to the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method on [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span></span> <span data-ttu-id="d7d91-222">受信したメッセージは、ハブに接続されているすべてのクライアントに送信されます。</span><span class="sxs-lookup"><span data-stu-id="d7d91-222">The received messages are sent to all clients connected to the hub.</span></span>

## <a name="test-the-app"></a><span data-ttu-id="d7d91-223">アプリのテスト</span><span class="sxs-lookup"><span data-stu-id="d7d91-223">Test the app</span></span>

<span data-ttu-id="d7d91-224">次の手順で、アプリの動作を確認します。</span><span class="sxs-lookup"><span data-stu-id="d7d91-224">Confirm that the app works with the following steps.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d7d91-225">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d7d91-225">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="d7d91-226">*リリース* モードで Webpack を実行します。</span><span class="sxs-lookup"><span data-stu-id="d7d91-226">Run Webpack in *release* mode.</span></span> <span data-ttu-id="d7d91-227">**パッケージ マネージャー コンソール** ウィンドウを使用して、プロジェクト ルートで次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="d7d91-227">Using the **Package Manager Console** window, execute the following command in the project root.</span></span> <span data-ttu-id="d7d91-228">プロジェクト ルートにいない場合は、コマンドを入力する前に `cd SignalRWebPack` と入力します。</span><span class="sxs-lookup"><span data-stu-id="d7d91-228">If you are not in the project root, enter `cd SignalRWebPack` before entering the command.</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="d7d91-229">**[デバッグ]** > **[デバッグなしで開始]** の順に選択して、デバッガーをアタッチせずに、ブラウザーでアプリを起動します。</span><span class="sxs-lookup"><span data-stu-id="d7d91-229">Select **Debug** > **Start without debugging** to launch the app in a browser without attaching the debugger.</span></span> <span data-ttu-id="d7d91-230">*wwroot/index.html* ファイルは `http://localhost:<port_number>` で提供されます。</span><span class="sxs-lookup"><span data-stu-id="d7d91-230">The *wwwroot/index.html* file is served at `http://localhost:<port_number>`.</span></span>

1. <span data-ttu-id="d7d91-231">別のブラウザー インスタンスを開きます (任意のブラウザー)。</span><span class="sxs-lookup"><span data-stu-id="d7d91-231">Open another browser instance (any browser).</span></span> <span data-ttu-id="d7d91-232">アドレス バーに URL を貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="d7d91-232">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="d7d91-233">いずれかのブラウザーを選択し、**[メッセージ]** テキスト ボックスにメッセージを入力し、**[送信]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="d7d91-233">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="d7d91-234">次の瞬間、両方のページに一意のユーザー名とメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="d7d91-234">The unique user name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d7d91-235">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d7d91-235">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="d7d91-236">プロジェクト ルートで次のコマンドを実行して、*リリース* モードで Webpack を実行します。</span><span class="sxs-lookup"><span data-stu-id="d7d91-236">Run Webpack in *release* mode by executing the following command in the project root:</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="d7d91-237">プロジェクト ルートで次のコマンドを実行して、アプリをビルドして実行します。</span><span class="sxs-lookup"><span data-stu-id="d7d91-237">Build and run the app by executing the following command in the project root:</span></span>

    ```console
    dotnet run
    ```

    <span data-ttu-id="d7d91-238">Web サーバーは、アプリを起動して、localhost 上で使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="d7d91-238">The web server starts the app and makes it available on localhost.</span></span>

1. <span data-ttu-id="d7d91-239">ブラウザーを開いて `http://localhost:<port_number>` にアクセスします。</span><span class="sxs-lookup"><span data-stu-id="d7d91-239">Open a browser to `http://localhost:<port_number>`.</span></span> <span data-ttu-id="d7d91-240">*wwwroot/index.html* ファイルが提供されます。</span><span class="sxs-lookup"><span data-stu-id="d7d91-240">The *wwwroot/index.html* file is served.</span></span> <span data-ttu-id="d7d91-241">アドレス バーから URL をコピーします。</span><span class="sxs-lookup"><span data-stu-id="d7d91-241">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="d7d91-242">別のブラウザー インスタンスを開きます (任意のブラウザー)。</span><span class="sxs-lookup"><span data-stu-id="d7d91-242">Open another browser instance (any browser).</span></span> <span data-ttu-id="d7d91-243">アドレス バーに URL を貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="d7d91-243">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="d7d91-244">いずれかのブラウザーを選択し、**[メッセージ]** テキスト ボックスにメッセージを入力し、**[送信]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="d7d91-244">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="d7d91-245">次の瞬間、両方のページに一意のユーザー名とメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="d7d91-245">The unique user name and message are displayed on both pages instantly.</span></span>

---

![両方のブラウザー ウィンドウに表示されるメッセージ](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

## <a name="additional-resources"></a><span data-ttu-id="d7d91-247">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="d7d91-247">Additional resources</span></span>

* <xref:signalr/javascript-client>
* <xref:signalr/hubs>
