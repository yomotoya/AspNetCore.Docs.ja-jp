---
title: 'チュートリアル: ASP.NET Core 上で SignalR の使用を開始する'
author: tdykstra
description: このチュートリアルでは、ASP.NET Core 用 SignalR を使用するチャット アプリを作成します。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/31/2018
uid: tutorials/signalr
ms.openlocfilehash: 8581628b3f0cf878b8cc1d0684046d22a8374af9
ms.sourcegitcommit: c12ebdab65853f27fbb418204646baf6ce69515e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/21/2018
ms.locfileid: "46523221"
---
# <a name="tutorial-get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="963db-103">チュートリアル: ASP.NET Core 上で SignalR の使用を開始する</span><span class="sxs-lookup"><span data-stu-id="963db-103">Tutorial: Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="963db-104">このチュートリアルでは、SignalR を使用してリアルタイム アプリをビルドするための基礎について説明します。</span><span class="sxs-lookup"><span data-stu-id="963db-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="963db-105">以下の方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="963db-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="963db-106">ASP.NET Core 上で SignalR を使用する Web アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="963db-106">Create a web app that uses SignalR on ASP.NET Core.</span></span>
> * <span data-ttu-id="963db-107">サーバー上で SignalR ハブを作成します。</span><span class="sxs-lookup"><span data-stu-id="963db-107">Create a SignalR hub on the server.</span></span>
> * <span data-ttu-id="963db-108">JavaScript クライアントから SignalR ハブに接続します。</span><span class="sxs-lookup"><span data-stu-id="963db-108">Connect to the SignalR hub from JavaScript clients.</span></span>
> * <span data-ttu-id="963db-109">ハブを使用して、任意のクライアントから、接続されているすべてのクライアントにメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="963db-109">Use the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="963db-110">最後には、次のように動作するチャット アプリが作成されます。</span><span class="sxs-lookup"><span data-stu-id="963db-110">At the end, you'll have a working chat app:</span></span>

