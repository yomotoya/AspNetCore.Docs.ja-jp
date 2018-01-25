---
title: "Angular プロジェクト テンプレートを使用します。"
author: SteveSandersonMS
description: "角速度と角運動の CLI for ASP.NET Core 単一ページ アプリケーション (SPA) リリース候補プロジェクト テンプレートを使って開始する方法を説明します。"
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 12/06/2017
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/angular
ms.openlocfilehash: 4162b1c26e9d278c811f691c4277d4de25adb204
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
# <a name="use-the-angular-project-template-release-candidate"></a><span data-ttu-id="9326e-103">Angular プロジェクト テンプレート (リリース候補) を使用します。</span><span class="sxs-lookup"><span data-stu-id="9326e-103">Use the Angular project template (release candidate)</span></span>

> [!NOTE]
> <span data-ttu-id="9326e-104">このドキュメントは、リリースの角度プロジェクト テンプレートではありません。</span><span class="sxs-lookup"><span data-stu-id="9326e-104">This documentation isn't about the released Angular project template.</span></span> <span data-ttu-id="9326e-105">**このドキュメントは、角度のテンプレートのリリース候補です。**</span><span class="sxs-lookup"><span data-stu-id="9326e-105">**This documentation is about the release candidate of the Angular template.**</span></span> <span data-ttu-id="9326e-106">初期 2018 でリリースされたバージョンを出荷する予定です。</span><span class="sxs-lookup"><span data-stu-id="9326e-106">We hope to ship the released version in early 2018.</span></span>

<span data-ttu-id="9326e-107">更新された角度のプロジェクト テンプレートは、角運動の 5 と角運動の CLI を使用して、クライアント側の豊富なユーザー インターフェイス (UI) を実装するアプリケーション、ASP.NET Core の便利な開始点を提供します。</span><span class="sxs-lookup"><span data-stu-id="9326e-107">The updated Angular project template provides a convenient starting point for ASP.NET Core apps using Angular 5 and the Angular CLI to implement a rich, client-side user interface (UI).</span></span>

<span data-ttu-id="9326e-108">テンプレートは、ASP.NET Core プロジェクト API バックエンドとして機能して、UI として機能する角度 CLI プロジェクトの作成と同じです。</span><span class="sxs-lookup"><span data-stu-id="9326e-108">The template is equivalent to creating an ASP.NET Core project to act as an API backend and an Angular CLI project to act as a UI.</span></span> <span data-ttu-id="9326e-109">テンプレートは、1 つのアプリ プロジェクトの両方のプロジェクト タイプをホストしているの利便性を提供します。</span><span class="sxs-lookup"><span data-stu-id="9326e-109">The template offers the convenience of hosting both project types in a single app project.</span></span> <span data-ttu-id="9326e-110">その結果、アプリ プロジェクトをビルドおよび 1 つの単位として公開します。</span><span class="sxs-lookup"><span data-stu-id="9326e-110">Consequently, the app project can be built and published as a single unit.</span></span>

## <a name="create-a-new-app"></a><span data-ttu-id="9326e-111">新しいアプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="9326e-111">Create a new app</span></span>

<span data-ttu-id="9326e-112">確認した結果を作業を開始する[角運動の更新されたプロジェクト テンプレートがインストールされている](xref:spa/index#installation)です。</span><span class="sxs-lookup"><span data-stu-id="9326e-112">To get started, ensure you've [installed the updated Angular project template](xref:spa/index#installation).</span></span> <span data-ttu-id="9326e-113">これらの手順は、.NET Core で含まれている前の角度のプロジェクト テンプレートを適用しない 2.0.x SDK。</span><span class="sxs-lookup"><span data-stu-id="9326e-113">These instructions don't apply to the previous Angular project template included in the .NET Core 2.0.x SDK.</span></span>

<span data-ttu-id="9326e-114">コマンドを使用してコマンド プロンプトから、新しいプロジェクトを作成する`dotnet new angular`空のディレクトリにします。</span><span class="sxs-lookup"><span data-stu-id="9326e-114">Create a new project from a command prompt using the command `dotnet new angular` in an empty directory.</span></span> <span data-ttu-id="9326e-115">次のコマンドがでアプリを作成するなど、 *my-新しい-アプリ*ディレクトリとそのディレクトリへの切り替え。</span><span class="sxs-lookup"><span data-stu-id="9326e-115">For example, the following commands create the app in a *my-new-app* directory and switch to that directory:</span></span>

```console
dotnet new angular -o my-new-app
cd my-new-app
```

<span data-ttu-id="9326e-116">Visual Studio または .NET Core CLI からアプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="9326e-116">Run the app from either Visual Studio or the .NET Core CLI:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9326e-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9326e-117">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="9326e-118">開く、生成された*.csproj*ファイル、およびそこから通常の方法でアプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="9326e-118">Open the generated *.csproj* file, and run the app as normal from there.</span></span>

<span data-ttu-id="9326e-119">ビルド プロセスでは、数分かかることが、最初の実行で npm の依存関係を復元します。</span><span class="sxs-lookup"><span data-stu-id="9326e-119">The build process restores npm dependencies on the first run, which can take several minutes.</span></span> <span data-ttu-id="9326e-120">後続のビルドは非常に高速です。</span><span class="sxs-lookup"><span data-stu-id="9326e-120">Subsequent builds are much faster.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="9326e-121">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="9326e-121">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="9326e-122">という環境変数があることを確認`ASPNETCORE_Environment`の値を持つ`Development`します。</span><span class="sxs-lookup"><span data-stu-id="9326e-122">Ensure you have an environment variable called `ASPNETCORE_Environment` with a value of `Development`.</span></span> <span data-ttu-id="9326e-123">Windows PowerShell 以外の入力を要求) の「、で実行される`SET ASPNETCORE_Environment=Development`です。</span><span class="sxs-lookup"><span data-stu-id="9326e-123">On Windows (in non-PowerShell prompts), run `SET ASPNETCORE_Environment=Development`.</span></span> <span data-ttu-id="9326e-124">Linux または macOS、実行`export ASPNETCORE_Environment=Development`です。</span><span class="sxs-lookup"><span data-stu-id="9326e-124">On Linux or macOS, run `export ASPNETCORE_Environment=Development`.</span></span>

