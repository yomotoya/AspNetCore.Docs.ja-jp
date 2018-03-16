---
title: "ASP.NET Core の SignalR を概要します。"
author: rachelappel
description: "このチュートリアルでは、ASP.NET Core の SignalR を使用してアプリを作成します。"
manager: wpickett
ms.author: rachelap
ms.custom: mvc
ms.date: 03/16/2018
ms.prod: aspnet-core
ms.topic: tutorial
ms.technology: aspnet
uid: signalr/get-started-signalr-core
ms.openlocfilehash: 139da5a2d0dadf51fece94b7c54ccd531e0ae8c2
ms.sourcegitcommit: 6548a3dd0cd1e3e92ac2310dee757ddad9fd6456
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/16/2018
---
# <a name="tutorial-get-started-with-signalr-for-aspnet-core"></a><span data-ttu-id="f66b5-103">チュートリアル: SignalR による ASP.NET Core の開始します。</span><span class="sxs-lookup"><span data-stu-id="f66b5-103">Tutorial: Get started with SignalR for ASP.NET Core</span></span>

<span data-ttu-id="f66b5-104">作成者: [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="f66b5-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="f66b5-105">このチュートリアルでは ASP.NET Core の SignalR を使用してリアルタイム アプリの構築の基礎を説明します。</span><span class="sxs-lookup"><span data-stu-id="f66b5-105">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![ソリューション](get-started-signalr-core/_static/signalr-get-started-finished.png)

<span data-ttu-id="f66b5-107">このチュートリアルでは、次の SignalR 開発タスクについて説明します。</span><span class="sxs-lookup"><span data-stu-id="f66b5-107">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f66b5-108">ASP.NET Core web アプリで、SignalR を作成します。</span><span class="sxs-lookup"><span data-stu-id="f66b5-108">Create a SignalR on ASP.NET Core web app.</span></span>
> * <span data-ttu-id="f66b5-109">クライアントにコンテンツをプッシュする SignalR ハブを作成します。</span><span class="sxs-lookup"><span data-stu-id="f66b5-109">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="f66b5-110">変更、`Startup`クラスし、アプリを構成します。</span><span class="sxs-lookup"><span data-stu-id="f66b5-110">Modify the `Startup` class and configure the app.</span></span>

<span data-ttu-id="f66b5-111">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started-signalr-core/sample/)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="f66b5-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started-signalr-core/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="prerequisites"></a><span data-ttu-id="f66b5-112">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="f66b5-112">Prerequisites</span></span>

