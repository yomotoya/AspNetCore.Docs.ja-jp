---
uid: mvc/overview/deployment/docker
title: Windows コンテナーへの ASP.NET MVC アプリケーションの移行
description: 既存の ASP.NET MVC アプリケーションを取得し、Windows Docker コンテナーで実行する方法について説明します。
keywords: Windows Containers,Docker,ASP.NET MVC
author: BillWagner
ms.author: wiwagn
ms.date: 02/01/2017
ms.assetid: c9f1d52c-b4bd-4b5d-b7f9-8f9ceaf778c4
ms.openlocfilehash: 2e7b2a2e9d915aec0814def56daab860e1efa8af
ms.sourcegitcommit: 6e6002de467cd135a69e5518d4ba9422d693132a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/16/2018
ms.locfileid: "49348417"
---
# <a name="migrating-aspnet-mvc-applications-to-windows-containers"></a><span data-ttu-id="80e22-104">Windows コンテナーへの ASP.NET MVC アプリケーションの移行</span><span class="sxs-lookup"><span data-stu-id="80e22-104">Migrating ASP.NET MVC Applications to Windows Containers</span></span>

<span data-ttu-id="80e22-105">Windows コンテナーで既存の .NET Framework ベース アプリケーションを実行するとき、アプリに変更を加える必要はありません。</span><span class="sxs-lookup"><span data-stu-id="80e22-105">Running an existing .NET Framework-based application in a Windows container doesn't require any changes to your app.</span></span> <span data-ttu-id="80e22-106">Windows コンテナーでアプリを実行するには、アプリを含む Docker イメージを作成し、コンテナーを起動します。</span><span class="sxs-lookup"><span data-stu-id="80e22-106">To run your app in a Windows container you create a Docker image containing your app and start the container.</span></span> <span data-ttu-id="80e22-107">このトピックでは、既存の [ASP.NET MVC アプリケーション](http://www.asp.net/mvc)を取得し、Windows コンテナーに展開する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="80e22-107">This topic explains how to take an existing [ASP.NET MVC application](http://www.asp.net/mvc) and deploy it in a Windows container.</span></span>

<span data-ttu-id="80e22-108">既存の ASP.NET MVC アプリから開始し、Visual Studio を使用して発行した資産をビルドします。</span><span class="sxs-lookup"><span data-stu-id="80e22-108">You start with an existing ASP.NET MVC app, then build the published assets using Visual Studio.</span></span> <span data-ttu-id="80e22-109">Docker を使用し、アプリを含み、これを実行するイメージを作成します。</span><span class="sxs-lookup"><span data-stu-id="80e22-109">You use Docker to create the image that contains and runs your app.</span></span> <span data-ttu-id="80e22-110">Windows コンテナーで実行されているサイトに移動し、アプリが動作していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="80e22-110">You'll browse to the site running in a Windows container and verify the app is working.</span></span>

