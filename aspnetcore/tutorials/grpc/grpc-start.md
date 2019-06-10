---
title: ASP.NET Core で .NET Core gRPC のクライアントとサーバーを作成する
author: juntaoluo
description: このチュートリアルでは、ASP.NET Core で gRPC サービスと gRPC クライアントを作成する方法を示します。 gRPC サービス プロジェクトの作成方法、proto ファイルの編集方法、二重ストリーミング呼び出しの追加方法について学習します。
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 06/05/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: 71e3321819eb7169f0896abe3e07849f59ea6fc7
ms.sourcegitcommit: 5dd2ce9709c9e41142771e652d1a4bd0b5248cec
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2019
ms.locfileid: "66692530"
---
# <a name="tutorial-create-a-grpc-client-and-server-in-aspnet-core"></a><span data-ttu-id="a8ec4-104">チュートリアル: ASP.NET Core で gRPC のクライアントとサーバーを作成する</span><span class="sxs-lookup"><span data-stu-id="a8ec4-104">Tutorial: Create a gRPC client and server in ASP.NET Core</span></span>

<span data-ttu-id="a8ec4-105">作成者: [John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="a8ec4-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="a8ec4-106">このチュートリアルでは、.NET Core [gRPC](https://grpc.io/docs/guides/) クライアントと ASP.NET Core gRPC サーバーを作成する方法を紹介します。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-106">This tutorial shows how to create a .NET Core [gRPC](https://grpc.io/docs/guides/) client and an ASP.NET Core gRPC Server.</span></span>

<span data-ttu-id="a8ec4-107">最終的に、gRPC あいさつサービスと通信する gRPC クライアントが与えられます。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-107">At the end, you'll have a gRPC client that communicates with the gRPC Greeter service.</span></span>

<span data-ttu-id="a8ec4-108">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="a8ec4-109">このチュートリアルでは、次の作業を行いました。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a8ec4-110">gRPC サービスを作成する。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-110">Create a gRPC Server.</span></span>
> * <span data-ttu-id="a8ec4-111">gRPC クライアントを作成します。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-111">Create a gRPC client.</span></span>
> * <span data-ttu-id="a8ec4-112">gRPC あいさつサービスで gRPC クライアント サービスをテストする。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-112">Test the gRPC client service with the gRPC Greeter service.</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="create-a-grpc-service"></a><span data-ttu-id="a8ec4-113">gRPC サービスの作成</span><span class="sxs-lookup"><span data-stu-id="a8ec4-113">Create a gRPC service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a8ec4-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a8ec4-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a8ec4-115">Visual Studio の **[ファイル]** メニューから、 **[新規作成]**  >  **[プロジェクト]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="a8ec4-116">**[新しいプロジェクト]** ダイアログで、 **[ASP.NET Core Web アプリケーション]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-116">In the **Create a new project** dialog, select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="a8ec4-117">**[次へ]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-117">Select **Next**</span></span>
* <span data-ttu-id="a8ec4-118">プロジェクトに **GrpcGreeter** という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-118">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="a8ec4-119">コードのコピーおよび貼り付けを行う際に名前空間が一致するように、プロジェクトに *GrpcGreeter* という名前を付けることが重要です。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-119">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
* <span data-ttu-id="a8ec4-120">**[作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-120">Select **Create**</span></span>
* <span data-ttu-id="a8ec4-121">**[新しい ASP.NET Core Web アプリケーションの作成]** ダイアログで:</span><span class="sxs-lookup"><span data-stu-id="a8ec4-121">In the **Create a new ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="a8ec4-122">ドロップダウン メニューで **[.NET Core]** と **[ASP.NET Core 3.0]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-122">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdown menus.</span></span> 
  * <span data-ttu-id="a8ec4-123">**gRPC サービス** テンプレートを選択します。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-123">Select the **gRPC Service** template.</span></span>
  * <span data-ttu-id="a8ec4-124">**[作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-124">Select **Create**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a8ec4-125">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a8ec4-125">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="a8ec4-126">[統合ターミナル](https://code.visualstudio.com/docs/editor/integrated-terminal)を開きます。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-126">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="a8ec4-127">ディレクトリ (`cd`) を、プロジェクトを格納するフォルダーに変更します。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-127">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="a8ec4-128">次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-128">Run the following commands:</span></span>

  ```console
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * <span data-ttu-id="a8ec4-129">`dotnet new` コマンドでは、*GrpcGreeter* フォルダー内に新しい gRPC サービスが作成されます。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-129">The `dotnet new` command creates a new gRPC service in the *GrpcGreeter* folder.</span></span>
  * <span data-ttu-id="a8ec4-130">`code` コマンドでは、Visual Studio Code の新しいインスタンス内に *GrpcGreeter* フォルダーが開かれます。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-130">The `code` command opens the *GrpcGreeter* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="a8ec4-131">"**ビルドとデバッグに必要な資産が 'GrpcGreeter' にありません。追加しますか?** " という内容のダイアログ ボックスが表示されたら、</span><span class="sxs-lookup"><span data-stu-id="a8ec4-131">A dialog box appears with **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?**</span></span>
* <span data-ttu-id="a8ec4-132">**[はい]** を選択します</span><span class="sxs-lookup"><span data-stu-id="a8ec4-132">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a8ec4-133">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="a8ec4-133">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="a8ec4-134">端末から、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-134">From a terminal, run the following commands:</span></span>

```console
  dotnet new grpc -o GrpcGreeter
  cd GrpcGreeter
```

<span data-ttu-id="a8ec4-135">上記のコマンドでは、[.NET Core CLI](/dotnet/core/tools/dotnet) を使用して、gRPC サービスが作成されます。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-135">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a gRPC service.</span></span>

### <a name="open-the-project"></a><span data-ttu-id="a8ec4-136">プロジェクトを開く</span><span class="sxs-lookup"><span data-stu-id="a8ec4-136">Open the project</span></span>

<span data-ttu-id="a8ec4-137">Visual Studio から、 **[ファイル]、[開く]** の順に選択し、*GrpcGreeter.sln* ファイルを選択します。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-137">From Visual Studio, select **File > Open**, and then select the *GrpcGreeter.sln* file.</span></span>

<!-- End of VS tabs -->

---

### <a name="run-the-service"></a><span data-ttu-id="a8ec4-138">サービスを実行する</span><span class="sxs-lookup"><span data-stu-id="a8ec4-138">Run the service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a8ec4-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a8ec4-139">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a8ec4-140">デバッガーなしで gRPC サービスを実行するには、Ctrl+F5 キーを押します。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-140">Press Ctrl+F5 to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="a8ec4-141">Visual Studio では、コマンド プロンプトでサービスが実行されます。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-141">Visual Studio runs the service in a command prompt.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="a8ec4-142">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="a8ec4-142">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="a8ec4-143">`dotnet run` を使用し、コマンド ラインから gRPC あいさつプロジェクト GrpcGreeter を実行します。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-143">Run the gRPC Greeter project GrpcGreeter from the command line using `dotnet run`.</span></span>

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="a8ec4-144">サービスが `http://localhost:50051` でリッスンしていることがログに示されます。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-144">The logs show the service listening on `http://localhost:50051`.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
```

### <a name="examine-the-project-files"></a><span data-ttu-id="a8ec4-145">プロジェクト ファイルを確認する</span><span class="sxs-lookup"><span data-stu-id="a8ec4-145">Examine the project files</span></span>

<span data-ttu-id="a8ec4-146">GrpcGreeter ファイル:</span><span class="sxs-lookup"><span data-stu-id="a8ec4-146">GrpcGreeter files:</span></span>

* <span data-ttu-id="a8ec4-147">*greet.proto*:*Protos/greet.proto* ファイルは、`Greeter` gRPC を定義し、gRPC サーバー資産を生成するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-147">*greet.proto*: The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="a8ec4-148">詳細については、「[gRPC の概要](xref:grpc/index)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-148">For more information, see [Introduction to gRPC](xref:grpc/index).</span></span>
* <span data-ttu-id="a8ec4-149">*Services* フォルダー:`Greeter` サービスの実装が含まれます。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-149">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="a8ec4-150">*appSettings.json*:Kestrel で使用されるプロトコルなどの構成データが含まれています。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-150">*appSettings.json*: Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="a8ec4-151">詳細については、「<xref:fundamentals/configuration/index>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-151">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="a8ec4-152">*Program.cs*:gRPC サービスのエントリ ポイントが含まれています。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-152">*Program.cs*: Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="a8ec4-153">詳細については、「<xref:fundamentals/host/web-host>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-153">For more information, see <xref:fundamentals/host/web-host>.</span></span>
* <span data-ttu-id="a8ec4-154">*Startup.cs*:アプリの動作を構成するコードが含まれています。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-154">*Startup.cs*: Contains code that configures app behavior.</span></span> <span data-ttu-id="a8ec4-155">詳細については、[アプリの Startup](xref:fundamentals/startup)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-155">For more information, see [App startup](xref:fundamentals/startup).</span></span>

## <a name="create-the-grpc-client-in-a-net-console-app"></a><span data-ttu-id="a8ec4-156">.NET コンソール アプリで gRPC クライアントを作成する</span><span class="sxs-lookup"><span data-stu-id="a8ec4-156">Create the gRPC client in a .NET console app</span></span>

## <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a8ec4-157">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a8ec4-157">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a8ec4-158">**[ファイル]**  >  **[新規作成]**  >  **[プロジェクト]** をメニュー バーから選択します。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-158">Select **File** > **New** > **Project** from the menu bar.</span></span>
* <span data-ttu-id="a8ec4-159">**[新しいプロジェクトの作成]** ダイアログで、 **[コンソール アプリ (.NET Core)]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-159">In the **Create a new project** dialog, select **Console App (.NET Core)**.</span></span>
* <span data-ttu-id="a8ec4-160">**[次へ]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-160">Select **Next**</span></span>
* <span data-ttu-id="a8ec4-161">**[名前]** テキスト ボックスに、「GrpcGreeterClient」を入力します。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-161">In the **Name** text box, enter "GrpcGreeterClient".</span></span>
* <span data-ttu-id="a8ec4-162">**[作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-162">Select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a8ec4-163">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a8ec4-163">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="a8ec4-164">[統合ターミナル](https://code.visualstudio.com/docs/editor/integrated-terminal)を開きます。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-164">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="a8ec4-165">ディレクトリ (`cd`) を、プロジェクトを格納するフォルダーに変更します。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-165">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="a8ec4-166">次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-166">Run the following commands:</span></span>

```console
dotnet new console -o GrpcGreeterClient
code -r GrpcGreeterClient
```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a8ec4-167">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="a8ec4-167">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="a8ec4-168">[こちら](/dotnet/core/tutorials/using-on-mac-vs-full-solution)の指示に従い、*GrpcGreeterClient* という名前でコンソール アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-168">Follow the instructions [here](/dotnet/core/tutorials/using-on-mac-vs-full-solution) to create a console app with the name *GrpcGreeterClient*.</span></span>

<!-- End of VS tabs -->

---

### <a name="add-required-packages"></a><span data-ttu-id="a8ec4-169">必要なパッケージを追加する</span><span class="sxs-lookup"><span data-stu-id="a8ec4-169">Add required packages</span></span>

<span data-ttu-id="a8ec4-170">次のパッケージを gRPC クライアント プロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-170">Add the following packages to the gRPC client project:</span></span>

* <span data-ttu-id="a8ec4-171">[Grpc.Core](https://www.nuget.org/packages/Grpc.Core)。これには C-core クライアント用の C# API が含まれています。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-171">[Grpc.Core](https://www.nuget.org/packages/Grpc.Core), which contains the C# API for the C-core client.</span></span>
* <span data-ttu-id="a8ec4-172">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/)。これに C# の protobuf メッセージ API が含まれています。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-172">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), which contains protobuf message APIs for C#.</span></span>
* <span data-ttu-id="a8ec4-173">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/)。これには protobuf ファイルの C# ツール サポートが含まれています。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-173">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), which contains C# tooling support for protobuf files.</span></span> <span data-ttu-id="a8ec4-174">ツール パッケージは実行時に不要であり、依存関係には `PrivateAssets="All"` のマークが付きます。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-174">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`.</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a8ec4-175">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a8ec4-175">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a8ec4-176">パッケージ マネージャー コンソール (PMC) または NuGet パッケージの管理を使用してパッケージをインストールする</span><span class="sxs-lookup"><span data-stu-id="a8ec4-176">Install the packages using either the Package Manager Console (PMC) or Manage NuGet Package</span></span>

#### <a name="pmc-option-to-install-packages"></a><span data-ttu-id="a8ec4-177">パッケージをインストールするための PMC オプション</span><span class="sxs-lookup"><span data-stu-id="a8ec4-177">PMC option to install packages</span></span>

* <span data-ttu-id="a8ec4-178">Visual Studio で **[ツール]** 、 **[NuGet パッケージ マネージャー]** 、 **[パッケージ マネージャー コンソール]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-178">From Visual Studio, select **Tools** > **NuGet Package Manager** > **Package Manager Console**</span></span>
* <span data-ttu-id="a8ec4-179">**[パッケージ マネージャー コンソール]** ウィンドウから、*GrpcGreeterClient.csproj* ファイルがあるディレクトリに移動します。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-179">From the **Package Manager Console** window, navigate to the directory in which the *GrpcGreeterClient.csproj* file exists.</span></span>
* <span data-ttu-id="a8ec4-180">次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-180">Run the following commands:</span></span>

 ```powershell
Install-Package Grpc.Core
Install-Package Google.Protobuf
Install-Package Grpc.Tools
```

#### <a name="manage-nuget-packages-option-to-install-packages"></a><span data-ttu-id="a8ec4-181">パッケージをインストールするための [NuGet パッケージの管理] オプション</span><span class="sxs-lookup"><span data-stu-id="a8ec4-181">Manage NuGet Packages option to install packages</span></span>

* <span data-ttu-id="a8ec4-182">**[ソリューション エクスプローラー]**  >  **[NuGet パッケージの管理]** でプロジェクトを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-182">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
* <span data-ttu-id="a8ec4-183">**[参照]** タブを選択します。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-183">Select the **Browse** tab.</span></span>
* <span data-ttu-id="a8ec4-184">検索ボックスに「**Grpc.Core**」と入力します。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-184">Enter **Grpc.Core** in the search box.</span></span>
* <span data-ttu-id="a8ec4-185">**[参照]** タブから **Grpc.Core** パッケージを選択し、 **[インストール]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-185">Select the **Grpc.Core** package from the **Browse** tab and select **Install**.</span></span>
* <span data-ttu-id="a8ec4-186">`Google.Protobuf` と `Grpc.Tools` に同じ手順を繰り返します。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-186">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a8ec4-187">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a8ec4-187">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="a8ec4-188">**統合ターミナル**から次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-188">Run the following commands from the **Integrated Terminal**:</span></span>

```console
dotnet add GrpcGreeterClient.csproj package Grpc.Core
dotnet add GrpcGreeterClient.csproj package Google.Protobuf
dotnet add GrpcGreeterClient.csproj package Grpc.Tools
```

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a8ec4-189">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="a8ec4-189">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="a8ec4-190">**[Solution Pad]**  >  **[パッケージを追加]** で **[パッケージ]** フォルダーを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-190">Right-click the **Packages** folder in **Solution Pad** > **Add Packages**</span></span>
* <span data-ttu-id="a8ec4-191">検索ボックスに「**Grpc.Core**」と入力します。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-191">Enter **Grpc.Core** in the search box.</span></span>
* <span data-ttu-id="a8ec4-192">結果ウィンドウから **Grpc.Core** パッケージを選択し、 **[パッケージを追加]** を選択します</span><span class="sxs-lookup"><span data-stu-id="a8ec4-192">Select the **Grpc.Core** package from the results pane and select **Add Package**</span></span>
* <span data-ttu-id="a8ec4-193">`Google.Protobuf` と `Grpc.Tools` に同じ手順を繰り返します。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-193">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

---

### <a name="add-greetproto"></a><span data-ttu-id="a8ec4-194">greet.proto を追加する</span><span class="sxs-lookup"><span data-stu-id="a8ec4-194">Add greet.proto</span></span>

* <span data-ttu-id="a8ec4-195">gRPC クライアント プロジェクトで **Protos** フォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-195">Create a **Protos** folder in the gRPC client project.</span></span>
* <span data-ttu-id="a8ec4-196">gRPC あいさつサービスから gRPC クライアント プロジェクトに **Protos\greet.proto** ファイルをコピーします。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-196">Copy the **Protos\greet.proto** file from the gRPC Greeter service to the gRPC client project.</span></span>
* <span data-ttu-id="a8ec4-197">*GrpcGreeterClient.csproj* プロジェクト ファイルを編集します。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-197">Edit the *GrpcGreeterClient.csproj* project file:</span></span>

  # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a8ec4-198">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a8ec4-198">Visual Studio</span></span>](#tab/visual-studio) 

  <span data-ttu-id="a8ec4-199">プロジェクトを右クリックして、 **[Edit GrpcGreeterClient.csproj]\(GrpcGreeterClient.csproj の編集\)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-199">Right-click the project and select the **Edit GrpcGreeterClient.csproj**.</span></span>

  # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a8ec4-200">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a8ec4-200">Visual Studio Code</span></span>](#tab/visual-studio-code) 

  <span data-ttu-id="a8ec4-201">*GrpcGreeterClient.csproj* ファイルを選択します。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-201">Select the *GrpcGreeterClient.csproj* file.</span></span>

  # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a8ec4-202">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="a8ec4-202">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  <span data-ttu-id="a8ec4-203">プロジェクトを右クリックし、 **[ツール]、[ファイルの編集]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-203">Right click the project and select **Tools > Edit File**.</span></span>

  ---

* <span data-ttu-id="a8ec4-204">GrpcGreeterClient プロジェクト ファイルの `<Protobuf>` 項目グループに **greet.proto** ファイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-204">Add the **greet.proto** file to the `<Protobuf>` item group of the GrpcGreeterClient project file:</span></span>

  ```XML
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```

<span data-ttu-id="a8ec4-205">クライアント プロジェクトをビルドし、C# クライアント アセットの生成をトリガーします。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-205">Build the client project to trigger the generation of the C# client assets.</span></span>

### <a name="create-the-greeter-client"></a><span data-ttu-id="a8ec4-206">Greeter クライアントを作成する</span><span class="sxs-lookup"><span data-stu-id="a8ec4-206">Create the Greeter client</span></span>

<span data-ttu-id="a8ec4-207">プロジェクトをビルドして **Greeter** 名前空間内に型を作成します。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-207">Build the project to create the types in the **Greeter** namespace.</span></span> <span data-ttu-id="a8ec4-208">`Greeter` 型は、ビルド プロセスによって自動的に生成されます。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-208">The `Greeter` types are generated automatically by the build process.</span></span>

<span data-ttu-id="a8ec4-209">次のコードを使用して、gRPC クライアントの *Program.cs* ファイルを更新します。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-209">Update the gRPC client *Program.cs* file with the following code:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet2)]

<span data-ttu-id="a8ec4-210">*Program.cs* には、gRPC クライアントのエントリ ポイントとロジックが含まれています。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-210">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

<span data-ttu-id="a8ec4-211">Greeter クライアントは、次の方法で作成されます。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-211">The Greeter client is created by:</span></span>

* <span data-ttu-id="a8ec4-212">gRPC サービスへの接続を作成するための情報が含まれている `Channel` をインスタンス化する。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-212">Instantiating a `Channel` containing the information for creating the connection to the gRPC service.</span></span>
* <span data-ttu-id="a8ec4-213">`Channel` を使用して、Greeter クライアントを構築します。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-213">Using the `Channel` to construct the Greeter client:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=4-6)]

<span data-ttu-id="a8ec4-214">Greeter クライアントから非同期の `SayHello` メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-214">The Greeter client calls the asynchronous `SayHello` method.</span></span> <span data-ttu-id="a8ec4-215">`SayHello` 呼び出しの結果が表示されます。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-215">The result of the `SayHello` call is displayed:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=7-9)]

<span data-ttu-id="a8ec4-216">操作が完了してすべてのリソースが解放されたときに、クライアントによって使用される `Channel` をシャットダウンします。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-216">Shut down the `Channel` used by the client when operations have finished to release all resources.</span></span>

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a><span data-ttu-id="a8ec4-217">gRPC あいさつサービスで gRPC クライアントをテストする</span><span class="sxs-lookup"><span data-stu-id="a8ec4-217">Test the gRPC client with the gRPC Greeter service</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a8ec4-218">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a8ec4-218">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a8ec4-219">あいさつサービスで、Ctrl キーを押しながら F5 キーを押して、デバッガーなしでサーバーを起動します。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-219">In the Greeter service, press Ctrl+F5 to start the server without the debugger.</span></span>
* <span data-ttu-id="a8ec4-220">GrpcGreeterClient プロジェクトで、Ctrl キーを押しながら F5 キーを押して、デバッガーなしでサーバーを起動します。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-220">In the GrpcGreeterClient project, press Ctrl+F5 to start the server without the debugger.</span></span>

### <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="a8ec4-221">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="a8ec4-221">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="a8ec4-222">あいさつサービスを開始します。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-222">Start the Greeter service.</span></span>
* <span data-ttu-id="a8ec4-223">クライアントを起動します。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-223">Start the client.</span></span>

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="a8ec4-224">クライアントがその名前 "GreeterClient" を含むメッセージを使用して、サービスにあいさつを送信します。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-224">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="a8ec4-225">サービスから応答として "Hello GreeterClient" のメッセージが送信されます。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-225">The service sends the message "Hello GreeterClient" as a response.</span></span> <span data-ttu-id="a8ec4-226">"Hello GreeterClient" の応答がコマンド プロンプトに表示されます。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-226">The "Hello GreeterClient" response is displayed in the command prompt:</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="a8ec4-227">gRPC サービスは、成功した呼び出しの詳細をコマンド プロンプトに書き込まれるログに記録します。</span><span class="sxs-lookup"><span data-stu-id="a8ec4-227">The gRPC service records the details of the successful call in the logs written to the command prompt.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: C:\GH\aspnet\docs\4\Docs\aspnetcore\tutorials\grpc\grpc-start\sample\GrpcGreeter
info: Microsoft.AspNetCore.Hosting.Diagnostics[1]
      Request starting HTTP/2 POST http://localhost:50051/Greet.Greeter/SayHello application/grpc
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[0]
      Executing endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[1]
      Executed endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Hosting.Diagnostics[2]
      Request finished in 78.32260000000001ms 200 application/grpc
```

### <a name="next-steps"></a><span data-ttu-id="a8ec4-228">次の手順</span><span class="sxs-lookup"><span data-stu-id="a8ec4-228">Next steps</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
