---
uid: signalr/overview/older-versions/tutorial-high-frequency-realtime-with-signalr
title: SignalR の頻度が高いリアルタイム 1.x |Microsoft ドキュメント
author: pfletcher
description: このチュートリアルでは、ASP.NET SignalR を使用して、頻度が高いメッセージング機能を提供する web アプリケーションを作成する方法を示します。 高周波数のメッセージングには.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/16/2013
ms.topic: article
ms.assetid: ad2a5da5-2e79-40ea-bc84-028d327f5982
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 0c680a7d8b911b2734647948b683d5ff6e47aec4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
ms.locfileid: "26505841"
---
<a name="high-frequency-realtime-with-signalr-1x"></a><span data-ttu-id="acf89-104">SignalR の頻度が高いリアルタイム 1.x</span><span class="sxs-lookup"><span data-stu-id="acf89-104">High-Frequency Realtime with SignalR 1.x</span></span>
====================
<span data-ttu-id="acf89-105">によって[Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="acf89-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="acf89-106">このチュートリアルでは、ASP.NET SignalR を使用して、頻度が高いメッセージング機能を提供する web アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="acf89-106">This tutorial shows how to create a web application that uses ASP.NET SignalR to provide high-frequency messaging functionality.</span></span> <span data-ttu-id="acf89-107">高周波数のメッセージング、固定の率で送信される更新プログラムは、ここではこのアプリケーションの場合は、最大で 10 は、2 つ目をメッセージです。</span><span class="sxs-lookup"><span data-stu-id="acf89-107">High-frequency messaging in this case means updates that are sent at a fixed rate; in the case of this application, up to 10 messages a second.</span></span>
> 
> <span data-ttu-id="acf89-108">このチュートリアルで作成したアプリケーションには、ユーザーがドラッグできる図形が表示されます。</span><span class="sxs-lookup"><span data-stu-id="acf89-108">The application you'll create in this tutorial displays a shape that users can drag.</span></span> <span data-ttu-id="acf89-109">接続されている他のすべてのブラウザーでの図形の位置は、時間指定の更新プログラムを使用して、ドラッグした図形の位置に一致するように更新されます。</span><span class="sxs-lookup"><span data-stu-id="acf89-109">The position of the shape in all other connected browsers will then be updated to match the position of the dragged shape using timed updates.</span></span>
> 
> <span data-ttu-id="acf89-110">このチュートリアルで導入された概念には、リアルタイムのゲーム内のアプリケーションおよびその他のシミュレーションのアプリケーションがあります。</span><span class="sxs-lookup"><span data-stu-id="acf89-110">Concepts introduced in this tutorial have applications in real-time gaming and other simulation applications.</span></span>
> 
> <span data-ttu-id="acf89-111">チュートリアルのコメントは歓迎します。</span><span class="sxs-lookup"><span data-stu-id="acf89-111">Comments on the tutorial are welcome.</span></span> <span data-ttu-id="acf89-112">チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com)です。</span><span class="sxs-lookup"><span data-stu-id="acf89-112">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com).</span></span>


## <a name="overview"></a><span data-ttu-id="acf89-113">概要</span><span class="sxs-lookup"><span data-stu-id="acf89-113">Overview</span></span>

<span data-ttu-id="acf89-114">このチュートリアルでは、リアルタイムで他のブラウザーとオブジェクトの状態を共有しているアプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="acf89-114">This tutorial demonstrates how to create an application that shares the state of an object with other browsers in real time.</span></span> <span data-ttu-id="acf89-115">作成したアプリケーションには、MoveShape が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="acf89-115">The application we'll create is called MoveShape.</span></span> <span data-ttu-id="acf89-116">ユーザーがドラッグできる; HTML Div 要素を MoveShape ページが表示されます。ドラッグすると、ユーザー、Div の新しい位置に一致する図形の位置を更新するその他のすべての接続しているクライアントに指示しは、サーバーに送信されます。</span><span class="sxs-lookup"><span data-stu-id="acf89-116">The MoveShape page will display an HTML Div element that the user can drag; when the user drags the Div, its new position will be sent to the server, which will then tell all other connected clients to update the shape's position to match.</span></span>

