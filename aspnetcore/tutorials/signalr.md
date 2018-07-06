---
title: ASP.NET Core の SignalR 概要
author: rachelappel
description: このチュートリアルでは、ASP.NET Core の SignalR を使用してアプリを作成します。
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/22/2018
uid: tutorials/signalr
ms.openlocfilehash: 62cef2d6f032caa2f048cfdd49a225d975dad10d
ms.sourcegitcommit: 7003d27b607e529642ded0400aa48ae692a0e666
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/27/2018
ms.locfileid: "37033343"
---
# <a name="get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="26588-103">ASP.NET Core の SignalR 概要</span><span class="sxs-lookup"><span data-stu-id="26588-103">Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="26588-104">作成者: [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="26588-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="26588-105">このチュートリアルでは、ASP.NET Core の SignalR を使用してリアルタイム アプリをビルドするための基本について説明します。</span><span class="sxs-lookup"><span data-stu-id="26588-105">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![ソリューション](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="26588-107">このチュートリアルでは、次の SignalR 開発タスクを行います。</span><span class="sxs-lookup"><span data-stu-id="26588-107">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="26588-108">ASP.NET Core Web アプリで SignalR を作成します。</span><span class="sxs-lookup"><span data-stu-id="26588-108">Create a SignalR on ASP.NET Core web app.</span></span>
> * <span data-ttu-id="26588-109">クライアントにコンテンツをプッシュする SignalR ハブを作成します。</span><span class="sxs-lookup"><span data-stu-id="26588-109">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="26588-110">`Startup` クラスを変更し、アプリを構成します。</span><span class="sxs-lookup"><span data-stu-id="26588-110">Modify the `Startup` class and configure the app.</span></span>

<span data-ttu-id="26588-111">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="26588-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="26588-112">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="26588-112">Prerequisites</span></span>

