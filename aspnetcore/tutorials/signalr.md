---
title: ASP.NET Core SignalR の概要
author: tdykstra
description: このチュートリアルでは、ASP.NET Core SignalR を使用するチャット アプリを作成します。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/30/2018
uid: tutorials/signalr
ms.openlocfilehash: c52041b34d6c9d1d8f06f980c900b805a0933293
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861984"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a><span data-ttu-id="b3acb-103">チュートリアル: ASP.NET Core SignalR の概要</span><span class="sxs-lookup"><span data-stu-id="b3acb-103">Tutorial: Get started with ASP.NET Core SignalR</span></span>

<span data-ttu-id="b3acb-104">このチュートリアルでは、SignalR を使用してリアルタイム アプリをビルドするための基礎について説明します。</span><span class="sxs-lookup"><span data-stu-id="b3acb-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="b3acb-105">以下の方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="b3acb-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b3acb-106">Web プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="b3acb-106">Create a web project.</span></span>
> * <span data-ttu-id="b3acb-107">SignalR クライアント ライブラリを追加する。</span><span class="sxs-lookup"><span data-stu-id="b3acb-107">Add the SignalR client library.</span></span>
> * <span data-ttu-id="b3acb-108">SignalR ハブを作成する。</span><span class="sxs-lookup"><span data-stu-id="b3acb-108">Create a SignalR hub.</span></span>
> * <span data-ttu-id="b3acb-109">SignalR を使用するようにプロジェクトを構成する。</span><span class="sxs-lookup"><span data-stu-id="b3acb-109">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="b3acb-110">任意のクライアントから、接続されているすべてのクライアントにメッセージを送信するコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="b3acb-110">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="b3acb-111">最後には、次のように動作するチャット アプリが作成されます。</span><span class="sxs-lookup"><span data-stu-id="b3acb-111">At the end, you'll have a working chat app:</span></span>