![アプリケーション ウィンドウ](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

<span data-ttu-id="acf89-118">このチュートリアルで作成したアプリケーションは、Damian Edwards によるデモに基づいています。</span><span class="sxs-lookup"><span data-stu-id="acf89-118">The application created in this tutorial is based on a demo by Damian Edwards.</span></span> <span data-ttu-id="acf89-119">このデモを格納しているビデオがわかるように[ここ](https://channel9.msdn.com/Series/Building-Web-Apps-with-ASP-NET-Jump-Start/Building-Web-Apps-with-ASPNET-Jump-Start-08-Real-time-Communication-with-SignalR)です。</span><span class="sxs-lookup"><span data-stu-id="acf89-119">A video containing this demo can be seen [here](https://channel9.msdn.com/Series/Building-Web-Apps-with-ASP-NET-Jump-Start/Building-Web-Apps-with-ASPNET-Jump-Start-08-Real-time-Communication-with-SignalR).</span></span>

<span data-ttu-id="acf89-120">このチュートリアルは、各発生するイベントにドラッグしたときに、その形状から SignalR メッセージを送信する方法のデモで開始されます。</span><span class="sxs-lookup"><span data-stu-id="acf89-120">The tutorial will start by demonstrating how to send SignalR messages from each event that fires as the shape is dragged.</span></span> <span data-ttu-id="acf89-121">接続されている各クライアントは、メッセージを受信するたびに、図形のローカル バージョンの位置を更新されます。</span><span class="sxs-lookup"><span data-stu-id="acf89-121">Each connected client will then update the position of the local version of the shape each time a message is received.</span></span>

<span data-ttu-id="acf89-122">このメソッドを使用して、アプリケーションが機能がこれはできません、推奨されるプログラミング モデルがあるためありません、クライアントとサーバーが取得集中してパンク状態メッセージと、パフォーマンスが低下するので、送信先のメッセージの数に上限を設定.</span><span class="sxs-lookup"><span data-stu-id="acf89-122">While the application will function using this method, this is not a recommended programming model, since there would be no upper limit to the number of messages getting sent, so the clients and server could get overwhelmed with messages and performance would degrade.</span></span> <span data-ttu-id="acf89-123">クライアントで表示されているアニメーションは場所ごとにスムーズに移行するのではなく、図形は、各方法に瞬時に移動するは、不整合にもなります。</span><span class="sxs-lookup"><span data-stu-id="acf89-123">The displayed animation on the client would also be disjointed, as the shape would be moved instantly by each method, rather than moving smoothly to each new location.</span></span> <span data-ttu-id="acf89-124">このチュートリアルの以降のセクションは、クライアントまたはサーバーがメッセージを送信する最大転送率を制限するタイマー関数を作成する方法と場所の間での図形をスムーズに移動する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="acf89-124">Later sections of the tutorial will demonstrate how to create a timer function that restricts the maximum rate at which messages are sent by either the client or server, and how to move the shape smoothly between locations.</span></span> <span data-ttu-id="acf89-125">このチュートリアルで作成したアプリケーションの最終バージョンからダウンロードできます[コード ギャラリー](https://code.msdn.microsoft.com/SignalR-MoveShape-demo-3366dac6)です。</span><span class="sxs-lookup"><span data-stu-id="acf89-125">The final version of the application created in this tutorial can be downloaded from [Code Gallery](https://code.msdn.microsoft.com/SignalR-MoveShape-demo-3366dac6).</span></span>

<span data-ttu-id="acf89-126">このチュートリアルでは、次のセクションでは、含まれています。</span><span class="sxs-lookup"><span data-stu-id="acf89-126">This tutorial contains the following sections:</span></span>

- [<span data-ttu-id="acf89-127">前提条件</span><span class="sxs-lookup"><span data-stu-id="acf89-127">Prerequisites</span></span>](#prerequisites)
- [<span data-ttu-id="acf89-128">プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="acf89-128">Create the project</span></span>](#createtheproject)
- [<span data-ttu-id="acf89-129">ASP.NET SignalR と JQuery.UI NuGet パッケージを追加します。</span><span class="sxs-lookup"><span data-stu-id="acf89-129">Add the ASP.NET SignalR and JQuery.UI NuGet packages</span></span>](#nugetpackages)
- [<span data-ttu-id="acf89-130">ベースのアプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="acf89-130">Create the base application</span></span>](#baseapp)
- [<span data-ttu-id="acf89-131">クライアント ループに追加します。</span><span class="sxs-lookup"><span data-stu-id="acf89-131">Add the client loop</span></span>](#clientloop)
- [<span data-ttu-id="acf89-132">サーバーのループを追加します。</span><span class="sxs-lookup"><span data-stu-id="acf89-132">Add the server loop</span></span>](#serverloop)
- [<span data-ttu-id="acf89-133">クライアントで滑らかなアニメーションを追加します。</span><span class="sxs-lookup"><span data-stu-id="acf89-133">Add smooth animation on the client</span></span>](#animation)
- [<span data-ttu-id="acf89-134">以降の手順</span><span class="sxs-lookup"><span data-stu-id="acf89-134">Further Steps</span></span>](#furthersteps)

<a id="prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="acf89-135">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="acf89-135">Prerequisites</span></span>

<span data-ttu-id="acf89-136">このチュートリアルでは、Visual Studio 2012 または Visual Studio 2010 が必要です。</span><span class="sxs-lookup"><span data-stu-id="acf89-136">This tutorial requires Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="acf89-137">Visual Studio 2010 を使用すると、プロジェクトでは、.NET Framework 4.5 ではなく、.NET Framework 4 を使用します。</span><span class="sxs-lookup"><span data-stu-id="acf89-137">If Visual Studio 2010 is used, the project will use .NET Framework 4 rather than .NET Framework 4.5.</span></span>

<span data-ttu-id="acf89-138">Visual Studio 2012 を使用している場合は、インストールすることをお勧めしますが、 [ASP.NET および Web ツール 2012.2 更新](https://go.microsoft.com/fwlink/?LinkId=282650)です。</span><span class="sxs-lookup"><span data-stu-id="acf89-138">If you are using Visual Studio 2012, it's recommended that you install the [ASP.NET and Web Tools 2012.2 update](https://go.microsoft.com/fwlink/?LinkId=282650).</span></span> <span data-ttu-id="acf89-139">この更新プログラムには、発行する、新しい機能の強化などの新機能が含まれています。 新しいテンプレートとします。</span><span class="sxs-lookup"><span data-stu-id="acf89-139">This update contains new features such as enhancements to publishing, new functionality, and new templates.</span></span>

<span data-ttu-id="acf89-140">Visual Studio 2010 を使っている場合、以下のことを確認[NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)がインストールされています。</span><span class="sxs-lookup"><span data-stu-id="acf89-140">If you have Visual Studio 2010, make sure that [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) is installed.</span></span>

<a id="createtheproject"></a>

## <a name="create-the-project"></a><span data-ttu-id="acf89-141">プロジェクトの作成</span><span class="sxs-lookup"><span data-stu-id="acf89-141">Create the project</span></span>

<span data-ttu-id="acf89-142">このセクションでは、Visual Studio でプロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="acf89-142">In this section, we'll create the project in Visual Studio.</span></span>

1. <span data-ttu-id="acf89-143">**ファイル**ボタンをクリックし**新しいプロジェクト**です。</span><span class="sxs-lookup"><span data-stu-id="acf89-143">From the **File** menu click **New Project**.</span></span>
2. <span data-ttu-id="acf89-144">**新しいプロジェクト**] ダイアログ ボックスで、展開**c#** [**テンプレート**選択と**Web**です。</span><span class="sxs-lookup"><span data-stu-id="acf89-144">In the **New Project** dialog box, expand **C#** under **Templates** and select **Web**.</span></span>
3. <span data-ttu-id="acf89-145">選択、**空の ASP.NET Web アプリケーション**名では、プロジェクト テンプレートは、 *MoveShapeDemo*、 をクリック**OK**です。</span><span class="sxs-lookup"><span data-stu-id="acf89-145">Select the **ASP.NET Empty Web Application** template, name the project *MoveShapeDemo*, and click **OK**.</span></span>

    ![新しいプロジェクトの作成](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)

<a id="nugetpackages"></a>

## <a name="add-the-signalr-and-jqueryui-nuget-packages"></a><span data-ttu-id="acf89-147">SignalR と JQuery.UI NuGet パッケージを追加します。</span><span class="sxs-lookup"><span data-stu-id="acf89-147">Add the SignalR and JQuery.UI NuGet Packages</span></span>

<span data-ttu-id="acf89-148">SignalR 機能は、NuGet パッケージをインストールしてプロジェクトに追加できます。</span><span class="sxs-lookup"><span data-stu-id="acf89-148">You can add SignalR functionality to a project by installing a NuGet package.</span></span> <span data-ttu-id="acf89-149">このチュートリアルは、図形をドラッグし、アニメーション化を許可する場合に、JQuery.UI パッケージを使用してもします。</span><span class="sxs-lookup"><span data-stu-id="acf89-149">This tutorial will also use the JQuery.UI package for allowing the shape to be dragged and animated.</span></span>

1. <span data-ttu-id="acf89-150">をクリックして**ツール |ライブラリ パッケージ マネージャー |パッケージ マネージャー コンソール**です。</span><span class="sxs-lookup"><span data-stu-id="acf89-150">Click **Tools | Library Package Manager | Package Manager Console**.</span></span>
2. <span data-ttu-id="acf89-151">パッケージ マネージャーで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="acf89-151">Enter the following command in the package manager.</span></span>

    [!code-powershell[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.ps1)]

    <span data-ttu-id="acf89-152">SignalR パッケージは、依存関係として、他の NuGet パッケージの数をインストールします。</span><span class="sxs-lookup"><span data-stu-id="acf89-152">The SignalR package installs a number of other NuGet packages as dependencies.</span></span> <span data-ttu-id="acf89-153">インストールが完了するとすべての SignalR を ASP.NET アプリケーションで使用するために必要なサーバーとクライアント コンポーネントがあります。</span><span class="sxs-lookup"><span data-stu-id="acf89-153">When the installation is finished you have all of the server and client components required to use SignalR in an ASP.NET application.</span></span>
3. <span data-ttu-id="acf89-154">JQuery、および JQuery.UI パッケージをインストールするパッケージ マネージャー コンソールに次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="acf89-154">Enter the following command into the package manager console to install the JQuery and JQuery.UI packages.</span></span>

    [!code-powershell[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.ps1)]

<a id="baseapp"></a>

## <a name="create-the-base-application"></a><span data-ttu-id="acf89-155">ベースのアプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="acf89-155">Create the base application</span></span>

<span data-ttu-id="acf89-156">このセクションの各マウス移動イベント中に、サーバーに、図形の場所を送信するブラウザー アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="acf89-156">In this section, we'll create a browser application that sends the location of the shape to the server during each mouse move event.</span></span> <span data-ttu-id="acf89-157">受信すると、サーバーは、接続されている他のすべてのクライアントにこの情報をブロードキャストします。</span><span class="sxs-lookup"><span data-stu-id="acf89-157">The server then broadcasts this information to all other connected clients as it is received.</span></span> <span data-ttu-id="acf89-158">以降のセクションでは、このアプリケーションに展開してあります。</span><span class="sxs-lookup"><span data-stu-id="acf89-158">We'll expand on this application in later sections.</span></span>

1. <span data-ttu-id="acf89-159">**ソリューション エクスプ ローラー**プロジェクトを右クリックし、選択、**追加**、**クラス.**.クラスの名前を付けます**MoveShapeHub**  をクリック**追加**です。</span><span class="sxs-lookup"><span data-stu-id="acf89-159">In **Solution Explorer**, right-click on the project and select **Add**, **Class...**. Name the class **MoveShapeHub** and click **Add**.</span></span>
2. <span data-ttu-id="acf89-160">新しいコードを置き換える**MoveShapeHub**クラスを次のコード。</span><span class="sxs-lookup"><span data-stu-id="acf89-160">Replace the code in the new **MoveShapeHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.cs)]

    <span data-ttu-id="acf89-161">`MoveShapeHub`上記のクラスは、SignalR ハブの実装です。</span><span class="sxs-lookup"><span data-stu-id="acf89-161">The `MoveShapeHub` class above is an implementation of a SignalR hub.</span></span> <span data-ttu-id="acf89-162">同様、 [SignalR の概要](index.md)チュートリアルでは、ハブに直接に、クライアントが呼び出すメソッドがあります。</span><span class="sxs-lookup"><span data-stu-id="acf89-162">As in the [Getting Started with SignalR](index.md) tutorial, the hub has a method that the clients will call directly.</span></span> <span data-ttu-id="acf89-163">ここでは、クライアントが新しいを格納するオブジェクトを送信図形を接続している他のすべてのクライアントにブロードキャストを取得し、サーバーの X および Y 座標。</span><span class="sxs-lookup"><span data-stu-id="acf89-163">In this case, the client will send an object containing the new X and Y coordinates of the shape to the server, which then gets broadcasted to all other connected clients.</span></span> <span data-ttu-id="acf89-164">SignalR には、JSON を使用してこのオブジェクトは自動的にシリアル化します。</span><span class="sxs-lookup"><span data-stu-id="acf89-164">SignalR will automatically serialize this object using JSON.</span></span>

    <span data-ttu-id="acf89-165">クライアントに送信されるオブジェクト (`ShapeModel`) 図形の位置を格納するメンバーが含まれます。</span><span class="sxs-lookup"><span data-stu-id="acf89-165">The object that will be sent to the client (`ShapeModel`) contains members to store the position of the shape.</span></span> <span data-ttu-id="acf89-166">サーバー上のオブジェクトのバージョンも含まれていますを追跡するにはメンバーをクライアントのデータが格納される、特定のクライアントがしない独自のデータを送信できるようにします。</span><span class="sxs-lookup"><span data-stu-id="acf89-166">The version of the object on the server also contains a member to track which client's data is being stored, so that a given client won't be sent their own data.</span></span> <span data-ttu-id="acf89-167">このメンバーは使用して、`JsonIgnore`にならないようにシリアル化され、クライアントに送信される属性です。</span><span class="sxs-lookup"><span data-stu-id="acf89-167">This member uses the `JsonIgnore` attribute to keep it from being serialized and sent to the client.</span></span>
3. <span data-ttu-id="acf89-168">次に、アプリケーションの起動時に、ハブを設定します。</span><span class="sxs-lookup"><span data-stu-id="acf89-168">Next, we'll set up the hub when the application starts.</span></span> <span data-ttu-id="acf89-169">**ソリューション エクスプ ローラー**プロジェクトを右クリックし、クリックして**追加 |グローバル アプリケーション クラス**です。</span><span class="sxs-lookup"><span data-stu-id="acf89-169">In **Solution Explorer**, right-click the project, then click **Add | Global Application Class**.</span></span> <span data-ttu-id="acf89-170">既定の名前を受け入れる*グローバル* をクリック**OK**です。</span><span class="sxs-lookup"><span data-stu-id="acf89-170">Accept the default name of *Global* and click **OK**.</span></span>

    ![グローバル アプリケーション クラスを追加します。](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)
4. <span data-ttu-id="acf89-172">次の追加`using`、提供された後のステートメント**を使用して**Global.asax.cs クラス内のステートメント。</span><span class="sxs-lookup"><span data-stu-id="acf89-172">Add the following `using` statement after the provided **using** statements in the Global.asax.cs class.</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.cs)]
5. <span data-ttu-id="acf89-173">次のコードの行を追加、 `Application_Start` SignalR の既定のルートを登録するグローバル クラスのメソッドです。</span><span class="sxs-lookup"><span data-stu-id="acf89-173">Add the following line of code in the `Application_Start` method of the Global class to register the default route for SignalR.</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

    <span data-ttu-id="acf89-174">Global.asax は、ファイルは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="acf89-174">Your global.asax file should look like the following:</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.cs)]
6. <span data-ttu-id="acf89-175">次に、クライアントを追加します。</span><span class="sxs-lookup"><span data-stu-id="acf89-175">Next, we'll add the client.</span></span> <span data-ttu-id="acf89-176">**ソリューション エクスプ ローラー**プロジェクトを右クリックし、クリックして**追加 |新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="acf89-176">In **Solution Explorer**, right-click the project, then click **Add | New Item**.</span></span> <span data-ttu-id="acf89-177">**新しい項目の追加**ダイアログで、 **Html ページ**です。</span><span class="sxs-lookup"><span data-stu-id="acf89-177">In the **Add New Item** dialog, select **Html Page**.</span></span> <span data-ttu-id="acf89-178">ページの適切な名前を付けます (と同様に**Default.html**) をクリック**追加**です。</span><span class="sxs-lookup"><span data-stu-id="acf89-178">Give the page an appropriate name (like **Default.html**) and click **Add**.</span></span>
7. <span data-ttu-id="acf89-179">**ソリューション エクスプ ローラー**を作成したページを右クリックし、をクリックして**スタート ページとして設定**です。</span><span class="sxs-lookup"><span data-stu-id="acf89-179">In **Solution Explorer**, right-click the page you just created and click **Set as Start Page**.</span></span>
8. <span data-ttu-id="acf89-180">次のコード スニペットでは、HTML ページで、既定のコードを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="acf89-180">Replace the default code in the HTML page with the following code snippet.</span></span>

    > [!NOTE]
    > <span data-ttu-id="acf89-181">スクリプトを参照している一致の下の Scripts フォルダーでプロジェクトに追加されたパッケージを確認します。</span><span class="sxs-lookup"><span data-stu-id="acf89-181">Verify that the script references below match the packages added to your project in the Scripts folder.</span></span> <span data-ttu-id="acf89-182">Visual Studio 2010 でと JQuery、および SignalR プロジェクトに追加のバージョンでは次のバージョン番号は一致しない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="acf89-182">In Visual Studio 2010, the version of JQuery and SignalR added to the project may not match the version numbers below.</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample7.html)]

    <span data-ttu-id="acf89-183">図形と呼ばれる赤い Div を作成、jQuery ライブラリを使用して、図形のドラッグ動作を有効にし、図形の使用上の HTML および JavaScript コード`drag`図形の位置をサーバーに送信するイベントです。</span><span class="sxs-lookup"><span data-stu-id="acf89-183">The above HTML and JavaScript code creates a red Div called Shape, enables the shape's dragging behavior using the jQuery library, and uses the shape's `drag` event to send the shape's position to the server.</span></span>
9. <span data-ttu-id="acf89-184">F5 キーを押して、アプリケーションを起動します。</span><span class="sxs-lookup"><span data-stu-id="acf89-184">Start the application by pressing F5.</span></span> <span data-ttu-id="acf89-185">ページの URL をコピーし、2 番目のブラウザー ウィンドウに貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="acf89-185">Copy the page's URL, and paste it into a second browser window.</span></span> <span data-ttu-id="acf89-186">ブラウザーのウィンドウのいずれかで図形をドラッグしてください。他のブラウザー ウィンドウで、図形を移動する必要があります。</span><span class="sxs-lookup"><span data-stu-id="acf89-186">Drag the shape in one of the browser windows; the shape in the other browser window should move.</span></span>

    ![アプリケーション ウィンドウ](tutorial-high-frequency-realtime-with-signalr/_static/image4.png)

<a id="clientloop"></a>

## <a name="add-the-client-loop"></a><span data-ttu-id="acf89-188">クライアント ループに追加します。</span><span class="sxs-lookup"><span data-stu-id="acf89-188">Add the client loop</span></span>

<span data-ttu-id="acf89-189">すべてのマウス移動イベントで図形の場所を送信すると、ネットワーク トラフィック量が不要なを作成するため、クライアントからのメッセージを調整する必要があります。</span><span class="sxs-lookup"><span data-stu-id="acf89-189">Since sending the location of the shape on every mouse move event will create an unneccesary amount of network traffic, the messages from the client need to be throttled.</span></span> <span data-ttu-id="acf89-190">Javascript を使用して`setInterval`固定の率でサーバーに新しい位置情報を送信するループを設定する関数。</span><span class="sxs-lookup"><span data-stu-id="acf89-190">We'll use the javascript `setInterval` function to set up a loop that sends new position information to the server at a fixed rate.</span></span> <span data-ttu-id="acf89-191">このループは、「ゲーム ループ」、ドライブのすべてのゲームやその他のシミュレーションの機能を繰り返し呼び出された関数の非常に基本的な表現です。</span><span class="sxs-lookup"><span data-stu-id="acf89-191">This loop is a very basic representation of a "game loop", a repeatedly called function that drives all of the functionality of a game or other simulation.</span></span>

1. <span data-ttu-id="acf89-192">次のコード スニペットを一致するように HTML ページ内のクライアント コードを更新します。</span><span class="sxs-lookup"><span data-stu-id="acf89-192">Update the client code in the HTML page to match the following code snippet.</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample8.html)]

    <span data-ttu-id="acf89-193">上記の更新プログラムの追加、`updateServerModel`固定周波数に呼び出される関数。</span><span class="sxs-lookup"><span data-stu-id="acf89-193">The above update adds the `updateServerModel` function, which gets called on a fixed frequency.</span></span> <span data-ttu-id="acf89-194">この関数は、サーバーに、位置データを送信するたびに、`moved`フラグが示される位置に新しいデータを送信します。</span><span class="sxs-lookup"><span data-stu-id="acf89-194">This function sends the position data to the server whenever the `moved` flag indicates that there is new position data to send.</span></span>
2. <span data-ttu-id="acf89-195">F5 キーを押して、アプリケーションを起動します。</span><span class="sxs-lookup"><span data-stu-id="acf89-195">Start the application by pressing F5.</span></span> <span data-ttu-id="acf89-196">ページの URL をコピーし、2 番目のブラウザー ウィンドウに貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="acf89-196">Copy the page's URL, and paste it into a second browser window.</span></span> <span data-ttu-id="acf89-197">ブラウザーのウィンドウのいずれかで図形をドラッグしてください。他のブラウザー ウィンドウで、図形を移動する必要があります。</span><span class="sxs-lookup"><span data-stu-id="acf89-197">Drag the shape in one of the browser windows; the shape in the other browser window should move.</span></span> <span data-ttu-id="acf89-198">サーバーに送信するメッセージの数を調整するため、アニメーションは、前のセクションと同様に円滑表示されません。</span><span class="sxs-lookup"><span data-stu-id="acf89-198">Since the number of messages that get sent to the server will be throttled, the animation will not appear as smooth as in the previous section.</span></span>

    ![アプリケーション ウィンドウ](tutorial-high-frequency-realtime-with-signalr/_static/image5.png)

<a id="serverloop"></a>

## <a name="add-the-server-loop"></a><span data-ttu-id="acf89-200">サーバーのループを追加します。</span><span class="sxs-lookup"><span data-stu-id="acf89-200">Add the server loop</span></span>

<span data-ttu-id="acf89-201">現在のアプリケーションで、サーバーからクライアントに送信されたメッセージは受信するときに多くの場合、移動します。</span><span class="sxs-lookup"><span data-stu-id="acf89-201">In the current application, messages sent from the server to the client go out as often as they are received.</span></span> <span data-ttu-id="acf89-202">クライアント上で見つかりましたが、この場合は、同様の問題メッセージが必要であれば、および接続状態になるあふれたその結果よりも頻繁に送信できます。</span><span class="sxs-lookup"><span data-stu-id="acf89-202">This presents a similar problem as was seen on the client; messages can be sent more often than they are needed, and the connection could become flooded as a result.</span></span> <span data-ttu-id="acf89-203">このセクションでは、送信メッセージのレートを調整するタイマーを実装するサーバーを更新する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="acf89-203">This section describes how to update the server to implement a timer that throttles the rate of the outgoing messages.</span></span>

1. <span data-ttu-id="acf89-204">内容を置き換える`MoveShapeHub.cs`次のコード スニペットとします。</span><span class="sxs-lookup"><span data-stu-id="acf89-204">Replace the contents of `MoveShapeHub.cs` with the following code snippet.</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample9.cs)]

    <span data-ttu-id="acf89-205">上記のコードを追加するクライアントの展開、`Broadcaster`クラスは、調整して、送信メッセージを使用して、 `Timer` .NET framework からクラスです。</span><span class="sxs-lookup"><span data-stu-id="acf89-205">The above code expands the client to add the `Broadcaster` class, which throttles the outgoing messages using the `Timer` class from the .NET framework.</span></span>

    <span data-ttu-id="acf89-206">ハブ自体が一時的なものから (は、その作成が必要なたびに)、`Broadcaster`はシングルトンとして作成されます。</span><span class="sxs-lookup"><span data-stu-id="acf89-206">Since the hub itself is transitory (it is created every time it is needed), the `Broadcaster` will be created as a singleton.</span></span> <span data-ttu-id="acf89-207">(.NET 4 で導入) 限定的な初期化を使用して、作成を遅らせる必要になるまでは、タイマーが開始される前に、最初のハブ インスタンスが完全に作成されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="acf89-207">Lazy initialization (introduced in .NET 4) is used to defer its creation until it is needed, ensuring that the first hub instance is completely created before the timer is started.</span></span>

    <span data-ttu-id="acf89-208">クライアントへの呼び出し`UpdateShape`関数は、ハブの外移動`UpdateModel`メソッド、受信メッセージが受信されたときにすぐにその it が呼び出されないようにします。</span><span class="sxs-lookup"><span data-stu-id="acf89-208">The call to the clients' `UpdateShape` function is then moved out of the hub's `UpdateModel` method, so that it is no longer called immediately whenever incoming messages are received.</span></span> <span data-ttu-id="acf89-209">25 の呼び出しの 1 秒あたりのレートでクライアントにメッセージの送信される代わりに、によって管理される、`_broadcastLoop`内からタイマー、`Broadcaster`クラスです。</span><span class="sxs-lookup"><span data-stu-id="acf89-209">Instead, the messages to the clients will be sent at a rate of 25 calls per second, managed by the `_broadcastLoop` timer from within the `Broadcaster` class.</span></span>

    <span data-ttu-id="acf89-210">ハブから直接クライアント メソッドを呼び出すのではなく、最後に、`Broadcaster`クラスは、現在の作業ハブへの参照を取得する必要があります (`_hubContext`) を使用して、`GlobalHost`です。</span><span class="sxs-lookup"><span data-stu-id="acf89-210">Lastly, instead of calling the client method from the hub directly, the `Broadcaster` class needs to obtain a reference to the currently operating hub (`_hubContext`) using the `GlobalHost`.</span></span>
