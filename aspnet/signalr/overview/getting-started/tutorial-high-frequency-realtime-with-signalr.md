---
uid: signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
title: 'チュートリアル: 頻度が高いリアルタイム signalr 2 |Microsoft ドキュメント'
author: pfletcher
description: このチュートリアルでは、ASP.NET SignalR を使用して、頻度が高いメッセージング機能を提供する web アプリケーションを作成する方法を示します。 高周波数のメッセージングには.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 9f969dda-78ea-4329-b1e3-e51c02210a2b
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: ab051b2ab85d1aac1e7179f342f22f470b1d1cc7
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="tutorial-high-frequency-realtime-with-signalr-2"></a><span data-ttu-id="f1528-104">チュートリアル: 頻度が高いリアルタイム signalr 2</span><span class="sxs-lookup"><span data-stu-id="f1528-104">Tutorial: High-Frequency Realtime with SignalR 2</span></span>
====================
<span data-ttu-id="f1528-105">によって[Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="f1528-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[<span data-ttu-id="f1528-106">完成したプロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="f1528-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)

> <span data-ttu-id="f1528-107">このチュートリアルでは、ASP.NET SignalR 2 を使用して、頻度が高いメッセージング機能を提供する web アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="f1528-107">This tutorial shows how to create a web application that uses ASP.NET SignalR 2 to provide high-frequency messaging functionality.</span></span> <span data-ttu-id="f1528-108">高周波数のメッセージング、固定の率で送信される更新プログラムは、ここではこのアプリケーションの場合は、最大で 10 は、2 つ目をメッセージです。</span><span class="sxs-lookup"><span data-stu-id="f1528-108">High-frequency messaging in this case means updates that are sent at a fixed rate; in the case of this application, up to 10 messages a second.</span></span>
> 
> <span data-ttu-id="f1528-109">このチュートリアルで作成したアプリケーションには、ユーザーがドラッグできる図形が表示されます。</span><span class="sxs-lookup"><span data-stu-id="f1528-109">The application you'll create in this tutorial displays a shape that users can drag.</span></span> <span data-ttu-id="f1528-110">接続されている他のすべてのブラウザーでの図形の位置は、時間指定の更新プログラムを使用して、ドラッグした図形の位置に一致するように更新されます。</span><span class="sxs-lookup"><span data-stu-id="f1528-110">The position of the shape in all other connected browsers will then be updated to match the position of the dragged shape using timed updates.</span></span>
> 
> <span data-ttu-id="f1528-111">このチュートリアルで導入された概念には、リアルタイムのゲーム内のアプリケーションおよびその他のシミュレーションのアプリケーションがあります。</span><span class="sxs-lookup"><span data-stu-id="f1528-111">Concepts introduced in this tutorial have applications in real-time gaming and other simulation applications.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f1528-112">このチュートリアルで使用されているソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="f1528-112">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="f1528-113">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="f1528-113">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="f1528-114">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="f1528-114">.NET 4.5</span></span>
> - <span data-ttu-id="f1528-115">SignalR バージョン 2</span><span class="sxs-lookup"><span data-stu-id="f1528-115">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="f1528-116">このチュートリアルで Visual Studio 2012 を使用します。</span><span class="sxs-lookup"><span data-stu-id="f1528-116">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="f1528-117">このチュートリアルでは、Visual Studio 2012 を使用するには、次の操作を行います。</span><span class="sxs-lookup"><span data-stu-id="f1528-117">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="f1528-118">更新プログラム、 [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget)最新バージョンにします。</span><span class="sxs-lookup"><span data-stu-id="f1528-118">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="f1528-119">インストール、 [Web プラットフォーム インストーラー](https://www.microsoft.com/web/downloads/platform.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="f1528-119">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="f1528-120">Web Platform Installer で検索し、インストール**ASP.NET と Visual Studio 2012 用 Web ツール 2013.1**です。</span><span class="sxs-lookup"><span data-stu-id="f1528-120">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="f1528-121">SignalR クラス用の Visual Studio テンプレートなどのインストールはこの**ハブ**です。</span><span class="sxs-lookup"><span data-stu-id="f1528-121">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="f1528-122">一部のテンプレート (など**OWIN スタートアップ クラス**) できなくなります。 これらの場合には、クラス ファイルを代わりに使用します。</span><span class="sxs-lookup"><span data-stu-id="f1528-122">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="f1528-123">チュートリアルのバージョン</span><span class="sxs-lookup"><span data-stu-id="f1528-123">Tutorial Versions</span></span>
> 
> <span data-ttu-id="f1528-124">SignalR の以前のバージョンについては、次を参照してください。[古いバージョンの SignalR](../older-versions/index.md)です。</span><span class="sxs-lookup"><span data-stu-id="f1528-124">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="f1528-125">質問やコメント</span><span class="sxs-lookup"><span data-stu-id="f1528-125">Questions and comments</span></span>
> 
> <span data-ttu-id="f1528-126">このチュートリアルをリンクする方法と、ページの下部にあるコメントで改善新機能にフィードバックを送信してください。</span><span class="sxs-lookup"><span data-stu-id="f1528-126">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="f1528-127">チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)です。</span><span class="sxs-lookup"><span data-stu-id="f1528-127">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="f1528-128">概要</span><span class="sxs-lookup"><span data-stu-id="f1528-128">Overview</span></span>

