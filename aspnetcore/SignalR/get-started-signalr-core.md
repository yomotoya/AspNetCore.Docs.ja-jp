---
uid: signalr/get-started-signalr-core
title: "ASP.NET Core の SignalR を概要します。"
author: rachelappel
ms.author: rachelap
description: "このチュートリアルでは、ASP.NET Core の SignalR を使用してアプリを作成します。"
manager: wpickett
ms.date: 03/06/2018
ms.topic: tutorial
ms.technology: dotnet-signalr
ms.prod: aspnet-core
ms.custom: mvc
ms.openlocfilehash: 5e16569aa492e3639aa97abd241610b361fb0c56
ms.sourcegitcommit: 53ee14b9c8200f44705d8997c3619fa874192d45
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2018
---
# <a name="tutorial-get-started-with-signalr-for-aspnet-core"></a><span data-ttu-id="fb972-103">チュートリアル: SignalR による ASP.NET Core の開始します。</span><span class="sxs-lookup"><span data-stu-id="fb972-103">Tutorial: Get started with SignalR for ASP.NET Core</span></span>

<span data-ttu-id="fb972-104">作成者: [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="fb972-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="fb972-105">このチュートリアルでは ASP.NET Core の SignalR を使用してリアルタイム アプリの構築の基礎を説明します。</span><span class="sxs-lookup"><span data-stu-id="fb972-105">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![ソリューション](get-started-signalr-core/_static/signalr-get-started-finished.png)

<span data-ttu-id="fb972-107">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started-signalr-core/sample/)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="fb972-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started-signalr-core/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="fb972-108">このチュートリアルでは、次の SignalR 開発タスクについて説明します。</span><span class="sxs-lookup"><span data-stu-id="fb972-108">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="fb972-109">ASP.NET Core web アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="fb972-109">Create an ASP.NET Core web app.</span></span>
> * <span data-ttu-id="fb972-110">クライアントにコンテンツをプッシュする SignalR ハブを作成します。</span><span class="sxs-lookup"><span data-stu-id="fb972-110">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="fb972-111">SignalR JavaScript ライブラリを使用すると、メッセージを送信してハブからの更新プログラムを表示します。</span><span class="sxs-lookup"><span data-stu-id="fb972-111">Use the SignalR JavaScript library to send messages and display updates from the hub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fb972-112">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="fb972-112">Prerequisites</span></span>

<span data-ttu-id="fb972-113">次のソフトウェアをインストールします。</span><span class="sxs-lookup"><span data-stu-id="fb972-113">Install the following software:</span></span>