2. <span data-ttu-id="acf89-211">F5 キーを押して、アプリケーションを起動します。</span><span class="sxs-lookup"><span data-stu-id="acf89-211">Start the application by pressing F5.</span></span> <span data-ttu-id="acf89-212">ページの URL をコピーし、2 番目のブラウザー ウィンドウに貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="acf89-212">Copy the page's URL, and paste it into a second browser window.</span></span> <span data-ttu-id="acf89-213">ブラウザーのウィンドウのいずれかで図形をドラッグしてください。他のブラウザー ウィンドウで、図形を移動する必要があります。</span><span class="sxs-lookup"><span data-stu-id="acf89-213">Drag the shape in one of the browser windows; the shape in the other browser window should move.</span></span> <span data-ttu-id="acf89-214">前のセクションからブラウザーで表示される相違点されませんが、クライアントに送信するメッセージの数を調整します。</span><span class="sxs-lookup"><span data-stu-id="acf89-214">There will not be a visible difference in the browser from the previous section, but the number of messages that get sent to the client will be throttled.</span></span>

    ![アプリケーション ウィンドウ](tutorial-high-frequency-realtime-with-signalr/_static/image6.png)

<a id="animation"></a>

## <a name="add-smooth-animation-on-the-client"></a><span data-ttu-id="acf89-216">クライアントで滑らかなアニメーションを追加します。</span><span class="sxs-lookup"><span data-stu-id="acf89-216">Add smooth animation on the client</span></span>