<span data-ttu-id="9326e-125">実行`dotnet build`正しくビルドするのには、アプリを確認してください。</span><span class="sxs-lookup"><span data-stu-id="9326e-125">Run `dotnet build` to verify the app builds correctly.</span></span> <span data-ttu-id="9326e-126">最初の実行には、ビルド プロセスは、数分かかることが npm の依存関係を復元します。</span><span class="sxs-lookup"><span data-stu-id="9326e-126">On the first run, the build process restores npm dependencies, which can take several minutes.</span></span> <span data-ttu-id="9326e-127">後続のビルドは非常に高速です。</span><span class="sxs-lookup"><span data-stu-id="9326e-127">Subsequent builds are much faster.</span></span>

<span data-ttu-id="9326e-128">実行`dotnet run`でアプリを起動します。</span><span class="sxs-lookup"><span data-stu-id="9326e-128">Run `dotnet run` to start the app.</span></span> <span data-ttu-id="9326e-129">次のようなメッセージが記録されます。</span><span class="sxs-lookup"><span data-stu-id="9326e-129">A message similar to the following is logged:</span></span>

```console
Now listening on: http://localhost:<port>
```

<span data-ttu-id="9326e-130">ブラウザーでこの URL に移動します。</span><span class="sxs-lookup"><span data-stu-id="9326e-130">Navigate to this URL in a browser.</span></span>

<span data-ttu-id="9326e-131">アプリは、バック グラウンドで角度 CLI サーバーのインスタンスを開始します。</span><span class="sxs-lookup"><span data-stu-id="9326e-131">The app starts up an instance of the Angular CLI server in the background.</span></span> <span data-ttu-id="9326e-132">次のようなメッセージが記録されます: *NG ライブ開発サーバーは、localhost でリッスンしている:&lt;otherport&gt;、http://localhost にブラウザーを開きます:&lt;otherport&gt; /*.</span><span class="sxs-lookup"><span data-stu-id="9326e-132">A message similar to the following is logged: *NG Live Development Server is listening on localhost:&lt;otherport&gt;, open your browser on http://localhost:&lt;otherport&gt;/*.</span></span> <span data-ttu-id="9326e-133">このメッセージは無視&mdash;が**いない**結合された ASP.NET Core と角運動 CLI アプリの URL。</span><span class="sxs-lookup"><span data-stu-id="9326e-133">Ignore this message&mdash;it's **not** the URL for the combined ASP.NET Core and Angular CLI app.</span></span>

---

<span data-ttu-id="9326e-134">プロジェクト テンプレートは、ASP.NET Core アプリケーションと角運動のアプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="9326e-134">The project template creates an ASP.NET Core app and an Angular app.</span></span> <span data-ttu-id="9326e-135">ASP.NET Core アプリケーションがデータ アクセス、承認、およびその他のサーバー側の懸念事項に使用するためのものです。</span><span class="sxs-lookup"><span data-stu-id="9326e-135">The ASP.NET Core app is intended to be used for data access, authorization, and other server-side concerns.</span></span> <span data-ttu-id="9326e-136">内に存在する角度のアプリ、 *ClientApp*サブディレクトリがすべての UI に関する注意事項を使用するためです。</span><span class="sxs-lookup"><span data-stu-id="9326e-136">The Angular app, residing in the *ClientApp* subdirectory, is intended to be used for all UI concerns.</span></span>

## <a name="add-pages-images-styles-modules-etc"></a><span data-ttu-id="9326e-137">ページ、画像、スタイル、モジュールなどを追加します。</span><span class="sxs-lookup"><span data-stu-id="9326e-137">Add pages, images, styles, modules, etc.</span></span>

