---
title: Razor Components のホストと展開
author: guardrex
description: ASP.NET Core、Content Delivery Networks (CDN)、ファイル サーバー、GitHub ページを使用して、Razor Components と Blazor アプリをホストし展開する方法について説明します。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: host-and-deploy/razor-components/index
ms.openlocfilehash: 9debd75128ceecb805fc673a8182a785fc9f7942
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667953"
---
# <a name="host-and-deploy-razor-components"></a><span data-ttu-id="fe67f-103">Razor Components のホストと展開</span><span class="sxs-lookup"><span data-stu-id="fe67f-103">Host and deploy Razor Components</span></span>

<span data-ttu-id="fe67f-104">作成者: [Luke Latham](https://github.com/guardrex)、[Rainer Stropek](https://www.timecockpit.com)、[Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="fe67f-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

## <a name="publish-the-app"></a><span data-ttu-id="fe67f-105">アプリの発行</span><span class="sxs-lookup"><span data-stu-id="fe67f-105">Publish the app</span></span>

<span data-ttu-id="fe67f-106">アプリは [dotnet publish](/dotnet/core/tools/dotnet-publish) コマンドによって、リリース構成での展開のために発行されます。</span><span class="sxs-lookup"><span data-stu-id="fe67f-106">Apps are published for deployment in Release configuration with the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="fe67f-107">`dotnet publish` コマンドの実行は、IDE により組み込みの発行機能を使用して自動的に行われることがあります。そのため、ご使用の展開ツールによっては、このコマンドを手動でコマンド プロンプトから実行する必要がない場合があります。</span><span class="sxs-lookup"><span data-stu-id="fe67f-107">An IDE may handle executing the `dotnet publish` command automatically using its built-in publishing features, so it might not be necessary to manually execute the command from a command prompt depending on the development tools in use.</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="fe67f-108">`dotnet publish` により、プロジェクトの依存関係の[復元](/dotnet/core/tools/dotnet-restore)がトリガーされ、展開に使用されるアセットの作成前にプロジェクトが[ビルド](/dotnet/core/tools/dotnet-build)されます。</span><span class="sxs-lookup"><span data-stu-id="fe67f-108">`dotnet publish` triggers a [restore](/dotnet/core/tools/dotnet-restore) of the project's dependencies and [builds](/dotnet/core/tools/dotnet-build) the project before creating the assets for deployment.</span></span> <span data-ttu-id="fe67f-109">ビルド プロセスの一環として、アプリのダウンロード サイズを縮小し読み込み時間を短縮するため、未使用のメソッドおよびアセンブリが削除されます。</span><span class="sxs-lookup"><span data-stu-id="fe67f-109">As part of the build process, unused methods and assemblies are removed to reduce app download size and load times.</span></span> <span data-ttu-id="fe67f-110">*/bin/Release/&lt;&gt;/publish* フォルダーに、展開が作成されます。</span><span class="sxs-lookup"><span data-stu-id="fe67f-110">The deployment is created in the */bin/Release/&lt;target-framework&gt;/publish* folder.</span></span>

<span data-ttu-id="fe67f-111">*publish* フォルダー内のアセットは、Web サーバーに展開されます。</span><span class="sxs-lookup"><span data-stu-id="fe67f-111">The assets in the *publish* folder are deployed to the web server.</span></span> <span data-ttu-id="fe67f-112">展開のプロセスが手動であるか自動であるかは、ご使用の展開ツールによって異なります。</span><span class="sxs-lookup"><span data-stu-id="fe67f-112">Deployment might be a manual or automated process depending on the development tools in use.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="fe67f-113">ホストの構成値</span><span class="sxs-lookup"><span data-stu-id="fe67f-113">Host configuration values</span></span>

<span data-ttu-id="fe67f-114">[サーバー側のホスティング モデル](xref:razor-components/hosting-models#server-side-hosting-model)を使用する Razor Components アプリでは、[Web ホスト構成値](xref:fundamentals/host/web-host#host-configuration-values)を受け入れることができます。</span><span class="sxs-lookup"><span data-stu-id="fe67f-114">Razor Components apps that use the [server-side hosting model](xref:razor-components/hosting-models#server-side-hosting-model) can accept [Web Host configuration values](xref:fundamentals/host/web-host#host-configuration-values).</span></span>

<span data-ttu-id="fe67f-115">[クライアント側のホスティング モデル](xref:razor-components/hosting-models#client-side-hosting-model) を使用する Blazor アプリでは、開発環境での実行時に以下のホスト構成値をコマンドライン引数として受け入れることができます。</span><span class="sxs-lookup"><span data-stu-id="fe67f-115">Blazor apps that use the [client-side hosting model](xref:razor-components/hosting-models#client-side-hosting-model) can accept the following host configuration values as command-line arguments at runtime in the development environment.</span></span>

### <a name="content-root"></a><span data-ttu-id="fe67f-116">コンテンツ ルート</span><span class="sxs-lookup"><span data-stu-id="fe67f-116">Content Root</span></span>

<span data-ttu-id="fe67f-117">`--contentroot` 引数では、アプリのコンテンツ ファイルを含むディレクトリへの絶対パスが設定されます。</span><span class="sxs-lookup"><span data-stu-id="fe67f-117">The `--contentroot` argument sets the absolute path to the directory that contains the app's content files.</span></span>

* <span data-ttu-id="fe67f-118">コマンド プロンプトでアプリをローカルに実行するときに、引数を渡します。</span><span class="sxs-lookup"><span data-stu-id="fe67f-118">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="fe67f-119">アプリのディレクトリから、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="fe67f-119">From the app's directory, execute:</span></span>

  ```console
  dotnet run --contentroot=/<content-root>
  ```

* <span data-ttu-id="fe67f-120">**IIS Express** プロファイル内のアプリの *launchSettings.json* ファイルに、エントリを追加します。</span><span class="sxs-lookup"><span data-stu-id="fe67f-120">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="fe67f-121">アプリを Visual Studio デバッガーを使用して実行する場合、およびコマンド プロンプトから `dotnet run` を使用して実行する場合、この設定がピックアップされます。</span><span class="sxs-lookup"><span data-stu-id="fe67f-121">This setting is picked up when running the app with the Visual Studio Debugger and when running the app from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--contentroot=/<content-root>"
  ```

* <span data-ttu-id="fe67f-122">Visual Studio の **[プロパティ]** > **[デバッグ]** > **[アプリケーションの引数]** で、引数を指定します。</span><span class="sxs-lookup"><span data-stu-id="fe67f-122">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="fe67f-123">Visual Studio のプロパティ ページで引数を設定すると、その引数が *launchSettings.json* ファイルに追加されます。</span><span class="sxs-lookup"><span data-stu-id="fe67f-123">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --contentroot=/<content-root>
  ```

### <a name="path-base"></a><span data-ttu-id="fe67f-124">パス ベース</span><span class="sxs-lookup"><span data-stu-id="fe67f-124">Path base</span></span>

<span data-ttu-id="fe67f-125">`--pathbase` 引数により、ルート以外の仮想パスでローカルで実行されているアプリに対して、アプリのベース パスを設定することができます (ステージングと運用環境の場合、`<base>` タグ `href` は `/` 以外のパスに設定されます)。</span><span class="sxs-lookup"><span data-stu-id="fe67f-125">The `--pathbase` argument sets the app base path for an app run locally with a non-root virtual path (the `<base>` tag `href` is set to a path other than `/` for staging and production).</span></span> <span data-ttu-id="fe67f-126">詳細については、「[App base path](#app-base-path)」(アプリのベース パス) セクションを参照してください。</span><span class="sxs-lookup"><span data-stu-id="fe67f-126">For more information, see the [App base path](#app-base-path) section.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fe67f-127">`<base>` タグの `href` に指定するパスとは異なり、`--pathbase` 引数値を渡すときはスラッシュ (`/`) を末尾に含めないでください。</span><span class="sxs-lookup"><span data-stu-id="fe67f-127">Unlike the path provided to `href` of the `<base>` tag, don't include a trailing slash (`/`) when passing the `--pathbase` argument value.</span></span> <span data-ttu-id="fe67f-128">`<base>` タグでアプリのベース パスが `<base href="/CoolApp/" />` と指定されている場合 (末尾にスラッシュあり)、コマンドライン引数値としては `--pathbase=/CoolApp` を渡してください (末尾にスラッシュなし)。</span><span class="sxs-lookup"><span data-stu-id="fe67f-128">If the app base path is provided in the `<base>` tag as `<base href="/CoolApp/" />` (includes a trailing slash), pass the command-line argument value as `--pathbase=/CoolApp` (no trailing slash).</span></span>

* <span data-ttu-id="fe67f-129">コマンド プロンプトでアプリをローカルに実行するときに、引数を渡します。</span><span class="sxs-lookup"><span data-stu-id="fe67f-129">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="fe67f-130">アプリのディレクトリから、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="fe67f-130">From the app's directory, execute:</span></span>

  ```console
  dotnet run --pathbase=/<virtual-path>
  ```

* <span data-ttu-id="fe67f-131">**IIS Express** プロファイル内のアプリの *launchSettings.json* ファイルに、エントリを追加します。</span><span class="sxs-lookup"><span data-stu-id="fe67f-131">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="fe67f-132">アプリを Visual Studio デバッガーを使用して実行する場合、およびコマンド プロンプトから `dotnet run` を使用して実行する場合、この設定がピックアップされます。</span><span class="sxs-lookup"><span data-stu-id="fe67f-132">This setting is picked up when running the app with the Visual Studio Debugger and when running the app from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--pathbase=/<virtual-path>"
  ```

* <span data-ttu-id="fe67f-133">Visual Studio の **[プロパティ]** > **[デバッグ]** > **[アプリケーションの引数]** で、引数を指定します。</span><span class="sxs-lookup"><span data-stu-id="fe67f-133">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="fe67f-134">Visual Studio のプロパティ ページで引数を設定すると、その引数が *launchSettings.json* ファイルに追加されます。</span><span class="sxs-lookup"><span data-stu-id="fe67f-134">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --pathbase=/<virtual-path>
  ```

### <a name="urls"></a><span data-ttu-id="fe67f-135">URL</span><span class="sxs-lookup"><span data-stu-id="fe67f-135">URLs</span></span>

<span data-ttu-id="fe67f-136">`--urls` 引数では、要求をリッスンするポートとプロトコルを使用して、IP アドレスまたはホスト アドレスが示されます。</span><span class="sxs-lookup"><span data-stu-id="fe67f-136">The `--urls` argument indicates the IP addresses or host addresses with ports and protocols to listen on for requests.</span></span>

* <span data-ttu-id="fe67f-137">コマンド プロンプトでアプリをローカルに実行するときに、引数を渡します。</span><span class="sxs-lookup"><span data-stu-id="fe67f-137">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="fe67f-138">アプリのディレクトリから、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="fe67f-138">From the app's directory, execute:</span></span>

  ```console
  dotnet run --urls=http://127.0.0.1:0
  ```

* <span data-ttu-id="fe67f-139">**IIS Express** プロファイル内のアプリの *launchSettings.json* ファイルに、エントリを追加します。</span><span class="sxs-lookup"><span data-stu-id="fe67f-139">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="fe67f-140">アプリを Visual Studio デバッガーを使用して実行する場合、およびコマンド プロンプトから `dotnet run` を使用して実行する場合、この設定がピックアップされます。</span><span class="sxs-lookup"><span data-stu-id="fe67f-140">This setting is picked up when running the app with the Visual Studio Debugger and when running the app from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--urls=http://127.0.0.1:0"
  ```

* <span data-ttu-id="fe67f-141">Visual Studio の **[プロパティ]** > **[デバッグ]** > **[アプリケーションの引数]** で、引数を指定します。</span><span class="sxs-lookup"><span data-stu-id="fe67f-141">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="fe67f-142">Visual Studio のプロパティ ページで引数を設定すると、その引数が *launchSettings.json* ファイルに追加されます。</span><span class="sxs-lookup"><span data-stu-id="fe67f-142">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --urls=http://127.0.0.1:0
  ```

## <a name="deploy-a-client-side-blazor-app"></a><span data-ttu-id="fe67f-143">クライアント側 Blazor アプリの展開</span><span class="sxs-lookup"><span data-stu-id="fe67f-143">Deploy a client-side Blazor app</span></span>

<span data-ttu-id="fe67f-144">[クライアント側のホスティング モデルは次のとおりです](xref:razor-components/hosting-models#client-side-hosting-model)。</span><span class="sxs-lookup"><span data-stu-id="fe67f-144">With the [client-side hosting model](xref:razor-components/hosting-models#client-side-hosting-model):</span></span>

* <span data-ttu-id="fe67f-145">Blazor アプリ、その依存関係、.NET ランタイムがブラウザーにダウンロードされます。</span><span class="sxs-lookup"><span data-stu-id="fe67f-145">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="fe67f-146">アプリがブラウザー UI スレッド上で直接実行されます。</span><span class="sxs-lookup"><span data-stu-id="fe67f-146">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="fe67f-147">次の方法のいずれかがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="fe67f-147">Either of the following strategies is supported:</span></span>
  * <span data-ttu-id="fe67f-148">Blazor アプリは、ASP.NET Core アプリによって提供されます。</span><span class="sxs-lookup"><span data-stu-id="fe67f-148">The Blazor app is served by an ASP.NET Core app.</span></span> <span data-ttu-id="fe67f-149">詳細については、「[ASP.NET Core を使用してホストされているクライアント側 Blazor 展開](#client-side-blazor-hosted-deployment-with-aspnet-core)」セクションで説明します。</span><span class="sxs-lookup"><span data-stu-id="fe67f-149">Covered in the [Client-side Blazor hosted deployment with ASP.NET Core](#client-side-blazor-hosted-deployment-with-aspnet-core) section.</span></span>
  * <span data-ttu-id="fe67f-150">Blazor アプリは、Blazor アプリの提供に .NET が使用されていない静的ホスティング Web サーバーまたはサービス上に配置されます。</span><span class="sxs-lookup"><span data-stu-id="fe67f-150">The Blazor app is placed on a static hosting web server or service, where .NET isn't used to serve the Blazor app.</span></span> <span data-ttu-id="fe67f-151">詳細については、「[クライアント側 Blazor スタンドアロン展開](#client-side-blazor-standalone-deployment)」セクションで説明します。</span><span class="sxs-lookup"><span data-stu-id="fe67f-151">Covered in the [Client-side Blazor standalone deployment](#client-side-blazor-standalone-deployment) section.</span></span>
  
### <a name="configure-the-linker"></a><span data-ttu-id="fe67f-152">リンカーを構成する</span><span class="sxs-lookup"><span data-stu-id="fe67f-152">Configure the Linker</span></span>

<span data-ttu-id="fe67f-153">Blazor では、出力アセンブリから不要な中間言語 (IL) を削除するために、IL リンク設定が各ビルド上で実行されます。</span><span class="sxs-lookup"><span data-stu-id="fe67f-153">Blazor performs Intermediate Language (IL) linking on each build to remove unnecessary IL from the output assemblies.</span></span> <span data-ttu-id="fe67f-154">ビルド上のアセンブリのリンク設定を制御することができます。</span><span class="sxs-lookup"><span data-stu-id="fe67f-154">You can control assembly linking on build.</span></span> <span data-ttu-id="fe67f-155">詳細については、「<xref:host-and-deploy/razor-components/configure-linker>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="fe67f-155">For more information, see <xref:host-and-deploy/razor-components/configure-linker>.</span></span>

### <a name="rewrite-urls-for-correct-routing"></a><span data-ttu-id="fe67f-156">正しいルーティングのために URL を書き換える</span><span class="sxs-lookup"><span data-stu-id="fe67f-156">Rewrite URLs for correct routing</span></span>

<span data-ttu-id="fe67f-157">クライアント側のアプリ内のページ コンポーネントに対するルーティング要求は、サーバー側のホストされているアプリに対するルーティング要求のようにシンプルなものではありません。</span><span class="sxs-lookup"><span data-stu-id="fe67f-157">Routing requests for page components in a client-side app isn't as simple as routing requests to a server-side, hosted app.</span></span> <span data-ttu-id="fe67f-158">次の 2 つのページがあるクライアント側のアプリについて考えてみます。</span><span class="sxs-lookup"><span data-stu-id="fe67f-158">Consider a client-side app with two pages:</span></span>

* <span data-ttu-id="fe67f-159">**_Main.cshtml_** &ndash; アプリのルートで読み込まれ、About ページ (`href="About"`) へのリンクが含まれています。</span><span class="sxs-lookup"><span data-stu-id="fe67f-159">**_Main.cshtml_** &ndash; Loads at the root of the app and contains a link to the About page (`href="About"`).</span></span>
* <span data-ttu-id="fe67f-160">**_About.cshtml_** &ndash; About ページ。</span><span class="sxs-lookup"><span data-stu-id="fe67f-160">**_About.cshtml_** &ndash; About page.</span></span>

<span data-ttu-id="fe67f-161">アプリの既定のドキュメントがブラウザーのアドレス バー (例: `https://www.contoso.com/`) を使用して要求された場合:</span><span class="sxs-lookup"><span data-stu-id="fe67f-161">When the app's default document is requested using the browser's address bar (for example, `https://www.contoso.com/`):</span></span>

1. <span data-ttu-id="fe67f-162">ブラウザーにより要求が送信されます。</span><span class="sxs-lookup"><span data-stu-id="fe67f-162">The browser makes a request.</span></span>
1. <span data-ttu-id="fe67f-163">既定のページ (通常は *index.html*) が返されます。</span><span class="sxs-lookup"><span data-stu-id="fe67f-163">The default page is returned, which is usually *index.html*.</span></span>
1. <span data-ttu-id="fe67f-164">*index.html* によりアプリがブートストラップされます。</span><span class="sxs-lookup"><span data-stu-id="fe67f-164">*index.html* bootstraps the app.</span></span>
1. <span data-ttu-id="fe67f-165">Blazor のルーターによって読み込まれ、Razor Main ページ (*Main.cshtml*) が表示されます。</span><span class="sxs-lookup"><span data-stu-id="fe67f-165">Blazor's router loads and the Razor Main page (*Main.cshtml*) is displayed.</span></span>

<span data-ttu-id="fe67f-166">Main ページで About ページへのリンクを選択すると、About ページが読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="fe67f-166">On the Main page, selecting the link to the About page loads the About page.</span></span> <span data-ttu-id="fe67f-167">Blazor のルーターにより、ブラウザーからのインターネット上での `www.contoso.com` に対する `About` の要求が停止され、About ページ自体が提供されるため、クライアント上での About ページへのリンクの選択が機能します。</span><span class="sxs-lookup"><span data-stu-id="fe67f-167">Selecting the link to the About page works on the client because the Blazor router stops the browser from making a request on the Internet to `www.contoso.com` for `About` and serves the About page itself.</span></span> <span data-ttu-id="fe67f-168">*クライアント側のアプリ内*の内部ページの要求もすべて、同じように動作します。要求によって、サーバーにホストされているインターネット上のリソースに対するブラウザーベースの要求がトリガーされることはありません。</span><span class="sxs-lookup"><span data-stu-id="fe67f-168">All of the requests for internal pages *within the client-side app* work the same way: Requests don't trigger browser-based requests to server-hosted resources on the Internet.</span></span> <span data-ttu-id="fe67f-169">要求は、ルーターによって内部的に処理されます。</span><span class="sxs-lookup"><span data-stu-id="fe67f-169">The router handles the requests internally.</span></span>

<span data-ttu-id="fe67f-170">ブラウザーのアドレス バーを使用して `www.contoso.com/About` の要求が行われた場合、その要求は失敗します。</span><span class="sxs-lookup"><span data-stu-id="fe67f-170">If a request is made using the browser's address bar for `www.contoso.com/About`, the request fails.</span></span> <span data-ttu-id="fe67f-171">アプリのインターネット ホスト上にそのようなリソースは存在しないため、*404 見つかりません*という応答が返されます。</span><span class="sxs-lookup"><span data-stu-id="fe67f-171">No such resource exists on the app's Internet host, so a *404 Not found* response is returned.</span></span>

<span data-ttu-id="fe67f-172">ブラウザーではクライアント側ページの要求がインターネットベースのホストに対して行われるため、Web サーバーとホスティング サービスでは、サーバー上に物理的に存在しないリソースに対する *index.html* ページへのすべての要求を、書き換える必要があります。</span><span class="sxs-lookup"><span data-stu-id="fe67f-172">Because browsers make requests to Internet-based hosts for client-side pages, web servers and hosting services must rewrite all requests for resources not physically on the server to the *index.html* page.</span></span> <span data-ttu-id="fe67f-173">*index.html* が返されると、アプリのクライアント側のルーターがそれを受け取り、正しいリソースで応答します。</span><span class="sxs-lookup"><span data-stu-id="fe67f-173">When *index.html* is returned, the app's client-side router takes over and responds with the correct resource.</span></span>

### <a name="app-base-path"></a><span data-ttu-id="fe67f-174">アプリのベース パス</span><span class="sxs-lookup"><span data-stu-id="fe67f-174">App base path</span></span>

<span data-ttu-id="fe67f-175">アプリのベース パスは、サーバー上の仮想アプリ ルート パスです。</span><span class="sxs-lookup"><span data-stu-id="fe67f-175">The app base path is the virtual app root path on the server.</span></span> <span data-ttu-id="fe67f-176">例えば、`/CoolApp/` の仮想フォルダー内の Contoso サーバー上に存在するアプリの場合、そのアプリは `https://www.contoso.com/CoolApp` で到達可能であり、仮想ベース パスは `/CoolApp/` となります。</span><span class="sxs-lookup"><span data-stu-id="fe67f-176">For example, an app that resides on the Contoso server in a virtual folder at `/CoolApp/` is reached at `https://www.contoso.com/CoolApp` and has a virtual base path of `/CoolApp/`.</span></span> <span data-ttu-id="fe67f-177">アプリのベース パスを `CoolApp/` に設定することで、アプリのサーバー上の仮想位置がわかります。</span><span class="sxs-lookup"><span data-stu-id="fe67f-177">By setting the app base path to `CoolApp/`, the app is made aware of where it virtually resides on the server.</span></span> <span data-ttu-id="fe67f-178">アプリでは、アプリのベース パスを使用して、ルート ディレクトリに存在しないコンポーネントからのアプリのルートへの相対 URL を構築することができます。</span><span class="sxs-lookup"><span data-stu-id="fe67f-178">The app can use the app base path to construct URLs relative to the app root from a component that isn't in the root directory.</span></span> <span data-ttu-id="fe67f-179">これにより、ディレクトリ構造の別のレベルに存在するコンポーネントでは、アプリ内のさまざまな場所にある他のリソースに対するリンクを構築することができます。</span><span class="sxs-lookup"><span data-stu-id="fe67f-179">This allows components that exist at different levels of the directory structure to build links to other resources at locations throughout the app.</span></span> <span data-ttu-id="fe67f-180">リンクの `href` ターゲットがアプリのベース パス URI 空間内の場合に、そのハイパーリンクのクリックを阻止するためにも使用できます。つまり、Blazor のルーターにより内部ナビゲーションが処理されます。</span><span class="sxs-lookup"><span data-stu-id="fe67f-180">The app base path is also used to intercept hyperlink clicks where the `href` target of the link is within the app base path URI space&mdash;the Blazor router handles the internal navigation.</span></span>

<span data-ttu-id="fe67f-181">多くのホスティング シナリオでは、サーバーのアプリへの仮想パスは、アプリのルートです。</span><span class="sxs-lookup"><span data-stu-id="fe67f-181">In many hosting scenarios, the server's virtual path to the app is the root of the app.</span></span> <span data-ttu-id="fe67f-182">このような場合、アプリのベース パスは最初にスラッシュ (`<base href="/" />`) が付きます。これは、アプリの既定の構成です。</span><span class="sxs-lookup"><span data-stu-id="fe67f-182">In these cases, the app base path is a forward slash (`<base href="/" />`), which is the default configuration for an app.</span></span> <span data-ttu-id="fe67f-183">GitHub ページと IIS 仮想ディレクトリまたはサブアプリケーションなど、その他のホスティング シナリオの場合、サーバーのアプリへの仮想パスを、アプリのベース パスとして設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="fe67f-183">In other hosting scenarios, such as GitHub Pages and IIS virtual directories or sub-applications, the app base path must be set to the server's virtual path to the app.</span></span> <span data-ttu-id="fe67f-184">アプリのベース パスを設定するには、`<head>`タグ要素内で見つかった *index.html* 内の `<base>` タグを追加または更新します。</span><span class="sxs-lookup"><span data-stu-id="fe67f-184">To set the app's base path, add or update the `<base>` tag in *index.html* found within the `<head>` tag elements.</span></span> <span data-ttu-id="fe67f-185">`href` 属性値として `<virtual-path>/` (末尾にスラッシュが必要) を設定します。ここで、`<virtual-path>/` は、アプリに対するサーバー上の完全仮想アプリ ルート パスです。</span><span class="sxs-lookup"><span data-stu-id="fe67f-185">Set the `href` attribute value to `<virtual-path>/` (the trailing slash is required), where `<virtual-path>/` is the full virtual app root path on the server for the app.</span></span> <span data-ttu-id="fe67f-186">前の例では、仮想パスには `CoolApp/`: `<base href="CoolApp/" />` が設定されていました。</span><span class="sxs-lookup"><span data-stu-id="fe67f-186">In the preceding example, the virtual path is set to `CoolApp/`: `<base href="CoolApp/" />`.</span></span>

<span data-ttu-id="fe67f-187">ルート以外の仮想パスが構成されているアプリの場合 (例: `<base href="CoolApp/" />`)、そのアプリは*ローカルで実行*されていると自身のリソースを見つけることができません。</span><span class="sxs-lookup"><span data-stu-id="fe67f-187">For an app with a non-root virtual path configured (for example, `<base href="CoolApp/" />`), the app fails to find its resources *when run locally*.</span></span> <span data-ttu-id="fe67f-188">ローカルでの開発およびテスト中は、実行時の `<base>` タグの `href` 値と一致する*パス ベース*引数を指定することで、この問題を克服することができます。</span><span class="sxs-lookup"><span data-stu-id="fe67f-188">To overcome this problem during local development and testing, you can supply a *path base* argument that matches the `href` value of the `<base>` tag at runtime.</span></span>

<span data-ttu-id="fe67f-189">アプリをローカルで実行しているときにパス ベース引数をルート パス (`/`) と共に渡すには、アプリのディレクトリから次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="fe67f-189">To pass the path base argument with the root path (`/`) when running the app locally, execute the following command from the app's directory:</span></span>

```console
dotnet run --pathbase=/CoolApp
```

<span data-ttu-id="fe67f-190">アプリは `http://localhost:port/CoolApp` でローカルで応答します。</span><span class="sxs-lookup"><span data-stu-id="fe67f-190">The app responds locally at `http://localhost:port/CoolApp`.</span></span>

<span data-ttu-id="fe67f-191">詳細については、「ホストの構成値」の「[パス ベース](#path-base)」セクションを参照してください。</span><span class="sxs-lookup"><span data-stu-id="fe67f-191">For more information, see the [path base host configuration value](#path-base) section.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fe67f-192">アプリが[クライアント側のホスティング モデル](xref:razor-components/hosting-models#client-side-hosting-model) (**Blazor** プロジェクト テンプレートに基づく) を使用し、ASP.NET Core アプリ内で IIS サブアプリケーションとしてホストされている場合、継承された ASP.NET Core モジュール ハンドラーを無効とするか、*web.config*ファイル内のルート (親) アプリの `<handlers>` セクションがサブアプリに継承されていないことを確認することが必要となります。</span><span class="sxs-lookup"><span data-stu-id="fe67f-192">If an app uses the [client-side hosting model](xref:razor-components/hosting-models#client-side-hosting-model) (based on the **Blazor** project template) and is hosted as an IIS sub-application in an ASP.NET Core app, it's important to disable the inherited ASP.NET Core Module handler or make sure the root (parent) app's `<handlers>` section in the *web.config* file isn't inherited by the sub-app.</span></span>
>
> <span data-ttu-id="fe67f-193">アプリの発行された *web.config* ファイル内のハンドラーを、`<handlers>` セクションをファイルに追加することで削除します。</span><span class="sxs-lookup"><span data-stu-id="fe67f-193">Remove the handler in the app's published *web.config* file by adding a `<handlers>` section to the file:</span></span>
>
> ```xml
> <handlers>
>   <remove name="aspNetCore" />
> </handlers>
> ```
>
> <span data-ttu-id="fe67f-194">または、`inheritInChildApplications` が `false` に設定された `<location>` 要素を使用して、ルート (親) アプリの `<system.webServer>` セクションの継承を無効にします。</span><span class="sxs-lookup"><span data-stu-id="fe67f-194">Alternatively, disable inheritance of the root (parent) app's `<system.webServer>` section using a `<location>` element with `inheritInChildApplications` set to `false`:</span></span>
>
> ```xml
> <?xml version="1.0" encoding="utf-8"?>
> <configuration>
>  <location path="." inheritInChildApplications="false">
>    <system.webServer>
>      <handlers>
>        <add name="aspNetCore" ... />
>      </handlers>
>      <aspNetCore ... />
>    </system.webServer>
>  </location>
> </configuration>
> ```
>
> <span data-ttu-id="fe67f-195">ハンドラーの削除または継承の無効化は、このセクションで説明したアプリのベース パスの構成に加えて実行されます。</span><span class="sxs-lookup"><span data-stu-id="fe67f-195">Removing the handler or disabling inheritance is performed in addition to configuring the app's base path as described in this section.</span></span> <span data-ttu-id="fe67f-196">IIS でサブアプリを構成するときに、アプリの *index.html* ファイル内のアプリのベース パスを、使用している IIS エイリアスに設定します。</span><span class="sxs-lookup"><span data-stu-id="fe67f-196">Set the app base path in the app's *index.html* file to the IIS alias used when configuring the sub-app in IIS.</span></span>

### <a name="client-side-blazor-hosted-deployment-with-aspnet-core"></a><span data-ttu-id="fe67f-197">ASP.NET Core を使用してホストされているクライアント側 Blazor 展開</span><span class="sxs-lookup"><span data-stu-id="fe67f-197">Client-side Blazor hosted deployment with ASP.NET Core</span></span>

<span data-ttu-id="fe67f-198">*ホストされている展開*により、クライアント側の Blazor アプリが、サーバー上で実行されている [ASP.NET Core アプリ](xref:index)からブラウザーに提供されます。</span><span class="sxs-lookup"><span data-stu-id="fe67f-198">A *hosted deployment* serves the client-side Blazor app to browsers from an [ASP.NET Core app](xref:index) that runs on a server.</span></span>

<span data-ttu-id="fe67f-199">Blazor アプリは、発行された出力に ASP.NET Core アプリと共に含まれているため、2 つのアプリを一緒に展開することができます。</span><span class="sxs-lookup"><span data-stu-id="fe67f-199">The Blazor app is included with the ASP.NET Core app in the published output so that the two apps are deployed together.</span></span> <span data-ttu-id="fe67f-200">ASP.NET Core アプリをホストできる Web サーバーが必要です。</span><span class="sxs-lookup"><span data-stu-id="fe67f-200">A web server that is capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="fe67f-201">ホストされている展開の場合、Visual Studio には **Blazor (ASP.NET Core でホストされる)** プロジェクト テンプレートが含まれています ([dotnet new](/dotnet/core/tools/dotnet-new) コマンドを使用する場合は `blazorhosted` テンプレート)。</span><span class="sxs-lookup"><span data-stu-id="fe67f-201">For a hosted deployment, Visual Studio includes the **Blazor (ASP.NET Core hosted)** project template (`blazorhosted` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

<span data-ttu-id="fe67f-202">ASP.NET Core アプリでのホストと展開の詳細については、「<xref:host-and-deploy/index>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="fe67f-202">For more information on ASP.NET Core app hosting and deployment, see <xref:host-and-deploy/index>.</span></span>

<span data-ttu-id="fe67f-203">Azure App Service の展開については、次のトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="fe67f-203">For information on deploying to Azure App Service, see the following topics:</span></span>

<xref:tutorials/publish-to-azure-webapp-using-vs>  
<span data-ttu-id="fe67f-204">Visual Studio を使用して Azure App Service に ASP.NET Core アプリを発行する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="fe67f-204">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>

### <a name="client-side-blazor-standalone-deployment"></a><span data-ttu-id="fe67f-205">クライアント側 Blazor スタンドアロン展開</span><span class="sxs-lookup"><span data-stu-id="fe67f-205">Client-side Blazor standalone deployment</span></span>

<span data-ttu-id="fe67f-206">*スタンドアロン展開*により、クライアント側 Blazor アプリが、クライアントによって直接要求される静的ファイルのセットとして提供されます。</span><span class="sxs-lookup"><span data-stu-id="fe67f-206">A *standalone deployment* serves the client-side Blazor app as a set of static files that are requested directly by clients.</span></span> <span data-ttu-id="fe67f-207">Web サーバーは、Blazor アプリの提供には使用されません。</span><span class="sxs-lookup"><span data-stu-id="fe67f-207">A web server isn't used to serve the Blazor app.</span></span>

#### <a name="client-side-blazor-standalone-hosting-with-iis"></a><span data-ttu-id="fe67f-208">IIS を使用したクライアント側 Blazor スタンドアロン ホスティング</span><span class="sxs-lookup"><span data-stu-id="fe67f-208">Client-side Blazor standalone hosting with IIS</span></span>

<span data-ttu-id="fe67f-209">IIS は、Blazor アプリ対応の静的ファイル サーバーです。</span><span class="sxs-lookup"><span data-stu-id="fe67f-209">IIS is a capable static file server for Blazor apps.</span></span> <span data-ttu-id="fe67f-210">Blazor をホストするよう IIS を構成する方法については、「[Build a Static Website on IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis)」 (IIS で静的 Web サイトを構築する) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="fe67f-210">To configure IIS to host Blazor, see [Build a Static Website on IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).</span></span>

<span data-ttu-id="fe67f-211">発行されたアセットは、*\\bin\\Release\\&lt;ターゲットフレームワーク&gt;\\publish* フォルダーに作成されます。</span><span class="sxs-lookup"><span data-stu-id="fe67f-211">Published assets are created in the *\\bin\\Release\\&lt;target-framework&gt;\\publish* folder.</span></span> <span data-ttu-id="fe67f-212">*publish*フォルダーのコンテンツを、Web サーバーまたはホスティング サービス上でホストします。</span><span class="sxs-lookup"><span data-stu-id="fe67f-212">Host the contents of the *publish* folder on the web server or hosting service.</span></span>

<span data-ttu-id="fe67f-213">**web.config**</span><span class="sxs-lookup"><span data-stu-id="fe67f-213">**web.config**</span></span>

<span data-ttu-id="fe67f-214">Blazor プロジェクトが発行されると、*web.config* ファイルが以下の IIS 構成で作成されます。</span><span class="sxs-lookup"><span data-stu-id="fe67f-214">When a Blazor project is published, a *web.config* file is created with the following IIS configuration:</span></span>

* <span data-ttu-id="fe67f-215">各ファイル拡張子に対して設定される MIME の種類は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="fe67f-215">MIME types are set for the following file extensions:</span></span>
  * <span data-ttu-id="fe67f-216">*\*.dll*: `application/octet-stream`</span><span class="sxs-lookup"><span data-stu-id="fe67f-216">*\*.dll*: `application/octet-stream`</span></span>
  * <span data-ttu-id="fe67f-217">*\*.json*: `application/json`</span><span class="sxs-lookup"><span data-stu-id="fe67f-217">*\*.json*: `application/json`</span></span>
  * <span data-ttu-id="fe67f-218">*\*.wasm*: `application/wasm`</span><span class="sxs-lookup"><span data-stu-id="fe67f-218">*\*.wasm*: `application/wasm`</span></span>
  * <span data-ttu-id="fe67f-219">*\*.woff*: `application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="fe67f-219">*\*.woff*: `application/font-woff`</span></span>
  * <span data-ttu-id="fe67f-220">*\*.woff2*: `application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="fe67f-220">*\*.woff2*: `application/font-woff`</span></span>
* <span data-ttu-id="fe67f-221">次の MIME の種類に対しては、HTTP 圧縮が有効にされます。</span><span class="sxs-lookup"><span data-stu-id="fe67f-221">HTTP compression is enabled for the following MIME types:</span></span>
  * `application/octet-stream`
  * `application/wasm`
* <span data-ttu-id="fe67f-222">URL Rewrite Module のルールが確立されます。</span><span class="sxs-lookup"><span data-stu-id="fe67f-222">URL Rewrite Module rules are established:</span></span>
  * <span data-ttu-id="fe67f-223">アプリの静的なアセットが存在するサブディレクトリ (*&lt;アセンブリ名&gt;\\dist\\&lt;要求されたパス&gt;*) が提供されます。</span><span class="sxs-lookup"><span data-stu-id="fe67f-223">Serve the sub-directory where the app's static assets reside (*&lt;assembly_name&gt;\\dist\\&lt;path_requested&gt;*).</span></span>
  * <span data-ttu-id="fe67f-224">ファイル以外のアセットの要求が、アプリの静的アセット フォルダー内の既定のドキュメント (*&lt;アセンブリ名&gt;\\dist\\index.html*) にリダイレクトされるように、SPA フォールバック ルーティングが作成されます</span><span class="sxs-lookup"><span data-stu-id="fe67f-224">Create SPA fallback routing so that requests for non-file assets are redirected to the app's default document in its static assets folder (*&lt;assembly_name&gt;\\dist\\index.html*).</span></span>

<span data-ttu-id="fe67f-225">**URL Rewrite Module をインストールする**</span><span class="sxs-lookup"><span data-stu-id="fe67f-225">**Install the URL Rewrite Module**</span></span>

<span data-ttu-id="fe67f-226">[URL Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite) は、URL の書き換えに必要となります。</span><span class="sxs-lookup"><span data-stu-id="fe67f-226">The [URL Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite) is required to rewrite URLs.</span></span> <span data-ttu-id="fe67f-227">このモジュールは既定ではインストールされていません。また、Web サーバー (IIS) の役割サービス機能としてインストールすることはできません。</span><span class="sxs-lookup"><span data-stu-id="fe67f-227">The module isn't installed by default, and it isn't available for install as an Web Server (IIS) role service feature.</span></span> <span data-ttu-id="fe67f-228">モジュールは、IIS Web サイトからダウンロードする必要があります。</span><span class="sxs-lookup"><span data-stu-id="fe67f-228">The module must be downloaded from the IIS website.</span></span> <span data-ttu-id="fe67f-229">Web Platform Installer を使用してモジュールをインストールします。</span><span class="sxs-lookup"><span data-stu-id="fe67f-229">Use the Web Platform Installer to install the module:</span></span>

1. <span data-ttu-id="fe67f-230">ローカルで、[URL Rewrite Module のダウンロード ページ](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads)に移動します。</span><span class="sxs-lookup"><span data-stu-id="fe67f-230">Locally, navigate to the [URL Rewrite Module downloads page](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads).</span></span> <span data-ttu-id="fe67f-231">英語版については、**WebPI** を選択して WebPI インストーラーをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="fe67f-231">For the English version, select **WebPI** to download the WebPI installer.</span></span> <span data-ttu-id="fe67f-232">その他の言語版については、サーバーの適切なアーキテクチャ (x86/x64) を選択して、インストーラーをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="fe67f-232">For other languages, select the appropriate architecture for the server (x86/x64) to download the installer.</span></span>
1. <span data-ttu-id="fe67f-233">インストーラーをサーバーにコピーします。</span><span class="sxs-lookup"><span data-stu-id="fe67f-233">Copy the installer to the server.</span></span> <span data-ttu-id="fe67f-234">インストーラーを実行します。</span><span class="sxs-lookup"><span data-stu-id="fe67f-234">Run the installer.</span></span> <span data-ttu-id="fe67f-235">**[インストール]** ボタンを選択して、ライセンス条項に同意します。</span><span class="sxs-lookup"><span data-stu-id="fe67f-235">Select the **Install** button and accept the license terms.</span></span> <span data-ttu-id="fe67f-236">インストールが完了した後、サーバーの再起動は必要はありません。</span><span class="sxs-lookup"><span data-stu-id="fe67f-236">A server restart isn't required after the install completes.</span></span>

<span data-ttu-id="fe67f-237">**Web サイトを構成する**</span><span class="sxs-lookup"><span data-stu-id="fe67f-237">**Configure the website**</span></span>

<span data-ttu-id="fe67f-238">Web サイトの**物理パス**をアプリのフォルダーに設定します。</span><span class="sxs-lookup"><span data-stu-id="fe67f-238">Set the website's **Physical path** to the app's folder.</span></span> <span data-ttu-id="fe67f-239">フォルダーには次のものが含まれます。</span><span class="sxs-lookup"><span data-stu-id="fe67f-239">The folder contains:</span></span>

* <span data-ttu-id="fe67f-240">*web.config* ファイル。IIS ではこのファイルを使用して、必要なリダイレクト ルールやファイルのコンテンツの種類など、Web サイトの構成が行われます。</span><span class="sxs-lookup"><span data-stu-id="fe67f-240">The *web.config* file that IIS uses to configure the website, including the required redirect rules and file content types.</span></span>
* <span data-ttu-id="fe67f-241">アプリの静的なアセット フォルダー。</span><span class="sxs-lookup"><span data-stu-id="fe67f-241">The app's static asset folder.</span></span>

<span data-ttu-id="fe67f-242">**トラブルシューティング**</span><span class="sxs-lookup"><span data-stu-id="fe67f-242">**Troubleshooting**</span></span>

<span data-ttu-id="fe67f-243">Web サイトの構成にアクセスしようとしたときに、*500 内部サーバー エラー*という応答が返され、IIS マネージャーによりエラーがスローされた場合、URL Rewrite Module がインストールされていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="fe67f-243">If a *500 Internal Server Error* is received and IIS Manager throws errors when attempting to access the website's configuration, confirm that the URL Rewrite Module is installed.</span></span> <span data-ttu-id="fe67f-244">モジュールがインストールされていない場合、IIS では *web.config* ファイルを解析できません。</span><span class="sxs-lookup"><span data-stu-id="fe67f-244">When the module isn't installed, the *web.config* file can't be parsed by IIS.</span></span> <span data-ttu-id="fe67f-245">これは、IIS マネージャーによる Web サイトの構成の読み込み、そして Web サイトによる Blazor の静的ファイルの提供を阻止するためのものです。</span><span class="sxs-lookup"><span data-stu-id="fe67f-245">This prevents the IIS Manager from loading the website's configuration and the website from serving Blazor's static files.</span></span>

<span data-ttu-id="fe67f-246">IIS への展開に関するトラブルシューティングの詳細については、「<xref:host-and-deploy/iis/troubleshoot>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="fe67f-246">For more information on troubleshooting deployments to IIS, see <xref:host-and-deploy/iis/troubleshoot>.</span></span>

#### <a name="client-side-blazor-standalone-hosting-with-nginx"></a><span data-ttu-id="fe67f-247">Nginx を使用したクライアント側 Blazor スタンドアロン ホスティング</span><span class="sxs-lookup"><span data-stu-id="fe67f-247">Client-side Blazor standalone hosting with Nginx</span></span>

<span data-ttu-id="fe67f-248">以下に示す *nginx.conf* ファイルは、Nginx が対応するファイルをディスク上で見つけられないときは *Index.html* ファイルを送信するよう Nginx を構成する方法を示すために、簡素化したものです。</span><span class="sxs-lookup"><span data-stu-id="fe67f-248">The following *nginx.conf* file is simplified to show how to configure Nginx to send the *Index.html* file whenever it can't find a corresponding file on disk.</span></span>

```
events { }
http {
    server {
        listen 80;

        location / {
            root /usr/share/nginx/html;
            try_files $uri $uri/ /Index.html =404;
        }
    }
}
```

<span data-ttu-id="fe67f-249">運用環境での Nginx Web サーバーの構成に関する詳細については、「[Creating NGINX Plus and NGINX Configuration Files](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/)」 (NGINX Plus と NGINX 構成ファイルの作成) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="fe67f-249">For more information on production Nginx web server configuration, see [Creating NGINX Plus and NGINX Configuration Files](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/).</span></span>

#### <a name="client-side-blazor-standalone-hosting-with-nginx-in-docker"></a><span data-ttu-id="fe67f-250">Docker で Nginx を使用したクライアント側 Blazor スタンドアロン ホスティング</span><span class="sxs-lookup"><span data-stu-id="fe67f-250">Client-side Blazor standalone hosting with Nginx in Docker</span></span>

<span data-ttu-id="fe67f-251">Nginx を使用して Docker で Blazor をホストするには、Alpine ベースの Nginx イメージを使用するように Dockerfile をセットアップします。</span><span class="sxs-lookup"><span data-stu-id="fe67f-251">To host Blazor in Docker using Nginx, setup the Dockerfile to use the Alpine-based Nginx image.</span></span> <span data-ttu-id="fe67f-252">*nginx.config* ファイルをコンテナーにコピーするように、Dockerfile を更新します。</span><span class="sxs-lookup"><span data-stu-id="fe67f-252">Update the Dockerfile to copy the *nginx.config* file into the container.</span></span>

<span data-ttu-id="fe67f-253">次の例に示すように、1 つの行を Dockerfile に追加します。</span><span class="sxs-lookup"><span data-stu-id="fe67f-253">Add one line to the Dockerfile, as shown in the following example:</span></span>

```
FROM nginx:alpine
COPY ./bin/Release/netstandard2.0/publish /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/nginx.conf
```

#### <a name="client-side-blazor-standalone-hosting-with-github-pages"></a><span data-ttu-id="fe67f-254">GitHub ページを使用したクライアント側 Blazor スタンドアロン ホスティング</span><span class="sxs-lookup"><span data-stu-id="fe67f-254">Client-side Blazor standalone hosting with GitHub Pages</span></span>

<span data-ttu-id="fe67f-255">URL の書き換えを処理するために、*404.html* ファイルを、要求を *index.html* ページにリダイレクトするスクリプトと共に追加します。</span><span class="sxs-lookup"><span data-stu-id="fe67f-255">To handle URL rewrites, add a *404.html* file with a script that handles redirecting the request to the *index.html* page.</span></span> <span data-ttu-id="fe67f-256">コミュニティが提供する実装例については、「[Single Page Apps for GitHub Pages](http://spa-github-pages.rafrex.com/)」(GitHub ページ用の単一ページ アプリ) ([rafrex/spa-github-pages on GitHub](https://github.com/rafrex/spa-github-pages#readme)) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="fe67f-256">For an example implementation provided by the community, see [Single Page Apps for GitHub Pages](http://spa-github-pages.rafrex.com/) ([rafrex/spa-github-pages on GitHub](https://github.com/rafrex/spa-github-pages#readme)).</span></span> <span data-ttu-id="fe67f-257">コミュニティのアプローチの使用例については、[GitHub 上の blazor-demo/blazor-demo.github.io に関するページ](https://github.com/blazor-demo/blazor-demo.github.io) ([ライブ サイト](https://blazor-demo.github.io/)) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="fe67f-257">An example using the community approach can be seen at [blazor-demo/blazor-demo.github.io on GitHub](https://github.com/blazor-demo/blazor-demo.github.io) ([live site](https://blazor-demo.github.io/)).</span></span>

<span data-ttu-id="fe67f-258">組織のサイトではなくプロジェクトのサイトを使用している場合、*index.html*内の `<base>` タグを追加または更新します。</span><span class="sxs-lookup"><span data-stu-id="fe67f-258">When using a project site instead of an organization site, add or update the `<base>` tag in *index.html*.</span></span> <span data-ttu-id="fe67f-259">`href` 属性値として `<repository-name>/` を設定します。ここで、`<repository-name>/` は GitHub のリポジトリ名です。</span><span class="sxs-lookup"><span data-stu-id="fe67f-259">Set the `href` attribute value to `<repository-name>/`, where `<repository-name>/` is the GitHub repository name.</span></span>

## <a name="deploy-a-server-side-razor-components-app"></a><span data-ttu-id="fe67f-260">サーバー側 Razor Components アプリの展開</span><span class="sxs-lookup"><span data-stu-id="fe67f-260">Deploy a server-side Razor Components app</span></span>

<span data-ttu-id="fe67f-261">[サーバー側のホスティング モデル](xref:razor-components/hosting-models#server-side-hosting-model)では、Razor Components はサーバー上で ASP.NET Core アプリ内から実行されます。</span><span class="sxs-lookup"><span data-stu-id="fe67f-261">With the [server-side hosting model](xref:razor-components/hosting-models#server-side-hosting-model), Razor Components is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="fe67f-262">UI の更新、イベント処理、JavaScript の呼び出しは、SignalR 接続経由で処理されます。</span><span class="sxs-lookup"><span data-stu-id="fe67f-262">UI updates, event handling, and JavaScript calls are handled over a SignalR connection.</span></span>

<span data-ttu-id="fe67f-263">アプリは、発行された出力に ASP.NET Core アプリと共に含まれているため、2 つのアプリを一緒に展開することができます。</span><span class="sxs-lookup"><span data-stu-id="fe67f-263">The app is included with the ASP.NET Core app in the published output so that the two apps are deployed together.</span></span> <span data-ttu-id="fe67f-264">ASP.NET Core アプリをホストできる Web サーバーが必要です。</span><span class="sxs-lookup"><span data-stu-id="fe67f-264">A web server that is capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="fe67f-265">サーバー側の展開の場合、Visual Studio には **Blazor (ASP.NET Core 内でサーバー側)** プロジェクト テンプレートが含まれています ([dotnet new](/dotnet/core/tools/dotnet-new) コマンドを使用する場合は `blazorserver` テンプレート)。</span><span class="sxs-lookup"><span data-stu-id="fe67f-265">For a server-side deployment, Visual Studio includes the **Blazor (Server-side in ASP.NET Core)** project template (`blazorserver` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

<!--

**INSERT: Concerns are the same as publishing an ASP.NET Core SignalR app**

**INSERT: Content on the Azure SignalR Service**

**INSERT: Manually turn on WebSockets support**

-->

<span data-ttu-id="fe67f-266">ASP.NET Core アプリが発行されると、ASP.NET Core アプリと Razor Components アプリを一緒に展開することができるように、発行された出力内に Razor コンポーネント アプリが含まれます。</span><span class="sxs-lookup"><span data-stu-id="fe67f-266">When the ASP.NET Core app is published, the Razor Components app is included in the published output so that the ASP.NET Core app and the Razor Components app can be deployed together.</span></span> <span data-ttu-id="fe67f-267">ASP.NET Core アプリでのホストと展開の詳細については、「<xref:host-and-deploy/index>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="fe67f-267">For more information on ASP.NET Core app hosting and deployment, see <xref:host-and-deploy/index>.</span></span>

<span data-ttu-id="fe67f-268">Azure App Service の展開については、次のトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="fe67f-268">For information on deploying to Azure App Service, see the following topics:</span></span>

<xref:tutorials/publish-to-azure-webapp-using-vs>  
<span data-ttu-id="fe67f-269">Visual Studio を使用して Azure App Service に ASP.NET Core アプリを発行する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="fe67f-269">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>
