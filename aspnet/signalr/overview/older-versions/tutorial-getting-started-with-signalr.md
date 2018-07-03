---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr
title: 'チュートリアル: SignalR の概要 1.x |Microsoft Docs'
author: pfletcher
description: ASP.NET SignalR を使用すると、HTML ページで、リアルタイムのチャット アプリケーションを作成します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: fdc3599a-5217-44c1-951f-0eec9812dce7
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 3cfeb95bcaa984de5cff246173ad03e2a774fc0c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402023"
---
<a name="tutorial-getting-started-with-signalr-1x"></a><span data-ttu-id="40f70-103">チュートリアル: SignalR の概要 1.x</span><span class="sxs-lookup"><span data-stu-id="40f70-103">Tutorial: Getting Started with SignalR 1.x</span></span>
====================
<span data-ttu-id="40f70-104">によって[Patrick Fletcher](https://github.com/pfletcher)、 [Tim Teebken](https://github.com/timlt)</span><span class="sxs-lookup"><span data-stu-id="40f70-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span></span>

> <span data-ttu-id="40f70-105">このチュートリアルでは、SignalR を使用してリアルタイムのチャット アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="40f70-105">This tutorial shows how to use SignalR to create a real-time chat application.</span></span> <span data-ttu-id="40f70-106">SignalR を空の ASP.NET web アプリケーションに追加し、送信し、メッセージを表示する HTML ページを作成します。</span><span class="sxs-lookup"><span data-stu-id="40f70-106">You will add SignalR to an empty ASP.NET web application and create an HTML page to send and display messages.</span></span>


## <a name="overview"></a><span data-ttu-id="40f70-107">概要</span><span class="sxs-lookup"><span data-stu-id="40f70-107">Overview</span></span>

<span data-ttu-id="40f70-108">このチュートリアルでは、ブラウザー ベースの単純なチャット アプリケーションを構築する方法を示す、SignalR の開発について説明します。</span><span class="sxs-lookup"><span data-stu-id="40f70-108">This tutorial introduces SignalR development by showing how to build a simple browser-based chat application.</span></span> <span data-ttu-id="40f70-109">SignalR ライブラリ空の ASP.NET web アプリケーションを追加、クライアントにメッセージを送信するためのハブ クラスを作成、およびユーザー チャット メッセージを送受信できるようにする HTML ページが作成されます。</span><span class="sxs-lookup"><span data-stu-id="40f70-109">You will add the SignalR library to an empty ASP.NET web application, create a hub class for sending messages to clients, and create an HTML page that lets users send and receive chat messages.</span></span> <span data-ttu-id="40f70-110">MVC 4 のチャット アプリケーションを作成する方法を示しています、MVC ビューを使用して類似のチュートリアルを参照してください。 [SignalR と MVC 4 の概要](index.md)します。</span><span class="sxs-lookup"><span data-stu-id="40f70-110">For a similar tutorial that shows how to create a chat application in MVC 4 using an MVC view, see [Getting Started with SignalR and MVC 4](index.md).</span></span>

> [!NOTE]
> <span data-ttu-id="40f70-111">このチュートリアルでは、SignalR のリリース (1.x) バージョンを使用します。</span><span class="sxs-lookup"><span data-stu-id="40f70-111">This tutorial uses the release (1.x) version of SignalR.</span></span> <span data-ttu-id="40f70-112">SignalR の間の変更の詳細についての 1.x と 2.0 を参照してください。[アップグレード SignalR 1.x プロジェクト](../releases/upgrading-signalr-1x-projects-to-20.md)します。</span><span class="sxs-lookup"><span data-stu-id="40f70-112">For details on changes between SignalR 1.x and 2.0, see [Upgrading SignalR 1.x Projects](../releases/upgrading-signalr-1x-projects-to-20.md).</span></span>

<span data-ttu-id="40f70-113">SignalR はライブ ユーザーとの対話やリアルタイムのデータの更新プログラムを必要とする web アプリケーションを構築するためのオープン ソース .NET ライブラリです。</span><span class="sxs-lookup"><span data-stu-id="40f70-113">SignalR is an open-source .NET library for building web applications that require live user interaction or real-time data updates.</span></span> <span data-ttu-id="40f70-114">例には、ソーシャル アプリケーション、ゲームのマルチ ユーザー、ビジネス コラボレーション、およびニュース、天気、またはアプリケーションの財務の更新が含まれます。</span><span class="sxs-lookup"><span data-stu-id="40f70-114">Examples include social applications, multiuser games, business collaboration, and news, weather, or financial update applications.</span></span> <span data-ttu-id="40f70-115">これらは多くの場合、リアルタイム アプリケーションと呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="40f70-115">These are often called real-time applications.</span></span>

<span data-ttu-id="40f70-116">SignalR は、リアルタイム アプリケーションの構築プロセスを簡略化します。</span><span class="sxs-lookup"><span data-stu-id="40f70-116">SignalR simplifies the process of building real-time applications.</span></span> <span data-ttu-id="40f70-117">これには、ASP.NET のサーバー ライブラリとクライアント サーバー接続を管理し、クライアントにコンテンツの更新をプッシュするが簡単に JavaScript クライアント ライブラリが含まれます。</span><span class="sxs-lookup"><span data-stu-id="40f70-117">It includes an ASP.NET server library and a JavaScript client library to make it easier to manage client-server connections and push content updates to clients.</span></span> <span data-ttu-id="40f70-118">SignalR ライブラリは、リアルタイムの機能を利用する既存の ASP.NET アプリケーションを追加できます。</span><span class="sxs-lookup"><span data-stu-id="40f70-118">You can add the SignalR library to an existing ASP.NET application to gain real-time functionality.</span></span>

<span data-ttu-id="40f70-119">このチュートリアルでは、次の SignalR 開発タスクを示しています。</span><span class="sxs-lookup"><span data-stu-id="40f70-119">The tutorial demonstrates the following SignalR development tasks:</span></span>

- <span data-ttu-id="40f70-120">ASP.NET web アプリケーションに SignalR ライブラリを追加します。</span><span class="sxs-lookup"><span data-stu-id="40f70-120">Adding the SignalR library to an ASP.NET web application.</span></span>
- <span data-ttu-id="40f70-121">クライアントにコンテンツをプッシュするハブ クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="40f70-121">Creating a hub class to push content to clients.</span></span>
- <span data-ttu-id="40f70-122">Web ページで、SignalR jQuery ライブラリを使用して、メッセージを送信し、ハブから更新プログラムを表示します。</span><span class="sxs-lookup"><span data-stu-id="40f70-122">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="40f70-123">次のスクリーン ショットは、ブラウザーで実行されているチャット アプリケーションを示しています。</span><span class="sxs-lookup"><span data-stu-id="40f70-123">The following screen shot shows the chat application running in a browser.</span></span> <span data-ttu-id="40f70-124">各新規ユーザー コメントを投稿でき、ユーザーがチャットに参加した後に追加されたコメントを参照してください。</span><span class="sxs-lookup"><span data-stu-id="40f70-124">Each new user can post comments and see comments added after the user joins the chat.</span></span>

![チャット インスタンス](tutorial-getting-started-with-signalr/_static/image1.png)

<span data-ttu-id="40f70-126">セクション:</span><span class="sxs-lookup"><span data-stu-id="40f70-126">Sections:</span></span>

- [<span data-ttu-id="40f70-127">プロジェクトを設定します。</span><span class="sxs-lookup"><span data-stu-id="40f70-127">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="40f70-128">サンプルを実行します。</span><span class="sxs-lookup"><span data-stu-id="40f70-128">Run the Sample</span></span>](#run)
- [<span data-ttu-id="40f70-129">コードを確認します</span><span class="sxs-lookup"><span data-stu-id="40f70-129">Examine the Code</span></span>](#code)
- [<span data-ttu-id="40f70-130">次の手順</span><span class="sxs-lookup"><span data-stu-id="40f70-130">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="40f70-131">プロジェクトを設定します。</span><span class="sxs-lookup"><span data-stu-id="40f70-131">Set up the Project</span></span>

<span data-ttu-id="40f70-132">SignalR を追加するこのセクションでは、空の ASP.NET web アプリケーションを作成する方法を示していて、チャット アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="40f70-132">This section shows how to create an empty ASP.NET web application, add SignalR, and create the chat application.</span></span>

<span data-ttu-id="40f70-133">必要条件:</span><span class="sxs-lookup"><span data-stu-id="40f70-133">Prerequisites:</span></span>

- <span data-ttu-id="40f70-134">Visual Studio 2010 SP1 または 2012。</span><span class="sxs-lookup"><span data-stu-id="40f70-134">Visual Studio 2010 SP1 or 2012.</span></span> <span data-ttu-id="40f70-135">Visual Studio がないを参照してください。 [ASP.NET ダウンロード](https://www.asp.net/downloads)無料 Visual Studio 2012 Express 開発ツールを取得します。</span><span class="sxs-lookup"><span data-stu-id="40f70-135">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2012 Express Development Tool.</span></span>
- <span data-ttu-id="40f70-136">[Microsoft ASP.NET および Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=279941)します。</span><span class="sxs-lookup"><span data-stu-id="40f70-136">[Microsoft ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=279941).</span></span> <span data-ttu-id="40f70-137">Visual Studio 2012 の場合は、このインストーラーは、Visual Studio に SignalR テンプレートを含む ASP.NET の新機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="40f70-137">For Visual Studio 2012, this installer adds new ASP.NET features including SignalR templates to Visual Studio.</span></span> <span data-ttu-id="40f70-138">Visual Studio 2010 SP1 では、インストーラーは使用できませんが、セットアップ手順」の説明に従って、SignalR の NuGet パッケージをインストールすることで、チュートリアルを行うことができます。</span><span class="sxs-lookup"><span data-stu-id="40f70-138">For Visual Studio 2010 SP1, an installer is not available but you can complete the tutorial by installing the SignalR NuGet package as described in the setup steps.</span></span>

<span data-ttu-id="40f70-139">次の手順では、空の ASP.NET Web アプリケーションを作成し、SignalR ライブラリを追加する Visual Studio 2012 を使用します。</span><span class="sxs-lookup"><span data-stu-id="40f70-139">The following steps use Visual Studio 2012 to create an ASP.NET Empty Web Application and add the SignalR library:</span></span>

1. <span data-ttu-id="40f70-140">Visual Studio では、空の ASP.NET Web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="40f70-140">In Visual Studio create an ASP.NET Empty Web Application.</span></span>

    ![空の web を作成します。](tutorial-getting-started-with-signalr/_static/image2.png)
2. <span data-ttu-id="40f70-142">開く、**パッケージ マネージャー コンソール**を選択して**ツール |ライブラリ パッケージ マネージャー |パッケージ マネージャー コンソール**します。</span><span class="sxs-lookup"><span data-stu-id="40f70-142">Open the **Package Manager Console** by selecting **Tools | Library Package Manager | Package Manager Console**.</span></span> <span data-ttu-id="40f70-143">コンソール ウィンドウには、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="40f70-143">Enter the following command into the console window:</span></span>

    `Install-Package Microsoft.AspNet.SignalR -Version 1.1.3`

    <span data-ttu-id="40f70-144">このコマンドは、SignalR の最新バージョンをインストールします。 1.x します。</span><span class="sxs-lookup"><span data-stu-id="40f70-144">This command installs the latest version of SignalR 1.x.</span></span>
3. <span data-ttu-id="40f70-145">**ソリューション エクスプ ローラー**、プロジェクトを右クリックし、選択**追加 |クラス**します。</span><span class="sxs-lookup"><span data-stu-id="40f70-145">In **Solution Explorer**, right-click the project, select **Add | Class**.</span></span> <span data-ttu-id="40f70-146">新しいクラスの名前**ChatHub**します。</span><span class="sxs-lookup"><span data-stu-id="40f70-146">Name the new class **ChatHub**.</span></span>
4. <span data-ttu-id="40f70-147">**ソリューション エクスプ ローラー**スクリプト ノードを展開します。</span><span class="sxs-lookup"><span data-stu-id="40f70-147">In **Solution Explorer** expand the Scripts node.</span></span> <span data-ttu-id="40f70-148">JQuery と SignalR 用のスクリプト ライブラリは、プロジェクトに表示されます。</span><span class="sxs-lookup"><span data-stu-id="40f70-148">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    ![ライブラリの参照](tutorial-getting-started-with-signalr/_static/image3.png)
5. <span data-ttu-id="40f70-150">コードに置き換えます、 **ChatHub**クラスを次のコード。</span><span class="sxs-lookup"><span data-stu-id="40f70-150">Replace the code in the **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. <span data-ttu-id="40f70-151">**ソリューション エクスプ ローラー**プロジェクトを右クリックし、クリックして**追加 |新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="40f70-151">In **Solution Explorer**, right-click the project, then click **Add | New Item**.</span></span> <span data-ttu-id="40f70-152">**新しい項目の追加**ダイアログ ボックスで、**グローバル アプリケーション クラス**クリック**追加**します。</span><span class="sxs-lookup"><span data-stu-id="40f70-152">In the **Add New Item** dialog, select **Global Application Class** and click **Add**.</span></span>

    ![グローバルの追加します。](tutorial-getting-started-with-signalr/_static/image4.png)
7. <span data-ttu-id="40f70-154">次の追加`using`後にある、指定されたステートメント`using`Global.asax.cs クラス内のステートメント。</span><span class="sxs-lookup"><span data-stu-id="40f70-154">Add the following `using` statements after the provided `using` statements in the Global.asax.cs class.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. <span data-ttu-id="40f70-155">次のコードの行を追加、 `Application_Start` SignalR ハブの既定のルートを登録するグローバル クラスのメソッド。</span><span class="sxs-lookup"><span data-stu-id="40f70-155">Add the following line of code in the `Application_Start` method of the Global class to register the default route for SignalR hubs.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample3.cs)]
9. <span data-ttu-id="40f70-156">**ソリューション エクスプ ローラー**プロジェクトを右クリックし、クリックして**追加 |新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="40f70-156">In **Solution Explorer**, right-click the project, then click **Add | New Item**.</span></span> <span data-ttu-id="40f70-157">**新しい項目の追加**ダイアログ、選択の Html ページとクリック**追加**します。</span><span class="sxs-lookup"><span data-stu-id="40f70-157">In the **Add New Item** dialog, select Html Page and click **Add**.</span></span>
10. <span data-ttu-id="40f70-158">**ソリューション エクスプ ローラー**で作成した HTML ページを右クリックし、をクリックして**スタート ページとして設定**します。</span><span class="sxs-lookup"><span data-stu-id="40f70-158">In **Solution Explorer**, right-click the HTML page you just created and click **Set as Start Page**.</span></span>
11. <span data-ttu-id="40f70-159">HTML ページの既定のコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="40f70-159">Replace the default code in the HTML page with the following code.</span></span>

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample4.html)]
12. <span data-ttu-id="40f70-160">**すべて保存**プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="40f70-160">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="40f70-161">サンプルを実行します。</span><span class="sxs-lookup"><span data-stu-id="40f70-161">Run the Sample</span></span>

1. <span data-ttu-id="40f70-162">F5 キーを押して、デバッグ モードで、プロジェクトを実行します。</span><span class="sxs-lookup"><span data-stu-id="40f70-162">Press F5 to run the project in debug mode.</span></span> <span data-ttu-id="40f70-163">ブラウザーのインスタンスとユーザー名のプロンプトで、HTML ページが読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="40f70-163">The HTML page loads in a browser instance and prompts for a user name.</span></span>

    ![ユーザー名の入力](tutorial-getting-started-with-signalr/_static/image5.png)
2. <span data-ttu-id="40f70-165">ユーザー名を入力します。</span><span class="sxs-lookup"><span data-stu-id="40f70-165">Enter a user name.</span></span>
3. <span data-ttu-id="40f70-166">ブラウザーのアドレスの行から URL をコピーし、2 つのブラウザー インスタンスを開くために使用します。</span><span class="sxs-lookup"><span data-stu-id="40f70-166">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="40f70-167">各ブラウザー インスタンスでは、一意のユーザー名を入力します。</span><span class="sxs-lookup"><span data-stu-id="40f70-167">In each browser instance, enter a unique user name.</span></span>
4. <span data-ttu-id="40f70-168">各ブラウザー インスタンスでコメントを追加し、をクリックして**送信**します。</span><span class="sxs-lookup"><span data-stu-id="40f70-168">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="40f70-169">すべてのブラウザー インスタンスで、コメントが表示されます。</span><span class="sxs-lookup"><span data-stu-id="40f70-169">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="40f70-170">この簡単なチャット アプリケーションでは、サーバー上のディスカッション コンテキストは保持されません。</span><span class="sxs-lookup"><span data-stu-id="40f70-170">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="40f70-171">ハブは、現在のすべてのユーザーへのコメントをブロードキャストします。</span><span class="sxs-lookup"><span data-stu-id="40f70-171">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="40f70-172">後で、チャットに参加するユーザーは、参加する時点から追加されたメッセージに表示されます。</span><span class="sxs-lookup"><span data-stu-id="40f70-172">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="40f70-173">次のスクリーン ショットは、1 つのインスタンス メッセージを送信するすべての更新は、次の 3 つのブラウザー インスタンスで実行されるチャット アプリケーションを示しています。</span><span class="sxs-lookup"><span data-stu-id="40f70-173">The following screen shot shows the chat application running in three browser instances, all of which are updated when one instance sends a message:</span></span>

    ![チャット ブラウザー](tutorial-getting-started-with-signalr/_static/image6.png)
5. <span data-ttu-id="40f70-175">**ソリューション エクスプ ローラー**、検査、**スクリプト ドキュメント**実行中のアプリケーションのノード。</span><span class="sxs-lookup"><span data-stu-id="40f70-175">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="40f70-176">という名前のスクリプト ファイルがある**hubs** SignalR ライブラリは、実行時に動的に生成します。</span><span class="sxs-lookup"><span data-stu-id="40f70-176">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="40f70-177">このファイルは、jQuery スクリプトとサーバー側コード間の通信を管理します。</span><span class="sxs-lookup"><span data-stu-id="40f70-177">This file manages the communication between jQuery script and server-side code.</span></span>

    ![生成されたハブのスクリプト](tutorial-getting-started-with-signalr/_static/image7.png)

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="40f70-179">コードを確認します</span><span class="sxs-lookup"><span data-stu-id="40f70-179">Examine the Code</span></span>

<span data-ttu-id="40f70-180">SignalR チャット アプリケーションを 2 つの基本的な SignalR 開発タスクを示します。 サーバーで、主要な調整オブジェクトとしてハブを作成し、メッセージを送受信する SignalR jQuery ライブラリを使用します。</span><span class="sxs-lookup"><span data-stu-id="40f70-180">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="40f70-181">SignalR ハブ</span><span class="sxs-lookup"><span data-stu-id="40f70-181">SignalR Hubs</span></span>

<span data-ttu-id="40f70-182">コード サンプルでは、 **ChatHub**クラスから派生、 **Microsoft.AspNet.SignalR.Hub**クラス。</span><span class="sxs-lookup"><span data-stu-id="40f70-182">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="40f70-183">派生する、**ハブ**クラスは、SignalR アプリケーションを構築する便利な方法です。</span><span class="sxs-lookup"><span data-stu-id="40f70-183">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="40f70-184">ハブ クラスにパブリック メソッドを作成し、web ページに jQuery スクリプトから呼び出すことによってこれらのメソッドにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="40f70-184">You can create public methods on your hub class and then access those methods by calling them from jQuery scripts in a web page.</span></span>

<span data-ttu-id="40f70-185">クライアントが呼び出すチャットのコードで、 **ChatHub.Send**メソッドを新しいメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="40f70-185">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="40f70-186">ハブに送信メッセージのすべてのクライアントを呼び出して**Clients.All.broadcastMessage**します。</span><span class="sxs-lookup"><span data-stu-id="40f70-186">The hub in turn sends the message to all clients by calling **Clients.All.broadcastMessage**.</span></span>

<span data-ttu-id="40f70-187">**送信**メソッドがいくつかのハブの概念を示します。</span><span class="sxs-lookup"><span data-stu-id="40f70-187">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="40f70-188">クライアントが呼び出すことができるように、ハブ上のパブリック メソッドを宣言します。</span><span class="sxs-lookup"><span data-stu-id="40f70-188">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="40f70-189">使用して、 **Microsoft.AspNet.SignalR.Hub.Clients**この hub に接続されているすべてのクライアントにアクセスする動的プロパティ。</span><span class="sxs-lookup"><span data-stu-id="40f70-189">Use the **Microsoft.AspNet.SignalR.Hub.Clients** dynamic property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="40f70-190">クライアントの jQuery 関数を呼び出す (など、`broadcastMessage`関数) クライアントを更新します。</span><span class="sxs-lookup"><span data-stu-id="40f70-190">Call a jQuery function on the client (such as the `broadcastMessage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="40f70-191">SignalR と jQuery</span><span class="sxs-lookup"><span data-stu-id="40f70-191">SignalR and jQuery</span></span>

<span data-ttu-id="40f70-192">コード サンプルでは、HTML ページは、SignalR jQuery ライブラリを使用して、SignalR hub と通信する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="40f70-192">The HTML page in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="40f70-193">コードで基本的なタスクは、プロキシをクライアントにコンテンツをプッシュするサーバーを呼び出すことができる関数を宣言して、ハブにメッセージを送信する接続を開始、ハブの参照を宣言することです。</span><span class="sxs-lookup"><span data-stu-id="40f70-193">The essential tasks in the code are declaring a proxy to reference the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="40f70-194">次のコードは、ハブのプロキシを宣言します。</span><span class="sxs-lookup"><span data-stu-id="40f70-194">The following code declares a proxy for a hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="40f70-195">JQuery でサーバー クラスとそのメンバーへの参照がキャメル ケースです。</span><span class="sxs-lookup"><span data-stu-id="40f70-195">In jQuery the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="40f70-196">コード サンプルを参照して、c# **ChatHub**クラスとしての jquery **chatHub**します。</span><span class="sxs-lookup"><span data-stu-id="40f70-196">The code sample references the C# **ChatHub** class in jQuery as **chatHub**.</span></span>


<span data-ttu-id="40f70-197">次のコードは、スクリプトのコールバック関数を作成する方法です。</span><span class="sxs-lookup"><span data-stu-id="40f70-197">The following code is how you create a callback function in the script.</span></span> <span data-ttu-id="40f70-198">サーバー上のハブ クラスは、各クライアントにコンテンツの更新をプッシュするには、この関数を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="40f70-198">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="40f70-199">HTML が表示する前にコンテンツをエンコードする 2 つの行は省略可能ですし、スクリプト インジェクションを防止する簡単な方法を表示します。</span><span class="sxs-lookup"><span data-stu-id="40f70-199">The two lines that HTML encode the content before displaying it are optional and show a simple way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample7.html)]

<span data-ttu-id="40f70-200">次のコードでは、ハブとの接続を開く方法を示します。</span><span class="sxs-lookup"><span data-stu-id="40f70-200">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="40f70-201">コードが接続を開始しのクリック イベントを処理する関数に渡します、**送信**HTML ページにボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="40f70-201">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the HTML page.</span></span>

> [!NOTE]
> <span data-ttu-id="40f70-202">このアプローチでは、イベント ハンドラーが実行される前に、接続が確立されていることを保証します。</span><span class="sxs-lookup"><span data-stu-id="40f70-202">This approach insures that the connection is established before the event handler executes.</span></span>


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="40f70-203">次の手順</span><span class="sxs-lookup"><span data-stu-id="40f70-203">Next Steps</span></span>

<span data-ttu-id="40f70-204">SignalR がリアルタイムの web アプリケーションを構築するためのフレームワークについて説明しました。</span><span class="sxs-lookup"><span data-stu-id="40f70-204">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="40f70-205">いくつかの SignalR 開発タスクも学習しました。 SignalR を ASP.NET アプリケーションを追加する方法、ハブ クラスを作成する方法と、ハブからメッセージを送受信する方法。</span><span class="sxs-lookup"><span data-stu-id="40f70-205">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="40f70-206">利用できるサンプル アプリケーションは、このチュートリアルやその他の SignalR アプリケーションで、インターネット経由でしてホスティング プロバイダーにデプロイすること。</span><span class="sxs-lookup"><span data-stu-id="40f70-206">You can make the sample application in this tutorial or other SignalR applications available over the Internet by deploying them to a hosting provider.</span></span> <span data-ttu-id="40f70-207">Microsoft 提供の無料 web ホスティングを無料で最大 10 個の web サイトを[Windows Azure トライアル アカウント](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604)します。</span><span class="sxs-lookup"><span data-stu-id="40f70-207">Microsoft offers free web hosting for up to 10 web sites in a free [Windows Azure trial account](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604).</span></span> <span data-ttu-id="40f70-208">サンプルの SignalR アプリケーションをデプロイする方法のチュートリアルは、次を参照してください。 [、SignalR 入門サンプルとして Windows Azure の Web サイトを発行](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="40f70-208">For a walkthrough on how to deploy the sample SignalR application, see [Publish the SignalR Getting Started Sample as a Windows Azure Web Site](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx).</span></span> <span data-ttu-id="40f70-209">Visual Studio web プロジェクトを Windows Azure の Web サイトにデプロイする方法の詳細については、次を参照してください。 [ASP.NET アプリケーションを Windows Azure Web サイトを展開する](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)します。</span><span class="sxs-lookup"><span data-stu-id="40f70-209">For detailed information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Deploying an ASP.NET Application to a Windows Azure Web Site](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).</span></span> <span data-ttu-id="40f70-210">(注: WebSocket トランスポートでは現在の Windows Azure Web サイトがサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="40f70-210">(Note: The WebSocket transport is not currently supported for Windows Azure Web Sites.</span></span> <span data-ttu-id="40f70-211">ときに WebSocket トランスポートを使用できません、SignalR のトランスポートのセクションで説明したように、その他の利用可能なトランスポートを使用して、 [SignalR のトピックで初めに](index.md))。</span><span class="sxs-lookup"><span data-stu-id="40f70-211">When WebSocket transport is not available, SignalR uses the other available transports as described in the Transports section of the [Introduction to SignalR topic](index.md).)</span></span>

<span data-ttu-id="40f70-212">高度な SignalR 開発の概念については、SignalR のソース コードおよびリソースは、次のサイトを参照してください。</span><span class="sxs-lookup"><span data-stu-id="40f70-212">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources:</span></span>

- [<span data-ttu-id="40f70-213">SignalR プロジェクト</span><span class="sxs-lookup"><span data-stu-id="40f70-213">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="40f70-214">SignalR Github とサンプル</span><span class="sxs-lookup"><span data-stu-id="40f70-214">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="40f70-215">SignalR の Wiki</span><span class="sxs-lookup"><span data-stu-id="40f70-215">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