<span data-ttu-id="80e22-111">この記事を読むには、Docker の基本的な知識があることが前提となります。</span><span class="sxs-lookup"><span data-stu-id="80e22-111">This article assumes a basic understanding of Docker.</span></span> <span data-ttu-id="80e22-112">Docker の詳細については、「[Docker Overview](https://docs.docker.com/engine/understanding-docker/)」 (Docker の概要) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="80e22-112">You can learn about Docker by reading the [Docker Overview](https://docs.docker.com/engine/understanding-docker/).</span></span>

<span data-ttu-id="80e22-113">コンテナーで実行するアプリは、質問にランダムに回答する単純な Web サイトです。</span><span class="sxs-lookup"><span data-stu-id="80e22-113">The app you'll run in a container is a simple website that answers questions randomly.</span></span> <span data-ttu-id="80e22-114">このアプリは基本的な MVC アプリケーションであり、認証やデータベース ストレージはないため Web 層をコンテナーに移動することに集中できます。</span><span class="sxs-lookup"><span data-stu-id="80e22-114">This app is a basic MVC application with no authentication or database storage; it lets you focus on moving the web tier to a container.</span></span> <span data-ttu-id="80e22-115">今後のトピックで、コンテナー化されたアプリケーションに永続的な記憶域を移動して管理する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="80e22-115">Future topics will show how to move and manage persistent storage in containerized applications.</span></span>

<span data-ttu-id="80e22-116">アプリケーションを移行する手順は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="80e22-116">Moving your application involves these steps:</span></span>

1. [<span data-ttu-id="80e22-117">イメージの資産をビルドする発行タスクを作成します。</span><span class="sxs-lookup"><span data-stu-id="80e22-117">Creating a publish task to build the assets for an image.</span></span>](#publish-script)
1. [<span data-ttu-id="80e22-118">アプリケーションを実行する Docker イメージをビルドします。</span><span class="sxs-lookup"><span data-stu-id="80e22-118">Building a Docker image that will run your application.</span></span>](#build-the-image)
1. [<span data-ttu-id="80e22-119">イメージを実行する Docker コンテナーを開始します。</span><span class="sxs-lookup"><span data-stu-id="80e22-119">Starting a Docker container that runs your image.</span></span>](#start-a-container)
1. [<span data-ttu-id="80e22-120">ブラウザーを使用してアプリケーションを検証します。</span><span class="sxs-lookup"><span data-stu-id="80e22-120">Verifying the application using your browser.</span></span>](#verify-in-the-browser)

<span data-ttu-id="80e22-121">[完成したアプリケーション](https://github.com/dotnet/samples/tree/master/framework/docker/MVCRandomAnswerGenerator)が GitHub にあります。</span><span class="sxs-lookup"><span data-stu-id="80e22-121">The [finished application](https://github.com/dotnet/samples/tree/master/framework/docker/MVCRandomAnswerGenerator) is on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="80e22-122">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="80e22-122">Prerequisites</span></span>

<span data-ttu-id="80e22-123">開発用コンピューターでは、次のソフトウェアが必要です。</span><span class="sxs-lookup"><span data-stu-id="80e22-123">The development machine must have the following software:</span></span>

- <span data-ttu-id="80e22-124">[Windows 10 Anniversary Update](https://www.microsoft.com/software-download/windows10/) (またはそれ以降) または[Windows Server 2016](https://www.microsoft.com/cloud-platform/windows-server) (またはそれ以降)</span><span class="sxs-lookup"><span data-stu-id="80e22-124">[Windows 10 Anniversary Update](https://www.microsoft.com/software-download/windows10/) (or higher) or [Windows Server 2016](https://www.microsoft.com/cloud-platform/windows-server) (or higher)</span></span>
- <span data-ttu-id="80e22-125">[Docker for Windows](https://docs.docker.com/docker-for-windows/) - バージョン Stable 1.13.0 または 1.12 Beta 26 (以降のバージョン)</span><span class="sxs-lookup"><span data-stu-id="80e22-125">[Docker for Windows](https://docs.docker.com/docker-for-windows/) - version Stable 1.13.0 or 1.12 Beta 26 (or newer versions)</span></span>
- [<span data-ttu-id="80e22-126">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="80e22-126">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)

> [!IMPORTANT]
> <span data-ttu-id="80e22-127">Windows Server 2016 を使用している場合は、「[コンテナー ホストの展開 - Windows Server](https://msdn.microsoft.com/virtualization/windowscontainers/deployment/deployment)」の指示に従ってください。</span><span class="sxs-lookup"><span data-stu-id="80e22-127">If you are using Windows Server 2016, follow the instructions for [Container Host Deployment - Windows Server](https://msdn.microsoft.com/virtualization/windowscontainers/deployment/deployment).</span></span>

<span data-ttu-id="80e22-128">トレイ アイコンを選択します右クリックしてインストールし、Docker などの起動後**Windows コンテナーに切り替える**します。</span><span class="sxs-lookup"><span data-stu-id="80e22-128">After installing and starting Docker, right-click on the tray icon and select **Switch to Windows containers**.</span></span> <span data-ttu-id="80e22-129">これは Windows をベースに Docker イメージを実行するために必要です。</span><span class="sxs-lookup"><span data-stu-id="80e22-129">This is required to run Docker images based on Windows.</span></span> <span data-ttu-id="80e22-130">このコマンドの実行には数秒かかります。</span><span class="sxs-lookup"><span data-stu-id="80e22-130">This command takes a few seconds to execute:</span></span>

<span data-ttu-id="80e22-131">![Windows コンテナー][windows-container]</span><span class="sxs-lookup"><span data-stu-id="80e22-131">![Windows Container][windows-container]</span></span>

## <a name="publish-script"></a><span data-ttu-id="80e22-132">発行スクリプト</span><span class="sxs-lookup"><span data-stu-id="80e22-132">Publish script</span></span>

<span data-ttu-id="80e22-133">Docker イメージに読み込む必要があるすべての資産を 1 か所に集めます。</span><span class="sxs-lookup"><span data-stu-id="80e22-133">Collect all the assets that you need to load into a Docker image in one place.</span></span> <span data-ttu-id="80e22-134">Visual Studio の **[発行]** コマンドを使用してアプリの発行プロファイルを作成できます。</span><span class="sxs-lookup"><span data-stu-id="80e22-134">You can use the Visual Studio **Publish** command to create a publish profile for your app.</span></span> <span data-ttu-id="80e22-135">このプロファイルですべての資産を 1 つのディレクトリ ツリーに集めます。このチュートリアルの後半で、そのディレクトリ ツリーをターゲット イメージにコピーします。</span><span class="sxs-lookup"><span data-stu-id="80e22-135">This profile will put all the assets in one directory tree that you copy to your target image later in this tutorial.</span></span>

<span data-ttu-id="80e22-136">**発行の手順**</span><span class="sxs-lookup"><span data-stu-id="80e22-136">**Publish Steps**</span></span>

1. <span data-ttu-id="80e22-137">Visual Studio で Web プロジェクトを右クリックし、**[発行]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="80e22-137">Right click on the web project in Visual Studio, and select **Publish**.</span></span>
1. <span data-ttu-id="80e22-138">**[カスタム プロファイル]** ボタンをクリックし、方法として **[ファイル システム]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="80e22-138">Click the **Custom profile button**, and then select **File System** as the method.</span></span>
1. <span data-ttu-id="80e22-139">ディレクトリを選択します。</span><span class="sxs-lookup"><span data-stu-id="80e22-139">Choose the directory.</span></span> <span data-ttu-id="80e22-140">規則により、ダウンロードしたサンプルには `bin\Release\PublishOutput` が使用されます。</span><span class="sxs-lookup"><span data-stu-id="80e22-140">By convention, the downloaded sample uses `bin\Release\PublishOutput`.</span></span>

<span data-ttu-id="80e22-141">![接続の発行][publish-connection]</span><span class="sxs-lookup"><span data-stu-id="80e22-141">![Publish Connection][publish-connection]</span></span>

<span data-ttu-id="80e22-142">**[設定]** タブの **[ファイル発行オプション]** のセクションを開きます。**[発行中にプリコンパイルする]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="80e22-142">Open the **File Publish Options** section of the **Settings** tab. Select **Precompile during publishing**.</span></span> <span data-ttu-id="80e22-143">この最適化は、Docker コンテナー内のビューをコンパイルすることを意味し、プリコンパイル済みのビューをコピーすることになります。</span><span class="sxs-lookup"><span data-stu-id="80e22-143">This optimization means that you'll be compiling views in the Docker container, you are copying the precompiled views.</span></span>

<span data-ttu-id="80e22-144">![発行の設定][publish-settings]</span><span class="sxs-lookup"><span data-stu-id="80e22-144">![Publish Settings][publish-settings]</span></span>

<span data-ttu-id="80e22-145">**[発行]** をクリックすると、Visual Studio で必要なすべての資産がコピー先フォルダーにコピーされます。</span><span class="sxs-lookup"><span data-stu-id="80e22-145">Click **Publish**, and Visual Studio will copy all the needed assets to the destination folder.</span></span>

## <a name="build-the-image"></a><span data-ttu-id="80e22-146">イメージのビルド</span><span class="sxs-lookup"><span data-stu-id="80e22-146">Build the image</span></span>

<span data-ttu-id="80e22-147">Dockerfile に Docker イメージを定義します。</span><span class="sxs-lookup"><span data-stu-id="80e22-147">Define your Docker image in a Dockerfile.</span></span> <span data-ttu-id="80e22-148">Dockerfile は、基本イメージ、追加のコンポーネント、実行するアプリ、その他の構成イメージに関する指示を含みます。</span><span class="sxs-lookup"><span data-stu-id="80e22-148">The Dockerfile contains instructions for the base image, additional components, the app you want to run, and other configuration images.</span></span>  <span data-ttu-id="80e22-149">Dockerfile は、イメージを作成する `docker build` コマンドの入力です。</span><span class="sxs-lookup"><span data-stu-id="80e22-149">The Dockerfile is the input to the `docker build` command, which creates the image.</span></span>

<span data-ttu-id="80e22-150">[Docker Hub](https://hub.docker.com/r/microsoft/aspnet/) にある `microsoft/aspnet` イメージに基づいてイメージをビルドします。</span><span class="sxs-lookup"><span data-stu-id="80e22-150">You will build an image based on the `microsoft/aspnet` image located on [Docker Hub](https://hub.docker.com/r/microsoft/aspnet/).</span></span>
<span data-ttu-id="80e22-151">基本イメージである `microsoft/aspnet` は、Windows Server イメージです。</span><span class="sxs-lookup"><span data-stu-id="80e22-151">The base image, `microsoft/aspnet`, is a Windows Server image.</span></span> <span data-ttu-id="80e22-152">Windows Server Core、IIS と ASP.NET 4.6.2 が含まれています。</span><span class="sxs-lookup"><span data-stu-id="80e22-152">It contains Windows Server Core, IIS and ASP.NET 4.6.2.</span></span> <span data-ttu-id="80e22-153">このイメージをコンテナー内で実行すると、IIS とインストールされている Web サイトが自動的に起動します。</span><span class="sxs-lookup"><span data-stu-id="80e22-153">When you run this image in your container, it will automatically start IIS and installed websites.</span></span>

<span data-ttu-id="80e22-154">イメージを作成する Dockerfile は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="80e22-154">The Dockerfile that creates your image looks like this:</span></span>

```console
# The `FROM` instruction specifies the base image. You are
# extending the `microsoft/aspnet` image.

FROM microsoft/aspnet

# The final instruction copies the site you published earlier into the container.
COPY ./bin/Release/PublishOutput/ /inetpub/wwwroot
```

<span data-ttu-id="80e22-155">この Dockfile に `ENTRYPOINT` コマンドは使用されていません。</span><span class="sxs-lookup"><span data-stu-id="80e22-155">There is no `ENTRYPOINT` command in this Dockerfile.</span></span> <span data-ttu-id="80e22-156">使用する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="80e22-156">You don't need one.</span></span> <span data-ttu-id="80e22-157">IIS と Windows Server を実行するときに、IIS プロセスは、aspnet の基本イメージで起動するように構成がエントリ ポイントになります。</span><span class="sxs-lookup"><span data-stu-id="80e22-157">When running Windows Server with IIS, the IIS process is the entrypoint, which is configured to start in the aspnet base image.</span></span>

<span data-ttu-id="80e22-158">Docker ビルド コマンドを実行し、ASP.NET アプリを実行するイメージを作成します。</span><span class="sxs-lookup"><span data-stu-id="80e22-158">Run the Docker build command to create the image that runs your ASP.NET app.</span></span> <span data-ttu-id="80e22-159">これを行うには、プロジェクトのディレクトリで PowerShell ウィンドウを開くし、ソリューション ディレクトリで次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="80e22-159">To do this, open a PowerShell window in the directory of your project and type the following command in the solution directory:</span></span>

```console
docker build -t mvcrandomanswers .
```

<span data-ttu-id="80e22-160">このコマンドは、Dockerfile の手順に従って、新しいイメージをビルドの名前付け (-t がタグ付け) mvcrandomanswers としてイメージ。</span><span class="sxs-lookup"><span data-stu-id="80e22-160">This command will build the new image using the instructions in your Dockerfile, naming (-t tagging) the image as mvcrandomanswers.</span></span> <span data-ttu-id="80e22-161">その手順には、[Docker Hub](http://hub.docker.com) から基本イメージをプルし、それからそのイメージにアプリを追加する作業が含まれることがあります。</span><span class="sxs-lookup"><span data-stu-id="80e22-161">This may include pulling the base image from [Docker Hub](http://hub.docker.com), and then adding your app to that image.</span></span>

<span data-ttu-id="80e22-162">そのコマンドの完了後、`docker images` コマンドを実行して新しいイメージの情報を参照できます。</span><span class="sxs-lookup"><span data-stu-id="80e22-162">Once that command completes, you can run the `docker images` command to see information on the new image:</span></span>

```console
REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
mvcrandomanswers              latest              86838648aab6        2 minutes ago       10.1 GB
```

<span data-ttu-id="80e22-163">IMAGE ID はコンピューターによって異なります。</span><span class="sxs-lookup"><span data-stu-id="80e22-163">The IMAGE ID will be different on your machine.</span></span> <span data-ttu-id="80e22-164">では、アプリを実行しましょう。</span><span class="sxs-lookup"><span data-stu-id="80e22-164">Now, let's run the app.</span></span>

## <a name="start-a-container"></a><span data-ttu-id="80e22-165">コンテナーの開始</span><span class="sxs-lookup"><span data-stu-id="80e22-165">Start a container</span></span>

<span data-ttu-id="80e22-166">コンテナーを開始するには、次の `docker run` コマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="80e22-166">Start a container by executing the following `docker run` command:</span></span>

```console
docker run -d --name randomanswers mvcrandomanswers
```

<span data-ttu-id="80e22-167">`-d` 引数は、デタッチ モードでイメージを開始するよう Docker に指示します。</span><span class="sxs-lookup"><span data-stu-id="80e22-167">The `-d` argument tells Docker to start the image in detached mode.</span></span> <span data-ttu-id="80e22-168">つまり、Docker イメージは現在のシェルから切断された状態で実行されます。</span><span class="sxs-lookup"><span data-stu-id="80e22-168">That means the Docker image runs disconnected from the current shell.</span></span>

<span data-ttu-id="80e22-169">多くの docker の例では、コンテナーとホストのポートにマップする-p を参照してください可能性があります。</span><span class="sxs-lookup"><span data-stu-id="80e22-169">In many docker examples, you may see -p to map the container and host ports.</span></span> <span data-ttu-id="80e22-170">既定の aspnet イメージでは、ポート 80 でリッスンし、それを公開するコンテナーが既に構成します。</span><span class="sxs-lookup"><span data-stu-id="80e22-170">The default aspnet image has already configured the container to listen on port 80 and expose it.</span></span>

<span data-ttu-id="80e22-171">`--name randomanswers` は、実行するコンテナーに名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="80e22-171">The `--name randomanswers` gives a name to the running container.</span></span> <span data-ttu-id="80e22-172">この名前は、ほとんどのコマンドでコンテナー ID の代わりに使用できます。</span><span class="sxs-lookup"><span data-stu-id="80e22-172">You can use this name instead of the container ID in most commands.</span></span>

<span data-ttu-id="80e22-173">`mvcrandomanswers` は、開始するイメージの名前です。</span><span class="sxs-lookup"><span data-stu-id="80e22-173">The `mvcrandomanswers` is the name of the image to start.</span></span>

## <a name="verify-in-the-browser"></a><span data-ttu-id="80e22-174">ブラウザーでの確認</span><span class="sxs-lookup"><span data-stu-id="80e22-174">Verify in the browser</span></span>

> [!NOTE]
> <span data-ttu-id="80e22-175">Windows コンテナーの現在のリリースを参照できない`http://localhost`します。</span><span class="sxs-lookup"><span data-stu-id="80e22-175">With the current Windows Container release, you can't browse to `http://localhost`.</span></span>
> <span data-ttu-id="80e22-176">これは WinNAT での既知の動作によるものであり、将来のリリースで修正される予定です。</span><span class="sxs-lookup"><span data-stu-id="80e22-176">This is a known behavior in WinNAT, and it will be resolved in the future.</span></span> <span data-ttu-id="80e22-177">解決されるまでは、コンテナーの IP アドレスを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="80e22-177">Until that is addressed, you need to use the IP address of the container.</span></span>

<span data-ttu-id="80e22-178">コンテナーの開始後、実行中のコンテナーにブラウザーから接続できるように、コンテナーの IP アドレスを見つけます。</span><span class="sxs-lookup"><span data-stu-id="80e22-178">Once the container starts, find its IP address so that you can connect to your running container from a browser:</span></span>

```console
docker inspect -f "{{ .NetworkSettings.Networks.nat.IPAddress }}" randomanswers
172.31.194.61
```

<span data-ttu-id="80e22-179">IPv4 アドレスを使用して、実行中のコンテナーに接続する`http://172.31.194.61`に示す例です。</span><span class="sxs-lookup"><span data-stu-id="80e22-179">Connect to the running container using the IPv4 address, `http://172.31.194.61` in the example shown.</span></span> <span data-ttu-id="80e22-180">その URL をブラウザーに入力すると、実行中のサイトが表示されます。</span><span class="sxs-lookup"><span data-stu-id="80e22-180">Type that URL into your browser, and you should see the running site.</span></span>

> [!NOTE]
> <span data-ttu-id="80e22-181">一部の VPN またはプロキシ ソフトウェアが原因でサイトに移動できない場合があります。</span><span class="sxs-lookup"><span data-stu-id="80e22-181">Some VPN or proxy software may prevent you from navigating to your site.</span></span>
> <span data-ttu-id="80e22-182">コンテナーが動作していることを確認するために、それらを一時的に無効にできます。</span><span class="sxs-lookup"><span data-stu-id="80e22-182">You can temporarily disable it to make sure your container is working.</span></span>

<span data-ttu-id="80e22-183">GitHub のサンプル ディレクトリに含まれる [PowerShell スクリプト](https://github.com/dotnet/docs/tree/master/samples/framework/docker/MVCRandomAnswerGenerator/run.ps1)は、これらのコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="80e22-183">The sample directory on GitHub contains a [PowerShell script](https://github.com/dotnet/docs/tree/master/samples/framework/docker/MVCRandomAnswerGenerator/run.ps1) that executes these commands for you.</span></span> <span data-ttu-id="80e22-184">PowerShell ウィンドウを開き、ソリューション ディレクトリに移動して、次のように入力します。</span><span class="sxs-lookup"><span data-stu-id="80e22-184">Open a PowerShell window, change directory to your solution directory, and type:</span></span>

```console
./run.ps1
```

<span data-ttu-id="80e22-185">上のコマンドによりイメージをビルドし、コンピューターにイメージの一覧を表示して、コンテナーを開始した後、そのコンテナーの IP アドレスを表示します。</span><span class="sxs-lookup"><span data-stu-id="80e22-185">The command above builds the image, displays the list of images on your machine, starts a container, and displays the IP address for that container.</span></span>

<span data-ttu-id="80e22-186">コンテナーを停止するには、`docker
stop` コマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="80e22-186">To stop your container, issue a `docker
stop` command:</span></span>

```console
docker stop randomanswers
```

<span data-ttu-id="80e22-187">コンテナーを削除するには、`docker rm` コマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="80e22-187">To remove the container, issue a `docker rm` command:</span></span>

```console
docker rm randomanswers
```

[windows-container]: media/aspnetmvc/SwitchContainer.png "Windows コンテナーへの切り替え"
[publish-connection]: media/aspnetmvc/PublishConnection.png "ファイル システムへの発行"
[publish-settings]: media/aspnetmvc/PublishSettings.png "発行の設定"