* <span data-ttu-id="fb972-114">[.NET core 2.1.0 Preview 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1)以降</span><span class="sxs-lookup"><span data-stu-id="fb972-114">[.NET Core 2.1.0 Preview 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) or later</span></span>
* <span data-ttu-id="fb972-115">[Visual Studio 2017](https://www.visualstudio.com/downloads/) 15.6 以降、ASP.NET と web 開発のワークロードでのバージョン</span><span class="sxs-lookup"><span data-stu-id="fb972-115">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.6 or later with the ASP.NET and web development workload</span></span>
* [<span data-ttu-id="fb972-116">npm</span><span class="sxs-lookup"><span data-stu-id="fb972-116">npm</span></span>](https://www.npmjs.com/get-npm)

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="fb972-117">SignalR クライアントとサーバーをホストする ASP.NET Core プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="fb972-117">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

1. <span data-ttu-id="fb972-118">使用して、**ファイル** > **新しいプロジェクト**メニュー オプションを**ASP.NET Core Web アプリケーション**です。</span><span class="sxs-lookup"><span data-stu-id="fb972-118">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="fb972-119">プロジェクトに `SignalRChat` という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="fb972-119">Name the project `SignalRChat`.</span></span>

  ![Visual Studio で新しいプロジェクト ダイアログ ボックス](get-started-signalr-core/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="fb972-121">選択**Web アプリケーション**Razor ページを使用してプロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="fb972-121">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="fb972-122">選択し、 **Ok**です。</span><span class="sxs-lookup"><span data-stu-id="fb972-122">Then select **Ok**.</span></span> <span data-ttu-id="fb972-123">必ず**ASP.NET Core 2.1** SignalR は、.NET の以前のバージョンが実行される場合は、framework セレクターから選択します。</span><span class="sxs-lookup"><span data-stu-id="fb972-123">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

  ![Visual Studio で新しいプロジェクト ダイアログ ボックス](get-started-signalr-core/_static/signalr-new-project-choose-type.png)

  <span data-ttu-id="fb972-125">プロジェクト テンプレートでは、SignalR のサーバー側コードをホストしているライブラリが含まれています。</span><span class="sxs-lookup"><span data-stu-id="fb972-125">The libraries that host SignalR server-side code are included in the project template.</span></span> <span data-ttu-id="fb972-126">インストールを別々 にクライアント側 JavaScript [npm](https://www.npmjs.com/)です。</span><span class="sxs-lookup"><span data-stu-id="fb972-126">Install the client-side JavaScript separately with [npm](https://www.npmjs.com/).</span></span>

  ```console
   npm install @aspnet/signalr
  ```

3. <span data-ttu-id="fb972-127">コピー、 *signalr.js*から*node_modules\\ @aspnet\signalr\dist\browser* を*wwwroot\lib*プロジェクトのフォルダーにします。</span><span class="sxs-lookup"><span data-stu-id="fb972-127">Copy the *signalr.js* from *node_modules\\@aspnet\signalr\dist\browser* to the *wwwroot\lib* folder in your project.</span></span>

## <a name="create-the-signalr-hub"></a><span data-ttu-id="fb972-128">SignalR ハブを作成します。</span><span class="sxs-lookup"><span data-stu-id="fb972-128">Create the SignalR Hub</span></span>

<span data-ttu-id="fb972-129">ハブは、クライアントとサーバーが互いにメソッドを呼び出すことができる高度なパイプラインとして機能するクラスです。</span><span class="sxs-lookup"><span data-stu-id="fb972-129">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

1. <span data-ttu-id="fb972-130">クラスを選択してプロジェクトに追加**ファイル** > **新規** > **ファイル**を選択して**Visual c# クラス**です。</span><span class="sxs-lookup"><span data-stu-id="fb972-130">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span> 

1. <span data-ttu-id="fb972-131">継承`Microsoft.AspNetCore.SignalR.Hub`です。</span><span class="sxs-lookup"><span data-stu-id="fb972-131">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="fb972-132">`Hub`プロパティおよび接続とグループ、さらに送信側と受信側のデータを管理するためのイベント クラスが含まれています。</span><span class="sxs-lookup"><span data-stu-id="fb972-132">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

1. <span data-ttu-id="fb972-133">作成、`Send`接続されているチャットのすべてのクライアントにメッセージを送信するメソッド。</span><span class="sxs-lookup"><span data-stu-id="fb972-133">Create the `Send` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="fb972-134">返すことを確認、 `Task`SignalR が非同期であるためです。</span><span class="sxs-lookup"><span data-stu-id="fb972-134">Notice it returns a `Task`, because SignalR is asynchronous.</span></span> <span data-ttu-id="fb972-135">非同期コードの拡張性が優れています。</span><span class="sxs-lookup"><span data-stu-id="fb972-135">Asynchronous code scales better.</span></span>

  [!code-csharp[Startup](get-started-signalr-core/sample/Hubs/ChatHub.cs?range=7-14)]

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="fb972-136">SignalR を使用するプロジェクトを構成します。</span><span class="sxs-lookup"><span data-stu-id="fb972-136">Configure the project to use SignalR</span></span>

<span data-ttu-id="fb972-137">SignalR に要求を渡すを認識できるように、SignalR のサーバーを構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="fb972-137">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="fb972-138">SignalR プロジェクトを構成するのには、変更、`ConfigureServices`メソッドは、アプリケーションの`Startup`クラスへの呼び出しを挿入することで`services.AddSignalR`です。</span><span class="sxs-lookup"><span data-stu-id="fb972-138">To configure a SignalR project, modify the `ConfigureServices` method of the application's `Startup` class by inserting a call to `services.AddSignalR`.</span></span>

  <span data-ttu-id="fb972-139">`services.AddSignalR` 一部として SignalR を追加、 [ASP.NET Core ミドルウェア](xref:fundamentals/middleware/index)パイプライン。</span><span class="sxs-lookup"><span data-stu-id="fb972-139">`services.AddSignalR` adds SignalR as part of the [ASP.NET Core middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

1. <span data-ttu-id="fb972-140">使用して、ハブへのルートを構成する`UseSignalR`です。</span><span class="sxs-lookup"><span data-stu-id="fb972-140">Configure routes to your hubs using `UseSignalR`.</span></span>

  [!code-csharp[Startup](get-started-signalr-core/sample/Startup.cs?highlight=22,40-43)]

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="fb972-141">SignalR クライアント コードを作成します。</span><span class="sxs-lookup"><span data-stu-id="fb972-141">Create the SignalR client code</span></span>

1. <span data-ttu-id="fb972-142">コンテンツを置き換える*Pages\Index.cshtml*を次のコード。</span><span class="sxs-lookup"><span data-stu-id="fb972-142">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](get-started-signalr-core/sample/Pages/Index.cshtml)]

  <span data-ttu-id="fb972-143">上記の HTML では、名前とメッセージ フィールド、および送信ボタンが表示されます。</span><span class="sxs-lookup"><span data-stu-id="fb972-143">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="fb972-144">下部にあるスクリプト参照に注意してください: SignalR への参照と*chat.js*です。</span><span class="sxs-lookup"><span data-stu-id="fb972-144">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

1. <span data-ttu-id="fb972-145">JavaScript ファイルを追加、 *wwwroot\js*という名前のフォルダー *chat.js*を次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="fb972-145">Add a JavaScript file to the *wwwroot\js* folder named *chat.js* and add the following code to it:</span></span>

  [!code-javascript[Index](get-started-signalr-core/sample/wwwroot/js/chat.js)]

## <a name="run-the-app"></a><span data-ttu-id="fb972-146">アプリを実行する</span><span class="sxs-lookup"><span data-stu-id="fb972-146">Run the app</span></span>

1. <span data-ttu-id="fb972-147">選択**デバッグ** > **デバッグなしで開始**ブラウザーを起動して、ローカルの web サイトを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="fb972-147">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="fb972-148">アドレス バーから URL をコピーします。</span><span class="sxs-lookup"><span data-stu-id="fb972-148">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="fb972-149">別のブラウザー インスタンス (任意のブラウザー) を開き、アドレス バーに URL を貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="fb972-149">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="fb972-150">いずれかのブラウザーを選択し、名前と、メッセージを入力してをクリックして、**送信**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="fb972-150">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="fb972-151">名前とメッセージ」は即座に両方のページに表示されます。</span><span class="sxs-lookup"><span data-stu-id="fb972-151">The name and message are displayed on both pages instantly.</span></span>

  ![ソリューション](get-started-signalr-core/_static/signalr-get-started-finished.png)