![SignalR のサンプル アプリ](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="963db-112">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="963db-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="963db-113">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="963db-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="963db-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="963db-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="963db-115">**ASP.NET および Web 開発**ワークロードを含む [Visual Studio 2017](https://www.visualstudio.com/downloads/) バージョン 15.8 以降</span><span class="sxs-lookup"><span data-stu-id="963db-115">[Visual Studio 2017 version 15.8 or later](https://www.visualstudio.com/downloads/) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="963db-116">.NET Core SDK 2.1 以降</span><span class="sxs-lookup"><span data-stu-id="963db-116">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="963db-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="963db-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="963db-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="963db-118">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="963db-119">.NET Core SDK 2.1 以降</span><span class="sxs-lookup"><span data-stu-id="963db-119">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="963db-120">Visual Studio Code 用 C#</span><span class="sxs-lookup"><span data-stu-id="963db-120">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="963db-121">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="963db-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="963db-122">Visual Studio for Mac バージョン 7.5.4 以降</span><span class="sxs-lookup"><span data-stu-id="963db-122">Visual Studio for Mac version 7.5.4 or later</span></span>](https://www.visualstudio.com/downloads/)
* <span data-ttu-id="963db-123">[.NET Core SDK 2.1 以降](https://www.microsoft.com/net/download/all) (Visual Studio インストールに含まれている)</span><span class="sxs-lookup"><span data-stu-id="963db-123">[.NET Core SDK 2.1 or later](https://www.microsoft.com/net/download/all) (included in the Visual Studio install)</span></span>

---

## <a name="create-the-project"></a><span data-ttu-id="963db-124">プロジェクトの作成</span><span class="sxs-lookup"><span data-stu-id="963db-124">Create the project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="963db-125">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="963db-125">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="963db-126">メニューから、**[ファイル]、[新規プロジェクト]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="963db-126">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="963db-127">**[新しいプロジェクト]** ダイアログで、**[インストール]、[Visual C#]、[Web]、[ASP.NET Core Web アプリケーション]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="963db-127">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="963db-128">プロジェクトに "*SignalRChat*" という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="963db-128">Name the project *SignalRChat*.</span></span>

  ![Visual Studio の [新しいプロジェクト] ダイアログ ボックス](signalr/_static/signalr-new-project-dialog.png)

* <span data-ttu-id="963db-130">**[Web アプリケーション]** を選択して、Razor Pages を使用するプロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="963db-130">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="963db-131">**.NET Core** のターゲット フレームワークを選択し、**ASP.NET Core 2.1** を選択して、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="963db-131">Select a target framework of **.NET Core**, select **ASP.NET Core 2.1**, and click **OK**.</span></span>

  ![Visual Studio の [新しいプロジェクト] ダイアログ ボックス](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="963db-133">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="963db-133">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="963db-134">新しいプロジェクトのために使用できるフォルダーを開きます。</span><span class="sxs-lookup"><span data-stu-id="963db-134">Open a folder that you can use for a new project.</span></span>

* <span data-ttu-id="963db-135">**[統合端末]** で、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="963db-135">In the **Integrated Terminal**, run the following command:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="963db-136">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="963db-136">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="963db-137">メニューから、**[ファイル]、[新しいソリューション]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="963db-137">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="963db-138">**[.NET Core]、[アプリ]、[ASP.NET Core Web アプリ]** の順に選択します (**[ASP.NET Core Web アプリ (MVC)]** を選択しないでください)。</span><span class="sxs-lookup"><span data-stu-id="963db-138">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="963db-139">**[次へ]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="963db-139">Select **Next**.</span></span>

* <span data-ttu-id="963db-140">プロジェクトに *SignalRChat* という名前を付けて、**[作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="963db-140">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="963db-141">SignalR クライアント ライブラリを追加する</span><span class="sxs-lookup"><span data-stu-id="963db-141">Add the SignalR client library</span></span>

<span data-ttu-id="963db-142">SignalR サーバー ライブラリは、[Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app) に含まれています。</span><span class="sxs-lookup"><span data-stu-id="963db-142">The SignalR server library is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="963db-143">JavaScript クライアント ライブラリはプロジェクトに自動的に含まれません。</span><span class="sxs-lookup"><span data-stu-id="963db-143">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="963db-144">このチュートリアルでは、[ライブラリ マネージャー (LibMan)](xref:client-side/libman/index) を使用して *unpkg* からクライアント ライブラリを取得します。</span><span class="sxs-lookup"><span data-stu-id="963db-144">For this tutorial, you use [Library Manager (LibMan)](xref:client-side/libman/index) to get the client library from *unpkg*.</span></span> <span data-ttu-id="963db-145">[unpkg](https://unpkg.com/#/) は、[npm (Node.js パッケージ マネージャー)](https://www.npmjs.com/get-npm) で見つかるものすべてを配信できる[コンテンツ配信ネットワーク](https://wikipedia.org/wiki/Content_delivery_network)です。</span><span class="sxs-lookup"><span data-stu-id="963db-145">[unpkg](https://unpkg.com/#/) is a [content delivery network](https://wikipedia.org/wiki/Content_delivery_network) that can deliver anything found in [npm, the Node.js package manager](https://www.npmjs.com/get-npm).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="963db-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="963db-146">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="963db-147">**ソリューション エクスプローラー**で、プロジェクトを右クリックし、**[追加]** > **[クライアント側のライブラリ]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="963db-147">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="963db-148">**[Add Client-Side Library]\(クライアント側のライブラリの追加\)** ダイアログで、**[プロバイダー]** に対して **[unpkg]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="963db-148">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span> 

* <span data-ttu-id="963db-149">**[ライブラリ]** に対して「_@aspnet/signalr@1_」を入力し、プレビューでない最新のバージョンを選択します。</span><span class="sxs-lookup"><span data-stu-id="963db-149">For **Library**, enter _@aspnet/signalr@1_, and select the latest version that isn't preview.</span></span>

  ![クライアント側のライブラリの追加ダイアログ - ライブラリの選択](signalr/_static/libman1.png)

* <span data-ttu-id="963db-151">**[Choose specific files]\(特定のファイルの選択\)** を選択して *dist/browser* フォルダーを展開し、*signalr.js* と *signalr.min.js* を選択します。</span><span class="sxs-lookup"><span data-stu-id="963db-151">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="963db-152">**[ターゲットの場所]** を *wwwroot/lib/signalr/* に設定して、**[インストール]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="963db-152">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![クライアント側のライブラリの追加ダイアログ - ファイルと保存先の選択](signalr/_static/libman2.png)

  <span data-ttu-id="963db-154">[LibMan](xref:client-side/libman/index) によって *wwwroot/lib/signalr* フォルダーが作成され、そこに選択したファイルがコピーされます。</span><span class="sxs-lookup"><span data-stu-id="963db-154">[LibMan](xref:client-side/libman/index) creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="963db-155">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="963db-155">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="963db-156">**[統合端末]** で、次のコマンドを実行して LibMan をインストールします。</span><span class="sxs-lookup"><span data-stu-id="963db-156">In the **Integrated Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="963db-157">プロジェクト フォルダー (ファイル *SignalRChat.csproj* を含んでいるフォルダー) に移動します。</span><span class="sxs-lookup"><span data-stu-id="963db-157">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="963db-158">次のコマンドを実行して、LibMan を使用して SignalR クライアント ライブラリを取得します。</span><span class="sxs-lookup"><span data-stu-id="963db-158">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="963db-159">出力が表示されるまでに数秒待機する必要がある場合があります。</span><span class="sxs-lookup"><span data-stu-id="963db-159">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot\lib\signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="963db-160">パラメーターによって次のオプションが指定されます。</span><span class="sxs-lookup"><span data-stu-id="963db-160">The parameters specify the following options:</span></span>
  * <span data-ttu-id="963db-161">unpkg プロバイダーを使用する。</span><span class="sxs-lookup"><span data-stu-id="963db-161">Use the unpkg provider.</span></span>
  * <span data-ttu-id="963db-162">ファイルを *wwwroot/lib/signalr* にコピーする。</span><span class="sxs-lookup"><span data-stu-id="963db-162">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="963db-163">指定したファイルのみをコピーする。</span><span class="sxs-lookup"><span data-stu-id="963db-163">Copy only the specified files.</span></span>

  <span data-ttu-id="963db-164">出力は次の例のようになります。</span><span class="sxs-lookup"><span data-stu-id="963db-164">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot\lib\signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="963db-165">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="963db-165">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="963db-166">**[端末]** で、次のコマンドを実行して LibMan をインストールします。</span><span class="sxs-lookup"><span data-stu-id="963db-166">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="963db-167">プロジェクト フォルダー (ファイル *SignalRChat.csproj* を含んでいるフォルダー) に移動します。</span><span class="sxs-lookup"><span data-stu-id="963db-167">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="963db-168">次のコマンドを実行して、LibMan を使用して SignalR クライアント ライブラリを取得します。</span><span class="sxs-lookup"><span data-stu-id="963db-168">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot\lib\signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="963db-169">パラメーターによって次のオプションが指定されます。</span><span class="sxs-lookup"><span data-stu-id="963db-169">The parameters specify the following options:</span></span>
  * <span data-ttu-id="963db-170">unpkg プロバイダーを使用する。</span><span class="sxs-lookup"><span data-stu-id="963db-170">Use the unpkg provider.</span></span>
  * <span data-ttu-id="963db-171">ファイルを *wwwroot/lib/signalr* にコピーする。</span><span class="sxs-lookup"><span data-stu-id="963db-171">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="963db-172">指定したファイルのみをコピーする。</span><span class="sxs-lookup"><span data-stu-id="963db-172">Copy only the specified files.</span></span>

  <span data-ttu-id="963db-173">出力は次の例のようになります。</span><span class="sxs-lookup"><span data-stu-id="963db-173">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot\lib\signalr"
  ```

---

## <a name="create-the-signalr-hub"></a><span data-ttu-id="963db-174">SignalR ハブを作成する</span><span class="sxs-lookup"><span data-stu-id="963db-174">Create the SignalR hub</span></span>

<span data-ttu-id="963db-175">[ハブ](xref:signalr/hubs)はクライアント サーバー通信を処理するハイレベル パイプラインとして機能するクラスです。</span><span class="sxs-lookup"><span data-stu-id="963db-175">A [hub](xref:signalr/hubs) is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="963db-176">SignalRChat プロジェクト フォルダー内で、*Hubs* フォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="963db-176">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="963db-177">*Hubs* フォルダー内で、次のコードを使用して *ChatHub.cs* ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="963db-177">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  <span data-ttu-id="963db-178">`ChatHub` クラスは、SignalR [Hub](/dotnet/api/microsoft.aspnetcore.signalr.hub) クラスを継承します。</span><span class="sxs-lookup"><span data-stu-id="963db-178">The `ChatHub` class inherits from the SignalR [Hub](/dotnet/api/microsoft.aspnetcore.signalr.hub) class.</span></span> <span data-ttu-id="963db-179">`Hub` クラスでは、接続、グループ、およびメッセージングが管理されます。</span><span class="sxs-lookup"><span data-stu-id="963db-179">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="963db-180">`SendMessage` メソッドは、接続されている任意のクライアントによって呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="963db-180">The `SendMessage` method can be called by any connected client.</span></span> <span data-ttu-id="963db-181">それによって、受信したメッセージがすべてのクライアントに送信されます。</span><span class="sxs-lookup"><span data-stu-id="963db-181">It sends the received message to all clients.</span></span> <span data-ttu-id="963db-182">最大のスケーラビリティを実現するために、SignalR コードは非同期になっています。</span><span class="sxs-lookup"><span data-stu-id="963db-182">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="963db-183">SignalR を使用するようにプロジェクトを構成する</span><span class="sxs-lookup"><span data-stu-id="963db-183">Configure the project to use SignalR</span></span>

<span data-ttu-id="963db-184">SignalR 要求が SignalR に渡されるように SignalR サーバーを構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="963db-184">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="963db-185">次の強調表示されたコードを *Startup.cs* ファイルに追加します。</span><span class="sxs-lookup"><span data-stu-id="963db-185">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="963db-186">これらの変更によって、SignalR が[依存関係挿入](xref:fundamentals/dependency-injection)システムと[ミドルウェア](xref:fundamentals/middleware/index) パイプラインに追加されます。</span><span class="sxs-lookup"><span data-stu-id="963db-186">These changes add SignalR to the [dependency injection](xref:fundamentals/dependency-injection) system and the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="963db-187">SignalR クライアント コードを作成する</span><span class="sxs-lookup"><span data-stu-id="963db-187">Create the SignalR client code</span></span>

* <span data-ttu-id="963db-188">*Pages\Index.cshtml* のコンテンツを次のコードに変更します。</span><span class="sxs-lookup"><span data-stu-id="963db-188">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  <span data-ttu-id="963db-189">上のコードでは以下の操作が行われます。</span><span class="sxs-lookup"><span data-stu-id="963db-189">The preceding code:</span></span>

  * <span data-ttu-id="963db-190">名前およびメッセージ テキスト用のテキスト ボックスと送信ボタンを作成します。</span><span class="sxs-lookup"><span data-stu-id="963db-190">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="963db-191">SignalR ハブから受信したメッセージを表示するために、`id="messagesList"` を使用してリストを作成します。</span><span class="sxs-lookup"><span data-stu-id="963db-191">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="963db-192">SignalR へのスクリプト参照と、次の手順で作成する *chat.js* アプリケーション コードを含めます。</span><span class="sxs-lookup"><span data-stu-id="963db-192">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="963db-193">*wwwroot/js* フォルダー内に、次のコードを使用して *chat.js* ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="963db-193">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  <span data-ttu-id="963db-194">上のコードでは以下の操作が行われます。</span><span class="sxs-lookup"><span data-stu-id="963db-194">The preceding code:</span></span>

  * <span data-ttu-id="963db-195">接続を作成して開始します。</span><span class="sxs-lookup"><span data-stu-id="963db-195">Creates and starts a connection.</span></span>
  * <span data-ttu-id="963db-196">ハブにメッセージを送信するハンドラーを送信ボタンに追加します。</span><span class="sxs-lookup"><span data-stu-id="963db-196">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="963db-197">ハブからメッセージを受信してからそれをリストに追加するハンドラーを接続オブジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="963db-197">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="963db-198">アプリを実行する</span><span class="sxs-lookup"><span data-stu-id="963db-198">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="963db-199">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="963db-199">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="963db-200">**Ctrl + F5** キーを押して、デバッグを行わずにアプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="963db-200">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="963db-201">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="963db-201">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="963db-202">**Ctrl + F5** キーを押して、デバッグを行わずにアプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="963db-202">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="963db-203">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="963db-203">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="963db-204">メニューから、**[実行]、[デバッグなしで開始]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="963db-204">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="963db-205">アドレス バーから URL をコピーし、別のブラウザー インスタンスまたはタブを開き、アドレス バーに URL を貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="963db-205">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="963db-206">いずれかのブラウザーを選択し、名前とメッセージを入力し、**[送信]** ボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="963db-206">Choose either browser, enter a name and message, and select the **Send** button.</span></span>

  <span data-ttu-id="963db-207">次の瞬間、両方のページに名前とメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="963db-207">The name and message are displayed on both pages instantly.</span></span>

  ![SignalR のサンプル アプリ](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="963db-209">アプリが動作しない場合は、ご利用のブラウザーの開発者ツール (F12) を開き、コンソールに移動します。</span><span class="sxs-lookup"><span data-stu-id="963db-209">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="963db-210">HTML および JavaScript コードに関連するエラーが発生している場合があります。</span><span class="sxs-lookup"><span data-stu-id="963db-210">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="963db-211">たとえば、*signalr.js* を指示されたフォルダーとは別のフォルダーに配置したとします。</span><span class="sxs-lookup"><span data-stu-id="963db-211">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="963db-212">その場合、そのファイルへの参照は機能せず、コンソールに 404 エラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="963db-212">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="963db-213">![エラー "signalr.js が見つかりませんでした"](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="963db-213">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="963db-214">次の手順</span><span class="sxs-lookup"><span data-stu-id="963db-214">Next steps</span></span>

<span data-ttu-id="963db-215">クライアントが別のドメインから SignalR アプリに接続するようにしたい場合は、クロス オリジン リソース共有 (CORS) を有効にする必要です。</span><span class="sxs-lookup"><span data-stu-id="963db-215">If you want clients to connect to a SignalR app from different domains, you have to enable Cross-Origin Resource Sharing (CORS).</span></span> <span data-ttu-id="963db-216">詳細については、「[クロス オリジン リソース共有](xref:signalr/security?view=aspnetcore-2.1#cross-origin-resource-sharing)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="963db-216">For more information, see [Cross-origin resource sharing](xref:signalr/security?view=aspnetcore-2.1#cross-origin-resource-sharing).</span></span>

<span data-ttu-id="963db-217">SignalR、ハブ、および JavaScript クライアントの詳細については、次のリソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="963db-217">To learn more about SignalR, hubs, and JavaScript clients, see these resources:</span></span>

* [<span data-ttu-id="963db-218">ASP.NET Core 用 SignalR の概要</span><span class="sxs-lookup"><span data-stu-id="963db-218">Introduction to SignalR for ASP.NET Core</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="963db-219">ASP.NET Core 用 SignalR 内でハブを使用する</span><span class="sxs-lookup"><span data-stu-id="963db-219">Use hubs in SignalR for ASP.NET Core</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="963db-220">ASP.NET Core SignalR JavaScript クライアント</span><span class="sxs-lookup"><span data-stu-id="963db-220">ASP.NET Core SignalR JavaScript client</span></span>](xref:signalr/javascript-client)
