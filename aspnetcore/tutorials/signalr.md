---
title: ASP.NET Core の SignalR 概要
author: tdykstra
description: このチュートリアルでは、ASP.NET Core の SignalR を使用してアプリを作成します。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/22/2018
uid: tutorials/signalr
ms.openlocfilehash: 83be28b30cf06eeea37e8d76b3f6444ffd9a10e8
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095492"
---
# <a name="get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="144c9-103">ASP.NET Core の SignalR 概要</span><span class="sxs-lookup"><span data-stu-id="144c9-103">Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="144c9-104">このチュートリアルでは、ASP.NET Core の SignalR を使用してリアルタイム アプリをビルドするための基本について説明します。</span><span class="sxs-lookup"><span data-stu-id="144c9-104">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![ソリューション](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="144c9-106">このチュートリアルでは、次の SignalR 開発タスクを行います。</span><span class="sxs-lookup"><span data-stu-id="144c9-106">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="144c9-107">ASP.NET Core Web アプリで SignalR を作成します。</span><span class="sxs-lookup"><span data-stu-id="144c9-107">Create a SignalR on ASP.NET Core web app.</span></span>
> * <span data-ttu-id="144c9-108">クライアントにコンテンツをプッシュする SignalR ハブを作成します。</span><span class="sxs-lookup"><span data-stu-id="144c9-108">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="144c9-109">`Startup` クラスを変更し、アプリを構成します。</span><span class="sxs-lookup"><span data-stu-id="144c9-109">Modify the `Startup` class and configure the app.</span></span>

<span data-ttu-id="144c9-110">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="144c9-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="144c9-111">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="144c9-111">Prerequisites</span></span>

