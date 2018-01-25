---
uid: signalr/overview/getting-started/real-time-web-applications-with-signalr
title: "ハンズ オン ラボ: SignalR でリアルタイムの Web アプリケーション |Microsoft ドキュメント"
author: rick-anderson
description: "リアルタイムの Web アプリケーションには、サーバー側のように、リアルタイムで接続しているクライアントにコンテンツをプッシュする機能が機能します。 ASP、ASP.NET 開発者向けしています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: ba07958c-42e1-4da0-81db-ba6925ed6db0
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/real-time-web-applications-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 22123a9c61e6830f3f9f66a45182e1e923950341
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="hands-on-lab-real-time-web-applications-with-signalr"></a><span data-ttu-id="0ecf7-104">SignalR でリアルタイムの Web アプリケーションをハンズ オン ラボ:</span><span class="sxs-lookup"><span data-stu-id="0ecf7-104">Hands On Lab: Real-Time Web Applications with SignalR</span></span>
====================
<span data-ttu-id="0ecf7-105">によって[Web キャンプ チーム](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="0ecf7-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="0ecf7-106">Web キャンプ トレーニング キットをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-106">Download Web Camps Training Kit</span></span>](http://aka.ms/webcamps-training-kit)

> <span data-ttu-id="0ecf7-107">リアルタイムの Web アプリケーションには、サーバー側のように、リアルタイムで接続しているクライアントにコンテンツをプッシュする機能が機能します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-107">Real-time Web applications feature the ability to push server-side content to the connected clients as it happens, in real-time.</span></span> <span data-ttu-id="0ecf7-108">ASP.NET 開発者向けの**ASP.NET SignalR**は各自のアプリケーションにリアルタイム web 機能を追加するライブラリ。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-108">For ASP.NET developers, **ASP.NET SignalR** is a library to add real-time web functionality to their applications.</span></span> <span data-ttu-id="0ecf7-109">これは、自動的にクライアントとサーバーの最適な使用可能なトランスポート最適使用可能なトランスポートを選択するいくつかの配送の活用します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-109">It takes advantage of several transports, automatically selecting the best available transport given the client and server's best available transport.</span></span> <span data-ttu-id="0ecf7-110">これを活用**WebSocket**ブラウザーとサーバー間で双方向通信を有効にする HTML5 API です。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-110">It takes advantage of **WebSocket**, an HTML5 API that enables bi-directional communication between the browser and server.</span></span>
> 
> <span data-ttu-id="0ecf7-111">**SignalR**クライアント RPC サーバーを行うためのシンプルで高度な API も用意されています (サーバー側の .NET コードからのクライアントのブラウザーで JavaScript 関数を呼び出す)、ASP.NET アプリケーションだけでなく、接続の管理用に便利ですフックを追加します。などの接続切断イベント、グループ化接続、および承認します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-111">**SignalR** also provides a simple, high-level API for doing server to client RPC (call JavaScript functions in your clients' browsers from server-side .NET code) in your ASP.NET application, as well as adding useful hooks for connection management, such as connect/disconnect events, grouping connections, and authorization.</span></span>
> 
> <span data-ttu-id="0ecf7-112">**SignalR**は、クライアントとサーバー間のリアルタイムの作業を行うために必要なトランスポートの中に、抽象化します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-112">**SignalR** is an abstraction over some of the transports that are required to do real-time work between client and server.</span></span> <span data-ttu-id="0ecf7-113">A **SignalR**接続は、HTTP、として起動しに昇格し、 **WebSocket**使用可能な場合に接続します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-113">A **SignalR** connection starts as HTTP, and is then promoted to a **WebSocket** connection if available.</span></span> <span data-ttu-id="0ecf7-114">**WebSocket**の理想的なトランスポートは、 **SignalR**サーバーのメモリを最も効率的に利用するため、最短の待機時間を持ち、最も基本的な機能 (クライアントの間の全二重通信など、サーバー)、最も厳しい要件がありますが、: **WebSocket**サーバーを使用する必要があります**Windows Server 2012**または**Windows 8**、と共に**.NET framework 4.5**です。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-114">**WebSocket** is the ideal transport for **SignalR**, since it makes the most efficient use of server memory, has the lowest latency, and has the most underlying features (such as full duplex communication between client and server), but it also has the most stringent requirements: **WebSocket** requires the server to be using **Windows Server 2012** or **Windows 8**, along with **.NET Framework 4.5**.</span></span> <span data-ttu-id="0ecf7-115">これらの要件が満たされない場合**SignalR**がその接続を確立する他のトランスポートを使用するとき (と同様に*Ajax ロング ポーリング*)。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-115">If these requirements are not met, **SignalR** will attempt to use other transports to make its connections (like *Ajax long polling*).</span></span>
> 
> <span data-ttu-id="0ecf7-116">**SignalR** API には、クライアントとサーバー間で通信するための 2 つのモデルが含まれています:**永続的な接続**と**ハブ**です。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-116">The **SignalR** API contains two models for communicating between clients and servers: **Persistent Connections** and **Hubs**.</span></span> <span data-ttu-id="0ecf7-117">A**接続**グループ化すると、受信者の 1 つの送信、またはブロードキャスト メッセージを単純なエンドポイントを表します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-117">A **Connection** represents a simple endpoint for sending single-recipient, grouped, or broadcast messages.</span></span> <span data-ttu-id="0ecf7-118">A**ハブ**を互いに直接メソッドを呼び出すには、クライアントとサーバーをできるようにする接続 API に基づいて構築されておりより高度なパイプラインがします。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-118">A **Hub** is a more high-level pipeline built upon the Connection API that allows your client and server to call methods on each other directly.</span></span>
> 
> ![SignalR のアーキテクチャ](real-time-web-applications-with-signalr/_static/image1.png)
> 
> <span data-ttu-id="0ecf7-120">すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit)です。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-120">All sample code and snippets are included in the Web Camps Training Kit, available at [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span></span>


<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="0ecf7-121">概要</span><span class="sxs-lookup"><span data-stu-id="0ecf7-121">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="0ecf7-122">目的</span><span class="sxs-lookup"><span data-stu-id="0ecf7-122">Objectives</span></span>

<span data-ttu-id="0ecf7-123">このハンズオン ラボでは学習する方法。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-123">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="0ecf7-124">SignalR を使用してクライアントにサーバーからの通知を送信します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-124">Send notifications from server to client using SignalR.</span></span>
- <span data-ttu-id="0ecf7-125">スケール アウトを使用して SignalR アプリケーションを**SQL Server**です。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-125">Scale Out your SignalR application using **SQL Server**.</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="0ecf7-126">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="0ecf7-126">Prerequisites</span></span>

<span data-ttu-id="0ecf7-127">このハンズオン ラボを完了する、次が必要。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-127">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="0ecf7-128">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/)以上</span><span class="sxs-lookup"><span data-stu-id="0ecf7-128">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/) or greater</span></span>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="0ecf7-129">セットアップ</span><span class="sxs-lookup"><span data-stu-id="0ecf7-129">Setup</span></span>

<span data-ttu-id="0ecf7-130">このハンズオン ラボでは、演習を実行するのには、最初に、環境を設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-130">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="0ecf7-131">Windows エクスプ ローラー ウィンドウを開き、ラボを参照**ソース**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-131">Open a Windows Explorer window and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="0ecf7-132">右クリック**Setup.cmd**選択**管理者として実行**を環境を構成して、このラボ用の Visual Studio のコード スニペットをインストールするセットアップ プロセスを起動します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-132">Right-click **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="0ecf7-133">ユーザー アカウント制御 ダイアログ ボックスが表示されている場合は、続行するアクションを確認します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-133">If the User Account Control dialog box is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="0ecf7-134">セットアップを実行する前に、このラボのすべての依存関係をチェックしたことを確認してください。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-134">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="0ecf7-135">コード スニペットを使用します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-135">Using the Code Snippets</span></span>

<span data-ttu-id="0ecf7-136">ラボ ドキュメント全体にわたって、コード ブロックを挿入するように指示されます。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-136">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="0ecf7-137">便宜上、このコードのほとんどは、Visual Studio コード スニペットを手動で追加することを回避する Visual Studio 2013 内からアクセスできるとして提供されます。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-137">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="0ecf7-138">各演習にある開始ソリューションが組み込まれた、**開始**個別に各手順をたどることによってこの作業のフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-138">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="0ecf7-139">実習では、追加のコード スニペットがこれらのソリューションの開始から不足しており、演習を完了するまで動作しない可能性がありますに注意してください。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-139">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="0ecf7-140">演習では、ソース コード、内部も表示されます、**終了**を対応する手順の手順を実行した結果のコードの Visual Studio ソリューションを含むフォルダー。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-140">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="0ecf7-141">このハンズオン ラボを使用すると、追加のヘルプを必要がある場合は、これらのソリューションをガイドとして使用できます。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-141">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>


