---
uid: signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
title: 'チュートリアル: SignalR 2 使用頻度の高いリアルタイム アプリの作成 |Microsoft Docs'
author: bradygaster
description: このチュートリアルでは、ASP.NET SignalR を使用して、頻度の高いメッセージング機能を提供する web アプリケーションを作成する方法を示します。
ms.author: bradyg
ms.date: 01/22/2019
ms.assetid: 9f969dda-78ea-4329-b1e3-e51c02210a2b
msc.legacyurl: /signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: 44aaa2b0c059de310e963f642fa56c2f00a7e443
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836728"
---
# <a name="tutorial-create-high-frequency-real-time-app-with-signalr-2"></a><span data-ttu-id="6fcb1-103">チュートリアル: SignalR 2 使用頻度の高いリアルタイム アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-103">Tutorial: Create high-frequency real-time app with SignalR 2</span></span>

<span data-ttu-id="6fcb1-104">このチュートリアルでは、ASP.NET SignalR 2 を使用して、頻度の高いメッセージング機能を提供する web アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-104">This tutorial shows how to create a web application that uses ASP.NET SignalR 2 to provide high-frequency messaging functionality.</span></span> <span data-ttu-id="6fcb1-105">この場合は、「頻度の高いメッセージング」は、サーバーは、固定の率での更新を送信します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-105">In this case, "high-frequency messaging" means the server sends updates at a fixed rate.</span></span> <span data-ttu-id="6fcb1-106">1 秒間に最大 10 個のメッセージを送信するとします。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-106">You send up to 10 messages a second.</span></span>

<span data-ttu-id="6fcb1-107">アプリケーションを作成するには、ユーザーがドラッグできるシェイプが表示されます。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-107">The application you create displays a shape that users can drag.</span></span> <span data-ttu-id="6fcb1-108">サーバーは、時間指定の更新プログラムを使用して、ドラッグした図形の位置に一致するように接続されているすべてのブラウザーでの図形の位置を更新します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-108">The server updates the position of the shape in all connected browsers to match the position of the dragged shape using timed updates.</span></span>

<span data-ttu-id="6fcb1-109">このチュートリアルで導入された概念には、リアルタイムのゲームでのアプリケーションおよびその他のシミュレーション アプリケーションがあります。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-109">Concepts introduced in this tutorial have applications in real-time gaming and other simulation applications.</span></span>

<span data-ttu-id="6fcb1-110">このチュートリアルでは、次の作業を行いました。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-110">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6fcb1-111">プロジェクトを設定します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-111">Set up the project</span></span>
> * <span data-ttu-id="6fcb1-112">ベースのアプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-112">Create the base application</span></span>
> * <span data-ttu-id="6fcb1-113">アプリの開始時に、ハブにマップします。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-113">Map to the hub when app starts</span></span>
> * <span data-ttu-id="6fcb1-114">クライアントを追加します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-114">Add the client</span></span>
> * <span data-ttu-id="6fcb1-115">アプリを実行する</span><span class="sxs-lookup"><span data-stu-id="6fcb1-115">Run the app</span></span>
> * <span data-ttu-id="6fcb1-116">クライアントのループを追加します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-116">Add the client loop</span></span>
> * <span data-ttu-id="6fcb1-117">サーバー ループを追加します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-117">Add the server loop</span></span>
> * <span data-ttu-id="6fcb1-118">滑らかなアニメーションを追加します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-118">Add smooth animation</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a><span data-ttu-id="6fcb1-119">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="6fcb1-119">Prerequisites</span></span>

* <span data-ttu-id="6fcb1-120">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)で、 **ASP.NET および web 開発**ワークロード。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-120">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="6fcb1-121">プロジェクトを設定します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-121">Set up the project</span></span>

<span data-ttu-id="6fcb1-122">このセクションでは、Visual Studio 2017 でプロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-122">In this section, you create the project in Visual Studio 2017.</span></span>

