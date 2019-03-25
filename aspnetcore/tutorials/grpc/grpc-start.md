---
title: 'チュートリアル: ASP.NET Core の gRPC の概要'
author: juntaoluo
description: このチュートリアル シリーズでは、ASP.NET Core で gRPC サービスを作成する方法を示します。 gRPC サービス プロジェクトの作成方法、proto ファイルの編集方法、および二重ストリーミング呼び出しの追加方法について学習します。
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 2/26/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: 5f7a2f6b57804b3295b23c322dcbac553b05528b
ms.sourcegitcommit: 088e6744cd67a62f214f25146313a53949b17d35
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/21/2019
ms.locfileid: "58320057"
---
# <a name="tutorial-get-started-with-grpc-service-in-aspnet-core"></a><span data-ttu-id="d3fc7-104">チュートリアル: ASP.NET Core の gRPC サービスの概要</span><span class="sxs-lookup"><span data-stu-id="d3fc7-104">Tutorial: Get started with gRPC service in ASP.NET Core</span></span>

<span data-ttu-id="d3fc7-105">作成者: [John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="d3fc7-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="d3fc7-106">このチュートリアルでは、ASP.NET Core で gRPC サービスをビルドする基本について説明します。</span><span class="sxs-lookup"><span data-stu-id="d3fc7-106">This tutorial teaches the basics of building a gRPC service on ASP.NET Core.</span></span>

<span data-ttu-id="d3fc7-107">最後には、あいさつをエコーする gRPC サービスができます。</span><span class="sxs-lookup"><span data-stu-id="d3fc7-107">At the end, you'll have a gRPC service that echoes greetings.</span></span>

[!INCLUDE[View or download sample code](~/includes/grpc/download.md)]

<span data-ttu-id="d3fc7-108">このチュートリアルでは、次の作業を行いました。</span><span class="sxs-lookup"><span data-stu-id="d3fc7-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d3fc7-109">gRPC サービスを作成します。</span><span class="sxs-lookup"><span data-stu-id="d3fc7-109">Create a gRPC service.</span></span>
> * <span data-ttu-id="d3fc7-110">サービスを実行します。</span><span class="sxs-lookup"><span data-stu-id="d3fc7-110">Run the service.</span></span>
> * <span data-ttu-id="d3fc7-111">プロジェクト ファイルを確認する</span><span class="sxs-lookup"><span data-stu-id="d3fc7-111">Examine the project files.</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="create-a-grpc-service"></a><span data-ttu-id="d3fc7-112">gRPC サービスの作成</span><span class="sxs-lookup"><span data-stu-id="d3fc7-112">Create a gRPC service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d3fc7-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d3fc7-113">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d3fc7-114">Visual Studio の **[ファイル]** メニューから、**[新規作成]** > **[プロジェクト]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="d3fc7-114">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="d3fc7-115">新しい ASP.NET Core Web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="d3fc7-115">Create a new ASP.NET Core Web Application.</span></span>
  <span data-ttu-id="d3fc7-116">![新しい ASP.NET Core Web アプリケーション](grpc-start/_static/np_3_0.1.png)</span><span class="sxs-lookup"><span data-stu-id="d3fc7-116">![new ASP.NET Core Web Application](grpc-start/_static/np_3_0.1.png)</span></span>
* <span data-ttu-id="d3fc7-117">プロジェクトに **GrpcGreeter** という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="d3fc7-117">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="d3fc7-118">コードのコピーおよび貼り付けを行う際に名前空間が一致するように、プロジェクトに *GrpcGreeter* という名前を付けることが重要です。</span><span class="sxs-lookup"><span data-stu-id="d3fc7-118">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
  <span data-ttu-id="d3fc7-119">![新しい ASP.NET Core Web アプリケーション](grpc-start/_static/np_3_0.2.png)</span><span class="sxs-lookup"><span data-stu-id="d3fc7-119">![new ASP.NET Core Web Application](grpc-start/_static/np_3_0.2.png)</span></span>
* <span data-ttu-id="d3fc7-120">ドロップダウンで **[.NET Core]** と **[ASP.NET Core 3.0]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="d3fc7-120">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdown.</span></span> <span data-ttu-id="d3fc7-121">**gRPC サービス** テンプレートを選択します。</span><span class="sxs-lookup"><span data-stu-id="d3fc7-121">Choose the **gRPC Service** template.</span></span>

  <span data-ttu-id="d3fc7-122">次のスターター プロジェクトが作成されます。</span><span class="sxs-lookup"><span data-stu-id="d3fc7-122">The following starter project is created:</span></span>

  ![ソリューション エクスプローラー](grpc-start/_static/se3.0.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d3fc7-124">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d3fc7-124">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="d3fc7-125">[統合ターミナル](https://code.visualstudio.com/docs/editor/integrated-terminal)を開きます。</span><span class="sxs-lookup"><span data-stu-id="d3fc7-125">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="d3fc7-126">ディレクトリ (`cd`) を、プロジェクトを格納するフォルダーに変更します。</span><span class="sxs-lookup"><span data-stu-id="d3fc7-126">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="d3fc7-127">次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="d3fc7-127">Run the following commands:</span></span>

  ```console
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * <span data-ttu-id="d3fc7-128">`dotnet new` コマンドでは、*GrpcGreeter* フォルダー内に新しい gRPC サービスが作成されます。</span><span class="sxs-lookup"><span data-stu-id="d3fc7-128">The `dotnet new` command creates a new gRPC service in the *GrpcGreeter* folder.</span></span>
  * <span data-ttu-id="d3fc7-129">`code` コマンドでは、Visual Studio Code の新しいインスタンス内に *GrpcGreeter* フォルダーが開かれます。</span><span class="sxs-lookup"><span data-stu-id="d3fc7-129">The `code` command opens the *GrpcGreeter* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="d3fc7-130">"**ビルドとデバッグに必要な資産が 'GrpcGreeter' にありません。追加しますか?**" という内容のダイアログ ボックスが表示されたら、</span><span class="sxs-lookup"><span data-stu-id="d3fc7-130">A dialog box appears with **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?**</span></span>
* <span data-ttu-id="d3fc7-131">**[はい]** を選択します</span><span class="sxs-lookup"><span data-stu-id="d3fc7-131">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d3fc7-132">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="d3fc7-132">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="d3fc7-133">端末から、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="d3fc7-133">From a terminal, run the following commands:</span></span>

```console
  dotnet new grpc -o GrpcGreeter
  cd GrpcGreeter
```

<span data-ttu-id="d3fc7-134">上記のコマンドでは、[.NET Core CLI](/dotnet/core/tools/dotnet) を使用して、gRPC サービスが作成されます。</span><span class="sxs-lookup"><span data-stu-id="d3fc7-134">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a gRPC service.</span></span>

### <a name="open-the-project"></a><span data-ttu-id="d3fc7-135">プロジェクトを開く</span><span class="sxs-lookup"><span data-stu-id="d3fc7-135">Open the project</span></span>

<span data-ttu-id="d3fc7-136">Visual Studio から、**[ファイル]、[開く]** の順に選択し、*GrpcGreeter.sln* ファイルを選択します。</span><span class="sxs-lookup"><span data-stu-id="d3fc7-136">From Visual Studio, select **File > Open**, and then select the *GrpcGreeter.sln* file.</span></span>

<!-- End of VS tabs -->

---

### <a name="test-the-service"></a><span data-ttu-id="d3fc7-137">サービスをテストする</span><span class="sxs-lookup"><span data-stu-id="d3fc7-137">Test the service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d3fc7-138">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d3fc7-138">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d3fc7-139">**GrpcGreeter.Server** がスタートアップ プロジェクトとして設定されていることを確認し、Ctrl + F5 キーを押してデバッガーなしで gRPC サービスを実行します。</span><span class="sxs-lookup"><span data-stu-id="d3fc7-139">Ensure the **GrpcGreeter.Server** is set as the Startup Project and press Ctrl+F5 to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="d3fc7-140">Visual Studio では、コマンド プロンプトでサービスが実行されます。</span><span class="sxs-lookup"><span data-stu-id="d3fc7-140">Visual Studio runs the service in a command prompt.</span></span> <span data-ttu-id="d3fc7-141">サービスが `http://localhost:50051` でリッスンを開始したことがログに示されます。</span><span class="sxs-lookup"><span data-stu-id="d3fc7-141">The logs show that the service started listening on `http://localhost:50051`.</span></span>

  ![新しい ASP.NET Core Web アプリケーション](grpc-start/_static/server_start.png)

* <span data-ttu-id="d3fc7-143">サービスが実行されたら、**GrpcGreeter.Client** をスタートアップ プロジェクトとして設定し、Ctrl + F5 キーを押してデバッガーなしでクライアントを実行します。</span><span class="sxs-lookup"><span data-stu-id="d3fc7-143">Once the service is running, set the **GrpcGreeter.Client** is set as the Startup Project and press Ctrl+F5 to run the client without the debugger.</span></span>

  <span data-ttu-id="d3fc7-144">クライアントがその名前 "GreeterClient" を含むメッセージを使用して、サービスにあいさつを送信します。</span><span class="sxs-lookup"><span data-stu-id="d3fc7-144">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="d3fc7-145">サービスはコマンド プロンプトに表示される応答として、メッセージ "Hello GreeterClient" を送信します。</span><span class="sxs-lookup"><span data-stu-id="d3fc7-145">The service will send a message "Hello GreeterClient" as a response that is displayed in the command prompt.</span></span>

  ![新しい ASP.NET Core Web アプリケーション](grpc-start/_static/client.png)

  <span data-ttu-id="d3fc7-147">サービスは、成功した呼び出しの詳細をコマンド プロンプトに書き込まれるログに記録します。</span><span class="sxs-lookup"><span data-stu-id="d3fc7-147">The service records the details of the successful call in the logs written to the command prompt.</span></span>

  ![新しい ASP.NET Core Web アプリケーション](grpc-start/_static/server_complete.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="d3fc7-149">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="d3fc7-149">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="d3fc7-150">`dotnet run` を使用して、コマンド ラインからサーバー プロジェクト GrpcGreeter.Server を実行します。</span><span class="sxs-lookup"><span data-stu-id="d3fc7-150">Run the Server project GrpcGreeter.Server from the command line using `dotnet run`.</span></span> <span data-ttu-id="d3fc7-151">サービスが `http://localhost:50051` でリッスンを開始したことがログに示されます。</span><span class="sxs-lookup"><span data-stu-id="d3fc7-151">The logs show that the service started listening on `http://localhost:50051`.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: C:\example\GrpcGreeter\GrpcGreeter.Server
```

* <span data-ttu-id="d3fc7-152">`dotnet run` を使用して、コマンド ラインからクライアント プロジェクト GrpcGreeter.Client を実行します。</span><span class="sxs-lookup"><span data-stu-id="d3fc7-152">Run the Client project GrpcGreeter.Client from the separate command line using `dotnet run`.</span></span>

<span data-ttu-id="d3fc7-153">クライアントがその名前 "GreeterClient" を含むメッセージを使用して、サービスにあいさつを送信します。</span><span class="sxs-lookup"><span data-stu-id="d3fc7-153">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="d3fc7-154">サービスはコマンド プロンプトに表示される応答として、メッセージ "Hello GreeterClient" を送信します。</span><span class="sxs-lookup"><span data-stu-id="d3fc7-154">The service will send a message "Hello GreeterClient" as a response that is displayed in the command prompt.</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="d3fc7-155">サービスは、成功した呼び出しの詳細をコマンド プロンプトに書き込まれるログに記録します。</span><span class="sxs-lookup"><span data-stu-id="d3fc7-155">The service records the details of the successful call in the logs written to the command prompt.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: C:\gh\tp\GrpcGreeter\GrpcGreeter.Server
info: Microsoft.AspNetCore.Hosting.Internal.GenericWebHostService[1]
      Request starting HTTP/2 POST http://localhost:50051/Greet.Greeter/SayHello application/grpc
info: Microsoft.AspNetCore.Hosting.Internal.GenericWebHostService[2]
      Request finished in 107.46730000000001ms 200 application/grpc
```

<!-- End of combined VS/Mac tabs -->

---

### <a name="examine-the-project-files-of-the-grpc-project"></a><span data-ttu-id="d3fc7-156">gRPC プロジェクトのプロジェクト ファイルを調査する</span><span class="sxs-lookup"><span data-stu-id="d3fc7-156">Examine the project files of the gRPC project</span></span>

<span data-ttu-id="d3fc7-157">GrpcGreeter.Server ファイル:</span><span class="sxs-lookup"><span data-stu-id="d3fc7-157">GrpcGreeter.Server files:</span></span>

* <span data-ttu-id="d3fc7-158">greet.proto:*Protos/greet.proto* ファイルは、`Greeter` gRPC を定義し、gRPC サーバー資産を生成するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="d3fc7-158">greet.proto: The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="d3fc7-159">詳細については、「<xref:grpc/index>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="d3fc7-159">For more information, see <xref:grpc/index>.</span></span>
* <span data-ttu-id="d3fc7-160">*Services* フォルダー:`Greeter` サービスの実装が含まれます。</span><span class="sxs-lookup"><span data-stu-id="d3fc7-160">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="d3fc7-161">*appSettings.json*: Kestrel で使用されるプロトコルなどの構成データが含まれています。</span><span class="sxs-lookup"><span data-stu-id="d3fc7-161">*appSettings.json*:Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="d3fc7-162">詳細については、「<xref:fundamentals/configuration/index>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="d3fc7-162">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="d3fc7-163">*Program.cs*:gRPC サービスのエントリ ポイントが含まれています。</span><span class="sxs-lookup"><span data-stu-id="d3fc7-163">*Program.cs*: Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="d3fc7-164">詳細については、「<xref:fundamentals/host/web-host>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="d3fc7-164">For more information, see <xref:fundamentals/host/web-host>.</span></span>
* <span data-ttu-id="d3fc7-165">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="d3fc7-165">Startup.cs</span></span>

<span data-ttu-id="d3fc7-166">アプリの動作を構成するコードが含まれています。</span><span class="sxs-lookup"><span data-stu-id="d3fc7-166">Contains code that configures app behavior.</span></span> <span data-ttu-id="d3fc7-167">詳細については、「<xref:fundamentals/startup>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="d3fc7-167">For more information, see <xref:fundamentals/startup>.</span></span>

<span data-ttu-id="d3fc7-168">gRPC クライアントの GrpcGreeter.Client ファイル:</span><span class="sxs-lookup"><span data-stu-id="d3fc7-168">gRPC client GrpcGreeter.Client file:</span></span>

<span data-ttu-id="d3fc7-169">*Program.cs* には、gRPC クライアントのエントリ ポイントとロジックが含まれています。</span><span class="sxs-lookup"><span data-stu-id="d3fc7-169">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

<span data-ttu-id="d3fc7-170">このチュートリアルでは、次の作業を行いました。</span><span class="sxs-lookup"><span data-stu-id="d3fc7-170">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d3fc7-171">gRPC サービスを作成する。</span><span class="sxs-lookup"><span data-stu-id="d3fc7-171">Created a gRPC service.</span></span>
> * <span data-ttu-id="d3fc7-172">サービスとクライアントを実行してサービスをテストする。</span><span class="sxs-lookup"><span data-stu-id="d3fc7-172">Ran the service and a client to test the service.</span></span>
> * <span data-ttu-id="d3fc7-173">プロジェクト ファイルを確認する。</span><span class="sxs-lookup"><span data-stu-id="d3fc7-173">Examined the project files.</span></span>
