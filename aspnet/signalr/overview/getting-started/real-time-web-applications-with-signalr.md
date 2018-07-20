---
uid: signalr/overview/getting-started/real-time-web-applications-with-signalr
title: 'ハンズ オン ラボ: SignalR によるリアルタイムの Web アプリケーション |Microsoft Docs'
author: rick-anderson
description: リアルタイムの Web アプリケーションには、サーバー側の偶然ですが、リアルタイムで接続されているクライアントにコンテンツをプッシュする機能が機能します。 ASP.NET 開発者は、ASP.
ms.author: aspnetcontent
ms.date: 07/16/2014
ms.assetid: ba07958c-42e1-4da0-81db-ba6925ed6db0
msc.legacyurl: /signalr/overview/getting-started/real-time-web-applications-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 50ed2bed6b5b20684d00d7887494ee41346b5c3f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828910"
---
<a name="hands-on-lab-real-time-web-applications-with-signalr"></a><span data-ttu-id="32783-104">SignalR によるリアルタイムの Web アプリケーションをハンズ オン ラボ:</span><span class="sxs-lookup"><span data-stu-id="32783-104">Hands On Lab: Real-Time Web Applications with SignalR</span></span>
====================
<span data-ttu-id="32783-105">によって[Web キャンプ チーム](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="32783-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="32783-106">Web のキャンプ トレーニング キットをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="32783-106">Download Web Camps Training Kit</span></span>](http://aka.ms/webcamps-training-kit)

> <span data-ttu-id="32783-107">リアルタイムの Web アプリケーションには、サーバー側の偶然ですが、リアルタイムで接続されているクライアントにコンテンツをプッシュする機能が機能します。</span><span class="sxs-lookup"><span data-stu-id="32783-107">Real-time Web applications feature the ability to push server-side content to the connected clients as it happens, in real-time.</span></span> <span data-ttu-id="32783-108">ASP.NET 開発者向けの**ASP.NET SignalR**は自分のアプリケーションにリアルタイム web 機能を追加するライブラリです。</span><span class="sxs-lookup"><span data-stu-id="32783-108">For ASP.NET developers, **ASP.NET SignalR** is a library to add real-time web functionality to their applications.</span></span> <span data-ttu-id="32783-109">これは、クライアントとサーバーの最適な使用可能なトランスポート最適使用可能なトランスポートを自動的に選択すると、いくつかのトランスポート利用します。</span><span class="sxs-lookup"><span data-stu-id="32783-109">It takes advantage of several transports, automatically selecting the best available transport given the client and server's best available transport.</span></span> <span data-ttu-id="32783-110">活用されて**WebSocket**ブラウザーとサーバー間の双方向通信を実現する HTML5 API。</span><span class="sxs-lookup"><span data-stu-id="32783-110">It takes advantage of **WebSocket**, an HTML5 API that enables bi-directional communication between the browser and server.</span></span>
> 
> <span data-ttu-id="32783-111">**SignalR**クライアント RPC サーバーを行うためのシンプルで高レベルの API も提供します (サーバー側の .NET コードからのクライアントのブラウザーで JavaScript 関数を呼び出す) で、ASP.NET アプリケーションとの接続管理などの便利なフックを追加します。など、接続/切断イベント、接続のグループ化、および承認します。</span><span class="sxs-lookup"><span data-stu-id="32783-111">**SignalR** also provides a simple, high-level API for doing server to client RPC (call JavaScript functions in your clients' browsers from server-side .NET code) in your ASP.NET application, as well as adding useful hooks for connection management, such as connect/disconnect events, grouping connections, and authorization.</span></span>
> 
> <span data-ttu-id="32783-112">**SignalR**クライアントとサーバー間のリアルタイムの作業を行うには、必要なトランスポートの中に、抽象化です。</span><span class="sxs-lookup"><span data-stu-id="32783-112">**SignalR** is an abstraction over some of the transports that are required to do real-time work between client and server.</span></span> <span data-ttu-id="32783-113">A **SignalR**接続は、HTTP、として起動しには、昇格を**WebSocket**使用可能な場合に接続します。</span><span class="sxs-lookup"><span data-stu-id="32783-113">A **SignalR** connection starts as HTTP, and is then promoted to a **WebSocket** connection if available.</span></span> <span data-ttu-id="32783-114">**WebSocket**の理想的なトランスポートは、 **SignalR**、サーバー メモリが最も効率的な使用には、最小の待機時間を備え、基になる最も機能があります (クライアント間の全二重通信など、サーバー) が最も厳しい要件があります: **WebSocket** 、サーバーを使用する必要があります**Windows Server 2012**または**Windows 8**、と共に **.NET framework 4.5**します。</span><span class="sxs-lookup"><span data-stu-id="32783-114">**WebSocket** is the ideal transport for **SignalR**, since it makes the most efficient use of server memory, has the lowest latency, and has the most underlying features (such as full duplex communication between client and server), but it also has the most stringent requirements: **WebSocket** requires the server to be using **Windows Server 2012** or **Windows 8**, along with **.NET Framework 4.5**.</span></span> <span data-ttu-id="32783-115">これらの要件が満たされない場合**SignalR**他のトランスポートを使用して、その接続の確立を試みます (など*長いポーリングの Ajax*)。</span><span class="sxs-lookup"><span data-stu-id="32783-115">If these requirements are not met, **SignalR** will attempt to use other transports to make its connections (like *Ajax long polling*).</span></span>
> 
> <span data-ttu-id="32783-116">**SignalR** API には、クライアントとサーバー間の通信に 2 つのモデルが含まれています:**永続的な接続**と**Hubs**します。</span><span class="sxs-lookup"><span data-stu-id="32783-116">The **SignalR** API contains two models for communicating between clients and servers: **Persistent Connections** and **Hubs**.</span></span> <span data-ttu-id="32783-117">A**接続**別にグループ化、またはブロードキャスト メッセージを受信者の 1 つの送信を単純なエンドポイントを表します。</span><span class="sxs-lookup"><span data-stu-id="32783-117">A **Connection** represents a simple endpoint for sending single-recipient, grouped, or broadcast messages.</span></span> <span data-ttu-id="32783-118">A**ハブ**は、クライアントとサーバーが相互に直接メソッドを呼び出すことができる接続 API に基づいて構築されておりより高度なパイプラインです。</span><span class="sxs-lookup"><span data-stu-id="32783-118">A **Hub** is a more high-level pipeline built upon the Connection API that allows your client and server to call methods on each other directly.</span></span>
> 
> ![SignalR のアーキテクチャ](real-time-web-applications-with-signalr/_static/image1.png)
> 
> <span data-ttu-id="32783-120">すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[ http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit)します。</span><span class="sxs-lookup"><span data-stu-id="32783-120">All sample code and snippets are included in the Web Camps Training Kit, available at [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span></span>


<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="32783-121">概要</span><span class="sxs-lookup"><span data-stu-id="32783-121">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="32783-122">目的</span><span class="sxs-lookup"><span data-stu-id="32783-122">Objectives</span></span>

<span data-ttu-id="32783-123">このハンズオン ラボでは、学習する方法。</span><span class="sxs-lookup"><span data-stu-id="32783-123">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="32783-124">SignalR を使用してクライアントにサーバーからの通知を送信します。</span><span class="sxs-lookup"><span data-stu-id="32783-124">Send notifications from server to client using SignalR.</span></span>
- <span data-ttu-id="32783-125">スケール アウト、SignalR アプリケーションを使用して**SQL Server**します。</span><span class="sxs-lookup"><span data-stu-id="32783-125">Scale Out your SignalR application using **SQL Server**.</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="32783-126">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="32783-126">Prerequisites</span></span>

<span data-ttu-id="32783-127">このハンズオン ラボを完了する、次が必要。</span><span class="sxs-lookup"><span data-stu-id="32783-127">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="32783-128">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/)以上</span><span class="sxs-lookup"><span data-stu-id="32783-128">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/) or greater</span></span>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="32783-129">セットアップ</span><span class="sxs-lookup"><span data-stu-id="32783-129">Setup</span></span>

<span data-ttu-id="32783-130">このハンズオン ラボの演習を実行するためには、まず環境を設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="32783-130">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="32783-131">Windows エクスプ ローラー ウィンドウを開き、ラボを参照**ソース**フォルダー。</span><span class="sxs-lookup"><span data-stu-id="32783-131">Open a Windows Explorer window and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="32783-132">右クリック**Setup.cmd**選択**管理者として実行**環境を構成すると、このラボの Visual Studio のコード スニペットがインストールのセットアップ プロセスを起動します。</span><span class="sxs-lookup"><span data-stu-id="32783-132">Right-click **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="32783-133">ユーザー アカウント制御ダイアログ ボックスが表示されている場合は、続行するアクションを確認します。</span><span class="sxs-lookup"><span data-stu-id="32783-133">If the User Account Control dialog box is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="32783-134">セットアップを実行する前に、このラボのすべての依存関係をチェックしたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="32783-134">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="32783-135">コード スニペットの使用</span><span class="sxs-lookup"><span data-stu-id="32783-135">Using the Code Snippets</span></span>

<span data-ttu-id="32783-136">ラボ ドキュメント全体を通じて、コード ブロックを挿入するよう指示されます。</span><span class="sxs-lookup"><span data-stu-id="32783-136">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="32783-137">便宜上、このコードのほとんどは、Visual Studio コード スニペットを手動で追加することを避けるために Visual Studio 2013 内からアクセスできるとして提供されます。</span><span class="sxs-lookup"><span data-stu-id="32783-137">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="32783-138">ソリューションでは個々 の演習を伴います、**開始**を使用すると、各演習を他のユーザーとは無関係に練習のフォルダー。</span><span class="sxs-lookup"><span data-stu-id="32783-138">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="32783-139">演習の中に追加されるコード スニペットはこれらのスターティング ソリューションが表示されないし、演習を完了するまで動作しない可能性がありますに注意してください。</span><span class="sxs-lookup"><span data-stu-id="32783-139">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="32783-140">演習では、ソース コード内でも表示されます、**エンド**結果から、対応する演習の手順を実行するコードと Visual Studio ソリューションを含むフォルダー。</span><span class="sxs-lookup"><span data-stu-id="32783-140">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="32783-141">このハンズオン ラボを使用すると、追加のヘルプが必要な場合は、これらのソリューションをガイドとして使用できます。</span><span class="sxs-lookup"><span data-stu-id="32783-141">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>


* * *

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="32783-142">演習</span><span class="sxs-lookup"><span data-stu-id="32783-142">Exercises</span></span>

<span data-ttu-id="32783-143">このハンズオン ラボには、次の演習が含まれます。</span><span class="sxs-lookup"><span data-stu-id="32783-143">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="32783-144">SignalR を使用してリアルタイムのデータの使用</span><span class="sxs-lookup"><span data-stu-id="32783-144">Working with Real-Time Data Using SignalR</span></span>](#Exercise1)
2. [<span data-ttu-id="32783-145">SQL Server を使用したスケール アウト</span><span class="sxs-lookup"><span data-stu-id="32783-145">Scaling Out Using SQL Server</span></span>](#Exercise2)

<span data-ttu-id="32783-146">この演習の所要時間を推定: **60 分**</span><span class="sxs-lookup"><span data-stu-id="32783-146">Estimated time to complete this lab: **60 minutes**</span></span>

> [!NOTE]
> <span data-ttu-id="32783-147">Visual Studio を初めて起動すると、定義済みの設定のコレクションの 1 つを選択する必要があります。</span><span class="sxs-lookup"><span data-stu-id="32783-147">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="32783-148">定義済みの各コレクションは、特定の開発スタイルに一致するように設計されていて、ウィンドウのレイアウト、エディターの動作、IntelliSense コード スニペット、およびダイアログ ボックスのオプションを決定します。</span><span class="sxs-lookup"><span data-stu-id="32783-148">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="32783-149">このラボの手順を使用する場合は、Visual Studio で特定のタスクを実行するために必要な操作を記述する、**汎用開発設定**コレクション。</span><span class="sxs-lookup"><span data-stu-id="32783-149">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="32783-150">開発環境のさまざまな設定のコレクションを選択する場合、考慮する必要がある手順に違いがあります。</span><span class="sxs-lookup"><span data-stu-id="32783-150">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>


<a id="Exercise1"></a>
### <a name="exercise-1-working-with-real-time-data-using-signalr"></a><span data-ttu-id="32783-151">手順 1: SignalR を使用してリアルタイムのデータの使用</span><span class="sxs-lookup"><span data-stu-id="32783-151">Exercise 1: Working with Real-Time Data Using SignalR</span></span>

<span data-ttu-id="32783-152">全体を行うことができますチャットの使用が例としては、多くの場合、リアルタイム Web 機能を備えたより。</span><span class="sxs-lookup"><span data-stu-id="32783-152">While chat is often used as an example, you can do a whole lot more with real-time Web functionality.</span></span> <span data-ttu-id="32783-153">ユーザーは、新しいデータまたは長いポーリングが新しいデータを取得する Ajax ページの実装を表示する web ページを更新します。 いつでも SignalR を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="32783-153">Any time a user refreshes a web page to see new data or the page implements Ajax long polling to retrieve new data, you can use SignalR.</span></span>

<span data-ttu-id="32783-154">SignalR をサポートしています**サーバー プッシュ**または**ブロードキャスト**機能; 接続の管理を自動的に処理されます。</span><span class="sxs-lookup"><span data-stu-id="32783-154">SignalR supports **server push** or **broadcasting** functionality; it handles connection management automatically.</span></span> <span data-ttu-id="32783-155">クライアント/サーバー通信のクラシックの HTTP 接続では、接続が要求ごとに再確立ですが SignalR クライアントとサーバー間の永続的な接続を提供します。</span><span class="sxs-lookup"><span data-stu-id="32783-155">In classic HTTP connections for client-server communication, connection is re-established for each request, but SignalR provides persistent connection between the client and server.</span></span> <span data-ttu-id="32783-156">Signalr は、リモート プロシージャ コール (RPC) を使用してブラウザー内のクライアント コードを呼び出すサーバー コードで要求-応答のモデルではなく今日私たちが知る。</span><span class="sxs-lookup"><span data-stu-id="32783-156">In SignalR the server code calls out to a client code in the browser using Remote Procedure Calls (RPC), rather than the request-response model we know today.</span></span>

<span data-ttu-id="32783-157">この演習では、構成、**ギーク Quiz** SignalR を使用して、ページ全体を更新する必要はありません、更新されたメトリックと統計情報のダッシュ ボードを表示するアプリケーション。</span><span class="sxs-lookup"><span data-stu-id="32783-157">In this exercise, you will configure the **Geek Quiz** application to use SignalR to display the Statistics dashboard with the updated metrics without the need to refresh the entire page.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1--exploring-the-geek-quiz-statistics-page"></a><span data-ttu-id="32783-158">タスク 1 – おたくクイズの統計情報 ページの調査</span><span class="sxs-lookup"><span data-stu-id="32783-158">Task 1 – Exploring the Geek Quiz Statistics Page</span></span>

<span data-ttu-id="32783-159">このタスクでは、アプリケーションを経由して統計情報 ページを表示する方法を確認、方法は、情報をどのように向上できる更新されます。</span><span class="sxs-lookup"><span data-stu-id="32783-159">In this task, you will go through the application and verify how the statistics page is shown and how you can improve the way the information is updated.</span></span>

1. <span data-ttu-id="32783-160">開く**Visual Studio Express 2013 for Web**を開くと、 **GeekQuiz.sln**ソリューション、 **Source\Ex1 WorkingWithRealTimeData\Begin**フォルダー。</span><span class="sxs-lookup"><span data-stu-id="32783-160">Open **Visual Studio Express 2013 for Web** and open the **GeekQuiz.sln** solution located in the **Source\Ex1-WorkingWithRealTimeData\Begin** folder.</span></span>
2. <span data-ttu-id="32783-161">キーを押して**F5**ソリューションを実行します。</span><span class="sxs-lookup"><span data-stu-id="32783-161">Press **F5** to run the solution.</span></span> <span data-ttu-id="32783-162">**ログイン**ページがブラウザーに表示する必要があります。</span><span class="sxs-lookup"><span data-stu-id="32783-162">The **Log in** page should appear in the browser.</span></span>

    <span data-ttu-id="32783-163">![ソリューションを実行している](real-time-web-applications-with-signalr/_static/image2.png "ソリューションの実行")</span><span class="sxs-lookup"><span data-stu-id="32783-163">![Running the solution](real-time-web-applications-with-signalr/_static/image2.png "Running the solution")</span></span>

    <span data-ttu-id="32783-164">*ソリューションの実行*</span><span class="sxs-lookup"><span data-stu-id="32783-164">*Running the solution*</span></span>
3. <span data-ttu-id="32783-165">クリックして**登録**アプリケーションに新しいユーザーを作成するページの右上隅にします。</span><span class="sxs-lookup"><span data-stu-id="32783-165">Click **Register** in the upper-right corner of the page to create a new user in the application.</span></span>

    <span data-ttu-id="32783-166">![登録リンク](real-time-web-applications-with-signalr/_static/image3.png "登録リンク")</span><span class="sxs-lookup"><span data-stu-id="32783-166">![Register link](real-time-web-applications-with-signalr/_static/image3.png "Register link")</span></span>

    <span data-ttu-id="32783-167">*登録リンク*</span><span class="sxs-lookup"><span data-stu-id="32783-167">*Register link*</span></span>
4. <span data-ttu-id="32783-168">**登録**ページで、入力、**ユーザー名**と**パスワード**、順にクリックします**登録**します。</span><span class="sxs-lookup"><span data-stu-id="32783-168">In the **Register** page, enter a **User name** and **Password**, and then click **Register**.</span></span>

    <span data-ttu-id="32783-169">![ユーザーの登録](real-time-web-applications-with-signalr/_static/image4.png "ユーザーの登録")</span><span class="sxs-lookup"><span data-stu-id="32783-169">![Registering a user](real-time-web-applications-with-signalr/_static/image4.png "Registering a user")</span></span>

    <span data-ttu-id="32783-170">*ユーザーの登録*</span><span class="sxs-lookup"><span data-stu-id="32783-170">*Registering a user*</span></span>
5. <span data-ttu-id="32783-171">アプリケーションは、新しいアカウントを登録し、ユーザーが認証され、クイズの最初の質問を表示したホーム ページにリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="32783-171">The application registers the new account and the user is authenticated and redirected back to the home page showing the first quiz question.</span></span>
6. <span data-ttu-id="32783-172">開く、**統計**新しいウィンドウでページし、配置、**ホーム**ページと**統計**サイド バイ サイドでのページします。</span><span class="sxs-lookup"><span data-stu-id="32783-172">Open the **Statistics** page in a new window and put the **Home** page and **Statistics** page side-by-side.</span></span>

    <span data-ttu-id="32783-173">![サイド バイ サイドでの windows](real-time-web-applications-with-signalr/_static/image5.png "サイド バイ サイドで側 windows")</span><span class="sxs-lookup"><span data-stu-id="32783-173">![Side-by-side windows](real-time-web-applications-with-signalr/_static/image5.png "Side by side windows")</span></span>

    <span data-ttu-id="32783-174">*サイド バイ サイドでの windows*</span><span class="sxs-lookup"><span data-stu-id="32783-174">*Side-by-side windows*</span></span>
7. <span data-ttu-id="32783-175">**ホーム** ページで、オプションのいずれかをクリックして、質問に回答します。</span><span class="sxs-lookup"><span data-stu-id="32783-175">In the **Home** page, answer the question by clicking one of the options.</span></span>

    <span data-ttu-id="32783-176">![質問に答え](real-time-web-applications-with-signalr/_static/image6.png "質問の回答")</span><span class="sxs-lookup"><span data-stu-id="32783-176">![Answering a question](real-time-web-applications-with-signalr/_static/image6.png "Answering a question")</span></span>

    <span data-ttu-id="32783-177">*質問の回答*</span><span class="sxs-lookup"><span data-stu-id="32783-177">*Answering a question*</span></span>
8. <span data-ttu-id="32783-178">ボタンのいずれかをクリックした後、応答が表示されます。</span><span class="sxs-lookup"><span data-stu-id="32783-178">After clicking one of the buttons, the answer should appear.</span></span>

    <span data-ttu-id="32783-179">![質問の答えが正しい](real-time-web-applications-with-signalr/_static/image7.png "質問の答えが正しい")</span><span class="sxs-lookup"><span data-stu-id="32783-179">![Question answered correct](real-time-web-applications-with-signalr/_static/image7.png "Question answered correct")</span></span>

    <span data-ttu-id="32783-180">*正しく回答する質問*</span><span class="sxs-lookup"><span data-stu-id="32783-180">*Question answered correctly*</span></span>
9. <span data-ttu-id="32783-181">統計情報 ページで提供される情報が古いことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="32783-181">Notice that the information provided in the Statistics page is outdated.</span></span> <span data-ttu-id="32783-182">更新された結果を表示するには、ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="32783-182">Refresh the page in order to see the updated results.</span></span>

    <span data-ttu-id="32783-183">![統計のページ](real-time-web-applications-with-signalr/_static/image8.png "統計情報 ページ")</span><span class="sxs-lookup"><span data-stu-id="32783-183">![Statistics page](real-time-web-applications-with-signalr/_static/image8.png "Statistics page")</span></span>

    <span data-ttu-id="32783-184">*統計情報 ページ*</span><span class="sxs-lookup"><span data-stu-id="32783-184">*Statistics page*</span></span>
10. <span data-ttu-id="32783-185">Visual Studio に戻るし、デバッグを停止します。</span><span class="sxs-lookup"><span data-stu-id="32783-185">Go back to Visual Studio and stop debugging.</span></span>

<a id="Ex1Task2"></a>
#### <a name="task-2--adding-signalr-to-geek-quiz-to-show-online-charts"></a><span data-ttu-id="32783-186">タスク 2 – 追加の SignalR おたくクイズをオンラインのグラフを表示するには</span><span class="sxs-lookup"><span data-stu-id="32783-186">Task 2 – Adding SignalR to Geek Quiz to Show Online Charts</span></span>

<span data-ttu-id="32783-187">このタスクでは、SignalR をソリューションに追加し、サーバーに新しい回答が送信されるときに、クライアントに更新プログラムを自動的に送信します。</span><span class="sxs-lookup"><span data-stu-id="32783-187">In this task, you will add SignalR to the solution and send updates to the clients automatically when a new answer is sent to the server.</span></span>

1. <span data-ttu-id="32783-188">**ツール** メニューの選択 Visual Studio で**ライブラリ パッケージ マネージャー**、 をクリックし、**パッケージ マネージャー コンソール**します。</span><span class="sxs-lookup"><span data-stu-id="32783-188">From the **Tools** menu in Visual Studio, select **Library Package Manager**, and then click **Package Manager Console**.</span></span>
2. <span data-ttu-id="32783-189">**パッケージ マネージャー コンソール**ウィンドウで、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="32783-189">In the **Package Manager Console** window, execute the following command:</span></span>

    [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample1.ps1)]

    <span data-ttu-id="32783-190">![SignalR パッケージのインストール](real-time-web-applications-with-signalr/_static/image9.png "SignalR パッケージのインストール")</span><span class="sxs-lookup"><span data-stu-id="32783-190">![SignalR package installation](real-time-web-applications-with-signalr/_static/image9.png "SignalR package installation")</span></span>

    <span data-ttu-id="32783-191">*SignalR パッケージのインストール*</span><span class="sxs-lookup"><span data-stu-id="32783-191">*SignalR package installation*</span></span>

   > [!NOTE]
   > <span data-ttu-id="32783-192">インストールするときに**SignalR**手動で更新する必要はまったく新しい MVC 5 アプリケーションから NuGet パッケージ バージョン 2.0.2、 **OWIN**パッケージをバージョン 2.0.1 (またはそれ以降) SignalR をインストールする前にします。</span><span class="sxs-lookup"><span data-stu-id="32783-192">When installing **SignalR** NuGet packages version 2.0.2 from a brand new MVC 5 application, you will need to manually update **OWIN** packages to version 2.0.1 (or higher) before installing SignalR.</span></span> <span data-ttu-id="32783-193">これを行うには、以下のスクリプトを実行することができます、**パッケージ マネージャー コンソール**:</span><span class="sxs-lookup"><span data-stu-id="32783-193">To do this, you can execute the following script in the **Package Manager Console**:</span></span>
   > 
   > [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample2.ps1)]
   > 
   > <span data-ttu-id="32783-194">SignalR の将来のリリースで OWIN の依存関係は自動的に更新されます。</span><span class="sxs-lookup"><span data-stu-id="32783-194">In a future release of SignalR, OWIN dependencies will be automatically updated.</span></span>
