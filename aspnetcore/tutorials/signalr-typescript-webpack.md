---
title: TypeScript と Webpack で ASP.NET Core SignalR を使用する
author: ssougnez
description: このチュートリアルでは、クライアントが TypeScript で記述された ASP.NET Core SignalR Web アプリをバンドルおよびビルドするために Webpack を構成します。
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 06/29/2018
uid: tutorials/signalr-typescript-webpack
ms.openlocfilehash: 6d52e915ab20aa69179585eceb497e8abd72c997
ms.sourcegitcommit: 661d30492d5ef7bbca4f7e709f40d8f3309d2dac
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/10/2018
ms.locfileid: "37938185"
---
# <a name="use-aspnet-core-signalr-with-typescript-and-webpack"></a><span data-ttu-id="527c2-103">TypeScript と Webpack で ASP.NET Core SignalR を使用する</span><span class="sxs-lookup"><span data-stu-id="527c2-103">Use ASP.NET Core SignalR with TypeScript and Webpack</span></span>

<span data-ttu-id="527c2-104">作成者: [Sébastien Sougnez](https://twitter.com/ssougnez)、[Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="527c2-104">By [Sébastien Sougnez](https://twitter.com/ssougnez) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="527c2-105">[Webpack](https://webpack.js.org/) を使用すると、開発者は Web アプリのクライアント側のリソースをバンドルおよびビルドすることができます。</span><span class="sxs-lookup"><span data-stu-id="527c2-105">[Webpack](https://webpack.js.org/) enables developers to bundle and build the client-side resources of a web app.</span></span> <span data-ttu-id="527c2-106">このチュートリアルでは、クライアントが [TypeScript](https://www.typescriptlang.org/) で記述された ASP.NET Core SignalR Web アプリでの Webpack の使用法を示します。</span><span class="sxs-lookup"><span data-stu-id="527c2-106">This tutorial demonstrates using Webpack in an ASP.NET Core SignalR web app whose client is written in [TypeScript](https://www.typescriptlang.org/).</span></span>

<span data-ttu-id="527c2-107">このチュートリアルでは、次の作業を行う方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="527c2-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="527c2-108">スターター ASP.NET Core SignalR アプリをスキャフォールディングする</span><span class="sxs-lookup"><span data-stu-id="527c2-108">Scaffold a starter ASP.NET Core SignalR app</span></span>
> * <span data-ttu-id="527c2-109">SignalR TypeScript クライアントを構成する</span><span class="sxs-lookup"><span data-stu-id="527c2-109">Configure the SignalR TypeScript client</span></span>
> * <span data-ttu-id="527c2-110">Webpack を使用してビルド パイプラインを構成する</span><span class="sxs-lookup"><span data-stu-id="527c2-110">Configure a build pipeline using Webpack</span></span>
> * <span data-ttu-id="527c2-111">SignalR サーバーを構成する</span><span class="sxs-lookup"><span data-stu-id="527c2-111">Configure the SignalR server</span></span>
> * <span data-ttu-id="527c2-112">クライアントとサーバー間の通信を有効にする</span><span class="sxs-lookup"><span data-stu-id="527c2-112">Enable communication between client and server</span></span>

<span data-ttu-id="527c2-113">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/signalr-typescript-webpack/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="527c2-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/signalr-typescript-webpack/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="527c2-114">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="527c2-114">Prerequisites</span></span>

<span data-ttu-id="527c2-115">以下のソフトウェアをインストールします。</span><span class="sxs-lookup"><span data-stu-id="527c2-115">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="527c2-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="527c2-116">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="527c2-117">.NET Core SDK 2.1 以降</span><span class="sxs-lookup"><span data-stu-id="527c2-117">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="527c2-118">[Node.js](https://nodejs.org/) ([npm](https://www.npmjs.com/) 使用)</span><span class="sxs-lookup"><span data-stu-id="527c2-118">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>
* <span data-ttu-id="527c2-119">**ASP.NET および Web 開発**ワークロードを含む [Visual Studio 2017](https://www.visualstudio.com/downloads/) バージョン 15.7.3 以降</span><span class="sxs-lookup"><span data-stu-id="527c2-119">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7.3 or later with the **ASP.NET and web development** workload</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="527c2-120">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="527c2-120">.NET Core CLI</span></span>](#tab/netcore-cli)

* [<span data-ttu-id="527c2-121">.NET Core SDK 2.1 以降</span><span class="sxs-lookup"><span data-stu-id="527c2-121">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="527c2-122">[Node.js](https://nodejs.org/) ([npm](https://www.npmjs.com/) 使用)</span><span class="sxs-lookup"><span data-stu-id="527c2-122">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

---

## <a name="create-the-aspnet-core-web-app"></a><span data-ttu-id="527c2-123">ASP.NET Core Web アプリを作成する</span><span class="sxs-lookup"><span data-stu-id="527c2-123">Create the ASP.NET Core web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="527c2-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="527c2-124">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="527c2-125">*PATH* 環境変数で npm を検索するように Visual Studio を構成します。</span><span class="sxs-lookup"><span data-stu-id="527c2-125">Configure Visual Studio to look for npm in the *PATH* environment variable.</span></span> <span data-ttu-id="527c2-126">既定では、Visual Studio は、そのインストール ディレクトリ内で見つかった npm のバージョンを使用します。</span><span class="sxs-lookup"><span data-stu-id="527c2-126">By default, Visual Studio uses the version of npm found in its installation directory.</span></span> <span data-ttu-id="527c2-127">Visual Studio のこれらの説明に従ってください。</span><span class="sxs-lookup"><span data-stu-id="527c2-127">Follow these instructions in Visual Studio:</span></span>

1. <span data-ttu-id="527c2-128">**[ツール]** > **[オプション]** > **[プロジェクトおよびソリューション]** > **[Web パッケージ管理]** > **[外部 Web ツール]** の順に移動します。</span><span class="sxs-lookup"><span data-stu-id="527c2-128">Navigate to **Tools** > **Options** > **Projects and solutions** > **Web Package Management** > **External Web Tools**.</span></span>
1. <span data-ttu-id="527c2-129">リストから *[$(PATH)]* エントリを選択します。</span><span class="sxs-lookup"><span data-stu-id="527c2-129">Select the *$(PATH)* entry from the list.</span></span> <span data-ttu-id="527c2-130">上向き矢印をクリックして、エントリをリストの 2 番目の位置に移動します。</span><span class="sxs-lookup"><span data-stu-id="527c2-130">Click the up arrow to move the entry to the second position in the list.</span></span> <span data-ttu-id="527c2-131">余談ですが、最初のエントリは、プロジェクトのローカル パッケージを参照しています。</span><span class="sxs-lookup"><span data-stu-id="527c2-131">As an aside, the first entry refers to the project's local packages.</span></span>

    ![Visual Studio の構成](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

<span data-ttu-id="527c2-133">Visual Studio の構成が完了しました。</span><span class="sxs-lookup"><span data-stu-id="527c2-133">Visual Studio configuration is completed.</span></span> <span data-ttu-id="527c2-134">次はプロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="527c2-134">It's time to create the project.</span></span>

1. <span data-ttu-id="527c2-135">**[ファイル]** > **[新規]** > **[プロジェクト]** メニュー オプション、**[ASP.NET Core Web アプリケーション]** テンプレートの順に選択します。</span><span class="sxs-lookup"><span data-stu-id="527c2-135">Use the **File** > **New** > **Project** menu option and choose the **ASP.NET Core Web Application** template.</span></span>
1. <span data-ttu-id="527c2-136">プロジェクトに *SignalRWebPack* という名前を付け、**[OK]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="527c2-136">Name the project *SignalRWebPack*, and click the **OK** button.</span></span>
1. <span data-ttu-id="527c2-137">ターゲット フレームワークのドロップダウンから、*[.NET Core]* を選択し、フレームワーク セレクターのドロップダウンから *[ASP.NET Core 2.1]* を選択します。</span><span class="sxs-lookup"><span data-stu-id="527c2-137">Select *.NET Core* from the target framework drop-down, and select *ASP.NET Core 2.1* from the framework selector drop-down.</span></span> <span data-ttu-id="527c2-138">**空**のテンプレートを選択して、**[OK]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="527c2-138">Select the **Empty** template, and click the **OK** button.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="527c2-139">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="527c2-139">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="527c2-140">**統合端末**で次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="527c2-140">Run the following command in the **Integrated Terminal**:</span></span>

```console
dotnet new web -o SignalRWebPack
```

<span data-ttu-id="527c2-141">.NET Core をターゲットとする、空の ASP.NET Core Web アプリが *SignalRWebPack* ディレクトリ内に作成されます。</span><span class="sxs-lookup"><span data-stu-id="527c2-141">An empty ASP.NET Core web app, targeting .NET Core, is created in a *SignalRWebPack* directory.</span></span>

---

## <a name="configure-webpack-and-typescript"></a><span data-ttu-id="527c2-142">WebPack および TypeScript の構成</span><span class="sxs-lookup"><span data-stu-id="527c2-142">Configure Webpack and TypeScript</span></span>

<span data-ttu-id="527c2-143">次の手順では、TypeScript の JavaScript への変換と、クライアント側のリソースのバンドルを構成します。</span><span class="sxs-lookup"><span data-stu-id="527c2-143">The following steps configure the conversion of TypeScript to JavaScript and the bundling of client-side resources.</span></span>

1. <span data-ttu-id="527c2-144">プロジェクト ルートで次のコマンドを実行して、*package.json* ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="527c2-144">Execute the following command in the project root to create a *package.json* file:</span></span>

    ```console
    npm init -y
    ```

1. <span data-ttu-id="527c2-145">強調表示されているプロパティを *package.json* ファイルに追加します。</span><span class="sxs-lookup"><span data-stu-id="527c2-145">Add the highlighted property to the *package.json* file:</span></span>

    [!code-json[package.json](signalr-typescript-webpack/sample/snippets/package1.json?highlight=4)]

    <span data-ttu-id="527c2-146">`private` プロパティを `true` に設定して、次の手順でパッケージのインストールの警告が表示されないようにします。</span><span class="sxs-lookup"><span data-stu-id="527c2-146">Setting the `private` property to `true` prevents package installation warnings in the next step.</span></span>

1. <span data-ttu-id="527c2-147">必要な npm パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="527c2-147">Install the required npm packages.</span></span> <span data-ttu-id="527c2-148">プロジェクト ルートで、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="527c2-148">Execute the following command from the project root:</span></span>

    ```console
    npm install -D -E clean-webpack-plugin@0.1.19 css-loader@0.28.11 html-webpack-plugin@3.2.0 mini-css-extract-plugin@0.4.0 ts-loader@4.4.1 typescript@2.9.2 webpack@4.12.0 webpack-cli@3.0.6
    ```

    <span data-ttu-id="527c2-149">注目するべきコマンドの詳細:</span><span class="sxs-lookup"><span data-stu-id="527c2-149">Some command details to note:</span></span>

    * <span data-ttu-id="527c2-150">バージョン番号は、各パッケージ名の `@` 記号の後に続きます。</span><span class="sxs-lookup"><span data-stu-id="527c2-150">A version number follows the `@` sign for each package name.</span></span> <span data-ttu-id="527c2-151">npm によって、これらの特定のパッケージ バージョンがインストールされます。</span><span class="sxs-lookup"><span data-stu-id="527c2-151">npm installs those specific package versions.</span></span>
    * <span data-ttu-id="527c2-152">`-E` オプションは、[セマンティック バージョニング](https://semver.org/)範囲演算子を *package.json* に書き込む npm の既定の動作を無効にします。</span><span class="sxs-lookup"><span data-stu-id="527c2-152">The `-E` option disables npm's default behavior of writing [semantic versioning](https://semver.org/) range operators to *package.json*.</span></span> <span data-ttu-id="527c2-153">たとえば、`"webpack": "4.12.0"` が `"webpack": "^4.12.0"` の代わりに使用されています。</span><span class="sxs-lookup"><span data-stu-id="527c2-153">For example, `"webpack": "4.12.0"` is used instead of `"webpack": "^4.12.0"`.</span></span> <span data-ttu-id="527c2-154">このオプションにより、新しいパッケージ バージョンへの予期しないアップグレードが防止されます。</span><span class="sxs-lookup"><span data-stu-id="527c2-154">This option prevents unintended upgrades to newer package versions.</span></span>

    <span data-ttu-id="527c2-155">詳細については、[npm-install](https://docs.npmjs.com/cli/install) の公式ドキュメントを参照してください。</span><span class="sxs-lookup"><span data-stu-id="527c2-155">See the official [npm-install](https://docs.npmjs.com/cli/install) docs for more detail.</span></span>

1. <span data-ttu-id="527c2-156">*package.json* ファイルの `scripts` プロパティを次のスニペットで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="527c2-156">Replace the `scripts` property of the *package.json* file with the following snippet:</span></span>

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    <span data-ttu-id="527c2-157">スクリプトの説明 (一部):</span><span class="sxs-lookup"><span data-stu-id="527c2-157">Some explanation of the scripts:</span></span>

    * <span data-ttu-id="527c2-158">`build`: 開発モードでクライアント側のリソースをバンドルし、ファイルの変更を監視します。</span><span class="sxs-lookup"><span data-stu-id="527c2-158">`build`: Bundles your client-side resources in development mode and watches for file changes.</span></span> <span data-ttu-id="527c2-159">ファイル監視により、プロジェクト ファイルを変更するたびにバンドルが再生成されます。</span><span class="sxs-lookup"><span data-stu-id="527c2-159">The file watcher causes the bundle to regenerate each time a project file changes.</span></span> <span data-ttu-id="527c2-160">`mode` オプションは、ツリー シェイキングや縮小などの運用環境の最適化を無効にします。</span><span class="sxs-lookup"><span data-stu-id="527c2-160">The `mode` option disables production optimizations, such as tree shaking and minification.</span></span> <span data-ttu-id="527c2-161">`build` は開発でのみ使用します。</span><span class="sxs-lookup"><span data-stu-id="527c2-161">Only use `build` in development.</span></span>
    * <span data-ttu-id="527c2-162">`release`: 運用モードでクライアント側のリソースをバンドルします。</span><span class="sxs-lookup"><span data-stu-id="527c2-162">`release`: Bundles your client-side resources in production mode.</span></span>
    * <span data-ttu-id="527c2-163">`publish`: `release` スクリプトを実行して、運用モードでクライアント側のリソースをバンドルします。</span><span class="sxs-lookup"><span data-stu-id="527c2-163">`publish`: Runs the `release` script to bundle the client-side resources in production mode.</span></span> <span data-ttu-id="527c2-164">.NET Core CLI の [publish](/dotnet/core/tools/dotnet-publish) コマンドを呼び出してアプリを公開します。</span><span class="sxs-lookup"><span data-stu-id="527c2-164">It calls the .NET Core CLI's [publish](/dotnet/core/tools/dotnet-publish) command to publish the app.</span></span>

1. <span data-ttu-id="527c2-165">プロジェクト ルートに、次の内容を持つ *webpack.config.js* という名前のファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="527c2-165">Create a file named *webpack.config.js*, in the project root, with the following content:</span></span>

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/webpack.config.js)]

    <span data-ttu-id="527c2-166">上記のファイルは、Webpack コンパイルを構成します。</span><span class="sxs-lookup"><span data-stu-id="527c2-166">The preceding file configures the Webpack compilation.</span></span> <span data-ttu-id="527c2-167">注目するべき構成の詳細 (一部):</span><span class="sxs-lookup"><span data-stu-id="527c2-167">Some configuration details to note:</span></span>

    * <span data-ttu-id="527c2-168">`output` プロパティにより、*dist* の既定値がオーバーライドされます。</span><span class="sxs-lookup"><span data-stu-id="527c2-168">The `output` property overrides the default value of *dist*.</span></span> <span data-ttu-id="527c2-169">代わりにバンドルが *wwwroot* ディレクトリ内に生成されます。</span><span class="sxs-lookup"><span data-stu-id="527c2-169">The bundle is instead emitted in the *wwwroot* directory.</span></span>
    * <span data-ttu-id="527c2-170">`resolve.extensions` 配列には、SignalR クライアント JavaScript をインポートするための *.js* が含まれています。</span><span class="sxs-lookup"><span data-stu-id="527c2-170">The `resolve.extensions` array includes *.js* to import the SignalR client JavaScript.</span></span>

1. <span data-ttu-id="527c2-171">プロジェクト ルートに新しい *src* プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="527c2-171">Create a new *src* directory in the project root.</span></span> <span data-ttu-id="527c2-172">その目的は、プロジェクトのクライアント側アセットを格納することです。</span><span class="sxs-lookup"><span data-stu-id="527c2-172">Its purpose is to store the project's client-side assets.</span></span>

1. <span data-ttu-id="527c2-173">次のコンテンツを含む *src/index.html* を作成します。</span><span class="sxs-lookup"><span data-stu-id="527c2-173">Create *src/index.html* with the following content.</span></span>

    [!code-html[index.html](signalr-typescript-webpack/sample/src/index.html)]

    <span data-ttu-id="527c2-174">上記の HTML では、ホームページの定型マークアップが定義されます。</span><span class="sxs-lookup"><span data-stu-id="527c2-174">The preceding HTML defines the homepage's boilerplate markup.</span></span>

1. <span data-ttu-id="527c2-175">新しい *src/css* ディレクトリを作成します。</span><span class="sxs-lookup"><span data-stu-id="527c2-175">Create a new *src/css* directory.</span></span> <span data-ttu-id="527c2-176">その目的は、プロジェクトの *.css* ファイルを格納することです。</span><span class="sxs-lookup"><span data-stu-id="527c2-176">Its purpose is to store the project's *.css* files.</span></span>

1. <span data-ttu-id="527c2-177">次のコンテンツを含む *src/css/main.css* を作成します。</span><span class="sxs-lookup"><span data-stu-id="527c2-177">Create *src/css/main.css* with the following content:</span></span>

    [!code-css[main.css](signalr-typescript-webpack/sample/src/css/main.css)]

    <span data-ttu-id="527c2-178">上記の *main.css* ファイルは、アプリをスタイル設定します。</span><span class="sxs-lookup"><span data-stu-id="527c2-178">The preceding *main.css* file styles the app.</span></span>

1. <span data-ttu-id="527c2-179">次のコンテンツを含む *src/tsconfig.json* を作成します。</span><span class="sxs-lookup"><span data-stu-id="527c2-179">Create *src/tsconfig.json* with the following content:</span></span>

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/src/tsconfig.json)]

    <span data-ttu-id="527c2-180">上記のコードは、[ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5 対応の JavaScript を生成するように TypeScript コンパイラを構成します。</span><span class="sxs-lookup"><span data-stu-id="527c2-180">The preceding code configures the TypeScript compiler to produce [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5-compatible JavaScript.</span></span>

1. <span data-ttu-id="527c2-181">次のコンテンツを含む *src/index.ts* を作成します。</span><span class="sxs-lookup"><span data-stu-id="527c2-181">Create *src/index.ts* with the following content:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    <span data-ttu-id="527c2-182">上記の TypeScript は、DOM 要素への参照を取得し、次の 2 つのイベント ハンドラーをアタッチします。</span><span class="sxs-lookup"><span data-stu-id="527c2-182">The preceding TypeScript retrieves references to DOM elements and attaches two event handlers:</span></span>

    * <span data-ttu-id="527c2-183">`keyup`: ユーザーがテキスト ボックスに `tbMessage` として識別されるものを入力すると、このイベントが発生します。</span><span class="sxs-lookup"><span data-stu-id="527c2-183">`keyup`: This event fires when the user types something in the textbox identified as `tbMessage`.</span></span> <span data-ttu-id="527c2-184">ユーザーが **Enter** キーを押すと、`send` 関数が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="527c2-184">The `send` function is called when the user presses the **Enter** key.</span></span>
    * <span data-ttu-id="527c2-185">`click`: ユーザーが **[送信]** ボタンをクリックすると、このイベントが発生します。</span><span class="sxs-lookup"><span data-stu-id="527c2-185">`click`: This event fires when the user clicks the **Send** button.</span></span> <span data-ttu-id="527c2-186">`send` 関数が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="527c2-186">The `send` function is called.</span></span>

## <a name="configure-the-aspnet-core-app"></a><span data-ttu-id="527c2-187">ASP.NET Core アプリを構成する</span><span class="sxs-lookup"><span data-stu-id="527c2-187">Configure the ASP.NET Core app</span></span>

1. <span data-ttu-id="527c2-188">`Startup.Configure` メソッドで提供されたコードにより、*Hello World!* が表示されます。</span><span class="sxs-lookup"><span data-stu-id="527c2-188">The code provided in the `Startup.Configure` method displays *Hello World!*.</span></span> <span data-ttu-id="527c2-189">`app.Run` メソッド呼び出しを、[UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) と [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) の呼び出しで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="527c2-189">Replace the `app.Run` method call with calls to [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_UseStaticDefaultFiles)]

    <span data-ttu-id="527c2-190">上記のコードにより、サーバーが *index.html* ファイルを見つけて提供することができます。ユーザーがファイルの完全な URL または Web アプリのルート URL を入力するかどうかは関係ありません。</span><span class="sxs-lookup"><span data-stu-id="527c2-190">The preceding code allows the server to locate and serve the *index.html* file, whether the user enters its full URL or the root URL of the web app.</span></span>

1. <span data-ttu-id="527c2-191">`Startup.ConfigureServices` メソッドで [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="527c2-191">Call [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="527c2-192">これにより SignalR サービスがプロジェクトに追加されます。</span><span class="sxs-lookup"><span data-stu-id="527c2-192">It adds the SignalR services to your project.</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_AddSignalR)]

1. <span data-ttu-id="527c2-193">*/hub* ルートを `ChatHub` ハブにマップします。</span><span class="sxs-lookup"><span data-stu-id="527c2-193">Map a */hub* route to the `ChatHub` hub.</span></span> <span data-ttu-id="527c2-194">`Startup.Configure` メソッドの末尾に、次の行を追加します。</span><span class="sxs-lookup"><span data-stu-id="527c2-194">Add the following lines at the end of the `Startup.Configure` method:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_UseSignalR)]

1. <span data-ttu-id="527c2-195">プロジェクト ルートに *Hubs* という新しいディレクトリを作成します。</span><span class="sxs-lookup"><span data-stu-id="527c2-195">Create a new directory, called *Hubs*, in the project root.</span></span> <span data-ttu-id="527c2-196">その目的は、次の手順で作成される SignalR ハブを格納することです。</span><span class="sxs-lookup"><span data-stu-id="527c2-196">Its purpose is to store the SignalR hub, which is created in the next step.</span></span>

1. <span data-ttu-id="527c2-197">次のコードを使用して、*Hubs/ChatHub.cs* を作成します。</span><span class="sxs-lookup"><span data-stu-id="527c2-197">Create hub *Hubs/ChatHub.cs* with the following code:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. <span data-ttu-id="527c2-198">`ChatHub` 参照を解決するには、次のコードを *Startup.cs* ファイルの先頭に追加します。</span><span class="sxs-lookup"><span data-stu-id="527c2-198">Add the following code at the top of the *Startup.cs* file to resolve the `ChatHub` reference:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a><span data-ttu-id="527c2-199">クライアントとサーバーの通信を有効にする</span><span class="sxs-lookup"><span data-stu-id="527c2-199">Enable client and server communication</span></span>

<span data-ttu-id="527c2-200">アプリには現在、メッセージを送信するための単純なフォームが表示されています。</span><span class="sxs-lookup"><span data-stu-id="527c2-200">The app currently displays a simple form to send messages.</span></span> <span data-ttu-id="527c2-201">送信しようとしても、何も起こりません。</span><span class="sxs-lookup"><span data-stu-id="527c2-201">Nothing happens when you try to do so.</span></span> <span data-ttu-id="527c2-202">サーバーは特定のルートをリッスンしていますが、メッセージの送信については何も行いません。</span><span class="sxs-lookup"><span data-stu-id="527c2-202">The server is listening to a specific route but does nothing with sent messages.</span></span>

1. <span data-ttu-id="527c2-203">プロジェクト ルートで、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="527c2-203">Execute the following command at the project root:</span></span>

    ```console
    npm install @aspnet/signalr
    ```

    <span data-ttu-id="527c2-204">上記のコマンドにより [SignalR TypeScript クライアント](https://www.npmjs.com/package/@aspnet/signalr) がインストールされ、クライアントがサーバーにメッセージを送信できるようになります。</span><span class="sxs-lookup"><span data-stu-id="527c2-204">The preceding command installs the [SignalR TypeScript client](https://www.npmjs.com/package/@aspnet/signalr), which allows the client to send messages to the server.</span></span>

1. <span data-ttu-id="527c2-205">強調表示されたコードを *src/index.ts* ファイルに追加します。</span><span class="sxs-lookup"><span data-stu-id="527c2-205">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    <span data-ttu-id="527c2-206">上記のコードは、サーバーからのメッセージの受信をサポートします。</span><span class="sxs-lookup"><span data-stu-id="527c2-206">The preceding code supports receiving messages from the server.</span></span> <span data-ttu-id="527c2-207">`HubConnectionBuilder` クラスは、サーバー接続を構成するための新しいビルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="527c2-207">The `HubConnectionBuilder` class creates a new builder for configuring the server connection.</span></span> <span data-ttu-id="527c2-208">`withUrl` 関数は、ハブ URL を構成します。</span><span class="sxs-lookup"><span data-stu-id="527c2-208">The `withUrl` function configures the hub URL.</span></span>

    <span data-ttu-id="527c2-209">SignalR により、クライアントとサーバー間でのメッセージのやり取りが可能になります。</span><span class="sxs-lookup"><span data-stu-id="527c2-209">SignalR enables the exchange of messages between a client and a server.</span></span> <span data-ttu-id="527c2-210">各メッセージには特定の名前があります。</span><span class="sxs-lookup"><span data-stu-id="527c2-210">Each message has a specific name.</span></span> <span data-ttu-id="527c2-211">たとえば、メッセージ ゾーンに新しいメッセージを表示するためのロジックを実行する `messageReceived` という名前を持つメッセージを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="527c2-211">For example, you can have messages with the name `messageReceived` that execute the logic responsible for displaying the new message in the messages zone.</span></span> <span data-ttu-id="527c2-212">特定のメッセージをリッスンするには、`on` 関数を使用します。</span><span class="sxs-lookup"><span data-stu-id="527c2-212">Listening to a specific message can be done via the `on` function.</span></span> <span data-ttu-id="527c2-213">任意の数のメッセージ名をリッスンできます。</span><span class="sxs-lookup"><span data-stu-id="527c2-213">You can listen to any number of message names.</span></span> <span data-ttu-id="527c2-214">作成者の名前や受信したメッセージの内容など、パラメーターをメッセージに渡すこともできます。</span><span class="sxs-lookup"><span data-stu-id="527c2-214">It's also possible to pass parameters to the message, such as the author's name and the content of the message received.</span></span> <span data-ttu-id="527c2-215">クライアントがメッセージを受信すると、`innerHTML` 属性に作成者の名前とメッセージ コンテンツを持つ新しい `div` 要素が作成されます。</span><span class="sxs-lookup"><span data-stu-id="527c2-215">Once the client receives a message, a new `div` element is created with the author's name and the message content in its `innerHTML` attribute.</span></span> <span data-ttu-id="527c2-216">これはメッセージを表示する主要な `div` 要素に追加されます。</span><span class="sxs-lookup"><span data-stu-id="527c2-216">It's added to the main `div` element displaying the messages.</span></span>

1. <span data-ttu-id="527c2-217">これでクライアントがメッセージを受信できるようになったので、メッセージを送信するように構成します。</span><span class="sxs-lookup"><span data-stu-id="527c2-217">Now that the client can receive a message, configure it to send messages.</span></span> <span data-ttu-id="527c2-218">強調表示されたコードを *src/index.ts* ファイルに追加します。</span><span class="sxs-lookup"><span data-stu-id="527c2-218">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/src/index.ts?highlight=34-35)]

    <span data-ttu-id="527c2-219">WebSocket 接続を介してメッセージを送信するには、`send` メソッドを呼び出す必要があります。</span><span class="sxs-lookup"><span data-stu-id="527c2-219">Sending a message through the WebSockets connection requires calling the `send` method.</span></span> <span data-ttu-id="527c2-220">メソッドの最初のパラメーターは、メッセージ名です。</span><span class="sxs-lookup"><span data-stu-id="527c2-220">The method's first parameter is the message name.</span></span> <span data-ttu-id="527c2-221">メッセージ データは、他のパラメーターに存在しています。</span><span class="sxs-lookup"><span data-stu-id="527c2-221">The message data inhabits the other parameters.</span></span> <span data-ttu-id="527c2-222">この例では、`newMessage` として識別されたメッセージがサーバーに送信されます。</span><span class="sxs-lookup"><span data-stu-id="527c2-222">In this example, a message identified as `newMessage` is sent to the server.</span></span> <span data-ttu-id="527c2-223">メッセージは、ユーザー名と、テキスト ボックスへのユーザー入力で構成されます。</span><span class="sxs-lookup"><span data-stu-id="527c2-223">The message consists of the username and the user input from a text box.</span></span> <span data-ttu-id="527c2-224">送信が機能していると、テキスト ボックスの値はクリアされます。</span><span class="sxs-lookup"><span data-stu-id="527c2-224">If the send works, the text box value is cleared.</span></span>

1. <span data-ttu-id="527c2-225">強調表示されているメソッドを `ChatHub` クラスに追加します。</span><span class="sxs-lookup"><span data-stu-id="527c2-225">Add the highlighted method to the `ChatHub` class:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/Hubs/ChatHub.cs?highlight=8-11)]

    <span data-ttu-id="527c2-226">上記のコードは、サーバーがメッセージを受信すると、受信したメッセージをすべての接続されているユーザーにブロードキャストします。</span><span class="sxs-lookup"><span data-stu-id="527c2-226">The preceding code broadcasts received messages to all connected users once the server receives them.</span></span> <span data-ttu-id="527c2-227">すべてのメッセージを受信するのに、ジェネリック `on` メソッドは必要ありません。</span><span class="sxs-lookup"><span data-stu-id="527c2-227">It's unnecessary to have a generic `on` method to receive all the messages.</span></span> <span data-ttu-id="527c2-228">メソッドはメッセージ名のサフィックスに基づいて名前が付けられています。</span><span class="sxs-lookup"><span data-stu-id="527c2-228">A method named after the message name suffices.</span></span>

    <span data-ttu-id="527c2-229">この例では、TypeScript クライアントが `newMessage` として識別されるメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="527c2-229">In this example, the TypeScript client sends a message identified as `newMessage`.</span></span> <span data-ttu-id="527c2-230">C# `NewMessage` メソッドは、データがクライアントから送信されることを想定しています。</span><span class="sxs-lookup"><span data-stu-id="527c2-230">The C# `NewMessage` method expects the data sent by the client.</span></span> <span data-ttu-id="527c2-231">[Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all) で [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="527c2-231">A call is made to the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method on [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span></span> <span data-ttu-id="527c2-232">受信したメッセージは、ハブに接続されているすべてのクライアントに送信されます。</span><span class="sxs-lookup"><span data-stu-id="527c2-232">The received messages are sent to all clients connected to the hub.</span></span>

