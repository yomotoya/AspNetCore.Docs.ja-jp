---
title: 'チュートリアル: ASP.NET Core 上で SignalR の使用を開始する'
author: tdykstra
description: このチュートリアルでは、ASP.NET Core 用 SignalR を使用するチャット アプリを作成します。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/20/2018
uid: tutorials/signalr
ms.openlocfilehash: a2573e2817a2d8921954264ca17bc3a7e2a010a8
ms.sourcegitcommit: 847cc1de5526ff42a7303491e6336c2dbdb45de4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/27/2018
ms.locfileid: "43055833"
---
# <a name="tutorial-get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="6617e-103">チュートリアル: ASP.NET Core 上で SignalR の使用を開始する</span><span class="sxs-lookup"><span data-stu-id="6617e-103">Tutorial: Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="6617e-104">このチュートリアルでは、SignalR を使用してリアルタイム アプリをビルドするための基礎について説明します。</span><span class="sxs-lookup"><span data-stu-id="6617e-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="6617e-105">以下の方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="6617e-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6617e-106">ASP.NET Core 上で SignalR を使用する Web アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="6617e-106">Create a web app that uses SignalR on ASP.NET Core.</span></span>
> * <span data-ttu-id="6617e-107">サーバー上で SignalR ハブを作成します。</span><span class="sxs-lookup"><span data-stu-id="6617e-107">Create a SignalR hub on the server.</span></span>
> * <span data-ttu-id="6617e-108">JavaScript クライアントから SignalR ハブに接続します。</span><span class="sxs-lookup"><span data-stu-id="6617e-108">Connect to the SignalR hub from JavaScript clients.</span></span>
> * <span data-ttu-id="6617e-109">ハブを使用して、任意のクライアントから、接続されているすべてのクライアントにメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="6617e-109">Use the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="6617e-110">最後には、次のように動作するチャット アプリが作成されます。</span><span class="sxs-lookup"><span data-stu-id="6617e-110">At the end, you'll have a working chat app:</span></span>