<span data-ttu-id="acf89-217">アプリケーションがほぼ完了しましたが、1 つ以上のパフォーマンス向上、クライアントで図形を描くようにサーバー メッセージに応答を移動するときにすることもできます。</span><span class="sxs-lookup"><span data-stu-id="acf89-217">The application is almost complete, but we could make one more improvement, in the motion of the shape on the client as it is moved in response to server messages.</span></span> <span data-ttu-id="acf89-218">サーバーで指定された、新しい場所への図形の位置を設定するのではなく、JQuery UI ライブラリを使用`animate`の現在と新しい位置の間でスムーズに図形を移動する関数。</span><span class="sxs-lookup"><span data-stu-id="acf89-218">Rather than setting the position of the shape to the new location given by the server, we'll use the JQuery UI library's `animate` function to move the shape smoothly between its current and new position.</span></span>

1. <span data-ttu-id="acf89-219">クライアントの更新`updateShape`を検索するメソッドなどの次の強調表示されたコード。</span><span class="sxs-lookup"><span data-stu-id="acf89-219">Update the client's `updateShape` method to look like the highlighted code below:</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample10.html?highlight=35-42)]

    <span data-ttu-id="acf89-220">上記のコードを新しいアニメーション間隔 (ここでは、100 ミリ秒) の過程で、サーバーで指定された元の場所から、図形を移動します。</span><span class="sxs-lookup"><span data-stu-id="acf89-220">The above code moves the shape from the old location to the new one given by the server over the course of the animation interval (in this case, 100 milliseconds).</span></span> <span data-ttu-id="acf89-221">新しいアニメーションの開始前に、任意図形で実行されている前のアニメーションがクリアされます。</span><span class="sxs-lookup"><span data-stu-id="acf89-221">Any previous animation running on the shape is cleared before the new animation starts.</span></span>
