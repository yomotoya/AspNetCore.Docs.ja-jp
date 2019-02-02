---
title: ASP.NET Core で React プロジェクト テンプレートを使用する
author: SteveSandersonMS
description: React と create-react-app 用の ASP.NET Core シングル ページ アプリケーション (SPA) プロジェクト テンプレートの使用を開始する方法について説明します。
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
uid: spa/react
ms.openlocfilehash: d83bff8abcd5b59d8bc4a51a101510755394f0c4
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667688"
---
# <a name="use-the-react-project-template-with-aspnet-core"></a><span data-ttu-id="dc470-103">ASP.NET Core で React プロジェクト テンプレートを使用する</span><span class="sxs-lookup"><span data-stu-id="dc470-103">Use the React project template with ASP.NET Core</span></span>

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> <span data-ttu-id="dc470-104">このドキュメントは、ASP.NET Core 2.0 に含まれている React プロジェクト テンプレートについて説明するものではありません。</span><span class="sxs-lookup"><span data-stu-id="dc470-104">This documentation isn't about the React project template included in ASP.NET Core 2.0.</span></span> <span data-ttu-id="dc470-105">手動で更新できる新しい React テンプレートについて説明するものです。</span><span class="sxs-lookup"><span data-stu-id="dc470-105">It's about the newer React template to which you can update manually.</span></span> <span data-ttu-id="dc470-106">このテンプレートは、ASP.NET Core 2.1 に既定で含まれています。</span><span class="sxs-lookup"><span data-stu-id="dc470-106">The template is included in ASP.NET Core 2.1 by default.</span></span>

::: moniker-end

<span data-ttu-id="dc470-107">更新された React プロジェクト テンプレートは、React および [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) 規則を使用してリッチなクライアント側ユーザー インターフェイス (UI) を実装する ASP.NET Core アプリを開発する場合に、便利な開始点として利用できます。</span><span class="sxs-lookup"><span data-stu-id="dc470-107">The updated React project template provides a convenient starting point for ASP.NET Core apps using React and [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) conventions to implement a rich, client-side user interface (UI).</span></span>

<span data-ttu-id="dc470-108">このテンプレートには、API バックエンドとして機能する ASP.NET Core プロジェクトと、UI として機能する標準の CRA React プロジェクトの両方を作成する場合と同等の機能がありますが、単一のユニットとしてビルドと発行が可能な単一のアプリ プロジェクトで両方をホストできるので便利です。</span><span class="sxs-lookup"><span data-stu-id="dc470-108">The template is equivalent to creating both an ASP.NET Core project to act as an API backend, and a standard CRA React project to act as a UI, but with the convenience of hosting both in a single app project that can be built and published as a single unit.</span></span>

