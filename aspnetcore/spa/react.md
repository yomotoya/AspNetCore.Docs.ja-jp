---
title: "対応プロジェクト テンプレートを使用します。"
author: SteveSandersonMS
description: "ASP.NET Core 単一ページ アプリケーション (SPA) リリース候補プロジェクト テンプレートを使用して対応と作成対応アプリの作業を開始する方法を説明します。"
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 12/06/2017
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/react
ms.openlocfilehash: 5978094083a098a771f5dca103434ea8fcce7777
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
# <a name="use-the-react-project-template-release-candidate"></a><span data-ttu-id="ca786-103">対応プロジェクト テンプレート (リリース候補) を使用します。</span><span class="sxs-lookup"><span data-stu-id="ca786-103">Use the React project template (release candidate)</span></span>

> [!NOTE]
> <span data-ttu-id="ca786-104">このドキュメントは、リリースの反応するプロジェクト テンプレートではありません。</span><span class="sxs-lookup"><span data-stu-id="ca786-104">This documentation isn't about the released React project template.</span></span> <span data-ttu-id="ca786-105">**このドキュメントでは、対応のテンプレートのリリース候補に関するです。**</span><span class="sxs-lookup"><span data-stu-id="ca786-105">**This documentation is about the release candidate of the React template.**</span></span> <span data-ttu-id="ca786-106">初期 2018 でリリースされたバージョンを出荷する予定です。</span><span class="sxs-lookup"><span data-stu-id="ca786-106">We hope to ship the released version in early 2018.</span></span>