2. <span data-ttu-id="acf89-222">F5 キーを押して、アプリケーションを起動します。</span><span class="sxs-lookup"><span data-stu-id="acf89-222">Start the application by pressing F5.</span></span> <span data-ttu-id="acf89-223">ページの URL をコピーし、2 番目のブラウザー ウィンドウに貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="acf89-223">Copy the page's URL, and paste it into a second browser window.</span></span> <span data-ttu-id="acf89-224">ブラウザーのウィンドウのいずれかで図形をドラッグしてください。他のブラウザー ウィンドウで、図形を移動する必要があります。</span><span class="sxs-lookup"><span data-stu-id="acf89-224">Drag the shape in one of the browser windows; the shape in the other browser window should move.</span></span> <span data-ttu-id="acf89-225">受信メッセージごとに 1 回設定されているのではなく、時間、動きが補間と小さいがスムーズでない他のウィンドウで、図形の移動が表示されます。</span><span class="sxs-lookup"><span data-stu-id="acf89-225">The movement of the shape in the other window should appear less jerky as its movement is interpolated over time rather than being set once per incoming message.</span></span>

    ![アプリケーション ウィンドウ](tutorial-high-frequency-realtime-with-signalr/_static/image7.png)

<a id="furthersteps"></a>

## <a name="further-steps"></a><span data-ttu-id="acf89-227">以降の手順</span><span class="sxs-lookup"><span data-stu-id="acf89-227">Further Steps</span></span>