![SignalR のサンプル アプリ](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="b3acb-113">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="b3acb-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

> [!NOTE]
> <span data-ttu-id="b3acb-114">ASP.NET Core の目次の提案された新しい構造の有用性をテストしています。</span><span class="sxs-lookup"><span data-stu-id="b3acb-114">We’re testing the usability of a proposed new structure for the ASP.NET Core table of contents.</span></span>  <span data-ttu-id="b3acb-115">現在または提案された目次で 7 つのトピックを探す演習をする時間がある場合は、[ここをクリックして、調査に参加してください](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82)。</span><span class="sxs-lookup"><span data-stu-id="b3acb-115">If you have a few minutes to try an exercise of finding 7 different topics in the current or proposed table of contents, please [click here to participate in the study](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82).</span></span>


[!INCLUDE [|Prerequisites](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-web-project"></a><span data-ttu-id="b3acb-116">Web プロジェクトを作成する</span><span class="sxs-lookup"><span data-stu-id="b3acb-116">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b3acb-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b3acb-117">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="b3acb-118">メニューから、**[ファイル]、[新規プロジェクト]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="b3acb-118">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="b3acb-119">**[新しいプロジェクト]** ダイアログで、**[インストール]、[Visual C#]、[Web]、[ASP.NET Core Web アプリケーション]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="b3acb-119">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="b3acb-120">プロジェクトに "*SignalRChat*" という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="b3acb-120">Name the project *SignalRChat*.</span></span>

  ![Visual Studio の [新しいプロジェクト] ダイアログ ボックス](signalr/_static/signalr-new-project-dialog.png)

* <span data-ttu-id="b3acb-122">**[Web アプリケーション]** を選択して、Razor Pages を使用するプロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="b3acb-122">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="b3acb-123">**.NET Core** のターゲット フレームワークを選択し、**ASP.NET Core 2.2** を選択して、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="b3acb-123">Select a target framework of **.NET Core**, select **ASP.NET Core 2.2**, and click **OK**.</span></span>

  ![Visual Studio の [新しいプロジェクト] ダイアログ ボックス](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b3acb-125">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b3acb-125">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="b3acb-126">新しいプロジェクト フォルダーを作成するフォルダーで[統合ターミナル](https://code.visualstudio.com/docs/editor/integrated-terminal)を開きます。</span><span class="sxs-lookup"><span data-stu-id="b3acb-126">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="b3acb-127">次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="b3acb-127">Run the following commands:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b3acb-128">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="b3acb-128">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="b3acb-129">メニューから、**[ファイル]、[新しいソリューション]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="b3acb-129">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="b3acb-130">**[.NET Core]、[アプリ]、[ASP.NET Core Web アプリ]** の順に選択します (**[ASP.NET Core Web アプリ (MVC)]** を選択しないでください)。</span><span class="sxs-lookup"><span data-stu-id="b3acb-130">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="b3acb-131">**[次へ]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="b3acb-131">Select **Next**.</span></span>

* <span data-ttu-id="b3acb-132">プロジェクトに *SignalRChat* という名前を付けて、**[作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="b3acb-132">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="b3acb-133">SignalR クライアント ライブラリを追加する</span><span class="sxs-lookup"><span data-stu-id="b3acb-133">Add the SignalR client library</span></span>

<span data-ttu-id="b3acb-134">SignalR サーバー ライブラリは、`Microsoft.AspNetCore.App` メタパッケージに含まれています。</span><span class="sxs-lookup"><span data-stu-id="b3acb-134">The SignalR server library is included in the `Microsoft.AspNetCore.App` metapackage.</span></span> <span data-ttu-id="b3acb-135">JavaScript クライアント ライブラリはプロジェクトに自動的に含まれません。</span><span class="sxs-lookup"><span data-stu-id="b3acb-135">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="b3acb-136">このチュートリアルでは、ライブラリ マネージャー (LibMan) を使用して *unpkg* からクライアント ライブラリを取得します。</span><span class="sxs-lookup"><span data-stu-id="b3acb-136">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="b3acb-137">unpkg は、npm (Node.js パッケージ マネージャー) で見つかるものすべてを配信できるコンテンツ配信ネットワーク (CDN) です。</span><span class="sxs-lookup"><span data-stu-id="b3acb-137">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b3acb-138">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b3acb-138">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="b3acb-139">**ソリューション エクスプローラー**で、プロジェクトを右クリックし、**[追加]** > **[クライアント側のライブラリ]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="b3acb-139">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="b3acb-140">**[Add Client-Side Library]\(クライアント側のライブラリの追加\)** ダイアログで、**[プロバイダー]** に対して **[unpkg]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="b3acb-140">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span> 

* <span data-ttu-id="b3acb-141">**[ライブラリ]** に対して「`@aspnet/signalr@1`」を入力し、プレビューでない最新のバージョンを選択します。</span><span class="sxs-lookup"><span data-stu-id="b3acb-141">For **Library**, enter `@aspnet/signalr@1`, and select the latest version that isn't preview.</span></span>

  ![クライアント側のライブラリの追加ダイアログ - ライブラリの選択](signalr/_static/libman1.png)

* <span data-ttu-id="b3acb-143">**[Choose specific files]\(特定のファイルの選択\)** を選択して *dist/browser* フォルダーを展開し、*signalr.js* と *signalr.min.js* を選択します。</span><span class="sxs-lookup"><span data-stu-id="b3acb-143">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="b3acb-144">**[ターゲットの場所]** を *wwwroot/lib/signalr/* に設定して、**[インストール]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="b3acb-144">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![クライアント側のライブラリの追加ダイアログ - ファイルと保存先の選択](signalr/_static/libman2.png)

  <span data-ttu-id="b3acb-146">LibMan によって *wwwroot/lib/signalr* フォルダーが作成され、そこに選択したファイルがコピーされます。</span><span class="sxs-lookup"><span data-stu-id="b3acb-146">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b3acb-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b3acb-147">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="b3acb-148">統合端末で、次のコマンドを実行して LibMan をインストールします。</span><span class="sxs-lookup"><span data-stu-id="b3acb-148">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="b3acb-149">次のコマンドを実行して、LibMan を使用して SignalR クライアント ライブラリを取得します。</span><span class="sxs-lookup"><span data-stu-id="b3acb-149">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="b3acb-150">出力が表示されるまでに数秒待機する必要がある場合があります。</span><span class="sxs-lookup"><span data-stu-id="b3acb-150">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="b3acb-151">パラメーターによって次のオプションが指定されます。</span><span class="sxs-lookup"><span data-stu-id="b3acb-151">The parameters specify the following options:</span></span>
  * <span data-ttu-id="b3acb-152">unpkg プロバイダーを使用する。</span><span class="sxs-lookup"><span data-stu-id="b3acb-152">Use the unpkg provider.</span></span>
  * <span data-ttu-id="b3acb-153">ファイルを *wwwroot/lib/signalr* にコピーする。</span><span class="sxs-lookup"><span data-stu-id="b3acb-153">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="b3acb-154">指定したファイルのみをコピーする。</span><span class="sxs-lookup"><span data-stu-id="b3acb-154">Copy only the specified files.</span></span>

  <span data-ttu-id="b3acb-155">出力は次の例のようになります。</span><span class="sxs-lookup"><span data-stu-id="b3acb-155">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b3acb-156">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="b3acb-156">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="b3acb-157">**[端末]** で、次のコマンドを実行して LibMan をインストールします。</span><span class="sxs-lookup"><span data-stu-id="b3acb-157">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="b3acb-158">プロジェクト フォルダー (ファイル *SignalRChat.csproj* を含んでいるフォルダー) に移動します。</span><span class="sxs-lookup"><span data-stu-id="b3acb-158">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="b3acb-159">次のコマンドを実行して、LibMan を使用して SignalR クライアント ライブラリを取得します。</span><span class="sxs-lookup"><span data-stu-id="b3acb-159">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="b3acb-160">パラメーターによって次のオプションが指定されます。</span><span class="sxs-lookup"><span data-stu-id="b3acb-160">The parameters specify the following options:</span></span>
  * <span data-ttu-id="b3acb-161">unpkg プロバイダーを使用する。</span><span class="sxs-lookup"><span data-stu-id="b3acb-161">Use the unpkg provider.</span></span>
  * <span data-ttu-id="b3acb-162">ファイルを *wwwroot/lib/signalr* にコピーする。</span><span class="sxs-lookup"><span data-stu-id="b3acb-162">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="b3acb-163">指定したファイルのみをコピーする。</span><span class="sxs-lookup"><span data-stu-id="b3acb-163">Copy only the specified files.</span></span>

  <span data-ttu-id="b3acb-164">出力は次の例のようになります。</span><span class="sxs-lookup"><span data-stu-id="b3acb-164">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="b3acb-165">SignalR ハブを作成する</span><span class="sxs-lookup"><span data-stu-id="b3acb-165">Create a SignalR hub</span></span>

<span data-ttu-id="b3acb-166">*ハブ*はクライアント サーバー通信を処理するハイレベル パイプラインとして機能するクラスです。</span><span class="sxs-lookup"><span data-stu-id="b3acb-166">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="b3acb-167">SignalRChat プロジェクト フォルダー内で、*Hubs* フォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="b3acb-167">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="b3acb-168">*Hubs* フォルダー内で、次のコードを使用して *ChatHub.cs* ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="b3acb-168">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  <span data-ttu-id="b3acb-169">`ChatHub` クラスは、SignalR `Hub` クラスを継承します。</span><span class="sxs-lookup"><span data-stu-id="b3acb-169">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="b3acb-170">`Hub` クラスでは、接続、グループ、およびメッセージングが管理されます。</span><span class="sxs-lookup"><span data-stu-id="b3acb-170">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="b3acb-171">`SendMessage` メソッドは、接続されている任意のクライアントによって呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="b3acb-171">The `SendMessage` method can be called by any connected client.</span></span> <span data-ttu-id="b3acb-172">それによって、受信したメッセージがすべてのクライアントに送信されます。</span><span class="sxs-lookup"><span data-stu-id="b3acb-172">It sends the received message to all clients.</span></span> <span data-ttu-id="b3acb-173">最大のスケーラビリティを実現するために、SignalR コードは非同期になっています。</span><span class="sxs-lookup"><span data-stu-id="b3acb-173">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="b3acb-174">SignalR を構成する</span><span class="sxs-lookup"><span data-stu-id="b3acb-174">Configure SignalR</span></span>

<span data-ttu-id="b3acb-175">SignalR 要求が SignalR に渡されるように SignalR サーバーを構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b3acb-175">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="b3acb-176">次の強調表示されたコードを *Startup.cs* ファイルに追加します。</span><span class="sxs-lookup"><span data-stu-id="b3acb-176">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="b3acb-177">これらの変更によって、SignalR が ASP.NET Core 依存関係挿入システムとミドルウェア パイプラインに追加されます。</span><span class="sxs-lookup"><span data-stu-id="b3acb-177">These changes add SignalR to the ASP.NET Core dependency injection system and the middleware pipeline.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="b3acb-178">SignalR クライアント コードを追加する</span><span class="sxs-lookup"><span data-stu-id="b3acb-178">Add SignalR client code</span></span>

* <span data-ttu-id="b3acb-179">*Pages\Index.cshtml* のコンテンツを次のコードに変更します。</span><span class="sxs-lookup"><span data-stu-id="b3acb-179">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  <span data-ttu-id="b3acb-180">上のコードでは以下の操作が行われます。</span><span class="sxs-lookup"><span data-stu-id="b3acb-180">The preceding code:</span></span>

  * <span data-ttu-id="b3acb-181">名前およびメッセージ テキスト用のテキスト ボックスと送信ボタンを作成します。</span><span class="sxs-lookup"><span data-stu-id="b3acb-181">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="b3acb-182">SignalR ハブから受信したメッセージを表示するために、`id="messagesList"` を使用してリストを作成します。</span><span class="sxs-lookup"><span data-stu-id="b3acb-182">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="b3acb-183">SignalR へのスクリプト参照と、次の手順で作成する *chat.js* アプリケーション コードを含めます。</span><span class="sxs-lookup"><span data-stu-id="b3acb-183">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="b3acb-184">*wwwroot/js* フォルダー内に、次のコードを使用して *chat.js* ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="b3acb-184">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  <span data-ttu-id="b3acb-185">上のコードでは以下の操作が行われます。</span><span class="sxs-lookup"><span data-stu-id="b3acb-185">The preceding code:</span></span>

  * <span data-ttu-id="b3acb-186">接続を作成して開始します。</span><span class="sxs-lookup"><span data-stu-id="b3acb-186">Creates and starts a connection.</span></span>
  * <span data-ttu-id="b3acb-187">ハブにメッセージを送信するハンドラーを送信ボタンに追加します。</span><span class="sxs-lookup"><span data-stu-id="b3acb-187">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="b3acb-188">ハブからメッセージを受信してからそれをリストに追加するハンドラーを接続オブジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="b3acb-188">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="b3acb-189">アプリを実行する</span><span class="sxs-lookup"><span data-stu-id="b3acb-189">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b3acb-190">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b3acb-190">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b3acb-191">**Ctrl + F5** キーを押して、デバッグを行わずにアプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="b3acb-191">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b3acb-192">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b3acb-192">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="b3acb-193">統合端末で、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="b3acb-193">In the integrated terminal, run the following command:</span></span>

  ```console
  dotnet run -p SignalRChat.csproj
  ```
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b3acb-194">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="b3acb-194">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="b3acb-195">メニューから、**[実行]、[デバッグなしで開始]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="b3acb-195">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="b3acb-196">アドレス バーから URL をコピーし、別のブラウザー インスタンスまたはタブを開き、アドレス バーに URL を貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="b3acb-196">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="b3acb-197">いずれかのブラウザーを選択し、名前とメッセージを入力し、**[Send Message]\(メッセージの送信\)** ボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="b3acb-197">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>

  <span data-ttu-id="b3acb-198">次の瞬間、両方のページに名前とメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="b3acb-198">The name and message are displayed on both pages instantly.</span></span>

  ![SignalR のサンプル アプリ](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="b3acb-200">アプリが動作しない場合は、ご利用のブラウザーの開発者ツール (F12) を開き、コンソールに移動します。</span><span class="sxs-lookup"><span data-stu-id="b3acb-200">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="b3acb-201">HTML および JavaScript コードに関連するエラーが発生している場合があります。</span><span class="sxs-lookup"><span data-stu-id="b3acb-201">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="b3acb-202">たとえば、*signalr.js* を指示されたフォルダーとは別のフォルダーに配置したとします。</span><span class="sxs-lookup"><span data-stu-id="b3acb-202">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="b3acb-203">その場合、そのファイルへの参照は機能せず、コンソールに 404 エラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="b3acb-203">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="b3acb-204">![エラー "signalr.js が見つかりませんでした"](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="b3acb-204">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="b3acb-205">次の手順</span><span class="sxs-lookup"><span data-stu-id="b3acb-205">Next steps</span></span>

<span data-ttu-id="b3acb-206">このチュートリアルでは、次の作業を行う方法を学びました。</span><span class="sxs-lookup"><span data-stu-id="b3acb-206">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b3acb-207">Web アプリ プロジェクトを作成する。</span><span class="sxs-lookup"><span data-stu-id="b3acb-207">Create a web app project.</span></span>
> * <span data-ttu-id="b3acb-208">SignalR クライアント ライブラリを追加する。</span><span class="sxs-lookup"><span data-stu-id="b3acb-208">Add the SignalR client library.</span></span>
> * <span data-ttu-id="b3acb-209">SignalR ハブを作成する。</span><span class="sxs-lookup"><span data-stu-id="b3acb-209">Create a SignalR hub.</span></span>
> * <span data-ttu-id="b3acb-210">SignalR を使用するようにプロジェクトを構成する。</span><span class="sxs-lookup"><span data-stu-id="b3acb-210">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="b3acb-211">ハブを使用して任意のクライアントから接続済みのすべてのクライアントにメッセージを送信する、コードを追加する。</span><span class="sxs-lookup"><span data-stu-id="b3acb-211">Add code that uses the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="b3acb-212">SignalR の詳細については、次の概要を参照してください。</span><span class="sxs-lookup"><span data-stu-id="b3acb-212">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="b3acb-213">ASP.NET Core SignalR の概要</span><span class="sxs-lookup"><span data-stu-id="b3acb-213">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