<span data-ttu-id="9326e-138">*ClientApp*ディレクトリには、標準的な角度 CLI アプリが含まれています。</span><span class="sxs-lookup"><span data-stu-id="9326e-138">The *ClientApp* directory contains a standard Angular CLI app.</span></span> <span data-ttu-id="9326e-139">公式を参照してください[Angular ドキュメント](https://github.com/angular/angular-cli/wiki)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="9326e-139">See the official [Angular documentation](https://github.com/angular/angular-cli/wiki) for more information.</span></span>

<span data-ttu-id="9326e-140">このテンプレートで作成した角度のアプリと角運動の CLI 自体によって作成された 1 つのわずかな違いがあります (を介して`ng new`)。 ただし、アプリの機能は変更されていません。</span><span class="sxs-lookup"><span data-stu-id="9326e-140">There are slight differences between the Angular app created by this template and the one created by Angular CLI itself (via `ng new`); however, the app's capabilities are unchanged.</span></span> <span data-ttu-id="9326e-141">テンプレートによって作成されたアプリが含まれています、[ブートス トラップ](https://getbootstrap.com/)-ベースのレイアウトとルーティングの基本的な例です。</span><span class="sxs-lookup"><span data-stu-id="9326e-141">The app created by the template contains a [Bootstrap](https://getbootstrap.com/)-based layout and a basic routing example.</span></span>

## <a name="run-ng-commands"></a><span data-ttu-id="9326e-142">Ng コマンドを実行します</span><span class="sxs-lookup"><span data-stu-id="9326e-142">Run ng commands</span></span>

<span data-ttu-id="9326e-143">コマンド プロンプトでに切り替えると、 *ClientApp*サブディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="9326e-143">In a command prompt, switch to the *ClientApp* subdirectory:</span></span>

```console
cd ClientApp
```

<span data-ttu-id="9326e-144">ある場合、`ng`グローバルにインストールされているツールを実行してそのコマンドのです。</span><span class="sxs-lookup"><span data-stu-id="9326e-144">If you have the `ng` tool installed globally, you can run any of its commands.</span></span> <span data-ttu-id="9326e-145">たとえば、実行`ng lint`、 `ng test`、またはその他の[Angular CLI コマンド](https://github.com/angular/angular-cli/wiki#additional-commands)です。</span><span class="sxs-lookup"><span data-stu-id="9326e-145">For example, you can run `ng lint`, `ng test`, or any of the other [Angular CLI commands](https://github.com/angular/angular-cli/wiki#additional-commands).</span></span> <span data-ttu-id="9326e-146">実行する必要はありません`ng serve`しかし、ASP.NET Core アプリケーションがサービス中のアプリのサーバー側とクライアント側の両方の部分を処理するためです。</span><span class="sxs-lookup"><span data-stu-id="9326e-146">There's no need to run `ng serve` though, because your ASP.NET Core app deals with serving both server-side and client-side parts of your app.</span></span> <span data-ttu-id="9326e-147">使用して内部的には、`ng serve`開発します。</span><span class="sxs-lookup"><span data-stu-id="9326e-147">Internally, it uses `ng serve` in development.</span></span>

<span data-ttu-id="9326e-148">いない場合、`ng`ツールをインストールするには、実行`npm run ng`代わりにします。</span><span class="sxs-lookup"><span data-stu-id="9326e-148">If you don't have the `ng` tool installed, run `npm run ng` instead.</span></span> <span data-ttu-id="9326e-149">たとえば、実行`npm run ng lint`または`npm run ng test`です。</span><span class="sxs-lookup"><span data-stu-id="9326e-149">For example, you can run `npm run ng lint` or `npm run ng test`.</span></span>

## <a name="install-npm-packages"></a><span data-ttu-id="9326e-150">Npm パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="9326e-150">Install npm packages</span></span>

<span data-ttu-id="9326e-151">サード パーティ製の npm パッケージをインストールするでコマンド プロンプトを使用して、 *ClientApp*サブディレクトリです。</span><span class="sxs-lookup"><span data-stu-id="9326e-151">To install third-party npm packages, use a command prompt in the *ClientApp* subdirectory.</span></span> <span data-ttu-id="9326e-152">例:</span><span class="sxs-lookup"><span data-stu-id="9326e-152">For example:</span></span>

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a><span data-ttu-id="9326e-153">発行と配置</span><span class="sxs-lookup"><span data-stu-id="9326e-153">Publish and deploy</span></span>

<span data-ttu-id="9326e-154">開発中のアプリは、開発者の利便性の最適化モードで実行されます。</span><span class="sxs-lookup"><span data-stu-id="9326e-154">In development, the app runs in a mode optimized for developer convenience.</span></span> <span data-ttu-id="9326e-155">たとえば、JavaScript のバンドルは、(をデバッグする場合は、元の TypeScript コードを確認できます) をソース マップを含めます。</span><span class="sxs-lookup"><span data-stu-id="9326e-155">For example, JavaScript bundles include source maps (so that when debugging, you can see your original TypeScript code).</span></span> <span data-ttu-id="9326e-156">アプリはディスク上の TypeScript、HTML および CSS ファイルの変更を監視し、自動的にコンパイルされそれらのファイルの変更が発生したときに再読み込みされます。</span><span class="sxs-lookup"><span data-stu-id="9326e-156">The app watches for TypeScript, HTML, and CSS file changes on disk and automatically recompiles and reloads when it sees those files change.</span></span>

<span data-ttu-id="9326e-157">実稼働環境でパフォーマンスが最適化された、アプリのバージョンを提供します。</span><span class="sxs-lookup"><span data-stu-id="9326e-157">In production, serve a version of your app that's optimized for performance.</span></span> <span data-ttu-id="9326e-158">これは、自動的に実行される構成されます。</span><span class="sxs-lookup"><span data-stu-id="9326e-158">This is configured to happen automatically.</span></span> <span data-ttu-id="9326e-159">パブリッシュすると、ビルド構成を生成、縮小された、先行時間 (AoT) は、クライアント側コードのビルドをコンパイルします。</span><span class="sxs-lookup"><span data-stu-id="9326e-159">When you publish, the build configuration emits a minified, ahead-of-time (AoT) compiled build of your client-side code.</span></span> <span data-ttu-id="9326e-160">運用環境ビルド開発ビルドとは異なり、サーバーにインストールされている Node.js を必要としない (有効にしている場合を除き、[サーバー側の事前](#server-side-rendering))。</span><span class="sxs-lookup"><span data-stu-id="9326e-160">Unlike the development build, the production build doesn't require Node.js to be installed on the server (unless you have enabled [server-side prerendering](#server-side-rendering)).</span></span>

<span data-ttu-id="9326e-161">標準を使用する[ASP.NET Core のホストと展開方法](xref:host-and-deploy/index)です。</span><span class="sxs-lookup"><span data-stu-id="9326e-161">You can use standard [ASP.NET Core hosting and deployment methods](xref:host-and-deploy/index).</span></span>

## <a name="run-ng-serve-independently"></a><span data-ttu-id="9326e-162">"Ng serve"を個別に実行します。</span><span class="sxs-lookup"><span data-stu-id="9326e-162">Run "ng serve" independently</span></span>

<span data-ttu-id="9326e-163">プロジェクトが ASP.NET Core アプリケーションの開発モードの開始時に、バック グラウンドで角度 CLI サーバーの独自のインスタンスを開始するように構成します。</span><span class="sxs-lookup"><span data-stu-id="9326e-163">The project is configured to start its own instance of the Angular CLI server in the background when the ASP.NET Core app starts in development mode.</span></span> <span data-ttu-id="9326e-164">これは、機能は、別のサーバーを手動で実行する必要がないため便利です。</span><span class="sxs-lookup"><span data-stu-id="9326e-164">This is convenient because you don't have to run a separate server manually.</span></span>

<span data-ttu-id="9326e-165">この既定の設定の欠点があります。</span><span class="sxs-lookup"><span data-stu-id="9326e-165">There's a drawback to this default setup.</span></span> <span data-ttu-id="9326e-166">C# コードとアプリを再起動する必要があります、ASP.NET Core を変更するたびに、角運動 CLI サーバーを再起動します。</span><span class="sxs-lookup"><span data-stu-id="9326e-166">Each time you modify your C# code and your ASP.NET Core app needs to restart, the Angular CLI server restarts.</span></span> <span data-ttu-id="9326e-167">バックアップを開始するには、約 10 秒が必要です。</span><span class="sxs-lookup"><span data-stu-id="9326e-167">Around 10 seconds is required to start back up.</span></span> <span data-ttu-id="9326e-168">頻繁に c# コードの編集を加えようとしての角度の CLI を再起動するまで待機しない場合は、ASP.NET Core プロセスとは無関係に Angular CLI server 外部でを実行します。</span><span class="sxs-lookup"><span data-stu-id="9326e-168">If you're making frequent C# code edits and don't want to wait for Angular CLI to restart, run the Angular CLI server externally, independently of the ASP.NET Core process.</span></span> <span data-ttu-id="9326e-169">これを行うには。</span><span class="sxs-lookup"><span data-stu-id="9326e-169">To do so:</span></span>

1. <span data-ttu-id="9326e-170">コマンド プロンプトでに切り替えると、 *ClientApp*サブディレクトリ、および角度 CLI 開発サーバーの起動。</span><span class="sxs-lookup"><span data-stu-id="9326e-170">In a command prompt, switch to the *ClientApp* subdirectory, and launch the Angular CLI development server:</span></span>

    ```console
    cd ClientApp
    npm start
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="9326e-171">使用して`npm start`いない Angular CLI 開発サーバーを起動する`ng serve`いるためで構成*package.json*尊重します。</span><span class="sxs-lookup"><span data-stu-id="9326e-171">Use `npm start` to launch the Angular CLI development server, not `ng serve`, so that the configuration in *package.json* is respected.</span></span> <span data-ttu-id="9326e-172">Angular CLI サーバーに追加のパラメーターを渡すにそれらに関連する追加`scripts`線、 *package.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="9326e-172">To pass additional parameters to the Angular CLI server, add them to the relevant `scripts` line in your *package.json* file.</span></span>

2. <span data-ttu-id="9326e-173">独自のいずれかを起動せずに外部の角度 CLI インスタンスを使用する ASP.NET Core アプリケーションを変更します。</span><span class="sxs-lookup"><span data-stu-id="9326e-173">Modify your ASP.NET Core app to use the external Angular CLI instance instead of launching one of its own.</span></span> <span data-ttu-id="9326e-174">*スタートアップ*クラス、置換、`spa.UseAngularCliServer`を次の呼び出し。</span><span class="sxs-lookup"><span data-stu-id="9326e-174">In your *Startup* class, replace the `spa.UseAngularCliServer` invocation with the following:</span></span>

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:4200");
    ```

<span data-ttu-id="9326e-175">ASP.NET Core アプリを起動するときに Angular CLI サーバーを起動ことはありません。</span><span class="sxs-lookup"><span data-stu-id="9326e-175">When you start your ASP.NET Core app, it won't launch an Angular CLI server.</span></span> <span data-ttu-id="9326e-176">手動で開始したインスタンスが使用されます。</span><span class="sxs-lookup"><span data-stu-id="9326e-176">The instance you started manually is used instead.</span></span> <span data-ttu-id="9326e-177">これにより、起動し、再起動を高速にできます。</span><span class="sxs-lookup"><span data-stu-id="9326e-177">This enables it to start and restart faster.</span></span> <span data-ttu-id="9326e-178">Angular CLI たびに、クライアント アプリを再構築するを待機してが不要になった。</span><span class="sxs-lookup"><span data-stu-id="9326e-178">It's no longer waiting for Angular CLI to rebuild your client app each time.</span></span>

## <a name="server-side-rendering"></a><span data-ttu-id="9326e-179">サーバー側のレンダリング</span><span class="sxs-lookup"><span data-stu-id="9326e-179">Server-side rendering</span></span>

<span data-ttu-id="9326e-180">パフォーマンス機能としては、あらかじめサーバーだけでなく、クライアントで実行することで角運動のアプリを表示するために選択できます。</span><span class="sxs-lookup"><span data-stu-id="9326e-180">As a performance feature, you can choose to pre-render your Angular app on the server as well as running it on the client.</span></span> <span data-ttu-id="9326e-181">これは、ブラウザーがダウンロードして、JavaScript のバンドルを実行する前に表示するようにするために、アプリの初期の UI を表す HTML マークアップを受信することを意味します。</span><span class="sxs-lookup"><span data-stu-id="9326e-181">This means that browsers receive HTML markup representing your app's initial UI, so they display it even before downloading and executing your JavaScript bundles.</span></span> <span data-ttu-id="9326e-182">この実装の大部分と呼ばれる角度の機能から取得[Angular ユニバーサル](https://universal.angular.io/)です。</span><span class="sxs-lookup"><span data-stu-id="9326e-182">Most of the implementation of this comes from an Angular feature called [Angular Universal](https://universal.angular.io/).</span></span>

> [!TIP]
> <span data-ttu-id="9326e-183">サーバー側のレンダリングを有効にする (SSR) には、開発および展開時に余分な複雑さを両方の数が導入されています。</span><span class="sxs-lookup"><span data-stu-id="9326e-183">Enabling server-side rendering (SSR) introduces a number of extra complications both during development and deployment.</span></span> <span data-ttu-id="9326e-184">読み取り[SSR の欠点](#drawbacks-of-ssr)SSR が適合、要件を満たすかどうかを決定します。</span><span class="sxs-lookup"><span data-stu-id="9326e-184">Read [drawbacks of SSR](#drawbacks-of-ssr) to determine if SSR is a good fit for your requirements.</span></span>

<span data-ttu-id="9326e-185">SSR を有効にするには、多数のプロジェクトへの追加を行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="9326e-185">To enable SSR, you need to make a number of additions to your project.</span></span>

<span data-ttu-id="9326e-186">*スタートアップ*クラス、*後*を構成する行`spa.Options.SourcePath`、および*する前に*への呼び出し`UseAngularCliServer`または`UseProxyToSpaDevelopmentServer`次の追加。</span><span class="sxs-lookup"><span data-stu-id="9326e-186">In the *Startup* class, *after* the line that configures `spa.Options.SourcePath`, and *before* the call to `UseAngularCliServer` or `UseProxyToSpaDevelopmentServer`, add the following:</span></span>

[!code-csharp[](sample/AngularServerSideRendering/Startup.cs?name=snippet_Call_UseSpa&highlight=5-12)]

<span data-ttu-id="9326e-187">開発モードでこのコードがスクリプトを実行して、SSR バンドルが作成されます`build:ssr`で定義されている*ClientApp\package.json*です。</span><span class="sxs-lookup"><span data-stu-id="9326e-187">In development mode, this code attempts to build the SSR bundle by running the script `build:ssr`, which is defined in *ClientApp\package.json*.</span></span> <span data-ttu-id="9326e-188">これにより、ビルドという名前の角度アプリ`ssr`がまだ定義されていません。</span><span class="sxs-lookup"><span data-stu-id="9326e-188">This builds an Angular app named `ssr`, which isn't yet defined.</span></span> 

<span data-ttu-id="9326e-189">最後に、`apps`配列*ClientApp/.angular-cli.json*、名前の余分なアプリを定義する`ssr`です。</span><span class="sxs-lookup"><span data-stu-id="9326e-189">At the end of the `apps` array in *ClientApp/.angular-cli.json*, define an extra app with name `ssr`.</span></span> <span data-ttu-id="9326e-190">次のオプションを使用します。</span><span class="sxs-lookup"><span data-stu-id="9326e-190">Use the following options:</span></span>

[!code-json[](sample/AngularServerSideRendering/ClientApp/.angular-cli.json?range=24-41)]

<span data-ttu-id="9326e-191">この新しい SSR 対応アプリの構成には、さらに 2 つのファイルが必要です: *tsconfig.server.json*と*main.server.ts*です。</span><span class="sxs-lookup"><span data-stu-id="9326e-191">This new SSR-enabled app configuration requires two further files: *tsconfig.server.json* and *main.server.ts*.</span></span> <span data-ttu-id="9326e-192">*Tsconfig.server.json*ファイルは、TypeScript のコンパイル オプションを指定します。</span><span class="sxs-lookup"><span data-stu-id="9326e-192">The *tsconfig.server.json* file specifies TypeScript compilation options.</span></span> <span data-ttu-id="9326e-193">*Main.server.ts* SSR 中に、ファイルがコードのエントリ ポイントとして機能します。</span><span class="sxs-lookup"><span data-stu-id="9326e-193">The *main.server.ts* file serves as the code entry point during SSR.</span></span>

<span data-ttu-id="9326e-194">という名前の新しいファイルを追加*tsconfig.server.json*内*ClientApp/src* (既存と共に*tsconfig.app.json*)、次を含みます。</span><span class="sxs-lookup"><span data-stu-id="9326e-194">Add a new file called *tsconfig.server.json* inside *ClientApp/src* (alongside the existing *tsconfig.app.json*), containing the following:</span></span>

[!code-json[](sample/AngularServerSideRendering/ClientApp/src/tsconfig.server.json)]

<span data-ttu-id="9326e-195">このファイルの構成と呼ばれるモジュールを検索する角の AoT コンパイラ`app.server.module`です。</span><span class="sxs-lookup"><span data-stu-id="9326e-195">This file configures Angular's AoT compiler to look for a module called `app.server.module`.</span></span> <span data-ttu-id="9326e-196">これで新しいファイルを作成して追加*ClientApp/src/app/app.server.module.ts* (既存と共に*app.module.ts*)、次を含みます。</span><span class="sxs-lookup"><span data-stu-id="9326e-196">Add this by creating a new file at *ClientApp/src/app/app.server.module.ts* (alongside the existing *app.module.ts*) containing the following:</span></span> 

[!code-typescript[](sample/AngularServerSideRendering/ClientApp/src/app/app.server.module.ts)]

<span data-ttu-id="9326e-197">このモジュールは、クライアント側から継承`app.module`SSR 中に使用可能な余分な Angular モジュールを定義しています。</span><span class="sxs-lookup"><span data-stu-id="9326e-197">This module inherits from your client-side `app.module` and defines which extra Angular modules are available during SSR.</span></span>

<span data-ttu-id="9326e-198">注意してください、新しい`ssr`内のエントリ*.angular cli.json*というエントリ ポイント ファイルを参照*main.server.ts*です。</span><span class="sxs-lookup"><span data-stu-id="9326e-198">Recall that the new `ssr` entry in *.angular-cli.json* referenced an entry point file called *main.server.ts*.</span></span> <span data-ttu-id="9326e-199">そのファイルをまだ追加していないし、ここは、これを行うにします。</span><span class="sxs-lookup"><span data-stu-id="9326e-199">You haven't yet added that file, and now is time to do so.</span></span> <span data-ttu-id="9326e-200">新しいファイルを作成*ClientApp/src/main.server.ts* (既存と共に*main.ts*)、次を含みます。</span><span class="sxs-lookup"><span data-stu-id="9326e-200">Create a new file at *ClientApp/src/main.server.ts* (alongside the existing *main.ts*), containing the following:</span></span>

[!code-typescript[](sample/AngularServerSideRendering/ClientApp/src/main.server.ts)]

<span data-ttu-id="9326e-201">このファイルのコードはどのような ASP.NET Core 実行要求ごとに実行すると、`UseSpaPrerendering`に追加したミドルウェア、*スタートアップ*クラスです。</span><span class="sxs-lookup"><span data-stu-id="9326e-201">This file's code is what ASP.NET Core executes for each request when it runs the `UseSpaPrerendering` middleware that you added to the *Startup* class.</span></span> <span data-ttu-id="9326e-202">受信を扱う`params`(など、URL 要求されている)、.NET コードと結果の HTML を取得する角度 SSR Api を呼び出すからです。</span><span class="sxs-lookup"><span data-stu-id="9326e-202">It deals with receiving `params` from the .NET code (such as the URL being requested), and making calls to Angular SSR APIs to get the resulting HTML.</span></span> 

<span data-ttu-id="9326e-203">厳密に言うと、これは開発モードで SSR を有効にするだけで十分です。</span><span class="sxs-lookup"><span data-stu-id="9326e-203">Strictly-speaking, this is sufficient to enable SSR in development mode.</span></span> <span data-ttu-id="9326e-204">1 つの最終的な変更がようにするアプリが正常に発行する際に重要です。</span><span class="sxs-lookup"><span data-stu-id="9326e-204">It's essential to make one final change so that your app works correctly when published.</span></span> <span data-ttu-id="9326e-205">アプリのメインの*.csproj*ファイル、設定、`BuildServerSideRenderer`プロパティの値を`true`:</span><span class="sxs-lookup"><span data-stu-id="9326e-205">In your app's main *.csproj* file, set the `BuildServerSideRenderer` property value to `true`:</span></span>

[!code-xml[](sample/AngularServerSideRendering/AngularServerSideRendering.csproj?name=snippet_EnableBuildServerSideRenderer)]

<span data-ttu-id="9326e-206">これにより、構成、ビルド プロセスの実行を`build:ssr`発行時に SSR ファイルをサーバーに配置するとします。</span><span class="sxs-lookup"><span data-stu-id="9326e-206">This configures the build process to run `build:ssr` during publishing and deploy the SSR files to the server.</span></span> <span data-ttu-id="9326e-207">これを有効にしない場合は、実稼働環境で SSR が失敗します。</span><span class="sxs-lookup"><span data-stu-id="9326e-207">If you don't enable this, SSR fails in production.</span></span>

<span data-ttu-id="9326e-208">アプリを開発または実稼働モードで実行すると角運動のコードでは、事前としてレンダリング HTML サーバーにします。</span><span class="sxs-lookup"><span data-stu-id="9326e-208">When your app runs in either development or production mode, the Angular code pre-renders as HTML on the server.</span></span> <span data-ttu-id="9326e-209">クライアント側のコードは、通常どおり実行します。</span><span class="sxs-lookup"><span data-stu-id="9326e-209">The client-side code executes as normal.</span></span>

### <a name="pass-data-from-net-code-into-typescript-code"></a><span data-ttu-id="9326e-210">TypeScript コードに .NET コードからデータを渡す</span><span class="sxs-lookup"><span data-stu-id="9326e-210">Pass data from .NET code into TypeScript code</span></span>

<span data-ttu-id="9326e-211">SSR、中に、要求ごとのデータを ASP.NET Core アプリから Angular アプリに渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="9326e-211">During SSR, you might want to pass per-request data from your ASP.NET Core app into your Angular app.</span></span> <span data-ttu-id="9326e-212">たとえば、cookie 情報を渡すことがまたはデータベースから読み取られたものです。</span><span class="sxs-lookup"><span data-stu-id="9326e-212">For example, you could pass cookie information or something read from a database.</span></span> <span data-ttu-id="9326e-213">これを行うには、編集、*スタートアップ*クラスです。</span><span class="sxs-lookup"><span data-stu-id="9326e-213">To do this, edit your *Startup* class.</span></span> <span data-ttu-id="9326e-214">コールバックで`UseSpaPrerendering`の値を設定`options.SupplyData`次など。</span><span class="sxs-lookup"><span data-stu-id="9326e-214">In the callback for `UseSpaPrerendering`, set a value for `options.SupplyData` such as the following:</span></span>

```csharp
options.SupplyData = (context, data) =>
{
    // Creates a new value called isHttpsRequest that's passed to TypeScript code
    data["isHttpsRequest"] = context.Request.IsHttps;
};
```

<span data-ttu-id="9326e-215">`SupplyData`任意渡すコールバックにより、要求ごとの JSON でシリアル化データ (例、文字列、ブール値、または数値)。</span><span class="sxs-lookup"><span data-stu-id="9326e-215">The `SupplyData` callback lets you pass arbitrary, per-request, JSON-serializable data (for example, strings, booleans, or numbers).</span></span> <span data-ttu-id="9326e-216">*Main.server.ts*コードを受け取るとして`params.data`です。</span><span class="sxs-lookup"><span data-stu-id="9326e-216">Your *main.server.ts* code receives this as `params.data`.</span></span> <span data-ttu-id="9326e-217">たとえば、上記のコード サンプルにとしてブール値が渡されます`params.data.isHttpsRequest`に、`createServerRenderer`コールバック。</span><span class="sxs-lookup"><span data-stu-id="9326e-217">For example, the preceding code sample passes a boolean value as `params.data.isHttpsRequest` into the `createServerRenderer` callback.</span></span> <span data-ttu-id="9326e-218">これは、角でサポートされる任意の方法で、アプリの他の部分を渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="9326e-218">You can pass this to other parts of your app in any way supported by Angular.</span></span> <span data-ttu-id="9326e-219">たとえばを参照してください方法*main.server.ts*渡します、`BASE_URL`それを受信するコンス トラクターが宣言されたどのコンポーネントの値。</span><span class="sxs-lookup"><span data-stu-id="9326e-219">For example, see how *main.server.ts* passes the `BASE_URL` value to any component whose constructor is declared to receive it.</span></span>

### <a name="drawbacks-of-ssr"></a><span data-ttu-id="9326e-220">SSR の欠点</span><span class="sxs-lookup"><span data-stu-id="9326e-220">Drawbacks of SSR</span></span>

<span data-ttu-id="9326e-221">すべてのアプリ SSR を享受できます。</span><span class="sxs-lookup"><span data-stu-id="9326e-221">Not all apps benefit from SSR.</span></span> <span data-ttu-id="9326e-222">主な利点は、見かけ上のパフォーマンスです。</span><span class="sxs-lookup"><span data-stu-id="9326e-222">The primary benefit is perceived performance.</span></span> <span data-ttu-id="9326e-223">フェッチまたは JavaScript のバンドルを解析する、時間がかかる場合でも、訪問者が低速ネットワーク接続または低速のモバイル デバイスでアプリに到達するを迅速に、初期の UI 参照してください。</span><span class="sxs-lookup"><span data-stu-id="9326e-223">Visitors reaching your app over a slow network connection or on slow mobile devices see the initial UI quickly, even if it takes a while to fetch or parse the JavaScript bundles.</span></span> <span data-ttu-id="9326e-224">ただし、多くの SPAs は主に使用高速なコンピューターに高速、内部の会社のネットワーク経由でアプリがほぼ瞬時に表示します。</span><span class="sxs-lookup"><span data-stu-id="9326e-224">However, many SPAs are mainly used over fast, internal company networks on fast computers where the app appears almost instantly.</span></span>

<span data-ttu-id="9326e-225">同時には、SSR を有効にする重要な欠点があります。</span><span class="sxs-lookup"><span data-stu-id="9326e-225">At the same time, there are significant drawbacks to enabling SSR.</span></span> <span data-ttu-id="9326e-226">複雑さを開発プロセスに追加します。</span><span class="sxs-lookup"><span data-stu-id="9326e-226">It adds complexity to your development process.</span></span> <span data-ttu-id="9326e-227">2 つの環境でコードを実行する必要があります。 クライアント側およびサーバー側 (Node.js 環境で ASP.NET Core から呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="9326e-227">Your code must run in two different environments: client-side and server-side (in a Node.js environment invoked from ASP.NET Core).</span></span> <span data-ttu-id="9326e-228">念頭にいくつかの点を次に示します。</span><span class="sxs-lookup"><span data-stu-id="9326e-228">Here are some things to bear in mind:</span></span>

* <span data-ttu-id="9326e-229">SSR には、実稼働サーバー上の Node.js のインストールが必要です。</span><span class="sxs-lookup"><span data-stu-id="9326e-229">SSR requires a Node.js installation on your production servers.</span></span> <span data-ttu-id="9326e-230">これは、Azure App Services などの一部の展開シナリオについては、Azure Service Fabric など、他のケースでは自動的にです。</span><span class="sxs-lookup"><span data-stu-id="9326e-230">This is automatically the case for some deployment scenarios, such as Azure App Services, but not for others, such as Azure Service Fabric.</span></span>
* <span data-ttu-id="9326e-231">有効にすると、`BuildServerSideRenderer`フラグの原因をビルド、 *node_modules*を公開するディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="9326e-231">Enabling the `BuildServerSideRenderer` build flag causes your *node_modules* directory to publish.</span></span> <span data-ttu-id="9326e-232">このフォルダーには、20,000 + ファイル、配置時間が増加するが含まれています。</span><span class="sxs-lookup"><span data-stu-id="9326e-232">This folder contains 20,000+ files, which increases deployment time.</span></span>
* <span data-ttu-id="9326e-233">Node.js 環境で、コードを実行することに依存できないブラウザー固有の JavaScript Api の存在など`window`または`localStorage`です。</span><span class="sxs-lookup"><span data-stu-id="9326e-233">To run your code in a Node.js environment, it can't rely on the existence of browser-specific JavaScript APIs such as `window` or `localStorage`.</span></span> <span data-ttu-id="9326e-234">コード (または参照するいくつかのサード パーティ製ライブラリ) は、これらの Api を使用すると、SSR 中にエラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="9326e-234">If your code (or some third-party library you reference) tries to use these APIs, you'll get an error during SSR.</span></span> <span data-ttu-id="9326e-235">多くの場所でブラウザー固有の Api を参照しているために、jQuery がなど、使用しないでください。</span><span class="sxs-lookup"><span data-stu-id="9326e-235">For example, don't use jQuery because it references browser-specific APIs in many places.</span></span> <span data-ttu-id="9326e-236">エラーを防ぐためには、SSR を回避するか、または、ブラウザーに固有の Api またはライブラリを回避する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9326e-236">To prevent errors, you must either avoid SSR or avoid browser-specific APIs or libraries.</span></span> <span data-ttu-id="9326e-237">SSR 中に呼び出すことがないことを確認するためのチェックでこのような Api への呼び出しをラップすることができます。</span><span class="sxs-lookup"><span data-stu-id="9326e-237">You can wrap any calls to such APIs in checks to ensure they aren't invoked during SSR.</span></span> <span data-ttu-id="9326e-238">たとえば、JavaScript または TypeScript コードで、次のようなチェックを使用します。</span><span class="sxs-lookup"><span data-stu-id="9326e-238">For example, use a check such as the following in JavaScript or TypeScript code:</span></span>

    ```javascript
    if (typeof window !== 'undefined') {
        // Call browser-specific APIs here
    }
    ```
