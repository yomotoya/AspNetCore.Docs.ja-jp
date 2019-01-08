---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
title: 'チュートリアル: SignalR 2 と MVC 5 のリアルタイムのチャット |Microsoft Docs'
author: pfletcher
description: このチュートリアルでは、ASP.NET SignalR 2 を使用して、リアルタイムのチャット アプリケーションを作成する方法を示します。 SignalR は、MVC 5 アプリケーションを追加します。
ms.author: riande
ms.date: 01/02/2019
ms.assetid: 80bfe5fb-bdfc-41fe-ac43-2132e5d69fac
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: eb4b7e1403f4070d65702b756bf98c5294c7fb17
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098605"
---
# <a name="tutorial-real-time-chat-with-signalr-2-and-mvc-5"></a><span data-ttu-id="42b5a-104">チュートリアル: SignalR 2 と MVC 5 のリアルタイムのチャット</span><span class="sxs-lookup"><span data-stu-id="42b5a-104">Tutorial: Real-time chat with SignalR 2 and MVC 5</span></span>

<span data-ttu-id="42b5a-105">このチュートリアルでは、ASP.NET SignalR 2 を使用して、リアルタイムのチャット アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="42b5a-105">This tutorial shows how to use ASP.NET SignalR 2 to create a real-time chat application.</span></span> <span data-ttu-id="42b5a-106">SignalR を MVC 5 アプリケーションに追加し、チャットを送信し、メッセージを表示するビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="42b5a-106">You add SignalR to an MVC 5 application and create a chat view to send and display messages.</span></span>

<span data-ttu-id="42b5a-107">このチュートリアルでしました。</span><span class="sxs-lookup"><span data-stu-id="42b5a-107">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="42b5a-108">プロジェクトを設定します。</span><span class="sxs-lookup"><span data-stu-id="42b5a-108">Set up the project</span></span>
> * <span data-ttu-id="42b5a-109">サンプルを実行します。</span><span class="sxs-lookup"><span data-stu-id="42b5a-109">Run the sample</span></span>
> * <span data-ttu-id="42b5a-110">コードを確認します</span><span class="sxs-lookup"><span data-stu-id="42b5a-110">Examine the code</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a><span data-ttu-id="42b5a-111">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="42b5a-111">Prerequisites</span></span>

* <span data-ttu-id="42b5a-112">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)で、 **ASP.NET および web 開発**ワークロード。</span><span class="sxs-lookup"><span data-stu-id="42b5a-112">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="42b5a-113">プロジェクトを設定します。</span><span class="sxs-lookup"><span data-stu-id="42b5a-113">Set up the Project</span></span>

<span data-ttu-id="42b5a-114">このセクションでは、Visual Studio 2017 と SignalR 2 を使用して、空の ASP.NET MVC 5 アプリケーションを作成し、SignalR ライブラリの追加、チャット アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="42b5a-114">This section shows how to use Visual Studio 2017 and SignalR 2 to create an empty ASP.NET MVC 5 application, add the SignalR library, and create the chat application.</span></span>