3. <span data-ttu-id="32783-195">**ソリューション エクスプ ローラー**、展開、**スクリプト**フォルダーに注目してくださいを SignalR *js*ファイル、ソリューションに追加されました。</span><span class="sxs-lookup"><span data-stu-id="32783-195">In **Solution Explorer**, expand the **Scripts** folder and notice that the SignalR *js* files were added to the solution.</span></span>

    <span data-ttu-id="32783-196">![SignalR JavaScript 参照](real-time-web-applications-with-signalr/_static/image10.png "SignalR JavaScript 参照")</span><span class="sxs-lookup"><span data-stu-id="32783-196">![SignalR JavaScript references](real-time-web-applications-with-signalr/_static/image10.png "SignalR JavaScript references")</span></span>

    <span data-ttu-id="32783-197">*SignalR JavaScript 参照*</span><span class="sxs-lookup"><span data-stu-id="32783-197">*SignalR JavaScript references*</span></span>
4. <span data-ttu-id="32783-198">**ソリューション エクスプ ローラー**を右クリックし、 **GeekQuiz**プロジェクトで、**追加** | **新しいフォルダー**、名前を付けます**ハブ**します。</span><span class="sxs-lookup"><span data-stu-id="32783-198">In **Solution Explorer**, right-click the **GeekQuiz** project, select **Add** | **New Folder**, and name it **Hubs**.</span></span>
5. <span data-ttu-id="32783-199">右クリックし、 **Hubs**フォルダーと選択**追加 |新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="32783-199">Right-click the **Hubs** folder and select **Add | New Item**.</span></span>

    <span data-ttu-id="32783-200">![新しい項目の追加](real-time-web-applications-with-signalr/_static/image11.png "新しい項目の追加")</span><span class="sxs-lookup"><span data-stu-id="32783-200">![Add new item](real-time-web-applications-with-signalr/_static/image11.png "Add new item")</span></span>

    <span data-ttu-id="32783-201">*新しい項目を追加します。*</span><span class="sxs-lookup"><span data-stu-id="32783-201">*Add new item*</span></span>
