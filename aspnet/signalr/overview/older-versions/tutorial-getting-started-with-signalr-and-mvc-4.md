---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
title: チュートリアル:SignalR の概要 1.x と MVC 4 |Microsoft Docs
author: pfletcher
description: ASP.NET SignalR、ASP.NET MVC 4 を使用して、リアルタイムのチャット アプリケーションを構築します。
ms.author: riande
ms.date: 03/29/2013
ms.assetid: eeef9f73-6de3-49f9-b50b-9af22108f2ce
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: a67d05288252c17d84b1d7df5f7bcddde3c887f5
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287724"
---
<a name="tutorial-getting-started-with-signalr-1x-and-mvc-4"></a><span data-ttu-id="5858e-103">チュートリアル:SignalR の概要 1.x と MVC 4</span><span class="sxs-lookup"><span data-stu-id="5858e-103">Tutorial: Getting Started with SignalR 1.x and MVC 4</span></span>
====================
<span data-ttu-id="5858e-104">によって[Patrick Fletcher](https://github.com/pfletcher)、 [Tim Teebken](https://github.com/timlt)</span><span class="sxs-lookup"><span data-stu-id="5858e-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="5858e-105">このチュートリアルでは、ASP.NET SignalR を使用して、リアルタイムのチャット アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="5858e-105">This tutorial shows how to use ASP.NET SignalR to create a real-time chat application.</span></span> <span data-ttu-id="5858e-106">SignalR を MVC 4 アプリケーションに追加し、チャットを送信し、メッセージを表示するビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="5858e-106">You will add SignalR to an MVC 4 application and create a chat view to send and display messages.</span></span>


## <a name="overview"></a><span data-ttu-id="5858e-107">概要</span><span class="sxs-lookup"><span data-stu-id="5858e-107">Overview</span></span>

<span data-ttu-id="5858e-108">このチュートリアルでは、ASP.NET SignalR、ASP.NET MVC 4 とリアルタイムの web アプリケーションの開発について説明します。</span><span class="sxs-lookup"><span data-stu-id="5858e-108">This tutorial introduces you to real-time web application development with ASP.NET SignalR and ASP.NET MVC 4.</span></span> <span data-ttu-id="5858e-109">チュートリアルと同じのチャット アプリケーションのコードを使用して、 [SignalR 作業開始のチュートリアル](tutorial-getting-started-with-signalr.md)が、インターネットのテンプレートに基づく、MVC 4 アプリケーションに追加する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="5858e-109">The tutorial uses the same chat application code as the [SignalR Getting Started tutorial](tutorial-getting-started-with-signalr.md), but shows how to add it to an MVC 4 application based on the Internet template.</span></span>

<span data-ttu-id="5858e-110">このトピックでは、次の SignalR 開発タスクを学習します。</span><span class="sxs-lookup"><span data-stu-id="5858e-110">In this topic you will learn the following SignalR development tasks:</span></span>

- <span data-ttu-id="5858e-111">MVC 4 アプリケーション、SignalR ライブラリに追加します。</span><span class="sxs-lookup"><span data-stu-id="5858e-111">Adding the SignalR library to an MVC 4 application.</span></span>
- <span data-ttu-id="5858e-112">クライアントにコンテンツをプッシュするハブ クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="5858e-112">Creating a hub class to push content to clients.</span></span>
- <span data-ttu-id="5858e-113">Web ページで、SignalR jQuery ライブラリを使用して、メッセージを送信し、ハブから更新プログラムを表示します。</span><span class="sxs-lookup"><span data-stu-id="5858e-113">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="5858e-114">次のスクリーン ショットは、ブラウザーで実行されている完成したチャット アプリケーションを示しています。</span><span class="sxs-lookup"><span data-stu-id="5858e-114">The following screen shot shows the completed chat application running in a browser.</span></span>

![チャット インスタンス](tutorial-getting-started-with-signalr-and-mvc-4/_static/image2.png)

<span data-ttu-id="5858e-116">セクション:</span><span class="sxs-lookup"><span data-stu-id="5858e-116">Sections:</span></span>

- [<span data-ttu-id="5858e-117">プロジェクトを設定します。</span><span class="sxs-lookup"><span data-stu-id="5858e-117">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="5858e-118">サンプルを実行します。</span><span class="sxs-lookup"><span data-stu-id="5858e-118">Run the Sample</span></span>](#run)
- [<span data-ttu-id="5858e-119">コードを確認します</span><span class="sxs-lookup"><span data-stu-id="5858e-119">Examine the Code</span></span>](#code)
- [<span data-ttu-id="5858e-120">次の手順</span><span class="sxs-lookup"><span data-stu-id="5858e-120">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="5858e-121">プロジェクトを設定します。</span><span class="sxs-lookup"><span data-stu-id="5858e-121">Set up the Project</span></span>

<span data-ttu-id="5858e-122">必要条件:</span><span class="sxs-lookup"><span data-stu-id="5858e-122">Prerequisites:</span></span>

- <span data-ttu-id="5858e-123">Visual Studio 2010 SP1、Visual Studio 2012、または Visual Studio 2012 Express。</span><span class="sxs-lookup"><span data-stu-id="5858e-123">Visual Studio 2010 SP1, Visual Studio 2012, or Visual Studio 2012 Express.</span></span> <span data-ttu-id="5858e-124">Visual Studio がないを参照してください。 [ASP.NET ダウンロード](https://www.asp.net/downloads)無料 Visual Studio 2012 Express 開発ツールを取得します。</span><span class="sxs-lookup"><span data-stu-id="5858e-124">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2012 Express Development Tool.</span></span>
- <span data-ttu-id="5858e-125">Visual Studio 2010 では、インストール[ASP.NET MVC 4](https://www.microsoft.com/download/details.aspx?id=30683)します。</span><span class="sxs-lookup"><span data-stu-id="5858e-125">For Visual Studio 2010, install [ASP.NET MVC 4](https://www.microsoft.com/download/details.aspx?id=30683).</span></span>

<span data-ttu-id="5858e-126">このセクションでは、ASP.NET MVC 4 アプリケーションを作成し、SignalR ライブラリの追加、チャット アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="5858e-126">This section shows how to create an ASP.NET MVC 4 application, add the SignalR library, and create the chat application.</span></span>

1. 1. <span data-ttu-id="5858e-127">Visual Studio で ASP.NET MVC 4 アプリケーションを作成し、SignalRChat、という名前を付けます [ok] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5858e-127">In Visual Studio create an ASP.NET MVC 4 application, name it SignalRChat, and click OK.</span></span>

        > [!NOTE]
        > <span data-ttu-id="5858e-128">VS 2010 では、次のように選択します。 **.NET Framework 4** Framework のバージョンのドロップダウン コントロール。</span><span class="sxs-lookup"><span data-stu-id="5858e-128">In VS 2010, select **.NET Framework 4** in the Framework version dropdown control.</span></span> <span data-ttu-id="5858e-129">SignalR のコードは、.NET Framework version 4 および 4.5 で実行されます。</span><span class="sxs-lookup"><span data-stu-id="5858e-129">SignalR code runs on .NET Framework versions 4 and 4.5.</span></span>

        ![Mvc の web を作成します。](tutorial-getting-started-with-signalr-and-mvc-4/_static/image3.png)
      2. <span data-ttu-id="5858e-131">インターネット アプリケーション テンプレートを選択して、オプションをオフに**単体テスト プロジェクトを作成**、[ok] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5858e-131">Select the Internet Application template, clear the option to **Create a unit test project**, and click OK.</span></span>

         ![Mvc のインターネット サイトを作成します。](tutorial-getting-started-with-signalr-and-mvc-4/_static/image4.png)
      3. <span data-ttu-id="5858e-133">開く、**ツール > NuGet パッケージ マネージャー > パッケージ マネージャー コンソール**し、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="5858e-133">Open the **Tools > NuGet Package Manager > Package Manager Console** and run the following command.</span></span> <span data-ttu-id="5858e-134">この手順では、一連のスクリプト ファイルと SignalR の機能を有効にするアセンブリ参照をプロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="5858e-134">This step adds to the project a set of script files and assembly references that enable SignalR functionality.</span></span>

         `install-package Microsoft.AspNet.SignalR -Version 1.1.3`
      4. <span data-ttu-id="5858e-135">**ソリューション エクスプ ローラー** Scripts フォルダーを展開します。</span><span class="sxs-lookup"><span data-stu-id="5858e-135">In **Solution Explorer** expand the Scripts folder.</span></span> <span data-ttu-id="5858e-136">SignalR のスクリプト ライブラリがプロジェクトに追加されていることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="5858e-136">Note that script libraries for SignalR have been added to the project.</span></span>

         ![ライブラリの参照](tutorial-getting-started-with-signalr-and-mvc-4/_static/image6.png)
      5. <span data-ttu-id="5858e-138">**ソリューション エクスプ ローラー**、プロジェクトを右クリックし、選択**追加 |新しいフォルダー**、という名前の新しいフォルダーを追加および**Hubs**します。</span><span class="sxs-lookup"><span data-stu-id="5858e-138">In **Solution Explorer**, right-click the project, select **Add | New Folder**, and add a new folder named **Hubs**.</span></span>
      6. <span data-ttu-id="5858e-139">右クリックし、 **Hubs**フォルダー、をクリックして**追加 |クラス**、新しい c# という名前のクラスを作成および**ChatHub.cs**します。</span><span class="sxs-lookup"><span data-stu-id="5858e-139">Right-click the **Hubs** folder, click **Add | Class**, and create a new C# class named **ChatHub.cs**.</span></span> <span data-ttu-id="5858e-140">このクラスは、すべてのクライアントにメッセージを送信する SignalR サーバー ハブとして使用されます。</span><span class="sxs-lookup"><span data-stu-id="5858e-140">You will use this class as a SignalR server hub that sends messages to all clients.</span></span>

> [!NOTE]
> <span data-ttu-id="5858e-141">Visual Studio 2012 を使用して、インストールされている場合、 [ASP.NET and Web Tools 2012.2 update](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation)、ハブ クラスを作成する新しい SignalR 項目テンプレートを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="5858e-141">If you use Visual Studio 2012 and have installed the [ASP.NET and Web Tools 2012.2 update](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation), you can use the new SignalR item template to create the hub class.</span></span> <span data-ttu-id="5858e-142">右クリックし、 **Hubs**フォルダー、をクリックして**追加 |新しい項目の**、 **SignalR ハブ クラス (v1)**、クラスの名前と**ChatHub.cs**します。</span><span class="sxs-lookup"><span data-stu-id="5858e-142">To do that, right-click the **Hubs** folder, click **Add | New Item**, select **SignalR Hub Class (v1)**, and name the class **ChatHub.cs**.</span></span>


1. <span data-ttu-id="5858e-143">コードに置き換えます、 **ChatHub**クラスを次のコード。</span><span class="sxs-lookup"><span data-stu-id="5858e-143">Replace the code in the **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample1.cs)]
2. <span data-ttu-id="5858e-144">開く、 **Global.asax** 、プロジェクトのファイルを開き、メソッドの呼び出しを追加`RouteTable.Routes.MapHubs();`内のコードの最初の行として、`Application_Start`メソッド。</span><span class="sxs-lookup"><span data-stu-id="5858e-144">Open the **Global.asax** file for the project, and add a call to the method `RouteTable.Routes.MapHubs();` as the first line of code in the `Application_Start` method.</span></span> <span data-ttu-id="5858e-145">このコードでは、SignalR ハブの既定のルートを登録し、他のルートを登録する前に呼び出す必要があります。</span><span class="sxs-lookup"><span data-stu-id="5858e-145">This code registers the default route for SignalR hubs and must be called before you register any other routes.</span></span> <span data-ttu-id="5858e-146">完成した`Application_Start`メソッドは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="5858e-146">The completed `Application_Start` method looks like the following example.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample2.cs)]
3. <span data-ttu-id="5858e-147">編集、`HomeController`クラス**Controllers/HomeController.cs**クラスに次のメソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="5858e-147">Edit the `HomeController` class found in **Controllers/HomeController.cs** and add the following method to the class.</span></span> <span data-ttu-id="5858e-148">このメソッドが戻る、**チャット**後の手順で作成するビュー。</span><span class="sxs-lookup"><span data-stu-id="5858e-148">This method returns the **Chat** view that you will create in a later step.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample3.cs)]
4. <span data-ttu-id="5858e-149">内で右クリック、`Chat`メソッドだけに作成されるとクリックして**ビューの追加**ビューの新しいファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="5858e-149">Right-click within the `Chat` method you just created, and click **Add View** to create a new view file.</span></span>
5. <span data-ttu-id="5858e-150">**ビューの追加**ダイアログ ボックスで、チェック ボックスが選択されていることを確認します**レイアウトまたはマスター ページを使用して**(他のチェック ボックスをオフ)、順にクリックします**追加**します。</span><span class="sxs-lookup"><span data-stu-id="5858e-150">In the **Add View** dialog, make sure the check box is selected to **Use a layout or master page** (clear the other check boxes), and then click **Add**.</span></span>

    ![ビューを追加する](tutorial-getting-started-with-signalr-and-mvc-4/_static/image8.png)