<span data-ttu-id="ca786-107">更新された対応プロジェクト テンプレートでは便利な開始点 ASP.NET Core の反応を使用してアプリと[作成対応アプリ](https://github.com/facebookincubator/create-react-app)豊富なクライアント側のユーザー インターフェイス (UI) を実装する (CRA) 規則。</span><span class="sxs-lookup"><span data-stu-id="ca786-107">The updated React project template provides a convenient starting point for ASP.NET Core apps using React and [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) conventions to implement a rich, client-side user interface (UI).</span></span>

<span data-ttu-id="ca786-108">テンプレートは、利便性の両方で、ビルドと 1 つの単位として公開された 1 つのアプリ プロジェクトをホストしているのですが、UI として機能する標準的な CRA 対応プロジェクトと、API のバックエンドとして機能する、ASP.NET Core プロジェクトの両方を作成できます。</span><span class="sxs-lookup"><span data-stu-id="ca786-108">The template is equivalent to creating both an ASP.NET Core project to act as an API backend, and a standard CRA React project to act as a UI, but with the convenience of hosting both in a single app project that can be built and published as a single unit.</span></span>

## <a name="create-a-new-app"></a><span data-ttu-id="ca786-109">新しいアプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="ca786-109">Create a new app</span></span>

<span data-ttu-id="ca786-110">確認した結果を作業を開始する[更新の対応プロジェクト テンプレートがインストールされている](xref:spa/index#installation)です。</span><span class="sxs-lookup"><span data-stu-id="ca786-110">To get started, ensure you've [installed the updated React project template](xref:spa/index#installation).</span></span> <span data-ttu-id="ca786-111">これらの手順は、.NET Core で含まれている以前の対応プロジェクト テンプレートを適用しない 2.0.x SDK。</span><span class="sxs-lookup"><span data-stu-id="ca786-111">These instructions don't apply to the previous React project template included in the .NET Core 2.0.x SDK.</span></span>

<span data-ttu-id="ca786-112">コマンドを使用してコマンド プロンプトから、新しいプロジェクトを作成する`dotnet new react`空のディレクトリにします。</span><span class="sxs-lookup"><span data-stu-id="ca786-112">Create a new project from a command prompt using the command `dotnet new react` in an empty directory.</span></span> <span data-ttu-id="ca786-113">次のコマンドがでアプリを作成するなど、 *my-新しい-アプリ*ディレクトリとそのディレクトリへの切り替え。</span><span class="sxs-lookup"><span data-stu-id="ca786-113">For example, the following commands create the app in a *my-new-app* directory and switch to that directory:</span></span>

```console
dotnet new react -o my-new-app
cd my-new-app
```

<span data-ttu-id="ca786-114">Visual Studio または .NET Core CLI からアプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="ca786-114">Run the app from either Visual Studio or the .NET Core CLI:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ca786-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ca786-115">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="ca786-116">開く、生成された*.csproj*ファイル、およびそこから通常の方法でアプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="ca786-116">Open the generated *.csproj* file, and run the app as normal from there.</span></span>

<span data-ttu-id="ca786-117">ビルド プロセスでは、数分かかることが、最初の実行で npm の依存関係を復元します。</span><span class="sxs-lookup"><span data-stu-id="ca786-117">The build process restores npm dependencies on the first run, which can take several minutes.</span></span> <span data-ttu-id="ca786-118">後続のビルドは非常に高速です。</span><span class="sxs-lookup"><span data-stu-id="ca786-118">Subsequent builds are much faster.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="ca786-119">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="ca786-119">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="ca786-120">という環境変数があることを確認`ASPNETCORE_Environment`の値を持つ`Development`します。</span><span class="sxs-lookup"><span data-stu-id="ca786-120">Ensure you have an environment variable called `ASPNETCORE_Environment` with value of `Development`.</span></span> <span data-ttu-id="ca786-121">Windows PowerShell 以外の入力を要求) の「、で実行される`SET ASPNETCORE_Environment=Development`です。</span><span class="sxs-lookup"><span data-stu-id="ca786-121">On Windows (in non-PowerShell prompts), run `SET ASPNETCORE_Environment=Development`.</span></span> <span data-ttu-id="ca786-122">Linux または macOS、実行`export ASPNETCORE_Environment=Development`です。</span><span class="sxs-lookup"><span data-stu-id="ca786-122">On Linux or macOS, run `export ASPNETCORE_Environment=Development`.</span></span>

<span data-ttu-id="ca786-123">実行`dotnet build`正しくビルドするのには、アプリを確認してください。</span><span class="sxs-lookup"><span data-stu-id="ca786-123">Run `dotnet build` to verify your app builds correctly.</span></span> <span data-ttu-id="ca786-124">最初の実行には、ビルド プロセスは、数分かかることが npm の依存関係を復元します。</span><span class="sxs-lookup"><span data-stu-id="ca786-124">On the first run, the build process restores npm dependencies, which can take several minutes.</span></span> <span data-ttu-id="ca786-125">後続のビルドは非常に高速です。</span><span class="sxs-lookup"><span data-stu-id="ca786-125">Subsequent builds are much faster.</span></span>

<span data-ttu-id="ca786-126">実行`dotnet run`でアプリを起動します。</span><span class="sxs-lookup"><span data-stu-id="ca786-126">Run `dotnet run` to start the app.</span></span>

---

<span data-ttu-id="ca786-127">プロジェクト テンプレートは、ASP.NET Core アプリおよび対応アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="ca786-127">The project template creates an ASP.NET Core app and a React app.</span></span> <span data-ttu-id="ca786-128">ASP.NET Core アプリケーションがデータ アクセス、承認、およびその他のサーバー側の懸念事項に使用するためのものです。</span><span class="sxs-lookup"><span data-stu-id="ca786-128">The ASP.NET Core app is intended to be used for data access, authorization, and other server-side concerns.</span></span> <span data-ttu-id="ca786-129">内に存在する、対応アプリ、 *ClientApp*サブディレクトリがすべての UI に関する注意事項を使用するためです。</span><span class="sxs-lookup"><span data-stu-id="ca786-129">The React app, residing in the *ClientApp* subdirectory, is intended to be used for all UI concerns.</span></span>

## <a name="add-pages-images-styles-modules-etc"></a><span data-ttu-id="ca786-130">ページ、画像、スタイル、モジュールなどを追加します。</span><span class="sxs-lookup"><span data-stu-id="ca786-130">Add pages, images, styles, modules, etc.</span></span>

<span data-ttu-id="ca786-131">*ClientApp*ディレクトリは、標準の CRA 対応アプリ。</span><span class="sxs-lookup"><span data-stu-id="ca786-131">The *ClientApp* directory is a standard CRA React app.</span></span> <span data-ttu-id="ca786-132">公式を参照してください[CRA ドキュメント](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="ca786-132">See the official [CRA documentation](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md) for more information.</span></span>

<span data-ttu-id="ca786-133">このテンプレートで作成した対応アプリケーションと CRA 自体によって作成されたものの間のわずかな違いがあります。ただし、アプリの機能は変更されません。</span><span class="sxs-lookup"><span data-stu-id="ca786-133">There are slight differences between the React app created by this template and the one created by CRA itself; however, the app's capabilities are unchanged.</span></span> <span data-ttu-id="ca786-134">テンプレートによって作成されたアプリが含まれています、[ブートス トラップ](https://getbootstrap.com/)-ベースのレイアウトとルーティングの基本的な例です。</span><span class="sxs-lookup"><span data-stu-id="ca786-134">The app created by the template contains a [Bootstrap](https://getbootstrap.com/)-based layout and a basic routing example.</span></span>

## <a name="install-npm-packages"></a><span data-ttu-id="ca786-135">Npm パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="ca786-135">Install npm packages</span></span>

<span data-ttu-id="ca786-136">サード パーティ製の npm パッケージをインストールするでコマンド プロンプトを使用して、 *ClientApp*サブディレクトリです。</span><span class="sxs-lookup"><span data-stu-id="ca786-136">To install third-party npm packages, use a command prompt in the *ClientApp* subdirectory.</span></span> <span data-ttu-id="ca786-137">例:</span><span class="sxs-lookup"><span data-stu-id="ca786-137">For example:</span></span>

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a><span data-ttu-id="ca786-138">発行と配置</span><span class="sxs-lookup"><span data-stu-id="ca786-138">Publish and deploy</span></span>

<span data-ttu-id="ca786-139">開発中のアプリは、開発者の利便性の最適化モードで実行されます。</span><span class="sxs-lookup"><span data-stu-id="ca786-139">In development, the app runs in a mode optimized for developer convenience.</span></span> <span data-ttu-id="ca786-140">たとえば、JavaScript のバンドルは、(をデバッグする場合は、元のソース コードを確認できます) をソース マップを含めます。</span><span class="sxs-lookup"><span data-stu-id="ca786-140">For example, JavaScript bundles include source maps (so that when debugging, you can see your original source code).</span></span> <span data-ttu-id="ca786-141">アプリは JavaScript、HTML、および CSS のディスクのファイルの変更を監視し、自動的にコンパイルされそれらのファイルの変更が発生したときに再読み込みされます。</span><span class="sxs-lookup"><span data-stu-id="ca786-141">The app watches JavaScript, HTML, and CSS file changes on disk and automatically recompiles and reloads when it sees those files change.</span></span>

<span data-ttu-id="ca786-142">実稼働環境でパフォーマンスが最適化された、アプリのバージョンを提供します。</span><span class="sxs-lookup"><span data-stu-id="ca786-142">In production, serve a version of your app that's optimized for performance.</span></span> <span data-ttu-id="ca786-143">これは、自動的に実行される構成されます。</span><span class="sxs-lookup"><span data-stu-id="ca786-143">This is configured to happen automatically.</span></span> <span data-ttu-id="ca786-144">パブリッシュすると、ビルド構成は、クライアント側コードの縮小された、transpiled ビルドを出力します。</span><span class="sxs-lookup"><span data-stu-id="ca786-144">When you publish, the build configuration emits a minified, transpiled build of your client-side code.</span></span> <span data-ttu-id="ca786-145">開発ビルドとは異なり、運用ビルドは、サーバーにインストールされている Node.js を必要としません。</span><span class="sxs-lookup"><span data-stu-id="ca786-145">Unlike the development build, the production build doesn't require Node.js to be installed on the server.</span></span>

<span data-ttu-id="ca786-146">標準を使用する[ASP.NET Core のホストと展開方法](xref:host-and-deploy/index)です。</span><span class="sxs-lookup"><span data-stu-id="ca786-146">You can use standard [ASP.NET Core hosting and deployment methods](xref:host-and-deploy/index).</span></span>

## <a name="run-the-cra-server-independently"></a><span data-ttu-id="ca786-147">CRA サーバーを個別に実行します。</span><span class="sxs-lookup"><span data-stu-id="ca786-147">Run the CRA server independently</span></span>

<span data-ttu-id="ca786-148">プロジェクトが ASP.NET Core アプリケーションの開発モードの開始時に、バック グラウンドで CRA 開発サーバーの独自のインスタンスを開始するように構成します。</span><span class="sxs-lookup"><span data-stu-id="ca786-148">The project is configured to start its own instance of the CRA development server in the background when the ASP.NET Core app starts in development mode.</span></span> <span data-ttu-id="ca786-149">これは、機能は、別のサーバーを手動で実行する必要はありませんので便利です。</span><span class="sxs-lookup"><span data-stu-id="ca786-149">This is convenient because it means you don't have to run a separate server manually.</span></span>

<span data-ttu-id="ca786-150">この既定の設定の欠点があります。</span><span class="sxs-lookup"><span data-stu-id="ca786-150">There's a drawback to this default setup.</span></span> <span data-ttu-id="ca786-151">C# コードとアプリを再起動する必要があります、ASP.NET Core を変更するたびに、CRA サーバーを再起動します。</span><span class="sxs-lookup"><span data-stu-id="ca786-151">Each time you modify your C# code and your ASP.NET Core app needs to restart, the CRA server restarts.</span></span> <span data-ttu-id="ca786-152">バックアップを開始するには、数秒が必要です。</span><span class="sxs-lookup"><span data-stu-id="ca786-152">A few seconds are required to start back up.</span></span> <span data-ttu-id="ca786-153">頻繁に c# コードの編集を加えようとして CRA サーバーを再起動するまで待機しない場合は、ASP.NET Core プロセスとは無関係に外部から、CRA サーバーを実行します。</span><span class="sxs-lookup"><span data-stu-id="ca786-153">If you're making frequent C# code edits and don't want to wait for the CRA server to restart, run the CRA server externally, independently of the ASP.NET Core process.</span></span> <span data-ttu-id="ca786-154">これを行うには。</span><span class="sxs-lookup"><span data-stu-id="ca786-154">To do so:</span></span>

1. <span data-ttu-id="ca786-155">コマンド プロンプトでに切り替えると、 *ClientApp*サブディレクトリ、および CRA 開発サーバーの起動。</span><span class="sxs-lookup"><span data-stu-id="ca786-155">In a command prompt, switch to the *ClientApp* subdirectory, and launch the CRA development server:</span></span>

    ```console
    cd ClientApp
    npm start
    ```

2. <span data-ttu-id="ca786-156">独自のいずれかを起動せずに、外部の CRA サーバー インスタンスを使用する ASP.NET Core アプリケーションを変更します。</span><span class="sxs-lookup"><span data-stu-id="ca786-156">Modify your ASP.NET Core app to use the external CRA server instance instead of launching one of its own.</span></span> <span data-ttu-id="ca786-157">*スタートアップ*クラス、置換、`spa.UseReactDevelopmentServer`を次の呼び出し。</span><span class="sxs-lookup"><span data-stu-id="ca786-157">In your *Startup* class, replace the `spa.UseReactDevelopmentServer` invocation with the following:</span></span>

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:3000");
    ```

<span data-ttu-id="ca786-158">ASP.NET Core アプリを起動するときに CRA サーバーの起動時にはありません。</span><span class="sxs-lookup"><span data-stu-id="ca786-158">When you start your ASP.NET Core app, it won't launch a CRA server.</span></span> <span data-ttu-id="ca786-159">手動で開始したインスタンスが使用されます。</span><span class="sxs-lookup"><span data-stu-id="ca786-159">The instance you started manually is used instead.</span></span> <span data-ttu-id="ca786-160">これにより、起動し、再起動を高速にできます。</span><span class="sxs-lookup"><span data-stu-id="ca786-160">This enables it to start and restart faster.</span></span> <span data-ttu-id="ca786-161">不要になった、たびに再構築する対応アプリケーションが待機しています。</span><span class="sxs-lookup"><span data-stu-id="ca786-161">It's no longer waiting for your React app to rebuild each time.</span></span>