* * *

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="0ecf7-142">演習</span><span class="sxs-lookup"><span data-stu-id="0ecf7-142">Exercises</span></span>

<span data-ttu-id="0ecf7-143">このハンズオン ラボには、次の実習が含まれます。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-143">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="0ecf7-144">SignalR を使用したリアルタイムのデータの操作</span><span class="sxs-lookup"><span data-stu-id="0ecf7-144">Working with Real-Time Data Using SignalR</span></span>](#Exercise1)
2. [<span data-ttu-id="0ecf7-145">SQL Server を使用したスケール アウト</span><span class="sxs-lookup"><span data-stu-id="0ecf7-145">Scaling Out Using SQL Server</span></span>](#Exercise2)

<span data-ttu-id="0ecf7-146">この演習を完了する時間を推定: **60 分**</span><span class="sxs-lookup"><span data-stu-id="0ecf7-146">Estimated time to complete this lab: **60 minutes**</span></span>

> [!NOTE]
> <span data-ttu-id="0ecf7-147">Visual Studio を初めて起動するときに定義済みの設定のコレクションの 1 つを選択する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-147">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="0ecf7-148">定義済みの各コレクションは、特定の開発スタイルと一致するものではまた、ウィンドウのレイアウト、エディターの動作、IntelliSense コード スニペットでは、およびダイアログ ボックスのオプションを判断します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-148">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="0ecf7-149">このラボの手順を使用する場合は、Visual Studio での特定のタスクを実行に必要な操作を記述する、**全般的な開発設定**コレクション。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-149">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="0ecf7-150">開発環境に合わせてさまざまな設定のコレクションを選択する場合、考慮する必要がある手順に違いがあります。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-150">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>


<a id="Exercise1"></a>
### <a name="exercise-1-working-with-real-time-data-using-signalr"></a><span data-ttu-id="0ecf7-151">手順 1: SignalR を使用してリアルタイム データの使用</span><span class="sxs-lookup"><span data-stu-id="0ecf7-151">Exercise 1: Working with Real-Time Data Using SignalR</span></span>

<span data-ttu-id="0ecf7-152">全体を行うことができますチャットは例として使用される多くの場合、中にリアルタイム Web 機能を持つたくさんです。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-152">While chat is often used as an example, you can do a whole lot more with real-time Web functionality.</span></span> <span data-ttu-id="0ecf7-153">いつでも、ユーザーが新しいデータまたは Ajax の長いポーリングが、新しいデータを取得するページを実装する web ページを更新 SignalR を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-153">Any time a user refreshes a web page to see new data or the page implements Ajax long polling to retrieve new data, you can use SignalR.</span></span>

<span data-ttu-id="0ecf7-154">SignalR をサポートしている**サーバー プッシュ**または**ブロードキャスト**機能です。 接続の管理を自動的に処理されます。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-154">SignalR supports **server push** or **broadcasting** functionality; it handles connection management automatically.</span></span> <span data-ttu-id="0ecf7-155">クライアント サーバー通信のための従来の HTTP 接続では、接続の要求ごとに再確立が、SignalR は、クライアントとサーバー間で永続的な接続を提供します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-155">In classic HTTP connections for client-server communication, connection is re-established for each request, but SignalR provides persistent connection between the client and server.</span></span> <span data-ttu-id="0ecf7-156">SignalR のサーバー コードを呼び出すクライアント コード、リモート プロシージャ コール (RPC) を使用してブラウザーで、要求/応答モデルではなくお把握します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-156">In SignalR the server code calls out to a client code in the browser using Remote Procedure Calls (RPC), rather than the request-response model we know today.</span></span>

<span data-ttu-id="0ecf7-157">この演習では、構成、**マニア Quiz** SignalR を使用して、統計情報を持つダッシュ ボード全体のページを更新することがなく更新されたメトリックを表示するアプリケーション。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-157">In this exercise, you will configure the **Geek Quiz** application to use SignalR to display the Statistics dashboard with the updated metrics without the need to refresh the entire page.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1--exploring-the-geek-quiz-statistics-page"></a><span data-ttu-id="0ecf7-158">タスク 1 – マニア クイズの統計情報 ページの表示</span><span class="sxs-lookup"><span data-stu-id="0ecf7-158">Task 1 – Exploring the Geek Quiz Statistics Page</span></span>

<span data-ttu-id="0ecf7-159">このタスクでは、アプリケーションを経由して統計情報 ページを表示する方法を確認し、更新する方法についてを向上する方法します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-159">In this task, you will go through the application and verify how the statistics page is shown and how you can improve the way the information is updated.</span></span>

1. <span data-ttu-id="0ecf7-160">開く**Visual Studio Express 2013 for Web**を開くと、 **GeekQuiz.sln**にソリューションがある、 **Source\Ex1 WorkingWithRealTimeData\Begin**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-160">Open **Visual Studio Express 2013 for Web** and open the **GeekQuiz.sln** solution located in the **Source\Ex1-WorkingWithRealTimeData\Begin** folder.</span></span>
2. <span data-ttu-id="0ecf7-161">キーを押して**f5 キーを押して**ソリューションを実行します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-161">Press **F5** to run the solution.</span></span> <span data-ttu-id="0ecf7-162">**ログイン**ページがブラウザーに表示する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-162">The **Log in** page should appear in the browser.</span></span>

    <span data-ttu-id="0ecf7-163">![ソリューションを実行している](real-time-web-applications-with-signalr/_static/image2.png "ソリューションの実行")</span><span class="sxs-lookup"><span data-stu-id="0ecf7-163">![Running the solution](real-time-web-applications-with-signalr/_static/image2.png "Running the solution")</span></span>

    <span data-ttu-id="0ecf7-164">*ソリューションの実行*</span><span class="sxs-lookup"><span data-stu-id="0ecf7-164">*Running the solution*</span></span>
3. <span data-ttu-id="0ecf7-165">をクリックして**登録**アプリケーションに新しいユーザーを作成するページの右上隅にします。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-165">Click **Register** in the upper-right corner of the page to create a new user in the application.</span></span>

    <span data-ttu-id="0ecf7-166">![リンクの登録](real-time-web-applications-with-signalr/_static/image3.png "レジスタ リンク")</span><span class="sxs-lookup"><span data-stu-id="0ecf7-166">![Register link](real-time-web-applications-with-signalr/_static/image3.png "Register link")</span></span>

    <span data-ttu-id="0ecf7-167">*リンクを登録します。*</span><span class="sxs-lookup"><span data-stu-id="0ecf7-167">*Register link*</span></span>
4. <span data-ttu-id="0ecf7-168">**登録** ページで、入力、**ユーザー名**と**パスワード**、クリックして**登録**です。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-168">In the **Register** page, enter a **User name** and **Password**, and then click **Register**.</span></span>

    <span data-ttu-id="0ecf7-169">![ユーザーの登録](real-time-web-applications-with-signalr/_static/image4.png "ユーザーの登録")</span><span class="sxs-lookup"><span data-stu-id="0ecf7-169">![Registering a user](real-time-web-applications-with-signalr/_static/image4.png "Registering a user")</span></span>

    <span data-ttu-id="0ecf7-170">*ユーザーの登録*</span><span class="sxs-lookup"><span data-stu-id="0ecf7-170">*Registering a user*</span></span>
5. <span data-ttu-id="0ecf7-171">アプリケーションが、新しいアカウントを登録し、ユーザーが認証され、最初のクイズ問題を示す、ホーム ページにリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-171">The application registers the new account and the user is authenticated and redirected back to the home page showing the first quiz question.</span></span>
6. <span data-ttu-id="0ecf7-172">開く、**統計**を新しいウィンドウでページし、配置、**ホーム**ページと**統計**サイド バイ サイドのページです。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-172">Open the **Statistics** page in a new window and put the **Home** page and **Statistics** page side-by-side.</span></span>

    <span data-ttu-id="0ecf7-173">![サイド バイ サイド windows](real-time-web-applications-with-signalr/_static/image5.png "側 windows 側")</span><span class="sxs-lookup"><span data-stu-id="0ecf7-173">![Side-by-side windows](real-time-web-applications-with-signalr/_static/image5.png "Side by side windows")</span></span>

    <span data-ttu-id="0ecf7-174">*サイド バイ サイド windows*</span><span class="sxs-lookup"><span data-stu-id="0ecf7-174">*Side-by-side windows*</span></span>
7. <span data-ttu-id="0ecf7-175">**ホーム** ページをクリックすると、オプションのいずれかの質問に回答します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-175">In the **Home** page, answer the question by clicking one of the options.</span></span>

    <span data-ttu-id="0ecf7-176">![質問の回答](real-time-web-applications-with-signalr/_static/image6.png "質問に答えること")</span><span class="sxs-lookup"><span data-stu-id="0ecf7-176">![Answering a question](real-time-web-applications-with-signalr/_static/image6.png "Answering a question")</span></span>

    <span data-ttu-id="0ecf7-177">*質問に答えること*</span><span class="sxs-lookup"><span data-stu-id="0ecf7-177">*Answering a question*</span></span>
8. <span data-ttu-id="0ecf7-178">一方のボタンをクリックすると、回答が表示されます。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-178">After clicking one of the buttons, the answer should appear.</span></span>

    <span data-ttu-id="0ecf7-179">![質問の答えが正しい](real-time-web-applications-with-signalr/_static/image7.png "質問の答えが正しい")</span><span class="sxs-lookup"><span data-stu-id="0ecf7-179">![Question answered correct](real-time-web-applications-with-signalr/_static/image7.png "Question answered correct")</span></span>

    <span data-ttu-id="0ecf7-180">*正しく回答する質問*</span><span class="sxs-lookup"><span data-stu-id="0ecf7-180">*Question answered correctly*</span></span>
9. <span data-ttu-id="0ecf7-181">統計情報 ページで提供される情報が古くなっていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-181">Notice that the information provided in the Statistics page is outdated.</span></span> <span data-ttu-id="0ecf7-182">更新された結果を確認するために、ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-182">Refresh the page in order to see the updated results.</span></span>

    <span data-ttu-id="0ecf7-183">![統計情報ページ](real-time-web-applications-with-signalr/_static/image8.png "統計情報 ページ")</span><span class="sxs-lookup"><span data-stu-id="0ecf7-183">![Statistics page](real-time-web-applications-with-signalr/_static/image8.png "Statistics page")</span></span>

    <span data-ttu-id="0ecf7-184">*統計情報 ページ*</span><span class="sxs-lookup"><span data-stu-id="0ecf7-184">*Statistics page*</span></span>
10. <span data-ttu-id="0ecf7-185">Visual Studio に戻り、デバッグを停止します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-185">Go back to Visual Studio and stop debugging.</span></span>

<a id="Ex1Task2"></a>
#### <a name="task-2--adding-signalr-to-geek-quiz-to-show-online-charts"></a><span data-ttu-id="0ecf7-186">タスク 2: 追加の SignalR マニア クイズをオンラインのグラフを表示するには</span><span class="sxs-lookup"><span data-stu-id="0ecf7-186">Task 2 – Adding SignalR to Geek Quiz to Show Online Charts</span></span>

<span data-ttu-id="0ecf7-187">このタスクでは、SignalR をソリューションに追加し、新しい応答がサーバーに送信されるときに、クライアントに更新プログラムを自動的に送信します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-187">In this task, you will add SignalR to the solution and send updates to the clients automatically when a new answer is sent to the server.</span></span>

1. <span data-ttu-id="0ecf7-188">**ツール**選択、Visual Studio のメニュー**ライブラリ パッケージ マネージャー**、クリックして**パッケージ マネージャー コンソール**です。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-188">From the **Tools** menu in Visual Studio, select **Library Package Manager**, and then click **Package Manager Console**.</span></span>
2. <span data-ttu-id="0ecf7-189">**パッケージ マネージャー コンソール**ウィンドウで、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-189">In the **Package Manager Console** window, execute the following command:</span></span>

    [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample1.ps1)]

    <span data-ttu-id="0ecf7-190">![パッケージがインストールされた SignalR](real-time-web-applications-with-signalr/_static/image9.png "SignalR パッケージのインストール")</span><span class="sxs-lookup"><span data-stu-id="0ecf7-190">![SignalR package installation](real-time-web-applications-with-signalr/_static/image9.png "SignalR package installation")</span></span>

    <span data-ttu-id="0ecf7-191">*SignalR のパッケージのインストール*</span><span class="sxs-lookup"><span data-stu-id="0ecf7-191">*SignalR package installation*</span></span>

    > [!NOTE]
    > <span data-ttu-id="0ecf7-192">インストールするときに**SignalR** NuGet パッケージのバージョン 2.0.2 新しい MVC 5 のアプリケーションから、手動で更新する必要が**OWIN** 2.0.1 へのパッケージ (またはそれ以降) SignalR をインストールする前にします。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-192">When installing **SignalR** NuGet packages version 2.0.2 from a brand new MVC 5 application, you will need to manually update **OWIN** packages to version 2.0.1 (or higher) before installing SignalR.</span></span> <span data-ttu-id="0ecf7-193">これを行うに次のスクリプトを実行することができます、 **Package Manager Console**:</span><span class="sxs-lookup"><span data-stu-id="0ecf7-193">To do this, you can execute the following script in the **Package Manager Console**:</span></span>
    > 
    > [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample2.ps1)]
    > 
    > <span data-ttu-id="0ecf7-194">SignalR の将来のリリースで OWIN の依存関係が自動的に更新されます。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-194">In a future release of SignalR, OWIN dependencies will be automatically updated.</span></span>