<span data-ttu-id="6fcb1-123">このセクションでは、Visual Studio 2017 を使用して、空の ASP.NET Web アプリケーションを作成し、SignalR と jQuery.UI ライブラリを追加する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-123">This section shows how to use Visual Studio 2017 to create an empty ASP.NET Web Application and add the SignalR and jQuery.UI libraries.</span></span>

1. <span data-ttu-id="6fcb1-124">Visual Studio で ASP.NET Web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-124">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![Web を作成します。](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

1. <span data-ttu-id="6fcb1-126">**新しい ASP.NET Web アプリケーション - MoveShapeDemo**  ウィンドウのままに**空**を選択し、選択**OK**。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-126">In the **New ASP.NET Web Application - MoveShapeDemo** window, leave **Empty** selected and select **OK**.</span></span>

1. <span data-ttu-id="6fcb1-127">**ソリューション エクスプ ローラー**プロジェクトを右クリックし、選択、**追加** > **新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-127">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="6fcb1-128">**新しい項目の追加 - MoveShapeDemo**、**インストール済み** > **Visual C#**   >  **Web**  > **SignalR**選び**SignalR ハブ クラス (v2)** します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-128">In **Add New Item - MoveShapeDemo**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="6fcb1-129">クラスの名前*MoveShapeHub*し、プロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-129">Name the class *MoveShapeHub* and add it to the project.</span></span>

    <span data-ttu-id="6fcb1-130">この手順で作成、 *MoveShapeHub.cs*クラス ファイル。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-130">This step creates the *MoveShapeHub.cs* class file.</span></span> <span data-ttu-id="6fcb1-131">同時に、一連のスクリプト ファイルとプロジェクトに SignalR をサポートするアセンブリ参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-131">Simultaneously, it adds  a set of script files and assembly references that support SignalR to the project.</span></span>

1. <span data-ttu-id="6fcb1-132">選択**ツール** > **NuGet パッケージ マネージャー** > **パッケージ マネージャー コンソール**します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-132">Select **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

1. <span data-ttu-id="6fcb1-133">**パッケージ マネージャー コンソール**、このコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-133">In **Package Manager Console**, run this command:</span></span>

    ```console
    Install-Package jQuery.UI.Combined
    ```

    <span data-ttu-id="6fcb1-134">コマンドは、jQuery UI ライブラリをインストールします。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-134">The command installs the jQuery UI library.</span></span> <span data-ttu-id="6fcb1-135">これを使用して、図形をアニメーション化します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-135">You use it to animate the shape.</span></span>

1. <span data-ttu-id="6fcb1-136">**ソリューション エクスプ ローラー**スクリプトを展開します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-136">In **Solution Explorer**, expand the Scripts node.</span></span>

    ![スクリプト ライブラリの参照](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)

    <span data-ttu-id="6fcb1-138">JQuery や jQueryUI、SignalR のスクリプト ライブラリは、プロジェクトに表示されます。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-138">Script libraries for jQuery, jQueryUI, and SignalR are visible in the project.</span></span>

## <a name="create-the-base-application"></a><span data-ttu-id="6fcb1-139">ベースのアプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-139">Create the base application</span></span>

<span data-ttu-id="6fcb1-140">このセクションでは、ブラウザー アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-140">In this section, you create a browser application.</span></span> <span data-ttu-id="6fcb1-141">アプリは、各マウス移動イベント中に、図形の場所をサーバーに送信します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-141">The app sends the location of the shape to the server during each mouse move event.</span></span> <span data-ttu-id="6fcb1-142">サーバーは、リアルタイムで接続されているその他のすべてのクライアントにこの情報をブロードキャストします。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-142">The server broadcasts this information to all other connected clients in real time.</span></span> <span data-ttu-id="6fcb1-143">以降のセクションでは、このアプリケーションの詳細について説明します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-143">You learn more about this application in later sections.</span></span>

1. <span data-ttu-id="6fcb1-144">開く、 *MoveShapeHub.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-144">Open the *MoveShapeHub.cs* file.</span></span>

1. <span data-ttu-id="6fcb1-145">コードに置き換えます、 *MoveShapeHub.cs*このコード ファイル。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-145">Replace the code in the *MoveShapeHub.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.cs)]

