---
title: "Visual Studio Tools for Docker と ASP.NET Core"
description: "この記事では、Visual Studio 2017 ツールと Docker for Windows を使用して、ASP.NET Core アプリケーションをコンテナー化する方法を説明します。"
keywords: "Docker, ASP.NET Core, Visual Studio, コンテナー"
author: spboyer
ms.author: scaddie
manager: wpickett
ms.date: 09/26/2017
ms.topic: article
ms.prod: asp.net-core
ms.technology: aspnet
ms.assetid: 1f3b9a68-4dea-4b60-8cb3-f46164eedbbf
uid: publishing/vs-tools-for-docker
ms.openlocfilehash: 2d8e337141ae4e0d0258f1d7546510b0ab077e39
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="visual-studio-tools-for-docker"></a><span data-ttu-id="4915b-104">Visual Studio Tools for Docker</span><span class="sxs-lookup"><span data-stu-id="4915b-104">Visual Studio Tools for Docker</span></span>

<span data-ttu-id="4915b-105">[Docker for Windows](https://docs.docker.com/docker-for-windows/install/) を含む [Microsoft Visual Studio 2017](https://www.visualstudio.com/) では、Windows と Linux コンテナーを使用した .NET Framework および .NET Core の Web アプリケーションやコンソール アプリケーションのビルド、デバッグ、実行をサポートします。</span><span class="sxs-lookup"><span data-stu-id="4915b-105">[Microsoft Visual Studio 2017](https://www.visualstudio.com/) with [Docker for Windows](https://docs.docker.com/docker-for-windows/install/) supports building, debugging, and running .NET Framework and .NET Core web and console applications using Windows and Linux containers.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4915b-106">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="4915b-106">Prerequisites</span></span>

- <span data-ttu-id="4915b-107">.NET Core ワークロードを含む [Microsoft Visual Studio 2017](https://www.visualstudio.com/)</span><span class="sxs-lookup"><span data-stu-id="4915b-107">[Microsoft Visual Studio 2017](https://www.visualstudio.com/) with .NET Core workload</span></span>
- [<span data-ttu-id="4915b-108">Docker for Windows</span><span class="sxs-lookup"><span data-stu-id="4915b-108">Docker for Windows</span></span>](https://docs.docker.com/docker-for-windows/install/)

## <a name="installation-and-setup"></a><span data-ttu-id="4915b-109">インストールとセットアップ</span><span class="sxs-lookup"><span data-stu-id="4915b-109">Installation and setup</span></span>

<span data-ttu-id="4915b-110">.NET Core ワークロードで [Microsoft Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) をインストールします。</span><span class="sxs-lookup"><span data-stu-id="4915b-110">Install [Microsoft Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) with the .NET Core workload.</span></span> <span data-ttu-id="4915b-111">Visual Studio をインストール済みの場合は、[Visual Studio のインストールを変更して](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) .NET Core ワークロードを追加できます。</span><span class="sxs-lookup"><span data-stu-id="4915b-111">If you already have Visual Studio installed, you can [modify your Visual Studio installation](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) to add the .NET Core workload.</span></span>

<span data-ttu-id="4915b-112">Docker をインストールする場合、「[Docker for Windows: What to know before you install](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install)」 (Docker for Windows: インストール前に知っておくべきこと) の情報を確認し、[Docker for Windows](https://docs.docker.com/docker-for-windows/install/) をインストールします。</span><span class="sxs-lookup"><span data-stu-id="4915b-112">For Docker installation, review the information at [Docker for Windows: What to know before you install](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install) and install [Docker For Windows](https://docs.docker.com/docker-for-windows/install/).</span></span>

<span data-ttu-id="4915b-113">Docker for Windows では、**[共有ドライブ](https://docs.docker.com/docker-for-windows/#shared-drives)**を設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4915b-113">A required configuration is to setup **[Shared Drives](https://docs.docker.com/docker-for-windows/#shared-drives)** in Docker for Windows.</span></span> <span data-ttu-id="4915b-114">この設定は、ボリュームのマップとデバッグのサポートで必要です。</span><span class="sxs-lookup"><span data-stu-id="4915b-114">The setting is required for the volume mapping and debugging support.</span></span>

<span data-ttu-id="4915b-115">システム トレイの Docker アイコンを右クリックして **[設定]** をクリックし、**[Shared Drives]\(共有ドライブ\)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="4915b-115">Right-click the Docker icon in the System Tray, click **Settings**, and select **Shared Drives**.</span></span> <span data-ttu-id="4915b-116">Docker によるファイルの格納先となるドライブを選択して、変更を適用します。</span><span class="sxs-lookup"><span data-stu-id="4915b-116">Select the drive where Docker will store your files and apply changes.</span></span>

![共有ドライブ](./visual-studio-tools-for-docker/_static/settings-shared-drives-win.png)

## <a name="create-an-aspnet-web-application-and-add-docker-support"></a><span data-ttu-id="4915b-118">ASP.NET Web アプリケーションを作成し、Docker のサポートを追加する</span><span class="sxs-lookup"><span data-stu-id="4915b-118">Create an ASP.NET Web Application and add Docker Support</span></span>

<span data-ttu-id="4915b-119">Visual Studio を使用して、新しい ASP.NET Core Web アプリケーションを作成できます。</span><span class="sxs-lookup"><span data-stu-id="4915b-119">Using Visual Studio, create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="4915b-120">アプリケーションが読み込まれたら、**[プロジェクト] メニュー**の **[Add Docker Support]\(Docker サポートの追加\)** を選択するか、ソリューション エクスプローラーからプロジェクトを右クリックし、**[追加]** > **[Docker Support]\(Docker サポート\)** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="4915b-120">When the application is loaded, either select **Add Docker Support** from the **Project Menu** or right-click the project from the Solution Explorer and select **Add** > **Docker Support**.</span></span>

<span data-ttu-id="4915b-121">*[プロジェクト] メニュー*</span><span class="sxs-lookup"><span data-stu-id="4915b-121">*Project Menu*</span></span>

![[プロジェクト]、「Add Docker Support」 (Docker サポートの追加)](./visual-studio-tools-for-docker/_static/project-add-docker-support.png)

<span data-ttu-id="4915b-123">*[プロジェクト] のコンテキスト メニュー*</span><span class="sxs-lookup"><span data-stu-id="4915b-123">*Project Context Menu*</span></span>

![[追加]、[Docker Support]\(Docker サポート\) を右クリック](./visual-studio-tools-for-docker/_static/right-click-add-docker-support.png)

<span data-ttu-id="4915b-125">プロジェクトに Docker サポートを追加する場合、Windows または Linux のいずれかのコンテナーを選択できます。</span><span class="sxs-lookup"><span data-stu-id="4915b-125">When you add Docker support to your project, you can choose either Windows or Linux containers.</span></span> <span data-ttu-id="4915b-126">(Docker ホストが同じコンテナーの種類を実行している必要があります。</span><span class="sxs-lookup"><span data-stu-id="4915b-126">(The Docker host must be running the same container type.</span></span> <span data-ttu-id="4915b-127">実行中の Docker インスタンスでコンテナーの種類を変更する必要がある場合、システム トレイの **Docker** アイコンを右クリックして、**[Switch to Windows containers]\(Windows コンテナーに切り替える\)**、または **[Switch to Linux containers]\(Linux コンテナーに切り替える\)** を選択します。)</span><span class="sxs-lookup"><span data-stu-id="4915b-127">If you need change the container type in the running Docker instance, right-click the **Docker** icon in the System Tray, and choose **Switch to Windows containers** or **Switch to Linux containers**.)</span></span> 

<span data-ttu-id="4915b-128">プロジェクトに次のファイルが追加されます。</span><span class="sxs-lookup"><span data-stu-id="4915b-128">The following files are added to the project:</span></span>

- <span data-ttu-id="4915b-129">**Dockerfile**: ASP.NET Core アプリケーション用の Docker ファイルは、[microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore) イメージに基づきます。</span><span class="sxs-lookup"><span data-stu-id="4915b-129">**Dockerfile**: the Docker file for ASP.NET Core applications is based on the [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore) image.</span></span> <span data-ttu-id="4915b-130">このイメージには、Just-In-Time (JIT) にコンパイルされてパフォーマンスが向上した ASP.NET Core NuGet パッケージが含まれます。</span><span class="sxs-lookup"><span data-stu-id="4915b-130">This image includes the ASP.NET Core NuGet packages, which have been pre-jitted improving startup performance.</span></span> <span data-ttu-id="4915b-131">.NET Core のコンソール アプリケーションを構築するとき、Dockerfile の FROM は最新の [microsoft/dotnet](https://hub.docker.com/r/microsoft/dotnet) イメージを参照します。</span><span class="sxs-lookup"><span data-stu-id="4915b-131">When building .NET Core Console Applications, the Dockerfile FROM will reference the most recent [microsoft/dotnet](https://hub.docker.com/r/microsoft/dotnet) image.</span></span>   
- <span data-ttu-id="4915b-132">**docker-compose.yml**: docker-compose build/run と共に実行され構築されるイメージの集合を定義するために使用されるベースとなる Docker Compose ファイル。</span><span class="sxs-lookup"><span data-stu-id="4915b-132">**docker-compose.yml**: base Docker Compose file used to define the collection of images to be built and run with docker-compose build/run.</span></span>   
- <span data-ttu-id="4915b-133">**docker-compose.dev.debug.yml**: 構成がデバッグに設定されている場合の反復的な変更用の追加の docker-compose ファイル。</span><span class="sxs-lookup"><span data-stu-id="4915b-133">**docker-compose.dev.debug.yml**: additional docker-compose file with for iterative changes when your configuration is set to debug.</span></span> <span data-ttu-id="4915b-134">Visual Studio は -f docker-compose.yml -f docker-compose.dev.debug.yml を呼び出し、これらをマージします。</span><span class="sxs-lookup"><span data-stu-id="4915b-134">Visual Studio will call -f docker-compose.yml -f docker-compose.dev.debug.yml to merge these together.</span></span> <span data-ttu-id="4915b-135">この構成ファイルは、Visual Studio の開発ツールによって使用されます。</span><span class="sxs-lookup"><span data-stu-id="4915b-135">This compose file is used by Visual Studio development tools.</span></span>   
- <span data-ttu-id="4915b-136">**docker-compose.dev.release.yml**: リリース定義をデバッグする、追加の Docker Compose ファイル。</span><span class="sxs-lookup"><span data-stu-id="4915b-136">**docker-compose.dev.release.yml**: additional Docker Compose file to debug your release definition.</span></span> <span data-ttu-id="4915b-137">実稼働イメージの内容を変更しないように、デバッガーをボリューム マウントします。</span><span class="sxs-lookup"><span data-stu-id="4915b-137">It will volume mount the debugger so it doesn't change the contents of the production image.</span></span>  

<span data-ttu-id="4915b-138">*docker-compose.yml* ファイルには、プロジェクトの実行時に作成されたイメージの名前が含まれています。</span><span class="sxs-lookup"><span data-stu-id="4915b-138">The *docker-compose.yml* file contains the name of the image that is created when project is run.</span></span> 

```
version '2'

services:
  hellodockertools:
    image:  user/hellodockertools${TAG}
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - "80"
``` 

<span data-ttu-id="4915b-139">この例では、`image: user/hellodockertools${TAG}` によってアプリケーションが**デバッグ** モードで実行されるとき、`user/hellodockertools:dev` が生成され、**リリース** モードで実行されるときに、`user/hellodockertools:latest` が生成されます。</span><span class="sxs-lookup"><span data-stu-id="4915b-139">In this example, `image: user/hellodockertools${TAG}` generates the image `user/hellodockertools:dev` when the application is run in **Debug** mode and `user/hellodockertools:latest` in **Release** mode respectively.</span></span> 

<span data-ttu-id="4915b-140">イメージをレジストリにプッシュする場合、`user` を [Docker Hub](https://hub.docker.com/) のユーザー名に変更してください。</span><span class="sxs-lookup"><span data-stu-id="4915b-140">You will want to change the `user` to your [Docker Hub](https://hub.docker.com/) username if you plan to push the image to the registry.</span></span> <span data-ttu-id="4915b-141">たとえば、`spboyer/hellodockertools` や、構成に応じて `privateregistry.domain.com/` のプライベート レジストリ URL に変更します。</span><span class="sxs-lookup"><span data-stu-id="4915b-141">For example, `spboyer/hellodockertools`, or change to your private registry URL `privateregistry.domain.com/` depending on your configuration.</span></span>

### <a name="debugging"></a><span data-ttu-id="4915b-142">デバッグ</span><span class="sxs-lookup"><span data-stu-id="4915b-142">Debugging</span></span>

<span data-ttu-id="4915b-143">ツールバーのデバッグ ドロップダウンから **[Docker]** を選択し、F5 を使用してアプリケーションのデバッグを開始します。</span><span class="sxs-lookup"><span data-stu-id="4915b-143">Select **Docker** from the debug drop-down in the toolbar and use F5 to start debugging the application.</span></span> 

- <span data-ttu-id="4915b-144">(キャッシュにない場合) *microsoft/aspnetcore* イメージが取得されます。</span><span class="sxs-lookup"><span data-stu-id="4915b-144">The *microsoft/aspnetcore* image is acquired (if not already in your cache)</span></span>
- <span data-ttu-id="4915b-145">*ASPNETCORE_ENVIRONMENT* は、コンテナー内で Development に設定されます。</span><span class="sxs-lookup"><span data-stu-id="4915b-145">*ASPNETCORE_ENVIRONMENT* is set to Development within the container</span></span>
- <span data-ttu-id="4915b-146">ポート 80 が公開され、localhost 用に動的に割り当てられているポートにマップされます。</span><span class="sxs-lookup"><span data-stu-id="4915b-146">PORT 80 is EXPOSED and mapped to a dynamically assigned port for localhost.</span></span> <span data-ttu-id="4915b-147">このポートは、docker ホストによって決定され、docker ps でクエリを実行することができます。</span><span class="sxs-lookup"><span data-stu-id="4915b-147">The port is determined by the docker host and can be queried with docker ps.</span></span> 
- <span data-ttu-id="4915b-148">アプリケーションがコンテナーにコピーされます。</span><span class="sxs-lookup"><span data-stu-id="4915b-148">Your application is copied to the container</span></span>
- <span data-ttu-id="4915b-149">動的に割り当てられたポートを使用して、デバッガーがコンテナーにアタッチされ、既定のブラウザーが起動します。</span><span class="sxs-lookup"><span data-stu-id="4915b-149">Default browser is launched with the debugger attached to the container, using the dynamically assigned port.</span></span> 

<span data-ttu-id="4915b-150">構築された結果の Docker イメージは *microsoft/aspnetcore* イメージをベース イメージとするアプリケーションの *dev* イメージです。</span><span class="sxs-lookup"><span data-stu-id="4915b-150">The resulting Docker image built is the *dev* image of your application with the *microsoft/aspnetcore* images as the base image.</span></span>

<span data-ttu-id="4915b-151">**注:** デバッグ構成では、反復にボリューム マウントを使用するため、dev イメージにはアプリのコンテンツは含まれません。</span><span class="sxs-lookup"><span data-stu-id="4915b-151">**Note:** The dev image is empty of your app contents as Debug configurations use volume mounting to provide the iterative experience.</span></span> <span data-ttu-id="4915b-152">イメージをプッシュするには、リリース構成を使用します。</span><span class="sxs-lookup"><span data-stu-id="4915b-152">To push an image, use the Release configuration.</span></span>

```console
REPOSITORY                  TAG         IMAGE ID            CREATED         SIZE
spboyer/hellodockertools    dev         0b6e2e44b3df        4 minutes ago   268.9 MB
microsoft/aspnetcore        1.0.1       189ad4312ce7        5 days ago      268.9 MB
```

<span data-ttu-id="4915b-153">アプリケーションは、`docker ps` コマンドを実行して参照できるコンテナーを使用して実行されます。</span><span class="sxs-lookup"><span data-stu-id="4915b-153">The application is running using the container, which you can see by running the `docker ps` command.</span></span>

```console
CONTAINER ID        IMAGE                          COMMAND               CREATED             STATUS              PORTS                   NAMES
3f240cf686c9        spboyer/hellodockertools:dev   "tail -f /dev/null"   4 minutes ago       Up 4 minutes        0.0.0.0:32769->80/tcp   hellodockertools_hellodockertools_1
```

### <a name="edit-and-continue"></a><span data-ttu-id="4915b-154">エディット コンティニュ</span><span class="sxs-lookup"><span data-stu-id="4915b-154">Edit and Continue</span></span>

<span data-ttu-id="4915b-155">静的なファイルや razor テンプレート ファイル (*.cshtml*) に対する変更は、コンパイルをする必要はなく、自動的に更新されます。</span><span class="sxs-lookup"><span data-stu-id="4915b-155">Changes to static files and/or razor template files (*.cshtml*) are automatically updated without the need of a compilation step.</span></span> <span data-ttu-id="4915b-156">変更を行って保存し、タップしてブラウザーを更新し、更新を確認します。</span><span class="sxs-lookup"><span data-stu-id="4915b-156">Make the change, save, and tap refresh in the browser to view the update.</span></span>  

<span data-ttu-id="4915b-157">コード ファイルを変更した場合、コンパイルを行い、コンテナー内で Kestrel を再起動する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4915b-157">Modifications to code files require compiling and a restart of Kestrel within the container.</span></span> <span data-ttu-id="4915b-158">変更後、CTRL キーを押しながら F5 キーを使用して、手順を実行して、コンテナー内でアプリケーションを起動します。</span><span class="sxs-lookup"><span data-stu-id="4915b-158">After making the change, use CTRL + F5 to perform the process and start the application within the container.</span></span> <span data-ttu-id="4915b-159">Docker コンテナーは再構築されたり、停止されたりすることはありません。コマンド ラインから `docker ps` を使用すると、元のコンテナーが 10 分前と同じ状態で実行されていることを確認できます。</span><span class="sxs-lookup"><span data-stu-id="4915b-159">The Docker container is not rebuilt or stopped; using `docker ps` in the command line, you can see that the original container is still running as of 10 minutes ago.</span></span> 

```console
CONTAINER ID        IMAGE                          COMMAND               CREATED             STATUS              PORTS                   NAMES
3f240cf686c9        spboyer/hellodockertools:dev   "tail -f /dev/null"   10 minutes ago      Up 10 minutes       0.0.0.0:32769->80/tcp   hellodockertools_hellodockertools_1
```

### <a name="publishing-docker-images"></a><span data-ttu-id="4915b-160">Docker イメージの発行</span><span class="sxs-lookup"><span data-stu-id="4915b-160">Publishing Docker images</span></span>

<span data-ttu-id="4915b-161">アプリケーションの開発とデバッグのサイクルが完了したら、Visual Studio Tools for Docker でアプリケーションの実稼働イメージを作成できます。</span><span class="sxs-lookup"><span data-stu-id="4915b-161">Once you have completed the develop and debug cycle of your application, the Visual Studio Tools for Docker will help you create the production image of your application.</span></span> <span data-ttu-id="4915b-162">デバッグ ドロップダウンを **[リリース]** に変更し、アプリケーションを構築します。</span><span class="sxs-lookup"><span data-stu-id="4915b-162">Change the debug dropdown to **Release** and build the application.</span></span> <span data-ttu-id="4915b-163">このツールにより、イメージが、プライベート レジストリまたは Docker Hub にプッシュできる `:latest` タグ付きで生成されます。</span><span class="sxs-lookup"><span data-stu-id="4915b-163">The tooling will produce the image with the `:latest` tag which you can push to your private registry or Docker Hub.</span></span> 

<span data-ttu-id="4915b-164">`docker images` を使用すると、イメージの一覧を確認できます。</span><span class="sxs-lookup"><span data-stu-id="4915b-164">Using the `docker images` command, you can see the list of images.</span></span>

```console
REPOSITORY                 TAG                 IMAGE ID            CREATED             SIZE
spboyer/hellodockertools   latest              8184ae38ba91        5 seconds ago       278.4 MB
spboyer/hellodockertools   dev                 0b6e2e44b3df        About an hour ago   268.9 MB
microsoft/aspnetcore       1.0.1               189ad4312ce7        5 days ago          268.9 MB
```

<span data-ttu-id="4915b-165">**dev** イメージと比較した場合、実稼働またはリリース イメージはサイズが小さいと思うかもしれません。しかし、ボリューム マッピングを使用することにより、デバッガーとアプリケーションは実際はコンテナーではなくローカル マシンから実行されます。</span><span class="sxs-lookup"><span data-stu-id="4915b-165">There may be an expectation for the production or release image to be smaller in size by comparison to the **dev** image; however, through the use of the volume mapping, the debugger and application were actually being run from your local machine and not within the container.</span></span> <span data-ttu-id="4915b-166">**latest** イメージには、ホスト コンピューターでアプリケーションを実行するために必要なアプリケーションのコード全体がパッケージ化されているため、デルタはアプリケーション コードのサイズです。</span><span class="sxs-lookup"><span data-stu-id="4915b-166">The **latest** image has packaged the entire application code needed to run the application on a host machine, therefore the delta is the size of your application code.</span></span>