## <a name="create-a-new-app"></a><span data-ttu-id="dc470-109">新しいアプリを作成する</span><span class="sxs-lookup"><span data-stu-id="dc470-109">Create a new app</span></span>

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="dc470-110">ASP.NET Core 2.0 を使用している場合は、[更新された React プロジェクト テンプレートをインストールしている](xref:spa/index#installation)ことを確認します。</span><span class="sxs-lookup"><span data-stu-id="dc470-110">If using ASP.NET Core 2.0, ensure you've [installed the updated React project template](xref:spa/index#installation).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="dc470-111">ASP.NET Core 2.1 がインストールされている場合は、React プロジェクト テンプレートをインストールする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="dc470-111">If you have ASP.NET Core 2.1 installed, there's no need to install the React project template.</span></span>

::: moniker-end

<span data-ttu-id="dc470-112">コマンド プロンプトで `dotnet new react` コマンドを使用して、空のディレクトリの中に新しいプロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="dc470-112">Create a new project from a command prompt using the command `dotnet new react` in an empty directory.</span></span> <span data-ttu-id="dc470-113">たとえば、次のコマンドは、*my-new-app* ディレクトリにアプリを作成し、そのディレクトリに切り替えます。</span><span class="sxs-lookup"><span data-stu-id="dc470-113">For example, the following commands create the app in a *my-new-app* directory and switch to that directory:</span></span>

```console
dotnet new react -o my-new-app
cd my-new-app
```

<span data-ttu-id="dc470-114">Visual Studio または .NET Core CLI からアプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="dc470-114">Run the app from either Visual Studio or the .NET Core CLI:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dc470-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dc470-115">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="dc470-116">生成された *.csproj* ファイルを開き、そこから通常の方法でアプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="dc470-116">Open the generated *.csproj* file, and run the app as normal from there.</span></span>

<span data-ttu-id="dc470-117">ビルド プロセスは、初回の実行で npm の依存関係を復元します。これには数分かかる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="dc470-117">The build process restores npm dependencies on the first run, which can take several minutes.</span></span> <span data-ttu-id="dc470-118">以降のビルドは非常に高速になります。</span><span class="sxs-lookup"><span data-stu-id="dc470-118">Subsequent builds are much faster.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="dc470-119">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="dc470-119">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="dc470-120">値が `Development` である `ASPNETCORE_Environment` という名前の環境変数があることを確認します。</span><span class="sxs-lookup"><span data-stu-id="dc470-120">Ensure you have an environment variable called `ASPNETCORE_Environment` with value of `Development`.</span></span> <span data-ttu-id="dc470-121">Windows では (PowerShell ではないプロンプトで) `SET ASPNETCORE_Environment=Development` を実行します。</span><span class="sxs-lookup"><span data-stu-id="dc470-121">On Windows (in non-PowerShell prompts), run `SET ASPNETCORE_Environment=Development`.</span></span> <span data-ttu-id="dc470-122">Linux または macOS では、`export ASPNETCORE_Environment=Development` を実行します。</span><span class="sxs-lookup"><span data-stu-id="dc470-122">On Linux or macOS, run `export ASPNETCORE_Environment=Development`.</span></span>

<span data-ttu-id="dc470-123">[dotnet build](/dotnet/core/tools/dotnet-build) を実行して、アプリが正しくビルドされていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="dc470-123">Run [dotnet build](/dotnet/core/tools/dotnet-build) to verify your app builds correctly.</span></span> <span data-ttu-id="dc470-124">ビルド プロセスは、初回の実行で npm の依存関係を復元します。これには数分かかる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="dc470-124">On the first run, the build process restores npm dependencies, which can take several minutes.</span></span> <span data-ttu-id="dc470-125">以降のビルドは非常に高速になります。</span><span class="sxs-lookup"><span data-stu-id="dc470-125">Subsequent builds are much faster.</span></span>

<span data-ttu-id="dc470-126">[dotnet run](/dotnet/core/tools/dotnet-run) を実行してアプリを起動します。</span><span class="sxs-lookup"><span data-stu-id="dc470-126">Run [dotnet run](/dotnet/core/tools/dotnet-run) to start the app.</span></span>

---

<span data-ttu-id="dc470-127">プロジェクト テンプレートは、ASP.NET Core アプリと React アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="dc470-127">The project template creates an ASP.NET Core app and a React app.</span></span> <span data-ttu-id="dc470-128">ASP.NET Core アプリは、データ アクセス、承認、およびその他のサーバー側の操作で使用することを目的としています。</span><span class="sxs-lookup"><span data-stu-id="dc470-128">The ASP.NET Core app is intended to be used for data access, authorization, and other server-side concerns.</span></span> <span data-ttu-id="dc470-129">*ClientApp* サブディレクトリ内の React アプリは、UI に関するすべての 操作で使用することを目的としています。</span><span class="sxs-lookup"><span data-stu-id="dc470-129">The React app, residing in the *ClientApp* subdirectory, is intended to be used for all UI concerns.</span></span>

## <a name="add-pages-images-styles-modules-etc"></a><span data-ttu-id="dc470-130">ページ、画像、スタイル、モジュールなどを追加する</span><span class="sxs-lookup"><span data-stu-id="dc470-130">Add pages, images, styles, modules, etc.</span></span>

<span data-ttu-id="dc470-131">*ClientApp* ディレクトリは標準の CRA React アプリです。</span><span class="sxs-lookup"><span data-stu-id="dc470-131">The *ClientApp* directory is a standard CRA React app.</span></span> <span data-ttu-id="dc470-132">詳細については、公式の [CRA ドキュメント](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="dc470-132">See the official [CRA documentation](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md) for more information.</span></span>

<span data-ttu-id="dc470-133">このテンプレートによって作成される React アプリと CRA 自体によって作成されるアプリにはわずかな違いがありますが、アプリの機能は変わりません。</span><span class="sxs-lookup"><span data-stu-id="dc470-133">There are slight differences between the React app created by this template and the one created by CRA itself; however, the app's capabilities are unchanged.</span></span> <span data-ttu-id="dc470-134">テンプレートによって作成されるアプリには、[ブートストラップ](https://getbootstrap.com/)ベースのレイアウトと基本的なルーティングの例が含まれます。</span><span class="sxs-lookup"><span data-stu-id="dc470-134">The app created by the template contains a [Bootstrap](https://getbootstrap.com/)-based layout and a basic routing example.</span></span>

## <a name="install-npm-packages"></a><span data-ttu-id="dc470-135">npm パッケージをインストールする</span><span class="sxs-lookup"><span data-stu-id="dc470-135">Install npm packages</span></span>

<span data-ttu-id="dc470-136">サードパーティ製の npm パッケージをインストールするには、*ClientApp* サブディレクトリでコマンド プロンプトを使用します。</span><span class="sxs-lookup"><span data-stu-id="dc470-136">To install third-party npm packages, use a command prompt in the *ClientApp* subdirectory.</span></span> <span data-ttu-id="dc470-137">例:</span><span class="sxs-lookup"><span data-stu-id="dc470-137">For example:</span></span>

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a><span data-ttu-id="dc470-138">発行と配置</span><span class="sxs-lookup"><span data-stu-id="dc470-138">Publish and deploy</span></span>

<span data-ttu-id="dc470-139">開発中、アプリは、開発者に便利なように最適化されたモードで実行されます。</span><span class="sxs-lookup"><span data-stu-id="dc470-139">In development, the app runs in a mode optimized for developer convenience.</span></span> <span data-ttu-id="dc470-140">たとえば、JavaScript バンドルには、ソース マップが含まれます (デバッグ時に元のソース コードを確認できるようにするためです)。</span><span class="sxs-lookup"><span data-stu-id="dc470-140">For example, JavaScript bundles include source maps (so that when debugging, you can see your original source code).</span></span> <span data-ttu-id="dc470-141">アプリは、ディスク上の JavaScript、HTML および CSS ファイルの変更を監視し、これらのファイルの変更が発生した場合は、再コンパイルと再読み込みを自動的に実行します。</span><span class="sxs-lookup"><span data-stu-id="dc470-141">The app watches JavaScript, HTML, and CSS file changes on disk and automatically recompiles and reloads when it sees those files change.</span></span>

<span data-ttu-id="dc470-142">運用時は、パフォーマンスが最適化されたバージョンのアプリが提供されます。</span><span class="sxs-lookup"><span data-stu-id="dc470-142">In production, serve a version of your app that's optimized for performance.</span></span> <span data-ttu-id="dc470-143">これは、自動的に実行されるように構成されています。</span><span class="sxs-lookup"><span data-stu-id="dc470-143">This is configured to happen automatically.</span></span> <span data-ttu-id="dc470-144">発行すると、ビルド構成によって、クライアント側コードの縮小され、トランスパイルされたビルドが生成されます。</span><span class="sxs-lookup"><span data-stu-id="dc470-144">When you publish, the build configuration emits a minified, transpiled build of your client-side code.</span></span> <span data-ttu-id="dc470-145">開発ビルドとは異なり、運用ビルドは、サーバーへの Node.js のインストールを必要としません。</span><span class="sxs-lookup"><span data-stu-id="dc470-145">Unlike the development build, the production build doesn't require Node.js to be installed on the server.</span></span>

<span data-ttu-id="dc470-146">標準的な [ASP.NET Core のホストと展開方法](xref:host-and-deploy/index)を使用できます。</span><span class="sxs-lookup"><span data-stu-id="dc470-146">You can use standard [ASP.NET Core hosting and deployment methods](xref:host-and-deploy/index).</span></span>

## <a name="run-the-cra-server-independently"></a><span data-ttu-id="dc470-147">CRA サーバーを個別に実行する</span><span class="sxs-lookup"><span data-stu-id="dc470-147">Run the CRA server independently</span></span>

<span data-ttu-id="dc470-148">ASP.NET Core アプリが開発モードで起動された場合、プロジェクトは、CRA 開発サーバーの独自のインスタンスをバックグラウンドで開始するように構成されます。</span><span class="sxs-lookup"><span data-stu-id="dc470-148">The project is configured to start its own instance of the CRA development server in the background when the ASP.NET Core app starts in development mode.</span></span> <span data-ttu-id="dc470-149">これが便利なのは、別のサーバーを手動で実行する必要がないためです。</span><span class="sxs-lookup"><span data-stu-id="dc470-149">This is convenient because it means you don't have to run a separate server manually.</span></span>

<span data-ttu-id="dc470-150">この既定の設定には欠点があります。</span><span class="sxs-lookup"><span data-stu-id="dc470-150">There's a drawback to this default setup.</span></span> <span data-ttu-id="dc470-151">C# コードを変更し、ASP.NET Core アプリを再起動する必要がある場合、CRA サーバーが毎回再起動します。</span><span class="sxs-lookup"><span data-stu-id="dc470-151">Each time you modify your C# code and your ASP.NET Core app needs to restart, the CRA server restarts.</span></span> <span data-ttu-id="dc470-152">再起動には数秒かかります。</span><span class="sxs-lookup"><span data-stu-id="dc470-152">A few seconds are required to start back up.</span></span> <span data-ttu-id="dc470-153">C# コードを何度も編集するが、CRA サーバーが再起動するまで待ちたくない場合は、ASP.NET Core プロセスから独立した CRA サーバーを外部で実行します。</span><span class="sxs-lookup"><span data-stu-id="dc470-153">If you're making frequent C# code edits and don't want to wait for the CRA server to restart, run the CRA server externally, independently of the ASP.NET Core process.</span></span> <span data-ttu-id="dc470-154">次の手順に従います。</span><span class="sxs-lookup"><span data-stu-id="dc470-154">To do so:</span></span>

1. <span data-ttu-id="dc470-155">追加、 *.env*ファイルを*ClientApp*次の設定を持つサブディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="dc470-155">Add a *.env* file to the *ClientApp* subdirectory with the following setting:</span></span>

    ```
    BROWSER=none
    ```
    
    <span data-ttu-id="dc470-156">これにより、web ブラウザーを開く外部から CRA サーバーを開始するときにします。</span><span class="sxs-lookup"><span data-stu-id="dc470-156">This will prevent your web browser from opening when starting the CRA server externally.</span></span>

2. <span data-ttu-id="dc470-157">コマンド プロンプトで、*ClientApp* サブディレクトリに切り替え、CRA 開発サーバーを起動します。</span><span class="sxs-lookup"><span data-stu-id="dc470-157">In a command prompt, switch to the *ClientApp* subdirectory, and launch the CRA development server:</span></span>

    ```console
    cd ClientApp
    npm start
    ```

3. <span data-ttu-id="dc470-158">独自のインスタンスを起動する代わりに外部の CRA サーバー インスタンスを使用するように ASP.NET Core アプリケーションを変更します。</span><span class="sxs-lookup"><span data-stu-id="dc470-158">Modify your ASP.NET Core app to use the external CRA server instance instead of launching one of its own.</span></span> <span data-ttu-id="dc470-159">*Startup* クラスで、`spa.UseReactDevelopmentServer` の呼び出しを以下に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="dc470-159">In your *Startup* class, replace the `spa.UseReactDevelopmentServer` invocation with the following:</span></span>

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:3000");
    ```

<span data-ttu-id="dc470-160">ASP.NET Core アプリの起動時に CRA サーバーが起動されなくなります。</span><span class="sxs-lookup"><span data-stu-id="dc470-160">When you start your ASP.NET Core app, it won't launch a CRA server.</span></span> <span data-ttu-id="dc470-161">代わりに、手動で開始したインスタンスが使用されます。</span><span class="sxs-lookup"><span data-stu-id="dc470-161">The instance you started manually is used instead.</span></span> <span data-ttu-id="dc470-162">これにより、起動と再起動を高速化できます。</span><span class="sxs-lookup"><span data-stu-id="dc470-162">This enables it to start and restart faster.</span></span> <span data-ttu-id="dc470-163">React アプリが毎回リビルドされるまで待つ必要がなくなります。</span><span class="sxs-lookup"><span data-stu-id="dc470-163">It's no longer waiting for your React app to rebuild each time.</span></span>