1. <span data-ttu-id="6fcb1-146">ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-146">Save the file.</span></span>

<span data-ttu-id="6fcb1-147">`MoveShapeHub`クラスは、SignalR ハブの実装。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-147">The `MoveShapeHub` class is an implementation of a SignalR hub.</span></span> <span data-ttu-id="6fcb1-148">[SignalR の概要](tutorial-getting-started-with-signalr.md)チュートリアルでは、ハブには、クライアントを直接呼び出すメソッドがあります。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-148">As in the [Getting Started with SignalR](tutorial-getting-started-with-signalr.md) tutorial, the hub has a method that the clients call directly.</span></span> <span data-ttu-id="6fcb1-149">この場合、クライアントから送信図形をサーバーの新しい X 座標と Y を持つオブジェクトを調整します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-149">In this case, the client sends an object with the new X and Y coordinates of the shape to the server.</span></span> <span data-ttu-id="6fcb1-150">これらの座標は、接続されている他のすべてのクライアントにブロードキャストを取得します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-150">Those coordinates get broadcasted to all other connected clients.</span></span> <span data-ttu-id="6fcb1-151">SignalR には、このオブジェクトの JSON を使用して自動的にシリアル化します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-151">SignalR automatically serializes this object using JSON.</span></span>

<span data-ttu-id="6fcb1-152">アプリの送信、`ShapeModel`クライアント オブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-152">The app sends the `ShapeModel` object to the client.</span></span> <span data-ttu-id="6fcb1-153">図形の位置を格納するメンバーが存在します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-153">It has members to store the position of the shape.</span></span> <span data-ttu-id="6fcb1-154">サーバー上のオブジェクトのバージョンには、どのクライアントのデータが格納されるを追跡するにはメンバーもあります。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-154">The version of the object on the server also has a member to track which client's data is being stored.</span></span> <span data-ttu-id="6fcb1-155">このオブジェクトは、サーバーがそれ自体へのクライアントのデータを送信することを防ぎます。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-155">This object prevents the server from sending a client's data back to itself.</span></span> <span data-ttu-id="6fcb1-156">このメンバーを使用して、`JsonIgnore`からデータをシリアル化して、それをクライアントに送信するアプリケーションを保持する属性。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-156">This member uses the `JsonIgnore` attribute to keep the application from serializing the data and sending it back to the client.</span></span>

## <a name="map-to-the-hub-when-app-starts"></a><span data-ttu-id="6fcb1-157">アプリの開始時に、ハブにマップします。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-157">Map to the hub when app starts</span></span>

<span data-ttu-id="6fcb1-158">次に、設定、ハブへのマッピングをアプリケーションの起動時にします。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-158">Next, you set up mapping to the hub when the application starts.</span></span> <span data-ttu-id="6fcb1-159">SignalR 2 で OWIN startup クラスを追加するマッピングを作成します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-159">In SignalR 2, adding an OWIN startup class creates the mapping.</span></span>

1. <span data-ttu-id="6fcb1-160">**ソリューション エクスプ ローラー**プロジェクトを右クリックし、選択、**追加** > **新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-160">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="6fcb1-161">**新しい項目の追加 - MoveShapeDemo**選択**インストール済み** > **Visual C#**   >  **Web**し選択**OWIN Startup クラス**します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-161">In **Add New Item - MoveShapeDemo** select **Installed** > **Visual C#** > **Web** and then select **OWIN Startup Class**.</span></span>

1. <span data-ttu-id="6fcb1-162">クラスの名前*スタートアップ*選択と**OK**します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-162">Name the class *Startup* and select **OK**.</span></span>

1. <span data-ttu-id="6fcb1-163">既定のコードを置き換える、 *Startup.cs*このコード ファイル。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-163">Replace the default code in the *Startup.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.cs)]

<span data-ttu-id="6fcb1-164">OWIN スタートアップ クラスの呼び出し`MapSignalR`、アプリが実行される場合、`Configuration`メソッド。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-164">The OWIN startup class calls `MapSignalR` when the app executes the `Configuration` method.</span></span> <span data-ttu-id="6fcb1-165">OWIN のスタートアップにクラス処理を使用してアプリを追加、`OwinStartup`アセンブリ属性。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-165">The app adds the class to OWIN's startup process using the `OwinStartup` assembly attribute.</span></span>

