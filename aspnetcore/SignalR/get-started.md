---
title: ASP.NET Core の SignalR を概要します。
author: rachelappel
description: このチュートリアルでは、ASP.NET Core の SignalR を使用してアプリを作成します。
manager: wpickett
ms.author: rachelap
ms.custom: mvc
ms.date: 03/16/2018
ms.prod: aspnet-core
ms.topic: tutorial
ms.technology: aspnet
uid: signalr/get-started
ms.openlocfilehash: cf120d535c85c7871f5b1f27039018ea2405b9cb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="0f7e3-103">ASP.NET Core の SignalR を概要します。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-103">Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="0f7e3-104">作成者: [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="0f7e3-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

[!INCLUDE [Version notice](../includes/signalr-version-notice.md)]

<span data-ttu-id="0f7e3-105">このチュートリアルでは ASP.NET Core の SignalR を使用してリアルタイム アプリの構築の基礎を説明します。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-105">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![ソリューション](get-started/_static/signalr-get-started-finished.png)

<span data-ttu-id="0f7e3-107">このチュートリアルでは、次の SignalR 開発タスクについて説明します。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-107">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0f7e3-108">ASP.NET Core web アプリで、SignalR を作成します。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-108">Create a SignalR on ASP.NET Core web app.</span></span>
> * <span data-ttu-id="0f7e3-109">クライアントにコンテンツをプッシュする SignalR ハブを作成します。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-109">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="0f7e3-110">変更、`Startup`クラスし、アプリを構成します。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-110">Modify the `Startup` class and configure the app.</span></span>

<span data-ttu-id="0f7e3-111">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="prerequisites"></a><span data-ttu-id="0f7e3-112">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="0f7e3-112">Prerequisites</span></span>