## <a name="test-the-app"></a><span data-ttu-id="527c2-233">アプリのテスト</span><span class="sxs-lookup"><span data-stu-id="527c2-233">Test the app</span></span>

<span data-ttu-id="527c2-234">次の手順で、アプリの動作を確認します。</span><span class="sxs-lookup"><span data-stu-id="527c2-234">Confirm that the app works with the following steps.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="527c2-235">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="527c2-235">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="527c2-236">*リリース* モードで Webpack を実行します。</span><span class="sxs-lookup"><span data-stu-id="527c2-236">Run Webpack in *release* mode.</span></span> <span data-ttu-id="527c2-237">**パッケージ マネージャー コンソール** ウィンドウを使用して、プロジェクト ルートで次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="527c2-237">Using the **Package Manager Console** window, execute the following command in the project root:</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="527c2-238">**[デバッグ]** > **[デバッグなしで開始]** の順に選択して、デバッガーをアタッチせずに、ブラウザーでアプリを起動します。</span><span class="sxs-lookup"><span data-stu-id="527c2-238">Select **Debug** > **Start without debugging** to launch the app in a browser without attaching the debugger.</span></span> <span data-ttu-id="527c2-239">*wwroot/index.html* ファイルは `http://localhost:<port_number>` で提供されます。</span><span class="sxs-lookup"><span data-stu-id="527c2-239">The *wwwroot/index.html* file is served at `http://localhost:<port_number>`.</span></span>