## <a name="add-the-client"></a><span data-ttu-id="6fcb1-166">クライアントを追加します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-166">Add the client</span></span>

<span data-ttu-id="6fcb1-167">クライアントの HTML ページを追加します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-167">Add the HTML page for the client.</span></span>

1. <span data-ttu-id="6fcb1-168">**ソリューション エクスプ ローラー**プロジェクトを右クリックし、選択、**追加** > **HTML ページ**します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-168">In **Solution Explorer**, right-click the project and select **Add** > **HTML Page**.</span></span>

1. <span data-ttu-id="6fcb1-169">ページの名前を付けます**既定**選択 **[ok]**。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-169">Name the page **Default** and select **OK**.</span></span>

1. <span data-ttu-id="6fcb1-170">**ソリューション エクスプ ローラー**、右クリック*Default.html*選択と**スタート ページとして設定**します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-170">In **Solution Explorer**, right-click *Default.html* and select **Set as Start Page**.</span></span>

1. <span data-ttu-id="6fcb1-171">既定のコードを置き換える、 *Default.html*このコード ファイル。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-171">Replace the default code in the *Default.html* file with this code:</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.html?highlight=14-16)]

1. <span data-ttu-id="6fcb1-172">**ソリューション エクスプ ローラー**、展開**スクリプト**します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-172">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="6fcb1-173">JQuery と SignalR 用のスクリプト ライブラリは、プロジェクトに表示されます。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-173">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="6fcb1-174">パッケージ マネージャーは、以降のバージョンの SignalR スクリプトをインストールします。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-174">The package manager installs a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="6fcb1-175">プロジェクト内のスクリプト ファイルのバージョンに対応するコード ブロック内のスクリプト参照を更新します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-175">Update the script references in the code block to correspond to the versions of the script files in the project.</span></span>

<span data-ttu-id="6fcb1-176">この HTML および JavaScript コードの作成、red`div`と呼ばれる`shape`します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-176">This HTML and JavaScript code creates a red `div` called `shape`.</span></span> <span data-ttu-id="6fcb1-177">JQuery ライブラリを使用して、図形のドラッグ動作を有効にしを使用して、`drag`図形の位置をサーバーに送信するイベントです。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-177">It enables the shape's dragging behavior using the jQuery library and uses the `drag` event to send the shape's position to the server.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="6fcb1-178">アプリを実行する</span><span class="sxs-lookup"><span data-stu-id="6fcb1-178">Run the app</span></span>

<span data-ttu-id="6fcb1-179">アプリを実行する se'e に作動します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-179">You can run the app to se\`e it work.</span></span> <span data-ttu-id="6fcb1-180">ブラウザー ウィンドウを囲む、図形をドラッグすると、図形にも、他のブラウザーで移動します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-180">When you drag the shape around a browser window, the shape moves in the other browsers too.</span></span>

1. <span data-ttu-id="6fcb1-181">ツールバーで、有効にする**スクリプトのデバッグ**し、アプリケーションをデバッグ モードで実行する [再生] ボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-181">In the toolbar, turn on **Script Debugging** and then select the play button to run the application in Debug mode.</span></span>

    ![ユーザー モードをデバッグし、[再生] を選択のスクリーン ショット。](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)

    <span data-ttu-id="6fcb1-183">右上隅の赤色の図形と、ブラウザー ウィンドウが開きます。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-183">A browser window opens with the red shape in the upper-right corner.</span></span>

1. <span data-ttu-id="6fcb1-184">ページの URL をコピーします。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-184">Copy the page's URL.</span></span>

1. <span data-ttu-id="6fcb1-185">別のブラウザーを開き、アドレス バーに URL を貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-185">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="6fcb1-186">ブラウザー ウィンドウのいずれかで、図形をドラッグします。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-186">Drag the shape in one of the browser windows.</span></span> <span data-ttu-id="6fcb1-187">別のブラウザー ウィンドウ内の図形に従います。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-187">The shape in the other browser window follows.</span></span>