1. <span data-ttu-id="42b5a-115">Visual Studio で、c# ASP.NET アプリケーションを作成するには、.NET Framework 4.5 を対象として、SignalRChat、という名前を付けます [ok] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="42b5a-115">In Visual Studio, create a C# ASP.NET application that targets .NET Framework 4.5, name it SignalRChat, and click OK.</span></span>

    ![Web を作成します。](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

1. <span data-ttu-id="42b5a-117">**新しい ASP.NET Web アプリケーション - SignalRMvcChat**を選択します**MVC**選び**認証の変更**します。</span><span class="sxs-lookup"><span data-stu-id="42b5a-117">In **New ASP.NET Web Application - SignalRMvcChat**, select **MVC** and then select **Change Authentication**.</span></span>

1. <span data-ttu-id="42b5a-118">**認証の変更**を選択します**認証なし** をクリック**OK**します。</span><span class="sxs-lookup"><span data-stu-id="42b5a-118">In **Change Authentication**, select **No Authentication** and click **OK**.</span></span>

    ![認証なし を選択します。](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)

1. <span data-ttu-id="42b5a-120">**新しい ASP.NET Web アプリケーション - SignalRMvcChat**、 **OK**。</span><span class="sxs-lookup"><span data-stu-id="42b5a-120">In **New ASP.NET Web Application - SignalRMvcChat**, select **OK**.</span></span>

1. <span data-ttu-id="42b5a-121">**ソリューション エクスプ ローラー**プロジェクトを右クリックし、選択、**追加** > **新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="42b5a-121">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="42b5a-122">**新しい項目の追加 - SignalRChat**、**インストール済み** > **Visual C#**   >  **Web**  > **SignalR**選び**SignalR ハブ クラス (v2)** します。</span><span class="sxs-lookup"><span data-stu-id="42b5a-122">In **Add New Item - SignalRChat**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="42b5a-123">クラスの名前*ChatHub*し、プロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="42b5a-123">Name the class *ChatHub* and add it to the project.</span></span>

    <span data-ttu-id="42b5a-124">この手順で作成、 *ChatHub.cs*クラス ファイルとスクリプト ファイルとプロジェクトに SignalR をサポートするアセンブリ参照のセットを追加します。</span><span class="sxs-lookup"><span data-stu-id="42b5a-124">This step creates the *ChatHub.cs* class file and adds a set of script files and assembly references that support SignalR to the project.</span></span>

1. <span data-ttu-id="42b5a-125">新しいコードを置き換える*ChatHub.cs*次のコードでクラス ファイル。</span><span class="sxs-lookup"><span data-stu-id="42b5a-125">Replace the code in the new *ChatHub.cs* class file with this code:</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]

1. <span data-ttu-id="42b5a-126">**ソリューション エクスプ ローラー**プロジェクトを右クリックし、選択、**追加** > **クラス**します。</span><span class="sxs-lookup"><span data-stu-id="42b5a-126">In **Solution Explorer**, right-click the project and select **Add** > **Class**.</span></span>

1. <span data-ttu-id="42b5a-127">新しいクラスの名前*スタートアップ*し、プロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="42b5a-127">Name the new class *Startup* and add it to the project.</span></span>

1. <span data-ttu-id="42b5a-128">コードに置き換えます、 *Startup.cs*次のコードでクラス ファイル。</span><span class="sxs-lookup"><span data-stu-id="42b5a-128">Replace the code in the *Startup.cs* class file with this code:</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]

1. <span data-ttu-id="42b5a-129">**ソリューション エクスプ ローラー**、**コント ローラー** > **HomeController.cs**します。</span><span class="sxs-lookup"><span data-stu-id="42b5a-129">In **Solution Explorer**, select **Controllers** > **HomeController.cs**.</span></span>

1. <span data-ttu-id="42b5a-130">このメソッドを追加、 *HomeController.cs*します。</span><span class="sxs-lookup"><span data-stu-id="42b5a-130">Add this method to the *HomeController.cs*.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]

    <span data-ttu-id="42b5a-131">このメソッドが戻る、**チャット**後の手順で作成するビュー。</span><span class="sxs-lookup"><span data-stu-id="42b5a-131">This method returns the **Chat** view that you create in a later step.</span></span>

1. <span data-ttu-id="42b5a-132">**ソリューション エクスプ ローラー**、右クリック**ビュー** > **ホーム**、選び**追加** >   **ビュー**します。</span><span class="sxs-lookup"><span data-stu-id="42b5a-132">In **Solution Explorer**, right-click **Views** > **Home**, and select **Add** >  **View**.</span></span>