<span data-ttu-id="0f7e3-113">次のソフトウェアをインストールします。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-113">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0f7e3-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0f7e3-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0f7e3-115">[.NET core 2.1.0 Preview 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1)以降</span><span class="sxs-lookup"><span data-stu-id="0f7e3-115">[.NET Core 2.1.0 Preview 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) or later</span></span>
* <span data-ttu-id="0f7e3-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) 15.6 またはそれ以降のバージョン、 **ASP.NET および web 開発**ワークロード</span><span class="sxs-lookup"><span data-stu-id="0f7e3-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.6 or later with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="0f7e3-117">npm</span><span class="sxs-lookup"><span data-stu-id="0f7e3-117">npm</span></span>](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0f7e3-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0f7e3-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="0f7e3-119">[.NET core 2.1.0 Preview 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1)以降</span><span class="sxs-lookup"><span data-stu-id="0f7e3-119">[.NET Core 2.1.0 Preview 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) or later</span></span>
* [<span data-ttu-id="0f7e3-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0f7e3-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download) 
* [<span data-ttu-id="0f7e3-121">Visual Studio のコードの C#</span><span class="sxs-lookup"><span data-stu-id="0f7e3-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="0f7e3-122">npm</span><span class="sxs-lookup"><span data-stu-id="0f7e3-122">npm</span></span>](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="0f7e3-123">SignalR クライアントとサーバーをホストする ASP.NET Core プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-123">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

#### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0f7e3-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0f7e3-124">Visual Studio</span></span>](#tab/visual-studio/)
1. <span data-ttu-id="0f7e3-125">使用して、**ファイル** > **新しいプロジェクト**メニュー オプションを**ASP.NET Core Web アプリケーション**です。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-125">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="0f7e3-126">プロジェクトに名前を*SignalRChat*です。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-126">Name the project *SignalRChat*.</span></span>

   ![Visual Studio で新しいプロジェクト ダイアログ ボックス](get-started/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="0f7e3-128">選択**Web アプリケーション**Razor ページを使用してプロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-128">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="0f7e3-129">選択し、 **OK**です。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-129">Then select **OK**.</span></span> <span data-ttu-id="0f7e3-130">必ず**ASP.NET Core 2.1** SignalR は、.NET の以前のバージョンが実行される場合は、framework セレクターから選択します。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-130">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

   ![Visual Studio で新しいプロジェクト ダイアログ ボックス](get-started/_static/signalr-new-project-choose-type.png)

3. <span data-ttu-id="0f7e3-132">プロジェクトを右クリックして**ソリューション エクスプ ローラー** > **追加** > **新しい項目の** > **npm 構成ファイル**.</span><span class="sxs-lookup"><span data-stu-id="0f7e3-132">Right-click the project in **Solution Explorer** > **Add** > **New Item** > **npm Configuration File**.</span></span> <span data-ttu-id="0f7e3-133">ファイルの名前を付けます*package.json*です。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-133">Name the file *package.json*.</span></span>

4. <span data-ttu-id="0f7e3-134">次のコマンドを実行、 **Package Manager Console**ウィンドウで、プロジェクトのルートから。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-134">Run the following command in the **Package Manager Console** window, from the project root:</span></span>

    ```console
      npm install @aspnet/signalr
    ```
5. <span data-ttu-id="0f7e3-135">コピー、 <em>signalr.js</em>ファイルから<em>node_modules\\ @aspnet\signalr\dist\browser</em> を<em>wwwroot\lib</em>プロジェクトのフォルダーにします。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-135">Copy the <em>signalr.js</em> file from <em>node_modules\\@aspnet\signalr\dist\browser</em> to the <em>wwwroot\lib</em> folder in your project.</span></span>

#### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0f7e3-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0f7e3-136">Visual Studio Code</span></span>](#tab/visual-studio-code/)
1. <span data-ttu-id="0f7e3-137">**統合ターミナル**、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-137">From the **Integrated Terminal**, run the following command:</span></span>

    ```console
      dotnet new razor -o SignalRChat
    ```

2. <span data-ttu-id="0f7e3-138">JavaScript クライアント ライブラリを使用して、インストール*npm*です。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-138">Install the JavaScript client library using *npm*.</span></span>

    ```
      npm init -y
      npm install @aspnet/signalr
    ```

* * *
## <a name="create-the-signalr-hub"></a><span data-ttu-id="0f7e3-139">SignalR ハブを作成します。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-139">Create the SignalR Hub</span></span>

<span data-ttu-id="0f7e3-140">ハブは、クライアントとサーバーが互いにメソッドを呼び出すことができる高度なパイプラインとして機能するクラスです。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-140">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

#### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0f7e3-141">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0f7e3-141">Visual Studio</span></span>](#tab/visual-studio/)
1. <span data-ttu-id="0f7e3-142">クラスを選択してプロジェクトに追加**ファイル** > **新規** > **ファイル**を選択して**Visual c# クラス**です。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-142">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span>

2. <span data-ttu-id="0f7e3-143">継承`Microsoft.AspNetCore.SignalR.Hub`です。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-143">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="0f7e3-144">`Hub`プロパティおよび接続とグループ、さらに送信側と受信側のデータを管理するためのイベント クラスが含まれています。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-144">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

3. <span data-ttu-id="0f7e3-145">作成、`SendMessage`接続されているチャットのすべてのクライアントにメッセージを送信するメソッド。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-145">Create the `SendMessage` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="0f7e3-146">返すことを確認、[タスク](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task(v=vs.110).aspx)SignalR が非同期であるためです。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-146">Notice it returns a [Task](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task(v=vs.110).aspx), because SignalR is asynchronous.</span></span> <span data-ttu-id="0f7e3-147">非同期コードの拡張性が優れています。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-147">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs?range=7-14)]

#### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0f7e3-148">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0f7e3-148">Visual Studio Code</span></span>](#tab/visual-studio-code/)
1. <span data-ttu-id="0f7e3-149">開く、 *SignalRChat* Visual Studio のコード内のフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-149">Open the *SignalRChat* folder in Visual Studio Code.</span></span>

2. <span data-ttu-id="0f7e3-150">選択して、プロジェクトにクラスを追加**ファイル** > **新しいファイル** メニューからです。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-150">Add a class to the project by selecting **File** > **New File** from the menu.</span></span>

3. <span data-ttu-id="0f7e3-151">継承`Microsoft.AspNetCore.SignalR.Hub`です。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-151">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="0f7e3-152">`Hub`プロパティおよび接続とグループだけでなくクライアントに送信側と受信側のデータを管理するためのイベント クラスが含まれています。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-152">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data to clients.</span></span>

4. <span data-ttu-id="0f7e3-153">`SendMessage` メソッドをクラスに追加します。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-153">Add a `SendMessage` method to the class.</span></span> <span data-ttu-id="0f7e3-154">`SendMessage`メソッドは、接続されているチャットのすべてのクライアントにメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-154">The `SendMessage` method sends a message to all connected chat clients.</span></span> <span data-ttu-id="0f7e3-155">返すことを確認、[タスク](/dotnet/api/system.threading.tasks.task)SignalR が非同期であるためです。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-155">Notice it returns a [Task](/dotnet/api/system.threading.tasks.task), because SignalR is asynchronous.</span></span> <span data-ttu-id="0f7e3-156">非同期コードの拡張性が優れています。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-156">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs?range=7-14)]

* * *
## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="0f7e3-157">SignalR を使用するプロジェクトを構成します。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-157">Configure the project to use SignalR</span></span>

<span data-ttu-id="0f7e3-158">SignalR に要求を渡すを認識できるように、SignalR のサーバーを構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-158">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="0f7e3-159">SignalR プロジェクトを構成するには、変更、プロジェクトの`Startup.ConfigureServices`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-159">To configure a SignalR project, modify the project's `Startup.ConfigureServices` method.</span></span>

   <span data-ttu-id="0f7e3-160">`services.AddSignalR` 一部として SignalR を追加、[ミドルウェア](xref:fundamentals/middleware/index)パイプライン。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-160">`services.AddSignalR` adds SignalR as part of the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

2. <span data-ttu-id="0f7e3-161">使用して、ハブへのルートを構成する`UseSignalR`です。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-161">Configure routes to your hubs using `UseSignalR`.</span></span>

   [!code-csharp[Startup](get-started/sample/Startup.cs?highlight=22,40-43)]

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="0f7e3-162">SignalR クライアント コードを作成します。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-162">Create the SignalR client code</span></span>

1. <span data-ttu-id="0f7e3-163">コンテンツを置き換える*Pages\Index.cshtml*を次のコード。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-163">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

   [!code-cshtml[Index](get-started/sample/Pages/Index.cshtml)]

   <span data-ttu-id="0f7e3-164">上記の HTML では、名前とメッセージ フィールド、および送信ボタンが表示されます。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-164">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="0f7e3-165">下部にあるスクリプト参照に注意してください: SignalR への参照と*chat.js*です。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-165">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

2. <span data-ttu-id="0f7e3-166">という名前の JavaScript ファイルを追加*chat.js*を*wwwroot\js*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-166">Add a JavaScript file, named *chat.js*, to the *wwwroot\js* folder.</span></span> <span data-ttu-id="0f7e3-167">これに次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-167">Add the following code to it:</span></span>

   [!code-javascript[Index](get-started/sample/wwwroot/js/chat.js)]

## <a name="run-the-app"></a><span data-ttu-id="0f7e3-168">アプリを実行する</span><span class="sxs-lookup"><span data-stu-id="0f7e3-168">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0f7e3-169">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0f7e3-169">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="0f7e3-170">選択**デバッグ** > **デバッグなしで開始**ブラウザーを起動して、ローカルの web サイトを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-170">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="0f7e3-171">アドレス バーから URL をコピーします。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-171">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="0f7e3-172">別のブラウザー インスタンス (任意のブラウザー) を開き、アドレス バーに URL を貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-172">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="0f7e3-173">いずれかのブラウザーを選択し、名前と、メッセージを入力してをクリックして、**送信**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-173">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="0f7e3-174">名前とメッセージ」は即座に両方のページに表示されます。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-174">The name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0f7e3-175">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0f7e3-175">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="0f7e3-176">**[デバッグ]** (F5) を押してプログラムをビルドし、実行します。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-176">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="0f7e3-177">プログラムを実行すると、ブラウザー ウィンドウが開きます。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-177">Running the program opens a browser window.</span></span>

1. <span data-ttu-id="0f7e3-178">別のブラウザー ウィンドウを開き、ローカルに web サイトをロードします。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-178">Open another browser window and load the website locally in it.</span></span>

1. <span data-ttu-id="0f7e3-179">いずれかのブラウザーを選択し、名前と、メッセージを入力してをクリックして、**送信**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-179">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="0f7e3-180">名前とメッセージ」は即座に両方のページに表示されます。</span><span class="sxs-lookup"><span data-stu-id="0f7e3-180">The name and message are displayed on both pages instantly.</span></span>

-----

  ![ソリューション](get-started/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a><span data-ttu-id="0f7e3-182">関連資料</span><span class="sxs-lookup"><span data-stu-id="0f7e3-182">Related resources</span></span>

[<span data-ttu-id="0f7e3-183">ASP.NET Core SignalR の概要</span><span class="sxs-lookup"><span data-stu-id="0f7e3-183">Introduction to ASP.NET Core SignalR</span></span>](introduction.md)