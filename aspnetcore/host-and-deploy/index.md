---
title: "ASP.NET Core のホストと展開"
author: tdykstra
description: "ホスティング環境を設定し、ASP.NET Core アプリを展開する方法を学習します。"
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 08/07/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/index
ms.openlocfilehash: 6ce77922dd8a0fcb81ea6a72f9179c9c81105dda
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="host-and-deploy-aspnet-core"></a><span data-ttu-id="275e0-103">ASP.NET Core のホストと展開</span><span class="sxs-lookup"><span data-stu-id="275e0-103">Host and deploy ASP.NET Core</span></span>

<span data-ttu-id="275e0-104">通常、ASP.NET Core アプリをホスティング環境に展開するには、以下を実行します。</span><span class="sxs-lookup"><span data-stu-id="275e0-104">In general, to deploy an ASP.NET Core app to a hosting environment:</span></span>

* <span data-ttu-id="275e0-105">ホスティング サーバー上のフォルダーにアプリを発行します。</span><span class="sxs-lookup"><span data-stu-id="275e0-105">Publish the app to a folder on the hosting server.</span></span>
* <span data-ttu-id="275e0-106">要求の着信時にアプリを開始し、クラッシュ後、またはサーバーの再起動後にアプリを再起動するプロセス マネージャーを設定します。</span><span class="sxs-lookup"><span data-stu-id="275e0-106">Set up a process manager that starts the app when requests arrive and restarts the app after it crashes or the server reboots.</span></span>
* <span data-ttu-id="275e0-107">要求をアプリに転送するリバース プロキシを設定します。</span><span class="sxs-lookup"><span data-stu-id="275e0-107">Set up a reverse proxy that forwards requests to the app.</span></span>

## <a name="publish-to-a-folder"></a><span data-ttu-id="275e0-108">フォルダーに発行する</span><span class="sxs-lookup"><span data-stu-id="275e0-108">Publish to a folder</span></span> 

<span data-ttu-id="275e0-109">[dotnet publish](/dotnet/articles/core/tools/dotnet-publish) CLI コマンドはアプリ コードをコンパイルし、アプリを実行するために必要なファイルを *publish* フォルダーにコピーします。</span><span class="sxs-lookup"><span data-stu-id="275e0-109">The [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) CLI command compiles app code and copies the files needed to run the app into a *publish* folder.</span></span> <span data-ttu-id="275e0-110">Visual Studio から展開する場合は、ファイルが展開先にコピーされる前に `dotnet publish` ステップが自動的に行われます。</span><span class="sxs-lookup"><span data-stu-id="275e0-110">When deploying from Visual Studio, the `dotnet publish` step happens automatically before the files are copied to the deployment destination.</span></span>

### <a name="folder-contents"></a><span data-ttu-id="275e0-111">フォルダーの内容</span><span class="sxs-lookup"><span data-stu-id="275e0-111">Folder contents</span></span>

<span data-ttu-id="275e0-112">*publish* フォルダーには、アプリとその依存関係、および必要に応じて .NET ランタイムの *.exe* ファイルと *.dll* ファイルが含まれています。</span><span class="sxs-lookup"><span data-stu-id="275e0-112">The *publish* folder contains *.exe* and *.dll* files for the app, its dependencies, and optionally the .NET runtime.</span></span>

<span data-ttu-id="275e0-113">.NET Core アプリは、*自己完結型*または*フレームワーク依存*アプリとして発行できます。</span><span class="sxs-lookup"><span data-stu-id="275e0-113">A .NET Core app can be published as *self-contained* or *framework-dependent* app.</span></span> <span data-ttu-id="275e0-114">アプリが自己完結型の場合、.NET ランタイムを含む *.dll* ファイルが *publish* フォルダーに含まれています。</span><span class="sxs-lookup"><span data-stu-id="275e0-114">If the app is self-contained, the *.dll* files that contain the .NET runtime are included in the *publish* folder.</span></span> <span data-ttu-id="275e0-115">アプリがフレームワークに依存する場合、.NET ランタイムのファイルは含まれていません。これは、サーバーにインストールされている .NET のバージョンへの参照がアプリに含まれていないためです。</span><span class="sxs-lookup"><span data-stu-id="275e0-115">If the app is framework-dependent, the .NET runtime files aren't included because the app has a reference to a version of .NET that is installed on the server.</span></span> <span data-ttu-id="275e0-116">既定の展開モデルはフレームワークに依存します。</span><span class="sxs-lookup"><span data-stu-id="275e0-116">The default deployment model is framework-dependent.</span></span> <span data-ttu-id="275e0-117">詳細については、「[.NET Core アプリケーションの展開](/dotnet/articles/core/deploying/index)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="275e0-117">For more information, see [.NET Core application deployment](/dotnet/articles/core/deploying/index).</span></span>