1. <span data-ttu-id="527c2-240">別のブラウザー インスタンスを開きます (任意のブラウザー)。</span><span class="sxs-lookup"><span data-stu-id="527c2-240">Open another browser instance (any browser).</span></span> <span data-ttu-id="527c2-241">アドレス バーに URL を貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="527c2-241">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="527c2-242">いずれかのブラウザーを選択し、**[メッセージ]** テキスト ボックスにメッセージを入力し、**[送信]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="527c2-242">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="527c2-243">次の瞬間、両方のページに一意のユーザー名とメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="527c2-243">The unique user name and message are displayed on both pages instantly.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="527c2-244">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="527c2-244">.NET Core CLI</span></span>](#tab/netcore-cli)

1. <span data-ttu-id="527c2-245">プロジェクト ルートで次のコマンドを実行して、*リリース* モードで Webpack を実行します。</span><span class="sxs-lookup"><span data-stu-id="527c2-245">Run Webpack in *release* mode by executing the following command in the project root:</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="527c2-246">プロジェクト ルートで次のコマンドを実行して、アプリをビルドして実行します。</span><span class="sxs-lookup"><span data-stu-id="527c2-246">Build and run the app by executing the following command in the project root:</span></span>

    ```console
    dotnet run
    ```

    <span data-ttu-id="527c2-247">Web サーバーは、アプリを起動して、localhost 上で使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="527c2-247">The web server starts the app and makes it available on localhost.</span></span>

1. <span data-ttu-id="527c2-248">ブラウザーを開いて `http://localhost:<port_number>` にアクセスします。</span><span class="sxs-lookup"><span data-stu-id="527c2-248">Open a browser to `http://localhost:<port_number>`.</span></span> <span data-ttu-id="527c2-249">*wwwroot/index.html* ファイルが提供されます。</span><span class="sxs-lookup"><span data-stu-id="527c2-249">The *wwwroot/index.html* file is served.</span></span> <span data-ttu-id="527c2-250">アドレス バーから URL をコピーします。</span><span class="sxs-lookup"><span data-stu-id="527c2-250">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="527c2-251">別のブラウザー インスタンスを開きます (任意のブラウザー)。</span><span class="sxs-lookup"><span data-stu-id="527c2-251">Open another browser instance (any browser).</span></span> <span data-ttu-id="527c2-252">アドレス バーに URL を貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="527c2-252">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="527c2-253">いずれかのブラウザーを選択し、**[メッセージ]** テキスト ボックスにメッセージを入力し、**[送信]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="527c2-253">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="527c2-254">次の瞬間、両方のページに一意のユーザー名とメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="527c2-254">The unique user name and message are displayed on both pages instantly.</span></span>

---

![両方のブラウザー ウィンドウに表示されるメッセージ](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

## <a name="additional-resources"></a><span data-ttu-id="527c2-256">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="527c2-256">Additional resources</span></span>

* <xref:signalr/javascript-client>
* <xref:signalr/hubs>
