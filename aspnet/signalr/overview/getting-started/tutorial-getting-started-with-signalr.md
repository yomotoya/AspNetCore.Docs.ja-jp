---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: 'チュートリアル: SignalR 2 の概要 |Microsoft Docs'
author: pfletcher
description: このチュートリアルでは、SignalR を使用してリアルタイムのチャット アプリケーションを作成する方法を示します。 SignalR を空の ASP.NET web アプリケーションに追加され、HTML pa を作成しています.
ms.author: riande
ms.date: 06/10/2014
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 647dab496acd63dc774236ed448bd6b37b19c707
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832189"
---
<a name="tutorial-getting-started-with-signalr-2"></a><span data-ttu-id="119e4-104">チュートリアル: SignalR 2 の概要</span><span class="sxs-lookup"><span data-stu-id="119e4-104">Tutorial: Getting Started with SignalR 2</span></span>
====================
<span data-ttu-id="119e4-105">によって[Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="119e4-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[<span data-ttu-id="119e4-106">完成したプロジェクトのダウンロード</span><span class="sxs-lookup"><span data-stu-id="119e4-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)

> <span data-ttu-id="119e4-107">このチュートリアルでは、SignalR を使用してリアルタイムのチャット アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="119e4-107">This tutorial shows how to use SignalR to create a real-time chat application.</span></span> <span data-ttu-id="119e4-108">SignalR を空の ASP.NET web アプリケーションに追加し、送信し、メッセージを表示する HTML ページを作成します。</span><span class="sxs-lookup"><span data-stu-id="119e4-108">You will add SignalR to an empty ASP.NET web application and create an HTML page to send and display messages.</span></span> 
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="119e4-109">このチュートリアルで使用されるソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="119e4-109">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="119e4-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="119e4-110">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="119e4-111">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="119e4-111">.NET 4.5</span></span>
> - <span data-ttu-id="119e4-112">SignalR 2 のバージョン</span><span class="sxs-lookup"><span data-stu-id="119e4-112">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="119e4-113">このチュートリアルで Visual Studio 2012 の使用</span><span class="sxs-lookup"><span data-stu-id="119e4-113">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="119e4-114">このチュートリアルでは、Visual Studio 2012 を使用するには、次の操作を行います。</span><span class="sxs-lookup"><span data-stu-id="119e4-114">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="119e4-115">更新プログラム、[パッケージ マネージャー](http://docs.nuget.org/docs/start-here/installing-nuget)最新バージョンにします。</span><span class="sxs-lookup"><span data-stu-id="119e4-115">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="119e4-116">インストール、 [Web プラットフォーム インストーラー](https://www.microsoft.com/web/downloads/platform.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="119e4-116">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="119e4-117">Web Platform Installer で検索してインストール**ASP.NET と Visual Studio 2012 for Web Tools 2013.1**します。</span><span class="sxs-lookup"><span data-stu-id="119e4-117">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="119e4-118">SignalR クラスの Visual Studio テンプレートなどのインストールはこの**ハブ**します。</span><span class="sxs-lookup"><span data-stu-id="119e4-118">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="119e4-119">一部のテンプレート (など**OWIN Startup クラス**) はできなくなります。 これらの場合には、クラス ファイルを代わりに使用します。</span><span class="sxs-lookup"><span data-stu-id="119e4-119">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="119e4-120">チュートリアルのバージョン</span><span class="sxs-lookup"><span data-stu-id="119e4-120">Tutorial versions</span></span>
> 
> <span data-ttu-id="119e4-121">SignalR の以前のバージョンについては、次を参照してください。[以前のバージョンの SignalR](../older-versions/index.md)します。</span><span class="sxs-lookup"><span data-stu-id="119e4-121">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="119e4-122">意見やご質問</span><span class="sxs-lookup"><span data-stu-id="119e4-122">Questions and comments</span></span>
> 
> <span data-ttu-id="119e4-123">このチュートリアルの立った方法と、ページの下部にあるコメントで改良できるフィードバックを送信してください。</span><span class="sxs-lookup"><span data-stu-id="119e4-123">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="119e4-124">チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)します。</span><span class="sxs-lookup"><span data-stu-id="119e4-124">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="119e4-125">概要</span><span class="sxs-lookup"><span data-stu-id="119e4-125">Overview</span></span>

