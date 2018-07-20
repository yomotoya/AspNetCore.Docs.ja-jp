---
title: Visual Studio Tools for Docker と ASP.NET Core
author: spboyer
description: Visual Studio 2017 ツールと Docker for Windows を使用して、ASP.NET Core アプリをコンテナー化する方法を説明します。
ms.author: scaddie
ms.custom: mvc
ms.date: 12/12/2017
uid: host-and-deploy/docker/visual-studio-tools-for-docker
ms.openlocfilehash: fd485416ff0fab2508ab8ffd3f0ad309be338723
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276855"
---
# <a name="visual-studio-tools-for-docker-with-aspnet-core"></a><span data-ttu-id="ff3f9-103">Visual Studio Tools for Docker と ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ff3f9-103">Visual Studio Tools for Docker with ASP.NET Core</span></span>

<span data-ttu-id="ff3f9-104">[Visual Studio 2017](https://www.visualstudio.com/) は、.NET Core をターゲットとするコンテナー化された ASP.NET Core アプリのビルド、デバッグ、実行をサポートします。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-104">[Visual Studio 2017](https://www.visualstudio.com/) supports building, debugging, and running containerized ASP.NET Core apps targeting .NET Core.</span></span> <span data-ttu-id="ff3f9-105">Windows と Linux の両方のコンテナーがサポートされます。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-105">Both Windows and Linux containers are supported.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ff3f9-106">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="ff3f9-106">Prerequisites</span></span>

* <span data-ttu-id="ff3f9-107">[Visual Studio 2017](https://www.visualstudio.com/) と **[.NET Core クロスプラットフォームの開発]** ワークロード</span><span class="sxs-lookup"><span data-stu-id="ff3f9-107">[Visual Studio 2017](https://www.visualstudio.com/) with the **.NET Core cross-platform development** workload</span></span>
* [<span data-ttu-id="ff3f9-108">Docker for Windows</span><span class="sxs-lookup"><span data-stu-id="ff3f9-108">Docker for Windows</span></span>](https://docs.docker.com/docker-for-windows/install/)

## <a name="installation-and-setup"></a><span data-ttu-id="ff3f9-109">インストールとセットアップ</span><span class="sxs-lookup"><span data-stu-id="ff3f9-109">Installation and setup</span></span>

<span data-ttu-id="ff3f9-110">Docker をインストールする場合、「[Docker for Windows: What to know before you install](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install)」 (Docker for Windows: インストール前に知っておくべきこと) の情報を確認し、[Docker for Windows](https://docs.docker.com/docker-for-windows/install/) をインストールします。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-110">For Docker installation, review the information at [Docker for Windows: What to know before you install](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install) and install [Docker For Windows](https://docs.docker.com/docker-for-windows/install/).</span></span>

<span data-ttu-id="ff3f9-111">ボリュームのマッピングとデバッグをサポートするように、Docker for Windows で**[共有ドライブ](https://docs.docker.com/docker-for-windows/#shared-drives)** を構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-111">**[Shared Drives](https://docs.docker.com/docker-for-windows/#shared-drives)** in Docker for Windows must be configured to support volume mapping and debugging.</span></span> <span data-ttu-id="ff3f9-112">システム トレイの Docker アイコンを右クリックして **[設定]** をクリックし、**[Shared Drives]\(共有ドライブ\)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-112">Right-click the System Tray's Docker icon, select **Settings...**, and select **Shared Drives**.</span></span> <span data-ttu-id="ff3f9-113">Docker がファイルを保存するドライブを選択します。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-113">Select the drive where Docker stores files.</span></span> <span data-ttu-id="ff3f9-114">**[適用]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-114">Select **Apply**.</span></span>

![共有ドライブ](./visual-studio-tools-for-docker/_static/settings-shared-drives-win.png)

> [!TIP]
> <span data-ttu-id="ff3f9-116">Visual Studio 2017 バージョン 15.6 以降では、**共有ドライブ**が構成されていない場合にプロンプトが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-116">Visual Studio 2017 versions 15.6 and later prompt when **Shared Drives** aren't configured.</span></span>

## <a name="add-docker-support-to-an-app"></a><span data-ttu-id="ff3f9-117">アプリに Docker サポートを追加する</span><span class="sxs-lookup"><span data-stu-id="ff3f9-117">Add Docker support to an app</span></span>

<span data-ttu-id="ff3f9-118">ASP.NET Core プロジェクトに Docker のサポートを追加するには、プロジェクトで .NET Core をターゲットにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-118">In order to add Docker support to an ASP.NET Core project, the project must target .NET Core.</span></span> <span data-ttu-id="ff3f9-119">Linux と Windows の両方のコンテナーがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-119">Both Linux and Windows containers are supported.</span></span>

<span data-ttu-id="ff3f9-120">プロジェクトに Docker サポートを追加する場合は、Windows または Linux のいずれかのコンテナーを選択します。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-120">When adding Docker support to a project, choose either a Windows or a Linux container.</span></span> <span data-ttu-id="ff3f9-121">Docker ホストが同じコンテナーの種類を実行している必要があります。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-121">The Docker host must be running the same container type.</span></span> <span data-ttu-id="ff3f9-122">実行中の Docker インスタンスでコンテナーの種類を変更するには、システム トレイの Docker アイコンを右クリックして、**[Switch to Windows containers...]\(Windows コンテナーに切り替える...\)** または **[Switch to Linux containers...]\(Linux コンテナーに切り替える...\)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-122">To change the container type in the running Docker instance, right-click the System Tray's Docker icon and choose **Switch to Windows containers...** or **Switch to Linux containers...**.</span></span>

### <a name="new-app"></a><span data-ttu-id="ff3f9-123">新しいアプリ</span><span class="sxs-lookup"><span data-stu-id="ff3f9-123">New app</span></span>

<span data-ttu-id="ff3f9-124">**ASP.NET Core Web アプリケーション** プロジェクト テンプレートを使用して新しいアプリを作成する場合は、次のように **[Enable Docker Support]\(Docker サポートを有効にする\)** チェック ボックスをオンにします。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-124">When creating a new app with the **ASP.NET Core Web Application** project templates, select the **Enable Docker Support** checkbox:</span></span>

![[Enable Docker Support]\(Docker サポートを有効にする\) チェック ボックス](visual-studio-tools-for-docker/_static/enable-docker-support-checkbox.png)

<span data-ttu-id="ff3f9-126">ターゲット フレームワークが .NET Core の場合、**[OS]** ドロップダウンでコンテナーの種類を選択できます。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-126">If the target framework is .NET Core, the **OS** drop-down allows for the selection of a container type.</span></span>

### <a name="existing-app"></a><span data-ttu-id="ff3f9-127">既存のアプリ</span><span class="sxs-lookup"><span data-stu-id="ff3f9-127">Existing app</span></span>

<span data-ttu-id="ff3f9-128">Visual Studio Tools for Docker では、.NET Framework をターゲットとする既存の ASP.NET Core プロジェクトへの Docker の追加はサポートされません。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-128">The Visual Studio Tools for Docker don't support adding Docker to an existing ASP.NET Core project targeting .NET Framework.</span></span> <span data-ttu-id="ff3f9-129">.NET Core をターゲットとする ASP.NET Core プロジェクトの場合、ツールを使用して Docker サポートを追加するための 2 つのオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-129">For ASP.NET Core projects targeting .NET Core, there are two options for adding Docker support via the tooling.</span></span> <span data-ttu-id="ff3f9-130">Visual Studio でプロジェクトを開き、次のオプションのいずれかを選択します。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-130">Open the project in Visual Studio, and choose one of the following options:</span></span>

* <span data-ttu-id="ff3f9-131">**[プロジェクト]** メニューから **[Docker サポート]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-131">Select **Docker Support** from the **Project** menu.</span></span>
* <span data-ttu-id="ff3f9-132">ソリューション エクスプローラーでプロジェクトを右クリックして、**[追加]** > **[Docker サポート]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-132">Right-click the project in Solution Explorer and select **Add** > **Docker Support**.</span></span>

## <a name="docker-assets-overview"></a><span data-ttu-id="ff3f9-133">Docker 資産の概要</span><span class="sxs-lookup"><span data-stu-id="ff3f9-133">Docker assets overview</span></span>

<span data-ttu-id="ff3f9-134">Visual Studio Tools for Docker は、ソリューションに *docker-compose* プロジェクトを追加します。以下のものを含みます。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-134">The Visual Studio Tools for Docker add a *docker-compose* project to the solution, containing the following:</span></span>

* <span data-ttu-id="ff3f9-135">*.dockerignore*: ビルド コンテキストを生成するときに除外するファイルとディレクトリのパターンの一覧が含まれています。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-135">*.dockerignore*: Contains a list of file and directory patterns to exclude when generating a build context.</span></span>
* <span data-ttu-id="ff3f9-136">*docker-compose.yml*: `docker-compose build` と `docker-compose run` を使用してビルドされ実行されるイメージのコレクションを定義するために使用されるベースとなる [Docker Compose](https://docs.docker.com/compose/overview/) ファイル。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-136">*docker-compose.yml*: The base [Docker Compose](https://docs.docker.com/compose/overview/) file used to define the collection of images to be built and run with `docker-compose build` and `docker-compose run`, respectively.</span></span>
* <span data-ttu-id="ff3f9-137">*docker-compose.override.yml*: サービスの構成オーバーライドを含む、Docker Compose によって読み取られるオプション ファイル。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-137">*docker-compose.override.yml*: An optional file, read by Docker Compose, containing configuration overrides for services.</span></span> <span data-ttu-id="ff3f9-138">Visual Studio は `docker-compose -f "docker-compose.yml" -f "docker-compose.override.yml"` を実行してこれらのファイルをマージします。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-138">Visual Studio executes `docker-compose -f "docker-compose.yml" -f "docker-compose.override.yml"` to merge these files.</span></span>

<span data-ttu-id="ff3f9-139">*Dockerfile* (Docker の最終イメージを作成するためのレシピ) は、プロジェクトのルートに追加されます。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-139">A *Dockerfile*, the recipe for creating a final Docker image, is added to the project root.</span></span> <span data-ttu-id="ff3f9-140">その中に含まれるコマンドの詳細については、「[Dockerfile reference](https://docs.docker.com/engine/reference/builder/)」 (Dockerfile リファレンス) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-140">Refer to [Dockerfile reference](https://docs.docker.com/engine/reference/builder/) for an understanding of the commands within it.</span></span> <span data-ttu-id="ff3f9-141">この特定の *Dockerfile* では、次のような、4 つの異なる名前付きのビルド ステージを含む、[multi-stage build](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) を使用します。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-141">This particular *Dockerfile* uses a [multi-stage build](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) containing four distinct, named build stages:</span></span>

[!code-dockerfile[](visual-studio-tools-for-docker/samples/HelloDockerTools/HelloDockerTools/Dockerfile?highlight=1,5,14,17)]

<span data-ttu-id="ff3f9-142">*Dockerfile* は、[microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore) イメージに基づいています。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-142">The *Dockerfile* is based on the [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore) image.</span></span> <span data-ttu-id="ff3f9-143">このベース イメージには、Just-In-Time (JIT) にコンパイルされて起動時のパフォーマンスが向上した、ASP.NET Core NuGet パッケージが含まれます。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-143">This base image includes the ASP.NET Core NuGet packages, which have been pre-jitted to improve startup performance.</span></span>

<span data-ttu-id="ff3f9-144">*docker-compose.yml* ファイルには、プロジェクトの実行時に作成されたイメージの名前が含まれています。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-144">The *docker-compose.yml* file contains the name of the image that's created when the project runs:</span></span>

[!code-yaml[](visual-studio-tools-for-docker/samples/HelloDockerTools/docker-compose.yml?highlight=5)]

<span data-ttu-id="ff3f9-145">上の例では、`image: hellodockertools` によって、アプリが**デバッグ** モードで実行されるときに、イメージ `hellodockertools:dev` が生成されます。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-145">In the preceding example, `image: hellodockertools` generates the image `hellodockertools:dev` when the app runs in **Debug** mode.</span></span> <span data-ttu-id="ff3f9-146">`hellodockertools:latest` イメージは、アプリが**リリース** モードで実行されるときに生成されます。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-146">The `hellodockertools:latest` image is generated when the app runs in **Release** mode.</span></span>

<span data-ttu-id="ff3f9-147">イメージがレジストリにプッシュされる場合は、イメージ名の前に [Docker Hub](https://hub.docker.com/) のユーザー名を付けます (例: `dockerhubusername/hellodockertools`)。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-147">Prefix the image name with the [Docker Hub](https://hub.docker.com/) username (for example, `dockerhubusername/hellodockertools`) if the image will be pushed to the registry.</span></span> <span data-ttu-id="ff3f9-148">または、構成に応じて、プライベート レジストリ URL を含めるようにイメージ名を変更します (例: `privateregistry.domain.com/hellodockertools`)。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-148">Alternatively, change the image name to include the private registry URL (for example, `privateregistry.domain.com/hellodockertools`) depending on the configuration.</span></span>

## <a name="debug"></a><span data-ttu-id="ff3f9-149">デバッグ</span><span class="sxs-lookup"><span data-stu-id="ff3f9-149">Debug</span></span>

<span data-ttu-id="ff3f9-150">ツールバーのデバッグ ドロップダウンから **[Docker]** を選択し、アプリのデバッグを開始します。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-150">Select **Docker** from the debug drop-down in the toolbar, and start debugging the app.</span></span> <span data-ttu-id="ff3f9-151">**[出力]** ウィンドウの **[Docker]** ビューには、行われている次のアクションが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-151">The **Docker** view of the **Output** window shows the following actions taking place:</span></span>

* <span data-ttu-id="ff3f9-152">*microsoft/aspnetcore* ランタイム イメージが取得されます (まだキャッシュにない場合)。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-152">The *microsoft/aspnetcore* runtime image is acquired (if not already in the cache).</span></span>
* <span data-ttu-id="ff3f9-153">*microsoft/aspnetcore-build* コンパイル/発行イメージが取得されます (キャッシュにない場合)。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-153">The *microsoft/aspnetcore-build* compile/publish image is acquired (if not already in the cache).</span></span>
* <span data-ttu-id="ff3f9-154">*ASPNETCORE_ENVIRONMENT* 環境変数は、コンテナー内で `Development` に設定されます。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-154">The *ASPNETCORE_ENVIRONMENT* environment variable is set to `Development` within the container.</span></span>
* <span data-ttu-id="ff3f9-155">ポート 80 が公開され、localhost 用に動的に割り当てられているポートにマップされます。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-155">Port 80 is exposed and mapped to a dynamically-assigned port for localhost.</span></span> <span data-ttu-id="ff3f9-156">このポートは、Docker ホストによって決定され、`docker ps` コマンドでクエリを実行することができます。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-156">The port is determined by the Docker host and can be queried with the `docker ps` command.</span></span>
* <span data-ttu-id="ff3f9-157">アプリがコンテナーにコピーされます。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-157">The app is copied to the container.</span></span>
* <span data-ttu-id="ff3f9-158">動的に割り当てられたポートを使用して、デバッガーがコンテナーにアタッチされ、既定のブラウザーが起動します。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-158">The default browser is launched with the debugger attached to the container using the dynamically-assigned port.</span></span> 

<span data-ttu-id="ff3f9-159">結果の Docker イメージは *microsoft/aspnetcore* イメージをベース イメージとする、アプリの *dev* イメージです。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-159">The resulting Docker image is the *dev* image of the app with the *microsoft/aspnetcore* images as the base image.</span></span> <span data-ttu-id="ff3f9-160">**パッケージ マネージャー コンソール** (PMC) ウィンドウで `docker images` コマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-160">Run the `docker images` command in the **Package Manager Console** (PMC) window.</span></span> <span data-ttu-id="ff3f9-161">コンピューター上のイメージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-161">The images on the machine are displayed:</span></span>

```console
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
hellodockertools             latest                f8f9d6c923e2        About an hour ago   391MB
hellodockertools             dev                   85c5ffee5258        About an hour ago   389MB
microsoft/aspnetcore-build   2.0-nanoserver-1709   d7cce94e3eb0        15 hours ago        1.86GB
microsoft/aspnetcore         2.0-nanoserver-1709   8872347d7e5d        40 hours ago        389MB
```

> [!NOTE]
> <span data-ttu-id="ff3f9-162">**デバッグ**構成では、反復にボリューム マウントを使用するため、dev イメージにはアプリのコンテンツはありません。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-162">The dev image lacks the app contents, as **Debug** configurations use volume mounting to provide the iterative experience.</span></span> <span data-ttu-id="ff3f9-163">イメージをプッシュするには、**リリース**構成を使用します。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-163">To push an image, use the **Release** configuration.</span></span>

<span data-ttu-id="ff3f9-164">PMC で `docker ps` コマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-164">Run the `docker ps` command in PMC.</span></span> <span data-ttu-id="ff3f9-165">アプリがコンテナーを使用して実行されていることがわかります。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-165">Notice the app is running using the container:</span></span>

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   21 seconds ago      Up 19 seconds       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="edit-and-continue"></a><span data-ttu-id="ff3f9-166">エディット コンティニュ</span><span class="sxs-lookup"><span data-stu-id="ff3f9-166">Edit and continue</span></span>

<span data-ttu-id="ff3f9-167">静的なファイルや Razor ビューに対する変更は、コンパイルを行う必要はなく、自動的に更新されます。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-167">Changes to static files and Razor views are automatically updated without the need for a compilation step.</span></span> <span data-ttu-id="ff3f9-168">変更を行って保存し、ブラウザーを更新し、更新を確認します。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-168">Make the change, save, and refresh the browser to view the update.</span></span>  

<span data-ttu-id="ff3f9-169">コード ファイルを変更した場合、コンパイルを行い、コンテナー内で Kestrel を再起動する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-169">Modifications to code files requires compiling and a restart of Kestrel within the container.</span></span> <span data-ttu-id="ff3f9-170">変更後、CTRL キーを押しながら F5 キーを使用して、プロセスを実行して、コンテナー内でアプリを起動します。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-170">After making the change, use CTRL + F5 to perform the process and start the app within the container.</span></span> <span data-ttu-id="ff3f9-171">Docker コンテナーが再構築されたり、停止されたりすることはありません。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-171">The Docker container isn't rebuilt or stopped.</span></span> <span data-ttu-id="ff3f9-172">PMC で `docker ps` コマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-172">Run the `docker ps` command in PMC.</span></span> <span data-ttu-id="ff3f9-173">元のコンテナーが 10 分前と同じ状態で実行されていることを確認できます。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-173">Notice the original container is still running as of 10 minutes ago:</span></span>

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   10 minutes ago      Up 10 minutes       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="publish-docker-images"></a><span data-ttu-id="ff3f9-174">Docker イメージの発行</span><span class="sxs-lookup"><span data-stu-id="ff3f9-174">Publish Docker images</span></span>

<span data-ttu-id="ff3f9-175">アプリの開発とデバッグのサイクルが完了したら、Visual Studio Tools for Docker でアプリの実稼働イメージを作成できます。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-175">Once the develop and debug cycle of the app is completed, the Visual Studio Tools for Docker assist in creating the production image of the app.</span></span> <span data-ttu-id="ff3f9-176">構成ドロップダウンを **[リリース]** に変更し、アプリを構築します。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-176">Change the configuration drop-down to **Release** and build the app.</span></span> <span data-ttu-id="ff3f9-177">このツールにより、イメージが、プライベート レジストリまたは Docker Hub にプッシュできる *latest* タグ付きで生成されます。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-177">The tooling produces the image with the *latest* tag, which can be pushed to the private registry or Docker Hub.</span></span> 

<span data-ttu-id="ff3f9-178">PMC で `docker images` コマンドを実行して、次のようにイメージの一覧を表示します。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-178">Run the `docker images` command in PMC to see the list of images:</span></span>

```console
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
hellodockertools             latest                4cb1fca533f0        19 seconds ago      391MB
hellodockertools             dev                   85c5ffee5258        About an hour ago   389MB
microsoft/aspnetcore-build   2.0-nanoserver-1709   d7cce94e3eb0        16 hours ago        1.86GB
microsoft/aspnetcore         2.0-nanoserver-1709   8872347d7e5d        40 hours ago        389MB
```

> [!NOTE]
> <span data-ttu-id="ff3f9-179">`docker images` コマンドは、*\<none>* として識別されるリポジトリ名とタグを持つ中間イメージを返します (上にはリストされていません)。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-179">The `docker images` command returns intermediary images with repository names and tags identified as *\<none>* (not listed above).</span></span> <span data-ttu-id="ff3f9-180">これらの名前のないイメージは、[multi-stage build](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) *Dockerfile* によって生成されます。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-180">These unnamed images are produced by the [multi-stage build](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) *Dockerfile*.</span></span> <span data-ttu-id="ff3f9-181">これにより、最終イメージの構築効率が向上します&mdash;変更時には必要なレイヤーのみが再構築されます。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-181">They improve the efficiency of building the final image&mdash;only the necessary layers are rebuilt when changes occur.</span></span> <span data-ttu-id="ff3f9-182">中間イメージが不要になった場合は、[docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/) コマンドを使用して削除します。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-182">When the intermediary images are no longer needed, delete them using the [docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/) command.</span></span>

<span data-ttu-id="ff3f9-183">*dev* イメージと比較した場合、実稼働またはリリース イメージはサイズが小さいと思うかもしれません。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-183">There may be an expectation for the production or release image to be smaller in size by comparison to the *dev* image.</span></span> <span data-ttu-id="ff3f9-184">ボリューム マッピングにより、デバッガーとアプリは、コンテナー内ではなく、ローカル コンピューターから実行されています。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-184">Because of the volume mapping, the debugger and app were running from the local machine and not within the container.</span></span> <span data-ttu-id="ff3f9-185">*latest* イメージには、ホスト コンピューターでアプリを実行するために必要なアプリ コードがパッケージ化されています。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-185">The *latest* image has packaged the necessary app code to run the app on a host machine.</span></span> <span data-ttu-id="ff3f9-186">そのため、デルタはアプリ コードのサイズです。</span><span class="sxs-lookup"><span data-stu-id="ff3f9-186">Therefore, the delta is the size of the app code.</span></span>
