---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: 'チュートリアル: SignalR 2 とリアルタイムのチャット |Microsoft Docs'
author: bradygaster
description: このチュートリアルでは、SignalR を使用してリアルタイムのチャット アプリケーションを作成する方法を示します。 SignalR は、空の ASP.NET web アプリケーションに追加します。
ms.author: bradyg
ms.date: 01/22/2019
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: 90f2c03fbda522e3a46200bc0132cc74100ce70f
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836793"
---
# <a name="tutorial-real-time-chat-with-signalr-2"></a><span data-ttu-id="8e098-104">チュートリアル: SignalR 2 とリアルタイムのチャット</span><span class="sxs-lookup"><span data-stu-id="8e098-104">Tutorial: Real-time chat with SignalR 2</span></span>

<span data-ttu-id="8e098-105">このチュートリアルでは、SignalR を使用して、リアルタイムのチャット アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="8e098-105">This tutorial shows you how to use SignalR to create a real-time chat application.</span></span> <span data-ttu-id="8e098-106">SignalR を空の ASP.NET web アプリケーションに追加し、送信し、メッセージを表示する HTML ページを作成します。</span><span class="sxs-lookup"><span data-stu-id="8e098-106">You add SignalR to an empty ASP.NET web application and create an HTML page to send and display messages.</span></span>

<span data-ttu-id="8e098-107">このチュートリアルでは、次の作業を行いました。</span><span class="sxs-lookup"><span data-stu-id="8e098-107">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8e098-108">プロジェクトを設定します。</span><span class="sxs-lookup"><span data-stu-id="8e098-108">Set up the project</span></span>
> * <span data-ttu-id="8e098-109">サンプルを実行します。</span><span class="sxs-lookup"><span data-stu-id="8e098-109">Run the sample</span></span>
> * <span data-ttu-id="8e098-110">コードを確認します</span><span class="sxs-lookup"><span data-stu-id="8e098-110">Examine the code</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a><span data-ttu-id="8e098-111">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="8e098-111">Prerequisites</span></span>

* <span data-ttu-id="8e098-112">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)で、 **ASP.NET および web 開発**ワークロード。</span><span class="sxs-lookup"><span data-stu-id="8e098-112">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="8e098-113">プロジェクトを設定します。</span><span class="sxs-lookup"><span data-stu-id="8e098-113">Set up the Project</span></span>

<span data-ttu-id="8e098-114">SignalR を追加するこのセクションでは、Visual Studio 2017 と SignalR 2 を使用して、空の ASP.NET web アプリケーションを作成する方法を示していて、チャット アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="8e098-114">This section shows how to use Visual Studio 2017 and SignalR 2 to create an empty ASP.NET web application, add SignalR, and create the chat application.</span></span>

1. <span data-ttu-id="8e098-115">Visual Studio で ASP.NET Web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="8e098-115">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![Web を作成します。](tutorial-getting-started-with-signalr/_static/image2.png)

1. <span data-ttu-id="8e098-117">**新しい ASP.NET プロジェクト - SignalRChat**  ウィンドウのままに**空**を選択し、選択**OK**。</span><span class="sxs-lookup"><span data-stu-id="8e098-117">In the **New ASP.NET Project - SignalRChat** window, leave **Empty** selected and select **OK**.</span></span>

1. <span data-ttu-id="8e098-118">**ソリューション エクスプ ローラー**プロジェクトを右クリックし、選択、**追加** > **新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="8e098-118">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="8e098-119">**新しい項目の追加 - SignalRChat**、**インストール済み** > **Visual C#**   >  **Web**  > **SignalR**選び**SignalR ハブ クラス (v2)** します。</span><span class="sxs-lookup"><span data-stu-id="8e098-119">In **Add New Item - SignalRChat**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="8e098-120">クラスの名前*ChatHub*し、プロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="8e098-120">Name the class *ChatHub* and add it to the project.</span></span>

    <span data-ttu-id="8e098-121">この手順で作成、 *ChatHub.cs*クラス ファイルとスクリプト ファイルとプロジェクトに SignalR をサポートするアセンブリ参照のセットを追加します。</span><span class="sxs-lookup"><span data-stu-id="8e098-121">This step creates the *ChatHub.cs* class file and adds a set of script files and assembly references that support SignalR to the project.</span></span>