<span data-ttu-id="119e4-126">このチュートリアルでは、ブラウザー ベースの単純なチャット アプリケーションを構築する方法を示す、SignalR の開発について説明します。</span><span class="sxs-lookup"><span data-stu-id="119e4-126">This tutorial introduces SignalR development by showing how to build a simple browser-based chat application.</span></span> <span data-ttu-id="119e4-127">SignalR ライブラリ空の ASP.NET web アプリケーションを追加、クライアントにメッセージを送信するためのハブ クラスを作成、およびユーザー チャット メッセージを送受信できるようにする HTML ページが作成されます。</span><span class="sxs-lookup"><span data-stu-id="119e4-127">You will add the SignalR library to an empty ASP.NET web application, create a hub class for sending messages to clients, and create an HTML page that lets users send and receive chat messages.</span></span> <span data-ttu-id="119e4-128">MVC 5 でのチャット アプリケーションを作成する方法を示しています、MVC ビューを使用して類似のチュートリアルを参照してください。 [SignalR 2 と MVC 5 の概要](tutorial-getting-started-with-signalr-and-mvc.md)します。</span><span class="sxs-lookup"><span data-stu-id="119e4-128">For a similar tutorial that shows how to create a chat application in MVC 5 using an MVC view, see [Getting Started with SignalR 2 and MVC 5](tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

> [!NOTE]
> <span data-ttu-id="119e4-129">このチュートリアルでは、バージョン 2 で SignalR アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="119e4-129">This tutorial demonstrates how to create SignalR applications in version 2.</span></span> <span data-ttu-id="119e4-130">SignalR の間の変更の詳細についての 1.x と 2 を参照してください。[アップグレード SignalR 1.x プロジェクト](../releases/upgrading-signalr-1x-projects-to-20.md)と[Visual Studio 2013 リリース ノート](../../../visual-studio/overview/2013/release-notes.md#TOC13)します。</span><span class="sxs-lookup"><span data-stu-id="119e4-130">For details on changes between SignalR 1.x and 2, see [Upgrading SignalR 1.x Projects](../releases/upgrading-signalr-1x-projects-to-20.md) and [Visual Studio 2013 Release Notes](../../../visual-studio/overview/2013/release-notes.md#TOC13).</span></span>

<span data-ttu-id="119e4-131">SignalR はライブ ユーザーとの対話やリアルタイムのデータの更新プログラムを必要とする web アプリケーションを構築するためのオープン ソース .NET ライブラリです。</span><span class="sxs-lookup"><span data-stu-id="119e4-131">SignalR is an open-source .NET library for building web applications that require live user interaction or real-time data updates.</span></span> <span data-ttu-id="119e4-132">例には、ソーシャル アプリケーション、ゲームのマルチ ユーザー、ビジネス コラボレーション、およびニュース、天気、またはアプリケーションの財務の更新が含まれます。</span><span class="sxs-lookup"><span data-stu-id="119e4-132">Examples include social applications, multiuser games, business collaboration, and news, weather, or financial update applications.</span></span> <span data-ttu-id="119e4-133">これらは多くの場合、リアルタイム アプリケーションと呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="119e4-133">These are often called real-time applications.</span></span>

<span data-ttu-id="119e4-134">SignalR は、リアルタイム アプリケーションの構築プロセスを簡略化します。</span><span class="sxs-lookup"><span data-stu-id="119e4-134">SignalR simplifies the process of building real-time applications.</span></span> <span data-ttu-id="119e4-135">これには、ASP.NET のサーバー ライブラリとクライアント サーバー接続を管理し、クライアントにコンテンツの更新をプッシュするが簡単に JavaScript クライアント ライブラリが含まれます。</span><span class="sxs-lookup"><span data-stu-id="119e4-135">It includes an ASP.NET server library and a JavaScript client library to make it easier to manage client-server connections and push content updates to clients.</span></span> <span data-ttu-id="119e4-136">SignalR ライブラリは、リアルタイムの機能を利用する既存の ASP.NET アプリケーションを追加できます。</span><span class="sxs-lookup"><span data-stu-id="119e4-136">You can add the SignalR library to an existing ASP.NET application to gain real-time functionality.</span></span>

<span data-ttu-id="119e4-137">このチュートリアルでは、次の SignalR 開発タスクを示しています。</span><span class="sxs-lookup"><span data-stu-id="119e4-137">The tutorial demonstrates the following SignalR development tasks:</span></span>

- <span data-ttu-id="119e4-138">ASP.NET web アプリケーションに SignalR ライブラリを追加します。</span><span class="sxs-lookup"><span data-stu-id="119e4-138">Adding the SignalR library to an ASP.NET web application.</span></span>
- <span data-ttu-id="119e4-139">クライアントにコンテンツをプッシュするハブ クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="119e4-139">Creating a hub class to push content to clients.</span></span>
- <span data-ttu-id="119e4-140">アプリケーションを構成する OWIN startup クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="119e4-140">Creating an OWIN startup class to configure the application.</span></span>
- <span data-ttu-id="119e4-141">Web ページで、SignalR jQuery ライブラリを使用して、メッセージを送信し、ハブから更新プログラムを表示します。</span><span class="sxs-lookup"><span data-stu-id="119e4-141">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="119e4-142">次のスクリーン ショットは、ブラウザーで実行されているチャット アプリケーションを示しています。</span><span class="sxs-lookup"><span data-stu-id="119e4-142">The following screen shot shows the chat application running in a browser.</span></span> <span data-ttu-id="119e4-143">各新規ユーザー コメントを投稿でき、ユーザーがチャットに参加した後に追加されたコメントを参照してください。</span><span class="sxs-lookup"><span data-stu-id="119e4-143">Each new user can post comments and see comments added after the user joins the chat.</span></span>

![チャット インスタンス](tutorial-getting-started-with-signalr/_static/image1.png)

<span data-ttu-id="119e4-145">セクション:</span><span class="sxs-lookup"><span data-stu-id="119e4-145">Sections:</span></span>

- [<span data-ttu-id="119e4-146">プロジェクトを設定します。</span><span class="sxs-lookup"><span data-stu-id="119e4-146">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="119e4-147">サンプルを実行します。</span><span class="sxs-lookup"><span data-stu-id="119e4-147">Run the Sample</span></span>](#run)
- [<span data-ttu-id="119e4-148">コードを確認します</span><span class="sxs-lookup"><span data-stu-id="119e4-148">Examine the Code</span></span>](#code)
- [<span data-ttu-id="119e4-149">次の手順</span><span class="sxs-lookup"><span data-stu-id="119e4-149">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="119e4-150">プロジェクトを設定します。</span><span class="sxs-lookup"><span data-stu-id="119e4-150">Set up the Project</span></span>

<span data-ttu-id="119e4-151">SignalR を追加するこのセクションでは、Visual Studio 2013 と SignalR バージョン 2 を使用して、空の ASP.NET web アプリケーションを作成する方法を示していて、チャット アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="119e4-151">This section shows how to use Visual Studio 2013 and SignalR version 2 to create an empty ASP.NET web application, add SignalR, and create the chat application.</span></span>

<span data-ttu-id="119e4-152">必要条件:</span><span class="sxs-lookup"><span data-stu-id="119e4-152">Prerequisites:</span></span>

- <span data-ttu-id="119e4-153">Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="119e4-153">Visual Studio 2013.</span></span> <span data-ttu-id="119e4-154">Visual Studio がないを参照してください。 [ASP.NET ダウンロード](https://www.asp.net/downloads)無料 Visual Studio 2013 Express 開発ツールを取得します。</span><span class="sxs-lookup"><span data-stu-id="119e4-154">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2013 Express Development Tool.</span></span>

<span data-ttu-id="119e4-155">次の手順では、空の ASP.NET Web アプリケーションを作成し、SignalR ライブラリを追加する Visual Studio 2013 を使用します。</span><span class="sxs-lookup"><span data-stu-id="119e4-155">The following steps use Visual Studio 2013 to create an ASP.NET Empty Web Application and add the SignalR library:</span></span>

1. <span data-ttu-id="119e4-156">Visual Studio で ASP.NET Web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="119e4-156">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![Web を作成します。](tutorial-getting-started-with-signalr/_static/image2.png)
2. <span data-ttu-id="119e4-158">**新しい ASP.NET プロジェクト** ウィンドウのままに**空**選択およびクリックして**プロジェクトの作成**。</span><span class="sxs-lookup"><span data-stu-id="119e4-158">In the **New ASP.NET Project** window, leave **Empty** selected and click **Create Project**.</span></span>

    ![空の web を作成します。](tutorial-getting-started-with-signalr/_static/image3.png)
3. <span data-ttu-id="119e4-160">**ソリューション エクスプ ローラー**、プロジェクトを右クリックし、選択**追加 |SignalR ハブ クラス (v2)** します。</span><span class="sxs-lookup"><span data-stu-id="119e4-160">In **Solution Explorer**, right-click the project, select **Add | SignalR Hub Class (v2)**.</span></span> <span data-ttu-id="119e4-161">クラスの名前**ChatHub.cs**し、プロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="119e4-161">Name the class **ChatHub.cs** and add it to the project.</span></span> <span data-ttu-id="119e4-162">この手順で作成、 **ChatHub**クラスし、一連のスクリプト ファイルと SignalR をサポートするアセンブリ参照をプロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="119e4-162">This step creates the **ChatHub** class and adds to the project a set of script files and assembly references that support SignalR.</span></span>

    > [!NOTE]
    > <span data-ttu-id="119e4-163">開き、プロジェクトに SignalR を追加することも、**ツール |ライブラリ パッケージ マネージャー |パッケージ マネージャー コンソール**コマンドを実行しているとします。</span><span class="sxs-lookup"><span data-stu-id="119e4-163">You can also add SignalR to a project by opening the **Tools | Library Package Manager | Package Manager Console** and running a command:</span></span>

    `install-package Microsoft.AspNet.SignalR`

    <span data-ttu-id="119e4-164">SignalR を追加する、コンソールを使用する場合は、SignalR を追加した後に、別のステップとして、SignalR ハブ クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="119e4-164">If you use the console to add SignalR, create the SignalR hub class as a separate step after you add SignalR.</span></span>

    > [!NOTE]
    > <span data-ttu-id="119e4-165">Visual Studio 2012 を使用している場合、 **SignalR ハブ クラス (v2)** テンプレートは使用できません。</span><span class="sxs-lookup"><span data-stu-id="119e4-165">If you are using Visual Studio 2012, the **SignalR Hub Class (v2)** template will not be available.</span></span> <span data-ttu-id="119e4-166">プレーンなを追加する**クラス**と呼ばれる`ChatHub`代わりにします。</span><span class="sxs-lookup"><span data-stu-id="119e4-166">You can add a plain **Class** called `ChatHub` instead.</span></span>
4. <span data-ttu-id="119e4-167">**ソリューション エクスプ ローラー**スクリプトを展開します。</span><span class="sxs-lookup"><span data-stu-id="119e4-167">In **Solution Explorer**, expand the Scripts node.</span></span> <span data-ttu-id="119e4-168">JQuery と SignalR 用のスクリプト ライブラリは、プロジェクトに表示されます。</span><span class="sxs-lookup"><span data-stu-id="119e4-168">Script libraries for jQuery and SignalR are visible in the project.</span></span>
5. <span data-ttu-id="119e4-169">新しいコードを置き換える**ChatHub**クラスを次のコード。</span><span class="sxs-lookup"><span data-stu-id="119e4-169">Replace the code in the new **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. <span data-ttu-id="119e4-170">**ソリューション エクスプ ローラー**プロジェクトを右クリックし、クリックして**追加 |OWIN Startup クラス**します。</span><span class="sxs-lookup"><span data-stu-id="119e4-170">In **Solution Explorer**, right-click the project, then click **Add | OWIN Startup Class**.</span></span> <span data-ttu-id="119e4-171">新しいクラスの名前`Startup`[ok] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="119e4-171">Name the new class `Startup` and click OK.</span></span>

    > [!NOTE]
    > <span data-ttu-id="119e4-172">Visual Studio 2012 を使用している場合、 **OWIN Startup クラス**テンプレートは使用できません。</span><span class="sxs-lookup"><span data-stu-id="119e4-172">If you are using Visual Studio 2012, the **OWIN Startup Class** template will not be available.</span></span> <span data-ttu-id="119e4-173">プレーンなを追加する**クラス**と呼ばれる`Startup`代わりにします。</span><span class="sxs-lookup"><span data-stu-id="119e4-173">You can add a plain **Class** called `Startup` instead.</span></span>
7. <span data-ttu-id="119e4-174">次に、新しいスタートアップ クラスの内容を変更します。</span><span class="sxs-lookup"><span data-stu-id="119e4-174">Change the contents of the new Startup class to the following.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. <span data-ttu-id="119e4-175">**ソリューション エクスプ ローラー**プロジェクトを右クリックし、クリックして**追加 |HTML ページ**します。</span><span class="sxs-lookup"><span data-stu-id="119e4-175">In **Solution Explorer**, right-click the project, then click **Add | HTML Page**.</span></span> <span data-ttu-id="119e4-176">名前を新しいページ`index.html`します。</span><span class="sxs-lookup"><span data-stu-id="119e4-176">Name the new page `index.html`.</span></span>
    >[!NOTE]
    ><span data-ttu-id="119e4-177">JQuery と SignalR ライブラリへの参照のバージョン番号を変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="119e4-177">You might need to change the version numbers for the references to JQuery and SignalR libraries</span></span>
9. <span data-ttu-id="119e4-178">**ソリューション エクスプ ローラー**で作成した HTML ページを右クリックし、をクリックして**スタート ページとして設定**します。</span><span class="sxs-lookup"><span data-stu-id="119e4-178">In **Solution Explorer**, right-click the HTML page you just created and click **Set as Start Page**.</span></span>
10. <span data-ttu-id="119e4-179">HTML ページの既定のコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="119e4-179">Replace the default code in the HTML page with the following code.</span></span>

    > [!NOTE]
    > <span data-ttu-id="119e4-180">以降のバージョンの SignalR スクリプトは、パッケージ マネージャーによってインストールされます。</span><span class="sxs-lookup"><span data-stu-id="119e4-180">A later version of the SignalR scripts may be installed by the package manager.</span></span> <span data-ttu-id="119e4-181">次のスクリプト参照が (される SignalR ハブを追加するのではなく、NuGet を使用して追加した場合は、異なるします。) プロジェクトのスクリプト ファイルのバージョンに対応することを確認します。</span><span class="sxs-lookup"><span data-stu-id="119e4-181">Verify that the script references below correspond to the versions of the script files in the project (they will be different if you added SignalR using NuGet rather than adding a hub.)</span></span>

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]
11. <span data-ttu-id="119e4-182">**すべて保存**プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="119e4-182">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="119e4-183">サンプルを実行します。</span><span class="sxs-lookup"><span data-stu-id="119e4-183">Run the Sample</span></span>

1. <span data-ttu-id="119e4-184">F5 キーを押して、デバッグ モードで、プロジェクトを実行します。</span><span class="sxs-lookup"><span data-stu-id="119e4-184">Press F5 to run the project in debug mode.</span></span> <span data-ttu-id="119e4-185">ブラウザーのインスタンスとユーザー名のプロンプトで、HTML ページが読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="119e4-185">The HTML page loads in a browser instance and prompts for a user name.</span></span>

    ![ユーザー名の入力](tutorial-getting-started-with-signalr/_static/image4.png)
2. <span data-ttu-id="119e4-187">ユーザー名を入力します。</span><span class="sxs-lookup"><span data-stu-id="119e4-187">Enter a user name.</span></span>
3. <span data-ttu-id="119e4-188">ブラウザーのアドレスの行から URL をコピーし、2 つのブラウザー インスタンスを開くために使用します。</span><span class="sxs-lookup"><span data-stu-id="119e4-188">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="119e4-189">各ブラウザー インスタンスでは、一意のユーザー名を入力します。</span><span class="sxs-lookup"><span data-stu-id="119e4-189">In each browser instance, enter a unique user name.</span></span>
4. <span data-ttu-id="119e4-190">各ブラウザー インスタンスでコメントを追加し、をクリックして**送信**します。</span><span class="sxs-lookup"><span data-stu-id="119e4-190">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="119e4-191">すべてのブラウザー インスタンスで、コメントが表示されます。</span><span class="sxs-lookup"><span data-stu-id="119e4-191">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="119e4-192">この簡単なチャット アプリケーションでは、サーバー上のディスカッション コンテキストは保持されません。</span><span class="sxs-lookup"><span data-stu-id="119e4-192">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="119e4-193">ハブは、現在のすべてのユーザーへのコメントをブロードキャストします。</span><span class="sxs-lookup"><span data-stu-id="119e4-193">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="119e4-194">後で、チャットに参加するユーザーは、参加する時点から追加されたメッセージに表示されます。</span><span class="sxs-lookup"><span data-stu-id="119e4-194">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="119e4-195">次のスクリーン ショットは、1 つのインスタンス メッセージを送信するすべての更新は、次の 3 つのブラウザー インスタンスで実行されるチャット アプリケーションを示しています。</span><span class="sxs-lookup"><span data-stu-id="119e4-195">The following screen shot shows the chat application running in three browser instances, all of which are updated when one instance sends a message:</span></span>

    ![チャット ブラウザー](tutorial-getting-started-with-signalr/_static/image5.png)
5. <span data-ttu-id="119e4-197">**ソリューション エクスプ ローラー**、検査、**スクリプト ドキュメント**実行中のアプリケーションのノード。</span><span class="sxs-lookup"><span data-stu-id="119e4-197">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="119e4-198">という名前のスクリプト ファイルがある**hubs** SignalR ライブラリは、実行時に動的に生成します。</span><span class="sxs-lookup"><span data-stu-id="119e4-198">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="119e4-199">このファイルは、jQuery スクリプトとサーバー側コード間の通信を管理します。</span><span class="sxs-lookup"><span data-stu-id="119e4-199">This file manages the communication between jQuery script and server-side code.</span></span>

    ![](tutorial-getting-started-with-signalr/_static/image6.png)

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="119e4-200">コードを確認します</span><span class="sxs-lookup"><span data-stu-id="119e4-200">Examine the Code</span></span>

<span data-ttu-id="119e4-201">SignalR チャット アプリケーションを 2 つの基本的な SignalR 開発タスクを示します。 サーバーで、主要な調整オブジェクトとしてハブを作成し、メッセージを送受信する SignalR jQuery ライブラリを使用します。</span><span class="sxs-lookup"><span data-stu-id="119e4-201">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="119e4-202">SignalR ハブ</span><span class="sxs-lookup"><span data-stu-id="119e4-202">SignalR Hubs</span></span>

<span data-ttu-id="119e4-203">コード サンプルでは、 **ChatHub**クラスから派生、 **Microsoft.AspNet.SignalR.Hub**クラス。</span><span class="sxs-lookup"><span data-stu-id="119e4-203">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="119e4-204">派生する、**ハブ**クラスは、SignalR アプリケーションを構築する便利な方法です。</span><span class="sxs-lookup"><span data-stu-id="119e4-204">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="119e4-205">ハブ クラスにパブリック メソッドを作成し、web ページ内のスクリプトから呼び出すことによってこれらのメソッドにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="119e4-205">You can create public methods on your hub class and then access those methods by calling them from scripts in a web page.</span></span>

<span data-ttu-id="119e4-206">クライアントが呼び出すチャットのコードで、 **ChatHub.Send**メソッドを新しいメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="119e4-206">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="119e4-207">ハブに送信メッセージのすべてのクライアントを呼び出して**Clients.All.broadcastMessage**します。</span><span class="sxs-lookup"><span data-stu-id="119e4-207">The hub in turn sends the message to all clients by calling **Clients.All.broadcastMessage**.</span></span>

<span data-ttu-id="119e4-208">**送信**メソッドがいくつかのハブの概念を示します。</span><span class="sxs-lookup"><span data-stu-id="119e4-208">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="119e4-209">クライアントが呼び出すことができるように、ハブ上のパブリック メソッドを宣言します。</span><span class="sxs-lookup"><span data-stu-id="119e4-209">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="119e4-210">使用して、 **Microsoft.AspNet.SignalR.Hub.Clients**この hub に接続されているすべてのクライアントにアクセスする動的プロパティ。</span><span class="sxs-lookup"><span data-stu-id="119e4-210">Use the **Microsoft.AspNet.SignalR.Hub.Clients** dynamic property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="119e4-211">クライアントで関数を呼び出す (など、`broadcastMessage`関数) クライアントを更新します。</span><span class="sxs-lookup"><span data-stu-id="119e4-211">Call a function on the client (such as the `broadcastMessage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="119e4-212">SignalR と jQuery</span><span class="sxs-lookup"><span data-stu-id="119e4-212">SignalR and jQuery</span></span>

<span data-ttu-id="119e4-213">コード サンプルでは、HTML ページは、SignalR jQuery ライブラリを使用して、SignalR hub と通信する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="119e4-213">The HTML page in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="119e4-214">コードで基本的なタスクは、プロキシをクライアントにコンテンツをプッシュするサーバーを呼び出すことができる関数を宣言して、ハブにメッセージを送信する接続を開始、ハブの参照を宣言することです。</span><span class="sxs-lookup"><span data-stu-id="119e4-214">The essential tasks in the code are declaring a proxy to reference the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="119e4-215">次のコードでは、ハブ プロキシへの参照を宣言します。</span><span class="sxs-lookup"><span data-stu-id="119e4-215">The following code declares a reference to a hub proxy.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> <span data-ttu-id="119e4-216">JavaScript でサーバー クラスとそのメンバーへの参照がキャメル ケースです。</span><span class="sxs-lookup"><span data-stu-id="119e4-216">In JavaScript the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="119e4-217">コード サンプルを参照して、c# **ChatHub**として JavaScript でクラス**chatHub**します。</span><span class="sxs-lookup"><span data-stu-id="119e4-217">The code sample references the C# **ChatHub** class in JavaScript as **chatHub**.</span></span>


<span data-ttu-id="119e4-218">次のコードは、スクリプトのコールバック関数を作成する方法です。</span><span class="sxs-lookup"><span data-stu-id="119e4-218">The following code is how you create a callback function in the script.</span></span> <span data-ttu-id="119e4-219">サーバー上のハブ クラスは、各クライアントにコンテンツの更新をプッシュするには、この関数を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="119e4-219">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="119e4-220">HTML が表示する前にコンテンツをエンコードする 2 つの行は省略可能ですし、スクリプト インジェクションを防止する簡単な方法を表示します。</span><span class="sxs-lookup"><span data-stu-id="119e4-220">The two lines that HTML encode the content before displaying it are optional and show a simple way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

<span data-ttu-id="119e4-221">次のコードでは、ハブとの接続を開く方法を示します。</span><span class="sxs-lookup"><span data-stu-id="119e4-221">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="119e4-222">コードが接続を開始しのクリック イベントを処理する関数に渡します、**送信**HTML ページにボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="119e4-222">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the HTML page.</span></span>

> [!NOTE]
> <span data-ttu-id="119e4-223">このアプローチにより、イベント ハンドラーが実行される前に、接続が確立されていること。</span><span class="sxs-lookup"><span data-stu-id="119e4-223">This approach ensures that the connection is established before the event handler executes.</span></span>


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="119e4-224">次の手順</span><span class="sxs-lookup"><span data-stu-id="119e4-224">Next Steps</span></span>

<span data-ttu-id="119e4-225">SignalR がリアルタイムの web アプリケーションを構築するためのフレームワークについて説明しました。</span><span class="sxs-lookup"><span data-stu-id="119e4-225">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="119e4-226">いくつかの SignalR 開発タスクも学習しました。 SignalR を ASP.NET アプリケーションを追加する方法、ハブ クラスを作成する方法と、ハブからメッセージを送受信する方法。</span><span class="sxs-lookup"><span data-stu-id="119e4-226">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="119e4-227">サンプルの SignalR アプリケーションを Azure にデプロイする方法のチュートリアルは、次を参照してください。 [Azure App Service Web apps を使用して SignalR](../deployment/using-signalr-with-azure-web-sites.md)します。</span><span class="sxs-lookup"><span data-stu-id="119e4-227">For a walkthrough on how to deploy the sample SignalR application to Azure, see [Using SignalR with Web Apps in Azure App Service](../deployment/using-signalr-with-azure-web-sites.md).</span></span> <span data-ttu-id="119e4-228">Visual Studio web プロジェクトを Windows Azure の Web サイトにデプロイする方法の詳細については、次を参照してください。 [Azure App Service で ASP.NET web アプリを作成](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)です。</span><span class="sxs-lookup"><span data-stu-id="119e4-228">For detailed information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Create an ASP.NET web app in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span></span>

<span data-ttu-id="119e4-229">高度な SignalR 開発の概念については、SignalR のソース コードおよびリソースは、次のサイトを参照してください。</span><span class="sxs-lookup"><span data-stu-id="119e4-229">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources:</span></span>

- [<span data-ttu-id="119e4-230">SignalR プロジェクト</span><span class="sxs-lookup"><span data-stu-id="119e4-230">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="119e4-231">SignalR Github とサンプル</span><span class="sxs-lookup"><span data-stu-id="119e4-231">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="119e4-232">SignalR の Wiki</span><span class="sxs-lookup"><span data-stu-id="119e4-232">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