<span data-ttu-id="26588-113">以下のソフトウェアをインストールします。</span><span class="sxs-lookup"><span data-stu-id="26588-113">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="26588-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="26588-114">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="26588-115">.NET Core SDK 2.1 以降</span><span class="sxs-lookup"><span data-stu-id="26588-115">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="26588-116">**ASP.NET および Web 開発**ワークロードを含む [Visual Studio 2017](https://www.visualstudio.com/downloads/) バージョン 15.7 以降</span><span class="sxs-lookup"><span data-stu-id="26588-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7 or later with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="26588-117">npm</span><span class="sxs-lookup"><span data-stu-id="26588-117">npm</span></span>](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="26588-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="26588-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="26588-119">.NET Core SDK 2.1 以降</span><span class="sxs-lookup"><span data-stu-id="26588-119">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="26588-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="26588-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="26588-121">Visual Studio Code 用 C#</span><span class="sxs-lookup"><span data-stu-id="26588-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="26588-122">npm</span><span class="sxs-lookup"><span data-stu-id="26588-122">npm</span></span>](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="26588-123">SignalR クライアントとサーバーをホストする ASP.NET Core プロジェクトを作成する</span><span class="sxs-lookup"><span data-stu-id="26588-123">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="26588-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="26588-124">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="26588-125">**[ファイル]** > **[新しいプロジェクト]** メニュー オプション、**[ASP.NET Core Web アプリケーション]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="26588-125">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="26588-126">プロジェクトに "*SignalRChat*" という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="26588-126">Name the project *SignalRChat*.</span></span>

   ![Visual Studio の [新しいプロジェクト] ダイアログ ボックス](signalr/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="26588-128">**[Web アプリケーション]** を選択し、Razor Pages を使用してプロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="26588-128">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="26588-129">**[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="26588-129">Then select **OK**.</span></span> <span data-ttu-id="26588-130">SignalR は .NET. のより前のバージョンで動作しますが、フレームワークには **[ASP.NET Core 2.1]** を選択してください。</span><span class="sxs-lookup"><span data-stu-id="26588-130">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

   ![Visual Studio の [新しいプロジェクト] ダイアログ ボックス](signalr/_static/signalr-new-project-choose-type.png)

<span data-ttu-id="26588-132">Visual Studio には、その **ASP.NET Core Web アプリケーション** テンプレートの一部としてそのサーバー ライブラリを含む `Microsoft.AspNetCore.SignalR` パッケージが含まれています。</span><span class="sxs-lookup"><span data-stu-id="26588-132">Visual Studio includes the `Microsoft.AspNetCore.SignalR` package containing its server libraries as part of its **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="26588-133">ただし、SignalR の JavaScript クライアント ライブラリは *npm* を使用してインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="26588-133">However, the JavaScript client library for SignalR must be installed using *npm*.</span></span>

3. <span data-ttu-id="26588-134">**パッケージ マネージャー コンソール** ウィンドウでプロジェクト ルートから次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="26588-134">Run the following commands in the **Package Manager Console** window, from the project root:</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

4. <span data-ttu-id="26588-135">プロジェクトの *lib* フォルダー内に "signalr" という名前のフォルダーを新しく作成します。</span><span class="sxs-lookup"><span data-stu-id="26588-135">Create a new folder named "signalr" inside the  *lib* folder in your project.</span></span> <span data-ttu-id="26588-136">*node_modules\\@aspnet\signalr\dist\browser* からこのフォルダーに *signalr.js* ファイルをコピーします。</span><span class="sxs-lookup"><span data-stu-id="26588-136">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to this folder.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="26588-137">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="26588-137">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="26588-138">**統合端末**から次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="26588-138">From the **Integrated Terminal**, run the following command:</span></span>

    ```console
    dotnet new webapp -o SignalRChat
    ```

    [!INCLUDE[](~/includes/webapp-alias-notice.md)]

2. <span data-ttu-id="26588-139">*npm* を使用して JavaScript クライアント ライブラリをインストールします。</span><span class="sxs-lookup"><span data-stu-id="26588-139">Install the JavaScript client library using *npm*.</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

3. <span data-ttu-id="26588-140">プロジェクトの *lib* フォルダー内に "signalr" という名前のフォルダーを新しく作成します。</span><span class="sxs-lookup"><span data-stu-id="26588-140">Create a new folder named "signalr" inside the  *lib* folder in your project.</span></span> <span data-ttu-id="26588-141">*node_modules\\@aspnet\signalr\dist\browser* からこのフォルダーに *signalr.js* ファイルをコピーします。</span><span class="sxs-lookup"><span data-stu-id="26588-141">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to this folder.</span></span>

---

## <a name="create-the-signalr-hub"></a><span data-ttu-id="26588-142">SignalR ハブを作成する</span><span class="sxs-lookup"><span data-stu-id="26588-142">Create the SignalR Hub</span></span>

<span data-ttu-id="26588-143">ハブはハイレベル パイプラインとして機能するクラスであり、クライアントとサーバーが相互にメソッドを呼び出すことを可能にします。</span><span class="sxs-lookup"><span data-stu-id="26588-143">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="26588-144">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="26588-144">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="26588-145">**[ファイル]** > **[新規]** > **[ファイル]**、**[Visual C# クラス]** の順に選択し、プロジェクトにクラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="26588-145">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span> <span data-ttu-id="26588-146">クラスに `ChatHub`、ファイルに *ChatHub.cs* と名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="26588-146">Name the class `ChatHub` and the file *ChatHub.cs*.</span></span>

2. <span data-ttu-id="26588-147">`Microsoft.AspNetCore.SignalR.Hub` から継承します。</span><span class="sxs-lookup"><span data-stu-id="26588-147">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="26588-148">`Hub` クラスには、接続とグループを管理し、データを送受信するためのプロパティとイベントが含まれています。</span><span class="sxs-lookup"><span data-stu-id="26588-148">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

3. <span data-ttu-id="26588-149">接続されているすべてのチャット クライアントにメッセージを送信する `SendMessage` メソッドを作成します。</span><span class="sxs-lookup"><span data-stu-id="26588-149">Create the `SendMessage` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="26588-150">SignalR は非同期であるため、[Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx) が返されます。</span><span class="sxs-lookup"><span data-stu-id="26588-150">Notice it returns a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), because SignalR is asynchronous.</span></span> <span data-ttu-id="26588-151">非同期コードは拡張性に優れています。</span><span class="sxs-lookup"><span data-stu-id="26588-151">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="26588-152">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="26588-152">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="26588-153">Visual Studio Code で *SignalRChat* フォルダーを開きます。</span><span class="sxs-lookup"><span data-stu-id="26588-153">Open the *SignalRChat* folder in Visual Studio Code.</span></span>

2. <span data-ttu-id="26588-154">メニューから **[ファイル]** > **[新しいファイル]** の順に選択し、プロジェクトにクラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="26588-154">Add a class to the project by selecting **File** > **New File** from the menu.</span></span> <span data-ttu-id="26588-155">クラスに `ChatHub`、ファイルに *ChatHub.cs* と名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="26588-155">Name the class `ChatHub` and the file *ChatHub.cs*.</span></span>

3. <span data-ttu-id="26588-156">`Microsoft.AspNetCore.SignalR.Hub` から継承します。</span><span class="sxs-lookup"><span data-stu-id="26588-156">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="26588-157">`Hub` クラスには、接続とグループを管理し、クライアントとの間でデータを送受信するためのプロパティとイベントが含まれています。</span><span class="sxs-lookup"><span data-stu-id="26588-157">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data to clients.</span></span>

4. <span data-ttu-id="26588-158">`SendMessage` メソッドをクラスに追加します。</span><span class="sxs-lookup"><span data-stu-id="26588-158">Add a `SendMessage` method to the class.</span></span> <span data-ttu-id="26588-159">`SendMessage` メソッドは、接続されているすべてのチャット クライアントにメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="26588-159">The `SendMessage` method sends a message to all connected chat clients.</span></span> <span data-ttu-id="26588-160">SignalR は非同期であるため、[Task](/dotnet/api/system.threading.tasks.task) が返されます。</span><span class="sxs-lookup"><span data-stu-id="26588-160">Notice it returns a [Task](/dotnet/api/system.threading.tasks.task), because SignalR is asynchronous.</span></span> <span data-ttu-id="26588-161">非同期コードは拡張性に優れています。</span><span class="sxs-lookup"><span data-stu-id="26588-161">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

-----

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="26588-162">SignalR を使用するようにプロジェクトを構成する</span><span class="sxs-lookup"><span data-stu-id="26588-162">Configure the project to use SignalR</span></span>

<span data-ttu-id="26588-163">SignalR に要求を渡すように SignalR のサーバーを構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="26588-163">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="26588-164">SignalR プロジェクトを構成するには、プロジェクトの `Startup.ConfigureServices` メソッドを変更します。</span><span class="sxs-lookup"><span data-stu-id="26588-164">To configure a SignalR project, modify the project's `Startup.ConfigureServices` method.</span></span>

   <span data-ttu-id="26588-165">`services.AddSignalR` は[ミドルウェア](xref:fundamentals/middleware/index) パイプラインの一部として SignalR を追加します。</span><span class="sxs-lookup"><span data-stu-id="26588-165">`services.AddSignalR` adds SignalR as part of the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

2. <span data-ttu-id="26588-166">`UseSignalR` を使用し、ハブへのルートを構成します。</span><span class="sxs-lookup"><span data-stu-id="26588-166">Configure routes to your hubs using `UseSignalR`.</span></span>

   [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=37,57-60)]

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="26588-167">SignalR クライアント コードを作成する</span><span class="sxs-lookup"><span data-stu-id="26588-167">Create the SignalR client code</span></span>

1. <span data-ttu-id="26588-168">"*chat.js*" という名前の JavaScript ファイルを *wwwroot\js* フォルダーに追加します。</span><span class="sxs-lookup"><span data-stu-id="26588-168">Add a JavaScript file, named *chat.js*, to the *wwwroot\js* folder.</span></span> <span data-ttu-id="26588-169">これに次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="26588-169">Add the following code to it:</span></span>

   [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

2. <span data-ttu-id="26588-170">*Pages\Index.cshtml* のコンテンツを次のコードに変更します。</span><span class="sxs-lookup"><span data-stu-id="26588-170">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

   [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

   <span data-ttu-id="26588-171">上記の HTML では、名前フィールド、メッセージ フィールド、送信ボタンが表示されます。</span><span class="sxs-lookup"><span data-stu-id="26588-171">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="26588-172">一番下のスクリプト参照をご覧ください。SignalR と *chat.js* が参照されています。</span><span class="sxs-lookup"><span data-stu-id="26588-172">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="26588-173">アプリを実行する</span><span class="sxs-lookup"><span data-stu-id="26588-173">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="26588-174">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="26588-174">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="26588-175">**[デバッグ]** > **[デバッグなしで開始]** の順に選択し、ブラウザーを起動し、Web サイトをローカルで読み込みます。</span><span class="sxs-lookup"><span data-stu-id="26588-175">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="26588-176">アドレス バーから URL をコピーします。</span><span class="sxs-lookup"><span data-stu-id="26588-176">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="26588-177">別のブラウザー インスタンスを開き (どのブラウザーでもかまいません)、アドレス バーに URL を貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="26588-177">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="26588-178">いずれかのブラウザーを選択し、名前とメッセージを入力し、**[送信]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="26588-178">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="26588-179">次の瞬間、両方のページに名前とメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="26588-179">The name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="26588-180">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="26588-180">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="26588-181">**[デバッグ]** (F5) を押してプログラムをビルドし、実行します。</span><span class="sxs-lookup"><span data-stu-id="26588-181">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="26588-182">プログラムを実行すると、ブラウザー ウィンドウが開きます。</span><span class="sxs-lookup"><span data-stu-id="26588-182">Running the program opens a browser window.</span></span>

1. <span data-ttu-id="26588-183">別のブラウザー ウィンドウを開き、ローカルで Web サイトを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="26588-183">Open another browser window and load the website locally in it.</span></span>

1. <span data-ttu-id="26588-184">いずれかのブラウザーを選択し、名前とメッセージを入力し、**[送信]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="26588-184">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="26588-185">次の瞬間、両方のページに名前とメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="26588-185">The name and message are displayed on both pages instantly.</span></span>

---

  ![ソリューション](signalr/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a><span data-ttu-id="26588-187">関連資料</span><span class="sxs-lookup"><span data-stu-id="26588-187">Related resources</span></span>

[<span data-ttu-id="26588-188">ASP.NET Core SignalR の概要</span><span class="sxs-lookup"><span data-stu-id="26588-188">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
