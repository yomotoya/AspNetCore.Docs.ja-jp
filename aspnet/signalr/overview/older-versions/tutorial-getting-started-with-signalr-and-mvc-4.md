---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
title: 'チュートリアル: SignalR の概要 1.x と MVC 4 |Microsoft ドキュメント'
author: pfletcher
description: ASP.NET SignalR と ASP.NET MVC 4 を使用して、リアルタイムのチャット アプリケーションをビルドします。
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/29/2013
ms.topic: article
ms.assetid: eeef9f73-6de3-49f9-b50b-9af22108f2ce
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 1ae330be5caf00c3cac7451f326398c0958538af
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="tutorial-getting-started-with-signalr-1x-and-mvc-4"></a><span data-ttu-id="5a9f8-103">チュートリアル: SignalR の概要 1.x と MVC 4</span><span class="sxs-lookup"><span data-stu-id="5a9f8-103">Tutorial: Getting Started with SignalR 1.x and MVC 4</span></span>
====================
<span data-ttu-id="5a9f8-104">によって[Patrick Fletcher](https://github.com/pfletcher)、 [Tim Teebken](https://github.com/timlt)</span><span class="sxs-lookup"><span data-stu-id="5a9f8-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span></span>

> <span data-ttu-id="5a9f8-105">このチュートリアルでは、ASP.NET SignalR を使用して、リアルタイムのチャット アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-105">This tutorial shows how to use ASP.NET SignalR to create a real-time chat application.</span></span> <span data-ttu-id="5a9f8-106">SignalR の MVC 4 アプリケーションに追加し、送信、メッセージを表示するチャット ビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-106">You will add SignalR to an MVC 4 application and create a chat view to send and display messages.</span></span>


## <a name="overview"></a><span data-ttu-id="5a9f8-107">概要</span><span class="sxs-lookup"><span data-stu-id="5a9f8-107">Overview</span></span>

<span data-ttu-id="5a9f8-108">このチュートリアルでは、ASP.NET SignalR と ASP.NET MVC 4 のリアルタイムの web アプリケーションの開発に紹介します。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-108">This tutorial introduces you to real-time web application development with ASP.NET SignalR and ASP.NET MVC 4.</span></span> <span data-ttu-id="5a9f8-109">チュートリアル、チャット アプリケーションと同じコードを使用して、[チュートリアル SignalR が入門](tutorial-getting-started-with-signalr.md)、インターネット テンプレートに基づいて、MVC 4 アプリケーションに追加する方法を示していますが、します。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-109">The tutorial uses the same chat application code as the [SignalR Getting Started tutorial](tutorial-getting-started-with-signalr.md), but shows how to add it to an MVC 4 application based on the Internet template.</span></span>

<span data-ttu-id="5a9f8-110">このトピックでは、次の SignalR 開発タスクを学習します。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-110">In this topic you will learn the following SignalR development tasks:</span></span>

- <span data-ttu-id="5a9f8-111">SignalR ライブラリを MVC 4 アプリケーションに追加します。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-111">Adding the SignalR library to an MVC 4 application.</span></span>
- <span data-ttu-id="5a9f8-112">クライアントにコンテンツをプッシュするハブ クラスを作成しています。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-112">Creating a hub class to push content to clients.</span></span>
- <span data-ttu-id="5a9f8-113">Web ページでは、SignalR jQuery ライブラリを使用して、メッセージを送信し、ハブからの更新プログラムを表示します。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-113">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="5a9f8-114">次のスクリーン ショットは、ブラウザーで実行されている完了したチャット アプリケーションを示します。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-114">The following screen shot shows the completed chat application running in a browser.</span></span>

![チャット インスタンス](tutorial-getting-started-with-signalr-and-mvc-4/_static/image2.png)

<span data-ttu-id="5a9f8-116">セクション:</span><span class="sxs-lookup"><span data-stu-id="5a9f8-116">Sections:</span></span>

- [<span data-ttu-id="5a9f8-117">プロジェクトを設定します。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-117">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="5a9f8-118">このサンプルを実行します。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-118">Run the Sample</span></span>](#run)
- [<span data-ttu-id="5a9f8-119">コードを調べます</span><span class="sxs-lookup"><span data-stu-id="5a9f8-119">Examine the Code</span></span>](#code)
- [<span data-ttu-id="5a9f8-120">次の手順</span><span class="sxs-lookup"><span data-stu-id="5a9f8-120">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="5a9f8-121">プロジェクトを設定します。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-121">Set up the Project</span></span>

<span data-ttu-id="5a9f8-122">必要条件:</span><span class="sxs-lookup"><span data-stu-id="5a9f8-122">Prerequisites:</span></span>

- <span data-ttu-id="5a9f8-123">Visual Studio 2010 SP1、Visual Studio 2012、または Visual Studio 2012 Express です。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-123">Visual Studio 2010 SP1, Visual Studio 2012, or Visual Studio 2012 Express.</span></span> <span data-ttu-id="5a9f8-124">Visual Studio がない場合は、次を参照してください。 [ASP.NET ダウンロード](https://www.asp.net/downloads)空き Visual Studio 2012 Express 開発ツールを取得します。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-124">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2012 Express Development Tool.</span></span>
- <span data-ttu-id="5a9f8-125">Visual Studio 2010 をインストールする[ASP.NET MVC 4](https://www.microsoft.com/download/details.aspx?id=30683)です。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-125">For Visual Studio 2010, install [ASP.NET MVC 4](https://www.microsoft.com/download/details.aspx?id=30683).</span></span>

<span data-ttu-id="5a9f8-126">このセクションでは、ASP.NET MVC 4 アプリケーションを作成し、SignalR ライブラリを追加し、チャット アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-126">This section shows how to create an ASP.NET MVC 4 application, add the SignalR library, and create the chat application.</span></span>

1. 1. <span data-ttu-id="5a9f8-127">Visual Studio で ASP.NET MVC 4 アプリケーションを作成し、SignalRChat、という名前を [ok] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-127">In Visual Studio create an ASP.NET MVC 4 application, name it SignalRChat, and click OK.</span></span>

        > [!NOTE]
        > <span data-ttu-id="5a9f8-128">VS 2010 で選択 **.NET Framework 4**のフレームワークのバージョンのドロップダウン コントロールでします。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-128">In VS 2010, select **.NET Framework 4** in the Framework version dropdown control.</span></span> <span data-ttu-id="5a9f8-129">SignalR のコードは、.NET Framework version 4 および 4.5 で実行されます。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-129">SignalR code runs on .NET Framework versions 4 and 4.5.</span></span>

        ![Mvc web を作成します。](tutorial-getting-started-with-signalr-and-mvc-4/_static/image3.png)
      2. <span data-ttu-id="5a9f8-131">インターネット アプリケーション テンプレートを選択し、オプションをオフに**単体テスト プロジェクトを作成**、[ok] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-131">Select the Internet Application template, clear the option to **Create a unit test project**, and click OK.</span></span>

         ![Mvc のインターネット サイトを作成します。](tutorial-getting-started-with-signalr-and-mvc-4/_static/image4.png)
      3. <span data-ttu-id="5a9f8-133">開く、**ツール |ライブラリ パッケージ マネージャー |パッケージ マネージャー コンソール**し、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-133">Open the **Tools | Library Package Manager | Package Manager Console** and run the following command.</span></span> <span data-ttu-id="5a9f8-134">この手順では、一連のスクリプト ファイルと SignalR 機能を有効にするアセンブリ参照をプロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-134">This step adds to the project a set of script files and assembly references that enable SignalR functionality.</span></span>

         `install-package Microsoft.AspNet.SignalR -Version 1.1.3`
      4. <span data-ttu-id="5a9f8-135">**ソリューション エクスプ ローラー** Scripts フォルダーを展開します。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-135">In **Solution Explorer** expand the Scripts folder.</span></span> <span data-ttu-id="5a9f8-136">SignalR のスクリプト ライブラリがプロジェクトに追加されたことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-136">Note that script libraries for SignalR have been added to the project.</span></span>

         ![ライブラリの参照](tutorial-getting-started-with-signalr-and-mvc-4/_static/image6.png)
      5. <span data-ttu-id="5a9f8-138">**ソリューション エクスプ ローラー**、プロジェクトを右クリックし、選択**追加 |新しいフォルダー**、という名前の新しいフォルダーを追加および**ハブ**です。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-138">In **Solution Explorer**, right-click the project, select **Add | New Folder**, and add a new folder named **Hubs**.</span></span>
      6. <span data-ttu-id="5a9f8-139">右クリックし、**ハブ**フォルダーで、をクリックして**追加 |クラス**、新しい c# という名前のクラスを作成および**ChatHub.cs**です。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-139">Right-click the **Hubs** folder, click **Add | Class**, and create a new C# class named **ChatHub.cs**.</span></span> <span data-ttu-id="5a9f8-140">このクラスは、すべてのクライアントにメッセージを送信する SignalR サーバー ハブとして使用されます。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-140">You will use this class as a SignalR server hub that sends messages to all clients.</span></span>

> [!NOTE]
> <span data-ttu-id="5a9f8-141">Visual Studio 2012 を使用してインストールした場合、 [ASP.NET および Web ツール 2012.2 更新](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation)、ハブ クラスを作成する新しい SignalR 項目テンプレートを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-141">If you use Visual Studio 2012 and have installed the [ASP.NET and Web Tools 2012.2 update](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation), you can use the new SignalR item template to create the hub class.</span></span> <span data-ttu-id="5a9f8-142">右クリックし、**ハブ**フォルダーで、をクリックして**追加 |新しい項目の** **SignalR ハブ クラス (v1)**、クラスの名前と**ChatHub.cs**です。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-142">To do that, right-click the **Hubs** folder, click **Add | New Item**, select **SignalR Hub Class (v1)**, and name the class **ChatHub.cs**.</span></span>


1. <span data-ttu-id="5a9f8-143">コードで置き換え、 **ChatHub**クラスを次のコード。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-143">Replace the code in the **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample1.cs)]
2. <span data-ttu-id="5a9f8-144">開く、 **Global.asax** 、プロジェクトのファイルし、メソッドの呼び出しを追加`RouteTable.Routes.MapHubs();`内のコードの最初の行として、`Application_Start`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-144">Open the **Global.asax** file for the project, and add a call to the method `RouteTable.Routes.MapHubs();` as the first line of code in the `Application_Start` method.</span></span> <span data-ttu-id="5a9f8-145">このコードは、SignalR ハブの既定のルートを登録し、他のルートを登録する前に呼び出す必要があります。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-145">This code registers the default route for SignalR hubs and must be called before you register any other routes.</span></span> <span data-ttu-id="5a9f8-146">完成した`Application_Start`メソッドは次の例のようになります。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-146">The completed `Application_Start` method looks like the following example.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample2.cs)]
3. <span data-ttu-id="5a9f8-147">編集、`HomeController`クラス**Controllers/HomeController.cs**クラスに次のメソッドを追加するとします。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-147">Edit the `HomeController` class found in **Controllers/HomeController.cs** and add the following method to the class.</span></span> <span data-ttu-id="5a9f8-148">このメソッドが戻る、**チャット**後の手順で作成するビュー。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-148">This method returns the **Chat** view that you will create in a later step.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample3.cs)]
4. <span data-ttu-id="5a9f8-149">内で右クリックし、`Chat`メソッドを作成してをクリックして**ビューの追加**を新しいファイルの表示を作成します。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-149">Right-click within the `Chat` method you just created, and click **Add View** to create a new view file.</span></span>
5. <span data-ttu-id="5a9f8-150">**ビューの追加**ダイアログ ボックスで、チェック ボックスが選択されていることを確認してください**レイアウトまたはマスター ページを使用して**(その他のチェック ボックスをオフ)、をクリックして**追加**です。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-150">In the **Add View** dialog, make sure the check box is selected to **Use a layout or master page** (clear the other check boxes), and then click **Add**.</span></span>

    ![ビューを追加する](tutorial-getting-started-with-signalr-and-mvc-4/_static/image8.png)
6. <span data-ttu-id="5a9f8-152">という新しいビュー ファイルを編集**Chat.cshtml**です。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-152">Edit the new view file named **Chat.cshtml**.</span></span> <span data-ttu-id="5a9f8-153">後に、 &lt;h2&gt;タグで、以下を貼り付けます&lt;div&gt;セクションと`@section scripts`コード ブロックをページにします。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-153">After the &lt;h2&gt; tag, paste the following &lt;div&gt; section and `@section scripts` code block into the page.</span></span> <span data-ttu-id="5a9f8-154">このスクリプトは、チャット メッセージを送信し、サーバーからメッセージを表示するページを有効にします。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-154">This script enables the page to send chat messages and display messages from the server.</span></span> <span data-ttu-id="5a9f8-155">次のコード ブロックにチャット ビューの完全なコードが表示されます。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-155">The complete code for the chat view appears in the following code block.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="5a9f8-156">SignalR と他のスクリプト ライブラリを Visual Studio プロジェクトに追加するときにパッケージ マネージャーはこのトピックに示すバージョンより新しくなっているスクリプトのバージョンをインストール可能性があります。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-156">When you add SignalR and other script libraries to your Visual Studio project, the Package Manager might install versions of the scripts that are more recent than the versions shown in this topic.</span></span> <span data-ttu-id="5a9f8-157">コード内のスクリプト参照がプロジェクトにインストールされているスクリプト ライブラリのバージョンを一致することを確認してください。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-157">Make sure that the script references in your code match the versions of the script libraries installed in your project.</span></span>

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample4.cshtml)]
7. <span data-ttu-id="5a9f8-158">**Save All**プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-158">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="5a9f8-159">このサンプルを実行します。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-159">Run the Sample</span></span>

1. <span data-ttu-id="5a9f8-160">F5 キーを押して、プロジェクトをデバッグ モードで実行します。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-160">Press F5 to run the project in debug mode.</span></span>
2. <span data-ttu-id="5a9f8-161">ブラウザーのアドレスの行で追加**ホーム/チャット**プロジェクトの既定のページの URL にします。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-161">In the browser address line, append **/home/chat** to the URL of the default page for the project.</span></span> <span data-ttu-id="5a9f8-162">ブラウザーのインスタンスと、ユーザー名のプロンプトで、チャット ページが読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-162">The Chat page loads in a browser instance and prompts for a user name.</span></span>

    ![ユーザー名の入力](tutorial-getting-started-with-signalr-and-mvc-4/_static/image9.png)
3. <span data-ttu-id="5a9f8-164">ユーザー名を入力します。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-164">Enter a user name.</span></span>
4. <span data-ttu-id="5a9f8-165">ブラウザーのアドレスの行から URL をコピーし、2 つ以上のブラウザー インスタンスを開くために使用します。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-165">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="5a9f8-166">各ブラウザー インスタンスでは、一意のユーザー名を入力します。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-166">In each browser instance, enter a unique user name.</span></span>
5. <span data-ttu-id="5a9f8-167">各ブラウザー インスタンスでコメントを追加し、をクリックして**送信**です。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-167">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="5a9f8-168">コメントは、すべてのブラウザー インスタンスで表示されます。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-168">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5a9f8-169">この単純なチャット アプリケーションでは、サーバー上のディスカッション コンテキストは保持されません。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-169">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="5a9f8-170">ハブは、現在のすべてのユーザーにコメントをブロードキャストします。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-170">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="5a9f8-171">後で、チャットに参加するユーザーは、参加時から追加されたメッセージに表示されます。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-171">Users who join the chat later will see messages added from the time they join.</span></span>
6. <span data-ttu-id="5a9f8-172">次のスクリーン ショットは、ブラウザーで実行されているチャット アプリケーションを示します。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-172">The following screen shot shows the chat application running in a browser.</span></span>

    ![チャット ブラウザー](tutorial-getting-started-with-signalr-and-mvc-4/_static/image11.png)
7. <span data-ttu-id="5a9f8-174">**ソリューション エクスプ ローラー**、検査、**スクリプト ドキュメント**の実行中のアプリケーション ノード。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-174">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="5a9f8-175">お使いのブラウザーとして Internet Explorer を使用している場合、このノードはデバッグ モードに表示されます。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-175">This node is visible in debug mode if you are using Internet Explorer as your browser.</span></span> <span data-ttu-id="5a9f8-176">という名前のスクリプト ファイルがある**ハブ**SignalR ライブラリは、実行時に動的に生成します。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-176">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="5a9f8-177">このファイルは、jQuery スクリプトとサーバー側コード間の通信を管理します。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-177">This file manages the communication between jQuery script and server-side code.</span></span> <span data-ttu-id="5a9f8-178">Internet Explorer ではないブラウザーを使用する場合は、アクセスすることも、動的**ハブ**ファイルを参照して、直接、たとえばhttp://mywebsite/signalr/hubsします。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-178">If you use a browser other than Internet Explorer, you can also access the dynamic **hubs** file by browsing to it directly, for example http://mywebsite/signalr/hubs.</span></span>

    ![生成されたハブのスクリプト](tutorial-getting-started-with-signalr-and-mvc-4/_static/image13.png)

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="5a9f8-180">コードを調べます</span><span class="sxs-lookup"><span data-stu-id="5a9f8-180">Examine the Code</span></span>

<span data-ttu-id="5a9f8-181">SignalR チャット アプリケーションを 2 つの基本的な SignalR 開発タスクを示しています。 サーバーで、メインの調整オブジェクトとしてハブを作成すると、SignalR jQuery ライブラリを使用してメッセージを送信および受信します。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-181">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="5a9f8-182">SignalR ハブ</span><span class="sxs-lookup"><span data-stu-id="5a9f8-182">SignalR Hubs</span></span>

<span data-ttu-id="5a9f8-183">コード サンプルでは、 **ChatHub**から派生したクラス、 **Microsoft.AspNet.SignalR.Hub**クラスです。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-183">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="5a9f8-184">派生する、**ハブ**クラスは、SignalR アプリケーションをビルドする便利な方法です。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-184">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="5a9f8-185">ハブ クラスにパブリック メソッドを作成し、web ページの jQuery スクリプトから呼び出すことによってこれらのメソッドにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-185">You can create public methods on your hub class and then access those methods by calling them from jQuery scripts in a web page.</span></span>

<span data-ttu-id="5a9f8-186">クライアントが呼び出すチャットのコードで、 **ChatHub.Send**メソッドを新しいメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-186">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="5a9f8-187">ハブに送信メッセージのすべてのクライアントを呼び出して**Clients.All.addNewMessageToPage**です。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-187">The hub in turn sends the message to all clients by calling **Clients.All.addNewMessageToPage**.</span></span>

<span data-ttu-id="5a9f8-188">**送信**メソッドは、ハブの概念をいくつかを示します。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-188">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="5a9f8-189">ハブのパブリック メソッドを宣言のクライアントが呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-189">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="5a9f8-190">使用して、 **Microsoft.AspNet.SignalR.Hub.Clients**のすべてのクライアントにアクセスするプロパティがこのハブに接続します。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-190">Use the **Microsoft.AspNet.SignalR.Hub.Clients** property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="5a9f8-191">クライアントで jQuery 関数を呼び出す (など、`addNewMessageToPage`関数) クライアントを更新します。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-191">Call a jQuery function on the client (such as the `addNewMessageToPage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="5a9f8-192">SignalR と jQuery</span><span class="sxs-lookup"><span data-stu-id="5a9f8-192">SignalR and jQuery</span></span>

<span data-ttu-id="5a9f8-193">**Chat.cshtml**コード サンプルではファイルの表示は、SignalR jQuery ライブラリを使用して SignalR ハブと通信する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-193">The **Chat.cshtml** view file in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="5a9f8-194">コード内の必須のタスクには、プロキシへの参照、自動生成されたハブのサーバーがクライアントにプッシュ コンテンツを呼び出すことができる関数を宣言して、接続を開始、ハブにメッセージを送信が作成されます。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-194">The essential tasks in the code are creating a reference to the auto-generated proxy for the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="5a9f8-195">次のコードでは、ハブのプロキシを宣言します。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-195">The following code declares a proxy for a hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="5a9f8-196">JQuery では、サーバー クラスとそのメンバーへの参照は、camel 形式でです。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-196">In jQuery the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="5a9f8-197">コード サンプルを参照して、c# **ChatHub**クラスとして jQuery の**chatHub**です。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-197">The code sample references the C# **ChatHub** class in jQuery as **chatHub**.</span></span> <span data-ttu-id="5a9f8-198">参照する場合、`ChatHub`クラス従来 pascal 形式での jQuery の場合と、C# の場合は、大文字小文字の区別、ChatHub.cs クラス ファイルを編集します。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-198">If you want to reference the `ChatHub` class in jQuery with conventional Pascal casing as you would in C#, edit the ChatHub.cs class file.</span></span> <span data-ttu-id="5a9f8-199">追加、`using`を参照するステートメント、`Microsoft.AspNet.SignalR.Hubs`名前空間。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-199">Add a `using` statement to reference the `Microsoft.AspNet.SignalR.Hubs` namespace.</span></span> <span data-ttu-id="5a9f8-200">追加し、`HubName`属性を`ChatHub`クラス、たとえば`[HubName("ChatHub")]`します。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-200">Then add the `HubName` attribute to the `ChatHub` class, for example `[HubName("ChatHub")]`.</span></span> <span data-ttu-id="5a9f8-201">参照を jQuery を最後に、更新、`ChatHub`クラスです。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-201">Finally, update your jQuery reference to the `ChatHub` class.</span></span>


<span data-ttu-id="5a9f8-202">次のコードでは、スクリプトにコールバック関数を作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-202">The following code shows how to create a callback function in the script.</span></span> <span data-ttu-id="5a9f8-203">サーバーの hub クラスは、各クライアントにコンテンツの更新プログラムをプッシュするには、この関数を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-203">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="5a9f8-204">省略可能な呼び出し、`htmlEncode`関数の表示方法を HTML には、スクリプト インジェクションを防止する方法として、ページに表示する前に、メッセージの内容をエンコードします。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-204">The optional call to the `htmlEncode` function shows a way to HTML encode the message content before displaying it in the page, as a way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample7.html)]

<span data-ttu-id="5a9f8-205">次のコードでは、ハブとの接続を開く方法を示します。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-205">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="5a9f8-206">コードの接続を開始しますおよびの click イベントを処理する関数を渡します、**送信**チャット ページでボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-206">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the Chat page.</span></span>

> [!NOTE]
> <span data-ttu-id="5a9f8-207">これにより、イベント ハンドラーが実行される前に、接続が確立されています。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-207">This approach ensures that the connection is established before the event handler executes.</span></span>


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="5a9f8-208">次の手順</span><span class="sxs-lookup"><span data-stu-id="5a9f8-208">Next Steps</span></span>

<span data-ttu-id="5a9f8-209">SignalR には、リアルタイムの web アプリケーションを構築するためのフレームワークを学習しました。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-209">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="5a9f8-210">いくつかの SignalR 開発タスクについても学びました。 SignalR を ASP.NET アプリケーションに追加する方法、ハブ クラスを作成する方法、およびハブからメッセージを送受信する方法です。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-210">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="5a9f8-211">高度な SignalR 開発の概念については、SignalR のソース コードおよびリソースの次のサイトを参照してください。</span><span class="sxs-lookup"><span data-stu-id="5a9f8-211">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources :</span></span>

- [<span data-ttu-id="5a9f8-212">SignalR Project</span><span class="sxs-lookup"><span data-stu-id="5a9f8-212">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="5a9f8-213">SignalR Github とサンプル</span><span class="sxs-lookup"><span data-stu-id="5a9f8-213">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="5a9f8-214">SignalR Wiki</span><span class="sxs-lookup"><span data-stu-id="5a9f8-214">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