6. <span data-ttu-id="32783-202">**新しい項目の追加**ダイアログ ボックスで、 **(Visual C#) |Web |SignalR**選択の左側のウィンドウでノード**SignalR ハブ クラス (v2)** ファイルの名前を中央のウィンドウから**StatisticsHub.cs**  をクリック**追加**します。</span><span class="sxs-lookup"><span data-stu-id="32783-202">In the **Add New Item** dialog box, select the **Visual C# | Web | SignalR** node in the left pane, select **SignalR Hub Class (v2)** from the center pane, name the file **StatisticsHub.cs** and click **Add**.</span></span>

    <span data-ttu-id="32783-203">![新しい項目 ダイアログ ボックスを追加](real-time-web-applications-with-signalr/_static/image12.png "追加新しい項目 ダイアログ ボックス")</span><span class="sxs-lookup"><span data-stu-id="32783-203">![Add new item dialog box](real-time-web-applications-with-signalr/_static/image12.png "Add new item dialog box")</span></span>

    <span data-ttu-id="32783-204">*新しい項目 ダイアログ ボックスを追加します。*</span><span class="sxs-lookup"><span data-stu-id="32783-204">*Add new item dialog box*</span></span>
7. <span data-ttu-id="32783-205">コードに置き換えます、 **StatisticsHub**クラスを次のコード。</span><span class="sxs-lookup"><span data-stu-id="32783-205">Replace the code in the **StatisticsHub** class with the following code.</span></span>

    <span data-ttu-id="32783-206">(コード スニペット - *RealTimeSignalR - Ex1 - StatisticsHubClass*)</span><span class="sxs-lookup"><span data-stu-id="32783-206">(Code Snippet - *RealTimeSignalR - Ex1 - StatisticsHubClass*)</span></span>

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample3.cs)]
8. <span data-ttu-id="32783-207">開いている**Startup.cs**の末尾に次の行を追加し、**構成**メソッド。</span><span class="sxs-lookup"><span data-stu-id="32783-207">Open **Startup.cs** and add the following line at the end of the **Configuration** method.</span></span>

    <span data-ttu-id="32783-208">(コード スニペット - *RealTimeSignalR - Ex1 - MapSignalR*)</span><span class="sxs-lookup"><span data-stu-id="32783-208">(Code Snippet - *RealTimeSignalR - Ex1 - MapSignalR*)</span></span>

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample4.cs)]
9. <span data-ttu-id="32783-209">開く、 **StatisticsService.cs**内でページ、**サービス**フォルダー以下を追加しますディレクティブを使用します。</span><span class="sxs-lookup"><span data-stu-id="32783-209">Open the **StatisticsService.cs** page inside the **Services** folder and add the following using directives.</span></span>

    <span data-ttu-id="32783-210">(コード スニペット - *RealTimeSignalR - Ex1 - UsingDirectives*)</span><span class="sxs-lookup"><span data-stu-id="32783-210">(Code Snippet - *RealTimeSignalR - Ex1 - UsingDirectives*)</span></span>

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample5.cs)]
10. <span data-ttu-id="32783-211">取得する最初の更新プログラムの接続されているクライアントを通知する、**コンテキスト**現在の接続オブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="32783-211">To notify connected clients of updates, you first retrieve a **Context** object for the current connection.</span></span> <span data-ttu-id="32783-212">**ハブ**オブジェクトには、1 つのクライアントまたはブロードキャストに接続されているすべてのクライアントにメッセージを送信するメソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="32783-212">The **Hub** object contains methods to send messages to a single client or broadcast to all connected clients.</span></span> <span data-ttu-id="32783-213">次のメソッドを追加、 **StatisticsService**統計データをブロードキャストするクラス。</span><span class="sxs-lookup"><span data-stu-id="32783-213">Add the following method to the **StatisticsService** class to broadcast the statistics data.</span></span>

    <span data-ttu-id="32783-214">(コード スニペット - *RealTimeSignalR - Ex1 - NotifyUpdatesMethod*)</span><span class="sxs-lookup"><span data-stu-id="32783-214">(Code Snippet - *RealTimeSignalR - Ex1 - NotifyUpdatesMethod*)</span></span>

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample6.cs)]

    > [!NOTE]
    > <span data-ttu-id="32783-215">上記のコードでは、クライアントで関数を呼び出すため、任意のメソッド名を使用します (つまり: *updateStatistics*)。</span><span class="sxs-lookup"><span data-stu-id="32783-215">In the code above, you are using an arbitrary method name to call a function on the client (i.e.: *updateStatistics*).</span></span> <span data-ttu-id="32783-216">指定したメソッド名は、IntelliSense またはコンパイル時検証がないことを意味の動的オブジェクトとして解釈されます。</span><span class="sxs-lookup"><span data-stu-id="32783-216">The method name that you specify is interpreted as a dynamic object, which means there is no IntelliSense or compile-time validation for it.</span></span> <span data-ttu-id="32783-217">式は、実行時に評価されます。</span><span class="sxs-lookup"><span data-stu-id="32783-217">The expression is evaluated at run time.</span></span> <span data-ttu-id="32783-218">メソッドの呼び出しが実行されると、SignalR は、メソッド名とパラメーターの値をクライアントに送信します。</span><span class="sxs-lookup"><span data-stu-id="32783-218">When the method call executes, SignalR sends the method name and the parameter values to the client.</span></span> <span data-ttu-id="32783-219">クライアントが名前に一致するメソッドの場合、そのメソッドが呼び出され、パラメーターの値が渡されました。</span><span class="sxs-lookup"><span data-stu-id="32783-219">If the client has a method that matches the name, that method is called and the parameter values are passed to it.</span></span> <span data-ttu-id="32783-220">クライアントに一致するメソッドが見つからない場合、エラーは発生しません。</span><span class="sxs-lookup"><span data-stu-id="32783-220">If no matching method is found on the client, no error is raised.</span></span> <span data-ttu-id="32783-221">詳細についてを参照してください[ASP.NET SignalR ハブ API ガイド](../guide-to-the-api/hubs-api-guide-server.md)します。</span><span class="sxs-lookup"><span data-stu-id="32783-221">For more information, refer to [ASP.NET SignalR Hubs API Guide](../guide-to-the-api/hubs-api-guide-server.md).</span></span>