3. <span data-ttu-id="0ecf7-195">**ソリューション エクスプ ローラー**、展開、**スクリプト**フォルダーとに注意してくださいを SignalR *js*ファイル、ソリューションに追加されました。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-195">In **Solution Explorer**, expand the **Scripts** folder and notice that the SignalR *js* files were added to the solution.</span></span>

    <span data-ttu-id="0ecf7-196">![SignalR JavaScript 参照](real-time-web-applications-with-signalr/_static/image10.png "SignalR JavaScript 参照")</span><span class="sxs-lookup"><span data-stu-id="0ecf7-196">![SignalR JavaScript references](real-time-web-applications-with-signalr/_static/image10.png "SignalR JavaScript references")</span></span>

    <span data-ttu-id="0ecf7-197">*SignalR JavaScript 参照*</span><span class="sxs-lookup"><span data-stu-id="0ecf7-197">*SignalR JavaScript references*</span></span>
4. <span data-ttu-id="0ecf7-198">**ソリューション エクスプ ローラー**を右クリックし、 **GeekQuiz**プロジェクトで、**追加** | **新しいフォルダー**、名前を付けます**ハブ**です。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-198">In **Solution Explorer**, right-click the **GeekQuiz** project, select **Add** | **New Folder**, and name it **Hubs**.</span></span>
5. <span data-ttu-id="0ecf7-199">右クリックし、**ハブ**フォルダーと選択**追加 |新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-199">Right-click the **Hubs** folder and select **Add | New Item**.</span></span>

    <span data-ttu-id="0ecf7-200">![新しい項目の追加](real-time-web-applications-with-signalr/_static/image11.png "新しい項目の追加")</span><span class="sxs-lookup"><span data-stu-id="0ecf7-200">![Add new item](real-time-web-applications-with-signalr/_static/image11.png "Add new item")</span></span>

    <span data-ttu-id="0ecf7-201">*新しい項目を追加します。*</span><span class="sxs-lookup"><span data-stu-id="0ecf7-201">*Add new item*</span></span>
6. <span data-ttu-id="0ecf7-202">**新しい項目の追加**ダイアログ ボックスで、 **Visual c# |Web |SignalR** 、左ペインで、選択ノード**SignalR ハブ クラス (v2)** 、ファイルの名前を中央のウィンドウから**StatisticsHub.cs**  をクリック**追加**です。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-202">In the **Add New Item** dialog box, select the **Visual C# | Web | SignalR** node in the left pane, select **SignalR Hub Class (v2)** from the center pane, name the file **StatisticsHub.cs** and click **Add**.</span></span>

    <span data-ttu-id="0ecf7-203">![新しい項目 ダイアログ ボックスを追加](real-time-web-applications-with-signalr/_static/image12.png "追加新しい項目 ダイアログ ボックス")</span><span class="sxs-lookup"><span data-stu-id="0ecf7-203">![Add new item dialog box](real-time-web-applications-with-signalr/_static/image12.png "Add new item dialog box")</span></span>

    <span data-ttu-id="0ecf7-204">*新しい項目 ダイアログ ボックスを追加します。*</span><span class="sxs-lookup"><span data-stu-id="0ecf7-204">*Add new item dialog box*</span></span>