6. <span data-ttu-id="5858e-152">という名前の新しいビュー ファイルを編集**Chat.cshtml**します。</span><span class="sxs-lookup"><span data-stu-id="5858e-152">Edit the new view file named **Chat.cshtml**.</span></span> <span data-ttu-id="5858e-153">後に、 &lt;h2&gt;タグに、次を貼り付けます&lt;div&gt;セクションと`@section scripts`ページにコード ブロックです。</span><span class="sxs-lookup"><span data-stu-id="5858e-153">After the &lt;h2&gt; tag, paste the following &lt;div&gt; section and `@section scripts` code block into the page.</span></span> <span data-ttu-id="5858e-154">このスクリプトでは、チャット メッセージを送信し、サーバーからメッセージを表示するページが有効にします。</span><span class="sxs-lookup"><span data-stu-id="5858e-154">This script enables the page to send chat messages and display messages from the server.</span></span> <span data-ttu-id="5858e-155">チャット ビューの完全なコードは、次のコード ブロックに表示されます。</span><span class="sxs-lookup"><span data-stu-id="5858e-155">The complete code for the chat view appears in the following code block.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="5858e-156">SignalR およびその他のスクリプト ライブラリを Visual Studio プロジェクトに追加するとパッケージ マネージャー可能性がありますが、このトピックで示されているバージョンよりも新しいスクリプトのバージョンをインストールします。</span><span class="sxs-lookup"><span data-stu-id="5858e-156">When you add SignalR and other script libraries to your Visual Studio project, the Package Manager might install versions of the scripts that are more recent than the versions shown in this topic.</span></span> <span data-ttu-id="5858e-157">コード内のスクリプト参照がプロジェクトにインストールされているスクリプト ライブラリのバージョンと一致することを確認します。</span><span class="sxs-lookup"><span data-stu-id="5858e-157">Make sure that the script references in your code match the versions of the script libraries installed in your project.</span></span>

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample4.cshtml)]
7. <span data-ttu-id="5858e-158">**すべて保存**プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="5858e-158">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="5858e-159">サンプルを実行します。</span><span class="sxs-lookup"><span data-stu-id="5858e-159">Run the Sample</span></span>

