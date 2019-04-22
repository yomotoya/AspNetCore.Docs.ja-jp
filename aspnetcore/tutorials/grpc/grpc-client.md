---
title: 'チュートリアル: .NET Core gRPC クライアントを作成する'
author: juntaoluo
description: このチュートリアル シリーズでは、ASP.NET Core で gRPC サービスを作成する方法を示します。 gRPC サービス プロジェクトの作成方法、proto ファイルの編集方法、二重ストリーミング呼び出しの追加方法について学習します。
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 4/10/2019
uid: tutorials/grpc/grpc-client
ms.openlocfilehash: 031afbfaf097c518a85400b0b6abbc135c1bc611
ms.sourcegitcommit: 57a974556acd09363a58f38c26f74dc21e0d4339
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/17/2019
ms.locfileid: "59674164"
---
# <a name="tutorial-create-a-net-core-grpc-client"></a><span data-ttu-id="4aeb6-104">チュートリアル: .NET Core gRPC クライアントを作成する</span><span class="sxs-lookup"><span data-stu-id="4aeb6-104">Tutorial: Create a .NET Core gRPC client</span></span>

<span data-ttu-id="4aeb6-105">作成者: [John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="4aeb6-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="4aeb6-106">このチュートリアルでは、gRPC サービスと通信できる .NET Core [gRPC](https://grpc.io/docs/guides/) クライアントを作成する方法を紹介します。</span><span class="sxs-lookup"><span data-stu-id="4aeb6-106">This tutorial shows how to create a .NET Core [gRPC](https://grpc.io/docs/guides/) client that can communicate with gRPC services.</span></span>

<span data-ttu-id="4aeb6-107">最終的に、gRPC あいさつサービスと通信する gRPC クライアントが与えられます。</span><span class="sxs-lookup"><span data-stu-id="4aeb6-107">At the end, you'll have a gRPC client that communicates with the gRPC Greeter service.</span></span>

[!INCLUDE[View or download sample code](~/includes/grpc/downloadClient.md)]

<span data-ttu-id="4aeb6-108">このチュートリアルでは、次の作業を行いました。</span><span class="sxs-lookup"><span data-stu-id="4aeb6-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="4aeb6-109">gRPC クライアントを作成します。</span><span class="sxs-lookup"><span data-stu-id="4aeb6-109">Create a gRPC client.</span></span>
> * <span data-ttu-id="4aeb6-110">このサービスを前のチュートリアルで作成した gRPC あいさつサービスに対して実行します。</span><span class="sxs-lookup"><span data-stu-id="4aeb6-110">Run the service against the gRPC Greeter service created in the previous tutorial.</span></span>
> * <span data-ttu-id="4aeb6-111">プロジェクト ファイルを確認する</span><span class="sxs-lookup"><span data-stu-id="4aeb6-111">Examine the project files.</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="create-a-net-console-application"></a><span data-ttu-id="4aeb6-112">.NET コンソール アプリケーションを作成する</span><span class="sxs-lookup"><span data-stu-id="4aeb6-112">Create a .NET console application</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4aeb6-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4aeb6-113">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="4aeb6-114">[こちら](https://docs.microsoft.com/en-us/dotnet/core/tutorials/with-visual-studio)の指示に従い、*GrpcGreeterClient* という名前でコンソール アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="4aeb6-114">Follow the instructions [here](https://docs.microsoft.com/en-us/dotnet/core/tutorials/with-visual-studio) to create a console app with the name *GrpcGreeterClient*.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4aeb6-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4aeb6-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="4aeb6-116">[こちら](https://docs.microsoft.com/en-us/dotnet/core/tutorials/with-visual-studio-code)の指示に従い、*GrpcGreeterClient* という名前でコンソール アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="4aeb6-116">Follow the instructions [here](https://docs.microsoft.com/en-us/dotnet/core/tutorials/with-visual-studio-code) to create a console app with the name *GrpcGreeterClient*.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="4aeb6-117">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="4aeb6-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="4aeb6-118">[こちら](https://docs.microsoft.com/en-us/dotnet/core/tutorials/using-on-mac-vs-full-solution)の指示に従い、*GrpcGreeterClient* という名前でコンソール アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="4aeb6-118">Follow the instructions [here](https://docs.microsoft.com/en-us/dotnet/core/tutorials/using-on-mac-vs-full-solution) to create a console app with the name *GrpcGreeterClient*.</span></span>

<!-- End of VS tabs -->

---

## <a name="add-required-packages"></a><span data-ttu-id="4aeb6-119">必要なパッケージを追加する</span><span class="sxs-lookup"><span data-stu-id="4aeb6-119">Add required packages</span></span>

<span data-ttu-id="4aeb6-120">次のパッケージを gRPC クライアント プロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="4aeb6-120">Add the following packages to the gRPC client project:</span></span>

* <span data-ttu-id="4aeb6-121">[Grpc.Core](https://www.nuget.org/packages/Grpc.Core)。これには C-core クライアント用の C# API が含まれています。</span><span class="sxs-lookup"><span data-stu-id="4aeb6-121">[Grpc.Core](https://www.nuget.org/packages/Grpc.Core), which contains the C# API for the C-core client.</span></span>
* <span data-ttu-id="4aeb6-122">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/)。これに C# の protobuf メッセージ API が含まれています。</span><span class="sxs-lookup"><span data-stu-id="4aeb6-122">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), which contains protobuf message APIs for C#.</span></span>
* <span data-ttu-id="4aeb6-123">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/)。これには protobuf ファイルの C# ツール サポートが含まれています。</span><span class="sxs-lookup"><span data-stu-id="4aeb6-123">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), which contains C# tooling support for protobuf files.</span></span> <span data-ttu-id="4aeb6-124">ツール パッケージは実行時に不要であり、依存関係には `PrivateAssets="All"` のマークが付きます。</span><span class="sxs-lookup"><span data-stu-id="4aeb6-124">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`.</span></span>

<span data-ttu-id="4aeb6-125">パッケージは次の方法で追加できます。</span><span class="sxs-lookup"><span data-stu-id="4aeb6-125">Packages can be added with the following approaches:</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4aeb6-126">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4aeb6-126">Visual Studio</span></span>](#tab/visual-studio)

### <a name="pmc-option-to-install-packages"></a><span data-ttu-id="4aeb6-127">パッケージをインストールするための PMC オプション</span><span class="sxs-lookup"><span data-stu-id="4aeb6-127">PMC option to install packages</span></span>

* <span data-ttu-id="4aeb6-128">Visual Studio で **[ツール]**、**[NuGet パッケージ マネージャー]**、**[パッケージ マネージャー コンソール]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="4aeb6-128">From Visual Studio, select **Tools** > **NuGet Package Manager** > **Package Manager Console**</span></span>
* <span data-ttu-id="4aeb6-129">**[パッケージ マネージャー コンソール]** ウィンドウから、*GrpcGreeterClient.csproj* ファイルがあるディレクトリに移動します。</span><span class="sxs-lookup"><span data-stu-id="4aeb6-129">From the **Package Manager Console** window, navigate to the directory in which the *GrpcGreeterClient.csproj* file exists.</span></span>
* <span data-ttu-id="4aeb6-130">次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="4aeb6-130">Run the following command:</span></span>

    ```powershell
    Install-Package Grpc.Core
    ```

* <span data-ttu-id="4aeb6-131">Google.Protobuf と Grpc.Tools に対して `Install-Package` を繰り返します</span><span class="sxs-lookup"><span data-stu-id="4aeb6-131">Repeat the `Install-Package` for Google.Protobuf and Grpc.Tools</span></span>

<!-- Tutorials shouldn't have multiple options. Select what you think is the best approach. Recommend you removed this approach. -->

### <a name="manage-nuget-packages-option-to-install-packages"></a><span data-ttu-id="4aeb6-132">パッケージをインストールするための [NuGet パッケージの管理] オプション</span><span class="sxs-lookup"><span data-stu-id="4aeb6-132">Manage NuGet Packages option to install packages</span></span>

* <span data-ttu-id="4aeb6-133">**[ソリューション エクスプローラー]** > **[NuGet パッケージの管理]** でプロジェクトを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="4aeb6-133">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
* <span data-ttu-id="4aeb6-134">**パッケージ ソース**を "nuget.org" に設定します。</span><span class="sxs-lookup"><span data-stu-id="4aeb6-134">Set the **Package source** to "nuget.org"</span></span>
* <span data-ttu-id="4aeb6-135">検索ボックスに "Grpc.Core" と入力します</span><span class="sxs-lookup"><span data-stu-id="4aeb6-135">Enter "Grpc.Core" in the search box</span></span>
* <span data-ttu-id="4aeb6-136">**[参照]** タブから "Grpc.Core" パッケージを選択し、**[インストール]** をクリックします</span><span class="sxs-lookup"><span data-stu-id="4aeb6-136">Select the "Grpc.Core" package from the **Browse** tab and click **Install**</span></span>
* <span data-ttu-id="4aeb6-137">Google.Protobuf と Grpc.Tools に対して繰り返します</span><span class="sxs-lookup"><span data-stu-id="4aeb6-137">Repeat for Google.Protobuf and Grpc.Tools</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4aeb6-138">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4aeb6-138">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="4aeb6-139">**統合端末**からから次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="4aeb6-139">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package Grpc.Core
```

<span data-ttu-id="4aeb6-140">Google.Protobuf と Grpc.Tools に対して繰り返します</span><span class="sxs-lookup"><span data-stu-id="4aeb6-140">Repeat for Google.Protobuf and Grpc.Tools</span></span>

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="4aeb6-141">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="4aeb6-141">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="4aeb6-142">**[Solution Pad]** > **[パッケージを追加]** で [*パッケージ*] フォルダーを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="4aeb6-142">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="4aeb6-143">**[パッケージを追加]** ウィンドウの **[ソース]** ドロップダウンを "nuget.org" に設定します。</span><span class="sxs-lookup"><span data-stu-id="4aeb6-143">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="4aeb6-144">検索ボックスに "Grpc.Core" と入力します</span><span class="sxs-lookup"><span data-stu-id="4aeb6-144">Enter "Grpc.Core" in the search box</span></span>
* <span data-ttu-id="4aeb6-145">結果ウィンドウから "Grpc.Core" パッケージを選択し、**[パッケージを追加]** をクリックします</span><span class="sxs-lookup"><span data-stu-id="4aeb6-145">Select the "Grpc.Core" package from the results pane and click **Add Package**</span></span>
* <span data-ttu-id="4aeb6-146">Google.Protobuf と Grpc.Tools に対して繰り返します</span><span class="sxs-lookup"><span data-stu-id="4aeb6-146">Repeat for Google.Protobuf and Grpc.Tools</span></span>

---

## <a name="add-the-greetproto-file"></a><span data-ttu-id="4aeb6-147">greet.proto ファイルを追加する</span><span class="sxs-lookup"><span data-stu-id="4aeb6-147">Add the greet.proto file</span></span>

<span data-ttu-id="4aeb6-148">gRPC あいさつサービスから gRPC クライアント プロジェクトに **Protos\greet.proto** ファイルをコピーします。</span><span class="sxs-lookup"><span data-stu-id="4aeb6-148">Copy the **Protos\greet.proto** file from the gRPC Greeter service to the gRPC client project.</span></span> <span data-ttu-id="4aeb6-149">GrpcGreeterClient プロジェクト ファイルの `<Protobuf>` 項目グループに **greet.proto** ファイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="4aeb6-149">Add the **greet.proto** file to the `<Protobuf>` item group of the GrpcGreeterClient project file:</span></span>

```XML
<Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
```

> [!NOTE]
> <span data-ttu-id="4aeb6-150">プロジェクトを右クリックし、ドロップダウン メニューから **[Edit GrpcGreeterClient.csproj]\(GrpcGreeterClient.csproj の編集\)** オプションを選択すると、GrpcGreeterClient のプロジェクト ファイルを開くことができます。</span><span class="sxs-lookup"><span data-stu-id="4aeb6-150">You can open the project file of GrpcGreeterClient by right-clicking the project and selecting the **Edit GrpcGreeterClient.csproj** option from the dropdown menu.</span></span>
>
> ![新しい ASP.NET Core Web アプリケーション](grpc-start/_static/edit_csproj.png)
>
> <span data-ttu-id="4aeb6-152">あるいは、GrpcGreeterClient ディレクトリに移動し、普段お使いのエディターで `GrpcGreeterClient.csproj` を編集できます。</span><span class="sxs-lookup"><span data-stu-id="4aeb6-152">Alternatively, you can navigate to the GrpcGreeterClient directory and edit the `GrpcGreeterClient.csproj` with your favorite editor.</span></span>

<span data-ttu-id="4aeb6-153">`GrpcServices="Client"` 属性が追加されます。そのため、含まれる protobuf ファイルに対して C# クライアント アセットのみが生成されます。</span><span class="sxs-lookup"><span data-stu-id="4aeb6-153">The `GrpcServices="Client"` attribute is added so that only the C# client assets are generated for the included protobuf file.</span></span> <span data-ttu-id="4aeb6-154">クライアント プロジェクトをビルドし、C# クライアント アセットの生成をトリガーします。</span><span class="sxs-lookup"><span data-stu-id="4aeb6-154">Build the client project to trigger the generation of the C# client assets.</span></span>

## <a name="create-a-greeterclient-and-invoke-the-sayhello-unary-call"></a><span data-ttu-id="4aeb6-155">GreeterClient を作成し、単項呼び出し SayHello を呼び出す</span><span class="sxs-lookup"><span data-stu-id="4aeb6-155">Create a GreeterClient and invoke the SayHello unary call</span></span>

<span data-ttu-id="4aeb6-156">gRPC クライアント プロジェクトの `Main` ファイルの `Program.cs` メソッドに次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="4aeb6-156">Add the following code to `Main` method of the `Program.cs` file of the gRPC client project:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet)]

<span data-ttu-id="4aeb6-157">必須の型にアクセスするには、次の using ステートメントが必要です。</span><span class="sxs-lookup"><span data-stu-id="4aeb6-157">To access the required types the following using statements are required:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=using)]

<span data-ttu-id="4aeb6-158">gRPC サービスへの接続を作成するための情報を含む `Channel` をインスタンス化し、それを利用して `GreeterClient` を構築することで GreeterClient が作成されます。</span><span class="sxs-lookup"><span data-stu-id="4aeb6-158">The GreeterClient is created by instantiating a `Channel` containing the information for creating the connection to the gRPC service and using it to construct the `GreeterClient`:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=4-5)]

<span data-ttu-id="4aeb6-159">GreeterClient には単項呼び出し `SayHello` が含まれます。これは非同期で呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="4aeb6-159">The GreeterClient contains the unary call `SayHello` which can be invoked asynchronously:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=7-8)]

<span data-ttu-id="4aeb6-160">`SayHello` 呼び出しの結果は `reply` に格納されます。結果は次のように表示できます。</span><span class="sxs-lookup"><span data-stu-id="4aeb6-160">The results of the `SayHello` call is stored in `reply` which can then be displayed:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=9)]

<span data-ttu-id="4aeb6-161">クライアントによって使用される `Channel` は、操作が完了してリソースが解放されたとき、シャットダウンする必要があります。</span><span class="sxs-lookup"><span data-stu-id="4aeb6-161">The `Channel` used by the client should be shut down when operations have finished to release all resources:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=11)]

> [!NOTE]
> <span data-ttu-id="4aeb6-162">**Greeter** 名前空間の型を解決するには、プロジェクトをビルドする必要があります。</span><span class="sxs-lookup"><span data-stu-id="4aeb6-162">You will need to build the project before the types in the **Greeter** namespace can be resolved.</span></span> <span data-ttu-id="4aeb6-163">これらの型はビルド中に自動的に生成されます。ビルドの実行前には利用できません。</span><span class="sxs-lookup"><span data-stu-id="4aeb6-163">These types are generated automatically during build and are not be available before a build is run.</span></span>

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a><span data-ttu-id="4aeb6-164">gRPC あいさつサービスで gRPC クライアントをテストする</span><span class="sxs-lookup"><span data-stu-id="4aeb6-164">Test the gRPC client with the gRPC Greeter service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4aeb6-165">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4aeb6-165">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="4aeb6-166">前のチュートリアルで作成したあいさつサービスが実行中であることを確認します。</span><span class="sxs-lookup"><span data-stu-id="4aeb6-166">Ensure the Greeter service created in the previous tutorial is running.</span></span>

* <span data-ttu-id="4aeb6-167">サービスが実行中であれば、**GrpcGreeterClient** プロジェクトに戻り、それをスタートアップ プロジェクトとして設定します。</span><span class="sxs-lookup"><span data-stu-id="4aeb6-167">Once the service is running, return to the **GrpcGreeterClient** project set it as the Startup Project.</span></span> <span data-ttu-id="4aeb6-168">Ctrl+F5 キーを押し、デバッガーなしでクライアントを実行します。</span><span class="sxs-lookup"><span data-stu-id="4aeb6-168">Press Ctrl+F5 to run the client without the debugger.</span></span>

  <span data-ttu-id="4aeb6-169">クライアントがその名前 "GreeterClient" を含むメッセージを使用して、サービスにあいさつを送信します。</span><span class="sxs-lookup"><span data-stu-id="4aeb6-169">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="4aeb6-170">サービスはコマンド プロンプトに表示される応答として、メッセージ "Hello GreeterClient" を送信します。</span><span class="sxs-lookup"><span data-stu-id="4aeb6-170">The service will send a message "Hello GreeterClient" as a response that is displayed in the command prompt.</span></span>

  ![新しい ASP.NET Core Web アプリケーション](grpc-start/_static/client.png)

  <span data-ttu-id="4aeb6-172">サービスは、成功した呼び出しの詳細をコマンド プロンプトに書き込まれるログに記録します。</span><span class="sxs-lookup"><span data-stu-id="4aeb6-172">The service records the details of the successful call in the logs written to the command prompt.</span></span>

  ![新しい ASP.NET Core Web アプリケーション](grpc-start/_static/server_complete.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="4aeb6-174">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="4aeb6-174">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="4aeb6-175">前のチュートリアルで作成したあいさつサービスが実行中であることを確認します。</span><span class="sxs-lookup"><span data-stu-id="4aeb6-175">Ensure the Greeter service created in the previous tutorial is running.</span></span>

* <span data-ttu-id="4aeb6-176">`dotnet run` を使用して、コマンド ラインからクライアント プロジェクト GrpcGreeter.Client を実行します。</span><span class="sxs-lookup"><span data-stu-id="4aeb6-176">Run the Client project GrpcGreeter.Client from the separate command line using `dotnet run`.</span></span>

<span data-ttu-id="4aeb6-177">クライアントがその名前 "GreeterClient" を含むメッセージを使用して、サービスにあいさつを送信します。</span><span class="sxs-lookup"><span data-stu-id="4aeb6-177">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="4aeb6-178">サービスはコマンド プロンプトに表示される応答として、メッセージ "Hello GreeterClient" を送信します。</span><span class="sxs-lookup"><span data-stu-id="4aeb6-178">The service will send a message "Hello GreeterClient" as a response that is displayed in the command prompt.</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="4aeb6-179">サービスは、成功した呼び出しの詳細をコマンド プロンプトに書き込まれるログに記録します。</span><span class="sxs-lookup"><span data-stu-id="4aeb6-179">The service records the details of the successful call in the logs written to the command prompt.</span></span>

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
info: Microsoft.AspNetCore.Hosting.Internal.GenericWebHostService[1]
      Request starting HTTP/2 POST http://localhost:50051/Greet.Greeter/SayHello application/grpc
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[0]
      Executing endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[1]
      Executed endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Hosting.Internal.GenericWebHostService[2]
      Request finished in 194.5798ms 200 application/grpc
```

<!-- End of combined VS/Mac tabs -->

---

### <a name="examine-the-project-files-of-the-grpc-project"></a><span data-ttu-id="4aeb6-180">gRPC プロジェクトのプロジェクト ファイルを調査する</span><span class="sxs-lookup"><span data-stu-id="4aeb6-180">Examine the project files of the gRPC project</span></span>

<span data-ttu-id="4aeb6-181">gRPC クライアントの GrpcGreeterClient ファイル:</span><span class="sxs-lookup"><span data-stu-id="4aeb6-181">gRPC client GrpcGreeterClient file:</span></span>

<span data-ttu-id="4aeb6-182">*Program.cs* には、gRPC クライアントのエントリ ポイントとロジックが含まれています。</span><span class="sxs-lookup"><span data-stu-id="4aeb6-182">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4aeb6-183">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="4aeb6-183">Additional resources</span></span>

<span data-ttu-id="4aeb6-184">このチュートリアルでは、次の作業を行いました。</span><span class="sxs-lookup"><span data-stu-id="4aeb6-184">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="4aeb6-185">gRPC クライアントを作成します。</span><span class="sxs-lookup"><span data-stu-id="4aeb6-185">Create a gRPC client.</span></span>
> * <span data-ttu-id="4aeb6-186">このサービスを前のチュートリアルで作成した gRPC あいさつサービスに対して実行します。</span><span class="sxs-lookup"><span data-stu-id="4aeb6-186">Run the service against the gRPC Greeter service created in the previous tutorial.</span></span>
> * <span data-ttu-id="4aeb6-187">プロジェクト ファイルを確認する</span><span class="sxs-lookup"><span data-stu-id="4aeb6-187">Examine the project files.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="4aeb6-188">前へ:gRPC あいさつサービスを作成する</span><span class="sxs-lookup"><span data-stu-id="4aeb6-188">Previous: Create a gRPC Greeter Service</span></span>](xref:tutorials/grpc/grpc-start)