11. <span data-ttu-id="32783-222">開く、 **TriviaController.cs**内でページ、**コント ローラー**フォルダー以下を追加しますディレクティブを使用します。</span><span class="sxs-lookup"><span data-stu-id="32783-222">Open the **TriviaController.cs** page inside the **Controllers** folder and add the following using directives.</span></span>

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample7.cs)]
12. <span data-ttu-id="32783-223">次の強調表示されたコードを追加、 **Post**アクション メソッド。</span><span class="sxs-lookup"><span data-stu-id="32783-223">Add the following highlighted code to the **Post** action method.</span></span>

    <span data-ttu-id="32783-224">(コード スニペット - *RealTimeSignalR - Ex1 - NotifyUpdatesCall*)</span><span class="sxs-lookup"><span data-stu-id="32783-224">(Code Snippet - *RealTimeSignalR - Ex1 - NotifyUpdatesCall*)</span></span>

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample8.cs)]
13. <span data-ttu-id="32783-225">開く、 **Statistics.cshtml**内でページ、**ビュー |ホーム**フォルダー。</span><span class="sxs-lookup"><span data-stu-id="32783-225">Open the **Statistics.cshtml** page inside the **Views | Home** folder.</span></span> <span data-ttu-id="32783-226">検索、**スクリプト**セクションし、セクションの先頭に次のスクリプト参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="32783-226">Locate the **Scripts** section and add the following script references at the beginning of the section.</span></span>

    <span data-ttu-id="32783-227">(コード スニペット - *RealTimeSignalR - Ex1 - SignalRScriptReferences*)</span><span class="sxs-lookup"><span data-stu-id="32783-227">(Code Snippet - *RealTimeSignalR - Ex1 - SignalRScriptReferences*)</span></span>

    [!code-cshtml[Main](real-time-web-applications-with-signalr/samples/sample9.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="32783-228">SignalR およびその他のスクリプト ライブラリを Visual Studio プロジェクトに追加すると、パッケージ マネージャーはこのトピックに示すバージョンよりも新しい SignalR スクリプト ファイルのバージョンをインストール可能性があります。</span><span class="sxs-lookup"><span data-stu-id="32783-228">When you add SignalR and other script libraries to your Visual Studio project, the Package Manager might install a version of the SignalR script file that is more recent than the version shown in this topic.</span></span> <span data-ttu-id="32783-229">コード内のスクリプト参照がプロジェクトにインストールされているスクリプト ライブラリのバージョンと一致することを確認します。</span><span class="sxs-lookup"><span data-stu-id="32783-229">Make sure that the script reference in your code matches the version of the script library installed in your project.</span></span>
14. <span data-ttu-id="32783-230">SignalR ハブにクライアントを接続し、新しいメッセージがハブから受信したときに、統計データを更新する次の強調表示されたコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="32783-230">Add the following highlighted code to connect the client to the SignalR hub and update the statistics data when a new message is received from the hub.</span></span>

    <span data-ttu-id="32783-231">(コード スニペット - *RealTimeSignalR - Ex1 - SignalRClientCode*)</span><span class="sxs-lookup"><span data-stu-id="32783-231">(Code Snippet - *RealTimeSignalR - Ex1 - SignalRClientCode*)</span></span>

    [!code-cshtml[Main](real-time-web-applications-with-signalr/samples/sample10.cshtml)]

    <span data-ttu-id="32783-232">このコードではハブ プロキシの作成され、サーバーによって送信されたメッセージをリッスンするイベント ハンドラーを登録します。</span><span class="sxs-lookup"><span data-stu-id="32783-232">In this code, you are creating a Hub Proxy and registering an event handler to listen for messages sent by the server.</span></span> <span data-ttu-id="32783-233">経由で送信メッセージをリッスンするこの例では、 *updateStatistics*メソッド。</span><span class="sxs-lookup"><span data-stu-id="32783-233">In this case, you listen for messages sent through the *updateStatistics* method.</span></span>

<a id="Ex1Task3"></a>
#### <a name="task-3--running-the-solution"></a><span data-ttu-id="32783-234">タスク 3 – ソリューションの実行</span><span class="sxs-lookup"><span data-stu-id="32783-234">Task 3 – Running the Solution</span></span>

<span data-ttu-id="32783-235">このタスクでは、新しい質問に回答した後は、SignalR を使用して自動的に統計情報 ビューを更新することを確認するソリューションを実行します。</span><span class="sxs-lookup"><span data-stu-id="32783-235">In this task, you will run the solution to verify that the statistics view is updated automatically using SignalR after answering a new question.</span></span>

1. <span data-ttu-id="32783-236">キーを押して**F5**ソリューションを実行します。</span><span class="sxs-lookup"><span data-stu-id="32783-236">Press **F5** to run the solution.</span></span>

    > [!NOTE]
    > <span data-ttu-id="32783-237">場合は、アプリケーションにまだログインして、タスク 1 で作成したユーザーでログインします。</span><span class="sxs-lookup"><span data-stu-id="32783-237">If not already logged in to the application, log in with the user you created in Task 1.</span></span>
2. <span data-ttu-id="32783-238">開く、**統計**新しいウィンドウでページし、配置、**ホーム**ページと**統計**タスク 1 で行ったように、サイド バイ サイドをページします。</span><span class="sxs-lookup"><span data-stu-id="32783-238">Open the **Statistics** page in a new window and put the **Home** page and **Statistics** page side-by-side as you did in Task 1.</span></span>
3. <span data-ttu-id="32783-239">**ホーム** ページで、オプションのいずれかをクリックして、質問に回答します。</span><span class="sxs-lookup"><span data-stu-id="32783-239">In the **Home** page, answer the question by clicking one of the options.</span></span>

    <span data-ttu-id="32783-240">![別の質問に答える](real-time-web-applications-with-signalr/_static/image13.png "別の質問に答える")</span><span class="sxs-lookup"><span data-stu-id="32783-240">![Answering another question](real-time-web-applications-with-signalr/_static/image13.png "Answering another question")</span></span>

    <span data-ttu-id="32783-241">*別の質問に答える*</span><span class="sxs-lookup"><span data-stu-id="32783-241">*Answering another question*</span></span>
4. <span data-ttu-id="32783-242">ボタンのいずれかをクリックした後、応答が表示されます。</span><span class="sxs-lookup"><span data-stu-id="32783-242">After clicking one of the buttons, the answer should appear.</span></span> <span data-ttu-id="32783-243">ページ全体を更新することがなく最新の情報を質問に回答した後、ページ上の統計情報が自動的に更新されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="32783-243">Notice that the Statistics information on the page is updated automatically after answering the question with the updated information without the need to refresh the entire page.</span></span>

    <span data-ttu-id="32783-244">![応答の後に統計情報のページが更新される](real-time-web-applications-with-signalr/_static/image14.png "応答の後に統計情報のページが更新されます。")</span><span class="sxs-lookup"><span data-stu-id="32783-244">![Statistics page refreshed after answer](real-time-web-applications-with-signalr/_static/image14.png "Statistics page refreshed after answer")</span></span>

    <span data-ttu-id="32783-245">*応答の後に更新された統計情報ページ*</span><span class="sxs-lookup"><span data-stu-id="32783-245">*Statistics page refreshed after answer*</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-scaling-out-using-sql-server"></a><span data-ttu-id="32783-246">手順 2: をスケール アウト、SQL Server の使用</span><span class="sxs-lookup"><span data-stu-id="32783-246">Exercise 2: Scaling Out Using SQL Server</span></span>

<span data-ttu-id="32783-247">選択できます一般に、web アプリケーションをスケーリングするときに*スケール アップ*と*スケール アウト*オプション。</span><span class="sxs-lookup"><span data-stu-id="32783-247">When scaling a web application, you can generally choose between *scaling up* and *scaling out* options.</span></span> <span data-ttu-id="32783-248">*スケール アップ*中にその他のリソース (CPU、RAM など) で大規模なサーバーを使用することを意味*スケール アウト*負荷を処理するサーバーを追加することを意味します。</span><span class="sxs-lookup"><span data-stu-id="32783-248">*Scale up* means using a larger server, with more resources (CPU, RAM, etc.) while *scale out* means adding more servers to handle the load.</span></span> <span data-ttu-id="32783-249">後者の場合、問題は、クライアント別のサーバーにルーティングできることです。</span><span class="sxs-lookup"><span data-stu-id="32783-249">The problem with the latter is that the clients can get routed to different servers.</span></span> <span data-ttu-id="32783-250">1 つのサーバーに接続されているクライアントは、別のサーバーから送信されたメッセージを受信しません。</span><span class="sxs-lookup"><span data-stu-id="32783-250">A client that is connected to one server will not receive messages sent from another server.</span></span>

<span data-ttu-id="32783-251">呼ばれるコンポーネントを使用してこれらの問題を解決できる*バック プレーン*サーバー間でメッセージを転送するようにします。</span><span class="sxs-lookup"><span data-stu-id="32783-251">You can solve these issues by using a component called *backplane*, to forward messages between servers.</span></span> <span data-ttu-id="32783-252">有効になっているバック プレーン、各アプリケーション インスタンスが、バック プレーンにメッセージを送信し、バック プレーンの他のアプリケーション インスタンスに転送します。</span><span class="sxs-lookup"><span data-stu-id="32783-252">With a backplane enabled, each application instance sends messages to the backplane, and the backplane forwards them to the other application instances.</span></span>

<span data-ttu-id="32783-253">現在は、SignalR のバック プレーンの 3 つの種類があります。</span><span class="sxs-lookup"><span data-stu-id="32783-253">There are currently three types of backplanes for SignalR:</span></span>

- <span data-ttu-id="32783-254">**Windows Azure Service Bus**します。</span><span class="sxs-lookup"><span data-stu-id="32783-254">**Windows Azure Service Bus**.</span></span> <span data-ttu-id="32783-255">Service Bus は、メッセージング インフラストラクチャを使用する疎結合のメッセージを送信するコンポーネントです。</span><span class="sxs-lookup"><span data-stu-id="32783-255">Service Bus is a messaging infrastructure that allows components to send loosely coupled messages.</span></span>
- <span data-ttu-id="32783-256">**SQL Server**します。</span><span class="sxs-lookup"><span data-stu-id="32783-256">**SQL Server**.</span></span> <span data-ttu-id="32783-257">SQL Server バック プレーンでは、SQL テーブルにメッセージを書き込みます。</span><span class="sxs-lookup"><span data-stu-id="32783-257">The SQL Server backplane writes messages to SQL tables.</span></span> <span data-ttu-id="32783-258">バック プレーンでは、効率的なメッセージングを Service Broker を使用します。</span><span class="sxs-lookup"><span data-stu-id="32783-258">The backplane uses Service Broker for efficient messaging.</span></span> <span data-ttu-id="32783-259">ただし、これは Service Broker が有効になっていない場合にも機能します。</span><span class="sxs-lookup"><span data-stu-id="32783-259">However, it also works if Service Broker is not enabled.</span></span>
- <span data-ttu-id="32783-260">**Redis**します。</span><span class="sxs-lookup"><span data-stu-id="32783-260">**Redis**.</span></span> <span data-ttu-id="32783-261">Redis はメモリ内のキー値ストアです。</span><span class="sxs-lookup"><span data-stu-id="32783-261">Redis is an in-memory key-value store.</span></span> <span data-ttu-id="32783-262">Redis では、メッセージを送信するため、(「パブリッシュ/サブスクライブ」) の発行/サブスクライブ パターンをサポートしています。</span><span class="sxs-lookup"><span data-stu-id="32783-262">Redis supports a publish/subscribe ("pub/sub") pattern for sending messages.</span></span>

<span data-ttu-id="32783-263">すべてのメッセージは、メッセージ バスを通じて送信されます。</span><span class="sxs-lookup"><span data-stu-id="32783-263">Every message is sent through a message bus.</span></span> <span data-ttu-id="32783-264">メッセージ バス実装、 [IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx)インターフェイスで、パブリッシュ/サブスクライブ抽象化を提供します。</span><span class="sxs-lookup"><span data-stu-id="32783-264">A message bus implements the [IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx) interface, which provides a publish/subscribe abstraction.</span></span> <span data-ttu-id="32783-265">既定値を置き換えることで、バック プレーンの動作**IMessageBus**バス バック プレーンに対するに設計されています。</span><span class="sxs-lookup"><span data-stu-id="32783-265">The backplanes work by replacing the default **IMessageBus** with a bus designed for that backplane.</span></span>

<span data-ttu-id="32783-266">各サーバー インスタンスは、バスを介してバック プレーンに接続します。</span><span class="sxs-lookup"><span data-stu-id="32783-266">Each server instance connects to the backplane through the bus.</span></span> <span data-ttu-id="32783-267">メッセージが送信されると、バック プレーンに移動して、バック プレーンのすべてのサーバーに送信します。</span><span class="sxs-lookup"><span data-stu-id="32783-267">When a message is sent, it goes to the backplane, and the backplane sends it to every server.</span></span> <span data-ttu-id="32783-268">サーバーは、バック プレーンからメッセージを受信したときに、そのローカル キャッシュ内で、メッセージを格納します。</span><span class="sxs-lookup"><span data-stu-id="32783-268">When a server receives a message from the backplane, it stores the message in its local cache.</span></span> <span data-ttu-id="32783-269">サーバーはし、そのローカル キャッシュから、クライアントにメッセージを配信します。</span><span class="sxs-lookup"><span data-stu-id="32783-269">The server then delivers messages to clients from its local cache.</span></span>

<span data-ttu-id="32783-270">SignalR のバック プレーンの動作は、こちらの詳細については[記事](../performance/scaleout-in-signalr.md)します。</span><span class="sxs-lookup"><span data-stu-id="32783-270">For more information about how the SignalR backplane works, read this [article](../performance/scaleout-in-signalr.md).</span></span>

> [!NOTE]
> <span data-ttu-id="32783-271">バック プレーンがボトルネックになることがいくつかのシナリオがあります。</span><span class="sxs-lookup"><span data-stu-id="32783-271">There are some scenarios where a backplane can become a bottleneck.</span></span> <span data-ttu-id="32783-272">SignalR の一般的なシナリオを次に示します。</span><span class="sxs-lookup"><span data-stu-id="32783-272">Here are some typical SignalR scenarios:</span></span>
> 
> - <span data-ttu-id="32783-273">[サーバー ブロードキャスト](tutorial-server-broadcast-with-signalr.md)(たとえば、株価表示器): サーバー メッセージが送信される速度を制御するために、バック プレーンがこのシナリオに動作します。</span><span class="sxs-lookup"><span data-stu-id="32783-273">[Server broadcast](tutorial-server-broadcast-with-signalr.md) (e.g., stock ticker): Backplanes work well for this scenario, because the server controls the rate at which messages are sent.</span></span>
> - <span data-ttu-id="32783-274">[クライアントで](tutorial-getting-started-with-signalr.md)(チャットなど)。 このシナリオでのバック プレーン場合がありますボトルネックになっているクライアントの数のメッセージの数に合わせて; は、メッセージの数が増えた場合それに比例して増えるクライアントが参加します。</span><span class="sxs-lookup"><span data-stu-id="32783-274">[Client-to-client](tutorial-getting-started-with-signalr.md) (e.g., chat): In this scenario, the backplane might be a bottleneck if the number of messages scales with the number of clients; that is, if the rate of messages grows proportionally as more clients join.</span></span>
> - <span data-ttu-id="32783-275">[高頻度リアルタイム メッセージング](tutorial-high-frequency-realtime-with-signalr.md)(リアルタイムのゲームなど)。 このシナリオのバック プレーンは推奨されません。</span><span class="sxs-lookup"><span data-stu-id="32783-275">[High-frequency realtime](tutorial-high-frequency-realtime-with-signalr.md) (e.g., real-time games): A backplane is not recommended for this scenario.</span></span>


<span data-ttu-id="32783-276">この演習では使用して**SQL Server**間でメッセージを配布する、**ギーク Quiz**アプリケーション。</span><span class="sxs-lookup"><span data-stu-id="32783-276">In this exercise, you will use **SQL Server** to distribute messages across the **Geek Quiz** application.</span></span> <span data-ttu-id="32783-277">完全な効果を取得するために、構成を設定する方法については、1 つのテスト マシンでこれらのタスクを実行するは、SignalR アプリケーションを 2 つまたは複数のサーバーをデプロイする必要があります。</span><span class="sxs-lookup"><span data-stu-id="32783-277">You will run these tasks on a single test machine to learn how to set up the configuration, but in order to get the full effect, you will need to deploy the SignalR application to two or more servers.</span></span> <span data-ttu-id="32783-278">サーバーのいずれか、または別の専用サーバーでは、SQL Server をインストールすることもする必要があります。</span><span class="sxs-lookup"><span data-stu-id="32783-278">You must also install SQL Server on one of the servers, or on a separate dedicated server.</span></span>

![SQL Server の図を使用してスケール](real-time-web-applications-with-signalr/_static/image15.png)

<a id="Ex2Task1"></a>
#### <a name="task-1---understanding-the-scenario"></a><span data-ttu-id="32783-280">タスク 1 - シナリオを理解します。</span><span class="sxs-lookup"><span data-stu-id="32783-280">Task 1 - Understanding the Scenario</span></span>

<span data-ttu-id="32783-281">このタスクでは、2 つのインスタンスを実行する**ギーク Quiz**インスタンスをローカル コンピューターに複数の IIS をシミュレートします。</span><span class="sxs-lookup"><span data-stu-id="32783-281">In this task, you will run 2 instances of **Geek Quiz** simulating multiple IIS instances on your local machine.</span></span> <span data-ttu-id="32783-282">この場合、1 つのアプリケーションのトリビアの質問に応答するときは、2 番目のインスタンスの統計情報 ページで更新プログラムを通知しません。</span><span class="sxs-lookup"><span data-stu-id="32783-282">In this scenario, when answering trivia questions on one application, update won't be notified on the statistics page of the second instance.</span></span> <span data-ttu-id="32783-283">このシミュレーションよう、アプリケーションが複数のインスタンスに配置される場所、環境と通信するロード バランサーを使用します。</span><span class="sxs-lookup"><span data-stu-id="32783-283">This simulation resembles an environment where your application is deployed on multiple instances and using a load balancer to communicate with them.</span></span>

1. <span data-ttu-id="32783-284">開く、 **Begin.sln**ソリューション、**ソース/Ex2-ScalingOutWithSQLServer/開始**フォルダー。</span><span class="sxs-lookup"><span data-stu-id="32783-284">Open the **Begin.sln** solution located in the **Source/Ex2-ScalingOutWithSQLServer/Begin** folder.</span></span> <span data-ttu-id="32783-285">読み込まれた後に表示されます、**サーバー エクスプ ローラー**構造体が異なる名前のソリューションには、同一である 2 つのプロジェクトが含まれています。</span><span class="sxs-lookup"><span data-stu-id="32783-285">Once loaded, you will notice on the **Server Explorer** that the solution has two projects with identical structures but different names.</span></span> <span data-ttu-id="32783-286">これは、ローカル コンピューターに、同じアプリケーションの 2 つのインスタンスを実行しているシミュレートします。</span><span class="sxs-lookup"><span data-stu-id="32783-286">This will simulate running two instances of the same application on your local machine.</span></span>

    <span data-ttu-id="32783-287">![コンピューターおたくクイズの 2 つのインスタンスをシミュレートするソリューションの開始](real-time-web-applications-with-signalr/_static/image16.png "おたくクイズの 2 つのインスタンスをシミュレートするソリューションの開始")</span><span class="sxs-lookup"><span data-stu-id="32783-287">![Begin Solution Simulating 2 Instances of Geek Quiz](real-time-web-applications-with-signalr/_static/image16.png "Begin Solution Simulating 2 Instances of Geek Quiz")</span></span>

    <span data-ttu-id="32783-288">*コンピューターおたくクイズの 2 つのインスタンスをシミュレートするソリューションを開始します。*</span><span class="sxs-lookup"><span data-stu-id="32783-288">*Begin Solution Simulating 2 Instances of Geek Quiz*</span></span>
2. <span data-ttu-id="32783-289">ソリューション ノードを右クリックして、ソリューションのプロパティ ページを開く**プロパティ**します。</span><span class="sxs-lookup"><span data-stu-id="32783-289">Open the properties page of the solution by right-clicking the solution node and selecting **Properties**.</span></span> <span data-ttu-id="32783-290">**スタートアップ プロジェクト**を選択します**マルチ スタートアップ プロジェクト**を変更して、**アクション**両方のプロジェクトに対して値*開始*します。</span><span class="sxs-lookup"><span data-stu-id="32783-290">Under **Startup Project**, select **Multiple startup projects** and change the **Action** value for both projects to *Start*.</span></span>

    <span data-ttu-id="32783-291">![複数のプロジェクトを開始](real-time-web-applications-with-signalr/_static/image17.png "複数のプロジェクトを開始")</span><span class="sxs-lookup"><span data-stu-id="32783-291">![Starting Multiple Projects](real-time-web-applications-with-signalr/_static/image17.png "Starting Multiple Projects")</span></span>

    <span data-ttu-id="32783-292">*複数のプロジェクトを開始*</span><span class="sxs-lookup"><span data-stu-id="32783-292">*Starting Multiple Projects*</span></span>
3. <span data-ttu-id="32783-293">キーを押して**F5**ソリューションを実行します。</span><span class="sxs-lookup"><span data-stu-id="32783-293">Press **F5** to run the solution.</span></span> <span data-ttu-id="32783-294">アプリケーションが 2 つのインスタンスを起動**ギーク Quiz**別々 のポートで同じアプリケーションの複数のインスタンスをシミュレートします。</span><span class="sxs-lookup"><span data-stu-id="32783-294">The application will launch two instances of **Geek Quiz** in different ports, simulating multiple instances of the same application.</span></span> <span data-ttu-id="32783-295">左辺と他の画面の右側で、ブラウザーのいずれかをピン留めします。</span><span class="sxs-lookup"><span data-stu-id="32783-295">Pin one of the browsers on left and the other on the right of your screen.</span></span> <span data-ttu-id="32783-296">資格情報でログインするか、新しいユーザーを登録します。</span><span class="sxs-lookup"><span data-stu-id="32783-296">Log in with your credentials or register a new user.</span></span> <span data-ttu-id="32783-297">ログインすると、左側のトリビアのページを保持しに移動、**統計**右側のブラウザーでページ。</span><span class="sxs-lookup"><span data-stu-id="32783-297">Once logged in, keep the Trivia page on the left and go to the **Statistics** page in the browser on the right.</span></span>

    ![サイド バイ サイドのおたくクイズ](real-time-web-applications-with-signalr/_static/image18.png)

    <span data-ttu-id="32783-299">*サイド バイ サイドのおたくクイズ*</span><span class="sxs-lookup"><span data-stu-id="32783-299">*Geek Quiz Side by Side*</span></span>

    ![別のポートのおたくのクイズ](real-time-web-applications-with-signalr/_static/image19.png)

    <span data-ttu-id="32783-301">*別のポートのおたくのクイズ*</span><span class="sxs-lookup"><span data-stu-id="32783-301">*Geek Quiz in Different Ports*</span></span>
4. <span data-ttu-id="32783-302">左側のブラウザーでの質問に回答を起動し、表示になります、**統計**適切なブラウザーでページが更新されません。</span><span class="sxs-lookup"><span data-stu-id="32783-302">Start answering questions in the left browser and you will notice that the **Statistics** page in the right browser is not being updated.</span></span> <span data-ttu-id="32783-303">これは、ため**SignalR**クライアント間でメッセージを配布する、ローカル キャッシュの使用、このシナリオでは、複数のインスタンスをシミュレートします。 そのため、キャッシュはそれらの間で共有されません。</span><span class="sxs-lookup"><span data-stu-id="32783-303">This is because **SignalR** uses a local cache to distribute messages across their clients and this scenario is simulating multiple instances, therefore the cache is not shared between them.</span></span> <span data-ttu-id="32783-304">確認できます**SignalR**で同じ手順をテストが 1 つのアプリを使用して、動作します。</span><span class="sxs-lookup"><span data-stu-id="32783-304">You can verify that **SignalR** is working by testing the same steps but using a single app.</span></span> <span data-ttu-id="32783-305">次のタスクでは、インスタンス間でメッセージをレプリケート バック プレーンを構成します。</span><span class="sxs-lookup"><span data-stu-id="32783-305">In the following tasks you will configure a backplane to replicate the messages across instances.</span></span>
5. <span data-ttu-id="32783-306">Visual Studio に戻るし、デバッグを停止します。</span><span class="sxs-lookup"><span data-stu-id="32783-306">Go back to Visual Studio and stop debugging.</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-sql-server-backplane"></a><span data-ttu-id="32783-307">タスク 2 – SQL Server のバック プレーンの作成</span><span class="sxs-lookup"><span data-stu-id="32783-307">Task 2 – Creating the SQL Server Backplane</span></span>

<span data-ttu-id="32783-308">このタスクでのバック プレーンとして使用するデータベースを作成するが、**ギーク Quiz**アプリケーション。</span><span class="sxs-lookup"><span data-stu-id="32783-308">In this task, you will create a database that will serve as a backplane for the **Geek Quiz** application.</span></span> <span data-ttu-id="32783-309">使用する**SQL Server オブジェクト エクスプ ローラー**をサーバーを参照し、データベースを初期化します。</span><span class="sxs-lookup"><span data-stu-id="32783-309">You will use **SQL Server Object Explorer** to browse your server and initialize the database.</span></span> <span data-ttu-id="32783-310">さらに、有効にする、 **Service Broker**します。</span><span class="sxs-lookup"><span data-stu-id="32783-310">Additionally, you will enable the **Service Broker**.</span></span>

1. <span data-ttu-id="32783-311">**Visual Studio**開き、メニュー**ビュー**選択と**SQL Server オブジェクト エクスプ ローラー**します。</span><span class="sxs-lookup"><span data-stu-id="32783-311">In **Visual Studio**, open menu **View** and select **SQL Server Object Explorer**.</span></span>
2. <span data-ttu-id="32783-312">右クリックして、LocalDB インスタンスへの接続、 **SQL Server**ノードと選択**SQL Server の追加.** オプション。</span><span class="sxs-lookup"><span data-stu-id="32783-312">Connect to your LocalDB instance by right-clicking the **SQL Server** node and selecting **Add SQL Server...** option.</span></span>

    <span data-ttu-id="32783-313">![SQL Server のインスタンスを追加する](real-time-web-applications-with-signalr/_static/image20.png "SQL Server のインスタンスを追加します。")</span><span class="sxs-lookup"><span data-stu-id="32783-313">![Adding a SQL Server Instance](real-time-web-applications-with-signalr/_static/image20.png "Adding a SQL Server Instance")</span></span>

    <span data-ttu-id="32783-314">*SQL Server オブジェクト エクスプ ローラーへの SQL Server インスタンスの追加*</span><span class="sxs-lookup"><span data-stu-id="32783-314">*Adding a SQL Server instance to SQL Server Object Explorer*</span></span>
3. <span data-ttu-id="32783-315">設定、**サーバー名**に *(localdb) \v11.0*して**Windows 認証**認証モードとして。</span><span class="sxs-lookup"><span data-stu-id="32783-315">Set the **server name** to *(localdb)\v11.0* and leave **Windows Authentication** as your authentication mode.</span></span> <span data-ttu-id="32783-316">クリックして**Connect**を続行します。</span><span class="sxs-lookup"><span data-stu-id="32783-316">Click **Connect** to continue.</span></span>

    <span data-ttu-id="32783-317">![LocalDB への接続](real-time-web-applications-with-signalr/_static/image21.png "LocalDB への接続")</span><span class="sxs-lookup"><span data-stu-id="32783-317">![Connecting to LocalDB](real-time-web-applications-with-signalr/_static/image21.png "Connecting to LocalDB")</span></span>

    <span data-ttu-id="32783-318">*LocalDB への接続*</span><span class="sxs-lookup"><span data-stu-id="32783-318">*Connecting to LocalDB*</span></span>
4. <span data-ttu-id="32783-319">これで、LocalDB インスタンスに接続すると、SignalR の SQL Server バック プレーンを表すデータベースを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="32783-319">Now that you are connected to your LocalDB instance, you will need to create a database that will represent the SQL Server backplane for SignalR.</span></span> <span data-ttu-id="32783-320">これを行うを右クリックし、**データベース**ノード**新しいデータベースの追加**します。</span><span class="sxs-lookup"><span data-stu-id="32783-320">To do this, right-click the **Databases** node and select **Add New Database**.</span></span>

    <span data-ttu-id="32783-321">![新しいデータベースを追加する](real-time-web-applications-with-signalr/_static/image22.png "の新しいデータベースの追加")</span><span class="sxs-lookup"><span data-stu-id="32783-321">![Adding a new database](real-time-web-applications-with-signalr/_static/image22.png "Adding a new database")</span></span>

    <span data-ttu-id="32783-322">*新しいデータベースを追加します。*</span><span class="sxs-lookup"><span data-stu-id="32783-322">*Adding a new database*</span></span>
5. <span data-ttu-id="32783-323">データベース名を設定*SignalR*  をクリック**OK**を作成します。</span><span class="sxs-lookup"><span data-stu-id="32783-323">Set the database name to *SignalR* and click **OK** to create it.</span></span>

    <span data-ttu-id="32783-324">![SignalR のデータベースを作成する](real-time-web-applications-with-signalr/_static/image23.png "SignalR データベースを作成します。")</span><span class="sxs-lookup"><span data-stu-id="32783-324">![Creating the SignalR database](real-time-web-applications-with-signalr/_static/image23.png "Creating the SignalR database")</span></span>

    <span data-ttu-id="32783-325">*SignalR のデータベースを作成します。*</span><span class="sxs-lookup"><span data-stu-id="32783-325">*Creating the SignalR database*</span></span>

    > [!NOTE]
    > <span data-ttu-id="32783-326">データベースの任意の名前を選択できます。</span><span class="sxs-lookup"><span data-stu-id="32783-326">You can choose any name for the database.</span></span>
6. <span data-ttu-id="32783-327">バック プレーンからより効率的に更新プログラムを受信するには、データベースの Service Broker を有効にすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="32783-327">To receive updates more efficiently from the backplane, it is recommended to enable Service Broker for the database.</span></span> <span data-ttu-id="32783-328">Service Broker では、メッセージングと SQL Server でのキューのネイティブ サポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="32783-328">Service Broker provides native support for messaging and queuing in SQL Server.</span></span> <span data-ttu-id="32783-329">バック プレーンは、Service Broker なくも機能します。</span><span class="sxs-lookup"><span data-stu-id="32783-329">The backplane also works without Service Broker.</span></span> <span data-ttu-id="32783-330">クリックし、データベースを右クリックして新しいクエリを開く**新しいクエリ**します。</span><span class="sxs-lookup"><span data-stu-id="32783-330">Open a new query by right-clicking the database and select **New Query**.</span></span>

    <span data-ttu-id="32783-331">![新しいクエリを開いて、](real-time-web-applications-with-signalr/_static/image24.png "新しいクエリを開く")</span><span class="sxs-lookup"><span data-stu-id="32783-331">![Opening a New Query](real-time-web-applications-with-signalr/_static/image24.png "Opening a New Query")</span></span>

    <span data-ttu-id="32783-332">*新しいクエリを開く*</span><span class="sxs-lookup"><span data-stu-id="32783-332">*Opening a New Query*</span></span>
7. <span data-ttu-id="32783-333">Service Broker が有効になっているかどうかを確認するには、クエリ、**は\_broker\_有効になっている**内の列、 **sys.databases**カタログ ビューです。</span><span class="sxs-lookup"><span data-stu-id="32783-333">To check whether Service Broker is enabled, query the **is\_broker\_enabled** column in the **sys.databases** catalog view.</span></span> <span data-ttu-id="32783-334">最近開いたクエリ ウィンドウで、次のスクリプトを実行します。</span><span class="sxs-lookup"><span data-stu-id="32783-334">Execute the following script in the recently opened query window.</span></span>

    [!code-sql[Main](real-time-web-applications-with-signalr/samples/sample11.sql)]

    <span data-ttu-id="32783-335">![サービス ブローカーの状態を照会](real-time-web-applications-with-signalr/_static/image25.png "サービス ブローカーの状態のクエリを実行します。")</span><span class="sxs-lookup"><span data-stu-id="32783-335">![Querying the Service Broker Status](real-time-web-applications-with-signalr/_static/image25.png "Querying the Service Broker Status")</span></span>

    <span data-ttu-id="32783-336">*サービス ブローカーの状態のクエリを実行します。*</span><span class="sxs-lookup"><span data-stu-id="32783-336">*Querying the Service Broker Status*</span></span>
8. <span data-ttu-id="32783-337">場合の値、**は\_broker\_有効になっている**、データベース内の列が&quot;0&quot;、有効にする、次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="32783-337">If the value of the **is\_broker\_enabled** column in your database is &quot;0&quot;, use the following command to enable it.</span></span> <span data-ttu-id="32783-338">置換 **&lt;、データベース&gt;** データベースを作成するときに設定した名前 (例:: SignalR)。</span><span class="sxs-lookup"><span data-stu-id="32783-338">Replace **&lt;YOUR-DATABASE&gt;** with the name you set when creating the database (e.g.: SignalR).</span></span>

    [!code-sql[Main](real-time-web-applications-with-signalr/samples/sample12.sql)]

    <span data-ttu-id="32783-339">![Service Broker を有効にする](real-time-web-applications-with-signalr/_static/image26.png "Service Broker を有効にします。")</span><span class="sxs-lookup"><span data-stu-id="32783-339">![Enabling Service Broker](real-time-web-applications-with-signalr/_static/image26.png "Enabling Service Broker")</span></span>

    <span data-ttu-id="32783-340">*Service Broker を有効にします。*</span><span class="sxs-lookup"><span data-stu-id="32783-340">*Enabling Service Broker*</span></span>

    > [!NOTE]
    > <span data-ttu-id="32783-341">このクエリは、デッドロックを確認が表示された場合、DB に接続されているアプリケーションはありません。</span><span class="sxs-lookup"><span data-stu-id="32783-341">If this query appears to deadlock, make sure there are no applications connected to the DB.</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3--configuring-the-signalr-application"></a><span data-ttu-id="32783-342">タスク 3 – SignalR アプリケーションを構成します。</span><span class="sxs-lookup"><span data-stu-id="32783-342">Task 3 – Configuring the SignalR Application</span></span>

<span data-ttu-id="32783-343">このタスクでは、構成**ギーク Quiz** SQL Server バック プレーンに接続します。</span><span class="sxs-lookup"><span data-stu-id="32783-343">In this task, you will configure **Geek Quiz** to connect to the SQL Server backplane.</span></span> <span data-ttu-id="32783-344">最初に、追加、 **SignalR.SqlServer**バック プレーン データベースに NuGet パッケージと、接続設定の文字列します。</span><span class="sxs-lookup"><span data-stu-id="32783-344">You will first add the **SignalR.SqlServer** NuGet package and set the connection string to your backplane database.</span></span>

1. <span data-ttu-id="32783-345">開く、**パッケージ マネージャー コンソール**から**ツール** | **ライブラリ パッケージ マネージャー**します。</span><span class="sxs-lookup"><span data-stu-id="32783-345">Open the **Package Manager Console** from **Tools** | **Library Package Manager**.</span></span> <span data-ttu-id="32783-346">確認します**GeekQuiz**でプロジェクトを選択、**既定のプロジェクト**ドロップダウン リスト。</span><span class="sxs-lookup"><span data-stu-id="32783-346">Make sure that **GeekQuiz** project is selected in the **Default project** drop-down list.</span></span> <span data-ttu-id="32783-347">インストールするには、次のコマンドを入力、 **Microsoft.AspNet.SignalR.SqlServer** NuGet パッケージ。</span><span class="sxs-lookup"><span data-stu-id="32783-347">Type the following command to install the **Microsoft.AspNet.SignalR.SqlServer** NuGet package.</span></span>

    [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample13.ps1)]
2. <span data-ttu-id="32783-348">プロジェクトに対してこの時間が、前の手順を繰り返します**GeekQuiz2**します。</span><span class="sxs-lookup"><span data-stu-id="32783-348">Repeat the previous step but this time for project **GeekQuiz2**.</span></span>
3. <span data-ttu-id="32783-349">SQL Server バック プレーンを構成するには、開く、 **Startup.cs**のファイル、 **GeekQuiz**プロジェクトし、次のコードを追加、**構成**メソッド。</span><span class="sxs-lookup"><span data-stu-id="32783-349">To configure the SQL Server backplane, open the **Startup.cs** file of the **GeekQuiz** project and add the following code to the **Configure** method.</span></span> <span data-ttu-id="32783-350">置換**&lt;YOUR DATABASE&gt;** を SQL Server バック プレーンを作成するときに使用したデータベース名。</span><span class="sxs-lookup"><span data-stu-id="32783-350">Replace **&lt;YOUR-DATABASE&gt;** with your database name you used when creating the SQL Server backplane.</span></span> <span data-ttu-id="32783-351">この手順を繰り返します、 **GeekQuiz2**プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="32783-351">Repeat this step for the **GeekQuiz2** project.</span></span>

    <span data-ttu-id="32783-352">(コード スニペット - *RealTimeSignalR - Ex2 - StartupConfiguration*)</span><span class="sxs-lookup"><span data-stu-id="32783-352">(Code Snippet - *RealTimeSignalR - Ex2 - StartupConfiguration*)</span></span>

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample14.cs)]
4. <span data-ttu-id="32783-353">これで、両方のプロジェクトは、SQL Server バック プレーンを使用する構成は、次のようにキーを押して**F5**それらを同時に実行します。</span><span class="sxs-lookup"><span data-stu-id="32783-353">Now that both projects are configured to use the SQL Server backplane, press **F5** to run them simultaneously.</span></span>
5. <span data-ttu-id="32783-354">ここでも、 **Visual Studio**の 2 つのインスタンスが起動**ギーク Quiz**で別のポート。</span><span class="sxs-lookup"><span data-stu-id="32783-354">Again, **Visual Studio** will launch two instances of **Geek Quiz** in different ports.</span></span> <span data-ttu-id="32783-355">他の画面の右側および左側に、ブラウザーのいずれかをピン留めし、資格情報でログインします。</span><span class="sxs-lookup"><span data-stu-id="32783-355">Pin one of the browsers on the left and the other on the right of your screen and log in with your credentials.</span></span> <span data-ttu-id="32783-356">左側のトリビアのページを保持しに移動**統計**ページイン適切なブラウザー。</span><span class="sxs-lookup"><span data-stu-id="32783-356">Keep the Trivia page on the left and go to **Statistics** pagein the right browser.</span></span>
6. <span data-ttu-id="32783-357">左側のブラウザーでの質問に答えることを開始します。</span><span class="sxs-lookup"><span data-stu-id="32783-357">Start answering questions in the left browser.</span></span> <span data-ttu-id="32783-358">今回は、**統計**バック プレーンに協力してくれたページが更新されます。</span><span class="sxs-lookup"><span data-stu-id="32783-358">This time, the **Statistics** page is updated thanks to the backplane.</span></span> <span data-ttu-id="32783-359">アプリケーションを切り替える (**統計**、左側のようになりましたが、**トリビア**が右側) ことが、両方のインスタンスの動作を検証するテストを繰り返すとします。</span><span class="sxs-lookup"><span data-stu-id="32783-359">Switch between applications (**Statistics** is now on the left, and **Trivia** is on the right) and repeat the test to validate that it is working for both instances.</span></span> <span data-ttu-id="32783-360">バック プレーンとして、*共有キャッシュ*、接続されている各サーバーと各サーバーのメッセージは、メッセージを保存する接続されているクライアントに配布するための独自のローカル キャッシュです。</span><span class="sxs-lookup"><span data-stu-id="32783-360">The backplane serves as a *shared cache* of messages for each connected server, and each server will store the messages in their own local cache to distribute to connected clients.</span></span>
7. <span data-ttu-id="32783-361">Visual Studio に戻るし、デバッグを停止します。</span><span class="sxs-lookup"><span data-stu-id="32783-361">Go back to Visual Studio and stop debugging.</span></span>
8. <span data-ttu-id="32783-362">SQL Server のバック プレーン コンポーネントには、指定したデータベースで必要なテーブルが自動的に生成されます。</span><span class="sxs-lookup"><span data-stu-id="32783-362">The SQL Server backplane component automatically generates the necessary tables on the specified database.</span></span> <span data-ttu-id="32783-363">**SQL Server オブジェクト エクスプ ローラー**パネルで、バック プレーン用に作成したデータベースを開きます (例:: SignalR) し、そのテーブルを展開します。</span><span class="sxs-lookup"><span data-stu-id="32783-363">In the **SQL Server Object Explorer** panel, open the database you created for the backplane (e.g.: SignalR) and expand its tables.</span></span> <span data-ttu-id="32783-364">次の表が表示されます。</span><span class="sxs-lookup"><span data-stu-id="32783-364">You should see the following tables:</span></span>

    ![生成されたテーブルのバック プレーン](real-time-web-applications-with-signalr/_static/image27.png)

    <span data-ttu-id="32783-366">*生成されたテーブルのバック プレーン*</span><span class="sxs-lookup"><span data-stu-id="32783-366">*Backplane Generated Tables*</span></span>