1. <span data-ttu-id="42b5a-133">**ビューの追加**、新しいビューを指定して**チャット**選択**追加**します。</span><span class="sxs-lookup"><span data-stu-id="42b5a-133">In **Add View**, name the new view **Chat** and select **Add**.</span></span>

1. <span data-ttu-id="42b5a-134">内容を置き換える**Chat.cshtml**次のコードで。</span><span class="sxs-lookup"><span data-stu-id="42b5a-134">Replace the contents of **Chat.cshtml** with this code:</span></span>

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]

1. <span data-ttu-id="42b5a-135">**ソリューション エクスプ ローラー**、展開**スクリプト**します。</span><span class="sxs-lookup"><span data-stu-id="42b5a-135">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="42b5a-136">JQuery と SignalR 用のスクリプト ライブラリは、プロジェクトに表示されます。</span><span class="sxs-lookup"><span data-stu-id="42b5a-136">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="42b5a-137">パッケージ マネージャーは、以降のバージョンの SignalR スクリプトをインストールがある可能性があります。</span><span class="sxs-lookup"><span data-stu-id="42b5a-137">The package manager may have installed a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="42b5a-138">コード ブロック内のスクリプト参照がプロジェクト内のスクリプト ファイルのバージョンに対応することを確認します。</span><span class="sxs-lookup"><span data-stu-id="42b5a-138">Check that the script references in the code block correspond to the versions of the script files in the project.</span></span>

    <span data-ttu-id="42b5a-139">スクリプトの元のコード ブロックからの参照。</span><span class="sxs-lookup"><span data-stu-id="42b5a-139">Script references from the original code block:</span></span>

    ```cshtml
    <!--Script references. -->
    <!--The jQuery library is required and is referenced by default in _Layout.cshtml. -->
    <!--Reference the SignalR library. -->
    <script src="~/Scripts/jquery.signalR-2.1.0.min.js"></script>
    ```

1. <span data-ttu-id="42b5a-140">一致しない場合は、更新、 *.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="42b5a-140">If they don't match, update the *.cshtml* file.</span></span>

1. <span data-ttu-id="42b5a-141">メニュー バーから選択**ファイル** > **すべて保存**します。</span><span class="sxs-lookup"><span data-stu-id="42b5a-141">From the menu bar, select **File** > **Save All**.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="42b5a-142">サンプルを実行します。</span><span class="sxs-lookup"><span data-stu-id="42b5a-142">Run the Sample</span></span>

1. <span data-ttu-id="42b5a-143">ツールバーで、有効にする**スクリプトのデバッグ**し、サンプルをデバッグ モードで実行する [再生] ボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="42b5a-143">In the toolbar, turn on **Script Debugging** and then select the play button to run the sample in Debug mode.</span></span>

    ![ユーザー名の入力](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)

1. <span data-ttu-id="42b5a-145">ブラウザーが開いたら、チャット、id の名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="42b5a-145">When the browser opens, enter a name for your chat identity.</span></span>

1. <span data-ttu-id="42b5a-146">ブラウザーから URL をコピーし、他の 2 つのブラウザーを開き、アドレス バーに Url を貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="42b5a-146">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

1. <span data-ttu-id="42b5a-147">各ブラウザーでは、一意の名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="42b5a-147">In each browser, enter a unique name.</span></span>