7. <span data-ttu-id="0ecf7-205">コードで置き換え、 **StatisticsHub**クラスを次のコード。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-205">Replace the code in the **StatisticsHub** class with the following code.</span></span>

    <span data-ttu-id="0ecf7-206">(コード スニペットの*RealTimeSignalR - Ex1 - StatisticsHubClass*)</span><span class="sxs-lookup"><span data-stu-id="0ecf7-206">(Code Snippet - *RealTimeSignalR - Ex1 - StatisticsHubClass*)</span></span>

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample3.cs)]
8. <span data-ttu-id="0ecf7-207">開いている**Startup.cs**の末尾に次の行を追加し、**構成**メソッドです。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-207">Open **Startup.cs** and add the following line at the end of the **Configuration** method.</span></span>

    <span data-ttu-id="0ecf7-208">(コード スニペットの*RealTimeSignalR - Ex1 - MapSignalR*)</span><span class="sxs-lookup"><span data-stu-id="0ecf7-208">(Code Snippet - *RealTimeSignalR - Ex1 - MapSignalR*)</span></span>

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample4.cs)]
9. <span data-ttu-id="0ecf7-209">開いている、 **StatisticsService.cs**内のページ、 **Services**フォルダーに次の追加とディレクティブを使用します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-209">Open the **StatisticsService.cs** page inside the **Services** folder and add the following using directives.</span></span>

    <span data-ttu-id="0ecf7-210">(コード スニペットの*RealTimeSignalR - Ex1 - UsingDirectives*)</span><span class="sxs-lookup"><span data-stu-id="0ecf7-210">(Code Snippet - *RealTimeSignalR - Ex1 - UsingDirectives*)</span></span>

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample5.cs)]
10. <span data-ttu-id="0ecf7-211">取得する最初の更新プログラムの接続されているクライアントを通知するために、**コンテキスト**現在の接続オブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-211">To notify connected clients of updates, you first retrieve a **Context** object for the current connection.</span></span> <span data-ttu-id="0ecf7-212">**ハブ**オブジェクトには、1 つまたは複数の接続をすべてにブロードキャスト クライアントにメッセージを送信するメソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-212">The **Hub** object contains methods to send messages to a single client or broadcast to all connected clients.</span></span> <span data-ttu-id="0ecf7-213">次のメソッドを追加、 **StatisticsService**統計データをブロードキャストするクラス。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-213">Add the following method to the **StatisticsService** class to broadcast the statistics data.</span></span>

    <span data-ttu-id="0ecf7-214">(コード スニペットの*RealTimeSignalR - Ex1 - NotifyUpdatesMethod*)</span><span class="sxs-lookup"><span data-stu-id="0ecf7-214">(Code Snippet - *RealTimeSignalR - Ex1 - NotifyUpdatesMethod*)</span></span>

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample6.cs)]

    > [!NOTE]
    > <span data-ttu-id="0ecf7-215">上記のコードでは、クライアントで関数を呼び出すには任意のメソッド名を使っている (つまり: *updateStatistics*)。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-215">In the code above, you are using an arbitrary method name to call a function on the client (i.e.: *updateStatistics*).</span></span> <span data-ttu-id="0ecf7-216">指定したメソッド名は、動的なオブジェクトは、IntelliSense またはのコンパイル時の検証がないことを意味として解釈されます。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-216">The method name that you specify is interpreted as a dynamic object, which means there is no IntelliSense or compile-time validation for it.</span></span> <span data-ttu-id="0ecf7-217">式は、実行時に評価されます。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-217">The expression is evaluated at run time.</span></span> <span data-ttu-id="0ecf7-218">メソッドの呼び出しが実行されると、SignalR は、メソッド名とパラメーターの値をクライアントに送信します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-218">When the method call executes, SignalR sends the method name and the parameter values to the client.</span></span> <span data-ttu-id="0ecf7-219">場合は、クライアントは、名前に一致するメソッドを持ち、そのメソッドが呼び出され、パラメーターの値が渡されます。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-219">If the client has a method that matches the name, that method is called and the parameter values are passed to it.</span></span> <span data-ttu-id="0ecf7-220">クライアントで一致するメソッドが見つからない場合、エラーは発生しません。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-220">If no matching method is found on the client, no error is raised.</span></span> <span data-ttu-id="0ecf7-221">詳細についてを参照してください[ASP.NET SignalR ハブ API ガイド](../guide-to-the-api/hubs-api-guide-server.md)です。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-221">For more information, refer to [ASP.NET SignalR Hubs API Guide](../guide-to-the-api/hubs-api-guide-server.md).</span></span>