9. <span data-ttu-id="32783-367">右クリックし、 **SignalR.Messages\_0**テーブルを選択**ビュー データ**します。</span><span class="sxs-lookup"><span data-stu-id="32783-367">Right-click the **SignalR.Messages\_0** table and select **View Data**.</span></span>

    ![SignalR のバック プレーン メッセージ テーブルな表示](real-time-web-applications-with-signalr/_static/image28.png)

    <span data-ttu-id="32783-369">*SignalR のバック プレーン メッセージ テーブルな表示*</span><span class="sxs-lookup"><span data-stu-id="32783-369">*View SignalR Backplane Messages Table*</span></span>
10. <span data-ttu-id="32783-370">送信されたさまざまなメッセージを表示、**ハブ**トリビアの質問に答えるときです。</span><span class="sxs-lookup"><span data-stu-id="32783-370">You can see the different messages sent to the **Hub** when answering the trivia questions.</span></span> <span data-ttu-id="32783-371">バック プレーンでは、接続されている任意のインスタンスにこれらのメッセージを配布します。</span><span class="sxs-lookup"><span data-stu-id="32783-371">The backplane distributes these messages to any connected instance.</span></span>

    ![バック プレーン メッセージ テーブル](real-time-web-applications-with-signalr/_static/image29.png)

    <span data-ttu-id="32783-373">*バック プレーン メッセージ テーブル*</span><span class="sxs-lookup"><span data-stu-id="32783-373">*Backplane Messages Table*</span></span>

* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="32783-374">まとめ</span><span class="sxs-lookup"><span data-stu-id="32783-374">Summary</span></span>

<span data-ttu-id="32783-375">このハンズオン ラボでは、追加する方法を説明しました**SignalR**を使用して、接続されているクライアントをサーバーからアプリケーションおよび送信通知に**Hubs**します。</span><span class="sxs-lookup"><span data-stu-id="32783-375">In this hands-on lab, you have learned how to add **SignalR** to your application and send notifications from the server to your connected clients using **Hubs**.</span></span> <span data-ttu-id="32783-376">さらを使用して、アプリケーションをスケーリングする方法について説明しました、*バック プレーン*コンポーネントの複数の IIS インスタンスで、アプリケーションがデプロイされた場合。</span><span class="sxs-lookup"><span data-stu-id="32783-376">Additionally, you learned how to scale out your application by using a *backplane* component when your application is deployed in multiple IIS instances.</span></span>