1. <span data-ttu-id="5858e-160">F5 キーを押して、デバッグ モードで、プロジェクトを実行します。</span><span class="sxs-lookup"><span data-stu-id="5858e-160">Press F5 to run the project in debug mode.</span></span>
2. <span data-ttu-id="5858e-161">ブラウザーのアドレスの行の追加 **/ホーム/チャット**プロジェクトの既定のページの URL にします。</span><span class="sxs-lookup"><span data-stu-id="5858e-161">In the browser address line, append **/home/chat** to the URL of the default page for the project.</span></span> <span data-ttu-id="5858e-162">ブラウザーのインスタンスとユーザー名のプロンプトで、チャット ページが読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="5858e-162">The Chat page loads in a browser instance and prompts for a user name.</span></span>

    ![ユーザー名の入力](tutorial-getting-started-with-signalr-and-mvc-4/_static/image9.png)
3. <span data-ttu-id="5858e-164">ユーザー名を入力します。</span><span class="sxs-lookup"><span data-stu-id="5858e-164">Enter a user name.</span></span>
4. <span data-ttu-id="5858e-165">ブラウザーのアドレスの行から URL をコピーし、2 つのブラウザー インスタンスを開くために使用します。</span><span class="sxs-lookup"><span data-stu-id="5858e-165">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="5858e-166">各ブラウザー インスタンスでは、一意のユーザー名を入力します。</span><span class="sxs-lookup"><span data-stu-id="5858e-166">In each browser instance, enter a unique user name.</span></span>
5. <span data-ttu-id="5858e-167">各ブラウザー インスタンスでコメントを追加し、をクリックして**送信**します。</span><span class="sxs-lookup"><span data-stu-id="5858e-167">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="5858e-168">すべてのブラウザー インスタンスで、コメントが表示されます。</span><span class="sxs-lookup"><span data-stu-id="5858e-168">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5858e-169">この簡単なチャット アプリケーションでは、サーバー上のディスカッション コンテキストは保持されません。</span><span class="sxs-lookup"><span data-stu-id="5858e-169">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="5858e-170">ハブは、現在のすべてのユーザーへのコメントをブロードキャストします。</span><span class="sxs-lookup"><span data-stu-id="5858e-170">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="5858e-171">後で、チャットに参加するユーザーは、参加する時点から追加されたメッセージに表示されます。</span><span class="sxs-lookup"><span data-stu-id="5858e-171">Users who join the chat later will see messages added from the time they join.</span></span>
6. <span data-ttu-id="5858e-172">次のスクリーン ショットは、ブラウザーで実行されているチャット アプリケーションを示しています。</span><span class="sxs-lookup"><span data-stu-id="5858e-172">The following screen shot shows the chat application running in a browser.</span></span>

    ![チャット ブラウザー](tutorial-getting-started-with-signalr-and-mvc-4/_static/image11.png)