11. <span data-ttu-id="0ecf7-222">開く、 **TriviaController.cs**内のページ、**コント ローラー**フォルダーに次の追加とディレクティブを使用します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-222">Open the **TriviaController.cs** page inside the **Controllers** folder and add the following using directives.</span></span>

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample7.cs)]
12. <span data-ttu-id="0ecf7-223">次の強調表示されたコードを追加、 **Post**アクション メソッド。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-223">Add the following highlighted code to the **Post** action method.</span></span>

    <span data-ttu-id="0ecf7-224">(コード スニペットの*RealTimeSignalR - Ex1 - NotifyUpdatesCall*)</span><span class="sxs-lookup"><span data-stu-id="0ecf7-224">(Code Snippet - *RealTimeSignalR - Ex1 - NotifyUpdatesCall*)</span></span>

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample8.cs)]
13. <span data-ttu-id="0ecf7-225">開く、 **Statistics.cshtml**内のページ、**ビュー |ホーム**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-225">Open the **Statistics.cshtml** page inside the **Views | Home** folder.</span></span> <span data-ttu-id="0ecf7-226">検索、**スクリプト**セクションおよびセクションの先頭に次のスクリプト参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-226">Locate the **Scripts** section and add the following script references at the beginning of the section.</span></span>

    <span data-ttu-id="0ecf7-227">(コード スニペットの*RealTimeSignalR - Ex1 - SignalRScriptReferences*)</span><span class="sxs-lookup"><span data-stu-id="0ecf7-227">(Code Snippet - *RealTimeSignalR - Ex1 - SignalRScriptReferences*)</span></span>

    [!code-cshtml[Main](real-time-web-applications-with-signalr/samples/sample9.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="0ecf7-228">SignalR と他のスクリプト ライブラリを Visual Studio プロジェクトに追加するときに、パッケージ マネージャーがここに示すように、バージョンより新しくなっている SignalR スクリプト ファイルのバージョンをインストールできます。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-228">When you add SignalR and other script libraries to your Visual Studio project, the Package Manager might install a version of the SignalR script file that is more recent than the version shown in this topic.</span></span> <span data-ttu-id="0ecf7-229">コード内のスクリプト参照がプロジェクトにインストールされているスクリプト ライブラリのバージョンと一致することを確認します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-229">Make sure that the script reference in your code matches the version of the script library installed in your project.</span></span>
14. <span data-ttu-id="0ecf7-230">SignalR ハブにクライアントを接続し、ハブから、新しいメッセージが受信したときに、統計データを更新する次の強調表示されたコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-230">Add the following highlighted code to connect the client to the SignalR hub and update the statistics data when a new message is received from the hub.</span></span>

    <span data-ttu-id="0ecf7-231">(コード スニペットの*RealTimeSignalR - Ex1 - SignalRClientCode*)</span><span class="sxs-lookup"><span data-stu-id="0ecf7-231">(Code Snippet - *RealTimeSignalR - Ex1 - SignalRClientCode*)</span></span>

    [!code-cshtml[Main](real-time-web-applications-with-signalr/samples/sample10.cshtml)]

    <span data-ttu-id="0ecf7-232">このコードではハブ プロキシの作成され、サーバーから送信されたメッセージをリッスンするイベント ハンドラーを登録します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-232">In this code, you are creating a Hub Proxy and registering an event handler to listen for messages sent by the server.</span></span> <span data-ttu-id="0ecf7-233">この場合、を介して送信されるメッセージをリッスンする、 *updateStatistics*メソッドです。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-233">In this case, you listen for messages sent through the *updateStatistics* method.</span></span>

<a id="Ex1Task3"></a>
#### <a name="task-3--running-the-solution"></a><span data-ttu-id="0ecf7-234">タスク 3: ソリューションの実行</span><span class="sxs-lookup"><span data-stu-id="0ecf7-234">Task 3 – Running the Solution</span></span>

<span data-ttu-id="0ecf7-235">このタスクでは、SignalR を使用して自動的に新しい質問に回答した後、統計情報 ビューを更新することを確認するようにソリューションを実行します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-235">In this task, you will run the solution to verify that the statistics view is updated automatically using SignalR after answering a new question.</span></span>

1. <span data-ttu-id="0ecf7-236">キーを押して**f5 キーを押して**ソリューションを実行します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-236">Press **F5** to run the solution.</span></span>

    > [!NOTE]
    > <span data-ttu-id="0ecf7-237">アプリケーションにログインしていない場合、は、タスク 1 で作成したユーザーでログインします。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-237">If not already logged in to the application, log in with the user you created in Task 1.</span></span>
2. <span data-ttu-id="0ecf7-238">開く、**統計**を新しいウィンドウでページし、配置、**ホーム**ページと**統計**タスク 1 の場合と同様に、サイド バイ サイドをページします。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-238">Open the **Statistics** page in a new window and put the **Home** page and **Statistics** page side-by-side as you did in Task 1.</span></span>
3. <span data-ttu-id="0ecf7-239">**ホーム** ページをクリックすると、オプションのいずれかの質問に回答します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-239">In the **Home** page, answer the question by clicking one of the options.</span></span>

    <span data-ttu-id="0ecf7-240">![別の質問に答えること](real-time-web-applications-with-signalr/_static/image13.png "別の質問に答えること")</span><span class="sxs-lookup"><span data-stu-id="0ecf7-240">![Answering another question](real-time-web-applications-with-signalr/_static/image13.png "Answering another question")</span></span>

    <span data-ttu-id="0ecf7-241">*別の質問に答えること*</span><span class="sxs-lookup"><span data-stu-id="0ecf7-241">*Answering another question*</span></span>
4. <span data-ttu-id="0ecf7-242">一方のボタンをクリックすると、回答が表示されます。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-242">After clicking one of the buttons, the answer should appear.</span></span> <span data-ttu-id="0ecf7-243">全体のページを更新する必要がない最新の情報を質問に回答した後、ページの統計情報が自動的に更新することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-243">Notice that the Statistics information on the page is updated automatically after answering the question with the updated information without the need to refresh the entire page.</span></span>

    <span data-ttu-id="0ecf7-244">![応答の後に統計情報のページが更新される](real-time-web-applications-with-signalr/_static/image14.png "応答後の統計情報 ページの更新")</span><span class="sxs-lookup"><span data-stu-id="0ecf7-244">![Statistics page refreshed after answer](real-time-web-applications-with-signalr/_static/image14.png "Statistics page refreshed after answer")</span></span>

    <span data-ttu-id="0ecf7-245">*応答の後に更新された統計情報 ページ*</span><span class="sxs-lookup"><span data-stu-id="0ecf7-245">*Statistics page refreshed after answer*</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-scaling-out-using-sql-server"></a><span data-ttu-id="0ecf7-246">手順 2: 使用したスケール アウト SQL Server</span><span class="sxs-lookup"><span data-stu-id="0ecf7-246">Exercise 2: Scaling Out Using SQL Server</span></span>

<span data-ttu-id="0ecf7-247">Web アプリケーションを拡張するときに、通常のできます*スケール アップ*と*スケール アウト*オプション。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-247">When scaling a web application, you can generally choose between *scaling up* and *scaling out* options.</span></span> <span data-ttu-id="0ecf7-248">*スケール アップ*中に複数のリソース (CPU、RAM など) でより大きなサーバーを使用することを意味*スケール アウト*負荷を処理するサーバーを追加することを意味します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-248">*Scale up* means using a larger server, with more resources (CPU, RAM, etc.) while *scale out* means adding more servers to handle the load.</span></span> <span data-ttu-id="0ecf7-249">後者の問題は、クライアントを別のサーバーにルーティング取得ことができます。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-249">The problem with the latter is that the clients can get routed to different servers.</span></span> <span data-ttu-id="0ecf7-250">1 つのサーバーに接続されているクライアントは、別のサーバーから送信されたメッセージを受信しません。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-250">A client that is connected to one server will not receive messages sent from another server.</span></span>

<span data-ttu-id="0ecf7-251">呼ばれるコンポーネントを使用してこれらの問題を解決できる*バック プレーン*サーバー間でメッセージを転送します場合。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-251">You can solve these issues by using a component called *backplane*, to forward messages between servers.</span></span> <span data-ttu-id="0ecf7-252">有効になっているバック プレーンと各アプリケーション インスタンスが、バック プレーンにメッセージを送信バック プレーンに転送し、その他のアプリケーション インスタンス。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-252">With a backplane enabled, each application instance sends messages to the backplane, and the backplane forwards them to the other application instances.</span></span>

<span data-ttu-id="0ecf7-253">現在は、SignalR のバック プレーンの 3 つの種類があります。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-253">There are currently three types of backplanes for SignalR:</span></span>

- <span data-ttu-id="0ecf7-254">**Windows Azure Service Bus**です。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-254">**Windows Azure Service Bus**.</span></span> <span data-ttu-id="0ecf7-255">Service Bus は、メッセージング インフラストラクチャにより、疎結合されたメッセージを送信するコンポーネントです。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-255">Service Bus is a messaging infrastructure that allows components to send loosely coupled messages.</span></span>
- <span data-ttu-id="0ecf7-256">**SQL Server**です。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-256">**SQL Server**.</span></span> <span data-ttu-id="0ecf7-257">SQL Server のバック プレーンでは、SQL テーブルにメッセージを書き込みます。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-257">The SQL Server backplane writes messages to SQL tables.</span></span> <span data-ttu-id="0ecf7-258">バック プレーンでは、効率的なメッセージの Service Broker を使用します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-258">The backplane uses Service Broker for efficient messaging.</span></span> <span data-ttu-id="0ecf7-259">ただし、これは Service Broker が有効になっていない場合にでも動作します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-259">However, it also works if Service Broker is not enabled.</span></span>
- <span data-ttu-id="0ecf7-260">**Redis**です。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-260">**Redis**.</span></span> <span data-ttu-id="0ecf7-261">Redis は、メモリ内キー値ストアです。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-261">Redis is an in-memory key-value store.</span></span> <span data-ttu-id="0ecf7-262">Redis は、メッセージを送信する (「パブリッシュ/サブスクライブ」) の公開/定期受信パターンをサポートします。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-262">Redis supports a publish/subscribe ("pub/sub") pattern for sending messages.</span></span>

<span data-ttu-id="0ecf7-263">メッセージ バスを介してすべてのメッセージが送信されます。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-263">Every message is sent through a message bus.</span></span> <span data-ttu-id="0ecf7-264">メッセージ バスを実装して、 [IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx)インターフェイスで、パブリッシュ/サブスクライブ抽象化を提供します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-264">A message bus implements the [IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx) interface, which provides a publish/subscribe abstraction.</span></span> <span data-ttu-id="0ecf7-265">既定値に置き換えることで、バック プレーンの作業**IMessageBus**そのバック プレーン用に設計されたバスとします。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-265">The backplanes work by replacing the default **IMessageBus** with a bus designed for that backplane.</span></span>

<span data-ttu-id="0ecf7-266">各サーバー インスタンスは、バスを介してバック プレーンに接続します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-266">Each server instance connects to the backplane through the bus.</span></span> <span data-ttu-id="0ecf7-267">メッセージが送信されると、バック プレーンに移動して、バック プレーンのすべてのサーバーに送信します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-267">When a message is sent, it goes to the backplane, and the backplane sends it to every server.</span></span> <span data-ttu-id="0ecf7-268">サーバーは、バック プレーンからメッセージを受信したときに、そのローカル キャッシュ内で、メッセージを格納します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-268">When a server receives a message from the backplane, it stores the message in its local cache.</span></span> <span data-ttu-id="0ecf7-269">サーバーはのローカル キャッシュから、クライアントにメッセージを配信します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-269">The server then delivers messages to clients from its local cache.</span></span>

<span data-ttu-id="0ecf7-270">詳細については、SignalR バック プレーンのしくみ、このテキストを読み取る[記事](../performance/scaleout-in-signalr.md)です。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-270">For more information about how the SignalR backplane works, read this [article](../performance/scaleout-in-signalr.md).</span></span>

> [!NOTE]
> <span data-ttu-id="0ecf7-271">バック プレーンがボトルネックになることがいくつかのシナリオがあります。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-271">There are some scenarios where a backplane can become a bottleneck.</span></span> <span data-ttu-id="0ecf7-272">SignalR の一般的なシナリオを次に示します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-272">Here are some typical SignalR scenarios:</span></span>
> 
> - <span data-ttu-id="0ecf7-273">[サーバーのブロードキャスト](tutorial-server-broadcast-with-signalr.md)(たとえば、株価表示器): サーバーにメッセージが送信される速度を制御するために、バック プレーンがこのシナリオの適切に動作します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-273">[Server broadcast](tutorial-server-broadcast-with-signalr.md) (e.g., stock ticker): Backplanes work well for this scenario, because the server controls the rate at which messages are sent.</span></span>
> - <span data-ttu-id="0ecf7-274">[クライアントからクライアントへの](tutorial-getting-started-with-signalr.md)(チャットなど)。 このシナリオでバック プレーン可能性がありますするボトルネックになっている場合は、クライアントの数によるメッセージの数が拡大縮小。 つまり、メッセージの数が増えた場合比例して増えるにつれてクライアントが参加します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-274">[Client-to-client](tutorial-getting-started-with-signalr.md) (e.g., chat): In this scenario, the backplane might be a bottleneck if the number of messages scales with the number of clients; that is, if the rate of messages grows proportionally as more clients join.</span></span>
> - <span data-ttu-id="0ecf7-275">[高周波数のリアルタイム](tutorial-high-frequency-realtime-with-signalr.md)(リアルタイムのゲームなど)。 このシナリオのバック プレーンは推奨されません。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-275">[High-frequency realtime](tutorial-high-frequency-realtime-with-signalr.md) (e.g., real-time games): A backplane is not recommended for this scenario.</span></span>


<span data-ttu-id="0ecf7-276">この演習では、使用**SQL Server**間でメッセージを配信、**マニア Quiz**アプリケーションです。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-276">In this exercise, you will use **SQL Server** to distribute messages across the **Geek Quiz** application.</span></span> <span data-ttu-id="0ecf7-277">効果を取得するために、構成を設定する方法を学習する 1 つのテスト コンピューターでこれらのタスクを実行するが、SignalR アプリケーションを 2 つまたは複数のサーバーを展開する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-277">You will run these tasks on a single test machine to learn how to set up the configuration, but in order to get the full effect, you will need to deploy the SignalR application to two or more servers.</span></span> <span data-ttu-id="0ecf7-278">いずれかのサーバーまたは別の専用サーバーでは、SQL Server をインストールすることも必要があります。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-278">You must also install SQL Server on one of the servers, or on a separate dedicated server.</span></span>

![スケール アウト、SQL Server のダイアグラムの使用](real-time-web-applications-with-signalr/_static/image15.png)

<a id="Ex2Task1"></a>
#### <a name="task-1---understanding-the-scenario"></a><span data-ttu-id="0ecf7-280">タスク 1 のシナリオを理解します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-280">Task 1 - Understanding the Scenario</span></span>

<span data-ttu-id="0ecf7-281">このタスクでは、2 つのインスタンスを実行して**マニア Quiz**ローカル コンピューターにインスタンスの複数の IIS をシミュレートします。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-281">In this task, you will run 2 instances of **Geek Quiz** simulating multiple IIS instances on your local machine.</span></span> <span data-ttu-id="0ecf7-282">このシナリオで 1 つのアプリケーションでトリビア質問に答えることと、更新通知が送られません統計情報のページで、2 番目のインスタンス。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-282">In this scenario, when answering trivia questions on one application, update won't be notified on the statistics page of the second instance.</span></span> <span data-ttu-id="0ecf7-283">このシミュレーションのようになりますアプリケーションが複数のインスタンスに配置した環境に通信するために、ロード バランサーを使用するとします。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-283">This simulation resembles an environment where your application is deployed on multiple instances and using a load balancer to communicate with them.</span></span>

1. <span data-ttu-id="0ecf7-284">開く、 **Begin.sln**にソリューションがある、**ソース/Ex2-ScalingOutWithSQLServer/開始**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-284">Open the **Begin.sln** solution located in the **Source/Ex2-ScalingOutWithSQLServer/Begin** folder.</span></span> <span data-ttu-id="0ecf7-285">読み込まれるに表示されます、**サーバー エクスプ ローラー**構造体が異なる名前のソリューションには、同一である 2 つのプロジェクトが含まれています。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-285">Once loaded, you will notice on the **Server Explorer** that the solution has two projects with identical structures but different names.</span></span> <span data-ttu-id="0ecf7-286">これにより、ローカル コンピューターに同じアプリケーションの 2 つのインスタンスを実行しているをシミュレートします。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-286">This will simulate running two instances of the same application on your local machine.</span></span>

    <span data-ttu-id="0ecf7-287">![マニア クイズの 2 つのインスタンスをシミュレートするソリューションを開始](real-time-web-applications-with-signalr/_static/image16.png "マニア クイズの 2 つのインスタンスをシミュレートするソリューションの開始")</span><span class="sxs-lookup"><span data-stu-id="0ecf7-287">![Begin Solution Simulating 2 Instances of Geek Quiz](real-time-web-applications-with-signalr/_static/image16.png "Begin Solution Simulating 2 Instances of Geek Quiz")</span></span>

    <span data-ttu-id="0ecf7-288">*マニア クイズの 2 つのインスタンスをシミュレートするソリューションを開始します。*</span><span class="sxs-lookup"><span data-stu-id="0ecf7-288">*Begin Solution Simulating 2 Instances of Geek Quiz*</span></span>
2. <span data-ttu-id="0ecf7-289">ソリューション ノードを右クリックし、ソリューションのプロパティ ページを開く**プロパティ**です。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-289">Open the properties page of the solution by right-clicking the solution node and selecting **Properties**.</span></span> <span data-ttu-id="0ecf7-290">**スタートアップ プロジェクト**を選択**マルチ スタートアップ プロジェクト**を変更して、**アクション**両方のプロジェクトに対して値*開始*です。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-290">Under **Startup Project**, select **Multiple startup projects** and change the **Action** value for both projects to *Start*.</span></span>

    <span data-ttu-id="0ecf7-291">![複数のプロジェクトを起動](real-time-web-applications-with-signalr/_static/image17.png "複数のプロジェクトの開始")</span><span class="sxs-lookup"><span data-stu-id="0ecf7-291">![Starting Multiple Projects](real-time-web-applications-with-signalr/_static/image17.png "Starting Multiple Projects")</span></span>

    <span data-ttu-id="0ecf7-292">*複数のプロジェクトの開始*</span><span class="sxs-lookup"><span data-stu-id="0ecf7-292">*Starting Multiple Projects*</span></span>
3. <span data-ttu-id="0ecf7-293">キーを押して**f5 キーを押して**ソリューションを実行します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-293">Press **F5** to run the solution.</span></span> <span data-ttu-id="0ecf7-294">アプリケーションでは、2 つのインスタンスを起動します。**マニア Quiz**で別のポートを同じアプリケーションの複数のインスタンスをシミュレートします。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-294">The application will launch two instances of **Geek Quiz** in different ports, simulating multiple instances of the same application.</span></span> <span data-ttu-id="0ecf7-295">左と上、画面の右側に、ブラウザーのいずれかをピン留めします。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-295">Pin one of the browsers on left and the other on the right of your screen.</span></span> <span data-ttu-id="0ecf7-296">資格情報でログインするか、新しいユーザーを登録します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-296">Log in with your credentials or register a new user.</span></span> <span data-ttu-id="0ecf7-297">ログインすると、左側のトリビア ページを保持しに移動、**統計**右側のブラウザーでページ。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-297">Once logged in, keep the Trivia page on the left and go to the **Statistics** page in the browser on the right.</span></span>

    ![マニア クイズ サイド バイ サイド](real-time-web-applications-with-signalr/_static/image18.png)

    <span data-ttu-id="0ecf7-299">*マニア クイズ サイド バイ サイド*</span><span class="sxs-lookup"><span data-stu-id="0ecf7-299">*Geek Quiz Side by Side*</span></span>

    ![別のポートでマニア クイズ](real-time-web-applications-with-signalr/_static/image19.png)

    <span data-ttu-id="0ecf7-301">*別のポートでマニア クイズ*</span><span class="sxs-lookup"><span data-stu-id="0ecf7-301">*Geek Quiz in Different Ports*</span></span>
4. <span data-ttu-id="0ecf7-302">左側のブラウザーでの質問に回答を起動し、表示になります、**統計**右のブラウザーでページが更新されていません。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-302">Start answering questions in the left browser and you will notice that the **Statistics** page in the right browser is not being updated.</span></span> <span data-ttu-id="0ecf7-303">これは、ため**SignalR**クライアント間でメッセージを配信は、ローカル キャッシュ、このシナリオでは、複数のインスタンスをシミュレートします。 そのため、キャッシュはそれらの間で共有されません。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-303">This is because **SignalR** uses a local cache to distribute messages across their clients and this scenario is simulating multiple instances, therefore the cache is not shared between them.</span></span> <span data-ttu-id="0ecf7-304">確認することができます**SignalR**同じ手順をテストが 1 つのアプリを使用してが動作します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-304">You can verify that **SignalR** is working by testing the same steps but using a single app.</span></span> <span data-ttu-id="0ecf7-305">次のタスクでは、インスタンス間でメッセージをレプリケートするバック プレーンを構成します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-305">In the following tasks you will configure a backplane to replicate the messages across instances.</span></span>
5. <span data-ttu-id="0ecf7-306">Visual Studio に戻り、デバッグを停止します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-306">Go back to Visual Studio and stop debugging.</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-sql-server-backplane"></a><span data-ttu-id="0ecf7-307">タスク 2 – SQL Server のバック プレーンの作成</span><span class="sxs-lookup"><span data-stu-id="0ecf7-307">Task 2 – Creating the SQL Server Backplane</span></span>

<span data-ttu-id="0ecf7-308">このタスクでは、バック プレーンとして使用するデータベースを作成する、**マニア Quiz**アプリケーションです。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-308">In this task, you will create a database that will serve as a backplane for the **Geek Quiz** application.</span></span> <span data-ttu-id="0ecf7-309">使用して**SQL Server オブジェクト エクスプ ローラー**をサーバーを参照して、データベースを初期化します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-309">You will use **SQL Server Object Explorer** to browse your server and initialize the database.</span></span> <span data-ttu-id="0ecf7-310">さらに、有効にする、 **Service Broker**です。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-310">Additionally, you will enable the **Service Broker**.</span></span>

1. <span data-ttu-id="0ecf7-311">**Visual Studio**、開かれているメニュー**ビュー**選択**SQL Server オブジェクト エクスプ ローラー**です。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-311">In **Visual Studio**, open menu **View** and select **SQL Server Object Explorer**.</span></span>
2. <span data-ttu-id="0ecf7-312">右クリックして、LocalDB インスタンスに接続、 **SQL Server**ノードを選択して**SQL サーバーを追加しています.**オプション。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-312">Connect to your LocalDB instance by right-clicking the **SQL Server** node and selecting **Add SQL Server...** option.</span></span>

    <span data-ttu-id="0ecf7-313">![SQL Server インスタンスを追加する](real-time-web-applications-with-signalr/_static/image20.png "SQL Server インスタンスを追加します。")</span><span class="sxs-lookup"><span data-stu-id="0ecf7-313">![Adding a SQL Server Instance](real-time-web-applications-with-signalr/_static/image20.png "Adding a SQL Server Instance")</span></span>

    <span data-ttu-id="0ecf7-314">*SQL Server オブジェクト エクスプ ローラーに SQL Server インスタンスを追加します。*</span><span class="sxs-lookup"><span data-stu-id="0ecf7-314">*Adding a SQL Server instance to SQL Server Object Explorer*</span></span>
3. <span data-ttu-id="0ecf7-315">設定、**サーバー名**に*(localdb) \v11.0*のままにして**Windows 認証**認証モードとして。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-315">Set the **server name** to *(localdb)\v11.0* and leave **Windows Authentication** as your authentication mode.</span></span> <span data-ttu-id="0ecf7-316">をクリックして**接続**を続行します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-316">Click **Connect** to continue.</span></span>

    <span data-ttu-id="0ecf7-317">![LocalDB への接続](real-time-web-applications-with-signalr/_static/image21.png "LocalDB への接続")</span><span class="sxs-lookup"><span data-stu-id="0ecf7-317">![Connecting to LocalDB](real-time-web-applications-with-signalr/_static/image21.png "Connecting to LocalDB")</span></span>

    <span data-ttu-id="0ecf7-318">*LocalDB への接続*</span><span class="sxs-lookup"><span data-stu-id="0ecf7-318">*Connecting to LocalDB*</span></span>
4. <span data-ttu-id="0ecf7-319">これで LocalDB インスタンスに接続している、SignalR の SQL Server のバック プレーンと共に表示されるデータベースを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-319">Now that you are connected to your LocalDB instance, you will need to create a database that will represent the SQL Server backplane for SignalR.</span></span> <span data-ttu-id="0ecf7-320">これを行うを右クリックし、**データベース**ノード**新しいデータベースの追加**です。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-320">To do this, right-click the **Databases** node and select **Add New Database**.</span></span>

    <span data-ttu-id="0ecf7-321">![新しいデータベースを追加する](real-time-web-applications-with-signalr/_static/image22.png "新しいデータベースを追加します。")</span><span class="sxs-lookup"><span data-stu-id="0ecf7-321">![Adding a new database](real-time-web-applications-with-signalr/_static/image22.png "Adding a new database")</span></span>

    <span data-ttu-id="0ecf7-322">*新しいデータベースを追加します。*</span><span class="sxs-lookup"><span data-stu-id="0ecf7-322">*Adding a new database*</span></span>
5. <span data-ttu-id="0ecf7-323">データベース名を設定します*SignalR*  をクリック**OK**を作成します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-323">Set the database name to *SignalR* and click **OK** to create it.</span></span>

    <span data-ttu-id="0ecf7-324">![SignalR のデータベースを作成する](real-time-web-applications-with-signalr/_static/image23.png "SignalR データベースを作成します。")</span><span class="sxs-lookup"><span data-stu-id="0ecf7-324">![Creating the SignalR database](real-time-web-applications-with-signalr/_static/image23.png "Creating the SignalR database")</span></span>

    <span data-ttu-id="0ecf7-325">*SignalR のデータベースを作成します。*</span><span class="sxs-lookup"><span data-stu-id="0ecf7-325">*Creating the SignalR database*</span></span>

    > [!NOTE]
    > <span data-ttu-id="0ecf7-326">データベースの任意の名前を選択できます。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-326">You can choose any name for the database.</span></span>
6. <span data-ttu-id="0ecf7-327">バック プレーンから更新プログラムをより効率的に受信、データベースの Service Broker を有効にすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-327">To receive updates more efficiently from the backplane, it is recommended to enable Service Broker for the database.</span></span> <span data-ttu-id="0ecf7-328">Service Broker は、メッセージングと SQL Server のキューに対するネイティブ サポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-328">Service Broker provides native support for messaging and queuing in SQL Server.</span></span> <span data-ttu-id="0ecf7-329">バック プレーンは、Service Broker なしでも動作します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-329">The backplane also works without Service Broker.</span></span> <span data-ttu-id="0ecf7-330">クリックし、データベースを右クリックして新しいクエリを開く**新しいクエリ**です。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-330">Open a new query by right-clicking the database and select **New Query**.</span></span>

    <span data-ttu-id="0ecf7-331">![新しいクエリを開く](real-time-web-applications-with-signalr/_static/image24.png "新しいクエリを開く")</span><span class="sxs-lookup"><span data-stu-id="0ecf7-331">![Opening a New Query](real-time-web-applications-with-signalr/_static/image24.png "Opening a New Query")</span></span>

    <span data-ttu-id="0ecf7-332">*新しいクエリを開く*</span><span class="sxs-lookup"><span data-stu-id="0ecf7-332">*Opening a New Query*</span></span>
7. <span data-ttu-id="0ecf7-333">Service Broker が有効になっているかどうかを確認するには、クエリ、**は\_broker\_有効になっている**内の列、 **sys.databases**カタログ ビューです。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-333">To check whether Service Broker is enabled, query the **is\_broker\_enabled** column in the **sys.databases** catalog view.</span></span> <span data-ttu-id="0ecf7-334">直前に開かれたクエリ ウィンドウで、次のスクリプトを実行します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-334">Execute the following script in the recently opened query window.</span></span>

    [!code-sql[Main](real-time-web-applications-with-signalr/samples/sample11.sql)]

    <span data-ttu-id="0ecf7-335">![サービス ブローカーの状態を照会する](real-time-web-applications-with-signalr/_static/image25.png "サービス ブローカーの状態を照会します。")</span><span class="sxs-lookup"><span data-stu-id="0ecf7-335">![Querying the Service Broker Status](real-time-web-applications-with-signalr/_static/image25.png "Querying the Service Broker Status")</span></span>

    <span data-ttu-id="0ecf7-336">*サービス ブローカーの状態を照会します。*</span><span class="sxs-lookup"><span data-stu-id="0ecf7-336">*Querying the Service Broker Status*</span></span>
8. <span data-ttu-id="0ecf7-337">場合の値、**は\_broker\_有効になっている**、データベース内の列が&quot;0&quot;、有効にして、次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-337">If the value of the **is\_broker\_enabled** column in your database is &quot;0&quot;, use the following command to enable it.</span></span> <span data-ttu-id="0ecf7-338">置き換える **&lt;、データベース&gt;**データベースを作成するときに設定した名前を持つ (例:: SignalR)。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-338">Replace **&lt;YOUR-DATABASE&gt;** with the name you set when creating the database (e.g.: SignalR).</span></span>

    [!code-sql[Main](real-time-web-applications-with-signalr/samples/sample12.sql)]

    <span data-ttu-id="0ecf7-339">![Service Broker を有効にする](real-time-web-applications-with-signalr/_static/image26.png "Service Broker を有効にします。")</span><span class="sxs-lookup"><span data-stu-id="0ecf7-339">![Enabling Service Broker](real-time-web-applications-with-signalr/_static/image26.png "Enabling Service Broker")</span></span>

    <span data-ttu-id="0ecf7-340">*Service Broker を有効にします。*</span><span class="sxs-lookup"><span data-stu-id="0ecf7-340">*Enabling Service Broker*</span></span>

    > [!NOTE]
    > <span data-ttu-id="0ecf7-341">このクエリは、デッドロックを起こすことを確認するが表示された場合、DB に接続されているアプリケーションはありません。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-341">If this query appears to deadlock, make sure there are no applications connected to the DB.</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3--configuring-the-signalr-application"></a><span data-ttu-id="0ecf7-342">タスク 3 – SignalR アプリケーションを構成します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-342">Task 3 – Configuring the SignalR Application</span></span>

<span data-ttu-id="0ecf7-343">このタスクでは、構成**マニア Quiz** SQL Server のバック プレーンに接続します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-343">In this task, you will configure **Geek Quiz** to connect to the SQL Server backplane.</span></span> <span data-ttu-id="0ecf7-344">最初に追加するが、 **SignalR.SqlServer**バック プレーン データベースに NuGet パッケージと接続文字列します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-344">You will first add the **SignalR.SqlServer** NuGet package and set the connection string to your backplane database.</span></span>

1. <span data-ttu-id="0ecf7-345">開く、 **Package Manager Console**から**ツール** | **ライブラリ パッケージ マネージャー**です。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-345">Open the **Package Manager Console** from **Tools** | **Library Package Manager**.</span></span> <span data-ttu-id="0ecf7-346">確認して**GeekQuiz**でプロジェクトを選択、**既定のプロジェクト**ドロップダウン リスト。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-346">Make sure that **GeekQuiz** project is selected in the **Default project** drop-down list.</span></span> <span data-ttu-id="0ecf7-347">インストールする次のコマンドを入力、 **Microsoft.AspNet.SignalR.SqlServer** NuGet パッケージです。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-347">Type the following command to install the **Microsoft.AspNet.SignalR.SqlServer** NuGet package.</span></span>

    [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample13.ps1)]
2. <span data-ttu-id="0ecf7-348">プロジェクトに対して、前の手順がこの時間を繰り返します**GeekQuiz2**です。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-348">Repeat the previous step but this time for project **GeekQuiz2**.</span></span>
3. <span data-ttu-id="0ecf7-349">SQL Server のバック プレーンを構成するには、開く、 **Startup.cs**のファイル、 **GeekQuiz**プロジェクトし、次のコードを追加、**構成**メソッドです。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-349">To configure the SQL Server backplane, open the **Startup.cs** file of the **GeekQuiz** project and add the following code to the **Configure** method.</span></span> <span data-ttu-id="0ecf7-350">置き換える **&lt;、データベース&gt;**を SQL Server のバック プレーンの作成時に使用するデータベース名。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-350">Replace **&lt;YOUR-DATABASE&gt;** with your database name you used when creating the SQL Server backplane.</span></span> <span data-ttu-id="0ecf7-351">この手順を繰り返します、 **GeekQuiz2**プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-351">Repeat this step for the **GeekQuiz2** project.</span></span>

    <span data-ttu-id="0ecf7-352">(コード スニペットの*RealTimeSignalR - Ex2 - StartupConfiguration*)</span><span class="sxs-lookup"><span data-stu-id="0ecf7-352">(Code Snippet - *RealTimeSignalR - Ex2 - StartupConfiguration*)</span></span>

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample14.cs)]
4. <span data-ttu-id="0ecf7-353">SQL Server のバック プレーンの使用には、両方のプロジェクトが構成されたら、キーを押します**f5 キーを押して**を同時に実行します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-353">Now that both projects are configured to use the SQL Server backplane, press **F5** to run them simultaneously.</span></span>
5. <span data-ttu-id="0ecf7-354">もう一度、 **Visual Studio**の 2 つのインスタンスが起動**マニア Quiz**別のポートにします。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-354">Again, **Visual Studio** will launch two instances of **Geek Quiz** in different ports.</span></span> <span data-ttu-id="0ecf7-355">他の画面の右側および左側にピン留め、ブラウザーのいずれかと、資格情報でログインします。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-355">Pin one of the browsers on the left and the other on the right of your screen and log in with your credentials.</span></span> <span data-ttu-id="0ecf7-356">左側のトリビア ページを保持しに移動**統計**ページイン右ブラウザー。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-356">Keep the Trivia page on the left and go to **Statistics** pagein the right browser.</span></span>
6. <span data-ttu-id="0ecf7-357">左側のブラウザーで質問に対する回答を開始します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-357">Start answering questions in the left browser.</span></span> <span data-ttu-id="0ecf7-358">現時点では、**統計**バック プレーン感謝ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-358">This time, the **Statistics** page is updated thanks to the backplane.</span></span> <span data-ttu-id="0ecf7-359">アプリケーション間でのスイッチ (**統計**は現在は、左と**トリビア**は右端に表示) が機能する両方のインスタンスを検証するテストを繰り返すとします。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-359">Switch between applications (**Statistics** is now on the left, and **Trivia** is on the right) and repeat the test to validate that it is working for both instances.</span></span> <span data-ttu-id="0ecf7-360">バック プレーン役割を果たす、*共有キャッシュ*各接続のサーバーと各サーバーのメッセージは、メッセージを保存するローカル キャッシュに接続しているクライアントに配布します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-360">The backplane serves as a *shared cache* of messages for each connected server, and each server will store the messages in their own local cache to distribute to connected clients.</span></span>
7. <span data-ttu-id="0ecf7-361">Visual Studio に戻り、デバッグを停止します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-361">Go back to Visual Studio and stop debugging.</span></span>
8. <span data-ttu-id="0ecf7-362">SQL Server のバック プレーン コンポーネントは、指定したデータベースで必要なテーブルを自動的に生成します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-362">The SQL Server backplane component automatically generates the necessary tables on the specified database.</span></span> <span data-ttu-id="0ecf7-363">**SQL Server オブジェクト エクスプ ローラー**パネルで、バック プレーン用に作成したデータベースを開きます (例:: SignalR) し、そのテーブルを展開します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-363">In the **SQL Server Object Explorer** panel, open the database you created for the backplane (e.g.: SignalR) and expand its tables.</span></span> <span data-ttu-id="0ecf7-364">次の表が表示されます。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-364">You should see the following tables:</span></span>

    ![生成されたテーブルのバック プレーン](real-time-web-applications-with-signalr/_static/image27.png)

    <span data-ttu-id="0ecf7-366">*生成されたテーブルのバック プレーン*</span><span class="sxs-lookup"><span data-stu-id="0ecf7-366">*Backplane Generated Tables*</span></span>