1. <span data-ttu-id="8e098-122">新しいコードを置き換える*ChatHub.cs*次のコードでクラス ファイル。</span><span class="sxs-lookup"><span data-stu-id="8e098-122">Replace the code in the new *ChatHub.cs* class file with this code:</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]

1. <span data-ttu-id="8e098-123">**ソリューション エクスプ ローラー**プロジェクトを右クリックし、選択、**追加** > **新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="8e098-123">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="8e098-124">**新しい項目の追加 - SignalRChat**選択**インストール済み** > **Visual C#**   >  **Web**し選択**OWIN Startup クラス**します。</span><span class="sxs-lookup"><span data-stu-id="8e098-124">In **Add New Item - SignalRChat** select **Installed** > **Visual C#** > **Web**  and then select **OWIN Startup Class**.</span></span>

1. <span data-ttu-id="8e098-125">クラスの名前*スタートアップ*し、プロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="8e098-125">Name the class *Startup* and add it to the project.</span></span>

1. <span data-ttu-id="8e098-126">**ソリューション エクスプ ローラー**プロジェクトを右クリックし、選択、**追加** > **HTML ページ**します。</span><span class="sxs-lookup"><span data-stu-id="8e098-126">In **Solution Explorer**, right-click the project and select **Add** > **HTML Page**.</span></span>

1. <span data-ttu-id="8e098-127">名前を新しいページ*インデックス*を選択し、 **OK**します。</span><span class="sxs-lookup"><span data-stu-id="8e098-127">Name the new page *index* and select **OK**.</span></span>

1. <span data-ttu-id="8e098-128">**ソリューション エクスプ ローラー**で作成した HTML ページを右クリックし、選択**スタート ページとして設定**します。</span><span class="sxs-lookup"><span data-stu-id="8e098-128">In **Solution Explorer**, right-click the HTML page you created and select **Set as Start Page**.</span></span>

1. <span data-ttu-id="8e098-129">このコードでは、HTML ページの既定のコードを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="8e098-129">Replace the default code in the HTML page with this code:</span></span>

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]

1. <span data-ttu-id="8e098-130">**ソリューション エクスプ ローラー**、展開**スクリプト**します。</span><span class="sxs-lookup"><span data-stu-id="8e098-130">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="8e098-131">JQuery と SignalR 用のスクリプト ライブラリは、プロジェクトに表示されます。</span><span class="sxs-lookup"><span data-stu-id="8e098-131">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="8e098-132">パッケージ マネージャーは、以降のバージョンの SignalR スクリプトをインストールがある可能性があります。</span><span class="sxs-lookup"><span data-stu-id="8e098-132">The package manager may have installed a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="8e098-133">コード ブロック内のスクリプト参照がプロジェクト内のスクリプト ファイルのバージョンに対応することを確認します。</span><span class="sxs-lookup"><span data-stu-id="8e098-133">Check that the script references in the code block correspond to the versions of the script files in the project.</span></span>

    <span data-ttu-id="8e098-134">スクリプトの元のコード ブロックからの参照。</span><span class="sxs-lookup"><span data-stu-id="8e098-134">Script references from the original code block:</span></span>
    ```html
    <!--Script references. -->
    <!--Reference the jQuery library. -->
    <script src="Scripts/jquery-3.1.1.min.js" ></script>
    <!--Reference the SignalR library. -->
    <script src="Scripts/jquery.signalR-2.2.1.min.js"></script>
    ```

1. <span data-ttu-id="8e098-135">一致しない場合は、更新、 *.html*ファイル。</span><span class="sxs-lookup"><span data-stu-id="8e098-135">If they don't match, update the *.html* file.</span></span>

1. <span data-ttu-id="8e098-136">メニュー バーから選択**ファイル** > **すべて保存**します。</span><span class="sxs-lookup"><span data-stu-id="8e098-136">From the menu bar, select **File** > **Save All**.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="8e098-137">サンプルを実行します。</span><span class="sxs-lookup"><span data-stu-id="8e098-137">Run the Sample</span></span>