<span data-ttu-id="acf89-228">このチュートリアルでは、クライアントとサーバー間の頻度が高いメッセージを送信する SignalR アプリケーションをプログラミングする方法を学びました。</span><span class="sxs-lookup"><span data-stu-id="acf89-228">In this tutorial, you've learned how to program a SignalR application that sends high-frequency messages between clients and servers.</span></span> <span data-ttu-id="acf89-229">この通信パラダイムはなどオンライン ゲームとその他のシミュレーションを開発するために役立ちます[SignalR で作成された ShootR ゲーム](http://shootr.signalr.net)です。</span><span class="sxs-lookup"><span data-stu-id="acf89-229">This communication paradigm is useful for developing online games and other simulations, such as [the ShootR game created with SignalR](http://shootr.signalr.net).</span></span>

<span data-ttu-id="acf89-230">このチュートリアルで作成した完全なアプリケーションからダウンロードできます[コード ギャラリー](https://code.msdn.microsoft.com/SignalR-MoveShape-demo-3366dac6)です。</span><span class="sxs-lookup"><span data-stu-id="acf89-230">The complete application created in this tutorial can be downloaded from [Code Gallery](https://code.msdn.microsoft.com/SignalR-MoveShape-demo-3366dac6).</span></span>

<span data-ttu-id="acf89-231">SignalR 開発の概念に関する詳細についてには、SignalR のソース コードおよびリソースの次のサイトを参照してください。</span><span class="sxs-lookup"><span data-stu-id="acf89-231">To learn more about SignalR development concepts, visit the following sites for SignalR source code and resources:</span></span>

- [<span data-ttu-id="acf89-232">SignalR プロジェクト</span><span class="sxs-lookup"><span data-stu-id="acf89-232">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="acf89-233">SignalR Github とサンプル</span><span class="sxs-lookup"><span data-stu-id="acf89-233">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="acf89-234">SignalR Wiki</span><span class="sxs-lookup"><span data-stu-id="acf89-234">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