<span data-ttu-id="f1528-129">このチュートリアルでは、リアルタイムで他のブラウザーとオブジェクトの状態を共有しているアプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="f1528-129">This tutorial demonstrates how to create an application that shares the state of an object with other browsers in real time.</span></span> <span data-ttu-id="f1528-130">作成したアプリケーションには、MoveShape が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="f1528-130">The application we'll create is called MoveShape.</span></span> <span data-ttu-id="f1528-131">ユーザーがドラッグできる; HTML Div 要素を MoveShape ページが表示されます。ドラッグすると、ユーザー、Div の新しい位置に一致する図形の位置を更新するその他のすべての接続しているクライアントに指示しは、サーバーに送信されます。</span><span class="sxs-lookup"><span data-stu-id="f1528-131">The MoveShape page will display an HTML Div element that the user can drag; when the user drags the Div, its new position will be sent to the server, which will then tell all other connected clients to update the shape's position to match.</span></span>

![アプリケーション ウィンドウ](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

<span data-ttu-id="f1528-133">このチュートリアルで作成したアプリケーションは、Damian Edwards によるデモに基づいています。</span><span class="sxs-lookup"><span data-stu-id="f1528-133">The application created in this tutorial is based on a demo by Damian Edwards.</span></span> <span data-ttu-id="f1528-134">このデモを格納しているビデオがわかるように[ここ](https://channel9.msdn.com/Series/Building-Web-Apps-with-ASP-NET-Jump-Start/Building-Web-Apps-with-ASPNET-Jump-Start-08-Real-time-Communication-with-SignalR)です。</span><span class="sxs-lookup"><span data-stu-id="f1528-134">A video containing this demo can be seen [here](https://channel9.msdn.com/Series/Building-Web-Apps-with-ASP-NET-Jump-Start/Building-Web-Apps-with-ASPNET-Jump-Start-08-Real-time-Communication-with-SignalR).</span></span>

<span data-ttu-id="f1528-135">このチュートリアルは、各発生するイベントにドラッグしたときに、その形状から SignalR メッセージを送信する方法のデモで開始されます。</span><span class="sxs-lookup"><span data-stu-id="f1528-135">The tutorial will start by demonstrating how to send SignalR messages from each event that fires as the shape is dragged.</span></span> <span data-ttu-id="f1528-136">接続されている各クライアントは、メッセージを受信するたびに、図形のローカル バージョンの位置を更新されます。</span><span class="sxs-lookup"><span data-stu-id="f1528-136">Each connected client will then update the position of the local version of the shape each time a message is received.</span></span>

<span data-ttu-id="f1528-137">このメソッドを使用して、アプリケーションが機能がこれはできません、推奨されるプログラミング モデルがあるためありません、クライアントとサーバーが取得集中してパンク状態メッセージと、パフォーマンスが低下するので、送信先のメッセージの数に上限を設定.</span><span class="sxs-lookup"><span data-stu-id="f1528-137">While the application will function using this method, this is not a recommended programming model, since there would be no upper limit to the number of messages getting sent, so the clients and server could get overwhelmed with messages and performance would degrade.</span></span> <span data-ttu-id="f1528-138">クライアントで表示されているアニメーションは場所ごとにスムーズに移行するのではなく、図形は、各方法に瞬時に移動するは、不整合にもなります。</span><span class="sxs-lookup"><span data-stu-id="f1528-138">The displayed animation on the client would also be disjointed, as the shape would be moved instantly by each method, rather than moving smoothly to each new location.</span></span> <span data-ttu-id="f1528-139">このチュートリアルの以降のセクションは、クライアントまたはサーバーがメッセージを送信する最大転送率を制限するタイマー関数を作成する方法と場所の間での図形をスムーズに移動する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="f1528-139">Later sections of the tutorial will demonstrate how to create a timer function that restricts the maximum rate at which messages are sent by either the client or server, and how to move the shape smoothly between locations.</span></span> <span data-ttu-id="f1528-140">このチュートリアルで作成したアプリケーションの最終バージョンからダウンロードできます[コード ギャラリー](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)です。</span><span class="sxs-lookup"><span data-stu-id="f1528-140">The final version of the application created in this tutorial can be downloaded from [Code Gallery](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a).</span></span>

<span data-ttu-id="f1528-141">このチュートリアルでは、次のセクションでは、含まれています。</span><span class="sxs-lookup"><span data-stu-id="f1528-141">This tutorial contains the following sections:</span></span>

- [<span data-ttu-id="f1528-142">前提条件</span><span class="sxs-lookup"><span data-stu-id="f1528-142">Prerequisites</span></span>](#prerequisites)
- [<span data-ttu-id="f1528-143">プロジェクトを作成し、SignalR と JQuery.UI NuGet パッケージの追加</span><span class="sxs-lookup"><span data-stu-id="f1528-143">Create the project and add the SignalR and JQuery.UI NuGet package</span></span>](#createtheproject2013)
- [<span data-ttu-id="f1528-144">ベースのアプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="f1528-144">Create the base application</span></span>](#baseapp)
- [<span data-ttu-id="f1528-145">アプリケーションの起動時に、ハブの開始</span><span class="sxs-lookup"><span data-stu-id="f1528-145">Starting the hub when the application starts</span></span>](#startup2013)
- [<span data-ttu-id="f1528-146">クライアント ループに追加します。</span><span class="sxs-lookup"><span data-stu-id="f1528-146">Add the client loop</span></span>](#clientloop)
- [<span data-ttu-id="f1528-147">サーバーのループを追加します。</span><span class="sxs-lookup"><span data-stu-id="f1528-147">Add the server loop</span></span>](#serverloop)
- [<span data-ttu-id="f1528-148">クライアントで滑らかなアニメーションを追加します。</span><span class="sxs-lookup"><span data-stu-id="f1528-148">Add smooth animation on the client</span></span>](#animation)
- [<span data-ttu-id="f1528-149">以降の手順</span><span class="sxs-lookup"><span data-stu-id="f1528-149">Further Steps</span></span>](#furthersteps)

<a id="prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="f1528-150">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="f1528-150">Prerequisites</span></span>

<span data-ttu-id="f1528-151">このチュートリアルでは、Visual Studio 2013 が必要です。</span><span class="sxs-lookup"><span data-stu-id="f1528-151">This tutorial requires Visual Studio 2013.</span></span>

<a id="createtheproject2013"></a>

## <a name="create-the-project-and-add-the-signalr-and-jqueryui-nuget-package"></a><span data-ttu-id="f1528-152">プロジェクトを作成し、SignalR と JQuery.UI NuGet パッケージの追加</span><span class="sxs-lookup"><span data-stu-id="f1528-152">Create the project and add the SignalR and JQuery.UI NuGet package</span></span>

<span data-ttu-id="f1528-153">このセクションで Visual Studio 2013 で、プロジェクトが作成されます。</span><span class="sxs-lookup"><span data-stu-id="f1528-153">In this section, we'll create the project in Visual Studio 2013.</span></span>

<span data-ttu-id="f1528-154">次の手順では、空の ASP.NET Web アプリケーションを作成し、SignalR および jQuery.UI ライブラリを追加する Visual Studio 2013 を使用します。</span><span class="sxs-lookup"><span data-stu-id="f1528-154">The following steps use Visual Studio 2013 to create an ASP.NET Empty Web Application and add the SignalR and jQuery.UI libraries:</span></span>

1. <span data-ttu-id="f1528-155">Visual Studio では、ASP.NET Web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="f1528-155">In Visual Studio create an ASP.NET Web Application.</span></span>

    ![Web を作成します。](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)
2. <span data-ttu-id="f1528-157">**新しい ASP.NET プロジェクト** ウィンドウのままにして**空**を選択し、をクリックして**プロジェクトの作成**です。</span><span class="sxs-lookup"><span data-stu-id="f1528-157">In the **New ASP.NET Project** window, leave **Empty** selected and click **Create Project**.</span></span>

    ![空の web を作成します。](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)
3. <span data-ttu-id="f1528-159">**ソリューション エクスプ ローラー**、プロジェクトを右クリックし、選択**追加 |SignalR ハブ クラス (v2)** です。</span><span class="sxs-lookup"><span data-stu-id="f1528-159">In **Solution Explorer**, right-click the project, select **Add | SignalR Hub Class (v2)**.</span></span> <span data-ttu-id="f1528-160">クラスの名前を付けます**MoveShapeHub.cs**し、プロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="f1528-160">Name the class **MoveShapeHub.cs** and add it to the project.</span></span> <span data-ttu-id="f1528-161">この手順で作成、 **MoveShapeHub**クラスし、一連のスクリプト ファイルと SignalR をサポートするアセンブリ参照がプロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="f1528-161">This step creates the **MoveShapeHub** class and adds to the project a set of script files and assembly references that support SignalR.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f1528-162">クリックして、プロジェクトに SignalR を追加することも**ツール |ライブラリ パッケージ マネージャー |パッケージ マネージャー コンソール**コマンドを実行しているとします。</span><span class="sxs-lookup"><span data-stu-id="f1528-162">You can also add SignalR to a project by clicking **Tools | Library Package Manager | Package Manager Console** and running a command:</span></span>

    <span data-ttu-id="f1528-163">`install-package Microsoft.AspNet.SignalR`。</span><span class="sxs-lookup"><span data-stu-id="f1528-163">`install-package Microsoft.AspNet.SignalR`.</span></span> 

    <span data-ttu-id="f1528-164">コンソールを使用して SignalR を追加する場合は、SignalR を追加した後に、別のステップとして SignalR ハブ クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="f1528-164">If you use the console to add SignalR, create the SignalR hub class as a separate step after you add SignalR.</span></span>
4. <span data-ttu-id="f1528-165">をクリックして**ツール |ライブラリ パッケージ マネージャー |パッケージ マネージャー コンソール**です。</span><span class="sxs-lookup"><span data-stu-id="f1528-165">Click **Tools | Library Package Manager | Package Manager Console**.</span></span> <span data-ttu-id="f1528-166">パッケージ マネージャー ウィンドウで、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="f1528-166">In the package manager window, run the following command:</span></span>

    `Install-Package jQuery.UI.Combined`

    <span data-ttu-id="f1528-167">これは、図形をアニメーション化するときに使用される、jQuery UI ライブラリをインストールします。</span><span class="sxs-lookup"><span data-stu-id="f1528-167">This installs the jQuery UI library, which you'll use to animate the shape.</span></span>
5. <span data-ttu-id="f1528-168">**ソリューション エクスプ ローラー**スクリプト ノードを展開します。</span><span class="sxs-lookup"><span data-stu-id="f1528-168">In **Solution Explorer** expand the Scripts node.</span></span> <span data-ttu-id="f1528-169">JQuery、jQueryUI、および SignalR のスクリプト ライブラリは、プロジェクトに表示されます。</span><span class="sxs-lookup"><span data-stu-id="f1528-169">Script libraries for jQuery, jQueryUI, and SignalR are visible in the project.</span></span>

    ![スクリプト ライブラリの参照](tutorial-high-frequency-realtime-with-signalr/_static/image4.png)

<a id="baseapp"></a>

## <a name="create-the-base-application"></a><span data-ttu-id="f1528-171">ベースのアプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="f1528-171">Create the base application</span></span>

<span data-ttu-id="f1528-172">このセクションの各マウス移動イベント中に、サーバーに、図形の場所を送信するブラウザー アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="f1528-172">In this section, we'll create a browser application that sends the location of the shape to the server during each mouse move event.</span></span> <span data-ttu-id="f1528-173">受信すると、サーバーは、接続されている他のすべてのクライアントにこの情報をブロードキャストします。</span><span class="sxs-lookup"><span data-stu-id="f1528-173">The server then broadcasts this information to all other connected clients as it is received.</span></span> <span data-ttu-id="f1528-174">以降のセクションでは、このアプリケーションに展開してあります。</span><span class="sxs-lookup"><span data-stu-id="f1528-174">We'll expand on this application in later sections.</span></span>

1. <span data-ttu-id="f1528-175">MoveShapeHub.cs クラスをまだ作成していないかどうかは**ソリューション エクスプ ローラー**プロジェクトを右クリックし、選択、**追加**、**クラス.**.クラスの名前を付けます**MoveShapeHub**  をクリック**追加**です。</span><span class="sxs-lookup"><span data-stu-id="f1528-175">If you haven't already created the MoveShapeHub.cs class, in **Solution Explorer**, right-click on the project and select **Add**, **Class...**. Name the class **MoveShapeHub** and click **Add**.</span></span>
2. <span data-ttu-id="f1528-176">新しいコードを置き換える**MoveShapeHub**クラスを次のコード。</span><span class="sxs-lookup"><span data-stu-id="f1528-176">Replace the code in the new **MoveShapeHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.cs)]

    <span data-ttu-id="f1528-177">`MoveShapeHub`上記のクラスは、SignalR ハブの実装です。</span><span class="sxs-lookup"><span data-stu-id="f1528-177">The `MoveShapeHub` class above is an implementation of a SignalR hub.</span></span> <span data-ttu-id="f1528-178">同様、 [SignalR の概要](tutorial-getting-started-with-signalr.md)チュートリアルでは、ハブに直接に、クライアントが呼び出すメソッドがあります。</span><span class="sxs-lookup"><span data-stu-id="f1528-178">As in the [Getting Started with SignalR](tutorial-getting-started-with-signalr.md) tutorial, the hub has a method that the clients will call directly.</span></span> <span data-ttu-id="f1528-179">ここでは、クライアントが新しいを格納するオブジェクトを送信図形を接続している他のすべてのクライアントにブロードキャストを取得し、サーバーの X および Y 座標。</span><span class="sxs-lookup"><span data-stu-id="f1528-179">In this case, the client will send an object containing the new X and Y coordinates of the shape to the server, which then gets broadcasted to all other connected clients.</span></span> <span data-ttu-id="f1528-180">SignalR には、JSON を使用してこのオブジェクトは自動的にシリアル化します。</span><span class="sxs-lookup"><span data-stu-id="f1528-180">SignalR will automatically serialize this object using JSON.</span></span>

    <span data-ttu-id="f1528-181">クライアントに送信されるオブジェクト (`ShapeModel`) 図形の位置を格納するメンバーが含まれます。</span><span class="sxs-lookup"><span data-stu-id="f1528-181">The object that will be sent to the client (`ShapeModel`) contains members to store the position of the shape.</span></span> <span data-ttu-id="f1528-182">サーバー上のオブジェクトのバージョンも含まれていますを追跡するにはメンバーをクライアントのデータが格納される、特定のクライアントがしない独自のデータを送信できるようにします。</span><span class="sxs-lookup"><span data-stu-id="f1528-182">The version of the object on the server also contains a member to track which client's data is being stored, so that a given client won't be sent their own data.</span></span> <span data-ttu-id="f1528-183">このメンバーは使用して、`JsonIgnore`にならないようにシリアル化され、クライアントに送信される属性です。</span><span class="sxs-lookup"><span data-stu-id="f1528-183">This member uses the `JsonIgnore` attribute to keep it from being serialized and sent to the client.</span></span>

<a id="startup2013"></a>
## <a name="starting-the-hub-when-the-application-starts"></a><span data-ttu-id="f1528-184">アプリケーションの起動時に、ハブの開始</span><span class="sxs-lookup"><span data-stu-id="f1528-184">Starting the hub when the application starts</span></span>

1. <span data-ttu-id="f1528-185">次に、アプリケーションの起動時に、ハブへのマッピングを設定します。</span><span class="sxs-lookup"><span data-stu-id="f1528-185">Next, we'll set up mapping to the hub when the application starts.</span></span> <span data-ttu-id="f1528-186">SignalR 2 では、呼び出しは、OWIN スタートアップ クラスを追加することによって実行`MapSignalR`ときにスタートアップ クラス`Configuration`OWIN の開始時に、メソッドを実行します。</span><span class="sxs-lookup"><span data-stu-id="f1528-186">In SignalR 2, this is done by adding an OWIN startup class, which will call `MapSignalR` when the startup class' `Configuration` method is executed when OWIN starts.</span></span> <span data-ttu-id="f1528-187">OWIN の起動に、クラスが追加処理を使用して、`OwinStartup`アセンブリの属性です。</span><span class="sxs-lookup"><span data-stu-id="f1528-187">The class is added to OWIN's startup process using the `OwinStartup` assembly attribute.</span></span>

    <span data-ttu-id="f1528-188">**ソリューション エクスプ ローラー**プロジェクトを右クリックし、クリックして**追加 |OWIN 起動クラス**です。</span><span class="sxs-lookup"><span data-stu-id="f1528-188">In **Solution Explorer**, right-click the project, then click **Add | OWIN Startup Class**.</span></span> <span data-ttu-id="f1528-189">クラスに名前を*スタートアップ* をクリック**OK**です。</span><span class="sxs-lookup"><span data-stu-id="f1528-189">Name the class *Startup* and click **OK**.</span></span>
2. <span data-ttu-id="f1528-190">Startup.cs の内容を次のように変更します。</span><span class="sxs-lookup"><span data-stu-id="f1528-190">Change the contents of Startup.cs to the following:</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.cs)]

<a id="client"></a>
## <a name="adding-the-client"></a><span data-ttu-id="f1528-191">クライアントを追加します。</span><span class="sxs-lookup"><span data-stu-id="f1528-191">Adding the client</span></span>

1. <span data-ttu-id="f1528-192">次に、クライアントを追加します。</span><span class="sxs-lookup"><span data-stu-id="f1528-192">Next, we'll add the client.</span></span> <span data-ttu-id="f1528-193">**ソリューション エクスプ ローラー**プロジェクトを右クリックし、クリックして**追加 |新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="f1528-193">In **Solution Explorer**, right-click the project, then click **Add | New Item**.</span></span> <span data-ttu-id="f1528-194">**新しい項目の追加**ダイアログで、 **Html ページ**です。</span><span class="sxs-lookup"><span data-stu-id="f1528-194">In the **Add New Item** dialog, select **Html Page**.</span></span> <span data-ttu-id="f1528-195">ページの名前を付けます**Default.html**  をクリック**追加**です。</span><span class="sxs-lookup"><span data-stu-id="f1528-195">Name the page **Default.html** and click **Add**.</span></span>
2. <span data-ttu-id="f1528-196">**ソリューション エクスプ ローラー**を作成したページを右クリックし、をクリックして**スタート ページとして設定**です。</span><span class="sxs-lookup"><span data-stu-id="f1528-196">In **Solution Explorer**, right-click the page you just created and click **Set as Start Page**.</span></span>
3. <span data-ttu-id="f1528-197">次のコード スニペットでは、HTML ページで、既定のコードを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="f1528-197">Replace the default code in the HTML page with the following code snippet.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f1528-198">スクリプトを参照している一致の下の Scripts フォルダーでプロジェクトに追加されたパッケージを確認します。</span><span class="sxs-lookup"><span data-stu-id="f1528-198">Verify that the script references below match the packages added to your project in the Scripts folder.</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.html)]

    <span data-ttu-id="f1528-199">図形と呼ばれる赤い Div を作成、jQuery ライブラリを使用して、図形のドラッグ動作を有効にし、図形の使用上の HTML および JavaScript コード`drag`図形の位置をサーバーに送信するイベントです。</span><span class="sxs-lookup"><span data-stu-id="f1528-199">The above HTML and JavaScript code creates a red Div called Shape, enables the shape's dragging behavior using the jQuery library, and uses the shape's `drag` event to send the shape's position to the server.</span></span>
4. <span data-ttu-id="f1528-200">F5 キーを押して、アプリケーションを起動します。</span><span class="sxs-lookup"><span data-stu-id="f1528-200">Start the application by pressing F5.</span></span> <span data-ttu-id="f1528-201">ページの URL をコピーし、2 番目のブラウザー ウィンドウに貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="f1528-201">Copy the page's URL, and paste it into a second browser window.</span></span> <span data-ttu-id="f1528-202">ブラウザーのウィンドウのいずれかで図形をドラッグしてください。他のブラウザー ウィンドウで、図形を移動する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f1528-202">Drag the shape in one of the browser windows; the shape in the other browser window should move.</span></span>

    ![アプリケーション ウィンドウ](tutorial-high-frequency-realtime-with-signalr/_static/image5.png)

<a id="clientloop"></a>

## <a name="add-the-client-loop"></a><span data-ttu-id="f1528-204">クライアント ループに追加します。</span><span class="sxs-lookup"><span data-stu-id="f1528-204">Add the client loop</span></span>

<span data-ttu-id="f1528-205">すべてのマウス移動イベントで図形の場所を送信すると、ネットワーク トラフィック量が不要なを作成するため、クライアントからのメッセージを調整する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f1528-205">Since sending the location of the shape on every mouse move event will create an unneccesary amount of network traffic, the messages from the client need to be throttled.</span></span> <span data-ttu-id="f1528-206">Javascript を使用して`setInterval`固定の率でサーバーに新しい位置情報を送信するループを設定する関数。</span><span class="sxs-lookup"><span data-stu-id="f1528-206">We'll use the javascript `setInterval` function to set up a loop that sends new position information to the server at a fixed rate.</span></span> <span data-ttu-id="f1528-207">このループは、「ゲーム ループ」、ドライブのすべてのゲームやその他のシミュレーションの機能を繰り返し呼び出された関数の非常に基本的な表現です。</span><span class="sxs-lookup"><span data-stu-id="f1528-207">This loop is a very basic representation of a "game loop", a repeatedly called function that drives all of the functionality of a game or other simulation.</span></span>

1. <span data-ttu-id="f1528-208">次のコード スニペットを一致するように HTML ページ内のクライアント コードを更新します。</span><span class="sxs-lookup"><span data-stu-id="f1528-208">Update the client code in the HTML page to match the following code snippet.</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.html)]

    <span data-ttu-id="f1528-209">上記の更新プログラムの追加、`updateServerModel`固定周波数に呼び出される関数。</span><span class="sxs-lookup"><span data-stu-id="f1528-209">The above update adds the `updateServerModel` function, which gets called on a fixed frequency.</span></span> <span data-ttu-id="f1528-210">この関数は、サーバーに、位置データを送信するたびに、`moved`フラグが示される位置に新しいデータを送信します。</span><span class="sxs-lookup"><span data-stu-id="f1528-210">This function sends the position data to the server whenever the `moved` flag indicates that there is new position data to send.</span></span>
2. <span data-ttu-id="f1528-211">F5 キーを押して、アプリケーションを起動します。</span><span class="sxs-lookup"><span data-stu-id="f1528-211">Start the application by pressing F5.</span></span> <span data-ttu-id="f1528-212">ページの URL をコピーし、2 番目のブラウザー ウィンドウに貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="f1528-212">Copy the page's URL, and paste it into a second browser window.</span></span> <span data-ttu-id="f1528-213">ブラウザーのウィンドウのいずれかで図形をドラッグしてください。他のブラウザー ウィンドウで、図形を移動する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f1528-213">Drag the shape in one of the browser windows; the shape in the other browser window should move.</span></span> <span data-ttu-id="f1528-214">サーバーに送信するメッセージの数を調整するため、アニメーションは、前のセクションと同様に円滑表示されません。</span><span class="sxs-lookup"><span data-stu-id="f1528-214">Since the number of messages that get sent to the server will be throttled, the animation will not appear as smooth as in the previous section.</span></span>

    ![アプリケーション ウィンドウ](tutorial-high-frequency-realtime-with-signalr/_static/image6.png)

<a id="serverloop"></a>

## <a name="add-the-server-loop"></a><span data-ttu-id="f1528-216">サーバーのループを追加します。</span><span class="sxs-lookup"><span data-stu-id="f1528-216">Add the server loop</span></span>

<span data-ttu-id="f1528-217">現在のアプリケーションで、サーバーからクライアントに送信されたメッセージは受信するときに多くの場合、移動します。</span><span class="sxs-lookup"><span data-stu-id="f1528-217">In the current application, messages sent from the server to the client go out as often as they are received.</span></span> <span data-ttu-id="f1528-218">クライアント上で見つかりましたが、この場合は、同様の問題メッセージが必要であれば、および接続状態になるあふれたその結果よりも頻繁に送信できます。</span><span class="sxs-lookup"><span data-stu-id="f1528-218">This presents a similar problem as was seen on the client; messages can be sent more often than they are needed, and the connection could become flooded as a result.</span></span> <span data-ttu-id="f1528-219">このセクションでは、送信メッセージのレートを調整するタイマーを実装するサーバーを更新する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="f1528-219">This section describes how to update the server to implement a timer that throttles the rate of the outgoing messages.</span></span>

1. <span data-ttu-id="f1528-220">内容を置き換える`MoveShapeHub.cs`次のコード スニペットとします。</span><span class="sxs-lookup"><span data-stu-id="f1528-220">Replace the contents of `MoveShapeHub.cs` with the following code snippet.</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

    <span data-ttu-id="f1528-221">上記のコードを追加するクライアントの展開、`Broadcaster`クラスは、調整して、送信メッセージを使用して、 `Timer` .NET framework からクラスです。</span><span class="sxs-lookup"><span data-stu-id="f1528-221">The above code expands the client to add the `Broadcaster` class, which throttles the outgoing messages using the `Timer` class from the .NET framework.</span></span>

    <span data-ttu-id="f1528-222">ハブ自体が一時的なものから (は、その作成が必要なたびに)、`Broadcaster`はシングルトンとして作成されます。</span><span class="sxs-lookup"><span data-stu-id="f1528-222">Since the hub itself is transitory (it is created every time it is needed), the `Broadcaster` will be created as a singleton.</span></span> <span data-ttu-id="f1528-223">(.NET 4 で導入) 限定的な初期化を使用して、作成を遅らせる必要になるまでは、タイマーが開始される前に、最初のハブ インスタンスが完全に作成されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="f1528-223">Lazy initialization (introduced in .NET 4) is used to defer its creation until it is needed, ensuring that the first hub instance is completely created before the timer is started.</span></span>

    <span data-ttu-id="f1528-224">クライアントへの呼び出し`UpdateShape`関数は、ハブの外移動`UpdateModel`メソッド、受信メッセージが受信されたときにすぐにその it が呼び出されないようにします。</span><span class="sxs-lookup"><span data-stu-id="f1528-224">The call to the clients' `UpdateShape` function is then moved out of the hub's `UpdateModel` method, so that it is no longer called immediately whenever incoming messages are received.</span></span> <span data-ttu-id="f1528-225">25 の呼び出しの 1 秒あたりのレートでクライアントにメッセージの送信される代わりに、によって管理される、`_broadcastLoop`内からタイマー、`Broadcaster`クラスです。</span><span class="sxs-lookup"><span data-stu-id="f1528-225">Instead, the messages to the clients will be sent at a rate of 25 calls per second, managed by the `_broadcastLoop` timer from within the `Broadcaster` class.</span></span>

    <span data-ttu-id="f1528-226">ハブから直接クライアント メソッドを呼び出すのではなく、最後に、`Broadcaster`クラスは、現在の作業ハブへの参照を取得する必要があります (`_hubContext`) を使用して、`GlobalHost`です。</span><span class="sxs-lookup"><span data-stu-id="f1528-226">Lastly, instead of calling the client method from the hub directly, the `Broadcaster` class needs to obtain a reference to the currently operating hub (`_hubContext`) using the `GlobalHost`.</span></span>
2. <span data-ttu-id="f1528-227">F5 キーを押して、アプリケーションを起動します。</span><span class="sxs-lookup"><span data-stu-id="f1528-227">Start the application by pressing F5.</span></span> <span data-ttu-id="f1528-228">ページの URL をコピーし、2 番目のブラウザー ウィンドウに貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="f1528-228">Copy the page's URL, and paste it into a second browser window.</span></span> <span data-ttu-id="f1528-229">ブラウザーのウィンドウのいずれかで図形をドラッグしてください。他のブラウザー ウィンドウで、図形を移動する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f1528-229">Drag the shape in one of the browser windows; the shape in the other browser window should move.</span></span> <span data-ttu-id="f1528-230">前のセクションからブラウザーで表示される相違点されませんが、クライアントに送信するメッセージの数を調整します。</span><span class="sxs-lookup"><span data-stu-id="f1528-230">There will not be a visible difference in the browser from the previous section, but the number of messages that get sent to the client will be throttled.</span></span>

    ![アプリケーション ウィンドウ](tutorial-high-frequency-realtime-with-signalr/_static/image7.png)

<a id="animation"></a>

## <a name="add-smooth-animation-on-the-client"></a><span data-ttu-id="f1528-232">クライアントで滑らかなアニメーションを追加します。</span><span class="sxs-lookup"><span data-stu-id="f1528-232">Add smooth animation on the client</span></span>

<span data-ttu-id="f1528-233">アプリケーションがほぼ完了しましたが、1 つ以上のパフォーマンス向上、クライアントで図形を描くようにサーバー メッセージに応答を移動するときにすることもできます。</span><span class="sxs-lookup"><span data-stu-id="f1528-233">The application is almost complete, but we could make one more improvement, in the motion of the shape on the client as it is moved in response to server messages.</span></span> <span data-ttu-id="f1528-234">サーバーで指定された、新しい場所への図形の位置を設定するのではなく、JQuery UI ライブラリを使用`animate`の現在と新しい位置の間でスムーズに図形を移動する関数。</span><span class="sxs-lookup"><span data-stu-id="f1528-234">Rather than setting the position of the shape to the new location given by the server, we'll use the JQuery UI library's `animate` function to move the shape smoothly between its current and new position.</span></span>

1. <span data-ttu-id="f1528-235">クライアントの更新`updateShape`を検索するメソッドなどの次の強調表示されたコード。</span><span class="sxs-lookup"><span data-stu-id="f1528-235">Update the client's `updateShape` method to look like the highlighted code below:</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.html?highlight=33-40)]

    <span data-ttu-id="f1528-236">上記のコードを新しいアニメーション間隔 (ここでは、100 ミリ秒) の過程で、サーバーで指定された元の場所から、図形を移動します。</span><span class="sxs-lookup"><span data-stu-id="f1528-236">The above code moves the shape from the old location to the new one given by the server over the course of the animation interval (in this case, 100 milliseconds).</span></span> <span data-ttu-id="f1528-237">新しいアニメーションの開始前に、任意図形で実行されている前のアニメーションがクリアされます。</span><span class="sxs-lookup"><span data-stu-id="f1528-237">Any previous animation running on the shape is cleared before the new animation starts.</span></span>
2. <span data-ttu-id="f1528-238">F5 キーを押して、アプリケーションを起動します。</span><span class="sxs-lookup"><span data-stu-id="f1528-238">Start the application by pressing F5.</span></span> <span data-ttu-id="f1528-239">ページの URL をコピーし、2 番目のブラウザー ウィンドウに貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="f1528-239">Copy the page's URL, and paste it into a second browser window.</span></span> <span data-ttu-id="f1528-240">ブラウザーのウィンドウのいずれかで図形をドラッグしてください。他のブラウザー ウィンドウで、図形を移動する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f1528-240">Drag the shape in one of the browser windows; the shape in the other browser window should move.</span></span> <span data-ttu-id="f1528-241">受信メッセージごとに 1 回設定されているのではなく、時間、動きが補間と小さいがスムーズでない他のウィンドウで、図形の移動が表示されます。</span><span class="sxs-lookup"><span data-stu-id="f1528-241">The movement of the shape in the other window should appear less jerky as its movement is interpolated over time rather than being set once per incoming message.</span></span>

    ![アプリケーション ウィンドウ](tutorial-high-frequency-realtime-with-signalr/_static/image8.png)

<a id="furthersteps"></a>

## <a name="further-steps"></a><span data-ttu-id="f1528-243">以降の手順</span><span class="sxs-lookup"><span data-stu-id="f1528-243">Further Steps</span></span>

<span data-ttu-id="f1528-244">このチュートリアルでは、クライアントとサーバー間の頻度が高いメッセージを送信する SignalR アプリケーションをプログラミングする方法を学びました。</span><span class="sxs-lookup"><span data-stu-id="f1528-244">In this tutorial, you've learned how to program a SignalR application that sends high-frequency messages between clients and servers.</span></span> <span data-ttu-id="f1528-245">この通信パラダイムはなどオンライン ゲームとその他のシミュレーションを開発するために役立ちます[SignalR で作成された ShootR ゲーム](http://shootr.signalr.net)です。</span><span class="sxs-lookup"><span data-stu-id="f1528-245">This communication paradigm is useful for developing online games and other simulations, such as [the ShootR game created with SignalR](http://shootr.signalr.net).</span></span>

<span data-ttu-id="f1528-246">このチュートリアルで作成した完全なアプリケーションからダウンロードできます[コード ギャラリー](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)です。</span><span class="sxs-lookup"><span data-stu-id="f1528-246">The complete application created in this tutorial can be downloaded from [Code Gallery](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a).</span></span>

<span data-ttu-id="f1528-247">SignalR 開発の概念に関する詳細についてには、SignalR のソース コードおよびリソースの次のサイトを参照してください。</span><span class="sxs-lookup"><span data-stu-id="f1528-247">To learn more about SignalR development concepts, visit the following sites for SignalR source code and resources:</span></span>

- [<span data-ttu-id="f1528-248">SignalR Project</span><span class="sxs-lookup"><span data-stu-id="f1528-248">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="f1528-249">SignalR Github とサンプル</span><span class="sxs-lookup"><span data-stu-id="f1528-249">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="f1528-250">SignalR Wiki</span><span class="sxs-lookup"><span data-stu-id="f1528-250">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

<span data-ttu-id="f1528-251">SignalR アプリケーションを Azure にデプロイする方法のチュートリアルについては、次を参照してください。 [Azure App service Web アプリを使用してを使用して SignalR](../deployment/using-signalr-with-azure-web-sites.md)です。</span><span class="sxs-lookup"><span data-stu-id="f1528-251">For a walkthrough on how to deploy a SignalR application to Azure, see [Using SignalR with Web Apps in Azure App Service](../deployment/using-signalr-with-azure-web-sites.md).</span></span> <span data-ttu-id="f1528-252">Visual Studio web プロジェクトを Windows Azure Web サイトを展開する方法の詳細については、次を参照してください。 [Azure App Service で ASP.NET web アプリを作成](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)です。</span><span class="sxs-lookup"><span data-stu-id="f1528-252">For detailed information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Create an ASP.NET web app in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span></span>
