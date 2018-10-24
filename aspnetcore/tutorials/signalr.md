---
title: ASP.NET Core SignalR の概要
author: tdykstra
description: このチュートリアルでは、ASP.NET Core SignalR を使用するチャット アプリを作成します。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/31/2018
uid: tutorials/signalr
ms.openlocfilehash: 55fb6b1c13549129a00541c1228956a93854ad78
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578030"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a><span data-ttu-id="f445a-103">チュートリアル: ASP.NET Core SignalR の概要</span><span class="sxs-lookup"><span data-stu-id="f445a-103">Tutorial: Get started with ASP.NET Core SignalR</span></span>

<span data-ttu-id="f445a-104">このチュートリアルでは、SignalR を使用してリアルタイム アプリをビルドするための基礎について説明します。</span><span class="sxs-lookup"><span data-stu-id="f445a-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="f445a-105">以下の方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="f445a-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f445a-106">Web アプリ プロジェクトを作成する。</span><span class="sxs-lookup"><span data-stu-id="f445a-106">Create a web app project.</span></span>
> * <span data-ttu-id="f445a-107">SignalR クライアント ライブラリを追加する。</span><span class="sxs-lookup"><span data-stu-id="f445a-107">Add the SignalR client library.</span></span>
> * <span data-ttu-id="f445a-108">SignalR ハブを作成する。</span><span class="sxs-lookup"><span data-stu-id="f445a-108">Create a SignalR hub.</span></span>
> * <span data-ttu-id="f445a-109">SignalR を使用するようにプロジェクトを構成する。</span><span class="sxs-lookup"><span data-stu-id="f445a-109">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="f445a-110">ハブを使用して任意のクライアントから接続済みのすべてのクライアントにメッセージを送信する、コードを追加する。</span><span class="sxs-lookup"><span data-stu-id="f445a-110">Add code that uses the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="f445a-111">最後には、次のように動作するチャット アプリが作成されます。</span><span class="sxs-lookup"><span data-stu-id="f445a-111">At the end, you'll have a working chat app:</span></span>

