---
uid: mvc/overview/deployment/docker
title: Windows コンテナーへの ASP.NET MVC アプリケーションの移行
description: 既存の ASP.NET MVC アプリケーションを取得し、Windows Docker コンテナーで実行する方法について説明します。
keywords: Windows Containers,Docker,ASP.NET MVC
author: BillWagner
ms.author: wiwagn
ms.date: 12/14/2018
ms.assetid: c9f1d52c-b4bd-4b5d-b7f9-8f9ceaf778c4
ms.openlocfilehash: ef184f4256c20e2a66de8fd2d4f8e67f07d9a086
ms.sourcegitcommit: 6548c19f345850ee22b50f7ef9fca732895d9e08
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/14/2018
ms.locfileid: "53425160"
---
# <a name="migrating-aspnet-mvc-applications-to-windows-containers"></a><span data-ttu-id="72e8a-104">Windows コンテナーへの ASP.NET MVC アプリケーションの移行</span><span class="sxs-lookup"><span data-stu-id="72e8a-104">Migrating ASP.NET MVC Applications to Windows Containers</span></span>

<span data-ttu-id="72e8a-105">Windows コンテナーで既存の .NET Framework ベース アプリケーションを実行するとき、アプリに変更を加える必要はありません。</span><span class="sxs-lookup"><span data-stu-id="72e8a-105">Running an existing .NET Framework-based application in a Windows container doesn't require any changes to your app.</span></span> <span data-ttu-id="72e8a-106">Windows コンテナーでアプリを実行するには、アプリを含む Docker イメージを作成し、コンテナーを起動します。</span><span class="sxs-lookup"><span data-stu-id="72e8a-106">To run your app in a Windows container you create a Docker image containing your app and start the container.</span></span> <span data-ttu-id="72e8a-107">このトピックでは、既存の [ASP.NET MVC アプリケーション](http://www.asp.net/mvc)を取得し、Windows コンテナーに展開する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="72e8a-107">This topic explains how to take an existing [ASP.NET MVC application](http://www.asp.net/mvc) and deploy it in a Windows container.</span></span>

<span data-ttu-id="72e8a-108">既存の ASP.NET MVC アプリから開始し、Visual Studio を使用して発行した資産をビルドします。</span><span class="sxs-lookup"><span data-stu-id="72e8a-108">You start with an existing ASP.NET MVC app, then build the published assets using Visual Studio.</span></span> <span data-ttu-id="72e8a-109">Docker を使用し、アプリを含み、これを実行するイメージを作成します。</span><span class="sxs-lookup"><span data-stu-id="72e8a-109">You use Docker to create the image that contains and runs your app.</span></span> <span data-ttu-id="72e8a-110">Windows コンテナーで実行されているサイトに移動し、アプリが動作していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="72e8a-110">You'll browse to the site running in a Windows container and verify the app is working.</span></span>