<span data-ttu-id="6fcb1-188">アプリケーションの中にこのメソッドを使用して関数は推奨されるプログラミング モデル。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-188">While the application functions using this method, it's not a recommended programming model.</span></span> <span data-ttu-id="6fcb1-189">メッセージの送信先の数に上限はありません。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-189">There's no upper limit to the number of messages getting sent.</span></span> <span data-ttu-id="6fcb1-190">その結果、クライアントとサーバー メッセージを使用した負荷がかかって取得おり、パフォーマンスが低下します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-190">As a result, the clients and server get overwhelmed with messages and performance degrades.</span></span> <span data-ttu-id="6fcb1-191">また、アプリでは、クライアントの不整合なアニメーションを表示します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-191">Also, the app displays a disjointed animation on the client.</span></span> <span data-ttu-id="6fcb1-192">このぎくしゃくしたアニメーションは、各メソッドによって、図形が瞬時に移動するために発生します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-192">This jerky animation happens because the shape moves instantly by each method.</span></span> <span data-ttu-id="6fcb1-193">図形は、それぞれの新しい場所にスムーズに移動しますを用意することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-193">It's better if the shape moves smoothly to each new location.</span></span> <span data-ttu-id="6fcb1-194">次に、こうした問題を修正する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-194">Next, you learn how to fix those issues.</span></span>

## <a name="add-the-client-loop"></a><span data-ttu-id="6fcb1-195">クライアントのループを追加します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-195">Add the client loop</span></span>

<span data-ttu-id="6fcb1-196">すべてのマウス移動イベントに図形の位置を送信するには、ネットワーク トラフィック量が不要なが作成されます。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-196">Sending the location of the shape on every mouse move event creates an unnecessary amount of network traffic.</span></span> <span data-ttu-id="6fcb1-197">アプリは、クライアントからメッセージを抑制する必要があります。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-197">The app needs to throttle the messages from the client.</span></span>

<span data-ttu-id="6fcb1-198">Javascript を使用して、`setInterval`固定の率でサーバーに新しい位置情報を送信するループを設定する関数。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-198">Use the javascript `setInterval` function to set up a loop that sends new position information to the server at a fixed rate.</span></span> <span data-ttu-id="6fcb1-199">このループは「ゲーム ループ」の基本的な表現</span><span class="sxs-lookup"><span data-stu-id="6fcb1-199">This loop is a basic representation of a "game loop."</span></span> <span data-ttu-id="6fcb1-200">ゲームのすべての機能を駆動する繰り返し呼び出された関数になります。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-200">It's a repeatedly called function that drives all the functionality of a game.</span></span>

1. <span data-ttu-id="6fcb1-201">クライアント コードに置き換えます、 *Default.html*このコード ファイル。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-201">Replace the client code in the *Default.html* file with this code:</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.html?highlight=14-16)]

    > [!IMPORTANT]
    > <span data-ttu-id="6fcb1-202">もう一度スクリプト参照を交換する必要があります。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-202">You have to replace the script references again.</span></span> <span data-ttu-id="6fcb1-203">これらは、プロジェクト内のスクリプトのバージョンと一致する必要があります。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-203">They must match the versions of the scripts in the project.</span></span>

    <span data-ttu-id="6fcb1-204">この新しいコードを追加、`updateServerModel`関数。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-204">This new code adds the `updateServerModel` function.</span></span> <span data-ttu-id="6fcb1-205">固定の頻度にこれが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-205">It gets called on a fixed frequency.</span></span> <span data-ttu-id="6fcb1-206">関数は、位置データをサーバーに送信するたびに、`moved`フラグは、送信する新しい位置データがあることを示します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-206">The function sends the position data to the server whenever the `moved` flag indicates that there's new position data to send.</span></span>

1. <span data-ttu-id="6fcb1-207">アプリケーションを起動する再生ボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-207">Select the play button to start the application</span></span>

1. <span data-ttu-id="6fcb1-208">ページの URL をコピーします。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-208">Copy the page's URL.</span></span>