<span data-ttu-id="f66b5-113">次のソフトウェアをインストールします。</span><span class="sxs-lookup"><span data-stu-id="f66b5-113">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f66b5-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f66b5-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f66b5-115">[.NET core 2.1.0 Preview 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1)以降</span><span class="sxs-lookup"><span data-stu-id="f66b5-115">[.NET Core 2.1.0 Preview 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) or later</span></span>
* <span data-ttu-id="f66b5-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) 15.6 またはそれ以降のバージョン、 **ASP.NET および web 開発**ワークロード</span><span class="sxs-lookup"><span data-stu-id="f66b5-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.6 or later with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="f66b5-117">npm</span><span class="sxs-lookup"><span data-stu-id="f66b5-117">npm</span></span>](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f66b5-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f66b5-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="f66b5-119">[.NET core 2.1.0 Preview 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1)以降</span><span class="sxs-lookup"><span data-stu-id="f66b5-119">[.NET Core 2.1.0 Preview 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) or later</span></span>
* [<span data-ttu-id="f66b5-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f66b5-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download) 
* [<span data-ttu-id="f66b5-121">Visual Studio のコードの C#</span><span class="sxs-lookup"><span data-stu-id="f66b5-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="f66b5-122">npm</span><span class="sxs-lookup"><span data-stu-id="f66b5-122">npm</span></span>](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="f66b5-123">SignalR クライアントとサーバーをホストする ASP.NET Core プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="f66b5-123">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f66b5-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f66b5-124">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="f66b5-125">使用して、**ファイル** > **新しいプロジェクト**メニュー オプションを**ASP.NET Core Web アプリケーション**です。</span><span class="sxs-lookup"><span data-stu-id="f66b5-125">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="f66b5-126">プロジェクトに名前を*SignalRChat*です。</span><span class="sxs-lookup"><span data-stu-id="f66b5-126">Name the project *SignalRChat*.</span></span>

  ![Visual Studio で新しいプロジェクト ダイアログ ボックス](get-started-signalr-core/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="f66b5-128">選択**Web アプリケーション**Razor ページを使用してプロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="f66b5-128">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="f66b5-129">選択し、 **OK**です。</span><span class="sxs-lookup"><span data-stu-id="f66b5-129">Then select **OK**.</span></span> <span data-ttu-id="f66b5-130">必ず**ASP.NET Core 2.1** SignalR は、.NET の以前のバージョンが実行される場合は、framework セレクターから選択します。</span><span class="sxs-lookup"><span data-stu-id="f66b5-130">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

  ![Visual Studio で新しいプロジェクト ダイアログ ボックス](get-started-signalr-core/_static/signalr-new-project-choose-type.png)

3. <span data-ttu-id="f66b5-132">プロジェクトを右クリックして**ソリューション エクスプ ローラー** > **追加** > **新しい項目の** > **npm 構成ファイル**.</span><span class="sxs-lookup"><span data-stu-id="f66b5-132">Right-click the project in **Solution Explorer** > **Add** > **New Item** > **npm Configuration File**.</span></span> <span data-ttu-id="f66b5-133">ファイルの名前を付けます*package.json*です。</span><span class="sxs-lookup"><span data-stu-id="f66b5-133">Name the file *package.json*.</span></span>

4. <span data-ttu-id="f66b5-134">次のコマンドを実行、 **Package Manager Console**ウィンドウで、プロジェクトのルートから。</span><span class="sxs-lookup"><span data-stu-id="f66b5-134">Run the following command in the **Package Manager Console** window, from the project root:</span></span>

    ```console
      npm install @aspnet/signalr
    ```
5. <span data-ttu-id="f66b5-135">コピー、 *signalr.js*ファイルから*node_modules\\ @aspnet\signalr\dist\browser* を*wwwroot\lib*プロジェクトのフォルダーにします。</span><span class="sxs-lookup"><span data-stu-id="f66b5-135">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to the *wwwroot\lib* folder in your project.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f66b5-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f66b5-136">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="f66b5-137">**統合ターミナル**、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="f66b5-137">From the **Integrated Terminal**, run the following command:</span></span>
 
    ```console
      dotnet new razor -o SignalRChat
    ```

2. <span data-ttu-id="f66b5-138">JavaScript クライアント ライブラリを使用して、インストール*npm*です。</span><span class="sxs-lookup"><span data-stu-id="f66b5-138">Install the JavaScript client library using *npm*.</span></span>

    ```
      npm init -y
      npm install @aspnet/signalr
    ```

-----

## <a name="create-the-signalr-hub"></a><span data-ttu-id="f66b5-139">SignalR ハブを作成します。</span><span class="sxs-lookup"><span data-stu-id="f66b5-139">Create the SignalR Hub</span></span>

<span data-ttu-id="f66b5-140">ハブは、クライアントとサーバーが互いにメソッドを呼び出すことができる高度なパイプラインとして機能するクラスです。</span><span class="sxs-lookup"><span data-stu-id="f66b5-140">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f66b5-141">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f66b5-141">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="f66b5-142">クラスを選択してプロジェクトに追加**ファイル** > **新規** > **ファイル**を選択して**Visual c# クラス**です。</span><span class="sxs-lookup"><span data-stu-id="f66b5-142">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span>

1. <span data-ttu-id="f66b5-143">継承`Microsoft.AspNetCore.SignalR.Hub`です。</span><span class="sxs-lookup"><span data-stu-id="f66b5-143">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="f66b5-144">`Hub`プロパティおよび接続とグループ、さらに送信側と受信側のデータを管理するためのイベント クラスが含まれています。</span><span class="sxs-lookup"><span data-stu-id="f66b5-144">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

1. <span data-ttu-id="f66b5-145">作成、`SendMessage`接続されているチャットのすべてのクライアントにメッセージを送信するメソッド。</span><span class="sxs-lookup"><span data-stu-id="f66b5-145">Create the `SendMessage` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="f66b5-146">返すことを確認、[タスク](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task(v=vs.110).aspx)SignalR が非同期であるためです。</span><span class="sxs-lookup"><span data-stu-id="f66b5-146">Notice it returns a [Task](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task(v=vs.110).aspx), because SignalR is asynchronous.</span></span> <span data-ttu-id="f66b5-147">非同期コードの拡張性が優れています。</span><span class="sxs-lookup"><span data-stu-id="f66b5-147">Asynchronous code scales better.</span></span>

  [!code-csharp[Startup](get-started-signalr-core/sample/Hubs/ChatHub.cs?range=7-14)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f66b5-148">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f66b5-148">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="f66b5-149">開く、 *SignalRChat* Visual Studio のコード内のフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="f66b5-149">Open the *SignalRChat* folder in Visual Studio Code.</span></span>

1. <span data-ttu-id="f66b5-150">選択して、プロジェクトにクラスを追加**ファイル** > **新しいファイル** メニューからです。</span><span class="sxs-lookup"><span data-stu-id="f66b5-150">Add a class to the project by selecting **File** > **New File** from the menu.</span></span>

1. <span data-ttu-id="f66b5-151">継承`Microsoft.AspNetCore.SignalR.Hub`です。</span><span class="sxs-lookup"><span data-stu-id="f66b5-151">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="f66b5-152">`Hub`プロパティおよび接続とグループだけでなくクライアントに送信側と受信側のデータを管理するためのイベント クラスが含まれています。</span><span class="sxs-lookup"><span data-stu-id="f66b5-152">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data to clients.</span></span>

1. <span data-ttu-id="f66b5-153">`SendMessage` メソッドをクラスに追加します。</span><span class="sxs-lookup"><span data-stu-id="f66b5-153">Add a `SendMessage` method to the class.</span></span> <span data-ttu-id="f66b5-154">`SendMessage`メソッドは、接続されているチャットのすべてのクライアントにメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="f66b5-154">The `SendMessage` method sends a message to all connected chat clients.</span></span> <span data-ttu-id="f66b5-155">返すことを確認、[タスク](/dotnet/api/system.threading.tasks.task)SignalR が非同期であるためです。</span><span class="sxs-lookup"><span data-stu-id="f66b5-155">Notice it returns a [Task](/dotnet/api/system.threading.tasks.task), because SignalR is asynchronous.</span></span> <span data-ttu-id="f66b5-156">非同期コードの拡張性が優れています。</span><span class="sxs-lookup"><span data-stu-id="f66b5-156">Asynchronous code scales better.</span></span>

  [!code-csharp[Startup](get-started-signalr-core/sample/Hubs/ChatHub.cs?range=7-14)]

-----

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="f66b5-157">SignalR を使用するプロジェクトを構成します。</span><span class="sxs-lookup"><span data-stu-id="f66b5-157">Configure the project to use SignalR</span></span>

<span data-ttu-id="f66b5-158">SignalR に要求を渡すを認識できるように、SignalR のサーバーを構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f66b5-158">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="f66b5-159">SignalR プロジェクトを構成するには、変更、プロジェクトの`Startup.ConfigureServices`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="f66b5-159">To configure a SignalR project, modify the project's `Startup.ConfigureServices` method.</span></span>

  <span data-ttu-id="f66b5-160">`services.AddSignalR` 一部として SignalR を追加、[ミドルウェア](xref:fundamentals/middleware/index)パイプライン。</span><span class="sxs-lookup"><span data-stu-id="f66b5-160">`services.AddSignalR` adds SignalR as part of the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

1. <span data-ttu-id="f66b5-161">使用して、ハブへのルートを構成する`UseSignalR`です。</span><span class="sxs-lookup"><span data-stu-id="f66b5-161">Configure routes to your hubs using `UseSignalR`.</span></span>

  [!code-csharp[Startup](get-started-signalr-core/sample/Startup.cs?highlight=22,40-43)]

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="f66b5-162">SignalR クライアント コードを作成します。</span><span class="sxs-lookup"><span data-stu-id="f66b5-162">Create the SignalR client code</span></span>

1. <span data-ttu-id="f66b5-163">コンテンツを置き換える*Pages\Index.cshtml*を次のコード。</span><span class="sxs-lookup"><span data-stu-id="f66b5-163">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](get-started-signalr-core/sample/Pages/Index.cshtml)]

  <span data-ttu-id="f66b5-164">上記の HTML では、名前とメッセージ フィールド、および送信ボタンが表示されます。</span><span class="sxs-lookup"><span data-stu-id="f66b5-164">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="f66b5-165">下部にあるスクリプト参照に注意してください: SignalR への参照と*chat.js*です。</span><span class="sxs-lookup"><span data-stu-id="f66b5-165">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

1. <span data-ttu-id="f66b5-166">という名前の JavaScript ファイルを追加*chat.js*を*wwwroot\js*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="f66b5-166">Add a JavaScript file, named *chat.js*, to the *wwwroot\js* folder.</span></span> <span data-ttu-id="f66b5-167">これに次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="f66b5-167">Add the following code to it:</span></span>

  [!code-javascript[Index](get-started-signalr-core/sample/wwwroot/js/chat.js)]

## <a name="run-the-app"></a><span data-ttu-id="f66b5-168">アプリを実行する</span><span class="sxs-lookup"><span data-stu-id="f66b5-168">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f66b5-169">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f66b5-169">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="f66b5-170">選択**デバッグ** > **デバッグなしで開始**ブラウザーを起動して、ローカルの web サイトを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="f66b5-170">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="f66b5-171">アドレス バーから URL をコピーします。</span><span class="sxs-lookup"><span data-stu-id="f66b5-171">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="f66b5-172">別のブラウザー インスタンス (任意のブラウザー) を開き、アドレス バーに URL を貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="f66b5-172">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="f66b5-173">いずれかのブラウザーを選択し、名前と、メッセージを入力してをクリックして、**送信**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="f66b5-173">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="f66b5-174">名前とメッセージ」は即座に両方のページに表示されます。</span><span class="sxs-lookup"><span data-stu-id="f66b5-174">The name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f66b5-175">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f66b5-175">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="f66b5-176">**[デバッグ]** (F5) を押してプログラムをビルドし、実行します。</span><span class="sxs-lookup"><span data-stu-id="f66b5-176">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="f66b5-177">プログラムを実行すると、ブラウザー ウィンドウが開きます。</span><span class="sxs-lookup"><span data-stu-id="f66b5-177">Running the program opens a browser window.</span></span>

1. <span data-ttu-id="f66b5-178">別のブラウザー ウィンドウを開き、ローカルに web サイトをロードします。</span><span class="sxs-lookup"><span data-stu-id="f66b5-178">Open another browser window and load the website locally in it.</span></span>

1. <span data-ttu-id="f66b5-179">いずれかのブラウザーを選択し、名前と、メッセージを入力してをクリックして、**送信**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="f66b5-179">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="f66b5-180">名前とメッセージ」は即座に両方のページに表示されます。</span><span class="sxs-lookup"><span data-stu-id="f66b5-180">The name and message are displayed on both pages instantly.</span></span>

-----

  ![ソリューション](get-started-signalr-core/_static/signalr-get-started-finished.png)