<span data-ttu-id="72e8a-111">この記事を読むには、Docker の基本的な知識があることが前提となります。</span><span class="sxs-lookup"><span data-stu-id="72e8a-111">This article assumes a basic understanding of Docker.</span></span> <span data-ttu-id="72e8a-112">Docker の詳細については、「[Docker Overview](https://docs.docker.com/engine/understanding-docker/)」 (Docker の概要) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="72e8a-112">You can learn about Docker by reading the [Docker Overview](https://docs.docker.com/engine/understanding-docker/).</span></span>

<span data-ttu-id="72e8a-113">コンテナーで実行するアプリは、質問にランダムに回答する単純な Web サイトです。</span><span class="sxs-lookup"><span data-stu-id="72e8a-113">The app you'll run in a container is a simple website that answers questions randomly.</span></span> <span data-ttu-id="72e8a-114">このアプリは基本的な MVC アプリケーションであり、認証やデータベース ストレージはないため Web 層をコンテナーに移動することに集中できます。</span><span class="sxs-lookup"><span data-stu-id="72e8a-114">This app is a basic MVC application with no authentication or database storage; it lets you focus on moving the web tier to a container.</span></span> <span data-ttu-id="72e8a-115">今後のトピックで、コンテナー化されたアプリケーションに永続的な記憶域を移動して管理する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="72e8a-115">Future topics will show how to move and manage persistent storage in containerized applications.</span></span>

<span data-ttu-id="72e8a-116">アプリケーションを移行する手順は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="72e8a-116">Moving your application involves these steps:</span></span>

1. [<span data-ttu-id="72e8a-117">イメージの資産をビルドする発行タスクを作成します。</span><span class="sxs-lookup"><span data-stu-id="72e8a-117">Creating a publish task to build the assets for an image.</span></span>](#publish-script)
1. [<span data-ttu-id="72e8a-118">アプリケーションを実行する Docker イメージをビルドします。</span><span class="sxs-lookup"><span data-stu-id="72e8a-118">Building a Docker image that will run your application.</span></span>](#build-the-image)
1. [<span data-ttu-id="72e8a-119">イメージを実行する Docker コンテナーを開始します。</span><span class="sxs-lookup"><span data-stu-id="72e8a-119">Starting a Docker container that runs your image.</span></span>](#start-a-container)
1. [<span data-ttu-id="72e8a-120">ブラウザーを使用してアプリケーションを検証します。</span><span class="sxs-lookup"><span data-stu-id="72e8a-120">Verifying the application using your browser.</span></span>](#verify-in-the-browser)

<span data-ttu-id="72e8a-121">[完成したアプリケーション](https://github.com/dotnet/samples/tree/master/framework/docker/MVCRandomAnswerGenerator)が GitHub にあります。</span><span class="sxs-lookup"><span data-stu-id="72e8a-121">The [finished application](https://github.com/dotnet/samples/tree/master/framework/docker/MVCRandomAnswerGenerator) is on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="72e8a-122">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="72e8a-122">Prerequisites</span></span>

<span data-ttu-id="72e8a-123">開発用コンピューターでは、次のソフトウェアが必要です。</span><span class="sxs-lookup"><span data-stu-id="72e8a-123">The development machine must have the following software:</span></span>

- <span data-ttu-id="72e8a-124">[Windows 10 Anniversary Update](https://www.microsoft.com/software-download/windows10/) (またはそれ以降) または[Windows Server 2016](https://www.microsoft.com/cloud-platform/windows-server) (またはそれ以降)</span><span class="sxs-lookup"><span data-stu-id="72e8a-124">[Windows 10 Anniversary Update](https://www.microsoft.com/software-download/windows10/) (or higher) or [Windows Server 2016](https://www.microsoft.com/cloud-platform/windows-server) (or higher)</span></span>
- <span data-ttu-id="72e8a-125">[Docker for Windows](https://docs.docker.com/docker-for-windows/) - バージョン Stable 1.13.0 または 1.12 Beta 26 (以降のバージョン)</span><span class="sxs-lookup"><span data-stu-id="72e8a-125">[Docker for Windows](https://docs.docker.com/docker-for-windows/) - version Stable 1.13.0 or 1.12 Beta 26 (or newer versions)</span></span>
- [<span data-ttu-id="72e8a-126">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="72e8a-126">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)

> [!IMPORTANT]
> <span data-ttu-id="72e8a-127">Windows Server 2016 を使用している場合は、「[コンテナー ホストの展開 - Windows Server](https://msdn.microsoft.com/virtualization/windowscontainers/deployment/deployment)」の指示に従ってください。</span><span class="sxs-lookup"><span data-stu-id="72e8a-127">If you are using Windows Server 2016, follow the instructions for [Container Host Deployment - Windows Server](https://msdn.microsoft.com/virtualization/windowscontainers/deployment/deployment).</span></span>

<span data-ttu-id="72e8a-128">トレイ アイコンを選択します右クリックしてインストールし、Docker などの起動後**Windows コンテナーに切り替える**します。</span><span class="sxs-lookup"><span data-stu-id="72e8a-128">After installing and starting Docker, right-click on the tray icon and select **Switch to Windows containers**.</span></span> <span data-ttu-id="72e8a-129">これは Windows をベースに Docker イメージを実行するために必要です。</span><span class="sxs-lookup"><span data-stu-id="72e8a-129">This is required to run Docker images based on Windows.</span></span> <span data-ttu-id="72e8a-130">このコマンドの実行には数秒かかります。</span><span class="sxs-lookup"><span data-stu-id="72e8a-130">This command takes a few seconds to execute:</span></span>

<span data-ttu-id="72e8a-131">![Windows コンテナー][windows-container]</span><span class="sxs-lookup"><span data-stu-id="72e8a-131">![Windows Container][windows-container]</span></span>

## <a name="publish-script"></a><span data-ttu-id="72e8a-132">発行スクリプト</span><span class="sxs-lookup"><span data-stu-id="72e8a-132">Publish script</span></span>

<span data-ttu-id="72e8a-133">Docker イメージに読み込む必要があるすべての資産を 1 か所に集めます。</span><span class="sxs-lookup"><span data-stu-id="72e8a-133">Collect all the assets that you need to load into a Docker image in one place.</span></span> <span data-ttu-id="72e8a-134">Visual Studio の **[発行]** コマンドを使用してアプリの発行プロファイルを作成できます。</span><span class="sxs-lookup"><span data-stu-id="72e8a-134">You can use the Visual Studio **Publish** command to create a publish profile for your app.</span></span> <span data-ttu-id="72e8a-135">このプロファイルですべての資産を 1 つのディレクトリ ツリーに集めます。このチュートリアルの後半で、そのディレクトリ ツリーをターゲット イメージにコピーします。</span><span class="sxs-lookup"><span data-stu-id="72e8a-135">This profile will put all the assets in one directory tree that you copy to your target image later in this tutorial.</span></span>

<span data-ttu-id="72e8a-136">**発行の手順**</span><span class="sxs-lookup"><span data-stu-id="72e8a-136">**Publish Steps**</span></span>

1. <span data-ttu-id="72e8a-137">Visual Studio で Web プロジェクトを右クリックし、**[発行]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="72e8a-137">Right click on the web project in Visual Studio, and select **Publish**.</span></span>
1. <span data-ttu-id="72e8a-138">**[カスタム プロファイル]** ボタンをクリックし、方法として **[ファイル システム]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="72e8a-138">Click the **Custom profile button**, and then select **File System** as the method.</span></span>
1. <span data-ttu-id="72e8a-139">ディレクトリを選択します。</span><span class="sxs-lookup"><span data-stu-id="72e8a-139">Choose the directory.</span></span> <span data-ttu-id="72e8a-140">規則により、ダウンロードしたサンプルには `bin\Release\PublishOutput` が使用されます。</span><span class="sxs-lookup"><span data-stu-id="72e8a-140">By convention, the downloaded sample uses `bin\Release\PublishOutput`.</span></span>

<span data-ttu-id="72e8a-141">![接続の発行][publish-connection]</span><span class="sxs-lookup"><span data-stu-id="72e8a-141">![Publish Connection][publish-connection]</span></span>

<span data-ttu-id="72e8a-142">**[設定]** タブの **[ファイル発行オプション]** のセクションを開きます。**[発行中にプリコンパイルする]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="72e8a-142">Open the **File Publish Options** section of the **Settings** tab. Select **Precompile during publishing**.</span></span> <span data-ttu-id="72e8a-143">この最適化は、Docker コンテナー内のビューをコンパイルすることを意味し、プリコンパイル済みのビューをコピーすることになります。</span><span class="sxs-lookup"><span data-stu-id="72e8a-143">This optimization means that you'll be compiling views in the Docker container, you are copying the precompiled views.</span></span>

<span data-ttu-id="72e8a-144">![発行の設定][publish-settings]</span><span class="sxs-lookup"><span data-stu-id="72e8a-144">![Publish Settings][publish-settings]</span></span>

<span data-ttu-id="72e8a-145">**[発行]** をクリックすると、Visual Studio で必要なすべての資産がコピー先フォルダーにコピーされます。</span><span class="sxs-lookup"><span data-stu-id="72e8a-145">Click **Publish**, and Visual Studio will copy all the needed assets to the destination folder.</span></span>

## <a name="build-the-image"></a><span data-ttu-id="72e8a-146">イメージのビルド</span><span class="sxs-lookup"><span data-stu-id="72e8a-146">Build the image</span></span>

<span data-ttu-id="72e8a-147">という名前の新しいファイルを作成する*Dockerfile* Docker イメージを定義します。</span><span class="sxs-lookup"><span data-stu-id="72e8a-147">Create a new file named *Dockerfile* to define your Docker image.</span></span> <span data-ttu-id="72e8a-148">*Dockerfile*最終的なイメージを作成する手順について説明し、任意の基本イメージ名、必要なコンポーネントを実行するアプリおよびその他の構成イメージが含まれています。</span><span class="sxs-lookup"><span data-stu-id="72e8a-148">*Dockerfile* contains instructions to build the final image and includes any base image names, required components, the app you want to run, and other configuration images.</span></span> <span data-ttu-id="72e8a-149">*Dockerfile*への入力には、`docker build`イメージを作成するコマンド。</span><span class="sxs-lookup"><span data-stu-id="72e8a-149">*Dockerfile* is the input to the `docker build` command that creates the image.</span></span>

<span data-ttu-id="72e8a-150">この練習に基づくイメージを作成、`microsoft/aspnet`上にあるイメージ[Docker Hub](https://hub.docker.com/r/microsoft/aspnet/)します。</span><span class="sxs-lookup"><span data-stu-id="72e8a-150">For this exercise, you will build an image based on the `microsoft/aspnet` image located on [Docker Hub](https://hub.docker.com/r/microsoft/aspnet/).</span></span>
<span data-ttu-id="72e8a-151">基本イメージである `microsoft/aspnet` は、Windows Server イメージです。</span><span class="sxs-lookup"><span data-stu-id="72e8a-151">The base image, `microsoft/aspnet`, is a Windows Server image.</span></span> <span data-ttu-id="72e8a-152">Windows Server Core、IIS、および 4.7.2 の ASP.NET が含まれています。</span><span class="sxs-lookup"><span data-stu-id="72e8a-152">It contains Windows Server Core, IIS, and ASP.NET 4.7.2.</span></span> <span data-ttu-id="72e8a-153">このイメージをコンテナー内で実行すると、IIS とインストールされている Web サイトが自動的に起動します。</span><span class="sxs-lookup"><span data-stu-id="72e8a-153">When you run this image in your container, it will automatically start IIS and installed websites.</span></span>

<span data-ttu-id="72e8a-154">イメージを作成する Dockerfile は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="72e8a-154">The Dockerfile that creates your image looks like this:</span></span>

```console
# The `FROM` instruction specifies the base image. You are
# extending the `microsoft/aspnet` image.

FROM microsoft/aspnet

# The final instruction copies the site you published earlier into the container.
COPY ./bin/Release/PublishOutput/ /inetpub/wwwroot
```

<span data-ttu-id="72e8a-155">この Dockfile に `ENTRYPOINT` コマンドは使用されていません。</span><span class="sxs-lookup"><span data-stu-id="72e8a-155">There is no `ENTRYPOINT` command in this Dockerfile.</span></span> <span data-ttu-id="72e8a-156">使用する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="72e8a-156">You don't need one.</span></span> <span data-ttu-id="72e8a-157">IIS と Windows Server を実行するときに、IIS プロセスは、aspnet の基本イメージで起動するように構成がエントリ ポイントになります。</span><span class="sxs-lookup"><span data-stu-id="72e8a-157">When running Windows Server with IIS, the IIS process is the entrypoint, which is configured to start in the aspnet base image.</span></span>

<span data-ttu-id="72e8a-158">Docker ビルド コマンドを実行し、ASP.NET アプリを実行するイメージを作成します。</span><span class="sxs-lookup"><span data-stu-id="72e8a-158">Run the Docker build command to create the image that runs your ASP.NET app.</span></span> <span data-ttu-id="72e8a-159">これを行うには、プロジェクトのディレクトリで PowerShell ウィンドウを開くし、ソリューション ディレクトリで次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="72e8a-159">To do this, open a PowerShell window in the directory of your project and type the following command in the solution directory:</span></span>

```console
docker build -t mvcrandomanswers .
```

<span data-ttu-id="72e8a-160">このコマンドは、Dockerfile の手順に従って、新しいイメージをビルドの名前付け (-t がタグ付け) mvcrandomanswers としてイメージ。</span><span class="sxs-lookup"><span data-stu-id="72e8a-160">This command will build the new image using the instructions in your Dockerfile, naming (-t tagging) the image as mvcrandomanswers.</span></span> <span data-ttu-id="72e8a-161">その手順には、[Docker Hub](http://hub.docker.com) から基本イメージをプルし、それからそのイメージにアプリを追加する作業が含まれることがあります。</span><span class="sxs-lookup"><span data-stu-id="72e8a-161">This may include pulling the base image from [Docker Hub](http://hub.docker.com), and then adding your app to that image.</span></span>

<span data-ttu-id="72e8a-162">そのコマンドの完了後、`docker images` コマンドを実行して新しいイメージの情報を参照できます。</span><span class="sxs-lookup"><span data-stu-id="72e8a-162">Once that command completes, you can run the `docker images` command to see information on the new image:</span></span>

```console
REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
mvcrandomanswers              latest              86838648aab6        2 minutes ago       10.1 GB
```

<span data-ttu-id="72e8a-163">IMAGE ID はコンピューターによって異なります。</span><span class="sxs-lookup"><span data-stu-id="72e8a-163">The IMAGE ID will be different on your machine.</span></span> <span data-ttu-id="72e8a-164">では、アプリを実行しましょう。</span><span class="sxs-lookup"><span data-stu-id="72e8a-164">Now, let's run the app.</span></span>

## <a name="start-a-container"></a><span data-ttu-id="72e8a-165">コンテナーの開始</span><span class="sxs-lookup"><span data-stu-id="72e8a-165">Start a container</span></span>

<span data-ttu-id="72e8a-166">コンテナーを開始するには、次の `docker run` コマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="72e8a-166">Start a container by executing the following `docker run` command:</span></span>

```console
docker run -d --name randomanswers mvcrandomanswers
```

<span data-ttu-id="72e8a-167">`-d` 引数は、デタッチ モードでイメージを開始するよう Docker に指示します。</span><span class="sxs-lookup"><span data-stu-id="72e8a-167">The `-d` argument tells Docker to start the image in detached mode.</span></span> <span data-ttu-id="72e8a-168">つまり、Docker イメージは現在のシェルから切断された状態で実行されます。</span><span class="sxs-lookup"><span data-stu-id="72e8a-168">That means the Docker image runs disconnected from the current shell.</span></span>

<span data-ttu-id="72e8a-169">多くの docker の例では、コンテナーとホストのポートにマップする-p を参照してください可能性があります。</span><span class="sxs-lookup"><span data-stu-id="72e8a-169">In many docker examples, you may see -p to map the container and host ports.</span></span> <span data-ttu-id="72e8a-170">既定の aspnet イメージでは、ポート 80 でリッスンし、それを公開するコンテナーが既に構成します。</span><span class="sxs-lookup"><span data-stu-id="72e8a-170">The default aspnet image has already configured the container to listen on port 80 and expose it.</span></span>

<span data-ttu-id="72e8a-171">`--name randomanswers` は、実行するコンテナーに名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="72e8a-171">The `--name randomanswers` gives a name to the running container.</span></span> <span data-ttu-id="72e8a-172">この名前は、ほとんどのコマンドでコンテナー ID の代わりに使用できます。</span><span class="sxs-lookup"><span data-stu-id="72e8a-172">You can use this name instead of the container ID in most commands.</span></span>

<span data-ttu-id="72e8a-173">`mvcrandomanswers` は、開始するイメージの名前です。</span><span class="sxs-lookup"><span data-stu-id="72e8a-173">The `mvcrandomanswers` is the name of the image to start.</span></span>

## <a name="verify-in-the-browser"></a><span data-ttu-id="72e8a-174">ブラウザーでの確認</span><span class="sxs-lookup"><span data-stu-id="72e8a-174">Verify in the browser</span></span>

<span data-ttu-id="72e8a-175">コンテナーが起動、実行中のコンテナーを使用する接続`http://localhost`に示す例です。</span><span class="sxs-lookup"><span data-stu-id="72e8a-175">Once the container starts, connect to the running container using `http://localhost` in the example shown.</span></span> <span data-ttu-id="72e8a-176">その URL をブラウザーに入力すると、実行中のサイトが表示されます。</span><span class="sxs-lookup"><span data-stu-id="72e8a-176">Type that URL into your browser, and you should see the running site.</span></span>

> [!NOTE]
> <span data-ttu-id="72e8a-177">一部の VPN またはプロキシ ソフトウェアが原因でサイトに移動できない場合があります。</span><span class="sxs-lookup"><span data-stu-id="72e8a-177">Some VPN or proxy software may prevent you from navigating to your site.</span></span>
> <span data-ttu-id="72e8a-178">コンテナーが動作していることを確認するために、それらを一時的に無効にできます。</span><span class="sxs-lookup"><span data-stu-id="72e8a-178">You can temporarily disable it to make sure your container is working.</span></span>

<span data-ttu-id="72e8a-179">GitHub のサンプル ディレクトリに含まれる [PowerShell スクリプト](https://github.com/dotnet/samples/blob/master/framework/docker/MVCRandomAnswerGenerator/run.ps1)は、これらのコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="72e8a-179">The sample directory on GitHub contains a [PowerShell script](https://github.com/dotnet/samples/blob/master/framework/docker/MVCRandomAnswerGenerator/run.ps1) that executes these commands for you.</span></span> <span data-ttu-id="72e8a-180">PowerShell ウィンドウを開き、ソリューション ディレクトリに移動して、次のように入力します。</span><span class="sxs-lookup"><span data-stu-id="72e8a-180">Open a PowerShell window, change directory to your solution directory, and type:</span></span>

```console
./run.ps1
```

<span data-ttu-id="72e8a-181">上記のコマンドは、イメージをビルド、コンピューターにイメージの一覧を表示し、コンテナーを開始します。</span><span class="sxs-lookup"><span data-stu-id="72e8a-181">The command above builds the image, displays the list of images on your machine, and starts a container.</span></span>

<span data-ttu-id="72e8a-182">コンテナーを停止するには、`docker stop` コマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="72e8a-182">To stop your container, issue a `docker stop` command:</span></span>

```console
docker stop randomanswers
```

<span data-ttu-id="72e8a-183">コンテナーを削除するには、`docker rm` コマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="72e8a-183">To remove the container, issue a `docker rm` command:</span></span>

```console
docker rm randomanswers
```

[windows-container]: media/aspnetmvc/SwitchContainer.png "Windows コンテナーへの切り替え"
[publish-connection]: media/aspnetmvc/PublishConnection.png "ファイル システムへの発行"
[publish-settings]: media/aspnetmvc/PublishSettings.png "発行の設定"