![SignalR のサンプル アプリ](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="f445a-113">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="f445a-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f445a-114">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="f445a-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f445a-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f445a-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f445a-116">**ASP.NET および Web 開発**ワークロードを含む [Visual Studio 2017](https://www.visualstudio.com/downloads/) バージョン 15.8 以降</span><span class="sxs-lookup"><span data-stu-id="f445a-116">[Visual Studio 2017 version 15.8 or later](https://www.visualstudio.com/downloads/) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="f445a-117">.NET Core SDK 2.1 以降</span><span class="sxs-lookup"><span data-stu-id="f445a-117">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f445a-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f445a-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="f445a-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f445a-119">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="f445a-120">.NET Core SDK 2.1 以降</span><span class="sxs-lookup"><span data-stu-id="f445a-120">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="f445a-121">Visual Studio Code 用 C#</span><span class="sxs-lookup"><span data-stu-id="f445a-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f445a-122">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f445a-122">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="f445a-123">Visual Studio for Mac バージョン 7.5.4 以降</span><span class="sxs-lookup"><span data-stu-id="f445a-123">Visual Studio for Mac version 7.5.4 or later</span></span>](https://www.visualstudio.com/downloads/)
* <span data-ttu-id="f445a-124">[.NET Core SDK 2.1 以降](https://www.microsoft.com/net/download/all) (Visual Studio インストールに含まれている)</span><span class="sxs-lookup"><span data-stu-id="f445a-124">[.NET Core SDK 2.1 or later](https://www.microsoft.com/net/download/all) (included in the Visual Studio install)</span></span>

---

## <a name="create-a-web-project"></a><span data-ttu-id="f445a-125">Web プロジェクトを作成する</span><span class="sxs-lookup"><span data-stu-id="f445a-125">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f445a-126">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f445a-126">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="f445a-127">メニューから、**[ファイル]、[新規プロジェクト]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="f445a-127">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="f445a-128">**[新しいプロジェクト]** ダイアログで、**[インストール]、[Visual C#]、[Web]、[ASP.NET Core Web アプリケーション]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="f445a-128">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="f445a-129">プロジェクトに "*SignalRChat*" という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="f445a-129">Name the project *SignalRChat*.</span></span>

  ![Visual Studio の [新しいプロジェクト] ダイアログ ボックス](signalr/_static/signalr-new-project-dialog.png)

* <span data-ttu-id="f445a-131">**[Web アプリケーション]** を選択して、Razor Pages を使用するプロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="f445a-131">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="f445a-132">**.NET Core** のターゲット フレームワークを選択し、**ASP.NET Core 2.1** を選択して、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="f445a-132">Select a target framework of **.NET Core**, select **ASP.NET Core 2.1**, and click **OK**.</span></span>

  ![Visual Studio の [新しいプロジェクト] ダイアログ ボックス](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f445a-134">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f445a-134">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="f445a-135">新しいプロジェクトのために使用できるフォルダーを開きます。</span><span class="sxs-lookup"><span data-stu-id="f445a-135">Open a folder that you can use for a new project.</span></span>

* <span data-ttu-id="f445a-136">**[統合端末]** で、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="f445a-136">In the **Integrated Terminal**, run the following command:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f445a-137">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f445a-137">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="f445a-138">メニューから、**[ファイル]、[新しいソリューション]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="f445a-138">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="f445a-139">**[.NET Core]、[アプリ]、[ASP.NET Core Web アプリ]** の順に選択します (**[ASP.NET Core Web アプリ (MVC)]** を選択しないでください)。</span><span class="sxs-lookup"><span data-stu-id="f445a-139">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="f445a-140">**[次へ]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f445a-140">Select **Next**.</span></span>

* <span data-ttu-id="f445a-141">プロジェクトに *SignalRChat* という名前を付けて、**[作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f445a-141">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="f445a-142">SignalR クライアント ライブラリを追加する</span><span class="sxs-lookup"><span data-stu-id="f445a-142">Add the SignalR client library</span></span>

<span data-ttu-id="f445a-143">SignalR サーバー ライブラリは、`Microsoft.AspNetCore.App` メタパッケージに含まれています。</span><span class="sxs-lookup"><span data-stu-id="f445a-143">The SignalR server library is included in the `Microsoft.AspNetCore.App` metapackage.</span></span> <span data-ttu-id="f445a-144">JavaScript クライアント ライブラリはプロジェクトに自動的に含まれません。</span><span class="sxs-lookup"><span data-stu-id="f445a-144">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="f445a-145">このチュートリアルでは、ライブラリ マネージャー (LibMan) を使用して *unpkg* からクライアント ライブラリを取得します。</span><span class="sxs-lookup"><span data-stu-id="f445a-145">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="f445a-146">unpkg は、npm (Node.js パッケージ マネージャー) で見つかるものすべてを配信できるコンテンツ配信ネットワーク (CDN) です。</span><span class="sxs-lookup"><span data-stu-id="f445a-146">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f445a-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f445a-147">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="f445a-148">**ソリューション エクスプローラー**で、プロジェクトを右クリックし、**[追加]** > **[クライアント側のライブラリ]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f445a-148">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="f445a-149">**[Add Client-Side Library]\(クライアント側のライブラリの追加\)** ダイアログで、**[プロバイダー]** に対して **[unpkg]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f445a-149">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span> 

* <span data-ttu-id="f445a-150">**[ライブラリ]** に対して「`@aspnet/signalr@1`」を入力し、プレビューでない最新のバージョンを選択します。</span><span class="sxs-lookup"><span data-stu-id="f445a-150">For **Library**, enter `@aspnet/signalr@1`, and select the latest version that isn't preview.</span></span>

  ![クライアント側のライブラリの追加ダイアログ - ライブラリの選択](signalr/_static/libman1.png)

* <span data-ttu-id="f445a-152">**[Choose specific files]\(特定のファイルの選択\)** を選択して *dist/browser* フォルダーを展開し、*signalr.js* と *signalr.min.js* を選択します。</span><span class="sxs-lookup"><span data-stu-id="f445a-152">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="f445a-153">**[ターゲットの場所]** を *wwwroot/lib/signalr/* に設定して、**[インストール]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f445a-153">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![クライアント側のライブラリの追加ダイアログ - ファイルと保存先の選択](signalr/_static/libman2.png)

  <span data-ttu-id="f445a-155">LibMan によって *wwwroot/lib/signalr* フォルダーが作成され、そこに選択したファイルがコピーされます。</span><span class="sxs-lookup"><span data-stu-id="f445a-155">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f445a-156">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f445a-156">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="f445a-157">**[統合端末]** で、次のコマンドを実行して LibMan をインストールします。</span><span class="sxs-lookup"><span data-stu-id="f445a-157">In the **Integrated Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="f445a-158">プロジェクト フォルダー (ファイル *SignalRChat.csproj* を含んでいるフォルダー) に移動します。</span><span class="sxs-lookup"><span data-stu-id="f445a-158">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="f445a-159">次のコマンドを実行して、LibMan を使用して SignalR クライアント ライブラリを取得します。</span><span class="sxs-lookup"><span data-stu-id="f445a-159">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="f445a-160">出力が表示されるまでに数秒待機する必要がある場合があります。</span><span class="sxs-lookup"><span data-stu-id="f445a-160">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="f445a-161">パラメーターによって次のオプションが指定されます。</span><span class="sxs-lookup"><span data-stu-id="f445a-161">The parameters specify the following options:</span></span>
  * <span data-ttu-id="f445a-162">unpkg プロバイダーを使用する。</span><span class="sxs-lookup"><span data-stu-id="f445a-162">Use the unpkg provider.</span></span>
  * <span data-ttu-id="f445a-163">ファイルを *wwwroot/lib/signalr* にコピーする。</span><span class="sxs-lookup"><span data-stu-id="f445a-163">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="f445a-164">指定したファイルのみをコピーする。</span><span class="sxs-lookup"><span data-stu-id="f445a-164">Copy only the specified files.</span></span>

  <span data-ttu-id="f445a-165">出力は次の例のようになります。</span><span class="sxs-lookup"><span data-stu-id="f445a-165">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f445a-166">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f445a-166">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="f445a-167">**[端末]** で、次のコマンドを実行して LibMan をインストールします。</span><span class="sxs-lookup"><span data-stu-id="f445a-167">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="f445a-168">プロジェクト フォルダー (ファイル *SignalRChat.csproj* を含んでいるフォルダー) に移動します。</span><span class="sxs-lookup"><span data-stu-id="f445a-168">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="f445a-169">次のコマンドを実行して、LibMan を使用して SignalR クライアント ライブラリを取得します。</span><span class="sxs-lookup"><span data-stu-id="f445a-169">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="f445a-170">パラメーターによって次のオプションが指定されます。</span><span class="sxs-lookup"><span data-stu-id="f445a-170">The parameters specify the following options:</span></span>
  * <span data-ttu-id="f445a-171">unpkg プロバイダーを使用する。</span><span class="sxs-lookup"><span data-stu-id="f445a-171">Use the unpkg provider.</span></span>
  * <span data-ttu-id="f445a-172">ファイルを *wwwroot/lib/signalr* にコピーする。</span><span class="sxs-lookup"><span data-stu-id="f445a-172">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="f445a-173">指定したファイルのみをコピーする。</span><span class="sxs-lookup"><span data-stu-id="f445a-173">Copy only the specified files.</span></span>

  <span data-ttu-id="f445a-174">出力は次の例のようになります。</span><span class="sxs-lookup"><span data-stu-id="f445a-174">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="f445a-175">SignalR ハブを作成する</span><span class="sxs-lookup"><span data-stu-id="f445a-175">Create a SignalR hub</span></span>

<span data-ttu-id="f445a-176">*ハブ*はクライアント サーバー通信を処理するハイレベル パイプラインとして機能するクラスです。</span><span class="sxs-lookup"><span data-stu-id="f445a-176">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="f445a-177">SignalRChat プロジェクト フォルダー内で、*Hubs* フォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="f445a-177">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="f445a-178">*Hubs* フォルダー内で、次のコードを使用して *ChatHub.cs* ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="f445a-178">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  <span data-ttu-id="f445a-179">`ChatHub` クラスは、SignalR `Hub` クラスを継承します。</span><span class="sxs-lookup"><span data-stu-id="f445a-179">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="f445a-180">`Hub` クラスでは、接続、グループ、およびメッセージングが管理されます。</span><span class="sxs-lookup"><span data-stu-id="f445a-180">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="f445a-181">`SendMessage` メソッドは、接続されている任意のクライアントによって呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="f445a-181">The `SendMessage` method can be called by any connected client.</span></span> <span data-ttu-id="f445a-182">それによって、受信したメッセージがすべてのクライアントに送信されます。</span><span class="sxs-lookup"><span data-stu-id="f445a-182">It sends the received message to all clients.</span></span> <span data-ttu-id="f445a-183">最大のスケーラビリティを実現するために、SignalR コードは非同期になっています。</span><span class="sxs-lookup"><span data-stu-id="f445a-183">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="f445a-184">SignalR を構成する</span><span class="sxs-lookup"><span data-stu-id="f445a-184">Configure SignalR</span></span>

<span data-ttu-id="f445a-185">SignalR 要求が SignalR に渡されるように SignalR サーバーを構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f445a-185">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="f445a-186">次の強調表示されたコードを *Startup.cs* ファイルに追加します。</span><span class="sxs-lookup"><span data-stu-id="f445a-186">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="f445a-187">これらの変更によって、SignalR が ASP.NET Core 依存関係挿入システムとミドルウェア パイプラインに追加されます。</span><span class="sxs-lookup"><span data-stu-id="f445a-187">These changes add SignalR to the ASP.NET Core dependency injection system and the middleware pipeline.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="f445a-188">SignalR クライアント コードを追加する</span><span class="sxs-lookup"><span data-stu-id="f445a-188">Add SignalR client code</span></span>

* <span data-ttu-id="f445a-189">*Pages\Index.cshtml* のコンテンツを次のコードに変更します。</span><span class="sxs-lookup"><span data-stu-id="f445a-189">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  <span data-ttu-id="f445a-190">上のコードでは以下の操作が行われます。</span><span class="sxs-lookup"><span data-stu-id="f445a-190">The preceding code:</span></span>

  * <span data-ttu-id="f445a-191">名前およびメッセージ テキスト用のテキスト ボックスと送信ボタンを作成します。</span><span class="sxs-lookup"><span data-stu-id="f445a-191">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="f445a-192">SignalR ハブから受信したメッセージを表示するために、`id="messagesList"` を使用してリストを作成します。</span><span class="sxs-lookup"><span data-stu-id="f445a-192">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="f445a-193">SignalR へのスクリプト参照と、次の手順で作成する *chat.js* アプリケーション コードを含めます。</span><span class="sxs-lookup"><span data-stu-id="f445a-193">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="f445a-194">*wwwroot/js* フォルダー内に、次のコードを使用して *chat.js* ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="f445a-194">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  <span data-ttu-id="f445a-195">上のコードでは以下の操作が行われます。</span><span class="sxs-lookup"><span data-stu-id="f445a-195">The preceding code:</span></span>

  * <span data-ttu-id="f445a-196">接続を作成して開始します。</span><span class="sxs-lookup"><span data-stu-id="f445a-196">Creates and starts a connection.</span></span>
  * <span data-ttu-id="f445a-197">ハブにメッセージを送信するハンドラーを送信ボタンに追加します。</span><span class="sxs-lookup"><span data-stu-id="f445a-197">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="f445a-198">ハブからメッセージを受信してからそれをリストに追加するハンドラーを接続オブジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="f445a-198">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="f445a-199">アプリを実行する</span><span class="sxs-lookup"><span data-stu-id="f445a-199">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f445a-200">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f445a-200">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f445a-201">**Ctrl + F5** キーを押して、デバッグを行わずにアプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="f445a-201">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f445a-202">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f445a-202">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="f445a-203">**Ctrl + F5** キーを押して、デバッグを行わずにアプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="f445a-203">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f445a-204">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f445a-204">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="f445a-205">メニューから、**[実行]、[デバッグなしで開始]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="f445a-205">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="f445a-206">アドレス バーから URL をコピーし、別のブラウザー インスタンスまたはタブを開き、アドレス バーに URL を貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="f445a-206">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="f445a-207">いずれかのブラウザーを選択し、名前とメッセージを入力し、**[送信]** ボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="f445a-207">Choose either browser, enter a name and message, and select the **Send** button.</span></span>

  <span data-ttu-id="f445a-208">次の瞬間、両方のページに名前とメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="f445a-208">The name and message are displayed on both pages instantly.</span></span>

  ![SignalR のサンプル アプリ](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="f445a-210">アプリが動作しない場合は、ご利用のブラウザーの開発者ツール (F12) を開き、コンソールに移動します。</span><span class="sxs-lookup"><span data-stu-id="f445a-210">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="f445a-211">HTML および JavaScript コードに関連するエラーが発生している場合があります。</span><span class="sxs-lookup"><span data-stu-id="f445a-211">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="f445a-212">たとえば、*signalr.js* を指示されたフォルダーとは別のフォルダーに配置したとします。</span><span class="sxs-lookup"><span data-stu-id="f445a-212">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="f445a-213">その場合、そのファイルへの参照は機能せず、コンソールに 404 エラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="f445a-213">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="f445a-214">![エラー "signalr.js が見つかりませんでした"](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="f445a-214">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="f445a-215">次の手順</span><span class="sxs-lookup"><span data-stu-id="f445a-215">Next steps</span></span>

<span data-ttu-id="f445a-216">このチュートリアルでは、次の作業を行う方法を学びました。</span><span class="sxs-lookup"><span data-stu-id="f445a-216">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f445a-217">Web アプリ プロジェクトを作成する。</span><span class="sxs-lookup"><span data-stu-id="f445a-217">Create a web app project.</span></span>
> * <span data-ttu-id="f445a-218">SignalR クライアント ライブラリを追加する。</span><span class="sxs-lookup"><span data-stu-id="f445a-218">Add the SignalR client library.</span></span>
> * <span data-ttu-id="f445a-219">SignalR ハブを作成する。</span><span class="sxs-lookup"><span data-stu-id="f445a-219">Create a SignalR hub.</span></span>
> * <span data-ttu-id="f445a-220">SignalR を使用するようにプロジェクトを構成する。</span><span class="sxs-lookup"><span data-stu-id="f445a-220">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="f445a-221">ハブを使用して任意のクライアントから接続済みのすべてのクライアントにメッセージを送信する、コードを追加する。</span><span class="sxs-lookup"><span data-stu-id="f445a-221">Add code that uses the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="f445a-222">SignalR の詳細については、次の概要を参照してください。</span><span class="sxs-lookup"><span data-stu-id="f445a-222">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="f445a-223">ASP.NET Core SignalR の概要</span><span class="sxs-lookup"><span data-stu-id="f445a-223">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
