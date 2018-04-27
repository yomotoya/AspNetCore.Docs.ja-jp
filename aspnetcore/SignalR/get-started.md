---
title: ASP.NET Core の SignalR を概要します。
author: rachelappel
description: このチュートリアルでは、ASP.NET Core の SignalR を使用してアプリを作成します。
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 03/16/2018
ms.prod: aspnet-core
ms.topic: tutorial
ms.technology: aspnet
uid: signalr/get-started
ms.openlocfilehash: 03735359bb22cc3085ddc7b34372ecfc9501a940
ms.sourcegitcommit: 07903a1be39a99dcf538d57981161592d0e658b8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/20/2018
---
# <a name="get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="62a51-103">ASP.NET Core の SignalR を概要します。</span><span class="sxs-lookup"><span data-stu-id="62a51-103">Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="62a51-104">作成者: [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="62a51-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

[!INCLUDE [2.1 preview notice](~/includes/2.1.md)]

<span data-ttu-id="62a51-105">このチュートリアルでは ASP.NET Core の SignalR を使用してリアルタイム アプリの構築の基礎を説明します。</span><span class="sxs-lookup"><span data-stu-id="62a51-105">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![ソリューション](get-started/_static/signalr-get-started-finished.png)

<span data-ttu-id="62a51-107">このチュートリアルでは、次の SignalR 開発タスクについて説明します。</span><span class="sxs-lookup"><span data-stu-id="62a51-107">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="62a51-108">ASP.NET Core web アプリで、SignalR を作成します。</span><span class="sxs-lookup"><span data-stu-id="62a51-108">Create a SignalR on ASP.NET Core web app.</span></span>
> * <span data-ttu-id="62a51-109">クライアントにコンテンツをプッシュする SignalR ハブを作成します。</span><span class="sxs-lookup"><span data-stu-id="62a51-109">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="62a51-110">変更、`Startup`クラスし、アプリを構成します。</span><span class="sxs-lookup"><span data-stu-id="62a51-110">Modify the `Startup` class and configure the app.</span></span>

<span data-ttu-id="62a51-111">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="62a51-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="prerequisites"></a><span data-ttu-id="62a51-112">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="62a51-112">Prerequisites</span></span>