<span data-ttu-id="275e0-118">*.exe* ファイルと *.dll* ファイルに加え、ASP.NET Core アプリの *publish* フォルダーには、通常、構成ファイル、静的資産、および MVC ビューが含まれています。</span><span class="sxs-lookup"><span data-stu-id="275e0-118">In addition to *.exe* and *.dll* files, the *publish* folder for an ASP.NET Core app typically contains configuration files, static assets, and MVC views.</span></span> <span data-ttu-id="275e0-119">詳細については、[ディレクトリ構造](xref:host-and-deploy/directory-structure)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="275e0-119">For more information, see [Directory structure](xref:host-and-deploy/directory-structure).</span></span>

## <a name="set-up-a-process-manager"></a><span data-ttu-id="275e0-120">プロセス マネージャーを設定する</span><span class="sxs-lookup"><span data-stu-id="275e0-120">Set up a process manager</span></span>

<span data-ttu-id="275e0-121">ASP.NET Core アプリは、サーバーの起動時に起動し、クラッシュした場合には再起動する必要があるコンソール アプリです。</span><span class="sxs-lookup"><span data-stu-id="275e0-121">An ASP.NET Core app is a console app that must be started when a server boots and restarted if it crashes.</span></span> <span data-ttu-id="275e0-122">起動と再起動を自動化するには、プロセス マネージャーが必要です。</span><span class="sxs-lookup"><span data-stu-id="275e0-122">To automate starts and restarts, a process manager is required.</span></span> <span data-ttu-id="275e0-123">ASP.NET Core の最も一般的なプロセス マネージャーは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="275e0-123">The most common process managers for ASP.NET Core are:</span></span>

* <span data-ttu-id="275e0-124">Linux</span><span class="sxs-lookup"><span data-stu-id="275e0-124">Linux</span></span>
  * [<span data-ttu-id="275e0-125">Nginx</span><span class="sxs-lookup"><span data-stu-id="275e0-125">Nginx</span></span>](xref:host-and-deploy/linux-nginx)
  * [<span data-ttu-id="275e0-126">Apache</span><span class="sxs-lookup"><span data-stu-id="275e0-126">Apache</span></span>](xref:host-and-deploy/linux-apache)
* <span data-ttu-id="275e0-127">Windows</span><span class="sxs-lookup"><span data-stu-id="275e0-127">Windows</span></span>
  * [<span data-ttu-id="275e0-128">IIS</span><span class="sxs-lookup"><span data-stu-id="275e0-128">IIS</span></span>](xref:host-and-deploy/iis/index)
  * [<span data-ttu-id="275e0-129">Windows サービス</span><span class="sxs-lookup"><span data-stu-id="275e0-129">Windows Service</span></span>](xref:host-and-deploy/windows-service)

## <a name="set-up-a-reverse-proxy"></a><span data-ttu-id="275e0-130">リバース プロキシを設定する</span><span class="sxs-lookup"><span data-stu-id="275e0-130">Set up a reverse proxy</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="275e0-131">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="275e0-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="275e0-132">アプリで [Kestrel](xref:fundamentals/servers/kestrel) Web サーバーを使用する場合は、リバース プロキシ サーバーとして、[Nginx](xref:host-and-deploy/linux-nginx)、[Apache](xref:host-and-deploy/linux-apache)、または [IIS](xref:host-and-deploy/iis/index) を使用できます。</span><span class="sxs-lookup"><span data-stu-id="275e0-132">If the app uses the [Kestrel](xref:fundamentals/servers/kestrel) web server, [Nginx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache), or [IIS](xref:host-and-deploy/iis/index) can be used as a reverse proxy server.</span></span> <span data-ttu-id="275e0-133">リバース プロキシ サーバーはインターネットから HTTP 要求を受け取り、事前にいくつかの処理を行ってから Kestrel に転送します。</span><span class="sxs-lookup"><span data-stu-id="275e0-133">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span> <span data-ttu-id="275e0-134">詳細については、「[When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#when-to-use-kestrel-with-a-reverse-proxy)」 (Kestrel とリバース プロキシを使用するタイミング) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="275e0-134">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="275e0-135">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="275e0-135">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="275e0-136">アプリを [Kestrel](xref:fundamentals/servers/kestrel) Web サーバーを使用して、インターネットに公開する場合は、リバース プロキシ サーバーとして、[Nginx](xref:host-and-deploy/linux-nginx)、[Apache](xref:host-and-deploy/linux-apache)、または [IIS](xref:host-and-deploy/iis/index) を使用します。</span><span class="sxs-lookup"><span data-stu-id="275e0-136">If the app uses the [Kestrel](xref:fundamentals/servers/kestrel) web server and will be exposed to the Internet, use [Nginx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache), or [IIS](xref:host-and-deploy/iis/index) as a reverse proxy server.</span></span> <span data-ttu-id="275e0-137">リバース プロキシ サーバーはインターネットから HTTP 要求を受け取り、事前にいくつかの処理を行ってから Kestrel に転送します。</span><span class="sxs-lookup"><span data-stu-id="275e0-137">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span> <span data-ttu-id="275e0-138">リバース プロキシを使用する主な理由はセキュリティです。</span><span class="sxs-lookup"><span data-stu-id="275e0-138">The main reason for using a reverse proxy is security.</span></span> <span data-ttu-id="275e0-139">詳細については、「[When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel?tabs=aspnetcore1x#when-to-use-kestrel-with-a-reverse-proxy)」 (Kestrel とリバース プロキシを使用するタイミング) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="275e0-139">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel?tabs=aspnetcore1x#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