7. <span data-ttu-id="5858e-174">**ソリューション エクスプ ローラー**、検査、**スクリプト ドキュメント**実行中のアプリケーションのノード。</span><span class="sxs-lookup"><span data-stu-id="5858e-174">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="5858e-175">お使いのブラウザーとして Internet Explorer を使用している場合、このノードはデバッグ モードに表示されます。</span><span class="sxs-lookup"><span data-stu-id="5858e-175">This node is visible in debug mode if you are using Internet Explorer as your browser.</span></span> <span data-ttu-id="5858e-176">という名前のスクリプト ファイルがある**hubs** SignalR ライブラリは、実行時に動的に生成します。</span><span class="sxs-lookup"><span data-stu-id="5858e-176">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="5858e-177">このファイルは、jQuery スクリプトとサーバー側コード間の通信を管理します。</span><span class="sxs-lookup"><span data-stu-id="5858e-177">This file manages the communication between jQuery script and server-side code.</span></span> <span data-ttu-id="5858e-178">Internet Explorer 以外のブラウザーを使用する場合は、動的もアクセスできる**hubs**ファイルを参照して直接、たとえば http://mywebsite/signalr/hubsします。</span><span class="sxs-lookup"><span data-stu-id="5858e-178">If you use a browser other than Internet Explorer, you can also access the dynamic **hubs** file by browsing to it directly, for example http://mywebsite/signalr/hubs.</span></span>

    ![生成されたハブのスクリプト](tutorial-getting-started-with-signalr-and-mvc-4/_static/image13.png)

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="5858e-180">コードを確認します</span><span class="sxs-lookup"><span data-stu-id="5858e-180">Examine the Code</span></span>

