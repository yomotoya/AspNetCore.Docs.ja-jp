---
title: 'チュートリアル: ASP.NET Core の gRPC の概要'
author: juntaoluo
description: このチュートリアル シリーズでは、ASP.NET Core で gRPC サービスを作成する方法を示します。 gRPC サービス プロジェクトの作成方法、proto ファイルの編集方法、および二重ストリーミング呼び出しの追加方法について学習します。
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 2/26/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: a67050331cc8563b1baf5312b92910c1bbdfbd69
ms.sourcegitcommit: 57a974556acd09363a58f38c26f74dc21e0d4339
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/17/2019
ms.locfileid: "59672568"
---
# <a name="tutorial-get-started-with-grpc-service-in-aspnet-core"></a><span data-ttu-id="68e4f-104">チュートリアル: ASP.NET Core の gRPC サービスの概要</span><span class="sxs-lookup"><span data-stu-id="68e4f-104">Tutorial: Get started with gRPC service in ASP.NET Core</span></span>

<span data-ttu-id="68e4f-105">作成者: [John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="68e4f-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="68e4f-106">このチュートリアルでは、ASP.NET Core で gRPC サービスをビルドする基本について説明します。</span><span class="sxs-lookup"><span data-stu-id="68e4f-106">This tutorial teaches the basics of building a gRPC service on ASP.NET Core.</span></span>

<span data-ttu-id="68e4f-107">最後には、あいさつをエコーする gRPC サービスができます。</span><span class="sxs-lookup"><span data-stu-id="68e4f-107">At the end, you'll have a gRPC service that echoes greetings.</span></span>

[!INCLUDE[View or download sample code](~/includes/grpc/download.md)]

<span data-ttu-id="68e4f-108">このチュートリアルでは、次の作業を行いました。</span><span class="sxs-lookup"><span data-stu-id="68e4f-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="68e4f-109">gRPC サービスを作成します。</span><span class="sxs-lookup"><span data-stu-id="68e4f-109">Create a gRPC service.</span></span>
> * <span data-ttu-id="68e4f-110">gRPC サービスを実行します。</span><span class="sxs-lookup"><span data-stu-id="68e4f-110">Run the gRPC service.</span></span>
> * <span data-ttu-id="68e4f-111">プロジェクト ファイルを確認する</span><span class="sxs-lookup"><span data-stu-id="68e4f-111">Examine the project files.</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="create-a-grpc-service"></a><span data-ttu-id="68e4f-112">gRPC サービスの作成</span><span class="sxs-lookup"><span data-stu-id="68e4f-112">Create a gRPC service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="68e4f-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="68e4f-113">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="68e4f-114">Visual Studio の **[ファイル]** メニューから、**[新規作成]** > **[プロジェクト]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="68e4f-114">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="68e4f-115">新しい ASP.NET Core Web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="68e4f-115">Create a new ASP.NET Core Web Application.</span></span>
  <span data-ttu-id="68e4f-116">![新しい ASP.NET Core Web アプリケーション](grpc-start/_static/np_3_0.1.png)</span><span class="sxs-lookup"><span data-stu-id="68e4f-116">![new ASP.NET Core Web Application](grpc-start/_static/np_3_0.1.png)</span></span>
* <span data-ttu-id="68e4f-117">プロジェクトに **GrpcGreeter** という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="68e4f-117">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="68e4f-118">コードのコピーおよび貼り付けを行う際に名前空間が一致するように、プロジェクトに *GrpcGreeter* という名前を付けることが重要です。</span><span class="sxs-lookup"><span data-stu-id="68e4f-118">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
  <span data-ttu-id="68e4f-119">![新しい ASP.NET Core Web アプリケーション](grpc-start/_static/np_3_0.2.png)</span><span class="sxs-lookup"><span data-stu-id="68e4f-119">![new ASP.NET Core Web Application](grpc-start/_static/np_3_0.2.png)</span></span>
* <span data-ttu-id="68e4f-120">ドロップダウンで **[.NET Core]** と **[ASP.NET Core 3.0]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="68e4f-120">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdown.</span></span> <span data-ttu-id="68e4f-121">**gRPC サービス** テンプレートを選択します。</span><span class="sxs-lookup"><span data-stu-id="68e4f-121">Choose the **gRPC Service** template.</span></span>

  <span data-ttu-id="68e4f-122">次のスターター プロジェクトが作成されます。</span><span class="sxs-lookup"><span data-stu-id="68e4f-122">The following starter project is created:</span></span>

  ![ソリューション エクスプローラー](grpc-start/_static/se3.0.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="68e4f-124">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="68e4f-124">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="68e4f-125">[統合ターミナル](https://code.visualstudio.com/docs/editor/integrated-terminal)を開きます。</span><span class="sxs-lookup"><span data-stu-id="68e4f-125">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="68e4f-126">ディレクトリ (`cd`) を、プロジェクトを格納するフォルダーに変更します。</span><span class="sxs-lookup"><span data-stu-id="68e4f-126">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="68e4f-127">次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="68e4f-127">Run the following commands:</span></span>

  ```console
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * <span data-ttu-id="68e4f-128">`dotnet new` コマンドでは、*GrpcGreeter* フォルダー内に新しい gRPC サービスが作成されます。</span><span class="sxs-lookup"><span data-stu-id="68e4f-128">The `dotnet new` command creates a new gRPC service in the *GrpcGreeter* folder.</span></span>
  * <span data-ttu-id="68e4f-129">`code` コマンドでは、Visual Studio Code の新しいインスタンス内に *GrpcGreeter* フォルダーが開かれます。</span><span class="sxs-lookup"><span data-stu-id="68e4f-129">The `code` command opens the *GrpcGreeter* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="68e4f-130">"**ビルドとデバッグに必要な資産が 'GrpcGreeter' にありません。追加しますか?**" という内容のダイアログ ボックスが表示されたら、</span><span class="sxs-lookup"><span data-stu-id="68e4f-130">A dialog box appears with **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?**</span></span>
* <span data-ttu-id="68e4f-131">**[はい]** を選択します</span><span class="sxs-lookup"><span data-stu-id="68e4f-131">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="68e4f-132">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="68e4f-132">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="68e4f-133">端末から、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="68e4f-133">From a terminal, run the following commands:</span></span>

```console
  dotnet new grpc -o GrpcGreeter
  cd GrpcGreeter
```

<span data-ttu-id="68e4f-134">上記のコマンドでは、[.NET Core CLI](/dotnet/core/tools/dotnet) を使用して、gRPC サービスが作成されます。</span><span class="sxs-lookup"><span data-stu-id="68e4f-134">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a gRPC service.</span></span>

### <a name="open-the-project"></a><span data-ttu-id="68e4f-135">プロジェクトを開く</span><span class="sxs-lookup"><span data-stu-id="68e4f-135">Open the project</span></span>

<span data-ttu-id="68e4f-136">Visual Studio から、**[ファイル]、[開く]** の順に選択し、*GrpcGreeter.sln* ファイルを選択します。</span><span class="sxs-lookup"><span data-stu-id="68e4f-136">From Visual Studio, select **File > Open**, and then select the *GrpcGreeter.sln* file.</span></span>

<!-- End of VS tabs -->

---

### <a name="run-the-service"></a><span data-ttu-id="68e4f-137">サービスを実行する</span><span class="sxs-lookup"><span data-stu-id="68e4f-137">Run the service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="68e4f-138">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="68e4f-138">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="68e4f-139">デバッガーなしで gRPC サービスを実行するには、Ctrl+F5 キーを押します。</span><span class="sxs-lookup"><span data-stu-id="68e4f-139">Press Ctrl+F5 to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="68e4f-140">Visual Studio では、コマンド プロンプトでサービスが実行されます。</span><span class="sxs-lookup"><span data-stu-id="68e4f-140">Visual Studio runs the service in a command prompt.</span></span> <span data-ttu-id="68e4f-141">サービスが `http://localhost:50051` でリッスンを開始したことがログに示されます。</span><span class="sxs-lookup"><span data-stu-id="68e4f-141">The logs show that the service started listening on `http://localhost:50051`.</span></span>

  ![新しい ASP.NET Core Web アプリケーション](grpc-start/_static/server_start.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="68e4f-143">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="68e4f-143">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="68e4f-144">`dotnet run` を使用し、コマンド ラインから gRPC あいさつプロジェクト GrpcGreeter を実行します。</span><span class="sxs-lookup"><span data-stu-id="68e4f-144">Run the gRPC Greeter project GrpcGreeter from the command line using `dotnet run`.</span></span> <span data-ttu-id="68e4f-145">サービスが `http://localhost:50051` でリッスンを開始したことがログに示されます。</span><span class="sxs-lookup"><span data-stu-id="68e4f-145">The logs show that the service started listening on `http://localhost:50051`.</span></span>

```console
dbug: Grpc.AspNetCore.Server.Internal.GrpcServiceBinder[1]
      Added gRPC method 'SayHello' to service 'Greet.Greeter'. Method type: 'Unary', route pattern: '/Greet.Greeter/SayHello'.
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: C:\gh\Docs\aspnetcore\tutorials\grpc\grpc-start\samples\GrpcGreeter
```

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="68e4f-146">次のチュートリアルでは、挨拶サービスをテストするために必要な gRPC クライアントを構築する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="68e4f-146">The next tutorial will demonstrate how to build a gRPC client, which is required to test the Greeter service.</span></span>

### <a name="examine-the-project-files-of-the-grpc-project"></a><span data-ttu-id="68e4f-147">gRPC プロジェクトのプロジェクト ファイルを調査する</span><span class="sxs-lookup"><span data-stu-id="68e4f-147">Examine the project files of the gRPC project</span></span>

<span data-ttu-id="68e4f-148">GrpcGreeter ファイル:</span><span class="sxs-lookup"><span data-stu-id="68e4f-148">GrpcGreeter files:</span></span>

* <span data-ttu-id="68e4f-149">greet.proto:*Protos/greet.proto* ファイルは、`Greeter` gRPC を定義し、gRPC サーバー資産を生成するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="68e4f-149">greet.proto: The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="68e4f-150">詳細については、「<xref:grpc/index>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="68e4f-150">For more information, see <xref:grpc/index>.</span></span>
* <span data-ttu-id="68e4f-151">*Services* フォルダー:`Greeter` サービスの実装が含まれます。</span><span class="sxs-lookup"><span data-stu-id="68e4f-151">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="68e4f-152">*appSettings.json*: Kestrel で使用されるプロトコルなどの構成データが含まれています。</span><span class="sxs-lookup"><span data-stu-id="68e4f-152">*appSettings.json*:Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="68e4f-153">詳細については、「<xref:fundamentals/configuration/index>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="68e4f-153">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="68e4f-154">*Program.cs*:gRPC サービスのエントリ ポイントが含まれています。</span><span class="sxs-lookup"><span data-stu-id="68e4f-154">*Program.cs*: Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="68e4f-155">詳細については、「<xref:fundamentals/host/web-host>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="68e4f-155">For more information, see <xref:fundamentals/host/web-host>.</span></span>
* <span data-ttu-id="68e4f-156">Startup.cs:アプリの動作を構成するコードが含まれています。</span><span class="sxs-lookup"><span data-stu-id="68e4f-156">Startup.cs: Contains code that configures app behavior.</span></span> <span data-ttu-id="68e4f-157">詳細については、「<xref:fundamentals/startup>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="68e4f-157">For more information, see <xref:fundamentals/startup>.</span></span>

### <a name="test-the-service"></a><span data-ttu-id="68e4f-158">サービスをテストする</span><span class="sxs-lookup"><span data-stu-id="68e4f-158">Test the service</span></span>

## <a name="additional-resources"></a><span data-ttu-id="68e4f-159">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="68e4f-159">Additional resources</span></span>

<span data-ttu-id="68e4f-160">このチュートリアルでは、次の作業を行いました。</span><span class="sxs-lookup"><span data-stu-id="68e4f-160">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="68e4f-161">gRPC サービスを作成する。</span><span class="sxs-lookup"><span data-stu-id="68e4f-161">Created a gRPC service.</span></span>
> * <span data-ttu-id="68e4f-162">gRPC サービスを実行しました。</span><span class="sxs-lookup"><span data-stu-id="68e4f-162">Ran the gRPC service.</span></span>
> * <span data-ttu-id="68e4f-163">プロジェクト ファイルを確認する。</span><span class="sxs-lookup"><span data-stu-id="68e4f-163">Examined the project files.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="68e4f-164">次へ: .NET Core gRPC クライアントを作成する</span><span class="sxs-lookup"><span data-stu-id="68e4f-164">Next: Create a .NET Core gRPC client</span></span>](xref:tutorials/grpc/grpc-client)