---

## <a name="using-visual-studio-and-msbuild-to-automate-deployment"></a><span data-ttu-id="275e0-140">Visual Studio と MSBuild を使用して展開を自動化する</span><span class="sxs-lookup"><span data-stu-id="275e0-140">Using Visual Studio and MSBuild to automate deployment</span></span>

<span data-ttu-id="275e0-141">多くの場合、展開には、`dotnet publish` からサーバーへの出力のコピー以外の追加の作業が必要になります。</span><span class="sxs-lookup"><span data-stu-id="275e0-141">Deployment often requires additional tasks besides copying the output from `dotnet publish` to a server.</span></span> <span data-ttu-id="275e0-142">たとえば、追加のファイルが必要になる場合や、*publish* フォルダーから除外される場合があります。</span><span class="sxs-lookup"><span data-stu-id="275e0-142">For example, extra files might be required or excluded from the *publish* folder.</span></span> <span data-ttu-id="275e0-143">Visual Studio では Web 展開で MSBuild を使用します。この MSBuild は、展開時に他の多くの作業を行うためにカスタマイズすることができます。</span><span class="sxs-lookup"><span data-stu-id="275e0-143">Visual Studio uses MSBuild for web deployment, and MSBuild can be customized to do many other tasks during deployment.</span></span> <span data-ttu-id="275e0-144">詳細については、[Visual Studio の発行プロファイル](xref:host-and-deploy/visual-studio-publish-profiles)に関するページと、『[Using MSBuild and Team Foundation Build](http://msbuildbook.com/)』という書籍を参照してください。</span><span class="sxs-lookup"><span data-stu-id="275e0-144">For more information, see [Publish profiles in Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) and the [Using MSBuild and Team Foundation Build](http://msbuildbook.com/) book.</span></span>

<span data-ttu-id="275e0-145">[Web の発行機能](xref:tutorials/publish-to-azure-webapp-using-vs)または[組み込みの Git サポート](xref:host-and-deploy/azure-apps/azure-continuous-deployment)を使用して、Visual Studio から Azure App Service にアプリを直接展開することができます。</span><span class="sxs-lookup"><span data-stu-id="275e0-145">By using [the Publish Web feature](xref:tutorials/publish-to-azure-webapp-using-vs) or [built-in Git support](xref:host-and-deploy/azure-apps/azure-continuous-deployment), apps can be deployed directly from Visual Studio to the Azure App Service.</span></span> <span data-ttu-id="275e0-146">Visual Studio Team Services では、[Azure App Service への継続的な展開](/vsts/build-release/apps/cd/azure/aspnet-core-to-azure-webapp?tabs=vsts)がサポートされています。</span><span class="sxs-lookup"><span data-stu-id="275e0-146">Visual Studio Team Services supports [continuous deployment to Azure App Service](/vsts/build-release/apps/cd/azure/aspnet-core-to-azure-webapp?tabs=vsts).</span></span>

## <a name="publishing-to-azure"></a><span data-ttu-id="275e0-147">Azure への発行</span><span class="sxs-lookup"><span data-stu-id="275e0-147">Publishing to Azure</span></span>

<span data-ttu-id="275e0-148">Visual Studio を使用してアプリを Azure に発行する方法については、「[Visual Studio を使用して Azure App Service に ASP.NET Core アプリを発行する](xref:tutorials/publish-to-azure-webapp-using-vs)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="275e0-148">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on how to publish an app to Azure using Visual Studio.</span></span> <span data-ttu-id="275e0-149">Azure へのアプリの発行は、[コマンド ライン](xref:tutorials/publish-to-azure-webapp-using-cli)で行うこともできます。</span><span class="sxs-lookup"><span data-stu-id="275e0-149">The app can also be published to Azure from the [command line](xref:tutorials/publish-to-azure-webapp-using-cli).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="275e0-150">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="275e0-150">Additional resources</span></span>

<span data-ttu-id="275e0-151">ホスティング環境として Docker を使用する方法については、[Docker での ASP.NET Core アプリのホスティング](xref:host-and-deploy/docker/index)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="275e0-151">For information on using Docker as a hosting environment, see [Host ASP.NET Core apps in Docker](xref:host-and-deploy/docker/index).</span></span>