<span data-ttu-id="5858e-181">SignalR チャット アプリケーションを 2 つの基本的な SignalR 開発タスクを示します。 サーバーで、主要な調整オブジェクトとしてハブを作成し、メッセージを送受信する SignalR jQuery ライブラリを使用します。</span><span class="sxs-lookup"><span data-stu-id="5858e-181">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="5858e-182">SignalR ハブ</span><span class="sxs-lookup"><span data-stu-id="5858e-182">SignalR Hubs</span></span>

<span data-ttu-id="5858e-183">コード サンプルでは、 **ChatHub**クラスから派生、 **Microsoft.AspNet.SignalR.Hub**クラス。</span><span class="sxs-lookup"><span data-stu-id="5858e-183">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="5858e-184">派生する、**ハブ**クラスは、SignalR アプリケーションを構築する便利な方法です。</span><span class="sxs-lookup"><span data-stu-id="5858e-184">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="5858e-185">ハブ クラスにパブリック メソッドを作成し、web ページに jQuery スクリプトから呼び出すことによってこれらのメソッドにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="5858e-185">You can create public methods on your hub class and then access those methods by calling them from jQuery scripts in a web page.</span></span>

<span data-ttu-id="5858e-186">クライアントが呼び出すチャットのコードで、 **ChatHub.Send**メソッドを新しいメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="5858e-186">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="5858e-187">ハブに送信メッセージのすべてのクライアントを呼び出して**Clients.All.addNewMessageToPage**します。</span><span class="sxs-lookup"><span data-stu-id="5858e-187">The hub in turn sends the message to all clients by calling **Clients.All.addNewMessageToPage**.</span></span>