1. <span data-ttu-id="6fcb1-209">別のブラウザーを開き、アドレス バーに URL を貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-209">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="6fcb1-210">ブラウザー ウィンドウのいずれかで、図形をドラッグします。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-210">Drag the shape in one of the browser windows.</span></span> <span data-ttu-id="6fcb1-211">別のブラウザー ウィンドウ内の図形に従います。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-211">The shape in the other browser window follows.</span></span>

<span data-ttu-id="6fcb1-212">アプリは、アニメーションをスムーズに表示されません、サーバーに送信するメッセージの数を調整するため最初でした。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-212">Since the app throttles the number of messages that get sent to the server, the animation won't appear as smooth did at first.</span></span>

## <a name="add-the-server-loop"></a><span data-ttu-id="6fcb1-213">サーバー ループを追加します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-213">Add the server loop</span></span>

<span data-ttu-id="6fcb1-214">現在のアプリケーションで、サーバーからクライアントに送信されるメッセージは受信しているときに多くの場合、移動します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-214">In the current application, messages sent from the server to the client go out as often as they're received.</span></span> <span data-ttu-id="6fcb1-215">このネットワーク トラフィックは、クライアントでわかるとおり、同様の問題を表示します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-215">This network traffic presents a similar problem as we see on the client.</span></span>

<span data-ttu-id="6fcb1-216">アプリでは、必要なときよりもより多くの場合、メッセージを送信できます。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-216">The app can send messages more often than they're needed.</span></span> <span data-ttu-id="6fcb1-217">接続は、その結果大量に送られたなります。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-217">The connection can become flooded as a result.</span></span> <span data-ttu-id="6fcb1-218">このセクションでは、送信メッセージのレートを調整するタイマーを追加するサーバーを更新する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-218">This section describes how to update the server to add a timer that throttles the rate of the outgoing messages.</span></span>

1. <span data-ttu-id="6fcb1-219">内容を置き換える`MoveShapeHub.cs`次のコードで。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-219">Replace the contents of `MoveShapeHub.cs` with this code:</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

1. <span data-ttu-id="6fcb1-220">アプリケーションを起動する再生ボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-220">Select the play button to start the application.</span></span>

1. <span data-ttu-id="6fcb1-221">ページの URL をコピーします。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-221">Copy the page's URL.</span></span>

1. <span data-ttu-id="6fcb1-222">別のブラウザーを開き、アドレス バーに URL を貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-222">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="6fcb1-223">ブラウザー ウィンドウのいずれかで、図形をドラッグします。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-223">Drag the shape in one of the browser windows.</span></span>

<span data-ttu-id="6fcb1-224">このコードを追加するクライアントの展開、`Broadcaster`クラス。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-224">This code expands the client to add the `Broadcaster` class.</span></span> <span data-ttu-id="6fcb1-225">新しいクラスを使用して、送信メッセージの調整、 `Timer` .NET framework からクラス。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-225">The new class throttles the outgoing messages using the `Timer` class from the .NET framework.</span></span>

<span data-ttu-id="6fcb1-226">については、ハブ自体が一時的なものであることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-226">It's good to learn that the hub itself is transitory.</span></span> <span data-ttu-id="6fcb1-227">これが必要になるたびに作成されます。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-227">It's created every time it's needed.</span></span> <span data-ttu-id="6fcb1-228">したがって、アプリを作成し、`Broadcaster`をシングルトンとして。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-228">So the app creates the `Broadcaster` as a singleton.</span></span> <span data-ttu-id="6fcb1-229">遅延初期化を遅延を使用して、`Broadcaster`の作成が必要になるまでです。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-229">It uses lazy initialization to defer the `Broadcaster`'s creation until it's needed.</span></span> <span data-ttu-id="6fcb1-230">確実にタイマーを開始する前にこと、アプリが最初のハブ インスタンスを完全に作成します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-230">That guarantees that the app creates the first hub instance completely before starting the timer.</span></span>