1. <span data-ttu-id="8e098-138">ツールバーで、有効にする**スクリプトのデバッグ**し、サンプルをデバッグ モードで実行する [再生] ボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="8e098-138">In the toolbar, turn on **Script Debugging** and then select the play button to run the sample in Debug mode.</span></span>

    ![ユーザー名の入力](tutorial-getting-started-with-signalr/_static/image3.png)

1. <span data-ttu-id="8e098-140">ブラウザーが開いたら、チャット、id の名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="8e098-140">When the browser opens, enter a name for your chat identity.</span></span>

1. <span data-ttu-id="8e098-141">ブラウザーから URL をコピーし、他の 2 つのブラウザーを開き、アドレス バーに Url を貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="8e098-141">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

1. <span data-ttu-id="8e098-142">各ブラウザーでは、一意の名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="8e098-142">In each browser, enter a unique name.</span></span>

1. <span data-ttu-id="8e098-143">ここで、追加、コメントを**送信**します。</span><span class="sxs-lookup"><span data-stu-id="8e098-143">Now, add a comment and select **Send**.</span></span> <span data-ttu-id="8e098-144">他のブラウザーでは繰り返しません。</span><span class="sxs-lookup"><span data-stu-id="8e098-144">Repeat that in the other browsers.</span></span> <span data-ttu-id="8e098-145">コメントは、リアルタイムで表示されます。</span><span class="sxs-lookup"><span data-stu-id="8e098-145">The comments appear in real-time.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8e098-146">この簡単なチャット アプリケーションでは、サーバー上のディスカッション コンテキストは保持されません。</span><span class="sxs-lookup"><span data-stu-id="8e098-146">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="8e098-147">ハブは、現在のすべてのユーザーへのコメントをブロードキャストします。</span><span class="sxs-lookup"><span data-stu-id="8e098-147">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="8e098-148">後で、チャットに参加するユーザーは、参加する時点から追加されたメッセージに表示されます。</span><span class="sxs-lookup"><span data-stu-id="8e098-148">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="8e098-149">次の 3 つのさまざまなブラウザーでのチャット アプリケーションの実行方法を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8e098-149">See how the chat application runs in three different browsers.</span></span> <span data-ttu-id="8e098-150">Tom、Anand、Susan は、メッセージを送信して、すべてのブラウザーはリアルタイムで更新します。</span><span class="sxs-lookup"><span data-stu-id="8e098-150">When Tom, Anand, and Susan send messages, all browsers update in real time:</span></span>

    ![次の 3 つのすべてのブラウザーが同じチャットの履歴を表示します。](tutorial-getting-started-with-signalr/_static/image4.png)

1. <span data-ttu-id="8e098-152">**ソリューション エクスプ ローラー**、検査、**スクリプト ドキュメント**実行中のアプリケーションのノード。</span><span class="sxs-lookup"><span data-stu-id="8e098-152">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="8e098-153">という名前のスクリプト ファイルがある*hubs* SignalR ライブラリは、実行時に生成します。</span><span class="sxs-lookup"><span data-stu-id="8e098-153">There's a script file named *hubs* that the SignalR library generates at runtime.</span></span> <span data-ttu-id="8e098-154">このファイルは、jQuery スクリプトとサーバー側コード間の通信を管理します。</span><span class="sxs-lookup"><span data-stu-id="8e098-154">This file manages the communication between jQuery script and server-side code.</span></span>

    ![スクリプト ドキュメント ノード内の自動生成されたハブのスクリプト](tutorial-getting-started-with-signalr/_static/image5.png)

## <a name="examine-the-code"></a><span data-ttu-id="8e098-156">コードを確認します</span><span class="sxs-lookup"><span data-stu-id="8e098-156">Examine the Code</span></span>

<span data-ttu-id="8e098-157">SignalRChat アプリケーションでは、2 つの基本的な SignalR 開発タスクを示します。</span><span class="sxs-lookup"><span data-stu-id="8e098-157">The SignalRChat application demonstrates two basic SignalR development tasks.</span></span> <span data-ttu-id="8e098-158">ハブを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="8e098-158">It shows you how to create a hub.</span></span> <span data-ttu-id="8e098-159">サーバーは、メインの調整オブジェクトとしてそのハブを使用します。</span><span class="sxs-lookup"><span data-stu-id="8e098-159">The server uses that hub as the main coordination object.</span></span> <span data-ttu-id="8e098-160">ハブは、メッセージを送受信する SignalR jQuery ライブラリを使用します。</span><span class="sxs-lookup"><span data-stu-id="8e098-160">The hub uses the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs-in-the-chathubcs"></a><span data-ttu-id="8e098-161">SignalR ハブで、ChatHub.cs</span><span class="sxs-lookup"><span data-stu-id="8e098-161">SignalR Hubs in the ChatHub.cs</span></span>