<span data-ttu-id="5858e-188">**送信**メソッドがいくつかのハブの概念を示します。</span><span class="sxs-lookup"><span data-stu-id="5858e-188">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="5858e-189">クライアントが呼び出すことができるように、ハブ上のパブリック メソッドを宣言します。</span><span class="sxs-lookup"><span data-stu-id="5858e-189">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="5858e-190">使用して、 **Microsoft.AspNet.SignalR.Hub.Clients**この hub に接続されているすべてのクライアントにアクセスするプロパティ。</span><span class="sxs-lookup"><span data-stu-id="5858e-190">Use the **Microsoft.AspNet.SignalR.Hub.Clients** property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="5858e-191">クライアントの jQuery 関数を呼び出す (など、`addNewMessageToPage`関数) クライアントを更新します。</span><span class="sxs-lookup"><span data-stu-id="5858e-191">Call a jQuery function on the client (such as the `addNewMessageToPage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="5858e-192">SignalR と jQuery</span><span class="sxs-lookup"><span data-stu-id="5858e-192">SignalR and jQuery</span></span>

<span data-ttu-id="5858e-193">**Chat.cshtml**ビュー ファイルのコード サンプルでは、SignalR jQuery ライブラリを使用して、SignalR hub と通信する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="5858e-193">The **Chat.cshtml** view file in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="5858e-194">コードで基本的なタスクでは、自動生成されたプロキシをハブをクライアントにコンテンツをプッシュするサーバーを呼び出すことができる関数を宣言して、接続を開始、ハブにメッセージを送信への参照を作成します。</span><span class="sxs-lookup"><span data-stu-id="5858e-194">The essential tasks in the code are creating a reference to the auto-generated proxy for the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="5858e-195">次のコードは、ハブのプロキシを宣言します。</span><span class="sxs-lookup"><span data-stu-id="5858e-195">The following code declares a proxy for a hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="5858e-196">JQuery でサーバー クラスとそのメンバーへの参照がキャメル ケースです。</span><span class="sxs-lookup"><span data-stu-id="5858e-196">In jQuery the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="5858e-197">コード サンプルを参照して、c# **ChatHub**クラスとしての jquery **chatHub**します。</span><span class="sxs-lookup"><span data-stu-id="5858e-197">The code sample references the C# **ChatHub** class in jQuery as **chatHub**.</span></span> <span data-ttu-id="5858e-198">参照する場合、`ChatHub`クラス jquery では通常 pascal 形式で、c# の場合と大文字小文字の区別 ChatHub.cs クラス ファイルを編集します。</span><span class="sxs-lookup"><span data-stu-id="5858e-198">If you want to reference the `ChatHub` class in jQuery with conventional Pascal casing as you would in C#, edit the ChatHub.cs class file.</span></span> <span data-ttu-id="5858e-199">追加、`using`を参照するステートメント、`Microsoft.AspNet.SignalR.Hubs`名前空間。</span><span class="sxs-lookup"><span data-stu-id="5858e-199">Add a `using` statement to reference the `Microsoft.AspNet.SignalR.Hubs` namespace.</span></span> <span data-ttu-id="5858e-200">追加し、`HubName`属性を`ChatHub`クラス、たとえば`[HubName("ChatHub")]`します。</span><span class="sxs-lookup"><span data-stu-id="5858e-200">Then add the `HubName` attribute to the `ChatHub` class, for example `[HubName("ChatHub")]`.</span></span> <span data-ttu-id="5858e-201">参照を jQuery を最後に、更新、`ChatHub`クラス。</span><span class="sxs-lookup"><span data-stu-id="5858e-201">Finally, update your jQuery reference to the `ChatHub` class.</span></span>


<span data-ttu-id="5858e-202">次のコードでは、スクリプトでコールバック関数を作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="5858e-202">The following code shows how to create a callback function in the script.</span></span> <span data-ttu-id="5858e-203">サーバー上のハブ クラスは、各クライアントにコンテンツの更新をプッシュするには、この関数を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="5858e-203">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="5858e-204">省略可能な呼び出し、 `htmlEncode`  ページで、スクリプト インジェクションを防止する手段として表示する前に関数を HTML に方法が表示されますが、メッセージの内容をエンコードします。</span><span class="sxs-lookup"><span data-stu-id="5858e-204">The optional call to the `htmlEncode` function shows a way to HTML encode the message content before displaying it in the page, as a way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample7.html)]

<span data-ttu-id="5858e-205">次のコードでは、ハブとの接続を開く方法を示します。</span><span class="sxs-lookup"><span data-stu-id="5858e-205">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="5858e-206">コードが接続を開始しのクリック イベントを処理する関数に渡します、**送信**チャット ページ ボタン。</span><span class="sxs-lookup"><span data-stu-id="5858e-206">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the Chat page.</span></span>

> [!NOTE]
> <span data-ttu-id="5858e-207">このアプローチにより、イベント ハンドラーが実行される前に、接続が確立されていること。</span><span class="sxs-lookup"><span data-stu-id="5858e-207">This approach ensures that the connection is established before the event handler executes.</span></span>


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="5858e-208">次の手順</span><span class="sxs-lookup"><span data-stu-id="5858e-208">Next Steps</span></span>

<span data-ttu-id="5858e-209">SignalR がリアルタイムの web アプリケーションを構築するためのフレームワークについて説明しました。</span><span class="sxs-lookup"><span data-stu-id="5858e-209">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="5858e-210">いくつかの SignalR 開発タスクも学習しました。 SignalR を ASP.NET アプリケーションを追加する方法、ハブ クラスを作成する方法と、ハブからメッセージを送受信する方法。</span><span class="sxs-lookup"><span data-stu-id="5858e-210">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="5858e-211">高度な SignalR 開発の概念については、SignalR のソース コードおよびリソースは、次のサイトを参照してください。</span><span class="sxs-lookup"><span data-stu-id="5858e-211">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources :</span></span>

- [<span data-ttu-id="5858e-212">SignalR プロジェクト</span><span class="sxs-lookup"><span data-stu-id="5858e-212">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="5858e-213">SignalR Github とサンプル</span><span class="sxs-lookup"><span data-stu-id="5858e-213">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="5858e-214">SignalR の Wiki</span><span class="sxs-lookup"><span data-stu-id="5858e-214">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