9. <span data-ttu-id="0ecf7-367">右クリックし、 **SignalR.Messages\_0**テーブルを選択して**ビュー データ**です。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-367">Right-click the **SignalR.Messages\_0** table and select **View Data**.</span></span>

    ![SignalR のバック プレーンのメッセージ テーブルな表示](real-time-web-applications-with-signalr/_static/image28.png)

    <span data-ttu-id="0ecf7-369">*SignalR のバック プレーンのメッセージ テーブルな表示*</span><span class="sxs-lookup"><span data-stu-id="0ecf7-369">*View SignalR Backplane Messages Table*</span></span>
10. <span data-ttu-id="0ecf7-370">送信される異なるメッセージを表示、**ハブ**トリビア質問に答えるときです。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-370">You can see the different messages sent to the **Hub** when answering the trivia questions.</span></span> <span data-ttu-id="0ecf7-371">バック プレーンでは、接続されている任意のインスタンスにこれらのメッセージを配信します。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-371">The backplane distributes these messages to any connected instance.</span></span>

    ![バック プレーン メッセージ テーブル](real-time-web-applications-with-signalr/_static/image29.png)

    <span data-ttu-id="0ecf7-373">*バック プレーン メッセージ テーブル*</span><span class="sxs-lookup"><span data-stu-id="0ecf7-373">*Backplane Messages Table*</span></span>

* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="0ecf7-374">まとめ</span><span class="sxs-lookup"><span data-stu-id="0ecf7-374">Summary</span></span>

<span data-ttu-id="0ecf7-375">このハンズオン ラボでは、追加する方法を学習しました**SignalR**を使用して接続されているクライアントにサーバーからアプリケーションおよび送信通知に**ハブ**です。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-375">In this hands-on lab, you have learned how to add **SignalR** to your application and send notifications from the server to your connected clients using **Hubs**.</span></span> <span data-ttu-id="0ecf7-376">さらを使用して、アプリケーションを拡張する方法を学習しました、*バック プレーン*アプリケーションが複数の IIS インスタンスに展開されるコンポーネントです。</span><span class="sxs-lookup"><span data-stu-id="0ecf7-376">Additionally, you learned how to scale out your application by using a *backplane* component when your application is deployed in multiple IIS instances.</span></span>