<span data-ttu-id="144c9-112">以下のソフトウェアをインストールします。</span><span class="sxs-lookup"><span data-stu-id="144c9-112">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="144c9-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="144c9-113">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="144c9-114">.NET Core SDK 2.1 以降</span><span class="sxs-lookup"><span data-stu-id="144c9-114">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="144c9-115">**ASP.NET および Web 開発**ワークロードを含む [Visual Studio 2017](https://www.visualstudio.com/downloads/) バージョン 15.7.3 以降</span><span class="sxs-lookup"><span data-stu-id="144c9-115">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7.3 or later with the **ASP.NET and web development** workload</span></span>
* <span data-ttu-id="144c9-116">[npm](https://www.npmjs.com/get-npm) (Node.js のパッケージ マネージャー)</span><span class="sxs-lookup"><span data-stu-id="144c9-116">[npm](https://www.npmjs.com/get-npm) (package manager for Node.js)</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="144c9-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="144c9-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="144c9-118">.NET Core SDK 2.1 以降</span><span class="sxs-lookup"><span data-stu-id="144c9-118">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="144c9-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="144c9-119">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="144c9-120">Visual Studio Code 用 C#</span><span class="sxs-lookup"><span data-stu-id="144c9-120">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* <span data-ttu-id="144c9-121">[npm](https://www.npmjs.com/get-npm) (Node.js のパッケージ マネージャー)</span><span class="sxs-lookup"><span data-stu-id="144c9-121">[npm](https://www.npmjs.com/get-npm) (package manager for Node.js)</span></span>

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="144c9-122">SignalR クライアントとサーバーをホストする ASP.NET Core プロジェクトを作成する</span><span class="sxs-lookup"><span data-stu-id="144c9-122">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="144c9-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="144c9-123">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="144c9-124">**[ファイル]** > **[新しいプロジェクト]** メニュー オプション、**[ASP.NET Core Web アプリケーション]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="144c9-124">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="144c9-125">プロジェクトに "*SignalRChat*" という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="144c9-125">Name the project *SignalRChat*.</span></span>

   ![Visual Studio の [新しいプロジェクト] ダイアログ ボックス](signalr/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="144c9-127">**[Web アプリケーション]** を選択し、Razor Pages を使用してプロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="144c9-127">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="144c9-128">**[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="144c9-128">Then select **OK**.</span></span> <span data-ttu-id="144c9-129">SignalR は .NET. のより前のバージョンで動作しますが、フレームワークには **[ASP.NET Core 2.1]** を選択してください。</span><span class="sxs-lookup"><span data-stu-id="144c9-129">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

   ![Visual Studio の [新しいプロジェクト] ダイアログ ボックス](signalr/_static/signalr-new-project-choose-type.png)

<span data-ttu-id="144c9-131">Visual Studio には、その **ASP.NET Core Web アプリケーション** テンプレートの一部としてそのサーバー ライブラリを含む `Microsoft.AspNetCore.SignalR` パッケージが含まれています。</span><span class="sxs-lookup"><span data-stu-id="144c9-131">Visual Studio includes the `Microsoft.AspNetCore.SignalR` package containing its server libraries as part of its **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="144c9-132">ただし、SignalR の JavaScript クライアント ライブラリは *npm* を使用してインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="144c9-132">However, the JavaScript client library for SignalR must be installed using *npm*.</span></span>

3. <span data-ttu-id="144c9-133">**パッケージ マネージャー コンソール** ウィンドウでプロジェクト ルートから次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="144c9-133">Run the following commands in the **Package Manager Console** window, from the project root:</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

4. <span data-ttu-id="144c9-134">プロジェクトの *wwwroot/lib* フォルダー内に "signalr" という名前のフォルダーを新しく作成します。</span><span class="sxs-lookup"><span data-stu-id="144c9-134">Create a new folder named "signalr" inside the  *wwwroot/lib* folder in your project.</span></span> <span data-ttu-id="144c9-135">*node_modules\\@aspnet\signalr\dist\browser* からこのフォルダーに *signalr.js* ファイルをコピーします。</span><span class="sxs-lookup"><span data-stu-id="144c9-135">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to this folder.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="144c9-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="144c9-136">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="144c9-137">**統合端末**から次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="144c9-137">From the **Integrated Terminal**, run the following command:</span></span>

    ```console
    dotnet new webapp -o SignalRChat
    ```

    [!INCLUDE[](~/includes/webapp-alias-notice.md)]

2. <span data-ttu-id="144c9-138">*npm* を使用して JavaScript クライアント ライブラリをインストールします。</span><span class="sxs-lookup"><span data-stu-id="144c9-138">Install the JavaScript client library using *npm*.</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

3. <span data-ttu-id="144c9-139">プロジェクトの *lib* フォルダー内に "signalr" という名前のフォルダーを新しく作成します。</span><span class="sxs-lookup"><span data-stu-id="144c9-139">Create a new folder named "signalr" inside the  *lib* folder in your project.</span></span> <span data-ttu-id="144c9-140">*node_modules\\@aspnet\signalr\dist\browser* からこのフォルダーに *signalr.js* ファイルをコピーします。</span><span class="sxs-lookup"><span data-stu-id="144c9-140">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to this folder.</span></span>

---

## <a name="create-the-signalr-hub"></a><span data-ttu-id="144c9-141">SignalR ハブを作成する</span><span class="sxs-lookup"><span data-stu-id="144c9-141">Create the SignalR Hub</span></span>

<span data-ttu-id="144c9-142">ハブはハイレベル パイプラインとして機能するクラスであり、クライアントとサーバーが相互にメソッドを呼び出すことを可能にします。</span><span class="sxs-lookup"><span data-stu-id="144c9-142">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="144c9-143">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="144c9-143">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="144c9-144">**[ファイル]** > **[新規]** > **[ファイル]**、**[Visual C# クラス]** の順に選択し、プロジェクトにクラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="144c9-144">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span> <span data-ttu-id="144c9-145">クラスに `ChatHub`、ファイルに *ChatHub.cs* と名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="144c9-145">Name the class `ChatHub` and the file *ChatHub.cs*.</span></span>

2. <span data-ttu-id="144c9-146">`Microsoft.AspNetCore.SignalR.Hub` から継承します。</span><span class="sxs-lookup"><span data-stu-id="144c9-146">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="144c9-147">`Hub` クラスには、接続とグループを管理し、データを送受信するためのプロパティとイベントが含まれています。</span><span class="sxs-lookup"><span data-stu-id="144c9-147">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

3. <span data-ttu-id="144c9-148">接続されているすべてのチャット クライアントにメッセージを送信する `SendMessage` メソッドを作成します。</span><span class="sxs-lookup"><span data-stu-id="144c9-148">Create the `SendMessage` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="144c9-149">SignalR は非同期であるため、[Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx) が返されます。</span><span class="sxs-lookup"><span data-stu-id="144c9-149">Notice it returns a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), because SignalR is asynchronous.</span></span> <span data-ttu-id="144c9-150">非同期コードは拡張性に優れています。</span><span class="sxs-lookup"><span data-stu-id="144c9-150">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="144c9-151">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="144c9-151">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="144c9-152">Visual Studio Code で *SignalRChat* フォルダーを開きます。</span><span class="sxs-lookup"><span data-stu-id="144c9-152">Open the *SignalRChat* folder in Visual Studio Code.</span></span>

2. <span data-ttu-id="144c9-153">メニューから **[ファイル]** > **[新しいファイル]** の順に選択し、プロジェクトにクラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="144c9-153">Add a class to the project by selecting **File** > **New File** from the menu.</span></span> <span data-ttu-id="144c9-154">クラスに `ChatHub`、ファイルに *ChatHub.cs* と名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="144c9-154">Name the class `ChatHub` and the file *ChatHub.cs*.</span></span>

3. <span data-ttu-id="144c9-155">`Microsoft.AspNetCore.SignalR.Hub` から継承します。</span><span class="sxs-lookup"><span data-stu-id="144c9-155">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="144c9-156">`Hub` クラスには、接続とグループを管理し、クライアントとの間でデータを送受信するためのプロパティとイベントが含まれています。</span><span class="sxs-lookup"><span data-stu-id="144c9-156">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data to clients.</span></span>

4. <span data-ttu-id="144c9-157">`SendMessage` メソッドをクラスに追加します。</span><span class="sxs-lookup"><span data-stu-id="144c9-157">Add a `SendMessage` method to the class.</span></span> <span data-ttu-id="144c9-158">`SendMessage` メソッドは、接続されているすべてのチャット クライアントにメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="144c9-158">The `SendMessage` method sends a message to all connected chat clients.</span></span> <span data-ttu-id="144c9-159">SignalR は非同期であるため、[Task](/dotnet/api/system.threading.tasks.task) が返されます。</span><span class="sxs-lookup"><span data-stu-id="144c9-159">Notice it returns a [Task](/dotnet/api/system.threading.tasks.task), because SignalR is asynchronous.</span></span> <span data-ttu-id="144c9-160">非同期コードは拡張性に優れています。</span><span class="sxs-lookup"><span data-stu-id="144c9-160">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

-----

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="144c9-161">SignalR を使用するようにプロジェクトを構成する</span><span class="sxs-lookup"><span data-stu-id="144c9-161">Configure the project to use SignalR</span></span>

<span data-ttu-id="144c9-162">SignalR に要求を渡すように SignalR のサーバーを構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="144c9-162">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="144c9-163">SignalR プロジェクトを構成するには、プロジェクトの `Startup.ConfigureServices` メソッドを変更します。</span><span class="sxs-lookup"><span data-stu-id="144c9-163">To configure a SignalR project, modify the project's `Startup.ConfigureServices` method.</span></span>

   <span data-ttu-id="144c9-164">`services.AddSignalR` は、[ 依存関係の挿入](xref:fundamentals/dependency-injection)システムで SignalR サービスを使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="144c9-164">`services.AddSignalR` makes the SignalR services available to the [dependency injection](xref:fundamentals/dependency-injection) system.</span></span>

1. <span data-ttu-id="144c9-165">`Configure` メソッドで `UseSignalR` を使用して、ハブへのルートを構成します。</span><span class="sxs-lookup"><span data-stu-id="144c9-165">Configure routes to your hubs with `UseSignalR` in the `Configure` method.</span></span> <span data-ttu-id="144c9-166">`app.UseSignalR` は、[ミドルウェア](xref:fundamentals/middleware/index) パイプラインに SignalR を追加します。</span><span class="sxs-lookup"><span data-stu-id="144c9-166">`app.UseSignalR` adds SignalR to the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

   [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=37,57-60)]

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="144c9-167">SignalR クライアント コードを作成する</span><span class="sxs-lookup"><span data-stu-id="144c9-167">Create the SignalR client code</span></span>

1. <span data-ttu-id="144c9-168">"*chat.js*" という名前の JavaScript ファイルを *wwwroot\js* フォルダーに追加します。</span><span class="sxs-lookup"><span data-stu-id="144c9-168">Add a JavaScript file, named *chat.js*, to the *wwwroot\js* folder.</span></span> <span data-ttu-id="144c9-169">これに次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="144c9-169">Add the following code to it:</span></span>

   [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

2. <span data-ttu-id="144c9-170">*Pages\Index.cshtml* のコンテンツを次のコードに変更します。</span><span class="sxs-lookup"><span data-stu-id="144c9-170">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

   [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

   <span data-ttu-id="144c9-171">上記の HTML では、名前フィールド、メッセージ フィールド、送信ボタンが表示されます。</span><span class="sxs-lookup"><span data-stu-id="144c9-171">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="144c9-172">一番下のスクリプト参照をご覧ください。SignalR と *chat.js* が参照されています。</span><span class="sxs-lookup"><span data-stu-id="144c9-172">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="144c9-173">アプリを実行する</span><span class="sxs-lookup"><span data-stu-id="144c9-173">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="144c9-174">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="144c9-174">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="144c9-175">**[デバッグ]** > **[デバッグなしで開始]** の順に選択し、ブラウザーを起動し、Web サイトをローカルで読み込みます。</span><span class="sxs-lookup"><span data-stu-id="144c9-175">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="144c9-176">アドレス バーから URL をコピーします。</span><span class="sxs-lookup"><span data-stu-id="144c9-176">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="144c9-177">別のブラウザー インスタンスを開き (どのブラウザーでもかまいません)、アドレス バーに URL を貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="144c9-177">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="144c9-178">いずれかのブラウザーを選択し、名前とメッセージを入力し、**[送信]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="144c9-178">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="144c9-179">次の瞬間、両方のページに名前とメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="144c9-179">The name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="144c9-180">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="144c9-180">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="144c9-181">**[デバッグ]** (F5) を押してプログラムをビルドし、実行します。</span><span class="sxs-lookup"><span data-stu-id="144c9-181">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="144c9-182">プログラムを実行すると、ブラウザー ウィンドウが開きます。</span><span class="sxs-lookup"><span data-stu-id="144c9-182">Running the program opens a browser window.</span></span>

1. <span data-ttu-id="144c9-183">別のブラウザー ウィンドウを開き、ローカルで Web サイトを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="144c9-183">Open another browser window and load the website locally in it.</span></span>

1. <span data-ttu-id="144c9-184">いずれかのブラウザーを選択し、名前とメッセージを入力し、**[送信]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="144c9-184">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="144c9-185">次の瞬間、両方のページに名前とメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="144c9-185">The name and message are displayed on both pages instantly.</span></span>

---

  ![ソリューション](signalr/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a><span data-ttu-id="144c9-187">関連資料</span><span class="sxs-lookup"><span data-stu-id="144c9-187">Related resources</span></span>

[<span data-ttu-id="144c9-188">ASP.NET Core SignalR の概要</span><span class="sxs-lookup"><span data-stu-id="144c9-188">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