<span data-ttu-id="8e098-162">上記のコード サンプルで、`ChatHub`クラスから派生、`Microsoft.AspNet.SignalR.Hub`クラス。</span><span class="sxs-lookup"><span data-stu-id="8e098-162">In the code sample above, the `ChatHub` class derives from the `Microsoft.AspNet.SignalR.Hub` class.</span></span> <span data-ttu-id="8e098-163">派生する、`Hub`クラスは、SignalR アプリケーションを構築する便利な方法です。</span><span class="sxs-lookup"><span data-stu-id="8e098-163">Deriving from the `Hub` class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="8e098-164">ハブ クラスにパブリック メソッドを作成し、web ページ内のスクリプトから呼び出すことによってこれらのメソッドを使用できます。</span><span class="sxs-lookup"><span data-stu-id="8e098-164">You can create public methods on your hub class and then use those methods by calling them from scripts in a web page.</span></span>

<span data-ttu-id="8e098-165">クライアントが呼び出すチャットのコードで、`ChatHub.Send`新しいメッセージを送信する方法。</span><span class="sxs-lookup"><span data-stu-id="8e098-165">In the chat code, clients call the `ChatHub.Send` method to send a new message.</span></span> <span data-ttu-id="8e098-166">ハブのメッセージを送信しすべてのクライアントを呼び出して`Clients.All.broadcastMessage`します。</span><span class="sxs-lookup"><span data-stu-id="8e098-166">The hub then sends the message to all clients by calling `Clients.All.broadcastMessage`.</span></span>

<span data-ttu-id="8e098-167">`Send`メソッドがいくつかのハブの概念を示します。</span><span class="sxs-lookup"><span data-stu-id="8e098-167">The `Send` method demonstrates several hub concepts:</span></span>

* <span data-ttu-id="8e098-168">クライアントが呼び出すことができるように、ハブ上のパブリック メソッドを宣言します。</span><span class="sxs-lookup"><span data-stu-id="8e098-168">Declare public methods on a hub so that clients can call them.</span></span>

* <span data-ttu-id="8e098-169">使用して、`Microsoft.AspNet.SignalR.Hub.Clients`この hub に接続されているすべてのクライアントと通信する動的プロパティ。</span><span class="sxs-lookup"><span data-stu-id="8e098-169">Use the `Microsoft.AspNet.SignalR.Hub.Clients` dynamic property to communicate with all clients connected to this hub.</span></span>