1. <span data-ttu-id="42b5a-148">ここで、追加、コメントを**送信**します。</span><span class="sxs-lookup"><span data-stu-id="42b5a-148">Now, add a comment and select **Send**.</span></span> <span data-ttu-id="42b5a-149">他のブラウザーでは繰り返しません。</span><span class="sxs-lookup"><span data-stu-id="42b5a-149">Repeat that in the other browsers.</span></span> <span data-ttu-id="42b5a-150">コメントは、リアルタイムで表示されます。</span><span class="sxs-lookup"><span data-stu-id="42b5a-150">The comments appear in real time.</span></span>

    > [!NOTE]
    > <span data-ttu-id="42b5a-151">この簡単なチャット アプリケーションでは、サーバー上のディスカッション コンテキストは保持されません。</span><span class="sxs-lookup"><span data-stu-id="42b5a-151">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="42b5a-152">ハブは、現在のすべてのユーザーへのコメントをブロードキャストします。</span><span class="sxs-lookup"><span data-stu-id="42b5a-152">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="42b5a-153">後で、チャットに参加するユーザーは、参加する時点から追加されたメッセージに表示されます。</span><span class="sxs-lookup"><span data-stu-id="42b5a-153">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="42b5a-154">次の 3 つのさまざまなブラウザーでのチャット アプリケーションの実行方法を参照してください。</span><span class="sxs-lookup"><span data-stu-id="42b5a-154">See how the chat application runs in three different browsers.</span></span> <span data-ttu-id="42b5a-155">Tom、Anand、Susan は、メッセージを送信して、すべてのブラウザーはリアルタイムで更新します。</span><span class="sxs-lookup"><span data-stu-id="42b5a-155">When Tom, Anand, and Susan send messages, all browsers update in real time:</span></span>

    ![次の 3 つのすべてのブラウザーが同じチャットの履歴を表示します。](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

1. <span data-ttu-id="42b5a-157">**ソリューション エクスプ ローラー**、検査、**スクリプト ドキュメント**実行中のアプリケーションのノード。</span><span class="sxs-lookup"><span data-stu-id="42b5a-157">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="42b5a-158">という名前のスクリプト ファイルがある*hubs* SignalR ライブラリは、実行時に生成します。</span><span class="sxs-lookup"><span data-stu-id="42b5a-158">There's a script file named *hubs* that the SignalR library generates at runtime.</span></span> <span data-ttu-id="42b5a-159">このファイルは、jQuery スクリプトとサーバー側コード間の通信を管理します。</span><span class="sxs-lookup"><span data-stu-id="42b5a-159">This file manages the communication between jQuery script and server-side code.</span></span>

    ![スクリプト ドキュメント ノード内の自動生成されたハブのスクリプト](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)

## <a name="examine-the-code"></a><span data-ttu-id="42b5a-161">コードを確認します</span><span class="sxs-lookup"><span data-stu-id="42b5a-161">Examine the Code</span></span>

<span data-ttu-id="42b5a-162">SignalR のチャット アプリケーションでは、2 つの基本的な SignalR 開発タスクを示しています。</span><span class="sxs-lookup"><span data-stu-id="42b5a-162">The SignalR chat application demonstrates two basic SignalR development tasks.</span></span> <span data-ttu-id="42b5a-163">ハブを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="42b5a-163">It shows you how to create a hub.</span></span> <span data-ttu-id="42b5a-164">サーバーは、メインの調整オブジェクトとしてそのハブを使用します。</span><span class="sxs-lookup"><span data-stu-id="42b5a-164">The server uses that hub as the main coordination object.</span></span> <span data-ttu-id="42b5a-165">ハブは、メッセージを送受信する SignalR jQuery ライブラリを使用します。</span><span class="sxs-lookup"><span data-stu-id="42b5a-165">The hub uses the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs-in-the-chathubcs"></a><span data-ttu-id="42b5a-166">SignalR ハブで、ChatHub.cs</span><span class="sxs-lookup"><span data-stu-id="42b5a-166">SignalR Hubs in the ChatHub.cs</span></span>

<span data-ttu-id="42b5a-167">コード サンプルで、`ChatHub`クラスから派生、`Microsoft.AspNet.SignalR.Hub`クラス。</span><span class="sxs-lookup"><span data-stu-id="42b5a-167">In the code sample, the `ChatHub` class derives from the `Microsoft.AspNet.SignalR.Hub` class.</span></span> <span data-ttu-id="42b5a-168">派生する、`Hub`クラスは、SignalR アプリケーションを構築する便利な方法です。</span><span class="sxs-lookup"><span data-stu-id="42b5a-168">Deriving from the `Hub` class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="42b5a-169">ハブ クラスにパブリック メソッドを作成し、web ページ内のスクリプトから呼び出すことによってこれらのメソッドにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="42b5a-169">You can create public methods on your hub class and then access those methods by calling them from scripts in a web page.</span></span>

<span data-ttu-id="42b5a-170">クライアントが呼び出すチャットのコードで、`ChatHub.Send`新しいメッセージを送信する方法。</span><span class="sxs-lookup"><span data-stu-id="42b5a-170">In the chat code, clients call the `ChatHub.Send` method to send a new message.</span></span> <span data-ttu-id="42b5a-171">ハブに送信メッセージのすべてのクライアントを呼び出して`Clients.All.addNewMessageToPage`します。</span><span class="sxs-lookup"><span data-stu-id="42b5a-171">The hub in turn sends the message to all clients by calling `Clients.All.addNewMessageToPage`.</span></span>

<span data-ttu-id="42b5a-172">`Send`メソッドがいくつかのハブの概念を示します。</span><span class="sxs-lookup"><span data-stu-id="42b5a-172">The `Send` method demonstrates several hub concepts:</span></span>

* <span data-ttu-id="42b5a-173">クライアントが呼び出すことができるように、ハブ上のパブリック メソッドを宣言します。</span><span class="sxs-lookup"><span data-stu-id="42b5a-173">Declare public methods on a hub so that clients can call them.</span></span>

* <span data-ttu-id="42b5a-174">使用して、`Microsoft.AspNet.SignalR.Hub.Clients`この hub に接続されているすべてのクライアントと通信する動的プロパティ。</span><span class="sxs-lookup"><span data-stu-id="42b5a-174">Use the `Microsoft.AspNet.SignalR.Hub.Clients` dynamic property to communicate with all clients connected to this hub.</span></span>

* <span data-ttu-id="42b5a-175">クライアントで関数を呼び出す (など、`addNewMessageToPage`関数) クライアントを更新します。</span><span class="sxs-lookup"><span data-stu-id="42b5a-175">Call a function on the client (like the `addNewMessageToPage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery-chatcshtml"></a><span data-ttu-id="42b5a-176">SignalR と jQuery Chat.cshtml</span><span class="sxs-lookup"><span data-stu-id="42b5a-176">SignalR and jQuery Chat.cshtml</span></span>

<span data-ttu-id="42b5a-177">*Chat.cshtml*ビュー ファイルのコード サンプルでは、SignalR jQuery ライブラリを使用して、SignalR hub と通信する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="42b5a-177">The *Chat.cshtml* view file in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span>  <span data-ttu-id="42b5a-178">コードは、多くの重要なタスクを実行します。</span><span class="sxs-lookup"><span data-stu-id="42b5a-178">The code carries out many important tasks.</span></span> <span data-ttu-id="42b5a-179">ハブの自動生成されたプロキシへの参照を作成しをクライアントにコンテンツをプッシュするサーバーを呼び出すことができ、ハブにメッセージを送信への接続を開始する関数を宣言します。</span><span class="sxs-lookup"><span data-stu-id="42b5a-179">It creates a reference to the autogenerated proxy for the hub, declares a function that the server can call to push content to clients, and it starts a connection to send messages to the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="42b5a-180">JavaScript では、サーバー クラスとそのメンバーへの参照はキャメル ケースでは。</span><span class="sxs-lookup"><span data-stu-id="42b5a-180">In JavaScript, the reference to the server class and its members is in camelCase.</span></span> <span data-ttu-id="42b5a-181">コード サンプルの参照、 C# `ChatHub`として JavaScript でクラス`chatHub`します。</span><span class="sxs-lookup"><span data-stu-id="42b5a-181">The code sample references the C# `ChatHub` class in JavaScript as `chatHub`.</span></span>

<span data-ttu-id="42b5a-182">このコード ブロックでは、スクリプト内のコールバック関数を作成します。</span><span class="sxs-lookup"><span data-stu-id="42b5a-182">In this code block, you create a callback function in the script.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

<span data-ttu-id="42b5a-183">サーバー上のハブ クラスは、各クライアントにコンテンツの更新をプッシュするには、この関数を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="42b5a-183">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="42b5a-184">省略可能な呼び出し、`htmlEncode`関数を HTML に方法が表示されますが、ページに表示する前に、メッセージの内容をエンコードします。</span><span class="sxs-lookup"><span data-stu-id="42b5a-184">The optional call to the `htmlEncode` function shows a way to HTML encode the message content before displaying it in the page.</span></span> <span data-ttu-id="42b5a-185">スクリプト インジェクションを防止する方法になります。</span><span class="sxs-lookup"><span data-stu-id="42b5a-185">It's a way to prevent script injection.</span></span>

<span data-ttu-id="42b5a-186">このコードは、ハブとの接続を開きます。</span><span class="sxs-lookup"><span data-stu-id="42b5a-186">This code opens a connection with the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

> [!NOTE]
> <span data-ttu-id="42b5a-187">このアプローチによりイベント ハンドラーが実行される前に、接続を確立するようになります。</span><span class="sxs-lookup"><span data-stu-id="42b5a-187">This approach ensures that you establish a connection before the event handler executes.</span></span>

<span data-ttu-id="42b5a-188">コードが接続を開始しのクリック イベントを処理する関数に渡します、**送信**チャット ページ ボタン。</span><span class="sxs-lookup"><span data-stu-id="42b5a-188">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the Chat page.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="42b5a-189">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="42b5a-189">Additional resources</span></span>

<span data-ttu-id="42b5a-190">詳細については、SignalR は、次のリソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="42b5a-190">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="42b5a-191">SignalR プロジェクト</span><span class="sxs-lookup"><span data-stu-id="42b5a-191">SignalR Project</span></span>](http://signalr.net)

* [<span data-ttu-id="42b5a-192">SignalR GitHub とサンプル</span><span class="sxs-lookup"><span data-stu-id="42b5a-192">SignalR GitHub and Samples</span></span>](https://github.com/SignalR/SignalR)

* [<span data-ttu-id="42b5a-193">SignalR の Wiki</span><span class="sxs-lookup"><span data-stu-id="42b5a-193">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="42b5a-194">次の手順</span><span class="sxs-lookup"><span data-stu-id="42b5a-194">Next steps</span></span>

<span data-ttu-id="42b5a-195">このチュートリアルでしました。</span><span class="sxs-lookup"><span data-stu-id="42b5a-195">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="42b5a-196">プロジェクトを設定します。</span><span class="sxs-lookup"><span data-stu-id="42b5a-196">Set up the project</span></span>
> * <span data-ttu-id="42b5a-197">サンプルの実行</span><span class="sxs-lookup"><span data-stu-id="42b5a-197">Ran the sample</span></span>
> * <span data-ttu-id="42b5a-198">コードを調べる</span><span class="sxs-lookup"><span data-stu-id="42b5a-198">Examined the code</span></span>

<span data-ttu-id="42b5a-199">ASP.NET SignalR 2 を使用して、頻度の高いメッセージング機能を提供する web アプリケーションを作成する方法については、次の記事に進んでください。</span><span class="sxs-lookup"><span data-stu-id="42b5a-199">Advance to the next article to learn how to create a web application that uses ASP.NET SignalR 2 to provide high-frequency messaging functionality.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="42b5a-200">頻度の高いメッセージングを使用した web アプリ</span><span class="sxs-lookup"><span data-stu-id="42b5a-200">Web app with high-frequency messaging</span></span>](tutorial-high-frequency-realtime-with-signalr.md)