<span data-ttu-id="62a51-113">次のソフトウェアをインストールします。</span><span class="sxs-lookup"><span data-stu-id="62a51-113">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="62a51-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="62a51-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="62a51-115">[.NET core 2.1.0 Preview 2 の SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview2)以降</span><span class="sxs-lookup"><span data-stu-id="62a51-115">[.NET Core 2.1.0 Preview 2 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview2) or later</span></span>
* <span data-ttu-id="62a51-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) 15.7 またはそれ以降のバージョン、 **ASP.NET および web 開発**ワークロード</span><span class="sxs-lookup"><span data-stu-id="62a51-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7 or later with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="62a51-117">npm</span><span class="sxs-lookup"><span data-stu-id="62a51-117">npm</span></span>](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="62a51-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="62a51-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="62a51-119">[.NET core 2.1.0 Preview 2 の SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview2)以降</span><span class="sxs-lookup"><span data-stu-id="62a51-119">[.NET Core 2.1.0 Preview 2 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview2) or later</span></span>
* [<span data-ttu-id="62a51-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="62a51-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="62a51-121">Visual Studio のコードの C#</span><span class="sxs-lookup"><span data-stu-id="62a51-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="62a51-122">npm</span><span class="sxs-lookup"><span data-stu-id="62a51-122">npm</span></span>](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="62a51-123">SignalR クライアントとサーバーをホストする ASP.NET Core プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="62a51-123">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="62a51-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="62a51-124">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="62a51-125">使用して、**ファイル** > **新しいプロジェクト**メニュー オプションを**ASP.NET Core Web アプリケーション**です。</span><span class="sxs-lookup"><span data-stu-id="62a51-125">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="62a51-126">プロジェクトに名前を*SignalRChat*です。</span><span class="sxs-lookup"><span data-stu-id="62a51-126">Name the project *SignalRChat*.</span></span>

   ![Visual Studio で新しいプロジェクト ダイアログ ボックス](get-started/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="62a51-128">選択**Web アプリケーション**Razor ページを使用してプロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="62a51-128">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="62a51-129">選択し、 **OK**です。</span><span class="sxs-lookup"><span data-stu-id="62a51-129">Then select **OK**.</span></span> <span data-ttu-id="62a51-130">必ず**ASP.NET Core 2.1** SignalR は、.NET の以前のバージョンが実行される場合は、framework セレクターから選択します。</span><span class="sxs-lookup"><span data-stu-id="62a51-130">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

   ![Visual Studio で新しいプロジェクト ダイアログ ボックス](get-started/_static/signalr-new-project-choose-type.png)

<span data-ttu-id="62a51-132">Visual Studio が含まれています、`Microsoft.AspNetCore.SignalR`パッケージの一部として、サーバーのライブラリを含むその**ASP.NET Core Web アプリケーション**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="62a51-132">Visual Studio includes the `Microsoft.AspNetCore.SignalR` package containing its server libraries as part of its **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="62a51-133">使用して SignalR の JavaScript クライアント ライブラリをインストールする必要がありますただし、 *npm*です。</span><span class="sxs-lookup"><span data-stu-id="62a51-133">However, the JavaScript client library for SignalR must be installed using *npm*.</span></span>

3. <span data-ttu-id="62a51-134">次のコマンドを実行、 **Package Manager Console**ウィンドウで、プロジェクトのルートから。</span><span class="sxs-lookup"><span data-stu-id="62a51-134">Run the following commands in the **Package Manager Console** window, from the project root:</span></span>

    ```console
      npm init -y
      npm install @aspnet/signalr
    ```     

4. <span data-ttu-id="62a51-135">コピー、 *signalr.js*ファイルから*node_modules\\ @aspnet\signalr\dist\browser* を*lib*プロジェクトのフォルダーにします。</span><span class="sxs-lookup"><span data-stu-id="62a51-135">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to the *lib* folder in your project.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="62a51-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="62a51-136">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="62a51-137">**統合ターミナル**、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="62a51-137">From the **Integrated Terminal**, run the following command:</span></span>

    ```console
      dotnet new razor -o SignalRChat
    ```

2. <span data-ttu-id="62a51-138">JavaScript クライアント ライブラリを使用して、インストール*npm*です。</span><span class="sxs-lookup"><span data-stu-id="62a51-138">Install the JavaScript client library using *npm*.</span></span>

    ```
      npm init -y
      npm install @aspnet/signalr
    ```

-----

## <a name="create-the-signalr-hub"></a><span data-ttu-id="62a51-139">SignalR ハブを作成します。</span><span class="sxs-lookup"><span data-stu-id="62a51-139">Create the SignalR Hub</span></span>

<span data-ttu-id="62a51-140">ハブは、クライアントとサーバーが互いにメソッドを呼び出すことができる高度なパイプラインとして機能するクラスです。</span><span class="sxs-lookup"><span data-stu-id="62a51-140">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="62a51-141">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="62a51-141">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="62a51-142">クラスを選択してプロジェクトに追加**ファイル** > **新規** > **ファイル**を選択して**Visual c# クラス**です。</span><span class="sxs-lookup"><span data-stu-id="62a51-142">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span>

2. <span data-ttu-id="62a51-143">継承`Microsoft.AspNetCore.SignalR.Hub`です。</span><span class="sxs-lookup"><span data-stu-id="62a51-143">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="62a51-144">`Hub`プロパティおよび接続とグループ、さらに送信側と受信側のデータを管理するためのイベント クラスが含まれています。</span><span class="sxs-lookup"><span data-stu-id="62a51-144">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

3. <span data-ttu-id="62a51-145">作成、`SendMessage`接続されているチャットのすべてのクライアントにメッセージを送信するメソッド。</span><span class="sxs-lookup"><span data-stu-id="62a51-145">Create the `SendMessage` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="62a51-146">返すことを確認、[タスク](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx)SignalR が非同期であるためです。</span><span class="sxs-lookup"><span data-stu-id="62a51-146">Notice it returns a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), because SignalR is asynchronous.</span></span> <span data-ttu-id="62a51-147">非同期コードの拡張性が優れています。</span><span class="sxs-lookup"><span data-stu-id="62a51-147">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="62a51-148">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="62a51-148">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="62a51-149">開く、 *SignalRChat* Visual Studio のコード内のフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="62a51-149">Open the *SignalRChat* folder in Visual Studio Code.</span></span>

2. <span data-ttu-id="62a51-150">選択して、プロジェクトにクラスを追加**ファイル** > **新しいファイル** メニューからです。</span><span class="sxs-lookup"><span data-stu-id="62a51-150">Add a class to the project by selecting **File** > **New File** from the menu.</span></span>

3. <span data-ttu-id="62a51-151">継承`Microsoft.AspNetCore.SignalR.Hub`です。</span><span class="sxs-lookup"><span data-stu-id="62a51-151">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="62a51-152">`Hub`プロパティおよび接続とグループだけでなくクライアントに送信側と受信側のデータを管理するためのイベント クラスが含まれています。</span><span class="sxs-lookup"><span data-stu-id="62a51-152">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data to clients.</span></span>

4. <span data-ttu-id="62a51-153">`SendMessage` メソッドをクラスに追加します。</span><span class="sxs-lookup"><span data-stu-id="62a51-153">Add a `SendMessage` method to the class.</span></span> <span data-ttu-id="62a51-154">`SendMessage`メソッドは、接続されているチャットのすべてのクライアントにメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="62a51-154">The `SendMessage` method sends a message to all connected chat clients.</span></span> <span data-ttu-id="62a51-155">返すことを確認、[タスク](/dotnet/api/system.threading.tasks.task)SignalR が非同期であるためです。</span><span class="sxs-lookup"><span data-stu-id="62a51-155">Notice it returns a [Task](/dotnet/api/system.threading.tasks.task), because SignalR is asynchronous.</span></span> <span data-ttu-id="62a51-156">非同期コードの拡張性が優れています。</span><span class="sxs-lookup"><span data-stu-id="62a51-156">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs?range=6-12)]