![SignalR のサンプル アプリ](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="6617e-112">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="6617e-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6617e-113">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="6617e-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6617e-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6617e-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6617e-115">**ASP.NET および Web 開発**ワークロードを含む [Visual Studio 2017](https://www.visualstudio.com/downloads/) バージョン 15.7.3 以降</span><span class="sxs-lookup"><span data-stu-id="6617e-115">[Visual Studio 2017 version 15.7.3 or later](https://www.visualstudio.com/downloads/) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="6617e-116">.NET Core SDK 2.1 以降</span><span class="sxs-lookup"><span data-stu-id="6617e-116">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="6617e-117">[npm](https://www.npmjs.com/get-npm) (SignalR JavaScript クライアント ライブラリに使用される、Node.js のパッケージ マネージャー)。</span><span class="sxs-lookup"><span data-stu-id="6617e-117">[npm](https://www.npmjs.com/get-npm) (Package manager for Node.js, used for the SignalR JavaScript client library.)</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6617e-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6617e-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="6617e-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6617e-119">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="6617e-120">.NET Core SDK 2.1 以降</span><span class="sxs-lookup"><span data-stu-id="6617e-120">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="6617e-121">Visual Studio Code 用 C#</span><span class="sxs-lookup"><span data-stu-id="6617e-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* <span data-ttu-id="6617e-122">[npm](https://www.npmjs.com/get-npm) (SignalR JavaScript クライアント ライブラリに使用される、Node.js のパッケージ マネージャー)。</span><span class="sxs-lookup"><span data-stu-id="6617e-122">[npm](https://www.npmjs.com/get-npm) (Package manager for Node.js, used for the SignalR JavaScript client library.)</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6617e-123">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="6617e-123">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="6617e-124">Visual Studio for Mac バージョン 7.5.4 以降</span><span class="sxs-lookup"><span data-stu-id="6617e-124">Visual Studio for Mac version 7.5.4 or later</span></span>](https://www.visualstudio.com/downloads/)
* <span data-ttu-id="6617e-125">[.NET Core SDK 2.1 以降](https://www.microsoft.com/net/download/all) (Visual Studio インストールに含まれている)</span><span class="sxs-lookup"><span data-stu-id="6617e-125">[.NET Core SDK 2.1 or later](https://www.microsoft.com/net/download/all) (included in the Visual Studio install)</span></span>
* <span data-ttu-id="6617e-126">[npm](https://www.npmjs.com/get-npm) (SignalR JavaScript クライアント ライブラリに使用される、Node.js のパッケージ マネージャー)。</span><span class="sxs-lookup"><span data-stu-id="6617e-126">[npm](https://www.npmjs.com/get-npm) (Package manager for Node.js, used for the SignalR JavaScript client library.)</span></span>

---

## <a name="create-the-project"></a><span data-ttu-id="6617e-127">プロジェクトの作成</span><span class="sxs-lookup"><span data-stu-id="6617e-127">Create the project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6617e-128">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6617e-128">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="6617e-129">メニューから、**[ファイル]、[新規プロジェクト]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="6617e-129">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="6617e-130">**[新しいプロジェクト]** ダイアログで、**[インストール]、[Visual C#]、[Web]、[ASP.NET Core Web アプリケーション]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="6617e-130">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="6617e-131">プロジェクトに "*SignalRChat*" という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="6617e-131">Name the project *SignalRChat*.</span></span>

  ![Visual Studio の [新しいプロジェクト] ダイアログ ボックス](signalr/_static/signalr-new-project-dialog.png)

* <span data-ttu-id="6617e-133">**[Web アプリケーション]** を選択して、Razor Pages を使用するプロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="6617e-133">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="6617e-134">**.NET Core** のターゲット フレームワークを選択し、**ASP.NET Core 2.1** を選択して、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="6617e-134">Select a target framework of **.NET Core**, select **ASP.NET Core 2.1**, and click **OK**.</span></span>

  ![Visual Studio の [新しいプロジェクト] ダイアログ ボックス](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6617e-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6617e-136">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="6617e-137">新しいプロジェクトのために使用できるフォルダーを開きます。</span><span class="sxs-lookup"><span data-stu-id="6617e-137">Open a folder that you can use for a new project.</span></span>

* <span data-ttu-id="6617e-138">**[統合端末]** で、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="6617e-138">In the **Integrated Terminal**, run the following command:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   ```

   [!INCLUDE[](~/includes/webapp-alias-notice.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6617e-139">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="6617e-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="6617e-140">メニューから、**[ファイル]、[新しいソリューション]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="6617e-140">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="6617e-141">**[.NET Core]、[アプリ]、[ASP.NET Core Web アプリ]** の順に選択します (**[ASP.NET Core Web アプリ (MVC)]** を選択しないでください)。</span><span class="sxs-lookup"><span data-stu-id="6617e-141">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="6617e-142">**[次へ]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="6617e-142">Select **Next**.</span></span>

* <span data-ttu-id="6617e-143">プロジェクトに *SignalRChat* という名前を付けて、**[作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="6617e-143">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="6617e-144">SignalR クライアント ライブラリを追加する</span><span class="sxs-lookup"><span data-stu-id="6617e-144">Add the SignalR client library</span></span>

<span data-ttu-id="6617e-145">SignalR サーバー ライブラリは、[Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app) に含まれています。</span><span class="sxs-lookup"><span data-stu-id="6617e-145">The SignalR server library is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="6617e-146">しかし、JavaScript クライアント ライブラリについては、[npm (Node.js パッケージ マネージャー)](https://www.npmjs.com/get-npm) から取得する必要があります。</span><span class="sxs-lookup"><span data-stu-id="6617e-146">But you have to get the JavaScript client library from [npm, the Node.js package manager](https://www.npmjs.com/get-npm).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6617e-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6617e-147">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="6617e-148">**パッケージ マネージャー コンソール** (PMC) 内で、プロジェクト フォルダー (ファイル *SignalRChat.csproj* を含んでいるフォルダー) に移動します。</span><span class="sxs-lookup"><span data-stu-id="6617e-148">In **Package Manager Console** (PMC), change to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

  ```console
  cd SignalRChat
  ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6617e-149">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6617e-149">Visual Studio Code</span></span>](#tab/visual-studio-code/)

2. <span data-ttu-id="6617e-150">新しいプロジェクト フォルダーに移動します。</span><span class="sxs-lookup"><span data-stu-id="6617e-150">Change to the new project folder.</span></span>

  ```console
  cd SignalRChat
  ``` 

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6617e-151">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="6617e-151">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="6617e-152">**[端末]** で、プロジェクト フォルダー (ファイル *SignalRChat.csproj* を含んでいるフォルダー) に移動します。</span><span class="sxs-lookup"><span data-stu-id="6617e-152">In the **Terminal**, navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

---

* <span data-ttu-id="6617e-153">npm 初期化子を実行して、*package.json* ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="6617e-153">Run the npm initializer to create a *package.json* file:</span></span>

  ```console
  npm init -y
  ```

  <span data-ttu-id="6617e-154">このコマンドによって次の例に示すような出力が作成されます。</span><span class="sxs-lookup"><span data-stu-id="6617e-154">The command creates output similar to the following example:</span></span>

  ```console
  Wrote to C:\tmp\SignalRChat\package.json:
  {
    "name": "SignalRChat",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "scripts": {
      "test": "echo \"Error: no test specified\" && exit 1"
    },
    "keywords": [],
    "author": "",
    "license": "ISC"0
  }
  ```

* <span data-ttu-id="6617e-155">クライアント ライブラリ パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="6617e-155">Install the client library package:</span></span>

  ```console
  npm install @aspnet/signalr
  ```

  <span data-ttu-id="6617e-156">このコマンドによって次の例に示すような出力が作成されます。</span><span class="sxs-lookup"><span data-stu-id="6617e-156">The command creates output similar to the following example:</span></span>

  ```
  npm notice created a lockfile as package-lock.json. You should commit this file.
  npm WARN signalrchat@1.0.0 No description
  npm WARN signalrchat@1.0.0 No repository field.

  + @aspnet/signalr@1.0.2
  added 1 package in 0.98s
  ```

<span data-ttu-id="6617e-157">`npm install` コマンドでは、*node_modules* の下にあるサブフォルダーに JavaScript クライアント ライブラリがダウンロードされます。</span><span class="sxs-lookup"><span data-stu-id="6617e-157">The `npm install` command downloaded the JavaScript client library to a subfolder under *node_modules*.</span></span> <span data-ttu-id="6617e-158">それをそこからコピーして、チャット アプリの Web ページから参照できる *wwwroot* の下のフォルダーにコピーします。</span><span class="sxs-lookup"><span data-stu-id="6617e-158">Copy it from there to a folder under *wwwroot* that you can reference from the chat app web page.</span></span>

* <span data-ttu-id="6617e-159">*wwwroot/lib* 内に *signalr* フォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="6617e-159">Create a *signalr* folder in *wwwroot/lib*.</span></span>

* <span data-ttu-id="6617e-160">*signalr.js* ファイルを、*node_modules/@aspnet/signalr/dist/browser* から新しい *wwwroot/lib/signalr* フォルダーにコピーします。</span><span class="sxs-lookup"><span data-stu-id="6617e-160">Copy the *signalr.js* file from *node_modules/@aspnet/signalr/dist/browser* to the new *wwwroot/lib/signalr* folder.</span></span>

## <a name="create-the-signalr-hub"></a><span data-ttu-id="6617e-161">SignalR ハブを作成する</span><span class="sxs-lookup"><span data-stu-id="6617e-161">Create the SignalR hub</span></span>

<span data-ttu-id="6617e-162">[ハブ](xref:signalr/hubs)はクライアント サーバー通信を処理するハイレベル パイプラインとして機能するクラスです。</span><span class="sxs-lookup"><span data-stu-id="6617e-162">A [hub](xref:signalr/hubs) is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="6617e-163">SignalRChat プロジェクト フォルダー内で、*Hubs* フォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="6617e-163">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="6617e-164">*Hubs* フォルダー内で、次のコードを使用して *ChatHub.cs* ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="6617e-164">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  <span data-ttu-id="6617e-165">`ChatHub` クラスは、SignalR [Hub](/dotnet/api/microsoft.aspnetcore.signalr.hub) クラスを継承します。</span><span class="sxs-lookup"><span data-stu-id="6617e-165">The `ChatHub` class inherits from the SignalR [Hub](/dotnet/api/microsoft.aspnetcore.signalr.hub) class.</span></span> <span data-ttu-id="6617e-166">`Hub` クラスでは、接続、グループ、およびメッセージングが管理されます。</span><span class="sxs-lookup"><span data-stu-id="6617e-166">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="6617e-167">`SendMessage` メソッドは、接続されている任意のクライアントによって呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="6617e-167">The `SendMessage` method can be called by any connected client.</span></span> <span data-ttu-id="6617e-168">それによって、受信したメッセージがすべてのクライアントに送信されます。</span><span class="sxs-lookup"><span data-stu-id="6617e-168">It sends the received message to all clients.</span></span> <span data-ttu-id="6617e-169">最大のスケーラビリティを実現するために、SignalR コードは非同期になっています。</span><span class="sxs-lookup"><span data-stu-id="6617e-169">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="6617e-170">SignalR を使用するようにプロジェクトを構成する</span><span class="sxs-lookup"><span data-stu-id="6617e-170">Configure the project to use SignalR</span></span>

<span data-ttu-id="6617e-171">SignalR 要求が SignalR に渡されるように SignalR サーバーを構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="6617e-171">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="6617e-172">次の強調表示されたコードを *Startup.cs* ファイルに追加します。</span><span class="sxs-lookup"><span data-stu-id="6617e-172">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="6617e-173">これらの変更によって、SignalR が[依存関係挿入](xref:fundamentals/dependency-injection)システムと[ミドルウェア](xref:fundamentals/middleware/index) パイプラインに追加されます。</span><span class="sxs-lookup"><span data-stu-id="6617e-173">These changes add SignalR to the [dependency injection](xref:fundamentals/dependency-injection) system and the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="6617e-174">SignalR クライアント コードを作成する</span><span class="sxs-lookup"><span data-stu-id="6617e-174">Create the SignalR client code</span></span>

* <span data-ttu-id="6617e-175">*Pages\Index.cshtml* のコンテンツを次の内容に変更します。</span><span class="sxs-lookup"><span data-stu-id="6617e-175">Replace the content in *Pages\Index.cshtml* with the following:</span></span>

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  <span data-ttu-id="6617e-176">上のコードでは以下の操作が行われます。</span><span class="sxs-lookup"><span data-stu-id="6617e-176">The preceding code:</span></span>

  * <span data-ttu-id="6617e-177">名前およびメッセージ テキスト用のテキスト ボックスと送信ボタンを作成します。</span><span class="sxs-lookup"><span data-stu-id="6617e-177">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="6617e-178">SignalR ハブから受信したメッセージを表示するために、`id="messagesList"` を使用してリストを作成します。</span><span class="sxs-lookup"><span data-stu-id="6617e-178">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="6617e-179">SignalR へのスクリプト参照と、次の手順で作成する *chat.js* アプリケーション コードを含めます。</span><span class="sxs-lookup"><span data-stu-id="6617e-179">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="6617e-180">*wwwroot/js* フォルダー内に、次のコードを使用して *chat.js* ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="6617e-180">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  <span data-ttu-id="6617e-181">上のコードでは以下の操作が行われます。</span><span class="sxs-lookup"><span data-stu-id="6617e-181">The preceding code:</span></span>

  * <span data-ttu-id="6617e-182">接続を作成して開始します。</span><span class="sxs-lookup"><span data-stu-id="6617e-182">Creates and starts a connection.</span></span>
  * <span data-ttu-id="6617e-183">ハブにメッセージを送信するハンドラーを送信ボタンに追加します。</span><span class="sxs-lookup"><span data-stu-id="6617e-183">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="6617e-184">ハブからメッセージを受信してからそれをリストに追加するハンドラーを接続オブジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="6617e-184">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="6617e-185">アプリを実行する</span><span class="sxs-lookup"><span data-stu-id="6617e-185">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6617e-186">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6617e-186">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6617e-187">**Ctrl + F5** キーを押して、デバッグを行わずにアプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="6617e-187">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6617e-188">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6617e-188">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="6617e-189">**Ctrl + F5** キーを押して、デバッグを行わずにアプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="6617e-189">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6617e-190">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="6617e-190">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="6617e-191">メニューから、**[実行]、[デバッグなしで開始]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="6617e-191">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="6617e-192">アドレス バーから URL をコピーし、別のブラウザー インスタンスまたはタブを開き、アドレス バーに URL を貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="6617e-192">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="6617e-193">いずれかのブラウザーを選択し、名前とメッセージを入力し、**[送信]** ボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="6617e-193">Choose either browser, enter a name and message, and select the **Send** button.</span></span>

  <span data-ttu-id="6617e-194">次の瞬間、両方のページに名前とメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="6617e-194">The name and message are displayed on both pages instantly.</span></span>

  ![SignalR のサンプル アプリ](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="6617e-196">アプリが動作しない場合は、ご利用のブラウザーの開発者ツール (F12) を開き、コンソールに移動します。</span><span class="sxs-lookup"><span data-stu-id="6617e-196">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="6617e-197">HTML および JavaScript コードに関連するエラーが発生している場合があります。</span><span class="sxs-lookup"><span data-stu-id="6617e-197">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="6617e-198">たとえば、*signalr.js* を指示されたフォルダーとは別のフォルダーに配置したとします。</span><span class="sxs-lookup"><span data-stu-id="6617e-198">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="6617e-199">その場合、そのファイルへの参照は機能せず、コンソールに 404 エラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="6617e-199">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="6617e-200">![エラー "signalr.js が見つかりませんでした"](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="6617e-200">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="6617e-201">次の手順</span><span class="sxs-lookup"><span data-stu-id="6617e-201">Next steps</span></span>

<span data-ttu-id="6617e-202">クライアントが別のドメインから SignalR アプリに接続するようにしたい場合は、クロス オリジン リソース共有 (CORS) を有効にする必要です。</span><span class="sxs-lookup"><span data-stu-id="6617e-202">If you want clients to connect to a SignalR app from different domains, you have to enable Cross-Origin Resource Sharing (CORS).</span></span> <span data-ttu-id="6617e-203">詳細については、「[クロス オリジン リソース共有](xref:signalr/security?view=aspnetcore-2.1#cross-origin-resource-sharing)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="6617e-203">For more information, see [Cross-origin resource sharing](xref:signalr/security?view=aspnetcore-2.1#cross-origin-resource-sharing).</span></span>

<span data-ttu-id="6617e-204">SignalR、ハブ、および JavaScript クライアントの詳細については、次のリソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="6617e-204">To learn more about SignalR, hubs, and JavaScript clients, see these resources:</span></span>

* [<span data-ttu-id="6617e-205">ASP.NET Core 用 SignalR の概要</span><span class="sxs-lookup"><span data-stu-id="6617e-205">Introduction to SignalR for ASP.NET Core</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="6617e-206">ASP.NET Core 用 SignalR 内でハブを使用する</span><span class="sxs-lookup"><span data-stu-id="6617e-206">Use hubs in SignalR for ASP.NET Core</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="6617e-207">ASP.NET Core SignalR JavaScript クライアント</span><span class="sxs-lookup"><span data-stu-id="6617e-207">ASP.NET Core SignalR JavaScript client</span></span>](xref:signalr/javascript-client)