<span data-ttu-id="6fcb1-231">クライアントの呼び出し`UpdateShape`関数は、ハブからは移動し、`UpdateModel`メソッド。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-231">The call to the clients' `UpdateShape` function is then moved out of the hub's `UpdateModel` method.</span></span> <span data-ttu-id="6fcb1-232">アプリが着信メッセージを受信するたびにすぐにできなく呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-232">It's no longer called immediately whenever the app receives incoming messages.</span></span> <span data-ttu-id="6fcb1-233">代わりに、アプリでは、1 秒あたり 25 の呼び出しのレートでクライアントに、メッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-233">Instead, the app sends the messages to the clients at a rate of 25 calls per second.</span></span> <span data-ttu-id="6fcb1-234">によって、プロセスが管理されている、`_broadcastLoop`内からタイマー、`Broadcaster`クラス。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-234">The process is  managed by the `_broadcastLoop` timer from within the `Broadcaster` class.</span></span>

<span data-ttu-id="6fcb1-235">ハブから直接、クライアント メソッドを呼び出すことではなく、最後に、`Broadcaster`クラスは、現在オペレーティング システムへの参照を取得する必要がある`_hubContext`ハブ。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-235">Lastly, instead of calling the client method from the hub directly, the `Broadcaster` class needs to get a reference to the currently operating `_hubContext` hub.</span></span> <span data-ttu-id="6fcb1-236">参照を取得、`GlobalHost`します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-236">It gets the reference with the `GlobalHost`.</span></span>

## <a name="add-smooth-animation"></a><span data-ttu-id="6fcb1-237">滑らかなアニメーションを追加します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-237">Add smooth animation</span></span>

<span data-ttu-id="6fcb1-238">アプリケーションが終了間近には、1 つ以上の向上することもできます。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-238">The application is almost finished, but we could make one more improvement.</span></span> <span data-ttu-id="6fcb1-239">アプリは、クライアント上のサーバー メッセージに応答して、図形を移動します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-239">The app moves the shape on the client in response to server messages.</span></span> <span data-ttu-id="6fcb1-240">サーバーで指定された新しい場所に図形の位置を設定する代わりに、JQuery UI ライブラリを使用して、`animate`関数。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-240">Instead of setting the position of the shape to the new location given by the server, use the JQuery UI library's `animate` function.</span></span> <span data-ttu-id="6fcb1-241">現在および新しい位置の間に滑らかに図形を移動できます。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-241">It can move the shape smoothly between its current and new position.</span></span>

1. <span data-ttu-id="6fcb1-242">クライアントの更新`updateShape`メソッドで、 *Default.html*ファイルを強調表示されたコードのようになります。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-242">Update the client's `updateShape` method in the *Default.html* file to look like the highlighted code:</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.html?highlight=33-40)]

1. <span data-ttu-id="6fcb1-243">アプリケーションを起動する再生ボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-243">Select the play button to start the application.</span></span>

1. <span data-ttu-id="6fcb1-244">ページの URL をコピーします。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-244">Copy the page's URL.</span></span>

1. <span data-ttu-id="6fcb1-245">別のブラウザーを開き、アドレス バーに URL を貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-245">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="6fcb1-246">ブラウザー ウィンドウのいずれかで、図形をドラッグします。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-246">Drag the shape in one of the browser windows.</span></span>

<span data-ttu-id="6fcb1-247">他のウィンドウで、図形の移動が小さい画像が表示されます。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-247">The movement of the shape in the other window appears less jerky.</span></span> <span data-ttu-id="6fcb1-248">アプリでは、受信メッセージごとに 1 回設定されているのではなく、時間経由でその動きを補間します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-248">The app interpolates its movement over time rather than being set once per incoming message.</span></span>

<span data-ttu-id="6fcb1-249">このコードは、元の場所を新しい図形を移動します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-249">This code moves the shape from the old location to the new one.</span></span> <span data-ttu-id="6fcb1-250">サーバーは、アニメーションの間隔の過程で、図形の位置を示します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-250">The server gives the position of the shape over the course of the animation interval.</span></span> <span data-ttu-id="6fcb1-251">この場合、100 ミリ秒です。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-251">In this case, that's 100 milliseconds.</span></span> <span data-ttu-id="6fcb1-252">アプリでは、新しいアニメーションが開始される前に、図形の実行の前のアニメーションをクリアします。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-252">The app clears any previous animation running on the shape before the new animation starts.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="6fcb1-253">コードを取得する</span><span class="sxs-lookup"><span data-stu-id="6fcb1-253">Get the code</span></span>