* <span data-ttu-id="8e098-170">クライアントで関数を呼び出す (など、`broadcastMessage`関数) クライアントを更新します。</span><span class="sxs-lookup"><span data-stu-id="8e098-170">Call a function on the client (like the `broadcastMessage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery-in-the-indexhtml"></a><span data-ttu-id="8e098-171">SignalR と、index.html で jQuery</span><span class="sxs-lookup"><span data-stu-id="8e098-171">SignalR and jQuery in the index.html</span></span>

<span data-ttu-id="8e098-172">*Index.html*ページ コード サンプルでは、SignalR jQuery ライブラリを使用して、SignalR hub と通信する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="8e098-172">The *index.html* page in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="8e098-173">コードは、多くの重要なタスクを実行します。</span><span class="sxs-lookup"><span data-stu-id="8e098-173">The code carries out many important tasks.</span></span> <span data-ttu-id="8e098-174">ハブを参照するプロキシの宣言、ことをクライアントにコンテンツをプッシュするサーバーを呼び出すことができ、ハブにメッセージを送信への接続を開始関数を宣言します。</span><span class="sxs-lookup"><span data-stu-id="8e098-174">It declares a proxy to reference the hub, declares a function that the server can call to push content to clients, and it starts a connection to send messages to the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> <span data-ttu-id="8e098-175">JavaScript でサーバー クラスとそのメンバーへの参照は camelCase が。</span><span class="sxs-lookup"><span data-stu-id="8e098-175">In JavaScript the reference to the server class and its members has to be camelCase.</span></span> <span data-ttu-id="8e098-176">コード サンプルの参照、 C# *ChatHub*として JavaScript でクラス`chatHub`します。</span><span class="sxs-lookup"><span data-stu-id="8e098-176">The code sample references the C# *ChatHub* class in JavaScript as `chatHub`.</span></span>

<span data-ttu-id="8e098-177">このコード ブロックでは、スクリプト内のコールバック関数を作成します。</span><span class="sxs-lookup"><span data-stu-id="8e098-177">In this code block, you create a callback function in the script.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

<span data-ttu-id="8e098-178">サーバー上のハブ クラスは、各クライアントにコンテンツの更新をプッシュするには、この関数を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="8e098-178">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="8e098-179">2 つの行を HTML エンコード、コンテンツを表示する前に省略可能で、スクリプト インジェクションを防止する優れた方法を表示します。</span><span class="sxs-lookup"><span data-stu-id="8e098-179">The two lines that HTML-encode the content before displaying it are optional and show a good way to prevent script injection.</span></span>

<span data-ttu-id="8e098-180">このコードは、ハブとの接続を開きます。</span><span class="sxs-lookup"><span data-stu-id="8e098-180">This code opens a connection with the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

> [!NOTE]
> <span data-ttu-id="8e098-181">このアプローチにより、イベント ハンドラーが実行される前に、コードでの接続を確立するようになります。</span><span class="sxs-lookup"><span data-stu-id="8e098-181">This approach ensures that the code establishes a connection before the event handler executes.</span></span>

<span data-ttu-id="8e098-182">コードが接続を開始しのクリック イベントを処理する関数に渡します、**送信**HTML ページにボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="8e098-182">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the HTML page.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="8e098-183">コードを取得する</span><span class="sxs-lookup"><span data-stu-id="8e098-183">Get the code</span></span>

[<span data-ttu-id="8e098-184">完成したプロジェクトのダウンロード</span><span class="sxs-lookup"><span data-stu-id="8e098-184">Download Completed Project</span></span>](http://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)

## <a name="additional-resources"></a><span data-ttu-id="8e098-185">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="8e098-185">Additional resources</span></span>

<span data-ttu-id="8e098-186">詳細については、SignalR は、次のリソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="8e098-186">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="8e098-187">SignalR プロジェクト</span><span class="sxs-lookup"><span data-stu-id="8e098-187">SignalR Project</span></span>](http://signalr.net)

* [<span data-ttu-id="8e098-188">SignalR Github とサンプル</span><span class="sxs-lookup"><span data-stu-id="8e098-188">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)

* [<span data-ttu-id="8e098-189">SignalR の Wiki</span><span class="sxs-lookup"><span data-stu-id="8e098-189">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="8e098-190">次の手順</span><span class="sxs-lookup"><span data-stu-id="8e098-190">Next steps</span></span>

<span data-ttu-id="8e098-191">このチュートリアルではします。</span><span class="sxs-lookup"><span data-stu-id="8e098-191">In this tutorial you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8e098-192">プロジェクトを設定します。</span><span class="sxs-lookup"><span data-stu-id="8e098-192">Set up the project</span></span>
> * <span data-ttu-id="8e098-193">サンプルの実行</span><span class="sxs-lookup"><span data-stu-id="8e098-193">Ran the sample</span></span>
> * <span data-ttu-id="8e098-194">コードを調べる</span><span class="sxs-lookup"><span data-stu-id="8e098-194">Examined the code</span></span>

<span data-ttu-id="8e098-195">SignalR と MVC 5 を使用する方法については、次の記事に進んでください。</span><span class="sxs-lookup"><span data-stu-id="8e098-195">Advance to the next article to learn how to use SignalR and MVC 5.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="8e098-196">SignalR 2 と MVC 5</span><span class="sxs-lookup"><span data-stu-id="8e098-196">SignalR 2 and MVC 5</span></span>](tutorial-getting-started-with-signalr-and-mvc.md)