-----

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="62a51-157">SignalR を使用するプロジェクトを構成します。</span><span class="sxs-lookup"><span data-stu-id="62a51-157">Configure the project to use SignalR</span></span>

<span data-ttu-id="62a51-158">SignalR に要求を渡すを認識できるように、SignalR のサーバーを構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="62a51-158">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="62a51-159">SignalR プロジェクトを構成するには、変更、プロジェクトの`Startup.ConfigureServices`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="62a51-159">To configure a SignalR project, modify the project's `Startup.ConfigureServices` method.</span></span>

   <span data-ttu-id="62a51-160">`services.AddSignalR` 一部として SignalR を追加、[ミドルウェア](xref:fundamentals/middleware/index)パイプライン。</span><span class="sxs-lookup"><span data-stu-id="62a51-160">`services.AddSignalR` adds SignalR as part of the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

2. <span data-ttu-id="62a51-161">使用して、ハブへのルートを構成する`UseSignalR`です。</span><span class="sxs-lookup"><span data-stu-id="62a51-161">Configure routes to your hubs using `UseSignalR`.</span></span>


   [!code-csharp[Startup](get-started/sample/Startup.cs?highlight=36,56-59)]


## <a name="create-the-signalr-client-code"></a><span data-ttu-id="62a51-162">SignalR クライアント コードを作成します。</span><span class="sxs-lookup"><span data-stu-id="62a51-162">Create the SignalR client code</span></span>

1. <span data-ttu-id="62a51-163">コンテンツを置き換える*Pages\Index.cshtml*を次のコード。</span><span class="sxs-lookup"><span data-stu-id="62a51-163">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

   [!code-cshtml[Index](get-started/sample/Pages/Index.cshtml)]

   <span data-ttu-id="62a51-164">上記の HTML では、名前とメッセージ フィールド、および送信ボタンが表示されます。</span><span class="sxs-lookup"><span data-stu-id="62a51-164">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="62a51-165">下部にあるスクリプト参照に注意してください: SignalR への参照と*chat.js*です。</span><span class="sxs-lookup"><span data-stu-id="62a51-165">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

2. <span data-ttu-id="62a51-166">という名前の JavaScript ファイルを追加*chat.js*を*wwwroot\js*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="62a51-166">Add a JavaScript file, named *chat.js*, to the *wwwroot\js* folder.</span></span> <span data-ttu-id="62a51-167">これに次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="62a51-167">Add the following code to it:</span></span>

   [!code-javascript[Index](get-started/sample/wwwroot/js/chat.js)]

## <a name="run-the-app"></a><span data-ttu-id="62a51-168">アプリを実行する</span><span class="sxs-lookup"><span data-stu-id="62a51-168">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="62a51-169">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="62a51-169">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="62a51-170">選択**デバッグ** > **デバッグなしで開始**ブラウザーを起動して、ローカルの web サイトを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="62a51-170">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="62a51-171">アドレス バーから URL をコピーします。</span><span class="sxs-lookup"><span data-stu-id="62a51-171">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="62a51-172">別のブラウザー インスタンス (任意のブラウザー) を開き、アドレス バーに URL を貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="62a51-172">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="62a51-173">いずれかのブラウザーを選択し、名前と、メッセージを入力してをクリックして、**送信**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="62a51-173">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="62a51-174">名前とメッセージ」は即座に両方のページに表示されます。</span><span class="sxs-lookup"><span data-stu-id="62a51-174">The name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="62a51-175">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="62a51-175">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="62a51-176">**[デバッグ]** (F5) を押してプログラムをビルドし、実行します。</span><span class="sxs-lookup"><span data-stu-id="62a51-176">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="62a51-177">プログラムを実行すると、ブラウザー ウィンドウが開きます。</span><span class="sxs-lookup"><span data-stu-id="62a51-177">Running the program opens a browser window.</span></span>

1. <span data-ttu-id="62a51-178">別のブラウザー ウィンドウを開き、ローカルに web サイトをロードします。</span><span class="sxs-lookup"><span data-stu-id="62a51-178">Open another browser window and load the website locally in it.</span></span>

1. <span data-ttu-id="62a51-179">いずれかのブラウザーを選択し、名前と、メッセージを入力してをクリックして、**送信**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="62a51-179">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="62a51-180">名前とメッセージ」は即座に両方のページに表示されます。</span><span class="sxs-lookup"><span data-stu-id="62a51-180">The name and message are displayed on both pages instantly.</span></span>

-----

  ![ソリューション](get-started/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a><span data-ttu-id="62a51-182">関連資料</span><span class="sxs-lookup"><span data-stu-id="62a51-182">Related resources</span></span>

[<span data-ttu-id="62a51-183">ASP.NET Core SignalR の概要</span><span class="sxs-lookup"><span data-stu-id="62a51-183">Introduction to ASP.NET Core SignalR</span></span>](introduction.md)