[<span data-ttu-id="6fcb1-254">完成したプロジェクトのダウンロード</span><span class="sxs-lookup"><span data-stu-id="6fcb1-254">Download Completed Project</span></span>](http://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)

## <a name="additional-resources"></a><span data-ttu-id="6fcb1-255">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="6fcb1-255">Additional resources</span></span>

<span data-ttu-id="6fcb1-256">学習した通信パラダイムはこのようなオンライン ゲームとその他のシミュレーションを開発するために便利です[SignalR を使って作成された ShootR ゲーム](https://shootr.azurewebsites.net/)します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-256">The communication paradigm you just learned about is useful for developing online games and other simulations, like [the ShootR game created with SignalR](https://shootr.azurewebsites.net/).</span></span>

<span data-ttu-id="6fcb1-257">詳細については、SignalR は、次のリソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-257">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="6fcb1-258">SignalR プロジェクト</span><span class="sxs-lookup"><span data-stu-id="6fcb1-258">SignalR Project</span></span>](http://signalr.net)

* [<span data-ttu-id="6fcb1-259">SignalR GitHub とサンプル</span><span class="sxs-lookup"><span data-stu-id="6fcb1-259">SignalR GitHub and Samples</span></span>](https://github.com/SignalR/SignalR)

* [<span data-ttu-id="6fcb1-260">SignalR の Wiki</span><span class="sxs-lookup"><span data-stu-id="6fcb1-260">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="6fcb1-261">次の手順</span><span class="sxs-lookup"><span data-stu-id="6fcb1-261">Next steps</span></span>

<span data-ttu-id="6fcb1-262">このチュートリアルでは、次の作業を行いました。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-262">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6fcb1-263">プロジェクトを設定します。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-263">Set up the project</span></span>
> * <span data-ttu-id="6fcb1-264">ベースのアプリケーションの作成</span><span class="sxs-lookup"><span data-stu-id="6fcb1-264">Created the base application</span></span>
> * <span data-ttu-id="6fcb1-265">アプリの起動時に、ハブにマップ</span><span class="sxs-lookup"><span data-stu-id="6fcb1-265">Mapped to the hub when app starts</span></span>
> * <span data-ttu-id="6fcb1-266">クライアントの追加</span><span class="sxs-lookup"><span data-stu-id="6fcb1-266">Added the client</span></span>
> * <span data-ttu-id="6fcb1-267">アプリの実行</span><span class="sxs-lookup"><span data-stu-id="6fcb1-267">Ran the app</span></span>
> * <span data-ttu-id="6fcb1-268">クライアントのループの追加</span><span class="sxs-lookup"><span data-stu-id="6fcb1-268">Added the client loop</span></span>
> * <span data-ttu-id="6fcb1-269">サーバー ループの追加</span><span class="sxs-lookup"><span data-stu-id="6fcb1-269">Added the server loop</span></span>
> * <span data-ttu-id="6fcb1-270">滑らかなアニメーションを追加しました</span><span class="sxs-lookup"><span data-stu-id="6fcb1-270">Added smooth animation</span></span>

<span data-ttu-id="6fcb1-271">ASP.NET SignalR 2 を使用してサーバー ブロードキャストの機能を提供する web アプリケーションを作成する方法については、次の記事に進んでください。</span><span class="sxs-lookup"><span data-stu-id="6fcb1-271">Advance to the next article to learn how to create a web application that uses ASP.NET SignalR 2 to provide server broadcast functionality.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="6fcb1-272">SignalR 2 とサーバー ブロードキャスト</span><span class="sxs-lookup"><span data-stu-id="6fcb1-272">SignalR 2 and server broadcasting</span></span>](tutorial-server-broadcast-with-signalr.md)